---
title: "O mapování sítě pro replikaci virtuálního počítače technologie Hyper-V pomocí Site Recovery | Microsoft Docs"
description: "Nastavte mapování sítě pro Hyper-V replikaci virtuálního počítače z překážek místní datacentra do Azure nebo do sekundární lokality."
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: article
ms.date: 02/07/2018
ms.author: raynew
ms.openlocfilehash: d56f8f5bfb40c1c43090f43e119bf9b98918d6e5
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/09/2018
---
# <a name="about-network-mapping-for-hyper-v-vm-replication"></a>O mapování sítě pro replikaci virtuálního počítače technologie Hyper-V


Tento článek vám pomůže pochopit a naplánovat sítě mapování během replikace virtuálních počítačů Hyper-V do Azure nebo do sekundární lokality, pomocí [služba Azure Site Recovery](site-recovery-overview.md).

Po přečtení tohoto článku post jakékoli komentáře v dolní části tohoto článku, nebo požádejte technické dotazy na [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="network-mapping-for-replication-to-azure"></a>Mapování sítě pro replikaci do Azure.

Mapování sítě se používá při replikaci virtuálních počítačů Hyper-V (spravované v nástroji VMM) do Azure. Síťové mapování mapuje sítě virtuálních počítačů na zdrojovém serveru VMM a cílové sítě Azure. Mapování provede následující akce:

- **Síťové připojení**– zajišťuje, že replikované virtuální počítače Azure jsou připojené k namapované síti. Všechny počítače, které převzetí služeb při selhání ve stejné síti může připojit k sobě navzájem i v případě, že při selhání v plánech různých obnovení.
- **Brána sítě**– Pokud v cílové síti Azure nastavená brána sítě, virtuální počítače připojit k jiné místní virtuální počítače.

Poznámky:

- Namapujete zdrojové síti virtuálních počítačů nástroje VMM na virtuální síť Azure.
- Po převzetí služeb při selhání virtuálních počítačů Azure ve zdroji sítě se připojí k namapované cílové virtuální síti.
- Nové virtuální počítače přidat do zdrojové síti virtuálních počítačů jsou připojené k namapované síti Azure, když dojde k replikaci.
- Pokud má cílová síť více podsítí a jedna z těchto podsítí má stejný název jako podsíť, ve které je umístěný zdrojový virtuální počítač, pak se virtuální počítač repliky po převzetí služeb při selhání připojí k této cílové podsíti.
- Pokud neexistuje žádná cílová podsíť s odpovídajícím názvem, připojí se virtuální počítač k první podsíti v síti.


## <a name="network-mapping-for-replication-to-a-secondary-datacenter"></a>Mapování sítě pro replikaci do sekundárního datacentra

Mapování sítě se používá při replikaci virtuálních počítačů Hyper-V (spravované v System Center Virtual Machine Manager (VMM)) do sekundárního datacentra. Mapování sítě zajišťuje mapování mezi sítěmi virtuálních počítačů na zdrojovém serveru VMM a sítě virtuálních počítačů na cílovém serveru VMM. Mapování provede následující akce:

- **Síťové připojení**– připojí virtuální počítače na odpovídající sítě po převzetí služeb při selhání. Virtuální počítač repliky se připojí k cílové síti, který je namapovaný na zdrojovou síť.
- **Optimální umístění**– optimálně umístí virtuální počítače replik na hostitelských serverech technologie Hyper-V. Virtuální počítače repliky jsou umístěny na hostitelích, kteří mohou přistupovat k namapovanou síť virtuálních počítačů.
- **Žádné mapování sítě**– Pokud nenakonfigurujete mapování sítě, virtuální počítače replik se nepřipojí k žádné síti virtuálních počítačů po převzetí služeb při selhání.

Poznámky:

- Mapování sítě můžete nakonfigurovat sítě virtuálních počítačů na dvěma servery VMM nebo na jednom serveru VMM, pokud dvě lokality jsou spravovány stejném serveru.
- Při mapování správně nakonfigurovaná a je povolená replikace, virtuální počítač v primární lokalitě se připojí k síti a jeho repliky v cílovém umístění, budou připojené k jeho namapované síťové.
-
- Pokud sítě byla nastavili správně v nástroji VMM, když při mapování sítě vyberte cílovou síť virtuálních počítačů, cloudy VMM zdroje, které používají zdrojové síti virtuálních počítačů zobrazí se, společně s sítí dostupných cílových virtuálních počítačů v cílové cloudech, které se používají pro ochranu.
- Pokud má cílová síť více podsítí a jedna z těchto podsítí má stejný název jako podsíť, na kterém se nachází zdrojový virtuální počítač, pak virtuální počítač repliky se připojí k této cílové podsíti po převzetí služeb při selhání. Pokud neexistuje žádná cílová podsíť s odpovídajícím názvem, připojí se virtuální počítač k první podsíti v síti.



### <a name="example"></a>Příklad:

Tady je příklad pro ilustraci tento mechanismus. Podívejme se na organizaci s dvěma umístěními v New Yorku a Chicagu.

**Umístění** | **Server VMM** | **Sítě virtuálních počítačů** | **Mapovat na**
---|---|---|---
New York | VMM NewYork| VMNetwork1-NewYork | Mapovat do Chicaga VMNetwork1
 |  | VMNetwork2-NewYork | Není mapováno
Chicago | VMM Chicago| VMNetwork1 Chicago | Mapovat na VMNetwork1 NewYork
 | | VMNetwork1 Chicago | Není mapováno

V tomto příkladu:

- Když virtuální počítač repliky se vytvoří pro virtuální počítač, který je připojen k VMNetwork1 NewYork, bude připojen k VMNetwork1 Chicagu.
- Když virtuální počítač repliky se vytvoří pro VMNetwork2 NewYork nebo VMNetwork2 Chicagu, nebude připojen k žádné síti.

Zde je, jak jsou cloudy VMM nastavit v našem příkladu organizace a logické sítě přidružené ke cloudům.

#### <a name="cloud-protection-settings"></a>Nastavení ochrany cloudu

**Chráněném cloudu** | **Ochrana cloudu** | **Logické sítě (New Yorku)**  
---|---|---
GoldCloud1 | GoldCloud2 |
SilverCloud1| SilverCloud2 |
GoldCloud2 | <p>Není k dispozici</p><p></p> | <p>LogicalNetwork1-NewYork</p><p>LogicalNetwork1-Chicago</p>
SilverCloud2 | <p>Není k dispozici</p><p></p> | <p>LogicalNetwork1-NewYork</p><p>LogicalNetwork1-Chicago</p>

#### <a name="logical-and-vm-network-settings"></a>Nastavení sítě logické a virtuální počítač

**Umístění** | **Logické sítě** | **Přidružené sítě virtuálních počítačů**
---|---|---
New York | LogicalNetwork1-NewYork | VMNetwork1-NewYork
Chicago | LogicalNetwork1-Chicago | VMNetwork1 Chicago
 | LogicalNetwork2Chicago | VMNetwork2-Chicago

#### <a name="target-network-settings"></a>Nastavení cílové sítě

Na základě tohoto nastavení, když vyberete Cílová síť virtuálních počítačů, v následující tabulce jsou možnosti, které budou k dispozici.

**Výběr** | **Chráněném cloudu** | **Ochrana cloudu** | **Cílová síť k dispozici**
---|---|---|---
VMNetwork1 Chicago | SilverCloud1 | SilverCloud2 | Dostupné
 | GoldCloud1 | GoldCloud2 | Dostupné
VMNetwork2-Chicago | SilverCloud1 | SilverCloud2 | Není k dispozici
 | GoldCloud1 | GoldCloud2 | Dostupné


Pokud má cílová síť více podsítí a jedna z těchto podsítí má stejný název jako podsíť, na kterém se nachází zdrojový virtuální počítač, pak virtuální počítač repliky se připojí k této cílové podsíti po převzetí služeb při selhání. Pokud neexistuje žádná cílová podsíť s odpovídajícím názvem, připojí se virtuální počítač k první podsíti v síti.


#### <a name="failback-behavior"></a>Chování navrácení služeb po obnovení

Pokud chcete zobrazit, co se stane, že v případě navrácení služeb po obnovení (zpětná replikace), Předpokládejme, že VMNetwork1 NewYork je namapována na VMNetwork1-Chicagu s následujícím nastavením.


**Virtuální počítač** | **Připojení k síti virtuálních počítačů**
---|---
VM1 | VMNetwork1-Network
Virtuálního počítače 2 (repliky VM1) | VMNetwork1 Chicago

S těmito nastaveními pojďme si shrnout, co se stane, že v několika možných scénářích.

**Scénář** | **Výsledek**
---|---
Žádná změna v vlastnosti sítě virtuálních počítačů 2 po převzetí služeb při selhání. | Zůstane připojené ke zdrojové síti virtuálních počítačů 1.
Vlastnosti sítě virtuálních počítačů 2 se změní po převzetí služeb při selhání a není připojen. | 1 virtuální počítač není připojen.
Vlastnosti sítě virtuálních počítačů 2 se změní po převzetí služeb při selhání a je připojený k VMNetwork2 Chicagu. | Pokud není namapované VMNetwork2 Chicagu, bude odpojen virtuálních počítačů 1.
Mapování sítě VMNetwork1 Chicagu se změní. | 1 virtuální počítač bude připojený k síti nyní mapováno na VMNetwork1 Chicagu.



## <a name="next-steps"></a>Další postup

Další informace o [plánování síťové infrastruktury](site-recovery-network-design.md).
