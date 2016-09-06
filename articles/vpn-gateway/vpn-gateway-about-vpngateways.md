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
   ms.date="08/22/2016"
   ms.author="cherylmc" />

# Informace o službě VPN Gateway


Služba VPN Gateway je souborem prostředků, které slouží k posílání síťového provozu mezi virtuálními sítěmi a místními umístěními. Brány se používají pro připojení typu Site-to-Site, Point-to-Site a ExpressRoute. VPN Gateway slouží také k posílání provozu mezi virtuálními sítěmi v rámci Azure (VNet-to-VNet). 

Při vytváření připojení přidáváte bránu virtuální sítě do sítě VNet a konfigurujete další prostředky VPN Gateway a jejich nastavení. Každá virtuální síť může mít pouze jednu bránu virtuální sítě pro každý typ brány. Například můžete mít jednu bránu virtuální sítě, která má typ -GatewayType Vpn a jednu, který má typ -GatewayType ExpressRoute.

Informace týkající se požadavků na brány naleznete v tématu [Požadavky na bránu](vpn-gateway-about-vpn-gateway-settings.md#requirements). Odhadovanou agregovanou propustnost obsahuje téma [Informace o nastavení služby VPN Gateway](vpn-gateway-about-vpn-gateway-settings.md#aggthroughput). Ceny najdete v tématu [Ceny služby VPN Gateway](https://azure.microsoft.com/pricing/details/vpn-gateway). Omezení pro předplatné a služby naleznete v tématu [Omezení sítě](../articles/azure-subscription-service-limits.md#networking-limits).

Když konfigurujete bránu VPN Gateway, pokyny se budou lišit v závislosti na modelu nasazení, který jste použili k vytvoření virtuální sítě. Například pokud jste virtuální síť vytvořili pomocí modelu nasazení Classic, budete při vytváření a konfiguraci brány VPN postupovat podle pokynů pro model nasazení Classic. Další informace najdete v tématu [Pochopení modelu nasazení Classic a modelu nasazení Resource Manager](../resource-manager-deployment-model.md).

Následující část obsahuje tabulky, které uvádějí tyto informace potřebné pro konfiguraci:

- dostupný model nasazení,
- dostupné konfigurační nástroje,
- odkaz na příslušný článek, pokud existuje.


Diagramy a popisy vám pomohou s výběrem topologie konfigurace, která bude odpovídat vašim požadavkům. Diagramy popisují základní topologie, ale je možné vytvořit komplexnější konfigurace s použitím diagramů jako vodítek. Každá konfigurace závisí na nastaveních služby VPN Gateway, která zvolíte.

### Konfigurace nastavení služby VPN Gateway

Protože služba VPN Gateway je kolekce prostředků, můžete nakonfigurovat některé z těchto prostředků pomocí jednoho nástroje a pak přepnout do jiného a v něm konfigurovat nastavení dalších prostředků. V současné době není možné konfigurovat všechna nastavení služby VPN Gateway pomocí webu Azure Portal. Pokyny v článcích pro každou konfiguraci informují o tom, který nástroj budete potřebovat. Pokud pracujete s modelem klasického nasazení, můžete chtít v tuto chvíli pracovat na klasickém portálu nebo v prostředí PowerShell. Další informace o jednotlivých dostupných nastaveních najdete v tématu [Informace o nastavení služby VPN Gateway](vpn-gateway-about-vpn-gateway-settings.md).



## Site-to-Site a Multi-Site

### Site-to-Site

Připojení Site-to-Site (S2S) využívá tunel VPN typu IPsec/IKE (IKEv1 nebo IKEv2). Tento typ připojení vyžaduje místní zařízení sítě VPN, které má přiřazenou veřejnou IP adresu a není umístěné za službou NAT. Připojení S2S můžete použít pro konfigurace mezi různými místy a pro hybridní konfigurace.   

![Připojení typu Site-to-Site](./media/vpn-gateway-about-vpngateways/demos2s.png "site-to-site")


### Multi-Site

Můžete vytvořit a konfigurovat připojení VPN mezi vaší virtuální sítí a několika místními sítěmi. Při práci s více připojeními je nutné použít typ sítě VPN založený na směrování (dynamické brána pro klasické virtuální sítě). Vzhledem k tomu, že virtuální síť může mít pouze jednu bránu virtuální sítě, všechna připojení prostřednictvím brány sdílejí dostupnou šířku pásma. Tento typ konfigurace se často nazývá připojení „s více lokalitami“ (multi-site).
 

![Připojení typu Multi-Site](./media/vpn-gateway-about-vpngateways/demomulti.png "multi-site")

### Modely a metody nasazení

[AZURE.INCLUDE [vpn-gateway-table-site-to-site](../../includes/vpn-gateway-table-site-to-site-include.md)] 

## VNet-to-VNet

Propojení virtuální sítě s jinou virtuální sítí (VNet-to-VNet) je podobné propojení virtuální sítě s místním serverem. Oba typy připojení využívají bránu VPN Azure VPN, která poskytuje zabezpečené tunelové propojení prostřednictvím protokolu IPsec/IKE. Dokonce můžete kombinovat komunikaci VNet-to-VNet s konfigurací s více servery. Díky tomu je možné vytvářet topologie sítí, ve kterých se používá propojování více míst i propojování virtuálních sítí.

Virtuální sítě, které propojujete, můžou být:

- v jedné nebo několika oblastech,
- v jednom nebo několika předplatných, 
- v jednom nebo několika modelech nasazení.



![Připojení typu VNet-to-VNet](./media/vpn-gateway-about-vpngateways/demov2v.png "vnet-to-vnet")



### Připojení mezi různými modely nasazení

Azure v současné době nabízí dva modely nasazení: Classic a Resource Manager. Pokud již Azure nějakou dobu používáte, pravděpodobně vaše virtuální počítače a role instancí Azure fungují ve virtuální síti Classic. Vaše novější virtuální počítače a role instancí však mohou používat virtuální síť vytvořenou v nástroji Resource Manager. Můžete vytvořit připojení mezi virtuálními sítěmi umožňující prostředkům v jedné virtuální síti přímo komunikovat s prostředky v jiné.


### Modely a metody nasazení

[AZURE.INCLUDE [vpn-gateway-table-vnet-to-vnet](../../includes/vpn-gateway-table-vnet-to-vnet-include.md)] 

### Partnerské vztahy virtuálních sítí

K vytvoření propojení můžete využít metodu VNet peering, pokud konfigurace vaší virtuální sítě splňuje určité požadavky. VNet peering nepoužívá bránu virtuální sítě. Metoda [VNet peering](../virtual-network/virtual-network-peering-overview.md) je v současnosti ve verzi Preview.



## Point-to-Site

Konfigurace Point-to-Site (P2S) umožňuje vytvořit zabezpečené připojení k vaší virtuální sítě z jednotlivých klientských počítačů. P2S je připojení VPN prostřednictvím protokolu SSTP (Secure Socket Tunneling Protocol). Připojení typu P2S k fungování nevyžadují zařízení VPN ani veřejnou IP adresu. Připojení VPN se navazuje ze strany klientského počítače. Toto řešení je užitečné, pokud se chcete připojit k virtuální síti ze vzdáleného umístění, například z domova nebo z konference, nebo pokud máte pouze několik klientů, kteří se potřebují k virtuální síti připojit. 


![Připojení typu Point-to-Site](./media/vpn-gateway-about-vpngateways/demop2s.png "point-to-site")

### Modely a metody nasazení

[AZURE.INCLUDE [vpn-gateway-table-point-to-site](../../includes/vpn-gateway-table-point-to-site-include.md)] 


## ExpressRoute

[AZURE.INCLUDE [expressroute-intro](../../includes/expressroute-intro-include.md)]

Další informace o ExpressRoute najdete v [Technickém přehledu ExpressRoute](../expressroute/expressroute-introduction.md).


## Současně existující připojení typu Site-to-Site a ExpressRoute

ExpressRoute je přímé vyhrazené připojení ke službám Microsoftu, včetně Azure, z vaší sítě WAN (nikoli prostřednictvím veřejného internetu). Provoz VPN typu Site-to-Site je přenášen zašifrovaně prostřednictvím veřejného internetu. Možnost konfigurace VPN typu Site-to-Site a připojení ExpressRoute pro stejnou virtuální síť má několik výhod.

Můžete nakonfigurovat síť VPN typu Site-to-Site jako zabezpečenou cestu převzetí služeb při selhání pro ExpressRoute, nebo použít VPN typu Site-to-Site pro připojení k webům, které nejsou součástí vaší sítě, ale jsou připojené prostřednictvím ExpressRoute. Tato konfigurace vyžaduje dvě brány virtuální sítě pro stejnou virtuální síť, jednu typu -GatewayType Vpn a druhou typu -GatewayType ExpressRoute.


![Současně existující připojení](./media/vpn-gateway-about-vpngateways/demoer.png "expressroute-site2site")


### Modely a metody nasazení

[AZURE.INCLUDE [vpn-gateway-table-coexist](../../includes/vpn-gateway-table-coexist-include.md)] 


## Další kroky

Další informace najdete v tématu [Nejčastější dotazy k branám VPN](vpn-gateway-vpn-faq.md).

Připojte vaše místní umístění k virtuální síti. Viz [Vytvoření připojení typu Site-to-Site](vpn-gateway-howto-site-to-site-resource-manager-portal.md).





 



<!--HONumber=ago16_HO5-->


