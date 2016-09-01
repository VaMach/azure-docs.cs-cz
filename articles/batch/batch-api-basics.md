<properties
    pageTitle="Přehled funkcí Azure Batch pro vývojáře | Microsoft Azure"
    description="Informace o funkcích služby Batch a jejích rozhraní API z hlediska vývoje."
    services="batch"
    documentationCenter=".net"
    authors="mmacy"
    manager="timlt"
    editor=""/>

<tags
    ms.service="batch"
    ms.devlang="multiple"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.workload="big-compute"
    ms.date="08/12/2016"
    ms.author="marsma"/>

# Přehled funkcí Batch pro vývojáře

V tomto přehledu základních součástí služby Azure Batch probereme primární funkce služby, které mohou vývojáři služby Batch použít k tvorbě rozsáhlých paralelních výpočetních řešení.

Ať vyvíjíte distribuované výpočetní aplikace, nebo službu, která vystavuje přímá volání rozhraní API [Batch REST][batch_rest_api], nebo pracujete s některou ze [sad SDK služby Batch](batch-technical-overview.md#batch-development-apis), budete používat mnoho prostředků a funkcí popsaných v tomto článku.

> [AZURE.TIP] Obecný úvod do služby Batch najdete v části [Základy služby Azure Batch](batch-technical-overview.md).

## Pracovní postup služby Batch

Následující obecný pracovní postup je typický pro téměř všechny aplikace a služby, které používají službu Batch pro zpracování paralelních úloh:

1. Nahrajte **datové soubory**, které chcete zpracovat, na účet [Azure Storage][azure_storage]. Služba Batch obsahuje integrovanou podporu pro přístup ke službě Azure Blob Storage a vaše úkoly si při spuštění mohou stáhnout tyto soubory do [výpočetních uzlů](#compute-node).

2. Nahrajte **soubory aplikace**, které plánujete svými úkoly spouštět. Tyto soubory mohou být binární soubory nebo skripty a jejich závislosti a jsou spouštěny úkoly v úlohách. Vaše úkoly si mohou stahovat tyto soubory z vašeho účtu úložiště, nebo můžete použít funkci [balíčků aplikací](#application-packages) služby Batch pro správu aplikací a nasazení.

3. Vytvořte [fond](#pool) výpočetních uzlů. Při vytváření fondu zadejte počet výpočetní uzlů pro fond, jejich velikost a operační systém. Při spuštění každého úkolu v úloze je tento úkol přiřazen ke spouštění na jednom z uzlů ve vašem fondu.

4. Vytvořte [úlohu](#job). Úloha spravuje kolekci úkolů. Každou úlohu přidružíte ke konkrétnímu fondu, kde budou spouštěny úkoly této úlohy.

5. Přidejte do úlohy [úkoly](#task). Každý úkol spustí aplikaci nebo skript, které jste nahráli, pro zpracování datových souborů, které si úkol stahuje z vašeho účtu úložiště. Při dokončení může úkol nahrát svůj výstup do Azure Storage.

6. Monitorujte průběh úlohy a načtěte výstup úkolu z Azure Storage.

V následujících částech se dozvíte o jednotlivých prostředcích uvedených v tomto pracovním postupu a také o mnoha dalších funkcích služby Batch, které umožní váš distribuovaný výpočetní scénář.

> [AZURE.NOTE] K používání služby Batch budete potřebovat [účet Batch](batch-account-create-portal.md). Kromě toho budou téměř všechna řešení používat pro ukládání a načítání souborů účet [Azure Storage][azure_storage]. Služba Batch aktuálně podporuje pouze účty úložiště s typem **Obecné účely**, jak je popsáno v kroku č. 5 článku [Vytvoření účtu úložiště](../storage/storage-create-storage-account.md#create-a-storage-account) v dokumentu [Informace o účtech Azure Storage](../storage/storage-create-storage-account.md).

## Součásti služby Batch

Některé z následujících prostředků – účty, výpočetní uzly, fondy, úlohy a úkoly – jsou vyžadovány všemi řešeními, která používají službu Batch. Jiné, jako například plány úloh nebo balíčky aplikací, jsou užitečné, ale volitelné funkce.

- [Účet](#account)
- [Výpočetní uzel](#compute-node)
- [Fond](#pool)
- [Úloha](#job)

  - [Plány úlohy](#scheduled-jobs)

- [Úkol](#task)

  - [Spouštěcí úkol](#start-task)
  - [Úkol správce úloh](#job-manager-task)
  - [Úkoly přípravy a uvolnění úloh](#job-preparation-and-release-tasks)
  - [Úkoly s více instancemi (MPI)](#multi-instance-tasks)
  - [Závislosti úkolů](#task-dependencies)

- [Balíčky aplikací](#application-packages)

## Účet

Účet Batch je jednoznačně identifikovaná entita v rámci služby Batch. Veškeré zpracování je přidruženo k účtu Batch. Když provádíte operace se službou Batch, potřebujete jak název účtu, tak i jeden z jeho klíčů. [Účet Azure Batch můžete vytvořit a spravovat na portálu Azure](batch-account-create-portal.md).

## Výpočetní uzel

Výpočetní uzel je virtuální stroj (VM) služby Azure, který je vyhrazen pro zpracování části vašich úloh. Velikost uzlu určuje počet jader procesoru, kapacita paměti a velikost místního systému souborů, který je přidělen k uzlu. Fondy uzlů Windows nebo Linux můžete vytvořit pomocí imagí Azure Cloud Services nebo Virtual Machines Marketplace. Další informace o těchto možnostech najdete v následující části [Fond](#pool).

Uzly mohou spustit libovolný spustitelný soubor nebo skript, který je podporován prostředím operačního systému uzlu. To zahrnuje soubory s příponami \*.exe, \*cmd, \*.bat a skripty PowerShellu pro Windows a binární soubory, shell a Python skripty pro Linux.

Součástí všech výpočetních uzlů ve službě Batch také jsou:

- Standardní [struktura složek](#files-and-directories) a přidružené [proměnné prostředí](#environment-settings-for-tasks), které jsou úkolu k dispozici.
- Nastavení **brány firewall**, která jsou nakonfigurována pro řízení přístupu.
- [Vzdálený přístup](#connecting-to-compute-nodes) k uzlům Windows (Remote Desktop Protocol (RDP)) i Linux (Secure Shell (SSH)).

> [AZURE.NOTE] Podpora Linuxu ve službě Batch je aktuálně ve verzi Preview. Další podrobnosti najdete v článku [Zřízení linuxových výpočetních uzlů ve fondech Azure Batch](batch-linux-nodes.md) 

## Fond

Fond je kolekce uzlů, na kterých běží příslušná aplikace. Fond lze vytvořit ručně nebo automaticky pomocí služby Batch při zadání práce, kterou je zapotřebí vykonat. Můžete vytvořit a spravovat fond, který splňuje požadavky na prostředky příslušné aplikace. Fond může být používán pouze účtem Batch, ve kterém byl vytvořen. Účet Batch může mít více než jeden fond.

Fondy Azure Batch jsou založené na základní výpočetní platformě Azure: fondy Batch zajišťují rozsáhlé přidělení, instalaci aplikací, distribuci dat, monitorování stavu i flexibilní úpravy počtu výpočetních uzlů v rámci fondu ([škálování](#scaling-compute-resources)).

Každému uzlu, který je přidán do fondu, je přiřazen jedinečný název a IP adresa. Pokud je uzel odebrán z fondu, veškeré změny provedené v operačním systému nebo souborech jsou ztraceny a jeho název a IP adresa jsou uvolněny pro pozdější použití. Jestliže uzel opustí fond, je jeho životnost ukončena.

Při vytváření fondu můžete zadat následující atributy:

- **Operační systém** a **verzi** výpočetního uzlu 

    Při výběru operačního systému pro uzly ve vašem fondu máte dvě možnosti: **Konfigurace virtuálního počítače** a **Konfigurace služby Cloud Services**.

    **Konfigurace virtuálního počítače** poskytuje pro výpočetní uzly image systémů Linux i Windows z [Azure Virtual Machines Marketplace][vm_marketplace].
    Při vytváření fondu, který obsahuje uzly z konfigurace virtuálního počítače, je nutné zadat nejen velikost uzlů, ale také **referenční bitové kopie virtuálního počítače** a **agenta uzlu SKU** služby Batch, které se budou na uzly instalovat. Další informace o zadávání těchto vlastností fondů najdete v článku [Zřízení linuxových výpočetních uzlů ve fondech Azure Batch](batch-linux-nodes.md) 

    **Konfigurace služby Cloud Services** poskytuje *pouze* výpočetní uzly Windows. Operační systémy, které jsou k dispozici pro fondy konfigurace služby Cloud Services, jsou uvedeny v [matici kompatibility verzí hostovaného operačního systému Azure a sad SDK](../cloud-services/cloud-services-guestos-update-matrix.md). Při vytváření fondu, který obsahuje uzly Cloud Services, je třeba zadat pouze velikost uzlu a *řadu operačního systému*. Při vytváření fondů výpočetních uzlů s Windows se nejčastěji používá služba Cloud Services.

    - *Řada operačního systému* také určuje, jaké verze rozhraní .NET jsou s operačním systémem nainstalovány.
    - Podobně jako u rolí pracovního procesu v rámci služby Cloud Services lze zadat *verzi operačního systému* (další informace o rolích pracovního procesu najdete v části [Více informací o cloudových službách](../cloud-services/cloud-services-choose-me.md#tell-me-about-cloud-services) v článku [Přehled služby Cloud Services](../cloud-services/cloud-services-choose-me.md)).
    - Stejně jako u rolí pracovního procesu se doporučuje jako *Verze operačního systému* zadat `*`, aby se uzly automaticky upgradovaly a nebyla potřeba žádná práce k ošetření nově vydaných verzí. Hlavním případem použití s výběrem konkrétní verze operačního systému scénář zajištění kompatibility aplikací, který umožní testovat zpětnou kompatibilitu, než se povolí aktualizace verze. Po ověření funkčnosti je možné aktualizovat *verzi operačního systému* pro fond a nainstalovat image nového operačního systému – jakékoli spuštěné úkoly budou přerušeny a znovu zařazeny do fronty.

- **Velikost uzlů**

    Velikosti výpočetních uzlů z **konfigurace služby Cloud Services** jsou uvedeny v seznamu [Velikosti pro Cloud Services](../cloud-services/cloud-services-sizes-specs.md). Služba Batch podporuje všechny velikosti pro Cloud Services kromě `ExtraSmall`.

    Velikosti výpočetních uzlů z **konfigurace virtuálního počítače** jsou uvedeny v seznamech [Velikosti pro virtuální počítače v Azure](../virtual-machines/virtual-machines-linux-sizes.md) (Linux) a [Velikosti pro virtuální počítače v Azure](../virtual-machines/virtual-machines-windows-sizes.md) (Windows). Služba Batch podporuje všechny velikosti VM Azure kromě `STANDARD_A0` a těch, které mají úložiště Premium (série `STANDARD_GS`, `STANDARD_DS` a `STANDARD_DSV2`).

    Při volbě velikosti uzlu je třeba vzít v úvahu charakteristiky a požadavky aplikace nebo aplikací, které na výpočetních uzlech poběží. Velikost uzlu se obvykle volí s předpokladem, že na uzlu poběží současně pouze jeden úkol. K určení nejvhodnější a cenově nejefektivnější velikosti uzlu zvažte například to, jestli je aplikace vícevláknová nebo kolik paměti používá. Je možné mít [paralelně spuštěno](batch-parallel-node-tasks.md) několik úkolů a tudíž i několik instancí aplikace – v takovém případě obvykle vyberete větší uzel. Další informace najdete v následující části „Zásady plánování úkolů“.

    Všechny uzly ve fondu mají stejnou velikost. Jestliže chcete spustit různé aplikace s odlišnými požadavky na systém nebo úrovně zatížení, měli byste pro ně použít oddělené fondy.

- **Cílový počet uzlů**

    Toto je počet výpočetních uzlů, které chcete ve fondu nasadit. Označuje se jako *cílový* proto, že v některých situacích nemusí váš fond dosáhnout požadovaného počtu uzlů. Fond nemusí dosáhnout požadovaného počtu uzlů, pokud dosáhne [základní kvóty](batch-quota-limit.md#batch-account-quotas) pro váš účet Batch nebo pokud existuje vzorec automatického škálování, který jste použili pro fond a který omezuje maximální počet uzlů (viz následující část „Zásady škálování“).

- **Zásady škálování**

    Kromě zadání statického počtu uzlů můžete také napsat a aplikovat na fond [vzorec automatického škálování](#scaling-compute-resources). Služba Batch pak vzorec pravidelně vyhodnocuje a upravuje počet uzlů v rámci fondu na základě různých vámi zadaných parametrů fondu, úloh a úkolů.

- **Zásady plánování úkolů**

    Možnost konfigurace [maximálního počtu úkolů na uzel](batch-parallel-node-tasks.md) určuje maximální počet úkolů, které lze spustit souběžně na jednotlivých výpočetních uzlech v rámci fondu.

    Výchozí konfigurace předpokládá, že na uzlu běží současně jen jeden úkol, ale existují scénáře, kdy je výhodné mít na uzlu spuštěn ve stejnou dobu více než jeden úkol. Jaké výhody vám může přinést více úkolů na jednom uzlu je popsáno v části [příklad scénáře](batch-parallel-node-tasks.md#example-scenario) v článku [souběžné úkoly na uzlu](batch-parallel-node-tasks.md).

    Dále je možné zadat *typ vyplnění*, který určuje, jestli služba Batch rozdělí úkoly rovnoměrně mezi všechny uzly ve fondu, nebo zaneprázdní každý uzel maximálním počtem úkolů před přiřazením úkolů jinému uzlu.

- **Stav komunikace** výpočetních uzlů

    Ve většině scénářů pracují úkoly nezávisle a nepotřebují mezi sebou komunikovat. Mohou však být některé aplikace, ve kterých úkoly musí komunikovat (jako u [scénářů MPI](batch-mpi.md)).

    Fond můžete nakonfigurovat tak, aby umožňoval komunikaci mezi v něm obsaženými uzly – **komunikace mezi uzly**. Pokud je komunikace mezi uzly povolena, uzly ve fondech z konfigurace služby Cloud Services mohou vzájemně komunikovat na portech vyšších než 1100. Fondy z konfigurace virtuálního počítače neomezují provoz na žádném portu.

    Poznámka: povolení komunikace mezi uzly má vliv i na umístění uzlů v clusterech a z důvodu omezení platných pro nasazení může být omezen maximální počet uzlů ve fondu. Pokud aplikace nevyžaduje komunikaci mezi uzly, služba Batch může fondu přidělit potenciálně velký počet uzlů z mnoha různých clusterů a datových center, aby umožnila zvýšený výkon paralelního zpracování.

- **Spouštěcí úkoly** pro výpočetní uzly

    Volitelný *spouštěcí úkol* se spustí na každém uzlu, když se takový uzel připojí k fondu, a také pokaždé, když se uzel restartuje nebo obnoví z image. Spouštěcí úkol je obzvlášť užitečný pro přípravu výpočetních uzlů k provádění úkolů, například k instalaci aplikací, které budou vaše úkoly spouštět.

> [AZURE.IMPORTANT] Všechny účty služby Batch mají výchozí **kvótu**, která omezuje počet **jader** (a tedy výpočetních uzlů) na účtu Batch. Výchozí kvóty a pokyny pro [navýšení kvóty](batch-quota-limit.md#increase-a-quota) (například maximální počet jader na účtu Batch) najdete v článku [Kvóty a omezení služby Azure Batch](batch-quota-limit.md). Možná vás někdy napadne otázka, proč váš fond nedosahuje víc než X uzlů. Příčinou může být tato kvóta na jádra.

## Úloha

Úloha je kolekce úkolů. Řídí, jak se provádí výpočet pomocí jejích úkolů na výpočetních uzlech ve fondu.

- Úloha určuje **fond**, ve kterém poběží práce. Můžete vytvořit nový fond pro každou úlohu nebo použít jeden fond pro mnoho úloh. Můžete vytvořit fond pro každou úlohu, která je přidružená k plánu úlohy, nebo pro všechny úlohy, které jsou spojeny s plánem úlohy.
- Volitelně můžete zadat **prioritu úlohy**. Když je úloha odeslána s vyšší prioritou než aktuálně probíhající úlohy, jsou úkoly úlohy s vyšší prioritou vloženy do fronty před úkoly úloh s nižší prioritou. Úkoly s nižší prioritou, které jsou již spuštěny, nebudou zrušeny.
- Můžete použít **omezení** úlohy k zadání určitých omezení pro úlohy.

    Můžete nastavit **maximální uplynulý čas**, takže pokud některá úloha běží po dobu delší než určený maximální uplynulý čas, úloha a všechny její úkoly budou ukončeny.

    Služba Batch umí zjistit a poté opakovat neúspěšné úkoly. Jako omezení lze zadat **maximální počet opakování úkolů**, včetně toho, jestli je úkol opakován *vždy* nebo *nikdy*. Opakování úkolu znamená, že je úkol znovu zařazen do fronty k opětovnému spuštění.

- Klientská aplikace si může přidat úkoly do úlohy, nebo můžete zadat [úkol správce úloh](#job-manager-task). Úkol správce úloh obsahuje informace potřebné k vytvoření požadovaných úkolů pro úlohu, přičemž úkol správce úloh běží na jednom výpočetním uzlu v rámci fondu. Úkol správce úloh je službou Batch spravován specificky – je zařazen do fronty okamžitě po vytvoření úlohy a restartován, pokud selže. Úkol správce úloh je *vyžadován* pro úlohy vytvořené [plánem úloh](#scheduled-jobs), protože to je jediný způsob, jak definovat úkoly před vytvořením instance úlohy.

### Priorita úloh

Úlohám, které vytvoříte ve službě Batch, lze přiřadit prioritu. Hodnotu priority úlohy používá služba Batch k určení pořadí úlohy v rámci účtu (nezaměňovat s [naplánovanou úlohou](#scheduled-jobs)). Hodnoty priority se pohybují v rozsahu -1000 až 1000, kdy -1000 znamená nejnižší prioritu a 1000 nejvyšší. Prioritu úlohy je možné aktualizovat pomocí operace [Aktualizovat vlastnosti úlohy][rest_update_job] (Batch REST) nebo úpravou vlastnosti [CloudJob.Priority][net_cloudjob_priority] (Batch .NET).

V rámci stejného účtu mají úlohy s vyšší prioritou přednost při plánování před úlohami s nižší prioritou. Úloha s vyšší hodnotou priority v jednom účtu nemá přednost při plánování před jinou úlohou s nižší hodnotou priority v jiném účtu.

Plánování úloh mezi fondy je nezávislé. Mezi různými fondy není zaručeno, že úloha s vyšší prioritou bude naplánována první, pokud její přidružený fond nemá dostatek nečinných uzlů. Ve stejném fondu mají úlohy se stejnou úrovní priority stejnou šanci být naplánované.

### Naplánované úlohy

[Plány úloh][rest_job_schedules] umožňují vytvořit v rámci služby Batch opakované úlohy. Plán úloh určuje, kdy spustit úlohy a obsahuje specifikace pro úlohy, které mají být spuštěny. Můžete určit trvání plánu – na jak dlouho a kdy bude plán platný – a jak často během tohoto časového období by měly být vytvořeny úlohy.

## Úkol

Úkol je jednotka výpočtu, která je přidružena k úloze. Běží na uzlu. Úkoly jsou přiřazeny k uzlu pro provádění nebo jsou zařazeny do fronty, dokud se uzel neuvolní. Jednoduše řečeno, úkol spustí na výpočetním uzlu jeden nebo více programů nebo skriptů k provedení potřebné práce.

Při vytvoření úkolu můžete zadat:

- **Příkazový řádek** úkolu. Toto je příkazový řádek, kterým se na výpočetním uzlu spouští vaše aplikace nebo skript.

    Je důležité si uvědomit, že příkazový řádek ve skutečnosti není spuštěn v shellu. Proto nemůže nativně využívat funkce shellu, jako například rozšíření [proměnné prostředí](#environment-settings-for-tasks) (sem patří i `PATH`). Pokud chcete takové funkce využívat, musíte vyvolat shell v příkazovém řádku, například spuštěním `cmd.exe` na uzlech Windows nebo `/bin/sh` na Linuxu:

    `cmd /c MyTaskApplication.exe %MY_ENV_VAR%`

    `/bin/sh -c MyTaskApplication $MY_ENV_VAR`

    Pokud vaše úkoly potřebují spustit aplikaci nebo skript, které nejsou v `PATH` uzlu nebo v referenčních proměnných prostředí, vyvolejte shell explicitně v příkazovém řádku úkolu.

- **Soubory prostředků** obsahující data, která mají být zpracována. Tyto soubory se před provedením příkazového řádku úkolu automaticky zkopírují do uzlu z Blob Storage v účtu Azure Storage typu **Obecné účely**. Další informace najdete v částech [Spouštěcí úkol](#start-task) a [Soubory a adresáře](#files-and-directories).

- **Proměnné prostředí**, které jsou požadovány příslušnou aplikací. Další informace najdete v části [Nastavení prostředí pro úkoly](#environment-settings-for-tasks).

- **Omezení**, za kterých by měl být proveden úkol. Například: maximální doba, po kterou smí úkol běžet, maximální počet pokusů o opakování neúspěšného úkolu a maximální doba, po kterou jsou zachovány soubory v pracovním adresáři úkolu.

Vedle úkolů, které definujete, aby prováděly výpočty na uzlu, jsou také službou Batch poskytovány následující zvláštní úkoly:

- [Spouštěcí úkol](#start-task)
- [Úkol správce úloh](#job-manager-task)
- [Úkoly přípravy a uvolnění úloh](#job-preparation-and-release-tasks)
- [Úkoly s více instancemi (MPI)](#multi-instance-tasks)
- [Závislosti úkolů](#task-dependencies)

### Spouštěcí úkol

Přidružením **spouštěcího úkolu** k fondu můžete připravit provozní prostředí jeho uzlů. Můžete například provádět instalaci aplikací, které vaše úkoly budou spouštět, nebo spouštět procesy na pozadí. Spouštěcí úkol se spustí při každém spuštění uzlu, a běží tak dlouho, dokud zůstává ve fondu, včetně okamžiků prvního přidání uzlu do fondu a jeho restartování či obnovení z image.

Hlavní výhodou spouštěcího úkolu je, že obsahuje všechny informace potřebné ke konfiguraci výpočetního uzlu a instalaci aplikací nezbytných k provádění úkolu. Proto je zvýšení počtu uzlů ve fondu velmi jednoduché – stačí zadat nový cílový počet uzlů. Služba Batch již má všechny informace potřebné ke konfiguraci nových uzlů a jejich přípravu pro přijetí úkolů.

Stejně jako u každého úkolu služby Azure Batch můžete také zadat seznam **souborů prostředků** ve službě [Azure Storage][azure_storage] (vedle **příkazového řádku**, který má být proveden). Služba Batch nejprve zkopíruje soubory prostředků z úložiště Azure Storage do uzlu a pak spustí příkazový řádek. U spouštěcího úkolu fondu obsahuje seznam souborů obvykle aplikaci úkolu a její závislosti.

Může však také obsahovat referenční data, která budou k dispozici k použití ve všech úkolech spouštěných na výpočetním uzlu. Příkazový řádek spouštěcího úkolu může například provést operaci `robocopy` kopírování souborů aplikace (které byly zadány jako soubory prostředků a staženy do uzlu) z [pracovního adresáře](#files-and-directories) spouštěcího úkolu do [sdílené složky](#files-and-directories) a potom spustit soubor MSI nebo `setup.exe`.

> [AZURE.IMPORTANT] Služba Batch aktuálně podporuje *pouze* účty úložiště s typem **Obecné účely**, jak je popsáno v kroku č. 5 části [Vytvoření účtu úložiště](../storage/storage-create-storage-account.md#create-a-storage-account) v tématu [Informace o účtech Azure Storage](../storage/storage-create-storage-account.md). Úkoly služby Batch (včetně standardních úkolů, spouštěcích úkolů, úkolů přípravy úloh a úkolů uvolnění úloh) musí zadat soubory prostředků, které jsou umístěny *pouze* v účtech úložiště typu **Obecné účely**.

Obvykle je žádoucí, aby služba Batch čekala na dokončení spouštěcího úkolu předtím, než bude uzel považovat za připravený k přiřazení úkolů, ale tuto možnost lze nakonfigurovat.

Pokud na výpočetním uzlu selže spouštěcí úkol, je stav tohoto uzlu aktualizován, aby odrážel selhání a uzel nebude k dispozici pro úlohy, které mají být přiřazeny. Spouštěcí úkol může selhat, jestliže se vyskytl problém s kopírováním jeho souborů prostředků z úložiště nebo pokud proces spuštěný pomocí jeho příkazového řádku vrátí nenulový ukončovací kód.

### Úkol správce úloh

**Úkol správce úloh** se obvykle používá k řízení nebo monitorování provádění úlohy – například k vytvoření a odeslání úkolů pro úlohu, určení dalších úkolů ke spuštění a zjištění, jestli je práce dokončená. Úkol správce úloh však není omezen na tyto aktivity. Jedná se o plnohodnotný úkol, který může provádět všechny akce potřebné pro úlohu. Úkol správce úloh může například stáhnout soubor zadaný jako parametr, analyzovat obsah tohoto souboru a odeslat další úkoly na základě těchto obsahů.

Úkol správce úloh je spuštěn před všemi ostatními úkoly. Má následující funkce:

- Je službou Batch automaticky odeslán jako úkol při vytvoření úlohy.

- Je naplánován ke spuštění před dalšími úkoly v rámci úlohy.

- Jeho přidružený uzel je poslední, který se odebere z fondu, když je fond zmenšován.

- Jeho ukončení může být vázáno na ukončení všech úkolů v úloze.

- Úkolu správce úloh je přiřazena nejvyšší priorita, když je nutné jej restartovat. Pokud není k dispozici nečinný uzel, může služba Batch ukončit jeden z ostatních spuštěných úkolů ve fondu, aby uvolnila prostor pro spuštění úkolu správce úloh.

- Úkol správce úloh v jedné úloze nemá přednost před úkoly jiných úloh. Mezi úlohami jsou dodržovány pouze priority s úrovní úlohy.

### Úkoly přípravy a uvolnění úloh

Služba Batch zajišťuje úkoly přípravy úloh pro nastavení před provedením úlohy. Úkoly uvolnění úloh jsou určeny pro údržbu nebo čištění po provedení úlohy.

- **Úkol přípravy úlohy**: Úkol přípravy úlohy je spuštěn před spuštěním jiných úkolů úlohy na všech výpočetních uzlech, na kterých je naplánované spuštění úkolů. Úkol přípravy úlohy lze například použít ke zkopírování dat, která jsou sdílena všemi úkoly, ale jsou jedinečná pro úlohu.
- **Úkol uvolnění úlohy**: Po dokončení úlohy se na každém uzlu ve fondu, na kterém se spustil alespoň jeden úkol, spustí úkol uvolnění úlohy. Pomocí úkolu uvolnění úlohy můžete například odstranit data zkopírovaná pomocí úkolu přípravy úlohy nebo zkomprimovat a nahrát data diagnostického protokolu.

Úkoly přípravy i uvolnění úloh umožňují zadat příkazový řádek, který se provede, když je úkol vyvolán. Často nabízejí funkce jako stahování souborů, provádění se zvýšenými oprávněními, vlastní proměnné prostředí, maximální dobu provádění, počet opakování a dobu uchovávání souboru.

Další informace ohledně úkolů přípravy a uvolnění úloh najdete v části [Spouštění úkolů přípravy a dokončení úlohy na výpočetních uzlech Azure Batch](batch-job-prep-release.md).

### Úkoly s více instancemi

[Úkol s více instancemi](batch-mpi.md) je úkol, který je nakonfigurován, aby byl současně spuštěn na více než jednom výpočetním uzlu. Pomocí úkolů s více instancemi můžete povolit výpočetní scénáře s vysokým výkonem, které vyžadují skupinu výpočetních uzlů přidělených společně na zpracování jediné úlohy (například rozhraní MPI (Message Passing Interface)).

Podrobné informace o spouštění úloh MPI ve službě Batch pomocí knihovny Batch .NET najdete v článku [Použití úkolů s více instancemi ke spouštění aplikací rozhraní MPI (Message Passing Interface) v Azure Batch](batch-mpi.md).

### Závislosti úkolů

[Závislosti úkolů](batch-task-dependencies.md), jak již název napovídá, umožňují určit, jestli úkol závisí před svým spuštěním na dokončení jiných úkolů. Tato funkce poskytuje podporu pro situace, ve kterých „podřízený“ úkol spotřebovává výstup „nadřazeného“ úkolu, nebo kde nadřazený úkol provádí inicializaci, která je požadovaná podřízeným úkolem. Pokud chcete tuto funkci používat, je nutné nejprve v úloze služby Batch povolit závislosti úkolů. Pak pro každý úkol, který závisí na jiném (nebo mnoha jiných), zadáte úkoly, na kterých tento úkol závisí.

Pomocí závislosti úkolů lze nakonfigurovat například následující scénáře:

* *Úkol B* závisí na *úkolu A* (*úkol B* nezahájí provádění, dokud se nedokončí *úkol A*).
* *Úkol C* závisí na *úkolu A* i *úkolu B*.
* *Úkol D* závisí na celé řadě úkolů, například na úkolu *1* až *10*, než se provede

Podívejte se na článek [Závislosti úkolů v Azure Batch](batch-task-dependencies.md) a ukázku kódu [TaskDependencies][github_sample_taskdeps] v úložišti GitHub [azure-batch-samples][github_samples], kde najdete podrobnější informace o této funkci.

## Nastavení prostředí pro úlohy

Každý úkol, který se spustí v rámci služby Batch, má přístup k proměnným prostředí, které jsou jednak nastaveny službou Batch (definovány službou, jak je popsáno v následující tabulce), jednak jde o vlastní proměnné prostředí, které si můžete nastavit pro své úkoly. Aplikace a skripty spouštěné úkoly na uzlech mají během zpracování přístup do těchto proměnných prostředí.

Vlastní proměnné prostředí můžete nastavit na úrovni úkolů či úloh tím, že vyplníte vlastnost *nastavení prostředí* pro tyto entity. Viz například operace [Přidávání úkolu do úlohy][rest_add_task] (Batch REST API) nebo vlastnosti [CloudTask.EnvironmentSettings][net_cloudtask_env] a [CloudJob.CommonEnvironmentSettings][net_job_env] v Batch .NET.

Vaše klientská aplikace nebo služba může získat službou i uživatelem definované proměnné prostředí úkolu pomocí operace [Získat informace o úkolu][rest_get_task_info] (Batch REST) nebo přístupem k vlastnosti [CloudTask.EnvironmentSettings][net_cloudtask_env] (Batch .NET). Procesy prováděné na výpočetním uzlu mohou také přistupovat k těmto i ostatním proměnným prostředí na uzlu například pomocí známé syntaxe `%VARIABLE_NAME%` (Windows) nebo `$VARIABLE_NAME` (Linux).

Následující proměnné prostředí jsou nastaveny službou Batch a jsou přístupné pro vaše úkoly:

| Název proměnné prostředí       | Popis                                                              |
|---------------------------------|--------------------------------------------------------------------------|
| `AZ_BATCH_ACCOUNT_NAME`         | Název účtu, ke kterému úkol patří.                       |
| `AZ_BATCH_JOB_ID`               | Číslo ID úlohy, ke které úkol patří.                             |
| `AZ_BATCH_JOB_PREP_DIR`         | Úplná cesta adresáře úkolu přípravy úlohy na uzlu.         |
| `AZ_BATCH_JOB_PREP_WORKING_DIR` | Úplná cesta pracovního adresáře úkolu přípravy úlohy na uzlu. |
| `AZ_BATCH_NODE_ID`              | Číslo ID uzlu, na kterém je spuštěn úkol.                         |
| `AZ_BATCH_NODE_ROOT_DIR`        | Úplná cesta kořenového adresáře na uzlu.                         |
| `AZ_BATCH_NODE_SHARED_DIR`      | Úplná cesta sdíleného adresáře na uzlu.                       |
| `AZ_BATCH_NODE_STARTUP_DIR`     | Úplná cesta úkolu po spuštění výpočetního uzlu adresáře na uzlu.    |
| `AZ_BATCH_POOL_ID`              | Číslo ID fondu, ve kterém je spuštěn úkol.                         |
| `AZ_BATCH_TASK_DIR`             | Úplná cesta adresáře úkolu na uzlu.                         |
| `AZ_BATCH_TASK_ID`              | Číslo ID aktuálního úkolu.                                              |
| `AZ_BATCH_TASK_WORKING_DIR`     | Úplná cesta pracovního adresáře úkolu na uzlu.                 |

>[AZURE.IMPORTANT] Tyto proměnné prostředí jsou k dispozici pouze v kontextu **uživatele úkolu** – to znamená uživatelský účet v uzlu, pod kterým je úkol spuštěn. *Neuvidíte je*, pokud se k výpočetnímu uzlu [vzdáleně připojíte](#connecting-to-compute-nodes) prostřednictvím protokolu RDP (Remote Desktop) nebo Secure Shell (SSH) a vypíšete si seznam proměnných prostředí. Je to proto, že uživatelský účet, který se používá pro vzdálené připojení, není stejný jako účet, který je používán úkolem.

## Soubory a adresáře

Každý úkol má *pracovní adresář*, pod kterým vytváří žádný nebo více souborů a adresářů. Tento pracovní adresář lze použít pro ukládání programu, který je spuštěn úkolem, dat, která zpracovává, a výstupu zpracování, které provádí. Všechny soubory a adresáře úkolu jsou vlastněné uživatelem úkolu.

Služba Batch zveřejňuje část systému souborů na uzlu jako *kořenový adresář*. Úkoly mohou získat přístup do kořenového adresáře odkazem na proměnnou prostředí `AZ_BATCH_NODE_ROOT_DIR`. Další informace o používání proměnných prostředí najdete v tématu [Nastavení prostředí pro úkoly](#environment-settings-for-tasks).

Kořenový adresář obsahuje následující adresářovou strukturu:

![Adresářová struktura výpočetního uzlu][1]

- **shared**: Tento adresář poskytuje přístup pro čtení a zápis pro *všechny* úkoly, které jsou spouštěny na uzlu. Každý úkol spuštěný na uzlu může vytvořit, číst, aktualizovat a odstranit soubory v tomto adresáři. Úkoly mohou získat přístup do tohoto adresáře odkazem na proměnnou prostředí `AZ_BATCH_NODE_SHARED_DIR`.

- **startup**: Tento adresář je využíván spouštěcím úkolem jako jeho pracovní adresář. Jsou sem uloženy všechny soubory, které byly staženy do uzlu spouštěcím úkolem. Spouštěcí úkol může vytvořit, číst, aktualizovat a odstranit soubory v tomto adresáři. Úkoly mohou získat přístup do tohoto adresáře odkazem na proměnnou prostředí `AZ_BATCH_NODE_STARTUP_DIR`.

- **Úkoly**: Pro každý úkol, který běží na uzlu, je vytvořen adresář. Přistupuje se k němu odkazem na proměnnou prostředí `AZ_BATCH_TASK_DIR`.

    V rámci každého adresáře úkolu vytvoří služba Batch pracovní adresář (`wd`), jehož jedinečná cesta je určena proměnnou prostředí `AZ_BATCH_TASK_WORKING_DIR`. Tento adresář poskytuje přístup pro čtení a zápis pro úkol. Úkol může vytvořit, číst, aktualizovat a odstranit soubory v tomto adresáři. Tento adresář je zachován podle pravidel omezení *RetentionTime*, které je zadáno pro úkol.

    `stdout.txt` a `stderr.txt`: Tyto soubory jsou zapsány do složky úkolu během provádění úkolu.

>[AZURE.IMPORTANT] Pokud je uzel odebrán z fondu, jsou odebrány *všechny* soubory, které jsou uloženy na uzlu.

## Balíčky aplikací

Funkce [balíčků aplikací](batch-application-packages.md) poskytuje snadnou správu a nasazení aplikací na výpočetní uzly ve fondech. S balíčky aplikací lze snadno nahrávat a spravovat více verzí aplikací spouštěných prostřednictvím úkolů, včetně binárních a podpůrných souborů. Poté můžete automaticky nasadit jednu nebo více těchto aplikací na výpočetní uzly ve fondu.

Služba Batch zpracovává podrobnosti o práci s úložištěm Azure Storage na pozadí, aby bezpečně ukládala a nasazovala balíčky aplikací na výpočetní uzly, čímž lze zjednodušit režijní náklady na kód a správu.

Pokud chcete získat další informace o funkci balíčku aplikace, projděte si dokument [Nasazení aplikací pomocí balíčků aplikací Azure Batch](batch-application-packages.md).

## Životnost fondu a výpočetního uzlu

Při navrhování řešení Azure Batch musíte přijmout rozhodnutí o návrhu ohledně toho, kdy a jak jsou fondy vytvářeny a jak dlouho jsou výpočetní uzly v rámci těchto fondů udržovány dostupné.

Na jednom konci spektra můžete vytvořit fond pro každou úlohu, když je úloha odeslána, a odebrat jeho uzly, jakmile úkoly dokončí provádění. Tím se maximalizuje využití, neboť uzly jsou přiděleny pouze tehdy, když je to nezbytně nutné, a vypnuty, jakmile jsou nečinné. To znamená, že úloha musí čekat, až budou uzly přiděleny. Je důležité si uvědomit, že úkoly budou naplánovány na uzly, jakmile jsou jednotlivě k dispozici, přiděleny a spouštěcí úkol je dokončen. Služba Batch před přiřazením úkolů *nečeká*, až budou všechny uzly v rámci fondu dostupné. Tím je zajištěno maximální využití všech dostupných uzlů.

Na druhém konci spektra, pokud je nejvyšší prioritou okamžité spuštění úloh, můžete fond vytvořit s předstihem, aby jeho uzly byly k dispozici před odesláním úloh. V tomto scénáři se mohou úkoly úloh spustit okamžitě, ale uzly mohou „nečinně sedět“ a čekat na přiřazení úkolů.

V případě zpracovávání proměnlivého, ale stálého zatížení se obvykle používá kombinovaný přístup. Můžete mít fond, do nějž je odesíláno více úloh a který může podle zatížení úlohami měnit počet uzlů nahoru nebo dolů (viz [Škálování výpočetních prostředků](#scaling-compute-resources) v následující části). Toto přizpůsobování kapacity můžete provádět reaktivně, na základě aktuálního zatížení, nebo proaktivně, pokud lze zatížení předpovídat.

## Škálování výpočetních prostředků

Služba Batch může díky [automatickému škálování](batch-automatic-scaling.md) dynamicky upravit počet výpočetních uzlů ve fondu podle aktuálního zatížení a využití prostředků výpočetního scénáře. To umožňuje snížit celkové náklady na běh aplikace, protože se využívají pouze prostředky, které jsou nutné, a aktuálně nepotřebné se uvolňují.

Automatické škálování můžete zapnout napsáním [vzorce automatického škálování](batch-automatic-scaling.md#automatic-scaling-formulas) a jeho přidružením k fondu. Služba Batch používá tento vzorec k určování cílového počtu uzlů ve fondu pro další interval škálování (ten můžete nakonfigurovat). Nastavení automatického škálování můžete pro fond zadat při jeho vytvoření, nebo můžete škálování povolit ve fondu později. Také můžete aktualizovat nastavení škálování ve fondu, v němž je škálování povoleno.

Jako příklad si vezměme úlohu, která může vyžadovat odeslání velkého počtu úkolů ke spuštění. Fondu můžete přiřadit vzorec škálování na základě aktuálního počtu úkolů čekajících ve frontě a rychlosti dokončování těchto úkolů v úloze, který přizpůsobí počet uzlů ve fondu. Služba Batch pravidelně vyhodnocuje vzorec a mění velikost fondu na základě pracovního vytížení (přidá uzly v případě mnoha úkolů zařazených do fronty a odebere uzly v případě, že žádné úkoly nejsou ve frontě ani neběží) a dalších nastavení vzorce.

Vzorec škálování může být založen na následujících metrikách:

- **Časové metriky** jsou založeny na statistikách shromažďovaných každých pět minut po zadaný počet hodin.

- **Metriky prostředků** jsou založeny na využití procesoru, šířky pásma a paměti a na počtu uzlů.

- **Metriky úkolů** jsou založeny na stavu úkolů, jako je například *Aktivní* (zařazený do fronty), *Spuštěný* nebo *Dokončený*.

Když automatické škálování snižuje počet výpočetních uzlů ve fondu, je nutné zvážit, jak naložit s úkoly, které v okamžiku snižování již běží. K tomuto účelu poskytuje služba Batch *možnost zrušení přidělení uzlu*, kterou můžete ve vzorcích používat. Můžete například zadat, že spuštěné úkoly se mají okamžitě zastavit, okamžitě zastavit a pak znovu zařadit do fronty pro provedení na jiném uzlu, nebo nechat dokončit před odebráním uzlu z fondu.

Další informace o automatickém škálování aplikace najdete v tématu [Automatické škálování výpočetních uzlů ve fondu Azure Batch](batch-automatic-scaling.md).

> [AZURE.TIP] Pokud chcete maximalizovat využití výpočetních prostředků, nastavte cílový počet uzlů na konci úlohy na hodnotu nula, ale povolte dokončení spouštěných úkolů.

## Zabezpečení pomocí certifikátů

Při šifrování nebo dešifrování citlivých informací pro úkoly, jako například klíč pro [účet Azure Storage][azure_storage], je obvykle třeba použít certifikáty. Z toho důvodu můžete na uzly nainstalovat certifikáty. Šifrované tajné klíče jsou předány na úkoly prostřednictvím parametrů příkazového řádku nebo vložené v jednom prostředků úkolu a nainstalované certifikáty lze použít pro jejich dešifrování.

K přidání certifikátu do účtu služby Batch můžete použít operaci [Přidat certifikát][rest_add_cert] (Batch REST) nebo metodu [CertificateOperations.CreateCertificate][net_create_cert] (Batch .NET). Poté můžete certifikát přidružit k novému nebo existujícímu fondu. Pokud je certifikát přidružený k fondu, služba Batch nainstaluje certifikát na každý uzel ve fondu. Služba Batch nainstaluje příslušné certifikáty při spuštění uzlu, ještě před spuštěním úkolů (včetně spouštěcího úkolu a úkolu správce úloh).

## Zpracování chyb

V rámci vašeho řešení Batch může být nutné ošetřit selhání úkolů a aplikací.

### Zpracování selhání úkolů
Selhání úkolů spadá do následujících kategorií:

- **Selhání plánování**

    Pokud z nějakého důvodu selže přenos souborů zadaných pro úkol, je pro úkol nastavena „chyba plánování“.

    Chyby plánování mohou nastat, pokud byly soubory prostředků úkolu přesunuty, pokud účet Storage již není k dispozici, nebo v případě jiného problému, který zabránil úspěšnému zkopírování souborů do uzlu.

- **Selhání aplikace**

    Selhat může i proces zadaný pomocí příkazového řádku úkolu. Proces se považuje za neúspěšný, pokud proces provedený úkolem vrátí ukončovací kód s nenulovou hodnotou (viz *Ukončovací kódy úkolů* v následující části).

    Pro případy selhání aplikace je možné nakonfigurovat službu Batch tak, aby automaticky opakovala úkol až do zadaného počtu opakování.

- **Selhání omezení**

    Pomocí proměnné *maxWallClockTime* můžete nastavit omezení, které určuje maximální dobu trvání provádění úlohy nebo úkolu. To může být užitečné pro ukončení "neodpovídající" úlohy.

    Při překročení maximální doby je úkol označen jako *dokončený*, ale ukončovací kód je nastaven na hodnotu `0xC000013A` a pole *schedulingError* bude označeno jako `{ category:"ServerError", code="TaskEnded"}`.

### Ladění chyb aplikace

- `stderr` a `stdout`

    Během provádění může aplikace generovat diagnostický výstup, který lze použít k řešení potíží. Jak je uvedeno v části [Soubory a adresáře](#files-and-directories) výše, zapíše služba Batch standardní výstup a standardní chybový výstup do souborů `stdout.txt` a `stderr.txt` v adresáři úkolů na výpočetním uzlu. Tyto soubory můžete stáhnout pomocí webu Azure Portal nebo jedné ze sad SDK služby Batch. Tyto a další soubory můžete pro účely odstraňování potíží načíst například v knihovně Batch .NET pomocí funkcí [ComputeNode.GetNodeFile][net_getfile_node] a [CloudTask.GetNodeFile][net_getfile_task].

- **Ukončovací kódy úkolů**

    Jak bylo zmíněno výše, je úkol službou Batch označen jako neúspěšný, pokud proces, který je úkolem spuštěn, vrátí nenulový ukončovací kód. Pokud úkol spustí proces, služba Batch vyplní vlastnost ukončovacího kódu úkolu *návratovým kódem procesu*. Je důležité poznamenat, že ukončovací kód úkolu **není** určován službou Batch – je určen samotným procesem nebo operačním systémem, na kterém je proces spuštěn.

### Monitorování účtů pro selhání úkolů nebo přerušení

Úkoly mohou občas selhat nebo být přerušeny. Selhat může samotná aplikace úkolu, může dojít k restartování uzlu, na kterém je úkol spuštěn, nebo může být uzel odebrán z fondu během operace změny velikosti fondu, pokud jsou zásady deaktivace přidělení fondu nastaveny na odebrání uzlů okamžitě bez čekání na dokončení úkolů. Ve všech případech může být úkol službou Batch automaticky znovu zařazen do fronty, aby byl spuštěn na jiném uzlu.

Je také možné, že občasný problém způsobí, že se úkol zablokuje nebo že jeho spuštění trvá příliš dlouho. Pro úkol můžete nastavit maximální dobu provádění. Pokud je překročena, služba Batch přeruší aplikaci úkolu.

### Připojení k výpočetním uzlům

Další ladění a řešení potíží můžete provádět při vzdáleném přihlášení k výpočetnímu uzlu. Pro uzly Windows si můžete na portálu Azure stáhnout soubor protokolu RDP (Remote Desktop) a pro uzly Linux získat informace o připojení Secure Shell (SSH). Lze to také provést pomocí rozhraní API služby Batch – například s [Batch .NET][net_rdpfile] nebo [Batch Python](batch-linux-nodes.md#connect-to-linux-nodes).

>[AZURE.IMPORTANT] Pokud se chcete připojit k uzlu prostřednictvím protokolu RDP nebo SSH, musíte na uzlu nejprve vytvořit uživatele. Můžete to provést takto: na webu Azure Portal [přidáte uživatelský účet do uzlu][rest_create_user] pomocí rozhraní Batch REST API a zavoláte metodu [ComputeNode.CreateComputeNodeUser][net_create_user] v Batch .NET nebo zavoláte metodu [add_user][py_add_user] v modulu Batch Python module.

### Řešení potíží "špatných" výpočetních uzlů

V situacích, kdy některé úkoly selhávají, může klientská aplikace nebo služba Batch prozkoumat metadata neúspěšných úkolů, aby identifikovala uzel, který se chová nesprávně. Každý uzel ve fondu má přiřazeno jedinečné číslo ID a uzel, na kterém je spuštěn úkol, je zahrnut v metadatech úkolu. Po identifikaci problémového uzlu s ním můžete provést několik akcí:

- **Restartovat uzel** ([REST][rest_reboot] | [.NET][net_reboot])

    Restartování uzlu může někdy odstranit latentní problémy, jako jsou zablokované nebo zhroucené procesy. Vezměte na vědomí, že spouštěcí úkol nebo úkol přípravy úlohy bude po restartování uzlu proveden, pokud jej váš fond používá.

- **Obnovit uzel z image** ([REST][rest_reimage] | [.NET][net_reimage])

    Tato možnost přeinstaluje operační systém na uzlu. Stejně jako u restartování uzlu jsou spouštěcí úkoly a úkoly přípravy úlohy znovu spuštěny poté, co uzel byl obnoven z image.

- **Odebrat uzel z fondu** ([REST][rest_remove] | [.NET][net_remove])

    Někdy je nezbytné úplně odebrat uzel z fondu.

- **Zakázat plánování úkolů v uzlu** ([REST][rest_offline] | [.NET][net_offline])

    To efektivně převede uzel "do režimu offline" tak, aby mu nebyly přiřazeny žádné další úlohy, ale umožňuje, aby zůstal spuštěný a ve fondu. Díky tomu můžete provést další šetření příčin selhání bez ztráty dat neúspěšného úkolu, a aniž by uzel způsobil selhání dalších úkolů. Můžete například zakázat plánování úloh na uzlu a pak se [přihlásit vzdáleně](#connecting-to-compute-nodes) a prohlédnout si protokoly událostí uzlu nebo provádět jiné řešení potíží. Jakmile dokončíte šetření, můžete převést uzel zpět do online režimu povolením plánování úkolů ([REST][rest_online] | [.NET][net_online]) nebo provést některou z dalších akcí diskutovaných výše.

> [AZURE.IMPORTANT] U každé akce uvedené v této části – restartování, obnovení z image, odebrání, zakázání plánování úkolů – můžete určit, jak se při provedení akce naloží s úkoly, které na uzlu aktuálně běží. Pokud například zakážete plánování úkolů na uzlu pomocí klientské knihovny Batch .NET, můžete zadat výčtovou hodnotu [DisableComputeNodeSchedulingOption][net_offline_option] a určit, jestli spuštěné úkoly ukončit (**Terminate**), znovu zařadit do fronty **Requeue** pro naplánování na jiných uzlech, nebo umožnit dokončení běžících úkolů před provedením akce (**TaskCompletion**).

## Další kroky

- Projděte si podrobně ukázkovou aplikaci služby Batch v tématu [Začínáme s knihovnou služby Azure Batch pro .NET](batch-dotnet-get-started.md). K dispozici je také [verze pro Python](batch-python-tutorial.md) tohoto kurzu, která spouští úlohy na výpočetních uzlech systému Linux.

- Stáhněte si a sestavte ukázkový projekt [Batch Explorer][github_batchexplorer] pro použití při vývoji řešení Batch. Pomocí projektu Batch Explorer můžete provést následující a další akce:
  - Monitorování a manipulace s fondy, úlohami a úkoly v rámci vašeho účtu Batch
  - Stažení `stdout.txt`, `stderr.txt` a dalších souborů z uzlů
  - Vytvořit uživatele na uzlech a stáhnout soubory protokolu RDP pro vzdálené přihlášení

- Zjistěte, jak [vytvořit fondy výpočetních uzlů Linux](batch-linux-nodes.md).

- Navštivte [fórum Azure Batch][batch_forum] na webu MSDN. Fórum je vhodné místo pro kladení dotazů, ať se teprve učíte, nebo jste odborníky na používání služby Batch.

[1]: ./media/batch-api-basics/node-folder-structure.png

[azure_storage]: https://azure.microsoft.com/services/storage/
[batch_forum]: https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurebatch
[cloud_service_sizes]: ../cloud-services/cloud-services-sizes-specs.md
[msmpi]: https://msdn.microsoft.com/library/bb524831.aspx
[github_samples]: https://github.com/Azure/azure-batch-samples
[github_sample_taskdeps]:  https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/TaskDependencies
[github_batchexplorer]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/BatchExplorer
[batch_net_api]: https://msdn.microsoft.com/library/azure/mt348682.aspx
[net_cloudjob_jobmanagertask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.jobmanagertask.aspx
[net_cloudjob_priority]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.priority.aspx
[net_cloudpool_starttask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.starttask.aspx
[net_cloudtask_env]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.environmentsettings.aspx
[net_create_cert]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.certificateoperations.createcertificate.aspx
[net_create_user]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.createcomputenodeuser.aspx
[net_getfile_node]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.getnodefile.aspx
[net_getfile_task]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.getnodefile.aspx
[net_job_env]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.commonenvironmentsettings.aspx
[net_multiinstancesettings]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.multiinstancesettings.aspx
[net_rdp]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.getrdpfile.aspx
[net_reboot]: https://msdn.microsoft.com/library/azure/mt631495.aspx
[net_reimage]: https://msdn.microsoft.com/library/azure/mt631496.aspx
[net_remove]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.removefrompoolasync.aspx
[net_offline]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.disableschedulingasync.aspx
[net_online]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.enableschedulingasync.aspx
[net_offline_option]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.common.disablecomputenodeschedulingoption.aspx
[net_rdpfile]: https://msdn.microsoft.com/library/azure/Mt272127.aspx

[py_add_user]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.operations.html#azure.batch.operations.ComputeNodeOperations.add_user

[batch_rest_api]: https://msdn.microsoft.com/library/azure/Dn820158.aspx
[rest_add_job]: https://msdn.microsoft.com/library/azure/mt282178.aspx
[rest_add_pool]: https://msdn.microsoft.com/library/azure/dn820174.aspx
[rest_add_cert]: https://msdn.microsoft.com/library/azure/dn820169.aspx
[rest_add_task]: https://msdn.microsoft.com/library/azure/dn820105.aspx
[rest_create_user]: https://msdn.microsoft.com/library/azure/dn820137.aspx
[rest_get_task_info]: https://msdn.microsoft.com/library/azure/dn820133.aspx
[rest_job_schedules]: https://msdn.microsoft.com/library/azure/mt282179.aspx
[rest_multiinstance]: https://msdn.microsoft.com/library/azure/mt637905.aspx
[rest_multiinstancesettings]: https://msdn.microsoft.com/library/azure/dn820105.aspx#multiInstanceSettings
[rest_update_job]: https://msdn.microsoft.com/library/azure/dn820162.aspx
[rest_rdp]: https://msdn.microsoft.com/library/azure/dn820120.aspx
[rest_reboot]: https://msdn.microsoft.com/library/azure/dn820171.aspx
[rest_reimage]: https://msdn.microsoft.com/library/azure/dn820157.aspx
[rest_remove]: https://msdn.microsoft.com/library/azure/dn820194.aspx
[rest_offline]: https://msdn.microsoft.com/library/azure/mt637904.aspx
[rest_online]: https://msdn.microsoft.com/library/azure/mt637907.aspx

[vm_marketplace]: https://azure.microsoft.com/marketplace/virtual-machines/



<!--HONumber=Aug16_HO4-->


