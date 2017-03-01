---
title: "Připojení počítače k virtuální síti typu Point-to-Site: PowerShell | Dokumentace Microsoftu"
description: "Připojte počítač bezpečně ke službě Azure Virtual Network vytvořením připojení brány VPN typu Point-to-Site."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 3eddadf6-2e96-48c4-87c6-52a146faeec6
ms.service: vpn-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/17/2017
ms.author: cherylmc
translationtype: Human Translation
ms.sourcegitcommit: cf72197aba2c6e6c7a51f96d1161cf1fbe88a0c5
ms.openlocfilehash: fe3bb0a5faee806e7956acba23c22b9aefd1f0a8


---
# <a name="configure-a-point-to-site-connection-to-a-vnet-using-powershell"></a>Konfigurace připojení Point-to-Site k virtuální síti pomocí prostředí PowerShell
> [!div class="op_single_selector"]
> * [Resource Manager – Azure Portal](vpn-gateway-howto-point-to-site-resource-manager-portal.md)
> * [Resource Manager – PowerShell](vpn-gateway-howto-point-to-site-rm-ps.md)
> * [Classic – Azure Portal](vpn-gateway-howto-point-to-site-classic-azure-portal.md)
> 
> 

Konfigurace Point-to-Site (P2S) umožňuje vytvořit zabezpečené připojení jednotlivých klientských počítačů k virtuální síti. Připojení P2S je užitečné, když se chcete ke své virtuální síti připojit ze vzdáleného umístění, například z domova nebo z místa konání konference, nebo když máte jen několik klientů, kteří se potřebují připojovat k virtuální síti.

Připojení typu Point-to-Site k fungování nevyžadují zařízení VPN ani veřejnou IP adresu. Připojení VPN je vytvořeno spuštěním připojení na klientském počítači. Další informace o připojení Point-to-Site najdete v části [Nejčastější dotazy týkající se připojení Point-to-Site](#faq) na konci tohoto článku. 

Tento článek vás provede vytvořením virtuální sítě s připojením Point-to-Site v modelu nasazení Resource Manager pomocí PowerShellu.

### <a name="deployment-models-and-methods-for-p2s-connections"></a>Modely a metody nasazení pro připojení P2S
[!INCLUDE [deployment models](../../includes/vpn-gateway-deployment-models-include.md)]

Následující tabulka uvádí dva modely nasazení a dostupné metody nasazení pro konfigurace P2S. Když je článek s postupem konfigurace k dispozici, zařadíme do tabulky přímý odkaz na něj.

[!INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-table-point-to-site-include.md)]

## <a name="basic-workflow"></a>Základní pracovní postup
![Připojení počítače k virtuální síti Azure – diagram připojení Point-to-Site](./media/vpn-gateway-howto-point-to-site-rm-ps/point-to-site-diagram.png)

V tomto scénáři vytvoříte virtuální síť s připojením Point-to-Site. Pokyny vám také pomůžou vygenerovat certifikáty, které jsou požadovány pro tuto konfiguraci. Připojení P2S se skládá z následujících položek: virtuální sítě s bránou sítě VPN, kořenového certifikátu v souboru .cer (veřejný klíč), certifikátu klienta a konfigurace sítě VPN v klientu. 

