---
title: "Mapovat existující vlastní název DNS pro službu Azure Web Apps | Microsoft Docs"
description: "Naučte se přidávat existující vlastní název domény DNS (jednoduché doména) pro webovou aplikaci, back-end mobilní aplikace nebo aplikace API v Azure App Service."
keywords: "služby App service, služby azure app service, mapování domény, název domény, existující domény, název hostitele"
services: app-service\web
documentationcenter: nodejs
author: cephalin
manager: erikre
editor: 
ms.assetid: dc446e0e-0958-48ea-8d99-441d2b947a7c
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: tutorial
ms.date: 06/23/2017
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: 1a0b54e75bd6356ba7ba351d51d5f4a59bd64c75
ms.sourcegitcommit: f847fcbf7f89405c1e2d327702cbd3f2399c4bc2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/28/2017
---
# <a name="map-an-existing-custom-dns-name-to-azure-web-apps"></a>Mapovat existující vlastní název DNS pro službu Azure Web Apps

[Azure Web Apps](app-service-web-overview.md) je vysoce škálovatelná služba s automatickými opravami pro hostování webů. V tomto kurzu se dozvíte, jak mapovat existující vlastní název DNS Azure Web Apps.

![Portál navigace do aplikace Azure](./media/app-service-web-tutorial-custom-domain/app-with-custom-dns.png)

V tomto kurzu se naučíte:

> [!div class="checklist"]
> * Mapování subdoména (například `www.contoso.com`) pomocí záznamu CNAME
> * Mapování kořenové domény (například `contoso.com`) pomocí záznamu a.
> * Namapovat doménu zástupných znaků (například `*.contoso.com`) pomocí záznamu CNAME
> * Automatizovat mapování domény pomocí skriptů

Můžete použít buď **záznam CNAME** nebo **záznam** mapovat vlastní název DNS služby App Service. 

> [!NOTE]
> Doporučujeme použít záznam CNAME pro všechny vlastní názvy DNS kromě kořenové domény (například `contoso.com`).

K migraci za provozu lokality a jeho název domény DNS do služby App Service, v tématu [migrovat aktivním názvem DNS do služby Azure App Service](app-service-custom-domain-name-migrate.md).

## <a name="prerequisites"></a>Požadavky

Pro absolvování tohoto kurzu potřebujete:

* [Vytvořit aplikaci služby App Service](/azure/app-service/), nebo použijte aplikaci, kterou jste vytvořili pro jiné kurzu.
* Zakupte název domény a ujistěte se, že máte přístup do registru DNS u svého poskytovatele domény (například GoDaddy).

  Chcete-li například přidat záznamy DNS pro `contoso.com` a `www.contoso.com`, musí být schopný nakonfigurovat nastavení DNS `contoso.com` kořenové domény.

  > [!NOTE]
  > Pokud nemáte existující domény název, vezměte v úvahu [zakoupení domény pomocí portálu Azure](custom-dns-web-site-buydomains-web-app.md). 

## <a name="prepare-the-app"></a>Příprava aplikace

