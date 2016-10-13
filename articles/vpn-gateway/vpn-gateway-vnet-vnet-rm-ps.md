<properties
   pageTitle="Propojení virtuálních sítí Azure pomocí VPN Gateway a PowerShellu | Microsoft Azure"
   description="Tento článek vás provede propojováním virtuálních sítí s použitím Azure Resource Manageru a prostředí PowerShell."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/31/2016"
   ms.author="cherylmc"/>


# Konfigurace propojení VNet-to-VNet pro Resource Manager pomocí PowerShellu

> [AZURE.SELECTOR]
- [Resource Manager – PowerShell](vpn-gateway-vnet-vnet-rm-ps.md)
- [Classic – klasický portál](virtual-networks-configure-vnet-to-vnet-connection.md)

Tento článek vás provede postupem vytváření propojení mezi virtuálními sítěmi s použitím služby VPN Gateway a modelu nasazení Resource Manager. Virtuální sítě se můžou nacházet ve stejné oblasti nebo v různých oblastech a můžou patřit do stejného předplatného nebo do různých předplatných.


![Diagram v2v](./media/vpn-gateway-vnet-vnet-rm-ps/v2vrmps.png)


### Modely nasazení a metody pro VNet-to-VNet


[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

Propojení VNet-to-VNet je možné konfigurovat v obou režimech nasazení a s použitím několika různých nástrojů. Následující tabulku aktualizujeme přidáváním nových článků a dalších nástrojů, které jsou pro tuto konfiguraci postupně k dispozici. Když je článek k dispozici, zařadíme do tabulky přímý odkaz na něj.<br><br>

[AZURE.INCLUDE [vpn-gateway-table-vnet-vnet](../../includes/vpn-gateway-table-vnet-to-vnet-include.md)] 

#### Partnerské vztahy virtuálních sítí

[AZURE.INCLUDE [vpn-gateway-vnetpeeringlink](../../includes/vpn-gateway-vnetpeeringlink-include.md)]


## Informace o propojeních VNet-to-VNet

Propojení virtuální sítě s jinou virtuální sítí (VNet-to-VNet) je podobné propojení virtuální sítě s místním serverem. Oba typy připojení využívají bránu VPN Azure VPN, která poskytuje zabezpečené tunelové propojení prostřednictvím protokolu IPsec/IKE. Virtuální sítě, které propojujete, se můžou nacházet v různých oblastech. Můžou taky patřit do různých předplatných. Dokonce můžete kombinovat komunikaci VNet-to-VNet s konfigurací s více servery. Díky tomu je možné vytvářet topologie sítí, ve kterých se používá propojování více míst i propojování virtuálních sítí, jak je znázorněno v následujícím schématu:


![Informace o připojeních](./media/vpn-gateway-vnet-vnet-rm-ps/aboutconnections.png)
 
### Proč propojovat virtuální sítě?

Virtuální sítě může být vhodné propojit z následujících důvodů:

- **Geografická redundance a geografická přítomnost mezi oblastmi**
    - Můžete nastavit vlastní geografickou replikaci nebo synchronizaci se zabezpečeným připojením bez procházení koncovými body připojenými k internetu.
    - Pomocí Azure Traffic Manageru a služby Load Balancer je možné vytvářet úlohy s vysokou dostupností s geografickou redundancí nad několika oblastmi Azure. Jedním z důležitých příkladů je nastavení technologie SQL Always On se skupinami dostupnosti nad několika oblastmi Azure.

- **Regionální vícevrstvé aplikace s izolací nebo administrativní hranicí**
    - V rámci stejné oblasti můžete vytvářet vícevrstvé aplikace s několika virtuálními sítěmi propojenými z důvodu izolace nebo požadavků na správu.


### Nejčastější dotazy týkající se propojení VNet-to-VNet

[AZURE.INCLUDE [vpn-gateway-vnet-vnet-faq](../../includes/vpn-gateway-vnet-vnet-faq-include.md)] 


## Kterou posloupnost kroků provést?

V tomto článku uvidíte dvě různé sady kroků. Jedna sada kroků pro [virtuální sítě spadající do stejného předplatného](#samesub) a druhá sada kroků pro [virtuální sítě v různých předplatných](#difsub). Hlavní rozdíl mezi oběma postupy spočívá v tom, jestli je možné vytvářet a konfigurovat všechny prostředky virtuální sítě a brány v téže relaci prostředí PowerShell.

Kroky v tomto článku používají proměnné, které jsou deklarované na začátku každé části. Pokud již pracujete s existujícími virtuálními sítěmi, upravte proměnné tak, aby odrážely nastavení vašeho prostředí. 

![Obě připojení](./media/vpn-gateway-vnet-vnet-rm-ps/differentsubscription.png)


## <a name="samesub"></a>Postup při propojování virtuálních sítí patřících do stejného předplatného

![Diagram v2v](./media/vpn-gateway-vnet-vnet-rm-ps/v2vrmps.png)

### Než začnete
    
Než začnete, bude třeba nainstalovat nejnovější verzi rutin prostředí PowerShell pro Azure Resource Manager. Další informace o instalaci rutin prostředí PowerShell najdete v tématu [Instalace a konfigurace Azure PowerShellu](../powershell-install-configure.md).

### <a name="Step1"></a>Krok 1: Plánování rozsahů IP adres


V následujících krocích vytvoříme dvě virtuální sítě spolu s příslušnými podsítěmi a konfiguracemi brány. Poté vytvoříme propojení VPN mezi oběma virtuálními sítěmi. Je důležité určit rozsahy IP adres pro konfiguraci vaší sítě. Mějte na paměti, že je třeba zajistit, aby se žádné rozsahy virtuálních sítí ani místní síťové rozsahy žádným způsobem nepřekrývaly.

V příkladech používáme následující hodnoty:

**Hodnoty pro virtuální síť TestVNet1:**

- Název virtuální sítě: TestVNet1
- Skupina prostředků: TestRG1
- Umístění: Východní USA
- TestVNet1: 10.11.0.0/16 a 10.12.0.0/16
- FrontEnd: 10.11.0.0/24
- BackEnd: 10.12.0.0/24
- GatewaySubnet: 10.12.255.0/27
- Server DNS: 8.8.8.8
- Název brány: VNet1GW
- Veřejná IP adresa: VNet1GWIP
- Typ sítě VPN: RouteBased
- Připojení (1 ke 4): VNet1toVNet4
- Připojení (1 k 5): VNet1toVNet5
- Typ připojení: VNet2VNet


**Hodnoty pro virtuální síť TestVNet4:**

- Název virtuální sítě: TestVNet4
- TestVNet2: 10.41.0.0/16 a 10.42.0.0/16
- FrontEnd: 10.41.0.0/24
- BackEnd: 10.42.0.0/24
- GatewaySubnet: 10.42.255.0/27
- Skupina prostředků: TestRG4
- Umístění: Západní USA
- Server DNS: 8.8.8.8
- Název brány: VNet4GW
- Veřejná IP adresa: VNet4GWIP
- Typ sítě VPN: RouteBased
- Připojení: VNet4toVNet1
- Typ připojení: VNet2VNet



### <a name="Step2"></a>Krok 2: Vytvoření a konfigurace virtuální sítě TestVNet1

1. Deklarace proměnných

    Začneme deklarací proměnných. V tomto příkladu jsou proměnné deklarovány s použitím hodnot pro tento ukázkový postup. Ve většině případů byste měli hodnoty nahradit vlastními. Tyto hodnoty proměnných ale můžete použít, pokud procházíte kroky, abyste se seznámili s tímto typem konfigurace. Upravte proměnné podle potřeby a pak je zkopírujte a vložte do konzoly PowerShell.

        $Sub1 = "Replace_With_Your_Subcription_Name"
        $RG1 = "TestRG1"
        $Location1 = "East US"
        $VNetName1 = "TestVNet1"
        $FESubName1 = "FrontEnd"
        $BESubName1 = "Backend"
        $GWSubName1 = "GatewaySubnet"
        $VNetPrefix11 = "10.11.0.0/16"
        $VNetPrefix12 = "10.12.0.0/16"
        $FESubPrefix1 = "10.11.0.0/24"
        $BESubPrefix1 = "10.12.0.0/24"
        $GWSubPrefix1 = "10.12.255.0/27"
        $DNS1 = "8.8.8.8"
        $GWName1 = "VNet1GW"
        $GWIPName1 = "VNet1GWIP"
        $GWIPconfName1 = "gwipconf1"
        $Connection14 = "VNet1toVNet4"
        $Connection15 = "VNet1toVNet5"

2. Připojení k vašemu předplatnému

    Přejděte do režimu PowerShellu, aby bylo možné používat rutiny Resource Manageru. Otevřete konzolu prostředí PowerShell a připojte se ke svému účtu. Připojení vám usnadní následující ukázka:

        Login-AzureRmAccount

    Zkontrolujte předplatná pro příslušný účet.

        Get-AzureRmSubscription 

    Určete předplatné, které chcete použít.

        Select-AzureRmSubscription -SubscriptionName $Sub1

3. Vytvoření nové skupiny prostředků

        New-AzureRmResourceGroup -Name $RG1 -Location $Location1

4. Vytvoření konfigurací podsítě pro virtuální síť TestVNet1

    Tato ukázka vytvoří virtuální síť s názvem TestVNet1 a tři podsítě: jednu s názvem GatewaySubnet, jednu s názvem FrontEnd a jednu s názvem BackEnd. Při nahrazování hodnot je důležité vždy přiřadit podsíti brány konkrétní název GatewaySubnet. Pokud použijete jiný název, vytvoření brány se nezdaří. 

    Následující příklad používá proměnné, které jste nastavili dříve. V příkladu používá podsíť brány možnost /27. Přestože je možné vytvořit podsíť brány s použitím tak malé podsítě, jako je /29, nedoporučuje se to. Doporučujeme použít větší velikost, například /27 nebo /26. To vám umožní využívat stávající nebo budoucí konfigurace, které mohou vyžadovat větší podsíť brány. 

        $fesub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName1 -AddressPrefix $FESubPrefix1
        $besub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName1 -AddressPrefix $BESubPrefix1
        $gwsub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName1 -AddressPrefix $GWSubPrefix1


5. Vytvoření virtuální sítě TestVNet1

        New-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1 `
        -Location $Location1 -AddressPrefix $VNetPrefix11,$VNetPrefix12 -Subnet $fesub1,$besub1,$gwsub1

6. Vyžádání veřejné IP adresy

    Vyžádejte si veřejnou IP adresu, která bude přidělena bráně, kterou vytvoříte pro příslušnou virtuální síť. Všimněte si, že metoda AllocationMethod je dynamická. Není možné určit IP adresu, kterou chcete používat. Přiděluje se pro bránu dynamicky. 

        $gwpip1 = New-AzureRmPublicIpAddress -Name $GWIPName1 -ResourceGroupName $RG1 `
        -Location $Location1 -AllocationMethod Dynamic

7. Vytvoření konfigurace brány

    Konfigurace brány definuje podsíť a veřejnou IP adresu, která se bude používat. Podle následující ukázky vytvořte vlastní konfiguraci brány. 

        $vnet1 = Get-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1
        $subnet1 = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet1
        $gwipconf1 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName1 `
        -Subnet $subnet1 -PublicIpAddress $gwpip1

8. Vytvoření brány pro virtuální síť TestVNet1

    V tomto kroku vytvoříte bránu virtuální sítě pro virtuální síť TestVNet1. Konfigurace propojení VNet-to-VNet vyžadují typ sítě VPN RouteBased. Vytvoření brány může nějakou dobu trvat (45 minut nebo déle).

        New-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1 `
        -Location $Location1 -IpConfigurations $gwipconf1 -GatewayType Vpn `
        -VpnType RouteBased -GatewaySku Standard

### Krok 3: Vytvoření a konfigurace virtuální sítě TestVNet4

Po konfiguraci virtuální sítě TestVNet1 vytvořte virtuální síť TestVNet4. Postupujte podle následujících kroků a podle potřeby nahrazujte hodnoty vlastními. Tento krok lze provést v rámci stejné relace prostředí PowerShell, protože se jedná o stejné předplatné.

1. Deklarace proměnných

    Nezapomeňte nahradit hodnoty těmi, které chcete použít pro svou konfiguraci.

        $RG4 = "TestRG4"
        $Location4 = "West US"
        $VnetName4 = "TestVNet4"
        $FESubName4 = "FrontEnd"
        $BESubName4 = "Backend"
        $GWSubName4 = "GatewaySubnet"
        $VnetPrefix41 = "10.41.0.0/16"
        $VnetPrefix42 = "10.42.0.0/16"
        $FESubPrefix4 = "10.41.0.0/24"
        $BESubPrefix4 = "10.42.0.0/24"
        $GWSubPrefix4 = "10.42.255.0/27"
        $DNS4 = "8.8.8.8"
        $GWName4 = "VNet4GW"
        $GWIPName4 = "VNet4GWIP"
        $GWIPconfName4 = "gwipconf4"
        $Connection41 = "VNet4toVNet1"

    Než budete pokračovat, zkontrolujte, že jste stále připojeni k předplatnému 1.

2. Vytvoření nové skupiny prostředků

        New-AzureRmResourceGroup -Name $RG4 -Location $Location4

3. Vytvoření konfigurací podsítě pro virtuální síť TestVNet4

        $fesub4 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName4 -AddressPrefix $FESubPrefix4
        $besub4 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName4 -AddressPrefix $BESubPrefix4
        $gwsub4 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName4 -AddressPrefix $GWSubPrefix4

4. Vytvoření virtuální sítě TestVNet4

        New-AzureRmVirtualNetwork -Name $VnetName4 -ResourceGroupName $RG4 `
        -Location $Location4 -AddressPrefix $VnetPrefix41,$VnetPrefix42 -Subnet $fesub4,$besub4,$gwsub4

5. Vyžádání veřejné IP adresy

        $gwpip4 = New-AzureRmPublicIpAddress -Name $GWIPName4 -ResourceGroupName $RG4 `
        -Location $Location4 -AllocationMethod Dynamic

6. Vytvoření konfigurace brány

        $vnet4 = Get-AzureRmVirtualNetwork -Name $VnetName4 -ResourceGroupName $RG4
        $subnet4 = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet4
        $gwipconf4 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName4 -Subnet $subnet4 -PublicIpAddress $gwpip4

7. Vytvoření brány virtuální sítě TestVNet4

        New-AzureRmVirtualNetworkGateway -Name $GWName4 -ResourceGroupName $RG4 `
        -Location $Location4 -IpConfigurations $gwipconf4 -GatewayType Vpn `
        -VpnType RouteBased -GatewaySku Standard

### Krok 4: Propojení bran

1. Zjištění obou bran virtuálních sítí

    Jelikož obě brány v tomto příkladu patří do stejného předplatného, je možné tento krok provést v téže relaci prostředí PowerShell.

        $vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1
        $vnet4gw = Get-AzureRmVirtualNetworkGateway -Name $GWName4 -ResourceGroupName $RG4


2. Vytvoření připojení virtuální sítě TestVNet1 k virtuální síti TestVNet4

    V tomto kroku vytvoříte připojení z virtuální sítě TestVNet1 do virtuální sítě TestVNet4. Zobrazí se sdílený klíč uváděný v příkladech. Pro sdílený klíč můžete použít vlastní hodnoty. Důležité je, že se sdílený klíč pro obě připojení musí shodovat. Vytvoření připojení může nějakou dobu trvat.

        New-AzureRmVirtualNetworkGatewayConnection -Name $Connection14 -ResourceGroupName $RG1 `
        -VirtualNetworkGateway1 $vnet1gw -VirtualNetworkGateway2 $vnet4gw -Location $Location1 `
        -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3'

3. Vytvoření připojení virtuální sítě TestVNet4 k virtuální síti TestVNet1

    Tento krok je podobný předchozímu, vytváříte však připojení z virtuální sítě TestVNet4 do virtuální sítě TestVNet1. Ověřte, že se sdílené klíče shodují.

        New-AzureRmVirtualNetworkGatewayConnection -Name $Connection41 -ResourceGroupName $RG4 `
        -VirtualNetworkGateway1 $vnet4gw -VirtualNetworkGateway2 $vnet1gw -Location $Location4 `
        -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3'

    Připojení by se mělo vytvořit během několika minut.

4. Ověřte své propojení. Viz část [Ověření připojení](#verify).


## <a name="difsub"></a>Postup při propojování virtuálních sítí patřících do různých předplatných


![Diagram v2v](./media/vpn-gateway-vnet-vnet-rm-ps/v2vdiffsub.png)

V tomto scénáři propojíme sítě TestVNet1 a TestVNet5. Virtuální sítě TestVNet1 a TestVNet5 patří do různých předplatných. Tento postup přidá nové propojení VNet-to-VNet pro připojení virtuální sítě TestVNet1 k virtuální síti TestVNet5. 

Rozdíl spočívá v tom, že část kroků konfigurace je třeba provést v samostatné relaci prostředí PowerShell v kontextu druhého předplatného. To je zvláště podstatné, když druhé předplatné patří jiné organizaci. 

Následující pokyny jsou pokračováním kroků uvedených nahoře. Je třeba vytvořit a konfigurovat virtuální síť TestVNet1 a bránu VPN Gateway pro virtuální síť TestVNet1 provedením [kroku 1](#Step1) a [kroku 2](#Step2). Po dokončení kroků 1 a 2 pokračujte krokem 5 a vytvořte síť TestVNet5.

### Krok 5: Ověření dalších rozsahů IP adres

Je důležité zajistit, aby se prostor IP adres nové virtuální sítě TestVNet5 nepřekrýval se žádným z rozsahů virtuálních sítí ani rozsahů bran místních sítí. 

V tomto příkladu můžou virtuální sítě různým organizacím. Pro tento postup použijte následující hodnoty pro virtuální síť TestVNet5:

**Hodnoty pro virtuální síť TestVNet5:**

- Název virtuální sítě: TestVNet5
- Skupina prostředků: TestRG5
- Umístění: Japonsko – východ
- TestVNet5: 10.51.0.0/16 a 10.52.0.0/16
- FrontEnd: 10.51.0.0/24
- BackEnd: 10.52.0.0/24
- GatewaySubnet: 10.52.255.0.0/27
- Server DNS: 8.8.8.8
- Název brány: VNet5GW
- Veřejná IP adresa: VNet5GWIP
- Typ sítě VPN: RouteBased
- Připojení: VNet5toVNet1
- Typ připojení: VNet2VNet

**Další hodnoty pro virtuální síť TestVNet1:**

- Připojení: VNet1toVNet5


### Krok 6: Vytvoření a konfigurace virtuální sítě TestVNet5

Tento krok je třeba provést v rámci nového předplatného. Tuto část může provést správce v organizaci, která je vlastníkem druhého předplatného.

1. Deklarace proměnných

    Nezapomeňte nahradit hodnoty těmi, které chcete použít pro svou konfiguraci.

        $Sub5 = "Replace_With_the_New_Subcription_Name"
        $RG5 = "TestRG5"
        $Location5 = "Japan East"
        $VnetName5 = "TestVNet5"
        $FESubName5 = "FrontEnd"
        $BESubName5 = "Backend"
        $GWSubName5 = "GatewaySubnet"
        $VnetPrefix51 = "10.51.0.0/16"
        $VnetPrefix52 = "10.52.0.0/16"
        $FESubPrefix5 = "10.51.0.0/24"
        $BESubPrefix5 = "10.52.0.0/24"
        $GWSubPrefix5 = "10.52.255.0/27"
        $DNS5 = "8.8.8.8"
        $GWName5 = "VNet5GW"
        $GWIPName5 = "VNet5GWIP"
        $GWIPconfName5 = "gwipconf5"
        $Connection51 = "VNet5toVNet1"

2. Připojení k předplatnému 5

    Otevřete konzolu prostředí PowerShell a připojte se ke svému účtu. Připojení vám usnadní následující ukázka:

        Login-AzureRmAccount

    Zkontrolujte předplatná pro příslušný účet.

        Get-AzureRmSubscription 

    Určete předplatné, které chcete použít.

        Select-AzureRmSubscription -SubscriptionName $Sub5

3. Vytvoření nové skupiny prostředků

        New-AzureRmResourceGroup -Name $RG5 -Location $Location5

4. Vytvoření konfigurací podsítě pro virtuální síť TestVNet4
    
        $fesub5 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName5 -AddressPrefix $FESubPrefix5
        $besub5 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName5 -AddressPrefix $BESubPrefix5
        $gwsub5 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName5 -AddressPrefix $GWSubPrefix5

5. Vytvoření virtuální sítě TestVNet5

        New-AzureRmVirtualNetwork -Name $VnetName5 -ResourceGroupName $RG5 -Location $Location5 `
        -AddressPrefix $VnetPrefix51,$VnetPrefix52 -Subnet $fesub5,$besub5,$gwsub5

6. Vyžádání veřejné IP adresy

        $gwpip5 = New-AzureRmPublicIpAddress -Name $GWIPName5 -ResourceGroupName $RG5 `
        -Location $Location5 -AllocationMethod Dynamic


7. Vytvoření konfigurace brány

        $vnet5 = Get-AzureRmVirtualNetwork -Name $VnetName5 -ResourceGroupName $RG5
        $subnet5  = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet5
        $gwipconf5 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName5 -Subnet $subnet5 -PublicIpAddress $gwpip5

8. Vytvoření brány virtuální sítě TestVNet5

        New-AzureRmVirtualNetworkGateway -Name $GWName5 -ResourceGroupName $RG5 -Location $Location5 `
        -IpConfigurations $gwipconf5 -GatewayType Vpn -VpnType RouteBased -GatewaySku Standard

### Krok 7: Propojení bran

Jelikož brány v tomto příkladu patří do různých předplatných, rozdělíme tento krok do dvou relací prostředí PowerShell označených [Předplatné 1] a [Předplatné 5].

1. **[Předplatné 1]** Zjištění brány virtuální sítě pro předplatné 1

    Ujistěte se, že jste přihlášeni a připojeni k předplatnému 1.

        $vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1

    Zkopírujte výstup následujících prvků a pošlete je správci předplatného 5 prostřednictvím e-mailu nebo jiným způsobem.

        $vnet1gw.Name
        $vnet1gw.Id

    Tyto dva prvky budou mít hodnoty podobné výstupu v následujícím příkladu:

        PS D:\> $vnet1gw.Name
        VNet1GW
        PS D:\> $vnet1gw.Id
        /subscriptions/b636ca99-6f88-4df4-a7c3-2f8dc4545509/resourceGroupsTestRG1/providers/Microsoft.Network/virtualNetworkGateways/VNet1GW

2. **[Předplatné 5]** Zjištění brány virtuální sítě pro předplatné 5

    Ujistěte se, že jste přihlášeni a připojeni k předplatnému 5.

        $vnet5gw = Get-AzureRmVirtualNetworkGateway -Name $GWName5 -ResourceGroupName $RG5

    Zkopírujte výstup následujících prvků a pošlete jej správci předplatného 1 prostřednictvím e-mailu nebo jiným způsobem.

        $vnet5gw.Name
        $vnet5gw.Id

    Tyto dva prvky budou mít hodnoty podobné výstupu v následujícím příkladu:

        PS C:\> $vnet5gw.Name
        VNet5GW
        PS C:\> $vnet5gw.Id
        /subscriptions/66c8e4f1-ecd6-47ed-9de7-7e530de23994/resourceGroups/TestRG5/providers/Microsoft.Network/virtualNetworkGateways/VNet5GW

3. **[Předplatné 1]** Vytvoření připojení virtuální sítě TestVNet1 k virtuální síti TestVNet5

    V tomto kroku vytvoříte propojení z virtuální sítě TestVNet1 do sítě TestVNet5. Rozdíl zde spočívá v tom, že hodnotu $vnet5gw nelze získat přímo, protože patří do jiného předplatného. Je třeba vytvořit nový objekt prostředí PowerShell s hodnotami zjištěnými z předplatného 1 v předchozích krocích. Postupujte podle následujícího příkladu. Nahraďte název, ID a sdílený klíč vlastními hodnotami. Důležité je, že se sdílený klíč pro obě připojení musí shodovat. Vytvoření připojení může nějakou dobu trvat.

    Ujistěte se, že jste připojeni k předplatnému 1. 
    
        $vnet5gw = New-Object Microsoft.Azure.Commands.Network.Models.PSVirtualNetworkGateway
        $vnet5gw.Name = "VNet5GW"
        $vnet5gw.Id   = "/subscriptions/66c8e4f1-ecd6-47ed-9de7-7e530de23994/resourceGroups/TestRG5/providers/Microsoft.Network/virtualNetworkGateways/VNet5GW"
        $Connection15 = "VNet1toVNet5"
        New-AzureRmVirtualNetworkGatewayConnection -Name $Connection15 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -VirtualNetworkGateway2 $vnet5gw -Location $Location1 -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3'

4. **[Předplatné 5]** Vytvoření připojení virtuální sítě TestVNet5 k virtuální síti TestVNet1

    Tento krok je podobný předchozímu, vytváříte však připojení z virtuální sítě TestVNet5 do virtuální sítě TestVNet1. Stejný postup vytváření objektu prostředí PowerShell na základě hodnot zjištěných z předplatného 1 se používá i zde. V tomto kroku ověřte, že se sdílené klíče shodují.

    Ujistěte se, že jste připojeni k předplatnému 5.

        $vnet1gw = New-Object Microsoft.Azure.Commands.Network.Models.PSVirtualNetworkGateway
        $vnet1gw.Name = "VNet1GW"
        $vnet1gw.Id = "/subscriptions/b636ca99-6f88-4df4-a7c3-2f8dc4545509/resourceGroups/TestRG1/providers/Microsoft.Network/virtualNetworkGateways/VNet1GW "
        New-AzureRmVirtualNetworkGatewayConnection -Name $Connection51 -ResourceGroupName $RG5 -VirtualNetworkGateway1 $vnet5gw -VirtualNetworkGateway2 $vnet1gw -Location $Location5 -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3'

## <a name="verify"></a>Ověření připojení

[AZURE.INCLUDE [vpn-gateway-verify-connection-rm](../../includes/vpn-gateway-verify-connection-rm-include.md)]


## Další kroky

- Po dokončení připojení můžete do virtuálních sítí přidávat virtuální počítače. Kroky jsou uvedeny v tématu [Vytvoření virtuálního počítače](../virtual-machines/virtual-machines-windows-hero-tutorial.md).
- Informace o protokolu BGP najdete v tématech [Přehled protokolu BGP](vpn-gateway-bgp-overview.md) a [Postup při konfiguraci protokolu BGP](vpn-gateway-bgp-resource-manager-ps.md). 




<!--HONumber=Oct16_HO1-->


