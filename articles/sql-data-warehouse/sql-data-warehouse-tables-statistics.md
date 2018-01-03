---
title: "Správa statistiky u tabulek v SQL Data Warehouse | Microsoft Docs"
description: "Začínáme s statistiky pro tabulky v Azure SQL Data Warehouse."
services: sql-data-warehouse
documentationcenter: NA
author: barbkess
manager: jenniehubbard
editor: 
ms.assetid: faa1034d-314c-4f9d-af81-f5a9aedf33e4
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: tables
ms.date: 11/06/2017
ms.author: barbkess
ms.openlocfilehash: 4d5777e69b7ea3fa206bf8909c255b998be69e8a
ms.sourcegitcommit: 901a3ad293669093e3964ed3e717227946f0af96
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/21/2017
---
# <a name="managing-statistics-on-tables-in-sql-data-warehouse"></a>Správa statistiky u tabulek v SQL Data Warehouse
> [!div class="op_single_selector"]
> * [Přehled][Overview]
> * [Datové typy][Data Types]
> * [Distribuce][Distribute]
> * [Index][Index]
> * [Oddíl][Partition]
> * [Statistiky][Statistics]
> * [Dočasné][Temporary]
> 
> 

Čím SQL Data Warehouse ví o vašich dat, tím rychleji se spouštět dotazy na data.  Způsobem říct SQL Data Warehouse o vašich dat je shromažďování statistických údajů o vaše data. S statistické údaje o vašich dat je jedním z nejdůležitějších kroků, které můžete provést za účelem optimalizace své dotazy. Je to proto Optimalizátor dotazů SQL Data Warehouse je na základě nákladů pro optimalizaci. Porovnává náklady na různé plány dotazů a potom vybere plán s nejnižší náklady, které by se měly také plán, který provede nejrychlejší. Například pokud okně Optimalizace odhadne, že data jsou filtrování v dotazu vrátí 1 řádek, může vybrat, velmi jiné plánování než v případě ji odhadne jejich datum, na které jste vybrali bude vracet 1 milionu řádků.

Proces vytváření a aktualizaci statistiky je aktuálně ruční proces, ale je velmi jednoduchý udělat.  Youw bude možné brzy vytvářet a aktualizovat statistiku jednoho sloupce a indexy automaticky.  Pomocí následujících informací můžete výrazně automatizovat správu statistiku na vaše data. 

## <a name="getting-started-with-statistics"></a>Začínáme s statistiky
Vytvoření jen Vzorkovaná statistiku pro každý sloupec je snadný způsob, jak začít pracovat s statistiky. Zastaralé statistiky povede k dotazu optimální výkon. Ale spotřebovat paměti aktualizovat statistiku pro všechny sloupce, jelikož vaše data. 

Zde jsou některé doporučení pro různé scénáře:
| **Scénáře** | Doporučení |
|:--- |:--- |
| **Začínáme** | Aktualizovat všechny sloupce po migraci datového skladu SQL |
| **Nejdůležitější sloupec pro statistiky** | Distribuční klíč s algoritmem hash |
| **Druhý nejdůležitější sloupec pro statistiky** | Klíč oddílu |
| **Další důležité sloupce pro statistiky** | Datum, často spojení, GROUP BY, HAVING a kde |
| **Četnosti aktualizací statistik**  | Konzervativní: denně <br></br> Poté, co načítání nebo transformace dat |
| **Vzorkování** |  Pod 1 B řádky použijte výchozí vzorkování (20 %) <br></br> S více než 1 B řádky tabulky je dobré statistiky v rozsahu 2 % |

## <a name="updating-statistics"></a>Aktualizuje statistické údaje

Jeden osvědčeným postupem je aktualizovat statistiku sloupců s kalendářními daty každý den při přidávání nová data. Každé nové řádky času jsou načtená do datového skladu, nové zatížení kalendářní data nebo data transakcí se přidají. Tyto změnit distribuci dat a proveďte statistiku zastaralé. Naopak statistiky země sloupec v tabulce zákazníka může být nikdy potřeba aktualizovat, jako rozdělení hodnot nemění obecně. Za předpokladu, že distribuce je konstantní mezi odběrateli, přidávání nových řádků do tabulky variace není chystáte změnit rozdělení data. Ale pokud váš datový sklad obsahuje pouze jeden země a připojte ve data z nové země, výsledkem data z několika zemích, které ukládají, pak výborný musíte aktualizovat statistiku na sloupci země.

