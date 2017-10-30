---
title: "Nasazení balíčků SSIS do Azure | Dokumentace Microsoftu"
description: "Tento článek vysvětluje, jak nasadit balíčky SSIS do prostředí Azure SSIS Integration Runtime poskytovaného službou Azure Data Factory."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: 
ms.devlang: powershell
ms.topic: hero-article
ms.date: 09/06/2017
ms.author: spelluru
ms.openlocfilehash: c7ae13cd07d9f85376b664a0d51564f90c35f97e
ms.sourcegitcommit: c5eeb0c950a0ba35d0b0953f5d88d3be57960180
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/24/2017
---
# <a name="deploy-sql-server-integration-services-packages-to-azure"></a>Nasazení balíčků SSIS (SQL Server Integration Services) do Azure
Azure Data Factory je cloudová služba pro integraci dat umožňující vytváření pracovních postupů řízených daty v cloudu za účelem orchestrace a automatizace přesunu a transformace dat. Pomocí služby Azure Data Factory můžete vytvářet a plánovat pracovní postupy řízené daty (nazývané kanály) se schopností ingestovat data z různorodých úložišť dat, zpracovat a transformovat tato data pomocí výpočetních služeb, jako je Azure HDInsight Hadoop, Spark, Azure Data Lake Analytics a Azure Machine Learning, a publikovat výstupní data do úložišť dat, jako je Azure SQL Data Warehouse, aby je mohly využívat aplikace business intelligence (BI). 

Tento kurz obsahuje postup pro zřízení prostředí Azure-SSIS Integration Runtime (IR) ve službě Azure Data Factory. Následně můžete pomocí SQL Server Data Tools (SSDT) nebo aplikace SQL Server Management Studio (SSMS) do tohoto modulu runtime v Azure nasadit balíčky SSIS (SQL Server Integration Services). V tomto kurzu provedete následující kroky:

