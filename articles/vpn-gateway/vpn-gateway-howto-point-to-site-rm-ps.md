<properties 
   pageTitle="Konfigurace připojení VPN typu Point-to-Site k virtuální síti pomocí modelu nasazení Resource Manageru | Microsoft Azure"
   description="Připojte se bezpečně ke službě Azure Virtual Network vytvořením připojení VPN typu Point-to-Site."
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
   ms.date="08/16/2016"
   ms.author="cherylmc" />

# Konfigurace připojení Point-to-Site k virtuální síti pomocí prostředí PowerShell

> [AZURE.SELECTOR]
- [PowerShell – Resource Manager](vpn-gateway-howto-point-to-site-rm-ps.md)
- [Klasický portál](vpn-gateway-point-to-site-create.md)

Konfigurace Point-to-Site (P2S) umožňuje vytvořit zabezpečené připojení mezi jednotlivým klientským počítačem a virtuální sítí. Připojení P2S je užitečné, když se chcete ke své virtuální síti připojit ze vzdáleného umístění, například z domova nebo z místa konání konference, nebo když máte jen několik klientů, kteří se potřebují připojovat k virtuální síti. 

Připojení typu Point-to-Site k fungování nevyžadují zařízení VPN ani veřejnou IP adresu. Připojení VPN je vytvořeno spuštěním připojení na klientském počítači. Další informace o připojeních typu Point-to-Site najdete v tématech [Služba VPN Gateway – nejčastější dotazy](vpn-gateway-vpn-faq.md#point-to-site-connections) a [Plánování a navrhování](vpn-gateway-plan-design.md).

Tento článek se týká připojení Point-to-Site k virtuální síti vytvořených pomocí modelu nasazení Resource Manageru. Kroky postupu v tomto článku využívají prostředí PowerShell.

**O modelech nasazení Azure**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

**Modely nasazení a nástroje pro připojení typu Point-to-Site**

[AZURE.INCLUDE [vpn-gateway-table-point-to-site](../../includes/vpn-gateway-table-point-to-site-include.md)] 

![Diagram Point-to-Site](./media/vpn-gateway-point-to-site-create/point2site.png "point-to-site")


## O této konfiguraci

V tomto scénáři vytvoříte virtuální síť s připojením Point-to-Site. Připojení P2S se skládá z následujících položek: sítě VNet s bránou, kořenového certifikátu v souboru .cer, klientského certifikátu a konfigurace sítě VPN na straně klienta. 

Pro tuto konfiguraci používáme následující hodnoty:

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
    
- Nainstalujte rutiny prostředí PowerShell pro Azure Resource Manager (verze 1.0.2 nebo novější). Další informace o instalaci rutin prostředí PowerShell najdete v tématu [Instalace a konfigurace Azure PowerShellu](../powershell-install-configure.md).

## Konfigurace připojení Point-to-Site pro Azure

1. V konzole prostředí PowerShell se přihlaste k účtu Azure. Tato rutina vás vyzve k zadání přihlašovacích údajů k vašemu účtu Azure. Po přihlášení se stáhne nastavení účtu, aby bylo dostupné v prostředí Azure PowerShell.

        Login-AzureRmAccount 

2. Načtěte seznam předplatných Azure.

        Get-AzureRmSubscription

3. Určete předplatné, které chcete použít. 

        Select-AzureRmSubscription -SubscriptionName "Name of subscription"

4. V této konfiguraci jsou deklarovány následující proměnné prostředí PowerShell s hodnotami, které chcete použít. Deklarované hodnoty jsou použity v ukázkových skriptech. Deklarujte hodnoty, které chcete použít. Použijte následující příklad a dle potřeby nahraďte v něm uvedené hodnoty vlastními. 

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

5. Vytvořte novou skupinu prostředků.

        New-AzureRmResourceGroup -Name $RG -Location $Location

6. Vytvořte konfigurace podsítí pro virtuální síť, podsítě pojmenujte *FrontEnd*, *BackEnd* a *GatewaySubnet*. Tyto předpony musí být součástí adresního prostoru sítě VNet deklarovaného výše.

        $fesub = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName -AddressPrefix $FESubPrefix
        $besub = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName -AddressPrefix $BESubPrefix
        $gwsub = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName -AddressPrefix $GWSubPrefix

7. Vytvořte virtuální síť. Zadaný server DNS by měl být server DNS, který dokáže přeložit názvy pro prostředky, ke kterým se připojujete. V tomto příkladu jsme použili veřejnou IP adresu. Je potřeba, abyste použili svoje vlastní hodnoty.
    
        New-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $RG -Location $Location -AddressPrefix $VNetPrefix1,$VNetPrefix2 -Subnet $fesub, $besub, $gwsub -DnsServer $DNS

8. Určete proměnné pro virtuální síť, kterou jste vytvořili.

        $vnet = Get-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $RG
        $subnet = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet

9. Požádejte o dynamicky přidělovanou veřejnou IP adresu. Tato IP adresa je nezbytná k tomu, aby brána mohla správně fungovat. Později připojíte bránu do konfigurace IP brány.
        
        $pip = New-AzureRmPublicIpAddress -Name $GWIPName -ResourceGroupName $RG -Location $Location -AllocationMethod Dynamic
        $ipconf = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName -Subnet $subnet -PublicIpAddress $pip
        
10. Přidejte do Azure důvěryhodný certifikát. Můžete přidat až 20 certifikátů. Pokyny k vytvoření kořenového certifikátu podepsaného svým držitelem pomocí nástroje *makecert* naleznete v části [Práce s kořenovými certifikáty podepsanými svými držiteli pro účely konfigurace připojení Point-to-Site](vpn-gateway-certificates-point-to-site.md). Pokud do Azure přidáte soubor X.509 zakódovaný ve formátu Base64 (.cer), oznamujete tím Azure, že má důvěřovat kořenovému certifikátu, který tento soubor představuje.

    Chcete-li získat veřejný klíč, exportujte certifikát do souboru X.509 (.CER) zakódovaného ve formátu Base64. Poznamenejte si cestu k souboru, kam jste provedli export do souboru .cer. Toto je vzorek získání řetězcové reprezentace vašeho certifikátu ve formátu Base64. V tomto kroku budete muset použít cestu ke svému vlastnímu souboru .cer.
    
        $filePathForCert = "pasteYourCerFilePathHere"
        $cert = new-object System.Security.Cryptography.X509Certificates.X509Certificate2($filePathForCert)
        $CertBase64 = [system.convert]::ToBase64String($cert.RawData)
        $p2srootcert = New-AzureRmVpnClientRootCertificate -Name $P2SRootCertName -PublicCertData $CertBase64


11. Vytvořte bránu virtuální sítě pro vaší síť VNet. Parametr *-GatewayType* musí mít hodnotu **Vpn** a parametr *-VpnType* musí mít hodnotu **RouteBased**.

        New-AzureRmVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG `
        -Location $Location -IpConfigurations $ipconf -GatewayType Vpn `
        -VpnType RouteBased -EnableBgp $false -GatewaySku Standard `
        -VpnClientAddressPool $VPNClientAddressPool -VpnClientRootCertificates $p2srootcert

## Konfigurace klienta

Každý klient, který se připojuje k Azure pomocí připojení Point-to-Site musí mít splňovat dvě podmínky: klient VPN musí být nastavený pro připojení a klient musí mít nainstalovaný certifikát klienta. Pro klienty se systémem Windows jsou k dispozici balíčky pro konfiguraci klientů VPN. Další informace viz téma [Služba VPN Gateway – nejčastější dotazy](vpn-gateway-vpn-faq.md#point-to-site-connections). 

1. Stáhněte si balíček pro konfiguraci klienta VPN. V tomto kroku použijte následující příklad ke stažení konfiguračního balíčku klienta.

        Get-AzureRmVpnClientPackage -ResourceGroupName $RG `
        -VirtualNetworkGatewayName $GWName -ProcessorArchitecture Amd64

    Rutina prostředí PowerShell vrátí odkaz URL. Zkopírujte a vložte vrácený odkaz do webového prohlížeče a stáhněte balíček do počítače. Toto je příklad toho, jak vrácený odkaz URL vypadá.

        "https://mdsbrketwprodsn1prod.blob.core.windows.net/cmakexe/4a431aa7-b5c2-45d9-97a0-859940069d3f/amd64/4a431aa7-b5c2-45d9-97a0-859940069d3f.exe?sv=2014-02-14&sr=b&sig=jSNCNQ9aUKkCiEokdo%2BqvfjAfyhSXGnRG0vYAv4efg0%3D&st=2016-01-08T07%3A10%3A08Z&se=2016-01-08T08%3A10%3A08Z&sp=r&fileExtension=.exe"
    
2. Vygenerujte a nainstalujte klientské certifikáty (soubory *.pfx) vytvořené z kořenového certifikátu do klientských počítačů. Můžete použít libovolnou metodu instalace, se kterou jste obeznámeni. Pokud používáte kořenový certifikát podepsaný svým držitelem a nejste obeznámeni s postupem, jak tuto akci provést, najdete informace v tématu [Práce s kořenovými certifikáty podepsanými svými držiteli pro účely konfigurace připojení Point-to-Site](vpn-gateway-certificates-point-to-site.md). 

3. Chcete-li se připojit ke své síti VNet, přejděte na klientském počítači na připojení VPN a vyhledejte připojení VPN, které jste vytvořili. Bude mít stejný název jako vaše virtuální síť. Klikněte na **Připojit**. Může se zobrazit místní zpráva týkající se použití certifikátu. Pokud k tomu dojde, klikněte na možnost **Pokračovat**, abyste použili zvýšená oprávnění. 

4. Připojení spustíte kliknutím na tlačítko **Připojit** na stavové stránce **Připojení**. Pokud uvidíte obrazovku **Výběr certifikátu**, ujistěte se, že zobrazený klientský certifikát je ten, který chcete pro připojení použít. Pokud není, vyberte pomocí šipky rozevíracího seznamu správný certifikát, a poté klikněte na **OK**.

5. Připojení by nyní mělo být navázáno. Stav připojení můžete ověřit následujícím postupem.

## Ověření stavu připojení

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

### Přidání důvěryhodného kořenového certifikátu

Do Azure můžete přidat až 20 souborů .cer s důvěryhodnými kořenovými certifikáty. Kořenový certifikát přidáte pomocí následujícího postupu.

1. Vytvořte a připravte nový kořenový certifikát, který přidáte do Azure.

        $P2SRootCertName2 = "ARMP2SRootCert2.cer"
        $MyP2SCertPubKeyBase64_2 = "MIIC/zCCAeugAwIBAgIQKazxzFjMkp9JRiX+tkTfSzAJBgUrDgMCHQUAMBgxFjAUBgNVBAMTDU15UDJTUm9vdENlcnQwHhcNMTUxMjE5MDI1MTIxWhcNMzkxMjMxMjM1OTU5WjAYMRYwFAYDVQQDEw1NeVAyU1Jvb3RDZXJ0MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAyjIXoWy8xE/GF1OSIvUaA0bxBjZ1PJfcXkMWsHPzvhWc2esOKrVQtgFgDz4ggAnOUFEkFaszjiHdnXv3mjzE2SpmAVIZPf2/yPWqkoHwkmrp6BpOvNVOpKxaGPOuK8+dql1xcL0eCkt69g4lxy0FGRFkBcSIgVTViS9wjuuS7LPo5+OXgyFkAY3pSDiMzQCkRGNFgw5WGMHRDAiruDQF1ciLNojAQCsDdLnI3pDYsvRW73HZEhmOqRRnJQe6VekvBYKLvnKaxUTKhFIYwuymHBB96nMFdRUKCZIiWRIy8Hc8+sQEsAML2EItAjQv4+fqgYiFdSWqnQCPf/7IZbotgQIDAQABo00wSzBJBgNVHQEEQjBAgBAkuVrWvFsCJAdK5pb/eoCNoRowGDEWMBQGA1UEAxMNTXlQMlNSb290Q2VydIIQKazxzFjMkp9JRiX+tkTfSzAJBgUrDgMCHQUAA4IBAQA223veAZEIar9N12ubNH2+HwZASNzDVNqspkPKD97TXfKHlPlIcS43TaYkTz38eVrwI6E0yDk4jAuPaKnPuPYFRj9w540SvY6PdOUwDoEqpIcAVp+b4VYwxPL6oyEQ8wnOYuoAK1hhh20lCbo8h9mMy9ofU+RP6HJ7lTqupLfXdID/XevI8tW6Dm+C/wCeV3EmIlO9KUoblD/e24zlo3YzOtbyXwTIh34T0fO/zQvUuBqZMcIPfM1cDvqcqiEFLWvWKoAnxbzckye2uk1gHO52d8AVL3mGiX8wBJkjc/pMdxrEvvCzJkltBmqxTM6XjDJALuVh16qFlqgTWCIcb7ju"

2. Přidejte nový kořenový certifikát. Nelze přidat více certifikátů současně.

        Add-AzureRmVpnClientRootCertificate -VpnClientRootCertificateName $P2SRootCertName2 -VirtualNetworkGatewayname $GWName -ResourceGroupName $RG -PublicCertData $MyP2SCertPubKeyBase64_2

3. Pomocí následující rutiny můžete ověřit, zda byl nový certifikát přidán správně.

        Get-AzureRmVpnClientRootCertificate -ResourceGroupName $RG `
        -VirtualNetworkGatewayName $GWName

### Odebrání důvěryhodného kořenového certifikátu

Důvěryhodný kořenový certifikát můžete z Azure odebrat. Pokud odeberete důvěryhodný certifikát, nebude již možné se pomocí klientských certifikátů vygenerovaných z tohoto kořenového certifikátu připojit k Azure prostřednictvím připojení Point-to-Site. Pokud chcete, aby se klienti mohli i nadále připojovat, je nutné nainstalovat nový certifikát, který je vygenerován z certifikátu, který Azure považuje za důvěryhodný.

1. Chcete-li odebrat důvěryhodný kořenový certifikát, upravte dle potřeby následující vzorový příklad.

        Remove-AzureRmVpnClientRootCertificate -VpnClientRootCertificateName $P2SRootCertName2 -VirtualNetworkGatewayName $GWName -ResourceGroupName $RG -PublicCertData "MIIC/zCCAeugAwIBAgIQKazxzFjMkp9JRiX+tkTfSzAJBgUrDgMCHQUAMBgxFjAUBgNVBAMTDU15UDJTUm9vdENlcnQwHhcNMTUxMjE5MDI1MTIxWhcNMzkxMjMxMjM1OTU5WjAYMRYwFAYDVQQDEw1NeVAyU1Jvb3RDZXJ0MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAyjIXoWy8xE/GF1OSIvUaA0bxBjZ1PJfcXkMWsHPzvhWc2esOKrVQtgFgDz4ggAnOUFEkFaszjiHdnXv3mjzE2SpmAVIZPf2/yPWqkoHwkmrp6BpOvNVOpKxaGPOuK8+dql1xcL0eCkt69g4lxy0FGRFkBcSIgVTViS9wjuuS7LPo5+OXgyFkAY3pSDiMzQCkRGNFgw5WGMHRDAiruDQF1ciLNojAQCsDdLnI3pDYsvRW73HZEhmOqRRnJQe6VekvBYKLvnKaxUTKhFIYwuymHBB96nMFdRUKCZIiWRIy8Hc8+sQEsAML2EItAjQv4+fqgYiFdSWqnQCPf/7IZbotgQIDAQABo00wSzBJBgNVHQEEQjBAgBAkuVrWvFsCJAdK5pb/eoCNoRowGDEWMBQGA1UEAxMNTXlQMlNSb290Q2VydIIQKazxzFjMkp9JRiX+tkTfSzAJBgUrDgMCHQUAA4IBAQA223veAZEIar9N12ubNH2+HwZASNzDVNqspkPKD97TXfKHlPlIcS43TaYkTz38eVrwI6E0yDk4jAuPaKnPuPYFRj9w540SvY6PdOUwDoEqpIcAVp+b4VYwxPL6oyEQ8wnOYuoAK1hhh20lCbo8h9mMy9ofU+RP6HJ7lTqupLfXdID/XevI8tW6Dm+C/wCeV3EmIlO9KUoblD/e24zlo3YzOtbyXwTIh34T0fO/zQvUuBqZMcIPfM1cDvqcqiEFLWvWKoAnxbzckye2uk1gHO52d8AVL3mGiX8wBJkjc/pMdxrEvvCzJkltBmqxTM6XjDJALuVh16qFlqgTWCIcb7ju"

 
2. Pomocí následující rutiny můžete ověřit, zda byl certifikát úspěšně odebrán. 

        Get-AzureRmVpnClientRootCertificate -ResourceGroupName $RG -VirtualNetworkGatewayName $GWName

## Správa seznamu odvolaných klientských certifikátů

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





<!---HONumber=Aug16_HO4-->


