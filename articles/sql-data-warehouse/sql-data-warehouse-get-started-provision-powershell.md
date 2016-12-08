---
title: "Vytvoření SQL Data Warehouse pomocí prostředí PowerShell | Dokumentace Microsoftu"
description: "Vytvoření SQL Data Warehouse pomocí prostředí PowerShell"
services: sql-data-warehouse
documentationcenter: NA
author: barbkess
manager: jhubbard
editor: 
ms.assetid: 97434863-7938-4129-8949-5a119f5949e3
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.date: 10/31/2016
ms.author: barbkess
translationtype: Human Translation
ms.sourcegitcommit: 5a101aa78dbac4f1a0edb7f414b44c14db392652
ms.openlocfilehash: 7107c64a25b3f60d7789cec57181eec9ce844f6b


---
# <a name="create-sql-data-warehouse-using-powershell"></a>Vytvoření SQL Data Warehouse pomocí prostředí PowerShell
> [!div class="op_single_selector"]
> * [Azure Portal](sql-data-warehouse-get-started-provision.md)
> * [TSQL](sql-data-warehouse-get-started-create-database-tsql.md)
> * [PowerShell](sql-data-warehouse-get-started-provision-powershell.md)
>
>

V tomto článku zjistíte, jak můžete k vytvoření SQL Data Warehouse použít PowerShell.

## <a name="prerequisites"></a>Požadavky
Na začátek budete potřebovat:

* **Účet Azure:** Pokud si chcete vytvořit účet, přečtěte si článek [Bezplatná zkušební verze Azure][Bezplatná zkušební verze Azure] nebo [Kredity Azure pro předplatitele MSDN][Kredity Azure pro předplatitele MSDN].
* **Server Azure SQL:** Další podrobnosti najdete v článku [Vytvoření logického serveru Azure SQL Database pomocí webu Azure Portal][Vytvoření logického serveru Azure SQL Database pomocí webu Azure Portal] nebo [Vytvoření logického serveru Azure SQL Database pomocí prostředí PowerShell][Vytvoření logického serveru Azure SQL Database pomocí prostředí PowerShell].
* **Skupinu prostředků:** Buď použijte stejnou skupinu prostředků jako pro Azure SQL Server, nebo zjistěte, [jak vytvořit skupinu prostředků][jak vytvořit skupinu prostředků].
* **Prostředí PowerShell verze 1.0.3 nebo novější:** To, jakou máte verzi, můžete zjistit spuštěním rutiny **Get-Module -ListAvailable -Name Azure**.  Nejnovější verzi si můžete nainstalovat pomocí [instalačního programu Webové platformy Microsoft][instalačního programu Webové platformy Microsoft].  Další informace o instalaci nejnovější verze najdete v tématu [Jak nainstalovat a nakonfigurovat Azure PowerShell][Jak nainstalovat a nakonfigurovat Azure PowerShell].

> [!NOTE]
> Vytvoření služby SQL Data Warehouse může znamenat, že se vám začne fakturovat nová služba.  Další podrobnosti o cenách najdete v tématu [SQL Data Warehouse – ceny][SQL Data Warehouse – ceny].
>
>

## <a name="create-a-sql-data-warehouse"></a>Vytvoření SQL Data Warehouse
1. Otevřete Windows PowerShell.
2. Spuštěním této rutiny se přihlaste do Azure Resource Manageru.

    ```Powershell
    Login-AzureRmAccount
    ```
3. Vyberte předplatné, které chcete použít pro aktuální relaci.

    ```Powershell
    Get-AzureRmSubscription    -SubscriptionName "MySubscription" | Select-AzureRmSubscription
    ```
4. Vytvořte databázi. Tento příklad vytvoří databázi s názvem mynewsqldw s úrovní cíle služby DW400 na serveru s názvem sqldwserver1, který je ve skupině prostředků s názvem mywesteuroperesgp1.

   ```Powershell
   New-AzureRmSqlDatabase -RequestedServiceObjectiveName "DW400" -DatabaseName "mynewsqldw" -ServerName "sqldwserver1" -ResourceGroupName "mywesteuroperesgp1" -Edition "DataWarehouse" -CollationName "SQL_Latin1_General_CP1_CI_AS" -MaxSizeBytes 10995116277760
   ```

