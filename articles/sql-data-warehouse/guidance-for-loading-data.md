---
title: "Doprovodné materiály k načítání dat – Azure SQL Data Warehouse | Dokumentace Microsoftu"
description: "Doporučení pro načítání dat a provádění ELT pomocí služby Azure SQL Data Warehouse."
services: sql-data-warehouse
documentationcenter: NA
author: barbkess
manager: jenniehubbard
editor: 
ms.assetid: 7b698cad-b152-4d33-97f5-5155dfa60f79
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: performance
ms.date: 12/13/2017
ms.author: barbkess
ms.openlocfilehash: 8903be1361d1574a5d81b69223f608ccb7a698ea
ms.sourcegitcommit: fa28ca091317eba4e55cef17766e72475bdd4c96
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/14/2017
---
# <a name="guidance-for-loading-data-into-azure-sql-data-warehouse"></a>Doprovodné materiály k načítání dat do služby Azure SQL Data Warehouse
Doporučení a optimalizace výkonu pro načítání dat do služby Azure SQL Data Warehouse. 

- Další informace o PolyBase a návrhu procesu ELT (extrakce, načítání a transformace) najdete v tématu [Návrh ELT pro službu SQL Data Warehouse](design-elt-data-loading.md).
- Kurz načítání najdete v tématu [Použití PolyBase k načítání dat z úložiště objektů blob v Azure do služby Azure SQL Data Warehouse](load-data-from-azure-blob-storage-using-polybase.md).


## <a name="extract-source-data"></a>Extrakce zdrojových dat

Při exportování dat do formátu souboru ORC z SQL Serveru nebo služby Azure SQL Data Warehouse může být kvůli chybám Javy nebo paměti omezený počet sloupců obsahujících hodně textu až na 50 sloupců. Alternativním řešením je exportovat pouze podmnožinu sloupců.


## <a name="land-data-to-azure"></a>Příjem dat v Azure
PolyBase nedokáže načíst řádky, které obsahují více než milion bajtů dat. Když vkládáte data do textových souborů v úložišti objektů blob v Azure nebo ve službě Azure Data Lake Store, musí tyto soubory obsahovat méně než milion bajtů dat. To platí bez ohledu na definované schéma tabulky.

Umístěte vrstvu úložiště a datový sklad do stejného umístění, abyste minimalizovali latenci.

## <a name="data-preparation"></a>Příprava dat

Všechny formáty souborů mají jiné výkonové charakteristiky. Pokud chcete docílit nejrychlejšího načtení, použijte komprimované textové soubory s oddělovači. Rozdíl mezi výkonem kódování UTF-8 a UTF-16 je minimální.

Velké komprimované soubory rozdělte do menších komprimovaných souborů.

## <a name="create-designated-loading-users"></a>Vytvoření vyhrazených uživatelů načítání
Pokud chcete spouštět načítání s odpovídajícími výpočetními prostředky, vytvořte uživatele načítání vyhrazené pro spouštění načítání. Každého uživatele načítání přiřaďte ke konkrétní třídě prostředků. Pokud chcete spustit načítání, přihlaste se jako jeden z uživatelů načítání a pak spusťte načítání. Načítání se spustí s využitím třídy prostředků tohoto uživatele.  Tato metoda je jednodušší než se pokoušet o změnu třídy prostředků uživatele podle aktuálních potřeb třídy prostředků.

Tento kód vytvoří uživatele načítání pro třídu prostředků staticrc20. Udělí uživateli oprávnění řídit databázi a pak přidá uživatele jako člena databázové role staticrc20. Pokud chcete spustit načítání s prostředky pro třídy prostředků staticRC20, stačí se přihlásit jako LoaderRC20 a spustit načítání. 

    ```sql
    CREATE LOGIN LoaderRC20 WITH PASSWORD = 'a123STRONGpassword!';
    CREATE USER LoaderRC20 FOR LOGIN LoaderRC20;
    GRANT CONTROL ON DATABASE::[mySampleDataWarehouse] to LoaderRC20;
    EXEC sp_addrolemember 'staticrc20', 'LoaderRC20';
    ```

