<properties 
   pageTitle="Topologie připojení ke službě VPN Gateway | Microsoft Azure"
   description="Podívejte se na topologie připojení ke službě VPN Gateway a na dostupné nástroje konfigurace a modely nasazení."
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
   ms.date="07/19/2016"
   ms.author="cherylmc" />

# Připojení ke službě Azure VPN Gateway

Tento článek ukazuje základní topologie připojení brány VPN. Grafika a popisy vám pomohou s výběrem topologie konfigurace, která bude odpovídat vašim požadavkům. Přestože tento článek probírá základní topologie, je možné vytvořit komplexnější topologie použitím diagramů jako vodítek.

Každá topologie obsahuje tabulku se seznamem modelů nasazení, pro které je topologie dostupná, nástroje pro nasazení, které můžete použít ke konfiguraci jednotlivých topologií, a přímý odkaz na článek, pokud je k dispozici. Tabulky pravidelně aktualizujeme s tím, jak vznikají nové články a nástroje pro nasazení, které lze použít.

Po rozhodnutí ohledně připojení, které chcete vytvořit se budou pokyny, podle kterých vytvoříte vlastní bránu VPN, lišit v závislosti na modelu nasazení, který jste použili k vytvoření svojí virtuální sítě. Například pokud jste virtuální síť vytvořili pomocí modelu nasazení Classic, budete při vytváření a konfiguraci brány VPN postupovat podle pokynů pro model nasazení Classic. Není možné vytvořit bránu VPN Resource Manageru pro virtuální síť nasazenou pomocí modelu nasazení Classic. Další informace o modelech nasazení najdete v tématu [Pochopení modelu nasazení Classic a modelu nasazení Resource Manager](../resource-manager-deployment-model.md).

## Site-to-Site a Multi-Site

Připojení typu Site-to-Site je připojení přes tunelové propojení sítě VPN prostřednictvím protokolu IPsec/IKE (IKEv1 nebo IKEv2). Tento typ připojení vyžaduje zařízení VPN nebo místní službu Windows Server RRAS. Připojení typu Site-to-Site lze použít pro konfigurace mezi různými místy a pro hybridní konfigurace.   


**Diagram Site-to-Site**

![Připojení typu Site-to-Site](./media/vpn-gateway-topology/site2site.png "site-to-site")

Pokud používáte trasovou VPN Azure, můžete vytvořit a nakonfigurovat více připojení VPN typu Site-to-Site k místním sítím. Tento typ konfigurace se často nazývá připojení „s více servery“.
 

**Diagram Multi-Site**

![Připojení typu Multi-Site](./media/vpn-gateway-topology/multisite.png "multi-site")


**Dostupné modely a metody nasazení**

[AZURE.INCLUDE [vpn-gateway-table-site-to-site](../../includes/vpn-gateway-table-site-to-site-include.md)] 

## VNet-to-VNet

Propojení virtuální sítě s jinou virtuální sítí (VNet-to-VNet) je velmi podobné propojení virtuální sítě s místním serverem. Oba typy připojení využívají bránu VPN Azure VPN, která poskytuje zabezpečené tunelové propojení prostřednictvím protokolu IPsec/IKE. Virtuální sítě, které propojujete, se mohou nacházet v různých oblastech nebo předplatných. Dokonce můžete kombinovat komunikaci VNet-to-VNet s konfigurací s více servery. Díky tomu je možné vytvářet topologie sítí, ve kterých se používá propojování více míst i propojování virtuálních sítí.

Azure v současné době obsahuje dva modely nasazení: Azure Service Manager a Azure Resource Manager. Pokud již nějakou dobu používáte Azure, pravděpodobně vaše virtuální počítače a instance rolí fungují na klasické virtuální síti, zatímco vaše novější virtuální počítače a instance rolí mohou fungovat na virtuální síti vytvořené v ARM. Propojení virtuálních sítí můžete dokonce vytvořit i když jsou v různých modelech nasazení.


**Diagram VNet-to-VNet**

![Připojení typu VNet-to-VNet](./media/vpn-gateway-topology/vnet2vnet.png "vnet-to-vnet")


**Dostupné modely a metody nasazení**

[AZURE.INCLUDE [vpn-gateway-table-vnet-to-vnet](../../includes/vpn-gateway-table-vnet-to-vnet-include.md)] 


## Point-to-Site

Konfigurace Point-to-Site vám umožňuje vytvoření bezpečného připojení k virtuální síti z jednotlivých klientských počítačů. Připojení VPN je vytvořeno spuštěním připojení na klientském počítači. Toto řešení je užitečné, pokud se chcete připojit k virtuální síti ze vzdáleného umístění, například z domova nebo z konference, nebo pokud máte pouze několik klientů, kteří se potřebují připojit k virtuální síti. 

Připojení typu Point-to-Site je připojení VPN prostřednictvím protokolu SSTP (Secure Socket Tunneling Protocol). Připojení typu Point-to-Site k fungování nevyžadují zařízení VPN ani veřejnou IP adresu. 

**Diagram Point-to-Site**

![Připojení typu Point-to-Site](./media/vpn-gateway-topology/point2site.png "point-to-site")

**Dostupné modely a metody nasazení**

[AZURE.INCLUDE [vpn-gateway-table-point-to-site](../../includes/vpn-gateway-table-point-to-site-include.md)] 


##ExpressRoute

[AZURE.INCLUDE [expressroute-intro](../../includes/expressroute-intro-include.md)]

Další informace o ExpressRoute najdete v [Technickém přehledu ExpressRoute](../expressroute/expressroute-introduction.md).



## Současně existující připojení typu Site-to-Site a ExpressRoute

ExpressRoute je přímé vyhrazené připojení ke službám Microsoftu, včetně Azure, z vaší sítě WAN, nikoli prostřednictvím veřejného internetu. Provoz VPN typu Site-to-Site je přenášen zašifrovaně prostřednictvím veřejného internetu. Možnost konfigurace VPN typu Site-to-Site a připojení ExpressRoute pro stejnou virtuální síť má několik výhod. Můžete nakonfigurovat VPN typu Site-to-Site jako zabezpečenou cestu převzetí služeb při selhání pro ExpressRoute, nebo použít VPN typu Site-to-Site pro připojení k webům, které nejsou součástí vaší sítě, ale jsou připojené prostřednictvím ExpressRoute. 


**Diagram současně existujících připojení**

![Současně existující připojení](./media/vpn-gateway-topology/expressroutes2s.png "expressroute-site2site")


**Dostupné modely a metody nasazení**

[AZURE.INCLUDE [vpn-gateway-table-coexist](../../includes/vpn-gateway-table-coexist-include.md)] 




## Další kroky

Je vhodné se seznámit s tématy v článcích [Informace o službě VPN Gateway](vpn-gateway-about-vpngateways.md) a [Služba VPN Gateway – nejčastější dotazy](vpn-gateway-vpn-faq.md) pro lepší porozumění nastavení bran VPN.





 



<!----HONumber=Aug16_HO4-->


