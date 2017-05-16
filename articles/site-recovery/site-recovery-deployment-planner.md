---
title: "Azure Site Recovery Deployment Planner pro nasazení VMware do Azure | Dokumentace Microsoftu"
description: "Toto je uživatelská příručka k Azure Site Recovery Deployment Planneru."
services: site-recovery
documentationcenter: 
author: nsoneji
manager: garavd
editor: 
ms.assetid: 
ms.service: site-recovery
ms.workload: backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 2/21/2017
ms.author: nisoneji
ms.translationtype: Human Translation
ms.sourcegitcommit: 18d4994f303a11e9ce2d07bc1124aaedf570fc82
ms.openlocfilehash: 5c716069bdff2a23bf81b2d2d0793a8616cf9c83
ms.contentlocale: cs-cz
ms.lasthandoff: 05/09/2017


---
# <a name="azure-site-recovery-deployment-planner"></a>Azure Site Recovery Deployment Planner
Tento článek představuje uživatelskou příručku k nástroji Azure Site Recovery Deployment Planner pro produkční nasazení VMware do Azure.

## <a name="overview"></a>Přehled

Než začnete chránit jakékoli virtuální počítače VMware pomocí Site Recovery, přidělte dostatečnou šířku pásma v závislosti na vaší denní frekvenci změn dat, abyste dosáhli požadovaného cíle bodu obnovení (RPO). Nezapomeňte místně nasadit správný počet konfiguračních serverů a procesových serverů.

Také je nutné vytvořit správný typ a počet cílových účtů služby Azure Storage. Vytvoříte účty služby Storage úrovně Standard nebo Premium, které budou zohledňovat nárůst počtu vašich zdrojových produkčních serverů způsobený zvyšováním využití v průběhu času. Typ úložiště zvolíte pro každý virtuální počítač na základě charakteristik úloh (jako je počet vstupně-výstupních operací [IOPS] čtení a zápisu za sekundu nebo četnost změn dat) a omezení Site Recovery.

Site Recovery Deployment Planner Public Preview je nástroj příkazového řádku aktuálně dostupný pouze pro scénář nasazení VMware do Azure. Pomocí tohoto nástroje můžete vzdáleně profilovat virtuální počítače VMware (bez jakéhokoli dopadu na produkční prostředí) a porozumět tak požadavkům na šířku pásma a službu Azure Storage pro úspěšnou replikaci a testovací převzetí služeb při selhání. Nástroj můžete spustit místně bez nutnosti instalace jakýchkoli komponent Site Recovery. Nicméně pro získání přesných výsledků dosažené propustnosti se doporučuje spustit Deployment Planner na Windows Serveru splňujícím minimální požadavky konfiguračního serveru Azure Site Recovery, který časem budete muset nasadit v jednom z prvních kroků produkčního nasazení.

Nástroj poskytuje následující podrobnosti:

**Posouzení kompatibility**

