---
title: "Vyhodnocení výpočtů v Azure migrovat | Microsoft Docs"
description: "Poskytuje přehled o vyhodnocení výpočtů ve službě Azure migrovat."
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 06/02/2017
ms.author: raynew
ms.openlocfilehash: b264e2ceac4e76faa37d21972b94cfe323aa3ce5
ms.sourcegitcommit: 12fa5f8018d4f34077d5bab323ce7c919e51ce47
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/23/2018
---
# <a name="assessment-calculations"></a>Výpočty hodnocení

[Azure migrací](migrate-overview.md) vyhodnocuje místní úlohy pro migraci do Azure. Tento článek obsahuje informace o tom, jak jsou vypočítávány vyhodnocování.


## <a name="overview"></a>Přehled

Posouzení migrace Azure má tři fáze. Hodnocení začíná vhodnosti analýzy, za nímž následuje velikosti, a nakonec měsíční náklady odhad. Počítače s pouze přesune na pozdější fázi pokud předává předchozí. Například pokud je počítač selže kontrola Azure vhodnosti, je označen jako není vhodný pro Azure a velikost a nákladů nebude provedeno. 


## <a name="azure-suitability-analysis"></a>Analýza Azure vhodnosti

Ne všechny počítače jsou vhodné pro spuštění v cloudu jako cloud má svou vlastní omezení a požadavky. Azure migrací vyhodnocuje každý místního počítače pro migraci vhodnosti do Azure a rozděluje se počítače do jedné z následujících kategorií:
- **Připraveno k Azure** -na počítači mohou být migrovány jako-je do Azure bez uložení změn. Se spustí v Azure s podporu úplné Azure.
- **Podmíněná připraveni k Azure** -tento počítač může spustit v Azure, ale nemusí mít podporu úplné Azure. Například počítač se starší verzí operačního systému Windows Server nepodporuje v Azure. Budete muset pečlivě před migrací těchto počítačů do Azure a postupujte podle pokynů nápravy navržený v assessment opravit problémy připravenosti, před migrací.
- **Není připraven pro Azure** – počítač se nespustí v Azure. Například pokud místní počítač disk o velikosti více než 4 TB připojený, ho nemůže být hostovaná v Azure. Budete muset podle pokynů uvedených nápravy navržený v assessment vyřešit problém, připravenosti před migrací do Azure. Pro počítače, které jsou označeny jako není připraven pro Azure neprovádí odhadu optimalizaci velikosti a nákladů.
- **Neznámá připravenost** -Azure migrovat nelze nalézt připravenost počítače z důvodu nedostatečných data dostupná v systému vCenter Server. 

Azure migrací zkontroluje počítač vlastnosti a hostovaného operačního systému k identifikaci Azure připravenosti na místním počítači.

### <a name="machine-properties"></a>Vlastnosti počítače.
Azure migrací zkontroluje následující vlastnosti virtuálního počítače na místě a určete, jestli virtuální počítač můžete spustit v Azure.
 
**Vlastnost** | **Podrobnosti** | **Stav připravenosti Azure**
--- | --- | ---
**Typ spouštění** | Azure podporuje virtuální počítače s typem spuštění jako systém BIOS a ne rozhraní UEFI. | Podmíněná připravené pro Azure, pokud se spouštěcí typ rozhraní UEFI. 
**Počet jader** | Počet jader na počítačích musí být rovna nebo menší než maximální počet jader (32) podporované pro virtuální počítač Azure.<br/><br/> Pokud není k dispozici historie výkonu, migrovat Azure zvažuje využívané jader pro porovnání. Pokud v nastavení hodnocení je zadán koeficient pohodlí, počet jader využívané vynásobí faktorem pohodlí.<br/><br/> Pokud není žádná historie výkonu, migrovat Azure používá přidělených jader bez použití Multi-Factor pohodlí. | Není připraven Pokud počet jader je větší než 32. 
**Paměť** | Velikost paměti počítače musí být rovna nebo menší než maximální povolenou pro virtuální počítač Azure paměť (448 GB). <br/><br/> Pokud není k dispozici historie výkonu, migrovat Azure zvažuje využívané paměti pro porovnání. Pokud je zadán koeficient pohodlí, využívané paměti vynásobí faktorem pohodlí.<br/><br/> Pokud není žádná historie je použita přidělenou paměť, bez použití Multi-Factor pohodlí.<br/><br/> | Není připraven Pokud velikost paměti je větší než 448 GB.
**Disk úložiště** | Přidělená velikost disku musí být 4 TB (4096 GB) nebo méně.<br/><br/> Počet disků připojených k počítači musí být 65 nebo méně, včetně disku operačního systému. | Není připraven, pokud existuje disk má velikost větší než 4 TB, nebo pokud existuje víc než 65 disky připojené k počítači. 
**Sítě** | Na počítači musíte mít 32 nebo méně síťové adaptéry připojené k němu. | Není připraven, pokud má počítač více než 32 síťových adaptérů

