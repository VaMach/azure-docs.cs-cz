<properties
    pageTitle="Přehled funkcí Azure Batch | Microsoft Azure"
    description="Informace o funkcích služby Batch a jejích rozhraní API z hlediska vývoje."
    services="batch"
    documentationCenter=".net"
    authors="yidingzhou"
    manager="timlt"
    editor=""/>

<tags
    ms.service="batch"
    ms.devlang="multiple"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.workload="big-compute"
    ms.date="05/12/2016"
    ms.author="yidingz;marsma"/>

# Přehled funkcí Azure Batch

Tento článek obsahuje základní přehled klíčových funkcí rozhraní API služby Azure Batch. Bez ohledu na to, zda vyvíjíte distribuovaná výpočetní řešení pomocí rozhraní API [Batch REST][batch_rest_api] nebo [Batch .NET][batch_net_api], použijete mnoho entit a funkcí popsaných níže.

> [AZURE.TIP] Vyšší úroveň technického přehledu služby Batch naleznete v tématu [Základy Azure Batch](batch-technical-overview.md).

## <a name="workflow"></a>Pracovní postup služby Batch

Následující pracovní postup nejvyšší úrovně je typický příklad pracovního postupu používaného téměř všemi distribuovanými výpočetními scénáři vyvinutými v rámci služby Batch:

