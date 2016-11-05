---
title: Dotazování Azure SQL Data Warehouse (sqlcmd)| Microsoft Docs
description: Dotazování SQL Azure Data Warehouse pomocí nástroje příkazového řádku sqlcmd.
services: sql-data-warehouse
documentationcenter: NA
author: sonyam
manager: barbkess
editor: ''

ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.date: 09/06/2016
ms.author: barbkess;sonyama

---
# Dotazování Azure SQL Data Warehouse (sqlcmd)
> [!div class="op_single_selector"]
> * [Power BI](sql-data-warehouse-get-started-visualize-with-power-bi.md)
> * [Azure Machine Learning](sql-data-warehouse-get-started-analyze-with-azure-machine-learning.md)
> * [Visual Studio](sql-data-warehouse-query-visual-studio.md)
> * [sqlcmd](sql-data-warehouse-get-started-connect-sqlcmd.md) 
> 
> 

Tento návod používá k dotazování služby Azure SQL Data Warehouse nástroj příkazového řádku [sqlcmd][sqlcmd].  

## 1. Připojení
Chcete-li začít s nástrojem [sqlcmd][sqlcmd], otevřete příkazový řádek a zadejte příkaz **sqlcmd** následovaný připojovacím řetězcem pro vaši databázi SQL Data Warehouse. Připojovací řetězec bude muset mít následující parametry:

* **Server (-S):** Server v následující podobě: `<`název serveru`>`.database.windows.net
* **Database (-d):** Název databáze
* **Enable Quoted Identifiers (-I):** Aby bylo možné se připojit k instanci služby SQL Data Warehouse, musí být povolené identifikátory v uvozovkách.

Chcete-li používat ověřování systému SQL Server, je třeba přidat parametry uživatelského jména a hesla:

* **User (-U):** Uživatel serveru v následující podobě: `<`Uživatel`>`
* **Password (-P):** Heslo přidružené k uživateli

Připojovací řetězec může například vypadat následovně:

```sql
C:\>sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -U myuser -P myP@ssword -I
```

Chcete-li používat integrované ověřování v Azure Active Directory, je třeba přidat parametry Azure Active Directory:

* **Azure Active Directory Authentication (-G):** Pro ověřování používat Azure Active Directory

Připojovací řetězec může například vypadat následovně:

```sql
C:\>sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -G -I
```

> [!NOTE]
> Abyste mohli provádět ověřování pomocí Active Directory, je třeba [povolit ověřování Azure Active Directory](sql-data-warehouse-authentication.md).
> 
> 

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
Viz část [Dokumentace sqlcmd][sqlcmd], kde najdete další informace o možnostech dostupných v sqlcmd.

<!--Image references-->

<!--Article references-->

<!--MSDN references--> 
[sqlcmd]: https://msdn.microsoft.com/library/ms162773.aspx
[portál Azure]: https://portal.azure.com

<!--Other Web references-->



<!--HONumber=Sep16_HO3-->


