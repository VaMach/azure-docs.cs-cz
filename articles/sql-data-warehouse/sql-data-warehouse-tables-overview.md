---
title: "Přehled tabulek v SQL Data Warehouse | Microsoft Docs"
description: "Začínáme s Azure SQL Data Warehouse tabulky."
services: sql-data-warehouse
documentationcenter: NA
author: barbkess
manager: jenniehubbard
editor: 
ms.assetid: 2114d9ad-c113-43da-859f-419d72604bdf
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: tables
ms.date: 12/14/2017
ms.author: barbkess
ms.openlocfilehash: 46f7d2ea19a88e65b2d039fdf36d1619c4d74020
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/21/2017
---
# <a name="overview-of-tables-in-sql-data-warehouse"></a>Přehled tabulek v SQL Data Warehouse
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

Začínáme s vytváření tabulek v SQL Data Warehouse je jednoduché.  Základní [CREATE TABLE] [ CREATE TABLE] syntaxe následuje běžné syntaxe se pravděpodobně už znáte z práce jiné databáze.  Pokud chcete vytvořit tabulku, musíte jednoduše název nové tabulky, název sloupce a definování typů dat pro každý sloupec.  Pokud vytvoříte tabulky v ostatních databázích to velmi povědomé pro vás.

```sql  
CREATE TABLE Customers (FirstName VARCHAR(25), LastName VARCHAR(25))
 ``` 

Tento příklad vytvoří tabulku s názvem zákazníků se dva sloupce, jméno a příjmení.  Každý sloupec je definován s datovým typem VARCHAR(25), což omezí data 25 znaků.  Tyto atributy základní tabulku, jakož i ostatní, jsou z velké části stejný jako jiné databáze.  Datové typy jsou definovány pro každý sloupec a zajištění integrity dat.  Indexy mohou být přidány do zlepšit výkon snížením vstupně-výstupní operace.  Vytváření oddílů lze přidat ke zlepšení výkonu při budete muset upravit data.

[Přejmenování] [ RENAME] SQL Data Warehouse tabulku vypadá takto:

```sql  
RENAME OBJECT Customer TO CustomerOrig; 
 ```

## <a name="distributed-tables"></a>Distribuované tabulky
Nový základní atribut zaváděné distribuovaných systémů, jako je SQL Data Warehouse **distribuční sloupce**.  Sloupec distribuce je velmi mnohem co vypadá jako.  Je sloupec, který určuje, jak distribuovat nebo rozdělte data na pozadí.  Při vytvoření tabulky bez zadání distribuční sloupce v tabulce je automaticky distribuován pomocí **kruhové dotazování**.  Kruhové dotazování tabulky mohou být dostatečná v některých scénářích, definování distribuční sloupce může výrazně omezit přesun dat během dotazy, proto optimalizace výkonu.  V situacích, kde malé množství dat v tabulce se rozhodnete vytvořit tabulku s **replikovat** distribuční typ zkopíruje data na každém výpočetním uzlu a uloží přesun dat v době provedení dotazu. V tématu [distribuci tabulku] [ Distribute] Další informace o tom, jak vybrat distribuční sloupce.

## <a name="indexing-and-partitioning-tables"></a>Indexování a rozdělení do oddílů tabulky
Jako stát pokročilejší pomocí SQL Data Warehouse a chcete za účelem optimalizace výkonu, budete chtít získat další informace o návrh tabulky.  Další informace najdete v článcích na [tabulky datové typy][Data Types], [distribuci tabulku][Distribute], [indexování tabulku] [ Index] a [vytváření oddílů tabulky][Partition].

## <a name="table-statistics"></a>Statistiky tabulky
Statistiky jsou velmi důležité pro získávání nejlepší výkon z SQL Data Warehouse.  Vzhledem k tomu, že SQL Data Warehouse automaticky ještě není vytvářet a aktualizovat statistiku pro vás, jako jsou může pocházet očekávat ve službě Azure SQL Database, čtení na náš článek [statistiky] [ Statistics] může být jedním z nejvíc důležité články přečíst zajistit získáte nejlepší výkon ze své dotazy.