* Vyhodnocení způsobilosti virtuálního počítače na základě počtu disků, velikosti disků, počtu vstupně-výstupních operací za sekundu (IOPS, četnosti změn a typu spuštění (EFI nebo BIOS)
* Odhadovaná šířka pásma sítě potřebná pro rozdílovou replikaci

**Srovnání šířky pásma sítě a posouzení cíle bodu obnovení**

* Odhadovaná šířka pásma sítě potřebná pro rozdílovou replikaci
* Propustnost z místního prostředí do Azure, které Site Recovery může dosáhnout
* Počet virtuálních počítačů pro dávku na základě odhadované šířky pásma pro dokončení prvotní replikace v daném čase

**Požadavky na infrastrukturu Azure**

* Požadovaný typ úložiště (účet služby Storage úrovně Standard nebo Premium) pro každý virtuální počítač
* Celkový počet účtů služby Storage úrovně Standard a Premium, které se mají nastavit pro replikaci
* Návrhy pojmenování účtů úložiště na základě pokynů pro Azure Storage
* Umístění jednotlivých virtuálních počítačů v účtech úložiště
* Počet jader systému Azure, která se mají zřídit před testovacím převzetím služeb při selhání nebo převzetím služeb při selhání v rámci předplatného
* Doporučená velikost virtuálního počítače Azure pro každý místní virtuální počítač

**Požadavky na místní infrastrukturu**
* Požadovaný počet konfiguračních serverů a procesových serverů, které se mají místně nasadit

>[!IMPORTANT]
>
>Protože se využití časem bude pravděpodobně zvyšovat, všechny předchozí výpočty jsou provedeny s předpokladem 30% faktoru růstu v charakteristikách úloh a používají hodnoty 95. percentilu všech metrik profilace (počet vstupně-výstupních operací čtení a zápisu za sekundu [R/W IOPS], četnost změn atd.). Oba tyto elementy (faktor růstu a výpočet percentilu) je možné konfigurovat. Další informace o faktoru růstu najdete v části Aspekty faktoru růstu. Další informace o hodnotě percentilu najdete v části Hodnota percentilu používaná k výpočtu.
>

## <a name="requirements"></a>Požadavky
Nástroj má dvě hlavní fáze: profilace a generování sestav. Existuje také třetí možnost – výpočet pouze propustnosti. Požadavky na server, ze kterého se spouští profilace a měření propustnosti, jsou uvedené v následující tabulce:

| Požadavek na server | Popis|
|---|---|
|Profilace a měření propustnosti| <ul><li>Operační systém: Microsoft Windows Server 2012 R2<br>(Ideálně alespoň stejná velikost jako [doporučená velikost pro konfigurační server](https://aka.ms/asr-v2a-on-prem-components))</li><li>Konfigurace počítače: 8 virtuálních CPU, 16 GB paměti RAM, 300 GB HDD</li><li>[Microsoft .NET Framework 4.5](https://aka.ms/dotnet-framework-45)</li><li>[VMware vSphere PowerCLI 6.0 R3](https://developercenter.vmware.com/tool/vsphere_powercli/6.0)</li><li>[Microsoft Visual C++ Redistributable for Visual Studio 2012](https://aka.ms/vcplusplus-redistributable)</li><li>Internetový přístup k Azure z tohoto serveru</li><li>Účet služby Azure Storage</li><li>Přístup správce na server</li><li>Volné místo na disku alespoň 100 GB (za předpokladu 1 000 virtuálních počítačů, každý průměrně se 3 disky a profilovaný po dobu 30 dnů)</li><li>Úroveň statistiky VMware vCenter musí být nastavená na hodnotu 2 nebo vysokou úroveň.</li></ul>|
| Generování sestav | Počítač s Windows nebo Windows Server s aplikací Microsoft Excel 2013 nebo novější |
| Uživatelská oprávnění | Oprávnění jen ke čtení pro uživatelský účet používaný pro přístup k serveru VMware vCenter nebo k hostiteli VMware vSphere ESXi během profilace |

> [!NOTE]
>
>Nástroj může profilovat pouze virtuální počítače s disky VMDK a RDM. Nemůže profilovat virtuální počítače s disky iSCSI nebo NFS. Site Recovery podporuje disky iSCSI a NFS pro servery VMware, ale Deployment Planner nesídlí v hostu a profilaci provádí pouze pomocí čítačů výkonu vCenter, proto do těchto typů disků nevidí.
>

## <a name="download-and-extract-the-public-preview"></a>Stažení a rozbalení verze Public Preview
1. Stáhněte si nejnovější verzi nástroje [Site Recovery Deployment Planner Public Preview](https://aka.ms/asr-deployment-planner).  
Nástroje je zabalený ve složce .zip. Aktuální verze nástroje podporuje pouze scénář nasazení VMware do Azure.

2. Zkopírujte složku .zip na Windows Server, ze kterého chcete nástroj spustit.  
Nástroj můžete spustit z Windows Serveru 2012 R2, pokud má server přístup k internetu pro připojení k serveru vCenter nebo k hostiteli vSphere ESXi, na kterém sídlí virtuální počítače určené k profilaci. Doporučujeme však spustit nástroj na serveru, jehož konfigurace hardwaru odpovídá [pokynům k nastavení velikosti konfiguračního serveru](https://aka.ms/asr-v2a-on-prem-components). Pokud jste již místně nasadili komponenty Site Recovery, spusťte nástroj z konfiguračního serveru.

 Doporučujeme, abyste měli stejnou konfiguraci hardwaru konfiguračního serveru (který obsahuje integrovaný procesový server) a serveru, na kterém nástroj spouštíte. Taková konfigurace zajistí, že dosažená propustnost, kterou nástroj hlásí, bude odpovídat skutečné propustnosti, které může Site Recovery dosáhnout během replikace. Výpočet propustnosti závisí na dostupné šířce pásma sítě na serveru a na konfiguraci hardwaru (CPU, úložiště atd.) serveru. Pokud nástroj spustíte z nějakého jiného serveru, vypočítá se propustnost z tohoto serveru do Microsoft Azure. Navíc se může lišit konfigurace hardwaru tohoto serveru a konfiguračního serveru, proto dosažená propustnost, kterou nástroj hlásí, nemusí odpovídat skutečnosti.

3. Rozbalte složku .zip.  
Složka obsahuje několik souborů a podsložek. Spustitelný soubor je ASRDeploymentPlanner.exe v nadřazené složce.

    Příklad:  
    Zkopírujte soubor .zip na jednotku E:\ a rozbalte jej.
   E:\ASR Deployment Planner-Preview_v1.2.zip

    E:\ASR Deployment Planner-Preview_v1.2\ ASR Deployment Planner-Preview_v1.2\ ASRDeploymentPlanner.exe

## <a name="capabilities"></a>Možnosti
Nástroj příkazového řádku (ASRDeploymentPlanner.exe) můžete spustit v některém z následujících tří režimů:

1. Profilace  
2. Generování sestav
3. Zjištění propustnosti

Nejprve spusťte nástroj v režimu profilace, aby shromáždil četnost změn dat a počet IOPS virtuálních počítačů. Dále spusťte nástroj v režimu generování sestav, abyste zjistili požadavky na šířku pásma sítě a na úložiště.

## <a name="profiling"></a>Profilace
V režimu profilace se Deployment Planner připojí k serveru vCenter nebo k hostiteli vSphere ESXi za účelem shromáždění dat o výkonu virtuálního počítače.

* Profilace nemá vliv na výkon produkčních virtuálních počítačů, protože se k nim nevytváří žádné přímé připojení. Veškerá data o výkonu se shromažďují ze serveru vCenter nebo z hostitele vSphere ESXi.
* Aby se zajistilo, že profilace bude mít na server zanedbatelný dopad, nástroj provádí dotazování serveru vCenter nebo hostitele vSphere ESXi každých 15 minut. Tento interval dotazování však nesnižuje přesnost profilace, protože nástroj ukládá data čítače výkonu pro každou minutu.

### <a name="create-a-list-of-vms-to-profile"></a>Vytvoření seznamu virtuálních počítačů určených k profilaci
Nejprve potřebujete seznam virtuálních počítačů určených k profilaci. Všechny názvy virtuálních počítačů na serveru vCenter nebo hostiteli vSphere ESXi můžete získat pomocí příkazů rozhraní VMware vSphere PowerCLI v následujícím postupu. Případně můžete v souboru ručně vytvořit seznam popisných názvů nebo IP adres virtuálních počítačů, které chcete profilovat.

1. Přihlaste se k virtuálnímu počítači, na kterém je nainstalované rozhraní VMware vSphere PowerCLI.
2. Otevřete konzolu VMware vSphere PowerCLI.
3. Zkontrolujte, že jsou pro skript povolené zásady spouštění. Pokud jsou zakázané, spusťte konzolu VMware vSphere PowerCLI v režimu správce a povolte je spuštěním následujícího příkazu:

            Set-ExecutionPolicy –ExecutionPolicy AllSigned

4. Chcete-li získat všechny názvy virtuálních počítačů na serveru vCenter nebo hostiteli vSphere ESXi a jejich seznam uložit do souboru .txt, spusťte zde uvedené dva příkazy.
Nahraďte zástupné hodnoty &lsaquo;server name&rsaquo; (název serveru), &lsaquo;user name&rsaquo; (uživatelské jméno), &lsaquo;password&rsaquo; (heslo) a &lsaquo;outputfile.txt&rsaquo; (výstupní soubor) vlastními hodnotami.

            Connect-VIServer -Server <server name> -User <user name> -Password <password>

            Get-VM |  Select Name | Sort-Object -Property Name >  <outputfile.txt>

5. Otevřete výstupní soubor v Poznámkovém bloku a zkopírujte názvy všech virtuálních počítačů, které chcete profilovat, do jiného souboru (například ProfileVMList.txt), přičemž každý název virtuálního počítače musí být na samostatném řádku. Tento soubor se použije jako vstup pro parametr *-VMListFile* nástroje příkazového řádku.

    ![Seznam názvů virtuálních počítačů v Deployment Planneru](./media/site-recovery-deployment-planner/profile-vm-list.png)

### <a name="start-profiling"></a>Spuštění profilace
Jakmile budete mít seznam virtuálních počítačů určených k profilaci, můžete nástroj spustit v režimu profilace. Zde je seznam povinných a volitelných parametrů pro spuštění nástroje v režimu profilace.

ASRDeploymentPlanner.exe -Operation StartProfiling /?

| Název parametru | Popis |
|---|---|
| -Operation | StartProfiling |
| -Server | Plně kvalifikovaný název domény nebo IP adresa serveru vCenter nebo hostitele vSphere ESXi, které obsahují virtuální počítače určené k profilaci.|
| -User | Uživatelské jméno pro připojení k serveru vCenter nebo hostiteli vSphere ESXi. Uživatel musí mít alespoň přístup jen ke čtení.|
| -VMListFile |    Soubor se seznamem virtuálních počítačů určených k profilaci. Cesta k souboru může být absolutní nebo relativní. Soubor musí na každém řádku obsahovat jeden název nebo IP adresu virtuálního počítače. Název virtuálního počítače zadaný v souboru se musí shodovat s názvem virtuálního počítače na serveru vCenter nebo hostiteli vSphere ESXi.<br>Například soubor VMList.txt obsahuje následující virtuální počítače:<ul><li>virtual_machine_A</li><li>10.150.29.110</li><li>virtual_machine_B</li><ul> |
| -NoOfDaysToProfile | Počet dní, po které má být profilace spuštěna. Doporučujeme profilaci spouštět na více než 15 dnů, během kterých se zjistí vzorce úloh ve vašem prostředí a s jejich pomocí nástroj poskytne přesná doporučení. |
| -Directory | (Volitelné) Název UNC (Universal Naming Convention) nebo cesta k místnímu adresáři, do kterého se během profilace budou ukládat data profilace. Pokud název adresáře není zadaný, jako výchozí se použije adresář ProfiledData v aktuální cestě. |
| -Password | (Volitelné) Heslo, které se použije pro připojení k serveru vCenter nebo hostiteli vSphere ESXi. Pokud heslo nezadáte teď, budete k tomu vyzváni při spuštění příkazu.|
| -StorageAccountName | (Volitelné) Název účtu úložiště, který se použije k zjištění dosažitelné propustnost pro replikaci místních dat do Azure. Nástroj vypočítává propustnost tak, že do tohoto účtu úložiště nahrává testovací data.|
| -StorageAccountKey | (Volitelné) Klíč účtu úložiště, který se použije pro přístup k účtu úložiště. Přejděte na web Azure Portal > Účty úložiště > <*název účtu služby Storage*> > Nastavení > Přístupové klíče > Klíč1 (nebo primární přístupový klíč v případě klasického účtu úložiště). |
| -Environment | (Volitelné) Toto je vaše cílové prostředí účtu Azure Storage. Může to být jedna ze tří hodnot – AzureCloud, AzureUSGovernment a AzureChinaCloud. Výchozí hodnota je AzureCloud. Tento parametr použijte, pokud vaší cílovou oblastí Azure jsou cloudy Azure US Government nebo Azure China. |


Doporučujeme profilovat virtuální počítače po dobu alespoň 15 až 30 dnů. Během období profilace je ASRDeploymentPlanner.exe stále spuštěný. Nástroj na vstupu přijímá zadání času profilace ve dnech. Pokud chcete nástroj rychle otestovat spuštěním profilace na několik hodin nebo minut, ve verzi Public Preview budete muset tento čas převést na odpovídající zlomek dne. Pokud například chcete profilovat po dobu 30 minut, vstup musí být 30/(60×24) = 0,021 dnů. Minimální povolený čas profilace je 30 minut.

Během profilace můžete volitelně předat název a klíč účtu úložiště a zjistit tak propustnost, které může Site Recovery dosáhnout v době replikace z konfiguračního serveru nebo procesového serveru do Azure. Pokud název a klíč účtu úložiště během profilace nepředáte, nástroj dosažitelnou propustnost počítat nebude.

Můžete spouštět více instancí nástroje pro různé sady virtuálních počítačů. Zkontrolujte, že se názvy virtuálních počítačů v sadách profilace neopakují. Pokud například profilujete deset virtuálních počítačů (VM1–VM10) a po několika dnech chcete profilovat dalších pět virtuálních počítačů (VM11–VM15), můžete nástroj spustit z jiné konzoly příkazového řádku pro druhou sadu virtuálních počítačů (VM11–VM15). Zajistěte, aby druhá sada virtuálních počítačů neobsahovala žádné názvy virtuálních počítačů z první instance profilace, nebo pro druhé spuštění použijte jiný výstupní adresář. Pokud se k profilování stejných virtuálních počítačů používají dvě instance nástroje a stejný výstupní adresář, vygenerovaná sestava bude nepřesná.

Konfigurace virtuálních počítačů se zachytí jednou na začátku operace profilace a uloží se do souboru VMDetailList.xml. Tyto informace se použijí při generování sestavy. Žádné změny v konfiguraci virtuálních počítačů (například navýšení počtu jader, disků nebo síťových adaptérů) od začátku do konce profilace se nezachytí. Pokud se během profilace změnila konfigurace některého profilovaného virtuálního počítače, ve verzi Public Preview existuje alternativní řešení, pomocí kterého můžete získat nejnovější podrobnosti o virtuálním počítači bez nutnosti generovat sestavu:

* Zálohujte soubor VMdetailList.xml a odstraňte ho z aktuálního umístění.
* V okamžiku generování sestav předejte argumenty -User a -Password.

Příkaz profilace vygeneruje v adresáři profilace několik souborů. Žádný z těchto souborů neodstraňujte, jinak to bude mít dopad na generování sestav.

#### <a name="example-1-profile-vms-for-30-days-and-find-the-throughput-from-on-premises-to-azure"></a>Příklad 1: Profilování virtuálních počítačů po dobu 30 dnů a zjištění propustnosti z místního prostředí do Azure
```
ASRDeploymentPlanner.exe -Operation StartProfiling -Directory “E:\vCenter1_ProfiledData” -Server vCenter1.contoso.com -VMListFile “E:\vCenter1_ProfiledData\ProfileVMList1.txt”  -NoOfDaysToProfile  30  -User vCenterUser1 -StorageAccountName  asrspfarm1 -StorageAccountKey Eby8vdM02xNOcqFlqUwJPLlmEtlCDXJ1OUzFT50uSRZ6IFsuFq2UVErCz4I6tq/K1SZFPTOtr/KBHBeksoGMGw==
```

#### <a name="example-2-profile-vms-for-15-days"></a>Příklad 2: Profilování virtuálních počítačů po dobu 15 dnů

```
ASRDeploymentPlanner.exe -Operation StartProfiling -Directory “E:\vCenter1_ProfiledData” -Server vCenter1.contoso.com -VMListFile “E:\vCenter1_ProfiledData\ProfileVMList1.txt”  -NoOfDaysToProfile  15  -User vCenterUser1
```

#### <a name="example-3-profile-vms-for-1-hour-for-a-quick-test-of-the-tool"></a>Příklad 3: Rychlé otestování nástroje profilováním virtuálních počítačů po dobu 1 hodiny
```
ASRDeploymentPlanner.exe -Operation StartProfiling -Directory “E:\vCenter1_ProfiledData” -Server vCenter1.contoso.com -VMListFile “E:\vCenter1_ProfiledData\ProfileVMList1.txt”  -NoOfDaysToProfile  0.04  -User vCenterUser1
```

>[!NOTE]
>
>* Pokud se server, na kterém je nástroj spuštěný, restartuje nebo na něm dojde k chybě, nebo pokud nástroj zavřete stisknutím kombinace kláves Ctrl + C, profilovaná data se zachovají. Může se však stát, že kvůli tomu bude chybět posledních 15 minut profilovaných dat. V takové situaci po restartování serveru znovu spusťte nástroj v režimu profilace.
>* Pokud předáte název a klíč účtu úložiště, nástroj bude měřit propustnost v posledním kroku profilace. Pokud bude nástroj zavřen před dokončením profilace, propustnost se nevypočítá. Pokud chcete zjistit propustnost před generováním sestavy, můžete z konzoly příkazového řádku spustit operaci GetThroughput. Jinak vygenerovaná sestava nebude obsahovat informace o propustnosti.


## <a name="generate-a-report"></a>Generování sestav
Nástroj jako výstup sestavy generuje soubor aplikace Microsoft Excel s podporou maker (soubor XLSM), který shrnuje veškerá doporučení pro nasazení. Sestava má název DeploymentPlannerReport_<*jedinečný číselný identifikátor*>.xlsm a je umístěná v zadaném adresáři.

Po dokončení profilace můžete nástroj spustit v režimu generování sestav. Následující tabulka obsahuje seznam povinných a volitelných parametrů nástroje pro spuštění v režimu generování sestav.

`ASRDeploymentPlanner.exe -Operation GenerateReport /?`

|Název parametru | Popis |
|-|-|
| -Operation | GenerateReport |
| -Server |  Plně kvalifikovaný název domény nebo IP adresa serveru vCenter nebo vSphere (použijte stejný název nebo IP adresu, jako jste použili při profilaci), na němž jsou umístěné profilované virtuální počítače, pro které se má vygenerovat sestava. Všimněte si, že pokud jste při profilaci použili server vCenter, nemůžete pro generování sestav použít server vSphere a naopak.|
| -VMListFile | Soubor se seznamem profilovaných virtuálních počítačů, pro které se má vygenerovat sestava. Cesta k souboru může být absolutní nebo relativní. Soubor musí na každém řádku obsahovat jeden název nebo IP adresu virtuálního počítače. Názvy virtuálních počítačů zadané v tomto souboru musí být stejné jako názvy virtuálních počítačů na serveru vCenter nebo hostiteli vSphere ESXi a musí se shodovat s názvy, které jste použili při profilaci.|
| -Directory | (Volitelné) Název UNC nebo cesta k místnímu adresáři, ve kterém jsou uložena profilovaná data (soubory vytvořené během profilace). Tato data jsou vyžadovaná k vygenerování sestavy. Pokud název nezadáte, použije se adresář ProfiledData. |
| -GoalToCompleteIR | (Volitelné) Počet hodin, během kterých je potřeba dokončit prvotní replikaci profilovaných virtuálních počítačů. Vygenerovaná sestava obsahuje počet virtuálních počítačů, pro které lze v zadaném čase dokončit prvotní replikaci. Výchozí hodnota je 72 hodin. |
| -User | (Volitelné) Uživatelské jméno pro připojení k serveru vCenter nebo vSphere. Uživatelské jméno slouží k načtení nejnovějších informací o konfiguraci virtuálních počítačů (např. počet disků, počet jader a počet síťových adaptérů), které se použijí v sestavě. Pokud uživatelské jméno nezadáte, použijí se informace o konfiguraci shromážděné na začátku profilace. |
| -Password | (Volitelné) Heslo, které se použije pro připojení k serveru vCenter nebo hostiteli vSphere ESXi. Pokud heslo nezadáte jako parametr, budete k jeho zadání vyzváni později při spuštění příkazu. |
| -DesiredRPO | (Volitelné) Požadovaný cíl bodu obnovení v minutách. Výchozí hodnota je 15 minut.|
| -Bandwidth | Šířka pásma v Mb/s. Tento parametr se použije k výpočtu cíle bodu obnovení, kterého lze pro zadanou šířku pásma dosáhnout. |
| -StartDate | (Volitelné) Počáteční datum a čas ve formátu MM-DD-YYYY:HH:MM (ve 24hodinovém formátu). Parametr *StartDate* je nutné zadat společně s parametrem *EndDate*. Pokud zadáte parametr StartDate, sestava se vygeneruje pro profilovaná data shromážděná mezi StartDate a EndDate. |
| -EndDate | (Volitelné) Koncové datum a čas ve formátu MM-DD-YYYY:HH:MM (ve 24hodinovém formátu). Parametr *EndDate* je nutné zadat společně s parametrem *StartDate*. Pokud zadáte parametr EndDate, sestava se vygeneruje pro profilovaná data shromážděná mezi StartDate a EndDate. |
| -GrowthFactor | (Volitelné) Faktor růstu vyjádřený v procentech. Výchozí hodnota je 30 procent. |
| -UseManagedDisks | (Volitelné) UseManagedDisks – Yes/No (Ano/Ne). Výchozí hodnota je Yes (Ano). Počet virtuálních počítačů, které lze umístit do jednoho účtu úložiště, se počítá na základě toho, jestli je pro převzetí služeb při selhání nebo testovací převzetí služeb při selhání vybrán spravovaný disk. |

umístění do jednoho účtu úložiště se vypočítá s ohledem na to, že převzetí služeb při selhání nebo testovací převzetí služeb při selhání virtuálních počítačů se provádí na spravovaný disk namísto nespravovaného disku. |


#### <a name="example-1-generate-a-report-with-default-values-when-the-profiled-data-is-on-the-local-drive"></a>Příklad 1: Generování sestavy s použitím výchozích hodnot pro profilovaná data umístěná na místním disku
```
ASRDeploymentPlanner.exe -Operation GenerateReport -Server vCenter1.contoso.com -Directory “\\PS1-W2K12R2\vCenter1_ProfiledData” -VMListFile “\\PS1-W2K12R2\vCenter1_ProfiledData\ProfileVMList1.txt”
```

#### <a name="example-2-generate-a-report-when-the-profiled-data-is-on-a-remote-server"></a>Příklad 2: Generování sestavy pro profilovaná data umístěná na vzdáleném serveru
Ke vzdálenému adresáři musíte mít přístup ke čtení a zápisu.
```
ASRDeploymentPlanner.exe -Operation GenerateReport -Server vCenter1.contoso.com -Directory “\\PS1-W2K12R2\vCenter1_ProfiledData” -VMListFile “\\PS1-W2K12R2\vCenter1_ProfiledData\ProfileVMList1.txt”
```

#### <a name="example-3-generate-a-report-with-a-specific-bandwidth-and-goal-to-complete-ir-within-specified-time"></a>Příklad 3: Generování sestavy s použitím konkrétní šířky pásma a cíle dokončení prvotní replikace v zadaném čase
```
ASRDeploymentPlanner.exe -Operation GenerateReport -Server vCenter1.contoso.com -Directory “E:\vCenter1_ProfiledData” -VMListFile “E:\vCenter1_ProfiledData\ProfileVMList1.txt” -Bandwidth 100 -GoalToCompleteIR 24
```

#### <a name="example-4-generate-a-report-with-a-5-percent-growth-factor-instead-of-the-default-30-percent"></a>Příklad 4: Generování sestavy s použitím 5% faktoru růstu namísto výchozích 30 %
```
ASRDeploymentPlanner.exe -Operation GenerateReport -Server vCenter1.contoso.com -Directory “E:\vCenter1_ProfiledData” -VMListFile “E:\vCenter1_ProfiledData\ProfileVMList1.txt” -GrowthFactor 5
```

#### <a name="example-5-generate-a-report-with-a-subset-of-profiled-data"></a>Příklad 5: Generování sestavy s použitím podmnožiny profilovaných dat
Máte například profilovaná data za 30 dnů a chcete vygenerovat sestavu pouze pro 20 dnů.
```
ASRDeploymentPlanner.exe -Operation GenerateReport -Server vCenter1.contoso.com -Directory “E:\vCenter1_ProfiledData” -VMListFile “E:\vCenter1_ProfiledData\ProfileVMList1.txt” -StartDate  01-10-2017:12:30 -EndDate 01-19-2017:12:30
```

#### <a name="example-6-generate-a-report-for-5-minute-rpo"></a>Příklad 6: Generování sestavy s použitím 5minutového cíle bodu obnovení
```
ASRDeploymentPlanner.exe -Operation GenerateReport -Server vCenter1.contoso.com -Directory “E:\vCenter1_ProfiledData” -VMListFile “E:\vCenter1_ProfiledData\ProfileVMList1.txt”  -DesiredRPO 5
```

## <a name="percentile-value-used-for-the-calculation"></a>Hodnota percentilu používaná k výpočtu
**Jakou výchozí hodnotu percentilu metrik výkonu shromážděných během profilace nástroj používá při generování sestavy?**

Nástroj ve výchozím nastavení používá hodnoty 95. percentilu počtu R/W IOPS, vstupně-výstupních operací zápisu za sekundu a četnosti změn dat shromážděných během profilace všech virtuálních počítačů. Tato metrika zajišťuje, že se k určení požadavků na cílový účet úložiště a zdrojovou šířku pásma nepoužijí hodnoty 100. percentilu (špičky), které se můžou objevovat na virtuálních počítačích následkem dočasných událostí. Příkladem dočasné události může být úloha zálohování spouštěná jednou denně, pravidelné indexování databáze, aktivita generování analytických sestav nebo další podobné krátkodobé a jednorázové události.

Použitím hodnot 95. percentilu získáte pravdivou představu o skutečných charakteristikách úloh a nejlepší výkon při spouštění těchto úloh v Azure. Neočekáváme, že byste toto číslo potřebovali měnit. Pokud tuto hodnotu přesto změníte (například na 90. percentil), můžete upravit konfigurační soubor *ASRDeploymentPlanner.exe.config* ve výchozí složce a uložit jej. Tím vygenerujete novou sestavu pro existující profilovaná data.
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
* Výsledná zvýšená četnost změn na virtuálním počítači bude vyžadovat přesun virtuálního počítače do účtu služby Storage úrovně Premium, aby replikace Site Recovery mohla s četností změn držet krok.
* V důsledku toho budete muset zakázat a znovu povolit ochranu účtu služby Storage úrovně Premium.

Důrazně doporučujeme, abyste při plánování nasazení počítali s růstem, a přestože je jeho hodnota ve výchozím nastavení 30 %, sami nejlépe znáte vzorce používání vaší aplikace a očekávaný růst a během generování sestav můžete toto číslo odpovídajícím způsobem měnit. Kromě toho můžete generovat více sestav s použitím různých faktorů růstu pro stejná profilovaná data a určit, jaká doporučení ohledně cílového úložiště a zdrojové šířky pásma jsou pro vás nejvhodnější.

Vygenerovaná sestava aplikace Microsoft Excel obsahuje následující informace:

* [Input](site-recovery-deployment-planner.md#input) (Vstup)
* [Recommendations](site-recovery-deployment-planner.md#recommendations-with-desired-rpo-as-input) (Doporučení)
* [Recommendations-Bandwidth Input](site-recovery-deployment-planner.md#recommendations-with-available-bandwidth-as-input) (Doporučení – Vstupní šířka pásma)
* [VM<->Storage Placement](site-recovery-deployment-planner.md#vm-storage-placement) (Umístění virtuálních počítačů ve službě Storage)
* [Compatible VMs](site-recovery-deployment-planner.md#compatible-vms) (Kompatibilní virtuální počítače)
* [Incompatible VMs](site-recovery-deployment-planner.md#incompatible-vms) (Nekompatibilní virtuální počítače)

![Deployment Planner](./media/site-recovery-deployment-planner/dp-report.png)

## <a name="get-throughput"></a>Zjištění propustnosti

Pokud chcete odhadnout propustnost, které může Site Recovery dosáhnout během replikace z místního prostředí do Azure, spusťte nástroj v režimu GetThroughput. Nástroj vypočítá propustnost ze serveru, na kterém je spuštěný. Ideálně je tento server vytvořený podle pokynů k nastavení konfiguračního serveru. Pokud jste již místně nasadili komponenty infrastruktury Site Recovery, spusťte nástroj na konfiguračním serveru.

Otevřete konzolu příkazového řádku a přejděte do složky nástroje pro plánování nasazení Site Recovery. Spusťte ASRDeploymentPlanner.exe s použitím následujících parametrů.

`ASRDeploymentPlanner.exe -Operation GetThroughput /?`

|Název parametru | Popis |
|-|-|
| -Operation | GetThroughput |
| -Directory | (Volitelné) Název UNC nebo cesta k místnímu adresáři, ve kterém jsou uložena profilovaná data (soubory vytvořené během profilace). Tato data jsou vyžadovaná k vygenerování sestavy. Pokud název adresáře není zadaný, použije se adresář ProfiledData. |
| -StorageAccountName | Název účtu úložiště, který se použije k zjištění využité šířky pásma pro replikaci místních dat do Azure. Nástroj zjistí využitou šířku pásma tak, že do tohoto účtu úložiště nahrává testovací data. |
| -StorageAccountKey | Klíč účtu úložiště, který se použije pro přístup k účtu úložiště. Přejděte na web Azure Portal > Účty úložiště > <*název účtu služby Storage*> > Nastavení > Přístupové klíče > Klíč1 (nebo primární přístupový klíč v případě klasického účtu úložiště). |
| -VMListFile | Soubor se seznamem virtuálních počítačů určených k profilaci pro výpočet využité šířky pásma. Cesta k souboru může být absolutní nebo relativní. Soubor musí na každém řádku obsahovat jeden název nebo IP adresu virtuálního počítače. Názvy virtuálních počítačů zadané v souboru se musí shodovat s názvy virtuálních počítačů na serveru vCenter nebo hostiteli vSphere ESXi.<br>Například soubor VMList.txt obsahuje následující virtuální počítače:<ul><li>VM_A</li><li>10.150.29.110</li><li>VM_B</li></ul>|
| -Environment | (Volitelné) Toto je vaše cílové prostředí účtu Azure Storage. Může to být jedna ze tří hodnot – AzureCloud, AzureUSGovernment a AzureChinaCloud. Výchozí hodnota je AzureCloud. Tento parametr použijte, pokud vaší cílovou oblastí Azure jsou cloudy Azure US Government nebo Azure China. |

Nástroj vytvoří v zadaném adresáři několik souborů „asrvhdfile<#>.vhd“ (kde # je počet souborů) o velikosti 64 MB. Nástroj tyto soubory nahraje do účtu úložiště a tak zjistí propustnost. Po změření propustnosti nástroj všechny tyto soubory odstraní z účtu úložiště i z místního serveru. Pokud se nástroj z nějakého důvodu během výpočtu propustnosti ukončí, z úložiště ani z místního serveru tyto soubory neodstraní. Budete je muset odstranit ručně.

Propustnost se měří k určitému bodu v čase a je to maximální propustnost, které Site Recovery může během replikace dosáhnout za předpokladu, že všechny ostatní okolnosti budou stejné. Například pokud ve stejné síti nějaká aplikace začne využívat větší šířku pásma, skutečná propustnost během replikace se bude lišit. Pokud spustíte příkaz GetThroughput z konfiguračního serveru, nástroj nebude vědět o žádných chráněných virtuálních počítačích ani probíhajících replikacích. Výsledná naměřená propustnost se bude lišit, když operaci GetThroughput spustíte v době vysoké četnosti změn dat na chráněných virtuálních počítačích. Doporučujeme spouštět nástroj během profilace v různých okamžicích, abyste zjistili, jaké úrovně propustnosti můžete dosáhnout v různou dobu. V sestavě zobrazí nástroj poslední naměřenou propustnost.

### <a name="example"></a>Příklad
```
ASRDeploymentPlanner.exe -Operation GetThroughput -Directory  E:\vCenter1_ProfiledData -VMListFile E:\vCenter1_ProfiledData\ProfileVMList1.txt  -StorageAccountName  asrspfarm1 -StorageAccountKey by8vdM02xNOcqFlqUwJPLlmEtlCDXJ1OUzFT50uSRZ6IFsuFq2UVErCz4I6tq/K1SZFPTOtr/KBHBeksoGMGw==
```

>[!NOTE]
>
> Spouštějte nástroj na serveru, který má stejné charakteristiky úložiště a procesoru jako konfigurační server.
>
> Pro účely replikace nastavte šířku pásma doporučenou pro splnění cíle bodu obnovení 100 % času. Pokud se po nastavení správné šířky pásma nezvýší dosažená propustnost, kterou nástroj hlásí, proveďte následující:
>
>  1. Zkontrolujte, jestli v síti není nastavená technologie QoS (Quality of Service), která by omezovala propustnost Site Recovery.
>
>  2. Zkontrolujte, jestli je váš trezor Site Recovery v nejbližší podporované fyzické oblasti Microsoft Azure kvůli minimalizaci latence sítě.
>
>  3. Zkontrolujte charakteristiky vašeho místního úložiště a zjistěte, jestli můžete vylepšit hardware (například použít jednotky SSD místo HDD).
>
>  4. Změňte nastavení Site Recovery na procesovém serveru a [zvětšete šířku pásma sítě používanou k replikaci](./site-recovery-plan-capacity-vmware.md#control-network-bandwidth).

## <a name="recommendations-with-desired-rpo-as-input"></a>Doporučení s možností zadat požadovaný cíl bodu obnovení jako vstup

### <a name="profiled-data"></a>Profilovaná data

![Zobrazení profilovaných dat v Deployment Planneru](./media/site-recovery-deployment-planner/profiled-data-period.png)

**Profiled data period:** Doba, po kterou byla profilace spuštěná. Nástroj ve výchozím nastavení do výpočtu zahrnuje všechna profilovaná data, pokud sestavu negeneruje pro konkrétní období díky použití možností StartDate a EndDate během generování sestavy.

**Server Name:** Název nebo IP adresa hostitele ESXi nebo VMware vCenter, pro jehož virtuální počítače se sestava generuje.

**Desired RPO:** Požadovaný cíl bodu obnovení pro vaše nasazení. Ve výchozím nastavení se požadovaná šířka pásma sítě počítá pro hodnoty cíle bodu obnovení 15, 30 a 60 minut. V závislosti na výběru se na listu aktualizují ovlivněné hodnoty. Pokud jste při generování sestavy použili parametr *DesiredRPOinMin*, jako výsledný požadovaný cíl bodu obnovení se zobrazí použitá hodnota.

### <a name="profiling-overview"></a>Přehled profilace

![Výsledky profilace v Deployment Planneru](./media/site-recovery-deployment-planner/profiling-overview.png)

**Total Profiled Virtual Machines:** Celkový počet virtuálních počítačů, jejichž profilovaná data jsou k dispozici. Pokud soubor VMListFile obsahuje názvy virtuálních počítačů, které nebyly profilované, nebude se na takové virtuální počítače brát ohled při generování sestav a nezahrnou se do celkového počtu profilovaných virtuálních počítačů.

**Compatible Virtual Machines:** Počet virtuálních počítačů, které lze chránit v Azure pomocí Site Recovery. Je to celkový počet kompatibilních virtuálních počítačů, pro které se počítá požadovaná šířka pásma sítě, požadovaný počet účtů úložiště, počet jader Azure a počet konfiguračních serverů a dalších procesových serverů. Podrobnosti o každém kompatibilním virtuálním počítači jsou dostupné v části Compatible VMs.

**Incompatible Virtual Machines:** Počet profilovaných virtuálních počítačů, které jsou nekompatibilní s ochranou pomocí Site Recovery. Důvody nekompatibility jsou uvedené v části Incompatible VMs. Pokud soubor VMListFile obsahuje názvy virtuálních počítačů, které nebyly profilované, nezahrnou se do celkového počtu nekompatibilních virtuálních počítačů. Takové virtuální počítače jsou uvedené jako „Data not found“ (Data nenalezena) na konci části Incompatible VMs.

**Desired RPO:** Požadovaný cíl bodu obnovení v minutách. Sestava se generuje pro tři hodnoty cíle bodu obnovení: 15 (výchozí), 30 a 60 minut. Doporučení šířky pásma v sestavě se mění v závislosti na výběru možnosti v rozevíracím seznamu Desired RPO v pravé horní části listu. Pokud jste sestavu vygenerovali s použitím parametru *-DesiredRPO* s vlastní hodnotou, v rozevíracím seznamu Desired RPO se tato vlastní hodnota zobrazí jako výchozí možnost.

### <a name="required-network-bandwidth-mbps"></a>Požadovaná šířka pásma sítě (Mb/s)

![Požadovaná šířka pásma sítě v Deployment Planneru](./media/site-recovery-deployment-planner/required-network-bandwidth.png)

**To meet RPO 100 percent of the time** (Pro splnění cíle bodu obnovení 100 % času): Doporučená šířka pásma v Mb/s, kterou je potřeba přidělit pro splnění požadovaného cíle bodu obnovení 100 % času. Šířka pásma musí být vyhrazená pro zajištění stálé rozdílové replikace všech kompatibilních virtuálních počítačů, aby se předešlo jakémukoli narušení cíle bodu obnovení.

**To meet RPO 90 percent of the time** (Pro splnění cíle bodu obnovení 90 % času): Pokud kvůli cenám širokopásmového připojení nebo z jiného důvodu nemůžete nastavit šířku pásma potřebnou ke splnění požadovaného cíle bodu obnovení 100 % času, můžete se rozhodnout pro nastavení menší šířky pásma, která může splňovat požadovaný cíl bodu obnovení 90 % času. Abyste porozuměli dopadům nastavení menší šířky pásma, sestava poskytuje analýzu „co kdyby“ očekávaného počtu a trvání narušení cíle bodu obnovení.

**Achieved Throughput** (Dosažená propustnost): Propustnost ze serveru, na kterém jste spustili příkaz GetThroughput, do oblasti Microsoft Azure, ve které je umístěný účet úložiště. Tato hodnota propustnosti označuje odhadovanou úroveň propustnosti, které můžete dosáhnout při ochraně kompatibilních virtuálních počítačů pomocí Site Recovery za předpokladu, že charakteristiky sítě a úložiště konfiguračního serveru nebo procesového serveru budou stejné jako u serveru, ze kterého jste nástroj spustili.

Pro účely replikace byste měli nastavit šířku pásma doporučenou pro splnění cíle bodu obnovení 100 % času. Pokud se po nastavení šířky pásma nezvýší dosažená propustnost, kterou nástroj hlásí, proveďte následující:

1. Zkontrolujte, jestli v síti není nastavená technologie QoS (Quality of Service), která by omezovala propustnost Site Recovery.

2. Zkontrolujte, jestli je váš trezor Site Recovery v nejbližší podporované fyzické oblasti Microsoft Azure kvůli minimalizaci latence sítě.

3. Zkontrolujte charakteristiky vašeho místního úložiště a zjistěte, jestli můžete vylepšit hardware (například použít jednotky SSD místo HDD).

4. Změňte nastavení Site Recovery na procesovém serveru a [zvětšete šířku pásma sítě používanou k replikaci](./site-recovery-plan-capacity-vmware.md#control-network-bandwidth).

Pokud nástroj spouštíte na konfiguračním serveru nebo procesovém serveru, na kterém již jsou chráněné virtuální počítače, spusťte nástroj několikrát. Hodnota dosažené propustnosti se mění v závislosti na četnosti změn, které se v daném okamžiku zpracovávají.

Pro všechna podniková nasazení Site Recovery doporučujeme použít [ExpressRoute](https://aka.ms/expressroute).

### <a name="required-storage-accounts"></a>Požadované účty úložiště
Následující graf znázorňuje celkový počet účtů služby Storage (úrovně Standard a Premium) požadovaných k ochraně všech kompatibilních virtuálních počítačů. Informace o tom, jaký účet úložiště použít pro jednotlivé virtuální počítače, najdete v části VM-storage placement.

![Požadované účty úložiště v Deployment Planneru](./media/site-recovery-deployment-planner/required-azure-storage-accounts.png)

### <a name="required-number-of-azure-cores"></a>Požadovaný počet jader Azure
Tento výsledek je celkový počet jader, která se mají nastavit před převzetím služeb při selhání nebo testovacím převzetím služeb při selhání všech kompatibilních virtuálních počítačů. Pokud v rámci předplatného není k dispozici dostatečný počet jader, Site Recovery během testovacího převzetí služeb při selhání nebo převzetí služeb při selhání nebude moci vytvořit virtuální počítače.

![Požadovaný počet jader Azure v Deployment Planneru](./media/site-recovery-deployment-planner/required-number-of-azure-cores.png)

### <a name="required-on-premises-infrastructure"></a>Požadovaná místní infrastruktura
Toto číslo je celkový počet konfiguračních serverů a dalších procesových serverů, které se mají nakonfigurovat a které budou stačit k zajištění ochrany všech kompatibilních virtuálních počítačů. V závislosti na podporované [doporučené velikosti pro konfigurační server](https://aka.ms/asr-v2a-on-prem-components) může nástroj doporučit přidání dalších serverů. Doporučení je založeno na větší z hodnot denní četnosti změn a maximálního počtu chráněných virtuálních počítačů (za předpokladu, že každý z nich obsahuje průměrně tři disky), podle toho, čeho konfigurační server nebo další procesový server dosáhne dříve. Podrobnosti o celkové denní četnosti změn a celkovém počtu chráněných disků najdete v části Input.

![Požadovaná místní infrastruktura v Deployment Planneru](./media/site-recovery-deployment-planner/required-on-premises-infrastructure.png)

### <a name="what-if-analysis"></a>Analýza „co kdyby“
Tato analýza ukazuje, ke kolika narušením by mohlo dojít během období profilace, pokud nastavíte menší šířku pásma pro splnění požadovaného cíle bodu obnovení pouze 90 % času. Každý den může dojít k jednomu nebo několika narušením cíle bodu obnovení. Graf ukazuje pro každý den špičku cíle bodu obnovení.
Na základě této analýzy se můžete rozhodnout, jestli je se zadanou menší šířkou pásma přijatelný počet narušení cíle bodu obnovení ve všech dnech a denní dosažená špička cíle bodu obnovení. Pokud je to přijatelné, můžete pro replikaci přidělit menší šířku pásma, jinak podle návrhu přidělte větší šířku pásma pro splnění požadovaného cíle bodu obnovení 100 % času.

![Analýza „co kdyby“ v Deployment Planneru](./media/site-recovery-deployment-planner/what-if-analysis.png)

### <a name="recommended-vm-batch-size-for-initial-replication"></a>Doporučená velikost dávky virtuálních počítačů pro prvotní replikaci
V této části najdete doporučený počet virtuálních počítačů, které lze paralelně chránit, pro dokončení prvotní replikace během 72 hodin, a navrhovanou šířku pásma pro splnění požadovaného cíle bodu obnovení 100 % času. Tato hodnota je konfigurovatelná. Můžete ji změnit při generování sestavy pomocí parametru *GoalToCompleteIR*.

Zde uvedený graf na základě průměrné zjištěné velikosti kompatibilních virtuálních počítačů ukazuje rozsah hodnot šířky pásma a vypočítaného počtu virtuálních počítačů v dávce pro dokončení prvotní replikace během 72 hodin.

Ve verzi Public Preview sestava neudává, které virtuální počítače by měly být zahrnuté v dávce. Virtuální počítače pro dávku můžete vybrat na základě známých charakteristik úloh nebo podle velikosti jednotlivých virtuálních počítačů – tu můžete zjistit pomocí velikostí disků zobrazených v části Compatible VMs. Čas dokončení prvotní replikace se úměrně mění v závislosti na skutečné velikosti disku virtuálního počítače, využitém místu na disku a dostupné propustnosti sítě.

![Doporučená velikost dávky virtuálních počítačů](./media/site-recovery-deployment-planner/recommended-vm-batch-size.png)

### <a name="growth-factor-and-percentile-values-used"></a>Použitý faktor růstu a hodnoty percentilu
Tato oblast v dolní části listu ukazuje hodnotu percentilu použitou pro všechny čítače výkonu profilovaných virtuálních počítačů (výchozí je 95. percentil) a faktor růstu (výchozí hodnota je 30 %), který se používá ve všech výpočtech.

![Použitý faktor růstu a hodnoty percentilu](./media/site-recovery-deployment-planner/max-iops-and-data-churn-setting.png)

## <a name="recommendations-with-available-bandwidth-as-input"></a>Doporučení s možností zadat dostupnou šířku pásma jako vstup

![Doporučení s možností zadat dostupnou šířku pásma jako vstup](./media/site-recovery-deployment-planner/profiling-overview-bandwidth-input.png)

Může nastat situace, kdy víte, že pro účely replikace Site Recovery nemůžete nastavit šířku pásma větší než x Mb/s. Tento nástroj umožňuje zadat dostupnou šířku pásma (pomocí parametru -Bandwidth během generování sestavy) a zjistit dosažitelný cíl bodu obnovení v minutách. Na základě této hodnoty dosažitelného cíle bodu obnovení se můžete rozhodnout, jestli potřebujete nastavit větší šířku pásma, nebo vám vyhovuje řešení zotavení po havárii s tímto cílem bodu obnovení.

![Dosažitelný cíl bodu obnovení pro šířku pásma 500 Mb/s](./media/site-recovery-deployment-planner/achievable-rpos.png)

## <a name="input"></a>Vstup
List Input (Vstup) poskytuje přehled profilovaného prostředí VMware.

![Přehled profilovaného prostředí VMware](./media/site-recovery-deployment-planner/Input.png)

**Start Date** a **End Date**: Počáteční a koncové datum dat profilace zahrnutých do generování sestavy. Ve výchozím nastavení je počátečním datem datum zahájení profilace a koncovým datem je datum zastavení profilace. Můžou to být hodnoty StartDate a EndDate, pokud se sestava generuje s použitím těchto parametrů.

**Total number of profiling days:** Celkový počet dnů profilace mezi počátečním a koncovým datem, pro které se generuje sestava.

**Number of compatible virtual machines:** Celkový počet kompatibilních virtuálních počítačů, pro které se počítá požadovaná šířka pásma sítě, požadovaný počet účtů úložiště, počet jader Microsoft Azure a konfiguračních serverů a dalších procesových serverů.

**Total number of disks across all compatible virtual machines** (Celkový počet disků ve všech kompatibilních virtuálních počítačích): Toto číslo se používá jako jeden ze vstupů pro určení počtu konfiguračních serverů a dalších procesových serverů, které se mají v nasazení použít.

**Average number of disks per compatible virtual machine:** Průměrný počet disků ve všech kompatibilních virtuálních počítačích.

**Average disk size (GB):** Průměrná velikost disků ve všech kompatibilních virtuálních počítačích.

**Desired RPO (minutes):** Buď výchozí cíl bodu obnovení, nebo hodnota předaná v parametru DesiredRPO při generování sestavy, sloužící k odhadu požadované šířky pásma.

**Desired bandwidth (Mbps):** Hodnota, kterou jste předali v parametru Bandwidth při generování sestavy, sloužící k odhadu dosažitelného cíle bodu obnovení.

**Observed typical data churn per day (GB):** Průměrná denní četnost změn dat vypozorovaná během všech dnů profilace. Toto číslo se používá jako jeden ze vstupů pro určení počtu konfiguračních serverů a dalších procesových serverů, které se mají v nasazení použít.


## <a name="vm-storage-placement"></a>Umístění virtuálních počítačů v účtech úložiště

![Umístění virtuálních počítačů v účtech úložiště](./media/site-recovery-deployment-planner/vm-storage-placement.png)

**Disk Storage Type:** Účet služby Storage úrovně Standard nebo Premium, který slouží k replikaci všech odpovídajících virtuálních počítačů uvedených ve sloupci **VMs to Place** (Virtuální počítače k umístění).

**Suggested Prefix:** Navrhovaná tříznaková předpona, kterou můžete použít k pojmenování účtu úložiště. Můžete použít vlastní předponu, ale návrh nástroje se řídí [zásadami vytváření názvů pro oddíly účtů úložiště](https://aka.ms/storage-performance-checklist).

**Suggested Account Name:** Název účtu úložiště po zahrnutí navrhované předpony. Název v ostrých závorkách (< a >) nahraďte vlastním názvem.

**Log Storage Account:** Všechny protokoly replikace se ukládají v účtu služby Storage úrovně Standard. Pro virtuální počítače, které se replikují do účtu služby Storage úrovně Premium, nastavte další účet služby Storage úrovně Standard pro ukládání protokolů. Jeden účet úložiště protokolů úrovně Standard může využívat více účtů úložiště replikace úrovně Premium. Virtuální počítače replikované do účtů úložiště úrovně Standard používají stejný účet i k ukládání protokolů.

**Suggested Log Account Name:** Název účtu úložiště protokolů po zahrnutí navrhované předpony. Název v ostrých závorkách (< a >) nahraďte vlastním názvem.

**Placement Summary:** Souhrn celkové zátěže virtuálních počítačů na účet úložiště v době replikace a testovacího převzetí služeb při selhání nebo převzetí služeb při selhání. Zahrnuje celkový počet virtuálních počítačů namapovaných na účet úložiště, celkový počet R/W IOPS ve všech virtuálních počítačích umístěných v tomto účtu úložiště, celkový počet vstupně-výstupních operací zápisu (replikace) za sekundu, celkovou nastavenou velikost všech disků a celkový počet disků.

**Virtual Machines to Place:** Seznam všech virtuálních počítačů, které by se měly umístit do daného účtu úložiště pro zajištění optimálního výkonu a využití.

## <a name="compatible-vms"></a>Kompatibilní virtuální počítače
![Tabulka aplikace Excel s kompatibilními virtuálními počítači](./media/site-recovery-deployment-planner/compatible-vms.png)

**VM Name:** Název nebo IP adresa virtuálního počítače, které se použily v souboru VMListFile při generování sestavy. V tomto sloupci jsou uvedeny také disky (VMDK) připojené k virtuálním počítačům. Aby se rozlišily virtuální počítače vCenter s duplicitními názvy nebo IP adresami, je součástí názvů i název hostitele ESXi. Uvedený hostitel ESXi je ten, na kterém byl virtuální počítač umístěn při zjištění nástrojem během období profilace.

**VM Compatibility** (Kompatibilita virtuálního počítače): Hodnoty jsou **Yes** (Ano) a **Yes**\* (Ano). **Yes**\* je pro situace, kdy je virtuální počítač vhodný pro službu [Azure Storage úrovně Premium](https://aka.ms/premium-storage-workload). V takovém případě profilovaný disk s vysokou četností změn nebo vysokým počtem IOPS spadá do kategorie P20 nebo P30, ale kvůli velikosti se disk mapuje na nižší kategorii P10 nebo P20. Účet úložiště určuje, na jaký disk služby Storage úrovně Premium se disk bude mapovat, na základě jeho velikosti. Například:
* Menší než 128 GB je P10.
* 128 GB až 512 GB je P20.
* 512 GB až 1 023 GB je P30.

Pokud se díky charakteristikám úloh disk umístil do kategorie P20 nebo P30, ale kvůli velikosti je mapován na nižší typ disku služby Storage úrovně Premium, nástroj virtuální počítač označí jako **Yes**\*. Nástroj také doporučí změnit velikost zdrojového disku tak, aby se vešel do doporučeného typu disku služby Storage úrovně Premium, nebo po převzetí služeb při selhání změnit typ cílového disku.

**Storage Type** (Typ služby Storage): Standard nebo Premium.

**Suggested Prefix:** Tříznaková předpona účtu úložiště.

**Storage Account** (Účet služby Storage): Název s použitím navrhované předpony účtu úložiště.

**R/W IOPS (with Growth Factor):** Počet R/W IOPS na disku ve špičce (výchozí 95. percentil), včetně faktoru budoucího růstu (výchozí hodnota 30 %). Všimněte si, že celkový počet R/W IOPS virtuálního počítače nebude vždy odpovídat součtu R/W IOPS jednotlivých disků virtuálního počítače, protože počet R/W IOPS virtuálního počítače ve špičce je maximální hodnota součtu R/W IOPS jeho jednotlivých disků v každé minutě období profilace.

**Data Churn in Mbps (with Growth Factor):** Četnost změn dat na disku ve špičce (výchozí 95. percentil), včetně faktoru budoucího růstu (výchozí hodnota 30 %). Všimněte si, že celková četnost změn dat virtuálního počítače nebude vždy odpovídat součtu četností změn dat jednotlivých disků virtuálního počítače, protože četnost změn dat virtuálního počítače ve špičce je maximální hodnota součtu četností změn jeho jednotlivých disků v každé minutě období profilace.

**Azure VM Size:** Ideální velikost mapovaného virtuálního počítače Azure Cloud Services pro tento místní virtuální počítač. Mapování vychází z velikosti paměti, počtu disků, jader nebo síťových adaptérů a počtu R/W IOPS místního virtuálního počítače. Doporučení vždy představuje nejmenší velikost virtuálního počítače Azure, která odpovídá všem charakteristikám místního virtuálního počítače.

**Number of Disks:** Celkový počet disků (VMDK) ve virtuálním počítači.

**Disk size (GB):** Celková nastavená velikost všech disků virtuálního počítače. Nástroj ukazuje také velikosti jednotlivých disků ve virtuálním počítači.

**Cores:** Počet procesorových jader ve virtuálním počítači.

**Memory (MB):** Velikost paměti RAM ve virtuálním počítači.

**NICs:** Počet síťových adaptérů ve virtuálním počítači.

**Boot Type:** Jedná se o typ spuštění virtuálního počítače. Může to být buď BIOS, nebo EFI. Azure Site Recovery aktuálně podporuje pouze typ spuštění BIOS. Všechny virtuální počítače s typem spuštění EFI jsou uvedené na listu Nekompatibilní virtuální počítače.

**OS Type:** Jedná se o typ operačního systému virtuálního počítače. Může to být Windows, Linux, nebo jiný.

## <a name="incompatible-vms"></a>Nekompatibilní virtuální počítače

![Tabulka aplikace Excel s nekompatibilními virtuálními počítači](./media/site-recovery-deployment-planner/incompatible-vms.png)

**VM Name:** Název nebo IP adresa virtuálního počítače, které se použily v souboru VMListFile při generování sestavy. V tomto sloupci jsou uvedeny také disky VMDK připojené k virtuálním počítačům. Aby se rozlišily virtuální počítače vCenter s duplicitními názvy nebo IP adresami, je součástí názvů i název hostitele ESXi. Uvedený hostitel ESXi je ten, na kterém byl virtuální počítač umístěn při zjištění nástrojem během období profilace.

**VM Compatibility:** Označujte, proč je daný virtuální počítač nekompatibilní se Site Recovery. Pro každý nekompatibilní disk virtuálního počítače jsou popsané důvody. V závislosti na publikovaných [omezeních úložiště](https://aka.ms/azure-storage-scalbility-performance) může důvodem být některá z následujících možností:

* Disk size is >1023 GB (Disk je větší než 1 023 GB). Azure Storage v současné době nepodporuje disky větší než 1 TB.
* Typ spuštění je EFI. Azure Site Recovery aktuálně podporuje pouze virtuální počítač s typem spuštění BIOS.

* Total VM size (replication + TFO) exceeds the supported storage-account size limit (35 TB) (Celková velikost virtuálního počítače (replikace + testovací převzetí služeb při selhání) překračuje omezení podporované velikosti účtu úložiště (35 TB)). K této nekompatibilitě obvykle dochází, když je ve virtuálním počítači jeden disk, jehož některé charakteristiky výkonu překračují maximální podporovaná omezení systému Azure nebo Site Recovery pro účet služby Storage úrovně Standard. V takové situaci se virtuální počítač dostává do zóny účtu služby Storage úrovně Premium. Nicméně maximální podporovaná velikost účtu služby Storage úrovně Premium je 35 TB a jeden virtuální počítač nemůže být chráněn v rámci několika účtů úložiště. Všimněte si také, že testovací převzetí služeb při selhání spuštěné na chráněném virtuálním počítači poběží v rámci stejného účtu úložiště, ve kterém probíhá replikace. V takovém případě nastavte dvojnásobnou velikost disku, aby paralelně mohla probíhat replikace a úspěšné testovací převzetí služeb při selhání.
* Source IOPS exceeds supported storage IOPS limit of 5000 per disk (Počet zdrojových IOPS překračuje podporované omezení úložiště – 5 000 IOPS na disk).
* Source IOPS exceeds supported storage IOPS limit of 80,000 per VM (Počet zdrojových IOPS překračuje podporované omezení úložiště – 80 000 IOPS na virtuální počítač).
* Average data churn exceeds supported Site Recovery data churn limit of 10 MBps for average I/O size for the disk (Průměrná četnost změn dat překračuje podporované omezení Site Recovery pro četnost změn dat – průměrná velikost vstupně-výstupních operací disku 10 Mb/s).
* Total data churn across all disks on the VM exceeds the maximum supported Site Recovery data churn limit of 54 MBps per VM (Celková četnost změn dat na všech discích virtuálního počítače překračuje maximální podporované omezení Site Recovery pro četnost změn dat – 54 Mb/s na virtuální počítač).
* Average effective write IOPS exceeds the supported Site Recovery IOPS limit of 840 for disk (Průměrný počet efektivních vstupně-výstupních operací zápisu za sekundu překračuje podporované omezení Site Recovery pro počet IOPS – 840 na disk).
* Calculated snapshot storage exceeds the supported snapshot storage limit of 10 TB (Vypočtená velikost úložiště snímků překračuje podporované omezení velikosti úložiště snímků – 10 TB).

**R/W IOPS (with Growth Factor):** Počet IOPS na disku ve špičce (výchozí 95. percentil), včetně faktoru budoucího růstu (výchozí hodnota 30 %). Všimněte si, že celkový počet R/W IOPS virtuálního počítače nebude vždy odpovídat součtu R/W IOPS jednotlivých disků virtuálního počítače, protože počet R/W IOPS virtuálního počítače ve špičce je maximální hodnota součtu R/W IOPS jeho jednotlivých disků v každé minutě období profilace.

**Data Churn in Mbps (with Growth Factor):** 4etnost změn dat na disku ve špičce (výchozí 95. percentil), včetně faktoru budoucího růstu (výchozí hodnota 30 %). Všimněte si, že celková četnost změn dat virtuálního počítače nebude vždy odpovídat součtu četností změn dat jednotlivých disků virtuálního počítače, protože četnost změn dat virtuálního počítače ve špičce je maximální hodnota součtu četností změn jeho jednotlivých disků v každé minutě období profilace.

**Number of Disks:** Celkový počet disků VMDK ve virtuálním počítači.

**Disk size (GB):** Celková nastavená velikost všech disků virtuálního počítače. Nástroj ukazuje také velikosti jednotlivých disků ve virtuálním počítači.

**Cores:** Počet procesorových jader ve virtuálním počítači.

**Memory (MB):** Velikost paměti RAM ve virtuálním počítači.

**NICs:** Počet síťových adaptérů ve virtuálním počítači.

**Boot Type:** Jedná se o typ spuštění virtuálního počítače. Může to být buď BIOS, nebo EFI. Azure Site Recovery aktuálně podporuje pouze typ spuštění BIOS. Všechny virtuální počítače s typem spuštění EFI jsou uvedené na listu Nekompatibilní virtuální počítače.

**OS Type:** Jedná se o typ operačního systému virtuálního počítače. Může to být Windows, Linux, nebo jiný.


## <a name="site-recovery-limits"></a>Omezení Site Recovery

**Cíl ukládání replikace** | **Průměrná velikost vstupně-výstupních operací zdrojového disku** |**Průměrná četnost změn dat zdrojového disku** | **Celková denní četnost změn dat zdrojového disku**
---|---|---|---
Storage úrovně Standard | 8 kB    | 2 Mb/s | 168 GB na disk
Disk úrovně Premium P10 | 8 kB    | 2 Mb/s | 168 GB na disk
Disk úrovně Premium P10 | 16 kB | 4 Mb/s |    336 GB na disk
Disk úrovně Premium P10 | 32 kB nebo větší | 8 Mb/s | 672 GB na disk
Disk úrovně Premium P20 nebo P30 | 8 kB    | 5 Mb/s | 421 GB na disk
Disk úrovně Premium P20 nebo P30 | 16 kB nebo větší |10 Mb/s | 842 GB na disk

Toto jsou průměrné hodnoty za předpokladu, že se vstupně-výstupní operace z 30 % překrývají. Služba Site Recovery je schopna zpracovávat větší propustnost v závislosti na poměru překrývání, větší velikosti zápisů a skutečného chování vstupně-výstupních operací úloh. Předchozí čísla předpokládají typický backlog přibližně 5 minut. To znamená, že zpracování nahrávaných dat a vytvoření bodu obnovení proběhne do pěti minut od nahrání.

Tato omezení se zakládají na našich testováních, nemůžou však pokrýt všechny možné kombinace vstupně-výstupních operací aplikace. Skutečné výsledky se můžou lišit v závislosti na kombinaci vstupně-výstupních operací vaší aplikace. Pro dosažení co nejlepších výsledků i po naplánování nasazení vždy doporučujeme provádět rozsáhlá testování aplikace pomocí testovacího převzetí služeb při selhání, abyste získali skutečnou představu o výkonu.

## <a name="updating-the-deployment-planner"></a>Aktualizace Deployment Planneru
Pokud chcete aktualizovat Deployment Planner, proveďte následující:

1. Stáhněte si nejnovější verzi nástroje [Azure Site Recovery Deployment Planner](https://aka.ms/asr-deployment-planner).

2. Zkopírujte složku .zip na server, na kterém chcete nástroj spustit.

3. Rozbalte složku .zip.

4. Proveďte jednu z následujících akcí:
 * Pokud nejnovější verze neobsahuje opravu profilace a ve vaší stávající verzi Deployment Planneru již profilace probíhá, pokračujte v ní.
 * Pokud nejnovější verze obsahuje opravu profilace, doporučujeme zastavit profilaci ve stávající verzi a znovu ji spustit v nové verzi.

  >[!NOTE]
  >
  >Při spuštění profilace v nové verzi předejte stejnou cestu k výstupnímu adresáři, aby nástroj připojil data profilu k existujícím souborům. K vygenerování sestavy se použije úplná sada dat profilace. Pokud předáte jiný výstupní adresář, vytvoří se nové soubory a stará data profilace se k vygenerování sestavy nepoužijí.
  >
  >Každý nový Deployment Planner je kumulativní aktualizací souboru .zip. Nemusíte kopírovat nejnovější soubory do předchozí složky. Můžete vytvořit a použít novou složku.


## <a name="version-history"></a>Historie verzí

### <a name="13"></a>1.3
Aktualizováno: 9. května 2017

Je přidána následující nová funkce:

* Přidána podpora spravovaného disku v generování sestav. Počet virtuálních počítačů, které lze umístit do jednoho účtu úložiště, se počítá na základě toho, jestli je pro převzetí služeb při selhání nebo testovací převzetí služeb při selhání vybrán spravovaný disk.        


### <a name="12"></a>1.2
Aktualizace: 7. duben 2017

Byly přidány následující opravy:

* Pro každý virtuální počítač byla přidána kontrola typu spuštění (BIOS nebo EFI), aby se určilo, jestli je virtuální počítač kompatibilní nebo nekompatibilní z hlediska ochrany.
* Pro každý virtuální počítač byla přidána informace o typu operačního systému do listů kompatibilních a nekompatibilních virtuálních počítačů.
* Operace GetThroughput je nyní podporována v oblastech Microsoft Azure US Government a Čína.
* Bylo přidáno několik dalších kontrol požadovaných součástí pro vCenter a ESXi Server.
* Při nastavení národního prostředí na jiné než anglické se generovala nesprávná sestava.


### <a name="11"></a>1.1
Aktualizováno: 9. března 2017

Opravené následující problémy:

* Nástroj nemůže profilovat virtuální počítače, pokud server vCenter obsahuje dva nebo více virtuálních počítačů se stejným názvem nebo stejnou IP adresou v různých hostitelích ESXi.
* Kopírování a vyhledávání je pro listy kompatibilních a nekompatibilních virtuálních počítačů zakázáno.

### <a name="10"></a>1.0
Aktualizováno: 23. února 2017

Azure Site Recovery Deployment Planner Public Preview 1.0 má následující známé problémy (které se budou řešit v budoucích aktualizacích):

* Nástroj funguje pouze pro scénáře nasazení VMware do Azure, nikoli pro nasazení Hyper-V do Azure. Pro scénáře nasazení Hyper-V do Azure použijte [nástroj Hyper-V Capacity Planner](./site-recovery-capacity-planning-for-hyper-v-replication.md).
* Operace GetThroughput není podporována v oblastech Microsoft Azure US Government a Čína.
* Nástroj nemůže profilovat virtuální počítače, pokud server vCenter obsahuje dva nebo více virtuálních počítačů se stejným názvem nebo stejnou IP adresou v různých hostitelích ESXi. V této verzi nástroj při zjištění duplicitních výskytů názvů nebo IP adres virtuálních počítačů v souboru VMListFile profilaci takových virtuálních počítačů přeskočí. Alternativním řešením je profilování virtuálních počítačů pomocí hostitele ESXi namísto serveru vCenter. Pro každého hostitele ESXi je nutné spustit jednu instanci.