Jeden z první otázky při řešení potíží s dotazu se **"Jsou statistiku aktuální?"**

Tento dotaz není ten, který může odpovídat stáří data. Aktuální statistiku objektu může být velmi staré, pokud byl žádné závažné změny v základních datech. Pokud počet řádků, došlo ke změně podstatně nebo dojde ke změně podstatným v distribuci hodnot pro daný sloupec *pak* je třeba aktualizovat statistiku.

Vzhledem k tomu, že neexistuje žádná DMV k určení, jestli data v tabulce se změnil od posledního statistiku časových údajů byly aktualizovány, znalost stáří statistice můžete nabízejí část obrázku.  Následující dotaz můžete použít k určení poslední statistice tam, kde na každou tabulku aktualizovat.  

> [!NOTE]
> Mějte na paměti, že pokud dojde ke změně podstatným v distribuci hodnot pro daný sloupec, by měl aktualizovat statistiku bez ohledu na to, kdy se aktualizovaly naposledy.  
> 
> 

```sql
SELECT
    sm.[name] AS [schema_name],
    tb.[name] AS [table_name],
    co.[name] AS [stats_column_name],
    st.[name] AS [stats_name],
    STATS_DATE(st.[object_id],st.[stats_id]) AS [stats_last_updated_date]
FROM
    sys.objects ob
    JOIN sys.stats st
        ON  ob.[object_id] = st.[object_id]
    JOIN sys.stats_columns sc    
        ON  st.[stats_id] = sc.[stats_id]
        AND st.[object_id] = sc.[object_id]
    JOIN sys.columns co    
        ON  sc.[column_id] = co.[column_id]
        AND sc.[object_id] = co.[object_id]
    JOIN sys.types  ty    
        ON  co.[user_type_id] = ty.[user_type_id]
    JOIN sys.tables tb    
        ON  co.[object_id] = tb.[object_id]
    JOIN sys.schemas sm    
        ON  tb.[schema_id] = sm.[schema_id]
WHERE
    st.[user_created] = 1;
```

**Datum sloupce** v datovém skladu, například obvykle třeba často aktualizace statistiky. Každé nové řádky času jsou načtená do datového skladu, nové zatížení kalendářní data nebo data transakcí se přidají. Tyto změnit distribuci dat a proveďte statistiku zastaralé.  Naopak statistiky o pohlaví sloupec v tabulce zákazníka může být nikdy potřeba aktualizovat. Za předpokladu, že distribuce je konstantní mezi odběrateli, přidávání nových řádků do tabulky variace není chystáte změnit rozdělení data. Ale pokud váš datový sklad obsahuje pouze jeden pohlaví a nový požadavek výsledkem více pohlaví výborný musíte aktualizovat statistiku na sloupci pohlaví.

Další informace naleznete v části [statistiky] [ Statistics] na webu MSDN.

## <a name="implementing-statistics-management"></a>Implementace správy statistiky
Často je vhodné rozšířit vaše data načítání procesu zajistit, že se statistika aktualizuje na konci zatížení. Načtení dat je při tabulky nejčastěji změnit jejich velikost a jejich distribuci hodnoty. To je proto logické místo, kde můžete implementovat některé procesy správy.

Některé zásady jsou uvedené pro aktualizaci statistice během procesu načítání:

* Zajistěte, aby každá tabulka načíst minimálně jeden objekt statistiky aktualizovat. Tím se aktualizuje informace o velikosti (počet řádků a počet stránek) tabulky jako součást aktualizace statistiky.
* Zaměřit se na sloupců podílejících se na připojení, GROUP BY, ORDER BY a DISTINCT – klauzule
* Zvažte aktualizaci "vzestupné klíč" sloupců, jako je například transakce častěji, jak tyto hodnoty nebudou zahrnuty do statistiky histogram kalendářní data.
* Zvažte aktualizaci statické distribuční sloupce méně často.
* Mějte na paměti, že každý objekt statistiky je aktualizovat v řadě. Implementací `UPDATE STATISTICS <TABLE_NAME>` nemusí být právě ideální - hlavně pro široké tabulky s mnoha objekty statistiky.

