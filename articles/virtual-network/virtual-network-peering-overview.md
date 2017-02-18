---
title: "Partnerské vztahy virtuálních sítí v Azure | Dokumentace Microsoftu"
description: "Seznamte se s partnerskými vztahy virtuálních sítí v Azure."
services: virtual-network
documentationcenter: na
author: NarayanAnnamalai
manager: jefco
editor: tysonn
ms.assetid: eb0ba07d-5fee-4db0-b1cb-a569b7060d2a
ms.service: virtual-network
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/17/2016
ms.author: narayan
translationtype: Human Translation
ms.sourcegitcommit: 15afcad97941fc595478e36e826a73831f40475e
ms.openlocfilehash: eb05b504c5cf13cd852a5e01cc3bec79fd20d547


---
# <a name="virtual-network-peering"></a>Partnerské vztahy virtuálních sítí
Partnerský vztah virtuálních sít (VNet) umožňuje propojit dvě virtuální sítě ve stejné oblasti prostřednictvím páteřní sítě Azure. Po navázání partnerského vztahu se tyto dvě virtuální sítě pro účely připojení jeví jako jedna síť. Tyto dvě virtuální sítě se i nadále spravují jako samostatné prostředky, ale virtuální počítače v těchto virtuálních sítích spolu mohou komunikovat přímo pomocí privátních IP adres.

Přenos dat mezi virtuálními počítači ve virtuálních sítích spojených v partnerském vztahu je směrován přes infrastrukturu Azure v zásadě stejně jako mezi virtuálními počítači ve stejné virtuální síti. Mezi výhody použití partnerských vztahů virtuálních sítí patří:

* Nízká latence a velká šířka pásma při propojení prostředků v různých virtuálních sítích.
* Možnost používat prostředky, například síťová zařízení a VPN Gateway, jako přenosové body ve virtuální síti s navázaným partnerským vztahem.
* Možnost navázání partnerského vztahu mezi dvěma virtuálními sítěmi vytvořenými prostřednictvím modelu nasazení Azure Resource Manager, nebo mezi jednou virtuální sítí vytvořenou prostřednictvím modelu nasazení Resource Manager a druhou vytvořenou prostřednictvím modelu nasazení Classic. Další informace o rozdílech mezi těmito dvěma modely nasazení Azure najdete v článku [Vysvětlení modelů nasazení Azure](../azure-resource-manager/resource-manager-deployment-model.md).

Požadavky a klíčové aspekty partnerských vztahů virtuálních sítí:

* Partnerské virtuální sítě musí existovat ve stejné oblasti Azure.
* Adresní prostory IP adres partnerských virtuálních sítí se nesmí překrývat.
* Partnerský vztah virtuálních sítí se navazuje mezi dvěma virtuálními sítěmi, ale nevzniká žádný odvozený tranzitivní vztah přes partnerské vztahy. Například pokud je síť VNetA v partnerském vztahu se sítí VNetB a VNetB je v partnerském vztahu se sítí VNetC, VNetA *není* v partnerském vztahu se sítí VNetC.
* Partnerský vztah lze navázat mezi virtuálními sítěmi ve dvou různých předplatných za předpokladu, že navázání tohoto vztahu u obou předplatných autorizuje uživatel s oprávněním správce a že předplatná jsou přidružená stejnému tenantu Active Directory.
* Mezi virtuálními sítěmi jde navázat partnerský vztah, pokud jsou obě vytvořené prostřednictvím modelu nasazení Resource Manager, nebo pokud je jedna vytvořená prostřednictvím modelu nasazení Resource Manager a druhá vytvořená prostřednictvím modelu nasazení Classic. Nejde ale navázat partnerský vztah mezi dvěma virtuálními sítěmi, které obě používají model nasazení Classic. Při vytváření partnerského vztahu virtuálních sítí vytvořených prostřednictvím různých modelů nasazení musí obě virtuální sítě existovat ve *stejném* předplatném. Možnost vytváření partnerského vztahu virtuálních sítí vytvořených prostřednictvím různých modelů nasazení v *různých* předplatných je ve verzi **Preview**. Další podrobnosti najdete v tématu [Vytvoření partnerského vztahu virtuálních sítí pomocí PowerShellu](virtual-networks-create-vnetpeering-arm-ps.md).
* Ačkoli komunikace mezi virtuálními počítači ve virtuálních sítích propojených partnerským vztahem nemá žádná další omezení šířky pásma, stále platí omezení šířky pásma na základě velikosti virtuálního počítače. Další informace o maximální šířce pásma pro různé velikosti virtuálních počítačů najdete v článcích o velikosti virtuálních počítačů pro [Windows](../virtual-machines/virtual-machines-windows-sizes.md) a [Linux](../virtual-machines/virtual-machines-linux-sizes.md).