Spouštějte načítání v rámci statických, a ne dynamických, tříd prostředků. Použití statických tříd prostředků zaručuje stejné prostředky bez ohledu na vaši [úroveň služby](performance-tiers.md#service-levels). Pokud použijete dynamickou třídu prostředků, budou se prostředky lišit v závislosti na vaší úrovni služby. V případě dynamických tříd znamená nižší úroveň služby, že pro vašeho uživatele načítání pravděpodobně musíte použít větší třídu prostředků.

### <a name="example-for-isolating-loading-users"></a>Příklad izolace uživatelů načítání

Často je potřeba mít několik uživatelů, kteří můžou načítat data do služby SQL Data Warehouse. Vzhledem k tomu, že příkaz [CREATE TABLE AS SELECT (Transact-SQL)][CREATE TABLE AS SELECT (Transact-SQL)] vyžaduje k databázi oprávnění CONTROL, skončíte s několika uživateli, kteří budou mít přístup k řízení ke všem schématům. Pokud to chcete omezit, můžete použít příkaz DENY CONTROL.

Představte si například schémata databáze schema_A pro oddělení A a schema_B pro oddělení B. Uživatelé databáze user_A a user_B budou uživateli pro načítání PolyBase v oddělení A, respektive oddělení B. Oba uživatelé mají k databázi udělená oprávnění CONTROL. Autoři schémat A a B nyní svá schémata uzamknou pomocí příkazu DENY:

```sql
   DENY CONTROL ON SCHEMA :: schema_A TO user_B;
   DENY CONTROL ON SCHEMA :: schema_B TO user_A;
```   

Uživatelé user_A a user_B jsou nyní vyloučení ze schématu příslušného druhého oddělení.


## <a name="load-to-a-staging-table"></a>Načítání do pracovní tabulky

Pokud chcete dosáhnout nejvyšší rychlosti, načítejte data do pracovní tabulky haldy round_robin. To bude nejúčinnější způsob pro přesun dat z vrstvy Azure Storage do služby SQL Data Warehouse.

Pokud očekáváte velkou úlohu načítání, navyšte vertikálně kapacitu vašeho datového skladu.

Pro dosažení optimálního výkonu načítání spouštějte najednou pouze jednu úlohu načtení.

### <a name="optimize-columnstore-index-loads"></a>Optimalizace načítání indexů columnstore

Indexy columnstore vyžadují hodně paměti, aby mohly komprimovat data do vysoce kvalitních skupin řádků. Pro zajištění nejvyšší efektivity komprese a indexování musí index columnstore do každé skupiny řádků zkomprimovat 1 048 576 řádků. To je maximální počet řádků na skupinu řádků. V případě nedostatku paměti nemusí index columnstore dosahovat maximální míry komprese. To zase ovlivňuje výkon dotazů. Podrobné informace najdete v tématu [Optimalizace paměti pro columnstore](sql-data-warehouse-memory-optimizations-for-columnstore-compression.md).

- Pokud chcete zajistit, aby měl nahrávající uživatel dostatek paměti pro dosažení maximální míry komprese, použijte uživatele načítání, kteří jsou členy střední nebo velké třídy prostředků. 
- Načtěte dostatek dat pro úplně naplnění nových skupin řádků. Při hromadném načítání jde každých 1 048 576 řádků přímo do columnstore. Při načítání s méně než 102 400 řádky se řádky odesílají do indexu deltastore, který řádky uchovává v clusterovaném indexu, dokud jich nebude dostatek pro kompresi. Pokud načtete příliš málo řádků, můžou se všechny dostat do indexu deltastore, a nebudou se okamžitě komprimovat do formátu columnstore.


### <a name="handling-loading-failures"></a>Zpracování chyb načítání

Načtení s použitím externí tabulky může selhat s chybou *Query aborted-- the maximum reject threshold was reached while reading from an external source* (Dotaz byl přerušen – při čtení z externího zdroje byla dosažena maximální prahová hodnota pro odmítnutí). To značí, že vaše externí data obsahují *nezapsané* záznamy. Datový záznam se považuje za nezapsaný, pokud se skutečné typy dat nebo čísla sloupců neshodují s definicemi sloupců externí tabulky nebo pokud data neodpovídají zadanému formátu externího souboru. 

Pokud chcete tento problém odstranit, ujistěte se, že jsou správné definice formátů externí tabulky a externího souboru, a že vaše data odpovídají těmto definicím. V případě, že je nezapsaná podmnožina externích datových záznamů, můžete se rozhodnout ve svých dotazech tyto záznamy odmítnout s využitím možnosti odmítnutí v příkazu CREATE EXTERNAL TABLE DDL.



## <a name="insert-data-into-production-table"></a>Vložení dat do produkční tabulky
Tady najdete doporučení pro vkládání řádků do produkčních tabulek.


### <a name="batch-insert-statements"></a>Hromadné vložení pomocí příkazu INSERT
Pro jednorázové načtení do malé tabulky [příkazem INSERT](/sql/t-sql/statements/insert-transact-sql.md) nebo dokonce i opakované načítání vyhledávání si můžete vystačit s příkazem jako třeba `INSERT INTO MyLookup VALUES (1, 'Type 1')`.  Jednorázové vkládání není tak efektivní jako hromadné načtení. 

Pokud máte za den tisíce nebo více samostatných vložení, vytvořte z nich dávku, abyste je mohli načíst hromadně.  Vyvíjejte své procesy tak, aby samostatná vkládání připojovaly do souboru, a pak vytvořte další proces, který tento soubor bude pravidelně načítat.

### <a name="create-statistics-after-the-load"></a>Vytvoření statistiky po načtení

Pro zlepšení výkonu dotazů je důležité vytvořit statistiku pro všechny sloupce všech tabulek po prvním načtení, nebo když v datech dojde k zásadnějším změnám.  Podrobné vysvětlení statistiky najdete v tématu [Statistika][Statistika]. Následující příklad vytvoří statistiku pro pět sloupců tabulky Customer_Speed.

```sql
create statistics [SensorKey] on [Customer_Speed] ([SensorKey]);
create statistics [CustomerKey] on [Customer_Speed] ([CustomerKey]);
create statistics [GeographyKey] on [Customer_Speed] ([GeographyKey]);
create statistics [Speed] on [Customer_Speed] ([Speed]);
create statistics [YearMeasured] on [Customer_Speed] ([YearMeasured]);
```

## <a name="rotate-storage-keys"></a>Obměna klíčů úložiště
Osvědčeným postupem zabezpečení je pravidelně měnit přístupový klíč k úložišti objektů blob. Pro svůj účet úložiště objektů blob máte dva klíče úložiště. To je proto, abyste klíče mohli měnit.

Obměna klíčů účtu služby Azure Storage:

1. Vytvořte přihlašovací údaje v oboru druhé databáze, které vycházejí ze sekundárního přístupového klíče k úložišti.
2. Vytvořte s použitím těchto nových přihlašovacích údajů druhý externí zdroj dat.
3. Odstraňte externí tabulky a znovu je vytvořte tak, aby odkazovaly na nový externí zdroj dat. 

Po migraci externích tabulek do nového zdroje dat proveďte tyto úlohy čištění:

1. Odstraňte první externí zdroj dat.
2. Odstraňte přihlašovací údaje v oboru první databáze, které vycházejí z primárního přístupového klíče k úložišti.
3. Přihlaste se do Azure a znovu vygenerujte primární přístupový klíč, aby byl připravený na další obměnu.


## <a name="next-steps"></a>Další kroky
Informace o monitorování procesu načítání najdete v tématu [Monitorování úlohy pomocí zobrazení dynamické správy](sql-data-warehouse-manage-monitor.md).