> [!NOTE]
> Další podrobnosti na [vzestupné klíče] naleznete v SQL serveru 2014 mohutnost odhad modelu dokumentu White Paper.
> 
> 

Další informace naleznete v části [odhadu kardinality] [ Cardinality Estimation] na webu MSDN.

## <a name="examples-create-statistics"></a>Příklady: Vytvoření statistiky
Tyto příklady ukazují, jak používat různé možnosti pro vytvoření statistiky. Možnosti, které používáte pro každý sloupec závisí na vlastnosti data a jak sloupec se použije v dotazech.

### <a name="a-create-single-column-statistics-with-default-options"></a>A. Vytvoření statistiky jednoho sloupce s výchozími možnostmi
Chcete-li vytvoření statistiky pro sloupec, stačí zadáte název pro objekt statistiky a název sloupce.

Tuto syntaxi používá všechny výchozí možnosti. Ve výchozím nastavení SQL Data Warehouse **ukázky 20 procent** tabulky při vytváření statistik.

```sql
CREATE STATISTICS [statistics_name] ON [schema_name].[table_name]([column_name]);
```

Příklad:

```sql
CREATE STATISTICS col1_stats ON dbo.table1 (col1);
```

### <a name="b-create-single-column-statistics-by-examining-every-row"></a>B. Vytvořit jednosloupcovou statistiku tak, že prověří každý řádek
Pro většině případů stačí výchozí vzorkovací frekvenci 20 procent. Můžete však upravit vzorkovací frekvenci.

Chcete-li ukázkové úplné tabulky, použijte následující syntaxi:

```sql
CREATE STATISTICS [statistics_name] ON [schema_name].[table_name]([column_name]) WITH FULLSCAN;
```

Příklad:

```sql
CREATE STATISTICS col1_stats ON dbo.table1 (col1) WITH FULLSCAN;
```

### <a name="c-create-single-column-statistics-by-specifying-the-sample-size"></a>C. Vytvořte jednosloupcovou statistiku zadáním velikost vzorku
Alternativně můžete určit velikost vzorku v procentech:

```sql
CREATE STATISTICS col1_stats ON dbo.table1 (col1) WITH SAMPLE = 50 PERCENT;
```

### <a name="d-create-single-column-statistics-on-only-some-of-the-rows"></a>D. Vytvořit jednosloupcovou statistiku pro jenom některé řádky
Další možností statistiky můžete vytvořit na část řádky v tabulce. Tomu se říká filtrované statistiky.

Můžete například použít filtrovanou statistiku při plánování k dotazování na konkrétní oddíl velkých oddílů tabulky. Vytvořením statistiky na pouze hodnoty oddílu se přesnost statistik pro zlepšení a proto zlepšit výkon dotazu.

Tento příklad vytvoří statistiky na rozsah hodnot. Hodnoty můžete snadno nadefinovat tak, aby odpovídaly rozsahu hodnot v oddílu.

```sql
CREATE STATISTICS stats_col1 ON table1(col1) WHERE col1 > '2000101' AND col1 < '20001231';
```

> [!NOTE]
> Pro Optimalizátor dotazů, zvažte možnost použití filtrovanou statistiku, když se vybere plánu distribuovaných dotazů dotaz musí vejít do definice objektu statistiky. Použijeme předchozí příklad dotazu kde klauzule musí určovat Sloupec1 hodnoty mezi 2000101 a 20001231.
> 
> 

### <a name="e-create-single-column-statistics-with-all-the-options"></a>E. Vytvořit jednosloupcovou statistiku se všemi možnostmi
Možnosti můžete kombinovat samozřejmě společně. Následující příklad vytvoří objekt filtrovanou statistiku s velikost vlastní vzorku:

```sql
CREATE STATISTICS stats_col1 ON table1 (col1) WHERE col1 > '2000101' AND col1 < '20001231' WITH SAMPLE = 50 PERCENT;
```

Úplný přehled najdete v tématu [CREATE STATISTICS] [ CREATE STATISTICS] na webu MSDN.

### <a name="f-create-multi-column-statistics"></a>F. Vytvoření statistiky více sloupci
Vytvoření statistiky vícesloupcového, jednoduše použijte v předchozích příkladech, ale zadat více sloupců.

