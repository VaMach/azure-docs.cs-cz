---
title: "Přidání certifikátu protokolu SSL do aplikace Azure App Service | Microsoft Docs"
description: "Zjistěte, jak přidat certifikát SSL do vaší aplikace služby App Service."
services: app-service
documentationcenter: .net
author: ahmedelnably
manager: stefsch
editor: cephalin
tags: buy-ssl-certificates
ms.assetid: cdb9719a-c8eb-47e5-817f-e15eaea1f5f8
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/19/2016
ms.author: apurvajo
ms.openlocfilehash: 191dd7240ad15b4936a72bc27a2d0162350f3afb
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="buy-and-configure-an-ssl-certificate-for-your-azure-app-service"></a>Koupě a konfigurace certifikátu SSL pro službu Azure App Service

V tomto kurzu bude Zabezpečte svoji webovou aplikaci pomocí nákupu certifikát protokolu SSL pro vaše  **[Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714)**, bezpečně uložením do [Azure Key Vault](https://docs.microsoft.com/en-us/azure/key-vault/key-vault-whatis)a přiřadí se s vlastní doménu.

## <a name="step-1---log-in-to-azure"></a>Krok 1 – přihlášení do Azure

Přihlaste se k portálu Azure na http://portal.azure.com

## <a name="step-2---place-an-ssl-certificate-order"></a>Krok 2 – umístit pořadí certifikát SSL

Certifikát SSL pořadí můžete umístit tak, že vytvoříte novou [certifikát služby aplikace](https://portal.azure.com/#create/Microsoft.SSL) v **portál Azure**.

![Vytvoření certifikátu](./media/app-service-web-purchase-ssl-web-site/createssl.png)

Zadejte popisný **název** certifikátů pro zabezpečení SSL a zadejte **název domény**

> [!NOTE]
> To je jedním z nejdůležitějších části procesu nákupu. Ujistěte se, že zadejte název hostitele správný (vlastní domény), který chcete chránit pomocí tohoto certifikátu. **NECHCETE** připojit název hostitele s WWW. 
>

Vyberte vaše **předplatné**, **skupiny prostředků**, a **certifikátu SKU**

> [!WARNING]
> Certifikáty App Service můžete použít pouze v případě ostatních služeb aplikace v rámci stejného předplatného.  
>

## <a name="step-3---store-the-certificate-in-azure-key-vault"></a>Krok 3 – uložení certifikátu v Azure Key Vault

> [!NOTE]
> [Key Vault](https://docs.microsoft.com/en-us/azure/key-vault/key-vault-whatis) je služba Azure, která pomáhá chránit kryptografické klíče a tajné klíče používané cloudovými aplikacemi a službami.
>

Po dokončení nákupu certifikát SSL, je nutné otevřít [služby App Service Certificate](https://portal.azure.com/#blade/HubsExtension/Resources/resourceType/Microsoft.CertificateRegistration%2FcertificateOrders) okna prostředků.

![Vložit obrázek připravena k uložení v KV](./media/app-service-web-purchase-ssl-web-site/ReadyKV.png)

Si všimnete, že je stav certifikátu **"Čekající vystavení"** jsou několik další kroky je potřeba provést před zahájením s použitím tohoto certifikátu.

Klikněte na tlačítko **konfigurace certifikátu** v okně Vlastnosti certifikátu a klikněte na **krok 1: uložení** pro uložení tohoto certifikátu v Azure Key Vault.

Z **klíč trezoru stav** okně klikněte na tlačítko **klíč trezoru úložiště** zvolit existující trezor klíč pro uložení tohoto certifikátu **nebo vytvořit nový klíč trezoru** k vytvoření nové Key Vault ve stejném předplatném nebo skupině prostředků.

> [!NOTE]
> Azure Key Vault má minimální poplatky za uložení tohoto certifikátu.
> Další informace najdete v tématu  **[klíč trezoru podrobnostmi o cenách Azure](https://azure.microsoft.com/pricing/details/key-vault/)**.
>

Jakmile vyberete klíč trezoru úložiště pro uložení tohoto certifikátu, **ukládání** možnost by se měla zobrazit úspěch.

![Vložit obrázek úspěchu úložiště v KV](./media/app-service-web-purchase-ssl-web-site/KVStoreSuccess.png)

## <a name="step-4---verify-the-domain-ownership"></a>Krok 4 - ověřit vlastnictví domény

> [!NOTE]
> Existují tři typy ověření domény nepodporuje App service Certificate: ověření domény, e-mailu, ručně. Tyto mechanismy jsou popsány v další podrobnosti najdete [Advanced části](#advanced).

Ze stejné **konfigurace certifikátu** okno, které jste použili v kroku 3, klikněte na tlačítko **krok 2: ověření**.

**Ověření domény** jedná se o nejpohodlnější proces **pouze v případě** máte  **[zakoupili vaši vlastní doménu služby Azure App Service.](custom-dns-web-site-buydomains-web-app.md)**
Klikněte na **ověřte** tlačítko k dokončení tohoto kroku.

![Vložit obrázek ověření domény](./media/app-service-web-purchase-ssl-web-site/DomainVerificationRequired.png)

Po kliknutí na **ověřte**, použijte **aktualizovat** tlačítko až **ověřte** možnost by se měla zobrazit úspěch.

![Vložit obrázek ověřit úspěšné provedení v KV](./media/app-service-web-purchase-ssl-web-site/KVVerifySuccess.png)

## <a name="step-5---assign-certificate-to-app-service-app"></a>Krok 5 – přiřadit certifikát k aplikaci služby App Service

> [!NOTE]
> Před provedením kroků v této části, musí mít přidružené vlastního názvu domény s vaší aplikací. Další informace najdete v tématu  **[konfigurace vlastního názvu domény pro webovou aplikaci.](app-service-web-tutorial-custom-domain.md)**
>

V  **[portál Azure](https://portal.azure.com/)**, klikněte **služby App Service** možnost na levé straně stránky.

Klikněte na název aplikace, ke které chcete přiřadit certifikát.

V **nastavení**, klikněte na tlačítko **certifikáty SSL**.

Klikněte na tlačítko **importovat aplikaci služby certifikát** a vyberte certifikát, který jste zakoupili.

![Vložit obrázek importovat certifikát](./media/app-service-web-purchase-ssl-web-site/ImportCertificate.png)

V **vazby ssl** část kliknutím na **přidat vazby**a pomocí rozevíracích seznamů vyberte název domény pro zabezpečené pomocí protokolu SSL a certifikát používat. Může také vybrat, jestli se má používat  **[indikace názvu serveru (SNI)](http://en.wikipedia.org/wiki/Server_Name_Indication)**  nebo IP na základě protokolu SSL.

![Vložit obrázek vazby SSL](./media/app-service-web-purchase-ssl-web-site/SSLBindings.png)

Klikněte na tlačítko **přidat vazbu** uložte změny a povolit protokol SSL.

> [!NOTE]
> Pokud jste vybrali **IP na základě SSL** a vaše vlastní doména je nakonfigurována pomocí záznamu A, je třeba provést následující kroky. Tyto mechanismy jsou popsány v další podrobnosti najdete [Advanced části](#Advanced).

V tomto okamžiku nyní byste měli mít k navštívení vaší aplikace pomocí `HTTPS://` místo `HTTP://` k ověření, že je certifikát správně nakonfigurováno.

<!--![insert image of https](./media/app-service-web-purchase-ssl-web-site/Https.png)-->

## <a name="step-6---management-tasks"></a>Krok 6: úlohy správy

### <a name="azure-cli"></a>Azure CLI

[!code-azurecli[hlavní](../../cli_scripts/app-service/configure-ssl-certificate/configure-ssl-certificate.sh?highlight=3-5 "vlastní certifikát SSL vazbu do webové aplikace")] 

### <a name="powershell"></a>PowerShell

[!code-powershell[hlavní](../../powershell_scripts/app-service/configure-ssl-certificate/configure-ssl-certificate.ps1?highlight=1-3 "vlastní certifikát SSL vazbu do webové aplikace")]

## <a name="advanced"></a>Advanced

### <a name="verifying-domain-ownership"></a>Ověření vlastnictví domény

Existují dva další typy ověření domény nepodporuje App service Certificate: e-mailu a ruční ověření.

#### <a name="mail-verification"></a>Ověření e-mailu

Již byl odeslán ověřovací e-mail do e-mailové adresy, přidružené k této vlastní doménu.
K dokončení kroku ověření e-mailu, otevřete e-mailu a klikněte na odkaz ověření.

![Vložit obrázek ověření e-mailu](./media/app-service-web-purchase-ssl-web-site/KVVerifyEmailSuccess.png)

Pokud potřebujete znovu odeslat ověřovací e-mail, klikněte **znovu odeslat e-mailu** tlačítko.

#### <a name="manual-verification"></a>Ruční ověření

> [!IMPORTANT]
> Ověření webové stránky HTML (funguje pouze u standardní SKU certifikát)
>

1. Vytvořte soubor HTML s názvem **"starfield.html"**

1. Obsah tohoto souboru musí být přesný název tokenu ověření domény. (Můžete zkopírovat tokenu z okna Stav ověření domény)

1. Odeslat tento soubor v kořenovém adresáři webového serveru, který je hostitelem vaší domény`/.well-known/pki-validation/starfield.html`

1. Klikněte na tlačítko **aktualizovat** aktualizovat stav certifikátu po dokončení ověření. Může trvat několik minut na dokončení ověření.

> [!TIP]
> Ověřte v terminálu pomocí `curl -G http://<domain>/.well-known/pki-validation/starfield.html` by měl obsahovat odpověď `<verification-token>`.

#### <a name="dns-txt-record-verification"></a>Záznam ověření DNS TXT

1. Pomocí Správce DNS vytvořit záznam TXT na `@` subdomény se hodnota rovná tokenu ověření domény.
1. Klikněte na tlačítko **"Aktualizovat"** aktualizovat stav certifikátu po dokončení ověření.

> [!TIP]
> Musíte vytvořit záznam TXT na `@.<domain>` s hodnotou `<verification-token>`.

### <a name="assign-certificate-to-app-service-app"></a>Přiřadit certifikát k aplikaci služby App Service

Pokud jste vybrali **IP na základě SSL** a vaše vlastní doména je nakonfigurována pomocí záznamu A, je třeba provést následující kroky:

Po dokončení konfigurace IP adresy na základě vazby SSL, vyhrazenou IP adresu je přiřazené vaší aplikaci. Tuto IP adresu můžete najít na **vlastní domény** stránky v části Nastavení aplikace, vpravo nahoře **názvy hostitelů** části. Je uveden jako **externí IP adresu**

![Vložit obrázek IP protokolu SSL](./media/app-service-web-purchase-ssl-web-site/virtual-ip-address.png)

Upozorňujeme, že tato IP adresa se liší od virtuální IP adresy, které byly dříve slouží ke konfiguraci záznamu A pro doménu. Pokud jsou nakonfigurovány pro použití SNI na základě protokolu SSL, nebo nejsou nakonfigurovaná pro používání protokolu SSL, je uvedena žádnou adresu pro tuto položku.

Pomocí nástrojů vaším registrátorem názvu domény, upravte záznamu A pro váš vlastní název domény tak, aby odkazoval na IP adresu z předchozího kroku.

## <a name="rekey-and-sync-the-certificate"></a>Změna hodnoty klíče a synchronizovat certifikátu

Pokud byste někdy potřebovali změna hodnoty klíče vašeho certifikátu, vyberte **změna hodnoty klíče a synchronizovat** možnost z **vlastnosti certifikátu** okno.

Klikněte na tlačítko **změna hodnoty klíče** tlačítko na zahájení procesu. Tento proces může trvat 1 až 10 minut na dokončení.

![Vložit obrázek změna hodnoty klíče protokolu SSL](./media/app-service-web-purchase-ssl-web-site/Rekey.png)

Obnovení klíčů vašeho certifikátu vrátí certifikát s nový certifikát vydaný certifikační autority.

## <a name="next-steps"></a>Další kroky

* [Přidat síti pro doručování obsahu](app-service-web-tutorial-content-delivery-network.md)