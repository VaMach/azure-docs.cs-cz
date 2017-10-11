---
title: "Implementace po havárii obnovení pomocí zálohování a obnovení v Azure API Management | Microsoft Docs"
description: "Naučte se používat zálohování a obnovení provést zotavení po havárii v Azure API Management."
services: api-management
documentationcenter: 
author: steved0x
manager: erikre
editor: 
ms.assetid: 6f10be3c-f796-4a6c-bacd-7931b6aa82af
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: apimpm
ms.openlocfilehash: 07c0265490cfae733133b6e0c938f90f9b392da4
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="how-to-implement-disaster-recovery-using-service-backup-and-restore-in-azure-api-management"></a>Implementace zotavení po havárii pomocí služby zálohování a obnovení v Azure API Management
Výběrem publikovat a spravovat vaše rozhraní API pomocí Azure API Management jsou využívat výhod mnoha odolnost proti chybám a možnosti infrastruktury, které byste jinak museli k návrhu, implementaci a správě. Platformy Azure snižuje velké zlomek případy možného selhání za zlomek ceny.

Obnovení z dostupnosti otázkách oblast, kde vaše rozhraní API správy služby je hostovat jste měli být připravení rekonstruovat služby v jiné oblasti kdykoli. V závislosti na vašich cílů dostupnosti a cíli času obnovení můžete vyhradit zálohování služby v jedné nebo více oblastech a pokuste se udržovat synchronizované se službou active jejich konfigurace a obsahu. Služba zálohování a obnovení funkce poskytuje nezbytné stavební blok pro implementaci strategie zotavení po havárii.

Tato příručka ukazuje, jak k ověřování žádostí o Azure Resource Manager a postup zálohování a obnovení vaší instance služby API Management.

> [!NOTE]
> Proces zálohování a obnovení instance služby API Management pro zotavení po havárii můžete použít také pro replikaci instance služby API Management pro scénáře, jako je pracovní.
>
> Všimněte si, že každá záloha vyprší po 30 dnech. Pokud se pokusíte obnovit zálohu po vypršení platnosti 30denní, obnovení se nezdaří s `Cannot restore: backup expired` zprávy.
>
>

