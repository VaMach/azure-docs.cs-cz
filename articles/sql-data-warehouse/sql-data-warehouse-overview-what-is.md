---
title: Co je Azure SQL Data Warehouse? | Dokumentace Microsoftu
description: "Distribuovaná databáze podnikové třídy schopná zpracovávat petabajtové objemy relačních a nerelačních dat. Je to první cloudový datový sklad v odvětví, který se umí během pár sekund zvětšit, zmenšit nebo pozastavit."
services: sql-data-warehouse
documentationcenter: NA
author: jrowlandjones
manager: bjhubbard
editor: 
ms.assetid: 4006c201-ec71-4982-b8ba-24bba879d7bb
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: hero-article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: overview
ms.date: 2/28/2017
ms.author: jrj;barbkess
ms.translationtype: Human Translation
ms.sourcegitcommit: eeb56316b337c90cc83455be11917674eba898a3
ms.openlocfilehash: fd35b0e5c57e47c5e79fe926f24fb21089cbf89a
ms.contentlocale: cs-cz
ms.lasthandoff: 04/18/2017


---
# <a name="what-is-azure-sql-data-warehouse"></a>Co je Azure SQL Data Warehouse?
Azure SQL Data Warehouse je cloudová, škálovatelná, relační databáze, která dokáže zpracovávat ohromné objemy dat, postavená na architektuře MPP (Massively Parallel Processing). 

SQL Data Warehouse:

* Kombinuje relační databázi SQL Server s možnostmi cloudového škálování Azure. 
* Odděluje úložiště od výpočetních prostředků.
* Umožňuje navýšení, snížení, pozastavení nebo obnovení výpočetních prostředků. 
* Integruje se v rámci celé platformy Azure.
* Využívá jazyk Transact-SQL (T-SQL) a nástroje SQL Serveru.
* Je v souladu s různými zákonnými a podnikovými požadavky na zabezpečení, jako je například SOC a ISO.

Tento článek popisuje klíčové funkce SQL Data Warehouse.

## <a name="massively-parallel-processing-architecture"></a>Architektura MPP (Massively parallel processing)
Řešení SQL Data Warehouse je distribuovaný databázový systém, postavený na architektuře MPP (Massively Parallel Processing). SQL Data Warehouse rovnoměrně rozprostírá vaše data napříč mnoha úložnými a procesorovými jednotkami typu SN (shared-nothing). Data se ukládají ve vrstvě místně redundantního úložiště úrovně Premium, nad kterou provádí dotazy dynamicky propojené výpočetní uzly. SQL Data Warehouse používá ke spuštěnému načítání a složitým dotazům přístup „rozděl a panuj“. Požadavky jsou přijímány řídicím uzlem, optimalizovány pro distribuci a potom předány do výpočetních uzlů, aby mohly pracovat paralelně.

Díky oddělenému úložišti a výpočetním prostředkům může SQL Data Warehouse:

* Zvětšovat a zmenšovat úložiště nezávisle na výpočetních prostředcích
* Zvětšovat a zmenšovat výpočetní výkon bez přesouvání dat
* Pozastavit výpočetní kapacitu a zachovat neporušená data, zatímco platíte pouze za úložiště
* Obnovit výpočetní kapacitu za provozu

Následující diagram ukazuje architekturu podrobněji.

![Architektura služby SQL Data Warehouse][1]

**Řídicí uzel:** Řídicí uzel spravuje a optimalizuje dotazy. Jde o prvek front-end, který komunikuje se všemi aplikacemi a připojeními. Ve službě SQL Data Warehouse řídicí uzel používá technologii SQL Database a připojení k němu vypadá a funguje stejně. Pod povrchem řídicí uzel koordinuje všechny přesuny dat a výpočty potřebné ke spouštění paralelních dotazů na distribuovaných datech. Když odešlete dotaz T-SQL do služby SQL Data Warehouse, řídicí uzel ho transformuje na samostatné dotazy, které se spouští paralelně na jednotlivých výpočetních uzlech.

**Výpočetní uzly:** Výpočetní uzly zajišťují výkon služby SQL Data Warehouse. Jsou to databáze SQL, které ukládají vaše data a zpracovávají vaše dotazy. Když přidáte data, služba SQL Data Warehouse distribuuje řádky k vašim výpočetním uzlům. Výpočetní uzly jsou dělníky, kteří spouští paralelní dotazy na datech. Po zpracování předají výsledky zpět řídicímu uzlu. Řídicí uzel dokončí dotaz tím, že agreguje výsledky a vrátí konečný výsledek.

