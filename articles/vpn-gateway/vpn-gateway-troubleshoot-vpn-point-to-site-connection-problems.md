---
title: "Poradce při potížích Azure připojení point-to-site | Microsoft Docs"
description: "Zjistěte, jak chcete-li vyřešit potíže s připojením point-to-site."
services: vpn-gateway
documentationcenter: na
author: chadmath
manager: cshepard
editor: 
tags: 
ms.service: vpn-gateway
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/23/2017
ms.author: genli
ms.openlocfilehash: 76ab1600903705aad7f18f48f41cb7119c3c09bf
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="troubleshooting-azure-point-to-site-connection-problems"></a>Řešení potíží: Problémy Azure připojení point-to-site

Tento článek obsahuje seznam běžných problémů připojení point-to-site, které můžete zaznamenat. Popisuje také možné příčiny a řešení těchto problémů.

## <a name="vpn-client-error-a-certificate-could-not-be-found"></a>Chyba klienta sítě VPN: certifikát nebyl nalezen.

### <a name="symptom"></a>Příznaky

Když se pokusíte připojit k virtuální sítě Azure s použitím klienta VPN, zobrazí se následující chybová zpráva:

**Certifikát nebyl nalezen, který může být použit s Tento Extensible Authentication Protocol. (Chyba 798)**

### <a name="cause"></a>Příčina

K tomuto problému dochází, pokud je klientský certifikát chybí **certifikáty – aktuální User\Personal\Certificates**.

### <a name="solution"></a>Řešení

Chcete-li vyřešit tento problém, postupujte takto:

1. Ujistěte se, že tyto certifikáty jsou ve správném umístění:

    | Certifikát | Umístění |
    | ------------- | ------------- |
    | AzureClient.pfx  | Aktuální User\Personal\Certificates |
    | Azuregateway -*GUID*. cloudapp.net  | Aktuální User\Trusted kořenové certifikační autority|
    | AzureGateway -*GUID*. cloudapp.net, AzureRoot.cer    | Místní počítač\Důvěryhodné kořenové certifikační autority|

2. Přejděte na uživatele\<uživatelské jméno > \AppData\Roaming\Microsoft\Network\Connections\Cm\<GUID >, je nutné ručně nainstalovat certifikát (*.cer soubor) v úložišti počítače a uživatele.

Další informace o tom, jak nainstalovat certifikát klienta najdete v tématu [generování a exportu certifikátů pro připojení point-to-site](vpn-gateway-certificates-point-to-site.md).

> [!NOTE]
> Když importujete certifikát klienta, nevybírejte **povolit silnou ochranu privátního klíče** možnost.

## <a name="vpn-client-error-the-message-received-was-unexpected-or-badly-formatted"></a>Chyba klienta sítě VPN: byla přijata zpráva Neočekávaná nebo chybně formátovaná

### <a name="symptom"></a>Příznaky

Když se pokusíte připojit k virtuální sítě Azure s použitím klienta VPN, zobrazí se následující chybová zpráva:

**Přijatá zpráva byla neočekávané nebo chybně formátovaný. (Chyba 0x80090326)**

### <a name="cause"></a>Příčina

K tomuto problému dochází, pokud není veřejný klíč certifikátu kořenové nahrála do služby Azure VPN gateway. Může také dojít, pokud klíč je poškozený nebo vypršela platnost.

### <a name="solution"></a>Řešení

