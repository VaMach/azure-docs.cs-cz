---
title: "Virtuální síť Azure | Microsoft Docs"
description: "Další informace o Azure Virtual Network konceptů a funkcí."
services: virtual-network
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: 9633de4b-a867-4ddf-be3c-a332edf02e24
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/03/2017
ms.author: jdial
ms.openlocfilehash: dc6916bd25c5a020fdcef0707fe28a1e34fb0f88
ms.sourcegitcommit: 3df3fcec9ac9e56a3f5282f6c65e5a9bc1b5ba22
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/04/2017
---
# <a name="azure-virtual-network"></a>Azure Virtual Network

Služba Microsoft Azure Virtual Network umožňuje prostředků Azure k bezpečné komunikaci s jinými ve virtuální síti. Virtuální síť je reprezentace vlastní sítě v cloudu. Virtuální síť je to logická izolace cloudu Azure vyhrazeného pro vaše předplatné. Virtuální sítě můžete připojit k jiné virtuální sítě, nebo k místní síti. Následující obrázek znázorňuje některé funkce služby Azure Virtual Network:

![Síťový diagram](./media/virtual-networks-overview/virtual-network-overview.png)

Další informace o následující funkce Azure Virtual Network, klikněte na možnost:
- **[Izolace:](#isolation)**  virtuální sítě jsou izolované od sebe navzájem. Můžete vytvořit samostatné bloky adres virtuální sítě pro vývoj, testování a produkci, které používají stejné CIDR (např. 10.0.0.0/0). Naopak můžete vytvořit několik virtuálních sítí, které používají jiné bloky adres CIDR a společně připojení sítě. Virtuální síť můžete rozdělit do několika podsítí. Azure poskytuje interní překlad adres pro virtuální počítače a instance rolí Azure Cloud Services nasazený ve virtuální síti. Volitelně můžete nakonfigurovat virtuální sítě pro použití vlastní servery DNS, místo použití Azure interní překlad adres.
- **[Internetová komunikace:](#internet)**  cloudových služeb pro všechny virtuální počítače Azure a instancí rolí ve virtuální síti mají přístup k Internetu, ve výchozím nastavení. Podle potřeby můžete také povolit příchozí přístup ke konkrétním prostředkům.
- **[Komunikace prostředků Azure:](#within-vnet)**  prostředky Azure, jako je například cloudových služeb a virtuálních počítačů můžete nasadit ve stejné virtuální síti. Prostředky lze vzájemně komunikovat pomocí privátních IP adres, i když jsou v různých podsítích. Azure poskytuje výchozí směrování mezi podsítěmi, virtuálními sítěmi a místními sítěmi, takže není nutné konfigurovat a spravovat trasy. V případě potřeby můžete přizpůsobit Azure, je ale směrování.
- **[Připojení k virtuální síti:](#connect-vnets)**  virtuální sítě může být připojena k sobě navzájem, povolení prostředky v žádné virtuální síti komunikovat s prostředky v žádné jiné virtuální síti.
- **[Místní připojení:](#connect-on-premises)**  virtuální sítě může být soukromě připojené k místní síti nebo pomocí připojení site-to-site VPN přes Internet.
- **[Filtrování přenosu:](#filtering)**  virtuální počítače a cloudové služby role instance síťového provozu je možné filtrovat příchozí a odchozí zdrojové IP adresy a portu, cílové IP adresy a portu a protokolu.
- **[Směrování:](#routing)**  můžete volitelně přepsat výchozí Azure směrování pouze vlastní trasy, nebo pomocí protokolu BGP šíření tras prostřednictvím brány sítě.

## <a name = "isolation"></a>Izolace sítě a segmentace

Můžete implementovat několik virtuálních sítí v rámci každé Azure [předplatné](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#subscription) a Azure [oblast](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#region). Všechny virtuální sítě jsou mezi sebou izolované. Pro každou virtuální síť, můžete:
- Zadejte vlastní prostor privátní IP adresy pomocí veřejné a privátní adresy (RFC 1918). Azure přiřadí prostředky ve virtuální síti privátní IP adresy z adresního prostoru, který přiřadíte.
- Segment virtuální sítě do jedné nebo více podsítí a přidělovat část adresního prostoru virtuální sítě pro každou podsíť.
- Můžete použít Azure překlad nebo zadejte vlastní server DNS pro použití prostředky ve virtuální síti. Další informace o překladu názvů ve virtuálních sítích najdete v tématu [překlad názvů pro virtuální počítače a cloudové služby](virtual-networks-name-resolution-for-vms-and-role-instances.md) článku.

## <a name = "internet"></a>Internetová komunikace
Ve výchozím nastavení může komunikovat odchozí k Internetu, všechny prostředky ve virtuální síti. Privátní IP adresu prostředku je zdrojová síťová adresa přeložit (překládat pomocí SNAT) na veřejnou IP adresu vybraná infrastrukturu Azure. Další informace o odchozí připojení k Internetu, přečtěte si [pochopení odchozí připojení v Azure](..\load-balancer\load-balancer-outbound-connections.md?toc=%2fazure%2fvirtual-network%2ftoc.json#standalone-vm-with-no-instance-level-public-ip-address) článku. Pokud chcete zabránit odchozí připojení k Internetu, můžete implementovat vlastní trasy nebo filtrování provozu.

Pro komunikaci příchozí prostředky Azure z Internetu, nebo k Internetu bez překládat pomocí SNAT odchozí komunikaci, musí prostředek přiřazenou veřejnou adresu IP. Další informace o veřejné IP adresy, přečtěte si [veřejné IP adresy](virtual-network-public-ip-address.md) článku.

## <a name="within-vnet"></a>Zabezpečení komunikace mezi prostředky Azure

Můžete nasadit virtuální počítače v rámci virtuální sítě. Virtuální počítače komunikovat s další prostředky ve virtuální síti s využitím rozhraní sítě. Další informace o síťových rozhraní, najdete v části [síťových rozhraní](virtual-network-network-interface.md).

Můžete taky nasadit několik typů prostředků Azure k virtuální síti, jako jsou virtuální počítače Azure, Azure Cloud Services, prostředí Azure App Service a sady škálování virtuálního počítače Azure. Úplný seznam prostředků Azure, můžete nasadit do virtuální sítě najdete v tématu [integrace služby virtuální sítě pro služby Azure](virtual-network-for-azure-services.md).

Některé prostředky nelze nasadit do virtuální sítě, ale vám umožní omezit komunikaci z prostředků v rámci virtuální sítě. Další informace o tom, jak omezit přístup k prostředkům, najdete v části [koncové body služby virtuální sítě](virtual-network-service-endpoints-overview.md). 

## <a name="connect-vnets"></a>Připojit virtuální sítě

Virtuální sítě můžete připojit k sobě navzájem, povolení prostředky v buď virtuální sítě pro komunikaci mezi sebou pomocí virtuální sítě partnerský vztah. Šířka pásma a latence komunikace mezi prostředky v různých virtuálních sítích je stejný, jako kdyby byly prostředky ve stejné virtuální síti. Další informace o partnerském vztahu, najdete [partnerský vztah virtuální sítě](virtual-network-peering-overview.md) článku.

## <a name="connect-on-premises"></a>Připojit k místní síti

Místní sítě můžete připojit k virtuální síti pomocí libovolnou kombinaci těchto možností:
- **Point-to-site virtuální privátní sítě (VPN):** ve vaší síti vytvořené mezi virtuální sítí a na jednom počítači. Každý počítač, který chce navázat připojení k virtuální síti musíte nakonfigurovat nezávisle jejich připojení. Tento typ připojení je skvělé, pokud jste se právě Začínáme s Azure, nebo pro vývojáře, protože nevyžaduje skoro žádné nebo vůbec žádné změny k vaší existující síti. Připojení k poskytování šifrovanou komunikaci prostřednictvím Internetu mezi Počítačem a virtuální sítě používá protokol SSTP. Latence pro síť VPN point-to-site nepředvídatelným, protože provoz prochází přes Internet.
- **Site-to-site VPN:** mezi zařízení VPN a bránu VPN Azure, který je nasazený ve virtuální síti. Tento typ připojení umožňuje jakémukoli prostředku místní autorizaci pro přístup k virtuální síti. Připojení je VPN pomocí protokolu IPSec/IKE, která poskytuje šifrovanou komunikaci prostřednictvím Internetu mezi místní zařízení a brána Azure VPN. Latence pro připojení site-to-site nepředvídatelným, protože provoz prochází přes Internet.
- **Azure ExpressRoute:** navázat mezi vaší sítí a Azure prostřednictvím partnerem ExpressRoute. Toto připojení je soukromé. Provoz neprochází Internetu. Latence pro připojení typu ExpressRoute je předvídatelný, vzhledem k tomu, že provoz není procházení Internetu.

Další informace o všech předchozích možností připojení, přečtěte si [diagramy topologie připojení](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json#diagrams) článku.

## <a name="filtering"></a>Filtrování provozu sítě přenosů
Můžete filtrovat síťový provoz mezi podsítěmi pomocí jedné nebo obou z následujících možností:
- **Skupin zabezpečení sítě:** skupinu zabezpečení sítě může obsahovat více pravidel příchozí a odchozí zabezpečení, které umožňují filtrovat provoz ve zdrojové a cílové IP adresy, portu a protokolu. Skupina zabezpečení sítě můžete použít pro každé síťové rozhraní ve virtuálním počítači. Můžete taky použít skupinu zabezpečení sítě k podsíti síťové rozhraní je, nebo jiných prostředků Azure. Další informace o skupinách zabezpečení sítě najdete v tématu [skupin zabezpečení sítě](security-overview.md#network-security-groups).
- **Síťových virtuálních zařízení:** zařízení virtuální sítě je virtuální počítač spuštěný software, který provádí síťové funkce, jako je například Brána firewall. Zobrazit seznam dostupných síťových virtuálních zařízení v [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/category/networking?page=1&subcategories=appliances). Virtuální zařízení sítě jsou také k dispozici, poskytují optimalizace sítě WAN a jiných síťových přenosů funkce. Virtuální zařízení sítě jsou obvykle používány s uživatelem definované nebo trasy protokolu BGP. Virtuální zařízení sítě můžete použít také k filtrování provozu mezi virtuálními sítěmi.

## <a name="routing"></a>Směrovat síťový provoz

Azure vytvoří směrovací tabulky, které umožňují prostředky připojenými k žádné podsíti v žádné virtuální sítě pro komunikaci mezi sebou, ve výchozím nastavení. Můžete implementovat jednu nebo obě z následujících možností přepsat výchozí trasy, které vytvoří Azure:
- **Trasy definované uživatelem:** můžete vytvořit vlastní směrovací tabulky s trasami, které tuto kontrolu, kde provoz se směruje na pro každou podsíť. Další informace o trasy definované uživatelem, najdete v části [trasy definované uživatelem](virtual-networks-udr-overview.md#user-defined).
- **Trasy protokolu BGP:** Pokud virtuální sítě se připojit k místní síti pomocí připojení k Azure VPN Gateway nebo ExpressRoute, můžete rozšířit směrování BGP do virtuálních sítí.

## <a name="pricing"></a>Ceny

Skupiny zabezpečení není nijak zpoplatněn pro virtuální sítě, podsítě, směrovací tabulky nebo sítě. Odchozí šířky pásma Internetu, veřejné IP adresy, partnerský vztah virtuální sítě, brány sítě VPN a ExpressRoute každý mají svůj vlastní ceny struktury. Zobrazení [virtuální síť](https://azure.microsoft.com/pricing/details/virtual-network), [brány VPN](https://azure.microsoft.com/pricing/details/vpn-gateway), a [ExpressRoute](https://azure.microsoft.com/pricing/details/expressroute) ceny stránky pro další informace.

## <a name="faq"></a>Nejčastější dotazy

Nejčastější dotazy k virtuální síti Azure najdete v tématu [virtuální sítě – nejčastější dotazy](virtual-networks-faq.md) článku.


## <a name="next-steps"></a>Další kroky

- Vytvoření vaší první virtuální síť a nasazení několik virtuálních počítačů do, pomocí kroků v [vytvoření vaší první virtuální síť](virtual-network-get-started-vnet-subnet.md).
- Vytvořit připojení point-to-site k virtuální síti pomocí kroků v [konfigurace připojení typu point-to-site](../vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json).
- Přečtěte si o některých dalších klíče [sítě možnosti](../networking/networking-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json) Azure.
