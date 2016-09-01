<properties
   pageTitle="Vytvoření SQL Data Warehouse pomocí TSQL | Microsoft Azure"
   description="Naučte se vytvářet Azure SQL Data Warehouse pomocí TSQL."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="lodipalm"
   manager="barbkess"
   editor=""
   tags="azure-sql-data-warehouse"/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="hero-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/16/2016"
   ms.author="lodipalm;barbkess;sonyama"/>

# Vytvoření databáze SQL Data Warehouse pomocí jazyka Transact-SQL (TSQL)

> [AZURE.SELECTOR]
- [Azure Portal](sql-data-warehouse-get-started-provision.md)
- [TSQL](sql-data-warehouse-get-started-create-database-tsql.md)
- [PowerShell](sql-data-warehouse-get-started-provision-powershell.md)

Tento článek popisuje, jak vytvořit databázi SQL Data Warehouse pomocí T-SQL.

## Požadavky

Budete potřebovat: 

- **Účet Azure:** Pokud si chcete vytvořit účet, přečtěte si článek [bezplatné zkušební verzi Azure][] nebo [kreditům Azure pro předplatitele MSDN][].
- **Azure SQL Server:** Přečtěte si článek [Vytvoření logického serveru Azure SQL Database pomocí portálu Azure][] nebo [Vytvoření logického serveru Azure SQL Database pomocí prostředí PowerShell][], kde najdete další podrobnosti.
- **Skupinu prostředků:** Buď použijte stejnou skupinu prostředků jako pro Azure SQL Server, nebo zjistěte, [jak vytvořit skupinu prostředků][].
- **Prostředí ke spuštění T-SQL**: ke spuštění T-SQL můžete použít [Visual Studio][instalaci sady Visual Studio a rozšíření SSDT], [sqlcmd][] nebo [SSMS][].

> [AZURE.NOTE] Vytvoření nové služby SQL Data Warehouse může znamenat, že se vám začne fakturovat nová služba.  Další podrobnosti o cenách najdete v tématu [SQL Data Warehouse – ceny][].

## Vytvoření databáze pomocí sady Visual Studio

Pokud jste ještě sadu Visual Studio nikdy nepoužívali, přečtěte si článek [Dotazování Azure SQL Data Warehouse (Visual Studio)][].  Začněte tím, že otevřete Průzkumník objektů systému SQL Server v sadě Visual Studio a připojíte se k serveru, který bude hostitelem databáze SQL Data Warehouse.  Po připojení můžete vytvořit SQL Data Warehouse spuštěním následujícího příkazu SQL na **hlavní** databázi.  Tento příkaz vytvoří databázi MySqlDwDb s cílem služby DW400 a umožní zvětšování databáze až do maximální velikosti 10 TB.

```sql
CREATE DATABASE MySqlDwDb (EDITION='datawarehouse', SERVICE_OBJECTIVE = 'DW400', MAXSIZE= 10240 GB);
```

## Vytvoření databáze pomocí sqlcmd

Alternativně můžete spustit stejný příkaz pomocí sqlcmd spuštěním následujícího příkazu na příkazovém řádku.

```sql
sqlcmd -S <Server Name>.database.windows.net -I -U <User> -P <Password> -Q "CREATE DATABASE MySqlDwDb (EDITION='datawarehouse', SERVICE_OBJECTIVE = 'DW400', MAXSIZE= 10240 GB)"
```

`MAXSIZE` může být v rozsahu od 250 GB do 240 TB.  `SERVICE_OBJECTIVE` může být v rozmezí od DW100 do DW2000 [DWU][].  Seznam všech platných hodnot naleznete v dokumentaci MSDN pro příkaz [CREATE DATABASE][].  Parametr MAXSIZE i parametr SERVICE_OBJECTIVE je také možné měnit pomocí příkazu [ALTER DATABASE][] jazyka T-SQL.  Při změně hodnoty parametru SERVICE_OBJECTIVE je potřeba postupovat opatrně, protože způsobí restartování služeb, což bude znamenat zrušení všech probíhajících dotazů.  Změna hodnoty parametru MAXSIZE nerestartuje služby, protože jde pouze o operaci s metadaty.

## Další kroky

Až se vám zřídí SQL Data Warehouse, můžete [načíst ukázková data][] nebo se můžete podívat, jak na [vývoj][], [načítání][] nebo [migraci][].

<!--Article references-->
[DWU]: ./sql-data-warehouse-overview-what-is.md#data-warehouse-units
[jak vytvořit SQL Data Warehouse z portálu Azure]: sql-data-warehouse-get-started-provision.md
[Dotazování Azure SQL Data Warehouse (Visual Studio)]: sql-data-warehouse-query-visual-studio.md
[migraci]: sql-data-warehouse-overview-migrate.md
[vývoj]: sql-data-warehouse-overview-develop.md
[načítání]: sql-data-warehouse-overview-load.md
[načíst ukázková data]: sql-data-warehouse-load-sample-databases.md
[Vytvoření logického serveru Azure SQL Database pomocí portálu Azure]: ../sql-database/sql-database-get-started.md#create-an-azure-sql-database-logical-server
[Vytvoření logického serveru Azure SQL Database pomocí prostředí PowerShell]: ../sql-database/sql-database-get-started-powershell.md#database-setup-create-a-resource-group-server-and-firewall-rule
[jak vytvořit skupinu prostředků]: ../resource-group-template-deploy-portal.md#create-resource-group
[instalaci sady Visual Studio a rozšíření SSDT]: sql-data-warehouse-install-visual-studio.md
[sqlcmd]: sql-data-warehouse-get-started-connect-sqlcmd.md

<!--MSDN references--> 
[CREATE DATABASE]: https://msdn.microsoft.com/library/mt204021.aspx
[ALTER DATABASE]: https://msdn.microsoft.com/library/mt204042.aspx
[SSMS]: https://msdn.microsoft.com/library/mt238290.aspx

<!--Other Web references-->
[SQL Data Warehouse – ceny]: https://azure.microsoft.com/pricing/details/sql-data-warehouse/
[bezplatné zkušební verzi Azure]: https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F
[kreditům Azure pro předplatitele MSDN]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F



<!---HONumber=Aug16_HO4-->


