---
title: "Azure Site Recovery Deployment Planner pro nasazení VMware do Azure | Dokumentace Microsoftu"
description: "Toto je uživatelská příručka k nástroji Azure Site Recovery Deployment Planner."
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
translationtype: Human Translation
ms.sourcegitcommit: d9dad6cff80c1f6ac206e7fa3184ce037900fc6b
ms.openlocfilehash: 4e444deaa84c7f02608f4910e31f7033df51a73b
ms.lasthandoff: 03/06/2017


---
#<a name="azure-site-recovery-deployment-planner"></a>Azure Site Recovery Deployment Planner
Toto je uživatelská příručka k nástroji Azure Site Recovery Deployment Planner pro produkční nasazení VMware do Azure.


##<a name="overview"></a>Přehled

Než začnete chránit jakékoli virtuální počítače VMware pomocí Azure Site Recovery, je nutné přidělit dostatečnou šířku pásma v závislosti na vaší denní frekvenci změn dat, abyste dosáhli požadovaného cíle bodu obnovení (RPO). Je potřeba místně nasadit správný počet konfiguračních serverů a procesových serverů. Také je nutné vytvořit správný typ a počet cílových účtů služby Azure Storage (úrovně Standard nebo Premium), které budou zohledňovat nárůst počtu vašich zdrojových produkčních serverů způsobený zvyšováním využití v průběhu času. Typ služby Storage se určuje pro jednotlivé virtuální počítače v závislosti na charakteristikách úloh (vstupně-výstupní operace čtení a zápisu za sekundu (R/W IOPS), četnost změn dat) a omezeních Azure Site Recovery.  

Azure Site Recovery Deployment Planner Public Preview je nástroj příkazového řádku aktuálně dostupný pouze pro scénář nasazení VMware do Azure. Pomocí tohoto nástroje můžete vzdáleně profilovat virtuální počítače VMware (bez jakéhokoli dopadu na produkční prostředí) a porozumět tak požadavkům na šířku pásma a úložiště Azure pro úspěšnou replikaci a testovací převzetí služeb při selhání.  Nástroj můžete spustit místně bez nutnosti instalace jakýchkoli komponent Azure Site Recovery, nicméně pro získání přesných výsledků propustnosti se doporučuje spustit nástroj Deployment Planner na Windows Serveru splňujícím minimální požadavky konfiguračního serveru Azure Site Recovery, který časem budete muset nasadit v jednom z prvních kroků produkčního nasazení.

Nástroj poskytuje následující podrobnosti:

**Posouzení kompatibility**<br>
* Vyhodnocení způsobilosti virtuálního počítače na základě počtu disků, velikosti disků, počtu vstupně-výstupních operací za sekundu (IOPS) a četnosti změn

**Potřebná šířka pásma sítě vs. posouzení cíle bodu obnovení**<br>
* Odhadovaná šířka pásma sítě potřebná pro rozdílovou replikaci<br>
* Propustnost z místního prostředí do Azure, které Azure Site Recovery může dosáhnout<br>
* Počet virtuálních počítačů pro dávku na základě odhadované šířky pásma pro dokončení prvotní replikace v daném čase<br>

**Požadavky na infrastrukturu Microsoft Azure**<br>
* Požadovaný typ úložiště (Storage úrovně Standard nebo Premium) pro každý virtuální počítač<br>
* Celkový počet účtů služby Storage úrovně Standard a Premium, které se mají pro replikaci zřídit<br>
* Návrhy pojmenování účtů služby Storage na základě pokynů pro Azure Storage<br>
* Umístění jednotlivých virtuálních počítačů v účtech služby Storage<br>
* Počet jader systému Microsoft Azure, která se mají zřídit před testovacím převzetím služeb při selhání nebo převzetím služeb při selhání v rámci předplatného<br>
* Doporučená velikost virtuálního počítače Microsoft Azure pro každý místní virtuální počítač<br>

**Požadavky na místní infrastrukturu**<br>
* Požadovaný počet konfiguračních serverů a procesových serverů, které se mají místně nasadit<br>

