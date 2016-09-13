<properties
    pageTitle="Vytvoření nového fondu elastické databáze pomocí prostředí PowerShell | Microsoft Azure"
    description="Zjistěte, jak pomocí prostředí PowerShell zajistit horizontální navyšování kapacity prostředků Azure SQL Database vytvořením škálovatelného fondu elastické databáze pro správu více databází."
    services="sql-database"
    documentationCenter=""
    authors="srinia"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="powershell"
    ms.workload="data-management"
    ms.date="05/27/2016"
    ms.author="srinia"/>

# Vytvoření nového fondu elastické databáze pomocí prostředí PowerShell

> [AZURE.SELECTOR]
- [Azure Portal](sql-database-elastic-pool-create-portal.md)
- [PowerShell](sql-database-elastic-pool-create-powershell.md)
- [C#](sql-database-elastic-pool-create-csharp.md)


Naučte se vytvářet [fond elastické databáze](sql-database-elastic-pool.md) pomocí rutin PowerShell. 

Běžné kódy chyb naleznete v článku [Kódy chyb SQL pro klientské aplikace SQL Database: chyby připojení k databázi a další problémy](sql-database-develop-error-messages.md).

> [AZURE.NOTE] Elastické fondy jsou obecně dostupné ve všech oblastech Azure s výjimkou oblastí Střed USA – sever a Západní Indie, kde jsou aktuálně ve verzi Preview.  Obecná dostupnost elastických fondů v těchto oblastech bude zajištěna co nejdříve. Elastické fondy také v současné době nepodporují databáze [využívající OLTP nebo analýzu v paměti](sql-database-in-memory.md).


Budete potřebovat nainstalované a spuštěné prostředí Azure PowerShell 1.0 nebo novější. Podrobné informace najdete v tématu [Instalace a konfigurace prostředí Azure PowerShell](../powershell-install-configure.md).

## Vytvoření nového fondu

Nový fond vytváří rutina [New-AzureRmSqlElasticPool](https://msdn.microsoft.com/library/azure/mt619378.aspx). Hodnoty eDTU na fond a minimální a maximální DTU jsou omezeny úrovní služeb (Basic, Standard nebo Premium). Viz odstavec [Omezení eDTU a úložiště pro elastické fondy a elastické databáze](sql-database-elastic-pool.md#eDTU-and-storage-limits-for-elastic-pools-and-elastic-databases).

    New-AzureRmSqlElasticPool -ResourceGroupName "resourcegroup1" -ServerName "server1" -ElasticPoolName "elasticpool1" -Edition "Standard" -Dtu 400 -DatabaseDtuMin 10 -DatabaseDtuMax 100


## Vytvoření nové elastické databáze ve fondu

Použijte rutinu [New-AzureRmSqlDatabase](https://msdn.microsoft.com/library/azure/mt619339.aspx) a nastavte parametr **ElasticPoolName** na cílový fond. Chcete-li do fondu přesunout existující databázi, přečtěte si část [Přesun databáze do pružného fondu](sql-database-elastic-pool-manage-powershell.md#Move-a-database-into-an-elastic-pool).

    New-AzureRmSqlDatabase -ResourceGroupName "resourcegroup1" -ServerName "server1" -DatabaseName "database1" -ElasticPoolName "elasticpool1"

## Vytvoření fondu a přidání více nových databází 

Vytvoření většího počtu databází ve fondu může trvat delší dobu, pokud se provádí pomocí portálu nebo rutiny prostředí PowerShell jednu databázi po druhé. K automatizaci vytváření databází v novém fondu může posloužit rutina [CreateOrUpdateElasticPoolAndPopulate](https://gist.github.com/billgib/d80c7687b17355d3c2ec8042323819ae).   

## Příklad: vytvoření fondu pomocí prostředí PowerShell 

Tento skript vytvoří novou skupinu prostředků Azure a nový server. Po výzvě zadejte jméno a heslo správce pro nový server (nikoli vaše přihlašovací údaje Azure).

    $subscriptionId = '<your Azure subscription id>'
    $resourceGroupName = '<resource group name>'
    $location = '<datacenter location>'
    $serverName = '<server name>'
    $poolName = '<pool name>'
    $databaseName = '<database name>'

    Login-AzureRmAccount
    Set-AzureRmContext -SubscriptionId $subscriptionId

    New-AzureRmResourceGroup -Name $resourceGroupName -Location $location
    New-AzureRmSqlServer -ResourceGroupName $resourceGroupName -ServerName $serverName -Location $location -ServerVersion "12.0"
    New-AzureRmSqlServerFirewallRule -ResourceGroupName $resourceGroupName -ServerName $serverName -FirewallRuleName "rule1" -StartIpAddress "192.168.0.198" -EndIpAddress "192.168.0.199"

    New-AzureRmSqlElasticPool -ResourceGroupName $resourceGroupName -ServerName $serverName -ElasticPoolName $poolName -Edition "Standard" -Dtu 400 -DatabaseDtuMin 10 -DatabaseDtuMax 100

    New-AzureRmSqlDatabase -ResourceGroupName $resourceGroupName -ServerName $serverName -DatabaseName $databaseName -ElasticPoolName $poolName -MaxSizeBytes 10GB



## Další kroky

- [Správa fondu](sql-database-elastic-pool-manage-powershell.md)
- [Vytvoření elastických úloh](sql-database-elastic-jobs-overview.md): Elastické úlohy umožňují spouštění skriptů T-SQL pro libovolný počet databází ve fondu.
- [Horizontální navýšení kapacity s Azure SQL Database](sql-database-elastic-scale-introduction.md): Používejte nástroje elastické databáze k horizontálnímu navýšení kapacity.




<!--HONumber=sep16_HO1-->


