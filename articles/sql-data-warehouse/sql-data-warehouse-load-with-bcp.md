<properties
   pageTitle="Načtení dat do SQL Data Warehouse pomocí nástroje příkazového řádku bcp | Microsoft Azure"
   description="Zjistěte, co je bcp a jak tento nástroj používat pro scénáře datových skladů."
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
   ms.author="mausher;barbkess;sonyama"/>


# Načtení dat pomocí bcp

> [AZURE.SELECTOR]
- [Data Factory](sql-data-warehouse-get-started-load-with-azure-data-factory.md)
- [PolyBase](sql-data-warehouse-get-started-load-with-polybase.md)
- [BCP](sql-data-warehouse-load-with-bcp.md)


**[bcp][]** je nástroj příkazového řádku pro hromadné načítání, který umožňuje kopírovat data mezi SQL Serverem, datovými soubory a SQL Data Warehouse. Pomocí bcp můžete importovat velké počty řádků do tabulek SQL Data Warehouse nebo exportovat data z tabulek SQL Serveru do datových souborů. S výjimkou případu, kdy se nástroj bcp používá s parametrem queryout, nepotřebujete k použití nástroje bcp žádné znalosti jazyka Transact-SQL.

Nástroj bcp představuje rychlý a snadný způsob, jak přesunout menší datové sady z databáze SQL Data Warehouse nebo do ní. Přesné množství dat, který se doporučuje načítat/extrahovat přes bcp, bude záviset na vašem síťovém připojení k datovému centru Azure.  Obecně platí, že pomocí bcp můžete snadno načítat a extrahovat tabulky dimenzí, nedoporučuje se ale používat pro načítání nebo extrahování velkých objemů dat.  Doporučeným nástrojem pro načítání a extrahování velkých objemů dat je nástroj Polybase, který je také vhodnější pro využívání architektury MPP (Massively Parallel Processing) SQL Data Warehouse.

Pomocí nástroje bcp můžete:

- Načítat data do SQL Data Warehouse pomocí jednoduchého nástroje příkazového řádku
- Extrahovat data z SQL Data Warehouse pomocí jednoduchého nástroje příkazového řádku

V tomto kurzu se dozvíte, jak:

- Importovat data do tabulky pomocí příkazu bcp in
- Exportovat data z tabulky pomocí příkazu bcp out

>[AZURE.VIDEO loading-data-into-azure-sql-data-warehouse-with-bcp]

## Požadavky

Pro jednotlivé kroky v tomto kurzu budete potřebovat:

- Databázi SQL Data Warehouse
- Nainstalovaný nástroj příkazového řádku bcp
- Nainstalovaný nástroj příkazového řádku SQLCMD

>[AZURE.NOTE] Nástroje bcp a sqlcmd si můžete stáhnout z webu [Stažení softwaru společnosti Microsoft][].

## Import dat do SQL Data Warehouse

V tomto kurzu vytvoříte tabulku v Azure SQL Data Warehouse a naimportujete do ní data.

### Krok 1: Vytvoření tabulky v Azure SQL Data Warehouse

Pomocí příkazu sqlcmd na příkazovém řádku spusťte následující dotaz, který vytvoří tabulku ve vaší instanci:

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

>[AZURE.NOTE] Další informace o vytváření tabulek v SQL Data Warehouse a možnostech dostupných v klauzuli WITH viz [Přehled tabulek][] nebo [Syntaxe CREATE TABLE][].

### Krok 2: Vytvoření zdrojového datového souboru

Otevřete Poznámkový blok a zkopírujte následující řádky dat do nového textového souboru. Pak tento soubor uložte do místního dočasného adresáře C:\Temp\DimDate2.txt.

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

> [AZURE.NOTE] Je důležité pamatovat na to, že nástroj bcp.exe nepodporuje kódování souborů UTF-8. Při použití nástroje bcp.exe prosím použijte soubory ASCII nebo UTF-16.

### Krok 3: Připojení a import dat
Při použití nástroje bcp můžete připojit a importovat data pomocí následujícího příkazu (hodnoty podle potřeby nahraďte svými):

```sql
bcp DimDate2 in C:\Temp\DimDate2.txt -S <Server Name> -d <Database Name> -U <Username> -P <password> -q -c -t  ','
```

To, jestli byla data načtena, můžete ověřit spuštěním následujícího dotazu pomocí sqlcmd:

```sql
sqlcmd.exe -S <server name> -d <database name> -U <username> -P <password> -I -Q "SELECT * FROM DimDate2 ORDER BY 1;"
```

Tento dotaz by měl vrátit následující výsledky:

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

### Krok 4: Vytvoření statistiky pro vaše nově načtená data

Azure SQL Data Warehouse zatím nepodporuje automatické vytváření ani automatickou aktualizaci statistik. Aby vám dotazy vracely co nejlepší výsledky, je důležité, aby se statistiky vytvořily pro všechny sloupce všech tabulek po prvním načtením nebo kdykoli, kdy v datech dojde k podstatným změnám. Podrobné vysvětlení statistiky najdete v tématu [Statistika][] ve skupině témat věnovaných vývoji. Níže je zběžný příklad vytvoření statistik pro tabulku načtenou v tomto příkladě.

Proveďte následující příkazy CREATE STATISTICS na příkazovém řádku sqlcmd:

```sql
sqlcmd.exe -S <server name> -d <database name> -U <username> -P <password> -I -Q "
    create statistics [DateId] on [DimDate2] ([DateId]);
    create statistics [CalendarQuarter] on [DimDate2] ([CalendarQuarter]);
    create statistics [FiscalQuarter] on [DimDate2] ([FiscalQuarter]);
"
```

## Export dat z SQL Data Warehouse
V tomto kurzu vytvoříte z tabulky v SQL Data Warehouse datový soubor. Data, která jsme vytvořili výše, vyexportujeme do nového datového souboru s názvem DimDate2_export.txt.

### Krok 1: Export dat

Při použití nástroje bcp můžete připojit a vyexportovat data pomocí následujícího příkazu (hodnoty podle potřeby nahraďte svými):

```sql
bcp DimDate2 out C:\Temp\DimDate2_export.txt -S <Server Name> -d <Database Name> -U <Username> -P <password> -q -c -t ','
```
To, že se data vyexportovala správně, můžete ověřit tak, že nový soubor otevřete. Data v souboru by se měla shodovat následujícím textem:

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

>[AZURE.NOTE] Vzhledem k povaze distribuovaných systémů nemusí být pořadí dat napříč všemi databázemi SQL Data Warehouse stejné. Další možností je použít funkci **queryout** nástroje bcp, která místo vyexportování celé tabulky umožňuje zapsat extrakci dotazu.

## Další kroky
Přehled načítání najdete v tématu [Načtení dat do SQL Data Warehouse][].
Další tipy pro vývoj najdete v části [Přehled vývoje SQL Data Warehouse][].

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



<!--HONumber=Aug16_HO4-->


