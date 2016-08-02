<properties
   pageTitle="Vytvoření SQL Data Warehouse pomocí prostředí Powershell | Microsoft Azure"
   description="Vytvoření SQL Data Warehouse pomocí prostředí Powershell"
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
   ms.date="06/04/2016"
   ms.author="lodipalm;barbkess;sonyama"/>

# Vytvoření SQL Data Warehouse pomocí prostředí Powershell

> [AZURE.SELECTOR]
- [Portál Azure](sql-data-warehouse-get-started-provision.md)
- [TSQL](sql-data-warehouse-get-started-create-database-tsql.md)
- [PowerShell](sql-data-warehouse-get-started-provision-powershell.md)

## Požadavky
Než začnete, zkontrolujte, že jsou splněné následující požadavky.

- **Účet Azure:** Pokud si chcete vytvořit účet, najdete další informace v tématech věnovaných [bezplatné zkušební verzi Azure][] a [kreditům Azure pro předplatitele MSDN][].
- **Azure SQL Server V12:** Projděte si témata [Vytvoření logického serveru Azure SQL Database pomocí portálu Azure][] a [Vytvoření logického serveru Azure SQL Database pomocí prostředí PowerShell][].
- **Název skupiny prostředků:** Pokud chcete vytvořit novou skupinu prostředků, použijte buď stejnou skupinu prostředků jako pro SQL server Azure V12, nebo si projděte informace v tématu věnovaném [skupinám prostředků][].
- **Prostředí PowerShell verze 1.0.3 nebo novější:** To, jakou máte verzi, můžete zjistit spuštěním rutiny **Get-Module -ListAvailable -Name Azure**.  Nejnovější verzi si můžete nainstalovat pomocí [instalačního programu Webové platformy Microsoft][].  Další informace o instalaci nejnovější verze najdete v tématu [Jak nainstalovat a nakonfigurovat Azure PowerShell][].

> [AZURE.NOTE] Vytvoření nové služby SQL Data Warehouse může znamenat, že se vám začne fakturovat nová služba.  Další podrobnosti o cenách najdete v tématu [SQL Data Warehouse – ceny][].

## Vytvoření databáze SQL Data Warehouse
1. Otevřete Windows PowerShell.
2. Spuštěním této rutiny se přihlaste do Azure Resource Manageru.

    ```Powershell
    Login-AzureRmAccount
    ```
    
3. Vyberte předplatné, které chcete použít pro aktuální relaci.

    ```Powershell
    Get-AzureRmSubscription -SubscriptionName "MySubscription" | Select-AzureRmSubscription
    ```

4.  Vytvořte databázi. Tento příklad vytvoří novou databázi s názvem mynewsqldw s úrovní cíle služby DW400 na serveru s názvem sqldwserver1, který je ve skupině prostředků s názvem mywesteuroperesgp1.

    ```Powershell
    New-AzureRmSqlDatabase -RequestedServiceObjectiveName "DW400" -DatabaseName "mynewsqldw" -ServerName "sqldwserver1" -ResourceGroupName "mywesteuroperesgp1" -Edition "DataWarehouse"
    ```

Parametry požadované pro tuto rutinu:

- **RequestedServiceObjectiveName:** Počet jednotek DWU, o které žádáte, ve tvaru DWXXX. DWU představuje úroveň přidělení procesoru a paměti.  Každá hodnota DWU představuje lineární zvýšení těchto prostředků.  Aktuálně podporované hodnoty: 100, 200, 300, 400, 500, 600, 1000, 1200, 1500, 2000
- **DatabaseName**: Název služby SQL Data Warehouse, kterou vytváříte.
- **ServerName**: Název serveru, který používáte pro vytvoření (musí to být server V12).
- **ResourceGroupName**: Skupina prostředků, kterou používáte.  K vyhledání dostupných skupin prostředků v rámci vašeho předplatného použijte rutinu Get-AzureResource.
- **Edition:** Aby bylo možné vytvořit SQL Data Warehouse, je nutné nastavit edici DataWarehouse.

Další podrobnosti o možných parametrech najdete v tématu [Vytvoření databáze (Azure SQL Data Warehouse)][].
Přehled příkazů viz [New-AzureRmSqlDatabase][].

## Další kroky
Až se vám zřídí SQL Data Warehouse, můžete zkusit [načíst ukázková data][] nebo se můžete podívat, jak na [vývoj][], [načítání][] nebo [migraci][].

Pokud vás zajímají další informace o tom, jak SQL Data Warehouse spravovat prostřednictvím kódu programu, podívejte se na náš článek věnovaný tomu, jak používat [rutiny prostředí PowerShell a rozhraní REST API][].

<!--Image references-->

<!--Article references-->
[migrace]: ./sql-data-warehouse-overview-migrate.md
[vývoj]: ./sql-data-warehouse-overview-develop.md
[načtení]: ./sql-data-warehouse-load-with-bcp.md
[načíst ukázková data]: ./sql-data-warehouse-get-started-manually-load-samples.md
[rutiny prostředí PowerShell a rozhraní REST API]: ./sql-data-warehouse-reference-powershell-cmdlets.md
[pravidla brány firewall]: ./sql-database-configure-firewall-settings.md
[Jak nainstalovat a nakonfigurovat Azure PowerShell]: ../powershell/powershell-install-configure.md
[jak vytvořit SQL Data Warehouse z portálu Azure]: ./sql-data-warehouse-get-started-provision.md
[Vytvoření logického serveru Azure SQL Database pomocí portálu Azure]: ../sql-database/sql-database-get-started.md#create-an-azure-sql-database-logical-server
[Vytvoření logického serveru Azure SQL Database pomocí prostředí PowerShell]: ../sql-database/sql-database-get-started-powershell.md#database-setup-create-a-resource-group-server-and-firewall-rule
[skupinám prostředků]: ../azure-portal/resource-group-portal.md

<!--MSDN references--> 
[MSDN]:https://msdn.microsoft.com/library/azure/dn546722.aspx
[New-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619339.aspx
[Vytvoření databáze (Azure SQL Data Warehouse)]: https://msdn.microsoft.com/library/mt204021.aspx

<!--Other Web references-->
[instalačního programu Webové platformy Microsoft]: https://aka.ms/webpi-azps
[SQL Data Warehouse – ceny]: https://azure.microsoft.com/pricing/details/sql-data-warehouse/
[bezplatné zkušební verzi Azure]: https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F
[Kredity Azure pro předplatitele MSDN]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F



<!--HONumber=Jun16_HO2-->


