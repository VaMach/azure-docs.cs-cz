---
title: "Kontrola architektury pro replikaci Hyper-V (bez nástroje System Center VMM) do Azure s využitím služby Azure Site Recovery | Dokumentace Microsoftu"
description: "Tento článek obsahuje přehled komponent a architektury používané při replikaci místních virtuálních počítačů Hyper-V (bez nástroje VMM) do Azure s využitím služby Azure Site Recovery."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: c413efcd-d750-4b22-b34b-15bcaa03934a
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 06/22/2017
ms.author: raynew
ms.translationtype: Human Translation
ms.sourcegitcommit: 31ecec607c78da2253fcf16b3638cc716ba3ab89
ms.openlocfilehash: 044dfe686de36c56703d126db94d0b4382b85886
ms.contentlocale: cs-cz
ms.lasthandoff: 06/23/2017

---


# Krok 1: Kontrola architektury pro replikaci Hyper-V do Azure
<a id="step-1-review-the-architecture-for-hyper-v-replication-to-azure" class="xliff"></a>


Tento článek popisuje komponenty a procesy používané při replikaci místních virtuálních počítačů Hyper-V (které nejsou spravované nástrojem System Center VMM) do Azure s využitím služby [Azure Site Recovery](site-recovery-overview.md).

Jakékoli dotazy můžete publikovat na konci tohoto článku nebo na [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).



## Komponenty architektury
<a id="architectural-components" class="xliff"></a>

Při replikaci virtuálních počítačů Hyper-V do Azure bez nástroje VMM se využívá řada komponent.

**Komponenta** | **Umístění** | **Podrobnosti**
--- | --- | ---
**Azure** | Pro Azure potřebujete účet Microsoft Azure, účet úložiště Azure a síť Azure. | Replikovaná data jsou uložena v účtu úložiště a virtuální počítače Azure se z replikovaných dat vytvoří, jakmile dojde k převzetí místních služeb při selhání.<br/><br/> Virtuální počítače Azure se připojí k virtuální síti Azure po svém vytvoření.
**Hyper-V** | Hostitelé a clustery Hyper-V se shromažďují do lokalit Hyper-V. Na každém hostiteli Hyper-V se nainstaluje zprostředkovatel Azure Site Recovery a agent Recovery Services. | Zprostředkovatel orchestruje replikaci pomocí služby Site Recovery přes internet. Agent Recovery Services se stará o replikaci dat.<br/><br/> Komunikace z poskytovatele i agenta je zabezpečená a šifrovaná. Šifrují se rovněž replikovaná data v úložišti Azure.
**Virtuální počítače Hyper-V** | Na hostitelském serveru Hyper-V potřebujete jeden nebo několik spuštěných virtuálních počítačů. | Na virtuálních počítačích není výslovně potřeba nic instalovat.

Další informace o požadavcích pro nasazení a pro jednotlivé komponenty najdete v [matici podpory](site-recovery-support-matrix-to-azure.md).

**Obrázek 1: Replikace z lokality Hyper-V do Azure**

![Komponenty](./media/hyper-v-site-walkthrough-architecture/arch-onprem-azure-hypervsite.png)


## Proces replikace
<a id="replication-process" class="xliff"></a>

**Obrázek 2: Replikace a proces obnovení pro replikaci Hyper-V do Azure**

![pracovní postup](./media/hyper-v-site-walkthrough-architecture/arch-hyperv-azure-workflow.png)

### Povolení ochrany
<a id="enable-protection" class="xliff"></a>

