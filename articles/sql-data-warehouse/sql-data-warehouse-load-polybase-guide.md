---
title: "Příručka pro používání funkce PolyBase v SQL Data Warehouse | Microsoft Docs"
description: "Pokyny a doporučení pro používání funkce PolyBase v SQL Data Warehouse scénáře."
services: sql-data-warehouse
documentationcenter: NA
author: ckarst
manager: barbkess
editor: 
ms.assetid: 4757fce1-96b3-48ea-8a51-be1385705f9f
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.date: 9/13/2017
ms.custom: loading
ms.author: cakarst;barbkess
ms.openlocfilehash: e8ae0eb96200c167a8758df4ce20b51452cc59a4
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="guide-for-using-polybase-in-sql-data-warehouse"></a>Příručka pro používání funkce PolyBase v SQL Data Warehouse
Tato příručka poskytuje praktické informace pro používání funkce PolyBase v SQL Data Warehouse.

Abyste mohli začít, najdete v článku [načtení dat pomocí funkce PolyBase] [ Load data with PolyBase] kurzu.

## <a name="rotating-storage-keys"></a>Rotaci klíčů k úložišti
Čas od času budete chtít změnit přístupový klíč do úložiště objektů blob z bezpečnostních důvodů.

Většina elegantní způsob k provedení této úlohy je postupujte podle tento proces se označuje jako "rotaci klíčů". Jste si všimli, že máte dva klíče úložiště pro váš účet úložiště objektů blob. Toto je tak, aby můžete přejít

Otáčení klíče účtu úložiště Azure je jednoduchý třech krocích

1. Vytvoření druhého oboru databáze pověření podle přístupového klíče sekundární úložiště.
2. Vytvořte druhý externí zdroj dat založena na této nových přihlašovacích údajů
3. Vyřaďte a vytvořte externí tabulky či tabulek odkazující na nový zdroj externích dat

Pokud jste migrovali všechny externí tabulky na nový zdroj externích dat a poté můžete provádět úlohy čištění:

1. Vyřaďte první externí zdroj dat
2. První vyřaďte databázi vašich přihlašovacích údajů podle přístupový klíč primárního úložiště
3. Přihlaste se k Azure a znovu vygenerovat primární přístupový klíč připravené pro příští



## <a name="load-data-with-external-tables"></a>Načtení dat pomocí externí tabulky
Tento příklad načte data z Azure blob storage do SQL Data Warehouse databáze.

Ukládání dat přímo odebere doba přenosu dat pro dotazy. Ukládání dat s indexem columnstore zlepšuje výkon dotazů pro analýzu dotazy pomocí až 10 x.

Tento příklad používá příkaz CREATE TABLE AS SELECT načíst data. Nová tabulka dědí sloupce pojmenované v dotazu. Datové typy tyto sloupce dědí z definici externí tabulky.

CREATE TABLE AS SELECT je vysoce původce příkazu Transact-SQL, který načte data souběžně na výpočetní uzly z SQL Data Warehouse.  To byl původně vytvořen pro modul (MPP) massively parallel processing v Analytics Platform System a je nyní v SQL Data Warehouse.

```sql
-- Load data from Azure blob storage to SQL Data Warehouse

CREATE TABLE [dbo].[Customer_Speed]
WITH
(   
    CLUSTERED COLUMNSTORE INDEX
,    DISTRIBUTION = HASH([CarSensor_Data].[CustomerKey])
)
AS
SELECT *
FROM   [ext].[CarSensor_Data]
;
```

V tématu [CREATE TABLE AS SELECT (Transact-SQL)][CREATE TABLE AS SELECT (Transact-SQL)].

