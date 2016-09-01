<properties 
   pageTitle="Informace o službě VPN Gateway | Microsoft Azure"
   description="Zjistěte více o službě VPN Gateway pro Azure Virtual Network."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager,azure-service-management"/>
<tags 
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="07/20/2016"
   ms.author="cherylmc" />

# Informace o službě VPN Gateway

Služba VPN Gateway je souborem nastavení, která slouží k posílání síťového provozu mezi virtuálními sítěmi a místními umístěními. Oddíly v tomto článku popisují nastavení, která se týkají brány sítě VPN. Brána sítě VPN se používá pro připojení typu Site-to-Site, Point-to-Site a ExpressRoute. Brána sítě VPN slouží také k posílání provozu mezi virtuálními sítěmi v rámci Azure (VNet-to-VNet). 

Bránu sítě VPN lze přidat do virtuální sítě pro vytvoření připojení. Každá virtuální síť může obsahovat pouze jednu brány sítě VPN a několik kroků konfigurace specifické pro každé připojení. Grafy připojení naleznete v části [topologie připojení brány sítě VPN](vpn-gateway-topology.md). 

## <a name="gwsku"></a>Skladové jednotky (SKU) brány

Při vytváření brány VPN budete muset určit SKU brány, které chcete použít. Identifikátory SKU brány se vztahují na typy brány ExpressRoute a VPN. Ceny se liší pro jednotlivé SKU brány. Informace o cenách najdete v tématu [Ceny služby VPN Gateway](https://azure.microsoft.com/pricing/details/vpn-gateway/). Další informace o ExpressRoute najdete v [Technickém přehledu ExpressRoute](../expressroute/expressroute-introduction.md).

Existují 3 SKU služby VPN Gateway:

- Basic
- Standard
- Vysokovýkonné

Následující příklad určuje parametr `-GatewaySku` jako *Standardní*.

    New-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg -Location 'West US' -IpConfigurations $gwipconfig -GatewaySku Standard -GatewayType Vpn -VpnType RouteBased

###  <a name="aggthroughput"></a>Odhadovaná agregovaná propustnost podle typů SKU a brány


Následující tabulka ukazuje typy brány a odhadovanou agregovanou propustnost. Tato tabulka platí pro model nasazení Resource Manager a pro model nasazení Classic.

[AZURE.INCLUDE [vpn-gateway-table-gwtype-aggthroughput](../../includes/vpn-gateway-table-gwtype-aggtput-include.md)] 

## <a name="gwtype"></a>Typy bran

Typ brány určuje, jak se brána samotná připojuje, a jedná se o požadované nastavení pro model nasazení Resource Manager. Nepleťte si typ brány s typem sítě VPN, který určuje typ směrování pro vaši síť VPN. Dostupné hodnoty pro `-GatewayType` jsou: 

- Vpn
- ExpressRoute


Tento příklad pro model nasazení Resource Manager určuje parametr -GatewayType jako *Vpn*. Při vytváření brány se musíte ujistit, že typ brány odpovídá vaší konfiguraci. 

    New-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg -Location 'West US' -IpConfigurations $gwipconfig -GatewayType Vpn -VpnType RouteBased

## <a name="connectiontype"></a>Typy připojení

Každá konfigurace vyžaduje určitý typ připojení. Dostupné hodnoty prostředí PowerShell v Resource Manageru pro `-ConnectionType` jsou:

- Protokol IPsec
- Vnet2Vnet
- ExpressRoute
- VPNClient

V následujícím příkladu vytváříme připojení typu Site-to-Site, které vyžaduje typ připojení „IPsec“.

    New-AzureRmVirtualNetworkGatewayConnection -Name localtovon -ResourceGroupName testrg -Location 'West US' -VirtualNetworkGateway1 $gateway1 -LocalNetworkGateway2 $local -ConnectionType IPsec -RoutingWeight 10 -SharedKey 'abc123'

## <a name="vpntype"></a>Typy sítě VPN

Každá konfigurace vyžaduje pro správné fungování určitý typ sítě VPN. Pokud kombinujete dvě konfigurace, například vytváříte-li připojení typu Site-to-Site a Point-to-Site ke stejné virtuální síti, musíte použít typ sítě VPN, který splňuje požadavky obou připojení. 

V případě současných připojení typu Point-to-Site a Site-to-Site je potřeba použít trasový typ sítě VPN, pokud pracujete s modelem nasazení Azure Resource Manager, nebo dynamickou bránu, pokud pracujete s modelem nasazení Classic.

Při vytváření konfigurace vyberte typ sítě VPN, který je požadován pro vaše připojení. 

Existují dva typy sítě VPN:

[AZURE.INCLUDE [vpn-gateway-vpntype](../../includes/vpn-gateway-vpntype-include.md)]

Tento příklad pro model nasazení Resource Manager určuje parametr `-VpnType` jako *RouteBased*. Při vytváření brány se musíte ujistit, že parametr -VpnType odpovídá vaší konfiguraci. 

    New-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg -Location 'West US' -IpConfigurations $gwipconfig -GatewayType Vpn -VpnType RouteBased

##  <a name="requirements"></a>Požadavky na bránu

[AZURE.INCLUDE [vpn-gateway-table-requirements](../../includes/vpn-gateway-table-requirements-include.md)] 


## <a name="gwsub"></a>Podsíť brány

Abyste mohli konfigurovat bránu VPN, musíte nejdříve pro svoji virtuální síť vytvořit podsíť brány. Pro správné fungování podsítě brány je nutné, aby měla název *GatewaySubnet*. Azure tak bude vědět, že se pro bránu má použít tato podsíť.<BR>Používáte-li klasický portál, název podsítě brány bude v rozhraní portálu automaticky *Gateway*. To platí pouze pro zobrazení podsítě brány v klasickém portálu. V tomto případě se podsíť automaticky vytvoří v Azure jako *GatewaySubnet* a tímto způsobem ji lze zobrazit na webu Azure Portal a v prostředí PowerShell.

Minimální velikost podsítě brány zcela závisí na konfiguraci, kterou chcete vytvořit. Přestože je pro některé konfigurace možné vytvořit tak malou podsíť brány, jako je /29, doporučujeme vytvořit podsíť brány o velikosti /28 nebo větší (/28, /27, /26 atd.). 

Vytvořením brány o větší velikosti předcházíte problémům s omezením velikosti brány. Například pokud jste vytvořili bránu s podsítí brány o velikosti /29 a chcete konfigurovat současné fungování Site-to-Site a ExpressRoute, museli byste odstranit bránu, odstranit podsíť brány, vytvořit podsíť brány o velikosti /28 nebo větší, a poté znovu vytvořit bránu. 

Vytvořením podsítě brány o větší velikosti už na začátku můžete později ušetřit čas při přidávání funkcí konfigurace do prostředí vaší sítě. 

Následující příklad ukazuje podsíť brány s názvem GatewaySubnet. Jak vidíte, zápis CIDR určuje velikost /27, která poskytuje dostatek prostoru pro IP adresy u většiny současných konfigurací.

    Add-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -AddressPrefix 10.0.3.0/27

>[AZURE.IMPORTANT] Ujistěte se, že na GatewaySubnet není aplikovaná Skupina zabezpečení sítě (NSG), což může způsobit selhání připojení.



## <a name="lng"></a>Brány místní sítě

Brána místní sítě obvykle odkazuje na vaše místní umístění. V modelu nasazení Classic se brána místní sítě označovala jako „místní lokalita“. Zadáte název brány místní sítě, veřejnou IP adresu místního zařízení VPN, a zadáte předpony adres umístěné na místním umístění. Azure bude sledovat síťový provoz předpony cílových adres a podle vámi určené konfigurace pro bránu místní sítě bude směrovat pakety odpovídajícím způsobem. Tyto předpony můžete dle potřeby změnit.


### Úprava předpon adres – Resource Manager

Postup úpravy předpon adres se liší podle toho, zda jste již vytvořili svoji bránu VPN. Viz oddíl článku [Úprava předpon adres pro bránu místní sítě](vpn-gateway-create-site-to-site-rm-powershell.md#modify).

V následujícím příkladu vidíte specifikaci brány místní sítě, pojmenovanou MyOnPremisesWest, která obsahuje dvě předpony IP adresy.

    New-AzureRmLocalNetworkGateway -Name MyOnPremisesWest -ResourceGroupName testrg -Location 'West US' -GatewayIpAddress '23.99.221.164' -AddressPrefix @('10.0.0.0/24','20.0.0.0/24') 

### Úprava předpon adres – klasické nasazení

Pokud používáte model nasazení Classic a potřebujete upravit místní lokality, můžete k tomu použít stránku Konfigurace místních sítí v portálu Classic, nebo můžete upravit přímo soubor s konfigurací sítě NETCFG.XML.



## Další kroky

V článku [Služba VPN Gateway – nejčastější dotazy](vpn-gateway-vpn-faq.md) najdete další informace před tím, než se pustíte do plánování a návrhu vlastní konfigurace.





 



<!---HONumber=Aug16_HO4-->