## <a name="temporary-tables"></a>Dočasné tabulky
Dočasné tabulky jsou tabulky, které pouze existovat po dobu trvání vaše přihlašovací jméno a ostatní uživatelé je nemohou vidět.  Dočasné tabulky může být dobrým způsobem, jak zakázat ostatním zobrazit dočasné výsledky a také snížit nutnost čištění.  Vzhledem k tomu, že dočasných tabulek také používat místní úložiště, můžete nabízí vyšší výkon pro některé operace.  Najdete v článku [dočasné tabulky] [ Temporary] články pro další podrobnosti o dočasné tabulky.

## <a name="external-tables"></a>Externí tabulky
Externí tabulky, také známé jako Polybase tabulky, jsou tabulky, které můžete zadat dotaz na externí data z SQL Data Warehouse z SQL Data Warehouse, ale bodu.  Můžete například vytvořit externí tabulku které body k souborům na Azure Blob Storage nebo Azure Data Lake Store.  Další podrobnosti o tom, jak vytvořit a dotaz na externí tabulky najdete v tématu [načtení dat pomocí funkce Polybase][Load data with Polybase].  

## <a name="unsupported-table-features"></a>Funkce nepodporované tabulky
Zatímco SQL Data Warehouse obsahuje mnoho stejných funkcí tabulky nabízené jiných databází, nejsou některé funkce, které ještě nejsou podporovány.  Níže je seznam některých funkcí tabulky, které ještě nejsou podporovány.

| Nepodporované funkce |
| --- |
| Primární klíč, cizí klíče, jedinečný a zkontrolujte [omezení tabulky][Table Constraints] |
| [Jedinečné indexy][Unique Indexes] |
| [Počítané sloupce][Computed Columns] |
| [Zhuštěné sloupce][Sparse Columns] |
| [Uživatelem definované typy][User-Defined Types] |
| [Pořadí][Sequence] |
| [Aktivační události][Triggers] |
| [Indexovaná zobrazení][Indexed Views] |
| [Synonyma.][Synonyms] |

## <a name="table-size-queries"></a>Dotazy na velikost tabulky
Jeden způsob, jak identifikovat místa a řádky, které spotřebovávají tabulku v každé z 60 distribuce, je použití [DBCC PDW_SHOWSPACEUSED][DBCC PDW_SHOWSPACEUSED].

```sql
DBCC PDW_SHOWSPACEUSED('dbo.FactInternetSales');
```

Však příkazy DBCC může být poměrně omezení.  Zobrazení dynamické správy (zobrazení dynamické správy) vám umožní zobrazit mnohem podrobněji a také získáte mnohem větší kontrolu nad výsledky dotazu.  Začněte vytvořením toto zobrazení, která bude na ně odkazovat mnoha příkladech v tomto a dalších článků.

