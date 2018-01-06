---
title: "Ověření s Mobile Engagementem rozhraní REST API"
description: "Popisuje, jak ověřit pomocí rozhraní API REST služby Azure Mobile Engagement"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: da82cb36-957a-4e19-a805-b44733cf6597
ms.service: mobile-engagement
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: mobile-multiple
ms.workload: mobile
ms.date: 10/05/2016
ms.author: wesmc;ricksal
ms.openlocfilehash: 66bcd738b86f846eae3499b289a6629323009a44
ms.sourcegitcommit: d6984ef8cc057423ff81efb4645af9d0b902f843
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/05/2018
---
# <a name="authenticate-with-mobile-engagement-rest-apis"></a>Ověření s Mobile Engagementem rozhraní REST API

## <a name="overview"></a>Přehled

Tento dokument popisuje, jak se získat token k ověření s rozhraními API sady Mobile Engagement REST platný AD Oauth Azure.

Předpokládá se, že máte platné předplatné Azure a jste vytvořili aplikaci Mobile Engagement pomocí jedné z [vývojáře kurzy](mobile-engagement-windows-store-dotnet-get-started.md).

## <a name="authentication"></a>Authentication

Microsoft Azure Active Directory na základě tokenu se používá k ověřování OAuth. 

Aby žádosti o ověření rozhraní API musí hlavičku autorizace přidány do každého požadavku, která je v následujícím formátu:

    Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGmJlNmV2ZWJPamg2TTNXR1E...

> [!NOTE]
> Azure Active Directory tokeny vyprší za jednu hodinu.
> 
> 

Existuje několik způsobů, jak získat token. Vzhledem k tomu, že rozhraní API se nazývají z cloudové služby, kterou chcete použít klíč rozhraní API. Klíč rozhraní API v Azure terminologii nazývá hlavní heslo služby. Následující postup popisuje jeden způsob, jak ručně nastavení.

### <a name="one-time-setup-using-script"></a>Jednorázové instalace (pomocí skriptu.)

Postupujte podle sadu pokynů k provedení instalace pomocí skriptu prostředí PowerShell, který přebírá minimální hodnota času pro instalaci, ale používá většina povolenou výchozí hodnoty. Volitelně můžete také podle pokynů [ruční instalaci](mobile-engagement-api-authentication-manual.md) tohoto postupu z portálu Azure přímo a proveďte jemnějšího konfigurace.

