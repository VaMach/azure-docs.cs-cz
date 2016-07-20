<properties 
   pageTitle="Co jsou trasy definované uživatelem a předávání IP?"
   description="Naučte se pomocí tras definovaných uživatelem (UDR) a předávání IP přesměrovat provoz do síťových virtuálních zařízení v Azure."
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

# Co jsou trasy definované uživatelem a předávání IP?
Když přidáte virtuální počítače do virtuální sítě v Azure, uvidíte, že tyto virtuální počítače automaticky umí vzájemně komunikovat prostřednictvím sítě. Není nutné určit bránu, ani když jsou virtuální počítače v různých podsítích. Totéž platí pro komunikaci z virtuálních počítačů do veřejného internetu, a dokonce i do vaší místní sítě, pokud je dostupné hybridní připojení z Azure do vlastního datacentra.

Tento tok komunikace je možný díky tomu, že Azure pomocí řady systémových tras definuje toky provozu IP. Systémové trasy řídí tok komunikace v těchto scénářích:

- Ze stejné podsítě.
- Z jedné podsítě do jiné v rámci jedné virtuální sítě.
- Z virtuálních počítačů do internetu.
- Z jedné virtuální sítě do jiné prostřednictvím brány sítě VPN.
- Z virtuální sítě do místní sítě prostřednictvím brány sítě VPN.

Následující obrázek znázorňuje jednoduché uspořádání s jednou virtuální sítí, dvěma podsítěmi, několika virtuálními počítači a systémovými trasami, které umožňují tok provozu IP.

![Systémové trasy v Azure](./media/virtual-networks-udr-overview/Figure1.png)

Ačkoli použití systémových tras vašemu nasazení automaticky usnadňuje provoz, v některých případech je vhodné řídit směrování paketů prostřednictvím virtuálního zařízení. Můžete to provést tím, že vytvoříte trasy definované uživatelem, které určí další segment směrování tak, aby pakety směřující do konkrétní podsítě místo toho přicházely do virtuálního zařízení, a virtuálnímu počítači spuštěnému jako virtuální zařízení povolíte předávání IP.

Následující obrázek znázorňuje příklad tras definovaných uživatelem a předávání IP, kdy pakety odeslané z jedné podsítě do druhé jsou nucené procházet virtuálním zařízením ve třetí podsíti.

![Systémové trasy v Azure](./media/virtual-networks-udr-overview/Figure2.png)

>[AZURE.IMPORTANT] Trasy definované uživatelem se použijí jenom u odchozího provozu z podsítě. Nejde například vytvořit trasy, které by určily, jak bude provoz z internetu přicházet do podsítě. Zařízení, do kterých přesměrujete provoz, navíc nemůžou být ve stejné podsíti, odkud provoz pochází. Pro svoje zařízení vždycky vytvořte samostatnou podsíť. 

## Prostředek trasy
Pakety se přes síť TCP/IP směrují na základě směrovací tabulky definované v každém uzlu fyzické sítě. Směrovací tabulka je kolekce jednotlivých tras, podle které se na základě cílové IP adresy rozhoduje, kam se pakety předají. Trasa se skládá z těchto položek:

