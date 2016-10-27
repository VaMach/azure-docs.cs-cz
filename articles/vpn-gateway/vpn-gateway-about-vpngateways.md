<properties 
   pageTitle="Informace o službě VPN Gateway | Microsoft Azure"
   description="Přečtěte si o připojeních pomocí služby VPN Gateway pro Azure Virtual Network."
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
   ms.date="10/06/2016"
   ms.author="cherylmc" />


# Informace o službě VPN Gateway


Brána virtuální sítě slouží ke směrování síťového provozu mezi virtuálními sítěmi Azure a místními umístěními, stejně jako mezi virtuálními sítěmi v rámci Azure (VNet-to-VNet). Při konfiguraci brány sítě VPN je třeba vytvořit a nakonfigurovat bránu virtuální sítě a připojení brány virtuální sítě.

V modelu nasazení Resource Managerem je při vytváření prostředku brány virtuální sítě třeba zadat několik nastavení. Jedno z požadovaných nastavení je „-GatewayType“. Existují dva typy brány virtuální sítě: Vpn a ExpressRoute. 

Pokud je síťový provoz směrován přes vyhrazené privátní připojení, použijte typ brány „ExpressRoute“. Ten se označuje také jako brána ExpressRoute. Pokud je síťový provoz směrován šifrovaně přes veřejné připojení, použijte typ brány „Vpn“. Ten se označuje také jako brána VPN. Připojení typu Site-to-Site, Point-to-Site a VNet-to-VNet používají bránu VPN.

Každá virtuální síť může mít pouze jednu bránu virtuální sítě pro každý typ brány. Například můžete mít jednu bránu virtuální sítě, která má typ -GatewayType ExpressRoute a jednu, která má typ -GatewayType Vpn. Tento článek se zaměřují hlavně na službu VPN Gateway. Další informace o ExpressRoute najdete v [Technickém přehledu ExpressRoute](../expressroute/expressroute-introduction.md).

## Ceny

[AZURE.INCLUDE [vpn-gateway-about-pricing-include](../../includes/vpn-gateway-about-pricing-include.md)] 


## Skladové jednotky (SKU) brány