1. Získat nejnovější verzi prostředí Azure PowerShell z [zde](http://aka.ms/webpi-azps). Další informace o pokyny ke stažení, uvidíte to [odkaz](/powershell/azure/overview).
2. Po instalaci prostředí Azure PowerShell, použijte následující příkazy k zajištění, že máte **Azure modulu** nainstalován:

    a. Zkontrolujte, zda že je k dispozici v seznamu dostupných modulů modulu Azure PowerShell.

        Get-Module –ListAvailable

    ![K dispozici moduly Azure][1]

    b. Pokud v seznamu nenajdete modulu Azure PowerShell, budete muset spustit:

        Import-Module Azure
3. Přihlášení k Azure Resource Manager z prostředí PowerShell spuštěním následujícího příkazu a poskytuje uživatelské jméno a heslo pro účet Azure: 

        Login-AzureRmAccount
4. Pokud máte více předplatných, pak spusťte:

    a. Získat seznam všech odběrů a zkopírujte ID předplatného odběr, který chcete použít. Zajistěte, aby že toto předplatné je stejný jako ten, který má aplikace Mobile Engagementu, který se chystáte interakci s pomocí rozhraní API. 

        Get-AzureRmSubscription

    b. Spusťte následující příkaz poskytování ID odběru konfigurace odběru, který se má použít.

        Select-AzureRmSubscription –SubscriptionId <subscriptionId>
5. Zkopírujte text pro [New-AzureRmServicePrincipalOwner.ps1](https://raw.githubusercontent.com/matt-gibbs/azbits/master/src/New-AzureRmServicePrincipalOwner.ps1) skriptu do místního počítače a uložte ho jako rutiny prostředí PowerShell (například `APIAuth.ps1`) a spusťte ho `.\APIAuth.ps1`.
6. Skript zobrazí výzvu k poskytují vstup pro **principalName**. Zadejte vhodný název sem, chcete použít k vytvoření aplikace Active Directory (např. APIAuth). 
7. Po dokončení skriptu zobrazí následující čtyři hodnoty, které potřebujete k ověření prostřednictvím kódu programu s AD proto nezapomeňte zkopírovat je. 

    **TenantId**, **SubscriptionId**, **ApplicationId**, a **tajný klíč**.

    Budete používat TenantId jako `{TENANT_ID}`, ApplicationId jako `{CLIENT_ID}` a tajný jako `{CLIENT_SECRET}`.

   > [!NOTE]
   > Výchozí zásady zabezpečení mohou blokovat spouštění skriptů prostředí PowerShell. Pokud ano, můžete dočasně konfiguraci vaší zásady spouštění povolit spuštění skriptu pomocí následujícího příkazu:
   > 
   > Set-ExecutionPolicy RemoteSigned
8. Zde je, jak bude vypadat sadu rutin PS.
    ![][3]
9. Na portálu Azure přejděte do služby Active Directory, klikněte na **registrace aplikace** a hledání své aplikace a ujistěte se, zda existuje![][4]

### <a name="steps-to-get-a-valid-token"></a>Kroky, chcete-li získat platný token

1. Volání rozhraní API s následujícími parametry a nezapomeňte nahradit klienta\_ID, klient\_ID a klienta\_tajný klíč:
   
   * **Adresa URL požadavku** jako`https://login.microsoftonline.com/{TENANT_ID}/oauth2/token`
   * **Hlavičku HTTP Content-Type** jako`application/x-www-form-urlencoded`
   * **Text žádosti HTTP** jako`grant_type=client\_credentials&client_id={CLIENT_ID}&client_secret={CLIENT_SECRET}&resource=https%3A%2F%2Fmanagement.core.windows.net%2F`
     
    Toto je požadavek příklad:
    ```
    POST /{TENANT_ID}/oauth2/token HTTP/1.1
    Host: login.microsoftonline.com
    Content-Type: application/x-www-form-urlencoded
    grant_type=client_credentials&client_id={CLIENT_ID}&client_secret={CLIENT_SECRET}&reso
    urce=https%3A%2F%2Fmanagement.core.windows.net%2F
    ```
    Tady je příklad odpověď:
    ```
    HTTP/1.1 200 OK
    Content-Type: application/json; charset=utf-8
    Content-Length: 1234

    {"token_type":"Bearer","expires_in":"3599","expires_on":"1445395811","not_before":"144
    5391911","resource":"https://management.core.windows.net/","access_token":{ACCESS_TOKEN}}
    ```
     Tento příklad zahrnuté kódování URL parametrů POST, `resource` hodnota je ve skutečnosti `https://management.core.windows.net/`. Dávejte pozor, abyste také adresu URL kódování `{CLIENT_SECRET}` jako může obsahovat speciální znaky.

     > [!NOTE]
     > Pro testování, můžete použít nástroj pro klienta na HTTP jako [Fiddler](http://www.telerik.com/fiddler) nebo [rozšíření Chrome Postman](https://chrome.google.com/webstore/detail/postman/fhbjgbiflinjbdggehcddcbncdddomop) 
     > 
     > 
2. Každé volání rozhraní API nyní obsahují hlavičce autorizace požadavku:
   
        Authorization: Bearer {ACCESS_TOKEN}
   
    Pokud se vrátil kód stavu 401, zkontrolujte text odpovědi se vás může upozornit tokenu vypršela. V takovém případě získáte nový token.

## <a name="using-the-apis"></a>Pomocí rozhraní API
Teď, když máte platný token, jste připraveni k volání rozhraní API.

1. V každé žádosti o rozhraní API musíte předat token platný, neprošlé, který jste získali v předchozí části.
2. Musíte se připojit některé parametry do požadavku URI, který identifikuje vaši aplikaci. Požadavek URI vypadá takto
   
        https://management.azure.com/subscriptions/{subscription-id}/resourcegroups/{resource-group-name}/
        providers/Microsoft.MobileEngagement/appcollections/{app-collection}/apps/{app-resource-name}/
   
    K získání parametrů, klikněte na název vaší aplikace a klikněte na řídicí panel a vy se zobrazí na stránce jako na následujícím obrázku se 3 parametry.
   
   * **1** `{subscription-id}`
   * **2** `{app-collection}`
   * **3** `{app-resource-name}`
   * **4** název vaše skupiny prostředků bude **MobileEngagement** Pokud jste vytvořili novou. 

> [!NOTE]
> <br/>
> 
> 1. Ignorujte adresu kořenové rozhraní API, jako to byla pro předchozí rozhraní API.<br/>
> 2. Pokud jste vytvořili aplikaci pomocí portálu Azure budete muset použijte název prostředku aplikace, které se liší od názvu aplikace, sám sebe. Pokud jste vytvořili aplikaci na portálu Azure doporučujeme, abyste používali samotný (není žádný rozdíl mezi název prostředku aplikace a název aplikace pro aplikace vytvořené v nového portálu) název aplikace.  
> 
> 

<!-- Images -->
[1]: ./media/mobile-engagement-api-authentication/azure-module.png
[2]: ./media/mobile-engagement-api-authentication/mobile-engagement-api-uri-params.png
[3]: ./media/mobile-engagement-api-authentication/ps-cmdlets.png
[4]: ./media/mobile-engagement-api-authentication/search-app.png



