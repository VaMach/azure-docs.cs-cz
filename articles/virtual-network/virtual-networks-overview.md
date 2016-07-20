<properties
   pageTitle="Přehled služby Azure Virtual Network"
   description="Seznamte se s virtuálními sítěmi v Azure."
   services="virtual-network"
   documentationCenter="na"
   authors="telmosampaio"
   manager="carmonm"
   editor="tysonn" />
<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="03/15/2016"
   ms.author="telmos" />

# Přehled služby Virtual Network

Virtuální síť Azure je reprezentace vaší vlastní sítě v cloudu.  Je to logická izolace cloudu Azure vyhrazeného pro vaše předplatné. V rámci této sítě máte plnou kontrolu nad bloky IP adres, nastavením DNS, zásadami zabezpečení a směrovacími tabulkami. Virtuální síť taky můžete dál segmentovat do podsítí a spouštět virtuální počítače IaaS a/nebo [cloudové služby (instance rolí PaaS)](../cloud-services/cloud-services-choose-me.md). Virtuální síť můžete navíc připojit k místní síti pomocí jedné z [možností připojení](../vpn-gateway/vpn-gateway-cross-premises-options.md) dostupných v Azure. V podstatě můžete svoji síť rozšířit do Azure s úplnou kontrolou nad bloky IP adres a s výhodou poskytovatelů Azure celopodnikového rozsahu.

Pokud chcete virtuálním sítím porozumět lépe, prohlédněte si následující schéma, které znázorňuje zjednodušenou místní síť.

![Místní síť](./media/virtual-networks-overview/figure01.png)

Výše uvedené schéma znázorňuje místní síť, která je prostřednictvím směrovače připojená k veřejnému internetu. Taky můžete vidět, že mezi směrovačem a zónou DMZ hostující server DNS a webovou serverovou farmu je brána firewall. Webová serverová farma využívá hardwarový nástroj pro vyrovnávání zatížení, který je přístupný z internetu a spotřebovává prostředky z interní podsítě. Interní podsíť je od DMZ oddělená další bránou firewall a hostuje servery kontroleru Domény služby Active Directory, databázové servery a aplikační servery.

Stejnou síť je možné hostovat v Azure, jak znázorňuje schéma níže.

![Virtuální síť Azure](./media/virtual-networks-overview/figure02.png)

Všimněte si, jak infrastruktura Azure přebírá roli směrovače a umožňuje přístup z vaší virtuální sítě do veřejného internetu, aniž by bylo potřeba cokoli konfigurovat. Brány firewall se můžou nahradit skupinami zabezpečení sítě (NSG), které se použijí u každé jednotlivé podsítě. A konečně, fyzické nástroje pro vyrovnávání zatížení jsou nahrazené internetovým a interním nástrojem pro vyrovnávání zatížení v Azure.

>[AZURE.NOTE] V Azure existují dva režimy nasazení: Classic (označovaný taky jako Service Management) a Azure Resource Manager (ARM). Virtuální sítě Classic se dají přidat do skupiny vztahů nebo vytvořit jako oblastní virtuální síť. Pokud máte virtuální síť ve skupině vztahů, doporučujeme ji [migrovat do oblastní virtuální sítě](virtual-networks-migrate-to-regional-vnet.md).

## Výhody virtuálních sítí

- **Izolace.** Virtuální sítě jsou vzájemně zcela izolované. To vám umožňuje vytvářet oddělené sítě pro vývoj, testování a produkci, které používají stejné bloky adres CIDR.

- **Přístup k veřejnému internetu.** Všechny virtuální počítače IaaS a instance rolí PaaS ve virtuální síti mají ve výchozím nastavení přístup k veřejnému internetu. Přístup můžete řídit pomocí skupin zabezpečení sítě (NSG).

- **Přístup k virtuálním počítačům v rámci virtuální sítě.** Instance rolí PaaS a virtuální počítače IaaS se dají spustit ve stejné virtuální síti a můžou se vzájemně připojovat pomocí privátních IP adres, i když jsou v různých podsítích, aniž by bylo nutné konfigurovat bránu nebo použít veřejné IP adresy.

- **Překlad adres IP.** Azure poskytuje interní překlad adres IP pro virtuální počítače IaaS a instance rolí PaaS nasazené ve vaší virtuální síti. Můžete taky nasadit vlastní servery DNS a nakonfigurovat virtuální síť tak, aby je používala.

- **Zabezpečení.** Provoz vstupující do a vystupující z virtuálních počítačů a instancí rolí PaaS ve virtuální síti se dá řídit pomocí skupin zabezpečení sítě.

- **Připojení.** Virtuální sítě můžou být navzájem připojené, a dokonce můžou být připojené k místnímu datacentru, a to pomocí připojení site-to-site VPN nebo ExpressRoute. Další informace o bránách sítě VPN najdete v tématu [Informace o bránách sítě VPN](../vpn-gateway/vpn-gateway-about-vpngateways.md). Další informace o službě ExpressRoute najdete v [technickém přehledu služby ExpressRoute](../expressroute/expressroute-introduction.md).

    >[AZURE.NOTE] Před nasazením jakýchkoli virtuálních počítačů IaaS nebo instancí rolí PaaS do prostředí Azure nezapomeňte vytvořit virtuální síť. Virtuální počítače založené na ARM vyžadují virtuální síť, a pokud nezadáte stávající virtuální síť, Azure vytvoří výchozí virtuální síť, jejíž blok adres CIDR může kolidovat s místní sítí. V důsledku toho nebudete moci připojit virtuální sítě k místní síti.

