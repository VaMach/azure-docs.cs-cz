<properties
   pageTitle="Načtení dat z SQL Serveru do Azure SQL Data Warehouse (bcp) | Microsoft Azure"
   description="Pro malou velikost dat využívá bcp k exportu data z SQL Serveru do plochých souborů a k importu dat přímo do Azure SQL Data Warehouse."
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
   ms.date="06/30/2016"
   ms.author="lodipalm;barbkess;sonyama"/>


# Načtení dat z SQL Serveru do Azure SQL Data Warehouse (ploché soubory)

> [AZURE.SELECTOR]
- [SSIS](sql-data-warehouse-load-from-sql-server-with-integration-services.md)
- [PolyBase](sql-data-warehouse-load-from-sql-server-with-polybase.md)
- [bcp](sql-data-warehouse-load-from-sql-server-with-bcp.md)

Pro malé datové sady můžete použít nástroj příkazového řádku bcp k exportu dat z SQL Serveru, které pak můžete načíst přímo do Azure SQL Data Warehouse.

V tomto kurzu použijete nástroj bcp k následujícím operacím:

- Export tabulky z SQL Serveru pomocí příkazu bcp out (nebo vytvoření jednoduchého ukázkového souboru)
- Import tabulky z plochého souboru do SQL Data Warehouse
- Vytvoření statistiky pro načtená data

>[AZURE.VIDEO loading-data-into-azure-sql-data-warehouse-with-bcp]

## Než začnete

### Požadavky

Pro jednotlivé kroky v tomto kurzu budete potřebovat:

- Databázi SQL Data Warehouse
- Nainstalovaný nástroj příkazového řádku bcp
- Nainstalovaný nástroj příkazového řádku sqlcmd

Nástroje bcp a sqlcmd si můžete stáhnout z webu [Stažení softwaru společnosti Microsoft][].

### Data ve formátu ASCII nebo UTF-16

Pokud pro tento kurz používáte svoje vlastní data, musí vaše data používat kódování ASCII nebo UTF-16, protože bcp nepodporuje kódování UTF-8. 

PolyBase podporuje UTF-8, ale zatím nepodporuje UTF-16. Je potřeba upozornit na to, že pokud chcete kombinovat používání nástroje s funkcí PolyBase, budete muset data po vyexportování z SQL Serveru převést na UTF-8. 


## 1. Vytvoření cílové tabulky

Definujte v SQL Data Warehouse tabulku, která bude cílovou tabulkou pro načtení. Sloupce v tabulce musí odpovídat datům v jednotlivých řádcích vašeho datového souboru.

Pokud chcete vytvořit tabulku, otevřete okno příkazového řádku a pomocí sqlcmd.exe spusťte následující příkaz:


```sql
sqlcmd.exe -S <server name> -d <database name> -U <username> -P <password> -I -Q "
    CREATE TABLE DimDate2
    (
        DateId INT NOT NULL,
        CalendarQuarter TINYINT NOT NULL,
        FiscalQuarter TINYINT NOT NULL
    )
    WITH
    (
        CLUSTERED COLUMNSTORE INDEX,
        DISTRIBUTION = ROUND_ROBIN
    );
"
```


## 2. Vytvoření zdrojového datového souboru

Otevřete Poznámkový blok a zkopírujte následující řádky dat do nového textového souboru. Pak tento soubor uložte do místního dočasného adresáře C:\Temp\DimDate2.txt. Tato data jsou ve formátu ASCII.

```
20150301,1,3
20150501,2,4
20151001,4,2
20150201,1,3
20151201,4,2
20150801,3,1
20150601,2,4
20151101,4,2
20150401,2,4
20150701,3,1
20150901,3,1
20150101,1,3
```

(Volitelné) Pokud chcete z databáze SQL Serveru vyexportovat svoje vlastní data, otevřete příkazový řádek a spusťte následující příkaz. TableName, ServerName, DatabaseName, Username a Password nahraďte svými vlastními informacemi.

```sql
bcp <TableName> out C:\Temp\DimDate2_export.txt -S <ServerName> -d <DatabaseName> -U <Username> -P <Password> -q -c -t ','
```



## 3. Načtení dat
Pokud chcete načíst data, otevřete příkazový řádek a spusťte následující příkaz, přičemž hodnoty parametrů Server Name (Název serveru), Database name (Název databáze), Username (Uživatelské jméno) a Password (Heslo) nahraďte svými vlastními informacemi.

