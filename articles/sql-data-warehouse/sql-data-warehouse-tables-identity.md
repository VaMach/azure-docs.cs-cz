---
title: "Vytvoření klíčů náhradní pomocí IDENTITY | Microsoft Docs"
description: "Další informace o použití IDENTITY k vytváření klíčů náhradní na tabulky."
services: sql-data-warehouse
documentationcenter: NA
author: jrowlandjones
manager: jhubbard
editor: 
ms.assetid: faa1034d-314c-4f9d-af81-f5a9aedf33e4
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.date: 06/13/2017
ms.author: jrj;barbkess
ms.openlocfilehash: 3ab5d159e6eaeb830135fe134e108b0e4de4b7d6
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="create-surrogate-keys-by-using-identity"></a>Vytvoření klíčů náhradní pomocí IDENTITY
> [!div class="op_single_selector"]
> * [Přehled][Overview]
> * [Datové typy][Data Types]
> * [Distribuce][Distribute]
> * [Index][Index]
> * [Oddíl][Partition]
> * [Statistiky][Statistics]
> * [Dočasné][Temporary]
> * [Identity][Identity]
> 
> 

Mnoho modelování dat, například k vytváření klíčů náhradní na jejich tabulky při návrhu modely datového skladu. Vlastnost IDENTITY můžete použít k dosažení tohoto cíle jednoduše a efektivně, aniž by to ovlivnilo zatížení výkonu. 

## <a name="get-started-with-identity"></a>Začínáme s identitou
Můžete definovat tabulku tak, že má vlastnost IDENTITY při prvním vytvoření tabulky pomocí syntaxe, který je podobný následující příkaz:

```sql
CREATE TABLE dbo.T1
(   C1 INT IDENTITY(1,1) NOT NULL
,   C2 INT NULL
)
WITH
(   DISTRIBUTION = HASH(C2)
,   CLUSTERED COLUMNSTORE INDEX
)
;
```

Pak můžete použít `INSERT..SELECT` k vyplnění tabulky.

## <a name="behavior"></a>Chování
Vlastnost IDENTITY je určena pro škálovat napříč všechny distribuce v datovém skladu, aniž by to ovlivnilo zatížení výkonu. Implementace IDENTITY je proto orientované na dosažení těchto cílů. V této části jsou zdůrazněné drobné odlišnosti ve implementaci vám pomohou pochopit je podrobněji.  

### <a name="allocation-of-values"></a>Přidělování hodnot
Vlastnost IDENTITY nezaručuje pořadí, ve kterém jsou přiděleny náhradní hodnoty, které odráží chování systému SQL Server a databáze SQL Azure. Ale v Azure SQL Data Warehouse absenci záruku je mnohem výraznější. 

V následujícím příkladu je obrázek:

```sql
CREATE TABLE dbo.T1
(   C1 INT IDENTITY(1,1)    NOT NULL
,   C2 VARCHAR(30)              NULL
)
WITH
(   DISTRIBUTION = HASH(C2)
,   CLUSTERED COLUMNSTORE INDEX
)
;

INSERT INTO dbo.T1
VALUES (NULL);

INSERT INTO dbo.T1
VALUES (NULL);

SELECT *
FROM dbo.T1;

DBCC PDW_SHOWSPACEUSED('dbo.T1');
```

V předchozím příkladu dva řádky dostali distribuční 1. První řádek má náhradní hodnotu 1 ve sloupci `C1`, a druhý řádek obsahuje náhradní hodnota 61. Obě tyto hodnoty byly vygenerovány vlastnost IDENTITY. Přidělení hodnoty však není souvislé. Toto chování je záměrné.

### <a name="skewed-data"></a>Zkreslilo dat 
Rozsah hodnot pro datový typ jsou rovnoměrně rozloženy distribuce. Pokud se vyskytne distribuované tabulku z zkreslilo dat, můžete rozsah hodnot, které jsou k dispozici pro datový typ vyčerpán předčasně. Například pokud všechna data skončilo v jednom distribučním, pak efektivně tabulka má přístup k jedné pouze šedesátině hodnot datového typu. Z tohoto důvodu je omezena na vlastnost IDENTITY `INT` a `BIGINT` datové typy jenom.

### <a name="selectinto"></a>VYBERTE... DO
Pokud do nové tabulky je vybraná jako existující sloupec IDENTITY, nového sloupce, který dědí vlastnost Identita, pokud je splněna jedna z následujících podmínek:
- Příkaz SELECT obsahuje spojení.
- Více příkazů SELECT připojeni pomocí UNION.
- Sloupec IDENTITY je uveden více než jednou v seznamu SELECT.
- Sloupec IDENTITY je součástí výrazu.
    
Pokud platí jedna z těchto podmínek, vytvoří se sloupci NOT NULL místo dědí vlastnost Identita.