> [!NOTE]
> Histogram, který slouží k zjištění přibližné hodnoty počet řádků ve výsledku dotazu, je dostupná jenom pro první sloupec uvedené v definici objektu statistiky.
> 
> 

V tomto příkladu je histogramu na *produktu\_kategorie*. Statistiky mezi sloupce jsou vypočítány na *produktu\_kategorie* a *produktu\_sub_category*:

```sql
CREATE STATISTICS stats_2cols ON table1 (product_category, product_sub_category) WHERE product_category > '2000101' AND product_category < '20001231' WITH SAMPLE = 50 PERCENT;
```

Vzhledem k tomu, že existuje korelace mezi *produktu\_kategorie* a *produktu\_sub\_kategorie*, může být užitečné, pokud tyto sloupce, ke kterým se přistupuje vícesloupcového stat ve stejnou dobu.

### <a name="g-create-statistics-on-all-the-columns-in-a-table"></a>G. Vytvoření statistiky pro všechny sloupce v tabulce
Jeden způsob, jak vytvořit statistiku problémy příkazy CREATE STATISTICS je po vytvoření tabulky.

```sql
CREATE TABLE dbo.table1
(
   col1 int
,  col2 int
,  col3 int
)
WITH
  (
    CLUSTERED COLUMNSTORE INDEX
  )
;

CREATE STATISTICS stats_col1 on dbo.table1 (col1);
CREATE STATISTICS stats_col2 on dbo.table2 (col2);
CREATE STATISTICS stats_col3 on dbo.table3 (col3);
```

### <a name="h-use-a-stored-procedure-to-create-statistics-on-all-columns-in-a-database"></a>H. Vytvoření statistiky pro všechny sloupce v databázi pomocí uložené procedury
SQL Data Warehouse nemá ekvivalentní [] – [sp_create_stats] systémové uložené procedury v systému SQL Server. Tato uložená procedura vytvoří objekt statistiky jeden sloupec pro každý sloupec databáze, který ještě nemá statistiky.

To vám pomůže začít pracovat s návrhu databáze. Nebojte se, že jej přizpůsobit svým potřebám.

```sql
CREATE PROCEDURE    [dbo].[prc_sqldw_create_stats]
(   @create_type    tinyint -- 1 default 2 Fullscan 3 Sample
,   @sample_pct     tinyint
)
AS

IF @create_type NOT IN (1,2,3)
BEGIN
    THROW 151000,'Invalid value for @stats_type parameter. Valid range 1 (default), 2 (fullscan) or 3 (sample).',1;
END;

IF @sample_pct IS NULL
BEGIN;
    SET @sample_pct = 20;
END;

IF OBJECT_ID('tempdb..#stats_ddl') IS NOT NULL
BEGIN;
    DROP TABLE #stats_ddl;
END;

CREATE TABLE #stats_ddl
WITH    (   DISTRIBUTION    = HASH([seq_nmbr])
        ,   LOCATION        = USER_DB
        )
AS
WITH T
AS
(
SELECT      t.[name]                        AS [table_name]
,           s.[name]                        AS [table_schema_name]
,           c.[name]                        AS [column_name]
,           c.[column_id]                   AS [column_id]
,           t.[object_id]                   AS [object_id]
,           ROW_NUMBER()
            OVER(ORDER BY (SELECT NULL))    AS [seq_nmbr]
FROM        sys.[tables] t
JOIN        sys.[schemas] s         ON  t.[schema_id]       = s.[schema_id]
JOIN        sys.[columns] c         ON  t.[object_id]       = c.[object_id]
LEFT JOIN   sys.[stats_columns] l   ON  l.[object_id]       = c.[object_id]
                                    AND l.[column_id]       = c.[column_id]
                                    AND l.[stats_column_id] = 1
LEFT JOIN    sys.[external_tables] e    ON    e.[object_id]        = t.[object_id]
WHERE       l.[object_id] IS NULL
AND            e.[object_id] IS NULL -- not an external table
)
SELECT  [table_schema_name]
,       [table_name]
,       [column_name]
,       [column_id]
,       [object_id]
,       [seq_nmbr]
,       CASE @create_type
        WHEN 1
        THEN    CAST('CREATE STATISTICS '+QUOTENAME('stat_'+table_schema_name+ '_' + table_name + '_'+column_name)+' ON '+QUOTENAME(table_schema_name)+'.'+QUOTENAME(table_name)+'('+QUOTENAME(column_name)+')' AS VARCHAR(8000))
        WHEN 2
        THEN    CAST('CREATE STATISTICS '+QUOTENAME('stat_'+table_schema_name+ '_' + table_name + '_'+column_name)+' ON '+QUOTENAME(table_schema_name)+'.'+QUOTENAME(table_name)+'('+QUOTENAME(column_name)+') WITH FULLSCAN' AS VARCHAR(8000))
        WHEN 3
        THEN    CAST('CREATE STATISTICS '+QUOTENAME('stat_'+table_schema_name+ '_' + table_name + '_'+column_name)+' ON '+QUOTENAME(table_schema_name)+'.'+QUOTENAME(table_name)+'('+QUOTENAME(column_name)+') WITH SAMPLE '+@sample_pct+'PERCENT' AS VARCHAR(8000))
        END AS create_stat_ddl
FROM T
;

DECLARE @i INT              = 1
,       @t INT              = (SELECT COUNT(*) FROM #stats_ddl)
,       @s NVARCHAR(4000)   = N''
;

WHILE @i <= @t
BEGIN
    SET @s=(SELECT create_stat_ddl FROM #stats_ddl WHERE seq_nmbr = @i);

    PRINT @s
    EXEC sp_executesql @s
    SET @i+=1;
END

DROP TABLE #stats_ddl;
```

