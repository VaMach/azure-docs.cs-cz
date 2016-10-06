
<properties
   pageTitle="Partnerské vztahy virtuálních sítí ve službě Azure | Microsoft Azure"
   description="Informace o partnerských vztazích virtuálních sítí ve službě Azure."
   services="virtual-network"
   documentationCenter="na"
   authors="NarayanAnnamalai"
   manager="jefco"
   editor="tysonn" />
<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="07/28/2016"
   ms.author="narayan" />


# Partnerské vztahy virtuálních sítí

Partnerský vztah virtuálních sítí je mechanismus, který spojuje dvě virtuální sítě ve stejné oblasti prostřednictvím páteřní sítě Azure. Po navázání partnerského vztahu se tyto dvě virtuální sítě pro všechny účely připojení jeví jako jedna síť. Jsou i nadále spravovány jako samostatné prostředky, ale virtuální počítače v těchto virtuálních sítích spolu mohou komunikovat přímo pomocí privátních IP adres.

Přenos dat mezi virtuálními počítači ve virtuálních sítích spojených v partnerském vztahu je směrován přes infrastrukturu Azure v zásadě stejně jako mezi virtuálními počítači ve stejné virtuální síti. Mezi výhody použití partnerských vztahů virtuálních sítí patří:

- Nízká latence a velká šířka pásma při propojení prostředků v různých virtuálních sítích.
- Možnost používat prostředky, například síťová zařízení a VPN Gateway, jako přenosové body ve virtuální síti s navázaným partnerským vztahem.
- Možnost připojit virtuální síť, která používá model Azure Resource Manager, k virtuální síti používající klasický model nasazení a umožnit plnohodnotné propojení mezi prostředky v těchto virtuálních sítích.

Požadavky a klíčové aspekty partnerských vztahů virtuálních sítí:

- Dvě virtuální sítě, mezi nimiž má být navázán partnerský vztah, by měly být ve stejné oblasti Azure.
- Adresní prostory IP adres virtuálních sítí, které mají být v partnerském vztahu, by se neměly překrývat.
- Partnerský vztah virtuálních sítí se navazuje mezi dvěma virtuálními sítěmi a nevzniká žádný odvozený tranzitivní vztah. Pokud je například virtuální síť A v partnerském vztahu s virtuální sítí B a síť B je v partnerském vztahu s virtuální sítí C, neznamená to, že se vytvoří partnerský vztah mezi virtuálními sítěmi A a C.
- Partnerský vztah lze navázat mezi virtuálními sítěmi ve dvou různých předplatných za předpokladu, že navázání tohoto vztahu u obou předplatných autorizuje uživatel s oprávněním správce a že předplatná jsou přidružená stejnému tenantu Active Directory. 
- Virtuální síť používající model nasazení Resource Manageru můžete propojit s jinou virtuální sítí, která tento model používá také, nebo s virtuální sítí, která používá klasický model nasazení. Nejde ale navázat partnerský vztah mezi dvěma virtuálními sítěmi, které obě používají klasický model nasazení.
- Ačkoli komunikace mezi virtuálními počítači v sítích propojených partnerským vztahem nemá žádná další omezení šířky pásma, stále platí omezení šířky pásma na základě velikosti virtuálního počítače.


![Základní partnerské vztahy virtuálních sítí](./media/virtual-networks-peering-overview/figure01.png)

## Připojení
Po navázání partnerského vztahu mezi dvěma virtuálními sítěmi se virtuální počítač (ve webové roli či roli pracovního procesu) v jedné virtuální síti může přímo připojit k virtuálním počítačům v druhé síti. Mezi těmito dvěma sítěmi existuje plné propojení na úrovni protokolu IP.

Síťová latence odezvy komunikace mezi dvěma virtuálními počítači ve virtuálních sítích s navázaným partnerským vztahem je stejná jako latence v rámci místní virtuální sítě. Propustnost sítě závisí na šířce pásma, která je pro daný virtuální počítač povolena na základě jeho velikosti. Nejsou zde žádná další omezení týkající se šířky pásma.

Přenos dat mezi virtuálními počítači ve virtuálních sítích s navázaným partnerským vztahem je směrován přímo přes back-end infrastrukturu Azure, ne prostřednictvím brány.

Virtuální počítače ve virtuální síti mají přístup ke koncovým bodům s interním nástrojem pro vyrovnávání zatížení (ILB) v partnerské virtuální síti. Skupiny zabezpečení sítě (NSG) lze v obou virtuálních sítích v případě potřeby použít k blokování přístupu do jiných virtuálních sítí nebo podsítí.

Když uživatelé konfigurují partnerský vztah, mohou aktivovat nebo deaktivovat použití pravidel NSG mezi danými virtuálními sítěmi. Pokud si uživatel zvolí možnost otevření úplného propojení mezi partnerskými virtuálními sítěmi (výchozí volba), může následně pomocí skupin NSG u konkrétních podsítí nebo virtuálních počítačů blokovat nebo odepřít specifický přístup.

