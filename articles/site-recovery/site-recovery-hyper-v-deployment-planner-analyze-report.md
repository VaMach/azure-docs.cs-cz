---
title: "Plánovač nasazení služby Azure Site Recovery pro nasazení Hyper-V do Azure| Dokumentace Microsoftu"
description: "Tento článek popisuje analýzu vygenerované sestavy Plánovače nasazení služby Azure Site Recovery pro scénář nasazení Hyper-V do Azure."
services: site-recovery
documentationcenter: 
author: nsoneji
manager: garavd
editor: 
ms.assetid: 
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 12/02/2017
ms.author: nisoneji
ms.openlocfilehash: 714c2074f643d2b168c054c5af467b550f57daba
ms.sourcegitcommit: a48e503fce6d51c7915dd23b4de14a91dd0337d8
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/05/2017
---
# <a name="analyze-the-azure-site-recovery-deployment-planner-report"></a>Analýza sestavy Plánovače nasazení služby Azure Site Recovery
Vygenerovaná sestava aplikace Microsoft Excel obsahuje následující listy:

## <a name="on-premises-summary"></a>On-premises summary (Přehled místního prostředí)
List On-premises summary obsahuje přehled profilovaného prostředí Hyper-V. ![On-premises summary](media/site-recovery-hyper-v-deployment-planner-analyze-report/on-premises-summary-h2a.png)

**Start Date** a **End Date**: Počáteční a koncové datum dat profilace zahrnutých do generování sestavy. Ve výchozím nastavení je počátečním datem datum zahájení profilace a koncovým datem je datum zastavení profilace. Můžou to být hodnoty StartDate a EndDate, pokud se sestava generuje s použitím těchto parametrů.

**Total number of profiling days:** Celkový počet dnů profilace mezi počátečním a koncovým datem, pro které se generuje sestava.

**Number of compatible virtual machines:** Celkový počet kompatibilních virtuálních počítačů, pro které se počítá požadovaná šířka pásma sítě, požadovaný počet účtů úložiště a počet jader Microsoft Azure.

**Total number of disks across all compatible virtual machines:** Celkový počet disků ve všech kompatibilních virtuálních počítačích.

**Average number of disks per compatible virtual machine:** Průměrný počet disků ve všech kompatibilních virtuálních počítačích.

**Average disk size (GB):** Průměrná velikost disků ve všech kompatibilních virtuálních počítačích.

**Desired RPO (minutes):** Buď výchozí cíl bodu obnovení, nebo hodnota předaná v parametru DesiredRPO při generování sestavy, sloužící k odhadu požadované šířky pásma.

**Desired bandwidth (Mbps):** Hodnota, kterou jste předali v parametru Bandwidth při generování sestavy, sloužící k odhadu dosažitelného cíle bodu obnovení.

**Observed typical data churn per day (GB):** Průměrná denní četnost změn dat vypozorovaná během všech dnů profilace.

## <a name="recommendations"></a>Doporučení  
List sestavy nasazení Hyper-V do Azure s doporučeními obsahuje následující podrobné údaje pro vybraný požadovaný cíl bodu obnovení:

![Doporučení pro nasazení Hyper-V do Azure](media/site-recovery-hyper-v-deployment-planner-analyze-report/Recommendations-h2a.png)

### <a name="profile-data"></a>Data profilu
![Data profilu](media/site-recovery-hyper-v-deployment-planner-analyze-report/profile-data-h2a.png)

**Profiled data period:** Doba, po kterou byla profilace spuštěná. Ve výchozím nastavení tento nástroj zahrnuje do výpočtu všechna profilovaná data. Pokud jste při generování sestavy použili možnost StartDate a EndDate, sestava se generuje pro konkrétní období. 

**Number of Hyper-V servers profiled:** Počet serverů Hyper-V, pro jehož virtuální počítače se sestava generuje. Kliknutím na toto číslo zobrazíte název serverů Hyper-V. Otevře se list s požadavky na místní úložiště, ve kterém jsou uvedené všechny servery společně se svými požadavky na úložiště.    

**Desired RPO:** Požadovaný cíl bodu obnovení pro vaše nasazení. Ve výchozím nastavení se požadovaná šířka pásma sítě počítá pro hodnoty cíle bodu obnovení 15, 30 a 60 minut. V závislosti na výběru se na listu aktualizují ovlivněné hodnoty. Pokud jste při generování sestavy použili parametr DesiredRPOinMin, tato hodnota se zobrazí jako výsledek v poli Desired RPO.

### <a name="profiling-overview"></a>Přehled profilace
![Přehled profilace](media/site-recovery-hyper-v-deployment-planner-analyze-report/profiling-overview-h2a.png)

**Total Profiled Virtual Machines:** Celkový počet virtuálních počítačů, jejichž profilovaná data jsou k dispozici. Pokud soubor VMListFile obsahuje názvy virtuálních počítačů, které nebyly profilované, nebude se na takové virtuální počítače brát ohled při generování sestav a nezahrnou se do celkového počtu profilovaných virtuálních počítačů.

**Compatible Virtual Machines:** Počet virtuálních počítačů, které lze chránit v Azure pomocí Azure Site Recovery. Je to celkový počet kompatibilních virtuálních počítačů, pro které se počítá požadovaná šířka pásma sítě, požadovaný počet účtů úložiště a počet jader Azure. Podrobnosti o každém kompatibilním virtuálním počítači jsou dostupné v části Compatible VMs.

