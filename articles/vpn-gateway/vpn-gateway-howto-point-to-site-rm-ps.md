<properties 
   pageTitle="Konfigurace připojení brány VPN typu Point-to-Site k virtuální síti pomocí modelu nasazení Resource Manager | Microsoft Azure"
   description="Připojte se bezpečně ke službě Azure Virtual Network vytvořením připojení brány VPN typu Point-to-Site."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"/>
<tags 
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/31/2016"
   ms.author="cherylmc" />

# Konfigurace připojení Point-to-Site k virtuální síti pomocí prostředí PowerShell

> [AZURE.SELECTOR]
- [PowerShell – Resource Manager](vpn-gateway-howto-point-to-site-rm-ps.md)
- [Klasický portál](vpn-gateway-point-to-site-create.md)

Konfigurace Point-to-Site (P2S) umožňuje vytvořit zabezpečené připojení jednotlivých klientských počítačů k virtuální sítí. Připojení P2S je užitečné, když se chcete ke své virtuální síti připojit ze vzdáleného umístění, například z domova nebo z místa konání konference, nebo když máte jen několik klientů, kteří se potřebují připojovat k virtuální síti. 

Tento článek vás provede vytvořením virtuální sítě s připojením Point-to-Site v **modelu nasazení Resource Manager**. Kroky vyžadují prostředí PowerShell. V současné době nelze toto řešení vytvořit v rámci webu Azure Portal v plném rozsahu.