Chcete-li vyřešit tento problém, zkontrolujte stav kořenového certifikátu v portálu Azure, abyste zjistili, zda byl odvolán. Pokud ho nebude odvolaný, pokuste se odstranit kořenový certifikát a reupload. Další informace najdete v tématu [vytvářet certifikáty](vpn-gateway-howto-point-to-site-classic-azure-portal.md#generatecerts).

## <a name="vpn-client-error-a-certificate-chain-processed-but-terminated"></a>Chyba klienta sítě VPN: řetěz certifikátů zpracuje, ale byla ukončena 

### <a name="symptom"></a>Příznaky 

Když se pokusíte připojit k virtuální sítě Azure s použitím klienta VPN, zobrazí se následující chybová zpráva:

**Řetěz certifikátů zpracuje, ale ukončen v kořenovém certifikátu, který není důvěryhodný vztah důvěryhodnosti zprostředkovatele.**

### <a name="solution"></a>Řešení

1. Ujistěte se, že tyto certifikáty jsou ve správném umístění:

    | Certifikát | Umístění |
    | ------------- | ------------- |
    | AzureClient.pfx  | Aktuální User\Personal\Certificates |
    | Azuregateway -*GUID*. cloudapp.net  | Aktuální User\Trusted kořenové certifikační autority|
    | AzureGateway -*GUID*. cloudapp.net, AzureRoot.cer    | Místní počítač\Důvěryhodné kořenové certifikační autority|

2. Pokud certifikáty jsou již v umístění, zkuste odstranit certifikáty a je přeinstalovat. **Azuregateway -*GUID*. cloudapp.net** certifikát se nachází ve balíček konfigurace klienta VPN, který jste si stáhli z portálu Azure. Můžete použít soubor archivers extrahujte soubory z balíčku.

## <a name="file-download-error-target-uri-is-not-specified"></a>Chyba při stahování souborů: není zadaný cílový identifikátor URI

### <a name="symptom"></a>Příznaky

Zobrazí se následující chybová zpráva:

**Stahování souboru došlo k chybě. Není zadaný cílový identifikátor URI.**

### <a name="cause"></a>Příčina 

K tomuto problému dochází kvůli typ nesprávný brány. 

### <a name="solution"></a>Řešení

Typ brány VPN musí být **VPN**, a musí být typ sítě VPN **RouteBased**.

## <a name="vpn-client-error-azure-vpn-custom-script-failed"></a>Chyba klienta sítě VPN: Azure VPN vlastního skriptu se nezdařilo 

### <a name="symptom"></a>Příznaky

Když se pokusíte připojit k virtuální sítě Azure s použitím klienta VPN, zobrazí se následující chybová zpráva:

**Vlastní skript (Chcete-li aktualizovat směrovací tabulky), která se nezdařila. (Chyba 8007026f)**

### <a name="cause"></a>Příčina

Tento problém se může vyskytnout, pokud se pokoušíte otevřít připojení k síti VPN bodu lokality pomocí zástupce.

### <a name="solution"></a>Řešení 

Otevřete balíček VPN přímo místo ho otevřít pomocí zástupce.

## <a name="cannot-install-the-vpn-client"></a>Nelze nainstalovat klienta VPN

### <a name="cause"></a>Příčina 

Další certifikáty, je potřeba vztah důvěryhodnosti služby VPN gateway pro vaši virtuální síť. Certifikát je součástí balíček konfigurace klienta VPN, které se generují z portálu Azure.

### <a name="solution"></a>Řešení

Rozbalte balíček konfigurace klienta VPN a najít soubor .cer. K instalaci certifikátu, postupujte takto:

1. Otevřete mmc.exe.
2. Přidat **certifikáty** modul snap-in.
3. Vyberte **počítače** účet pro místní počítač.
4. Klikněte pravým tlačítkem myši **důvěryhodné kořenové certifikační autority** uzlu. Klikněte na tlačítko **všech úloh** > **Import**a přejděte do souboru .cer, který jste rozbalili ze konfiguračního balíčku klienta VPN.
5. Restartujte počítač. 
6. Pokuste se nainstalovat klienta VPN.

## <a name="azure-portal-error-failed-to-save-the-vpn-gateway-and-the-data-is-invalid"></a>Portál Azure Chyba: nepovedlo se uložit služby VPN gateway a data nejsou platná

### <a name="symptom"></a>Příznaky

Při pokusu uložit změny pro bránu sítě VPN na portálu Azure, zobrazí se následující chybová zpráva:

**Nepovedlo se uložit brány virtuální sítě &lt;* název brány*&gt;. Data pro certifikát &lt; *certifikátu ID* &gt; je invalid.* *

### <a name="cause"></a>Příčina 

Tento problém může dojít, pokud veřejný klíč kořenového certifikátu, který jste nahráli obsahuje neplatný znak, jako je například mezerou.

### <a name="solution"></a>Řešení

Ujistěte se, že data v certifikátu neobsahuje neplatné znaky, jako je například konce řádků (návrat na začátek). Celý hodnota musí být dlouhý jeden řádek. Tento text je ukázka certifikátu:

    -----BEGIN CERTIFICATE-----
    MIIC5zCCAc+gAwIBAgIQFSwsLuUrCIdHwI3hzJbdBjANBgkqhkiG9w0BAQsFADAW
    MRQwEgYDVQQDDAtQMlNSb290Q2VydDAeFw0xNzA2MTUwMjU4NDZaFw0xODA2MTUw
    MzE4NDZaMBYxFDASBgNVBAMMC1AyU1Jvb3RDZXJ0MIIBIjANBgkqhkiG9w0BAQEF
    AAOCAQ8AMIIBCgKCAQEAz8QUCWCxxxTrxF5yc5uUpL/bzwC5zZ804ltB1NpPa/PI
    sa5uwLw/YFb8XG/JCWxUJpUzS/kHUKFluqkY80U+fAmRmTEMq5wcaMhp3wRfeq+1
    G9OPBNTyqpnHe+i54QAnj1DjsHXXNL4AL1N8/TSzYTm7dkiq+EAIyRRMrZlYwije
    407ChxIp0stB84MtMShhyoSm2hgl+3zfwuaGXoJQwWiXh715kMHVTSj9zFechYd7
    5OLltoRRDyyxsf0qweTFKIgFj13Hn/bq/UJG3AcyQNvlCv1HwQnXO+hckVBB29wE
    sF8QSYk2MMGimPDYYt4ZM5tmYLxxxvGmrGhc+HWXzMeQIDAQABozEwLzAOBgNVHQ8B
    Af8EBAMCAgQwHQYDVR0OBBYEFBE9zZWhQftVLBQNATC/LHLvMb0OMA0GCSqGSIb3
    DQEBCwUAA4IBAQB7k0ySFUQu72sfj3BdNxrXSyOT4L2rADLhxxxiK0U6gHUF6eWz
    /0h6y4mNkg3NgLT3j/WclqzHXZruhWAXSF+VbAGkwcKA99xGWOcUJ+vKVYL/kDja
    gaZrxHlhTYVVmwn4F7DWhteFqhzZ89/W9Mv6p180AimF96qDU8Ez8t860HQaFkU6
    2Nw9ZMsGkvLePZZi78yVBDCWMogBMhrRVXG/xQkBajgvL5syLwFBo2kWGdC+wyWY
    U/Z+EK9UuHnn3Hkq/vXEzRVsYuaxchta0X2UNRzRq+o706l+iyLTpe6fnvW6ilOi
    e8Jcej7mzunzyjz4chN0/WVF94MtxbUkLkqP
    -----END CERTIFICATE-----

## <a name="azure-portal-error-failed-to-save-the-vpn-gateway-and-the-resource-name-is-invalid"></a>Portál Azure Chyba: Nepodařilo se uložit bránu VPN, a název prostředku je neplatný

### <a name="symptom"></a>Příznaky

Při pokusu uložit změny pro bránu sítě VPN na portálu Azure, zobrazí se následující chybová zpráva: 

**Nepovedlo se uložit brány virtuální sítě &lt;* název brány*&gt;. Název prostředku &lt; *název certifikátu pokusu nahrát* &gt; je neplatný **.

### <a name="cause"></a>Příčina

K tomuto problému dochází, protože název certifikátu obsahuje neplatný znak, jako je například mezerou. 

## <a name="azure-portal-error-vpn-package-file-download-error-503"></a>Portál Azure Chyba: Chyba stažení souboru balíčku VPN 503

### <a name="symptom"></a>Příznaky

Když se pokusíte stáhnout balíček konfigurace klienta VPN, zobrazí se následující chybová zpráva:

**Nepodařilo se stáhnout soubor. Podrobnosti o chybě: chyba 503. Server je zaneprázdněný.**
 
### <a name="solution"></a>Řešení

Tato chyba může být způsobeno k dočasným potížím sítě. Pokusí stáhnout balíček VPN znovu za několik minut.

## <a name="azure-vpn-gateway-upgrade-all-p2s-clients-are-unable-to-connect"></a>Upgrade služby Azure VPN Gateway: všechny P2S klienti se nelze připojit

### <a name="cause"></a>Příčina

Pokud je certifikát více než 50 procent prostřednictvím celé jeho životnosti certifikátu převracet.

### <a name="solution"></a>Řešení

Chcete-li vyřešit tento problém, vytvořte a znovu distribuovat nové certifikáty pro klienty VPN. 

## <a name="too-many-vpn-clients-connected-at-once"></a>Příliš mnoho klientů VPN připojení najednou

Pro každé brány sítě VPN je maximální počet povolených připojení 128. Zobrazí celkový počet připojených klientů na portálu Azure.

## <a name="point-to-site-vpn-incorrectly-adds-a-route-for-100008-to-the-route-table"></a>Point-to-site VPN trasu pro 10.0.0.0/8 nesprávně přidá do tabulky směrování

### <a name="symptom"></a>Příznaky

Při přidávání připojení k síti VPN na straně klienta point-to-site, aby klient VPN nutné přidat směrování směrem k virtuální síť Azure. Pomocná služba IP nutné přidat směrování pro podsíť klientů sítě VPN. 

Rozsah klienta VPN patří do menší podsíti 10.0.0.0/8, jako je například 10.0.12.0/24. Místo trasu pro 10.0.12.0/24 se přidá trasu pro 10.0.0.0/8, který má vyšší prioritu. 

Tato nesprávná trasa dělí připojení s další místní sítě, které můžou patřit k jiné podsíti 10.0.0.0/8 rozsahu, jako je například 10.50.0.0/24, které nemají konkrétní trasy definované. 

### <a name="cause"></a>Příčina

Toto chování je záměrné pro klienty se systémem Windows. Když klient používá protokol PPP IPCP, získá IP adresu pro rozhraní tunelového propojení ze serveru (brána sítě VPN v tomto případě). Ale kvůli omezení v protokolu, klient nemá masku podsítě. Protože neexistuje jiný způsob, jak ji připravit, klient se pokusí uhodnout masku podsítě, na základě třídy IP adresy rozhraní tunelového propojení. 

Proto je přidána trasa podle následující statických mapování: 

Pokud adresa patří do třídy A použít aplikaci--> podsíť/8

Pokud adresa patří do třídy B--> použít /16

Pokud adresa patří do třídy C--> použít /24

## <a name="vpn-client-cannot-access-network-file-shares"></a>Klient VPN nelze získat přístup k síťové sdílené složky

### <a name="symptom"></a>Příznaky

Klient VPN byl připojen k virtuální síti Azure. Klient nemá přístup k síťové sdílené složky.

### <a name="cause"></a>Příčina

Protokol SMB se používá pro přístup ke sdílené složce souborů. Při zahájení připojení klienta VPN přidá relace přihlašovací údaje a dojde k selhání. Po navázání připojení klienta bude muset používat mezipaměti pověření pro ověřování protokolu Kerberos. Tento proces se spustí dotazů Key Distribution Center (řadič domény) k získání tokenu. Protože klient připojí z Internetu, nemusí být schopen kontaktovat řadič domény. Proto klienta nelze převzetí služeb při selhání z protokolu Kerberos protokolu NTLM. 

Jenom jednou, klient se zobrazí výzva pro pověření je, když má platný certifikát (sítě SAN = UPN) vystavil doménu, ke kterému je připojen. Klient také musí být fyzicky připojen k doménové síti. V tomto případě klient se pokusí použít certifikát a spojí k řadiči domény. Potom Key Distribution Center vrátí chybu "KDC_ERR_C_PRINCIPAL_UNKNOWN". Klient je donucen k převzetí služeb při selhání protokolu NTLM. 

### <a name="solution"></a>Řešení

Pokud chcete tento problém vyřešit, zakažte ukládání do mezipaměti přihlašovací údaje domény z následující podklíč registru: 

    HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Lsa\DisableDomainCreds - Set the value to 1 


## <a name="cannot-find-the-point-to-site-vpn-connection-in-windows-after-reinstalling-the-vpn-client"></a>Nelze najít připojení point-to-site VPN v systému Windows po opětovné instalaci klienta VPN

### <a name="symptom"></a>Příznaky

Odebrat připojení VPN point-to-site a znovu nainstalujete klienta VPN. V takovém případě není připojení k síti VPN byl úspěšně nakonfigurován. Nevidíte v připojení k síti VPN **síťová připojení** nastavení v systému Windows.

### <a name="solution"></a>Řešení

Chcete-li problém vyřešit, odstraňte původní soubory konfigurace klienta VPN z **C:\Users\TheUserName\AppData\Roaming\Microsoft\Network\Connections**, a poté znovu spusťte instalační program klienta VPN.