![Základní partnerské vztahy virtuálních sítí](./media/virtual-networks-peering-overview/figure01.png)

## <a name="connectivity"></a>Připojení
Po vytvoření partnerského vztahu dvou virtuálních sítí, virtuální počítače nebo role Cloud Services ve virtuální síti se mohou připojovat přímo k jiným prostředkům připojeným k partnerské virtuální síti. Mezi těmito dvěma sítěmi existuje plné propojení na úrovni protokolu IP.

Síťová latence odezvy komunikace mezi dvěma virtuálními počítači ve virtuálních sítích s navázaným partnerským vztahem je stejná jako latence v rámci jedné virtuální sítě. Propustnost sítě závisí na šířce pásma, která je pro daný virtuální počítač povolena na základě jeho velikosti. S partnerským vztahem zde nejsou žádná další omezení týkající se šířky pásma.

Přenos dat mezi virtuálními počítači ve virtuálních sítích s navázaným partnerským vztahem je směrován přímo přes back-end infrastrukturu Azure, ne prostřednictvím brány.

Virtuální počítače připojené k virtuální síti mají přístup ke koncovým bodům s interním nástrojem pro vyrovnávání zatížení (ILB) v partnerské virtuální síti. Skupiny zabezpečení sítě (NSG) lze v obou virtuálních sítích v případě potřeby použít k blokování přístupu do jiných virtuálních sítí nebo podsítí.

Když konfigurujete partnerský vztah, můžete aktivovat nebo deaktivovat použití pravidel NSG mezi danými virtuálními sítěmi. Pokud otevřete úplné propojení mezi partnerskými virtuálními sítěmi (výchozí volba), může použitím skupin NSG u konkrétních podsítí nebo virtuálních počítačů blokovat nebo odepřít specifický přístup. Další informace o skupinách NSG najdete v tématu [Skupiny zabezpečení sítě](virtual-networks-nsg.md).

Interní překlad názvů DNS služby Azure pro virtuální počítače nebude mezi partnerskými virtuálními sítěmi fungovat. Virtuální počítače mají interní názvy DNS, které jsou přeložitelné pouze v rámci místní virtuální sítě. Můžete ale nakonfigurovat virtuální počítače připojené k partnerským virtuálním sítím jako servery DNS pro virtuální síť. Další podrobnosti najdete v článku s popisem [překladu IP adres pomocí vlastního serveru DNS](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server).

## <a name="service-chaining"></a>Řetězení služeb
Můžete nakonfigurovat uživatelsky definovaná směrování (UDR), která směřují na virtuální počítače v partnerských virtuálních sítích jako na IP adresy pro další směrování, jak je zobrazeno na diagramu dále v tomto článku. Tímto postupem lze dosáhnout řetězení služeb, s jehož pomocí je možné směrovat provoz z jedné virtuální sítě do virtuálního zařízení v partnerské virtuální síti s použitím uživatelsky definovaných směrování.

Můžete také fakticky vytvářet prostředí hvězdicovitého typu, kde centrální síť může být hostitelem komponent infrastruktury, například síťového virtuálního zařízení. Všechny virtuální sítě tvořící paprsky mohou navázat partnerský stav s centrální sítí a komunikovat se zařízeními, která běží v centrální virtuální síti. Partnerské vztahy virtuálních sítí tedy umožňují, aby adresou dalšího směrování v uživatelsky definovaných směrováních byla IP adresa virtuálního počítače v partnerské virtuální síti. Další informace o uživatelsky definovaných směrováních najdete v tématu s popisem [uživatelsky definovaných směrováních](virtual-networks-udr-overview.md).

