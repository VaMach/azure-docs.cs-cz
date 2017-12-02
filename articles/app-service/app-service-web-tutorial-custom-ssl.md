---
title: "Vytvoření vazby stávající vlastní certifikát SSL pro službu Azure Web Apps | Microsoft Docs"
description: "Naučte se vytvořit vazbu vlastní certifikát SSL pro webovou aplikaci, back-end mobilní aplikace nebo aplikace API v Azure App Service."
services: app-service\web
documentationcenter: nodejs
author: cephalin
manager: erikre
editor: 
ms.assetid: 5d5bf588-b0bb-4c6d-8840-1b609cfb5750
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: tutorial
ms.date: 11/30/2017
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: f69bc731b2858c338d7f7b4d347e7107a0f4eeed
ms.sourcegitcommit: be0d1aaed5c0bbd9224e2011165c5515bfa8306c
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/01/2017
---
# <a name="bind-an-existing-custom-ssl-certificate-to-azure-web-apps"></a>Vytvoření vazby stávající vlastní certifikát SSL pro službu Azure Web Apps

Azure Web Apps nabízí vysoce škálovatelnou a automatických oprav webové hostitelské služby. V tomto kurzu se dozvíte, jak vytvořit vazbu vlastní certifikát SSL, který zakoupenému od důvěryhodné certifikační autority do [Azure Web Apps](app-service-web-overview.md). Jakmile budete hotovi, budete mít přístup k vaší webové aplikace na koncový bod HTTPS vaše vlastní doména DNS.

![Webové aplikace s vlastní certifikát SSL](./media/app-service-web-tutorial-custom-ssl/app-with-custom-ssl.png)

V tomto kurzu se naučíte:

> [!div class="checklist"]
> * Upgrade cenová úroveň vaší aplikace
> * Vytvoření vazby svůj vlastní certifikát SSL do služby App Service
> * Vynutit HTTPS pro vaši aplikaci.
> * Automatizovat vazbu certifikátu SSL se skripty

> [!NOTE]
> Pokud potřebujete získat vlastní certifikát SSL, můžete získat na portálu Azure přímo a navázat jej do vaší webové aplikace. Postupujte podle [kurz služby App Service Certificate](web-sites-purchase-ssl-web-site.md).

## <a name="prerequisites"></a>Požadavky

Pro absolvování tohoto kurzu potřebujete:

- [Vytvořit aplikaci služby App Service](/azure/app-service/)
- [Mapování názvu DNS na vaší webové aplikace](app-service-web-tutorial-custom-domain.md)
- Získat certifikát SSL od důvěryhodné certifikační autority
- Privátní klíč, který jste použili pro přihlášení žádost o certifikát SSL

<a name="requirements"></a>

### <a name="requirements-for-your-ssl-certificate"></a>Požadavky pro svůj certifikát SSL

Pro použití certifikátu ve službě App Service, certifikát musí splňovat následující požadavky:

* Podepsaný důvěryhodnou certifikační autoritou.
* Exportovat jako soubor PFX chráněný heslem
* Obsahuje privátní klíč minimálně 2048 bitů dlouho
* Obsahuje všechny zprostředkující certifikáty v řetězu certifikátů

> [!NOTE]
> **Elliptic Curve Cryptography (ECC) certifikáty** můžete pracovat s App Service, ale nejsou uvedené v tomto článku. Spolupracovat s vaší certifikační autority na přesné kroky k vytvoření certifikáty ECC.

## <a name="prepare-your-web-app"></a>Příprava vaší webové aplikace