```sql
CREATE VIEW dbo.vTableSizes
AS
WITH base
AS
(
SELECT 
 GETDATE()                                                             AS  [execution_time]
, DB_NAME()                                                            AS  [database_name]
, s.name                                                               AS  [schema_name]
, t.name                                                               AS  [table_name]
, QUOTENAME(s.name)+'.'+QUOTENAME(t.name)                              AS  [two_part_name]
, nt.[name]                                                            AS  [node_table_name]
, ROW_NUMBER() OVER(PARTITION BY nt.[name] ORDER BY (SELECT NULL))     AS  [node_table_name_seq]
, tp.[distribution_policy_desc]                                        AS  [distribution_policy_name]
, c.[name]                                                             AS  [distribution_column]
, nt.[distribution_id]                                                 AS  [distribution_id]
, i.[type]                                                             AS  [index_type]
, i.[type_desc]                                                        AS  [index_type_desc]
, nt.[pdw_node_id]                                                     AS  [pdw_node_id]
, pn.[type]                                                            AS  [pdw_node_type]
, pn.[name]                                                            AS  [pdw_node_name]
, di.name                                                              AS  [dist_name]
, di.position                                                          AS  [dist_position]
, nps.[partition_number]                                               AS  [partition_nmbr]
, nps.[reserved_page_count]                                            AS  [reserved_space_page_count]
, nps.[reserved_page_count] - nps.[used_page_count]                    AS  [unused_space_page_count]
, nps.[in_row_data_page_count] 
    + nps.[row_overflow_used_page_count] 
    + nps.[lob_used_page_count]                                        AS  [data_space_page_count]
, nps.[reserved_page_count] 
 - (nps.[reserved_page_count] - nps.[used_page_count]) 
 - ([in_row_data_page_count] 
         + [row_overflow_used_page_count]+[lob_used_page_count])       AS  [index_space_page_count]
, nps.[row_count]                                                      AS  [row_count]
from 
    sys.schemas s
INNER JOIN sys.tables t
    ON s.[schema_id] = t.[schema_id]
INNER JOIN sys.indexes i
    ON  t.[object_id] = i.[object_id]
    AND i.[index_id] <= 1
INNER JOIN sys.pdw_table_distribution_properties tp
    ON t.[object_id] = tp.[object_id]
INNER JOIN sys.pdw_table_mappings tm
    ON t.[object_id] = tm.[object_id]
INNER JOIN sys.pdw_nodes_tables nt
    ON tm.[physical_name] = nt.[name]
INNER JOIN sys.dm_pdw_nodes pn
    ON  nt.[pdw_node_id] = pn.[pdw_node_id]
INNER JOIN sys.pdw_distributions di
    ON  nt.[distribution_id] = di.[distribution_id]
INNER JOIN sys.dm_pdw_nodes_db_partition_stats nps
    ON nt.[object_id] = nps.[object_id]
    AND nt.[pdw_node_id] = nps.[pdw_node_id]
    AND nt.[distribution_id] = nps.[distribution_id]
LEFT OUTER JOIN (select * from sys.pdw_column_distribution_properties where distribution_ordinal = 1) cdp
    ON t.[object_id] = cdp.[object_id]
LEFT OUTER JOIN sys.columns c
    ON cdp.[object_id] = c.[object_id]
    AND cdp.[column_id] = c.[column_id]
)
, size
AS
(
SELECT
   [execution_time]
,  [database_name]
,  [schema_name]
,  [table_name]
,  [two_part_name]
,  [node_table_name]
,  [node_table_name_seq]
,  [distribution_policy_name]
,  [distribution_column]
,  [distribution_id]
,  [index_type]
,  [index_type_desc]
,  [pdw_node_id]
,  [pdw_node_type]
,  [pdw_node_name]
,  [dist_name]
,  [dist_position]
,  [partition_nmbr]
,  [reserved_space_page_count]
,  [unused_space_page_count]
,  [data_space_page_count]
,  [index_space_page_count]
,  [row_count]
,  ([reserved_space_page_count] * 8.0)                                 AS [reserved_space_KB]
,  ([reserved_space_page_count] * 8.0)/1000                            AS [reserved_space_MB]
,  ([reserved_space_page_count] * 8.0)/1000000                         AS [reserved_space_GB]
,  ([reserved_space_page_count] * 8.0)/1000000000                      AS [reserved_space_TB]
,  ([unused_space_page_count]   * 8.0)                                 AS [unused_space_KB]
,  ([unused_space_page_count]   * 8.0)/1000                            AS [unused_space_MB]
,  ([unused_space_page_count]   * 8.0)/1000000                         AS [unused_space_GB]
,  ([unused_space_page_count]   * 8.0)/1000000000                      AS [unused_space_TB]
,  ([data_space_page_count]     * 8.0)                                 AS [data_space_KB]
,  ([data_space_page_count]     * 8.0)/1000                            AS [data_space_MB]
,  ([data_space_page_count]     * 8.0)/1000000                         AS [data_space_GB]
,  ([data_space_page_count]     * 8.0)/1000000000                      AS [data_space_TB]
,  ([index_space_page_count]  * 8.0)                                   AS [index_space_KB]
,  ([index_space_page_count]  * 8.0)/1000                              AS [index_space_MB]
,  ([index_space_page_count]  * 8.0)/1000000                           AS [index_space_GB]
,  ([index_space_page_count]  * 8.0)/1000000000                        AS [index_space_TB]
FROM base
)
SELECT * 
FROM size
;
```

### <a name="table-space-summary"></a>Souhrnné tabulky místa
Tento dotaz vrací řádky a místa v tabulce.  Je skvělým dotazu zobrazíte které tabulky jsou vaše největší tabulky a zda jsou kruhové dotazování, replikovat nebo distribuovat algoritmu hash.  Pro distribuované zatřiďovacích tabulkách také ukazuje distribuční sloupce.  Ve většině případů musí být vaše největší tabulky hash distribuované s clusterovaný index columnstore.

