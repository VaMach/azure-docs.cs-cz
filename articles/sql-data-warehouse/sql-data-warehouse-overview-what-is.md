<properties
   pageTitle="Co je Azure SQL Data Warehouse? | Microsoft Azure"
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
   ms.date="07/23/2016"
   ms.author="lodipalm;barbkess;mausher;jrj;sonyama;kevin"/>


# Co je Azure SQL Data Warehouse?

Azure SQL Data Warehouse je cloudová, škálovatelná databáze, která dokáže zpracovávat ohromné objemy dat, relačních i nerelačních. Služba SQL Data Warehouse je postavená na architektuře MPP (Massively Parallel Processing), díky které dokáže zpracovávat úlohy vaší organizace.

SQL Data Warehouse:

- Kombinuje relační databázi SQL Server s možnostmi cloudového škálování Azure. Během pár sekund můžete zvýšit, snížit, pozastavit nebo obnovit výpočty. Šetříte náklady, protože podle potřeby horizontálně navyšujete kapacitu CPU, zatímco v době mimo špičku snižujete využití.
- Využívá platformu Azure. Nasazení je snadné, údržba bezproblémová a díky automatickým zálohám je nástroj plně odolný proti selhání.
- Doplňuje ekosystém SQL Server. Můžete vyvíjet pomocí známých nástrojů a jazyka SQL Server Transact-SQL (T-SQL).

Tento článek popisuje klíčové funkce SQL Data Warehouse.

## Architektura MPP (Massively parallel processing)

Řešení SQL Data Warehouse je distribuovaný databázový systém, postavený na architektuře MPP (Massively Parallel Processing). SQL Data Warehouse nabízí jedinečnou škálovatelnost, daleko za možnostmi jediného systému, a to díky dělení dat a schopnosti zpracování napříč více uzly.  SQL Data Warehouse rovnoměrně rozprostírá vaše data napříč mnoha úložnými a procesorovými jednotkami typu SN (shared-nothing). Data se ukládají do místně redundantního úložiště úrovně Premium a jsou propojena s výpočetními uzly pro provedení dotazu. Díky této architektuře používá SQL Data Warehouse ke spuštěnému načítání a složitým dotazům přístup „rozděl a panuj“. Požadavky jsou přijímány Řídicím uzlem, optimalizovány a poté předány na Výpočetní uzly, aby mohly pracovat paralelně.

Díky kombinaci architektury MPP a možností úložiště Azure služba SQL Data Warehouse dokáže:

- zvětšovat a zmenšovat úložiště nezávisle na výpočtech,
- zvětšovat a zmenšovat výpočty bez přesouvání dat,
- pozastavit výpočetní kapacitu a zachovat neporušená data,
- během okamžiku obnovit výpočetní kapacitu.

Následující diagram ukazuje architekturu podrobněji.

![Architektura služby SQL Data Warehouse][1]


**Řídicí uzel:** Řídicí uzel spravuje a optimalizuje dotazy. Jde o prvek front-end, který komunikuje se všemi aplikacemi a připojeními. Ve službě SQL Data Warehouse řídicí uzel používá technologii SQL Database a připojení k němu vypadá a funguje stejně. Pod povrchem řídicí uzel koordinuje všechny přesuny dat a výpočty potřebné k spouštění paralelních dotazů na distribuovaných datech. Když odešlete dotaz T-SQL do služby SQL Data Warehouse, řídicí uzel ho transformuje na samostatné dotazy, které se spouští paralelně na jednotlivých výpočetních uzlech.

**Výpočetní uzly:** Výpočetní uzly zajišťují výkon služby SQL Data Warehouse. Jsou to databáze SQL, které ukládají vaše data a zpracovávají vaše dotazy. Když přidáte data, služba SQL Data Warehouse distribuuje řádky k vašim výpočetním uzlům. Výpočetní uzly jsou dělníky, kteří spouští paralelní dotazy na datech. Po zpracování předají výsledky zpět řídicímu uzlu. Řídicí uzel dokončí dotaz tím, že agreguje výsledky a vrátí konečný výsledek.

**Úložiště:** Data se ukládají do Úložiště objektů blob v Azure. Při práci s daty výpočetní uzly zapisují a čtou přímo z úložiště objektů blob. Vzhledem k tomu, že úložiště Azure se může transparentně a neomezeně rozšiřovat, služba SQL Data Warehouse dokáže totéž. Výpočty a úložiště jsou nezávislé, takže služba SQL Data Warehouse může automaticky škálovat úložiště nezávisle na škálování výpočtů, a naopak. Služba Úložiště objektů blob v Azure je taky plně odolná proti chybám a zjednodušuje proces zálohování a obnovení.