K vytvoření vazby certifikát SSL a vlastní webové aplikace, vaše [plán služby App Service](https://azure.microsoft.com/pricing/details/app-service/) musí být v **základní**, **standardní**, nebo **Premium** vrstvy. V tomto kroku je třeba zkontrolovat, že vaše webová aplikace je v podporovaném cenová úroveň.

### <a name="log-in-to-azure"></a>Přihlaste se k Azure.

Otevřete web [Azure Portal](https://portal.azure.com).

### <a name="navigate-to-your-web-app"></a>Přejděte do vaší webové aplikace

V levé nabídce klikněte na tlačítko **App Services**a potom klikněte na název vaší webové aplikace.

![Výběr webové aplikace](./media/app-service-web-tutorial-custom-ssl/select-app.png)

Dostali jste na stránce Správa webové aplikace.  

### <a name="check-the-pricing-tier"></a>Zkontrolujte cenové úrovně

V levém navigačním panelu webové stránky aplikace, přejděte na **nastavení** a vyberte **škálování (plán služby App Service)**.

![Škálování nabídky](./media/app-service-web-tutorial-custom-ssl/scale-up-menu.png)

Zkontrolujte, že není součástí vaší webové aplikace **volné** nebo **sdílené** vrstvy. Aktuální úroveň vaší webové aplikace se zvýrazní v tmavý blue pole.

![Zkontrolujte cenová úroveň.](./media/app-service-web-tutorial-custom-ssl/check-pricing-tier.png)

Vlastní SSL není podporována v **volné** nebo **sdílené** vrstvy. Pokud potřebujete škálování, postupujte podle kroků v další části. Jinak, zavřete **zvolte cenovou úroveň** stránky a přejít na [odesílání a vaše certifikát SSL vazbu](#upload).

### <a name="scale-up-your-app-service-plan"></a>Škálovat plán služby App Service

Vyberte jednu z **základní**, **standardní**, nebo **Premium** vrstev.

Klikněte na **Vybrat**.

![Vyberte cenovou úroveň](./media/app-service-web-tutorial-custom-ssl/choose-pricing-tier.png)

Když se zobrazí následující oznámení, je dokončena operace škálování.

![Škálování oznámení](./media/app-service-web-tutorial-custom-ssl/scale-notification.png)

<a name="upload"></a>

## <a name="bind-your-ssl-certificate"></a>Vytvoření vazby certifikátu SSL

Jste připraveni k odeslání vašeho certifikátu SSL pro webovou aplikaci.

### <a name="merge-intermediate-certificates"></a>Sloučení zprostředkující certifikáty

Pokud certifikační autorita poskytuje několik certifikátů v řetězu certifikátů, musíte sloučit certifikáty v pořadí. 

Chcete-li to provést, otevřete každý certifikát, který jste obdrželi v textovém editoru. 

Vytvořte soubor pro sloučené certifikát názvem _mergedcertificate.crt_. V textovém editoru zkopírujte obsah každý certifikát do tohoto souboru. Pořadí certifikáty postupujte podle pořadí v řetězu certifikátů, od vašeho certifikátu a konče kořenový certifikát. Vypadá v následujícím příkladu:

```
-----BEGIN CERTIFICATE-----
<your entire Base64 encoded SSL certificate>
-----END CERTIFICATE-----

-----BEGIN CERTIFICATE-----
<The entire Base64 encoded intermediate certificate 1>
-----END CERTIFICATE-----

-----BEGIN CERTIFICATE-----
<The entire Base64 encoded intermediate certificate 2>
-----END CERTIFICATE-----

-----BEGIN CERTIFICATE-----
<The entire Base64 encoded root certificate>
-----END CERTIFICATE-----
```

### <a name="export-certificate-to-pfx"></a>Export certifikátu PFX

Exportujte sloučené certifikát SSL s privátním klíčem, který žádost o certifikát byl vytvořen s.

Pokud jste vygenerovali žádosti o certifikát pomocí OpenSSL, jste vytvořili soubor privátního klíče. Chcete-li exportovat certifikát PFX, spusťte následující příkaz. Nahraďte zástupné symboly  _&lt;soubor privátního klíče >_ a  _&lt;sloučit soubor certifikátu >_ s cestami k privátní klíč a soubor sloučené certifikátu.

```bash
openssl pkcs12 -export -out myserver.pfx -inkey <private-key-file> -in <merged-certificate-file>  
```

Po zobrazení výzvy zadejte heslo k exportu. Toto heslo budete používat při nahrávání svůj certifikát SSL do služby App Service později.

Pokud jste použili služby IIS nebo _Certreq.exe_ generovat žádosti o certifikát, nainstalujte certifikát do místního počítače a potom [exportu certifikátu do PFX](https://technet.microsoft.com/library/cc754329(v=ws.11).aspx).

### <a name="upload-your-ssl-certificate"></a>Nahrajte certifikát SSL

Chcete-li nahrát svůj certifikát SSL, klikněte na tlačítko **certifikáty SSL** v levé navigaci vaší webové aplikace.

Klikněte na tlačítko **nahrát certifikát**. 

V **soubor certifikátu PFX**, vyberte soubor PFX. V **heslo certifikátu**, zadejte heslo, které jste vytvořili při exportu souboru PFX.

Klikněte na **Odeslat**.

![Nahrání certifikátu](./media/app-service-web-tutorial-custom-ssl/upload-certificate-private1.png)

Po dokončení nahrávání svůj certifikát služby App Service se zobrazí v **certifikáty SSL** stránky.

![Nahrát certifikát](./media/app-service-web-tutorial-custom-ssl/certificate-uploaded.png)

### <a name="bind-your-ssl-certificate"></a>Vytvoření vazby certifikátu SSL

V **vazby SSL** klikněte na tlačítko **přidat vazbu**.

V **přidat vazbu SSL** pomocí rozevíracích seznamů a vyberte název domény pro zabezpečení a certifikát, který chcete použít.

> [!NOTE]
> Pokud jste nahráli certifikát, ale nejsou zobrazeny názvy domény v **Hostname** rozevíracího seznamu, aktualizujte stránku prohlížeče.
>
>

V **SSL typ**, vyberte, zda chcete použít  **[indikace názvu serveru (SNI)](http://en.wikipedia.org/wiki/Server_Name_Indication)**  nebo SSL založenou na protokolu IP.

- **Založená na SNI** -vazby SSL na základě více SNI, mohou být přidány. Tato možnost umožňuje víc certifikátů SSL k zabezpečení více domén na stejnou IP adresu. Většina moderních prohlížeče (včetně aplikace Internet Explorer, Chrome, Firefox a Opera) podporují SNI (najít komplexnější informací podpory prohlížeče na [indikace názvu serveru](http://wikipedia.org/wiki/Server_Name_Indication)).
- **Založená na IP** -lze přidat pouze jednu vazbu SSL založenou na protokolu IP. Tato možnost umožňuje jenom jeden certifikát SSL k zabezpečení vyhrazené veřejné IP adresy. K zabezpečení více domén, je nutné zabezpečit je všechny používající stejný certifikát SSL. Toto je tradiční možnost pro vazbu SSL.

Klikněte na tlačítko **přidat vazbu**.

![Vytvoření vazby certifikátu SSL](./media/app-service-web-tutorial-custom-ssl/bind-certificate.png)

Po dokončení nahrávání svůj certifikát služby App Service se zobrazí v **vazby SSL** oddíly.

![Certifikát vázaný do webové aplikace](./media/app-service-web-tutorial-custom-ssl/certificate-bound.png)

## <a name="remap-a-record-for-ip-ssl"></a>Přemapování záznam pro protokol SSL IP

Pokud nepoužíváte založená na IP ve vaší webové aplikaci, přejděte k [HTTPS Test pro vaše vlastní doména](#test).

Ve výchozím nastavení používá sdílené veřejnou IP adresu webové aplikace. Při vytvoření vazby certifikátu SSL založenou na protokolu IP, služby App Service vytvoří nový, vyhrazenou IP adresu pro vaši webovou aplikaci.

Pokud záznam jsou namapovány na vaší webové aplikace, aktualizace registru vaší domény pomocí této nové, vyhrazené IP adresy.

Webové aplikace **vlastní domény** stránka se aktualizuje pomocí nové, vyhrazené IP adresy. [Zkopírujte tuto IP adresu](app-service-web-tutorial-custom-domain.md#info), pak [přemapovat záznam A](app-service-web-tutorial-custom-domain.md#map-an-a-record) na tuto novou IP adresu.

<a name="test"></a>

## <a name="test-https"></a>Test HTTPS

Nyní již zbývá udělat je a ujistěte se, že funguje protokol HTTPS pro vaši vlastní doménu. V různých prohlížečích, přejděte do `https://<your.custom.domain>` zobrazíte vykonává až vaše webová aplikace.

![Portál navigace do aplikace Azure](./media/app-service-web-tutorial-custom-ssl/app-with-custom-ssl.png)

> [!NOTE]
> Pokud vaše webová aplikace obsahuje certifikát chyby ověření, pravděpodobně používáte certifikát podepsaný svým držitelem.
>
> Pokud není tento případ, může mít vynecháno zprostředkující certifikáty při exportu certifikátu do souboru PFX.

<a name="bkmk_enforce"></a>

## <a name="enforce-https"></a>Vynucení HTTPS

Ve výchozím nastavení každý, kdo může pořád přístup k vaší webové aplikace pomocí protokolu HTTP. Můžete přesměrovat všechny požadavky protokolu HTTP s portem HTTPS.

Aplikace do webové stránky v levém navigačním panelu, vyberte **vlastní domény**. Potom v **pouze HTTPS**, vyberte **na**.

![Vynucení HTTPS](./media/app-service-web-tutorial-custom-ssl/enforce-https.png)

Po dokončení operace, přejděte k některému z adres URL protokolu HTTP, který odkazoval na vaši aplikaci. Například:

- `http://<app_name>.azurewebsites.net`
- `http://contoso.com`
- `http://www.contoso.com`

## <a name="automate-with-scripts"></a>Automatizovat pomocí skriptů

Je možné automatizovat vazby SSL pro webovou aplikaci pomocí skriptů, pomocí [rozhraní příkazového řádku Azure](/cli/azure/install-azure-cli) nebo [prostředí Azure PowerShell](/powershell/azure/overview).

### <a name="azure-cli"></a>Azure CLI

Následující příkaz odešle exportovaný soubor PFX a získá kryptografický otisk.

```bash
thumbprint=$(az webapp config ssl upload \
    --name <app_name> \
    --resource-group <resource_group_name> \
    --certificate-file <path_to_PFX_file> \
    --certificate-password <PFX_password> \
    --query thumbprint \
    --output tsv)
```

Následující příkaz přidá vazby SSL na základě SNI, pomocí kryptografického otisku z předchozí příkaz.

```bash
az webapp config ssl bind \
    --name <app_name> \
    --resource-group <resource_group_name>
    --certificate-thumbprint $thumbprint \
    --ssl-type SNI \
```

### <a name="azure-powershell"></a>Azure PowerShell

Následující příkaz odešle exportovaný soubor PFX a přidá vazbu na základě SNI SSL.

```PowerShell
New-AzureRmWebAppSSLBinding `
    -WebAppName <app_name> `
    -ResourceGroupName <resource_group_name> `
    -Name <dns_name> `
    -CertificateFilePath <path_to_PFX_file> `
    -CertificatePassword <PFX_password> `
    -SslState SniEnabled
```
## <a name="public-certificates-optional"></a>Certifikáty s veřejným (volitelné)
Můžete nahrát [veřejné certifikáty](https://blogs.msdn.microsoft.com/appserviceteam/2017/11/01/app-service-certificates-now-supports-public-certificates-cer/) do vaší webové aplikace. Veřejné certifikáty pro aplikace v prostředí App Service můžete také. Pokud potřebujete k uložení certifikátu v úložišti certifikátů LocalMachine, budete muset použít webové aplikace v App Service Environment. Další informace najdete v tématu [postup konfigurace veřejné certifikáty do vaší webové aplikace](https://blogs.msdn.microsoft.com/appserviceteam/2017/11/01/app-service-certificates-now-supports-public-certificates-cer).

![Nahrát veřejný certifikát](./media/app-service-web-tutorial-custom-ssl/upload-certificate-public1.png)

## <a name="next-steps"></a>Další kroky

V tomto kurzu jste se naučili:

> [!div class="checklist"]
> * Upgrade cenová úroveň vaší aplikace
> * Vytvoření vazby svůj vlastní certifikát SSL do služby App Service
> * Vynutit HTTPS pro vaši aplikaci.
> * Automatizovat vazbu certifikátu SSL se skripty

Přechodu na v dalším kurzu se dozvíte, jak používat Azure Content Delivery Network.

> [!div class="nextstepaction"]
> [Přidat síti pro doručování obsahu (CDN) do Azure App Service](app-service-web-tutorial-content-delivery-network.md)

Další informace najdete v tématu [používají certifikát SSL v kódu aplikace v Azure App Service](app-service-web-ssl-cert-load.md).