**Incompatible Virtual Machines:** Počet profilovaných virtuálních počítačů, které jsou nekompatibilní s ochranou pomocí Azure Site Recovery. Důvody nekompatibility jsou uvedené v části Incompatible VMs. Pokud soubor VMListFile obsahuje názvy virtuálních počítačů, které nebyly profilované, nezahrnou se do celkového počtu nekompatibilních virtuálních počítačů. Takové virtuální počítače jsou uvedené jako „Data not found“ (Data nenalezena) na konci části Incompatible VMs.

**Desired RPO:** Požadovaný cíl bodu obnovení v minutách. Sestava se generuje pro tři hodnoty cíle bodu obnovení: 15 (výchozí), 30 a 60 minut. Doporučení šířky pásma v sestavě se mění v závislosti na výběru možnosti v rozevíracím seznamu Desired RPO v pravé horní části listu. Pokud jste sestavu vygenerovali s použitím parametru -DesiredRPO s vlastní hodnotou, v rozevíracím seznamu Desired RPO se tato vlastní hodnota zobrazí jako výchozí možnost.

### <a name="required-network-bandwidth-mbps"></a>Požadovaná šířka pásma sítě (Mb/s)
![Požadovaná šířka pásma sítě](media/site-recovery-hyper-v-deployment-planner-analyze-report/required-network-bandwidth-h2a.png)

**To meet RPO 100 percent of the time:** Doporučená šířka pásma v Mb/s, kterou je potřeba přidělit pro splnění požadovaného cíle bodu obnovení po 100 % času. Šířka pásma musí být vyhrazená pro zajištění stálé rozdílové replikace všech kompatibilních virtuálních počítačů, aby se předešlo jakémukoli narušení cíle bodu obnovení.

**To meet RPO 90 percent of the time** (Pro splnění cíle bodu obnovení 90 % času): Pokud kvůli cenám širokopásmového připojení nebo z jiného důvodu nemůžete nastavit šířku pásma potřebnou ke splnění požadovaného cíle bodu obnovení 100 % času, můžete se rozhodnout pro nastavení menší šířky pásma, která může splňovat požadovaný cíl bodu obnovení 90 % času. Abyste porozuměli dopadům nastavení menší šířky pásma, sestava poskytuje analýzu „co kdyby“ očekávaného počtu a trvání narušení cíle bodu obnovení.

**Achieved Throughput:** Propustnost ze serveru, na kterém jste spustili příkaz GetThroughput, do oblasti Azure, ve které je umístěný účet úložiště. Tato hodnota propustnosti označuje odhadovanou úroveň propustnosti, které můžete dosáhnout při ochraně kompatibilních virtuálních počítačů pomocí Azure Site Recovery za předpokladu, že charakteristiky sítě a úložiště serveru Hyper-V budou stejné jako u serveru, ze kterého jste nástroj spustili.