[AZURE.INCLUDE [vpn-gateway-gwsku-include](../../includes/vpn-gateway-gwsku-include.md)]
Sítě VPN PolicyBased mohou být vytvořené jenom pro brány využívající základní SKU.
Další informace o SKU brány najdete v tématu [SKU brány](vpn-gateway-about-vpn-gateway-settings.md#gwsku).

Následující tabulka ukazuje typy brány a odhadovanou agregovanou propustnost. Tato tabulka platí pro model nasazení Resource Manager i pro klasický model.

[AZURE.INCLUDE [vpn-gateway-table-gwtype-aggthroughput](../../includes/vpn-gateway-table-gwtype-aggtput-include.md)] 

## Konfigurace služby VPN Gateway

Pokyny ke konfiguraci služby VPN Gateway se budou lišit v závislosti na modelu nasazení, který jste použili k vytvoření virtuální sítě. Například pokud jste virtuální síť vytvořili pomocí modelu nasazení Classic, budete při vytváření a konfiguraci brány VPN postupovat podle pokynů pro model nasazení Classic. Další informace o modelech nasazení najdete v tématu [Pochopení modelů nasazení Resource Manager a Classic](../resource-manager-deployment-model.md).

Připojení brány VPN se spoléhá na několik prostředků nakonfigurovaných se specifickými nastaveními. Většinu prostředků lze nakonfigurovat jednotlivě, nicméně v některých případech je třeba je konfigurovat v určitém pořadí. Prostředky můžete začít vytvářet a konfigurovat pomocí konfiguračního nástroje, jako je například Azure Portal. Později se můžete rozhodnout používat ke konfiguraci dalších prostředků nebo úpravám stávajících prostředků jiný nástroj, například PowerShell. V současné době nelze konfigurovat všechny prostředky a nastavení prostředků pomocí webu Azure Portal. Pokyny v článcích pro každou topologii připojení určují, kdy je zapotřebí specifický konfigurační nástroj. Informace o jednotlivých prostředcích a nastaveních služby VPN Gateway najdete v tématu [Informace o nastavení služby VPN Gateway](vpn-gateway-about-vpn-gateway-settings.md).

Následující oddíly obsahují tabulky, které uvádějí:

- dostupný model nasazení,
- dostupné konfigurační nástroje,
- odkaz na příslušný článek, pokud existuje.

Diagramy a popisy vám pomohou s výběrem topologie připojení, která bude odpovídat vašim požadavkům. Diagramy popisují základní topologie, ale je možné vytvořit komplexnější konfigurace s použitím diagramů jako vodítek.

## Site-to-Site a Multi-Site

### Site-to-Site

Připojení brány VPN typu Site-to-Site (S2S) je připojení přes tunel VPN prostřednictvím protokolu IPsec/IKE (IKEv1 nebo IKEv2). Tento typ připojení vyžaduje místní zařízení sítě VPN, které má přiřazenou veřejnou IP adresu a není umístěné za službou NAT. Připojení S2S můžete použít pro konfigurace mezi různými místy a pro hybridní konfigurace.   

![Připojení typu Site-to-Site](./media/vpn-gateway-about-vpngateways/demos2s.png "site-to-site")


### Multi-Site

Můžete vytvořit a konfigurovat připojení brány VPN mezi vaší virtuální sítí a několika místními sítěmi. Při práci s více připojeními je nutné použít typ sítě VPN RouteBased (dynamická brána pro klasické virtuální sítě). Vzhledem k tomu, že virtuální síť může mít pouze jednu bránu VPN, všechna připojení prostřednictvím brány sdílejí dostupnou šířku pásma. To se často označuje jako připojení „více lokalit“ (Multi-Site).
 

![Připojení typu Multi-Site](./media/vpn-gateway-about-vpngateways/demomulti.png "multi-site")

### Modely nasazení a metody pro Site-to-Site a Multi-Site

[AZURE.INCLUDE [vpn-gateway-table-site-to-site](../../includes/vpn-gateway-table-site-to-site-include.md)] 

## VNet-to-VNet

Propojení virtuální sítě s jinou virtuální sítí (VNet-to-VNet) je podobné propojení virtuální sítě s místním serverem. Oba typy připojení využívají bránu VPN k poskytnutí zabezpečeného tunelového propojení prostřednictvím protokolu IPsec/IKE. Dokonce je možné kombinovat komunikaci typu VNet-to-VNet s konfiguracemi připojení více lokalit. Díky tomu je možné vytvářet topologie sítí, ve kterých se používá propojování více míst i propojování virtuálních sítí.

Virtuální sítě, které propojujete, můžou být:

- v jedné nebo několika oblastech,
- v jednom nebo několika předplatných, 
- v jednom nebo několika modelech nasazení.


![Připojení typu VNet-to-VNet](./media/vpn-gateway-about-vpngateways/demov2v.png "vnet-to-vnet")

#### Připojení mezi různými modely nasazení

Azure v současné době nabízí dva modely nasazení: Classic a Resource Manager. Pokud již Azure nějakou dobu používáte, pravděpodobně vaše virtuální počítače a role instancí Azure fungují ve virtuální síti Classic. Vaše novější virtuální počítače a role instancí však mohou používat virtuální síť vytvořenou v nástroji Resource Manager. Můžete vytvořit připojení mezi virtuálními sítěmi umožňující prostředkům v jedné virtuální síti přímo komunikovat s prostředky v jiné.

#### Partnerské vztahy virtuálních sítí

Pokud virtuální síť splňuje určité požadavky, je možné k vytvoření připojení využít metodu VNet peering. VNet peering nepoužívá bránu virtuální sítě. Další informace najdete v tématu [Partnerské vztahy virtuálních sítí](../virtual-network/virtual-network-peering-overview.md).


### Modely nasazení a metody pro VNet-to-VNet

[AZURE.INCLUDE [vpn-gateway-table-vnet-to-vnet](../../includes/vpn-gateway-table-vnet-to-vnet-include.md)] 


## Point-to-Site

Připojení brány VPN typu Point-to-Site (P2S) umožňuje vytvořit zabezpečené připojení k virtuální síti z jednotlivých klientských počítačů. P2S je připojení VPN prostřednictvím protokolu SSTP (Secure Socket Tunneling Protocol). Připojení typu P2S k fungování nevyžadují zařízení VPN ani veřejnou IP adresu. Připojení VPN se navazuje ze strany klientského počítače. Toto řešení je užitečné, pokud se chcete připojit k virtuální síti ze vzdáleného umístění, například z domova nebo z konference, nebo pokud máte pouze několik klientů, kteří se potřebují k virtuální síti připojit. Připojení typu P2S lze použít ve spojení s připojeními typu S2S prostřednictvím stejné brány VPN, jestliže jsou kompatibilní všechny požadavky na konfiguraci obou připojení.


![Připojení typu Point-to-Site](./media/vpn-gateway-about-vpngateways/demop2s.png "point-to-site")

### Modely nasazení a metody pro Point-to-Site

[AZURE.INCLUDE [vpn-gateway-table-point-to-site](../../includes/vpn-gateway-table-point-to-site-include.md)] 


## ExpressRoute

[AZURE.INCLUDE [expressroute-intro](../../includes/expressroute-intro-include.md)]

U připojení ExpressRoute je brána virtuální sítě nakonfigurovaná s typem „ExpressRoute“ místo „Vpn“. Další informace o ExpressRoute najdete v [Technickém přehledu ExpressRoute](../expressroute/expressroute-introduction.md).


## Současně existující připojení typu Site-to-Site a ExpressRoute

ExpressRoute je přímé vyhrazené připojení ke službám Microsoftu, včetně Azure, z vaší sítě WAN (nikoli prostřednictvím veřejného internetu). Provoz VPN typu Site-to-Site je přenášen zašifrovaně prostřednictvím veřejného internetu. Možnost konfigurace VPN typu Site-to-Site a připojení ExpressRoute pro stejnou virtuální síť má několik výhod.

Můžete nakonfigurovat síť VPN typu Site-to-Site jako zabezpečenou cestu převzetí služeb při selhání pro ExpressRoute, nebo použít VPN typu Site-to-Site pro připojení k webům, které nejsou součástí vaší sítě, ale jsou připojené prostřednictvím ExpressRoute. Všimněte si, že tato konfigurace vyžaduje dvě brány virtuální sítě pro stejnou virtuální síť, jednu typu -GatewayType Vpn a druhou typu -GatewayType ExpressRoute.


![Současně existující připojení](./media/vpn-gateway-about-vpngateways/demoer.png "expressroute-site2site")


### Modely nasazení a metody pro S2S a ExpressRoute

[AZURE.INCLUDE [vpn-gateway-table-coexist](../../includes/vpn-gateway-table-coexist-include.md)] 


## Další kroky

Plánování konfigurace brány VPN. Viz [Plánování a návrh služby VPN Gateway](vpn-gateway-plan-design.md) a [Připojení místní sítě k Azure](../guidance/guidance-connecting-your-on-premises-network-to-azure.md).








 



<!--HONumber=Oct16_HO3-->