## <a name="gateways-and-on-premises-connectivity"></a>Brány a místní připojení
Každá virtuální síť, bez ohledu na to, jestli má navázaný partnerský vztah s jinou virtuální sítí, může mít stále svou vlastní bránu a používat ji pro připojení k místní síti. Uživatelé mohou také nakonfigurovat [připojení mezi virtuálními sítěmi](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md) pomocí bran, a to i v situaci, kdy mezi danými virtuálními sítěmi je navázán partnerský vztah.

Když jsou nakonfigurovány obě možnosti pro propojení virtuálních sítí, provoz mezi těmito sítěmi probíhá s použitím konfigurace partnerského vztahu (t.j. přes páteřní síť Azure).

Když je mezi virtuálními sítěmi navázán partnerský vztah, mohou uživatelé také v partnerské virtuální síti nakonfigurovat bránu, která bude sloužit jako tranzitní bod k místní síti. V tomto případě virtuální síť, která používá vzdálenou bránu, nemůže mít svou vlastní bránu. Jedna virtuální síť může mít pouze jednu bránu. Může jí být buď místní brána, nebo vzdálená brána (v partnerské virtuální síti), jak ukazuje následující obrázek:

![Přenos dat při partnerských vztazích virtuálních sítí](./media/virtual-networks-peering-overview/figure02.png)

Průchod branou není podporován v případě, že jde o partnerský vztah mezi virtuálními sítěmi vytvořenými pomocí různých modelů nasazení. Pokud chcete používat průchod branou, je nutné, aby byly obě partnerské virtuální sítě vytvořené pomocí modelu nasazení Resource Manager.

Po nastavení partnerského vztahu mezi virtuálními sítěmi sdílejícími jedno připojení Azure ExpressRoute prochází provoz skrz partnerský vztah (tj. prostřednictvím páteřní sítě Azure). K připojení k místnímu okruhu můžete v obou virtuálních sítích nadále používat místní brány. Alternativním postupem je použití sdílené brány a konfigurace průchodu pro místní připojení.

## <a name="provisioning"></a>Zřizování
Navázání partnerského vztahu virtuální sítě je privilegovaná operace. Jde o samostatnou funkci v rámci oboru názvů VirtualNetworks. Uživatel může dostat specifická práva k autorizaci partnerských vztahů. Uživatel, který má přístup pro čtení a zápis do virtuální sítě, zdědí tato práva automaticky.

Uživatel, který je buď správcem nebo privilegovaným uživatelem pro navázání partnerských vztahů, může iniciovat operaci navázání partnerství s jinou virtuální sítí. Pokud i na druhé straně existuje odpovídající požadavek a jsou splněny i další podmínky, partnerský vztah bude navázán.

V článcích uvedených v části [Další kroky](#next-steps) tohoto článku najdete informace o tom, jak navázat partnerský vztah mezi dvěma virtuálními sítěmi.

## <a name="limits"></a>Omezení
Existují omezení týkající se počtu partnerských vztahů, které lze vytvořit pro jednu virtuální síť. Další informace najdete v článku [Omezení pro sítě ve službě Azure](../azure-subscription-service-limits.md#networking-limits).

## <a name="pricing"></a>Ceny
Za příchozí a odchozí přenos dat využívající partnerský vztah virtuálních sítí se účtuje nominální poplatek. Další informace naleznete na [stránce s cenami](https://azure.microsoft.com/pricing/details/virtual-network).

## <a name="a-namenext-stepsanext-steps"></a><a name="next-steps"></a>Další kroky
Naučte se vytvořit partnerský vztah virtuálních sítí pomocí následujících metod:

* [Azure Portal](virtual-networks-create-vnetpeering-arm-portal.md)
* [Azure PowerShell](virtual-networks-create-vnetpeering-arm-ps.md)
* [Šablona Azure Resource Manageru](virtual-networks-create-vnetpeering-arm-template-click.md)



<!--HONumber=Feb17_HO1-->