> [!NOTE]
> Zatížení pomocí externí tabulku může selhat s chybou *"dotaz byl zrušen--při čtení z externího zdroje bylo dosaženo maximální odmítněte prahovou hodnotu"*. To znamená, že externích dat obsahuje *nekonzistence* záznamy. Záznam dat se považuje za 'nekonzistence' Pokud skutečná data typy nebo počet sloupců neodpovídají žádné definice sloupců externí tabulky, nebo pokud data neodpovídají zadaný externí soubor formátu. Chcete-li odstranit tento problém, zajistěte, aby externí tabulky a definice formátu externího souboru jsou správné a externích dat odpovídá tyto definice. V případě, že podmnožinu záznamů externích dat jsou chybná, můžete tak, aby zamítal tyto záznamy pro své dotazy pomocí možnosti odmítněte ve vytvoření externí tabulky DDL.
> 
> 


## <a name="create-statistics-on-newly-loaded-data"></a>Vytvoření statistiky pro nově načtená data
Azure SQL Data Warehouse zatím nepodporuje automatické vytváření ani automatickou aktualizaci statistik.  Aby vám dotazy vracely co nejlepší výsledky, je důležité, aby se statistiky vytvořily pro všechny sloupce všech tabulek po prvním načtením nebo kdykoli, kdy v datech dojde k podstatným změnám.  Podrobné vysvětlení statistiky najdete v tématu [Statistika][Statistics] ve skupině témat věnovaných vývoji.  Níže je zběžný příklad vytvoření statistik pro tabulku načtenou v tomto příkladě.

```sql
create statistics [SensorKey] on [Customer_Speed] ([SensorKey]);
create statistics [CustomerKey] on [Customer_Speed] ([CustomerKey]);
create statistics [GeographyKey] on [Customer_Speed] ([GeographyKey]);
create statistics [Speed] on [Customer_Speed] ([Speed]);
create statistics [YearMeasured] on [Customer_Speed] ([YearMeasured]);
```

## <a name="export-data-with-external-tables"></a>Export dat s externí tabulky
V této části ukazuje, jak exportovat data z SQL Data Warehouse do Azure blob storage pomocí externí tabulky. Tento příklad používá vytvořit externí TABLE AS SELECT, který je vysoce původce příkazu Transact-SQL k exportu dat paralelně z výpočetních uzlů.

Následující příklad vytvoří externí tabulku Weblogs2014 pomocí definice sloupců a data z dbo. Tabulka Weblogů. Definici externí tabulky je uložená v SQL Data Warehouse a k adresáři "/ archivaci/log2014 /" v kontejneru objektů blob zadaný zdroj dat exportují se výsledky příkazu SELECT. Data se exportují ve formátu zadaný text.

```sql
CREATE EXTERNAL TABLE Weblogs2014 WITH
(
    LOCATION='/archive/log2014/',
    DATA_SOURCE=azure_storage,
    FILE_FORMAT=text_file_format
)
AS
SELECT
    Uri,
    DateRequested
FROM
    dbo.Weblogs
WHERE
    1=1
    AND DateRequested > '12/31/2013'
    AND DateRequested < '01/01/2015';
```
## <a name="isolate-loading-users"></a>Izolovat načítání uživatelů
Je často potřeba mít více uživatelů, které můžete načíst data do datového skladu SQL. Protože [CREATE TABLE AS SELECT (Transact-SQL)] [ CREATE TABLE AS SELECT (Transact-SQL)] vyžaduje oprávnění pro řízení databáze, budete mít s více uživateli pomocí řízení přístupu přes všechny schémat. Pokud chcete omezit to, můžete použít příkaz ODEPŘÍT ovládacího PRVKU.

Příklad: Vzít schema_A schémat databáze pro oddělení A a schema_B pro oddělení B umožní user_A databáze uživatelů a user_B uživatelů pro PolyBase načtení z oddělení A a B, v uvedeném pořadí. Oba mají udělené oprávnění pro řízení databáze.
Tvůrci zámek schématu A a B nyní dolů jejich schémata pomocí ODEPŘÍT:

```sql
   DENY CONTROL ON SCHEMA :: schema_A TO user_B;
   DENY CONTROL ON SCHEMA :: schema_B TO user_A;
```   
 Pomocí tohoto user_A a user_B by měl nyní uzamčen z jiných oddělení schématu.
 
