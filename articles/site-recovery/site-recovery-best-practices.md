---
title: "Osvědčené postupy pro Azure Site Recovery | Dokumentace Microsoftu"
description: "Tento článek popisuje osvědčené postupy pro nasazení Azure Site Recovery."
services: site-recovery
documentationCenter: 
author: rayne-wiselman"
manager: cfreeman
editor: 
ms.assetid: c413efcd-d750-4b22-b34b-15bcaa03934a
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/14/2017
ms.author: raynew
ROBOTS: NOINDEX, NOFOLLOW
redirect_url: site-recovery-support-matrix-to-azure
ms.translationtype: Human Translation
ms.sourcegitcommit: a087df444c5c88ee1dbcf8eb18abf883549a9024
ms.openlocfilehash: a53e2a09fb2aabe96dd5347ea7509815d92bfdb8
ms.contentlocale: cs-cz
ms.lasthandoff: 03/15/2017

---

# <a name="get-ready-to-deploy-azure-site-recovery"></a>Příprava nasazení Azure Site Recovery

Tento článek popisuje přípravu na nasazení Azure Site Recovery.

## <a name="protecting-hyper-v-virtual-machines"></a>Ochrana virtuálních počítačů Hyper-V

Pro ochranu virtuálních počítačů máte na výběr z několika možností nasazení. Místní virtuální počítače Hyper-V můžete replikovat do Azure nebo do sekundárního datacentra. Každé nasazení má specifické požadavky.

**Požadavek** | **Replikace do Azure (s VMM)** | **Replikace virtuálních počítačů Hyper-V do Azure (bez VMM)** | **Replikace virtuálních počítačů Hyper-V do sekundární lokality (s VMM)** | **Podrobnosti**
---|---|---|---|---
**VMM** | Nástroj VMM běžící na řešení System Center 2012 R2 <br/><br/>Alespoň jeden cloud VMM obsahující minimálně jednu skupinu hostitelů VMM | Není k dispozici | Servery VMM v primární a sekundární lokalitě běžící na řešení System Center 2012 SP1 s nejnovějšími aktualizacemi <br/><br/> Alespoň jeden cloud na každém serveru VMM. Cloudy musí mít nastavený profil kapacity Hyper-V.<br/><br/> Zdrojový cloud musí obsahovat alespoň jednu skupinu hostitelů VMM. | Volitelné. K replikování virtuálních počítačů Hyper-V do Azure není potřeba mít nasazený nástroj System Center VMM, ale pokud ho nasazený máte, ujistěte se, že je správně nastavený server VMM. Mimo jiné ověřte, že používáte správnou verzi VMM a že jsou nastavené cloudy.
**Hyper-V** | Alespoň jeden hostitelský server Hyper-V v místní lokalitě s Windows Serverem 2012 R2 nebo novějším | Alespoň jeden server Hyper-V v místní a cílové lokalitě, na kterém běží přinejmenším Windows Server 2012 s nainstalovanými nejnovějšími aktualizacemi, a který je připojený k internetu.<br/><br/> Servery Hyper-V musí být ve skupině hostitelů v cloudu VMM. | Alespoň jeden server Hyper-V v místní a cílové lokalitě, na kterém běží přinejmenším Windows Server 2012 s nainstalovanými nejnovějšími aktualizacemi, a který je připojený k internetu.<br/><br/> Servery Hyper-V musí být umístěné ve skupině hostitelů v cloudu VMM. |
**Virtual Machines** | Alespoň jeden virtuální počítač na zdrojovém hostitelském serveru Hyper-V | Alespoň jeden virtuální počítač na hostitelském serveru Hyper-V ve zdrojovém cloudu VMM | Alespoň jeden virtuální počítač na hostitelském serveru Hyper-V ve zdrojovém cloudu VMM |  Virtuální počítače replikované do Azure musí splňovat požadavky na virtuální počítače Azure.
**Účet Azure** | Budete potřebovat účet Azure a předplatné služby Site Recovery. | Budete potřebovat účet Azure a předplatné služby Site Recovery. | Není k dispozici | Pokud účet nemáte, začněte s bezplatnou zkušební verzí.
**Úložiště Azure** | Budete potřebovat předplatné pro účet úložiště Azure, které má povolenou geografickou replikací. | Budete potřebovat předplatné pro účet úložiště Azure, které má povolenou geografickou replikací. | Není k dispozici | Účet musí být ve stejné oblasti jako trezor služby Azure Site Recovery a musí být přidružený ke stejnému předplatnému.
**Sítě** | Nastavte mapování sítě, abyste zajistili, že všechny počítače, u kterých dojde k převzetí služeb při selhání ve stejné síti Azure, se budou moct připojit k sobě navzájem, a to bez ohledu na to, v jakém jsou plánu obnovení. Pokud je navíc v cílové síti Azure nakonfigurovaná síťová brána, virtuální počítače se budou moct připojit k jiným místním virtuálním počítačům. Pokud mapování sítě nenastavíte, budou se moct připojit pouze virtuální počítače, u kterých došlo k převzetí služeb při selhání v rámci stejného plánu obnovení. | Není k dispozici |  <br/><br/>Nastavte mapování sítě, abyste zajistili, že se virtuální počítače po převzetí služeb při selhání připojí k odpovídajícím sítím, a že se virtuální počítače repliky optimálně umístí na hostitelské servery Hyper-V. Pokud mapování sítě nenakonfigurujete, replikované počítače se po převzetí služeb při selhání nepřipojí k žádné síti virtuálních počítačů. |  Pokud chcete nastavit mapování sítě pomocí VMM, budete se muset ujistit, že je správně nakonfigurovaná logická síť VMM a sítě virtuálních počítačů.
**Zprostředkovatelé a agenti** | Během nasazování nainstalujete na servery VMM zprostředkovatele Azure Site Recovery. Na servery Hyper-V v cloudech VMM nainstalujete agenta služby Azure Recovery Services. | Během nasazování nainstalujete na hostitelský server nebo cluster Hyper-V zprostředkovatele Azure Site Recovery a agenta služby Azure Recovery Services.| Během nasazování nainstalujete na servery VMM zprostředkovatele Azure Site Recovery. Na servery Hyper-V v cloudech VMM nainstalujete agenta služby Azure Recovery Services. | Zprostředkovatelé a agenti se připojují k Site Recovery přes internet pomocí šifrovaného připojení HTTPS. Pro připojení zprostředkovatele není nutné přidávat výjimky brány firewall ani vytvářet specifický proxy server.
**Připojení k Internetu** | Připojení k internetu je potřeba pouze na serverech VMM. | Připojení k internetu je potřeba pouze na hostitelských serverech Hyper-V. | Připojení k internetu je potřeba pouze na serverech VMM. | Virtuální počítače nemusí mít nic nainstalované a nepřipojují se přímo k internetu.



