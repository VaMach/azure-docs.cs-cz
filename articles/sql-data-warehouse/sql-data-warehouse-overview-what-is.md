<properties
   pageTitle="Co je Azure SQL Data Warehouse | Microsoft Azure"
   description="Distribuovaná databáze podnikové třídy schopná zpracovávat petabajtové objemy relačních a nerelačních dat. Je to první cloudový datový sklad v odvětví, který se umí během pár sekund zvětšit, zmenšit nebo pozastavit."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="lodipalm"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="06/01/2016"
   ms.author="lodipalm;barbkess;mausher;jrj;sonyama;kevin"/>


# Co je Azure SQL Data Warehouse?

Azure SQL Data Warehouse je cloudová, škálovatelná databáze, která dokáže zpracovávat ohromné objemy dat – relačních i nerelačních. Služba SQL Data Warehouse je postavená na architektuře MPP (Massively Parallel Processing), díky které dokáže zpracovávat úlohy vaší organizace. 

SQL Data Warehouse:

- Kombinuje osvědčenou relační databázi SQL Server s možnostmi cloudového škálování Azure. Během pár sekund můžete zvýšit, snížit, pozastavit nebo obnovit výpočty.  Díky tomu šetříte náklady, protože podle potřeby horizontálně navyšujete kapacitu CPU, zatímco v době mimo špičku snižujete využití.
- Využívá naši platformu Azure. Nasazení je snadné a správa bezproblémová. Automatické zálohování zajišťuje plnou odolnost proti chybám. 
- Doplňuje ekosystém SQL Server.  Můžete vyvíjet pomocí známých nástrojů a jazyka T-SQL systému SQL Server.

Pokračujte ve čtení a získejte další informace o klíčových funkcích služby SQL Data Warehouse.

## Optimalizované

### Architektura MPP (Massively Parallel Processing)

Služba SQL Data Warehouse používá architekturu MPP
(Massively Parallel Processing) společnosti Microsoft, která je navržená pro běh místních datových skladů patřících k největším na světě.

Naše architektura MPP aktuálně rozprostírá data mezi 60 nezávislých úložných a procesorových jednotek typu SN (Shared Nothing). Data se ukládají do objektů Blob služby Azure Storage v rámci služby Storage úrovně Premium a pro účely provádění dotazů se propojují s výpočetními uzly. Tato architektura nám umožňuje uplatnit přístup „rozděl a panuj“ ke spouštění složitých dotazů T-SQL. Řídicí uzel během zpracování parsuje dotaz a jednotlivé výpočetní uzly potom souběžně „panují“ nad svojí částí dat. 

Díky kombinaci architektury MPP a možností úložiště Azure služba SQL Data Warehouse dokáže:

- zvětšovat a zmenšovat úložiště nezávisle na výpočtech,
- zvětšovat a zmenšovat výpočty bez přesouvání dat, 
- pozastavit výpočetní kapacitu a zachovat neporušená data,
- během okamžiku obnovit výpočetní kapacitu.

Architektura je podrobně popsaná níže. 

![Architektura služby SQL Data Warehouse][1]


- **Řídicí uzel:** Řídicí uzel „řídí“ systém. Jde o prvek front-end, který komunikuje se všemi aplikacemi a připojeními. Ve službě SQL Data Warehouse řídicí uzel používá technologii SQL Database a připojení k němu vypadá a funguje stejně. Pod povrchem řídicí uzel koordinuje všechny přesuny dat a výpočty potřebné k spouštění paralelních dotazů na distribuovaných datech. Když odešlete dotaz TSQL do služby SQL Data Warehouse, řídicí uzel ho transformuje na samostatné dotazy, které se spustí paralelně na jednotlivých výpočetních uzlech.

- **Výpočetní uzly:** Výpočetní uzly zajišťují výkon služby SQL Data Warehouse. Jsou to databáze SQL Database, které zpracovávají kroky dotazu a spravují vaše data. Když přidáte data, služba SQL Data Warehouse distribuuje řádky pomocí výpočetních uzlů. Výpočetní uzly jsou současně dělníky, kteří spouští paralelní dotazy na datech. Po zpracování předají výsledky zpět řídicímu uzlu. Řídicí uzel dokončí dotaz tím, že agreguje výsledky a vrátí konečný výsledek.