```sql
SELECT 
     database_name
,    schema_name
,    table_name
,    distribution_policy_name
,      distribution_column
,    index_type_desc
,    COUNT(distinct partition_nmbr) as nbr_partitions
,    SUM(row_count)                 as table_row_count
,    SUM(reserved_space_GB)         as table_reserved_space_GB
,    SUM(data_space_GB)             as table_data_space_GB
,    SUM(index_space_GB)            as table_index_space_GB
,    SUM(unused_space_GB)           as table_unused_space_GB
FROM 
    dbo.vTableSizes
GROUP BY 
     database_name
,    schema_name
,    table_name
,    distribution_policy_name
,      distribution_column
,    index_type_desc
ORDER BY
    table_reserved_space_GB desc
;
```

### <a name="table-space-by-distribution-type"></a>Tabulka místo podle typu distribuce
```sql
SELECT 
     distribution_policy_name
,    SUM(row_count)                as table_type_row_count
,    SUM(reserved_space_GB)        as table_type_reserved_space_GB
,    SUM(data_space_GB)            as table_type_data_space_GB
,    SUM(index_space_GB)           as table_type_index_space_GB
,    SUM(unused_space_GB)          as table_type_unused_space_GB
FROM dbo.vTableSizes
GROUP BY distribution_policy_name
;
```

### <a name="table-space-by-index-type"></a>Tabulka místo podle indexu typu
```sql
SELECT 
     index_type_desc
,    SUM(row_count)                as table_type_row_count
,    SUM(reserved_space_GB)        as table_type_reserved_space_GB
,    SUM(data_space_GB)            as table_type_data_space_GB
,    SUM(index_space_GB)           as table_type_index_space_GB
,    SUM(unused_space_GB)          as table_type_unused_space_GB
FROM dbo.vTableSizes
GROUP BY index_type_desc
;
```

### <a name="distribution-space-summary"></a>Souhrn distribuční prostoru
```sql
SELECT 
    distribution_id
,    SUM(row_count)                as total_node_distribution_row_count
,    SUM(reserved_space_MB)        as total_node_distribution_reserved_space_MB
,    SUM(data_space_MB)            as total_node_distribution_data_space_MB
,    SUM(index_space_MB)           as total_node_distribution_index_space_MB
,    SUM(unused_space_MB)          as total_node_distribution_unused_space_MB
FROM dbo.vTableSizes
GROUP BY     distribution_id
ORDER BY    distribution_id
;
```

## <a name="next-steps"></a>Další kroky
Další informace najdete v článcích na [tabulky datové typy][Data Types], [distribuci tabulku][Distribute], [indexování tabulku] [ Index], [Vytváření oddílů tabulky][Partition], [zachování statistiky tabulky] [ Statistics] a [Dočasných tabulek][Temporary].  Další informace o osvědčených postupech najdete v tématu [SQL Data Warehouse osvědčené postupy][SQL Data Warehouse Best Practices].

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
[Load data with Polybase]: ./sql-data-warehouse-load-from-azure-blob-storage-with-polybase.md

<!--MSDN references-->
[CREATE TABLE]: https://msdn.microsoft.com/library/mt203953.aspx
[RENAME]: https://msdn.microsoft.com/library/mt631611.aspx
[DBCC PDW_SHOWSPACEUSED]: https://msdn.microsoft.com/library/mt204028.aspx
[Table Constraints]: https://msdn.microsoft.com/library/ms188066.aspx
[Computed Columns]: https://msdn.microsoft.com/library/ms186241.aspx
[Sparse Columns]: https://msdn.microsoft.com/library/cc280604.aspx
[User-Defined Types]: https://msdn.microsoft.com/library/ms131694.aspx
[Sequence]: https://msdn.microsoft.com/library/ff878091.aspx
[Triggers]: https://msdn.microsoft.com/library/ms189799.aspx
[Indexed Views]: https://msdn.microsoft.com/library/ms191432.aspx
[Synonyms]: https://msdn.microsoft.com/library/ms177544.aspx
[Unique Indexes]: https://msdn.microsoft.com/library/ms188783.aspx

<!--Other Web references-->
