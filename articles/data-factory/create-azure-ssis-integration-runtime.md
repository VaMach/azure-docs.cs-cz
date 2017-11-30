---
title: "Vytvoření vlastním hostováním integrace runtime v Azure Data Factory | Microsoft Docs"
description: "Zjistěte, jak můžete pomocí SQL Server aktivity uložené procedury vyvolat uloženou proceduru v databázi SQL Azure nebo Azure SQL Data Warehouse z objektu pro vytváření dat kanál."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/10/2017
ms.author: spelluru
ms.openlocfilehash: fcd2547112eb966420f33cec4939c83606029444
ms.sourcegitcommit: 29bac59f1d62f38740b60274cb4912816ee775ea
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/29/2017
---
# <a name="create-an-azure-ssis-integration-runtime-in-azure-data-factory"></a>Vytvoření modulu runtime integrace Azure SSIS v Azure Data Factory
Tento článek popisuje kroky pro zřizování modulu runtime integrace Azure SSIS v Azure Data Factory. Následně můžete pomocí SQL Server Data Tools (SSDT) nebo aplikace SQL Server Management Studio (SSMS) do tohoto modulu runtime v Azure nasadit balíčky SSIS (SQL Server Integration Services).

> [!NOTE]
> Tento článek se týká verze 2 služby Data Factory, která je aktuálně ve verzi Preview. Pokud používáte verzi 1 služby Data Factory, který je všeobecně dostupná (GA), prostudujte si [dokumentaci služby Data Factory verze 1](v1/data-factory-introduction.md).

Kurz: [kurz: nasadit balíčky SQL Server Integration Services (služby SSIS) do Azure](tutorial-deploy-ssis-packages-azure.md) ukazuje, jak vytvořit modul Runtime integrace Azure SSIS (IR) pomocí Azure SQL Database jako úložiště pro katalog služby SSIS. Tento článek se rozšíří na kurz a ukazuje, jak provést následující akce: 

- Použijte spravované Instance Azure SQL (soukromém náhledu) pro hostování katalog služby SSIS (databáze SSISDB).
- Připojení Azure SSIS Reakcí do virtuální sítě Azure (VNet). 

Koncepční informace o připojení Azure SSIS IR k virtuální síti a konfiguraci virtuální sítě na portálu Azure najdete v tématu [připojení k Azure SSIS Reakcí do virtuální sítě](join-azure-ssis-integration-runtime-virtual-network.md). 

## <a name="prerequisites"></a>Požadavky

