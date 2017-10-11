---
title: "Virtuální síť Azure | Microsoft Docs"
description: "Další informace o Azure Virtual Network konceptů a funkcí."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 9633de4b-a867-4ddf-be3c-a332edf02e24
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/23/2017
ms.author: jdial
ms.openlocfilehash: 6d6afd2b9b956138ed400fbd6cabd3b480fde0f0
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="azure-virtual-network"></a>Azure Virtual Network

Služba Azure Virtual Network umožňuje bezpečně se vzájemně připojovat prostředky Azure s virtuálními sítěmi (virtuální sítě). Virtuální síť je reprezentace vlastní sítě v cloudu. Virtuální síť je to logická izolace cloudu Azure vyhrazeného pro vaše předplatné. Virtuální sítě můžete také připojit k místní síti. Následující obrázek znázorňuje některé funkce služby Azure Virtual Network:

![Síťový diagram](./media/virtual-networks-overview/virtual-network-overview.png)

Další informace o následující funkce Azure Virtual Network, klikněte na možnost:
- **[Izolace:](#isolation)**  virtuální sítě jsou izolované od sebe navzájem. Můžete vytvořit samostatné virtuálních sítí patřících pro vývoj, testování a produkci použít stejné bloky adres CIDR. Naopak můžete vytvořit více virtuálních sítí, použít jiné bloky adres CIDR a společně připojení sítě. Virtuální síť můžete rozdělit do několika podsítí. Azure poskytuje interní překlad adres pro virtuální počítače a cloudové služby instance rolí, které jsou připojené k virtuální síti. Volitelně můžete nakonfigurovat virtuální síť použít vlastní servery DNS, místo použití Azure interní překlad adres.
- **[Připojení k Internetu:](#internet)**  cloudových služeb pro všechny virtuální počítače Azure (VM) a instancí rolí, které jsou připojené k virtuální síti mají přístup k Internetu, ve výchozím nastavení. Podle potřeby můžete také povolit příchozí přístup ke konkrétním prostředkům.
- **[Připojení prostředků Azure:](#within-vnet)**  prostředky Azure, jako je například cloudových služeb a virtuálních počítačů může být připojen k stejnou virtuální síť. Prostředky můžete připojit k sobě navzájem pomocí privátních IP adres, i když jsou v různých podsítích. Azure poskytuje výchozí směrování mezi podsítěmi, virtuálními sítěmi a místními sítěmi, takže není nutné konfigurovat a spravovat trasy.
- **[Připojení virtuální sítě:](#connect-vnets)**  virtuální sítě může být připojena k sobě navzájem, povolení prostředky připojenými k žádné virtuální sítě pro komunikaci s jakýmikoli prostředky na ostatní virtuální sítě.
- **[Místní připojení:](#connect-on-premises)**  virtuální sítě může být připojen k místní sítě prostřednictvím privátní sítě připojení mezi vaší sítí a Azure, nebo připojení site-to-site VPN přes Internet.
- **[Filtrování přenosu:](#filtering)**  virtuálního počítače a cloudové služby role instance síťového provozu je možné filtrovat příchozí a odchozí zdrojové IP adresy a portu, cílové IP adresy a portu a protokolu.
- **[Směrování:](#routing)**  Volitelně můžete přepsat výchozí Azure směrování konfiguraci vlastních tras, nebo pomocí trasy protokolu BGP prostřednictvím brány sítě.

## <a name = "isolation"></a>Izolace sítě a segmentace

Můžete implementovat více virtuálních sítí v rámci každé Azure [předplatné](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#subscription) a Azure [oblast](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#region). Každý virtuální sítě je izolovaná od jiných virtuálních sítí. Pro každý virtuální síť můžete:
- Zadejte vlastní prostor privátní IP adresy pomocí veřejné a privátní adresy (RFC 1918). Prostředky Azure přiřadí připojen k virtuální síti privátní IP adresy z adresního prostoru, který přiřadíte.
- Segment sítě VNet do jedné nebo více podsítí a přidělovat část adresní prostor virtuální sítě pro každou podsíť.
- Můžete použít Azure překlad nebo zadejte vlastní server DNS pro použití prostředky, které jsou připojené k virtuální síti. Další informace o překladu názvů v virtuální sítě, najdete [překlad názvů pro virtuální počítače a cloudové služby](virtual-networks-name-resolution-for-vms-and-role-instances.md) článku.

## <a name = "internet"></a>Připojení k Internetu
Všechny prostředky, které jsou připojené k virtuální síti mají ve výchozím nastavení odchozí připojení k Internetu. Privátní IP adresu prostředku je zdrojová síťová adresa přeložit (překládat pomocí SNAT) na veřejnou IP adresu pomocí infrastruktury Azure. Další informace o odchozí připojení k Internetu, přečtěte si [pochopení odchozí připojení v Azure](..\load-balancer\load-balancer-outbound-connections.md?toc=%2fazure%2fvirtual-network%2ftoc.json#standalone-vm-with-no-instance-level-public-ip-address) článku. Implementací vlastní směrování a filtrování provozu, můžete změnit výchozí připojení.

Pro komunikaci příchozí prostředky Azure z Internetu, nebo k Internetu bez překládat pomocí SNAT odchozí komunikaci, musí prostředek přiřazenou veřejnou adresu IP. Další informace o veřejné IP adresy, přečtěte si [veřejné IP adresy](virtual-network-public-ip-address.md) článku.

## <a name="within-vnet"></a>Připojení prostředků Azure
Několik prostředků Azure můžete připojit k virtuální síti, jako jsou virtuální počítače (VM), cloudové služby, prostředí App Service a sady škálování virtuálního počítače. Virtuální počítače připojit k podsítí v rámci virtuální sítě přes síťové rozhraní (NIC). Další informace o síťové karty, najdete [síťových rozhraní](virtual-network-network-interface.md) článku.

## <a name="connect-vnets"></a>Připojit virtuální sítě

Virtuální sítě můžete připojit k sobě navzájem, povolení prostředky připojenými k buď virtuální sítě spolu vzájemně komunikovat virtuální sítě. Propojení virtuálních sítí k sobě navzájem, můžete použít jednu nebo obě z následujících možností:
- **Partnerský vztah:** umožňuje prostředky připojenými k jiné sítě Azure Vnet v rámci stejné oblasti Azure pro komunikaci mezi sebou. Šířka pásma a čekací doba mezi virtuální sítě je stejný, jako kdyby prostředky byly připojené ke stejné síti VNet. Další informace o partnerském vztahu, najdete [partnerský vztah virtuální sítě](virtual-network-peering-overview.md) článku.
- **Připojení VNet-to-VNet:** umožňuje prostředky připojenými k jiné službě Azure VNet v rámci umístění Azure stejný nebo jiný. Na rozdíl od partnerského vztahu, šířka pásma je omezená mezi virtuálními sítěmi, protože provoz musí procházet skrz bránu VPN Azure. Další informace o připojení virtuální sítě pomocí připojení VNet-to-VNet, najdete [konfigurace připojení typu VNet-to-VNet](../vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) článku.

## <a name="connect-on-premises"></a>Připojit k místní síti

Do místní sítě lze připojit k virtuální síti pomocí libovolnou kombinaci těchto možností:
- **Point-to-site virtuální privátní sítě (VPN):** navázat mezi jeden počítač připojený k síti a virtuální sítě. Tento typ připojení je skvělé, pokud jste se právě Začínáme s Azure, nebo pro vývojáře, protože nevyžaduje skoro žádné nebo vůbec žádné změny k vaší existující síti. Připojení k poskytování šifrovanou komunikaci prostřednictvím Internetu mezi Počítačem a virtuální sítě používá protokol SSTP. Latence pro síť VPN point-to-site nepředvídatelným, protože provoz prochází přes Internet.
- **Site-to-site VPN:** mezi zařízení VPN a služby Azure VPN Gateway. Tento typ připojení umožňuje jakémukoli prostředku místní autorizaci pro přístup k virtuální síti. Připojení je VPN pomocí protokolu IPSec/IKE, která poskytuje šifrovanou komunikaci prostřednictvím Internetu mezi místní zařízení a brána Azure VPN. Latence pro připojení site-to-site nepředvídatelným, protože provoz prochází přes Internet.
- **Azure ExpressRoute:** navázat mezi vaší sítí a Azure prostřednictvím partnerem ExpressRoute. Toto připojení je soukromé. Provoz neprochází Internetu. Latence pro připojení typu ExpressRoute je předvídatelný, vzhledem k tomu, že provoz není procházení Internetu.

Další informace o všech předchozích možností připojení, přečtěte si [diagramy topologie připojení](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json#diagrams) článku.

## <a name="filtering"></a>Filtrování provozu sítě přenosů
Můžete filtrovat síťový provoz mezi podsítěmi pomocí jedné nebo obou z následujících možností:
- **Skupin zabezpečení (NSG) sítě:** každou NSG může obsahovat více pravidel příchozí a odchozí zabezpečení, které umožňují filtrovat provoz ve zdrojové a cílové IP adresy, portu a protokolu. Můžete použít skupinu NSG pro každý síťový adaptér ve virtuálním počítači. Můžete taky použít skupinu NSG k podsíti síťový adaptér nebo jiných prostředků Azure je připojen k. Další informace o skupinách Nsg, najdete [skupin zabezpečení sítě](virtual-networks-nsg.md) článku.
- **Síťových virtuálních zařízení (hodnocení chyb zabezpečení):** hodnocení chyb zabezpečení sítě je virtuální počítač spuštěný software, který provádí síťové funkce, jako je například Brána firewall. Zobrazit seznam dostupných NVAs v [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/category/networking?page=1&subcategories=appliances). NVAs jsou také k dispozici, které poskytují optimalizace sítě WAN a jiných síťových přenosů funkce. NVAs jsou obvykle používány s uživatelem nebo trasy protokolu BGP. Hodnocení chyb zabezpečení můžete také použít k filtrování provozu mezi virtuálními sítěmi.

## <a name="routing"></a>Směrovat síťový provoz

Azure vytvoří směrovací tabulky, které umožňují prostředky připojenými k žádné podsíti v žádné virtuální sítě pro komunikaci mezi sebou, ve výchozím nastavení. Můžete implementovat jednu nebo obě z následujících možností přepsat výchozí trasy, které vytvoří Azure:
- **Trasy definované uživatelem:** můžete vytvořit vlastní směrovací tabulky s trasami, které tuto kontrolu, kde provoz se směruje na pro každou podsíť. Další informace o trasách definovaných uživatelem najdete v článku [Trasy definované uživatelem](virtual-networks-udr-overview.md).
- **Trasy protokolu BGP:** Pokud virtuální sítě se připojit k místní síti pomocí připojení k Azure VPN Gateway nebo ExpressRoute, můžete rozšířit směrování BGP do vaší virtuální sítě.

## <a name="pricing"></a>Ceny

Skupiny zabezpečení není nijak zpoplatněn pro virtuální sítě, podsítě, směrovací tabulky nebo sítě. Odchozí šířky pásma Internetu, veřejné IP adresy, partnerský vztah virtuální sítě, brány sítě VPN a ExpressRoute každý mají svůj vlastní ceny struktury. Zobrazení [virtuální síť](https://azure.microsoft.com/pricing/details/virtual-network), [brány VPN](https://azure.microsoft.com/pricing/details/vpn-gateway), a [ExpressRoute](https://azure.microsoft.com/pricing/details/expressroute) ceny stránky pro další informace.

## <a name="faq"></a>Nejčastější dotazy

Nejčastější dotazy týkající se virtuální sítě najdete v tématu [virtuální sítě – nejčastější dotazy](virtual-networks-faq.md) článku.


## <a name="next-steps"></a>Další kroky

- Vytvoření vaší první virtuální sítě a připojení několika virtuálními počítači, pomocí kroků v [vytvoření vaší první virtuální síť](virtual-network-get-started-vnet-subnet.md) článku.
- Vytvořit připojení point-to-site k virtuální síti pomocí kroků v [konfigurace připojení typu point-to-site](../vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) článku.
- Přečtěte si o některých dalších klíče [sítě možnosti](../networking/networking-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json) Azure.