## <a name="polybase-performance-optimizations"></a>Optimalizace výkonu PolyBase
K dosažení optimálního výkonu pomocí funkce PolyBase načítání doporučujeme následující:
- Rozdělením velkých komprimované soubory menší komprimované soubory. Typy komprese podporované dnes nejsou rozdělitelné. V důsledku toho bude ovlivněn výkon načtením jeden velký soubor.
- Pro nejrychlejší rychlost načítání zaveďte do round_robin, haldy pracovní tabulky. Bude jím nejúčinnější způsob pro přesun dat z vrstvy úložiště do datového skladu.
- Všechny formáty souborů mít jiný výkonové charakteristiky. Použití komprimované nejrychlejší zátěže, oddělený textových souborů. Rozdíl mezi UTF-8 a UTF-16 výkonu je minimální.
- Společně umísťovat vrstvě úložiště a váš datový sklad pro zajištění minimální latence
- Vertikální navýšení kapacity datového skladu očekáváte velké načtení úlohy.

## <a name="polybase-limitations"></a>PolyBase omezení
Používání funkce PolyBase v SQL DW má následující omezení, které je potřeba vzít v úvahu při navrhování úlohu načítání:
- Jeden řádek nemůže být větší než 1 000 000 bajtů. To platí bez ohledu na to definované schéma tabulky.
- Při exportu dat do formátu ORC ze systému SQL Server nebo Azure SQL Data Warehouse velkou sloupců text může být omezen na pouhými 50 sloupců z důvodu java mimo chybami paměti. Chcete-li tento problém obejít, exportujte pouze podmnožinu sloupců.





## <a name="next-steps"></a>Další kroky
Další informace o přesun dat do SQL Data Warehouse, najdete v článku [Přehled migrace dat][data migration overview].

<!--Image references-->

<!--Article references-->
[Load data with bcp]: ./sql-data-warehouse-load-with-bcp.md
[Load data with PolyBase]: ./sql-data-warehouse-get-started-load-with-polybase.md
[Statistics]: ./sql-data-warehouse-tables-statistics.md
[data migration overview]: ./sql-data-warehouse-overview-migrate.md

<!--MSDN references-->
[supported source/sink]: https://msdn.microsoft.com/library/dn894007.aspx
[copy activity]: https://msdn.microsoft.com/library/dn835035.aspx
[SQL Server destination adapter]: https://msdn.microsoft.com/library/ms141095.aspx
[SSIS]: https://msdn.microsoft.com/library/ms141026.aspx

[CREATE EXTERNAL DATA SOURCE (Transact-SQL)]: https://msdn.microsoft.com/library/dn935022.aspx
[CREATE EXTERNAL FILE FORMAT (Transact-SQL)]: https://msdn.microsoft.com/library/dn935026.aspx
[CREATE EXTERNAL TABLE (Transact-SQL)]: https://msdn.microsoft.com/library/dn935021.aspx

[DROP EXTERNAL DATA SOURCE (Transact-SQL)]: https://msdn.microsoft.com/library/mt146367.aspx
[DROP EXTERNAL FILE FORMAT (Transact-SQL)]: https://msdn.microsoft.com/library/mt146379.aspx
[DROP EXTERNAL TABLE (Transact-SQL)]: https://msdn.microsoft.com/library/mt130698.aspx

[CREATE TABLE AS SELECT (Transact-SQL)]: https://msdn.microsoft.com/library/mt204041.aspx
[INSERT...SELECT (Transact-SQL)]: https://msdn.microsoft.com/library/ms174335.aspx
[CREATE MASTER KEY (Transact-SQL)]: https://msdn.microsoft.com/library/ms174382.aspx
[CREATE CREDENTIAL (Transact-SQL)]: https://msdn.microsoft.com/library/ms189522.aspx
[CREATE DATABASE SCOPED CREDENTIAL (Transact-SQL)]: https://msdn.microsoft.com/library/mt270260.aspx
[DROP CREDENTIAL (Transact-SQL)]: https://msdn.microsoft.com/library/ms189450.aspx

<!-- External Links -->
