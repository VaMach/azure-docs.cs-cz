---
title: "Osvědčené postupy načítání dat – Azure SQL Data Warehouse | Microsoft Docs"
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
ms.openlocfilehash: 10d06fd29640a350c5522c00c4c9ebd9c6b24c89
ms.sourcegitcommit: c87e036fe898318487ea8df31b13b328985ce0e1
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/19/2017
---
# <a name="best-practices-for-loading-data-into-azure-sql-data-warehouse"></a>Osvědčené postupy načítání dat do služby Azure SQL Data Warehouse
Doporučení a optimalizace výkonu pro načítání dat do služby Azure SQL Data Warehouse. 

- Další informace o PolyBase a návrhu procesu ELT (extrakce, načítání a transformace) najdete v tématu [Návrh ELT pro službu SQL Data Warehouse](design-elt-data-loading.md).
- Kurz načítání najdete v tématu [Použití PolyBase k načítání dat z úložiště objektů blob v Azure do služby Azure SQL Data Warehouse](load-data-from-azure-blob-storage-using-polybase.md).


## <a name="preparing-data-in-azure-storage"></a>Příprava dat v Azure Storage
Vrstvu úložiště i datový sklad uložte společně do stejného umístění, abyste minimalizovali latenci.

Při exportu dat do formátu souboru ORC může být počet textově náročných sloupců omezen na 50, aby se předešlo chybám v jazyce Java způsobeným nedostatkem paměti. Toto omezení můžete obejít tím, že importujete jen podmnožinu sloupců.

Technologie PolyBase nedokáže načíst řádky, které obsahují více než 1 000 000 bajtů dat. Když vkládáte data do textových souborů v úložišti Azure Blob nebo ve službě Azure Data Lake Store, musí tyto soubory obsahovat méně než 1 000 000 bajtů dat. Toto omezení platí bez ohledu na schéma tabulky.

Všechny formáty souborů mají jiné výkonové charakteristiky. Pokud chcete docílit nejrychlejšího načtení, použijte komprimované textové soubory s oddělovači. Rozdíl mezi výkonem kódování UTF-8 a UTF-16 je minimální.

Velké komprimované soubory rozdělte do menších komprimovaných souborů.

## <a name="running-loads-with-enough-compute"></a>Dostatečné výpočetní prostředky pro načítání dat

Největší rychlosti při načítání dosáhnete, když budete spouštět vždy jen jednu úlohu načtení dat. Pokud to není možné, spouštějte souběžně co nejmenší počet úloh. Při rozsáhlé úloze načítání dat byste před načítáním měli zvážit možnost rozšíření datového skladu škálováním.

Pokud chcete spouštět načítání s odpovídajícími výpočetními prostředky, vytvořte uživatele načítání vyhrazené pro spouštění načítání. Každého uživatele načítání přiřaďte ke konkrétní třídě prostředků. Pokud chcete spustit načítání, přihlaste se jako jeden z uživatelů načítání a pak spusťte načítání. Načítání se spustí s využitím třídy prostředků tohoto uživatele.  Tato metoda je jednodušší než se pokoušet o změnu třídy prostředků uživatele podle aktuálních potřeb třídy prostředků.

Tento kód vytvoří uživatele načítání pro třídu prostředků staticrc20. Udělí uživateli oprávnění řídit databázi a pak přidá uživatele jako člena databázové role staticrc20. Pokud chcete spustit načítání s prostředky pro třídy prostředků staticRC20, stačí se přihlásit jako LoaderRC20 a spustit načítání. 

    ```sql
    CREATE LOGIN LoaderRC20 WITH PASSWORD = 'a123STRONGpassword!';
    CREATE USER LoaderRC20 FOR LOGIN LoaderRC20;
    GRANT CONTROL ON DATABASE::[mySampleDataWarehouse] to LoaderRC20;
    EXEC sp_addrolemember 'staticrc20', 'LoaderRC20';
    ```