1. *Datové soubory*, které chcete používat v distribuovaném výpočetním scénáři, nahrajte na účet [Azure Storage][azure_storage]. Tyto soubory musí být v účtu Storage, aby na ně mohla služba Batch přistupovat. Úkoly při spuštění stáhnou tyto soubory na [výpočetní uzly](#computenode).

2. Závislé *binární soubory* nahrajte na účet Storage. Tyto binární soubory zahrnují program, který má být spuštěn prostřednictvím úkolů, a všechna jeho závislá sestavení. Na soubory musí také být přístup z účtu Storage, aby mohly být úlohami staženy na výpočetní uzly.

3. Vytvořte [fond](#pool) výpočetních uzlů. Zadáte [velikost výpočetních uzlů][cloud_service_sizes], které se použijí při vytvoření fondu, a když je úkol spuštěn, bude přiřazen uzlu v tomto fondu.

4. Vytvořte [úlohu](#job). Úloha umožňuje spravovat úkoly kolekce.

5. Přidejte [úkoly](#task) do úlohy. Každý úkol využívá program, který jste odeslali na zpracování informací v datových souborech nahraných do účtu Storage.

6. Sledujte průběh úlohy a načtěte výsledky.

> [AZURE.NOTE] Pro používání služby Batch budete potřebovat [účet Batch](batch-account-create-portal.md) a téměř všechna řešení budou používat účet [Azure Storage][azure_storage] pro ukládání a načítání souborů. Batch aktuálně podporuje pouze typ účtu úložiště pro **obecné účely**, jak je popsáno v kroku č. 5 [Vytvoření účtu úložiště](../storage/storage-create-storage-account.md#create-a-storage-account) v dokumentu [Informace o účtech úložiště Azure](../storage/storage-create-storage-account.md).

V následujících částech se dozvíte o jednotlivých zdrojích uvedených v dříve popsaném pracovním postupu, a také o mnoha dalších funkcích služby Batch, které umožní váš distribuovaný výpočetní scénář.

## <a name="resource"></a> Prostředky služby Batch

Při použití služby Batch budete používat mnoho z následujících prostředků. Některé z těchto prostředků, například účty, výpočetní uzly, fondy, úlohy a úkoly jsou používány ve všech řešeních služby Batch. Jiné, jako například plány úloh a balíčky aplikací, jsou užitečné, ale volitelné funkce.

- [Účet](#account)
- [Výpočetní uzel](#computenode)
- [Fond](#pool)
- [Úloha](#job)
- [Úkol](#task)
    - [Spouštěcí úkol](#starttask)
    - [Úkol správce úloh](#jobmanagertask)
    - [Úkoly přípravy a uvolnění úloh](#jobpreprelease)
    - [Úkoly s více instancemi](#multiinstance)
    - [Závislosti úkolů](#taskdep)
- [Plány úlohy](#jobschedule)
- [Balíčky aplikací](#appkg)

### <a name="account"></a>Účet

Účet Batch je jednoznačně identifikovaná entita v rámci služby Batch. Veškeré zpracování je přidruženo k účtu Batch. Když provádíte operace se službou Batch, potřebujete název účtu i klíč účtu. Chcete-li vytvořit dávkový účet, podívejte se na dokument [Vytvoření a správa účtu Azure Batch na portálu Azure](batch-account-create-portal.md).

### <a name="computenode"></a>Výpočetní uzel

Výpočetní uzel je virtuální stroj služby Azure, který je vyhrazen na konkrétní zatížení pro aplikaci. Velikost uzlu určuje počet jader procesoru, kapacita paměti a velikost místního systému souborů, který je přidělen k uzlu. Uzlem může být libovolná z [velikostí uzlu cloudových služeb][cloud_service_sizes], s výjimkou A0.

Uzly mohou spouštět spustitelné soubory a skripty, včetně spustitelných souborů (.exe), souborů příkazů (.cmd), dávkových souborů (.bat) a skriptů prostředí PowerShell. Uzel má také následující atributy:

- Na každém výpočetním uzlu je vytvořena standardní **struktura složek** a přidružených **proměnných prostředí** s podrobnostmi o jejich cestách. Další informace viz [Soubory a adresáře](#files) dále.
- Dostupné **proměnné prostředí** pro referenci podle úkolů.
- Nastavení **brány firewall**, která jsou nakonfigurována pro řízení přístupu.
- Pokud je vyžadován **vzdálený přístup** k výpočetnímu uzlu (například pro ladění), je možné získat soubor RDP, které pak mohou být použity pro přístup k uzlu prostřednictvím *Vzdálené plochy*.

### <a name="pool"></a>Fond

Fond je kolekce uzlů, na kterých běží příslušná aplikace. Fond lze vytvořit ručně nebo automaticky pomocí služby Batch při zadání práce, kterou je zapotřebí vykonat. Je možné vytvořit a spravovat fond, který vyhovuje potřebám aplikace, a fondy mohou být používány pouze účtem Batch, ve kterém byly vytvořeny. Účet Batch může mít více než jeden fond.

Fondy Azure Batch jsou založené na základní výpočetní platformě Azure: fondy Batch zajišťují rozsáhlé přidělení, instalaci aplikací, distribuci dat, monitorování stavu i flexibilní úpravy počtu výpočetních uzlů v rámci fondu (škálování).

Každému uzlu, který je přidán do fondu, je přiřazen jedinečný název a IP adresa. Pokud uzel je odebrán z fondu, veškeré změny provedené v operačním systému nebo souborech budou ztraceny a jeho název a IP adresy jsou uvolněny pro pozdější použití. Jestliže uzel opustí fond, je jeho životnost ukončena.

Fond můžete nakonfigurovat, aby umožňoval komunikaci mezi uzly v něm. Pokud je pro fond vyžadována komunikace uvnitř fondu, povolí služba Batch na každém uzlu ve fondu porty vyšší než 1100. Každý uzel ve fondu je konfigurovaný, aby umožňoval příchozí připojení pouze na tento rozsah portů a pouze z jiných uzlů v rámci fondu. Pokud aplikace nevyžaduje komunikaci mezi uzly, služba Batch můžete fondu přidělit potenciálně velký počet uzlů z mnoha různých clusterů a datových center, aby umožnila zvýšený výkon paralelního zpracování.

Při vytváření fondu můžete zadat následující atributy:

- **Velikost uzlů** ve fondu
    - Je třeba zvolit příslušnou velikost uzlu a brát v úvahu charakteristiky a požadavky aplikace nebo aplikací, které budou běžet na uzlech. Velikost uzlu je obvykle vybrána s předpokladem, že na uzlu poběží současně pouze jeden úkol. Uvážení aspektů, například zda je aplikace vícevláknová a kolik paměti spotřebuje, pomůže stanovit nejvíce vhodnou a cenově efektivní velikost uzlu. Je možné mít přiřazeno více úkolů a souběžně spustit více instancí aplikace. V takovém případě se obvykle vybírá větší uzel – podrobnější informace viz "Zásady plánování úkolů" dále.
    - Všechny uzly v rámci fondu musí mít stejnou velikost. Jestliže mají být spuštěny různé aplikace s odlišnými požadavky na systém anebo úrovně zatížení, měly by být vytvořeny samostatné fondy.
    - Pro fond mohou být konfigurovány všechny [velikosti uzlu cloudových služeb][cloud_service_sizes] s výjimkou A0.

- **Řada ** a **verze operačního systému** spuštěného na uzlech
    - Podobně jako u rolí pracovního procesu v rámci služby Cloud Services lze zadat *řadu operačního systému* a *verzi operačního systému* (další informace o rolích pracovního procesu naleznete v části [Více informací o cloudových službách][about_cloud_services] kapitoly *Možnosti hostování výpočtů poskytované platformou Azure*).
    - Řada operačního systému také určuje, jaké verze rozhraní .NET jsou nainstalovány s operačním systémem.
    - Stejně jako u rolí pracovního procesu se doporučuje, aby byl pro verzi operačního systému specifikovaný `*`, takže uzly budou automaticky upgradovány a nebude vyžadována žádná práce s ošetřováním nově vydaných verzí. Případem primárního použití pro výběr konkrétní verze operačního systému je zajistit, aby byla zachována kompatibilita aplikací umožňující testování zpětné kompatibility, jenž má být provedeno před tím, než se povolí aktualizace příslušné verze. Po ověření je možné aktualizovat verzi operačního systému pro fond a nainstalovat novou bitovou kopii operačního systému – všechny spuštěné úkoly budou přerušeny a znovu zařazeny do fronty.

- **Cílový počet uzlů**, který by měl být k dispozici pro fond

- **Zásady škálování** pro fond
    - Kromě počtu uzlů je pro fond také možné stanovit [vzorec automatického škálování](batch-automatic-scaling.md). Služba Batch spustí vzorec a upraví počet uzlů v rámci fondu v závislosti na různých parametrech fondu, úlohy a úkolu, které lze zadat.

- Zásady **plánování úkolů**
    - Možnost konfigurace [maximálního počtu úkolů na uzel](batch-parallel-node-tasks.md) určuje maximální počet úkolů, které lze spustit souběžně na každém uzlu v rámci fondu.
    - Výchozí konfigurace je, aby na výpočetním uzlu běžel současně jeden úkol, ale existují scénáře, kdy je výhodné mít na uzlu spuštěný ve stejnou dobu více než jeden úkol. Jedním z příkladů je zvýšení využití uzlu, pokud má aplikace čekat na V/V. Současné spuštění více než jedné aplikace zvýší využití procesoru. Dalším příkladem je snížení počtu uzlů ve fondu. Tím se může snížit objem přenosu dat vyžadovaných pro velké referenční datové sady – pokud je velikost uzlu A1 dostatečná pro aplikaci, pak by mohla být místo ní zvolena velikost uzlu A4 a fond nakonfigurován pro 8 paralelních úkolů, každý využívající jádro.
    - Dále je možno zadat "typ výplně", který určuje, zda služba Batch rozšíří úkoly rovnoměrně mezi všechny uzly, nebo zda zaneprázdní každý uzel maximálním počtem úkolů před přiřazením úkolů jinému uzlu ve fondu.

- **Stav komunikace** uzlů ve fondu
    - Fond může být nakonfigurován, aby umožňoval komunikaci mezi uzly ve fondu, což určuje jeho základní síťovou infrastrukturu. Vezměte na vědomí, že tím se zároveň ovlivňuje umístění uzlů v rámci clusterů.
    - Ve většině scénářů pracují úkoly nezávisle a nepotřebují mezi sebou komunikovat, nicméně mohou existovat aplikace, ve kterých úkoly musí komunikovat.

- **Spouštěcí úkol** pro uzly ve fondu
    - Je možné určit *spouštěcí úkol*, který je proveden při každém připojení výpočetního uzlu do fondu, a pokud je uzel restartován. To se často používá k instalaci aplikace, která má být používána úkoly běžícími na uzlu.

### <a name="job"></a>Úloha

Úloha je kolekce úkolů a určuje, jak se provádí výpočet na výpočetních uzlech v rámci fondu.

- Úloha určuje **fond**, ve kterém poběží práce. Fond může být existující fond, dříve vytvořený pro použití mnoha úlohami, nebo vytvořený na vyžádání pro každou úlohu přidruženou k plánu úlohy případně pro všechny úlohy přidružené k plánu úlohy.
- Volitelně lze zadat **prioritu úlohy**. Když je úloha odeslána s vyšší prioritou než aktuálně probíhající úlohy, jsou úlohy s vyšší prioritou vloženy do fronty před úlohy s nižší prioritou. Úlohy s nižší prioritou, které jsou již spuštěny, nebudou zrušeny.
- **Omezení** úlohy zadávají určitá omezení pro úlohy.
    - Pro úlohy je možné nastavit **maximální uplynulý čas**. Pokud úlohy běží po dobu delší než maximální určený uplynulý čas, budou úloha a všechny související úkoly ukončeny.
    - Azure Batch dokáže zjistit úkoly, které se nezdařily a opakovat je. Jako omezení lze zadat **maximální počet opakování úkolů**, včetně toho, zda je úkol opakován vždy nebo nikdy. Opakování úkolu znamená, že úkol je znovu zařazen do fronty, aby byl znovu spuštěn.
- Úkoly lze do úlohy přidat klientskou aplikací nebo je možné určit [úkol správce úloh](#jobmanagertask). Úkol správce úloh používá rozhraní Batch API a obsahuje informace potřebné k vytvoření požadovaných úkolů pro úlohu s úkolem spuštěným na jednom výpočetním uzlu v rámci fondu. Úkol správce úloh je spravován specificky službou Batch – je zařazený do fronty okamžitě po vytvoření úlohy, a restartovaný, pokud selže. Úkol správce úloh je vyžadován pro úlohy vytvořené podle plánu úlohy, jelikož toto je jediný způsob, jak definovat úkoly před vytvořením instance úlohy. Další informace o úkolech správce úloh jsou zobrazeny níže.

### <a name="task"></a>Úkol

Úkol je jednotka výpočtu, který je spojen s úlohou a běží na uzlu. Úkoly jsou přiřazeny k uzlu pro provádění nebo jsou zařazeny do fronty, dokud se uzel neuvolní. Úkol používá následující prostředky:

- Aplikaci určenou v **příkazovém řádku** úkolu.

- **Soubory prostředků** obsahující data, která mají být zpracována. Tyto soubory se automaticky zkopírují do uzlu z úložiště blob v účtu pro **obecné účely** úložiště Azure Storage. Další informace naleznete v dále uvedených tématech *Spouštěcí úkol* a [Soubory a adresáře](#files).

- **Proměnné prostředí**, které jsou požadované aplikací. Další informace naleznete v tématu [Nastavení prostředí pro úkoly](#environment) níže.

- **Omezení**, za kterých by měl nastat výpočet. Příklad: maximální doba, ve které je dovoleno, aby úkol běžel, maximální počet pokusů, po které by měla být úloha opakována, pokud selže a maximální doba, po kterou jsou zachovány soubory v pracovním adresáři.

Vedle úkolů, které definujete, aby prováděly výpočty na uzlu, jsou také službou Batch poskytovány následující zvláštní úkoly:

- [Spouštěcí úkol](#starttask)
- [Úkol správce úloh](#jobmanagertask)
- [Úkoly přípravy a uvolnění úloh](#jobmanagertask)
- [Úkoly s více instancemi](#multiinstance)
- [Závislosti úkolů](#taskdep)

#### <a name="starttask"></a>Spouštěcí úkol

Tím, že přidružíte **spouštěcí úkol** k fondu, můžete nakonfigurovat provozní prostředí jeho uzlů, provádět akce jako instalace softwaru nebo spuštění procesů na pozadí. Spouštěcí úkol běží při každém spuštění uzlu tak dlouho, dokud zůstává ve fondu, včetně okamžiku prvního přidání uzlu do fondu. Hlavní výhodou spouštěcího úkolu je, že obsahuje všechny informace potřebné ke konfiguraci výpočetních uzlů a instalaci aplikací nezbytných pro provádění úloh úkolu. Proto je zvýšení počtu uzlů ve fondu stejně jednoduché jako určení počtu nových cílových uzlů – služba Batch již má všechny informace potřebné ke konfiguraci nových uzlů a jejich přípravu pro přijetí úkolů.

U každého úkolu služby Batch lze zadat seznam **souborů prostředků** v úložišti [Azure Storage][azure_storage] navíc k **příkazovému řádku**, který má být proveden. Azure Batch nejprve zkopíruje soubory z úložiště Azure Storage a poté spustí příkazový řádek. Seznam souborů pro spouštěcí úkol fondu obvykle obsahuje balíček aplikace nebo soubory, může ale také zahrnovat referenční data, která mají být používána ve všech úkolech běžících na výpočetních uzlech. Příkazový řádek spouštěcího úkolu může spustit skript prostředí PowerShell nebo provádět operace `robocopy`, například kopírování souborů aplikace do "sdílené" složky a následně spustit soubor MSI nebo `setup.exe`.

> [AZURE.IMPORTANT] Služba Batch aktuálně podporuje *pouze* typ účtu úložiště **pro obecné účely**, jak je popsáno v kroku č. 5 [Vytvoření účtu úložiště](../storage/storage-create-storage-account.md#create-a-storage-account) v dokumentu [Informace o účtech úložiště Azure](../storage/storage-create-storage-account.md). Úkoly služby Batch (včetně standardních úkolů, spouštěcích úkolů, úkolů přípravy a uvolnění úloh), musí zadat soubory prostředků, které jsou umístěny *pouze* v účtech úložiště **pro obecné účely**.

Je obvykle žádoucí, aby služba Batch čekala na dokončení spouštěcího úkolu před tím, než uváží, že uzel je připraven k přiřazení úkolů, ale tuto možnost lze nakonfigurovat.

Pokud na výpočetním uzlu selže spouštěcí úkol, je stav tohoto uzlu aktualizován, aby odrážel selhání a uzel nebude k dispozici pro úlohy, které mají být přiřazeny. Spouštěcí úkol může selhat, jestliže se vyskytl problém s kopírováním jeho souborů prostředků z úložiště nebo pokud proces spuštěný pomocí svého příkazového řádku vrátí nenulový ukončovací kód.

#### <a name="jobmanagertask"></a>Úkol správce úloh

**Úkol Správce úloh** se obvykle používá při řízení nebo monitorování provádění úlohy. Například vytvoření a odeslání úkolů pro úlohu, určení dalších úkolů, které se mají spustit a určení, kdy je práce dokončena. Úkol Správce úloh však není omezen na tyto aktivity – jedná se o plnohodnotný úkol, který může provádět všechny akce nezbytné pro úlohu. Například úkol Správce úloh může stáhnout soubor zadaný jako parametr, analyzovat obsah tohoto souboru a odeslat dalších úkoly na základě těchto obsahů.

Úkol Správce úloh je spuštěn přede všemi ostatními úkoly a nabízí následující funkce:

- Je službou Batch automaticky odeslán jako úkol při vytvoření úlohy.

- Je naplánován ke spuštění před dalšími úkoly v rámci úlohy.

- Jeho přidružený uzel je poslední, který se odebere z fondu, když je fond zmenšován.

- Jeho ukončení může být vázáno na ukončení všech úkolů v úloze.

- Úkolu správce úloh je přiřazena nejvyšší priorita, když je nutné jej restartovat. Pokud není k dispozici nečinný uzel, může služba Batch ukončit jeden z ostatních spuštěných úkolů ve fondu, aby uvolnila prostor pro spuštění úkolu správce úloh.

- Úkol správce úloh v jedné úloze nemá přednost před úkoly jiných úloh. Mezi úlohami jsou dodržovány pouze priority s úrovní úlohy.

#### <a name="jobpreprelease"></a>Úkoly přípravy a uvolnění úloh

Služba Batch poskytuje úkol přípravy úlohy pro nastavení provádění před úlohou a úkol uvolnění úlohy pro údržbu nebo čištění po úloze.

- **Úkol přípravy úlohy** – úkol přípravy úlohy je spuštěn před spuštěním jiných úkolů úloh na všech výpočetních uzlech, na kterých je naplánované spuštění úkolů. Úkol přípravy úlohy lze například použít ke zkopírování dat, která jsou sdílena všemi úkoly, ale jsou jedinečná pro úlohu.
- **Úkol uvolnění úlohy** – po dokončení úlohy se na každém uzlu ve fondu, na kterém se spustil alespoň jeden úkol, spustí úkol uvolnění úlohy. Pomocí úkolu uvolnění úlohy můžete například odstranit data zkopírovaná pomocí úkolu přípravy úlohy, nebo zkomprimovat a odeslat data diagnostického protokolu.

Úkoly přípravy a uvolnění úlohy umožňují zadat příkazový řádek, který se má spustit, když je úkol vyvolán, a nabízejí funkce jako stažení souboru, provádění se zvýšenými oprávněními, vlastní proměnné prostředí, maximální dobu provádění, počet opakování a dobu uchovávání souboru.

Další informace ohledně úkolů přípravy a uvolnění úloh najdete v části [Spouštění úkolů přípravy a dokončení úlohy na výpočetních uzlech Azure Batch](batch-job-prep-release.md).

#### <a name="multiinstance"></a>Úkoly s více instancemi

[Úkol s více instancemi](batch-mpi.md) je úkol, který je nakonfigurován, aby byl současně spuštěn na více než jednom výpočetním uzlu. Pomocí úkolů s více instancemi můžete povolit scénáře vysokovýkonného výpočetního prostředí jako je rozhraní MPI (Message Passing Interface), které vyžadují skupinu výpočetních uzlů přidělených společně na zpracování jediné úlohy.

Podrobné informace o spouštění úloh MPI ve službě Batch pomocí knihovny Batch .NET naleznete v dokumentu [Použití úkolů s více instancemi ke spouštění aplikací rozhraní MPI Message Passing Interface) v Azure Batch](batch-mpi.md).

#### <a name="taskdep"></a>Závislosti úkolů

Závislosti úkolů, jak již název napovídá, umožňují určit, zda úkol závisí před svým spuštěním na dokončení jiného úkolu. Tato funkce poskytuje podporu pro situace, ve kterých "podřízený" úkol spotřebovává výstup "nadřazeného" úkolu nebo když nadřazený úkol provádí inicializaci, která je požadovaná podřízeným úkolem. Chcete-li tuto funkci používat, je nutné nejprve v úloze služby Batch povolit závislosti úkolů. Pak pro každý úkol, který závisí na jiném (nebo mnoha jiných), zadáte úkoly, na kterých tento úkol závisí.

Pomocí závislosti úkolů lze nakonfigurovat například následující scénáře:

* *ÚkolB* závisí na *úkoluA* (*úkolB* nezahájí provádění, dokud se nedokončí *úkolA*)
* *ÚkolC* závisí na *úkoluA* a *úkoluB*
* *ÚkolD* závisí na celé řadě úkolů, například na úkolu *1* až *10*, než se provede

Podívejte se na ukázkový kód [TaskDependencies][github_sample_taskdeps] v úložišti [azure-batch-samples][github_samples] GitHub. V něm uvidíte postup konfigurace úkolů, které jsou závislé na dalších úkolech, pomocí knihovny [Batch .NET][batch_net_api].

### <a name="jobschedule"></a>Naplánované úlohy

Plány úloh umožňují vytvořit opakované úlohy v rámci služby Batch. Plán úlohy určuje, kdy spustit úlohy a obsahuje specifikace pro úlohy, které mají být spuštěny. Plán úloh umožňuje specifikaci trvání plánu – na jak dlouho a kdy se plán uskuteční – a jak často během tohoto časového období by měly být vytvořeny úlohy.

### <a name="appkg"></a>Balíčky aplikací

Funkce [balíčků aplikací](batch-application-packages.md) poskytuje snadnou správu a nasazení aplikací na výpočetní uzly ve fondech. S balíčky aplikací lze snadno nahrávat a spravovat více verzí aplikací spouštěných prostřednictvím úkolů, včetně binárních a podpůrných souborů, pak automaticky nasadit jednu nebo více těchto aplikací na výpočetní uzly ve fondu.

Služba Batch zpracovává podrobnosti o práci s úložištěm Azure Storage na pozadí, aby bezpečně ukládala a nasazovala balíčky aplikací na výpočetní uzly, čímž lze zjednodušit režijní náklady na kód a správu.

Chcete-li získat další informace o funkci balíčku aplikace, projděte si dokument [Nasazení aplikací pomocí balíčků aplikací Azure Batch](batch-application-packages.md).

## <a name="files"></a>Soubory a adresáře

Každý úkol má pracovní adresář, ve kterém nevytvoří žádný nebo vytvoří soubory a adresáře pro uložení programu spuštěného úlohou, zpracovávaných dat a výstupu zpracování prováděného úkolem. Tyto soubory a adresáře jsou pak k dispozici pro použití dalšími úlohami v době běhu úlohy. Všechny úkoly, soubory a adresáře na uzlu jsou vlastněny jedním uživatelským účtem.

Služba Batch zveřejňuje část systému souborů na uzlu jako "kořenový adresář". Kořenový adresář je k dispozici úkolu prostřednictvím přístupu na proměnnou prostředí `%AZ_BATCH_NODE_ROOT_DIR%`. Další informace o používání proměnných prostředí naleznete v tématu [Nastavení prostředí pro úkoly](#environment).

![Adresářová struktura výpočetního uzlu][1]

Kořenový adresář obsahuje následující adresářovou strukturu:

- **Sdílené** – toto umístění je sdílený adresář pro všechny úkoly, které běží na uzlu, bez ohledu na úlohu. Na uzlu je se do sdíleného adresáře přistupuje prostřednictvím `%AZ_BATCH_NODE_SHARED_DIR%`. Tento adresář poskytuje přístup pro čtení a zápis pro všechny úkoly, které jsou spouštěny na uzlu. Úkoly mohou vytvořit, číst, aktualizovat a odstranit soubory v tomto adresáři.

- **Po spuštění** – toto umístění je využíváno spouštěcím úkolem jako jeho pracovní adresář. Všechny soubory, které jsou staženy službou Batch pro spuštění spouštěcího úkolu, jsou také uloženy pod tímto adresářem. Na uzlu je spouštěcí adresář k dispozici prostřednictvím proměnné prostředí `%AZ_BATCH_NODE_STARTUP_DIR%`. Spouštěcí úkol může vytvořit, číst, aktualizovat a odstranit soubory v tomto adresáři a tento adresář může být používán spouštěcími úkoly ke konfiguraci operačního systému.

- **Úkoly** – pro každý úkol, který běží na uzlu, na nějž se přistupuje prostřednictvím `%AZ_BATCH_TASK_DIR%`. V rámci každého adresáře úkolu vytvoří služba Batch pracovní adresář (`wd`), jehož jedinečná cesta je určena proměnnou prostředí `%AZ_BATCH_TASK_WORKING_DIR%`. Tento adresář poskytuje přístup pro čtení a zápis pro úkol. Úkol může vytvořit, číst, aktualizovat a odstranit soubory v tomto adresáři a tento adresář je zachován na základě omezení *RetentionTime* zadaného pro úlohu.
  - `stdout.txt` a `stderr.txt` – tyto soubory jsou zapsány do složky úkolu během provádění úkolu.

Pokud uzel je odebrán z fondu, jsou odebrány všechny soubory, které jsou uloženy na uzlu.

## <a name="lifetime"></a>Životnost fondu a výpočetního uzlu

Při navrhování řešení Azure Batch, musí být přijato rozhodnutí o návrhu s ohledem na skutečnost kdy a jak jsou fondy vytvářeny a jak dlouho jsou výpočetní uzly v rámci těchto fondů udržovány dostupné.

Na jednom konci spektra by bylo možné vytvořit fond pro každou úlohu, když je úloha odeslána, a jeho uzly odebrány, jakmile úkoly dokončí provádění. Tím se maximalizuje využití, neboť uzly jsou přiděleny pouze tehdy, když je to nezbytně nutné, a vypnuty, jakmile se stanou nečinné. To znamená, že úloha musí čekat, až budou uzly přiděleny. Je důležité si uvědomit, že úkoly budou naplánovány na uzly, jakmile jsou jednotlivě k dispozici, přiděleny a spouštěcí úkol je dokončen. Služba Batch před přiřazením úkolů *nečeká*, až budou všechny uzly v rámci fondu dostupné, čímž zajišťuje maximální využití všech dostupných uzlů.

Na druhém konci spektra, je-li nejvyšší prioritou okamžité spuštění úloh, může být fond vytvořen s předstihem a jeho uzly dány k dispozici před odesláním úloh. V tomto scénáři se mohou úkoly úloh spustit okamžitě, ale uzly mohou být v nečinnosti a čekat na přiřazení úkolů.

Kombinovaný přístup, obvykle používaný pro zpracování proměnného, ale neustálého zatížení, znamená mít fond, do kterého je odesláno více úloh, ale škálováním se podle zatížení úlohy zvyšuje nebo snižuje počet uzlů (viz *Škálování aplikací* dále). Tento přístup je možné uskutečnit reaktivně na základě aktuálního zatížení nebo proaktivně, pokud lze zatížení předpovídat.

## <a name="scaling"></a>Škálování aplikací

Služba Batch může díky [automatickému škálování](batch-automatic-scaling.md) dynamicky upravit počet výpočetních uzlů ve fondu podle aktuálního zatížení a využití prostředků výpočetního scénáře. To umožňuje snížit celkové náklady na spuštění aplikace pomocí pouze těch prostředků, které potřebujete, a uvolněním těch, které nepotřebujete. Pro fond lze určit nastavení automatického škálování, když je vytvořen, nebo povolit škálování později, a u fondu s povoleným automatickým škálováním je možné nastavení škálování aktualizovat.

Automatické škálování se provádí zadáním **vzorce automatického škálování** pro fond. Služba Batch používá tento vzorec k určení cílového počtu uzlů ve fondu pro další interval škálování (interval, který lze zadat).

Příklad: úloha může vyžadovat odeslání velkého počtu úkolů naplánovaných pro spuštění. Fondu je možné na základě aktuálního počtu úkolů čekajících na zpracování a míře dokončení těchto úloh přiřadit vzorec škálování, který přizpůsobí počet uzlů ve fondu. Služba Batch pravidelně vyhodnocuje vzorec a změní velikost fondu na základě vytížení a nastavení vzorce.

Vzorec škálování může být založen na následujících metrikách:

- **Časová metrika** – na základě statistik shromažďovaných každých pět minut za zadaný počet hodin.

- **Metrika prostředků** – na základě využití procesoru, šířky pásma, paměti a počtu uzlů.

- **Metrika úkolů** – na základě stavu úkolů, například Aktivní, Čeká na vyřízení a Dokončený.

Když automatické škálování sníží počet výpočetních uzlů ve fondu, je třeba zvážit aktuálně spuštěné úkoly. Aby se tomuto požadavku vyhovělo, může vzorec zahrnovat nastavení zásad uvolnění uzlu, které určuje, zda se spuštěné úlohy zastaví okamžitě nebo jim bude povoleno dokončení, než bude uzel odebrán z fondu.

> [AZURE.TIP] Chcete-li maximalizovat využití výpočetních prostředků, nastavte cílový počet uzlů na konci úlohy na hodnotu nula, ale povolte dokončení spouštěných úkolů.

Další informace o automatickém škálování aplikace naleznete v tématu [Automatické škálování výpočetních uzlů ve fondu Azure Batch](batch-automatic-scaling.md).

## <a name="cert"></a>Zabezpečení pomocí certifikátů

Při šifrování nebo dešifrování citlivých informací pro úkoly, jako například klíč pro [účet úložiště Azure Storage][azure_storage], je obvykle třeba použít certifikáty . Za tímto účelem je možné nainstalovat certifikáty na uzly. Šifrované tajné klíče jsou předány na úkoly prostřednictvím parametrů příkazového řádku nebo vložené v jednom prostředků úkolu a nainstalované certifikáty lze použít pro jejich dešifrování.

K přidání certifikátu do účtu služby Batch můžete použít operaci [Přidat certifikát][rest_add_cert] (Batch REST API) nebo metodu [CertificateOperations.CreateCertificate][net_create_cert] (rozhraní Batch .NET API). Poté můžete certifikát přidružit k novému nebo existujícímu fondu. Pokud je certifikát přidružený k fondu, služba Batch nainstaluje certifikát na každý uzel ve fondu. Služba Batch nainstaluje příslušné certifikáty při spuštění uzlu před spuštěním úkolů, včetně spouštěcího úkolu a úkolu správce úloh.

## <a name="scheduling"></a>Priorita plánování

Úlohám, které vytvoříte ve službě Batch, lze přiřadit prioritu. Hodnotu priority úlohy používá služba Batch k určení pořadí plánování úlohy v rámci účtu. Hodnoty priority se pohybují v rozsahu -1000 až 1000, kdy -1000 znamená nejnižší prioritu a 1000 nejvyšší. Prioritu úlohy je možné aktualizovat pomocí operace [Aktualizovat vlastnosti úlohy][rest_update_job] (Batch REST API) nebo úpravou vlastnosti [CloudJob.Priority][net_cloudjob_priority]  (Batch .NET API).

V rámci stejného účtu mají úlohy s vyšší prioritou přednost plánování před úlohami s nižší prioritou. Úloha s vyšší hodnotou priority v jednom účtu nemá přednost plánování před jinou úlohou s nižší hodnotou priority v jiném účtu.

Plánování úloh mezi fondy je nezávislé. Mezi různými fondy není zaručeno, že úloha s vyšší prioritou bude naplánována první, pokud její přidružený fond nemá dostatek nečinných uzlů. Ve stejném fondu mají úlohy se stejnou úrovní priority stejnou šanci být naplánované.

## <a name="environment"></a>Nastavení prostředí pro úlohy

Každý úkol, který se spustí v rámci úlohy služby Batch, má přístup k proměnným prostředí nastaveným službou Batch (definované systémem, viz následující tabulka) i proměnnými definovanými uživatelem. Aplikace a skripty spouštěné úkoly na výpočetních uzlech mají přístup do těchto proměnných prostředí během zpracování na uzlu.

Při přidávání úkolů do úlohy nastavte uživatelem definované proměnné prostředí při použití operace [Přidání úkolu do úlohy][rest_add_task] (Batch REST API) nebo úpravou vlastnosti [CloudTask.EnvironmentSettings][net_cloudtask_env] (Batch .NET API).

Systémem i uživatelem definované proměnné prostředí úkolu získáte pomocí operace [Získat informace o úkolu][rest_get_task_info] (Batch REST API) nebo přístupem k vlastnosti [CloudTask.EnvironmentSettings][net_cloudtask_env] (Batch .NET API). Jak je uvedeno, procesy prováděné na výpočetních uzlech mohou také přistupovat na všechny proměnné prostředí, například pomocí známé syntaxe `%VARIABLE_NAME%`.

Pro každý úkol, který je naplánován v rámci úlohy, je službou Batch nastavena následující sada proměnných prostředí definovaných systémem:

| Název proměnné prostředí       | Popis                                                              |
|---------------------------------|--------------------------------------------------------------------------|
| `AZ_BATCH_ACCOUNT_NAME`         | Název účtu, ke kterému patří úkol.                       |
| `AZ_BATCH_JOB_ID`               | Číslo ID úlohy, ke které patří úkol.                             |
| `AZ_BATCH_JOB_PREP_DIR`         | Úplná cesta adresáře úkolu přípravy úlohy na uzlu.         |
| `AZ_BATCH_JOB_PREP_WORKING_DIR` | Úplná cesta pracovního adresáře úkolu přípravy úlohy na uzlu. |
| `AZ_BATCH_NODE_ID`              | Číslo ID uzlu, na kterém je spuštěn úkol.                         |
| `AZ_BATCH_NODE_ROOT_DIR`        | Úplná cesta kořenového adresáře na uzlu.                         |
| `AZ_BATCH_NODE_SHARED_DIR`      | Úplná cesta sdíleného adresáře na uzlu.                       |
| `AZ_BATCH_NODE_STARTUP_DIR`     | Úplná cesta úkolu po spuštění výpočetního uzlu adresáře na uzlu.    |
| `AZ_BATCH_POOL_ID`              | Číslo ID fondu, ve kterém je úkol spuštěný.                         |
| `AZ_BATCH_TASK_DIR`             | Úplná cesta adresáře úkolu na uzlu.                         |
| `AZ_BATCH_TASK_ID`              | Číslo ID aktuálního úkolu.                                              |
| `AZ_BATCH_TASK_WORKING_DIR`     | Úplná cesta pracovního adresáře úkolu na uzlu.                 |

>[AZURE.NOTE] Žádnou z výše uvedených proměnných definovaných systémem nelze přepsat – jsou jen pro čtení.

## <a name="errorhandling"></a>Zpracování chyb

V rámci vašeho řešení Batch bude možná nutné vyřešit selhání úkolů a aplikací.

### Zpracování selhání úkolů
Selhání úkolů spadá do následujících kategorií:

- **Selhání plánování**
    - Pokud z nějakého důvodu selže přenos souborů zadaných pro úkol, je pro úkol nastavena "chyba plánování".
    - Příčiny chyb plánování mohou být z důvodu přesunutí souborů, že účet Storage již není k dispozici nebo došlo k jinému problému, který zabránil úspěšnému zkopírování souborů do uzlu.
- **Selhání aplikace**
    - Selhat může i proces zadaný pomocí příkazového řádku úkolu. Proces se považuje za neúspěšný, pokud proces provedený úkolem vrátí ukončovací kód s nenulovou hodnotou.
    - Pro selhání aplikace je možné nakonfigurovat službu Batch, aby automaticky opakovala úkol až do zadaného počtu opakování.
- **Selhání omezení**
    - Pomocí proměnné *maxWallClockTime* lze nastavit omezení, které určuje maximální dobu trvání provádění úlohy nebo úkolu. To může být užitečné pro ukončení "neodpovídající" úlohy.
    - Při překročení maximální doby je úkol označen jako *dokončený*, ale ukončovací kód je nastaven na hodnotu `0xC000013A` a pole *schedulingError* bude označeno jako `{ category:"ServerError", code="TaskEnded"}`.

### Ladění chyb aplikace

Během provádění může aplikace generovat výstup diagnostiky, které lze použít k řešení potíží. Jak je uvedeno v části [Soubory a adresáře](#files) výše, odešle služba Batch výstup stdout a stderr do souborů `stdout.txt` a `stderr.txt` v adresáři úkolů na výpočetním uzlu. Tyto a další soubory můžete pro účely odstraňování potíží načíst v rozhraní Batch .NET API pomocí funkcí [ComputeNode.GetNodeFile][net_getfile_node] a [CloudTask.GetNodeFile][net_getfile_task].

Rozsáhlejší ladění lze provést přihlášením k výpočetnímu uzlu pomocí *vzdálené plochy*. Je možné [získat soubor protokolu vzdálené plochy z uzlu][rest_rdp] (Batch REST API) nebo použít metodu [ComputeNode.GetRDPFile][net_rdp] (Batch .NET API) pro vzdálené přihlášení.

>[AZURE.NOTE] Pro připojení k uzlu prostřednictvím protokolu RDP musíte na uzlu nejprve vytvořit uživatele. Použijte funkci [Přidat uživatelský účet na uzel][rest_create_user] v Batch REST API nebo metodu [ComputeNode.CreateComputeNodeUser][net_create_user] v Batch .NET.

### Monitorování účtů pro selhání úkolů nebo přerušení

Úkoly mohou občas selhat nebo být přerušeny. Selhat může vlastní aplikace úkolu, uzel, na kterém je spuštěn úkol, může být restartován nebo uzel může být odebrán z fondu během operace změny velikosti, pokud jsou zásady deaktivace přidělení fondu nastaveny na odebrání uzlů okamžitě bez čekání na dokončení úkolů. Ve všech případech lze úkol službou Batch automaticky znovu zařadit do fronty, aby byl spuštěn na jiném uzlu.

Je také možné, že občasný problém způsobí, že se úkol zablokuje nebo že jeho spuštění trvá příliš dlouho. Pro úkol lze nastavit maximální dobu spuštění a pokud dojde k jejímu překročení, služba Batch přeruší aplikaci úkolu.

### Řešení potíží "špatných" výpočetních uzlů

V situacích, kdy některé úkoly selhávají, může klientská aplikace nebo služba Batch prozkoumat metadata neúspěšných úkolů, aby identifikovala uzel, který se chová nesprávně. Každý uzel ve fondu má přiřazeno jedinečné číslo ID a uzel, na kterém je spuštěn úkol, je zahrnut v metadatech úkolu. Jakmile jej rozpoznáte, můžete provést několik akcí:

- **Restartovat uzel** ([REST][rest_reboot] | [.NET][net_reboot])

    Restartování uzlu může někdy odstranit latentní problémy, jako jsou zablokované nebo zhroucené procesy. Vezměte na vědomí, že spouštěcí úkol nebo úkol přípravy úlohy bude po restartování uzlu proveden, pokud jej váš fond používá.

- **Obnovit uzel z image** ([REST][rest_reimage] | [.NET][net_reimage])

    Tato možnost přeinstaluje operační systém na uzlu. Stejně jako u restartování uzlu jsou spouštěcí úkoly a úkoly přípravy úlohy znovu spuštěny poté, co uzel byl obnoven z image.

- **Odebrat uzel z fondu** ([REST][rest_remove] | [.NET][net_remove])

    Někdy je nezbytné úplně odebrat uzel z fondu.

- **Zakázat plánování úkolů v uzlu** ([REST][rest_offline] | [.NET][net_offline])

    To efektivně převede uzel "do režimu offline" tak, aby mu nebyly přiřazeny žádné další úlohy, ale umožňuje, aby zůstal spuštěný a ve fondu. Díky tomu můžete provést další šetření příčin selhání bez ztráty dat neúspěšného úkolu, aniž by uzel způsobil selhání dalších úkolů. Můžete například zakázat plánování úloh na uzlu, pak se přihlásit vzdáleně a prohlédnout si protokoly událostí uzlu nebo provádět jiné řešení potíží. Jakmile dokončíte šetření, můžete převést uzel zpět do režimu online povolením plánování úkolů ([REST][rest_online], [.NET][net_online]) nebo provést jednu z dalších akcí uvedených výše.

> [AZURE.IMPORTANT] Každou výše uvedenou akcí--restartování, obnovení z image, odebrání, zakázání plánování úkolů--můžete určit způsob zpracování úlohy aktuálně spuštěné na uzlu, když akci uskutečníte. Pokud například zakážete plánování úkolů na uzlu pomocí klientské knihovny Batch .NET, můžete zadat hodnotu výčtu [DisableComputeNodeSchedulingOption][net_offline_option] a určit, zda **ukončit** spuštěné úkoly, **znovu je zařadit** pro plánování na jiných uzlech nebo umožnit dokončení spuštěných úkolů před provedením této akce (**TaskCompletion**).

## Další kroky

- Vytvořte svoji první aplikaci služby Batch podle kroků v dokumentu [Začínáme s knihovnou Azure Batch Library for .NET](batch-dotnet-get-started.md)
- Stáhněte si a sestavte ukázkový projekt [Batch Explorer][batch_explorer_project] pro použití při vývoji řešení pro službu Batch. Pomocí projektu Batch Explorer můžete provést následující a další akce:
  - Monitorování a manipulace s fondy, úlohami a úkoly v rámci vašeho účtu Batch
  - Stažení `stdout.txt`, `stderr.txt` a dalších souborů z uzlů
  - Vytvořit uživatele na uzlech a stáhnout soubory protokolu RDP pro vzdálené přihlášení

[1]: ./media/batch-api-basics/node-folder-structure.png

[about_cloud_services]: ../cloud-services/cloud-services-choose-me.md
[azure_storage]: https://azure.microsoft.com/services/storage/
[batch_explorer_project]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/BatchExplorer
[cloud_service_sizes]: ../cloud-services/cloud-services-sizes-specs.md
[msmpi]: https://msdn.microsoft.com/library/bb524831.aspx
[github_samples]: https://github.com/Azure/azure-batch-samples
[github_sample_taskdeps]:  https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/TaskDependencies

[batch_net_api]: https://msdn.microsoft.com/library/azure/mt348682.aspx
[net_cloudjob_jobmanagertask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.jobmanagertask.aspx
[net_cloudjob_priority]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.priority.aspx
[net_cloudpool_starttask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.starttask.aspx
[net_cloudtask_env]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.environmentsettings.aspx
[net_create_cert]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.certificateoperations.createcertificate.aspx
[net_create_user]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.createcomputenodeuser.aspx
[net_getfile_node]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.getnodefile.aspx
[net_getfile_task]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.getnodefile.aspx
[net_multiinstancesettings]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.multiinstancesettings.aspx
[net_rdp]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.getrdpfile.aspx
[net_reboot]: https://msdn.microsoft.com/library/azure/mt631495.aspx
[net_reimage]: https://msdn.microsoft.com/library/azure/mt631496.aspx
[net_remove]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.removefrompoolasync.aspx
[net_offline]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.disableschedulingasync.aspx
[net_online]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.enableschedulingasync.aspx
[net_offline_option]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.common.disablecomputenodeschedulingoption.aspx

[batch_rest_api]: https://msdn.microsoft.com/library/azure/Dn820158.aspx
[rest_add_job]: https://msdn.microsoft.com/library/azure/mt282178.aspx
[rest_add_pool]: https://msdn.microsoft.com/library/azure/dn820174.aspx
[rest_add_cert]: https://msdn.microsoft.com/library/azure/dn820169.aspx
[rest_add_task]: https://msdn.microsoft.com/library/azure/dn820105.aspx
[rest_create_user]: https://msdn.microsoft.com/library/azure/dn820137.aspx
[rest_get_task_info]: https://msdn.microsoft.com/library/azure/dn820133.aspx
[rest_multiinstance]: https://msdn.microsoft.com/library/azure/mt637905.aspx
[rest_multiinstancesettings]: https://msdn.microsoft.com/library/azure/dn820105.aspx#multiInstanceSettings
[rest_update_job]: https://msdn.microsoft.com/library/azure/dn820162.aspx
[rest_rdp]: https://msdn.microsoft.com/library/azure/dn820120.aspx
[rest_reboot]: https://msdn.microsoft.com/library/azure/dn820171.aspx
[rest_reimage]: https://msdn.microsoft.com/library/azure/dn820157.aspx
[rest_remove]: https://msdn.microsoft.com/library/azure/dn820194.aspx
[rest_offline]: https://msdn.microsoft.com/library/azure/mt637904.aspx
[rest_online]: https://msdn.microsoft.com/library/azure/mt637907.aspx



<!--HONumber=Jun16_HO2-->