**Služba pro přesun dat:** Služba pro přesun dat (DMS) zajišťuje přesun dat mezi uzly. Služba DMS dává výpočetním uzlům přístup k datům, který potřebují pro spojování a agregaci. DMS není jednou ze služeb Azure. Jde o službu systému Windows, která běží na všech uzlech souběžně se službou SQL Database. Vzhledem k tomu, že služba DMS běží na pozadí, nebudete s ní pracovat přímo. Při pohledu na plány dotazů nicméně uvidíte, že zahrnují některé operace DMS, protože při paralelním spuštění jednotlivých dotazů je vždycky nutný přesun dat.


## Optimalizováno pro úlohy datového skladu

Přístup MPP se opírá o několik optimalizací výkonu specifických pro datové sklady, včetně těchto:

- Optimalizátor distribuovaných dotazů a soubor komplexních statistik napříč všemi daty. Pomocí informací o velikosti a distribuci dat dokáže služba optimalizovat dotazy tím, že posoudí náklady na konkrétní operace distribuovaných dotazů.

- Pokročilé algoritmy a postupy integrované do procesu přesunu dat, které efektivně přesouvají data mezi výpočetními prostředky tak, jak to vyžaduje provedení dotazu. Tyto operace přesunu dat jsou integrované a všechny optimalizace služby pro přesun dat probíhají automaticky.

- Clusterované indexy **columnstore** ve výchozím nastavení. Díky použití úložiště založeného na sloupcích dosahuje služba SQL Data Warehouse průměrně 5x větší zvýšení komprese oproti tradičním úložištím orientovaným na řádky a 10x nebo i vícekrát vyšší výkon dotazů. Analytické dotazy, které potřebují prohledat velký počet řádků, výborně fungují s indexy columnstore.


## Předvídatelný a škálovatelný výkon

SQL Data Warehouse odděluje úložiště a výpočty, což umožňuje oddělené a samostatné škálování. SQL Data Warehouse nabízí rychlé a jednoduché škálování, díky kterému lze během okamžiku přidat další výpočetní prostředky. To se vzájemně doplňuje s použitím Úložiště objektů blob v Azure. Objekty Blob zajišťují nejen stabilní replikované úložiště, ale také infrastrukturu pro snadné rozšíření za nízkou cenu. Díky této kombinaci cloudového škálovatelného úložiště a výpočtů Azure vám služba SQL Data Warehouse umožňuje platit za výkon dotazu a úložiště podle toho, kdy a jak ho potřebujete. Objem výpočtů jde jednoduše změnit tím, že na Portálu Azure přesunete posuvník doleva nebo doprava, nebo ho můžete taky naplánovat pomocí T-SQL a prostředí PowerShell.

Spolu se schopností plně řídit objem výpočtů nezávisle na úložišti vám služba SQL Data Warehouse umožňuje plně pozastavit datový sklad, což znamená, že neplatíte za výpočetní výkon, když ho nepotřebujete. Zatímco úložiště zůstane zachované, všechny výpočty se uvolní do hlavního fondu Azure, což vám ušetří peníze. V případě potřeby můžete výpočetní prostředky jednoduše obnovit a zpřístupnit data a výpočetní prostředky pro úlohy.

## Jednotky datového skladu

Přidělování prostředků pro SQL Data Warehouse se měří v jednotkách datového skladu (Dwu). Dwu jsou jednotky základních prostředků jako jsou například procesor, paměť nebo IOPS, které jsou přidělené k vaší službě SQL Data Warehouse. Zvýšení výkonu jednotek DWU navyšuje prostředky a výkon. Jednotky DWU přináší především tyto výhody:

- Můžete jednoduše škálovat datový sklad, aniž byste museli řešit použitý hardware a software.

- Můžete předpovídat výkon pro úroveň DWU ještě před tím, než změníte velikost datového skladu.

- Hardware a software použitý pro vaši instanci se může změnit, aniž by se to dotklo výkonu úloh.

- Společnost Microsoft může provádět úpravy základní architektury služby, aniž by došlo k ovlivnění výkonu vašich úloh.

- Společnost Microsoft může rychle zvýšit výkon služby SQL Data Warehouse způsobem, který je škálovatelný a má rovnoměrný dopad na systém.