> [!div class="checklist"]
> * Vytvoření datové továrny
> * Vytvoření prostředí Azure-SSIS Integration Runtime
> * Spuštění prostředí Azure-SSIS Integration Runtime
> * Nasazení balíčků SSIS
> * Kontrola celého skriptu

Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/) před tím, než začnete. Koncepční informace o Azure-SSIS IR najdete v [přehledu prostředí Azure-SSIS Integration Runtime](concepts-integration-runtime.md#azure-ssis-integration-runtime).

## <a name="prerequisites"></a>Požadavky
- **Server služby Azure SQL Database**. Pokud ještě nemáte databázový server, vytvořte si ho na webu Azure Portal před tím, než začnete. Tento server hostuje databázi katalogu služby SSIS (SSISDB). Doporučujeme vytvořit databázový server ve stejné oblasti Azure jako prostředí Integration Runtime. Tato konfigurace umožňuje prostředí Integration Runtime zapisovat protokoly spuštění do databáze SSISDB bez přecházení mezi oblastmi Azure. 
    - Ujistěte se, že nastavení **Povolit přístup ke službám Azure** je pro databázový server **zapnuté**. Další informace najdete v tématu [Zabezpečení databáze SQL Azure](../sql-database/sql-database-security-tutorial.md#create-a-server-level-firewall-rule-in-the-azure-portal). Pokud chcete toto nastavení povolit pomocí PowerShellu, přečtěte si téma věnované rutině [New-AzureRmSqlServerFirewallRule](/powershell/module/azurerm.sql/new-azurermsqlserverfirewallrule?view=azurermps-4.4.1).
    - Přidejte IP adresu klientského počítače nebo rozsah IP adres, který obsahuje IP adresu klientského počítače, do seznamu IP adres v nastavení brány firewall pro databázový server. Další informace najdete v tématu [Pravidla brány firewall na úrovni serveru a databáze služby Azure SQL Database](../sql-database/sql-database-firewall-configure.md). 
- **Azure PowerShell**. Postupujte podle pokynů v tématu [Jak nainstalovat a nakonfigurovat Azure PowerShell](/powershell/azure/install-azurerm-ps). PowerShell použijete ke spuštění skriptu, který zřídí prostředí Azure SSIS Integration Runtime spouštějící balíčky SSIS v cloudu. 

## <a name="launch-windows-powershell-ise"></a>Spuštění integrovaného skriptovacího prostředí (ISE) v prostředí Windows PowerShell
Spusťte **Integrované skriptovací prostředí (ISE) v prostředí Windows PowerShell** s oprávněními správce. 

## <a name="create-variables"></a>Vytvoření proměnných
Zkopírujte a vložte následující skript a zadejte hodnoty pro proměnné. Seznam podporovaných **cenových úrovní** pro službu Azure SQL Database najdete v tématu [Omezení prostředků služby SQL Database](../sql-database/sql-database-resource-limits.md).

```powershell
$SubscriptionName = "<Azure subscription name>"
$ResourceGroupName = "<Azure resource group name>"
# Data factory name. Must be globally unique
$DataFactoryName = "<Data factory name>" 
# In public preview, only EastUS amd EastUS2 are supported.
$DataFactoryLocation = "EastUS" 

# Azure-SSIS integration runtime information. This is a Data Factory compute resource for running SSIS packages
$AzureSSISName = "<Specify a name for your Azure-SSIS IR>"
$AzureSSISDescription = "<Specify description for your Azure-SSIS IR"
# In public preview, only EastUS and NorthEurope are supported.
$AzureSSISLocation = "EastUS" 
 # In public preview, only Standard_A4_v2, Standard_A8_v2, Standard_D1_v2, Standard_D2_v2, Standard_D3_v2, Standard_D4_v2 are supported
$AzureSSISNodeSize = "Standard_A4_v2"
# In public preview, only 1-10 nodes are supported.
$AzureSSISNodeNumber = 2 
# In public preview, only 1-8 parallel executions per node are supported.
$AzureSSISMaxParallelExecutionsPerNode = 2 

# SSISDB info
$SSISDBServerEndpoint = "<Azure SQL server name>.database.windows.net"
$SSISDBServerAdminUserName = "<Azure SQL server - user name>"
$SSISDBServerAdminPassword = "<Azure SQL server - user password>"
# Remove the SSISDBPricingTier variable if you are using Azure SQL Managed Instance (private preview)
# This parameter applies only to Azure SQL Database. For the basic pricing tier, specify "Basic", not "B". For standard tiers, specify "S0", "S1", "S2", 'S3", etc.
$SSISDBPricingTier = "<pricing tier of your Azure SQL server. Examples: Basic, S0, S1, S2, S3, etc.>" 
```

## <a name="validate-the-connection-to-database"></a>Ověření připojení k databázi
Přidejte následující skript pro ověření vašeho serveru služby Azure SQL Database – server.database.windows.net. 

```powershell
$SSISDBConnectionString = "Data Source=" + $SSISDBServerEndpoint + ";User ID="+ $SSISDBServerAdminUserName +";Password="+ $SSISDBServerAdminPassword
$sqlConnection = New-Object System.Data.SqlClient.SqlConnection $SSISDBConnectionString;
Try
{
    $sqlConnection.Open();
}
Catch [System.Data.SqlClient.SqlException]
{
    Write-Warning "Cannot connect to your Azure SQL DB logical server/Azure SQL MI server, exception: $_"  ;
    Write-Warning "Please make sure the server you specified has already been created. Do you want to proceed? [Y/N]"
    $yn = Read-Host
    if(!($yn -ieq "Y"))
    {
        Return;
    } 
}
```

Pokud chcete vytvořit databázi SQL Azure jako součást skriptu, podívejte se na následující příklad: 

Nastavte hodnoty pro proměnné, které ještě nejsou definované. Například pro FirewallIPAddress (IP adresa brány firewall). 

```powershell
New-AzureRmSqlServer -ResourceGroupName $ResourceGroupName `
  -ServerName $SQLServerName `
    -Location $DataFactoryLocation `
    -SqlAdministratorCredentials $(New-Object -TypeName System.Management.Automation.PSCredential -ArgumentList $SQLServerAdmin, $(ConvertTo-SecureString -String $SQLServerPass -AsPlainText -Force))

New-AzureRmSqlServerFirewallRule -ResourceGroupName $ResourceGroupName `
    -ServerName $SQLServerName `
    -FirewallRuleName "ClientIPAddress_$today" -StartIpAddress $FirewallIPAddress -EndIpAddress $FirewallIPAddress

New-AzureRmSqlServerFirewallRule -ResourceGroupName $ResourceGroupName -ServerName $SQLServerName -AllowAllAzureIPs
```


## <a name="log-in-and-select-subscription"></a>Přihlášení a výběr předplatného
Přidejte do skriptu následující kód pro přihlášení a výběr vašeho předplatného Azure: 

```powershell
Login-AzureRmAccount
Select-AzureRmSubscription -SubscriptionName $SubscriptionName
```

## <a name="create-a-resource-group"></a>Vytvoření skupiny prostředků
Vytvořte [skupinu prostředků Azure](../azure-resource-manager/resource-group-overview.md) pomocí příkazu [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup). Skupina prostředků je logický kontejner, ve kterém se nasazují a spravují prostředky jako skupina. Následující příklad vytvoří skupinu prostředků s názvem `myResourceGroup` v umístění `westeurope`.

Pokud vaše skupina prostředků už existuje, nekopírujte tento kód do vašeho skriptu. 

```powershell
New-AzureRmResourceGroup -Location $DataFactoryLocation -Name $ResourceGroupName
```

## <a name="create-a-data-factory"></a>Vytvoření datové továrny
Spuštěním následujícího příkazu vytvořte datovou továrnu:

```powershell
Set-AzureRmDataFactoryV2 -ResourceGroupName $ResourceGroupName `
                        -Location $DataFactoryLocation `
                        -Name $DataFactoryName
```

## <a name="create-an-integration-runtime"></a>Vytvoření prostředí Integration Runtime
Spuštěním následujícího příkazu vytvořte prostředí Azure SSIS Integration Runtime, které spouští balíčky SSIS v Azure: 

```powershell
$secpasswd = ConvertTo-SecureString $SSISDBServerAdminPassword -AsPlainText -Force
$serverCreds = New-Object System.Management.Automation.PSCredential($SSISDBServerAdminUserName, $secpasswd)
Set-AzureRmDataFactoryV2IntegrationRuntime  -ResourceGroupName $ResourceGroupName `
                                            -DataFactoryName $DataFactoryName `
                                            -Name $AzureSSISName `
                                            -Type Managed `
                                            -CatalogServerEndpoint $SSISDBServerEndpoint `
                                            -CatalogAdminCredential $serverCreds `
                                            -CatalogPricingTier $SSISDBPricingTier `
                                            -Description $AzureSSISDescription `
                                            -Location $AzureSSISLocation `
                                            -NodeSize $AzureSSISNodeSize `
                                            -NodeCount $AzureSSISNodeNumber `
                                            -MaxParallelExecutionsPerNode $AzureSSISMaxParallelExecutionsPerNode 
```

## <a name="start-integration-runtime"></a>Spuštění prostředí Integration Runtime
Spuštěním následujícího příkazu spusťte prostředí Azure SSIS Integration Runtime: 

```powershell
write-host("##### Starting your Azure-SSIS integration runtime. This command takes 20 to 30 minutes to complete. #####")
Start-AzureRmDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                             -DataFactoryName $DataFactoryName `
                                             -Name $AzureSSISName `
                                             -Force

write-host("##### Completed #####")
write-host("If any cmdlet is unsuccessful, please consider using -Debug option for diagnostics.")                                  
```
Dokončení tohoto příkazu trvá **20 až 30 minut**. 

## <a name="deploy-ssis-packages"></a>Nasazení balíčků SSIS
Teď použijte SQL Server Data Tools (SSDT) nebo aplikaci SQL Server Management Studio (SSMS) k nasazení vašich balíčků SSIS do Azure. Připojte se k serveru SQL Azure, který hostuje katalog služby SSIS (SSISDB). Název serveru SQL Azure je ve formátu `<servername>.database.windows.net` (pro službu Azure SQL Database). 

Projděte si následující články v dokumentaci ke službě SSIS: 

- [Nasazení, spuštění a monitorování balíčku SSIS v Azure](/sql/integration-services/lift-shift/ssis-azure-deploy-run-monitor-tutorial)   
- [Připojení ke katalogu SSIS v Azure](/sql/integration-services/lift-shift/ssis-azure-connect-to-catalog-database)
- [Plánování spouštění balíčku v Azure](/sql/integration-services/lift-shift/ssis-azure-schedule-packages)
- [Připojení k místním zdrojům dat s využitím ověřování systému Windows](/sql/integration-services/lift-shift/ssis-azure-connect-with-windows-auth) 

## <a name="full-script"></a>Celý skript
V této verzi musíte ke zřízení instance prostředí Azure SSIS Integration Runtime spouštějícího balíčky SSIS v cloudu použít PowerShell. V současné době není možné tento modul runtime zřídit pomocí webu Azure Portal. 

Skript PowerShellu v této části nakonfiguruje instanci prostředí Azure SSIS Integration Runtime v cloudu, které spouští balíčky SSIS. Po úspěšném spuštění tohoto skriptu můžete nasazovat a spouštět balíčky SSIS v cloudu Microsoft Azure s databází SSISDB hostovanou ve službě Azure SQL Database.

1. Spusťte integrované skriptovací prostředí (ISE) v prostředí Windows PowerShell.
2. V integrovaném skriptovacím prostředí (ISE) spusťte z příkazového řádku následující příkaz.    
    ```powershell
    Set-ExecutionPolicy Unrestricted -Scope CurrentUser
    ```
3. Zkopírujte skript PowerShellu v této části a vložte jej do integrovaného skriptovacího prostředí (ISE).
4. V části SSIS in Azure specifications (Specifikace služby SSIS v Azure) na začátku skriptu zadejte příslušné hodnoty pro parametry skriptu. Popis těchto parametrů najdete v další části.
5. Spusťte skript. Příkaz `Start-AzureRmDataFactoryV2IntegrationRuntime` u konce skriptu běží **20 až 30 minut**.

> [!NOTE]
> Skript se připojí k vaší službě Azure SQL Database a připraví databázi katalogu služby SSIS (SSISDB). Skript také nakonfiguruje oprávnění a nastavení vaší virtuální sítě, pokud je zadáte, a připojí k této virtuální síti novou instanci prostředí Azure SSIS Integration Runtime.

Seznam podporovaných **cenových úrovní** pro službu Azure SQL Database najdete v tématu [Omezení prostředků služby SQL Database](../sql-database/sql-database-resource-limits.md).

```powershell
$SubscriptionName = "<Azure subscription name>"
$ResourceGroupName = "<Azure resource group name>"
# Data factory name. Must be globally unique
$DataFactoryName = "<Data factory name>" 
# In public preview, only EastUS amd EastUS2 are supported.
$DataFactoryLocation = "EastUS" 

# Azure-SSIS integration runtime information. This is a Data Factory compute resource for running SSIS packages
$AzureSSISName = "<Specify a name for your Azure-SSIS (IR)>"
$AzureSSISDescription = "<Specify description for your Azure-SSIS IR"
# In public preview, only EastUS and NorthEurope are supported.
$AzureSSISLocation = "EastUS" 
 # In public preview, only Standard_A4_v2, Standard_A8_v2, Standard_D1_v2, Standard_D2_v2, Standard_D3_v2, Standard_D4_v2 are supported
$AzureSSISNodeSize = "Standard_A4_v2"
# In public preview, only 1-10 nodes are supported.
$AzureSSISNodeNumber = 2 
# In public preview, only 1-8 parallel executions per node are supported.
$AzureSSISMaxParallelExecutionsPerNode = 2 

# SSISDB info
$SSISDBServerEndpoint = "<Azure SQL server name>.database.windows.net"
$SSISDBServerAdminUserName = "<Azure SQL server - user name>"
$SSISDBServerAdminPassword = "<Azure SQL server - user password>"
# Remove the SSISDBPricingTier variable if you are using Azure SQL Managed Instance (private preview)
# This parameter applies only to Azure SQL Database. For the basic pricing tier, specify "Basic", not "B". For standard tiers, specify "S0", "S1", "S2", 'S3", etc.
$SSISDBPricingTier = "<pricing tier of your Azure SQL server. Examples: Basic, S0, S1, S2, S3, etc.>" 

$SSISDBConnectionString = "Data Source=" + $SSISDBServerEndpoint + ";User ID="+ $SSISDBServerAdminUserName +";Password="+ $SSISDBServerAdminPassword
$sqlConnection = New-Object System.Data.SqlClient.SqlConnection $SSISDBConnectionString;
Try
{
    $sqlConnection.Open();
}
Catch [System.Data.SqlClient.SqlException]
{
    Write-Warning "Cannot connect to your Azure SQL DB logical server/Azure SQL MI server, exception: $_"  ;
    Write-Warning "Please make sure the server you specified has already been created. Do you want to proceed? [Y/N]"
    $yn = Read-Host
    if(!($yn -ieq "Y"))
    {
        Return;
    } 
}

Login-AzureRmAccount
Select-AzureRmSubscription -SubscriptionName $SubscriptionName

Set-AzureRmDataFactoryV2 -ResourceGroupName $ResourceGroupName `
                        -Location $DataFactoryLocation `
                        -Name $DataFactoryName

$secpasswd = ConvertTo-SecureString $SSISDBServerAdminPassword -AsPlainText -Force
$serverCreds = New-Object System.Management.Automation.PSCredential($SSISDBServerAdminUserName, $secpasswd)
Set-AzureRmDataFactoryV2IntegrationRuntime  -ResourceGroupName $ResourceGroupName `
                                            -DataFactoryName $DataFactoryName `
                                            -Name $AzureSSISName `
                                            -Type Managed `
                                            -CatalogServerEndpoint $SSISDBServerEndpoint `
                                            -CatalogAdminCredential $serverCreds `
                                            -CatalogPricingTier $SSISDBPricingTier `
                                            -Description $AzureSSISDescription `
                                            -Location $AzureSSISLocation `
                                            -NodeSize $AzureSSISNodeSize `
                                            -NodeCount $AzureSSISNodeNumber `
                                            -MaxParallelExecutionsPerNode $AzureSSISMaxParallelExecutionsPerNode

write-host("##### Starting your Azure-SSIS integration runtime. This command takes 20 to 30 minutes to complete. #####")
Start-AzureRmDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                             -DataFactoryName $DataFactoryName `
                                             -Name $AzureSSISName `
                                             -Force

write-host("##### Completed #####")
write-host("If any cmdlet is unsuccessful, please consider using -Debug option for diagnostics.")
```

## <a name="join-azure-ssis-ir-to-a-vnet"></a>Připojení Azure-SSIS IR k virtuální síti
Pokud k hostování katalogu SSIS (SQL Server Integration Services) uvnitř virtuální sítě (VNet) používáte spravovanou instanci Azure SQL (Private Preview), musíte ke stejné virtuální síti připojit také Azure-SSIS Integration Runtime. Azure Data Factory verze 2 (Preview) umožňuje připojit Azure-SSIS Integration Runtime ke klasické virtuální síti. Další informace najdete v tématu [Připojení modulu runtime Azure-SSIS k virtuální síti](join-azure-ssis-integration-runtime-virtual-network.md).

Kompletní skript pro vytvoření modulu runtime Azure-SSIS pro připojení k virtuální síti najdete v tématu věnovaném [vytvoření prostředí Azure-SSIS Integration Runtime](create-azure-ssis-integration-runtime.md).

## <a name="monitor-and-manage-azure-ssis-ir"></a>Monitorování a správa Azure-SSIS IR
Podrobné informace o monitorování a správě Azure-SSIS IR najdete v následujících článcích. 

- [Monitorování prostředí Azure-SSIS Integration Runtime](monitor-integration-runtime.md#azure-ssis-integration-runtime)
- [Správa prostředí Azure-SSIS Integration Runtime](manage-azure-ssis-integration-runtime.md)

## <a name="next-steps"></a>Další kroky
V tomto kurzu jste se naučili: 

> [!div class="checklist"]
> * Vytvoření datové továrny
> * Vytvoření prostředí Azure-SSIS Integration Runtime
> * Spuštění prostředí Azure-SSIS Integration Runtime
> * Nasazení balíčků SSIS
> * Kontrola celého skriptu

Pokud se chcete dozvědět víc o kopírování dat z místního prostředí do cloudu, přejděte k následujícímu kurzu: 

> [!div class="nextstepaction"]
>[Kopírování dat v cloudu](tutorial-copy-data-dot-net.md)