|Vlastnost|Popis|Omezení|Požadavky|
|---|---|---|---|
| Předpona adresy | Cílový rozsah CIDR, na který se trasa vztahuje, například 10.1.0.0/16.|Toto musí být platný rozsah CIDR, který reprezentuje adresy ve veřejném internetu, virtuální síti Azure nebo místním datacentru.|Ujistěte se, že **Předpona adresy** neobsahuje adresu uvedenou ve vlastnosti **Hodnota dalšího segmentu**, jinak se pakety dostanou do smyčky mezi zdrojem a dalším segmentem a nikdy nedorazí do cíle. |
| Typ dalšího segmentu | Typ segmentu Azure, do kterého se má paket odeslat. | Toto musí být jedna z následujících hodnot: <br/> **Místní.** Představuje místní virtuální síť. Pokud máte například dvě podsítě, 10.1.0.0/16 a 10.2.0.0/16, ve stejné virtuální síti, trasa každé podsítě ve směrovací tabulce bude obsahovat hodnotu dalšího segmentu *Místní*. <br/> **Brána sítě VPN.** Představuje bránu Azure S2S VPN Gateway. <br/> **Internet.** Představuje výchozí internetovou bránu poskytovanou infrastrukturou Azure. <br/> **Virtuální zařízení.** Představuje virtuální zařízení, které jste přidali do virtuální sítě Azure. <br/> **NULL**. Představuje černou díru. Pakety předané do černé díry se nepředají vůbec.| Typ **NULL** se vám může hodit, pokud chcete zastavit tok paketů do určitého cíle. | 
| Hodnota dalšího segmentu | Hodnota dalšího segmentu obsahuje IP adresu, na kterou se mají předávat pakety. Hodnoty dalšího segmentu jsou povolené jenom v trasách, kde typ dalšího segmentu je *Virtuální zařízení*.| Toto musí být dostupná IP adresa. | Pokud IP adresa představuje virtuální počítač, nezapomeňte tomuto virtuálnímu počítači povolit [předávání IP](#IP-forwarding) v Azure. |

### Systémové trasy
Každá podsíť vytvořená ve virtuální síti se automaticky přidruží k směrovací tabulce, která obsahuje následující pravidla systémových tras:

- **Pravidlo místní virtuální sítě:** Toto pravidlo se automaticky vytvoří pro každou podsíť ve virtuální síti. Určuje, že mezi virtuálními počítači ve virtuální síti je přímé propojení a není tu žádný zprostředkující další segment.
- **Místní pravidlo:** Toto pravidlo se používá na veškerý provoz, jehož cílem je místní rozsah adres, a jako cíl dalšího segmentu používá bránu sítě VPN.
- **Internetové pravidlo:** Toto pravidlo zpracovává veškerý provoz, jehož cílem je veřejný internet, a jako další segment pro veškerý provoz směřující do internetu používá internetovou bránu infrastruktury.

### Trasy definované uživatelem
U většiny prostředí budete potřebovat jenom systémové trasy, které jsou už v Azure definované. V určitých případech ale může být nutné vytvořit směrovací tabulku a přidat jednu nebo víc tras. Může jít například o tyto situace:

- Vynucené tunelování do internetu prostřednictvím místní sítě.
- Použití virtuálních zařízení v prostředí Azure.

Ve výše uvedených scénářích budete muset vytvořit směrovací tabulku a přidat do ní trasy definované uživatelem. Může mít víc směrovacích tabulek a stejná směrovací tabulka se dá přidružit k jedné nebo víc podsítím. Každá podsíť může být přidružená jenom k jedné směrovací tabulce. Všechny virtuální počítače a cloudové služby v podsíti používají směrovací tabulku, která je k této podsíti přidružená.

Dokud se k podsíti nepřidruží směrovací tabulka, podsíť používá systémové trasy. Jakmile existuje přidružení, směrování se provádí na základě nejdelší shody předpony (LPM) jak mezi trasami definovanými uživateli, tak mezi systémovými trasami. Pokud existuje víc tras se stejnou shodou LPM, trasa se vybere na základě původu v tomto pořadí:

1. Trasa definovaná uživatelem
1. Trasa protokolu BGP (pokud se používá služba ExpressRoute)
1. Systémová trasa

Pokud se chcete naučit vytvářet trasy definované uživatelem, informace najdete v části [Vytváření tras a povolení předávání IP v Azure](virtual-networks-udr-how-to.md#How-to-manage-routes).

>[AZURE.IMPORTANT] Trasy definované uživatelem se použijí jenom pro cloudové služby a virtuální počítače Azure. Pokud například chcete přidat virtuální zařízení brány firewall mezi místní síť a Azure, budete muset vytvořit trasu definovanou uživatelem pro směrovací tabulky Azure, které veškerý provoz směřující do místního adresního prostoru přesměrují do tohoto virtuálního zařízení. Příchozí provoz z místního adresního prostoru ale bude procházet bránou sítě VPN nebo okruhem ExpressRoute rovnou do prostředí Azure, a virtuální zařízení obejde.

### Trasy protokolu BGP
Pokud máte spojení ExpressRoute mezi místní sítí a Azure, můžete povolit, aby protokol BGP šířil trasy z místní sítě do Azure. Tyto trasy protokolu BGP se v jednotlivých podsítích Azure používají stejným způsobem jako systémové trasy a trasy definované uživatelem. Další informace najdete v tématu [Úvod do služby ExpressRoute](../expressroute/expressroute-introduction.md).

>[AZURE.IMPORTANT] Svoje prostředí Azure můžete nakonfigurovat, aby používalo vynucené tunelování prostřednictvím místní sítě, a to tak, že pro podsíť 0.0.0.0/0 vytvoříte trasu definovanou uživatelem, která jako další segment používá bránu sítě VPN. To ale funguje jenom v případě, že používáte bránu sítě VPN, nikoli službu ExpressRoute. U služby ExpressRoute se vynucené tunelování konfiguruje prostřednictvím BGP.

## Předávání IP
Jak se uvádí výše, jedním z hlavních důvodů k vytváření tras definovaných uživatelem je přesměrování provozu do virtuálního zařízení. Virtuální zařízení není nic jiného než virtuální počítač, na kterém běží aplikace sloužící k určitému zpracování síťového provozu, jako je například brána firewall nebo zařízení NAT.

Tento virtuální počítač virtuálního zařízení musí být schopný přijímat příchozí provoz, který mu není adresovaný. Pokud chcete virtuálnímu počítači povolit přijímání dat adresovaných jiným cílům, je nutné, abyste mu povolili předávání IP. Toto je nastavení Azure, nikoli nastavení hostovaného operačního systému.

## Další kroky

- Naučte se [vytvářet trasy v modelu nasazení Resource Manager](virtual-network-create-udr-arm-template.md) a přidružovat je k podsítím. 
- Naučte se [vytvářet trasy v modelu nasazení Classic](virtual-network-create-udr-classic-ps.md) a přidružovat je k podsítím.



<!--HONumber=Jun16_HO2-->


