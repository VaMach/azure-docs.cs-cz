---
title: Odhad replikace kapacity v Azure | Microsoft Docs
description: "Použijte tento článek k zjištění přibližné hodnoty kapacity při replikaci s Azure Site Recovery"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: jwhit
editor: 
ms.assetid: 0a1cd8eb-a8f7-4228-ab84-9449e0b2887b
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 10/30/2017
ms.author: nisoneji
ms.openlocfilehash: 840a559a82f3227a865d3c606b2fa321cb6144ab
ms.sourcegitcommit: 43c3d0d61c008195a0177ec56bf0795dc103b8fa
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/01/2017
---
# <a name="plan-capacity-for-protecting-hyper-v-vms-with-site-recovery"></a>Plánování kapacity pro ochranu virtuálních počítačů Hyper-V pomocí Site Recovery

Nástroj Azure Site Recovery Capacity Planner pomáhá pochopit požadavky na kapacitu při replikaci virtuálních počítačů technologie Hyper-V s Azure Site Recovery.

Použijte nástroj Capacity Planner obnovení lokality k analýze vašeho zdrojového prostředí a úlohy, musí odhad šířky pásma a prostředky serveru, které budete potřebovat pro umístění zdroje a prostředky (virtuální počítače a úložiště atd), které je třeba v cílovém umístění.

Nástroj můžete spustit v několika režimy:

* **Rychlé plánování**: spuštění nástroje v tomto režimu získat sítě a serveru projekce založené na průměrném počtu virtuálních počítačů, disků, úložiště a míru změn.
* **Podrobné plánování**: Spusťte nástroj v tomto režimu a uveďte podrobné informace o každé zatížení na úrovni virtuálního počítače. Analýza kompatibility virtuálních počítačů a získat projekce sítě a serveru.

## <a name="before-you-start"></a>Než začnete