Připojení typu Point-to-Site k fungování nevyžadují zařízení VPN ani veřejnou IP adresu. Připojení VPN je vytvořeno spuštěním připojení na klientském počítači. Další informace o připojeních typu Point-to-Site najdete v tématech [Služba VPN Gateway – nejčastější dotazy](vpn-gateway-vpn-faq.md#point-to-site-connections) a [Plánování a navrhování](vpn-gateway-plan-design.md).



**O modelech nasazení Azure**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

**Modely nasazení a nástroje pro připojení typu Point-to-Site**

[AZURE.INCLUDE [vpn-gateway-table-point-to-site](../../includes/vpn-gateway-table-point-to-site-include.md)] 

![Diagram Point-to-Site](./media/vpn-gateway-howto-point-to-site-rm-ps/p2srm.png "point-to-site")


## O této konfiguraci

V tomto scénáři vytvoříte virtuální síť s připojením Point-to-Site. Pokyny vám také pomůžou vygenerovat certifikáty, které jsou požadovány pro tuto konfiguraci. Připojení P2S se skládá z následujících položek: virtuální sítě s bránou sítě VPN, kořenového certifikátu v souboru .cer (veřejný klíč), certifikátu klienta a konfigurace sítě VPN v klientu. 

Pro tuto konfiguraci používáme následující hodnoty. Nastavíme proměnné uvedené v části [1](#declare) článku. Můžete buď využít kroky jako podrobný postup a převzít hodnoty beze jejich změny, nebo je změnit tak, aby odpovídaly vašemu prostředí. 

- Název: **VNet1**, síť používá adresní prostory **192.168.0.0/16** a **10.254.0.0/16**. Všimněte si, že pro jednu síť VNet lze použít více adresních prostorů.
- Název podsítě: **FrontEnd**, používá prostor **192.168.1.0/24**
- Název podsítě: **BackEnd**, používá prostor **10.254.1.0/24**
- Název podsítě: **GatewaySubnet**, používá prostor **192.168.200.0/24**. Název podsítě *GatewaySubnet* je pro správnou funkci brány povinný. 
- Fond adres klienta VPN: **172.16.201.0/24**. Klienti VPN, kteří se budou k síti VNet připojovat pomocí tohoto připojení Point-to-Site, dostanou IP adresu z tohoto fondu.
- Předplatné: Ověřte, zda máte správné předplatné, pokud máte více než jedno.
- Skupina prostředků: **TestRG**
- Umístění: **Východní USA**
- Server DNS: **IP adresa** serveru DNS, který chcete používat pro překlad názvů.
- Název brány: **GW**
- Název veřejné IP adresy: **GWIP**
- VpnType: **RouteBased**


## Před zahájením

- Ověřte, že máte předplatné Azure. Pokud ještě nemáte předplatné Azure, můžete si aktivovat [výhody pro předplatitele MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) nebo si zaregistrovat [bezplatný účet](https://azure.microsoft.com/pricing/free-trial/).
    
- Nainstalujte rutiny prostředí PowerShell pro Azure Resource Manager (verze 1.0.2 nebo novější). Další informace o instalaci rutin prostředí PowerShell najdete v tématu [Instalace a konfigurace Azure PowerShellu](../powershell-install-configure.md). Při práci s prostředím PowerShell pro tuto konfiguraci se ujistěte, že jej spouštíte jako správce. 

## <a name="declare"></a>Část 1 – Přihlášení a nastavení proměnných

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
        $GWName = "GW"
        $GWIPName = "GWIP"
        $GWIPconfName = "gwipconf"
        $P2SRootCertName = "ARMP2SRootCert.cer"


## Část 2 – Konfigurace virtuální sítě 


1. Vytvořte skupinu prostředků.

        New-AzureRmResourceGroup -Name $RG -Location $Location

2. Vytvořte konfigurace podsítí pro virtuální síť, podsítě pojmenujte *FrontEnd*, *BackEnd* a *GatewaySubnet*. Tyto předpony musí být součástí adresního prostoru sítě VNet deklarovaného výše.

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

## Část 3 – Přidání důvěryhodných certifikátů

Azure ověřuje klienty s požadavkem na připojení prostřednictvím P2S pomocí certifikátů. Importujete soubor .cer (veřejný klíč) pro kořenový certifikát do Azure. Pokud do Azure přidáte soubor X.509 zakódovaný ve formátu Base64 (.cer), oznamujete tím Azure, že má důvěřovat kořenovému certifikátu, který tento soubor představuje.

Pokud používáte podnikové řešení, můžete použít stávající řetěz certifikátů. Pokud nepoužíváte podnikové řešení certifikační autority, můžete vytvořit kořenový certifikát podepsaný svým držitelem. Jedním ze způsobů vytvoření certifikátu podepsaného svým držitelem je použití nástroje makecert. Pokyny k vytvoření kořenového certifikátu podepsaného svým držitelem pomocí nástroje *makecert* naleznete v části [Práce s kořenovými certifikáty podepsanými svými držiteli pro účely konfigurace připojení Point-to-Site](vpn-gateway-certificates-point-to-site.md).

Bez ohledu na to, jak certifikát získáte, nahrajte soubor .cer certifikátu do Azure a vygenerujte certifikáty klienta pro instalaci v klientech, které chcete připojit. Certifikát podepsaný svým držitelem neinstalujte přímo do klienta. Certifikáty klientů vygenerujete později v části [Konfigurace klienta](#cc) v tomto článku.
        
Do Azure můžete přidat až 20 důvěryhodných certifikátů. Chcete-li získat veřejný klíč, exportujte certifikát do souboru X.509 (.cer) zakódovaného ve formátu Base64. Jedním ze způsobů exportování do souboru .cer je použití nástroje **certmger.msc**. Certifikáty vyhledejte v části Osobní/certifikáty. Klikněte pravým tlačítkem myši a proveďte export bez soukromého klíče ve formátu „X.509, kódování Base-64 (CER)“. Poznamenejte si cestu k souboru, kam jste provedli export do souboru .cer. Toto je vzorek získání řetězcové reprezentace vašeho certifikátu ve formátu Base64. 

Přidejte důvěryhodný certifikát do Azure. V tomto kroku budete muset použít cestu ke svému vlastnímu souboru .cer. Věnujte zvláštní pozornost proměnné $P2SRootCertName = "ARMP2SRootCert.cer", kterou jste nastavili v části 1 tohoto článku. Pokud se název vašeho certifikátu liší, upravte proměnnou příslušným způsobem.
    
        $filePathForCert = "pasteYourCerFilePathHere"
        $cert = new-object System.Security.Cryptography.X509Certificates.X509Certificate2($filePathForCert)
        $CertBase64 = [system.convert]::ToBase64String($cert.RawData)
        $p2srootcert = New-AzureRmVpnClientRootCertificate -Name $P2SRootCertName -PublicCertData $CertBase64

## Část 4 – Vytvoření brány sítě VPN

Nakonfigurujte a vytvořte bránu virtuální sítě pro svou virtuální síť. Parametr *-GatewayType* musí mít hodnotu **Vpn** a parametr *-VpnType* musí mít hodnotu **RouteBased**. Dokončení operace může trvat až 45 minut.

        New-AzureRmVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG `
        -Location $Location -IpConfigurations $ipconf -GatewayType Vpn `
        -VpnType RouteBased -EnableBgp $false -GatewaySku Standard `
        -VpnClientAddressPool $VPNClientAddressPool -VpnClientRootCertificates $p2srootcert

## Část 5 – Stažení konfiguračního balíčku pro klienta VPN

Klienti připojení k Azure s použitím P2S musí mít nainstalovaný certifikát klienta a konfigurační balíček pro klienta VPN. Pro klienty se systémem Windows jsou k dispozici balíčky pro konfiguraci klientů VPN. Balíček klienta VPN obsahuje informace potřebné pro konfiguraci softwaru klienta VPN, který je součástí systému Windows a je specifický pro síť VPN, ke které se chcete připojit. Balíček neinstaluje další software. Další informace viz téma [Služba VPN Gateway – nejčastější dotazy](vpn-gateway-vpn-faq.md#point-to-site-connections).

1. Po vytvoření brány stáhněte konfigurační balíček klienta podle následujícího příkladu:

        Get-AzureRmVpnClientPackage -ResourceGroupName $RG `
        -VirtualNetworkGatewayName $GWName -ProcessorArchitecture Amd64

2. Rutina prostředí PowerShell vrátí odkaz URL. Toto je příklad vráceného odkazu URL:

        "https://mdsbrketwprodsn1prod.blob.core.windows.net/cmakexe/4a431aa7-b5c2-45d9-97a0-859940069d3f/amd64/4a431aa7-b5c2-45d9-97a0-859940069d3f.exe?sv=2014-02-14&sr=b&sig=jSNCNQ9aUKkCiEokdo%2BqvfjAfyhSXGnRG0vYAv4efg0%3D&st=2016-01-08T07%3A10%3A08Z&se=2016-01-08T08%3A10%3A08Z&sp=r&fileExtension=.exe"

3. Zkopírujte a vložte vrácený odkaz do webového prohlížeče a stáhněte balíček. Pak balíček nainstalujte do klientského počítače.

4. Na klientském počítači přejděte do části **Nastavení sítě** a klikněte na možnost **Síť VPN**. Zobrazí se příslušné připojení. Bude uveden název virtuální sítě, ke kterému se bude počítač připojovat. Název bude vypadat podobně jako tento: 

    ![Klient VPN](./media/vpn-gateway-howto-point-to-site-rm-ps/vpn.png "VPN client")

## <a name="cc"></a>Část 6 – Vygenerování klientského certifikátu

Dále vygenerujte certifikáty klientů. Můžete buď vygenerovat jedinečný certifikát pro každého klienta, který se připojí, nebo můžete použít stejný certifikát na více klientů. Výhodou generování jedinečných certifikátů pro klienty je možnost v případě potřeby jednotlivý certifikát odvolat. V opačném případě, pokud všichni používají stejný certifikát klienta a zjistíte, že pro jednoho klienta je třeba tento certifikát odvolat, bude nutné vygenerovat a nainstalovat nové certifikáty pro všechny klienty, u nichž je odvolaný certifikát taktéž používán k ověření.

- Pokud používáte podnikové certifikační řešení, vygenerujte certifikát klienta se společným názvem ve formátu „jmeno@vasedomena.com", spíše než formátu NetBIOS „Doména\uživatelské jméno“. 

- Používáte-li certifikát podepsaný svým držitelem, naleznete informace o vygenerování certifikátu klienta v tématu [Práce s kořenovými certifikáty podepsanými svými držiteli pro účely konfigurace připojení Point-to-Site](vpn-gateway-certificates-point-to-site.md).

## Část 7 – Instalace klientského certifikátu

Nainstalujte certifikát klienta v každém počítači, který se má připojovat k virtuální síti. Klientský certifikát je vyžadován pro ověřování. Instalaci klientského certifikátu lze buď automatizovat, nebo ji lze provádět ručně. Následující kroky vás provedou procesem exportu a ruční instalace klientského certifikátu.

1. Chcete-li exportovat klientský certifikát, použijte nástroj *certmgr.msc*. Klikněte pravým tlačítkem na klientský certifikát, který chcete exportovat, klikněte na **všechny úlohy**, a poté klikněte na **exportovat**.
2. Exportujte klientský certifikát s privátním klíčem. Jedná se o soubor *.pfx*. Ujistěte se, že jste si poznamenali nebo si pamatujete heslo (klíč), které jste pro tento certifikát nastavili.
3. Zkopírujte soubor *.pfx* na klientský počítač. Na klientském počítači spusťte instalaci dvojím kliknutím na soubor *.pfx*. Až budete vyzváni, zadejte heslo. Neměňte umístění instalace.


## Část 8 – Připojení k Azure

1. Chcete-li se připojit ke své síti VNet, přejděte na klientském počítači na připojení VPN a vyhledejte připojení VPN, které jste vytvořili. Bude mít stejný název jako vaše virtuální síť. Klikněte na **Připojit**. Může se zobrazit místní zpráva týkající se použití certifikátu. Pokud k tomu dojde, klikněte na možnost **Pokračovat**, abyste použili zvýšená oprávnění. 

2. Připojení spustíte kliknutím na tlačítko **Připojit** na stavové stránce **Připojení**. Pokud uvidíte obrazovku **Výběr certifikátu**, ujistěte se, že zobrazený klientský certifikát je ten, který chcete pro připojení použít. Pokud není, vyberte pomocí šipky rozevíracího seznamu správný certifikát, a poté klikněte na **OK**.

    ![Klient VPN 2](./media/vpn-gateway-howto-point-to-site-rm-ps/clientconnect.png "VPN client connection")

3. Připojení by nyní mělo být navázáno.

    ![Klient VPN 3](./media/vpn-gateway-howto-point-to-site-rm-ps/connected.png "VPN client connection 2")

## Část 9 – Ověření připojení

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

## Postup přidání a odebrání důvěryhodného kořenového certifikátu

Certifikáty slouží k ověřování klientů VPN pro sítě Point-to-Site VPN. Následující postup vás provede procesem přidávání a odebírání kořenových certifikátů. Pokud do Azure přidáte soubor X.509 zakódovaný ve formátu Base64 (.cer), oznamujete tím Azure, že má důvěřovat kořenovému certifikátu, který tento soubor představuje. 

Důvěryhodné kořenové certifikáty můžete přidávat nebo odebírat prostřednictvím prostředí PowerShell nebo v rámci webu Azure Portal. Chcete-li tuto operaci provést prostřednictvím webu Azure Portal, přejděte do části **brána virtuální sítě > Nastavení > Konfigurace Point-to-Site (P2S) > Kořenové certifikáty**. Následující kroky vás provedou těmito úkoly v rámci prostředí PowerShell. 

### Přidání důvěryhodného kořenového certifikátu

Do Azure můžete přidat až 20 souborů .cer s důvěryhodnými kořenovými certifikáty. Kořenový certifikát přidáte pomocí následujícího postupu.

1. Vytvořte a připravte nový kořenový certifikát, který přidáte do Azure. Exportujte veřejný klíč ve formátu X.509, kódování Base-64 (CER) a otevřete jej v textovém editoru. Pak zkopírujte pouze část uvedenou níže. 
 
    Zkopírujte hodnoty, jak je znázorněno v následujícím příkladu.

    ![certifikát](./media/vpn-gateway-howto-point-to-site-rm-ps/copycert.png "certificate")
    
2. V následujícím příkladu zadejte název certifikátu a informace o klíči jako proměnnou. Nahraďte informace vlastními údaji.

        $P2SRootCertName2 = "ARMP2SRootCert2.cer"
        $MyP2SCertPubKeyBase64_2 = "MIIC/zCCAeugAwIBAgIQKazxzFjMkp9JRiX+tkTfSzAJBgUrDgMCHQUAMBgxFjAUBgNVBAMTDU15UDJTUm9vdENlcnQwHhcNMTUxMjE5MDI1MTIxWhcNMzkxMjMxMjM1OTU5WjAYMRYwFAYDVQQDEw1NeVAyU1Jvb3RDZXJ0MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAyjIXoWy8xE/GF1OSIvUaA0bxBjZ1PJfcXkMWsHPzvhWc2esOKrVQtgFgDz4ggAnOUFEkFaszjiHdnXv3mjzE2SpmAVIZPf2/yPWqkoHwkmrp6BpOvNVOpKxaGPOuK8+dql1xcL0eCkt69g4lxy0FGRFkBcSIgVTViS9wjuuS7LPo5+OXgyFkAY3pSDiMzQCkRGNFgw5WGMHRDAiruDQF1ciLNojAQCsDdLnI3pDYsvRW73HZEhmOqRRnJQe6VekvBYKLvnKaxUTKhFIYwuymHBB96nMFdRUKCZIiWRIy8Hc8+sQEsAML2EItAjQv4+fqgYiFdSWqnQCPf/7IZbotgQIDAQABo00wSzBJBgNVHQEEQjBAgBAkuVrWvFsCJAdK5pb/eoCNoRowGDEWMBQGA1UEAxMNTXlQMlNSb290Q2VydIIQKazxzFjMkp9JRiX+tkTfSzAJBgUrDgMCHQUAA4IBAQA223veAZEIar9N12ubNH2+HwZASNzDVNqspkPKD97TXfKHlPlIcS43TaYkTz38eVrwI6E0yDk4jAuPaKnPuPYFRj9w540SvY6PdOUwDoEqpIcAVp+b4VYwxPL6oyEQ8wnOYuoAK1hhh20lCbo8h9mMy9ofU+RP6HJ7lTqupLfXdID/XevI8tW6Dm+C/wCeV3EmIlO9KUoblD/e24zlo3YzOtbyXwTIh34T0fO/zQvUuBqZMcIPfM1cDvqcqiEFLWvWKoAnxbzckye2uk1gHO52d8AVL3mGiX8wBJkjc/pMdxrEvvCzJkltBmqxTM6XjDJALuVh16qFlqgTWCIcb7ju"


3. Přidejte nový kořenový certifikát. Nelze přidat více certifikátů současně.

        Add-AzureRmVpnClientRootCertificate -VpnClientRootCertificateName $P2SRootCertName2 -VirtualNetworkGatewayname "GW" -ResourceGroupName "TestRG" -PublicCertData $MyP2SCertPubKeyBase64_2


4. Pomocí následující rutiny můžete ověřit, zda byl nový certifikát přidán správně.

        Get-AzureRmVpnClientRootCertificate -ResourceGroupName "TestRG" `
        -VirtualNetworkGatewayName "GW"

### Odebrání důvěryhodného kořenového certifikátu

Důvěryhodný kořenový certifikát můžete z Azure odebrat. Pokud odeberete důvěryhodný certifikát, nebude již možné se pomocí klientských certifikátů vygenerovaných z tohoto kořenového certifikátu připojit k Azure prostřednictvím připojení Point-to-Site. Pokud chcete, aby se klienti mohli i nadále připojovat, je nutné nainstalovat nový certifikát, který je vygenerován z certifikátu, který Azure považuje za důvěryhodný.

1. Chcete-li odebrat důvěryhodný kořenový certifikát, upravte následující příklad:

    Deklarujte proměnné.

        $P2SRootCertName2 = "ARMP2SRootCert2.cer"
        $MyP2SCertPubKeyBase64_2 = "MIIC/zCCAeugAwIBAgIQKazxzFjMkp9JRiX+tkTfSzAJBgUrDgMCHQUAMBgxFjAUBgNVBAMTDU15UDJTUm9vdENlcnQwHhcNMTUxMjE5MDI1MTIxWhcNMzkxMjMxMjM1OTU5WjAYMRYwFAYDVQQDEw1NeVAyU1Jvb3RDZXJ0MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAyjIXoWy8xE/GF1OSIvUaA0bxBjZ1PJfcXkMWsHPzvhWc2esOKrVQtgFgDz4ggAnOUFEkFaszjiHdnXv3mjzE2SpmAVIZPf2/yPWqkoHwkmrp6BpOvNVOpKxaGPOuK8+dql1xcL0eCkt69g4lxy0FGRFkBcSIgVTViS9wjuuS7LPo5+OXgyFkAY3pSDiMzQCkRGNFgw5WGMHRDAiruDQF1ciLNojAQCsDdLnI3pDYsvRW73HZEhmOqRRnJQe6VekvBYKLvnKaxUTKhFIYwuymHBB96nMFdRUKCZIiWRIy8Hc8+sQEsAML2EItAjQv4+fqgYiFdSWqnQCPf/7IZbotgQIDAQABo00wSzBJBgNVHQEEQjBAgBAkuVrWvFsCJAdK5pb/eoCNoRowGDEWMBQGA1UEAxMNTXlQMlNSb290Q2VydIIQKazxzFjMkp9JRiX+tkTfSzAJBgUrDgMCHQUAA4IBAQA223veAZEIar9N12ubNH2+HwZASNzDVNqspkPKD97TXfKHlPlIcS43TaYkTz38eVrwI6E0yDk4jAuPaKnPuPYFRj9w540SvY6PdOUwDoEqpIcAVp+b4VYwxPL6oyEQ8wnOYuoAK1hhh20lCbo8h9mMy9ofU+RP6HJ7lTqupLfXdID/XevI8tW6Dm+C/wCeV3EmIlO9KUoblD/e24zlo3YzOtbyXwTIh34T0fO/zQvUuBqZMcIPfM1cDvqcqiEFLWvWKoAnxbzckye2uk1gHO52d8AVL3mGiX8wBJkjc/pMdxrEvvCzJkltBmqxTM6XjDJALuVh16qFlqgTWCIcb7ju"

2. Odeberte certifikát.

        Remove-AzureRmVpnClientRootCertificate -VpnClientRootCertificateName $P2SRootCertName2 -VirtualNetworkGatewayName $GWName -ResourceGroupName $RG -PublicCertData $MyP2SCertPubKeyBase64_2
 
3. Pomocí následující rutiny můžete ověřit, zda byl certifikát úspěšně odebrán. 

        Get-AzureRmVpnClientRootCertificate -ResourceGroupName "TestRG" `
        -VirtualNetworkGatewayName "GW"

## Správa seznamu odvolaných certifikátů klienta

Certifikáty klientů lze odvolat. Seznam odvolaných certifikátů umožňuje selektivně odepřít připojení Point-to-Site na základě jednotlivých klientských certifikátů. Pokud odeberete z Azure kořenový certifikát v souboru .cer, dojde k odvolání přístupu pro všechny klientské certifikáty, které byly tímto odvolaným kořenovým certifikátem vygenerovány nebo podepsány. Pokud chcete odvolat konkrétní klientský (nikoli kořenový) certifikát, můžete tak učinit. Při použití tohoto postupu zůstanou ostatní certifikáty, které byly vygenerovány z daného kořenového certifikátu, nadále v platnosti. Běžnou praxí je použití kořenového certifikátu pro řízení přístupu na úrovni týmu nebo organizace, přičemž odvolání klientských certifikátů slouží pro detailní kontrolu přístupu jednotlivých uživatelů.

### Odvolání klientského certifikátu

1. Získejte kryptografický otisk klientského certifikátu, který chcete odvolat.

        $RevokedClientCert1 = "ClientCert1"
        $RevokedThumbprint1 = "‎ef2af033d0686820f5a3c74804d167b88b69982f"

2. Tento otisk přidejte do seznamu odvolaných kryptografických otisků.

        Add-AzureRmVpnClientRevokedCertificate -VpnClientRevokedCertificateName $RevokedClientCert1 `
        -VirtualNetworkGatewayName $GWName -ResourceGroupName $RG -Thumbprint $RevokedThumbprint1

3. Zkontrolujte, zda byl daný kryptografický otisk přidán do seznamu odvolaných certifikátů. Nelze přidat více kryptografických otisků současně.

        Get-AzureRmVpnClientRevokedCertificate -VirtualNetworkGatewayName $GWName -ResourceGroupName $RG

### Obnovení klientského certifikátu

Klientský certifikát lze obnovit odebráním jeho kryptografického otisku ze seznamu odvolaných klientských certifikátů.

1.  Kryptografický otisk odeberte ze seznamu otisků odvolaných klientských certifikátů.

        Remove-AzureRmVpnClientRevokedCertificate -VpnClientRevokedCertificateName $RevokedClientCert1 `
        -VirtualNetworkGatewayName $GWName -ResourceGroupName $RG -Thumbprint $RevokedThumbprint1

2. Zkontrolujte, za byl kryptografický otisk ze seznamu odebrán.

        Get-AzureRmVpnClientRevokedCertificate -VirtualNetworkGatewayName $GWName -ResourceGroupName $RG

## Další kroky

Do virtuální sítě můžete přidat virtuální počítač. Kroky jsou uvedeny v tématu [Vytvoření virtuálního počítače](../virtual-machines/virtual-machines-windows-hero-tutorial.md).





<!--HONumber=sep16_HO1-->