## Podsítě

Podsíť je rozsah IP adres ve virtuální síti. Virtuální síť můžete z organizačních a bezpečnostních důvodů rozdělit do několika podsítí. Virtuální počítače a instance rolí PaaS nasazené do podsítí (stejných nebo různých) v rámci jedné virtuální sítě můžou navzájem komunikovat bez jakékoli další konfigurace. Pro podsíť taky můžete konfigurovat směrovací tabulky a skupiny NSG.

## IP adresy


Existují dva typy IP adres, které se přiřazují prostředkům v Azure: *veřejné* a *privátní*. Veřejné IP adresy umožňují prostředkům Azure komunikovat s internetem a jinými veřejnými službami Azure, jako je například [Azure Redis Cache](https://azure.microsoft.com/services/cache/) nebo [Azure Event Hubs](https://azure.microsoft.com/documentation/services/event-hubs/). Privátní IP adresy umožňují komunikaci mezi prostředky ve virtuální síti a taky prostředky připojenými prostřednictvím sítě VPN, bez použití IP adres směrovatelných v internetu.

Další informace o IP adresách v Azure najdete v tématu [IP adresy ve virtuální síti](virtual-network-ip-addresses-overview-arm.md).

## Nástroje pro vyrovnávání zatížení Azure

Virtuální počítače a cloudové služby ve virtuální síti můžou být zpřístupněné internetu pomocí nástrojů pro vyrovnávání zatížení Azure. Obchodní aplikace, které směřují jenom do interní sítě, můžou používat vyrovnávání zatížení pomocí interního nástroje pro vyrovnávání zatížení.

- **Externí nástroj pro vyrovnávání zatížení.** Externí nástroj pro vyrovnávání zatížení můžete použít k zajištění vysoké dostupnosti virtuálních počítačů IaaS a instancí rolí PaaS, které jsou přístupné z veřejného internetu.

- **Interní nástroj pro vyrovnávání zatížení.** Interní nástroj pro vyrovnávání zatížení můžete použít k zajištění vysoké dostupnosti virtuálních počítačů IaaS a instancí rolí PaaS, které jsou přístupné z jiných služeb ve virtuální síti.

Další informace o vyrovnávání zatížení v Azure najdete v tématu [Přehled nástroje pro vyrovnávání zatížení](../load-balancer/load-balancer-overview.md).

## Skupiny zabezpečení sítě (NSG)

Můžete vytvářet skupiny NSG k řízení příchozího a odchozího přístupu k síťovým rozhraním (síťovým kartám), virtuálním počítačům a podsítím. Každá skupina NSG obsahuje jedno nebo víc pravidel, která určující, jestli je provoz schválený nebo odepřený, a to na základě zdrojové IP adresy, zdrojového portu, cílové IP adresy a cílového portu. Další informace o skupinách NSG najdete v tématu [Co je skupina zabezpečení sítě](virtual-networks-nsg.md).

## Virtuální zařízení

Virtuální zařízení je jenom další virtuální počítač ve vaší virtuální síti, na kterém je spuštěná softwarová funkce zařízení, jako je například brána firewall, optimalizace sítě WAN nebo zjišťování neoprávněných vniknutí. Pokud chcete využít funkce virtuálního zařízení, můžete v Azure vytvořit trasu a směrovat provoz virtuální sítě přes toto zařízení.

Seznamy NSG se například dají použít k zajištění zabezpečení ve virtuální síti. Seznamy NSG ale poskytují příchozím a odchozím paketům seznam řízení přístupu (ACL) vrstvy 4. Pokud chcete použít model zabezpečení vrstvy 7, budete muset použít zařízení brány firewall.

Virtuální zařízení závisí na [trasách definovaných uživatelem a předávání IP](virtual-networks-udr-overview.md).

## Omezení
Počet služeb Virtual Network povolených v předplatném podléhá omezením. Další informace najdete v tématu [Omezení sítí Azure](../azure-subscription-service-limits.md#networking-limits).

## Ceny
Za použití služeb Virtual Network se v Azure neúčtují žádné dodatečné poplatky. Výpočetní instance spuštěné v rámci virtuální sítě se účtují podle standardních sazeb, jak je popsáno v tématu [Ceny virtuálních počítačů Azure](https://azure.microsoft.com/pricing/details/virtual-machines/).  [Brány sítě VPN](https://azure.microsoft.com/pricing/details/vpn-gateway/) a [Veřejné IP adresy] (https://azure.microsoft.com/pricing/details/ip-addresses/) použité ve virtuální síti se taky budou účtovat podle standardních sazeb.

## Další kroky

- [Vytvoření virtuální sítě](virtual-networks-create-vnet-arm-pportal.md) a podsítí.
- [Vytvoření virtuálního počítače ve virtuální síti](../virtual-machines/virtual-machines-windows-hero-tutorial.md).
- Další informace o skupinách [NSG](virtual-networks-nsg.md).
- Další informace o [trasách definovaných uživatelem a předávání IP](virtual-networks-udr-overview.md).



<!--HONumber=Jun16_HO2-->


