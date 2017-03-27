---
title: "Přehled o branách VPN: Vytváření připojení VPN mezi místy a virtuálními sítěmi Azure | Dokumentace Microsoftu"
description: "Tento přehled o branách VPN vysvětluje způsoby připojení k virtuálním sítím Azure pomocí připojení VPN přes internet. Součástí článku jsou diagramy základní konfigurace připojení."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager,azure-service-management
ms.assetid: 2358dd5a-cd76-42c3-baf3-2f35aadc64c8
ms.service: vpn-gateway
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/13/2017
ms.author: cherylmc
translationtype: Human Translation
ms.sourcegitcommit: afe143848fae473d08dd33a3df4ab4ed92b731fa
ms.openlocfilehash: 3be72ac862650d2381a56c4ab682e41a8aaa8a27
ms.lasthandoff: 03/17/2017


---
# <a name="about-vpn-gateway"></a>Informace o službě VPN Gateway
Pokud chcete posílat síťový provoz mezi vaší virtuální sítí Azure a místní sítí, musíte pro virtuální síť vytvořit bránu. Brána VPN je typem brány virtuální sítě, která odesílá šifrovaný síťový provoz přes veřejné spojení. Brány VPN můžete použít také k posílání síťového provozu mezi virtuálními sítěmi Azure po síti Microsoftu.

Existují dva typy brány virtuální sítě: Vpn a ExpressRoute. Při vytváření brány virtuální sítě musíte určit typ brány, který chcete vytvořit. Brána VPN je brána virtuální sítě, která využívá typ brány Vpn. 

Každá virtuální síť může mít dvě brány virtuální sítě, ale každého typu jenom jednu. V závislosti na nastavení, které zvolíte, můžete k jedné bráně VPN vytvořit několik připojení. Příkladem je konfigurace připojení typu Multi-Site. Když vytvoříte několik připojení ke stejné bráně VPN, všechny tunely VPN, včetně sítí VPN typu Point-to-Site, sdílejí šířku pásma, která je pro tuto bránu dostupná.

## <a name="configuring-a-vpn-gateway"></a>Konfigurace služby VPN Gateway
Připojení brány VPN se spoléhá na několik prostředků nakonfigurovaných se specifickými nastaveními. Většinu prostředků lze nakonfigurovat jednotlivě, nicméně v některých případech je třeba je konfigurovat v určitém pořadí.

###<a name="settings"></a>Nastavení
Nastavení, která jste pro jednotlivé zdroje zvolili, jsou pro vytvoření úspěšného připojení zásadní. Informace o jednotlivých prostředcích a nastaveních služby VPN Gateway najdete v tématu [Informace o nastavení služby VPN Gateway](vpn-gateway-about-vpn-gateway-settings.md). Najdete tu informace, které vám pomohou pochopit typy bran, typy sítí VPN, typy připojení, podsítě brány, místní síťové brány a různá další nastavení prostředků, o kterých možná uvažujete.

###<a name="deployment-tools"></a>Nástroje pro nasazení
Prostředky můžete začít vytvářet a konfigurovat pomocí konfiguračního nástroje, jako je například Azure Portal. Později se můžete rozhodnout používat ke konfiguraci dalších prostředků nebo úpravám stávajících prostředků jiný nástroj, třeba PowerShell. V současné době nelze konfigurovat všechny prostředky a nastavení prostředků pomocí webu Azure Portal. Pokyny v článcích pro každou topologii připojení určují, kdy je zapotřebí specifický konfigurační nástroj. 

###<a name="deployment-model"></a>Model nasazení
Kroky při konfiguraci brány VPN se budou lišit v závislosti na modelu nasazení, který jste použili k vytvoření virtuální sítě. Například pokud jste virtuální síť vytvořili pomocí modelu nasazení Classic, budete při vytváření a konfiguraci brány VPN postupovat podle pokynů pro model nasazení Classic. Další informace o modelech nasazení najdete v tématu [Pochopení modelů nasazení Resource Manager a Classic](../azure-resource-manager/resource-manager-deployment-model.md).

## <a name="diagrams"></a>Diagramy topologie připojení
Je důležité vědět, že pro připojení brány VPN jsou dostupné různé konfigurace. Musíte určit, která konfigurace bude nejlépe vyhovovat vašim potřebám. V následujících oddílech si můžete prohlédnout diagramy topologie a informace o následujících připojení brány sítě VPN. Následující oddíly obsahují tabulky, které uvádějí:

* dostupný model nasazení,
* dostupné konfigurační nástroje,
* odkazy na příslušný článek, pokud existuje.

Diagramy a popisy vám pomohou s výběrem topologie připojení, která bude odpovídat vašim požadavkům. Diagramy popisují základní topologie, ale je možné vytvořit komplexnější konfigurace s použitím diagramů jako vodítek.