## <a name="protect-vmware-virtual-machines-or-physical-servers"></a>Ochrana virtuálních počítačů VMware nebo fyzických serverů

Pro ochranu virtuálních počítačů VMware nebo fyzických serverů s Windows/Linuxem máte na výběr z několika možností nasazení. Můžete je replikovat do Azure nebo do sekundárního datacentra. Každé nasazení má specifické požadavky.

**Požadavek** | **Replikace virtuálních počítačů VMware a fyzických serverů do Azure** | * **Replikace virtuálních počítačů VMware nebo fyzických serverů do sekundární lokality**  
---|---|---
**Primární lokalita** | **Procesový server:** Vyhrazený server s Windows (fyzický nebo virtuální) | **Procesový server:** Vyhrazený server s Windows (fyzický server nebo virtuální počítač VMware)<br/><br/>  
**Místní sekundární lokalita** | Není k dispozici | **Konfigurační server:** Vyhrazený server s Windows (fyzický nebo virtuální) <br/><br/> **Hlavní cílový server:** Vyhrazený server (fyzický nebo virtuální) Nakonfigurujte na něm Windows, pokud chcete chránit počítače s Windows, nebo Linux, pokud chcete chránit počítače s Linuxem.
**Azure** | **Předplatné:** Budete potřebovat předplatné služby Site Recovery. <br/><br/> **Účet úložiště:** Budete potřebovat účet úložiště s povolenou geografickou replikací. Účet musí být ve stejné oblasti jako trezor služby Site Recovery a musí být přidružený ke stejnému předplatnému. <br/><br/> **Konfigurační server:** Bude potřeba nastavit konfigurační server jako virtuální počítač Azure. <br/><br/> **Hlavní cílový server:** Bude potřeba nastavit hlavní cílový server jako virtuální počítač Azure. <br/><br/> Nakonfigurujte na něm Windows, pokud chcete chránit počítače s Windows, nebo Linux, pokud chcete chránit počítače s Linuxem.<br/><br/> **Virtuální síť Azure:** Budete potřebovat virtuální síť Azure, ve které se nasadí konfigurační server a hlavní cílový server. Musí být ve stejném předplatném a stejné oblasti jako trezor služby Azure Site Recovery. | Není k dispozici  
**Virtuální počítače/fyzické servery** | Alespoň jeden virtuální počítač VMware nebo fyzický server s Windows nebo Linuxem.<br/><br/>Během nasazení se na každý počítač nainstaluje služba Mobility.| Alespoň jeden virtuální počítač VMware nebo fyzický server s Windows nebo Linuxem.<br/><br/> Během nasazení se na každý počítač nainstaluje Unified Agent.