Pro tuto konfiguraci používáme následující hodnoty. Nastavíme proměnné uvedené v části [1](#declare) článku. Můžete buď využít kroky jako podrobný postup a převzít hodnoty beze jejich změny, nebo je změnit tak, aby odpovídaly vašemu prostředí. 

### <a name="a-nameexampleaexample-values"></a><a name="example"></a>Příklady hodnot
* **Název: VNet1**
* **Adresní prostor: 192.168.0.0/16** a **10.254.0.0/16**<br>V tomto příkladu se používá více adresních prostorů k ilustraci, že bude tato konfigurace fungovat s více adresními prostory. Více adresních prostorů pro ni ale není potřeba.
* **Název podsítě: FrontEnd**
  * **Rozsah adres podsítě: 192.168.1.0/24**
* **Název podsítě: BackEnd**
  * **Rozsah adres podsítě: 10.254.1.0/24**
* **Název podsítě: GatewaySubnet**<br>Název podsítě *GatewaySubnet* je pro správnou funkci brány VPN Gateway povinný.
  * **Rozsah adres podsítě: 192.168.200.0/24** 
* **Fond adres klienta VPN: 172.16.201.0/24**<br>Klienti VPN, kteří se budou k síti VNet připojovat pomocí tohoto připojení Point-to-Site, dostanou IP adresu ze zadaného fondu adres klienta VPN.
* **Předplatné:** Ujistěte se, že máte správné předplatné, pokud máte více než jedno.
* **Skupina prostředků: TestRG**
* **Umístění: Východní USA**
* **Server DNS: IP adresa** serveru DNS, který chcete používat pro překlad názvů.
* **Název brány: Vnet1GW**
* **Název veřejné IP adresy: VNet1GWPIP**
* **Typ sítě VPN: RouteBased**

## <a name="before-beginning"></a>Před zahájením
* Ověřte, že máte předplatné Azure. Pokud ještě nemáte předplatné Azure, můžete si aktivovat [výhody pro předplatitele MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details) nebo si zaregistrovat [bezplatný účet](https://azure.microsoft.com/pricing/free-trial).
* Nainstalujte nejnovější verzi rutin PowerShellu pro Azure Resource Manager. Další informace o instalaci rutin prostředí PowerShell najdete v tématu [Instalace a konfigurace Azure PowerShellu](/powershell/azureps-cmdlets-docs). Při práci s prostředím PowerShell pro tuto konfiguraci se ujistěte, že jej spouštíte jako správce. 

## <a name="a-namedeclareapart-1---log-in-and-set-variables"></a><a name="declare"></a>Část 1 – Přihlášení a nastavení proměnných
V této části se přihlásíte a deklarujete hodnoty používané pro tuto konfiguraci. Deklarované hodnoty jsou použity v ukázkových skriptech. Změňte hodnoty tak, aby odpovídaly vašemu prostředí. Můžete také použít deklarované hodnoty a projít kroky jako cvičení.

1. V konzole prostředí PowerShell se přihlaste k účtu Azure. Tato rutina vás vyzve k zadání přihlašovacích údajů k vašemu účtu Azure. Po přihlášení se stáhne nastavení účtu, aby bylo dostupné v prostředí Azure PowerShell.
   
        Login-AzureRmAccount 
2. Načtěte seznam předplatných Azure.
   
        Get-AzureRmSubscription
3. Určete předplatné, které chcete použít. 
   
        Select-AzureRmSubscription -SubscriptionName "Name of subscription"
4. Deklarujte proměnné, které chcete použít. Použijte následující příklad a dle potřeby nahraďte v něm uvedené hodnoty vlastními.
   
        $VNetName  = "VNet1"
        $FESubName = "FrontEnd"
        $BESubName = "Backend"
        $GWSubName = "GatewaySubnet"
        $VNetPrefix1 = "192.168.0.0/16"
        $VNetPrefix2 = "10.254.0.0/16"
        $FESubPrefix = "192.168.1.0/24"
        $BESubPrefix = "10.254.1.0/24"
        $GWSubPrefix = "192.168.200.0/26"
        $VPNClientAddressPool = "172.16.201.0/24"
        $RG = "TestRG"
        $Location = "East US"
        $DNS = "8.8.8.8"
        $GWName = "VNet1GW"
        $GWIPName = "VNet1GWPIP"
        $GWIPconfName = "gwipconf"

## <a name="a-nameconfigurevnetapart-2---configure-a-vnet"></a><a name="ConfigureVNet"></a>Část 2 – Konfigurace virtuální sítě
1. Vytvořte skupinu prostředků.
   
        New-AzureRmResourceGroup -Name $RG -Location $Location
2. Vytvořte konfigurace podsítí pro virtuální síť, podsítě pojmenujte *FrontEnd*, *BackEnd* a *GatewaySubnet*. Tyto předpony musí být součástí adresního prostoru virtuální sítě deklarovaného výše.
   
        $fesub = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName -AddressPrefix $FESubPrefix
        $besub = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName -AddressPrefix $BESubPrefix
        $gwsub = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName -AddressPrefix $GWSubPrefix
3. Vytvořte virtuální síť. Zadaný server DNS by měl být server DNS, který dokáže přeložit názvy pro prostředky, ke kterým se připojujete. V tomto příkladu jsme použili veřejnou IP adresu. Je potřeba, abyste použili svoje vlastní hodnoty.
   
        New-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $RG -Location $Location -AddressPrefix $VNetPrefix1,$VNetPrefix2 -Subnet $fesub, $besub, $gwsub -DnsServer $DNS
4. Určete proměnné pro virtuální síť, kterou jste vytvořili.
   
        $vnet = Get-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $RG
        $subnet = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet
5. Požádejte o dynamicky přidělovanou veřejnou IP adresu. Tato IP adresa je nezbytná k tomu, aby brána mohla správně fungovat. Později připojíte bránu ke konfiguraci IP brány.
   
        $pip = New-AzureRmPublicIpAddress -Name $GWIPName -ResourceGroupName $RG -Location $Location -AllocationMethod Dynamic
        $ipconf = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName -Subnet $subnet -PublicIpAddress $pip

## <a name="a-namecertificatesapart-3---certificates"></a><a name="Certificates"></a>Část 3 – Certifikáty
Azure používá certifikáty k ověřování klientů VPN pro sítě VPN Point-to-Site. Data veřejného certifikátu (ne privátní klíč) exportujete jako soubor .cer X.509 s kódováním Base-64 buď z kořenového certifikátu vygenerovaného podnikovým certifikačním řešením, nebo z certifikátu podepsaného svým držitelem. Pak naimportujete data veřejného certifikátu z kořenového certifikátu do Azure. Dále je potřeba vygenerovat klientský certifikát z kořenového certifikátu pro klienty. Každý klient, který se chce připojit k virtuální síti pomocí připojení P2S, musí mít nainstalovaný klientský certifikát vygenerovaný z kořenového certifikátu.

### <a name="a-namecerastep-1---obtain-the-cer-file-for-the-root-certificate"></a><a name="cer"></a>Krok 1 – Získání souboru .cer pro kořenový certifikát
Budete muset získat data veřejného certifikátu pro kořenový certifikát, který chcete použít.

* Pokud používáte podnikový certifikační systém, získejte soubor .cer pro kořenový certifikát, který chcete použít. 
* Pokud nepoužíváte podnikové certifikační řešení, musíte vygenerovat kořenový certifikát podepsaný svým držitelem. Doporučeným způsobem vytvoření certifikátu podepsaného svým držitelem pro připojení P2S je použití nástroje makecert. I když je možné vytvořit certifikáty podepsané svým držitelem pomocí prostředí PowerShell, certifikát vytvořený v prostředí PowerShell neobsahuje pole potřebná pro připojení P2S. Kroky pro Windows 10 najdete v tématu [Práce s kořenovými certifikáty podepsanými svým držitelem pro konfigurace Point-to-Site](vpn-gateway-certificates-point-to-site.md).

1. Pokud chcete získat soubor .cer z certifikátu, otevřete soubor **certmgr.msc** a vyhledejte kořenový certifikát. Klikněte pravým tlačítkem na kořenový certifikát podepsaný svým držitelem, klikněte na **Všechny úkoly** a potom na **Exportovat**. Otevře se **Průvodce exportem certifikátu**.
2. V průvodci klikněte na **Další**, vyberte **Ne, neexportovat privátní klíč** a klikněte na **Další**.
3. Na stránce **Formát souboru pro export** vyberte **X.509, kódování Base-64 (CER).** Pak klikněte na **Další**. 
4. V části **Soubor pro export** **přejděte** do umístění, do kterého chcete certifikát vyexportovat. V části **Název souboru** zadejte název souboru. Pak klikněte na tlačítko **Další**.
5. Certifikát vyexportujte kliknutím na **Dokončit**.

### <a name="a-namegenerateastep-2---generate-the-client-certificate"></a><a name="generate"></a>Krok 2 – Vygenerování klientského certifikátu
Dále vygenerujte certifikáty klientů. Můžete buď vygenerovat jedinečný certifikát pro každého klienta, který se připojí, nebo můžete použít stejný certifikát na více klientů. Výhodou generování jedinečných certifikátů pro klienty je možnost v případě potřeby jednotlivý certifikát odvolat. V opačném případě, pokud všichni používají stejný certifikát klienta a zjistíte, že pro jednoho klienta je třeba tento certifikát odvolat, bude nutné vygenerovat a nainstalovat nové certifikáty pro všechny klienty, u nichž je odvolaný certifikát taktéž používán k ověření. Klientské certifikáty jsou dále v tomto cvičení nainstalovány na každý klientský počítač.


####<a name="enterprise-certificate"></a>Podnikový certifikát
- Pokud používáte podnikové certifikační řešení, vygenerujte klientský certifikát s běžným názvem ve formátu 'name@yourdomain.com', (namísto formátu název domény\uživatelské jméno).
- Ujistěte se, že je vámi vydaný certifikát založený na šabloně uživatelského certifikátu, která má jako první položku v seznamu používání Ověření klienta místo Přihlášení pomocí čipové karty atd. Certifikát můžete zkontrolovat dvojím kliknutím na klientský certifikát a zobrazením **Podrobnosti > Použití rozšířeného klíče**.

####<a name="self-signed-certificate"></a>Certifikát podepsaný svým držitelem 
Používáte-li certifikát podepsaný svým držitelem, naleznete informace o vygenerování certifikátu klienta v tématu [Práce s kořenovými certifikáty podepsanými svými držiteli pro účely konfigurace připojení Point-to-Site](vpn-gateway-certificates-point-to-site.md).

### <a name="a-nameexportclientcertastep-3---export-the-client-certificate"></a><a name="exportclientcert"></a>Krok 3 – Export klientského certifikátu
Klientský certifikát je vyžadován pro ověřování. Po vygenerování klientský certifikát vyexportujte. Vyexportovaný klientský certifikát se později nainstaluje na každý klientský počítač.

1. Chcete-li exportovat klientský certifikát, použijte nástroj *certmgr.msc*. Klikněte pravým tlačítkem na klientský certifikát, který chcete exportovat, klikněte na **všechny úlohy**, a poté klikněte na **exportovat**.
2. Exportujte klientský certifikát s privátním klíčem. Jedná se o soubor *.pfx*. Ujistěte se, že jste si poznamenali nebo si pamatujete heslo (klíč), které jste pro tento certifikát nastavili.

### <a name="a-nameuploadastep-4---upload-the-root-certificate-cer-file"></a><a name="upload"></a>Krok 4 – Nahrání souboru .cer kořenového certifikátu
Deklarujte proměnnou pro název certifikátu, který nahradí hodnotu vaší vlastní hodnotou:

        $P2SRootCertName = "Mycertificatename.cer"

Přidejte data veřejného certifikátu z kořenového certifikátu do Azure. Můžete nahrát soubory až 20 kořenových certifikátů. Privátní klíč kořenového certifikátu nebudete nahrávat do Azure. Nahraný soubor .cer se v Azure používá k ověřování klientů, kteří se připojují k virtuální síti. 

Cestu k souboru nahraďte vlastní cestou a potom spusťte rutiny.

        $filePathForCert = "C:\cert\Mycertificatename.cer"
        $cert = new-object System.Security.Cryptography.X509Certificates.X509Certificate2($filePathForCert)
        $CertBase64 = [system.convert]::ToBase64String($cert.RawData)
        $p2srootcert = New-AzureRmVpnClientRootCertificate -Name $P2SRootCertName -PublicCertData $CertBase64

## <a name="a-namecreategatewayapart-4---create-the-vpn-gateway"></a><a name="creategateway"></a>Část 4 – Vytvoření brány VPN gateway
Nakonfigurujte a vytvořte bránu virtuální sítě pro svou virtuální síť. Parametr *-GatewayType* musí mít hodnotu **Vpn** a parametr *-VpnType* musí mít hodnotu **RouteBased**. Dokončení operace může trvat až 45 minut.

        New-AzureRmVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG `
        -Location $Location -IpConfigurations $ipconf -GatewayType Vpn `
        -VpnType RouteBased -EnableBgp $false -GatewaySku Standard `
        -VpnClientAddressPool $VPNClientAddressPool -VpnClientRootCertificates $p2srootcert

## <a name="a-nameclientconfigapart-5---download-the-vpn-client-configuration-package"></a><a name="clientconfig"></a>Část 5 – Stažení konfiguračního balíčku pro klienta VPN
Klienti připojení k Azure s použitím P2S musí mít nainstalovaný certifikát klienta a konfigurační balíček pro klienta VPN. Pro klienty se systémem Windows jsou k dispozici balíčky pro konfiguraci klientů VPN.

Balíček klienta VPN obsahuje informace o konfiguraci pro konfigurování klientského softwaru VPN, který je součástí Windows. Balíček neinstaluje další software. Nastavení jsou specifická pro virtuální síť, ke které se chcete připojit. Seznam podporovaných klientských operačních systémů naleznete v části [Nejčastější dotazy o připojení Point-to-Site](#faq) na konci tohoto článku.

1. Po vytvoření brány můžete stáhnout konfigurační balíček klienta. V tomto příkladu stahujeme balíček pro 64bitové klienty. Pokud chcete stáhnout 32bitového klienta, nahraďte hodnotu Amd64 hodnotou x86. Klienta VPN můžete stáhnout také pomocí portálu Azure Portal.
   
        Get-AzureRmVpnClientPackage -ResourceGroupName $RG `
        -VirtualNetworkGatewayName $GWName -ProcessorArchitecture Amd64
2. Rutina prostředí PowerShell vrátí odkaz URL. Toto je příklad vráceného odkazu URL:
   
        "https://mdsbrketwprodsn1prod.blob.core.windows.net/cmakexe/4a431aa7-b5c2-45d9-97a0-859940069d3f/amd64/4a431aa7-b5c2-45d9-97a0-859940069d3f.exe?sv=2014-02-14&sr=b&sig=jSNCNQ9aUKkCiEokdo%2BqvfjAfyhSXGnRG0vYAv4efg0%3D&st=2016-01-08T07%3A10%3A08Z&se=2016-01-08T08%3A10%3A08Z&sp=r&fileExtension=.exe"
3. Zkopírujte a vložte vrácený odkaz do webového prohlížeče a stáhněte balíček. Pak balíček nainstalujte do klientského počítače. Pokud se zobrazí automaticky otevírané okno filtru SmartScreen, klikněte na **Další informace** a potom na **Přesto spustit**, aby se balíček nainstaloval.
4. Na klientském počítači přejděte do části **Nastavení sítě** a klikněte na možnost **Síť VPN**. Zobrazí se příslušné připojení. Bude uveden název virtuální sítě, ke které se bude počítač připojovat. Název bude vypadat podobně jako tento příklad: 
   
    ![Klient VPN](./media/vpn-gateway-howto-point-to-site-rm-ps/vpn.png)

## <a name="a-nameclientcertificateapart-6---install-the-client-certificate"></a><a name="clientcertificate"></a>Část 6: Instalace klientského certifikátu
Každý klientský počítač musí mít klientský certifikát pro ověření. Při instalaci klientského certifikátu budete potřebovat heslo, které bylo vytvořeno při jeho exportu.

1. Zkopírujte soubor .pfx na klientský počítač.
2. Spusťte instalaci poklikáním na soubor .pfx. Neměňte umístění instalace.

## <a name="a-nameconnectapart-7---connect-to-azure"></a><a name="connect"></a>Část 7: Připojení k Azure
1. Chcete-li se připojit ke své síti VNet, přejděte na klientském počítači na připojení VPN a vyhledejte připojení VPN, které jste vytvořili. Bude mít stejný název jako vaše virtuální síť. Klikněte na **Připojit**. Může se zobrazit místní zpráva týkající se použití certifikátu. Pokud k tomu dojde, klikněte na možnost **Pokračovat**, abyste použili zvýšená oprávnění. 
2. Připojení spustíte kliknutím na tlačítko **Připojit** na stavové stránce **Připojení**. Pokud uvidíte obrazovku **Výběr certifikátu**, ujistěte se, že zobrazený klientský certifikát je ten, který chcete pro připojení použít. Pokud není, vyberte pomocí šipky rozevíracího seznamu správný certifikát, a poté klikněte na **OK**.
   
    ![Připojení klienta VPN k Azure](./media/vpn-gateway-howto-point-to-site-rm-ps/clientconnect.png)
3. Připojení by nyní mělo být navázáno.
   
    ![Vytvořené připojení](./media/vpn-gateway-howto-point-to-site-rm-ps/connected.png)

> [!NOTE]
> Pokud používáte certifikát vydaný řešením CA organizace a máte problémy s ověřováním, zkontrolujte pořadí ověřování na klientském certifikátu. Pořadí seznamu ověřování můžete zkontrolovat dvojím kliknutím na klientský certifikát a přechodem na **Podrobnosti > Použití rozšířeného klíče**. Ujistěte se, že je na prvním místě seznamu položka Ověření klienta. Pokud tomu tak není, musíte vydat klientský certifikát založený na šabloně uživatele, která má Ověření klienta jako první položku seznamu. 
>
>

## <a name="a-nameverifyapart-8---verify-your-connection"></a><a name="verify"></a>Část 8 – Ověření připojení
1. Chcete-li ověřit, zda je připojení VPN aktivní, v příkazovém řádku se zvýšenými oprávněními spusťte příkaz *ipconfig/all*.
2. Zkontrolujte výsledky. Všimněte si, že IP adresa, kterou jste obdrželi, je jedna z adres z fondu adres klienta VPN připojení Point-to-Site, který jste určili během konfigurace. Výsledek by se měl podobat následujícímu příkladu:
   
        PPP adapter VNet1:
            Connection-specific DNS Suffix .:
            Description.....................: VNet1
            Physical Address................:
            DHCP Enabled....................: No
            Autoconfiguration Enabled.......: Yes
            IPv4 Address....................: 172.16.201.3(Preferred)
            Subnet Mask.....................: 255.255.255.255
            Default Gateway.................:
            NetBIOS over Tcpip..............: Enabled

## <a name="a-nameaddremovecertato-add-or-remove-a-trusted-root-certificate"></a><a name="addremovecert"></a>Postup přidání a odebrání důvěryhodného kořenového certifikátu
Certifikáty slouží k ověřování klientů VPN pro sítě Point-to-Site VPN. Následující postup vás provede procesem přidávání a odebírání kořenových certifikátů. Pokud do Azure přidáte soubor X.509 zakódovaný ve formátu Base64 (.cer), oznamujete tím Azure, že má důvěřovat kořenovému certifikátu, který tento soubor představuje. 

Důvěryhodné kořenové certifikáty můžete přidávat nebo odebírat prostřednictvím prostředí PowerShell nebo v rámci webu Azure Portal. Chcete-li tuto operaci provést prostřednictvím webu Azure Portal, přejděte do části **brána virtuální sítě > Nastavení > Konfigurace Point-to-Site (P2S) > Kořenové certifikáty**. Následující kroky vás provedou těmito úkoly v rámci prostředí PowerShell. 

### <a name="add-a-trusted-root-certificate"></a>Přidání důvěryhodného kořenového certifikátu
Do Azure můžete přidat až 20 souborů .cer s důvěryhodnými kořenovými certifikáty. Kořenový certifikát přidáte pomocí následujícího postupu.

1. Vytvořte a připravte nový kořenový certifikát, který přidáte do Azure. Exportujte veřejný klíč ve formátu X.509, kódování Base-64 (CER) a otevřete jej v textovém editoru. Pak zkopírujte pouze část uvedenou níže. 
   
    Zkopírujte hodnoty, jak je znázorněno v následujícím příkladu:
   
    ![certifikát](./media/vpn-gateway-howto-point-to-site-rm-ps/copycert.png)

    > [!NOTE]
    > Při kopírování dat certifikátu se ujistěte, že kopírujete text jako jeden souvislý řádek bez konců řádků nebo odřádkování. Pokud chcete zobrazit konce řádků a odřádkování, může být nutné upravit zobrazení v textovém editoru na Zobrazit symbol / Zobrazit všechny znaky.                                                                                                                                                                            
    >


2. Zadejte název certifikátu a informace o klíči jako proměnnou. Nahraďte tyto informace svými vlastními, jak je ukázáno na následujícím příkladu:
   
        $P2SRootCertName2 = "ARMP2SRootCert2.cer"
        $MyP2SCertPubKeyBase64_2 = "MIIC/zCCAeugAwIBAgIQKazxzFjMkp9JRiX+tkTfSzAJBgUrDgMCHQUAMBgxFjAUBgNVBAMTDU15UDJTUm9vdENlcnQwHhcNMTUxMjE5MDI1MTIxWhcNMzkxMjMxMjM1OTU5WjAYMRYwFAYDVQQDEw1NeVAyU1Jvb3RDZXJ0MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAyjIXoWy8xE/GF1OSIvUaA0bxBjZ1PJfcXkMWsHPzvhWc2esOKrVQtgFgDz4ggAnOUFEkFaszjiHdnXv3mjzE2SpmAVIZPf2/yPWqkoHwkmrp6BpOvNVOpKxaGPOuK8+dql1xcL0eCkt69g4lxy0FGRFkBcSIgVTViS9wjuuS7LPo5+OXgyFkAY3pSDiMzQCkRGNFgw5WGMHRDAiruDQF1ciLNojAQCsDdLnI3pDYsvRW73HZEhmOqRRnJQe6VekvBYKLvnKaxUTKhFIYwuymHBB96nMFdRUKCZIiWRIy8Hc8+sQEsAML2EItAjQv4+fqgYiFdSWqnQCPf/7IZbotgQIDAQABo00wSzBJBgNVHQEEQjBAgBAkuVrWvFsCJAdK5pb/eoCNoRowGDEWMBQGA1UEAxMNTXlQMlNSb290Q2VydIIQKazxzFjMkp9JRiX+tkTfSzAJBgUrDgMCHQUAA4IBAQA223veAZEIar9N12ubNH2+HwZASNzDVNqspkPKD97TXfKHlPlIcS43TaYkTz38eVrwI6E0yDk4jAuPaKnPuPYFRj9w540SvY6PdOUwDoEqpIcAVp+b4VYwxPL6oyEQ8wnOYuoAK1hhh20lCbo8h9mMy9ofU+RP6HJ7lTqupLfXdID/XevI8tW6Dm+C/wCeV3EmIlO9KUoblD/e24zlo3YzOtbyXwTIh34T0fO/zQvUuBqZMcIPfM1cDvqcqiEFLWvWKoAnxbzckye2uk1gHO52d8AVL3mGiX8wBJkjc/pMdxrEvvCzJkltBmqxTM6XjDJALuVh16qFlqgTWCIcb7ju"
3. Přidejte nový kořenový certifikát. Nelze přidat více certifikátů současně.
   
        Add-AzureRmVpnClientRootCertificate -VpnClientRootCertificateName $P2SRootCertName2 -VirtualNetworkGatewayname "VNet1GW" -ResourceGroupName "TestRG" -PublicCertData $MyP2SCertPubKeyBase64_2
4. Pomocí následující rutiny můžete ověřit, zda byl nový certifikát přidán správně.
   
        Get-AzureRmVpnClientRootCertificate -ResourceGroupName "TestRG" `
        -VirtualNetworkGatewayName "VNet1GW"

### <a name="to-remove-a-trusted-root-certificate"></a>Odebrání důvěryhodného kořenového certifikátu
Důvěryhodný kořenový certifikát můžete z Azure odebrat. Pokud odeberete důvěryhodný certifikát, nebude již možné se pomocí klientských certifikátů vygenerovaných z tohoto kořenového certifikátu připojit k Azure prostřednictvím připojení Point-to-Site. Pokud chcete, aby se klienti mohli i nadále připojovat, je nutné nainstalovat nový certifikát, který je vygenerován z certifikátu, který Azure považuje za důvěryhodný.

1. Chcete-li odebrat důvěryhodný kořenový certifikát, upravte následující příklad:
   
    Deklarujte proměnné.
   
        $P2SRootCertName2 = "ARMP2SRootCert2.cer"
        $MyP2SCertPubKeyBase64_2 = "MIIC/zCCAeugAwIBAgIQKazxzFjMkp9JRiX+tkTfSzAJBgUrDgMCHQUAMBgxFjAUBgNVBAMTDU15UDJTUm9vdENlcnQwHhcNMTUxMjE5MDI1MTIxWhcNMzkxMjMxMjM1OTU5WjAYMRYwFAYDVQQDEw1NeVAyU1Jvb3RDZXJ0MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAyjIXoWy8xE/GF1OSIvUaA0bxBjZ1PJfcXkMWsHPzvhWc2esOKrVQtgFgDz4ggAnOUFEkFaszjiHdnXv3mjzE2SpmAVIZPf2/yPWqkoHwkmrp6BpOvNVOpKxaGPOuK8+dql1xcL0eCkt69g4lxy0FGRFkBcSIgVTViS9wjuuS7LPo5+OXgyFkAY3pSDiMzQCkRGNFgw5WGMHRDAiruDQF1ciLNojAQCsDdLnI3pDYsvRW73HZEhmOqRRnJQe6VekvBYKLvnKaxUTKhFIYwuymHBB96nMFdRUKCZIiWRIy8Hc8+sQEsAML2EItAjQv4+fqgYiFdSWqnQCPf/7IZbotgQIDAQABo00wSzBJBgNVHQEEQjBAgBAkuVrWvFsCJAdK5pb/eoCNoRowGDEWMBQGA1UEAxMNTXlQMlNSb290Q2VydIIQKazxzFjMkp9JRiX+tkTfSzAJBgUrDgMCHQUAA4IBAQA223veAZEIar9N12ubNH2+HwZASNzDVNqspkPKD97TXfKHlPlIcS43TaYkTz38eVrwI6E0yDk4jAuPaKnPuPYFRj9w540SvY6PdOUwDoEqpIcAVp+b4VYwxPL6oyEQ8wnOYuoAK1hhh20lCbo8h9mMy9ofU+RP6HJ7lTqupLfXdID/XevI8tW6Dm+C/wCeV3EmIlO9KUoblD/e24zlo3YzOtbyXwTIh34T0fO/zQvUuBqZMcIPfM1cDvqcqiEFLWvWKoAnxbzckye2uk1gHO52d8AVL3mGiX8wBJkjc/pMdxrEvvCzJkltBmqxTM6XjDJALuVh16qFlqgTWCIcb7ju"
2. Odeberte certifikát.
   
        Remove-AzureRmVpnClientRootCertificate -VpnClientRootCertificateName $P2SRootCertName2 -VirtualNetworkGatewayName $GWName -ResourceGroupName $RG -PublicCertData $MyP2SCertPubKeyBase64_2
3. Pomocí následující rutiny můžete ověřit, zda byl certifikát úspěšně odebrán. 
   
        Get-AzureRmVpnClientRootCertificate -ResourceGroupName "TestRG" `
        -VirtualNetworkGatewayName "VNet1GW"

## <a name="a-namerevokeato-manage-the-list-of-revoked-client-certificates"></a><a name="revoke"></a>Správa seznamu odvolaných klientských certifikátů
Certifikáty klientů lze odvolat. Seznam odvolaných certifikátů umožňuje selektivně odepřít připojení Point-to-Site na základě jednotlivých klientských certifikátů. Pokud odeberete z Azure kořenový certifikát v souboru .cer, dojde k odvolání přístupu pro všechny klientské certifikáty, které byly tímto odvolaným kořenovým certifikátem vygenerovány nebo podepsány. Pokud chcete odvolat konkrétní klientský (nikoli kořenový) certifikát, můžete tak učinit. Při použití tohoto postupu zůstanou ostatní certifikáty, které byly vygenerovány z daného kořenového certifikátu, nadále v platnosti.

Běžnou praxí je použití kořenového certifikátu pro řízení přístupu na úrovni týmu nebo organizace, přičemž odvolání klientských certifikátů slouží pro detailní kontrolu přístupu jednotlivých uživatelů.

### <a name="revoke-a-client-certificate"></a>Odvolání klientského certifikátu
1. Získejte kryptografický otisk klientského certifikátu, který chcete odvolat.
   
        $RevokedClientCert1 = "ClientCert1"
        $RevokedThumbprint1 = "?ef2af033d0686820f5a3c74804d167b88b69982f"
2. Tento otisk přidejte do seznamu odvolaných kryptografických otisků.
   
        Add-AzureRmVpnClientRevokedCertificate -VpnClientRevokedCertificateName $RevokedClientCert1 `
        -VirtualNetworkGatewayName $GWName -ResourceGroupName $RG -Thumbprint $RevokedThumbprint1
3. Zkontrolujte, zda byl daný kryptografický otisk přidán do seznamu odvolaných certifikátů. Nelze přidat více kryptografických otisků současně.
   
        Get-AzureRmVpnClientRevokedCertificate -VirtualNetworkGatewayName $GWName -ResourceGroupName $RG

### <a name="reinstate-a-client-certificate"></a>Obnovení klientského certifikátu
Klientský certifikát lze obnovit odebráním jeho kryptografického otisku ze seznamu odvolaných klientských certifikátů.

1. Kryptografický otisk odeberte ze seznamu otisků odvolaných klientských certifikátů.
   
       Remove-AzureRmVpnClientRevokedCertificate -VpnClientRevokedCertificateName $RevokedClientCert1 `
       -VirtualNetworkGatewayName $GWName -ResourceGroupName $RG -Thumbprint $RevokedThumbprint1
2. Zkontrolujte, za byl kryptografický otisk ze seznamu odebrán.
   
        Get-AzureRmVpnClientRevokedCertificate -VirtualNetworkGatewayName $GWName -ResourceGroupName $RG
## <a name="a-namefaqapoint-to-site-faq"></a><a name="faq"></a>Nejčastější dotazy týkající se připojení Point-to-Site

[!INCLUDE [Point-to-Site FAQ](../../includes/vpn-gateway-point-to-site-faq-include.md)]

## <a name="next-steps"></a>Další kroky
Po dokončení připojení můžete do virtuálních sítí přidávat virtuální počítače. Další informace najdete v tématu [Virtuální počítače](https://docs.microsoft.com/azure/#pivot=services&panel=Compute).




<!--HONumber=Feb17_HO3-->


