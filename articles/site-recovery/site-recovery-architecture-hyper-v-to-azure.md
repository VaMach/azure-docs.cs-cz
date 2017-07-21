---
title: Jak funguje replikace Hyper-V do Azure v Azure Site Recovery? | Dokumentace Microsoftu
description: "Tento článek obsahuje přehled komponent a architektury používané při replikaci místních virtuálních počítačů Hyper-V do Azure s využitím služby Azure Site Recovery."
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
ms.date: 06/23/2017
ms.author: raynew
ms.translationtype: Human Translation
ms.sourcegitcommit: 857267f46f6a2d545fc402ebf3a12f21c62ecd21
ms.openlocfilehash: 28f775afaf72b11eec0c22f755e4dbd6a485c895
ms.contentlocale: cs-cz
ms.lasthandoff: 06/28/2017

---


<a id="how-does-hyper-v-replication-to-azure-work-in-site-recovery" class="xliff"></a>

# Jak funguje replikace Hyper-V do Azure v Site Recovery?


Tento článek popisuje komponenty a procesy používané při replikaci místních virtuálních počítačů Hyper-V do Azure s využitím služby [Azure Site Recovery](site-recovery-overview.md).

Site Recovery umožňuje replikovat virtuální počítače Hyper-V na clustery Hyper-V a samostatné hostitele, které jsou spravované s využitím System Center Virtual Machine Manageru (VMM), nebo bez něj.

Jakékoli dotazy můžete publikovat na konci tohoto článku nebo na [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).



<a id="architectural-components" class="xliff"></a>

## Komponenty architektury

Při replikaci virtuálních počítačů Hyper-V do Azure se využívá řada komponent.

**Komponenta** | **Umístění** | **Podrobnosti**
--- | --- | ---
**Azure** | Pro Azure potřebujete účet Microsoft Azure, účet úložiště Azure a síť Azure. | Replikovaná data jsou uložena v účtu úložiště a virtuální počítače Azure se z replikovaných dat vytvoří, jakmile dojde k převzetí místních služeb při selhání.<br/><br/> Virtuální počítače Azure se připojí k virtuální síti Azure po svém vytvoření.
**Server VMM** | Hostitelé Hyper-V jsou umístění v cloudech VMM. | Pokud jsou hostitelé Hyper-V spravovaní v cloudech VMM, zaregistrujete server VMM v trezoru služby Recovery Services.<br/><br/> Na serveru VMM nainstalujete zprostředkovatele Site Recovery, který bude orchestrovat replikaci pomocí Azure.<br/><br/> Ke konfiguraci mapování sítě potřebujete mít nastavenou logickou síť a síť virtuálních počítačů. Síť virtuálních počítačů musí být propojena na logickou síť, která je přidružena ke cloudu.
**Hostitel Hyper-V** | Clustery a hostitele Hyper-V je možné nasadit se serverem VMM, nebo bez něj. | Pokud není k dispozici server VMM, zprostředkovatel Site Recovery se nainstaluje na hostitele, kde bude orchestrovat replikaci pomocí Site Recovery přes internet. Pokud je k dispozici server VMM, zprostředkovatel se nainstaluje na něj, a ne na hostitele.<br/><br/> Agent služby Recovery Services se nainstaluje na hostitele, kde bude zpracovávat replikaci dat.<br/><br/> Komunikace z poskytovatele i agenta je zabezpečená a šifrovaná. Šifrují se rovněž replikovaná data v úložišti Azure.
**Virtuální počítače Hyper-V** | Na hostitelském serveru Hyper-V potřebujete jeden nebo několik spuštěných virtuálních počítačů. | Na virtuálních počítačích není výslovně potřeba nic instalovat.

Další informace o požadavcích pro nasazení a pro jednotlivé komponenty najdete v [matici podpory](site-recovery-support-matrix-to-azure.md).

**Obr. 1: Replikace z lokality Hyper-V do Azure**

![Komponenty](./media/site-recovery-components/arch-onprem-azure-hypervsite.png)

**Obr. 2: Replikace z Hyper-V v cloudech VMM do Azure**

![Komponenty](./media/site-recovery-components/arch-onprem-onprem-azure-vmm.png)