1. Shromážděte informace o prostředí, včetně virtuálních počítačů, disků na virtuální počítač, úložišti na disk.
2. Určete vaše denní míry změn s ohledem pro replikovaná data. Chcete-li to provést, stáhněte [nástroj plánování kapacity služby technologie Hyper-V](https://www.microsoft.com/download/details.aspx?id=39057) získat míru změn. [Další informace](site-recovery-capacity-planning-for-hyper-v-replication.md) o tomto nástroji. Doporučujeme, aby že tento nástroj spustíte přes týden k zachycení průměry.
   

## <a name="run-the-quick-planner"></a>Spustit Rychlé plánovače
1. Stáhněte si a nainstalujte [Azure Site Recovery Capacity Planner](http://aka.ms/asr-capacity-planner-excel) nástroj. Budete muset spustit makra, takže vyberte možnost povolit, úpravy a povolit obsah po zobrazení výzvy.
2. V **vyberte typ planner** vyberte **rychlé Planner** z pole se seznamem.

   ![Začínáme](./media/site-recovery-capacity-planner/getting-started.png)
3. V **Capacity Planner** listu, zadejte požadované informace. Je nutné vyplnit všechna pole v kroužku červeně na tomto snímku obrazovky.

   * V **vyberte váš scénář**, zvolte **technologie Hyper-V do Azure** nebo **VMware nebo fyzický do Azure**.
   * V **průměrný denní data změnit rychlost (%)**, umístí informace shromáždit pomocí [nástroj plánování kapacity služby technologie Hyper-V](site-recovery-capacity-planning-for-hyper-v-replication.md) nebo [Azure Site Recovery nasazení Planner](./site-recovery-deployment-planner.md).  
   * **Komprese** nastavení se nepoužívá při replikaci virtuálních počítačů Hyper-V do Azure. Pro kompresi použijte například Riverbed zařízení třetích stran.
   * V **uchování vstupy**, zadejte, jak dlouho by měl zachována repliky, v hodinách.
   * V **počet hodin, ve které počáteční replikace pro dávku virtuální počítače by se měla Dokončit** a **počet virtuálních počítačů na jednu dávku počáteční replikace**, zadejte nastavení, které se používají k výpočtu požadavky počáteční replikace.  Při nasazení Site Recovery, musí být nahrán celé počáteční datové sady.

   ![Vstupy](./media/site-recovery-capacity-planner/inputs.png)
4. Poté, co jste chápat hodnoty pro zdrojové prostředí, zobrazený výstup zahrnuje:

   * **Šířka pásma vyžadovaná pro replikaci rozdílů** (MB/s). Šířku pásma sítě pro rozdílová replikace se počítá na průměrné denní míry změny dat.
   * **Pásma potřebná pro počáteční replikaci** (MB/s). Šířku pásma sítě pro počáteční replikaci se počítá na počáteční replikace hodnoty, které jste zadali v.
   * **Vyžaduje (v GB) úložiště** je celkový počet úložiště Azure vyžaduje.
   * **Celkový počet IOPS na účty úložiště standard storage** se vypočítává podle velikost jednotky 8 kb IOPS na účty celkový standardní úložiště.  Pro rychlé Planner číslo je vypočítáváno na všechny zdrojové disky virtuálních počítačů a denní data frekvence změny. Pro podrobné Planner číslo se počítá na základě celkového počtu virtuálních počítačů, které jsou namapované na standardní virtuálních počítačích Azure a změny dat, míra na těchto virtuálních počítačích.
   * **Počet účtů úložiště standard storage** poskytuje celkový počet účtů úložiště standard storage, které jsou potřeba k ochraně virtuálních počítačů. Standardní účet úložiště může obsahovat až na 20 000 IOPS napříč virtuálními počítači ve standardní úložiště a na disk je podporováno maximálně 500 IOPS.
   * **Počet objektů blob disky potřebné** poskytuje počet disků, které bude vytvořena na úložiště Azure.
   * **Počet účtů úložiště premium požadované** poskytuje celkový počet prémiové účty úložiště potřeba k ochraně virtuálních počítačů. Zdrojového virtuálního počítače s vysokou IOPS (větší než 20000) musí prémiový účet úložiště. Prémiový účet úložiště může obsahovat až 80000 IOPS.
   * **Celkový počet IOPS na storage úrovně premium** se vypočítává podle velikost jednotky 256 kB IOPS na celkový prémiové účty úložiště.  Pro rychlé Planner číslo je vypočítáváno na všechny zdrojové disky virtuálních počítačů a denní data frekvence změny. Pro podrobné Planner číslo se počítá na základě celkového počtu virtuálních počítačů, které jsou namapované na premium virtuální počítač Azure (series a GS DS) a data změnit rychlost na těchto virtuálních počítačích.
   * **Počet serverů konfigurace vyžaduje** ukazuje, kolik serverů konfigurace jsou požadovány pro nasazení.
   * **Počet serverů další proces vyžaduje** zobrazuje, zda jsou povinné, kromě procesový server, který běží na konfiguračním serveru ve výchozím nastavení proces další servery.
   * **100 % dodatečné úložiště pro zdroj** zobrazuje, zda je vyžadováno dodatečné úložiště ve zdrojovém umístění.

   ![Výstup](./media/site-recovery-capacity-planner/output.png)

## <a name="run-the-detailed-planner"></a>Spustit podrobnou Planner

1. Stáhněte si a nainstalujte [Azure Site Recovery Capacity Planner](http://aka.ms/asr-capacity-planner-excel) nástroj. Budete muset spustit makra, takže vyberte možnost povolit, úpravy a povolit obsah po zobrazení výzvy.
2. V **vyberte typ planner**, vyberte **podrobné Planner** z pole se seznamem.

   ![Začínáme](./media/site-recovery-capacity-planner/getting-started-2.png)
3. V **zatížení kvalifikace** listu, zadejte požadované informace. Je nutné vyplnit všechna pole označené.

   * V **jader procesoru**, zadejte celkový počet jader na zdrojovém serveru.
   * V **přidělení paměti v MB**, zadejte velikost paměti RAM zdrojového serveru.
   * **Počet síťových adaptérů**, zadejte počet síťových adaptérů na zdrojovém serveru.
   * V **celkové úložiště (v GB)**, zadejte je celková velikost úložiště virtuálního počítače. Například pokud má zdrojový server 3 disky s 500 GB, pak celkovou velikost úložiště je 1500 GB.
   * V **počet disků připojených**, zadejte celkový počet disků ze zdrojového serveru.
   * V **využití kapacity disku**, zadejte průměrné využití.
   * V **denně frekvence (%) změny**, zadejte denních dat změnit počet zdrojového serveru.
   * V **mapování Azure velikost**, zadejte velikost virtuálního počítače Azure, který chcete propojit. Pokud nechcete provést ručně, klikněte na tlačítko **výpočetní virtuální počítače IaaS**. Pokud vstupní ruční nastavení a pak klikněte na výpočetní virtuální počítače IaaS, může být ruční nastavení přepsat, protože proces výpočetní automaticky určí nejlepší shodu na velikost virtuálního počítače Azure.

   ![Kvalifikace pracovního vytížení](./media/site-recovery-capacity-planner/workload-qualification.png)
4. Pokud kliknete na tlačítko **výpočetní virtuální počítače IaaS** tady je co umožňuje:

   * Ověří povinné vstupy.
   * Vypočítá IOPS a doporučuje nejlepší virtuálního počítače Azure aize shodu pro každý virtuální počítače, který by byl způsobilý pro replikaci do Azure. Pokud správnou velikost virtuálního počítače Azure nelze rozpoznat, že dojde k chybě. Například pokud počet disků připojena v 65, chyba je vydána protože nejvyšší velikost virtuálního počítače Azure je 64.
   * Navrhuje účet úložiště, který lze použít pro virtuální počítač Azure.
   * Vypočítá celkový počet účty úložiště standard a premium úložiště účtů nezbytných pro pracovní vytížení. Posuňte se dolů a zobrazit typ úložiště Azure a účet úložiště, který lze použít pro zdrojový server.
   * Dokončí a seřadí zbytek v tabulce na základě typu požadované úložiště (standard nebo premium) přiřazené pro virtuální počítač a počet disků připojených. U všech virtuálních počítačů, které splňují požadavky pro Azure, sloupci **je virtuální počítač kvalifikovaný?** ukazuje **Ano**. Pokud virtuální počítač nelze zálohovat do Azure, zobrazí se chyba.

Sloupce AA AE jsou výstupem a zadejte informace pro každý virtuální počítač.

![Kvalifikace pracovního vytížení](./media/site-recovery-capacity-planner/workload-qualification-2.png)

### <a name="example"></a>Příklad
Jako příklad u šesti virtuálních počítačů s hodnoty zobrazené v tabulce nástroje a nejlepší shodu virtuální počítač Azure a požadavky na úložiště Azure.

![Kvalifikace pracovního vytížení](./media/site-recovery-capacity-planner/workload-qualification-3.png)

* V příklad výstupu pamatujte na následující:

  * První sloupec je sloupec ověření pro virtuální počítače, disků a změn.
  * Pro pět virtuální počítače jsou potřeba dva účty úložiště standard storage a jeden prémiový účet úložiště.
  * VM3 není splňují podmínky pro ochranu, protože jeden nebo více disků se více než 1 TB.
  * VM1 a virtuálního počítače 2 můžete použít první účet standardního úložiště
  * VM4 můžete použít druhý účet standardního úložiště.
  * VM5 a VM6 potřebovat prémiový účet úložiště, a jak používat jeden účet.

    > [!NOTE]
    > IOPS na úložiště standard a premium jsou vypočítávány na úrovni virtuálního počítače a nikoli na úrovni disku. Standardní virtuální počítač dokáže zpracovat až 500 IOPS na disku. Pokud jsou větší než 500 IOPS pro disk, je třeba úložiště premium. Ale pokud IOPS pro disk více než 500, ale jsou IOPS pro celkový disky virtuálních počítačů v rámci podpory standardní virtuální počítač Azure (velikost virtuálního počítače, počet disků, počet adaptérů, procesor, paměť), pak Plánovač vybere standardní virtuální počítač a nikoli na DS nebo GS řady. Budete muset ručně aktualizovat buňky Azure velikost mapování příslušné řady DS nebo GS virtuálních počítačů.


Po všechny podrobnosti jsou na místě, klikněte na tlačítko **odesílání dat do nástroje planner** otevřete **Capacity Planner** úlohy se zvýrazní, chcete-li zobrazit, zda jsou způsobilé pro ochranu nebo ne.

### <a name="submit-data-in-the-capacity-planner"></a>Odesílání dat v Capacity Planner
1. Když otevřete **Capacity Planner** listu je naplněna na základě nastavení jste určili. Slovo 'zatížení, se zobrazí v **Infra vstupy zdroj** buňky, chcete-li zobrazit, že vstup je **zatížení kvalifikace** listu.
2. Pokud chcete změnit, budete muset upravit **zatížení kvalifikace** listu a klikněte na **odesílání dat do nástroje planner** znovu.  

   ![Plánovač kapacity](./media/site-recovery-capacity-planner/capacity-planner.png)

## <a name="next-steps"></a>Další kroky

[Informace o spouštění](site-recovery-capacity-planning-for-hyper-v-replication.md) nástroje Capacity Planner.