**Úložiště:** Data se ukládají do Úložiště objektů blob v Azure. Při práci s daty výpočetní uzly zapisují a čtou přímo z úložiště objektů blob. Vzhledem k tomu, že úložiště Azure se může transparentně a neomezeně rozšiřovat, služba SQL Data Warehouse dokáže totéž. Výpočty a úložiště jsou nezávislé, takže služba SQL Data Warehouse může automaticky škálovat úložiště nezávisle na škálování výpočtů, a naopak. Služba Úložiště objektů blob v Azure je taky plně odolná proti chybám a zjednodušuje proces zálohování a obnovení.

**Služba pro přesun dat:** Služba pro přesun dat (DMS) zajišťuje přesun dat mezi uzly. Služba DMS dává výpočetním uzlům přístup k datům, který potřebují pro spojování a agregaci. DMS není jednou ze služeb Azure. Jde o službu systému Windows, která běží na všech uzlech souběžně se službou SQL Database. DMS je proces na pozadí, se kterým nelze přímo interagovat. Můžete se však podívat na plány dotazů, abyste zjistili, kdy probíhají operace DMS, protože při paralelním spuštění jednotlivých dotazů je vždycky nutný přesun dat.

## <a name="optimized-for-data-warehouse-workloads"></a>Optimalizováno pro úlohy datového skladu
Přístup MPP se opírá o několik optimalizací výkonu specifických pro datové sklady, včetně těchto:

* Optimalizátor distribuovaných dotazů a soubor komplexních statistik napříč všemi daty. Pomocí informací o velikosti a distribuci dat dokáže služba optimalizovat dotazy tím, že posoudí náklady na konkrétní operace distribuovaných dotazů.
* Pokročilé algoritmy a postupy integrované do procesu přesunu dat, které efektivně přesouvají data mezi výpočetními prostředky tak, jak to vyžaduje provedení dotazu. Tyto operace přesunu dat jsou integrované a všechny optimalizace služby pro přesun dat probíhají automaticky.
* Clusterované indexy **columnstore** ve výchozím nastavení. Díky použití úložiště založeného na sloupcích dosahuje služba SQL Data Warehouse průměrně 5x větší zvýšení komprese oproti tradičním úložištím orientovaným na řádky a 10x nebo i vícekrát vyšší výkon dotazů. Analytické dotazy, které potřebují prohledat velký počet řádků, fungují lépe s indexy columnstore.


## <a name="predictable-and-scalable-performance-with-data-warehouse-units"></a>Předvídatelný a škálovatelný výkon s jednotkami datového skladu
Služba SQL Data Warehouse je postavena na podobných technologiích jako SQL Database – to znamená, že uživatelé můžou očekávat konzistentní a předvídatelný výkon pro analytické dotazy. Uživatelé by měli očekávat lineární škálování výkonu v závislosti na přidávání nebo odebírání výpočetních uzlů. Přidělování prostředků pro SQL Data Warehouse se měří v jednotkách datového skladu (Dwu). Dwu jsou jednotky základních prostředků jako jsou například procesor, paměť nebo IOPS, které jsou přidělené k vaší službě SQL Data Warehouse. Zvýšení výkonu jednotek DWU navyšuje prostředky a výkon. Jednotky DWU přináší především tyto výhody:

* Můžete škálovat datový sklad, aniž byste museli řešit použitý hardware a software.
* Můžete předpovídat vylepšení výkonu pro úroveň DWU ještě před tím, než změníte výpočetní výkon datového skladu.
* Hardware a software použitý pro vaši instanci se může změnit, aniž by se to dotklo výkonu úloh.
* Microsoft může vylepšovat základní architekturu služby, aniž by došlo k ovlivnění výkonu vašich úloh.
* Společnost Microsoft může rychle zvýšit výkon služby SQL Data Warehouse způsobem, který je škálovatelný a má rovnoměrný dopad na systém.

Jednotky datového skladu poskytují měřítko tří metrik, které vysoce korelují s výkonem úloh datového skladu. Následující klíčové metriky výkonu se škálují lineárně s počtem DWU.