- **Úložiště:** Data se ukládají do objektů Blob služby Azure Storage. Při práci s daty výpočetní uzly zapisují a čtou přímo z úložiště objektů blob. Vzhledem k tomu, že úložiště Azure se může transparentně a neomezeně rozšiřovat, služba SQL Data Warehouse dokáže totéž. Výpočty a úložiště jsou nezávislé, takže služba SQL Data Warehouse může automaticky škálovat úložiště nezávisle na škálování výpočtů, a naopak.  Služba Azure Storage je taky plně odolná proti chybám a zjednodušuje proces zálohování a obnovení.
   

- **Služba pro přesun dat:** Služba pro přesun dat (DMS) je naše technologie pro přesun dat mezi uzly. Služba DMS dává výpočetním uzlům přístup k datům, který potřebují pro spojování a agregaci. DMS není jednou ze služeb Azure. Jde o službu systému Windows, která běží na všech uzlech souběžně se službou SQL Database. Vzhledem k tomu, že služba DMS běží na pozadí, nebudete s ní pracovat přímo. Při pohledu na plány dotazů nicméně uvidíte, že zahrnují některé operace DMS, protože při paralelním spuštění jednotlivých dotazů je vždycky nutná určitá forma přesunu dat.


### Optimalizovaný výkon dotazů

Kromě strategie „rozděl a panuj“ se přístup MPP opírá o několik optimalizací výkonu specifických pro datové sklady, včetně těchto:

- Optimalizátor distribuovaných dotazů a soubor komplexních statistik napříč všemi daty. Pomocí informací o velikosti a distribuci dat dokáže služba optimalizovat dotazy tím, že posoudí náklady na konkrétní operace distribuovaných dotazů.

- Pokročilé algoritmy a postupy integrované do procesu přesunu dat, které efektivně přesouvají data mezi výpočetními prostředky tak, jak to vyžaduje provedení dotazu. Tyto operace přesunu dat jsou integrované a všechny optimalizace služby pro přesun dat probíhají automaticky.

- Clusterované indexy columnstore ve výchozím nastavení. Díky použití úložiště založeného na sloupcích dosahuje služba SQL Data Warehouse až 5x větší zvýšení komprese oproti tradičním úložištím orientovaným na řádky a až 10x větší zvýšení výkonu dotazů. Analytické dotazy, které potřebují prohledat velký počet řádků, výborně fungují s indexy columnstore. 

## Škálovatelné

Architektura služby SQL Data Warehouse zavádí oddělená úložiště a výpočty, a tím umožňuje jejich nezávislé škálování. Rychlá a jednoduchá struktura nasazení služby SQL Database umožňuje během okamžiku získat další dostupné výpočetní prostředky. To se vzájemně doplňuje s použití objektů Blob služby Azure Storage. Použití objektů Blob nám zajišťuje stabilní, replikované úložiště, a navíc poskytuje infrastrukturu pro snadné rozšíření za nízkou cenu.  Díky této kombinaci cloudového škálovatelného úložiště a výpočtů Azure vám služba SQL Data Warehouse umožňuje platit za úložiště k provádění dotazů pode toho, kdy a jak ho potřebujete. Objem výpočtů jde jednoduše změnit tím, že na Portálu Azure přesunete posuvník doleva nebo doprava, ale můžete ho taky naplánovat pomocí T-SQL a prostředí PowerShell.

Spolu se schopností plně řídit objem výpočtů nezávisle na úložišti vám služba SQL Data Warehouse umožňuje plně pozastavit datový sklad. Zatímco úložiště zůstane zachované, všechny výpočty se uvolní do hlavního fondu Azure, což vám ihned ušetří peníze. V případě potřeby můžete výpočetní prostředky jednoduše obnovit a zpřístupnit data a výpočetní prostředky pro úlohy.

Využití výpočetních prostředků ve službě SQL Data Warehouse se měří pomocí jednotek datového skladu SQL (DWU). Jednotky DWU jsou měřítkem základního výkonu, kterým datový sklad disponuje, a slouží k zajištění toho, aby k vašemu datovému skladu byl vždycky přidružený standardní objem výkonu.  Jednotky DWU se konkrétně používají k zajištění tohoto:

- Můžete efektivně škálovat datový sklad, aniž byste museli řešit použitý hardware a software.

- Rozumíte výkonu a můžete ho sledovat na úrovni DWU předtím, než změníte velikost datového skladu.

- Hardware a software použitý pro vaši instanci se může změnit, aniž by se to dotklo výkonu úloh.