## <a name="azure-virtual-machine-requirements"></a>Požadavky na virtuální počítače Azure

Site Recovery můžete nasadit za účelem replikace virtuálních počítačů a fyzických serverů s jakýmkoli operačním systémem podporovaným v Azure. To zahrnuje většinu verzí systému Windows a Linux. Budete muset zajistit, aby virtuální počítače, které chcete chránit, splňovaly požadavky Azure.


## <a name="optimizing-your-deployment"></a>Optimalizace nasazení

Následující tipy vám pomůžou s optimalizací a škálováním nasazení.

- **Velikost svazku operačního systému:** Při replikaci virtuálního počítače do Azure musí být svazek operačního systému menší než 1 TB. Pokud máte více svazků, můžete je ručně přesunout na jiný disk před spuštěním nasazení.
- **Velikost datového disku:** Pokud replikujete do Azure, můžete mít ve virtuálním počítači až 32 datových disků, každý s maximální velikostí 1 TB. Efektivně můžete replikovat a přebírat služby při selhání virtuálního počítače o velikosti přibližně&32; TB.
- **Omezení plánu obnovení:** Site Recovery můžete škálovat na tisíce virtuálních počítačů. Plány obnovení jsou navrženy jako model pro aplikace, u kterých má dojít ke společnému převzetí služeb při selhání, proto omezujeme počet počítačů v plánu obnovení na 50.
- **Omezení služby Azure:** Každé předplatné Azure má přednastavena výchozí omezení počtu jader, cloudových služeb atd. Doporučujeme spuštěním testovacího převzetí služeb při selhání ověřit dostupnost prostředků ve vašem předplatném. Tato omezení můžete upravit prostřednictvím podpory Azure.
- **Plánování kapacity:** Plánování škálování a výkonu.
- **Šířka pásma replikace:** Pokud nemáte dostatečnou šířku pásma replikace, všimněte si, že:
    - **ExpressRoute:** Site Recovery spolupracuje s Azure ExpressRoute a nástroji pro optimalizaci sítě WAN, jako je Riverbed.
    - **Provoz replikace:** Site Recovery provádí inteligentní počáteční replikaci, ke které používá pouze bloky dat, a ne celý virtuální pevný disk. Během probíhající replikace se replikují jenom změny.
    - **Síťový provoz:** Síťový provoz používaný pro replikaci můžete řídit tak, že ve Windows QoS nastavíte zásadu na základě cílové IP adresy a portu.  Pokud navíc používáte k replikaci do Azure Site Recovery agenta Azure Backup, můžete pro takového agenta nakonfigurovat omezování.
- **Plánovaná doba obnovení (RTO):** Chcete-li změřit plánovanou dobu obnovení (RTO), kterou můžete očekávat při používání Site Recovery, doporučujeme spustit testovací převzetí služeb při selhání a pomocí zobrazení úloh Site Recovery analyzovat, kolik času je potřeba k dokončení operací. Pokud přebíráte služby při selhání do Azure, pro dosažení nejlepší plánované doby obnovení (RTO) doporučujeme automatizovat všechny ručně prováděné akce, a to pomocí integrace s Azure Automation a plány obnovení.
- **Cíl bodu obnovení (RPO):** Site Recovery při replikaci do Azure podporuje téměř synchronní cíl bodu obnovení (RPO). Předpokladem je dostatečná šířka pásma mezi vaším datacentrem a Azure.