Jednotky datového skladu poskytují měřítko tří přesných metrik, které vysoce korelují s výkonem úloh datového skladu. Cílem je, aby se následující klíčové metriky výkonu škálovaly lineárně s jednotkami DWU, které jste pro datový sklad zvolili.

**Prohledávání/agregace:** Tato metrika zatížení zohledňuje standardní dotaz datového skladu, který prohledá velký počet řádků a potom provede komplexní agregaci. Tato operace je náročná na oblast vstup/výstup a prostředky procesoru.

**Načtení:** Tato metrika měří schopnost ingestovat data do služby. Načtení se provádí tak, že se technologií PolyBase načte reprezentativní datová sada z Úložiště objektů blob v Azure. Tato metrika je navržená tak, aby vystavila zátěži síťové a procesorové aspekty služby.

**Funkce CTAS (Create Table As Select):** Funkce CTAS měří schopnost kopírování tabulky. To zahrnuje čtení dat z úložiště, jejich distribuci napříč uzly zařízení a zpětný zápis do úložiště. Tato operace je náročná na prostředky procesoru, vstup/výstup a síťové prostředky.

## Pozastavení a škálování na vyžádání

Když potřebujete rychlejší výsledky, zvyšte počet DWU a plaťte za vyšší výkon. Když potřebujete méně výpočetního výkonu, snižte počet DWU a plaťte jenom za to, co potřebujete. V následujících scénářích můžete uvažovat o změně jednotek DWU:

- Pokud nepotřebujete spouštět dotazy, pravděpodobně večer nebo o víkendu, uveďte vaše dotazy do nečinnosti. Potom pozastavte vaše výpočetní prostředky, čímž se vyhnete placení za jednotky DWU, když je nepotřebujete.

- Pokud má váš systém požadavků málo, zvažte snížení DWU na malou velikost. Přístup k datům máte i nadále, avšak s významnou úsporou nákladů.

- Když provádíte náročné operace načítání nebo transformace dat, bude nejspíš vhodné vertikálně navýšit kapacitu, aby byla data rychleji dostupná.

Když chcete spolehlivě zjistit, jaká hodnota DWU je pro vás optimální, zkuste po načtení dat vertikálně navýšit a snížit kapacitu a spustit pár dotazů. Škálování je rychlé, takže můžete zkusit několik různých úrovní výkonu za hodinu nebo méně.  Mějte na paměti, že služba SQL Data Warehouse je určena ke zpracování velkých objemů dat a pokud chcete zjistit její skutečné schopnosti škálování, zvláště v případě velkých měřítek, která nabízíme, měli byste použít velké datové sady, které se blíží nebo přesahují velikost 1 TB.


## Vytvořené na serveru SQL Server

Služba SQL Data Warehouse je založená na relačním databázovém stroji SQL Serveru a zahrnuje celou řadu funkcí, které od podnikového datového skladu očekáváte. Pokud znáte T-SQL, přechod na SQL Data Warehouse je jednoduchou záležitostí. Ať už máte pokročilé, nebo jen základní znalosti, příklady v dokumentaci vám pomůžou začít. Celkově platí, že způsob konstrukce prvků jazyka služby SQL Data Warehouse se dá chápat takto:

- SQL Data Warehouse používá syntaxi T-SQL pro mnoho operací. Také podporuje širokou škálu tradičních SQL konstruktorů, jako jsou uložené procedury, uživatelem definované funkce, vytváření oddílů tabulky, indexy a kolace.

- SQL Data Warehouse taky obsahuje řadu novějších funkcí systému SQL Server, k nimž patří clusterované indexy **columnstore**, integrace PolyBase a auditování dat (včetně hodnocení hrozeb).

- Některé elementy jazyka T-SQL, které jsou méně běžné pro úlohy datových skladů, nebo jsou pro SQL Server novější, nemusí být aktuálně k dispozici. Další informace najdete v tématu [Dokumentace k migraci][].

Díky shodnému jazyku Transact-SQL a shodným funkcím systému SQL Server, služby SQL Data Warehouse, služby SQL Database a řešení Analytics Platform System můžete vyvinout řešení, které vyhovuje vašim datovým potřebám. Na základě požadavků na výkon, zabezpečení a škálování se můžete rozhodnout, kam svoje data uložíte, a potom data podle potřeby přenášet mezi různými systémy.

## Ochrana dat

