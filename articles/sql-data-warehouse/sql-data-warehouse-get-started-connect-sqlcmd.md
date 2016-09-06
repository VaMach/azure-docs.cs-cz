<properties
   pageTitle="Dotazování Azure SQL Data Warehouse (sqlcmd)| Microsoft Azure"
   description="Dotazování SQL Azure Data Warehouse pomocí nástroje příkazového řádku sqlcmd."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="sonyam"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/30/2016"
   ms.author="mausher;barbkess;sonyama"/>

# Dotazování Azure SQL Data Warehouse (sqlcmd)

> [AZURE.SELECTOR]
- [Power BI](sql-data-warehouse-get-started-visualize-with-power-bi.md)
- [Azure Machine Learning](sql-data-warehouse-get-started-analyze-with-azure-machine-learning.md)
- [Visual Studio](sql-data-warehouse-query-visual-studio.md)
- [sqlcmd](sql-data-warehouse-get-started-connect-sqlcmd.md) 

Tento návod používá nástroj příkazového řádku [sqlcmd][] k dotazování Azure SQL Data Warehouse.  

## 1. Připojení

Chcete-li začít s nástrojem [sqlcmd][], otevřete příkazový řádek a zadejte příkaz **sqlcmd** následovaný připojovacím řetězcem pro vaši databázi SQL Data Warehouse. Připojovací řetězec bude muset mít následující parametry:

+ **Server (-S):** Server v následující podobě: `<`název serveru`>`.database.windows.net
+ **Database (-d):** Název databáze
+ **User (-U):** Uživatel serveru v následující podobě: `<`Uživatel`>`
+ **Password (-P):** Heslo přidružené k uživateli
+ **Enable Quoted Identifiers (-I):** Aby bylo možné se připojit k instanci SQL Data Warehouse, musí být povolené identifikátory v uvozovkách.

Připojovací řetězec může například vypadat následovně:

```sql
C:\>sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -U myuser -P myP@ssword -I
```

> [AZURE.NOTE] Možnost -I, která umožňuje identifikátory v uvozovkách, je momentálně nutná pro připojení k SQL Data Warehouse.

## 2. Dotaz

Po připojení můžete pro instanci zadávat všechny podporované příkazy jazyka Transact-SQL.  V tomto příkladu jsou dotazy zadávány v interaktivním režimu.

```sql
C:\>sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -U myuser -P myP@ssword -I
1> SELECT name FROM sys.tables;
2> GO
3> QUIT
```

Tyto další příklady ukazují, jak lze vaše dotazy spouštět v dávkovém režimu pomocí parametru -Q nebo vedení serveru SQL k příkazu sqlcmd.

```sql
sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -U myuser -P myP@ssword -I -Q "SELECT name FROM sys.tables;"
```

```sql
"SELECT name FROM sys.tables;" | sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -U myuser -P myP@ssword -I > .\tables.out
```

## Další kroky

Viz část [Dokumentace sqlcmd][sqlcmd], kde najdete další informace o možnostech dostupných v sqlcmd.

<!--Image references-->

<!--Article references-->

<!--MSDN references--> 
[sqlcmd]: https://msdn.microsoft.com/library/ms162773.aspx
[portál Azure]: https://portal.azure.com

<!--Other Web references-->



<!--HONumber=ago16_HO5-->