### <a name="create-table-as-select"></a>VYTVOŘENÍ TABLE AS SELECT
Vytvoření tabulky AS vyberte funkce CTAS () odpovídá stejné chování systému SQL Server, která je popsána vyberte... DO. V definici sloupce však nelze určit vlastnost IDENTITY `CREATE TABLE` část příkazu. Funkci IDENTITY v také nelze použít `SELECT` součástí funkce CTAS. Vyplnění tabulky, budete muset použít `CREATE TABLE` definovat v tabulce, za nímž následuje `INSERT..SELECT` k jejímu naplnění.

## <a name="explicitly-insert-values-into-an-identity-column"></a>Explicitně vložení hodnoty do sloupce IDENTITY 
SQL Data Warehouse podporuje `SET IDENTITY_INSERT <your table> ON|OFF` syntaxe. Pomocí této syntaxe explicitně vložení hodnoty do sloupce IDENTITY.

Mnoho modelování dat se chtěli použít předdefinované záporné hodnoty pro některé řádky v jejich dimenzí. Příkladem je "neznámého člena" řádek nebo hodnotu -1. 

Další skriptu ukazuje, jak chcete explicitně přidat pomocí nastavení IDENTITY_INSERT tento řádek:

```sql
SET IDENTITY_INSERT dbo.T1 ON;

INSERT INTO dbo.T1
(   C1
,   C2
)
VALUES (-1,'UNKNOWN')
;

SET IDENTITY_INSERT dbo.T1 OFF;

SELECT  *
FROM    dbo.T1
;
```    

## <a name="load-data-into-a-table-with-identity"></a>Načtení dat do tabulky s identitou

Přítomnost vlastnost IDENTITY má některé důsledky načítání dat kódu. V této části jsou zdůrazněné některé základní vzory pro načítání dat do tabulky pomocí IDENTITY. 

### <a name="load-data-with-polybase"></a>Načítání dat pomocí PolyBase
Načtení dat do tabulky a vygenerovat klíč náhradní pomocí IDENTITY, vytvořit v tabulce a pak použít příkaz INSERT... Vyberte nebo VLOŽTE... HODNOTY k provedení zatížení.

V následujícím příkladu jsou uvedeny základní vzor:
 
```sql
--CREATE TABLE with IDENTITY
CREATE TABLE dbo.T1
(   C1 INT IDENTITY(1,1)
,   C2 VARCHAR(30)
)
WITH
(   DISTRIBUTION = HASH(C2)
,   CLUSTERED COLUMNSTORE INDEX
)
;

--Use INSERT..SELECT to populate the table from an external table
INSERT INTO dbo.T1
(C2)
SELECT  C2
FROM    ext.T1
;

SELECT  *
FROM    dbo.T1
;

DBCC PDW_SHOWSPACEUSED('dbo.T1');
```

> [!NOTE] 
> Není možné použít `CREATE TABLE AS SELECT` aktuálně při načítání dat do tabulky se sloupcem IDENTITY.
> 

Další informace o načítání dat pomocí nástroje hromadné kopírování (BCP) programu najdete v následujících článcích:

- [Načíst pomocí funkce PolyBase][]
- [PolyBase osvědčené postupy][]

### <a name="load-data-with-bcp"></a>Načítání dat pomocí BCP
BCP je nástroj příkazového řádku, který slouží k načtení dat do SQL Data Warehouse. Jeden z jeho parametry (-E) řídí chování BCP při načítání dat do tabulky se sloupcem IDENTITY. 

Pokud je zadána -E, se zachovají hodnoty uchovávat ve vstupním souboru pro sloupec s identitou. Pokud je -E *není* určeno, hodnoty v tomto sloupci se ignorují. Pokud sloupec identity neuvedete, je načíst normální data. Hodnoty jsou generovány podle zásad přírůstek a počáteční hodnoty vlastnosti.

Další informace o načítání dat pomocí BCP najdete v následujících článcích:

- [Načíst pomocí BCP][]
- [BCP v MSDN][]

## <a name="catalog-views"></a>Zobrazení katalogu
Podporuje SQL Data Warehouse `sys.identity_columns` katalogu zobrazení. Toto zobrazení můžete použít k identifikaci sloupec, který má vlastnost IDENTITY.

Vám pomůžou lépe pochopit schématu databáze, tento příklad ukazuje, jak integrovat `sys.identity_columns` s dalšími zobrazeními katalogu systému:

```sql
SELECT  sm.name
,       tb.name
,       co.name
,       CASE WHEN ic.column_id IS NOT NULL
             THEN 1
        ELSE 0
        END AS is_identity 
FROM        sys.schemas AS sm
JOIN        sys.tables  AS tb           ON  sm.schema_id = tb.schema_id
JOIN        sys.columns AS co           ON  tb.object_id = co.object_id
LEFT JOIN   sys.identity_columns AS ic  ON  co.object_id = ic.object_id
                                        AND co.column_id = ic.column_id
WHERE   sm.name = 'dbo'
AND     tb.name = 'T1'
;
```