- Můžeme provádět úpravy základní architektury služby, aniž bychom ovlivnili výkon vašich úloh.

- Rychle zvyšujeme výkon služby SQL Data Warehouse způsobem, který je škálovatelný a má rovnoměrný dopad na systém.

### Jednotky datového skladu

Přesně řečeno, jednotky datového skladu chápeme jako měřítko tří přesných metrik, které vysoce korelují s výkonem úloh datového skladu. Pokud jde o obecnou dostupnost, usilujeme o to, aby se tyto metriky výkonu škálovaly lineárně s jednotkami DWU, které jste pro datový sklad zvolili.

**Prohledávání/agregace:** Tato metrika zatížení zohledňuje standardní dotaz datového skladu, který prohledá velký počet řádků a potom provede komplexní agregaci. Tato operace je náročná na oblast vstup/výstup a prostředky procesoru.

**Načtení:** Tato metrika měří schopnost ingestovat data do služby. Načtení se provádí tak, že se technologií PolyBase načte reprezentativní datová sada z objektu blob služby Azure Storage. Tato metrika je navržená tak, aby vystavila zátěži síťové a procesorové aspekty služby.

**Funkce CTAS (CREATE TABLE AS SELECT):** Funkce CTAS měří schopnost vytvořit kopii tabulky. To zahrnuje čtení dat z úložiště, jejich distribuci dat mezi uzly zařízení a zápis zpátky do úložiště. Tato operace je náročná na prostředky procesoru a síťové prostředky.

### Kdy škálovat

Obecně chceme, aby jednotky DWU byly jednoduché. Když potřebujete rychlejší výsledky, zvyšte počet DWU a plaťte za vyšší výkon.  Když potřebujete méně výpočetního výkonu, snižte počet DWU a plaťte jenom za to, co potřebujete. Změnu počtu jednotek DWU je vhodné zvážit například v těchto případech:

- Pokud nepotřebujete spouštět dotazy, například večer nebo o víkendech, můžete pozastavením výpočetních prostředků zrušit všechny spuštěné dotazy a odebrat všechny DWU přidělené datovému skladu.

- Když provádíte náročné operace načítání nebo transformace dat, bude nejspíš vhodné vertikálně navýšit kapacitu, aby byla data rychleji dostupná.

- Pokud chcete spolehlivě zjistit, jaká hodnota DWU je pro vás optimální, zkuste po načtení dat vertikálně navýšit a snížit kapacitu a spustit pár dotazů. Škálování je rychlé, takže můžete zkusit několik různých úrovní výkonu a přitom využít maximálně jednu hodinu.

> [AZURE.NOTE] Upozorňujeme, že vzhledem k architektuře služby SQL Data Warehouse nemusí výkon při nižším objemu dat odpovídat vašim očekáváním.  Pokud si chcete udělat skutečnou představu o přínosech v oblasti výkonu, doporučujeme začít s objemem dat nejméně 1 TB.

## Integrované

Služba SQL Data Warehouse je založená na osvědčeném relačním databázovém stroji SQL Serveru a zahrnuje celou řadu funkcí, které od podnikového datového skladu očekáváte. Pokud už znáte jazyk Transact-SQL, tyto znalosti se dají snadno přenést do služby SQL Data Warehouse. Ať už máte pokročilé, nebo jen základní znalosti, příklady v dokumentaci vám pomůžou začít. Celkově platí, že způsob konstrukce prvků jazyka služby SQL Data Warehouse se dá chápat takto:

- Služba SQL Data Warehouse používá u mnoha operací syntaxi jazyka Transact-SQL (TSQL) systému SQL Server a podporuje širokou škálu tradičních SQL konstruktorů, jako jsou uložené procedury, uživatelem definované funkce, vytváření oddílů tabulky, indexy a kolace.

- SQL Data Warehouse taky obsahuje řadu špičkových funkcí systému SQL Server, k nimž patří clusterované indexy columnstore, integrace PolyBase a auditování dat (včetně hodnocení hrozeb).

- Služba SQL Data Warehouse je pořád ve vývoji, a proto některé prvky jazyka TSQL, které jsou u úloh datových skladů méně běžné nebo jsou v systému SQL Server relativně nové, nemusí být aktuálně dostupné. Víc se o tom dozvíte v dokumentaci týkající se migrace.

