---
title: Vytvoření virtuální sítě s připojením VPN typu Site-to-Site pomocí Azure Resource Manageru a prostředí PowerShell | Microsoft Docs
description: Tento článek vás provede procesem vytvoření virtuální sítě pomocí modelu nasazení Resource Manageru a jejím propojením s vaší místní sítí pomocí připojení S2S brány VPN.
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: carmonm
editor: ''
tags: azure-resource-manager

ms.service: vpn-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/31/2016
ms.author: cherylmc

---
# Vytvoření virtuální sítě pomocí připojení Site-to-Site s použitím prostředí PowerShell
> [!div class="op_single_selector"]
> * [Resource Manager – Azure Portal](vpn-gateway-howto-site-to-site-resource-manager-portal.md)
> * [Resource Manager – PowerShell](vpn-gateway-create-site-to-site-rm-powershell.md)
> * [Classic – klasický portál](vpn-gateway-site-to-site-create.md)
> 
> 

Tento článek vás provede procesem vytvoření virtuální sítě a připojení VPN typu Site-to-Site k místní síti pomocí **modelu nasazení Azure Resource Manager**. Připojení typu Site-to-Site lze použít pro konfigurace mezi různými místy a pro hybridní konfigurace.

![Diagram Site-to-site](./media/vpn-gateway-create-site-to-site-rm-powershell/s2srmps.png "site-to-site") 