Požadované parametry jsou:

* **RequestedServiceObjectiveName:** Počet jednotek [DWU][DWU], o které žádáte.  Podporované hodnoty: DW100, DW200, DW300, DW400, DW500, DW600, DW1000, DW1200, DW1500, DW2000, DW3000 a DW6000.
* **DatabaseName**: Název služby SQL Data Warehouse, kterou vytváříte.
* **ServerName**: Název serveru, který používáte pro vytvoření (musí to být server V12).
* **ResourceGroupName**: Skupina prostředků, kterou používáte.  K vyhledání dostupných skupin prostředků v rámci vašeho předplatného použijte rutinu Get-AzureResource.
* **Edition:** Aby bylo možné vytvořit SQL Data Warehouse, je nutné nastavit edici DataWarehouse.

Volitelné parametry jsou:

* **CollationName:** Pokud není uvedeno, je výchozí kolace SQL_Latin1_General_CP1_CI_AS.  Kolaci nejde pro databázi změnit.
* **MaxSizeBytes:** Výchozí maximální velikost databáze je 10 GB.

Další podrobnosti o možných parametrech najdete v tématech [New-AzureRmSqlDatabase][New-AzureRmSqlDatabase] a [Vytvoření databáze (Azure SQL Data Warehouse)][Vytvoření databáze (Azure SQL Data Warehouse)].

## <a name="next-steps"></a>Další kroky
Až se vám zřídí SQL Data Warehouse, můžete zkusit [načíst ukázková data][načtení ukázkových dat] nebo se můžete podívat, jak na [vývoj][vývoj], [načítání][načítání] nebo [migraci][migraci].

Pokud vás zajímají další informace o tom, jak SQL Data Warehouse spravovat prostřednictvím kódu programu, podívejte se na náš článek věnovaný tomu, jak používat [rutiny prostředí PowerShell a rozhraní REST API][Rutiny prostředí PowerShell a rozhraní REST API].

<!--Image references-->

<!--Article references-->
[DWU]: ./sql-data-warehouse-overview-what-is.md#data-warehouse-units
[migraci]: ./sql-data-warehouse-overview-migrate.md
[vývoj]: ./sql-data-warehouse-overview-develop.md
[načítání]: ./sql-data-warehouse-load-with-bcp.md
[načtení ukázkových dat]: ./sql-data-warehouse-load-sample-databases.md
[Rutiny prostředí PowerShell a rozhraní REST API]: ./sql-data-warehouse-reference-powershell-cmdlets.md
[Pravidla brány firewall]: ../sql-database-configure-firewall-settings.md

[Jak nainstalovat a nakonfigurovat Azure PowerShell]: ../powershell-install-configure.md
[Jak vytvořit SQL Data Warehouse z portálu Azure Portal]: ./sql-data-warehouse-get-started-provision.md
[Vytvoření logického serveru Azure SQL Database pomocí webu Azure Portal]: ../sql-database/sql-database-get-started.md#create-logical-server-bk
[Vytvoření logického serveru Azure SQL Database pomocí prostředí PowerShell]: ../sql-database/sql-database-get-started-powershell.md#database-setup-create-a-resource-group-server-and-firewall-rule
[jak vytvořit skupinu prostředků]: ../resource-group-template-deploy-portal.md#create-resource-group

<!--MSDN references-->
[MSDN]: https://msdn.microsoft.com/library/azure/dn546722.aspx
[New-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619339.aspx
[Vytvoření databáze (Azure SQL Data Warehouse)]: https://msdn.microsoft.com/library/mt204021.aspx

<!--Other Web references-->
[instalačního programu Webové platformy Microsoft]: https://aka.ms/webpi-azps
[SQL Data Warehouse – ceny]: https://azure.microsoft.com/pricing/details/sql-data-warehouse/
[Bezplatná zkušební verze Azure]: https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F
[Kredity Azure pro předplatitele MSDN]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F



<!--HONumber=Nov16_HO2-->