Díky shodnému jazyku Transact-SQL a shodným funkcím systému SQL Server, služby SQL Data Warehouse, služby SQL Database a řešení Analytics Platform System můžete vyvinout řešení, které vyhovuje vašim datovým potřebám. Na základě požadavků na výkon, zabezpečení a škálování se můžete rozhodnout, kam svoje data uložíte, a potom data podle potřeby přenášet mezi různými systémy.

Nejenže služba SQL Data Warehouse přebírá možnosti jazyka TSQL systému SQL Server, ale taky se integruje s mnoha nástroji, které uživatelé systému SQL Server dobře znají. Konkrétně jsme se zaměřili na integraci několika kategorií nástrojů se službou SQL Data Warehouse, včetně těchto:

**Tradiční nástroje systému SQL Server:** Ve službě SQL Data Warehouse je dostupná plná integrace se službami SQL Server Analysis Services, Integration Services a Reporting.

**Cloudové nástroje:** Služba SQL Data Warehouse se dá používat souběžně s řadou nových nástrojů v Azure a je těsně integrovaná se službami Azure Data Factory, Stream Analytics, Machine Learning a Power BI.

**Nástroje třetích stran:** Mnoho poskytovatelů nástrojů třetích stran certifikovalo integraci svých nástrojů se službou SQL Data Warehouse. Prohlédněte si úplný seznam.

## Hybridní

Použití služby SQL Data Warehouse společně s technologií PolyBase poskytuje uživatelům bezprecedentní možnost přesouvat data v rámci svého ekosystému a dává jim možnost nastavit pokročilé hybridní scénáře s nerelačními a místními zdroji dat.

PolyBase se snadno používá a umožňuje vám využít data z různých zdrojů pomocí stejných příkazů T-SQL, které dobře znáte. PolyBase umožňuje dotazovat nerelační data uložená v úložišti objektů blob Azure, jako by šlo o běžnou tabulku. Použijte PolyBase k dotazům na nerelačních data a importu nerelačních dat do služby SQL Data Warehouse.

- PolyBase používá k přístupu k nerelačním datům externí tabulky. Definice tabulek jsou uložené ve službě SQL Data Warehouse a můžete k nim přistupovat pomocí SQL a nástrojů stejně, jako byste přistupovali k normálním relačním datům.

- PolyBase nedělá při integraci žádné rozdíly. Zpřístupňuje stejné prvky a funkce všem zdrojům, které podporuje. PolyBase čte data v mnoha různých formátech, včetně souborů s oddělovači a souborů ORC.

- PolyBase se dá použít k přístupu k úložišti objektů blob, který současně slouží jako úložiště clusteru služby HD Insight, a tím vám poskytuje špičkový přístup ke stejným datům pomocí relačních i nerelačních nástrojů.

## Další kroky

Teď, když už službu SQL Data Warehouse trochu znáte, seznamte se s [úlohami datového skladu] a naučte se [zřídit] službu SQL Data Warehouse a [načíst ukázková data].  Můžete se taky podívat na některé z těchto dalších zdrojů služby SQL Data Warehouse.  

- [Blogy] 
- [Žádosti o funkce]
- [Videa]
- [Blogy týmu CAT]
- [Vytvoření lístku podpory]
- [Fórum MSDN]
- [Fórum Stack Overflow]
- [Twitter]


<!--Image references-->
[1]: ./media/sql-data-warehouse-overview-what-is/dwarchitecture.png

<!--Article references-->
[Vytvoření lístku podpory]: ./sql-data-warehouse-get-started-create-support-ticket.md
[úlohami datového skladu]: ./sql-data-warehouse-overview-workload.md
[jak načíst ukázková data]: ./sql-data-warehouse-get-started-manually-load-samples.md
[jak zřídit]: ./sql-data-warehouse-get-started-provision.md

<!--MSDN references-->

<!--Other Web references-->
[Blogy]: https://azure.microsoft.com/blog/tag/azure-sql-data-warehouse/
[Blogy týmu CAT]: https://blogs.msdn.microsoft.com/sqlcat/tag/sql-dw/
[Žádosti o funkce]: https://feedback.azure.com/forums/307516-sql-data-warehouse
[Fórum MSDN]: https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureSQLDataWarehouse
[Fórum Stack Overflow]: http://stackoverflow.com/questions/tagged/azure-sqldw
[Twitter]: https://twitter.com/hashtag/SQLDW
[Videa]: https://azure.microsoft.com/documentation/videos/index/?services=sql-data-warehouse



<!--HONumber=Jun16_HO2-->