### <a name="guest-operating-system"></a>Hostovaný operační systém 
Společně s vlastností virtuálního počítače Azure migrovat také vypadá na hostovaný operační systém virtuálního počítače na místě a zjistit, zda virtuální počítač můžete spustit v Azure.

> [!NOTE]
> Azure migrací zvažuje operačního systému zadané v systému vCenter Server udělat následující analýzy. Vzhledem k tomu, že zjišťování provádí migraci Azure je založen na zařízení, nemá způsob, jak ověřit, zda operačního systému běžících v rámci virtuálního počítače je stejný jako jeden zadaný v systému vCenter Server.

Následující logice podle migrovat Azure slouží k identifikaci Azure připravenosti na virtuální počítač založený na operačním systému.

**Operační systém** | **Podrobnosti** | **Stav připravenosti Azure**
--- | --- | ---
Windows Server 2016 & všechny aktualizace Service Pack | Azure poskytuje úplnou podporu. | Připraveno pro Azure
Windows Server 2012 R2 a všechny aktualizace Service Pack | Azure poskytuje úplnou podporu. | Připraveno pro Azure
Windows Server 2012 & všechny aktualizace Service Pack | Azure poskytuje úplnou podporu. | Připraveno pro Azure
Windows Server 2008 R2 s všechny aktualizace Service Pack | Azure poskytuje úplnou podporu.| Připraveno pro Azure
Windows Server 2003-2008 R2 | Tyto operační systémy uplynulo jejich koncové datum podpory a nutnost [vlastní podporu smlouvy (CSA)](https://aka.ms/WSosstatement) pro podporu v Azure. | Podmíněná připraveni k Azure, zvažte upgradování operačního systému před migrací do Azure.
Windows 2000, 98, 95, NT, 3.1, MS-DOS | Předané jejich koncové datum podpory těchto operačních systémů, bude počítač může spustit v Azure, ale žádná podpora operačního systému je k dispozici v Azure. | Podmíněná připravený pro Azure, se doporučuje pro upgrade operačního systému před migrací do Azure.
Klient Windows 7, 8 a 10 | Azure poskytuje podporu pro předplatné sady Visual Studio. | Podmíněná připravené pro Azure.
Windows Vista, XP Professional | Předané jejich koncové datum podpory těchto operačních systémů, bude počítač může spustit v Azure, ale žádná podpora operačního systému je k dispozici v Azure. | Podmíněná připravený pro Azure, se doporučuje pro upgrade operačního systému před migrací do Azure.
Linux | Azure nezodpovídá tyto [operačních systémů Linux](../virtual-machines/linux/endorsed-distros.md). Jiných operačních systémů Linux může spustit v Azure, ale doporučuje se upgrade operačního systému na potvrzená verzi před migrací do Azure. | Připraveno pro Azure. Pokud je verze schválené.<br/><br/>Podmíněná připravené, pokud není schválené verzi.
Jiné operační systémy<br/><br/> například Oracle Solaris, atd. Apple Mac OS, FreeBSD atd. | Azure neschvaluje těchto operačních systémů. Tento počítač může spustit v Azure, ale žádná podpora operačního systému je k dispozici v Azure. | Podmíněná připravený pro Azure, se doporučuje nainstalovat podporovaný operační systém před migrací do Azure.  
Zadaný operační systém jako *jiných* v systému vCenter Server | Azure migrací nemůže v tomto případě identifikovat operačního systému. | Neznámý připravenosti. Ujistěte se, že operačního systému spuštěné ve virtuálním počítači podporovaná v Azure. 
32bitové operační systémy | Tento počítač může spustit v Azure, ale Azure nemusí poskytnout plnou podporu. | Podmíněná připravené pro Azure, zvažte upgrade operačního systému na počítač z 32bitového operačního systému na 64bitový operační systém, před migrací do Azure.

## <a name="sizing"></a>Nastavení velikosti

Po počítač je označena jako připravena pro Azure, Azure migrovat velikostí virtuálního počítače a jeho disky pro Azure. Je-li toto nastavení velikosti kritérium zadána ve vlastnostech assessment uděláte na základě výkonu nastavení velikosti, migrovat Azure za historie výkonu počítače k identifikaci velikost virtuálního počítače v Azure. Tato metoda je užitečná ve scénářích, kde jste přidělili přepsání místní virtuální počítač, ale je nízkou využití a byste chtěli optimální velikost virtuálních počítačů v Azure se uložit náklady.

> [!NOTE]
> Azure migrací shromažďuje historie výkonu místní virtuálních počítačů ze serveru vCenter Server. Aby přesné optimalizaci velikosti, zkontrolujte, že nastavení statistiky v systému vCenter Server je nastavená na úroveň 3 a počkejte alespoň jeden den před si to chtěl / vypnutí zjišťování místní virtuální počítače. Pokud nastavení statistiky v systému vCenter Server je nižší než úroveň 3, není shromažďovat údaje o výkonu pro disk a síť. 

Pokud nechcete, vezměte v úvahu historie výkonu a chcete převzít virtuální počítač jako-je do Azure, můžete zadat kritéria pro změnu velikosti jako *jako místní* a migrovat Azure bude potom velikost virtuálních počítačů, v závislosti na konfiguraci místní bez vzhledem k tomu data o využití.

### <a name="performance-based-sizing"></a>Nastavení velikosti na základě výkonu

Pro nastavení velikosti na základě výkonu, síťové adaptéry a potom migrovat Azure začíná disky připojené k virtuálnímu počítači, a pak mapy virtuální počítač Azure na základě požadavky na výpočetní místní virtuálního počítače. 
 
- **Disky**: migrace Azure pokusí namapovat všechny disky připojené k počítači na disk v Azure. 
    
    > [!NOTE]
    > Azure migrací podporuje pouze spravované disky pro vyhodnocení.
    
    - Pokud chcete získat efektivní diskové vstupně-výstupních operací za sekundu (IOPS) a propustnost (MB/s), Azure migrovat vynásobí disku IOPS a propustnost s Multi-Factor pohodlí. Na základě efektivní IOPS a hodnoty propustnosti, migrovat Azure označuje, jestli disk by měly být namapované na standard nebo premium disk v Azure.
    - Pokud migrace Azure nemůže najít disk s požadované IOPS & propustnost, označí počítač jako není vhodný pro Azure. [Další informace](../azure-subscription-service-limits.md#storage-limits) o Azure omezuje na disk a virtuální počítač.
    - Pokud najde sadu vhodné disky, Azure migrace vybere ty, které podporují metodu redundance úložiště a k umístění zadanému v nastavení hodnocení.
    - Pokud existují oprávněné víc disků, vybere jeden s nejnižší náklady.
    - Pokud se údaje o výkonu pro disky v není k dispozici, všechny disky jsou namapované na standardní disky v Azure.

- **Síťové adaptéry**: migrace Azure se pokusí najít virtuálního počítače Azure, který může podporovat počet síťových adaptérech připojených do místního počítače a výkonu vyžadují tyto síťové adaptéry.
    - Získat efektivní síťový výkon místní virtuální počítač, migrujte Azure agreguje data přenášená za sekundu (MBps) z počítače (síť out), ve všech síťových adaptérech a vztahuje na pohodlí faktor. Toto číslo se používá k vyhledání podporující požadované síťový výkon virtuálním počítači Azure.
    - Společně s výkon sítě, také považuje pokud virtuální počítač Azure může podporovat požadované počet síťových adaptérů.
    - Pokud je k dispozici žádná data o výkonu sítě, považuje za pouze počet síťových adaptérů pro nastavení velikosti virtuálních počítačů.

- **Velikost virtuálního počítače**: po úložiště a síťové požadavky jsou vypočítávány, migrovat Azure zvažuje procesoru a paměti požadavky najít vhodný velikost virtuálního počítače v Azure.
    - Azure migrací vyhledá využívané jader a paměti a platí pohodlí faktor, který chcete získat efektivní jader a paměti. Podle toho, že číslo, pokusí se najít vhodnou velikost virtuálního počítače v Azure.
    - Pokud je nalezen žádný vhodný velikost, tento počítač je označena jako není vhodný pro Azure.
    - Pokud se nenajde vhodný velikost, migrovat Azure platí výpočty úložiště a sítě. Pak provede umístění a cenovou úroveň nastavení pro konečné doporučení velikost virtuálního počítače.
    - Pokud existují více oprávněných velikosti virtuálního počítače Azure, se doporučuje jedna s nejnižší náklady.

### <a name="as-on-premises-sizing"></a>Jako místní Změna velikosti
Pokud je toto nastavení velikosti kritérium *jako místní nastavení velikosti*, migrovat Azure nebere v úvahu historie výkonu virtuálních počítačů a přiděluje virtuální počítače, a v závislosti na velikosti disků přidělené místní.
- **Úložiště**: pro každý disk se doporučuje standardní disk v Azure s stejnou velikost jako na místní disk.
- **Sítě**: pro každý síťový adaptér, se doporučuje síťový adaptér v Azure.
- **Výpočetní**: Azure migraci zjistí počet jader a velikost paměti virtuálního počítače na místě a doporučuje virtuální počítač Azure se stejnou konfigurací. Pokud existují více oprávněných velikosti virtuálního počítače Azure, se doporučuje jedna s nejnižší náklady.
 
### <a name="confidence-rating"></a>Hodnocení spolehlivosti

Každý assessment v migraci Azure souvisí s jistotou rozsahu od 1 hvězdička do 5 hvězdičky (1 hvězdička se nejnižší a nejvyšší 5 hvězdičkou). Hodnocení spolehlivosti je přiřazený k posouzení založené na dostupnosti datové body, které jsou potřebné k výpočtu hodnocení. Hodnocení spolehlivosti hodnocení pomáhá odhadnout spolehlivost velikost doporučení poskytované migrovat Azure. 

Hodnocení spolehlivosti se hodí, pokud byste *výkonu na základě velikosti* jako migrovat Azure nemusí mít dostatečná datové body k provést nastavení velikosti na základě využití. Pro *jako místní nastavení velikosti*, hodnocení spolehlivosti je vždy 5 hvězdičkami migrovat Azure má všechny datové body, je nutné upravit velikost virtuálního počítače. 

Pro výkonu na základě velikosti virtuálního počítače Azure migrovat musí data o využití procesoru a paměti. Navíc pro každý disk připojen k virtuálnímu počítači, musí pro čtení a zápis IOPS a propustnosti. Podobně pro každý síťový adaptér připojený k virtuálnímu počítači Azure migrovat musí sítě a konec provést nastavení velikosti na základě výkonu. Pokud některá z výše uvedených čísel využití není k dispozici v systému vCenter Server, nemusí být velikost doporučení provádí migraci Azure spolehlivé. V závislosti na procento datové body k dispozici je poskytována hodnocení spolehlivosti pro vyhodnocení:

   **Dostupnost datové body** | Hodnocení spolehlivosti
   --- | ---
   0%-20% | 1 Star
   21%-40% | 2 Star
   41%-60% | 3 Star
   61%-80% | 4 Star
   81%-100% | 5 Star

Posouzení nemusí mít všechny datové body k dispozici kvůli jednomu z následujících důvodů:
- Nastavení statistiky v systému vCenter Server není nastaven na úroveň 3 a hodnocení má na základě výkonu nastavení velikosti jako kritérium pro změnu velikosti. Pokud nastavení statistiky v systému vCenter Server je nižší než úroveň 3, údaje o výkonu pro diskových a síťových nejsou shromažďovány ze serveru vCenter Server. V takovém případě doporučení poskytované pro diskových a síťových migrace Azure není na základě využití. Pro úložiště migrace Azure doporučuje standardní disky, jako bez vzhledem k tomu IOPS nebo propustnost disku, migrovat Azure nemůže určit, zda disk potřebovat disk premium v Azure.
- Nastavení statistiky v systému vCenter Server byla nastavena na úroveň 3 kratší dobu, než si to chtěl / vypnuto zjišťování. Zvažte například scénář, kde můžete změnit statistiky nastavení úrovně 3 dnes a akci vypnutí zjišťování pomocí kolekce zařízení zítra (po 24 hodinách). Pokud vytváříte posouzení jeden den, máte všechny datové body a hodnocení spolehlivosti hodnocení by 5 hvězdičkami. Ale pokud chcete změnit dobu trvání výkonu ve vlastnostech hodnocení na jeden měsíc, hodnocení spolehlivosti přestane fungovat jako disk a nebude k dispozici údaje o výkonu sítě pro poslední jeden měsíc. Pokud chcete vzít v úvahu výkonnostní data součásti poslední jeden měsíc, doporučujeme, abyste vCenter nastavení statistiky serveru na úroveň 3 pro jeden měsíc předtím, než ji zjišťování. 
- Během období, pro kterou se vypočítává hodnocení byly vypnuty několika virtuálními počítači. Pokud žádné virtuální počítače byly vypnout některé doby trvání, vCenter Server nebude mít data výkonu pro toto období. 
- Mezi období, pro kterou se vypočítává hodnocení byly vytvořeny několika virtuálními počítači. Například pokud vytváříte posouzení pro historie výkonu poslední jeden měsíc, ale několika virtuálními počítači byly vytvořeny v prostředí jenom týdnem. V takových případech nebude historie výkonu nové virtuální počítače po celou dobu trvání existuje.

> [!NOTE]
> Pokud hodnocení spolehlivosti žádné vyhodnocování je menší než 4 hvězdiček, budete muset změnit úroveň nastavení statistiky serveru vCenter do 3, doporučujeme čekáme po dobu, která chcete pro vyhodnocení zvažte (1 den 1 týden/1 měsíc) a poté proveďte zjišťování a hodnocení. Pokud předchozí nelze provést, nastavení velikosti na základě výkonu nemusí být spolehlivé a doporučuje se přepnout na *jako místní nastavení velikosti* změnou vlastnosti hodnocení.

## <a name="monthly-cost-estimation"></a>Měsíční náklady na odhad

Po dokončení nastavení velikosti doporučení se migrovat Azure vypočítá náklady na výpočetní a úložnou po migraci.

- **Výpočetní náklady**: pomocí doporučená velikost virtuálního počítače Azure, Azure migrovat používá rozhraní API fakturace můžete vypočítat náklady na měsíční pro virtuální počítač. Výpočet trvá operačního systému, programu software assurance, umístění a nastavení měny v úvahu. Ji ve všech počítačích, chcete-li vypočítat celkové měsíční náklady na výpočetní agreguje náklady.
- **Náklady na úložiště**: měsíční úložiště, náklady pro počítače s je vypočítána agregování měsíční náklady na všechny disky připojené k počítači. Azure migrací se vypočítává agregování náklady na úložiště všech počítačů celkové měsíční náklady na úložiště. V současné době výpočtu neberou nabízí uvedenou v nastavení assessment v úvahu.

Náklady jsou zobrazeny v měny v nastavení hodnocení. 


## <a name="next-steps"></a>Další postup

[Vytvoření posouzení pro virtuální počítače VMware na místě](tutorial-assessment-vmware.md)
