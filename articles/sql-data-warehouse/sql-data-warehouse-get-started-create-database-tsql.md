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
   ms.date="06/04/2016"
   ms.author="lodipalm;barbkess;sonyama"/>

# Vytvoření databáze SQL Data Warehouse pomocí jazyka Transact-SQL (TSQL)

> [AZURE.SELECTOR]
- [Portál Azure](sql-data-warehouse-get-started-provision.md)
- [TSQL](sql-data-warehouse-get-started-create-database-tsql.md)
- [PowerShell](sql-data-warehouse-get-started-provision-powershell.md)

Tento článek popisuje, jak vytvořit databázi SQL Data Warehouse pomocí Transact-SQL (T-SQL).

## Požadavky
Než začnete, zkontrolujte, že jsou splněné následující požadavky.

- **Účet Azure:** Pokud si chcete vytvořit účet, najdete další informace v tématech věnovaných [bezplatné zkušební verzi Azure][] a [kreditům Azure pro předplatitele MSDN][].
- **Azure SQL Server V12:** Projděte si témata [Vytvoření logického serveru Azure SQL Database pomocí portálu Azure][] a [Vytvoření logického serveru Azure SQL Database pomocí prostředí PowerShell][].
- **Název skupiny prostředků:** Pokud chcete vytvořit novou skupinu prostředků, použijte buď stejnou skupinu prostředků jako pro SQL server Azure V12, nebo si projděte informace v tématu věnovaném [skupinám prostředků][].
- **Visual Studio s rozšířením SQL Server Data Tools**: Pokyny k instalaci najdete v tématu [Instalace sady Visual Studio a rozšíření SSDT][].

> [AZURE.NOTE] Vytvoření nové služby SQL Data Warehouse může znamenat, že se vám začne fakturovat nová služba.  Další podrobnosti o cenách najdete v tématu [SQL Data Warehouse – ceny][].

## Vytvoření databáze pomocí sady Visual Studio

Pokud jste ještě sadu Visual Studio nikdy nepoužívali, projděte si článek [Připojení k SQL Data Warehouse pomocí sady Visual Studio][].  Začněte tím, že otevřete Průzkumník objektů systému SQL Server v sadě Visual Studio a připojíte se k serveru, který bude hostitelem databáze SQL Data Warehouse.  Po připojení můžete vytvořit SQL Data Warehouse spuštěním následujícího příkazu SQL na **hlavní** databázi.  Tento příkaz vytvoří databázi MySqlDwDb s cílem služby DW400 a umožní zvětšování databáze až do maximální velikosti 10 TB.

```sql
CREATE DATABASE MySqlDwDb (EDITION='datawarehouse', SERVICE_OBJECTIVE = 'DW400', MAXSIZE= 10240 GB);
```

## Vytvoření databáze pomocí sqlcmd

Alternativně můžete spustit stejný příkaz pomocí sqlcmd spuštěním následujícího příkazu na příkazovém řádku.

```sql
sqlcmd -S <Server Name>.database.windows.net -I -U <User> -P <Password> -Q "CREATE DATABASE MySqlDwDb (EDITION='datawarehouse', SERVICE_OBJECTIVE = 'DW400', MAXSIZE= 10240 GB)"
```

Parametry **MAXSIZE** a **SERVICE_OBJECTIVE** určují maximální místo na disku, jaké může databáze využívat, a výpočetní prostředky přidělené vaší instanci datového skladu.  Cíl služby v podstatě představuje přidělení procesoru a paměti, které se škáluje lineárně s velikostí DWU.  

MAXSIZE může být v rozmezí od 250 GB do 240 TB.  Cíl služby může být v rozmezí od DW100 do DW2000.  Úplný seznam všech platných hodnot pro parametr MAXSIZE a SERVICE_OBJECTIVE najdete v MSDN dokumentaci pro příkaz [CREATE DATABASE][].  Parametr MAXSIZE i parametr SERVICE_OBJECTIVE je také možné měnit pomocí příkazu [ALTER DATABASE][] jazyka T-SQL.  Při změně hodnoty parametru SERVICE_OBJECTIVE je potřeba postupovat opatrně, protože způsobí restartování služeb, což bude znamenat zrušení všech probíhajících dotazů.  Změna hodnoty parametru MAXSIZE nerestartuje služby, protože jde pouze o operaci s metadaty.

## Další kroky
Až se vám zřídí SQL Data Warehouse, můžete [načíst ukázková data][] nebo se můžete podívat, jak na [vývoj][], [načítání][] nebo [migraci][].

<!--Article references-->
[jak vytvořit SQL Data Warehouse z portálu Azure]: ./sql-data-warehouse-get-started-provision.md
[Připojení k SQL Data Warehouse pomocí sady Visual Studio]: ./sql-data-warehouse-get-started-connect.md
[migrace]: ./sql-data-warehouse-overview-migrate.md
[vývoj]: ./sql-data-warehouse-overview-develop.md
[načtení]: ./sql-data-warehouse-overview-load.md
[načíst ukázková data]: ./sql-data-warehouse-get-started-manually-load-samples.md
[Vytvoření logického serveru Azure SQL Database pomocí portálu Azure]: ../sql-database/sql-database-get-started.md#create-an-azure-sql-database-logical-server
[Vytvoření logického serveru Azure SQL Database pomocí prostředí PowerShell]: ../sql-database/sql-database-get-started-powershell.md#database-setup-create-a-resource-group-server-and-firewall-rule
[skupinám prostředků]: ../azure-portal/resource-group-portal.md
[Instalace sady Visual Studio a rozšíření SSDT]: ./sql-data-warehouse-install-visual-studio.md

<!--MSDN references--> 
[CREATE DATABASE]: https://msdn.microsoft.com/library/mt204021.aspx
[ALTER DATABASE]: https://msdn.microsoft.com/library/mt204042.aspx

<!--Other Web references-->
[SQL Data Warehouse – ceny]: https://azure.microsoft.com/pricing/details/sql-data-warehouse/
[bezplatné zkušební verzi Azure]: https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F
[Kredity Azure pro předplatitele MSDN]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F



<!--HONumber=Jun16_HO2-->