<a id="replication-process" class="xliff"></a>

## Proces replikace

**Obr. 3: replikace a proces obnovení pro replikaci Hyper-V do Azure**

![pracovní postup](./media/site-recovery-components/arch-hyperv-azure-workflow.png)

<a id="enable-protection" class="xliff"></a>

### Povolení ochrany

1. Po povolení ochrany pro virtuální počítače Hyper-V (na webu Azure Portal nebo místně) se spustí **Povolení ochrany**.
2. Úloha zkontroluje, zda počítač splňuje požadavky, a potom vyvolá metodu [CreateReplicationRelationship](https://msdn.microsoft.com/library/hh850036.aspx), která nastaví replikaci s nastavením, které jste nakonfigurovali.
3. Úloha spustí počáteční replikaci vyvoláním metody [StartReplication](https://msdn.microsoft.com/library/hh850303.aspx), která zahájí úplnou replikaci virtuálního počítače a odešle virtuální disky virtuálního počítače do Azure.
4. Úlohu můžete sledovat na kartě **Úlohy**.
        ![Seznam úloh](media/site-recovery-hyper-v-azure-architecture/image1.png)
        ![Podrobnosti povolení ochrany](media/site-recovery-hyper-v-azure-architecture/image2.png)

<a id="replicate-the-initial-data" class="xliff"></a>

### Replikace počátečních dat

1. Při aktivaci počáteční replikace se pořídí [snímek virtuálního počítače Hyper-V](https://technet.microsoft.com/library/dd560637.aspx).
2. Virtuální pevné disky se postupně replikují, dokud nejsou všechny zkopírované do Azure. To může nějakou dobu trvat v závislosti na velikosti virtuálního počítače a šířce pásma sítě. Pokud chcete optimalizovat využití sítě, přečtěte si téma [Správa využití šířky pásma sítě při ochraně místního prostředí do Azure](https://support.microsoft.com/kb/3056159).
3. Pokud dojde ke změnám na disku v době, kdy probíhá počáteční replikace, Hyper-V Replica Replication Tracker je zaznamená jako protokoly replikace Hyper-V (.hrl). Tyto soubory jsou umístěné ve stejné složce jako disky. Každý disk má přidružený soubor .hrl, který se odešle do sekundárního úložiště.
4. Soubory snímků a protokolů spotřebovávají prostředky disku v době, kdy probíhá počáteční replikace.
5. Po dokončení počáteční replikace se snímek virtuálního počítače odstraní. Rozdílové změny na disku v protokolu se synchronizují a sloučí s nadřazeným diskem.


<a id="finalize-protection" class="xliff"></a>

### Dokončení ochrany

1. Po dokončení počáteční replikace nakonfiguruje úloha **Dokončení ochrany na virtuálním počítači** síťová a další postreplikační nastavení tak, aby byl virtuální počítač chráněn.
    ![Úloha dokončení ochrany](media/site-recovery-hyper-v-azure-architecture/image3.png)
2. Pokud replikujete do Azure, možná bude nutné upravit nastavení pro virtuální počítač tak, aby byl připraven k převzetí služeb při selhání. V tomto bodě můžete spustit test převzetí služeb při selhání a zkontrolovat, zda vše funguje podle očekávání.

<a id="replicate-the-delta" class="xliff"></a>

### Replikace rozdílů

1. Po počáteční replikaci se zahájí rozdílová synchronizace, a to v souladu s nastavením replikace.
2. Hyper-V Replica Replication Tracker zaznamenává změny na virtuálních pevných discích v souborech .hrl. Každý disk nakonfigurovaný pro replikaci má přidružený soubor .hrl. Tento protokol se po dokončení počáteční replikace odešle do účtu úložiště zákazníka. Při přenosu protokolu do Azure se změny na primárním disku zaznamenávají do jiného souboru protokolu ve stejném adresáři.
3. Během počáteční a rozdílové replikace můžete virtuální počítač monitorovat v zobrazení virtuálního počítače. [Další informace](site-recovery-monitoring-and-troubleshooting.md#monitor-replication-health-for-virtual-machines).  

<a id="synchronize-replication" class="xliff"></a>

### Synchronizace replikace

1. Pokud rozdílová replikace selže a úplná replikace by byla náročná, pokud jde o šířku pásma nebo čas, pak se virtuální počítač označí pro resynchronizaci. Pokud například soubory .hrl dosáhnou 50 % velikosti disku, pak se u virtuálního počítače stanoví nutnost resynchronizace.
2.  Resynchronizace minimalizuje množství odesílaných dat tím, že počítá kontrolní součty zdrojových a cílových virtuálních počítačů a odesílá pouze rozdílová data. Resynchronizace využívá algoritmus vytváření bloků s pevnou velikostí, kdy jsou zdrojové a cílové soubory rozdělené do pevných bloků. Pro každý blok se vytvoří kontrolní součet a následným porovnáním kontrolních součtů se určí, které bloky ze zdroje je potřeba použít na cíl.
3. Po dokončení resynchronizace by měla pokračovat normální rozdílová replikace. Ve výchozím nastavení je automatické spuštění resynchronizace naplánováno na mimopracovní dobu, ale můžete ji u virtuálního počítače spustit i ručně. Například můžete pokračovat v resynchronizaci, pokud dojde k výpadku sítě nebo jinému výpadku. Pokud to chcete provést, vyberte virtuální počítač na portálu a klikněte na **Resynchronizovat**.

    ![Ruční resynchronizace](media/site-recovery-hyper-v-azure-architecture/image4.png)


<a id="retry-logic" class="xliff"></a>

### Logika opakování

Pokud dojde k chybě replikace, je předdefinován opakovaný pokus. Tuto logiku je možné rozdělit do dvou kategorií:

**Kategorie** | **Podrobnosti**
--- | ---
**Neopravitelné chyby** | Pokus se nebude opakovat. Stav virtuálního počítače bude **Kritický** a bude nutný zásah správce. Mezi tyto chyby patří přerušený řetězec virtuálních pevných disků, neplatný stav replikovaného virtuálního počítače, chyby ověřování sítě, chyby autorizace, chyby kvůli nenalezení virtuálního počítače (platí pro samostatné servery Hyper-V).
**Opravitelné chyby** | Pokusy se budou opakovat v každém intervalu replikace a pomocí exponenciální regrese se bude od počátku prvního pokusu zvětšovat interval opakování o 1, 2, 4, 8 a 10 minut. Pokud chyba přetrvává, bude se pokus opakovat každých 30 minut. Příkladem můžou být chyby sítě, chyby kvůli malému místu na disku nebo nedostatek paměti. |



<a id="failover-and-failback-process" class="xliff"></a>

## Proces převzetí služeb při selhání a navrácení služeb po obnovení

1. Můžete spustit plánované nebo neplánované [převzetí služeb při selhání](site-recovery-failover.md) z místních virtuálních počítačů Hyper-V do Azure. Pokud spustíte plánovanou operaci, dojde k ukončení zdrojových virtuálních počítačů, aby se zcela předešlo možné ztrátě dat.
2. Můžete převzít službu při selhání jednoho počítače nebo vytvořit [plány zotavení](site-recovery-create-recovery-plans.md) a orchestrovat převzetí služeb více počítačů.
4. Po převzetí služeb při selhání byste v Azure měli vidět vytvořené replikované virtuální počítače. Virtuálnímu počítači můžete přiřadit veřejnou IP adresu, když je to třeba.
5. Po potvrzení procesu převzetí služeb můžete začít používat úlohu na replikovaném virtuálním počítači Azure.
6. Až bude vaše místní lokalita opět dostupná, můžete [navrátit služby po obnovení](site-recovery-failback-from-azure-to-hyper-v.md). Zahájíte plánované převzetí služeb při selhání z Azure do primární lokality. Pro plánované převzetí služeb při selhání můžete vybrat navrácení služeb po obnovení do stejného virtuálního počítače nebo do alternativního umístění a synchronizovat změny mezi Azure a místním systémem, aby nedošlo ke ztrátě dat. Jakmile budou místní virtuální počítače vytvořené, můžete převzetí služeb při selhání potvrdit.




<a id="next-steps" class="xliff"></a>

## Další kroky

Kontrola [matice podpory](site-recovery-support-matrix-to-azure.md)

