---
title: "Plánovač nasazení služby Azure Site Recovery pro nasazení Hyper-V do Azure| Dokumentace Microsoftu"
description: "Tento článek popisuje režim spuštění Plánovače nasazení služby Azure Site Recovery pro scénář nasazení Hyper-V do Azure."
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
ms.openlocfilehash: 34f0b641abcf4231655d74da46f1bdcadc5642f6
ms.sourcegitcommit: 384d2ec82214e8af0fc4891f9f840fb7cf89ef59
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/16/2018
---
# <a name="run-azure-site-recovery-deployment-planner-for-hyper-v-to-azure"></a>Spuštění Plánovače nasazení služby Azure Site Recovery pro nasazení Hyper-V do Azure

## <a name="modes-of-running-deployment-planner"></a>Režimy spuštění plánovače nasazení
Nástroj příkazového řádku (ASRDeploymentPlanner.exe) můžete spustit v některém z následujících čtyř režimů: 
1.  [Získání seznamu virtuálních počítačů](#get-vm-list-for-profiling-hyper-v-vms)
2.  [Profilace](#profile-hyper-v-vms)
3.  [Generování sestav](#generate-report)
4.  [Zjištění propustnosti](#get-throughput)

Nejdřív nástroj spusťte pro získání seznamu virtuálních počítačů z jednoho nebo několika hostitelů Hyper-V.  Potom nástroj spusťte v režimu profilace, aby shromáždil četnost změn dat a počet IOPS virtuálních počítačů. Dále spusťte nástroj v režimu generování sestav, abyste zjistili požadavky na šířku pásma sítě a na úložiště.

## <a name="get-vm-list-for-profiling-hyper-v-vms"></a>Získání seznamu virtuálních počítačů pro profilaci virtuálních počítačů Hyper-V
Nejprve potřebujete seznam virtuálních počítačů určených k profilaci. V režimu GetVMList nástroje plánovače nasazení můžete vygenerovat seznam virtuálních počítačů na několika hostitelích Hyper-V, a to pomocí jediného příkazu. Jakmile vygenerujete kompletní seznam, můžete z výstupního souboru odebrat virtuální počítače, které nechcete profilovat. Potom výstupní soubor použijte pro všechny ostatní operace – profilace, generování sestav a zjištění propustnosti.

Seznam virtuálních počítačů můžete vygenerovat nastavením nástroje na cluster Hyper-V, na samostatného hostitele Hyper-V nebo na jejich kombinaci.

### <a name="command-line-parameters"></a>Parametry příkazového řádku
Následující tabulka obsahuje seznam povinných a volitelných parametrů nástroje pro spuštění v režimu GetVMList. 
```
ASRDeploymentPlanner.exe -Operation GetVMList /?
```
| Název parametru | Popis |
|---|---|
| -Operation | GetVMList |
| -User | Uživatelské jméno pro připojení k hostiteli Hyper-V nebo clusteru Hyper-V. Uživatel musí mít přístup pro správu.|
|-ServerListFile | Soubor se seznamem serverů obsahujících virtuální počítače, které se mají profilovat. Cesta k souboru může být absolutní nebo relativní. Tento soubor by měl na každém řádku obsahovat jednu z těchto hodnot:<ul><li>IP adresa nebo název hostitele Hyper-V</li><li>IP adresa nebo název clusteru Hyper-V</li></ul><br>Příklad: Soubor ServerList.txt obsahuje následující servery:<ul><li>Host_1</li><li>10.8.59.27</li><li>Cluster_1</li><li>Host_2</li>|
| -Directory|(Volitelné) Název UNC (Universal Naming Convention) nebo cesta k místnímu adresáři, do kterého se budou ukládat data vygenerovaná během této operace. Pokud tento parametr není zadaný, jako výchozí se použije adresář ProfiledData v aktuální cestě.|
|-OutputFile| (Volitelné) Soubor, ve kterém je uložený seznam virtuálních počítačů načtený z daných serverů Hyper-V. Pokud se název neuvede, budou podrobné informace uložené v souboru VMList.txt.  Tento soubor použijte k zahájení profilace, až odeberete virtuální počítače, které se nemusí profilovat.|
|-Password|(Volitelné) Heslo pro připojení k hostiteli Hyper-V.   Pokud heslo nezadáte jako parametr, budete k jeho zadání vyzváni později při spuštění příkazu.|

### <a name="how-does-getvmlist-discovery-work"></a>Jak funguje zjišťování GetVMList?
**Cluster Hyper-V:** Když je v souboru se seznamem serverů uvedený název clusteru Hyper-V, nástroj najde všechny uzly Hyper-V tohoto clusteru a získá všechny virtuální počítače na jednotlivých hostitelích Hyper-V.

**Hostitel Hyper-V:** Když je zadaný název hostitele Hyper-V, nástroj nejdřív zkontroluje, jestli patří do clusteru. Pokud ano, načte uzly patřící do tohoto clusteru. Potom získá virtuální počítače ze všech hostitelů Hyper-V. 

Případně můžete v souboru uvést seznam popisných názvů nebo IP adres virtuálních počítačů, které chcete profilovat ručně.

Otevřete výstupní soubor v Poznámkovém bloku a zkopírujte názvy všech virtuálních počítačů, které chcete profilovat, do jiného souboru (například ProfileVMList.txt), přičemž každý název virtuálního počítače musí být na samostatném řádku. Tento soubor se používá jako vstup pro parametr -VMListFile tohoto nástroje pro všechny ostatní operace: profilace, generování sestav a zjištění propustnosti.

### <a name="example-1-to-store-the-list-of-vms-in-a-file"></a>Příklad 1: Uložení seznamu virtuálních počítačů v souboru
```
ASRDeploymentPlanner.exe -Operation GetVMlist -ServerListFile “E:\Hyper-V_ProfiledData\ServerList.txt" -User Hyper-VUser1 -OutputFile "E:\Hyper-V_ProfiledData\VMListFile.txt"
```

### <a name="example-2-to-store-the-list-of-vms-at-the-default-location-ie--directory-path"></a>Příklad 2: Uložení seznamu virtuálních počítačů ve výchozí umístění, t.j. na cestě -Directory
```
ASRDeploymentPlanner.exe -Operation GetVMList -Directory "E:\Hyper-V_ProfiledData" -ServerListFile "E:\Hyper-V_ProfiledData\ServerList.txt" -User Hyper-VUser1
```

## <a name="profile-hyper-v-vms"></a>Profilace virtuálních počítačů Hyper-V
V režimu profilace se plánovač nasazení připojí k jednotlivým hostitelům Hyper-V za účelem shromáždění dat o výkonu virtuálních počítačů. 

* Profilace nemá vliv na výkon produkčních virtuálních počítačů, protože se k nim nevytváří žádné přímé připojení. Všechny údaje o výkonu se shromažďují z hostitele Hyper-V.
* Nástroj provádí dotazování hostitele Hyper-V každých 15 sekund, aby se zajistila přesnost profilace, a každou minutu ukládá průměr dat čítače výkonu.
* Nástroj bezproblémově zpracovává migraci virtuálních počítačů mezi uzly v clusteru a migraci úložiště v rámci hostitele.

### <a name="get-vm-list-to-profile"></a>Získání seznamu virtuálních počítačů pro profilaci
Informace o vytvoření seznamu virtuálních počítačů pro profilaci najdete v tématu [GetVMList](#get-vm-list-for-profiling-hyper-v-vms).

Jakmile budete mít seznam virtuálních počítačů určených k profilaci, můžete nástroj spustit v režimu profilace. 

### <a name="command-line-parameters"></a>Parametry příkazového řádku
Následující tabulka obsahuje seznam povinných a volitelných parametrů nástroje pro spuštění v režimu profilace. Tento nástroj je společný pro scénáře VMware do Azure i Hyper-V do Azure. Následující parametry jsou pro Hyper-V. 
```
ASRDeploymentPlanner.exe -Operation StartProfiling /?
```
| Název parametru | Popis |
|---|---|
| -Operation | StartProfiling |
| -User | Uživatelské jméno pro připojení k hostiteli Hyper-V nebo clusteru Hyper-V. Uživatel musí mít přístup pro správu.|
| -VMListFile | Soubor se seznamem virtuálních počítačů určených k profilaci. Cesta k souboru může být absolutní nebo relativní. Pro Hyper-V je tento soubor výstupním souborem operace GetVMList. Pokud provádíte přípravu ručně, měl by tento soubor na každém řádku obsahovat jednu IP adresu nebo název serveru a za ním název virtuálního počítače oddělený \. Název virtuálního počítače zadaný v souboru se musí shodovat s názvem virtuálního počítače na hostiteli Hyper-V.<ul>Příklad: Soubor VMList.txt obsahuje následující virtuální počítače:<ul><li>Host_1\VM_A</li><li>10.8.59.27\VM_B</li><li>Host_2\VM_C</li><ul>|
|-NoOfMinutesToProfile|Počet minut, po které má být profilace spuštěna. Minimální hodnota je 30 minut.|
|-NoOfHoursToProfile|Počet hodin, po které má být profilace spuštěna.|
|-NoOfDaysToProfile |Počet dní, po které má být profilace spuštěna. Doporučuje se profilaci spouštět na více než 7 dnů, během kterých se zjistí vzorce úloh ve vašem prostředí a s jejich pomocí nástroj poskytne přesná doporučení.|
|-Virtualization|Určuje typ virtualizace (VMware nebo Hyper-V).|
|-Directory|(Volitelné) Název UNC (Universal Naming Convention) nebo cesta k místnímu adresáři, do kterého se během profilace budou ukládat data profilace. Pokud tento parametr není zadaný, jako výchozí se použije adresář ProfiledData v aktuální cestě.|
|-Password|(Volitelné) Heslo pro připojení k hostiteli Hyper-V. Pokud heslo nezadáte teď, budete k tomu vyzváni později při spuštění příkazu.|
|-StorageAccountName|(Volitelné) Název účtu úložiště, který se použije k zjištění dosažitelné propustnost pro replikaci místních dat do Azure. Nástroj vypočítává propustnost tak, že do tohoto účtu úložiště nahrává testovací data. Účet úložiště musí být verze 1 pro obecné účely nebo úložiště verze 2 (verze 2 pro obecné účely).|
|-StorageAccountKey|(Volitelné) Klíč účtu úložiště, který se použije pro přístup k účtu úložiště. Přejděte na Azure Portal > Účty úložiště > <<Storage account name>> Nastavení > Přístupové klíče > Klíč1 (nebo primární přístupový klíč v případě klasického účtu úložiště).|
|-Environment|(Volitelné) Toto je vaše cílové prostředí účtu Azure Storage. Může to být jedna ze tří hodnot – AzureCloud, AzureUSGovernment a AzureChinaCloud. Výchozí hodnota je AzureCloud. Tento parametr použijte, pokud vaší cílovou oblastí Azure jsou cloudy Azure US Government nebo Azure China.|

Doporučujeme profilovat virtuální počítače po dobu delší než 7 dní. Pokud se vzor četnosti změn v měsíci mění, doporučujeme profilaci v týdnu, kdy je četnost změn maximální. Nejlepší způsob, jak získat lepší doporučení, je provádět profilaci 31 dní. Během období profilace je ASRDeploymentPlanner.exe stále spuštěný. Nástroj na vstupu přijímá zadání času profilace ve dnech. Pokud chcete rychle nástroj otestovat nebo potvrdit koncept, můžete provádět profilaci po několik hodin nebo minut. Minimální povolený čas profilace je 30 minut. 

Během profilace můžete volitelně předat název a klíč účtu úložiště a zjistit tak propustnost, které může Azure Site Recovery dosáhnout v době replikace ze serveru Hyper-V do Azure. Pokud název a klíč účtu úložiště během profilace nepředáte, nástroj nevypočte dosažitelnou propustnost.

Můžete spouštět více instancí nástroje pro různé sady virtuálních počítačů. Zkontrolujte, že se názvy virtuálních počítačů v sadách profilace neopakují. Pokud například profilujete 10 virtuálních počítačů (VM1–VM10) a po několika dnech chcete profilovat dalších pět virtuálních počítačů (VM11–VM15), můžete nástroj spustit z jiné konzoly příkazového řádku pro druhou sadu virtuálních počítačů (VM11–VM15). Zajistěte, aby druhá sada virtuálních počítačů neobsahovala žádné názvy virtuálních počítačů z první instance profilace, nebo pro druhé spuštění použijte jiný výstupní adresář. Pokud se k profilování stejných virtuálních počítačů používají dvě instance nástroje a stejný výstupní adresář, vygenerovaná sestava bude nepřesná. 

Ve výchozím nastavení je tento nástroj konfigurovaný tak, aby profiloval a generoval sestavy až pro 1000 virtuálních počítačů. Tento limit můžete změnit nastavením hodnoty klíče MaxVMsSupported v souboru *ASRDeploymentPlanner.exe.config*.
```
<!-- Maximum number of vms supported-->
<add key="MaxVmsSupported" value="1000"/>
```
Pokud chcete s výchozím nastavením profilovat řekněme 1 500 virtuálních počítačů, vytvořte dva soubory VMList.txt. Jeden s 1000 virtuálních počítačů a druhý se seznamem 500 virtuálních počítačů. Spusťte dvě instance Plánovače nasazení ASR, jednu s VMList1.txt druhou s VMList2.txt. K uložení profilovaných dat virtuálních počítačů z obou seznamů VMList můžete použít stejnou adresářovou cestu. 

Viděli jsme, že na základě konfigurace hardwaru, zejména velikosti RAM na serveru, ze kterého se spouští nástroj pro vygenerování sestavy, může operace selhat z důvodu nedostatku paměti. Pokud máte kvalitní hardware, můžete změnit MaxVMsSupported na libovolnou vyšší hodnotu.  

Konfigurace virtuálních počítačů se zachytí jednou na začátku operace profilace a uloží se do souboru VMDetailList.xml. Tyto informace se použijí při generování sestavy. Žádné změny v konfiguraci virtuálních počítačů (například navýšení počtu jader, disků nebo síťových adaptérů) od začátku do konce profilace se nezachytí. Pokud se během profilace změnila konfigurace některého profilovaného virtuálního počítače, tady je alternativní řešení, pomocí kterého můžete získat nejnovější podrobnosti o virtuálním počítači bez nutnosti generovat sestavu:

* Zálohujte soubor VMdetailList.xml a odstraňte ho z aktuálního umístění.
* V okamžiku generování sestav předejte argumenty -User a -Password.

Příkaz profilace vygeneruje v adresáři profilace několik souborů. Žádný z těchto souborů neodstraňujte, jinak to bude mít dopad na generování sestav.

### <a name="examples"></a>Příklady
#### <a name="example-1-profile-vms-for-30-days-and-find-the-throughput-from-on-premises-to-azure"></a>Příklad 1: Profilování virtuálních počítačů po dobu 30 dnů a zjištění propustnosti z místního prostředí do Azure
```
ASRDeploymentPlanner.exe -Operation StartProfiling -virtualization Hyper-V -Directory “E:\Hyper-V_ProfiledData” -VMListFile “E:\Hyper-V_ProfiledData\ProfileVMList1.txt”  -NoOfDaysToProfile 30 -User Contoso\HyperVUser1 -StorageAccountName  asrspfarm1 -StorageAccountKey Eby8vdM02xNOcqFlqUwJPLlmEtlCDXJ1OUzFT50uSRZ6IFsuFq2UVErCz4I6tq/K1SZFPTOtr/KBHBeksoGMGw==
```

#### <a name="example-2-profile-vms-for-15-days"></a>Příklad 2: Profilování virtuálních počítačů po dobu 15 dnů
```
ASRDeploymentPlanner.exe -Operation StartProfiling -Virtualization Hyper-V -Directory “E:\Hyper-V_ProfiledData” -VMListFile “E:\vCenter1_ProfiledData\ProfileVMList1.txt”  -NoOfDaysToProfile  15  -User contoso\HypreVUser1
```

#### <a name="example-3-profile-vms-for-60-minutes-for-a-quick-test-of-the-tool"></a>Příklad 3: Rychlé otestování nástroje profilováním virtuálních počítačů po dobu 60 minut
```
ASRDeploymentPlanner.exe -Operation StartProfiling -Virtualization Hyper-V -Directory “E:\Hyper-V_ProfiledData” -VMListFile “E:\Hyper-V_ProfiledData\ProfileVMList1.txt”  -NoOfMinutesToProfile 60 -User Contoso\HyperVUser1
```

#### <a name="example-4-profile-vms-for-2-hours-for-a-proof-of-concept"></a>Příklad 4: Profilace virtuálních počítačů po dobu 2 hodin pro testování konceptu
```
ASRDeploymentPlanner.exe -Operation StartProfiling -Virtualization Hyper-V -Directory “E:\Hyper-V_ProfiledData” -VMListFile “E:\Hyper-V_ProfiledData\ProfileVMList1.txt”  -NoOfHoursToProfile 2 -User Contoso\HyperVUser1
```

>Poznámka:
>
* Pokud se server, na kterém je nástroj spuštěný, restartuje nebo na něm dojde k chybě, nebo pokud nástroj zavřete stisknutím kombinace kláves Ctrl + C, profilovaná data se zachovají. Může se však stát, že kvůli tomu bude chybět posledních 15 minut profilovaných dat. V takových případech po restartování serveru znovu spusťte nástroj v režimu profilace.
* Pokud předáte název a klíč účtu úložiště, nástroj bude měřit propustnost v posledním kroku profilace. Pokud bude nástroj zavřen před dokončením profilace, propustnost se nevypočítá. Pokud chcete zjistit propustnost před generováním sestavy, můžete z konzoly příkazového řádku spustit operaci GetThroughput. Jinak vygenerovaná sestava nebude obsahovat informace o propustnosti.
* Azure Site Recovery nepodporuje virtuální počítače, které mají iSCSI a průchozí disky. Tento nástroj ale nemůže detekovat a profilovat iSCSI a průchozí disky připojené k virtuálním počítačům.

## <a name="generate-report"></a>Generování sestav
Nástroj jako výstup sestavy generuje soubor aplikace Microsoft Excel s podporou maker (soubor XLSM), který shrnuje veškerá doporučení pro nasazení. Sestava má název DeploymentPlannerReport_<unique numeric identifier>.xlsm a je umístěná v zadaném adresáři.
Po dokončení profilace můžete nástroj spustit v režimu generování sestav. 

### <a name="command-line-parameters"></a>Parametry příkazového řádku
Následující tabulka obsahuje seznam povinných a volitelných parametrů nástroje pro spuštění v režimu generování sestav. Tento nástroj je společný pro scénáře VMware do Azure i Hyper-V do Azure. Následující parametry jsou pro Hyper-V.
```
ASRDeploymentPlanner.exe -Operation GenerateReport /?
```
| Název parametru | Popis |
|---|---|
| -Operation | GenerateReport |
|-VMListFile | Soubor se seznamem profilovaných virtuálních počítačů, pro které se má vygenerovat sestava. Cesta k souboru může být absolutní nebo relativní. Pro Hyper-V je tento soubor výstupním souborem operace GetVMList. Pokud provádíte přípravu ručně, měl by tento soubor na každém řádku obsahovat jednu IP adresu nebo název serveru a za ním název virtuálního počítače oddělený \. Název virtuálního počítače zadaný v souboru se musí shodovat s názvem virtuálního počítače na hostiteli Hyper-V.<ul>Příklad: Soubor VMList.txt obsahuje následující virtuální počítače:<ul><li>Host_1\VM_A</li><li>10.8.59.27\VM_B</li><li>Host_2\VM_C</li><ul>|
|-Virtualization|Určuje typ virtualizace (VMware nebo Hyper-V).|
|-Directory|(Volitelné) Název UNC (Universal Naming Convention) nebo cesta k místnímu adresáři, ve kterém jsou uložena profilovaná data (soubory vytvořené během profilace). Tato data jsou vyžadovaná k vygenerování sestavy. Pokud název není zadaný, jako výchozí se použije adresář ProfiledData v aktuální cestě.|
| -User | (Volitelné) Uživatelské jméno pro připojení k hostiteli Hyper-V nebo clusteru Hyper-V. Uživatel musí mít přístup pro správu.<br>Uživatelské jméno a heslo slouží k načtení nejnovějších informací o konfiguraci virtuálních počítačů (např. počet disků, počet jader, počet síťových adaptérů atd.), které se použijí v sestavě. Pokud tento parametr nezadáte, použijí se konfigurační informace shromážděné na začátku profilace.|
|-Password|(Volitelné) Heslo pro připojení k hostiteli Hyper-V. Pokud heslo nezadáte teď, budete k tomu vyzváni později při spuštění příkazu.|
| -DesiredRPO | (Volitelné) Požadovaný cíl bodu obnovení v minutách. Výchozí hodnota je 15 minut.|
| -Bandwidth | (Volitelné) Šířka pásma v Mb/s. Tento parametr se použije k výpočtu cíle bodu obnovení, kterého lze pro zadanou šířku pásma dosáhnout. |
| -StartDate | (Volitelné) Počáteční datum a čas ve formátu MM-DD-YYYY:HH:MM (ve 24hodinovém formátu). Parametr *StartDate* je nutné zadat společně s parametrem *EndDate*. Pokud zadáte parametr StartDate, sestava se vygeneruje pro profilovaná data shromážděná mezi StartDate a EndDate. |
| -EndDate | (Volitelné) Koncové datum a čas ve formátu MM-DD-YYYY:HH:MM (ve 24hodinovém formátu). Parametr *EndDate* je nutné zadat společně s parametrem *StartDate*. Pokud zadáte parametr EndDate, sestava se vygeneruje pro profilovaná data shromážděná mezi StartDate a EndDate. |
| -GrowthFactor | (Volitelné) Faktor růstu vyjádřený v procentech. Výchozí hodnota je 30 procent. |
| -UseManagedDisks | (Volitelné) UseManagedDisks – Yes/No (Ano/Ne). Výchozí hodnota je Yes (Ano). Počet virtuálních počítačů, které lze umístit do jednoho účtu úložiště, se vypočítá s ohledem na to, že převzetí služeb při selhání nebo testovací převzetí služeb při selhání virtuálních počítačů se provádí na spravovaný disk namísto nespravovaného disku. |
|-SubscriptionId |(Volitelné) GUID předplatného. Tento parametr slouží k vytvoření sestavy odhadu nákladů s nejnovějšími cenami na základě vašeho předplatného a nabídky, která je přidružená k vašemu předplatnému, a pro zadanou cílovou oblast Azure v zadané měně.|
|-TargetRegion|(Volitelné) Oblast Azure, která je cílem replikace. Vzhledem k tomu, že Azure má různé náklady pro jednotlivé oblasti, pro generování sestavy s konkrétní cílovou oblastí Azure použijte tento parametr.<br>Výchozí hodnota je WestUS2 nebo poslední použitá cílová oblast.<br>K dispozici je seznam [podporovaných cílových oblastí](site-recovery-hyper-v-deployment-planner-cost-estimation.md#supported-target-regions).|
|-OfferId|(Volitelné) Nabídka přidružená k danému předplatnému. Výchozí hodnota je MS-AZR-0003P (průběžné platby).|
|-Currency|(Volitelné) Měna, ve které se ve vygenerované sestavě zobrazí náklady. Výchozí hodnota je americký dolar ($) nebo poslední použitá měna.<br>K dispozici je seznam [podporovaných měn](site-recovery-hyper-v-deployment-planner-cost-estimation.md#supported-currencies).|

Ve výchozím nastavení je tento nástroj konfigurovaný tak, aby profiloval a generoval sestavy až pro 1000 virtuálních počítačů. Tento limit můžete změnit nastavením hodnoty klíče MaxVMsSupported v souboru *ASRDeploymentPlanner.exe.config*.
```
<!-- Maximum number of vms supported-->
<add key="MaxVmsSupported" value="1000"/>
```

### <a name="examples"></a>Příklady
#### <a name="example-1-generate-a-report-with-default-values-when-the-profiled-data-is-on-the-local-drive"></a>Příklad 1: Generování sestavy s použitím výchozích hodnot pro profilovaná data umístěná na místním disku
```
ASRDeploymentPlanner.exe -Operation GenerateReport -virtualization Hyper-V -Directory “E:\Hyper-V_ProfiledData” -VMListFile “E:\Hyper-V_ProfiledData\ProfileVMList1.txt”
```

#### <a name="example-2-generate-a-report-when-the-profiled-data-is-on-a-remote-server"></a>Příklad 2: Generování sestavy pro profilovaná data umístěná na vzdáleném serveru
Ke vzdálenému adresáři musíte mít přístup ke čtení a zápisu.
```
ASRDeploymentPlanner.exe -Operation GenerateReport -Virtualization Hyper-V - -Directory “\\PS1-W2K12R2\Hyper-V_ProfiledData” -VMListFile “\\PS1-W2K12R2\vCenter1_ProfiledData\ProfileVMList1.txt”
```

#### <a name="example-3-generate-a-report-with-a-specific-bandwidth-that-you-will-be-provision-for-the-replication"></a>Příklad 3: Generování sestavy s konkrétní šířkou pásma, která se zřídí pro replikaci
```
ASRDeploymentPlanner.exe -Operation GenerateReport -Virtualization Hyper-V -Directory “E:\Hyper-V_ProfiledData” -VMListFile “E:\Hyper-V_ProfiledData\ProfileVMList1.txt” -Bandwidth 100
```

#### <a name="example-4-generate-a-report-with-a-5-percent-growth-factor-instead-of-the-default-30-percent"></a>Příklad 4: Generování sestavy s použitím 5% faktoru růstu namísto výchozích 30 % 
```
ASRDeploymentPlanner.exe -Operation GenerateReport -Virtualization Hyper-V -Directory “E:\Hyper-V_ProfiledData” -VMListFile “E:\Hyper-V_ProfiledData\ProfileVMList1.txt” -GrowthFactor 5
```

#### <a name="example-5-generate-a-report-with-a-subset-of-profiled-data"></a>Příklad 5: Generování sestavy s použitím podmnožiny profilovaných dat
Máte například profilovaná data za 30 dnů a chcete vygenerovat sestavu pouze pro 20 dnů.
```
ASRDeploymentPlanner.exe -Operation GenerateReport -virtualization Hyper-V -Directory “E:\Hyper-V_ProfiledData” -VMListFile “E:\Hyper-V_ProfiledData\ProfileVMList1.txt” -StartDate  01-10-2017:12:30 -EndDate 01-19-2017:12:30
```

#### <a name="example-6-generate-a-report-for-5-minutes-rpo"></a>Příklad 6: Generování sestavy s použitím 5minutového cíle bodu obnovení
```
ASRDeploymentPlanner.exe -Operation GenerateReport -Virtualization Hyper-V -Directory “E:\Hyper-V_ProfiledData” -VMListFile “E:\Hyper-V_ProfiledData\ProfileVMList1.txt”  -DesiredRPO 5
```

#### <a name="example-7-generate-a-report-for-south-india-azure-region-with-indian-rupee-and-specific-offer-id"></a>Příklad 7: Generování sestavy pro oblast Azure Indie – jih s indickými rupiemi a konkrétním ID nabídky
```
ASRDeploymentPlanner.exe -Operation GenerateReport -Virtualization Hyper-V -Directory “E:\Hyper-V_ProfiledData” -VMListFile “E:\Hyper-V_ProfiledData\ProfileVMList1.txt”  -SubscriptionID 4d19f16b-3e00-4b89-a2ba-8645edf42fe5 -OfferID MS-AZR-0148P -TargetRegion southindia -Currency INR
```


## <a name="percentile-value-used-for-the-calculation"></a>Hodnota percentilu používaná k výpočtu
**Jakou výchozí hodnotu percentilu metrik výkonu shromážděných během profilace nástroj používá při generování sestavy?**

Nástroj ve výchozím nastavení používá hodnoty 95. percentilu počtu R/W IOPS, vstupně-výstupních operací zápisu za sekundu a četnosti změn dat shromážděných během profilace všech virtuálních počítačů. Tato metrika zajišťuje, že se k určení požadavků na cílový účet úložiště a zdrojovou šířku pásma nepoužijí hodnoty 100. percentilu (špičky), které se můžou objevovat na virtuálních počítačích následkem dočasných událostí. Příkladem dočasné události může být úloha zálohování spouštěná jednou denně, pravidelné indexování databáze, aktivita generování analytických sestav nebo další podobné krátkodobé a jednorázové události.

Použitím hodnot 95. percentilu získáte pravdivou představu o skutečných charakteristikách úloh a nejlepší výkon při spouštění těchto úloh v Azure. Neočekáváme, že byste toto číslo potřebovali měnit. Pokud tuto hodnotu přesto změníte (například na 90. percentil), můžete upravit konfigurační soubor ASRDeploymentPlanner.exe.config ve výchozí složce a uložit ho. Tím vygenerujete novou sestavu pro existující profilovaná data.
```
<add key="WriteIOPSPercentile" value="95" />      
<add key="ReadWriteIOPSPercentile" value="95" />      
<add key="DataChurnPercentile" value="95" />
```

## <a name="growth-factor-considerations"></a>Aspekty faktoru růstu
**Proč bych při plánování nasazení měl brát v úvahu faktor růstu?**
Je důležité počítat s nárůstem v charakteristikách vašich úloh v důsledku možného zvýšení využití v průběhu času. Pokud se charakteristiky vašich úloh změní po zapnutí ochrany, nebudete moci přepnout na ochranu pod jiným účtem úložiště bez nutnosti ochranu zakázat a znovu povolit.

Řekněme například, že dnes se váš virtuální počítač vejde do účtu replikace služby Storage úrovně Standard. Během následujících tří měsíců pravděpodobně dojde k několika změnám:

* Zvýší se počet uživatelů aplikace spuštěné na virtuálním počítači.
* Výsledná zvýšená četnost změn na virtuálním počítači bude vyžadovat přesun virtuálního počítače do účtu služby Storage úrovně Premium, aby replikace Azure Site Recovery mohla s touto četností změn držet krok.
* V důsledku toho budete muset zakázat a znovu povolit ochranu účtu služby Storage úrovně Premium.

Důrazně doporučujeme, abyste při plánování nasazení brali v úvahu růst. Výchozí hodnota je 30 procent, ale vy sami nejlépe znáte vzorce používání vaší aplikace a očekávaný růst a během generování sestav můžete toto číslo odpovídajícím způsobem měnit. Kromě toho můžete generovat více sestav s použitím různých faktorů růstu pro stejná profilovaná data a určit, jaká doporučení ohledně cílového úložiště a zdrojové šířky pásma jsou pro vás nejvhodnější. 

Vygenerovaná sestava aplikace Microsoft Excel obsahuje následující informace:

* [On-premises summary](site-recovery-hyper-v-deployment-planner-analyze-report.md#on-premises-summary) (Přehled místního prostředí)
* [Recommendations](site-recovery-hyper-v-deployment-planner-analyze-report.md#recommendations) (Doporučení)
* [VM&lt;-&gt;Storage Placement](site-recovery-hyper-v-deployment-planner-analyze-report.md#vm-storage-placement-recommendation) (Umístění virtuálních počítačů ve službě Storage)
* [Compatible VMs](site-recovery-hyper-v-deployment-planner-analyze-report.md#compatible-vms) (Kompatibilní virtuální počítače)
* [Incompatible VMs](site-recovery-hyper-v-deployment-planner-analyze-report.md#incompatible-vms) (Nekompatibilní virtuální počítače)
* [On-premises Storage Requirement](site-recovery-hyper-v-deployment-planner-analyze-report.md#on-premises-storage-requirement) (Požadavek na místní úložiště)
* [IR Batching](site-recovery-hyper-v-deployment-planner-analyze-report.md#initial-replication-batching) (Rozdělení počáteční replikace do dávek)
* [Cost Estimation](site-recovery-hyper-v-deployment-planner-cost-estimation.md) (Odhad nákladů)

![Sestava plánovače nasazení](media/site-recovery-hyper-v-deployment-planner-run/deployment-planner-report-h2a.png)


## <a name="get-throughput"></a>Zjištění propustnosti
Pokud chcete odhadnout propustnost, které může Azure Site Recovery dosáhnout během replikace z místního prostředí do Azure, spusťte nástroj v režimu GetThroughput. Nástroj vypočítá propustnost ze serveru, na kterém je spuštěný. V ideálním případě tímto serverem je server Hyper-V, jehož virtuální počítače se mají chránit. 

### <a name="command-line-parameters"></a>Parametry příkazového řádku 
Otevřete konzolu příkazového řádku a přejděte do složky nástroje pro plánování nasazení Azure Site Recovery. Spusťte ASRDeploymentPlanner.exe s použitím následujících parametrů.
```
ASRDeploymentPlanner.exe -Operation GetThroughput /?
```
 Název parametru | Popis |
|---|---|
| -Operation | GetThroughtput |
|-Virtualization|Určuje typ virtualizace (VMware nebo Hyper-V).|
|-Directory|(Volitelné) Název UNC (Universal Naming Convention) nebo cesta k místnímu adresáři, ve kterém jsou uložena profilovaná data (soubory vytvořené během profilace). Tato data jsou vyžadovaná k vygenerování sestavy. Pokud název není zadaný, jako výchozí se použije adresář ProfiledData v aktuální cestě.|
| -StorageAccountName | Název účtu úložiště, který se použije k zjištění využité šířky pásma pro replikaci místních dat do Azure. Nástroj zjistí využitou šířku pásma tak, že do tohoto účtu úložiště nahrává testovací data. Účet úložiště musí být verze 1 pro obecné účely nebo úložiště verze 2 (verze 2 pro obecné účely). |
| -StorageAccountKey | Klíč účtu úložiště, který se použije pro přístup k účtu úložiště. Přejděte na Azure Portal > Účty úložiště > <*název účtu služby Storage*> > Nastavení > Přístupové klíče > Klíč1.|
| -VMListFile | Soubor se seznamem virtuálních počítačů určených k profilaci pro výpočet využité šířky pásma. Cesta k souboru může být absolutní nebo relativní. Pro Hyper-V je tento soubor výstupním souborem operace GetVMList. Pokud provádíte přípravu ručně, měl by tento soubor na každém řádku obsahovat jednu IP adresu nebo název serveru a za ním název virtuálního počítače oddělený \. Název virtuálního počítače zadaný v souboru se musí shodovat s názvem virtuálního počítače na hostiteli Hyper-V.<ul>Příklad: Soubor VMList.txt obsahuje následující virtuální počítače:<ul><li>Host_1\VM_A</li><li>10.8.59.27\VM_B</li><li>Host_2\VM_C</li><ul>|
|-Environment|(Volitelné) Toto je vaše cílové prostředí účtu Azure Storage. Může to být jedna ze tří hodnot – AzureCloud, AzureUSGovernment nebo AzureChinaCloud. Výchozí hodnota je AzureCloud. Tento parametr použijte, pokud vaší cílovou oblastí Azure jsou cloudy Azure US Government nebo Azure China.|


Nástroj vytvoří v zadaném adresáři několik souborů asrvhdfile<#>.vhd (kde # je počet souborů) o velikosti 64 MB. Nástroj tyto soubory nahraje do účtu úložiště a tak zjistí propustnost. Po změření propustnosti nástroj všechny tyto soubory odstraní z účtu úložiště i z místního serveru. Pokud se nástroj z nějakého důvodu během výpočtu propustnosti ukončí, z úložiště ani z místního serveru tyto soubory neodstraní. Budete je muset odstranit ručně.

Propustnost se měří k určitému bodu v čase a je to maximální propustnost, které Azure Site Recovery může během replikace dosáhnout za předpokladu, že všechny ostatní okolnosti budou stejné. Například pokud ve stejné síti nějaká aplikace začne využívat větší šířku pásma, skutečná propustnost během replikace se bude lišit. Výsledná naměřená propustnost se bude lišit, když operaci GetThroughput spustíte v době vysoké četnosti změn dat na chráněných virtuálních počítačích. Doporučujeme spouštět nástroj během profilace v různých okamžicích, abyste zjistili, jaké úrovně propustnosti můžete dosáhnout v různou dobu. V sestavě zobrazí nástroj poslední naměřenou propustnost.
    
### <a name="example"></a>Příklad
```
ASRDeploymentPlanner.exe -Operation GetThroughput -Virtualization Hyper-V -Directory E:\Hyp-erV_ProfiledData -VMListFile E:\Hyper-V_ProfiledData\ProfileVMList1.txt  -StorageAccountName  asrspfarm1 -StorageAccountKey by8vdM02xNOcqFlqUwJPLlmEtlCDXJ1OUzFT50uSRZ6IFsuFq2UVErCz4I6tq/K1SZFPTOtr/KBHBeksoGMGw==
```

Poznámka:
>
> Spouštějte nástroj na serveru, který má stejné charakteristiky úložiště a procesoru jako server Hyper-V.
>
> Pro účely replikace nastavte šířku pásma doporučenou pro splnění cíle bodu obnovení 100 % času. Pokud se po nastavení správné šířky pásma nezvýší dosažená propustnost, kterou nástroj hlásí, proveďte následující:
>
>  1. Zkontrolujte, jestli v síti není nastavená technologie QoS (Quality of Service), která by omezovala propustnost Azure Site Recovery.
>
>  2. Zkontrolujte, jestli je váš trezor Azure Site Recovery v nejbližší podporované fyzické oblasti Microsoft Azure kvůli minimalizaci latence sítě.
>
>  3. Zkontrolujte charakteristiky vašeho místního úložiště a zjistěte, jestli můžete vylepšit hardware (například použít jednotky SSD místo HDD).
>

## <a name="next-steps"></a>Další kroky
* [Analýza vygenerované sestavy](site-recovery-hyper-v-deployment-planner-analyze-report.md)