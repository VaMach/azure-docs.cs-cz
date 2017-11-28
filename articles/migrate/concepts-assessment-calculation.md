---
title: "Vyhodnocení výpočtů v Azure migrovat | Microsoft Docs"
description: "Poskytuje přehled o vyhodnocení výpočtů ve službě Azure migrovat."
services: migrate
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 39a63769-31eb-49f9-9089-4d3e4e88a412
ms.service: migrate
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 11/21/2017
ms.author: raynew
ms.openlocfilehash: f00825ff9a5018e67672ce452f01130f84919e52
ms.sourcegitcommit: 5bced5b36f6172a3c20dbfdf311b1ad38de6176a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/27/2017
---
# <a name="assessment-calculations"></a>Výpočty hodnocení

[Azure migrací](migrate-overview.md) vyhodnocuje místní úlohy pro migraci do Azure. Tento článek obsahuje informace o tom, jak jsou vypočítávány vyhodnocování.



## <a name="overview"></a>Přehled

Posouzení migrace Azure má tři fáze. Hodnocení začíná vhodnosti analýzy, za nímž následuje odhady založené na výkon nastavení velikosti a nakonec měsíční náklady odhad. Počítače s pouze přesune na pozdější fázi pokud předává předchozí. Například pokud je počítač selže kontrola Azure vhodnosti, je označen jako není vhodný pro nebude vypočítat Azure a velikost a nákladů. 


## <a name="azure-suitability-analysis"></a>Analýza Azure vhodnosti

Počítače, které chcete migrovat do Azure, musí splňovat požadavky pro Azure a omezení. Například pokud disk virtuálního počítače, na místní je více než 4 TB, ho nemůže být hostovaná v Azure. Kontroly dodržování předpisů Azure jsou shrnuté v následující tabulce. 

**Kontrola** | **Podrobnosti**
--- | ---
**Typ spouštění** | Typ spouštěcí disk operačního systému hosta musí být v systému BIOS a ne rozhraní UEFI.
**Počet jader** | Počet jader na počítačích musí být roven (nebo menší než) maximální počet jader (32) podporované pro virtuální počítač Azure.<br/><br/> Pokud není k dispozici historie výkonu, migrovat Azure zvažuje využívané jader pro porovnání. Pokud v nastavení hodnocení je zadán koeficient pohodlí, počet jader využívané vynásobí faktorem pohodlí.<br/><br/> Pokud není žádná historie výkonu, migrovat Azure používá přidělených jader bez použití Multi-Factor pohodlí.
**Paměť** | Velikost paměti počítače musí být roven (nebo menší než) pro virtuální počítač Azure povolená maximální velikost paměti (448 GB). <br/><br/> Pokud není k dispozici historie výkonu, migrovat Azure zvažuje využívané paměti pro porovnání. Pokud je zadán koeficient pohodlí, využívané paměti vynásobí faktorem pohodlí.<br/><br/> Pokud není žádná historie je použita přidělenou paměť, bez použití Multi-Factor pohodlí.<br/><br/> 
**Windows Server 2003 – 2008 R2** | Podpora 32bitové a 64bitové verze.<br/><br/> Azure poskytuje nejlepší úsilí podporu jenom.
**Windows Server 2008 R2 s všechny aktualizace Service Pack** | Podpora 64bitových technologií.<br/><br/> Azure poskytuje úplnou podporu.
**Windows Server 2012 & všechny aktualizace Service Pack** | Podpora 64bitových technologií.<br/><br/> Azure poskytuje úplnou podporu.
**Windows Server 2012 R2 a všechny aktualizace Service Pack** | Podpora 64bitových technologií.<br/><br/> Azure poskytuje úplnou podporu.
**Windows Server 2016 & všechny aktualizace Service Pack** | Podpora 64bitových technologií.<br/><br/> Azure poskytuje úplnou podporu.
**Klient Windows 7 a novější** | Podpora 64bitových technologií.<br/><br/> Azure poskytuje podporu pro předplatné sady Visual Studio.
**Linux** | Podpora 64bitových technologií.<br/><br/> Azure poskytuje plnou podporu pro tyto [operační systémy](../virtual-machines/linux/endorsed-distros.md).
**Disk úložiště** | Přidělená velikost disku musí být 4 TB (4096 GB) nebo méně.<br/><br/> Počet disků připojených k počítači musí být 65 nebo méně, včetně disku operačního systému. 
**Sítě** | Na počítači musíte mít 32 nebo méně síťové adaptéry připojené k němu.