**Prohledávání/agregace:** Standardní dotaz datového skladu, který prohledá velký počet řádků a potom provede komplexní agregaci. Tato operace je náročná na oblast vstup/výstup a prostředky procesoru.

**Načtení:** Schopnost ingestovat data do služby. Načtení se nejlépe provádí pomocí funkce PolyBase z Azure Storage Blob do služby Azure Data Lake. Tato metrika je navržená tak, aby vystavila zátěži síťové a procesorové aspekty služby.

**Funkce CTAS (Create Table As Select):** Funkce CTAS měří schopnost kopírování tabulky. To zahrnuje čtení dat z úložiště, jejich distribuci napříč uzly zařízení a zpětný zápis do úložiště. Tato operace je náročná na prostředky procesoru, vstup/výstup a síťové prostředky.

## <a name="built-on-sql-server"></a>Vytvořené na serveru SQL Server
Služba SQL Data Warehouse je založená na relačním databázovém stroji SQL Serveru a zahrnuje celou řadu funkcí, které od podnikového datového skladu očekáváte. Pokud znáte T-SQL, přechod na SQL Data Warehouse je jednoduchou záležitostí. Ať už máte pokročilé, nebo jen základní znalosti, příklady v dokumentaci vám pomůžou začít. Celkově platí, že způsob konstrukce prvků jazyka služby SQL Data Warehouse se dá chápat takto:

* SQL Data Warehouse používá syntaxi T-SQL pro mnoho operací. Také podporuje širokou škálu tradičních SQL konstruktorů, jako jsou uložené procedury, uživatelem definované funkce, vytváření oddílů tabulky, indexy a kolace.
* SQL Data Warehouse také obsahuje různé novější funkce SQL Serveru, k nimž patří clusterované indexy **columnstore**, integrace PolyBase a auditování dat (včetně hodnocení hrozeb).
* Některé elementy jazyka T-SQL, které jsou méně běžné pro úlohy datových skladů, nebo jsou pro SQL Server novější, nemusí být aktuálně k dispozici. Další informace najdete v tématu [Dokumentace k migraci][Migration documentation].

Díky shodnému jazyku Transact-SQL a shodným funkcím systému SQL Server, služby SQL Data Warehouse, služby SQL Database a řešení Analytics Platform System můžete vyvinout řešení, které vyhovuje vašim datovým potřebám. Na základě požadavků na výkon, zabezpečení a škálování se můžete rozhodnout, kam svoje data uložíte, a potom data podle potřeby přenášet mezi různými systémy.

## <a name="data-protection"></a>Ochrana dat
SQL Data Warehouse ukládá všechna data v úložišti Azure úrovně Premium pomocí místně redundantního úložiště. V místním datovém centru se udržuje několik synchronních kopií dat, aby se zajistila transparentní ochrana dat před místním selháním. Kromě toho SQL Data Warehouse v pravidelných intervalech automaticky zálohuje aktivní (nepozastavené) databáze pomocí snímků služby Azure Storage. Další informace o zálohování a obnovení najdete v článku [Přehled zálohování a obnovení][Backup and restore overview].

## <a name="integrated-with-microsoft-tools"></a>Integrováno s nástroji společnosti Microsoft
SDL Data Warehouse také integruje mnoho nástrojů, které znají uživatelé z používání systému SQL Server. Mezi tyto nástroje patří:

**Tradiční nástroje systému SQL Server:** Služba SQL Data Warehouse je plně integrovaná se Službou Analysis Services serveru SQL, SSIS a Reporting Services.

**Cloudové nástroje:** Službu SQL Data Warehouse je možné integrovat s různými službami v Azure, včetně služeb Data Factory, Stream Analytics, Machine Learning a Power BI. Úplnější seznam viz [Přehled integrovaných nástrojů][Integrated tools overview].

**Nástroje třetích stran:** Mnoho poskytovatelů nástrojů třetích stran disponuje certifikovanou integrací svých nástrojů se službou SQL Data Warehouse. Úplný seznam najdete v tématu [Partneři řešení SQL Data Warehouse][SQL Data Warehouse solution partners].

## <a name="hybrid-data-sources-scenarios"></a>Scénáře hybridních zdrojů dat
PolyBase vám umožňuje využít data z různých zdrojů pomocí stejných příkazů T-SQL, které dobře znáte. PolyBase umožňuje dotazovat nerelační data uložená v Úložišti objektů blob v Azure, jako by šlo o běžnou tabulku. Použijte PolyBase k dotazům na nerelační data a importu nerelačních dat do služby SQL Data Warehouse.