Spouštějte načítání v rámci statických, a ne dynamických, tříd prostředků. Použití statických tříd prostředků zaručuje stejné prostředky bez ohledu na vaši [úroveň služby](performance-tiers.md#service-levels). Pokud použijete dynamickou třídu prostředků, budou se prostředky lišit v závislosti na vaší úrovni služby. V případě dynamických tříd znamená nižší úroveň služby, že pro vašeho uživatele načítání pravděpodobně musíte použít větší třídu prostředků.

## <a name="allowing-multiple-users-to-load"></a>Povolení načítání více uživatelům

Často je potřeba, aby data do datového skladu načítalo více uživatelů. Příkaz [CREATE TABLE AS SELECT (Transact-SQL)][CREATE TABLE AS SELECT (Transact-SQL)] vyžaduje k databázi oprávnění CONTROL.  Oprávnění CONTROL poskytuje přístup pro řízení ke všem schématům. Pravděpodobně ale nebudete chtít, aby všichni uživatelé, kteří načítají data, měli oprávnění CONTROL pro přístup ke všem schématům. K omezení oprávnění slouží příkaz DENY CONTROL.

Představte si například schémata databáze schema_A pro oddělení A a schema_B pro oddělení B. Uživatelé databáze user_A a user_B budou uživateli pro načítání PolyBase v oddělení A, respektive oddělení B. Oba uživatelé mají k databázi udělená oprávnění CONTROL. Autoři schémat A a B teď schémata zamknou příkazem DENY:

```sql
   DENY CONTROL ON SCHEMA :: schema_A TO user_B;
   DENY CONTROL ON SCHEMA :: schema_B TO user_A;
```   

Uživatelé user_A a user_B jsou nyní vyloučení ze schématu příslušného druhého oddělení.


## <a name="loading-to-a-staging-table"></a>Načítání do pracovní tabulky

Pokud chcete při přesunu dat do tabulky datového skladu dosáhnout největší načítací rychlosti, načtěte data do pracovní tabulky.  Pracovní tabulku definujte jako haldu a jako způsob distribuce použijte kruhové dotazování (round robin). 

Myslete na to, že načítání je většinou dvoufázový proces, kdy napřed načtete data do pracovní tabulky a pak je vložíte do tabulky v provozním datovém skladu. Pokud provozní tabulka používá k distribuci algoritmus hash, může být celková doba načtení a vložení dat kratší, než když k definici pracovní tabulky použijete distribuci hash. Načítání do pracovní tabulky trvá déle, ale druhý krok, který spočívá ve vkládání řádků do provozní tabulky, nepřesouvá data prostřednictvím distribuce.

## <a name="loading-to-a-columnstore-index"></a>Načítání do indexu columnstore

Indexy columnstore vyžadují hodně paměti, aby mohly komprimovat data do vysoce kvalitních skupin řádků. Kvůli zajištění co nejlepší účinnosti komprese a indexování musí index columnstore do každé skupiny řádků zkomprimovat maximální počet 1 048 576 řádků. V případě nedostatku paměti nemusí index columnstore dosahovat maximální míry komprese. To zase ovlivňuje výkon dotazů. Podrobné informace najdete v tématu [Optimalizace paměti pro columnstore](sql-data-warehouse-memory-optimizations-for-columnstore-compression.md).

- Pokud chcete zajistit, aby měl nahrávající uživatel dostatek paměti pro dosažení maximální míry komprese, použijte uživatele načítání, kteří jsou členy střední nebo velké třídy prostředků. 
- Načtěte dostatek dat pro úplně naplnění nových skupin řádků. Při hromadném načítání dat se každých 1 048 576 řádků zkomprimuje přímo do indexu columnstore jako kompletní skupina řádků. Při načítání méně než 102 400 řádků se řádky odesílají do tabulky deltastore, kde se řádky uchovávají v indexu B-stromu. Pokud načtete příliš málo řádků, můžou se všechny dostat do indexu deltastore, a nebudou se okamžitě komprimovat do formátu columnstore.


## <a name="handling-loading-failures"></a>Zpracování chyb načítání

Načtení s použitím externí tabulky může selhat s chybou *Query aborted-- the maximum reject threshold was reached while reading from an external source* (Dotaz byl přerušen – při čtení z externího zdroje byla dosažena maximální prahová hodnota pro odmítnutí). Tato zpráva znamená, že vaše externí data obsahují nezapsané záznamy. Datový záznam se považuje za nezapsaný, pokud se typy dat nebo čísla sloupců neshodují s definicemi sloupců externí tabulky nebo pokud data neodpovídají zadanému formátu externího souboru. 

Pokud chcete nezapsané záznamy opravit, ujistěte se, že jsou definice formátů externí tabulky a externího souboru správné a že externí data těmto definicím odpovídají. V případě nezapsané podmnožiny externích datových záznamů se můžete rozhodnout, že ve svých dotazech, které využívají možnosti zamítnutí v příkazu CREATE EXTERNAL TABLE, chcete tyto záznamy zamítnout.

## <a name="inserting-data-into-a-production-table"></a>Vložení dat do provozní tabulky
Při jednorázovém načtení malé tabulky [příkazem INSERT](/sql/t-sql/statements/insert-transact-sql.md) nebo i při pravidelně se opakujícím načítání funkcí look-up pravděpodobně vystačíte s následujícím příkazem: `INSERT INTO MyLookup VALUES (1, 'Type 1')`.  Vkládání jednotlivých záznamů ale není tak účinné jako hromadné načtení. 

Pokud máte za den tisíce nebo více samostatných vložení, vytvořte z nich dávku, abyste je mohli načíst hromadně.  Vyvíjejte své procesy tak, aby samostatná vkládání připojovaly do souboru, a pak vytvořte další proces, který tento soubor bude pravidelně načítat.

## <a name="creating-statistics-after-the-load"></a>Vytvoření statistiky po načtení

Pro zlepšení výkonu dotazů je důležité vytvořit statistiku pro všechny sloupce všech tabulek po prvním načtení, nebo když v datech dojde k zásadnějším změnám.  Podrobné vysvětlení statistiky najdete v tématu [Statistika][Statistika]. Následující příklad vytvoří statistiku pro pět sloupců tabulky Customer_Speed.

```sql
create statistics [SensorKey] on [Customer_Speed] ([SensorKey]);
create statistics [CustomerKey] on [Customer_Speed] ([CustomerKey]);
create statistics [GeographyKey] on [Customer_Speed] ([GeographyKey]);
create statistics [Speed] on [Customer_Speed] ([Speed]);
create statistics [YearMeasured] on [Customer_Speed] ([YearMeasured]);
```

## <a name="rotate-storage-keys"></a>Obměna klíčů úložiště
Osvědčeným postupem zabezpečení je pravidelně měnit přístupový klíč k úložišti objektů blob. Ke svému účtu úložiště objektů Blob máte dva klíče úložiště, abyste je mohli střídat.

Obměna klíčů účtu služby Azure Storage:

1. Vytvořte přihlašovací údaje v oboru druhé databáze, které vycházejí ze sekundárního přístupového klíče k úložišti.
2. Vytvořte s použitím těchto nových přihlašovacích údajů druhý externí zdroj dat.
3. Odstraňte externí tabulky a znovu je vytvořte tak, aby odkazovaly na nový externí zdroj dat. 

Po migraci externích tabulek do nového zdroje dat proveďte následující čisticí úlohy:

1. Odstraňte první externí zdroj dat.
2. Odstraňte přihlašovací údaje v oboru první databáze, které vycházejí z primárního přístupového klíče k úložišti.
3. Přihlaste se do Azure a znovu vygenerujte primární přístupový klíč, aby byl připravený na další obměnu.


## <a name="next-steps"></a>Další kroky
Informace o monitorování procesu načítání najdete v tématu [Monitorování úlohy pomocí zobrazení dynamické správy](sql-data-warehouse-manage-monitor.md).