1. Po povolení ochrany pro virtuální počítače Hyper-V (na webu Azure Portal nebo místně) se spustí **Povolení ochrany**.
2. Úloha zkontroluje, zda počítač splňuje požadavky, a potom vyvolá metodu [CreateReplicationRelationship](https://msdn.microsoft.com/library/hh850036.aspx), která nastaví replikaci s nastavením, které jste nakonfigurovali.
3. Úloha spustí počáteční replikaci vyvoláním metody [StartReplication](https://msdn.microsoft.com/library/hh850303.aspx), která zahájí úplnou replikaci virtuálního počítače a odešle virtuální disky virtuálního počítače do Azure.
4. Úlohu můžete sledovat na kartě **Úlohy**.
 
### Replikace počátečních dat
<a id="replicate-the-initial-data" class="xliff"></a>

1. Při aktivaci počáteční replikace se pořídí [snímek virtuálního počítače Hyper-V](https://technet.microsoft.com/library/dd560637.aspx).
2. Virtuální pevné disky se postupně replikují, dokud nejsou všechny zkopírované do Azure. To může nějakou dobu trvat v závislosti na velikosti virtuálního počítače a šířce pásma sítě. Pokud chcete optimalizovat využití sítě, přečtěte si téma [Správa využití šířky pásma sítě při ochraně místního prostředí do Azure](https://support.microsoft.com/kb/3056159).
3. Pokud dojde ke změnám na disku v době, kdy probíhá počáteční replikace, Hyper-V Replica Replication Tracker je zaznamená jako protokoly replikace Hyper-V (.hrl). Tyto soubory jsou umístěné ve stejné složce jako disky. Každý disk má přidružený soubor .hrl, který se odešle do sekundárního úložiště.
4. Soubory snímků a protokolů spotřebovávají prostředky disku v době, kdy probíhá počáteční replikace.
5. Po dokončení počáteční replikace se snímek virtuálního počítače odstraní. Rozdílové změny na disku v protokolu se synchronizují a sloučí s nadřazeným diskem.


### Dokončení ochrany
<a id="finalize-protection" class="xliff"></a>

1. Po dokončení počáteční replikace nakonfiguruje úloha **Dokončení ochrany na virtuálním počítači** síťová a další postreplikační nastavení tak, aby byl virtuální počítač chráněn.
2. Pokud replikujete do Azure, možná bude nutné upravit nastavení pro virtuální počítač tak, aby byl připraven k převzetí služeb při selhání. V tomto bodě můžete spustit test převzetí služeb při selhání a zkontrolovat, zda vše funguje podle očekávání.

### Replikace rozdílů
<a id="replicate-the-delta" class="xliff"></a>

1. Po počáteční replikaci se zahájí rozdílová synchronizace, a to v souladu s nastavením replikace.
2. Hyper-V Replica Replication Tracker zaznamenává změny na virtuálních pevných discích v souborech .hrl. Každý disk nakonfigurovaný pro replikaci má přidružený soubor .hrl. Tento protokol se po dokončení počáteční replikace odešle do účtu úložiště zákazníka. Při přenosu protokolu do Azure se změny na primárním disku zaznamenávají do jiného souboru protokolu ve stejném adresáři.
3. Během počáteční a rozdílové replikace můžete virtuální počítač monitorovat v zobrazení virtuálního počítače. [Další informace](site-recovery-monitoring-and-troubleshooting.md#monitor-replication-health-for-virtual-machines).  

### Synchronizace replikace
<a id="synchronize-replication" class="xliff"></a>

1. Pokud rozdílová replikace selže a úplná replikace by byla náročná, pokud jde o šířku pásma nebo čas, pak se virtuální počítač označí pro resynchronizaci. Pokud například soubory .hrl dosáhnou 50 % velikosti disku, pak se u virtuálního počítače stanoví nutnost resynchronizace.
2.  Resynchronizace minimalizuje množství odesílaných dat tím, že počítá kontrolní součty zdrojových a cílových virtuálních počítačů a odesílá pouze rozdílová data. Resynchronizace využívá algoritmus vytváření bloků s pevnou velikostí, kdy jsou zdrojové a cílové soubory rozdělené do pevných bloků. Pro každý blok se vytvoří kontrolní součet a následným porovnáním kontrolních součtů se určí, které bloky ze zdroje je potřeba použít na cíl.
3. Po dokončení resynchronizace by měla pokračovat normální rozdílová replikace. Ve výchozím nastavení je automatické spuštění resynchronizace naplánováno na mimopracovní dobu, ale můžete ji u virtuálního počítače spustit i ručně. Například můžete pokračovat v resynchronizaci, pokud dojde k výpadku sítě nebo jinému výpadku. Pokud to chcete provést, vyberte virtuální počítač na portálu a klikněte na **Resynchronizovat**.

    ![Ruční resynchronizace](media/hyper-v-site-walkthrough-architecture/image4.png)


### Logika opakování
<a id="retry-logic" class="xliff"></a>

Pokud dojde k chybě replikace, je předdefinován opakovaný pokus. Tuto logiku je možné rozdělit do dvou kategorií:

**Kategorie** | **Podrobnosti**
--- | ---
**Neopravitelné chyby** | Pokus se nebude opakovat. Stav virtuálního počítače bude **Kritický** a bude nutný zásah správce. Mezi tyto chyby patří přerušený řetězec virtuálních pevných disků, neplatný stav replikovaného virtuálního počítače, chyby ověřování sítě, chyby autorizace, chyby kvůli nenalezení virtuálního počítače (platí pro samostatné servery Hyper-V).
**Opravitelné chyby** | Pokusy se budou opakovat v každém intervalu replikace a pomocí exponenciální regrese se bude od počátku prvního pokusu zvětšovat interval opakování o 1, 2, 4, 8 a 10 minut. Pokud chyba přetrvává, bude se pokus opakovat každých 30 minut. Příkladem můžou být chyby sítě, chyby kvůli malému místu na disku nebo nedostatek paměti. |



## Proces převzetí služeb při selhání a navrácení služeb po obnovení
<a id="failover-and-failback-process" class="xliff"></a>

1. Můžete spustit plánované nebo neplánované [převzetí služeb při selhání](site-recovery-failover.md) z místních virtuálních počítačů Hyper-V do Azure. Pokud spustíte plánovanou operaci, dojde k ukončení zdrojových virtuálních počítačů, aby se zcela předešlo možné ztrátě dat.
2. Můžete převzít službu při selhání jednoho počítače nebo vytvořit [plány zotavení](site-recovery-create-recovery-plans.md) a orchestrovat převzetí služeb více počítačů.
4. Po převzetí služeb při selhání byste v Azure měli vidět vytvořené replikované virtuální počítače. Virtuálnímu počítači můžete přiřadit veřejnou IP adresu, když je to třeba.
5. Po potvrzení procesu převzetí služeb můžete začít používat úlohu na replikovaném virtuálním počítači Azure.
6. Až bude vaše místní lokalita opět dostupná, můžete [navrátit služby po obnovení](site-recovery-failback-from-azure-to-hyper-v.md). Zahájíte plánované převzetí služeb při selhání z Azure do primární lokality. Pro plánované převzetí služeb při selhání můžete vybrat navrácení služeb po obnovení do stejného virtuálního počítače nebo do alternativního umístění a synchronizovat změny mezi Azure a místním systémem, aby nedošlo ke ztrátě dat. Jakmile budou místní virtuální počítače vytvořené, můžete převzetí služeb při selhání potvrdit.




## Další kroky
<a id="next-steps" class="xliff"></a>

Přejděte na [Krok 2: Kontrola požadavků pro nasazení](hyper-v-site-walkthrough-prerequisites.md)