## <a name="authenticating-azure-resource-manager-requests"></a>Požadavky na ověřování Azure Resource Manager
> [!IMPORTANT]
> Rozhraní REST API pro zálohování a obnovení používá Azure Resource Manager a má jiný mechanismus ověřování než rozhraní REST API pro správu vaší entity API Management. Postup v této části popisují, jak k ověřování žádostí o Azure Resource Manager. Další informace najdete v tématu [požadavků ověřování Azure Resource Manager](http://msdn.microsoft.com/library/azure/dn790557.aspx).
>
>

Všechny úlohy, které můžete provést na prostředků pomocí Azure Resource Manager musí být ověřeny v Azure Active Directory pomocí následujících kroků.

* Přidání aplikace do klienta Azure Active Directory.
* Nastavte oprávnění pro aplikace, která jste přidali.
* Získáte token pro ověřování požadavků na Azure Resource Manager.

Prvním krokem je vytvoření aplikace Azure Active Directory. Přihlaste se [portálu Azure Classic](http://manage.windowsazure.com/) pomocí předplatného, který obsahuje služby API Management instance a přejděte do **aplikace** kartu pro výchozí Azure Active Directory.

> [!NOTE]
> Pokud výchozí adresář služby Azure Active Directory není váš účet vidí, obraťte se na správce předplatného Azure udělit požadovaná oprávnění ke svému účtu.

![Vytvoření aplikace Azure Active Directory][api-management-add-aad-application]

Klikněte na tlačítko **přidat**, **přidat aplikaci, kterou vyvíjí Moje organizace**a zvolte **nativní klientskou aplikaci**. Zadejte popisný název a klikněte na šipku Další. Zadejte adresu URL zástupného symbolu, jako `http://resources` pro **identifikátor URI pro přesměrování**, jako je povinné pole, ale hodnota nepoužívá později. Klikněte na zaškrtávací políčko pro uložení aplikace.

Po uložení aplikace, klikněte na tlačítko **konfigurace**, přejděte dolů k položce **oprávnění k ostatním aplikacím** a klikněte na **přidat aplikaci**.

![Přidání oprávnění][api-management-aad-permissions-add]

Vyberte **Windows** **Azure Service Management API** a klikněte na zaškrtávací políčko pro tuto aplikaci přidat.

![Přidání oprávnění][api-management-aad-permissions]

Klikněte na tlačítko **delegovaná oprávnění** vedle nově přidaný **Windows** **Azure Service Management API** aplikace, zaškrtněte políčko pro **přístupu Azure Service managementu (preview)**a klikněte na tlačítko **Uložit**.

![Přidání oprávnění][api-management-aad-delegated-permissions]

Před vyvoláním rozhraní API, která generovat zálohu a obnovte ji, je nutné získat token. Následující příklad používá [Microsoft.IdentityModel.Clients.ActiveDirectory](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory) balíček nuget pro načtení tokenu.

```c#
using Microsoft.IdentityModel.Clients.ActiveDirectory;
using System;

namespace GetTokenResourceManagerRequests
{
    class Program
    {
        static void Main(string[] args)
        {
            var authenticationContext = new AuthenticationContext("https://login.microsoftonline.com/{tenant id}");
            var result = authenticationContext.AcquireToken("https://management.azure.com/", {application id}, new Uri({redirect uri});

            if (result == null) {
                throw new InvalidOperationException("Failed to obtain the JWT token");
            }

            Console.WriteLine(result.AccessToken);

            Console.ReadLine();
        }
    }
}
```

Nahraďte `{tentand id}`, `{application id}`, a `{redirect uri}` podle následujících pokynů.

Nahraďte `{tenant id}` s id klienta aplikace Azure Active Directory, kterou jste právě vytvořili. Kliknutím můžete přistupovat k id **zobrazit koncové body**.

![Koncové body][api-management-aad-default-directory]

![Koncové body][api-management-endpoint]

Nahraďte `{application id}` a `{redirect uri}` pomocí **Id klienta** a adresu URL z **identifikátory URI přesměrování** části z vaší aplikace Azure Active Directory **konfigurace** kartě.

![Zdroje][api-management-aad-resources]

Jakmile jsou hodnoty zadané, příklad kódu by měl vrátit token podobně jako v následujícím příkladu.

![Token][api-management-arm-token]

Před voláním operace zálohování a obnovení, které jsou popsané v následujících částech, nastavte hlavičce autorizace požadavku pro volání REST.

```c#
request.Headers.Add(HttpRequestHeader.Authorization, "Bearer " + token);
```

## <a name="step1"></a>Zálohování služby API Management
Zálohování problém služby API Management následující požadavek HTTP:

`POST https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.ApiManagement/service/{serviceName}/backup?api-version={api-version}`

Kde:

* `subscriptionId`– id předplatného obsahující služba API Management, kterou zkoušíte pro zálohování
* `resourceGroupName`-řetězec ve formátu 'Api - výchozí – {služba region}, kde `service-region` identifikuje oblasti Azure, kde je služba API Management se pokoušíte zálohování hostovaný, například`North-Central-US`
* `serviceName`-název služby API Management provádíte zálohování určenou při jeho vytvoření
* `api-version`-nahradit`2014-02-14`

V těle žádosti zadejte název účtu úložiště Azure cíl, přístupový klíč, název kontejneru objektu blob a název zálohy:

```
'{  
    storageAccount : {storage account name for the backup},  
    accessKey : {access key for the account},  
    containerName : {backup container name},  
    backupName : {backup blob name}  
}'
```

Nastavte hodnotu `Content-Type` hlavička požadavku na `application/json`.

Zálohování je dlouhotrvající operace, která může trvat několik minut.  Pokud požadavek byl úspěšný a byl zahájen proces zálohování, dostanete `202 Accepted` stavový kód odpovědi s `Location` záhlaví.  Ujistěte se, "GET" požadavky na adresu URL v `Location` hlavičky k zjištění stavu operace. V průběhu zálohování můžete nadále budete dostávat '202 platných' stavový kód. Kód odpovědi `200 OK` označí úspěšném dokončení operace zálohování.

Poznámka: následující omezení při vytváření zálohy požadavku.

* **Kontejner** zadaná v těle žádosti **musí existovat**.
* Při zálohování se v průběhu **neměli žádné operace správy, služba** například SKU upgrade nebo přechod na starší verzi, změna názvu domény, atd.
* Obnovení systému **zálohování záruku, že se pouze po dobu 30 dnů** od okamžiku jeho vytvoření.
* **Data o využití** použitý k vytvoření sestavy analýzy **nezahrnuje** v záloze. Použití [REST API služby Azure API Management] [ Azure API Management REST API] pravidelně načíst analytics sestavy z bezpečnostních důvodů udržován.
* Frekvence, se kterým můžete provést zálohování služby ovlivní vaše plánovaného bodu obnovení. Chcete-li minimalizovat ho doporučujeme implementace pravidelných záloh a také provádění zálohování na vyžádání po důležitých změnách služby API Management.
* **Změny** provedené v konfiguraci služby (např. rozhraní API, zásady, vzhled portálu vývojáře) při zálohování operace je v procesu **nemusí být zahrnuty do zálohování a proto budou ztraceny**.

## <a name="step2"></a>Obnovení služby API Management
Chcete-li obnovit API Management služby z dříve vytvořeného udělejte následující požadavek HTTP:

`POST https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.ApiManagement/service/{serviceName}/restore?api-version={api-version}`

Kde:

* `subscriptionId`– id předplatného obsahující obnovujete zálohu do služby API Management
* `resourceGroupName`-řetězec ve formátu 'Api - výchozí – {služba region}, kde `service-region` identifikuje oblasti Azure, kde je umístěn obnovujete zálohu do služby API Management, například`North-Central-US`
* `serviceName`-Název rozhraní API pro správu služby obnovena do určenou při jeho vytvoření
* `api-version`-nahradit`2014-02-14`

V těle žádosti zadejte umístění záložního souboru, tj. název účtu úložiště Azure, přístupový klíč, název kontejneru objektu blob a název zálohy:

```
'{  
    storageAccount : {storage account name for the backup},  
    accessKey : {access key for the account},  
    containerName : {backup container name},  
    backupName : {backup blob name}  
}'
```

Nastavte hodnotu `Content-Type` hlavička požadavku na `application/json`.

Obnovení je dlouhotrvající operace, která může trvat až 30 nebo více minut na dokončení.  Pokud požadavek byl úspěšný a byl zahájen proces obnovení, dostanete `202 Accepted` stavový kód odpovědi s `Location` záhlaví.  Ujistěte se, "GET" požadavky na adresu URL v `Location` hlavičky k zjištění stavu operace. V průběhu obnovení budou nadále přijímat '202 platných' stavový kód. Kód odpovědi `200 OK` označí úspěšném dokončení operace obnovení.

> [!IMPORTANT]
> **Verze SKU** služby obnovena do **musí odpovídat** SKU služby zálohovaných obnovena.
>
> **Změny** provedené konfigurace služby (např. rozhraní API, zásady, vzhled portálu vývojáře) při obnovení v probíhá operace **může dojít k přepsání**.
>
>

## <a name="next-steps"></a>Další kroky
Podívejte se v těchto blozích Microsoft pro dva různé postupy procesu zálohování a obnovení.

* [Replikovat účty Azure API Management](https://www.returngis.net/en/2015/06/replicate-azure-api-management-accounts/)
  * Děkujeme, že k Gisela jeho příspěvku v tomto článku.
* [Azure API Management: Zálohování a obnovení konfigurace](http://blogs.msdn.com/b/stuartleeks/archive/2015/04/29/azure-api-management-backing-up-and-restoring-configuration.aspx)
  * Přístup podrobné podle Stanislav neodpovídá oficiální pokyny, ale je velmi zajímavé.

[Backup an API Management service]: #step1
[Restore an API Management service]: #step2


[Azure API Management REST API]: http://msdn.microsoft.com/library/azure/dn781421.aspx

[api-management-add-aad-application]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-add-aad-application.png

[api-management-aad-permissions]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-aad-permissions.png
[api-management-aad-permissions-add]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-aad-permissions-add.png
[api-management-aad-delegated-permissions]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-aad-delegated-permissions.png
[api-management-aad-default-directory]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-aad-default-directory.png
[api-management-aad-resources]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-aad-resources.png
[api-management-arm-token]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-arm-token.png
[api-management-endpoint]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-endpoint.png