Vytvoření statistiky pro všechny sloupce v tabulce s tímto postupem, jednoduše voláním procedury.

```sql
prc_sqldw_create_stats;
```

## <a name="examples-update-statistics"></a>Příklady: aktualizovat statistiky
Chcete-li aktualizovat statistiku, můžete:

1. Aktualizujte jeden objekt statistiky. Zadejte název objektu statistiku, který se má aktualizovat.
2. Aktualizujte všechny statistiky objekty v tabulce. Zadejte název tabulky místo jeden objekt konkrétní statistiku.

### <a name="a-update-one-specific-statistics-object"></a>A. Aktualizovat jeden objekt konkrétní Statistika
Aktualizovat objekt konkrétní statistiku použijte následující syntaxi:

```sql
UPDATE STATISTICS [schema_name].[table_name]([stat_name]);
```

Příklad:

```sql
UPDATE STATISTICS [dbo].[table1] ([stats_col1]);
```

Při aktualizaci statistiky konkrétní objekty, můžete snížit čas a prostředky, které jsou nezbytné ke správě statistiky. To vyžaduje některé myšlenku, ale vybrat nejlepší statistiky objekty, které chcete aktualizovat.

### <a name="b-update-all-statistics-on-a-table"></a>B. Aktualizovat všechny statistiky v tabulce
Ukazuje to jednoduše aktualizace všechny statistiky objektů v tabulce.

```sql
UPDATE STATISTICS [schema_name].[table_name];
```

Příklad:

```sql
UPDATE STATISTICS dbo.table1;
```

Tento příkaz je snadno použitelný. Jenom nezapomeňte to aktualizuje všechny statistiky v tabulce a proto může provést další práci, než je nezbytné. Pokud výkon není problém, to je výborný nejúplnější a nejjednodušší způsob, jak zajistit, že statistiky jsou aktuální.

> [!NOTE]
> Při aktualizaci všechny statistiky v tabulce se SQL Data Warehouse nepodporuje vyhledávání s cílem ukázková tabulka pro každou statistiku. Pokud je tabulka velký, má mnoho sloupců a mnoho statistiky, může to být efektivnější jednotlivých statistiku podle potřeb.
> 
> 

Implementace `UPDATE STATISTICS` postupu najdete v tématu [dočasných tabulek] [ Temporary] článku. Implementace metody se mírně liší na `CREATE STATISTICS` výše uvedený postup, ale konečný výsledek je stejný.

Úplná syntaxe, najdete v části [Update Statistics] [ Update Statistics] na webu MSDN.

## <a name="statistics-metadata"></a>Statistiky metadat
Existuje několik systémové zobrazení a funkce, které můžete použít k nalezení informací o statistikách. Například se zobrazí, pokud objekt statistiky může být zastaralé pomocí funkce statistiky date a zjistěte, kdy byly statistiky poslední vytvořil nebo aktualizoval.