## <a name="site-to-site-and-multi-site-ipsecike-vpn-tunnel"></a>Site-to-Site a Multi-Site (tunel VPN IPsec/IKE)
### <a name="S2S"></a>Site-to-Site
Připojení brány VPN typu Site-to-Site (S2S) je připojení přes tunel VPN prostřednictvím protokolu IPsec/IKE (IKEv1 nebo IKEv2). Tento typ připojení vyžaduje místní zařízení sítě VPN, které má přiřazenou veřejnou IP adresu a není umístěné za službou NAT. Připojení S2S můžete použít pro konfigurace mezi různými místy a pro hybridní konfigurace.   

![Příklad propojení Site-to-Site pomocí Azure VPN Gateway](./media/vpn-gateway-about-vpngateways/vpngateway-site-to-site-connection-diagram.png)

### <a name="Multi"></a>Multi-Site
Tento typ připojení je variací připojení Site-to-Site. Z brány virtuální sítě vytvoříte několik připojení VPN, obvykle pro připojení k několika místním lokalitám. Při práci s několika připojeními je nutné použít typ sítě VPN RouteBased (při práci s klasickými virtuálními sítěmi se označuje jako dynamická brána). Vzhledem k tomu, že virtuální síť může mít jenom jednu bránu virtuální sítě, všechna připojení prostřednictvím brány sdílejí dostupnou šířku pásma. To se často označuje jako připojení „více lokalit“ (Multi-Site).

![Příklad propojení Multi-Site pomocí Azure VPN Gateway](./media/vpn-gateway-about-vpngateways/vpngateway-multisite-connection-diagram.png)

### <a name="deployment-models-and-methods-for-site-to-site-and-multi-site"></a>Modely nasazení a metody pro Site-to-Site a Multi-Site
[!INCLUDE [vpn-gateway-table-site-to-site](../../includes/vpn-gateway-table-site-to-site-include.md)]

## <a name="P2S"></a>Point-to-Site (VPN prostřednictvím protokolu SSTP)
Připojení brány VPN typu Point-to-Site (P2S) umožňuje vytvořit zabezpečené připojení k virtuální síti z jednotlivých klientských počítačů. P2S je připojení VPN prostřednictvím protokolu SSTP (Secure Socket Tunneling Protocol). Připojení typu P2S k fungování nevyžadují zařízení VPN ani veřejnou IP adresu. Připojení VPN se navazuje ze strany klientského počítače. Toto řešení je užitečné, pokud se chcete připojit k virtuální síti ze vzdáleného umístění, například z domova nebo z konference, nebo pokud máte pouze několik klientů, kteří se potřebují k virtuální síti připojit. Připojení typu P2S je možné použít s připojeními typu S2S prostřednictvím stejné brány VPN za předpokladu, že všechny požadavky na konfiguraci obou připojení jsou kompatibilní.

![Příklad propojení Point-to-Site pomocí Azure VPN Gateway](./media/vpn-gateway-about-vpngateways/vpngateway-point-to-site-connection-diagram.png)

### <a name="deployment-models-and-methods-for-point-to-site"></a>Modely nasazení a metody pro Point-to-Site
[!INCLUDE [vpn-gateway-table-point-to-site](../../includes/vpn-gateway-table-point-to-site-include.md)]

## <a name="V2V"></a>Připojení typu VNet-to-VNet (tunel VPN IPsec/IKE)
Propojení virtuální sítě s jinou virtuální sítí (VNet-to-VNet) je podobné propojení virtuální sítě s místním serverem. Oba typy připojení využívají bránu VPN k poskytnutí zabezpečeného tunelového propojení prostřednictvím protokolu IPsec/IKE. Dokonce je možné kombinovat komunikaci typu VNet-to-VNet s konfiguracemi připojení více lokalit. Díky tomu je možné vytvářet topologie sítí, ve kterých se používá propojování více míst i propojování virtuálních sítí.

Virtuální sítě, které propojujete, můžou být:

* v jedné nebo několika oblastech,
* v jednom nebo několika předplatných, 
* v jednom nebo několika modelech nasazení.

![Příklad propojení VNet-to-VNet pomocí Azure VPN Gateway](./media/vpn-gateway-about-vpngateways/vpngateway-vnet-to-vnet-connection-diagram.png)

###<a name="connections-between-deployment-models"></a>Připojení mezi různými modely nasazení
Azure v současné době nabízí dva modely nasazení: Classic a Resource Manager. Pokud již Azure nějakou dobu používáte, pravděpodobně vaše virtuální počítače a role instancí Azure fungují ve virtuální síti Classic. Vaše novější virtuální počítače a role instancí však mohou používat virtuální síť vytvořenou v nástroji Resource Manager. Můžete vytvořit připojení mezi virtuálními sítěmi umožňující prostředkům v jedné virtuální síti přímo komunikovat s prostředky v jiné.

