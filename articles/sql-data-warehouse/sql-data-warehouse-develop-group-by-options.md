---
title: "Seskupit podle možností v SQL Data Warehouse | Microsoft Docs"
description: "Tipy pro implementaci skupiny pomocí možnosti v Azure SQL Data Warehouse na vývoj řešení."
services: sql-data-warehouse
documentationcenter: NA
author: jrowlandjones
manager: jhubbard
editor: 
ms.assetid: f95a1e43-768f-4b7b-8a10-8a0509d0c871
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: queries
ms.date: 10/31/2016
ms.author: jrj;barbkess
ms.openlocfilehash: da71cb834c13da5d0f5690f471efc6c696163f30
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="group-by-options-in-sql-data-warehouse"></a>Seskupit podle možností v SQL Data Warehouse
[GROUP BY] [ GROUP BY] klauzule slouží ke shromáždění dat pro souhrn sadu řádků. Je také několik možností, které rozšiřují jeho funkce, které musí být práce na incidentu kolem jako nejsou podporovány přímo pomocí Azure SQL Data Warehouse.

Tyto možnosti jsou

* GROUP BY se ZAHRNUTÍM
* GROUPING SETS
* Klauzule GROUP BY datové KRYCHLE

## <a name="rollup-and-grouping-sets-options"></a>Souhrn a seskupení nastaví možnosti
Nejjednodušší možnost je použít `UNION ALL` místo toho provést kumulativní z nespoléhá se na explicitní syntaxe. Výsledkem je přesně stejný

Dole je příklad skupiny pomocí příkazu `ROLLUP` možnost:

```sql
SELECT [SalesTerritoryCountry]
,      [SalesTerritoryRegion]
,      SUM(SalesAmount)             AS TotalSalesAmount
FROM  dbo.factInternetSales s
JOIN  dbo.DimSalesTerritory t       ON s.SalesTerritoryKey       = t.SalesTerritoryKey
GROUP BY ROLLUP (
                        [SalesTerritoryCountry]
                ,       [SalesTerritoryRegion]
                )
;
```

Pomocí KUMULATIVNÍ jsme odeslali žádost o následující agregace:

* Zemí a oblastí
* Země
* Celkový součet

K nahraďte ho budete muset použít `UNION ALL`; určení agregace explicitně potřeba vrátí stejné výsledky:

```sql
SELECT [SalesTerritoryCountry]
,      [SalesTerritoryRegion]
,      SUM(SalesAmount) AS TotalSalesAmount
FROM  dbo.factInternetSales s
JOIN  dbo.DimSalesTerritory t     ON s.SalesTerritoryKey       = t.SalesTerritoryKey
GROUP BY
       [SalesTerritoryCountry]
,      [SalesTerritoryRegion]
UNION ALL
SELECT [SalesTerritoryCountry]
,      NULL
,      SUM(SalesAmount) AS TotalSalesAmount
FROM  dbo.factInternetSales s
JOIN  dbo.DimSalesTerritory t     ON s.SalesTerritoryKey       = t.SalesTerritoryKey
GROUP BY
       [SalesTerritoryCountry]
UNION ALL
SELECT NULL
,      NULL
,      SUM(SalesAmount) AS TotalSalesAmount
FROM  dbo.factInternetSales s
JOIN  dbo.DimSalesTerritory t     ON s.SalesTerritoryKey       = t.SalesTerritoryKey;
```

Pro všechny GROUPING SETS budeme muset udělat, je použít stejný objekt zabezpečení, ale vytvořit pouze UNION ALL oddíly pro úrovně agregace, které chcete najdete v části

## <a name="cube-options"></a>Možnosti datové krychle
Je možné vytvořit SKUPINU podle s datovou KRYCHLI UNION ALL přístup. Problém je, že kód může být pracné a nepraktické. Toto riziko lze snížit můžete použít tento pokročilejší přístup.

Použijeme v předchozím příkladu.