>[!IMPORTANT]
>
>Z důvodu možného nárůstu využití v průběhu času se všechny tyto výpočty v nástroji provádí s předpokladem 30% faktoru růstu v charakteristikách úloh a berou v úvahu 95. percentil všech metrik profilace (počet R/W IOPS, četnost změn atd.). Oba tyto parametry (faktor růstu a výpočet percentilu) je možné konfigurovat. Další informace o [faktoru růstu](site-recovery-deployment-planner.md#growth-factor) a [percentilu hodnoty použitém pro výpočet](site-recovery-deployment-planner.md#percentile-value-used-for-the-calculation).
>


## <a name="requirements"></a>Požadavky
Nástroj má dvě hlavní fáze – profilace a generování sestav. Existuje také třetí možnost – pouze výpočet propustnosti. Níže jsou uvedené požadavky na server, ze kterého se spouští profilace nebo měření propustnosti.

| Požadavek | Popis|
|---|---|
|Profilace a měření propustnosti| <br>Operační systém: Microsoft Windows Server 2012 R2 <br>Ideálně alespoň stejná velikost jako [velikost](https://aka.ms/asr-v2a-on-prem-components) následujícího konfiguračního serveru<br>Konfigurace počítače: 8 virtuálních CPU, 16 GB paměti RAM, 300 GB HDD<br [Microsoft .NET Framework 4.5](https://aka.ms/dotnet-framework-45)<br>[VMware vSphere PowerCLI 6.0 R3](https://developercenter.vmware.com/tool/vsphere_powercli/6.0)<br>[Microsoft Visual C++ Redistributable for Visual Studio 2012](https://aka.ms/vcplusplus-redistributable)<br> Internetový přístup k Microsoft Azure z tohoto serveru<br> Účet služby Microsoft Azure Storage<Br>Přístup správce na server<br>Volné místo na disku alespoň 100 GB (za předpokladu 1 000 virtuálních počítačů, každý průměrně se 3 disky a profilovaný po dobu 30 dnů)|
| Generování sestav| Jakýkoli počítač s Windows nebo Windows Server s aplikací Microsoft Excel 2013 nebo novější |
| Uživatelská oprávnění | Oprávnění jen ke čtení pro uživatelský účet používaný pro přístup k serveru VMware vCenter nebo vSphere během profilace|


> [!NOTE]
>
> Nástroj může profilovat pouze virtuální počítače s disky VMDK a RDM. Nemůže profilovat virtuální počítače s disky iSCSI nebo NFS. Přestože Azure Site Recovery podporuje disky iSCSI a NFS pro servery VMware, nástroj Deployment Planner nesídlí v hostu a profilaci provádí pouze pomocí čítačů výkonu vCenter, proto do těchto typů disků nevidí.
>


##<a name="download"></a>Ke stažení
[Stáhněte si](https://aka.ms/asr-deployment-planner) nejnovější verzi nástroje Azure Site Recovery Deployment Planner Public Preview.  Nástroje je zabalený ve formátu zip.  Aktuální verze nástroje podporuje pouze scénář nasazení VMware do Azure.

Zkopírujte soubor .zip na Windows Server, na kterém chcete nástroj spustit. Přestože nástroj můžete spustit z libovolného Windows Serveru 2012 R2 s přístupem k internetu pro připojení k serveru VMware vCenter nebo z hostitele VMware vSphere ESXi, na kterém sídlí virtuální počítač určený k profilaci, doporučuje se spuštění nástroje na serveru, jehož konfigurace hardwaru odpovídá [pokynům k nastavení velikosti konfiguračního serveru](https://aka.ms/asr-v2a-on-prem-components).  Pokud jste již místně nasadili komponenty Azure Site Recovery, měli byste nástroj spustit z konfiguračního serveru. Doporučuje se mít stejnou konfiguraci hardwaru konfiguračního serveru (který obsahuje integrovaný procesový server) a serveru, na kterém nástroj spouštíte, aby dosažená propustnost, kterou nástroj hlásí, odpovídala skutečné propustnosti, které může Azure Site Recovery dosáhnout během replikace. Výpočet propustnosti závisí na dostupné šířce pásma sítě na serveru a na konfiguraci hardwaru (CPU, úložiště atd.) serveru. Pokud nástroj spustíte z nějakého jiného serveru, vypočítá se propustnost z tohoto serveru do Microsoft Azure a navíc se může lišit konfigurace hardwaru tohoto serveru a konfiguračního serveru, proto dosažená propustnost, kterou nástroj hlásí, nebude odpovídat skutečnosti.

Rozbalte složku zip. Uvidíte několik souborů a podsložek. Spustitelný soubor je ASRDeploymentPlanner.exe v nadřazené složce.

Příklad: Zkopírujte soubor .zip na jednotku E:\ a rozbalte jej.
E:\ASR Deployment Planner-Preview_v1.0.zip

E:\ASR Deployment Planner-Preview_v1.0\ ASR Deployment Planner-Preview_v1.0\ ASRDeploymentPlanner.exe

##<a name="capabilities"></a>Možnosti
Nástroj příkazového řádku (ASRDeploymentPlanner.exe) je možné spustit v některém z následujících tří režimů:

1.    Profilace  
2.    Generování sestav
3.    Zjištění propustnosti

Nejprve je nutné nástroj spustit v režimu profilace, aby shromáždil četnost změn dat a počet IOPS virtuálních počítačů.  Následně spusťte nástroj v režimu generování sestav, abyste zjistili požadavky na šířku pásma sítě a na úložiště.

##<a name="profiling"></a>Profilace
V režimu profilace se nástroj Deployment Planner připojí k serveru vCenter nebo k hostiteli vSphere ESXi za účelem shromáždění dat o výkonu virtuálního počítače.

* Profilace nemá vliv na výkon produkčních virtuálních počítačů, protože se k nim nevytváří žádné přímé připojení. Veškerá data o výkonu se shromažďují ze serveru vCenter nebo z hostitele vSphere ESXi.
* Dotazování serveru vCenter nebo hostitele vSphere ESXi probíhá každých 15 minut, aby se zajistilo, že profilace bude mít na server zanedbatelný dopad. To však nesnižuje přesnost profilace, protože nástroj ukládá data čítače výkonu pro každou minutu.

####<a name="create-a-list-of-virtual-machines-to-profile"></a>Vytvoření seznamu virtuálních počítačů určených k profilaci
Nejprve potřebujete seznam virtuálních počítačů, které chcete profilovat. Názvy virtuálních počítačů na hostiteli VMware vSphere ESXi nebo VMware vCenter můžete získat pomocí následujících příkazů rozhraní VMware vSphere PowerCLI. Případně můžete prostě v souboru ručně vytvořit seznam popisných názvů nebo IP adres virtuálních počítačů, které chcete profilovat.

1.    Přihlaste se k virtuálnímu počítači s nainstalovaným rozhraním VMware vSphere PowerCLI.
2.    Otevřete konzolu VMware vSphere PowerCLI.
3.    Zkontrolujte, že pro skript nejsou zakázané zásady spouštění. Pokud jsou zakázané, spusťte konzolu VMware vSphere PowerCLI v režimu správce a spuštěním následujícího příkazu je povolte:

            Set-ExecutionPolicy –ExecutionPolicy AllSigned

4.    Spusťte následující dva příkazy, které získají všechny názvy virtuálních počítačů na hostiteli VMware vSphere ESXi nebo VMware vCenter a uloží je do souboru .txt.
Nahraďte zástupné hodnoty &lsaquo;server name&rsaquo; (název serveru), &lsaquo;user name&rsaquo; (uživatelské jméno), &lsaquo;password&rsaquo; (heslo) a &lsaquo;outputfile.txt&rsaquo; (výstupní soubor) vlastními hodnotami.

            Connect-VIServer -Server <server name> -User <user name> -Password <password>

            Get-VM |  Select Name | Sort-Object -Property Name >  <outputfile.txt>


5.    Otevřete výstupní soubor v Poznámkovém bloku. Zkopírujte názvy všech virtuálních počítačů, které chcete profilovat, do jiného souboru (třeba ProfileVMList.txt), přičemž každý název virtuálního počítače musí být na samostatném řádku. Tento soubor se použije jako vstup pro parametr -VMListFile nástroje příkazového řádku.

    ![Deployment Planner](./media/site-recovery-deployment-planner/profile-vm-list.png)


####<a name="start-profiling"></a>Spuštění profilace
Jakmile budete mít seznam virtuálních počítačů určených k profilaci, můžete nástroj spustit v režimu profilace. Zde je seznam povinných a volitelných parametrů pro spuštění nástroje v režimu profilace. Parametry v hranatých závorkách jsou volitelné.

ASRDeploymentPlanner.exe -Operation StartProfiling /?

| Název parametru | Popis |
|---|---|
| -Operation |      StartProfiling |
| -Server | Plně kvalifikovaný název domény nebo IP adresa serveru vCenter nebo hostitele ESXi, které obsahují virtuální počítače určené k profilaci.|
| -User | Uživatelské jméno pro připojení k serveru vCenter nebo hostiteli ESXi. Uživatel musí mít alespoň přístup jen ke čtení.|
| -VMListFile |    Soubor se seznamem virtuálních počítačů určených k profilaci. Cesta k souboru může být absolutní nebo relativní. Tento soubor musí na každém řádku obsahovat jeden název nebo IP adresu virtuálního počítače. Název virtuálního počítače zadaný v souboru se musí shodovat s názvem virtuálního počítače na serveru vCenter nebo hostiteli ESXi. <br> Například soubor VMList.txt obsahuje následující virtuální počítače:<br>virtual_machine_A <br>10.150.29.110<br>virtual_machine_B |
| -NoOfDaysToProfile | Počet dní, po které má být profilace spuštěna. Doporučuje se profilaci spouštět na více než 15 dnů, během kterých se zjistí vzorce úloh ve vašem prostředí a s jejich pomocí nástroj poskytne přesná doporučení. |
| [-Directory] |    Název UNC nebo cesta k místnímu adresáři, do kterého se během profilace budou ukládat data profilace. Pokud tento parametr není zadaný, jako výchozí se použije adresář ProfiledData v aktuální cestě. |
| [-Password ] | Heslo pro připojení k serveru vCenter nebo hostiteli ESXi. Pokud heslo nezadáte teď, budete k tomu vyzváni při spuštění příkazu.|
|  [-StorageAccountName]  | Název účtu služby Azure Storage, u kterého se má zjistit dosažitelná propustnost pro replikaci místních dat do Azure. Nástroj vypočítává propustnost tak, že do tohoto účtu úložiště nahrává testovací data.|
| [-StorageAccountKey] | Klíč účtu služby Azure Storage, který se použije pro přístup k účtu úložiště. Přejděte na web Azure Portal > Účty úložiště > [název účtu služby Storage] > Nastavení > Přístupové klíče > Klíč1 (nebo Primární přístupový klíč v případě klasického účtu úložiště). |

Doporučujeme profilovat virtuální počítače po dobu alespoň 15 až 30 dnů. Během období profilace je ASRDeploymentPlanner.exe stále spuštěný. Nástroj na vstupu přijímá zadání času profilace ve dnech. Pokud chcete nástroj rychle otestovat spuštěním profilace na několik hodin nebo minut, ve verzi Public Preview budete muset tento čas převést na odpovídající zlomek dne.  Pokud například chcete profilovat po dobu 30 minut, vstup bude muset být 30/(60×24) = 0,021 dnů.  Minimální povolený čas profilace je 30 minut.

Během profilace můžete volitelně předat název a klíč účtu služby Azure Storage a zjistit tak propustnost, které může Azure Site Recovery dosáhnout v době replikace z konfiguračního serveru nebo procesového serveru do Azure. Pokud název a klíč účtu služby Azure Storage během profilace nepředáte, nástroj dosažitelnou propustnost počítat nebude.


Můžete spouštět více instancí nástroje pro různé sady virtuálních počítačů. Zkontrolujte, že se názvy virtuálních počítačů v sadách profilace neopakují. Pokud například profilujete deset virtuálních počítačů (VM1–VM10) a po několika dnech chcete profilovat dalších pět virtuálních počítačů (VM11–VM15), můžete nástroj spustit z jiné konzoly příkazového řádku pro druhou sadu virtuálních počítačů (VM11–VM15). Zajistěte však, aby druhá sada virtuálních počítačů neobsahovala žádné názvy virtuálních počítačů z první instance profilace, nebo pro druhé spuštění použijte jiný výstupní adresář. Pokud se k profilování stejných virtuálních počítačů používají dvě instance nástroje a stejný výstupní adresář, vygenerovaná sestava bude nepřesná.

Konfigurace virtuálních počítačů se zachytí jednou na začátku operace profilace a uloží se do souboru VMDetailList.xml. Tyto informace se použijí v okamžiku generování sestav. Žádné změny v konfiguraci virtuálních počítačů (např. navýšení počtu jader, disků, síťových adaptérů atd.) od začátku profilace do konce se nezachytí. Pokud jste se ocitli v situaci, že se během profilace změnila konfigurace některého profilovaného virtuálního počítače, ve verzi Public Preview existuje alternativní řešení, pomocí kterého můžete získat nejnovější podrobnosti o virtuálním počítači bez nutnosti generovat sestavu.   

* Zálohujte soubor VMdetailList.xml a odstraňte ho z aktuálního umístění.
* V okamžiku generování sestav předejte argumenty -User a -Password.

Příkaz profilace vygeneruje v adresáři profilace několik souborů – neodstraňujte je, jinak to bude mít dopad na generování sestav.

#####<a name="example-1-to-profile-virtual-machines-for-30-days-and-find-the-throughput-from-on-premises-to-azure"></a>Příklad 1: Profilování virtuálních počítačů po dobu 30 dnů a zjištění propustnosti z místního prostředí do Azure
ASRDeploymentPlanner.exe **-Operation** StartProfiling -Directory “E:\vCenter1_ProfiledData” **-Server** vCenter1.contoso.com **-VMListFile** “E:\vCenter1_ProfiledData\ProfileVMList1.txt” **-NoOfDaysToProfile** 30 **-User** vCenterUser1 **-StorageAccountName** asrspfarm1 **-StorageAccountKey** Eby8vdM02xNOcqFlqUwJPLlmEtlCDXJ1OUzFT50uSRZ6IFsuFq2UVErCz4I6tq/K1SZFPTOtr/KBHBeksoGMGw==

#####<a name="example-2-to-profile-virtual-machines-for-15-days"></a>Příklad 2: Profilování virtuálních počítačů po dobu 15 dnů
ASRDeploymentPlanner.exe **-Operation** StartProfiling **-Directory** “E:\vCenter1_ProfiledData” **-Server** vCenter1.contoso.com **-VMListFile** “E:\vCenter1_ProfiledData\ProfileVMList1.txt” **-NoOfDaysToProfile** 15 -User vCenterUser1

#####<a name="example-3-to-profile-virtual-machines-for-1-hour-for-a-quick-test-of-the-tool"></a>Příklad 3: Rychlé otestování nástroje profilováním virtuálních počítačů po dobu 1 hodiny
ASRDeploymentPlanner.exe **-Operation** StartProfiling **-Directory** “E:\vCenter1_ProfiledData” **-Server** vCenter1.contoso.com **-VMListFile** “E:\vCenter1_ProfiledData\ProfileVMList1.txt” **-NoOfDaysToProfile** 0.04 **-User** vCenterUser1


>[!NOTE]
>
> * Pokud se server, na kterém je nástroj spuštěný, restartuje nebo na něm dojde k chybě, nebo pokud nástroj ukončíte stisknutím kombinace Ctrl + C, profilovaná data se zachovají. Může se stát, že kvůli tomu bude chybět posledních 15 minut profilovaných dat. Po opětovném spuštění serveru budete muset nástroj znovu spustit v režimu profilace.
>
> * Pokud předáte název a klíč účtu služby Azure Storage, nástroj bude měřit propustnost v posledním kroku profilace. Pokud bude nástroj ukončen před řádným dokončením profilace, propustnost se nevypočítá. Z konzoly příkazového řádku můžete kdykoli spustit operaci GetThroughput pro zjištění propustnosti před generováním sestavy, jinak vygenerovaná sestava nebude obsahovat informaci o dosažené propustnosti.
>

##<a name="generate-report"></a>Generování sestav
Nástroj jako výstup sestavy generuje soubor XLSM (soubor aplikace Microsoft Excel s podporou maker), který shrnuje veškerá doporučení pro nasazení – sestava má název DeploymentPlannerReport_<Unique Numeric Identifier>.xlsm a je umístěná v zadaném adresáři.

Po dokončení profilace můžete nástroj spustit v režimu generování sestav. Zde je seznam povinných a volitelných parametrů pro spuštění nástroje v režimu generování sestav. Parametry v hranatých závorkách jsou volitelné.

ASRDeploymentPlanner.exe -Operation GenerateReport /?

|Název parametru | Popis |
|-|-|
| -Operation | GenerateReport |
| -Server |  Plně kvalifikovaný název domény nebo IP adresa serveru vCenter nebo v Sphere (použijte stejný název nebo IP adresu, jako jste použili při profilaci), na němž jsou umístěné virtuální počítače, pro které se má vygenerovat sestava. Všimněte si, že pokud jste při profilaci použili server vCenter, nemůžete pro generování sestav použít server vSphere a naopak.|
| -VMListFile | Soubor se seznamem profilovaných virtuálních počítačů, pro které se má vygenerovat sestava. Cesta k souboru může být absolutní nebo relativní. Tento soubor musí na každém řádku obsahovat jeden název nebo IP adresu virtuálního počítače. Názvy virtuálních počítačů zadané v tomto souboru musí být stejné jako názvy virtuálních počítačů na serveru vCenter nebo hostiteli ESXi a musí se shodovat s názvy, které jste použili při profilaci.|
| [-Directory] | Název UNC nebo cesta k místnímu adresáři, ve kterém jsou uložena profilovaná data (soubory vytvořené během profilace). Tato data jsou vyžadovaná k vygenerování sestavy. Pokud tento parametr nezadáte, použije se adresář ProfiledData. |
| [-GoalToCompleteIR] |    Počet hodin, během kterých je potřeba dokončit prvotní replikaci profilovaných virtuálních počítačů. Vygenerovaná sestava bude obsahovat počet virtuálních počítačů, pro které lze v zadaném čase dokončit prvotní replikaci. Výchozí hodnota je 72 hodin. |
| [-User] | Uživatelské jméno pro připojení k serveru vCenter nebo vSphere. Tento parametr slouží k načtení nejnovějších informací o konfiguraci virtuálních počítačů (např. počet disků, počet jader, počet síťových adaptérů atd.), které se použijí v sestavě. Pokud tento parametr nezadáte, použijí se informace o konfiguraci shromážděné na začátku profilace. |
| [-Password] | Heslo pro připojení k serveru vCenter nebo hostiteli ESXi. Pokud heslo nezadáte jako parametr, budete k jeho zadání vyzváni později při spuštění příkazu. |
| [-DesiredRPO] | Požadovaný cíl bodu obnovení (RPO) v minutách. Výchozí hodnota je 15 minut.|
| [-Bandwidth] | Šířka pásma v Mb/s. Tento parametr slouží k výpočtu cíle bodu obnovení, kterého lze pro zadanou šířku pásma dosáhnout. |
| [-StartDate]  | Počáteční datum a čas ve formátu MM-DD-YYYY:HH:MM (ve 24hodinovém formátu). Parametr StartDate je nutné zadat společně s parametrem EndDate. Pokud zadáte tyto parametry, sestava se vygeneruje pro profilovaná data shromážděná mezi StartDate a EndDate. |
| [-EndDate] | Koncové datum a čas ve formátu MM-DD-YYYY:HH:MM (ve 24hodinovém formátu). Parametr EndDate je nutné zadat společně s parametrem StartDate. Pokud zadáte tyto parametry, sestava se vygeneruje pro profilovaná data shromážděná mezi StartDate a EndDate. |
| [-GrowthFactor] |Faktor růstu v procentech. Výchozí hodnota je 30 %.  |


##### <a name="example-1-to-generate-report-with-default-values-when-profiled-data-is-on-the-local-drive"></a>Příklad 1: Generování sestavy s použitím výchozích hodnot pro profilovaná data umístěná na místním disku
ASRDeploymentPlanner.exe **-Operation** GenerateReport **-Server** vCenter1.contoso.com **-Directory** “E:\vCenter1_ProfiledData” **-VMListFile** “E:\vCenter1_ProfiledData\ProfileVMList1.txt”


##### <a name="example-2-to-generate-report-when-profiled-data-is-on-a-remote-server-user-should-have-readwrite-access-on-the-remote-directory"></a>Příklad 2: Generování sestavy pro profilovaná data umístěná na vzdáleném serveru. Uživatel musí mít ke vzdálenému adresáři přístup ke čtení a zápisu.
ASRDeploymentPlanner.exe **-Operation** GenerateReport **-Server** vCenter1.contoso.com **-Directory** “\\PS1-W2K12R2\vCenter1_ProfiledData” **-VMListFile** “\\PS1-W2K12R2\vCenter1_ProfiledData\ProfileVMList1.txt”

##### <a name="example-3-generate-report-with-specific-bandwidth-and-goal-to-complete-ir-within-specified-time"></a>Příklad 3: Generování sestavy s použitím konkrétní šířky pásma a cíle dokončení prvotní replikace v zadaném čase
ASRDeploymentPlanner.exe **-Operation** GenerateReport **-Server** vCenter1.contoso.com **-Directory** “E:\vCenter1_ProfiledData” **-VMListFile** “E:\vCenter1_ProfiledData\ProfileVMList1.txt” **-Bandwidth** 100 **-GoalToCompleteIR** 24

##### <a name="example-4-generate-report-with-5-growth-factor-instead-of-the-default-30"></a>Příklad 4: Generování sestavy s použitím 5% faktoru růstu namísto výchozích 30 %
ASRDeploymentPlanner.exe **-Operation** GenerateReport **-Server** vCenter1.contoso.com **-Directory** “E:\vCenter1_ProfiledData” **-VMListFile** “E:\vCenter1_ProfiledData\ProfileVMList1.txt” **-GrowthFactor** 5

##### <a name="example-5-generate-report-with-a-subset-of-profiled-data-say-you-have-30-days-of-profiled-data-and-want-to-generate-the-report-for-only-20-days"></a>Příklad 5: Generování sestavy s použitím podmnožiny profilovaných dat. Řekněme, že máte profilovaná data za 30 dnů a chcete vygenerovat sestavu pouze pro 20 dnů.
ASRDeploymentPlanner.exe **-Operation** GenerateReport **-Server** vCenter1.contoso.com **-Directory** “E:\vCenter1_ProfiledData” **-VMListFile** “E:\vCenter1_ProfiledData\ProfileVMList1.txt” **-StartDate** 01-10-2017:12:30 -**EndDate** 01-19-2017:12:30

##### <a name="example-6-generate-report-for-5-minutes-rpo"></a>Příklad 6: Generování sestavy s použitím 5minutového cíle bodu obnovení
ASRDeploymentPlanner.exe **-Operation** GenerateReport **-Server** vCenter1.contoso.com **-Directory** “E:\vCenter1_ProfiledData” **-VMListFile** “E:\vCenter1_ProfiledData\ProfileVMList1.txt” **-DesiredRPO** 5

### <a name="percentile-value-used-for-the-calculation"></a>Hodnota percentilu používaná k výpočtu
**Jaká výchozí hodnota percentilu metrik výkonu shromážděných během profilace se používá při generování sestav?**

Nástroj ve výchozím nastavení používá hodnoty 95. percentilu počtu R/W IOPS, vstupně-výstupních operací zápisu za sekundu a četnosti změn dat shromážděných během profilace všech virtuálních počítačů. To zajišťuje, že se k určení požadavků na cílovou službu Azure Storage a zdrojovou šířku pásma nepoužijí hodnoty 100. percentilu (špičky), které se můžou během profilace objevovat na virtuálních počítačích následkem dočasných událostí, jako je například úloha zálohování spouštěná jednou denně, pravidelné indexování databáze, aktivita generování analytických sestav nebo další podobné krátkodobé a jednorázové události. Použitím hodnot 95. percentilu získáte pravdivou představu o skutečných charakteristikách úloh a nejlepší výkon při spouštění těchto úloh v Microsoft Azure. Neočekáváme, že byste toto číslo často měnili, ale pokud se rozhodnete použít nižší hodnotu, např. 90. percentil, můžete upravit konfigurační soubor ASRDeploymentPlanner.exe.config ve výchozí složce a uložit jej. Tím vygenerujete novou sestavu pro existující profilovaná data.

        &lsaquo;add key="WriteIOPSPercentile" value="95" /&rsaquo;>      
        &lsaquo;add key="ReadWriteIOPSPercentile" value="95" /&rsaquo;>      
        &lsaquo;add key="DataChurnPercentile" value="95" /&rsaquo;

### <a name="growth-factor"></a>Faktor růstu
**Proč by se při plánování nasazení měl brát v úvahu faktor růstu?**

Je důležité počítat s nárůstem v charakteristikách vašich úloh v důsledku možného zvýšení využití v průběhu času. To proto, že jakmile zapnete ochranu a vaše charakteristiky úloh se změní, neexistuje aktuálně žádný způsob, jak přepnout na ochranu pod jiným účtem služby Azure Storage bez nutnosti ochranu zakázat a znovu povolit. Například pokud se dnes virtuální počítač vejde do účtu replikace služby Storage úrovně Standard a třeba za tři měsíce bude nutné jej z důvodu navýšení počtu uživatelů aplikace spuštěné na virtuálním počítači nebo navýšení četnosti změn na virtuálním počítači přesunout do účtu služby Storage úrovně Premium, aby replikace Azure Site Recovery mohla držet krok s četností změn, bude nutné zakázat a znovu povolit ochranu účtu služby Storage úrovně Premium. Proto se důrazně doporučuje při plánování nasazení počítat s růstem a jeho výchozí hodnota je 30 %. Sami nejlépe znáte vzorce používání vaší aplikace a očekávaný růst a během generování sestav můžete toto číslo odpovídajícím způsobem měnit. Ve skutečnosti můžete generovat více sestav s použitím různých faktorů růstu pro stejná profilovaná data a sami uvidíte, jaká doporučení ohledně cílové služby Azure Storage a zdrojové šířky pásma jsou pro vás nejvhodnější.

Vygenerovaná sestava aplikace Microsoft Excel obsahuje následující listy:

* [Input](site-recovery-deployment-planner.md#input) (Vstup)
* [Recommedations](site-recovery-deployment-planner.md#recommendations-with-desired-rpo-as-input) (Doporučení)
* [Recommedations-Bandwidth Input](site-recovery-deployment-planner.md#recommendations-with-available-bandwidth-as-input) (Doporučení – vstupní šířka pásma)
* [VM<->Storage Placement](site-recovery-deployment-planner.md#vm-storage-placement) (Umístění virtuálních počítačů ve službě Storage)
* [Compatible VMs](site-recovery-deployment-planner.md#compatible-vms) (Kompatibilní virtuální počítače)
* [Incompatible VMs](site-recovery-deployment-planner.md#incompatible-vms) (Nekompatibilní virtuální počítače)

![Deployment Planner](./media/site-recovery-deployment-planner/dp-report.png)


##<a name="get-throughput"></a>Zjištění propustnosti
Pokud chcete odhadnout propustnost, které může Azure Site Recovery dosáhnout během replikace z místního prostředí do Azure, spusťte nástroj v režimu GetThroughput. Nástroj vypočítá propustnost ze serveru, na kterém je spuštěn (ideálně server vytvořený podle pokynů k nastavení velikosti konfiguračního serveru).  Pokud jste již místně nasadili komponenty infrastruktury Azure Site Recovery, spusťte nástroj na konfiguračním serveru.

Otevřete konzolu příkazového řádku a přejděte do složky nástroje pro plánování nasazení ASR.  Spusťte ASRDeploymentPlanner.exe s použitím následujících parametrů. Parametry v hranatých závorkách jsou volitelné.

ASRDeploymentPlanner.exe -Operation GetThroughput /?

|Název parametru | Popis |
|-|-|
| -Operation | GetThroughput |
| [-Directory] | Název UNC nebo cesta k místnímu adresáři, ve kterém jsou uložena profilovaná data (soubory vytvořené během profilace). Tato data jsou vyžadovaná k vygenerování sestavy. Pokud tento parametr nezadáte, použije se adresář ProfiledData.  |
| -StorageAccountName | Název účtu služby Azure Storage, u kterého se má zjistit využitá šířka pásma pro replikaci místních dat do Azure. Nástroj zjistí využitou šířku pásma tak, že do tohoto účtu úložiště nahrává testovací data. |
| -StorageAccountKey | Klíč účtu služby Azure Storage, který se použije pro přístup k účtu úložiště. Přejděte na web Azure Portal > Účty úložiště > [název účtu služby Storage] > Nastavení > Přístupové klíče > Klíč1 (nebo Primární přístupový klíč v případě klasického účtu úložiště). |
| -VMListFile | Soubor se seznamem virtuálních počítačů určených k profilaci pro výpočet využité šířky pásma. Cesta k souboru může být absolutní nebo relativní. Tento soubor musí na každém řádku obsahovat jeden název nebo IP adresu virtuálního počítače. Názvy virtuálních počítačů zadané v tomto souboru musí být stejné jako názvy virtuálních počítačů na serveru vCenter nebo hostiteli ESXi.<br>Například soubor VMList.txt obsahuje následující virtuální počítače:<br>virtual machine_A <br>10.150.29.110<br>virtual machine_B|

Nástroj vytvoří v zadaném adresáři několik souborů „asrvhdfile<#>.vhd“ (kde # je číslo) o velikosti 64 MB.  Tyto soubory nahraje do účtu služby Azure Storage a tak zjistí propustnost. Jakmile se změří propustnost, nástroj všechny tyto soubory odstraní z účtu služby Azure Storage i z místního serveru. Pokud se nástroj z nějakého důvodu během výpočtu propustnosti ukončí, ze služby Azure Storage ani z místního serveru tyto soubory neodstraní a budete je muset odstranit ručně.

Propustnost se měří v konkrétním časovém okamžiku a je to maximální propustnost, které Azure Site Recovery může během replikace dosáhnout za předpokladu, že všechny ostatní okolnosti budou stejné. Například pokud ve stejné síti nějaká aplikace začne využívat větší šířku pásma, skutečná propustnost během replikace se bude lišit. Pokud spustíte příkaz GetThroughput z konfiguračního serveru, nástroj nebude vědět o žádných chráněných virtuálních počítačích ani probíhajících replikacích. Výsledná naměřená propustnost se bude lišit, když operaci GetThroughput spustíte v době vysoké četnosti změn dat na chráněných virtuálních počítačích oproti tomu, když ji spustíte v době nízké četnosti změn dat.  Doporučuje se spouštět nástroj během profilace v různých okamžicích, abyste zjistili, jaké propustnosti můžete dosáhnout v různou dobu. V sestavě zobrazí nástroj poslední naměřenou propustnost.


##### <a name="example"></a>Příklad
ASRDeploymentPlanner.exe **-Operation** GetThroughput **-Directory** E:\vCenter1_ProfiledData **-VMListFile** E:\vCenter1_ProfiledData\ProfileVMList1.txt **-StorageAccountName** asrspfarm1 **-StorageAccountKey** by8vdM02xNOcqFlqUwJPLlmEtlCDXJ1OUzFT50uSRZ6IFsuFq2UVErCz4I6tq/K1SZFPTOtr/KBHBeksoGMGw==

>[!NOTE]
>
> * Spouštějte nástroj na serveru, který má stejné charakteristiky úložiště a procesoru jako konfigurační server.
>
> * Pro účely replikace přidělte šířku pásma doporučenou pro splnění cíle bodu obnovení 100 % času. Pokud se ani po přidělení správné šířky pásma nezvýší dosažená propustnost, kterou nástroj hlásí, zkontrolujte následující:
>
> a. Zkontrolujte, jestli v síti není nastavená technologie QoS (Quality of Service), která by omezovala propustnost Azure Site Recovery.
>
> b. Zkontrolujte, jestli je váš trezor Azure Site Recovery v nejbližší podporované oblasti Microsoft Azure kvůli minimalizaci latence sítě.
>
> c. Zkontrolujte charakteristiky vašeho místního úložiště a zvažte vylepšení hardwaru (např. použití jednotek SSD místo HDD atd.).
>
> d. Změňte nastavení Azure Site Recovery na procesovém serveru a [zvětšete šířku pásma sítě používanou k replikaci](./site-recovery-plan-capacity-vmware.md#control-network-bandwidth).
>

##<a name="recommendations-with-desired-rpo-as-input"></a>Doporučení s možností zadat požadovaný cíl bodu obnovení jako vstup

###<a name="profiled-data"></a>Profilovaná data

![Deployment Planner](./media/site-recovery-deployment-planner/profiled-data-period.png)

**Profiled data period** je doba, po kterou byla profilace spuštěná. Nástroj ve výchozím nastavení pro výpočet používá všechna profilovaná data, pokud se sestava negeneruje pro konkrétní období díky použití možností StartDate a EndDate během generování sestavy.

**Server Name** je název nebo IP adresa hostitele ESXi nebo VMware vCenter, pro jehož virtuální počítače se sestava generuje.

**Desired RPO** je požadovaný cíl bodu obnovení (RPO) pro vaše nasazení. Ve výchozím nastavení se požadovaná šířka pásma sítě počítá pro hodnoty cíle bodu obnovení 15, 30 a 60 minut. V závislosti na výběru se na listu aktualizují ovlivněné hodnoty. Pokud jste při generování sestavy použili parametr DesiredRPOinMin, zobrazí se v této rozevírací nabídce Desired RPO použitá hodnota.

###<a name="profiling-overview"></a>Přehled profilace

![Deployment Planner](./media/site-recovery-deployment-planner/profiling-overview.png)

**Total Profiled Virtual Machines** je celkový počet virtuálních počítačů, jejichž profilovaná data jsou k dispozici. Pokud soubor VMListFile obsahuje názvy virtuálních počítačů, které nebyly profilované, nebude se na takové virtuální počítače brát ohled při generování sestav a nezahrnou se do celkového počtu profilovaných virtuálních počítačů.

**Compatible Virtual Machines** je počet virtuálních počítačů, které lze chránit v Azure pomocí Azure Site Recovery. Je to celkový počet kompatibilních virtuálních počítačů, pro které se počítá požadovaná šířka pásma sítě, požadovaný počet účtů služby Azure Storage, počet jader Microsoft Azure a počet konfiguračních serverů a dalších procesových serverů. Podrobnosti o každém kompatibilním virtuálním počítači jsou dostupné na listu sestavy Compatible VMs.

**Incompatible Virtual Machines** je počet profilovaných virtuálních počítačů, které jsou nekompatibilní s ochranou pomocí Azure Site Recovery. Důvody nekompatibility jsou uvedené níže v části Nekompatibilní virtuální počítače. Pokud soubor VMListFile obsahuje názvy virtuálních počítačů, které nebyly profilované, nezahrnou se do celkového počtu nekompatibilních virtuálních počítačů. Takové virtuální počítače jsou uvedené jako „Data not found“ (Data nenalezena) na konci listu Incompatible VMs.

**Desired RPO** (Požadovaný cíl bodu obnovení) je váš požadovaný cíl bodu obnovení v minutách. Sestava se generuje pro tři hodnoty cíle bodu obnovení – 15, 30 a 60 minut, přičemž 15 minut je výchozí hodnota. Doporučení šířky pásma v sestavě se bude měnit v závislosti na výběru možnosti v rozevírací nabídce Desired RPO v pravé horní části listu. Pokud jste sestavu vygenerovali s použitím parametru -DesiredRPO s vlastní hodnotou, v rozevírací nabídce Desired RPO se tato vlastní hodnota zobrazí jako výchozí možnost.

###<a name="required-network-bandwidth-mbps"></a>Požadovaná šířka pásma (Mb/s)

![Deployment Planner](./media/site-recovery-deployment-planner/required-network-bandwidth.png)

**To meet RPO 100% of the time** (Pro splnění cíle bodu obnovení 100 % času): Doporučená šířka pásma v Mb/s, kterou je potřeba přidělit pro splnění požadovaného cíle bodu obnovení 100 % času. Šířka pásma musí být vyhrazená pro zajištění stálé rozdílové replikace všech kompatibilních virtuálních počítačů, aby se předešlo jakémukoli narušení cíle bodu obnovení.

**To meet RPO 90% of the time** (Pro splnění cíle bodu obnovení 90 % času): Pokud kvůli cenám širokopásmového připojení nebo z jiného důvodu nemůžete zřídit šířku pásma potřebnou ke splnění požadovaného cíle bodu obnovení 100 % času, můžete se rozhodnout pro zřízení menší šířky pásma, která může splňovat váš požadovaný cíl bodu obnovení 90 % času. Abyste porozuměli dopadům zřízení menší šířky pásma, sestava poskytuje analýzu „co kdyby“ očekávaného počtu a trvání narušení cíle bodu obnovení.

**Achieved Throughput** (Dosažená propustnost): Propustnost ze serveru, na kterém jste spustili příkaz GetThroughput, do oblasti Microsoft Azure, ve které je umístěný účet služby Azure Storage. Označuje propustnost, které lze dosáhnout při ochraně kompatibilních virtuálních počítačů pomocí Azure Site Recovery za předpokladu, že charakteristiky sítě a úložiště konfiguračního serveru nebo procesového serveru budou stejné jako u serveru, ze kterého jste nástroj spustili.    

Pro účely replikace byste měli přidělit šířku pásma doporučenou pro splnění cíle bodu obnovení 100 % času. Pokud se ani po přidělení správné šířky pásma nezvýší dosažená propustnost, kterou nástroj hlásí, zkontrolujte následující:

a.    Zkontrolujte, jestli v síti není nastavená technologie QoS (Quality of Service), která by omezovala propustnost Azure Site Recovery.

b.    Zkontrolujte, jestli je váš trezor Azure Site Recovery v nejbližší podporované oblasti Microsoft Azure kvůli minimalizaci latence sítě.

c.    Zkontrolujte charakteristiky vašeho místního úložiště a zvažte vylepšení hardwaru (např. použití jednotek SSD místo HDD atd.).

d. Změňte nastavení Azure Site Recovery na procesovém serveru a [zvětšete šířku pásma sítě používanou k replikaci](./site-recovery-plan-capacity-vmware.md#control-network-bandwidth).

V případech, kdy spouštíte nástroj na konfiguračním serveru nebo procesovém serveru, na kterém již jsou chráněné virtuální počítače, spusťte nástroj několikrát, protože hodnota dosažené propustnosti se bude měnit v závislosti na četnosti změn, které se v daný okamžik zpracovávají.

Pro všechna podniková nasazení Azure Site Recovery se doporučuje použít [ExpressRoute](https://aka.ms/expressroute).

###<a name="required-azure-storage-accounts"></a>Požadované účty služby Azure Storage
Tento graf znázorňuje celkový počet účtů služby Azure Storage (úrovně Standard a Premium) požadovaných k ochraně všech kompatibilních virtuálních počítačů.  Pokud chcete zjisti, který účet úložiště byste měli použít pro jednotlivé virtuální počítače, klikněte na [Recommended VM placement plan](site-recovery-deployment-planner.md#vm-storage-placement) (Doporučený plán umístění virtuálních počítačů).  

![Deployment Planner](./media/site-recovery-deployment-planner/required-azure-storage-accounts.png)

###<a name="required-number-of-azure-cores"></a>Požadovaný počet jader Azure
Toto je celkový počet jader, která se mají zřídit před převzetím služeb při selhání nebo testovacím převzetím služeb při selhání všech kompatibilních virtuálních počítačů. Pokud v rámci předplatného není k dispozici dostatečný počet jader, Azure Site Recovery během testovacího převzetí služeb při selhání nebo převzetí služeb při selhání nebude moci vytvořit virtuální počítače.

![Deployment Planner](./media/site-recovery-deployment-planner/required-number-of-azure-cores.png)

###<a name="required-on-premises-infrastructure"></a>Požadovaná místní infrastruktura
Toto je celkový počet konfiguračních serverů a dalších procesových serverů, které se mají nakonfigurovat za účelem zajištění ochrany všech kompatibilních virtuálních počítačů. Nástroj může doporučit další servery v závislosti na podporovaných [omezeních](https://aka.ms/asr-v2a-on-prem-components) nejvyšší konfigurace, tj. omezení denní četnosti změn nebo maximálního počtu chráněných virtuálních počítačů (za předpokladu, že každý z nich obsahuje průměrně tři disky), podle toho, čeho konfigurační server nebo další procesový server dosáhne dříve. Podrobnosti o celkové denní četnosti změn a celkovém počtu chráněných disků jsou dostupné na listu [Input](site-recovery-deployment-planner.md#input).

![Deployment Planner](./media/site-recovery-deployment-planner/required-on-premises-infrastructure.png)

###<a name="what-if-analysis"></a>Analýza „co kdyby“
Tato analýza ukazuje, ke kolika narušením by mohlo dojít během období profilace, pokud zřídíte menší šířku pásma pro splnění požadovaného cíle bodu obnovení pouze 90 % času. Každý den může dojít k jednomu nebo několika narušením cíle bodu obnovení – graf ukazuje pro každý den špičku cíle bodu obnovení.
Na základě této analýzy se můžete rozhodnout, jestli je se zadanou menší šířkou pásma přijatelný počet narušení cíle bodu obnovení ve všech dnech a denní dosažená špička cíle bodu obnovení. Pokud je to přijatelné, můžete pro replikaci zřídit menší šířku pásma, jinak podle návrhu přidělte větší šířku pásma pro splnění požadovaného cíle bodu obnovení 100 % času.

![Deployment Planner](./media/site-recovery-deployment-planner/what-if-analysis.png)

###<a name="recommended-vm-batch-size-for-initial-replication"></a>Doporučená velikost dávky virtuálních počítačů pro prvotní replikaci
V této části najdete doporučený počet virtuálních počítačů, které lze paralelně chránit, pro dokončení prvotní replikace během 72 hodin (tato hodnota je konfigurovatelná – můžete ji změnit pomocí parametru GoalToCompleteIR při vytváření sestavy), a navrhovanou šířku pásma pro splnění požadovaného cíle bodu obnovení 100 % času.  Graf na základě průměrné zjištěné velikosti kompatibilních virtuálních počítačů ukazuje rozsah hodnot šířky pásma a vypočítaného počtu virtuálních počítačů v dávce pro dokončení prvotní replikace během 72 hodin.  

Ve verzi Public Preview sestava neudává, které virtuální počítače by měly být zahrnuté v dávce. Virtuální počítače pro dávku můžete vybrat na základě známých charakteristik úloh nebo podle velikosti jednotlivých virtuálních počítačů – tu můžete zjistit pomocí velikostí disků zobrazených na listu Compatible VMs.  Čas dokončení prvotní replikace se úměrně mění v závislosti na skutečné velikosti disku virtuálního počítače, využitém místu na disku a dostupné propustnosti sítě.

![Deployment Planner](./media/site-recovery-deployment-planner/recommended-vm-batch-size.png)

###<a name="growth-factor-and-percentile-values-used"></a>Použitý faktor růstu a hodnoty percentilu
Tato oblast v dolní části listu ukazuje hodnotu percentilu použitou pro všechny čítače výkonu profilovaných virtuálních počítačů (výchozí je 95. percentil) a faktor růstu v procentech, který se používá ve všech výpočtech (výchozí hodnota je 30 %).

![Deployment Planner](./media/site-recovery-deployment-planner/max-iops-and-data-churn-setting.png)

##<a name="recommendations-with-available-bandwidth-as-input"></a>Doporučení s možností zadat dostupnou šířku pásma jako vstup

![Deployment Planner](./media/site-recovery-deployment-planner/profiling-overview-bandwidth-input.png)

Může nastat situace, kdy víte, že pro účely replikace Azure Site Recovery nemůžete zřídit šířku pásma větší než x Mb/s. Tento nástroj umožňuje zadat dostupnou šířku pásma (pomocí parametru -Bandwidth během generování sestavy) a zjistit dosažitelný cíl bodu obnovení v minutách. Na základě této hodnoty dosažitelného cíle bodu obnovení se můžete rozhodnout, jestli potřebujete zřídit větší šířku pásma, nebo vám vyhovuje řešení zotavení po havárii s tímto cílem bodu obnovení.

![Deployment Planner](./media/site-recovery-deployment-planner/achievable-rpos.png)

##<a name="input"></a>Vstup
Stránka Input (Vstup) poskytuje přehled profilovaného prostředí VMware.

![Deployment Planner](./media/site-recovery-deployment-planner/Input.png)

**Start Date a End Date** jsou počáteční a koncové datum dat profilace zahrnutých do generování sestavy. Ve výchozím nastavení je počátečním datem datum zahájení profilace a koncovým datem je datum zastavení profilace.  Můžou to být hodnoty StartDate a EndDate, pokud se sestava generuje s použitím těchto parametrů. Start Date a End Date: To jsou počáteční a koncové datum dat profilace zahrnutých do generování sestavy. Ve výchozím nastavení je počátečním datem datum zahájení profilace a koncovým datem je datum zastavení profilace.  Můžou to být hodnoty StartDate a EndDate, pokud se sestava generuje s použitím těchto parametrů.

**Total number of profiling days** je celkový počet dnů profilace mezi počátečním a koncovým datem, pro které se generuje sestava. „Total number of profiling days“ je celkový počet dnů profilace mezi počátečním a koncovým datem, pro které se generuje sestava.

**Number of compatible virtual machines** je celkový počet kompatibilních virtuálních počítačů, pro které se počítá požadovaná šířka pásma sítě, požadovaný počet účtů služby Azure Storage, počet jader Microsoft Azure a konfiguračních serverů a dalších procesových serverů.
„Total number of disks across all compatible virtual machines“ je celkový počet disků ve všech kompatibilních virtuálních počítačích. Toto číslo se používá jako jeden ze vstupů pro určení počtu konfiguračních serverů a dalších procesových serverů, které se mají v nasazení použít.

**Average number of disks per compatible virtual machine** je průměrný počet disků ve všech kompatibilních virtuálních počítačích.

**Average disk size (GB)** je vypočítaná průměrná velikost disků ve všech kompatibilních virtuálních počítačích.

**Desired RPO (minutes)** je buď výchozí cíl bodu obnovení, nebo hodnota předaná v parametru DesiredRPO při generování sestavy, sloužící k odhadu požadované šířky pásma.

**Desired bandwidth (Mbps)** je hodnota, kterou jste předali v parametru Bandwidth při generování sestavy, sloužící k odhadu dosažitelného cíle bodu obnovení.

**Observed typical data churn per day (GB)** je průměrná denní četnost změn dat vypozorovaná během všech dnů profilace. Toto číslo se používá jako jeden ze vstupů pro určení počtu konfiguračních serverů a dalších procesových serverů, které se mají v nasazení použít.


##<a name="vm-storage-placement"></a>Umístění virtuálních počítačů ve službě Storage

![Deployment Planner](./media/site-recovery-deployment-planner/vm-storage-placement.png)

**Disk Storage Type** je účet služby Azure Storage úrovně Standard nebo Premium, který slouží k replikaci všech odpovídajících virtuálních počítačů uvedených ve sloupci „VMs to Place“ (Virtuální počítače k umístění).

**Suggested Prefix** je navrhovaná tříznaková předpona, kterou můžete použít k pojmenování účtu služby Azure Storage. Vždycky můžete použít vlastní předponu, ale návrh nástroje se řídí [zásadami vytváření názvů pro oddíly účtů služby Azure Storage](https://aka.ms/storage-performance-checklist).

**Suggested Account Name** ukazuje, jak by měl vypadat váš název účtu služby Azure Storage po zahrnutí navrhované předpony. Název ve špičatých závorkách nahraďte vlastím názvem.

**Log Storage Account:** Všechny protokoly replikace se ukládají v účtu služby Azure Storage úrovně Standard. Pro virtuální počítače, které se replikují do účtu služby Azure Storage úrovně Premium, je potřeba zřídit další účet služby Azure Storage úrovně Standard pro ukládání protokolů. Jeden účet úložiště protokolů úrovně Standard může využívat více účtů úložiště replikace úrovně Premium. Virtuální počítače replikované do účtů úložiště úrovně Standard můžou stejný účet použít i k ukládání protokolů.

**Suggested Log Account Name** ukazuje, jak by měl vypadat váš název účtu služby Azure Storage pro ukládání protokolů po zahrnutí navrhované předpony. Název ve špičatých závorkách nahraďte vlastím názvem.

**Placement Summary** poskytuje souhrn celkové zátěže virtuálních počítačů na účet služby Azure Storage v době replikace a testovacího převzetí služeb při selhání nebo převzetí služeb při selhání. Zahrnuje celkový počet virtuálních počítačů namapovaných na účet služby Azure Storage, celkový počet R/W IOPS ve všech virtuálních počítačích umístěných v tomto účtu služby Azure Storage, celkový počet vstupně-výstupních operací zápisu (replikace) za sekundu, celkovou zřízenou velikost všech disků a celkový počet disků.

**Virtual Machines to Place** obsahuje seznam všech virtuálních počítačů, které by se měly umístit do daného účtu služby Azure Storage pro zajištění optimálního výkonu a využití.

##<a name="compatible-vms"></a>Kompatibilní virtuální počítače
![Deployment Planner](./media/site-recovery-deployment-planner/compatible-vms.png)

**VM Name** je název nebo IP adresa virtuálního počítače tak, jak se použily v souboru VMListFile v okamžiku generování sestav. V tomto sloupci jsou uvedeny také disky (VMDK) připojené k virtuálním počítačům.

**VM Compatibility** (Kompatibilita virtuálního počítače) obsahuje dvě hodnoty – Yes / Yes*. Yes* (Ano) je pro případy, kdy je virtuální počítač vhodný pro [službu Azure Storage úrovně Premium](https://aka.ms/premium-storage-workload) a obsahuje profilovaný disk s vysokou četností změn nebo vysokým počtem IOPS, který spadá do kategorie P20 nebo P30, ale kvůli velikosti se disk mapuje na nižší kategorii P10 nebo P20. Azure Storage určuje, na jaký typ disku služby Storage úrovně Premium se disk bude mapovat, na základě jeho velikosti – menší než 128 GB je P10, 128 až 512 GB je P20 a 512 až 1 023 je P30. Proto pokud se díky charakteristikám úloh disk umístil do kategorie P20 nebo P30, ale kvůli velikosti je namapován na nižší typ disku služby Storage úrovně Premium, nástroj označí příslušný virtuální počítač jako Yes* a doporučí změnit velikost zdrojového disku tak, aby se vešel do správného doporučeného typu disku služby Storage úrovně Premium, nebo po převzetí služeb při selhání změnit typ cílového disku.
„Storage Type“ (Typ služby Storage) je Standard nebo Premium.

**Suggested Prefix** je tříznaková předpona účtu služby Azure Storage.

**Storage Account** je název účtu služby Storage s použitím navrhované předpony.

**R/W IOPS (with Growth Factor)** je počet IOPS na disku ve špičce (výchozí 95. percentil), včetně faktoru budoucího růstu (výchozí hodnota 30 %). Všimněte si, že celkový počet R/W IOPS virtuálního počítače nebude vždy odpovídat součtu R/W IOPS jednotlivých disků virtuálního počítače, protože počet R/W IOPS virtuálního počítače ve špičce je maximální hodnota součtu R/W IOPS jeho jednotlivých disků v každé minutě období profilace.

**Data Churn in Mbps (with Growth Factor)** je četnost změn dat na disku ve špičce (výchozí 95. percentil), včetně faktoru budoucího růstu (výchozí hodnota 30 %). Všimněte si, že celková četnost změn dat virtuálního počítače nebude vždy odpovídat součtu četností změn dat jednotlivých disků virtuálního počítače, protože četnost změn dat virtuálního počítače ve špičce je maximální hodnota součtu četností změn jeho jednotlivých disků v každé minutě období profilace.

**Azure VM Size** je ideální velikost mapovaného virtuálního počítače Azure Compute pro tento místní virtuální počítač. Toto mapování se provádí na základě velikosti paměti, počtu disků, jader nebo síťových adaptérů a počtu R/W IOPS místního virtuálního počítače. Doporučení vždy představuje nejmenší velikost virtuálního počítače Azure, která odpovídá všem těmto charakteristikám místního virtuálního počítače.

**Number of Disks** je celkový počet disků (VMDK) ve virtuálním počítači.

**Disk size (GB)** je celková zřízená velikost všech disků virtuálního počítače. Nástroj ukazuje také velikosti jednotlivých disků ve virtuálním počítači.

**Cores** je počet procesorových jader ve virtuálním počítači.

**Memory (MB)** je paměť RAM ve virtuálním počítači.

**NICs** je počet síťových adaptérů ve virtuálním počítači.

##<a name="incompatible-vms"></a>Nekompatibilní virtuální počítače

![Deployment Planner](./media/site-recovery-deployment-planner/incompatible-vms.png)

**VM Name** je název nebo IP adresa virtuálního počítače tak, jak se použily v souboru VMListFile v okamžiku generování sestav. V tomto sloupci jsou uvedeny také disky (VMDK) připojené k virtuálním počítačům.

**VM Compatibility** označujte, proč je daný virtuální počítač nekompatibilní s Azure Site Recovery. Pro jednotlivé nekompatibilní disky virtuálního počítače jsou uvedené důvody. V závislosti na publikovaných [omezeních](https://aka.ms/azure-storage-scalbility-performance) služby Azure Storage může důvodem být některá z následujících možností.

* Disk size > 1023 GB (Disk větší než 1 023 GB) – Azure Storage v současné době nepodporuje disky větší než 1 TB.
* Total VM size (replication + TFO) exceeds supported Azure Storage account size limit (35 TB) (Celková velikost virtuálního počítače (replikace + testovací převzetí služeb při selhání) překračuje omezení podporované velikosti účtu služby Azure Storage (35 TB)) – K tomu obvykle dochází, když je ve virtuálním počítači jeden disk, jehož některé charakteristiky výkonu překračují maximální podporovaná omezení systému Microsoft Azure nebo Azure Site Recovery pro účet služby Storage úrovně Standard, při jejichž překročení se virtuální počítač dostává do zóny účtu služby Storage úrovně Premium. Nicméně maximální podporovaná velikost účtu služby Azure Storage úrovně Premium je 35 TB a jeden virtuální počítač nemůže být chráněn v rámci několika účtů úložiště. Všimněte si také, že testovací převzetí služeb při selhání spuštěné na chráněném virtuálním počítači poběží v rámci stejného účtu úložiště, ve kterém probíhá replikace. Proto je potřeba zřídit dvojnásobnou velikost disku, aby paralelně mohla probíhat replikace a úspěšné testovací převzetí služeb při selhání.
* Source IOPS exceeds supported Azure Storage IOPS limit of 5000 per disk (Počet zdrojových IOPS překračuje podporované omezení služby Azure Storage – 5 000 IOPS na disk).
* Source IOPS exceeds supported Azure Storage IOPS limit of 80,000 per disk (Počet zdrojových IOPS překračuje podporované omezení služby Azure Storage – 80 000 IOPS na disk).
* Average data churn exceeds supported Azure Site Recovery data churn limit of 10 MBps for average IO size for disk (Průměrná četnost změn dat překračuje podporované omezení Azure Site Recovery pro četnost změn dat – průměrná velikost vstupně-výstupních operací disku 10 Mb/s).
* Total data churn across all disks on the VM exceeds the maximum supported Azure Site Recovery data churn limit of 54 MBps per VM (Celková četnost změn dat na všech discích virtuálního počítače překračuje maximální podporované omezení Azure Site Recovery pro četnost změn dat – 54 Mb/s na virtuální počítač).
* Average effective write IOPS exceeds supported Azure Site Recovery IOPS limit of 840 for disk (Průměrný počet efektivních vstupně-výstupních operací zápisu za sekundu překračuje podporované omezení Azure Site Recovery pro počet IOPS – 840 na disk).
* Calculated snapshot storage exceeding the supported snapshot storage limit of 10 TB (Vypočtená velikost úložiště snímků překračuje podporované omezení velikosti úložiště snímků – 10 TB).

**R/W IOPS (with Growth Factor)** je počet IOPS na disku ve špičce (výchozí 95. percentil), včetně faktoru budoucího růstu (výchozí hodnota 30 %). Všimněte si, že celkový počet R/W IOPS virtuálního počítače nebude vždy odpovídat součtu R/W IOPS jednotlivých disků virtuálního počítače, protože počet R/W IOPS virtuálního počítače ve špičce je maximální hodnota součtu R/W IOPS jeho jednotlivých disků v každé minutě období profilace.

**Data Churn in Mbps (with Growth Factor)** je četnost změn dat na disku ve špičce (výchozí 95. percentil), včetně faktoru budoucího růstu (výchozí hodnota 30 %). Všimněte si, že celková četnost změn dat virtuálního počítače nebude vždy odpovídat součtu četností změn dat jednotlivých disků virtuálního počítače, protože četnost změn dat virtuálního počítače ve špičce je maximální hodnota součtu četností změn jeho jednotlivých disků v každé minutě období profilace.

**Number of Disks** je celkový počet disků (VMDK) ve virtuálním počítači.

**Disk size (GB)** je celková zřízená velikost všech disků virtuálního počítače. Nástroj ukazuje také velikosti jednotlivých disků ve virtuálním počítači.

**Cores** je počet procesorových jader ve virtuálním počítači.

**Memory (MB)** je paměť RAM ve virtuálním počítači.

**NICs** je počet síťových adaptérů ve virtuálním počítači.


##<a name="azure-site-recovery-limits"></a>Omezení Azure Site Recovery

**Cíl ukládání replikace** | **Průměrná velikost vstupně-výstupních operací zdrojového disku** |**Průměrná četnost změn dat zdrojového disku** | **Celková denní četnost změn dat zdrojového disku**
---|---|---|---
Storage úrovně Standard | 8 kB    | 2 MB/s | 168 GB na disk
Disk úrovně Premium P10 | 8 kB    | 2 MB/s | 168 GB na disk
Disk úrovně Premium P10 | 16 kB | 4 MB/s |    336 GB na disk
Disk úrovně Premium P10 | 32 kB nebo větší | 8 MB/s | 672 GB na disk
Disk úrovně Premium P20 nebo P30 | 8 kB    | 5 MB/s | 421 GB na disk
Disk úrovně Premium P20 nebo P30 | 16 kB nebo větší |10 MB/s    | 842 GB na disk


Toto jsou průměrné hodnoty za předpokladu, že se vstupně-výstupní operace z 30 % překrývají. Služba Azure Site Recovery je schopna zpracovávat větší propustnost v závislosti na poměru překrývání, větší velikosti zápisů a skutečného chování vstupně-výstupních operací úloh. Výše uvedená čísla předpokládají typický backlog zhruba 5 minut, což znamená, že zpracování nahrávaných dat a vytvoření bodu obnovení proběhne do 5 minut od nahrání.

Výše uvedená omezení se zakládají na našich testováních, nemůžou však pokrýt všechny možné kombinace vstupně-výstupních operací aplikace. Skutečné výsledky se budou lišit v závislosti na kombinaci vstupně-výstupních operací vaší aplikace. Pro dosažení co nejlepších výsledků i po naplánování nasazení se vždy doporučuje provádět rozsáhlá testování aplikace pomocí testovacího převzetí služeb při selhání, abyste získali skutečnou představu o výkonu.

##<a name="release-notes"></a>Poznámky k verzi
Azure Site Recovery Deployment Planner Public Preview 1.0 má následující známé problémy, které se budou řešit v budoucích aktualizacích.

* Nástroj funguje pouze pro scénář nasazení VMware do Azure, nikoli pro nasazení Hyper-V do Azure. Pro scénář nasazení Hyper-V do Azure použijte [nástroj Hyper-V Capacity Planner](./site-recovery-capacity-planning-for-hyper-v-replication.md).
* Operace GetThroughput není podporována v oblastech Microsoft Azure US Government a Čína.
* Nástroj nemůže profilovat virtuální počítače, pokud server vCenter obsahuje dva nebo více virtuálních počítačů se stejným názvem nebo stejnou IP adresou v různých hostitelích ESXi. V této verzi nástroj při zjištění duplicitních výskytů názvů nebo IP adres virtuálních počítačů v souboru VMListFile profilaci takových virtuálních počítačů přeskočí. Alternativním řešením je profilování virtuálních počítačů pomocí hostitele ESXi namísto serveru vCenter. Pro každého hostitele ESXi je nutné spustit jednu instanci.