### Modely nasazení a nástroje pro připojení typu Site-to-Site
[!INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

[!INCLUDE [vpn-gateway-table-site-to-site](../../includes/vpn-gateway-table-site-to-site-include.md)]

Informace o propojení virtuálních sítí bez vytvoření připojení k místnímu umístění najdete v tématu [Konfigurace připojení typu VNet-to-VNet](vpn-gateway-vnet-vnet-rm-ps.md).

## Než začnete
Před zahájením konfigurace ověřte, zda máte následující.

* Kompatibilní zařízení VPN a někoho, kdo jej umí nakonfigurovat. Viz [Informace o zařízeních VPN](vpn-gateway-about-vpn-devices.md). Pokud nevíte, jak nakonfigurovat zařízení VPN, nebo neznáte rozsahy IP adres v konfiguraci vaší místní sítě, budete se muset spojit s někým, kdo vám s tím pomůže.
* Veřejnou IP adresu pro vaše zařízení VPN. Tato IP adresa nesmí být umístěná za překladem adres (NAT).
* Předplatné Azure. Pokud ještě nemáte předplatné Azure, můžete si aktivovat [výhody pro předplatitele MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) nebo si zaregistrovat [bezplatný účet](https://azure.microsoft.com/pricing/free-trial/).
* Nejnovější verzi rutin PowerShellu pro Azure Resource Manager. Další informace o instalaci rutin prostředí PowerShell najdete v tématu [Instalace a konfigurace Azure PowerShellu](../powershell-install-configure.md).

## 1. Připojení k vašemu předplatnému
Ujistěte se, že jste přešli do režimu prostředí PowerShell, aby bylo možné používat rutiny Resource Manageru. Další informace najdete v tématu [Použití prostředí Windows PowerShell s Resource Managerem](../powershell-azure-resource-manager.md).

Otevřete konzolu prostředí PowerShell a připojte se ke svému účtu. Připojení vám usnadní následující ukázka:

    Login-AzureRmAccount

Zkontrolujte předplatná pro příslušný účet.

    Get-AzureRmSubscription 

Určete předplatné, které chcete použít.

    Select-AzureRmSubscription -SubscriptionName "Replace_with_your_subscription_name"

## 2. Vytvoření virtuální sítě a podsítě brány
V příkladech se používá podsíť brány /28. I když je možné vytvořit podsíť brány s minimální velikostí /29, doporučujeme vytvořit větší podsíť, která pojme více adres, tzn. vybrat velikost aspoň /28 nebo /27. Tím vznikne dostatečný prostor pro adresy, který umožní nastavení případných dalších konfigurací v budoucnu.

Pokud již máte virtuální síť s podsítí brány, která je /29 nebo větší, můžete přeskočit na [Přidání vlastní brány místní sítě](#localnet).

[!INCLUDE [vpn-gateway-no-nsg](../../includes/vpn-gateway-no-nsg-include.md)]

### Chcete-li vytvořit virtuální síť a podsíť brány
Podle následující ukázky vytvořte virtuální síť a podsíť brány. Nahraďte hodnoty svými hodnotami. 

Nejdříve vytvořte skupinu prostředků:

    New-AzureRmResourceGroup -Name testrg -Location 'West US'

Poté vytvořte virtuální síť. Ověřte, že se zadané adresní prostory nepřekrývají s adresními prostory ve vaší místní síti.

Následující ukázka vytvoří virtuální síť s názvem *testvnet* a dvě podsítě: jednu s názvem *GatewaySubnet* a druhou s názvem *Subnet1*. Je důležité vytvořit jednu podsíť s konkrétním názvem *GatewaySubnet*. Pokud použijete jiný název, konfigurace připojení se nezdaří. 

Nastavte proměnné.

    $subnet1 = New-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -AddressPrefix 10.0.0.0/28
    $subnet2 = New-AzureRmVirtualNetworkSubnetConfig -Name 'Subnet1' -AddressPrefix '10.0.1.0/28'

Vytvořte virtuální síť.

    New-AzureRmVirtualNetwork -Name testvnet -ResourceGroupName testrg `
    -Location 'West US' -AddressPrefix 10.0.0.0/16 -Subnet $subnet1, $subnet2

### <a name="gatewaysubnet"></a>Chcete-li přidat podsíť brány k již vytvořené virtuální síti
Tento krok je vyžadován pouze pokud potřebujete přidat podsíť brány k již dříve vytvořené virtuální síti.

S vytvořením podsítě brány vám pomůže následující ukázka. Ujistěte se, že podsíť brány pojmenujete „GatewaySubnet“. Pokud zadáte jiný název, vytvoříte sice podsíť, ale Azure ji nebude považovat za podsíť brány.

Nastavte proměnné.

    $vnet = Get-AzureRmVirtualNetwork -ResourceGroupName testrg -Name testvnet

Vytvořte podsíť brány.

    Add-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -AddressPrefix 10.0.3.0/28 -VirtualNetwork $vnet

Nastavte konfiguraci. 

    Set-AzureRmVirtualNetwork -VirtualNetwork $vnet

## 3. <a name="localnet"></a>Přidání brány místní sítě
Ve virtuální síti brána místní sítě obvykle odkazuje na vaše místní umístění. Pro toto umístění určíte název, podle kterého na ně bude Azure odkazovat, a také zadáte předponu adresního prostoru pro bránu místní sítě. 

Azure pomocí zadané předpony IP adresy rozpozná, jaký provoz má zasílat na vaše místní umístění. To znamená, že budete muset zadat každou předponu adresy, kterou chcete přidružit k vaší bráně místní sítě. Tyto předpony můžete snadno aktualizovat v případě změny vaší místní sítě. 

Při použití příkladů v prostředí PowerShell je potřeba počítat s následujícím:

* *GatewayIPAddress* je IP adresa vašeho místního zařízení VPN. Zařízení VPN nesmí být umístěné za překladem adres (NAT). 
* *AddressPrefix* je váš místní adresní prostor.

Chcete-li přidat bránu místní sítě s jednou předponou adresy:

    New-AzureRmLocalNetworkGateway -Name LocalSite -ResourceGroupName testrg `
    -Location 'West US' -GatewayIpAddress '23.99.221.164' -AddressPrefix '10.5.51.0/24'

Chcete-li přidat bránu místní sítě s více předponami adresy:

    New-AzureRmLocalNetworkGateway -Name LocalSite -ResourceGroupName testrg `
    -Location 'West US' -GatewayIpAddress '23.99.221.164' -AddressPrefix @('10.0.0.0/24','20.0.0.0/24')

### Chcete-li upravit předpony IP adresy pro bránu místní sítě
Někdy dojde ke změně předpon brány místní sítě. Postup upravení předpon IP adresy závisí na tom, zda jste vytvořili připojení k bráně VPN. Viz oddíl [Úprava předpon IP adresy pro bránu místní sítě](#modify) v tomto článku.

## 4. Vyžádání veřejné IP adresy pro bránu VPN
Dále si vyžádáte veřejnou IP adresu, která bude přidělena pro vaši bránu virtuální sítě Azure VPN. Nejedná se o stejnou IP adresu, která je přiřazená vašemu zařízení VPN, místo toho je přiřazená samotné bráně Azure VPN. Nelze zadat IP adresu, kterou chcete použít. Přiděluje se pro bránu dynamicky. Tuto IP adresu použijete při konfigurování vašeho místního zařízení VPN pro připojení k bráně.

Brána Azure VPN pro model nasazení Resource Manager v současné době podporuje pouze veřejné IP adresy pomocí metody dynamického přidělení. To ale neznamená, že se IP adresa změní. Změna IP adresy brány Azure VPN proběhne pouze v případě odstranění a nového vytvoření brány. V případě změny velikosti, resetování nebo jiné operace údržby/upgradu vaší brány Azure VPN se veřejná IP adresa brány nezmění.

Použijte následující ukázku prostředí PowerShell:

    $gwpip= New-AzureRmPublicIpAddress -Name gwpip -ResourceGroupName testrg -Location 'West US' -AllocationMethod Dynamic

## 5. Vytvoření konfigurace adresování IP brány
Konfigurace brány definuje podsíť a veřejnou IP adresu, která se bude používat. Podle následující ukázky vytvořte vlastní konfiguraci brány.

    $vnet = Get-AzureRmVirtualNetwork -Name testvnet -ResourceGroupName testrg
    $subnet = Get-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -VirtualNetwork $vnet
    $gwipconfig = New-AzureRmVirtualNetworkGatewayIpConfig -Name gwipconfig1 -SubnetId $subnet.Id -PublicIpAddressId $gwpip.Id 

## 6. Vytvoření brány virtuální sítě
V tomto kroku vytvoříte bránu virtuální sítě. Dokončení vytvoření brány může trvat delší dobu. Často 45 minut nebo déle. 

Použijte následující hodnoty:

* Hodnota *-GatewayType* pro konfiguraci Site-to-Site je *Vpn*. Typ brány je vždy specifický pro konfiguraci, kterou implementujete. Například jiné konfigurace brány mohou vyžadovat jako -GatewayType hodnotu ExpressRoute. 
* Hodnota *-VpnType* může být *RouteBased* (v některé dokumentaci nazývaná Dynamická brána), nebo *PolicyBased* (v některé dokumentaci nazývaná Statická brána). Další informace o typech brány VPN najdete v tématu [Informace o branách VPN](vpn-gateway-about-vpngateways.md#vpntype).
* Hodnota *-GatewaySku* může být *Basic*, *Standard*, nebo *HighPerformance*.   
  
        New-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg `
        -Location 'West US' -IpConfigurations $gwipconfig -GatewayType Vpn `
        -VpnType RouteBased -GatewaySku Standard

## 7. Konfigurace zařízení VPN
V tomto okamžiku budete potřebovat veřejnou IP adresu brány virtuální sítě pro konfiguraci místního zařízení VPN. Pro konkrétní informace o konfiguraci se obraťte na výrobce zařízení. Další informace najdete také v tématu [Zařízení VPN](vpn-gateway-about-vpn-devices.md).

Se zjištěním veřejné IP adresy vaší brány virtuální sítě vám pomůže následující ukázka:

    Get-AzureRmPublicIpAddress -Name gwpip -ResourceGroupName testrg

## 8. Vytvoření připojení VPN
Dále vytvoříte připojení VPN typu Site-to-Site mezi bránou virtuální sítě a zařízením VPN. Nezapomeňte hodnoty nahradit vlastními. Sdílený klíč se musí shodovat s hodnotou, kterou jste použili pro konfiguraci zařízení VPN. Všimněte si, že hodnota `-ConnectionType` pro připojení Site-to-Site je *IPsec*. 

Nastavte proměnné.

    $gateway1 = Get-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg
    $local = Get-AzureRmLocalNetworkGateway -Name LocalSite -ResourceGroupName testrg

Vytvořte připojení.

    New-AzureRmVirtualNetworkGatewayConnection -Name localtovon -ResourceGroupName testrg `
    -Location 'West US' -VirtualNetworkGateway1 $gateway1 -LocalNetworkGateway2 $local `
    -ConnectionType IPsec -RoutingWeight 10 -SharedKey 'abc123'

Za malou chvíli dojde k vytvoření připojení. 

## <a name="toverify"></a>Ověření připojení VPN
Existuje několik různých způsobů, jak ověřit připojení VPN.

[!INCLUDE [vpn-gateway-verify-connection-rm](../../includes/vpn-gateway-verify-connection-rm-include.md)]

## <a name="modify"></a>Chcete-li upravit předpony IP adresy pro bránu místní sítě
Pokud potřebujete změnit předpony pro bránu místní sítě, postupujte podle následujících pokynů. K dispozici jsou dvě sady pokynů. Pokyny, které zvolíte, závisí na tom, jestli jste už vytvořili připojení brány. 

[!INCLUDE [vpn-gateway-modify-ip-prefix-rm](../../includes/vpn-gateway-modify-ip-prefix-rm-include.md)]

## <a name="modifygwipaddress"></a>Změna IP adresy místní síťové brány
[!INCLUDE [vpn-gateway-modify-lng-gateway-ip-rm](../../includes/vpn-gateway-modify-lng-gateway-ip-rm-include.md)]

## Další kroky
* Do virtuálních sítí můžete přidat virtuální počítače. Kroky jsou uvedeny v tématu [Vytvoření virtuálního počítače](../virtual-machines/virtual-machines-windows-hero-tutorial.md).
* Informace o protokolu BGP najdete v tématech [Přehled protokolu BGP](vpn-gateway-bgp-overview.md) a [Postup při konfiguraci protokolu BGP](vpn-gateway-bgp-resource-manager-ps.md).

<!--HONumber=Oct16_HO3-->