Prvním krokem je definování 'datové krychle, která definuje všechny úrovně agregace, který chcete vytvořit. Je důležité si poznamenejte CROSS JOIN dvě odvozené tabulky. Tím se vytvoří všechny úrovně pro nás. Zbytek kód skutečně existuje pro formátování.

```sql
CREATE TABLE #Cube
WITH
(   DISTRIBUTION = ROUND_ROBIN
,   LOCATION = USER_DB
)
AS
WITH GrpCube AS
(SELECT    CAST(ISNULL(Country,'NULL')+','+ISNULL(Region,'NULL') AS NVARCHAR(50)) as 'Cols'
,          CAST(ISNULL(Country+',','')+ISNULL(Region,'') AS NVARCHAR(50))  as 'GroupBy'
,          ROW_NUMBER() OVER (ORDER BY Country) as 'Seq'
FROM       ( SELECT 'SalesTerritoryCountry' as Country
             UNION ALL
             SELECT NULL
           ) c
CROSS JOIN ( SELECT 'SalesTerritoryRegion' as Region
             UNION ALL
             SELECT NULL
           ) r
)
SELECT Cols
,      CASE WHEN SUBSTRING(GroupBy,LEN(GroupBy),1) = ','
            THEN SUBSTRING(GroupBy,1,LEN(GroupBy)-1)
            ELSE GroupBy
       END AS GroupBy  --Remove Trailing Comma
,Seq
FROM GrpCube;
```

Výsledky funkce CTAS si můžete prohlédnout níže:

![][1]

Druhým krokem je zadání cílové tabulku pro ukládání dočasné výsledky:

```sql
DECLARE
 @SQL NVARCHAR(4000)
,@Columns NVARCHAR(4000)
,@GroupBy NVARCHAR(4000)
,@i INT = 1
,@nbr INT = 0
;
CREATE TABLE #Results
(
 [SalesTerritoryCountry] NVARCHAR(50)
,[SalesTerritoryRegion]  NVARCHAR(50)
,[TotalSalesAmount]      MONEY
)
WITH
(   DISTRIBUTION = ROUND_ROBIN
,   LOCATION = USER_DB
)
;
```

Třetí krok je smyčku naší datové krychle sloupců provádění agregace. Budou spuštěny jednou pro každý řádek v dočasné tabulce #Cube a ukládání výsledků do dočasné tabulky #Results dotazu

```sql
SET @nbr =(SELECT MAX(Seq) FROM #Cube);

WHILE @i<=@nbr
BEGIN
    SET @Columns = (SELECT Cols    FROM #Cube where seq = @i);
    SET @GroupBy = (SELECT GroupBy FROM #Cube where seq = @i);

    SET @SQL ='INSERT INTO #Results
              SELECT '+@Columns+'
              ,      SUM(SalesAmount) AS TotalSalesAmount
              FROM  dbo.factInternetSales s
              JOIN  dbo.DimSalesTerritory t  
              ON s.SalesTerritoryKey = t.SalesTerritoryKey
              '+CASE WHEN @GroupBy <>''
                     THEN 'GROUP BY '+@GroupBy ELSE '' END

    EXEC sp_executesql @SQL;
    SET @i +=1;
END
```

Nakonec se vrací výsledky načtením jednoduše z dočasné tabulky #Results

```sql
SELECT *
FROM #Results
ORDER BY 1,2,3
;
```

Kód rozdělení do oddílů a generování opakování konstrukce kód stane lepší spravovat a údržba.

## <a name="next-steps"></a>Další kroky
Další tipy pro vývoj, najdete v části [přehled vývoje][development overview].

<!--Image references-->
[1]: media/sql-data-warehouse-develop-group-by-options/sql-data-warehouse-develop-group-by-cube.png

<!--Article references-->
[development overview]: sql-data-warehouse-overview-develop.md

<!--MSDN references-->
[GROUP BY]: https://msdn.microsoft.com/library/ms177673.aspx


<!--Other Web references-->