Pro všechna podniková nasazení Azure Site Recovery doporučujeme použít [ExpressRoute](https://aka.ms/expressroute).

### <a name="required-storage-accounts"></a>Požadované účty úložiště
Následující graf znázorňuje celkový počet účtů služby Storage (úrovně Standard a Premium) požadovaných k ochraně všech kompatibilních virtuálních počítačů. Informace o tom, jaký účet úložiště použít pro jednotlivé virtuální počítače, najdete v části VM-storage placement.

![Požadované účty úložiště](media/site-recovery-hyper-v-deployment-planner-analyze-report/required-storage-accounts-h2a.png)

### <a name="required-number-of-azure-cores"></a>Požadovaný počet jader Azure
Tento výsledek je celkový počet jader, která se mají nastavit před převzetím služeb při selhání nebo testovacím převzetím služeb při selhání všech kompatibilních virtuálních počítačů. Pokud v rámci předplatného není k dispozici dostatečný počet jader, Azure Site Recovery během testovacího převzetí služeb při selhání nebo převzetí služeb při selhání nemůže vytvořit virtuální počítače.
![Požadovaný počet jader Azure](media/site-recovery-hyper-v-deployment-planner-analyze-report/required-number-of-azure-cores-h2a.png)


### <a name="additional-on-premises-storage-requirement"></a>Požadavek na další místní úložiště

Celkové volné úložiště vyžadované pro servery Hyper-V k zajištění úspěšné počáteční replikace a rozdílové replikace, které zajistí, že replikace virtuálních počítačů nebude způsobovat nežádoucí výpadky pro produkční aplikace. Podrobnosti o požadavcích jednotlivých svazků jsou uvedené v [požadavcích na místní úložiště](#on-premises-storage-requirement). 

Informace o tom, proč se k replikaci vyžaduje volné místo, najdete v části [Požadavky na volné místo](#why-do-i-need-free-space-on-the-hyper-v-server-for-the-replication).

![Požadavky na místní úložiště a frekvence kopírování](media/site-recovery-hyper-v-deployment-planner-analyze-report/on-premises-storage-and-copy-frequency-h2a.png)

### <a name="maximum-copy-frequency"></a>Maximální frekvence kopírování
Pro replikaci musí být nastavena doporučená maximální frekvence kopírování, aby se zajistil požadovaný cíl bodu obnovení. Výchozí hodnota je 5 minut. Pro zajištění lepšího cíle bodu obnovení je možné nastavit frekvenci kopírování na 30 sekund.

### <a name="what-if-analysis"></a>Analýza „co kdyby“
![Analýza „co kdyby“](media/site-recovery-hyper-v-deployment-planner-analyze-report/what-if-analysis-h2a.png) Tato analýza ukazuje, ke kolika narušením by mohlo dojít během období profilace, pokud nastavíte menší šířku pásma pro splnění požadovaného cíle bodu obnovení pouze 90 % času. Každý den může dojít k jednomu nebo několika narušením cíle bodu obnovení. Graf ukazuje pro každý den špičku cíle bodu obnovení. Na základě této analýzy se můžete rozhodnout, jestli je se zadanou menší šířkou pásma přijatelný počet narušení cíle bodu obnovení ve všech dnech a denní dosažená špička cíle bodu obnovení. Pokud je to přijatelné, můžete pro replikaci přidělit menší šířku pásma, jinak podle návrhu přidělte větší šířku pásma pro splnění požadovaného cíle bodu obnovení 100 % času. 

### <a name="recommendation-for-successful-initial-replication"></a>Doporučení pro úspěšnou počáteční replikaci
V této části doporučujeme počet dávek, ve kterých se virtuální počítače mají chránit, a minimální šířku pásma vyžadovanou k úspěšnému dokončení počáteční replikace (IR). 

![Rozdělení počáteční replikace do dávek](media/site-recovery-hyper-v-deployment-planner-analyze-report/ir-batching-h2a.png)

Virtuální počítače musí být chráněné ve stanoveném pořadí dávek. Každá dávka má konkrétní seznam virtuálních počítačů. Virtuální počítače dávky 1 musí být chráněny před dávkou 2. Virtuální počítače dávky 2 musí být chráněny před virtuálními počítači dávky 3 atd. Po dokončení počáteční replikace virtuálních počítačů dávky 1 můžete povolit replikaci pro virtuální počítače dávky 2. Podobně po dokončení počáteční replikace virtuálních počítačů dávky 2 můžete povolit replikaci pro virtuální počítače dávky 3 atd. Pokud se pořadí dávek nedodrží, může se stát, že pro počáteční replikaci nebude k dispozici dostatečná šířka pásma pro virtuální počítače, které jsou chráněné později. Výsledkem potom je, že se buď počáteční replikace virtuálních počítačů nikdy nedokončí, nebo že některé chráněné virtuální počítače budou v režimu resynchronizace. Rozdělení počáteční replikace do dávek pro list vybraného cíle bodu obnovení obsahuje podrobné informace o tom, které virtuální počítače by měly být součástí jednotlivých dávek.

Uvedený graf ukazuje distribuci šířky pásma pro počáteční a rozdílovou replikaci napříč dávkami v daném pořadí dávek. Když chráníte virtuální počítače první dávky, je pro počáteční replikaci k dispozici plná šířka pásma. Po dokončení počáteční replikace pro první dávku se bude část šířky pásma vyžadovat pro rozdílovou replikaci. Zbývající šířka pásma bude k dispozici pro počáteční replikaci virtuálních počítačů druhé dávky. Panel dávky 2 ukazuje požadovanou šířku pásma pro rozdílovou replikaci virtuálních počítačů dávky 1 a šířku pásma dostupnou pro počáteční replikaci virtuálních počítačů dávky 2. Podobně panel dávky 3 ukazuje požadovanou šířku pásma pro rozdílovou replikaci předchozích dávek (virtuální počítače dávky 1 a dávky 2) a šířku pásma dostupnou pro počáteční replikaci virtuálních počítačů dávky 3.  Jakmile se dokončí počáteční replikace všech dávek, poslední panel zobrazuje šířku pásma potřebnou pro rozdílovou replikaci pro všechny chráněné virtuální počítače. 

**Proč je nutné rozdělení počáteční replikace do dávek?**
Čas dokončení počáteční replikace závisí na velikosti disku virtuálního počítače, využitém místu na disku a dostupné propustnosti sítě. Podrobné informace jsou k dispozici v rozdělení počáteční replikace do dávek na listu vybraného cíle bodu obnovení.

### <a name="cost-estimation"></a>Odhad nákladů
Graf zobrazuje přehled odhadovaných celkových nákladů na zotavení po havárii do Azure ve zvolené cílové oblasti a měnu, kterou jste zadali pro generování sestav.
![Souhrn odhadu nákladů](media/site-recovery-hyper-v-deployment-planner-analyze-report/cost-estimation-summary-h2a.png) Tento souhrn pomáhá porozumět nákladům, které budete muset platit za úložiště, výpočetní funkce, sítě a licence při ochraně všech kompatibilní virtuálních počítačů v Azure pomocí Azure Site Recovery. Náklady se počítají pro kompatibilní virtuální počítače, a ne pro všechny profilované virtuální počítače.  
 
Náklady můžete zobrazit po měsících nebo letech. Další informace o [podporovaných cílových oblastech](./site-recovery-hyper-v-deployment-planner-cost-estimation.md#supported-target-regions) a [podporovaných měnách](./site-recovery-hyper-v-deployment-planner-cost-estimation.md#supported-currencies)

**Náklady podle komponent** Celkové náklady na zotavení po havárii se dělí do čtyř komponent: výpočetní služby, úložiště, síť a licenční náklady na Azure Site Recovery. Náklady se počítají na základě spotřeby, ke které dojde během replikace a v průběhu nácviku zotavení po havárii pro výpočetní funkce, úložiště (Premium a Standard), síť ExpressRoute nebo VPN, která je nakonfigurovaná mezi místní lokalitou a Azure, a licence Azure Site Recovery.

**Náklady podle stavu** Celkové náklady na zotavení po havárii jsou rozdělené do kategorií na základě dvou různých stavů – replikace a nácvik zotavení po havárii. 

**Náklady na replikaci:** Náklady, které vzniknou během replikace. Zahrnují náklady na úložiště, síť a licence Azure Site Recovery. 

**Náklady na nácvik zotavení po havárii:** Náklady, které vzniknou během testovacího převzetí služeb při selhání. Během testovacího převzetí služeb při selhání Azure Site Recovery rozjede virtuální počítače. Náklady na nácvik zotavení po havárii zahrnují náklady na výpočetní funkce a úložiště spuštěných virtuálních počítačů. 

**Náklady na úložiště Azure za měsíc a rok:** Celkové náklady na úložiště, které vzniknou pro úložiště úrovně Standard a Premium při replikaci a nácviku zotavení po havárii.
Podrobnou analýzu nákladů na virtuální počítač najdete na listu s [odhadem nákladů](site-recovery-hyper-v-deployment-planner-cost-estimation.md).

### <a name="growth-factor-and-percentile-values-used"></a>Použitý faktor růstu a hodnoty percentilu
Tato oblast v dolní části listu ukazuje hodnotu percentilu použitou pro všechny čítače výkonu profilovaných virtuálních počítačů (výchozí je 95. percentil) a faktor růstu (výchozí hodnota je 30 %), který se používá ve všech výpočtech.

![Použitý faktor růstu a hodnoty percentilu](media/site-recovery-hyper-v-deployment-planner-run/growth-factor-max-percentile-value.png)

## <a name="recommendations-with-available-bandwidth-as-input"></a>Doporučení s možností zadat dostupnou šířku pásma jako vstup
![Přehled profilace na základě šířky pásma](media/site-recovery-hyper-v-deployment-planner-analyze-report/profiling-overview-bandwidth-input-h2a.png)

Může nastat situace, kdy víte, že pro účely replikace Azure Site Recovery nemůžete nastavit šířku pásma větší než x Mb/s. Tento nástroj umožňuje zadat dostupnou šířku pásma (pomocí parametru -Bandwidth během generování sestavy) a zjistit dosažitelný cíl bodu obnovení v minutách. Na základě této hodnoty dosažitelného cíle bodu obnovení se můžete rozhodnout, jestli potřebujete zřídit větší šířku pásma, nebo vám vyhovuje řešení zotavení po havárii s tímto cílem bodu obnovení.

![Dosažitelný cíl bodu obnovení](media/site-recovery-hyper-v-deployment-planner-analyze-report/achivable-rpo-h2a.PNG)

## <a name="vm-storage-placement-recommendation"></a>Doporučení pro umístění úložiště virtuálního počítače 

![Umístění virtuálních počítačů ve službě Storage](media/site-recovery-hyper-v-deployment-planner-analyze-report/vm-storage-placement-h2a.png)

**Disk Storage Type:** Účet služby Storage úrovně Standard nebo Premium, který slouží k replikaci všech odpovídajících virtuálních počítačů uvedených ve sloupci **VMs to Place** (Virtuální počítače k umístění).

**Suggested Prefix:** Navrhovaná tříznaková předpona, kterou můžete použít k pojmenování účtu úložiště. Můžete použít vlastní předponu, ale návrh nástroje se řídí [zásadami vytváření názvů pro oddíly účtů úložiště](https://aka.ms/storage-performance-checklist).

**Suggested Account Name:** Název účtu úložiště po zahrnutí navrhované předpony. Název v ostrých závorkách (< a >) nahraďte vlastním názvem.

**Log Storage Account:** Všechny protokoly replikace se ukládají v účtu služby Storage úrovně Standard. Pro virtuální počítače, které se replikují do účtu služby Storage úrovně Premium, nastavte další účet služby Storage úrovně Standard pro ukládání protokolů. Jeden účet úložiště protokolů úrovně Standard může využívat více účtů úložiště replikace úrovně Premium. Virtuální počítače replikované do účtů úložiště úrovně Standard používají stejný účet i k ukládání protokolů.

**Suggested Log Account Name:** Název účtu úložiště protokolů po zahrnutí navrhované předpony. Název v ostrých závorkách (< a >) nahraďte vlastním názvem.

**Placement Summary:** Souhrn celkové zátěže virtuálních počítačů na účet úložiště v době replikace a testovacího převzetí služeb při selhání nebo převzetí služeb při selhání. Zahrnuje celkový počet virtuálních počítačů namapovaných na účet úložiště, celkový počet R/W IOPS ve všech virtuálních počítačích umístěných v tomto účtu úložiště, celkový počet vstupně-výstupních operací zápisu (replikace) za sekundu, celkovou nastavenou velikost všech disků a celkový počet disků.

**Virtual Machines to Place:** Seznam všech virtuálních počítačů, které by se měly umístit do daného účtu úložiště pro zajištění optimálního výkonu a využití.

## <a name="compatible-vms"></a>Kompatibilní virtuální počítače
Sestava v Microsoft Excelu, kterou vygeneroval Plánovač nasazení služby Azure Site Recovery, poskytuje na listu věnovaném kompatibilním virtuálním počítačům všechny podrobné informace o kompatibilních virtuálních počítačích.

![Kompatibilní virtuální počítače](media/site-recovery-hyper-v-deployment-planner-analyze-report/compatible-vms-h2a.png)

**VM Name:** Název virtuálního počítače, který se použil v souboru VMListFile při generování sestavy. V tomto sloupci jsou uvedeny také disky (VHD) připojené k virtuálním počítačům. Názvy zahrnují názvy hostitelů Hyper-V, kam byly virtuální počítače umístěné, když je nástroj během období profilace rozpoznal.

**VM Compatibility** (Kompatibilita virtuálního počítače): Hodnoty jsou **Yes** (Ano) a **Yes**\* (Ano). **Yes**\* je pro situace, kdy je virtuální počítač vhodný pro službu [Azure Storage úrovně Premium](https://aka.ms/premium-storage-workload). Tady profilovaný disk s vysokou četností změn nebo IOPS odpovídá větší velikosti disku typu Premium, než je velikost namapovaná na tento disk. Účet úložiště určuje, na jaký disk služby Storage úrovně Premium se disk bude mapovat, na základě jeho velikosti. 
* Menší než 128 GB je P10.
* 128 GB až 256 GB je P15.
* 256 GB až 512 GB je P20.
* 512 GB až 1 024 GB je P30.
* 1 025 GB až 2 048 GB je P40.
* 2 049 GB až 4 095 GB je P50.

Pokud se například díky charakteristikám úloh disk umístil do kategorie P20 nebo P30, ale kvůli velikosti je mapován na nižší typ disku služby Storage úrovně Premium, nástroj označí tento virtuální počítač jako **Yes**\*. Nástroj také doporučí změnit velikost zdrojového disku tak, aby se vešel do doporučeného typu disku služby Storage úrovně Premium, nebo po převzetí služeb při selhání změnit typ cílového disku.

**Storage Type** (Typ služby Storage): Standard nebo Premium.

**Suggested Prefix:** Tříznaková předpona účtu úložiště.

**Storage Account** (Účet služby Storage): Název s použitím navrhované předpony účtu úložiště.

**Peak R/W IOPS (with Growth Factor):** Počet R/W IOPS na disku ve špičce (výchozí 95. percentil), včetně faktoru budoucího růstu (výchozí hodnota je 30 %). Všimněte si, že celkový počet R/W IOPS virtuálního počítače nebude vždy odpovídat součtu R/W IOPS jednotlivých disků virtuálního počítače, protože počet R/W IOPS virtuálního počítače ve špičce je maximální hodnota součtu R/W IOPS jeho jednotlivých disků v každé minutě období profilace.

**Peak Data Churn in Mbps (with Growth Factor):** Četnost změn dat na disku ve špičce (výchozí 95. percentil), včetně faktoru budoucího růstu (výchozí hodnota je 30 %). Všimněte si, že celková četnost změn dat virtuálního počítače nebude vždy odpovídat součtu četností změn dat jednotlivých disků virtuálního počítače, protože četnost změn dat virtuálního počítače ve špičce je maximální hodnota součtu četností změn jeho jednotlivých disků v každé minutě období profilace.

**Azure VM Size:** Ideální velikost mapovaného virtuálního počítače Azure Cloud Services pro tento místní virtuální počítač. Mapování vychází z velikosti paměti, počtu disků, jader nebo síťových adaptérů a počtu R/W IOPS místního virtuálního počítače. Doporučení vždy představuje nejmenší velikost virtuálního počítače Azure, která odpovídá všem charakteristikám místního virtuálního počítače.

**Number of Disks:** Celkový počet disků virtuálního počítače (VHD) ve virtuálním počítači.

**Disk size (GB):** Celková velikost všech disků virtuálního počítače. Nástroj ukazuje také velikosti jednotlivých disků ve virtuálním počítači.

**Cores:** Počet procesorových jader ve virtuálním počítači.

**Memory (MB):** Velikost paměti RAM ve virtuálním počítači.

**NICs:** Počet síťových adaptérů ve virtuálním počítači.

**Boot Type:** Typ spuštění virtuálního počítače. Může to být buď BIOS, nebo EFI.

## <a name="incompatible-vms"></a>Nekompatibilní virtuální počítače
Sestava v Microsoft Excelu, kterou vygeneroval Plánovač nasazení služby Azure Site Recovery, poskytuje na listu věnovaném nekompatibilním virtuálním počítačům všechny podrobné informace o nekompatibilních virtuálních počítačích.

![Nekompatibilní virtuální počítače](media/site-recovery-hyper-v-deployment-planner-analyze-report/incompatible-vms-h2a.png)

**VM Name:** Název virtuálního počítače, který se použil v souboru VMListFile při generování sestavy. V tomto sloupci jsou uvedeny také disky (VHD) připojené k virtuálním počítačům. Názvy zahrnují názvy hostitelů Hyper-V, kam byly virtuální počítače umístěné, když je nástroj během období profilace rozpoznal.

**VM Compatibility:** Označujte, proč je daný virtuální počítač nekompatibilní s Azure Site Recovery. Pro každý nekompatibilní disk virtuálního počítače jsou popsané důvody. V závislosti na publikovaných [omezeních úložiště](https://aka.ms/azure-storage-scalbility-performance) může důvodem být některá z následujících možností:

* Disk je větší než 4 095 GB. Azure Storage v současné době nepodporuje disky větší než 4 095 GB.

* Disk s operačním systémem je pro virtuální počítač generace 1 (typ spuštění BIOS) větší než 2 047 GB.  Pro virtuální počítače generace 1 Azure Site Recovery nepodporuje velikost disku operačního systému větší než 2 047 GB.

* Disk s operačním systémem je pro virtuální počítač generace 2 (typ spuštění EFI) větší než 300 GB. Pro virtuální počítače generace 2 Azure Site Recovery nepodporuje velikost disku operačního systému větší než 300 GB.

* Název virtuálního počítače obsahuje některý z následujících nepodporovaných znaků: „“ [] `  Nástroj nemůže získat profilovaná data virtuálních počítačů, které mají některý z těchto znaků v názvu. 

* Virtuální pevný disk sdílí dva nebo více virtuálních počítačů. Azure nepodporuje virtuální počítače se sdíleným virtuálním pevným diskem.

* Virtuální počítač se službou Virtual Fiber Channel se nepodporuje. Azure Site Recovery nepodporuje virtuální počítače se službou Virtual Fiber Channel.

* Cluster Hyper-V neobsahuje zprostředkovatele replikace. Azure Site Recovery nepodporuje virtuální počítače v clusteru Hyper-V, pokud pro tento cluster není nakonfigurovaný zprostředkovatel replikace Hyper-V.

* Virtuální počítač není vysoce dostupný. Azure Site Recovery nepodporuje virtuální počítače uzlu clusteru Hyper-V, jejichž virtuální pevné disky jsou uložené na místním disku, a ne na disku clusteru. 

* Celková velikost virtuálního počítače (replikace + testovací převzetí služeb při selhání) překračuje omezení podporované velikosti účtu Storage úrovně Premium (35 TB). K této nekompatibilitě obvykle dochází, když je ve virtuálním počítači jeden disk, jehož některé charakteristiky výkonu překračují maximální podporovaná omezení systému Azure nebo Azure Site Recovery pro účet služby Storage úrovně Standard. V takové situaci se virtuální počítač dostává do zóny účtu služby Storage úrovně Premium. Nicméně maximální podporovaná velikost účtu služby Storage úrovně Premium je 35 TB a jeden virtuální počítač nemůže být chráněn v rámci několika účtů úložiště. Všimněte si také, že pokud je pro testovací převzetí služeb při selhání nakonfigurovaný nespravovaný disk a testovací převzetí služeb při selhání je spuštěné na chráněném virtuálním počítači, poběží v rámci stejného účtu úložiště, ve kterém probíhá replikace. V tomto případě se vyžaduje další prostor úložiště se stejnou velikostí jako u replikace. Tím se zajistí současné pokračování replikace a úspěšné testovací převzetí služeb při selhání. Pokud je pro testovací převzetí služeb při selhání nakonfigurovaný spravovaný disk, pro virtuální počítač testovacího převzetí služeb při selhání není potřeba žádné další místo.

* Source IOPS exceeds supported storage IOPS limit of 7500 per disk (Počet zdrojových IOPS překračuje podporované omezení úložiště – 7 500 IOPS na disk).

* Source IOPS exceeds supported storage IOPS limit of 80,000 per VM (Počet zdrojových IOPS překračuje podporované omezení úložiště – 80 000 IOPS na virtuální počítač).

* Average data churn exceeds supported Azure Site Recovery data churn limit of 10 MBps for average I/O size for the disk (Průměrná četnost změn dat překračuje podporované omezení Azure Site Recovery pro četnost změn dat – průměrná velikost vstupně-výstupních operací disku 10 Mb/s).

* Average effective write IOPS exceeds supported Azure Site Recovery IOPS limit of 840 for disk (Průměrný počet efektivních vstupně-výstupních operací zápisu za sekundu překračuje podporované omezení Azure Site Recovery pro počet IOPS – 840 na disk).

* Calculated snapshot storage exceeds the supported snapshot storage limit of 10 TB (Vypočtená velikost úložiště snímků překračuje podporované omezení velikosti úložiště snímků – 10 TB).

**Peak R/W IOPS (with Growth Factor):** Počet IOPS na disku ve špičce (výchozí 95. percentil), včetně faktoru budoucího růstu (výchozí hodnota je 30 %). Všimněte si, že celkový počet R/W IOPS virtuálního počítače nebude vždy odpovídat součtu R/W IOPS jednotlivých disků virtuálního počítače, protože počet R/W IOPS virtuálního počítače ve špičce je maximální hodnota součtu R/W IOPS jeho jednotlivých disků v každé minutě období profilace.

**Peak Data Churn in Mbps (with Growth Factor):** Četnost změn dat na disku ve špičce (výchozí 95. percentil), včetně faktoru budoucího růstu (výchozí hodnota je 30 %). Všimněte si, že celková četnost změn dat virtuálního počítače nebude vždy odpovídat součtu četností změn dat jednotlivých disků virtuálního počítače, protože četnost změn dat virtuálního počítače ve špičce je maximální hodnota součtu četností změn jeho jednotlivých disků v každé minutě období profilace.

**Number of Disks:** Celkový počet virtuálních pevných disků ve virtuálním počítači.

**Disk size (GB):** Celková nastavená velikost všech disků virtuálního počítače. Nástroj ukazuje také velikosti jednotlivých disků ve virtuálním počítači.

**Cores:** Počet procesorových jader ve virtuálním počítači.

**Memory (MB):** Velikost paměti RAM ve virtuálním počítači.

**NICs:** Počet síťových adaptérů ve virtuálním počítači.

**Boot Type:** Typ spuštění virtuálního počítače. Může to být buď BIOS, nebo EFI.

## <a name="azure-site-recovery-limits"></a>Omezení Azure Site Recovery
Následující tabulka obsahuje omezení služby Azure Site Recovery. Tato omezení se zakládají na našich testováních, nemůžou však pokrýt všechny možné kombinace vstupně-výstupních operací aplikace. Skutečné výsledky se můžou lišit v závislosti na kombinaci vstupně-výstupních operací vaší aplikace. Pro dosažení co nejlepších výsledků, a to i po naplánování nasazení, vždy doporučujeme provádět rozsáhlé testování aplikace pomocí testovacího převzetí služeb při selhání, abyste získali skutečnou představu o výkonu aplikace.
 
**Cíl ukládání replikace** | **Průměrná velikost vstupně-výstupních operací zdrojového disku** |**Průměrná četnost změn dat zdrojového disku** | **Celková denní četnost změn dat zdrojového disku**
---|---|---|---
Storage úrovně Standard | 8 kB | 2 Mb/s | 168 GB na disk
Disk úrovně Premium P10 nebo P15 | 8 kB  | 2 Mb/s | 168 GB na disk
Disk úrovně Premium P10 nebo P15 | 16 kB | 4 Mb/s |  336 GB na disk
Disk úrovně Premium P10 nebo P15 | 32 kB nebo větší | 8 Mb/s | 672 GB na disk
Disk úrovně Premium P20 nebo P30 nebo P40 nebo P50 | 8 kB    | 5 Mb/s | 421 GB na disk
Disk úrovně Premium P20 nebo P30 nebo P40 nebo P50 | 16 kB nebo větší |10 Mb/s | 842 GB na disk

Tato omezení jsou průměrné hodnoty za předpokladu, že se vstupně-výstupní operace z 30 % překrývají. Služba Azure Site Recovery je schopna zpracovávat větší propustnost v závislosti na poměru překrývání, větší velikosti zápisů a skutečného chování vstupně-výstupních operací úloh. Předchozí čísla předpokládají typický backlog přibližně 5 minut. To znamená, že zpracování nahrávaných dat a vytvoření bodu obnovení proběhne do pěti minut od nahrání.

## <a name="on-premises-storage-requirement"></a>Požadavek na místní úložiště

List obsahuje požadavek na celkový volný prostor úložiště pro všechny svazky serveru Hyper-V (kde jsou virtuální pevné disky umístěné) pro zajištění úspěšné počáteční a rozdílové replikace. Před povolením replikace přidejte na svazcích potřebný prostor úložiště, abyste zajistili, že replikace nezpůsobí žádné nežádoucí výpadky vašich provozních aplikací. 

Plánovač nasazení služby Azure Site Recovery identifikuje optimální prostor úložiště na základě velikosti virtuálních pevných disků a šířky pásma sítě využité pro replikaci.

![Požadavek na místní úložiště](media/site-recovery-hyper-v-deployment-planner-analyze-report/on-premises-storage-requirement-h2a.png)

### <a name="why-do-i-need-free-space-on-the-hyper-v-server-for-the-replication"></a>Proč je pro replikaci potřeba volné místo na serveru Hyper-V?
* Když povolíte replikaci virtuálního počítače, Azure Site Recovery pořídí snímek každého virtuálního pevného disku tohoto virtuálního počítače pro počáteční replikaci (IR). Při počáteční replikace jsou nové změny zapisovány na disky aplikací. Azure Site Recovery sleduje tyto rozdílové změny v souborech protokolů, které vyžadují další úložný prostor.  Než se počáteční replikace dokončí, soubory protokolu se ukládají místně. Pokud není k dispozici dostatek místa pro soubory protokolů a snímek (AVHDX), replikace přejde do režimu opakované synchronizace a nikdy se nedokončí. V nejhorším případě je pro počáteční replikaci potřeba další volné místo odpovídající 100 % velikosti virtuálního pevného disku.
* Po dokončení počáteční replikace se spustí rozdílová replikace. Azure Site Recovery sleduje tyto rozdílové změny v souborech protokolů, které jsou uložené na svazku, kde jsou umístěné virtuální pevné disky příslušného virtuálního počítače. Tyto soubory protokolů se replikují do Azure s nakonfigurovanou frekvencí kopírování. Na základě dostupné šířky pásma sítě replikace souborů protokolu do Azure nějakou dobu trvá. Pokud není k dispozici dostatek volného místa pro ukládání souborů protokolu, replikace se pozastaví a replikace virtuálního počítače přejde do stavu nutnosti opakované synchronizace.
* Pokud šířka pásma sítě nestačí pro nasdílení změn souborů protokolů do Azure, soubory protokolů se budou na svazku hromadit. V nejhorším případě pokud se velikost souborů protokolů zvýší na 50 % velikosti VHD, replikace virtuálního počítače přejde do stavu nutnosti opakované synchronizace. V nejhorším případě je pro rozdílovou replikaci potřeba další volné místo odpovídající 50 % velikosti virtuálního pevného disku.

**Hyper-V host:** Seznam profilovaných serverů Hyper-V. Pokud součástí clusteru Hyper-V je server, jsou všechny uzly clusteru seskupené dohromady.

**Volume (VHD path):** Každý svazek hostitele Hyper-V, kde jsou VHD/VHDX. 

**Free space available (GB):** Volné místo dostupné na svazku.

**Total storage space required on the volume (GB):** Celkový volný prostor úložiště vyžadovaný na svazku pro zajištění úspěšné počáteční a rozdílové replikace. 

**Total additional storage to be provisioned on the volume for successful replication:** Doporučené celkové další místo, které musí být na svazku zřízené pro úspěšnou počáteční a rozdílovou replikaci.

## <a name="initial-replication-batching"></a>Rozdělení počáteční replikace do dávek 

### <a name="why-do-i-need-initial-replication-ir-batching"></a>Proč je nutné rozdělení počáteční replikace do dávek?
Pokud by se všechny virtuální počítače chránily ve stejnou dobu, byl by požadavek na volné úložiště mnohem vyšší. Pokud není k dispozici dostatečně velké úložiště, replikace virtuálních počítačů přejde do režimu opakované synchronizace. Také požadavek na šířku pásma by pro úspěšné dokončení počáteční replikace všech virtuálních počítačů současně byl mnohem vyšší. 

### <a name="initial-replication-batching-for-a-selected-rpo"></a>Rozdělení počáteční replikace do dávek pro vybraný cíl bodu obnovení
Tento list poskytuje podrobné zobrazení jednotlivých dávek pro počáteční replikaci (IR). Pro každý cíl bodu obnovení se vytvoří samostatný list rozdělení počáteční replikace do dávek. 

Pokud jste pro každý svazek postupovali podle doporučení požadavku na místní úložiště, hlavní informace, které je potřeba replikovat, je seznam virtuálních počítačů, které se dají chránit paralelně. Tyto virtuální počítače jsou seskupené v dávce. Může existovat několik dávek. Virtuální počítače musí být chráněné ve stanoveném pořadí dávek. Nejdřív nastavte ochranu pro virtuální počítače dávky 1 a jakmile se dokončí počáteční replikace, nastavte ochranu pro virtuální počítače dávky 2 atd. Seznam dávek a odpovídajících virtuálních počítačů najdete můžete získat právě z tohoto listu. 

![Podrobnosti rozdělení počáteční replikace do dávek 1](media/site-recovery-hyper-v-deployment-planner-analyze-report/ir-batching-for-rpo-h2a.png)
![Podrobnosti rozdělení počáteční replikace do dávek 2](media/site-recovery-hyper-v-deployment-planner-analyze-report/ir-batching-for-rpo2-h2a.png)

### <a name="each-batch-provides-the-following-information"></a>Každá dávka poskytuje následující informace: 
**Hyper-V host:** Hostitel Hyper-V virtuálního počítače, který se má chránit.
Virtual machine: Virtuální počítač, který se má chránit. 

**Comments:** Pokud se pro konkrétní svazek virtuálního počítače vyžaduje nějaká akce, uvede se zde příslušný komentář.  Pokud například na svazku není dostatek volného místa, v komentáři se uvede Add additional storage to protect this VM (Přidat další úložiště pro ochranu tohoto virtuálního počítače).

**Volume (VHD path):** Název svazku, na kterém se nachází virtuální pevné disky virtuálního počítače. Free space available on the volume (GB): Volné místo na příslušném svazku pro tento virtuální počítač. Při výpočtu dostupného volného místa na svazcích se bere v úvahu místo na disku použité pro rozdílovou replikaci virtuálními počítači předchozích dávek, jejichž virtuální pevné disky jsou na stejném svazku.  

Příklad: VM1, VM2 a VM3 se nacházejí na svazku E:\VHDpath. Před zahájením replikace je na svazku 500 GB volného místa. VM1 je součástí dávky 1, VM2 je součástí dávky 2 a VM3 je součástí dávky 3.  Pro VM1 je k dispozici 500 GB volného místa. Dostupné místo pro VM2 je 500 – místo na disku vyžadované pro replikaci pro VM1.  Řekněme, že VM1 vyžaduje pro rozdílovou replikaci 300 GB. Volné místo pro VM2 by potom bylo 500 GB – 300 GB = 200 GB.  Podobně VM2 vyžaduje pro rozdílovou replikaci 300 GB. Dostupné volné místo pro VM3 by potom bylo 200 GB – 300 GB = -100 GB.

**Storage required on the volume for initial replication (GB):** Volný prostor úložiště vyžadovaný na svazku pro počáteční replikaci příslušného virtuálního počítače.

**Storage required on the volume for delta replication (GB):** Volný prostor úložiště vyžadovaný na svazku pro rozdílovou replikaci příslušného virtuálního počítače.

**Additional storage required based on deficit to avoid replication failure (GB):** Další prostor úložiště vyžadovaný na svazku pro příslušný virtuální počítač.  Odpovídá rozdílu mezi maximálním požadavkem na prostor úložiště pro počáteční a rozdílovou replikaci a volným místem, které je na svazku dostupné.

**Minimum bandwidth required for initial replication (Mbps):** Minimální šířka pásma vyžadovaná pro počáteční replikaci příslušného virtuálního počítače.

**Minimum bandwidth required for delta replication (Mbps):** Minimální šířka pásma vyžadovaná pro rozdílovou replikaci příslušného virtuálního počítače.

### <a name="network-utilization-details-for-each-batch"></a>Podrobnosti o využití sítě pro jednotlivé dávky 
Každá tabulka poskytuje přehled o využití sítě příslušnou dávkou.

**Bandwidth available for Batch:** Šířka pásma dostupná pro dávku potom, co se vezme v úvahu šířka pásma rozdílové replikace předchozí dávky.

**Approximate bandwidth available for initial replication of batch:** Šířka pásma dostupná pro počáteční replikaci virtuálních počítačů příslušné dávky. 

**Approximate bandwidth consumed for delta replication of batch:** Šířka pásma nutná pro rozdílovou replikaci virtuálních počítačů příslušné dávky. 

**Estimated Initial Replication time for Batch (HH:MM):** Odhadovaný čas počáteční replikace ve formátu hodiny:minuty.

## <a name="cost-estimation"></a>Odhad nákladů
Další informace o [odhadu nákladů](site-recovery-hyper-v-deployment-planner-cost-estimation.md) 

## <a name="next-steps"></a>Další kroky
Další informace o [odhadu nákladů](site-recovery-hyper-v-deployment-planner-cost-estimation.md)