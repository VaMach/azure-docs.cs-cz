---
title: Jak funguje replikace Hyper-V do Azure v Site Recovery? | Dokumentace Microsoftu
description: "Tento článek obsahuje přehled fungování replikace Hyper-V v Azure Site Recovery."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: jwhit
editor: 
ms.assetid: c413efcd-d750-4b22-b34b-15bcaa03934a
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 06/14/2017
ms.author: raynew
ROBOTS: NOINDEX, NOFOLLOW
redirect_url: site-recovery-architecture-hyper-v-to-azure
ms.translationtype: Human Translation
ms.sourcegitcommit: d9dad6cff80c1f6ac206e7fa3184ce037900fc6b
ms.openlocfilehash: 9adf266c6a2ac00c3aaa34e2a29aefe34abe2871
ms.contentlocale: cs-cz
ms.lasthandoff: 04/18/2017

---
# <a name="how-does-hyper-v-replication-to-azure-work"></a>Jak funguje replikace Hyper-V do Azure?

Tento článek vám pomůže porozumět architektuře a pracovním postupům replikace Hyper-V do Azure pomocí služby [Azure Site Recovery](site-recovery-overview.md).

Jakékoli dotazy můžete publikovat na konci tohoto článku nebo na [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

Do Azure můžete replikovat následující:
- **Hyper-V s VMM:** Virtuální počítače v místních hostitelích Hyper-V spravovaných v cloudech System Center Virtual Machine Manageru (VMM). Na hostitelích může běžet jakýkoli [podporovaný operační systém](site-recovery-support-matrix-to-azure.md#support-for-datacenter-management-servers). Můžete replikovat virtuální počítače Hyper-V s jakýmkoli hostovaným operačním systémem, který [podporuje technologie Hyper-V a Azure](https://technet.microsoft.com/en-us/windows-server-docs/compute/hyper-v/supported-windows-guest-operating-systems-for-hyper-v-on-windows).
- **Hyper-V bez VMM:** Místní virtuální počítače v hostitelích Hyper-V, kteří nejsou spravováni v cloudech VMM. Na hostitelích může běžet jakýkoli [podporovaný operační systém](site-recovery-support-matrix-to-azure.md#support-for-replicated-machine-os-versions). Můžete replikovat virtuální počítače Hyper-V s jakýmkoli hostovaným operačním systémem, který [podporuje technologie Hyper-V a Azure](https://technet.microsoft.com/en-us/windows-server-docs/compute/hyper-v/supported-windows-guest-operating-systems-for-hyper-v-on-windows).


## <a name="architectural-components"></a>Komponenty architektury

**Oblast** | **Komponenta** | **Podrobnosti**
--- | --- | ---
**Azure** | Pro Azure potřebujete účet Microsoft Azure, účet úložiště Azure a síť Azure. | Účty úložiště a sítě můžou být klasické účty nebo účty Resource Manageru.<br/><br/> Replikovaná data jsou uložena v účtu úložiště a virtuální počítače Azure se z replikovaných dat vytvoří, jakmile dojde k převzetí místních služeb při selhání.<br/><br/> Virtuální počítače Azure se připojí k virtuální síti Azure po svém vytvoření.
**Server VMM** | Hostitelé Hyper-V umístění v cloudech VMM | Pokud jsou hostitelé Hyper-V spravovaní v cloudech VMM, zaregistrujete server VMM v trezoru služby Recovery Services.<br/><br/> Na serveru VMM nainstalujete zprostředkovatele Site Recovery, který bude orchestrovat replikaci pomocí Azure.<br/><br/> Ke konfiguraci mapování sítě potřebujete mít nastavenou logickou síť a síť virtuálních počítačů. Síť virtuálních počítačů musí být propojena na logickou síť, která je přidružena ke cloudu.
**Hostitel Hyper-V** | Servery Hyper-V je možné nasadit se serverem VMM nebo bez. | Pokud není k dispozici server VMM, zprostředkovatel Site Recovery se nainstaluje na hostitele, kde bude orchestrovat replikaci pomocí Site Recovery přes internet. Pokud je k dispozici server VMM, zprostředkovatel se nainstaluje na něj, a ne na hostitele.<br/><br/> Agent služby Recovery Services se nainstaluje na hostitele, kde bude zpracovávat replikaci dat.<br/><br/> Komunikace z poskytovatele i agenta je zabezpečená a šifrovaná. Šifrují se rovněž replikovaná data v úložišti Azure.
**Virtuální počítače Hyper-V** | Na hostitelském serveru Hyper-V potřebujete jeden nebo několik virtuálních počítačů. | Na virtuálních počítačích není výslovně potřeba nic instalovat.

## <a name="deployment-steps"></a>Kroky nasazení

1. **Azure:** Nastavíte komponenty Azure. Doporučujeme připravit účty úložiště a sítě ještě než začnete s nasazením Site Recovery.
2. **Trezor:** Vytvoříte trezor služby Recovery Services pro Site Recovery a nakonfigurujete nastavení trezoru, včetně konfigurace nastavení zdroje a cíle, nastavení zásad replikace a povolení replikace.
3. **Zdroj a cíl:**
    - **Hostitelé Hyper-V v cloudech VMM:** Jako součást nastavování zdroje si stáhnete a nainstalujete zprostředkovatele Azure Site Recovery na server VMM, a agenta Azure Recovery Services na každého hostitele Hyper-V. Zdrojem bude server VMM. Cílem je Azure.
    - Hostitelé Hyper-V bez VMM: Při zadávání nastavení zdroje si stáhnete a nainstalujete zprostředkovatele a agenta na každého hostitele Hyper-V. Během nasazení hostitele shromáždíte v lokalitě Hyper-V a tuto lokalitu zadáte jako zdroj. Cílem je Azure.

    ![Replikace Hyper-V nebo VMM do Azure](./media/site-recovery-components/arch-onprem-onprem-azure-vmm.png)
    ![Replikace lokality Hyper-V do Azure](./media/site-recovery-components/arch-onprem-azure-hypervsite.png)


4. **Zásady replikace:** Vytvoříte zásady replikace pro lokalitu Hyper-V nebo cloud VMM. Zásady se použijí pro všechny virtuální počítače na hostitelích v lokalitě nebo v cloudu.
5. **Povolení replikace:** Povolíte replikaci pro virtuální počítače Hyper-V. Počáteční replikace proběhne podle nastavených zásad replikace. Pak budou sledovány změny dat a bude probíhat rozdílová replikace do Azure. Sledované změny se pro jednotlivé položky ukládají do souboru .hrl.
6. **Testovací převzetí služeb při selhání:** Nakonec otestujete převzetí služeb při selhání, abyste měli jistotu, že všechno funguje podle očekávání.

Další informace o nasazení:
- [Začínáme s replikací virtuálních počítačů Hyper-V do Azure – s VMM](site-recovery-vmm-to-azure.md)
- [Začínáme s replikací virtuálních počítačů Hyper-V do Azure – bez VMM](site-recovery-hyper-v-site-to-azure.md)

## <a name="hyper-v-replication-workflow"></a>Pracovní postup replikace Hyper-V

### <a name="enable-protection"></a>Povolení ochrany

1. Po povolení ochrany pro virtuální počítače Hyper-V (na webu Azure Portal nebo místně) se spustí **Povolení ochrany**.
2. Úloha zkontroluje, zda počítač splňuje požadavky, a potom vyvolá metodu [CreateReplicationRelationship](https://msdn.microsoft.com/library/hh850036.aspx), která nastaví replikaci s nastavením, které jste nakonfigurovali.
3. Úloha spustí počáteční replikaci vyvoláním metody [StartReplication](https://msdn.microsoft.com/library/hh850303.aspx), která zahájí úplnou replikaci virtuálního počítače a odešle virtuální disky virtuálního počítače do Azure.
4. Úlohu můžete sledovat na kartě **Úlohy**.
        ![Seznam úloh](media/site-recovery-hyper-v-azure-architecture/image1.png)
        ![Podrobnosti povolení ochrany](media/site-recovery-hyper-v-azure-architecture/image2.png)

### <a name="initial-replication"></a>Počáteční replikace

1. Při aktivaci počáteční replikace se pořídí [snímek virtuálního počítače Hyper-V](https://technet.microsoft.com/library/dd560637.aspx).
2. Virtuální pevné disky se postupně replikují, dokud nejsou všechny zkopírované do Azure. To může nějakou dobu trvat v závislosti na velikosti virtuálního počítače a šířce pásma sítě. Pokud chcete optimalizovat využití sítě, přečtěte si téma [Správa využití šířky pásma sítě při ochraně místního prostředí do Azure](https://support.microsoft.com/kb/3056159).
3. Pokud dojde ke změnám na disku v době, kdy probíhá počáteční replikace, Hyper-V Replica Replication Tracker je zaznamená jako protokoly replikace Hyper-V (.hrl). Tyto soubory jsou umístěné ve stejné složce jako disky. Každý disk má přidružený soubor .hrl, který se odešle do sekundárního úložiště.
4. Soubory snímků a protokolů spotřebovávají prostředky disku v době, kdy probíhá počáteční replikace.
5. Po dokončení počáteční replikace se snímek virtuálního počítače odstraní. Rozdílové změny na disku v protokolu se synchronizují a sloučí s nadřazeným diskem.


### <a name="finalize-protection"></a>Dokončení ochrany

1. Po dokončení počáteční replikace nakonfiguruje úloha **Dokončení ochrany na virtuálním počítači** síťová a další postreplikační nastavení tak, aby byl virtuální počítač chráněn.
    ![Úloha dokončení ochrany](media/site-recovery-hyper-v-azure-architecture/image3.png)
2. Pokud replikujete do Azure, možná bude nutné upravit nastavení pro virtuální počítač tak, aby byl připraven k převzetí služeb při selhání. V tomto bodě můžete spustit test převzetí služeb při selhání a zkontrolovat, zda vše funguje podle očekávání.

### <a name="delta-replication"></a>Rozdílová replikace

1. Po počáteční replikaci se zahájí rozdílová synchronizace, a to v souladu s nastavením replikace.
2. Hyper-V Replica Replication Tracker zaznamenává změny na virtuálních pevných discích v souborech .hrl. Každý disk nakonfigurovaný pro replikaci má přidružený soubor .hrl. Tento protokol se po dokončení počáteční replikace odešle do účtu úložiště zákazníka. Při přenosu protokolu do Azure se změny na primárním disku zaznamenávají do jiného souboru protokolu ve stejném adresáři.
3. Během počáteční a rozdílové replikace můžete virtuální počítač monitorovat v zobrazení virtuálního počítače. [Další informace](site-recovery-monitoring-and-troubleshooting.md#monitor-replication-health-for-virtual-machines).  

### <a name="replication-synchronization"></a>Synchronizace replikace

1. Pokud rozdílová replikace selže a úplná replikace by byla náročná, pokud jde o šířku pásma nebo čas, pak se virtuální počítač označí pro resynchronizaci. Pokud například soubory .hrl dosáhnou 50 % velikosti disku, pak se u virtuálního počítače stanoví nutnost resynchronizace.
2.  Resynchronizace minimalizuje množství odesílaných dat tím, že počítá kontrolní součty zdrojových a cílových virtuálních počítačů a odesílá pouze rozdílová data. Resynchronizace využívá algoritmus vytváření bloků s pevnou velikostí, kdy jsou zdrojové a cílové soubory rozdělené do pevných bloků. Pro každý blok se vytvoří kontrolní součet a následným porovnáním kontrolních součtů se určí, které bloky ze zdroje je potřeba použít na cíl.
3. Po dokončení resynchronizace by měla pokračovat normální rozdílová replikace. Ve výchozím nastavení je automatické spuštění resynchronizace naplánováno na mimopracovní dobu, ale můžete ji u virtuálního počítače spustit i ručně. Například můžete pokračovat v resynchronizaci, pokud dojde k výpadku sítě nebo jinému výpadku. Pokud to chcete provést, vyberte virtuální počítač na portálu a klikněte na **Resynchronizovat**.

    ![Ruční resynchronizace](media/site-recovery-hyper-v-azure-architecture/image4.png)


### <a name="retries"></a>Opakování

Pokud dojde k chybě replikace, je předdefinován opakovaný pokus. Tuto logiku je možné rozdělit do dvou kategorií:

**Kategorie** | **Podrobnosti**
--- | ---
**Neopravitelné chyby** | Pokus se nebude opakovat. Stav virtuálního počítače bude **Kritický** a bude nutný zásah správce. Mezi tyto chyby patří přerušený řetězec virtuálních pevných disků, neplatný stav replikovaného virtuálního počítače, chyby ověřování sítě, chyby autorizace, chyby kvůli nenalezení virtuálního počítače (platí pro samostatné servery Hyper-V).
**Opravitelné chyby** | Pokusy se budou opakovat v každém intervalu replikace a pomocí exponenciální regrese se bude od počátku prvního pokusu zvětšovat interval opakování o 1, 2, 4, 8 a 10 minut. Pokud chyba přetrvává, bude se pokus opakovat každých 30 minut. Příkladem můžou být chyby sítě, chyby kvůli malému místu na disku nebo nedostatek paměti. |

## <a name="protection-and-recovery-lifecycle"></a>Životní cyklus ochrany a zotavení

![pracovní postup](./media/site-recovery-components/arch-hyperv-azure-workflow.png)

## <a name="next-steps"></a>Další kroky

- [Kontrola požadavků nasazení](site-recovery-prereq.md)
- Řešení potíží:
    - [Monitorování a řešení potíží s ochranou](site-recovery-monitoring-and-troubleshooting.md)
    - [Získat pomoc od podpory společnosti Microsoft](site-recovery-monitoring-and-troubleshooting.md#reach-out-for-microsoft-support)
    - [Běžné chyby a jejich řešení](site-recovery-monitoring-and-troubleshooting.md#common-azure-site-recovery-errors-and-their-resolutions)