```sql
bcp DimDate2 in C:\Temp\DimDate2.txt -S <ServerName> -d <DatabaseName> -U <Username> -P <password> -q -c -t  ','
```

Pomocí tohoto příkazu ověřte, že se data načetla správně.

```sql
sqlcmd.exe -S <server name> -d <database name> -U <username> -P <password> -I -Q "SELECT * FROM DimDate2 ORDER BY 1;"
```

Výsledky by měly vypadat takto:

DateId |CalendarQuarter |FiscalQuarter
----------- |--------------- |-------------
20150101 |1 |3
20150201 |1 |3
20150301 |1 |3
20150401 |2 |4
20150501 |2 |4
20150601 |2 |4
20150701 |3 |1
20150801 |3 |1
20150801 |3 |1
20151001 |4 |2
20151101 |4 |2
20151201 |4 |2

## 4. Vytvoření statistiky

SQL Data Warehouse zatím nepodporuje automatické vytváření ani automatickou aktualizaci statistik. Aby vám dotazy vracely co nejlepší výsledky, je důležité, aby se statistiky vytvořily pro všechny sloupce všech tabulek po prvním načtením nebo kdykoli, kdy v datech dojde k zásadnějším změnám. Podrobné vysvětlení statistiky najdete v tématu [Statistika][]. 

Spuštěním následujícího příkazu vytvořte statistiku pro nově načtenou tabulku.

```sql
sqlcmd.exe -S <server name> -d <database name> -U <username> -P <password> -I -Q "
    create statistics [DateId] on [DimDate2] ([DateId]);
    create statistics [CalendarQuarter] on [DimDate2] ([CalendarQuarter]);
    create statistics [FiscalQuarter] on [DimDate2] ([FiscalQuarter]);
"
```

## 5. Export dat z SQL Data Warehouse
Cvičně si můžete data, která jste právě načetli, vyexportovat zpět z SQL Data Warehouse.  Příkaz pro export je úplně stejný jako příkaz pro export z SQL Serveru.

Rozdíl je ve výsledcích. Vzhledem k tomu, že jsou data v rámci SQL Data Warehouse uložená v distribuovaných umístěních, při exportu dat zapíše data do výstupního souboru každý uzel Výpočty. Pořadí dat ve výstupním souboru bude s velkou pravděpodobností jiné než pořadí dat ve vstupním souboru.

### Export tabulky a porovnání vyexportovaných výsledků

Pokud si budete chtít vyexportovaná data zobrazit, otevřete okno příkazového řádku a spustíte tento příkaz pomocí svých vlastních parametrů. ServerName je název vašeho logického SQL serveru Azure.

```sql
bcp DimDate2 out C:\Temp\DimDate2_export.txt -S <Server Name> -d <Database Name> -U <Username> -P <password> -q -c -t ','
```
To, že se data vyexportovala správně, můžete ověřit tak, že nový soubor otevřete. Data v souboru by se měla shodovat s následujícím textem, budou ale pravděpodobně uvedená v jiném pořadí:

```
20150301,1,3
20150501,2,4
20151001,4,2
20150201,1,3
20151201,4,2
20150801,3,1
20150601,2,4
20151101,4,2
20150401,2,4
20150701,3,1
20150901,3,1
20150101,1,3
```

### Export výsledků dotazu

Místo vyexportování celé tabulky můžete také pomocí funkce **queryout** nástroje bcp vyexportovat jenom výsledky dotazu. 

## Další kroky
Přehled načítání najdete v tématu [Načtení dat do SQL Data Warehouse][].
Další tipy pro vývoj najdete v části [Přehled vývoje SQL Data Warehouse][].
Další informace o vytvoření tabulky v SQL Data Warehouse viz [Přehled tabulek][] nebo [Syntaxe CREATE TABLE][].

<!--Image references-->

<!--Article references-->

[Načtení dat do SQL Data Warehouse]: ./sql-data-warehouse-overview-load.md
[Přehled vývoje SQL Data Warehouse]: ./sql-data-warehouse-overview-develop.md
[Přehled tabulek]: ./sql-data-warehouse-tables-overview.md
[Statistika]: ./sql-data-warehouse-tables-statistics.md

<!--MSDN references-->
[bcp]: https://msdn.microsoft.com/library/ms162802.aspx
[Syntaxe CREATE TABLE]: https://msdn.microsoft.com/library/mt203953.aspx

<!--Other Web references-->
[Stažení softwaru společnosti Microsoft]: https://www.microsoft.com/download/details.aspx?id=36433



<!---HONumber=Aug16_HO4-->