SQL Data Warehouse ukládá všechna data v úložišti Azure úrovně Premium pomocí místně redundantního úložiště. V místním datovém centru se udržuje několik synchronních kopií dat, aby se zajistila transparentní ochrana dat v případě lokálních selhání. Kromě toho SQL Data Warehouse automaticky zálohuje aktivní (ve stavu bez pozastavení) databáze v pravidelných intervalech pomocí Azure Storage Snapshots. Další informace o zálohování a obnovení najdete v článku [Přehled zálohování a obnovení][].

## Integrováno s nástroji společnosti Microsoft

SDL Data Warehouse také integruje mnoho nástrojů, které znají uživatelé z používání systému SQL Server. Mezi ně patří:

**Tradiční nástroje systému SQL Server:** Služba SQL Data Warehouse je plně integrovaná se Službou Analysis Services serveru SQL, SSIS a Reporting Services.

**Cloudové nástroje:** Služba SQL Data Warehouse se dá používat souběžně s řadou nových nástrojů v Azure včetně služeb Data Factory, Stream Analytics, Machine Learning a Power BI. Úplnější seznam viz [Přehled integrovaných nástrojů][].

**Nástroje třetích stran:** Velké množství poskytovatelů nástrojů třetích stran disponuje certifikovanou integrací svých nástrojů se službou SQL Data Warehouse. Úplný seznam najdete v tématu [Partneři řešení SQL Data Warehouse][].

## Scénáře hybridních zdrojů dat

Použití služby SQL Data Warehouse společně s technologií PolyBase poskytuje uživatelům bezprecedentní možnost přesouvat data v rámci svého ekosystému a dává jim možnost nastavit pokročilé hybridní scénáře s nerelačními a místními zdroji dat.

PolyBase vám umožňuje využít data z různých zdrojů pomocí stejných příkazů T-SQL, které dobře znáte. PolyBase umožňuje dotazovat nerelační data uložená v Úložišti objektů blob v Azure, jako by šlo o běžnou tabulku. Použijte PolyBase k dotazům na nerelační data a importu nerelačních dat do služby SQL Data Warehouse.

- PolyBase používá k přístupu k nerelačním datům externí tabulky. Definice tabulek jsou uložené ve službě SQL Data Warehouse a můžete k nim přistupovat pomocí SQL a nástrojů stejně, jako byste přistupovali k normálním relačním datům.

- PolyBase nedělá při integraci žádné rozdíly. Zpřístupňuje stejné prvky a funkce všem zdrojům, které podporuje. PolyBase čte data v mnoha různých formátech, včetně souborů s oddělovači a souborů ORC.

- PolyBase lze použít pro přístup k úložišti objektů blob, které se také používá jako úložiště clusteru služby HD Insight. To umožňuje přístup ke stejným datům pomocí relačních a nerelačních nástrojů.

## Další kroky

Teď, když jste se s SQL Data Warehouse seznámili, můžete zjistit, jak rychle [vytvořit SQL Data Warehouse][] a [načíst ukázková data][]. Pokud s Azure začínáte, můžete využít [Glosář Azure][], kde najdete potřebnou terminologii. Můžete se taky podívat na některé z těchto dalších zdrojů služby SQL Data Warehouse.  

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
[Vytvoření lístku podpory]: sql-data-warehouse-get-started-create-support-ticket.md
[načíst ukázková data]: sql-data-warehouse-load-sample-databases.md
[vytvořit SQL Data Warehouse]: sql-data-warehouse-get-started-provision.md
[Dokumentace k migraci]: sql-data-warehouse-overview-migrate.md
[Partneři řešení SQL Data Warehouse]: sql-data-warehouse-partner-business-intelligence.md
[Přehled integrovaných nástrojů]: sql-data-warehouse-overview-integrate.md
[Přehled zálohování a obnovení]: sql-data-warehouse-restore-database-overview.md
[Glosář Azure]: ../azure-glossary-cloud-terminology.md

<!--MSDN references-->

<!--Other Web references-->
[Blogy]: https://azure.microsoft.com/blog/tag/azure-sql-data-warehouse/
[Blogy týmu CAT]: https://blogs.msdn.microsoft.com/sqlcat/tag/sql-dw/
[Žádosti o funkce]: https://feedback.azure.com/forums/307516-sql-data-warehouse
[Fórum MSDN]: https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=AzureSQLDataWarehouse
[Fórum Stack Overflow]: http://stackoverflow.com/questions/tagged/azure-sqldw
[Twitter]: https://twitter.com/hashtag/SQLDW
[Videa]: https://azure.microsoft.com/documentation/videos/index/?services=sql-data-warehouse



<!---HONumber=Aug16_HO4-->


