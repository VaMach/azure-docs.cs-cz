---
title: "Koncové body služby virtuální sítě Azure | Dokumentace Microsoftu"
description: "Zjistěte, jak povolit přímý přístup k prostředkům Azure z virtuální sítě pomocí koncových bodů služby."
services: virtual-network
documentationcenter: na
author: anithaa
manager: narayan
editor: 
ms.assetid: 
ms.service: virtual-network
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/15/2017
ms.author: anithaa
ms.custom: 
ms.translationtype: HT
ms.sourcegitcommit: c3a2462b4ce4e1410a670624bcbcec26fd51b811
ms.openlocfilehash: 297c51cca31d981ffadd20cccce1cd3dd69dc81c
ms.contentlocale: cs-cz
ms.lasthandoff: 09/25/2017

---

# <a name="virtual-network-service-endpoints-preview"></a>Koncové body služby virtuální sítě (Preview)

Koncové body služby virtuální sítě rozšiřují privátní adresní prostor vaší virtuální sítě a její identitu do služeb Azure přes přímé připojení. Koncové body umožňují svázat vaše důležité prostředky služeb Azure pouze s vašimi virtuálními sítěmi. Provoz z vaší virtuální sítě do služby Azure vždy zůstává v páteřní síti Microsoft Azure.

Tato funkce je dostupná ve verzi __Preview__ pro následující služby a oblasti Azure:

__Azure Storage:__ WestCentralUS, WestUS2, EastUS, WestUS, AustraliaEast, AustraliaSouthEast.

__Azure SQL Database:__ WestCentralUS, WestUS2, EastUS.