- **Předplatné Azure**. Pokud předplatné nemáte, můžete si vytvořit [bezplatný zkušební](http://azure.microsoft.com/pricing/free-trial/) účet.
- **Server služby Azure SQL Database** nebo **spravovaná instance SQL Serveru (privátní verze Preview) (rozšířená verze Private Preview)**. Pokud ještě nemáte databázový server, vytvořte si ho na webu Azure Portal před tím, než začnete. Tento server hostuje databázi katalogu služby SSIS (SSISDB). Doporučujeme vytvořit databázový server ve stejné oblasti Azure jako prostředí Integration Runtime. Tato konfigurace umožňuje prostředí Integration Runtime zapisovat protokoly spuštění do databáze SSISDB bez přecházení mezi oblastmi Azure. Poznamenejte si cenové úrovně se server Azure SQL. Seznam podporovaných cenové úrovně pro databázi SQL Azure najdete v tématu [limitů prostředků databáze SQL](../sql-database/sql-database-resource-limits.md).
- **Klasická virtuální síť (volitelné)**. Virtuální síť Azure musíte mít v případě, že platí alespoň jedna z následujících podmínek:
    - Databázi katalogu služby SSIS hostujete na spravované instanci SQL Serveru (privátní verze Preview), která je součástí virtuální sítě.
    - Z balíčků SSIS spuštěných v prostředí Azure SSIS Integration Runtime se chcete připojit k místním úložištím dat.
- **Azure PowerShell**. Postupujte podle pokynů v tématu [Jak nainstalovat a nakonfigurovat Azure PowerShell](/powershell/azure/install-azurerm-ps). PowerShell použijete ke spuštění skriptu, který zřídí prostředí Azure SSIS Integration Runtime spouštějící balíčky SSIS v cloudu. 

> [!NOTE]
> Seznam oblastí podporovaných službou Azure Data Factory V2 a prostředím Azure SSIS Integration Runtime najdete v tématu [Dostupné produkty v jednotlivých oblastech](https://azure.microsoft.com/regions/services/). Rozbalením možnosti **Data a analýzy** zobrazíte **Data Factory V2** a **SSIS Integration Runtime**.


## <a name="create-variables"></a>Vytvoření proměnných
Definujte proměnné, které se použijí ve skriptech v tomto kurzu:

```powershell
# Azure Data Factory version 2 information 
# If your input contains a PSH special character, e.g. "$", precede it with the escape character "`" like "`$".
$SubscriptionName = "[your Azure subscription name]"
$ResourceGroupName = "[your Azure resource group name]"
$DataFactoryName = "[your data factory name]"
$DataFactoryLocation = "EastUS" 

# Azure-SSIS integration runtime information - This is the Data Factory compute resource for running SSIS packages
$AzureSSISName = "[your Azure-SSIS integration runtime name]"
$AzureSSISDescription = "This is my Azure-SSIS integration runtime"
$AzureSSISLocation = "EastUS" 
$AzureSSISNodeSize = "Standard_A4_v2" # In public preview, only Standard_A4_v2|Standard_A8_v2|Standard_D1_v2|Standard_D2_v2|Standard_D3_v2|Standard_D4_v2 are supported.
$AzureSSISNodeNumber = 2 # In public preview, only 1-10 nodes are supported.
$AzureSSISMaxParallelExecutionsPerNode = 2 # In public preview, only 1-8 parallel executions per node are supported.

# SSISDB info
$SSISDBServerEndpoint = "[your Azure SQL Database server name.database.windows.net or your Azure SQL Managed Instance (private preview) server endpoint]"
$SSISDBServerAdminUserName = "[your server admin username]"
$SSISDBServerAdminPassword = "[your server admin password]"

# Remove the SSISDBPricingTier variable if you are using Azure SQL Managed Instance (private preview)
# This parameter applies only to Azure SQL Database. For the basic pricing tier, specify "Basic", not "B". For standard tiers, specify "S0", "S1", "S2", 'S3", etc.
$SSISDBPricingTier = "[your Azure SQL Database pricing tier. Examples: Basic, S0, S1, S2, S3, etc.]"

## Remove these two variables if you are using Azure SQL Database. 
## These two parameters apply if you are using VNet and Azure SQL Managed Instance (private preview). 
$VnetId = "[your VNet resource ID or leave it empty]" # OPTIONAL: In public preview, only classic virtual network (VNet) is supported.
$SubnetName = "[your subnet name or leave it empty]" # OPTIONAL: In public preview, only classic VNet is supported.

```

## <a name="validate-the-connection-to-database"></a>Ověření připojení k databázi
Přidejte následující skript, který chcete ověřit vaše server.database.windows.net serveru Azure SQL Database nebo koncový bod serveru vaší spravované Instance SQL Azure (soukromém náhledu). 

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

## <a name="log-in-and-select-subscription"></a>Přihlášení a výběr předplatného
Přidejte následující kód skriptu se přihlaste a vyberte předplatné Azure: 

```powershell
Login-AzureRmAccount
Select-AzureRmSubscription -SubscriptionName $SubscriptionName
```

## <a name="configure-virtual-network"></a>Konfigurace virtuální sítě
Přidejte následující skript, který automaticky nakonfiguruje oprávnění a nastavení virtuální sítě, ke které se má vaše prostředí Azure SSIS Integration Runtime připojit.

```powershell
# Register to Azure Batch resource provider
if(![string]::IsNullOrEmpty($VnetId) -and ![string]::IsNullOrEmpty($SubnetName))
{
    $BatchObjectId = (Get-AzureRmADServicePrincipal -ServicePrincipalName "MicrosoftAzureBatch").Id
    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Batch
    while(!(Get-AzureRmResourceProvider -ProviderNamespace "Microsoft.Batch").RegistrationState.Contains("Registered"))
    {
    Start-Sleep -s 10
    }
    # Assign VM contributor role to Microsoft.Batch
    New-AzureRmRoleAssignment -ObjectId $BatchObjectId -RoleDefinitionName "Classic Virtual Machine Contributor" -Scope $VnetId
}
```

## <a name="create-a-resource-group"></a>Vytvoření skupiny prostředků
Vytvořte [skupinu prostředků Azure](../azure-resource-manager/resource-group-overview.md) pomocí příkazu [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup). Skupina prostředků je logický kontejner, ve kterém se nasazují a spravují prostředky jako skupina. Následující příklad vytvoří skupinu prostředků s názvem `myResourceGroup` v umístění `westeurope`.

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

Pokud používáte **Azure SQL Database** k hostování databáze SSISDB (SSIS katalog): 


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

Nemusíte předat hodnoty pro VNetId a podsíť, pokud budete potřebovat přístup k datům místně, tedy máte cíle zdroje dat na místě ve vašich balíčcích SSIS. Je nutné předat hodnotu pro parametr CatalogPricingTier. Seznam podporovaných cenové úrovně pro databázi SQL Azure najdete v tématu [limitů prostředků databáze SQL](../sql-database/sql-database-resource-limits.md).

Pokud používáte **spravované Instance Azure SQL (soukromém náhledu)** k hostování databáze SSISDB:

```powershell
$secpasswd = ConvertTo-SecureString $SSISDBServerAdminPassword -AsPlainText -Force
$serverCreds = New-Object System.Management.Automation.PSCredential($SSISDBServerAdminUserName, $secpasswd)
Set-AzureRmDataFactoryV2IntegrationRuntime  -ResourceGroupName $ResourceGroupName `
                                            -DataFactoryName $DataFactoryName `
                                            -Name $AzureSSISName `
                                            -Type Managed `
                                            -CatalogServerEndpoint $SSISDBServerEndpoint `
                                            -CatalogAdminCredential $serverCreds `
                                            -Description $AzureSSISDescription `
                                            -Location $AzureSSISLocation `
                                            -NodeSize $AzureSSISNodeSize `
                                            -NodeCount $AzureSSISNodeNumber `
                                            -MaxParallelExecutionsPerNode $AzureSSISMaxParallelExecutionsPerNode `
                                            -VnetId $VnetId `
                                            -Subnet $SubnetName
```

Je třeba předat hodnoty parametrů VnetId a podsíť s Azure spravované Instance SQL (soukromém náhledu) které připojí virtuální sítě. Parametr CatalogPricingTier nelze použít pro spravované Instance SQL Azure. 

## <a name="start-integration-runtime"></a>Spuštění prostředí Integration Runtime
Spuštěním následujícího příkazu spusťte prostředí Azure SSIS Integration Runtime: 

```powershell
write-host("##### Starting #####")
Start-AzureRmDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                             -DataFactoryName $DataFactoryName `
                                             -Name $AzureSSISName `
                                             -Force

write-host("##### Completed #####")
write-host("If any cmdlet is unsuccessful, please consider using -Debug option for diagnostics.")                                  
```
Dokončení tohoto příkazu trvá **20 až 30 minut**. 

## <a name="deploy-ssis-packages"></a>Nasazení balíčků SSIS
Teď použijte SQL Server Data Tools (SSDT) nebo aplikaci SQL Server Management Studio (SSMS) k nasazení vašich balíčků SSIS do Azure. Připojte se k serveru SQL Azure, který hostuje katalog služby SSIS (SSISDB). Název serveru SQL Azure je ve formátu &lt;název_serveru&gt;.database.windows.net (pro službu Azure SQL Database). Pokyny najdete v článku věnovaném [nasazení balíčků](/sql/integration-services/packages/deploy-integration-services-ssis-projects-and-packages#deploy-packages-to-integration-services-server). 

## <a name="next-steps"></a>Další kroky
Najdete v dalších tématech IR Azure SSIS v této dokumentaci:

- [Modul Runtime integrace Azure SSIS](concepts-integration-runtime.md#azure-ssis-integration-runtime). Tento článek obsahuje koncepční informace o integraci runtimes obecně včetně infračerveného signálu Azure SSIS. 
- [Kurz: Nasazení balíčků SSIS do Azure](tutorial-deploy-ssis-packages-azure.md) Tento článek obsahuje podrobné pokyny pro vytvoření Azure-SSIS IR a využívá databázi Azure SQL k hostování katalogu SSIS. 
- [Monitorování Azure-SSIS IR](monitor-integration-runtime.md#azure-ssis-integration-runtime). Tento článek ukazuje, jak načíst informace o Azure-SSIS IR a popisy stavů ve vrácených informacích. 
- [Správa Azure-SSIS IR](manage-azure-ssis-integration-runtime.md). Tento článek ukazuje, jak zastavit, spustit nebo odebrat Azure-SSIS IR. Ukazuje také postup horizontálního navýšení kapacity Azure-SSIS IR přidáním více uzlů. 
- [Připojení Azure-SSIS IR k virtuální síti](join-azure-ssis-integration-runtime-virtual-network.md) Tento článek obsahuje koncepční informace o připojení Azure-SSIS IR k virtuální síti Azure (VNet). Poskytuje také kroky pro využití webu Azure Portal ke konfiguraci virtuální sítě, aby se k ní prostředí Azure-SSIS IR mohlo připojit. 