## <a name="performance-based-sizing"></a>Nastavení velikosti na základě výkonu

Po počítač je označena jako vhodný pro Azure, Azure migrace se mapuje na velikost virtuálního počítače v Azure, pomocí následujících kritérií:

- **Kontrola úložiště**: Azure migrovat pokusí namapovat všechny disky připojené k počítači na disk v Azure:-migrovat Azure násobí Multi-Factor pohodlí vstupně-výstupních operací za sekundu (IOPS). Je také násobky propustnost (v MB/s) každý disk faktorem pohodlí. To poskytuje účinného IOPS a propustnost disku. Na základě Azure migrovat mapuje disk standard nebo premium disk v Azure.
    - Pokud služba nemůže najít disk s požadované IOPS & propustnost, označí počítač jako není vhodný pro Azure.
    - Pokud najde sadu vhodné disky, Azure migrace vybere ty, které podporují metodu redundance úložiště a k umístění zadanému v nastavení hodnocení.
    - Pokud existují oprávněné víc disků, vybere jeden s nejnižší náklady.
- **Propustnost disku úložiště**: [Další](../azure-subscription-service-limits.md#storage-limits) o Azure omezuje na disk a virtuální počítač.
- **Typ disku**: migrace Azure podporuje pouze spravované disky.
- **Sítě zkontrolujte**: migrace Azure se pokusí najít virtuálního počítače Azure, který může podporovat počet síťových adaptérů na místním počítači.
    - K tomu je agreguje data přenášená za sekundu (MBps) z počítače (síť out) na všech síťových adaptérů a vztahuje na pohodlí faktor agregovaného počtu. Toto číslo je-li použít k vyhledání podporující požadované síťový výkon virtuálním počítači Azure.
    - Azure migrací trvá nastavení sítě z virtuálního počítače a předpokládá, že bude v síti mimo datové centrum.
    - Pokud je k dispozici žádná data o výkonu sítě, považuje za pouze počet síťovou kartu pro nastavení velikosti virtuálních počítačů.
- **Výpočetní kontrola**: po úložiště a síťové požadavky jsou vypočítávány, migrovat Azure zvažuje výpočetní požadavky:
    - Pokud data výkonu je k dispozici pro virtuální počítač, porovná využívané jader a paměti a vztahuje na faktor pohodlí. Podle toho, že číslo, pokusí se najít vhodnou velikost virtuálního počítače v Azure.
    - Pokud je nalezen žádný vhodný velikost, tento počítač je označena jako není vhodný pro Azure.
    - Pokud se nenajde vhodný velikost, migrovat Azure platí výpočty úložiště a sítě. Pak provede umístění a cenovou úroveň nastavení pro konečné doporučení velikost virtuálního počítače.


## <a name="monthly-cost-estimation"></a>Měsíční náklady na odhad

Po dokončení nastavení velikosti doporučení se migrovat Azure vypočítá náklady na výpočetní a úložnou po migraci.

- **Výpočetní náklady**: pomocí doporučená velikost virtuálního počítače Azure, Azure migrovat používá rozhraní API fakturace můžete vypočítat náklady na měsíční pro virtuální počítač. Výpočet trvá operačního systému, programu software assurance, umístění a nastavení měny v úvahu. Ji ve všech počítačích, chcete-li vypočítat celkové měsíční náklady na výpočetní agreguje náklady.
- **Náklady na úložiště**: měsíční úložiště, náklady pro počítače s je vypočítána agregování měsíční náklady na všechny disky připojené k počítači. Azure migrací se vypočítává agregování náklady na úložiště všech počítačů celkové měsíční náklady na úložiště. V současné době výpočtu neberou nabízí uvedenou v nastavení assessment v úvahu.

Náklady jsou zobrazeny v měny v nastavení hodnocení. 


## <a name="next-steps"></a>Další kroky

[Nastavit hodnocení pro virtuální počítače VMware na místě](tutorial-assessment-vmware.md)