Pro mapování názvu DNS do webové aplikace, webové aplikace na [plán služby App Service](https://azure.microsoft.com/pricing/details/app-service/) musí být placené vrstvy (**sdílené**, **základní**, **standardní**, nebo  **Premium**). V tomto kroku je třeba zkontrolovat, že aplikace App Service je v podporovaném cenová úroveň.

### <a name="sign-in-to-azure"></a>Přihlášení k Azure

Otevřete [portál Azure](https://portal.azure.com) a přihlaste se pomocí účtu Azure.

### <a name="navigate-to-the-app-in-the-azure-portal"></a>Přejděte na aplikaci na portálu Azure

V nabídce vlevo vyberte **App Services**a pak vyberte název aplikace.

![Portál navigace do aplikace Azure](./media/app-service-web-tutorial-custom-domain/select-app.png)

Zobrazí na stránce Správa aplikace služby App Service.  

<a name="checkpricing"></a>

### <a name="check-the-pricing-tier"></a>Zkontrolujte cenové úrovně

V levém navigačním panelu stránku aplikace, přejděte na **nastavení** a vyberte **škálování (plán služby App Service)**.

![Škálování nabídky](./media/app-service-web-tutorial-custom-domain/scale-up-menu.png)

Aktuální úroveň aplikace je označený modré ohraničení. Zkontrolujte, zda není v aplikaci **volné** vrstvy. Vlastní DNS není podporována v **volné** vrstvy. 

![Zkontrolujte cenová úroveň.](./media/app-service-web-tutorial-custom-domain/check-pricing-tier.png)

Pokud plán služby App Service není **volné**ukončit **zvolte cenovou úroveň** stránky a přejít na [mapování záznam CNAME](#cname).

<a name="scaleup"></a>

### <a name="scale-up-the-app-service-plan"></a>Škálovat plán služby App Service

Vyberte některé z vrstvy a bezplatnou (**sdílené**, **základní**, **standardní**, nebo **Premium**). 

Klikněte na **Vybrat**.

![Zkontrolujte cenová úroveň.](./media/app-service-web-tutorial-custom-domain/choose-pricing-tier.png)

Když se zobrazí následující oznámení, je dokončena operace škálování.

![Potvrzení operace škálování](./media/app-service-web-tutorial-custom-domain/scale-notification.png)

<a name="cname"></a>

## <a name="map-a-cname-record"></a>Mapa záznam CNAME

V kurzu příkladu přidat záznam CNAME pro `www` subdomény (například `www.contoso.com`).

[!INCLUDE [Access DNS records with domain provider](../../includes/app-service-web-access-dns-records.md)]

### <a name="create-the-cname-record"></a>Vytvořit záznam CNAME

Přidejte záznam CNAME pro mapování subdoména k výchozí hostitele aplikace (`<app_name>.azurewebsites.net`, kde `<app_name>` je název vaší aplikace).

Pro `www.contoso.com` příklad domény, přidejte záznam CNAME, který mapuje název `www` k `<app_name>.azurewebsites.net`.

Po přidání CNAME stránky záznamů DNS vypadá jako v následujícím příkladu:

![Portál navigace do aplikace Azure](./media/app-service-web-tutorial-custom-domain/cname-record.png)

### <a name="enable-the-cname-record-mapping-in-azure"></a>Povolit mapování záznam CNAME ve službě Azure

V levém navigačním panelu stránku aplikace v portálu Azure, vyberte **vlastní domény**. 

![Nabídky vlastní doménu.](./media/app-service-web-tutorial-custom-domain/custom-domain-menu.png)

V **vlastní domény** stránku aplikace a přidat vlastní plně kvalifikovaný název DNS (`www.contoso.com`) do seznamu.

Vyberte  **+**  ikonu vedle **přidat název hostitele**.

![Přidat název hostitele](./media/app-service-web-tutorial-custom-domain/add-host-name-cname.png)

Zadejte název plně kvalifikované domény přidat záznam CNAME pro, jako například `www.contoso.com`. 

Vyberte **ověření**.

**Přidat název hostitele** se aktivuje tlačítko. 

Ujistěte se, že **typ záznamu Hostname** je nastaven na **CNAME (www.example.com nebo všechny subdomény)**.

Vyberte **přidat název hostitele**.

![Přidání názvu DNS do aplikace](./media/app-service-web-tutorial-custom-domain/validate-domain-name-cname.png)

Může trvat nějakou dobu, než nový název hostitele v aplikaci projeví **vlastní domény** stránky. Aktualizujte prohlížeč aktualizovat data.

![Přidat záznam CNAME](./media/app-service-web-tutorial-custom-domain/cname-record-added.png)

Pokud je vynechán krok nebo provedené překlepem někde dříve, se zobrazí chyba ověření v dolní části stránky.

![Chyba ověření](./media/app-service-web-tutorial-custom-domain/verification-error-cname.png)

<a name="a"></a>

## <a name="map-an-a-record"></a>Mapa záznamu a.

V kurzu příkladu přidáte záznamu A pro kořenové domény (například `contoso.com`). 

<a name="info"></a>

### <a name="copy-the-apps-ip-address"></a>Zkopírujte adresu IP aplikace

Pokud chcete mapovat záznam A, musíte aplikace externí IP adresu. Tuto IP adresu můžete najít v dané aplikaci **vlastní domény** na portálu Azure.

V levém navigačním panelu stránku aplikace v portálu Azure, vyberte **vlastní domény**. 

![Nabídky vlastní doménu.](./media/app-service-web-tutorial-custom-domain/custom-domain-menu.png)

V **vlastní domény** stránky, zkopírujte aplikace IP adresu.

![Portál navigace do aplikace Azure](./media/app-service-web-tutorial-custom-domain/mapping-information.png)

[!INCLUDE [Access DNS records with domain provider](../../includes/app-service-web-access-dns-records.md)]

### <a name="create-the-a-record"></a>Vytvořte záznam a.

Pro mapování záznam do aplikace, služby App Service vyžaduje **dva** záznamy DNS:

- **A** záznam aplikace IP adresu.
- A **TXT** záznam k mapování na název hostitele výchozí aplikace `<app_name>.azurewebsites.net`. Služby App Service používá tento záznam pouze v době konfigurace, chcete-li ověřit, že vlastníte vlastní doménu. Jakmile vaši vlastní doménu ověřit a nakonfigurovat ve službě App Service, můžete odstranit tento záznam TXT. 

Pro `contoso.com` například domény vytvořit záznamy A a TXT podle následující tabulky (`@` obvykle představuje kořenové domény). 

| Typ záznamu | Hostitel | Hodnota |
| - | - | - |
| A | `@` | IP adresu z [zkopírujte aplikace IP adresu](#info) |
| TXT | `@` | `<app_name>.azurewebsites.net` |

Když se přidají záznamy, vypadá stránky záznamů DNS v následujícím příkladu:

![Stránky záznamů DNS](./media/app-service-web-tutorial-custom-domain/a-record.png)

<a name="enable-a"></a>

### <a name="enable-the-a-record-mapping-in-the-app"></a>Povolit záznam mapování A v aplikaci

Zpět v dané aplikaci **vlastní domény** na portálu Azure, přidejte vlastní plně kvalifikovaný název DNS (například `contoso.com`) do seznamu.

Vyberte  **+**  ikonu vedle **přidat název hostitele**.

![Přidat název hostitele](./media/app-service-web-tutorial-custom-domain/add-host-name.png)

Zadejte název plně kvalifikované domény nakonfigurovat záznam A, jako například `contoso.com`.

Vyberte **ověření**.

**Přidat název hostitele** se aktivuje tlačítko. 

Ujistěte se, že **typ záznamu Hostname** je nastaven na **záznam (example.com)**.

Vyberte **přidat název hostitele**.

![Přidání názvu DNS do aplikace](./media/app-service-web-tutorial-custom-domain/validate-domain-name.png)

Může trvat nějakou dobu, než nový název hostitele v aplikaci projeví **vlastní domény** stránky. Aktualizujte prohlížeč aktualizovat data.

![Přidat záznam](./media/app-service-web-tutorial-custom-domain/a-record-added.png)

Pokud je vynechán krok nebo provedené překlepem někde dříve, se zobrazí chyba ověření v dolní části stránky.

![Chyba ověření](./media/app-service-web-tutorial-custom-domain/verification-error.png)

<a name="wildcard"></a>

## <a name="map-a-wildcard-domain"></a>Namapovat doménu zástupný znak

V kurzu příkladu se mapují [název DNS zástupný znak](https://en.wikipedia.org/wiki/Wildcard_DNS_record) (například `*.contoso.com`) do aplikace služby App Service přidáním záznam CNAME. 

[!INCLUDE [Access DNS records with domain provider](../../includes/app-service-web-access-dns-records.md)]

### <a name="create-the-cname-record"></a>Vytvořit záznam CNAME

Přidejte záznam CNAME pro mapování názvu zástupné k výchozí hostitele aplikace (`<app_name>.azurewebsites.net`).

Pro `*.contoso.com` například domény záznam CNAME se mapování názvu `*` k `<app_name>.azurewebsites.net`.

Při přidání CNAME stránky záznamů DNS vypadá jako v následujícím příkladu:

![Portál navigace do aplikace Azure](./media/app-service-web-tutorial-custom-domain/cname-record-wildcard.png)

### <a name="enable-the-cname-record-mapping-in-the-app"></a>Povolit mapování záznam CNAME v aplikaci

Nyní můžete přidat všechny subdomény, která odpovídá názvu zástupné k aplikaci (například `sub1.contoso.com` a `sub2.contoso.com` odpovídat `*.contoso.com`). 

V levém navigačním panelu stránku aplikace v portálu Azure, vyberte **vlastní domény**. 

![Nabídky vlastní doménu.](./media/app-service-web-tutorial-custom-domain/custom-domain-menu.png)

Vyberte  **+**  ikonu vedle **přidat název hostitele**.

![Přidat název hostitele](./media/app-service-web-tutorial-custom-domain/add-host-name-cname.png)

Zadejte název plně kvalifikované domény, který odpovídá domény zástupných znaků (například `sub1.contoso.com`) a potom vyberte **ověřením**.

**Přidat název hostitele** se aktivuje tlačítko. 

Ujistěte se, že **typ záznamu Hostname** je nastaven na **záznam CNAME (www.example.com nebo všechny subdomény)**.

Vyberte **přidat název hostitele**.

![Přidání názvu DNS do aplikace](./media/app-service-web-tutorial-custom-domain/validate-domain-name-cname-wildcard.png)

Může trvat nějakou dobu, než nový název hostitele v aplikaci projeví **vlastní domény** stránky. Aktualizujte prohlížeč aktualizovat data.

Vyberte  **+**  ikonu Přidat jiné název hostitele, který odpovídá zástupné domény. Například přidejte `sub2.contoso.com`.

![Přidat záznam CNAME](./media/app-service-web-tutorial-custom-domain/cname-record-added-wildcard2.png)

## <a name="test-in-browser"></a>Testování v prohlížeči

Přejděte na názvy DNS, který jste nakonfigurovali dříve (například `contoso.com`, `www.contoso.com`, `sub1.contoso.com`, a `sub2.contoso.com`).

![Portál navigace do aplikace Azure](./media/app-service-web-tutorial-custom-domain/app-with-custom-dns.png)

## <a name="resolve-404-error-web-site-not-found"></a>Vyřešte chybu 404 "Webový server nebyl nalezen"

Pokud se zobrazí chyba HTTP 404 (není nalezena) při procházení na adresu URL vaši vlastní doménu, ověřte, že doménu přeloží na vaše aplikace IP adresu pomocí <a href="https://www.whatsmydns.net/" target="_blank">WhatsmyDNS.net</a>. Pokud ne, může být způsobeno jedním z následujících důvodů:

- Vlastní domény nakonfigurované chybí záznam A nebo záznam CNAME.
- V mezipaměti klienta prohlížeče starou IP adresu vaší domény. Vymažte mezipaměť a testování překlad DNS znovu. Na počítači s Windows, vymažte mezipaměť s `ipconfig /flushdns`.

<a name="virtualdir"></a>

## <a name="direct-default-url-to-a-custom-directory"></a>Přímé výchozí adresy URL do vlastní adresáře

Ve výchozím nastavení bude směrovat webové požadavky na kořenovém adresáři kódu aplikace služby App Service. Ale některé webové rozhraní nespouštějte v kořenovém adresáři. Například [Laravel](https://laravel.com/) se spustí v `public` podadresáři. Chcete-li pokračovat `contoso.com` například DNS by takové aplikace přístupná na `http://contoso.com/public`, ale by Opravdu chcete přímé `http://contoso.com` k `public` directory místo toho. Tento krok nebude zahrnovat překlad názvů DNS, ale přizpůsobení virtuální adresář.

Chcete-li to provést, vyberte **nastavení aplikace** v levém navigačním panelu webové stránky aplikace. 

V dolní části stránky virtuální kořenový adresář `/` odkazuje na `site\wwwroot` ve výchozím nastavení, která je kořenový adresář kódu aplikace. Změňte jej tak, aby odkazoval `site\wwwroot\public` místo, například a uložte změny. 

![Přizpůsobení virtuálního adresáře](./media/app-service-web-tutorial-custom-domain/customize-virtual-directory.png)

Po dokončení operace, aplikace by měl vrátit na stránce vpravo na kořenovou cestu (například http://contoso.com).

## <a name="automate-with-scripts"></a>Automatizovat pomocí skriptů

Můžete automatizovat správu vlastních domén s skripty, pomocí [rozhraní příkazového řádku Azure](/cli/azure/install-azure-cli) nebo [prostředí Azure PowerShell](/powershell/azure/overview). 

### <a name="azure-cli"></a>Azure CLI 

Následující příkaz přidá do aplikace služby App Service nakonfigurované vlastní název DNS. 

```bash 
az webapp config hostname add \
    --webapp-name <app_name> \
    --resource-group <resource_group_name> \ 
    --hostname <fully_qualified_domain_name> 
``` 

Další informace najdete v tématu [namapovat vlastní doménu do webové aplikace](scripts/app-service-cli-configure-custom-domain.md). 

### <a name="azure-powershell"></a>Azure PowerShell 

Následující příkaz přidá do aplikace služby App Service nakonfigurované vlastní název DNS. 

```PowerShell  
Set-AzureRmWebApp `
    -Name <app_name> `
    -ResourceGroupName <resource_group_name> ` 
    -HostNames @("<fully_qualified_domain_name>","<app_name>.azurewebsites.net") 
```

Další informace najdete v tématu [přiřadit vlastní doménu do webové aplikace](scripts/app-service-powershell-configure-custom-domain.md).

## <a name="next-steps"></a>Další kroky

V tomto kurzu jste se naučili:

> [!div class="checklist"]
> * Mapa subdoména pomocí záznamu CNAME
> * Mapa kořenové domény pomocí záznamu a.
> * Mapa zástupné domény pomocí záznamu CNAME
> * Automatizovat mapování domény pomocí skriptů

Přechodu na v dalším kurzu se dozvíte, jak vytvořit vazbu vlastní certifikát SSL pro webovou aplikaci.

> [!div class="nextstepaction"]
> [Vytvoření vazby stávající vlastní certifikát SSL pro službu Azure Web Apps](app-service-web-tutorial-custom-ssl.md)