### <a name="catalog-views-for-statistics"></a>Zobrazení katalogu pro statistiky
Tato systémová zobrazení obsahují informace o statistiky:

| Zobrazení katalogu | Popis |
|:--- |:--- |
| [Sys.Columns][sys.columns] |Jeden řádek pro každý sloupec. |
| [Sys.Objects][sys.objects] |Jeden řádek pro každý objekt v databázi. |
| [Sys.Schemas][sys.schemas] |Jeden řádek pro každý schématu v databázi. |
| [Sys.stats][sys.stats] |Jeden řádek pro každý objekt statistiky. |
| [Sys.stats_columns][sys.stats_columns] |Jeden řádek pro každý sloupec v objektu statistiky. Odkazy Zpět na sys.columns. |
| [zobrazení Sys.Tables][sys.tables] |Jeden řádek pro každou tabulku (zahrnuje externí tabulky). |
| [Sys.table_types][sys.table_types] |Jeden řádek pro každý typ dat. |

### <a name="system-functions-for-statistics"></a>Funkce systému pro statistiky
Tyto funkce systému jsou užitečné pro práci s statistiky:

| System – funkce | Popis |
|:--- |:--- |
| [STATS_DATE][STATS_DATE] |Datum poslední aktualizace objekt statistiky. |
| [PŘÍKAZ DBCC SHOW_STATISTICS][DBCC SHOW_STATISTICS] |Poskytuje souhrnné úrovně a podrobné informace o distribuci hodnoty rozumí objekt statistiky. |

### <a name="combine-statistics-columns-and-functions-into-one-view"></a>Zkombinovat do jednoho zobrazení statistiky sloupce a funkce
Toto zobrazení přináší sloupce, které se týkají statistiky a výsledkem [] – funkce [STATS_DATE()] společně.

```sql
CREATE VIEW dbo.vstats_columns
AS
SELECT
        sm.[name]                           AS [schema_name]
,       tb.[name]                           AS [table_name]
,       st.[name]                           AS [stats_name]
,       st.[filter_definition]              AS [stats_filter_defiinition]
,       st.[has_filter]                     AS [stats_is_filtered]
,       STATS_DATE(st.[object_id],st.[stats_id])
                                            AS [stats_last_updated_date]
,       co.[name]                           AS [stats_column_name]
,       ty.[name]                           AS [column_type]
,       co.[max_length]                     AS [column_max_length]
,       co.[precision]                      AS [column_precision]
,       co.[scale]                          AS [column_scale]
,       co.[is_nullable]                    AS [column_is_nullable]
,       co.[collation_name]                 AS [column_collation_name]
,       QUOTENAME(sm.[name])+'.'+QUOTENAME(tb.[name])
                                            AS two_part_name
,       QUOTENAME(DB_NAME())+'.'+QUOTENAME(sm.[name])+'.'+QUOTENAME(tb.[name])
                                            AS three_part_name
FROM    sys.objects                         AS ob
JOIN    sys.stats           AS st ON    ob.[object_id]      = st.[object_id]
JOIN    sys.stats_columns   AS sc ON    st.[stats_id]       = sc.[stats_id]
                            AND         st.[object_id]      = sc.[object_id]
JOIN    sys.columns         AS co ON    sc.[column_id]      = co.[column_id]
                            AND         sc.[object_id]      = co.[object_id]
JOIN    sys.types           AS ty ON    co.[user_type_id]   = ty.[user_type_id]
JOIN    sys.tables          AS tb ON  co.[object_id]        = tb.[object_id]
JOIN    sys.schemas         AS sm ON  tb.[schema_id]        = sm.[schema_id]
WHERE   1=1
AND     st.[user_created] = 1
;
```

## <a name="dbcc-showstatistics-examples"></a>Příkaz DBCC SHOW_STATISTICS() příklady
Příkaz DBCC SHOW_STATISTICS() zobrazuje data ukládaná v rámci objektu statistiky. Tato data je rozdělena na tři části.

1. Záhlaví
2. Hustotu vektoru
3. Histogram

Hlavička metadata o statistikách. Histogramu zobrazí rozdělení hodnot ve sloupci první klíče objektu statistiky. Vektor hustotu měří korelace mezi sloupci. SQLDW vypočítá mohutnost odhady s žádným z dat v objektu statistiky.