## <a name="limitations"></a>Omezení
Vlastnost IDENTITY nelze použít v následujících scénářích:
- Kde je datový typ sloupce není INT nebo BIGINT
- Kde sloupec je také distribučního klíče
- Kde je v tabulce externí tabulky 

Následující související funkce nejsou podporovány v SQL Data Warehouse:

- [IDENTITY()][]
- [@@IDENTITY][]
- [SCOPE_IDENTITY][]
- [FUNKCE IDENT_CURRENT][]
- [IDENT_INCR][]
- [IDENT_SEED][]
- [PŘÍKAZ DBCC CHECK_IDENT()][]

## <a name="tasks"></a>Úlohy

Tato část obsahuje některé ukázkový kód, který slouží k provádění běžných úloh při práci s sloupců IDENTITY.

> [!NOTE] 
> Sloupec C1 je identita v následující úlohy.
> 
 
### <a name="find-the-highest-allocated-value-for-a-table"></a>Vyhledá nejvyšší hodnotu přidělené pro tabulku
Použití `MAX()` funkce lze určit nejvyšší hodnotu přidělené pro distribuované tabulku:

```sql
SELECT  MAX(C1)
FROM    dbo.T1
``` 

### <a name="find-the-seed-and-increment-for-the-identity-property"></a>Vyhledejte počáteční hodnoty a přírůstku pro vlastnost IDENTITY
Zobrazení katalogu můžete použít ke zjištění hodnoty identity přírůstek a počáteční konfigurace pro tabulku s použitím následujícího dotazu: 

```sql
SELECT  sm.name
,       tb.name
,       co.name
,       ic.seed_value
,       ic.increment_value 
FROM        sys.schemas AS sm
JOIN        sys.tables  AS tb           ON  sm.schema_id = tb.schema_id
JOIN        sys.columns AS co           ON  tb.object_id = co.object_id
JOIN        sys.identity_columns AS ic  ON  co.object_id = ic.object_id
                                        AND co.column_id = ic.column_id
WHERE   sm.name = 'dbo'
AND     tb.name = 'T1'
;
```

## <a name="next-steps"></a>Další kroky

* Další informace o vývoji tabulky najdete v tématu [tabulky přehled][Overview], [tabulky datové typy][Data Types], [distribuovat tabulku] [ Distribute], [Indexu tabulky][Index], [oddílu tabulky][Partition], a [ Dočasné tabulky][Temporary]. 
* Další informace o osvědčených postupech najdete v tématu [osvědčené postupy pro SQL Data Warehouse][SQL Data Warehouse Best Practices].  

<!--Image references-->

<!--Article references-->
[Overview]: ./sql-data-warehouse-tables-overview.md
[Data Types]: ./sql-data-warehouse-tables-data-types.md
[Distribute]: ./sql-data-warehouse-tables-distribute.md
[Index]: ./sql-data-warehouse-tables-index.md
[Partition]: ./sql-data-warehouse-tables-partition.md
[Statistics]: ./sql-data-warehouse-tables-statistics.md
[Temporary]: ./sql-data-warehouse-tables-temporary.md
[Identity]: ./sql-data-warehouse-tables-identity.md
[SQL Data Warehouse Best Practices]: ./sql-data-warehouse-best-practices.md

[Načíst pomocí bcp]: https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-load-with-bcp/
[Načíst pomocí funkce PolyBase]: https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-load-from-azure-blob-storage-with-polybase/
[PolyBase osvědčené postupy]: https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-load-polybase-guide/


<!--MSDN references-->
[Identity property]: https://msdn.microsoft.com/library/ms186775.aspx
[sys.identity_columns]: https://msdn.microsoft.com/library/ms187334.aspx
[IDENTITY()]: https://msdn.microsoft.com/library/ms189838.aspx
[@@IDENTITY]: https://msdn.microsoft.com/library/ms187342.aspx
[SCOPE_IDENTITY]: https://msdn.microsoft.com/library/ms190315.aspx
[FUNKCE IDENT_CURRENT]: https://msdn.microsoft.com/library/ms175098.aspx
[IDENT_INCR]: https://msdn.microsoft.com/library/ms189795.aspx
[IDENT_SEED]: https://msdn.microsoft.com/library/ms189834.aspx
[PŘÍKAZ DBCC CHECK_IDENT()]: https://msdn.microsoft.com/library/ms176057.aspx

[BCP v MSDN]: https://msdn.microsoft.com/library/ms162802.aspx
  

<!--Other Web references-->  
