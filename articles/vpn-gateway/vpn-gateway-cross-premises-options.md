<properties 
   pageTitle="Informace o bezpečném připojení mezi různými místy pro virtuální sítě | Microsoft Azure"
   description="Seznamte se s typy bezpečného připojení mezi různými místy pro virtuální sítě, včetně připojení typu Site-to-Site, Point-to-Site a ExpressRoute."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="05/16/2016"
   ms.author="cherylmc" />

# Informace o bezpečném připojení mezi různými místy pro virtuální sítě

Tento článek popisuje různé způsoby připojení místního serveru k virtuální síti Azure. Tento článek se týká modelu nasazení Resource Manager i modelu nasazení Classic. Diagramy připojení ke službě VPN Gateway najdete v tématu [Topologie připojení ke službě Azure VPN Gateway](vpn-gateway-topology.md).

K dispozici jsou tři možnosti připojení: Site-to-Site, Point-to-Site a ExpressRoute. Možnost, kterou zvolíte, může záviset na několika uváženích, jako například:


- Jaký druh propustnosti vyžaduje vaše řešení?
- Chcete komunikovat přes veřejný internet prostřednictvím bezpečné VPN, nebo přes privátní připojení?
- Máte k dispozici veřejnou IP adresu?
- Plánujete použít zařízení VPN? Pokud ano, je kompatibilní?
- Připojujete pouze několik počítačů, nebo chcete trvalé připojení pro svůj server?
- Jaký typ brány VPN vyžaduje řešení, které chcete vytvořit?

Následující tabulka vám může pomoci se zvolením nejlepší možnosti připojení pro vaše řešení.

[AZURE.INCLUDE [vpn-gateway-cross-premises](../../includes/vpn-gateway-cross-premises-include.md)]                                                                    

## Připojení typu Site-to-Site

VPN typu Site-to-Site vám umožňuje vytvořit bezpečné připojení mezi místním serverem a virtuální sítí. Pro vytvoření připojení typu Site-to-Site je zařízení VPN v místní síti nakonfigurované k vytvoření bezpečného připojení se službou Azure VPN Gateway. Po vytvoření připojení mohou prostředky v místní síti a prostředky ve virtuální síti komunikovat přímo a bezpečně. Připojení typu Site-to-Site nevyžadují vytvoření samostatného připojení pro každý počítač v místní síti pro přístup k prostředkům ve virtuální síti.

**Připojení typu Site-to-Site použijte v případě, že:**

- Chcete vytvořit hybridní řešení.
- Chcete připojení mezi místním serverem a virtuální sítí bez vyžadování konfigurace na straně klienta.
- Chcete trvalé připojení. 

**Požadavky**

- Místní zařízení VPN musí mít internetovou IP adresu IPv4. IP adresa nesmí být umístěná za překladem adres (NAT).
- Vaše zařízení VPN musí být kompatibilní. Viz [Informace o zařízeních VPN](vpn-gateway-about-vpn-devices.md). 
- Zařízení VPN, které používáte, musí být kompatibilní s typem brány vyžadovaným pro vaše řešení. Viz [Informace o službě VPN Gateway](vpn-gateway-about-vpngateways.md).
- Skladová položka brány také ovlivní agregovanou propustnost. Další informace najdete v oddílu [Skladové položky brány](vpn-gateway-about-vpngateways.md#gwsku). 

**Dostupné modely a metody nasazení pro S2S**

[AZURE.INCLUDE [vpn-gateway-table-site-to-site](../../includes/vpn-gateway-table-site-to-site-include.md)] 


## Připojení typu Point-to-Site

VPN typu Point-to-Site vám umožňuje vytvořit bezpečné připojení k virtuální síti. V konfiguraci Point-to-Site je připojení konfigurované jednotlivě na každém klientském počítači, který chcete připojit k virtuální síti. Připojení typu Point-to-Site nevyžaduje zařízení VPN. Tento typ připojení používá klienta VPN, kterého nainstalujete na každý klientský počítač. Síť VPN je vytvořena ručním spuštěním připojení na místním klientském počítači.

Konfigurace Point-to-Site a Site-to-Site mohou existovat současně, ale na rozdíl od připojení typu Site-to-Site nelze připojení typu Point-to-Site nakonfigurovat současně s připojením ExpressRoute ve stejné virtuální síti.

**Připojení typu Point-to-Site použijte v případě, že:**

- Chcete pro připojení k virtuální síti nakonfigurovat pouze několik klientů.

- Chcete se připojit k virtuální síti ze vzdáleného umístění. Například z kavárny nebo místa konání konference.

- Máte připojení typu Site-to-Site, ale někteří klienti se potřebují připojit ze vzdáleného umístění.

- Nemáte přístup k zařízení VPN, které splňuje minimální požadavky na připojení typu Site-to-Site.

- Nemáte internetovou IP adresu IPv4 pro svoje zařízení VPN.

**Dostupné modely a metody nasazení pro P2S**

[AZURE.INCLUDE [vpn-gateway-table-point-to-site](../../includes/vpn-gateway-table-point-to-site-include.md)] 

## Připojení ExpressRoute

Azure ExpressRoute vám umožňuje vytvářet privátní připojení mezi datovými centry Azure a infrastrukturou, která se nachází ve vašem umístění nebo v prostředí ve společném umístění. Připojení ExpressRoute neprobíhají prostřednictvím veřejného internetu a poskytují větší spolehlivost, vyšší rychlost, nižší latenci a vyšší zabezpečení než typická připojení přes internet.

V některých případech může používání připojení ExpressRoute pro přenos dat mezi místní sítí a Azure přispět také k výraznému snížení nákladů. Prostřednictvím ExpressRoute můžete vytvářet připojení k Azure v umístění ExpressRoute (zařízení poskytovatele Exchange), nebo se připojovat přímo z existující sítě WAN (například ze sítě VPN MPLS) poskytované poskytovatelem síťové služby.

Další informace o ExpressRoute najdete v [Technickém přehledu](../expressroute/expressroute-introduction.md) ExpressRoute.


## Další kroky

- Další informace o službě VPN Gateway najdete v článcích [Informace o službě VPN Gateway](vpn-gateway-about-vpngateways.md), [VPN Gateway – nejčastější dotazy](vpn-gateway-vpn-faq.md) a [Plánování a návrh](vpn-gateway-plan-design.md).

- Další informace o ExpressRoute najdete v [Technickém přehledu](../expressroute/expressroute-introduction.md) ExpressRoute, [Nejčastějších dotazech](../expressroute/expressroute-faqs.md) a v [Pracovních postupech](../expressroute/expressroute-workflows.md).







<!--HONumber=Jun16_HO2-->