### <a name="show-header-density-and-histogram"></a>Zobrazit záhlaví, hustotu a histogram
Tento jednoduchý příklad ukazuje všechny tři částí objektu statistiky.

```sql
DBCC SHOW_STATISTICS([<schema_name>.<table_name>],<stats_name>)
```

Příklad:

```sql
DBCC SHOW_STATISTICS (dbo.table1, stats_col1);
```

### <a name="show-one-or-more-parts-of-dbcc-showstatistics"></a>Zobrazit jednu nebo více částí DBCC SHOW_STATISTICS();
Pokud vás zajímá pouze v zobrazení konkrétní části, použijte `WITH` klauzule a zadejte části, které chcete zobrazit:

```sql
DBCC SHOW_STATISTICS([<schema_name>.<table_name>],<stats_name>) WITH stat_header, histogram, density_vector
```

Příklad:

```sql
DBCC SHOW_STATISTICS (dbo.table1, stats_col1) WITH histogram, density_vector
```

## <a name="dbcc-showstatistics-differences"></a>Příkaz DBCC SHOW_STATISTICS() rozdíly
Příkaz DBCC SHOW_STATISTICS() je implementováno více výhradně v SQL Data Warehouse ve srovnání s systému SQL Server.

1. Nezdokumentovaný funkce nejsou podporovány.
2. Nelze použít Stats_stream
3. Se nemůže připojit k výsledky pro konkrétní podmnožiny dat statistiky např (STAT_HEADER spojení DENSITY_VECTOR)
4. NO_INFOMSGS nelze nastavit pro potlačení zprávy
5. Hranaté závorky kolem názvů statistiky nelze použít.
6. Názvy sloupců nelze použít k identifikaci objektů statistiky
7. Vlastní chyba 2767 není podporovaná.

## <a name="next-steps"></a>Další postup
Další podrobnosti najdete v tématu [DBCC SHOW_STATISTICS] [ DBCC SHOW_STATISTICS] na webu MSDN.  Další informace najdete v článcích na [tabulky přehled][Overview], [tabulky datové typy][Data Types], [distribuci tabulku] [ Distribute], [Indexování tabulku][Index], [vytváření oddílů tabulky] [ Partition] a [Dočasných tabulek][Temporary].  Další informace o osvědčených postupech najdete v tématu [SQL Data Warehouse osvědčené postupy][SQL Data Warehouse Best Practices].  

<!--Image references-->

<!--Article references-->
[Overview]: ./sql-data-warehouse-tables-overview.md
[Data Types]: ./sql-data-warehouse-tables-data-types.md
[Distribute]: ./sql-data-warehouse-tables-distribute.md
[Index]: ./sql-data-warehouse-tables-index.md
[Partition]: ./sql-data-warehouse-tables-partition.md
[Statistics]: ./sql-data-warehouse-tables-statistics.md
[Temporary]: ./sql-data-warehouse-tables-temporary.md
[SQL Data Warehouse Best Practices]: ./sql-data-warehouse-best-practices.md

<!--MSDN references-->  
[Cardinality Estimation]: https://msdn.microsoft.com/library/dn600374.aspx
[CREATE STATISTICS]: https://msdn.microsoft.com/library/ms188038.aspx
[DBCC SHOW_STATISTICS]:https://msdn.microsoft.com/library/ms174384.aspx
[Statistics]: https://msdn.microsoft.com/library/ms190397.aspx
[STATS_DATE]: https://msdn.microsoft.com/library/ms190330.aspx
[sys.columns]: https://msdn.microsoft.com/library/ms176106.aspx
[sys.objects]: https://msdn.microsoft.com/library/ms190324.aspx
[sys.schemas]: https://msdn.microsoft.com/library/ms190324.aspx
[sys.stats]: https://msdn.microsoft.com/library/ms177623.aspx
[sys.stats_columns]: https://msdn.microsoft.com/library/ms187340.aspx
[sys.tables]: https://msdn.microsoft.com/library/ms187406.aspx
[sys.table_types]: https://msdn.microsoft.com/library/bb510623.aspx
[UPDATE STATISTICS]: https://msdn.microsoft.com/library/ms187348.aspx

<!--Other Web references-->  