* PolyBase používá k přístupu k nerelačním datům externí tabulky. Definice tabulek jsou uložené ve službě SQL Data Warehouse a můžete k nim přistupovat pomocí SQL a nástrojů stejně, jako byste přistupovali k normálním relačním datům.
* PolyBase nedělá při integraci žádné rozdíly. Zpřístupňuje stejné prvky a funkce všem zdrojům, které podporuje. PolyBase čte data v mnoha různých formátech, včetně souborů s oddělovači a souborů ORC.
* PolyBase lze použít pro přístup k úložišti objektů blob, které se také používá jako úložiště clusteru HDInsight. To umožňuje přístup ke stejným datům pomocí relačních a nerelačních nástrojů.

## <a name="sla"></a>SLA
SQL Data Warehouse nabízí smlouvu o úrovni služeb (SLA) na úrovni produktu jako součást smlouvy SLA pro Microsoft Online Services. Další informace najdete na stránkách [SLA pro SQL Data Warehouse][SLA for SQL Data Warehouse]. Informace o smlouvách SLA ke všem dalším produktům najdete na stránce Azure věnované [smlouvám o úrovni služeb], případně si je můžete stáhnout na stránce o [multilicencích][Volume Licensing]. 

## <a name="next-steps"></a>Další kroky
Teď, když jste se s SQL Data Warehouse seznámili, můžete zjistit, jak rychle [vytvořit datový sklad SQL Data Warehouse][create a SQL Data Warehouse] a [načíst ukázková data][load sample data]. Pokud s Azure začínáte, může vám být užitečný [Glosář Azure][Azure glossary], kde najdete potřebnou terminologii. Můžete se také podívat na některé z těchto dalších zdrojů ke službě SQL Data Warehouse.  

* [Úspěšné zákaznické implementace]
* [Blogy]
* [Žádosti o funkce]
* [Videa]
* [Blogy zákaznického poradního týmu]
* [Vytvoření lístku podpory]
* [Fórum MSDN]
* [Fórum Stack Overflow]
* [Twitter]

<!--Image references-->
[1]: ./media/sql-data-warehouse-overview-what-is/dwarchitecture.png

<!--Article references-->
[Vytvoření lístku podpory]: ./sql-data-warehouse-get-started-create-support-ticket.md
[load sample data]: ./sql-data-warehouse-load-sample-databases.md
[create a SQL Data Warehouse]: ./sql-data-warehouse-get-started-provision.md
[Migration documentation]: ./sql-data-warehouse-overview-migrate.md
[SQL Data Warehouse solution partners]: ./sql-data-warehouse-partner-business-intelligence.md
[Integrated tools overview]: ./sql-data-warehouse-overview-integrate.md
[Backup and restore overview]: ./sql-data-warehouse-restore-database-overview.md
[Azure glossary]: ../azure-glossary-cloud-terminology.md

<!--MSDN references-->

<!--Other Web references-->
[Úspěšné zákaznické implementace]: https://customers.microsoft.com/search?sq=&ff=story_products_services%26%3EAzure%2FAzure%2FAzure%20SQL%20Data%20Warehouse%26%26story_product_families%26%3EAzure%2FAzure%26%26story_product_categories%26%3EAzure&p=0
[Blogy]: https://azure.microsoft.com/blog/tag/azure-sql-data-warehouse/
[Blogy zákaznického poradního týmu]: https://blogs.msdn.microsoft.com/sqlcat/tag/sql-dw/
[Žádosti o funkce]: https://feedback.azure.com/forums/307516-sql-data-warehouse
[Fórum MSDN]: https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=AzureSQLDataWarehouse
[Fórum Stack Overflow]: http://stackoverflow.com/questions/tagged/azure-sqldw
[Twitter]: https://twitter.com/hashtag/SQLDW
[Videa]: https://azure.microsoft.com/documentation/videos/index/?services=sql-data-warehouse
[SLA for SQL Data Warehouse]: https://azure.microsoft.com/en-us/support/legal/sla/sql-data-warehouse/v1_0/
[Volume Licensing]: http://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=37
[smlouvám o úrovni služeb]: https://azure.microsoft.com/en-us/support/legal/sla/

