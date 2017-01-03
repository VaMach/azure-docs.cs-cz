---
title: "Správa služby Azure SQL Database pomocí PowerShellu | Dokumentace Microsoftu"
description: "Správa služby Azure SQL Database pomocí PowerShellu"
services: sql-database
documentationcenter: 
author: stevestein
manager: jhubbard
editor: monicar
ms.assetid: 3f21ad5e-ba99-4010-b244-5e5815074d31
ms.service: sql-database
ms.custom: overview
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 11/15/2016
ms.author: sstein
translationtype: Human Translation
ms.sourcegitcommit: adad6b8e27e0996559d5e6dacb8dd60fbf52a631
ms.openlocfilehash: 0c1ce1c29e447d9db4ef0df7873ef89cb835abee


---
# <a name="managing-azure-sql-database-using-powershell"></a>Správa služby Azure SQL Database pomocí PowerShellu
> [!div class="op_single_selector"]
> * [Azure Portal](sql-database-manage-portal.md)
> * [Transact-SQL (SSMS)](sql-database-manage-azure-ssms.md)
> * [PowerShell](sql-database-manage-powershell.md)
> 
> 

Toto téma popisuje rutiny PowerShellu, které slouží k provádění mnoha úkolů služby Azure SQL Database. Úplný seznam najdete v tématu [Rutiny služby Azure SQL Database](https://msdn.microsoft.com/library/mt574084\(v=azure.300\).aspx).

> [!TIP]
> Pokud potřebujete kurz, ve kterém se naučíte vytvořit server, vytvořit bránu firewall založenou na serveru, zobrazit vlastnosti serveru, připojit se k hlavní databázi a vytvářet v ní dotazy, vytvořit vzorovou databázi a prázdnou databázi nebo se připojit ke vzorové databázi a vytvářet v ní dotazy, přejděte na stránku [kurzu Začínáme](sql-database-get-started-powershell.md).
>

## <a name="how-do-i-create-a-resource-group"></a>Jak můžu vytvořit skupinu prostředků?
Chcete-li vytvořit skupinu prostředků pro službu SQL Database a související prostředky Azure, použijte rutinu [New-AzureRmResourceGroup](https://msdn.microsoft.com/library/azure/mt759837\(v=azure.300\).aspx).

```
$resourceGroupName = "resourcegroup1"
$resourceGroupLocation = "northcentralus"
New-AzureRmResourceGroup -Name $resourceGroupName -Location $resourceGroupLocation
```

Další informace najdete v tématu [Použití Azure PowerShellu s Azure Resource Managerem](../powershell-azure-resource-manager.md).
Úplný kurz najdete na stránce [Začínáme se servery, databázemi a pravidly brány firewall služby Azure SQL Database pomocí Azure PowerShellu](sql-database-get-started-powershell.md).

## <a name="how-do-i-create-a-sql-database-server"></a>Jak můžu vytvořit server služby SQL Database?
Server služby SQL Database můžete vytvořit pomocí rutiny [New-AzureRmSqlServer](https://msdn.microsoft.com/library/azure/mt603715\(v=azure.300\).aspx). Hodnotu *server1* nahraďte názvem vašeho serveru. Názvy serverů musí být u všech serverů v rámci služby Azure SQL Database jedinečné. Pokud se již název používá, obdržíte chybu. Provedení tohoto příkazu může trvat i několik minut. Skupina prostředků už musí existovat ve vašem předplatném.

```
$resourceGroupName = "resourcegroup1"

$sqlServerName = "server1"
$sqlServerVersion = "12.0"
$sqlServerLocation = "northcentralus"
$serverAdmin = "loginname"
$serverPassword = "password" 
$securePassword = ConvertTo-SecureString –String $serverPassword –AsPlainText -Force
$creds = New-Object –TypeName System.Management.Automation.PSCredential –ArgumentList $serverAdmin, $securePassword


$sqlServer = New-AzureRmSqlServer -ServerName $sqlServerName `
 -SqlAdministratorCredentials $creds -Location $sqlServerLocation `
 -ResourceGroupName $resourceGroupName -ServerVersion $sqlServerVersion
```

Další informace o serverech najdete v tématu [Funkce služby SQL Database](sql-database-features.md). Úplný kurz najdete na stránce [Začínáme se servery, databázemi a pravidly brány firewall služby Azure SQL Database pomocí Azure PowerShellu](sql-database-get-started-powershell.md).

## <a name="how-do-i-create-a-sql-database-server-firewall-rule"></a>Jak můžu vytvořit pravidlo brány firewall u serveru služby SQL Database?
Pokud chcete vytvořit pravidlo brány firewall pro přístup k serveru, použijte rutinu [New-AzureRmSqlServerFirewallRule](https://msdn.microsoft.com/library/azure/mt603860\(v=azure.300\).aspx). Spusťte následující příkaz a nahraďte počáteční a koncovou IP adresu hodnotami platnými pro vašeho klienta. Skupina prostředků a server už musí existovat ve vašem předplatném.

```
$resourceGroupName = "resourcegroup1"
$sqlServerName = "server1"

$firewallRuleName = "firewallrule1"
$firewallStartIp = "0.0.0.0"
$firewallEndIp = "255.255.255.255"

New-AzureRmSqlServerFirewallRule -ResourceGroupName $resourceGroupName `
 -ServerName $sqlServerName -FirewallRuleName $firewallRuleName `
 -StartIpAddress $firewallStartIp -EndIpAddress $firewallEndIp
```

Pokud chcete povolit přístup k vašemu serveru dalším službám Azure, vytvořte pravidlo brány firewall a nastavte parametry `-StartIpAddress` a `-EndIpAddress` na hodnotu **0.0.0.0**. Toto zvláštní pravidlo brány firewall povolí přístup k serveru všem službám Azure.

Další informace najdete v tématu [Brána firewall služby Azure SQL Database](https://msdn.microsoft.com/library/azure/ee621782.aspx). Úplný kurz najdete na stránce [Začínáme se servery, databázemi a pravidly brány firewall služby Azure SQL Database pomocí Azure PowerShellu](sql-database-get-started-powershell.md).

## <a name="how-do-i-create-a-sql-database"></a>Jak můžu vytvořit databázi služby SQL Database?
Databázi služby SQL Database můžete vytvořit pomocí rutiny [New-AzureRmSqlDatabase](https://msdn.microsoft.com/library/azure/mt619339\(v=azure.300\).aspx). Skupina prostředků a server už musí existovat ve vašem předplatném. 

```
$resourceGroupName = "resourcegroup1"
$sqlServerName = "server1"

$databaseName = "database1"
$databaseEdition = "Standard"
$databaseServiceLevel = "S0"

$currentDatabase = New-AzureRmSqlDatabase -ResourceGroupName $resourceGroupName `
 -ServerName $sqlServerName -DatabaseName $databaseName `
 -Edition $databaseEdition -RequestedServiceObjectiveName $databaseServiceLevel
```

Další informace najdete v tématu [Co je SQL Database](sql-database-technical-overview.md). Úplný kurz najdete na stránce [Začínáme se servery, databázemi a pravidly brány firewall služby Azure SQL Database pomocí Azure PowerShellu](sql-database-get-started-powershell.md).

## <a name="how-do-i-change-the-performance-level-of-a-sql-database"></a>Jak můžu změnit úroveň výkonu u databáze služby SQL Database?
Úroveň výkonu můžete změnit škálováním databáze pomocí rutiny [Set-AzureRmSqlDatabase](https://msdn.microsoft.com/library/azure/mt619433\(v=azure.300\).aspx). Skupina prostředků, server a databáze už musí existovat ve vašem předplatném. Jako hodnotu parametru `-RequestedServiceObjectiveName` pro úroveň Basic nastavte jednu mezeru (například jako v následujícím fragmentu kódu). Pro další úrovně použijte nastavení *S0*, *S1*, *P1*, *P6* a tak dále, podobně jako v předchozím příkladu.

```
$resourceGroupName = "resourcegroup1"
$sqlServerName = "server1"

$databaseName = "database1"
$databaseEdition = "Basic"
$databaseServiceLevel = " "

Set-AzureRmSqlDatabase -ResourceGroupName $resourceGroupName `
 -ServerName $sqlServerName -DatabaseName $databaseName `
 -Edition $databaseEdition -RequestedServiceObjectiveName $databaseServiceLevel
```

Další informace najdete v tématu [Možnosti a výkon služby SQL Database: co je dostupné v jednotlivých úrovních služby](sql-database-service-tiers.md). Vzorový skript najdete v tématu [Vzorový skript PowerShellu pro změnu úrovně služby a úrovně výkonu databáze služby SQL Database](sql-database-scale-up-powershell.md#sample-powershell-script-to-change-the-service-tier-and-performance-level-of-your-sql-database).

## <a name="how-do-i-copy-a-sql-database-to-the-same-server"></a>Jak můžu zkopírovat databázi služby SQL Database na stejný server?
Pokud chcete zkopírovat databázi služby SQL Database na stejný server, použijte rutinu [New-AzureRmSqlDatabaseCopy](https://msdn.microsoft.com/library/azure/mt603644\(v=azure.300\).aspx). V parametrech `-CopyServerName` a `-CopyResourceGroupName` nastavte stejné hodnoty pro server a skupinu prostředků jako u vaší zdrojové databáze.

```
$resourceGroupName = "resourcegroup1"
$sqlServerName = "server1"
$databaseName = "database1"

$copyDatabaseName = "database1_copy"
$copyServerName = $sqlServerName
$copyResourceGroupName = $resourceGroupName

New-AzureRmSqlDatabaseCopy -DatabaseName $databaseName `
 -ServerName $sqlServerName -ResourceGroupName $resourceGroupName `
 -CopyDatabaseName $copyDatabaseName -CopyServerName $sqlServerName `
 -CopyResourceGroupName $copyResourceGroupName
```

Další informace najdete v tématu [Kopírování databáze služby Azure SQL Database](sql-database-copy.md). Vzorový skript najdete v tématu [Skript PowerShellu pro kopírování databáze služby SQL Database](sql-database-copy-powershell.md#example-powershell-script).

## <a name="how-do-i-delete-a-sql-database"></a>Jak můžu odstranit databázi SQL?
Databázi služby SQL Database můžete odstranit pomocí rutiny [Remove-AzureRmSqlDatabase](https://msdn.microsoft.com/library/azure/mt619368\(v=azure.300\).aspx). Skupina prostředků, server a databáze už musí existovat ve vašem předplatném.

```
$resourceGroupName = "resourcegroup1"
$sqlServerName = "server1"
$databaseName = "database1"

Remove-AzureRmSqlDatabase -DatabaseName $databaseName `
 -ServerName $sqlServerName -ResourceGroupName $resourceGroupName
```

## <a name="how-do-i-delete-a-sql-database-server"></a>Jak můžu odstranit server služby SQL Database?
Server služby SQL Database můžete odstranit pomocí rutiny [Remove-AzureRmSqlServer](https://msdn.microsoft.com/library/azure/mt603488\(v=azure.300\).aspx).

```
$resourceGroupName = "resourcegroup1"
$sqlServerName = "server1"

Remove-AzureRmSqlServer -ServerName $sqlServerName -ResourceGroupName $resourceGroupName
```

## <a name="how-do-i-create-and-manage-elastic-pools-using-powershell"></a>Jak můžu vytvářet a spravovat elastické fondy pomocí PowerShellu?
Podrobnosti o vytváření elastických fondů v PowerShellu najdete v tématu [Vytvoření nového elastického fondu pomocí PowerShellu](sql-database-elastic-pool-create-powershell.md).

Podrobnosti o správě elastických fondů v PowerShellu najdete v tématu [Monitorování a správa elastického fondu pomocí PowerShellu](sql-database-elastic-pool-manage-powershell.md).

## <a name="related-information"></a>Související informace
* [Rutiny Azure SQL Database](https://msdn.microsoft.com/library/azure/mt574084\(v=azure.300\).aspx)
* [Referenční informace k rutinám Azure](https://msdn.microsoft.com/library/azure/dn708514\(v=azure.300\).aspx)




<!--HONumber=Jan17_HO1-->