Interní překlad názvů DNS služby Azure pro virtuální počítače nebude fungovat mezi partnerskými virtuálními sítěmi fungovat. Virtuální počítače mají interní názvy DNS, které jsou přeložitelné pouze v rámci místní virtuální sítě. Uživatelé ovšem mohou virtuální počítače běžící v partnerských virtuálních sítích nakonfigurovat jako servery DNS pro virtuální síť.

## Řetězení služeb
Uživatelé mohou nakonfigurovat uživatelsky definované tabulky směrování, které směřují na virtuální počítače v partnerských virtuálních sítích jako na IP adresy pro další směrování, jak je zobrazeno na diagramu dále v tomto článku. Tímto postupem lze dosáhnout řetězení služeb, s jehož pomocí je možné směrovat přenos dat z jedné virtuální sítě do virtuálního zařízení v partnerské virtuální síti s použitím uživatelsky definovaných tabulek směrování.

Uživatelé mohou také fakticky vytvářet prostředí hvězdicovitého typu, kde centrální síť může být hostitelem komponent infrastruktury, například síťového virtuálního zařízení. Všechny virtuální sítě tvořící paprsky mohou navázat partnerský stav s centrální sítí a komunikovat se zařízeními, která běží v centrální virtuální síti. Partnerské vztahy virtuálních sítí tedy umožňují, aby adresou dalšího směrování v uživatelsky definované tabulce směrování byla IP adresa virtuálního počítače v partnerské virtuální síti.

## Brány a místní připojení
Každá virtuální síť, bez ohledu na to, jestli má navázaný partnerský vztah s jinou virtuální sítí, může mít stále svou vlastní bránu a používat ji pro místní připojení. Uživatelé mohou také nakonfigurovat [připojení mezi virtuálními sítěmi](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md) pomocí bran, a to i v situaci, kdy mezi danými virtuálními sítěmi je navázán partnerský vztah.

Když jsou nakonfigurovány obě možnosti pro propojení virtuálních sítí, přenos dat mezi těmito sítěmi probíhá s použitím konfigurace partnerského vztahu (t.j. přes páteřní síť Azure).

Když je mezi virtuálními sítěmi navázán partnerský vztah, mohou uživatelé také v partnerské virtuální síti nakonfigurovat bránu, která bude sloužit jako tranzitní bod pro místní připojení. V tomto případě virtuální síť, která používá vzdálenou bránu, nemůže mít svou vlastní bránu. Jedna virtuální síť může mít pouze jednu bránu. Může jí být buď místní brána, nebo vzdálená brána (v partnerské virtuální síti), viz následující obrázek.

Průchod branou není podporován v případě, že jde o partnerský vztah mezi virtuální sítí používající model Resource Manageru a virtuální sítí s klasickým modelem nasazení. Pokud chcete používat průchod branou, je nutné, aby obě partnerské virtuální sítě používaly model nasazení Resource Manageru.

Po nastavení partnerského vztahu mezi virtuálními sítěmi sdílejícími jedno připojení Azure ExpressRoute přenos dat prochází skrze partnerský vztah (tj. prostřednictvím páteřní sítě Azure). K připojení k místnímu okruhu lze v obou sítích nadále používat místní brány. Alternativním postupem je použití sdílené brány a konfigurace průchodu pro místní připojení.

![Přenos dat při partnerských vztazích virtuálních sítí](./media/virtual-networks-peering-overview/figure02.png)

## Zřizování
Navázání partnerského vztahu virtuální sítě je privilegovaná operace. Jde o samostatnou funkci v rámci oboru názvů VirtualNetworks. Uživatel může dostat specifická práva k autorizaci partnerských vztahů. Uživatel, který má přístup pro čtení a zápis do virtuální sítě, zdědí tato práva automaticky.

Uživatel, který je buď správcem nebo privilegovaným uživatelem pro navázání partnerských vztahů, může iniciovat operaci navázání partnerství s jinou virtuální sítí. Pokud i na druhé straně existuje odpovídající požadavek a jsou splněny i další podmínky, partnerský vztah bude navázán.

V článcích uvedených v části „Další kroky“ naleznete další informace o tom, jak navázat partnerský vztah mezi dvěma virtuálními sítěmi.

## Omezení
Existují omezení týkající se počtu partnerských vztahů, které lze vytvořit pro jednu virtuální síť. Další informace najdete v článku [Omezení pro sítě ve službě Azure](../azure-subscription-service-limits.md#networking-limits).

## Ceny
Během období vyhodnocování bude funkce partnerských vztahů virtuálních sítí k dispozici bezplatně. Po jejím oficiálním vydání bude za příchozí a odchozí přenos dat využívající partnerské vztahy účtován nominální poplatek. Další informace naleznete na [stránce s cenami](https://azure.microsoft.com/pricing/details/virtual-network).


## Další kroky
- [Nastavení partnerského vztahu mezi virtuálními sítěmi](virtual-networks-create-vnetpeering-arm-portal.md).
- Další informace o skupinách [NSG](virtual-networks-nsg.md).
- Další informace o [uživatelem definovaných tabulkách směrování a předávání IP](virtual-networks-udr-overview.md).



<!--HONumber=Sep16_HO4-->