###<a name="vnet-peering"></a>Partnerské vztahy virtuálních sítí
Pokud virtuální síť splňuje určité požadavky, je možné k vytvoření připojení využít metodu VNet peering. VNet peering nepoužívá bránu virtuální sítě. Další informace najdete v tématu [Partnerské vztahy virtuálních sítí](../virtual-network/virtual-network-peering-overview.md).

###<a name="deployment-models-and-methods-for-vnet-to-vnet"></a>Modely nasazení a metody pro VNet-to-VNet
[!INCLUDE [vpn-gateway-table-vnet-to-vnet](../../includes/vpn-gateway-table-vnet-to-vnet-include.md)]

## <a name="ExpressRoute"></a>ExpressRoute (vyhrazené soukromé připojení)
Microsoft Azure ExpressRoute umožňuje rozšířit vaše místní sítě do cloudu Microsoftu přes vyhrazené soukromé připojení zajišťované poskytovatelem připojení. Pomocí ExpressRoute může vytvořit připojení ke cloudovým službám Microsoftu, jako je například Microsoft Azure, Office 365 a CRM Online. Co se týká připojení, může se jednat o síť typu any-to-any (IP VPN), síť Ethernet typu point-to-point nebo virtuální křížové připojení prostřednictvím poskytovatele připojení ve společném umístění.

Připojení ExpressRoute se nepřenášejí prostřednictvím veřejného internetu. To dovoluje připojením ExpressRoute poskytovat větší spolehlivost, vyšší rychlost, nižší latenci a vyšší zabezpečení než typická připojení přes internet.

Připojení typu ExpressRoute nepoužívá bránu sítě VPN, přestože brána virtuální sítě využívá jako součást požadované konfigurace. U připojení ExpressRoute je brána virtuální sítě nakonfigurovaná s typem ExpressRoute (a ne Vpn). Další informace o ExpressRoute najdete v [Technickém přehledu ExpressRoute](../expressroute/expressroute-introduction.md).

## <a name="coexisting"></a>Současně existující připojení typu Site-to-Site a ExpressRoute
ExpressRoute je přímé vyhrazené připojení ke službám Microsoftu, včetně Azure, z vaší sítě WAN (nikoli prostřednictvím veřejného internetu). Provoz VPN typu Site-to-Site je přenášen zašifrovaně prostřednictvím veřejného internetu. Možnost konfigurace VPN typu Site-to-Site a připojení ExpressRoute pro stejnou virtuální síť má několik výhod.

Můžete nakonfigurovat síť VPN typu Site-to-Site jako zabezpečenou cestu převzetí služeb při selhání pro ExpressRoute, nebo použít VPN typu Site-to-Site pro připojení k webům, které nejsou součástí vaší sítě, ale jsou připojené prostřednictvím ExpressRoute. Tato konfigurace vyžaduje dvě brány virtuální sítě pro stejnou virtuální síť, jednu typu Vpn a druhou typu ExpressRoute.

![Příklad současné existence ExpressRoute a VPN Gateway](./media/vpn-gateway-about-vpngateways/expressroute-vpngateway-coexisting-connections-diagram.png)

### <a name="deployment-models-and-methods-for-s2s-and-expressroute"></a>Modely nasazení a metody pro S2S a ExpressRoute
[!INCLUDE [vpn-gateway-table-coexist](../../includes/vpn-gateway-table-coexist-include.md)]

## <a name="pricing"></a>Ceny
[!INCLUDE [vpn-gateway-about-pricing-include](../../includes/vpn-gateway-about-pricing-include.md)]

## <a name="gateway-skus"></a>Skladové jednotky (SKU) brány
[!INCLUDE [vpn-gateway-gwsku-include](../../includes/vpn-gateway-gwsku-include.md)]

Další informace o skladových jednotkách (SKU) brány pro službu VPN Gateway najdete v tématu [Skladové jednotky (SKU) brány](vpn-gateway-about-vpn-gateway-settings.md#gwsku).

### <a name="estimated-aggregate-throughput-by-sku"></a>Odhadovaná agregovaná propustnost podle typů SKU
[!INCLUDE [vpn-gateway-table-gwtype-aggthroughput](../../includes/vpn-gateway-table-gwtype-aggtput-include.md)]

## <a name="next-steps"></a>Další kroky
- Plánování konfigurace brány VPN. Viz [Plánování a návrh pro VPN Gateway](vpn-gateway-plan-design.md).
- Další informace najdete v tématu věnovaném [nejčastějším dotazům k VPN Gateway](vpn-gateway-vpn-faq.md).
- Přečtěte si téma [Předplatné a omezení služeb](../azure-subscription-service-limits.md#networking-limits).