Nejaktuálnější oznámení pro verzi Preview najdete na stránce [Aktualizace služby Azure Virtual Network](https://azure.microsoft.com/updates/?product=virtual-network).

>[!NOTE]
Během období Preview tato funkce nemusí dosahovat stejné úrovně dostupnosti a spolehlivosti jako funkce, které jsou ve verzi všeobecné dostupnosti. Další informace najdete v [dodatečných podmínkách použití systémů Microsoft Azure Preview](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="key-benefits"></a>Klíčové výhody

Koncové body služby poskytují následující výhody:

- __Zlepšení zabezpečení prostředků služeb Azure__

  Pomocí koncových bodů služby je možné svázat prostředky služeb Azure s vaší virtuální sítí. Získáte tak lepší zabezpečení díky úplnému odebrání veřejného internetového přístupu k těmto prostředkům a povolení provozu pouze z vaší virtuální sítě.

- __Optimální směrování provozu služeb Azure z vaší virtuální sítě__

  V současné době všechny trasy, které vynucují internetový provoz přes místní nebo virtuální zařízení, což se označuje jako vynucené tunelování, vynucují také směrování provozu služeb Azure stejnou trasou jako internetový provoz. Koncové body služby poskytují optimální směrování provozu Azure. 

  Koncové body vždy směrují provoz služby přímo z vaší virtuální sítě do služby v páteřní síti Microsoft Azure. Díky tomu můžete pokračovat v auditování a monitorování odchozího internetového provozu z vašich virtuálních sítí prostřednictvím vynuceného tunelování, aniž by to mělo vliv na provoz služby. Další informace o [trasách definovaných uživatelem a vynuceném tunelování](https://docs.microsoft.com/azure/virtual-network/virtual-networks-udr-overview)

- __Snadné nastavení a méně režie na správu__

  Ve virtuálních sítích už nepotřebujete vyhrazené veřejné IP adresy pro zabezpečení prostředků Azure prostřednictvím brány firewall protokolu IP. K nastavení koncových bodů služby se nevyžaduje překlad adres ani zařízení brány. Koncový bod služby je možné konfigurovat pouhým kliknutím na podsíť. S údržbou koncových bodů není spojena žádná další režie.

## <a name="limitations"></a>Omezení

- Tato funkce je dostupná pouze pro virtuální sítě nasazené pomocí modelu nasazení Azure Resource Manager.
- Koncové body jsou povolené na podsítích nakonfigurovaných ve virtuálních sítích Azure. Koncové body není možné použít pro provoz z místního prostředí do služeb Azure. Další podrobnosti najdete v části [Zabezpečení přístupu ke službám Azure z místního prostředí](#securing-azure-services-to-virtual-networks).
- Koncový bod služby se vztahuje pouze na provoz služeb Azure v rámci oblasti virtuální sítě. V případě služby Azure Storage je pro zajištění podpory provozu služby RA-GRS a úložiště GRS zahrnuta také spárovaná oblast, ve které je virtuální síť nasazená. Další informace o [spárovaných oblastech Azure](https://docs.microsoft.com/azure/best-practices-availability-paired-regions#what-are-paired-regions)

## <a name="securing-azure-services-to-virtual-networks"></a>Svázání služeb Azure s virtuálními sítěmi

- Koncový bod služby virtuální sítě poskytuje službě Azure identitu vaší virtuální sítě. Jakmile budou ve vaší virtuální síti povolené koncové body služby, můžete svázat prostředky služeb Azure s virtuální sítí tím, že do prostředků přidáte pravidlo virtuální sítě.
- V současné době provoz služeb Azure z vaší virtuální sítě jako zdrojové IP adresy používá veřejné IP adresy. S koncovými body služby přepne provoz služby při přístupu ke službě Azure z vaší virtuální sítě na používání privátních adres virtuální sítě jako zdrojových IP adres. Toto přepnutí umožňuje přistupovat ke službám bez potřeby vyhrazených veřejných IP adres, které se používají v branách firewall protokolu IP.
- __Zabezpečení přístupu ke službám Azure z místního prostředí__:
 
   Prostředky služeb Azure svázané s virtuálními sítěmi ve výchozím nastavení nejsou přístupné z místních sítí. Pokud chcete povolit provoz z místního prostředí, musíte také povolit IP adresy pro překlad adres (NAT) z místních okruhů nebo okruhů ExpressRoute. IP adresy pro překlad adres (NAT) je možné přidat prostřednictvím konfigurace brány firewall protokolu IP pro prostředky služeb Azure.
  
   __ExpressRoute__: 

    Pokud používáte [ExpressRoute](/azure/expressroute/expressroute-introduction) z místního prostředí, každý okruh ExpressRoute využívá dvě IP adresy pro překlad adres (NAT), které se použijí na provoz služeb Azure při jeho přijetí v páteřní síti Microsoft Azure.  Pokud chcete povolit přístup k vašim prostředkům služeb, musíte také povolit tyto dvě IP adresy v nastavení brány firewall protokolu IP pro prostředek.  Pokud chcete zjistit IP adresy okruhů ExpressRoute, [otevřete lístek podpory pro ExpressRoute](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview) prostřednictvím webu Azure Portal.

![Svázání služeb Azure s virtuálními sítěmi](media/virtual-network-service-endpoints-overview/VNet_Service_Endpoints_Overview.png)

### <a name="configuration"></a>Konfigurace

- Koncové body služby se konfigurují v podsíti ve virtuální síti. Koncové body fungují s jakýmkoli typem výpočetních instancí spuštěných v rámci této podsítě.
- Z jedné podsítě je možné povolit pro konkrétní službu pouze jeden koncový bod služby. V podsíti můžete konfigurovat několik koncových bodů služby pro všechny podporované služby Azure (například Storage nebo SQL).
- Virtuální sítě musí být ve stejné oblasti jako prostředek služby Azure. Pokud používáte službu Azure Storage s účty GRS a RA-GRS, primární účet musí být ve stejné oblasti jako virtuální síť.
- Virtuální síť, ve které je koncový bod nakonfigurovaný, může být ve stejném předplatném jako prostředek služby Azure nebo v jiném předplatném. Další informace o oprávněních požadovaných pro nastavení koncových bodů a zabezpečení služeb Azure najdete níže v části [Zřizování](#Provisioning).
- U podporovaných služeb můžete pomocí koncových bodů služby svázat s virtuálními sítěmi nové nebo existující prostředky.

### <a name="considerations"></a>Požadavky

- Po povolení koncového bodu služby zdrojové IP adresy virtuálních počítačů v podsíti při komunikaci se službou z dané podsítě přepnou od používání veřejných IPv4 adres na používání vlastních privátních IPv4 adres.
  - Během tohoto přepnutí budou ukončena všechna existující otevřená připojení TCP ke službě. Při povolování nebo zakazování koncového bodu služby pro podsíť se ujistěte, že nejsou spuštěné žádné důležité úlohy. Také se ujistěte, že se vaše aplikace můžou po tomto přepnutí IP adres automaticky připojit ke službám Azure.
  - Přepnutí IP adres ovlivní pouze provoz služeb z vaší virtuální sítě. Veškerý ostatní provoz adresovaný na veřejné IPv4 adresy přiřazené vašim virtuálním počítačům nebo z nich nebude ovlivněn.
  - Pokud máte pro služby Azure existující pravidla brány firewall používající veřejné IP adresy Azure, tato pravidla s přepnutím na privátní adresy virtuální sítě přestanou fungovat.
- S koncovými body služby zůstávají záznamy DNS pro služby Azure tak, jak jsou, a nadále se překládají na IP adresy přiřazené příslušné službě Azure.
- Skupiny zabezpečení sítě (NSG) s koncovými body služby:
  - Skupiny zabezpečení sítě ve výchozím nastavení povolují odchozí internetový provoz a tedy povolují i provoz z virtuální sítě do služeb Azure. S koncovými body služby to nadále funguje stejným způsobem.
  - Pokud chcete odepřít veškerý odchozí internetový provoz a povolit pouze provoz do konkrétních služeb Azure, můžete to provést použitím __značek služeb Azure__ ve vašich skupinách zabezpečení sítě. V pravidlech NSG můžete zadat podporované služby Azure jako cíl a Azure zajistí údržbu IP adres, na které jednotlivé značky odkazují. Další informace najdete v tématu [Značky služeb Azure pro skupiny zabezpečení sítě](https://aka.ms/servicetags).

### <a name="scenarios"></a>Scénáře
- Partnerské, propojené nebo vícenásobné virtuální sítě:

Pokud chcete svázat služby Azure s několika podsítěmi v rámci virtuální sítě nebo mezi několika virtuálními sítěmi, můžete koncové body služby povolit v každé z těchto podsítí nezávisle na sobě a svázat tak prostředky služeb Azure se všemi těmito podsítěmi.

- Filtrování odchozího provozu z virtuální sítě do služeb Azure:

Pokud chcete prozkoumat nebo filtrovat provoz směřující z virtuální sítě do služby Azure, můžete v rámci příslušné virtuální sítě nasadit síťové virtuální zařízení (NVA). Potom můžete na podsíť s nasazeným síťovým virtuálním zařízením použít koncové body služby a svázat prostředky služby Azure pouze s touto podsítí. Tento scénář může být užitečný v případě, že chcete omezit přístup služby Azure z vaší virtuální sítě pouze ke konkrétním prostředkům Azure pomocí filtrování síťového virtuálního zařízení. Další informace najdete v článku věnovaném [výchozímu přenosu s použitím síťových virtuálních zařízení](https://docs.microsoft.com/en-us/azure/architecture/reference-architectures/dmz/nva-ha#egress-with-layer-7-nvas).

- Svázání prostředků Azure se službami nasazenými přímo do virtuálních sítí:

Různé služby Azure je možné nasadit přímo do konkrétních podsítí ve virtuálních sítích. Prostředky služby Azure můžete svázat s podsítěmi [spravované služby](virtual-network-for-azure-services.md) nastavením koncového bodu služby v podsíti spravované služby.

### <a name="logging-and-troubleshooting"></a>Protokolování a řešení potíží

Jakmile jsou koncové body služby pro konkrétní službu nakonfigurované, ověřte následujícím způsobem fungování trasy koncového bodu služby: 

- __Monitorování diagnostiky služby Azure:__ Potvrdit, že žádost o přístup přichází z vaší privátní sítě, tj. virtuální sítě, můžete ověřením zdrojové IP adresy jakékoli žádosti o službu v diagnostice služby. U všech nových žádostí pomocí koncových bodů služby se jako zdrojová IP adresa žádosti zobrazí privátní adresa virtuální sítě, která je přiřazená klientovi provádějícímu žádost z vaší virtuální sítě. Bez koncového bodu tato adresa bude veřejná IP adresa Azure.

- __Efektivní trasy__ na jakémkoli síťovém rozhraní v podsíti ukazují konkrétnější výchozí trasu k rozsahu předpon adresy dané služby. Trasa má jako typ dalšího segmentu směrování VirtualNetworkServiceEndpoint. Tato trasa označuje, že se v porovnání s trasami vynuceného tunelování používá přímější připojení ke službě.

>[!NOTE]
Trasa koncového bodu služby přepíše trasy BGP nebo UDR pro shodu předpon adresy se službou Azure. Další informace o [řešení potíží pomocí efektivních tras](https://docs.microsoft.com/azure/virtual-network/virtual-network-routes-troubleshoot-portal#using-effective-routes-to-troubleshoot-vm-traffic-flow)

## <a name="provisioning"></a>Zřizování

Koncové body služby může ve virtuálních sítích nezávisle na sobě konfigurovat uživatel s oprávněním k zápisu do virtuální sítě.

Pokud chce uživatel svázat prostředky služeb Azure s virtuální sítí, musí mít pro přidávané podsítě oprávnění k Microsoft.Network/JoinServicetoaSubnet. Toto oprávnění je ve výchozím nastavení součástí předdefinovaných rolí správců služeb a může se upravit vytvořením vlastních rolí.

Další informace o [předdefinovaných rolích](https://docs.microsoft.com/azure/active-directory/role-based-access-built-in-roles) a přiřazení konkrétních oprávnění k [vlastním rolím](https://docs.microsoft.com/en-us/azure/active-directory/role-based-access-control-custom-roles).

Virtuální sítě a prostředky služeb Azure můžou být ve stejném předplatném nebo v různých předplatných. Pokud jsou virtuální síť a prostředky služeb Azure v různých předplatných, během tohoto období Preview musí být prostředky ve stejném tenantovi Active Directory (AD). 

## <a name="pricing-and-limits"></a>Ceny a omezení

Používání koncových bodů služby je bez dalších poplatků. Používá se cenový model pro služby Azure (Azure Storage, Azure SQL Database) v současné podobě.

Celkový počet koncových bodů služby ve virtuální síti není nijak omezený.

Pro prostředky služeb Azure (například účet služby Storage) můžou služby vynucovat omezení počtu podsítí použitých k zabezpečení příslušného prostředku. Podrobnosti najdete v dokumentaci k různým službám v části [Další kroky](#next%20steps).

## <a name="next-steps"></a>Další kroky

- Informace o [postupu konfigurace koncových bodů služby virtuální sítě](virtual-network-service-endpoints-configure.md)
- Další informace o [svázání účtů služby Azure Storage s virtuálními sítěmi](https://docs.microsoft.com/azure/storage/common/storage-network-security)
- Další informace o [svázání služby Azure SQL Database s virtuálními sítěmi](https://docs.microsoft.com/azure/sql-database/sql-database-vnet-service-endpoint-rule-overview)
- [Přehled integrace služeb Azure pro virtuální sítě](virtual-network-for-azure-services.md)


