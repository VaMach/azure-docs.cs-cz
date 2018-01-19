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
ms.openlocfilehash: 7636f502a7dc631b96c3f091a6622c7db301b035
ms.sourcegitcommit: 2a70752d0987585d480f374c3e2dba0cd5097880
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/19/2018
---
# <a name="create-an-azure-ssis-integration-runtime-in-azure-data-factory"></a>Vytvoření modulu runtime integrace Azure SSIS v Azure Data Factory
Tento článek popisuje kroky pro zřizování modulu runtime integrace Azure SSIS v Azure Data Factory. Následně můžete pomocí SQL Server Data Tools (SSDT) nebo aplikace SQL Server Management Studio (SSMS) do tohoto modulu runtime v Azure nasadit balíčky SSIS (SQL Server Integration Services).

Kurz: [kurz: nasadit balíčky SQL Server Integration Services (služby SSIS) do Azure](tutorial-deploy-ssis-packages-azure.md) ukázal, jak vytvořit modul Runtime integrace Azure SSIS (IR) pomocí Azure SQL Database jako úložiště pro katalog služby SSIS. Tento článek se rozšíří na kurz a ukazuje, jak provést následující akce: 

- Použijte spravované Instance Azure SQL (soukromém náhledu) pro hostování katalog služby SSIS (databáze SSISDB).
- Připojení Azure SSIS Reakcí do virtuální sítě Azure (VNet). Koncepční informace o připojení Azure SSIS IR k virtuální síti a konfiguraci virtuální sítě na portálu Azure najdete v tématu [připojení k Azure SSIS Reakcí do virtuální sítě](join-azure-ssis-integration-runtime-virtual-network.md). 

> [!NOTE]
> Tento článek se týká verze 2 služby Data Factory, která je aktuálně ve verzi Preview. Pokud používáte verzi 1 služby Data Factory, který je všeobecně dostupná (GA), prostudujte si [dokumentaci služby Data Factory verze 1](v1/data-factory-introduction.md).


## <a name="overview"></a>Přehled
Tento článek ukazuje různé způsoby zřizování Azure SSIS IR:

- [portál Azure Portal](#azure-portal)
- [Azure PowerShell](#azure-powershell)
- [Šablona Azure Resource Manageru](#azure-resource-manager-template)

Při vytváření služby Azure SSIS IR objekt pro vytváření dat se připojuje k vaší databázi SQL Azure Příprava databáze katalogu služby SSIS (SSISDB). Skript také nakonfiguruje oprávnění a nastavení vaší virtuální sítě, pokud je zadáte, a připojí k této virtuální síti novou instanci prostředí Azure SSIS Integration Runtime.

Pokud zřídíte instanci databáze SQL pro hostování SSISDB, jsou nainstalovány také Azure Feature Pack pro službu SSIS a Redistributable přístup. Tyto součásti poskytovat připojení aplikace Excel a přístup k souborům a různých zdrojů dat Azure, kromě zdrojů dat, který nepodporuje integrované komponenty. V tuto chvíli (včetně komponenty jiných výrobců od Microsoftu, jako je Oracle a Teradata součásti od společnosti Attunity a součásti prostředí SAP BI) nelze nainstalovat komponenty třetích stran pro SSIS.

## <a name="prerequisites"></a>Požadavky

- **Předplatné Azure**. Pokud předplatné nemáte, můžete si vytvořit [bezplatný zkušební](http://azure.microsoft.com/pricing/free-trial/) účet.
- **Server služby Azure SQL Database** nebo **spravovaná instance SQL Serveru (privátní verze Preview) (rozšířená verze Private Preview)**. Pokud ještě nemáte databázový server, vytvořte si ho na webu Azure Portal před tím, než začnete. Tento server hostuje databázi katalogu služby SSIS (SSISDB). Doporučujeme vytvořit databázový server ve stejné oblasti Azure jako prostředí Integration Runtime. Tato konfigurace umožňuje prostředí Integration Runtime zapisovat protokoly spuštění do databáze SSISDB bez přecházení mezi oblastmi Azure. Poznamenejte si cenové úrovně se server Azure SQL. Seznam podporovaných cenové úrovně pro databázi SQL Azure najdete v tématu [limitů prostředků databáze SQL](../sql-database/sql-database-resource-limits.md).

    Potvrďte, že serveru Azure SQL Database nebo spravované Instance systému SQL Server (rozšířené soukromém náhledu) nemá katalog služby SSIS (SSIDB databáze). Zřizování IR Azure SSIS nepodporuje použití existující katalog služby SSIS.
- **Klasická virtuální síť (volitelné)**. Virtuální síť Azure musíte mít v případě, že platí alespoň jedna z následujících podmínek:
    - Databázi katalogu služby SSIS hostujete na spravované instanci SQL Serveru (privátní verze Preview), která je součástí virtuální sítě.
    - Z balíčků SSIS spuštěných v prostředí Azure SSIS Integration Runtime se chcete připojit k místním úložištím dat.
- **Azure PowerShell**. Postupujte podle pokynů v tématu [Jak nainstalovat a nakonfigurovat Azure PowerShell](/powershell/azure/install-azurerm-ps). PowerShell použijete ke spuštění skriptu, který zřídí prostředí Azure SSIS Integration Runtime spouštějící balíčky SSIS v cloudu. 

> [!NOTE]
> Seznam oblastí podporovaných službou Azure Data Factory V2 a prostředím Azure SSIS Integration Runtime najdete v tématu [Dostupné produkty v jednotlivých oblastech](https://azure.microsoft.com/regions/services/). Rozbalením možnosti **Data a analýzy** zobrazíte **Data Factory V2** a **SSIS Integration Runtime**.

## <a name="azure-portal"></a>Azure Portal
V této části použijete portál Azure, konkrétně Data Factory uživatelského rozhraní, k vytvoření služby Azure SSIS infračerveného signálu. 

### <a name="create-a-data-factory"></a>Vytvoření datové továrny

1. Přihlaste se k portálu [Azure Portal](https://portal.azure.com/).    
2. V nabídce vlevo klikněte na **Nový**, klikněte na **Data + analýzy** a pak na **Data Factory**. 
   
   ![Nový -> Objekt pro vytváření dat](./media/tutorial-create-azure-ssis-runtime-portal/new-data-factory-menu.png)
3. V **nový objekt pro vytváření dat** zadejte **MyAzureSsisDataFactory** pro **název**. 
      
     ![Nová stránka objektu pro vytváření dat](./media/tutorial-create-azure-ssis-runtime-portal/new-azure-data-factory.png)
 
   Název objektu pro vytváření dat Azure musí být **globálně jedinečný**. Pokud se zobrazí chybová zpráva, změňte název objektu pro vytváření dat (například yournameMyAzureSsisDataFactory) a zkuste to znovu. V tématu [pro vytváření dat – pravidla pojmenování](naming-rules.md) článku pravidla pojmenování artefaktů služby Data Factory.
  
       `Data factory name “MyAzureSsisDataFactory” is not available`
3. Vyberte své **předplatné** Azure, ve kterém chcete vytvořit datovou továrnu. 
4. Pro **Skupinu prostředků** proveďte jeden z následujících kroků:
     
      - Vyberte **Použít existující** a z rozevíracího seznamu vyberte existující skupinu prostředků. 
      - Vyberte **Vytvořit novou** a zadejte název skupiny prostředků.   
         
      Informace o skupinách prostředků najdete v článku [Použití skupin prostředků ke správě prostředků Azure](../azure-resource-manager/resource-group-overview.md).  
4. Jako **verzi** vyberte **V2 (Preview)**.
5. Vyberte **umístění** pro objekt pro vytváření dat. V seznamu jsou uvedeny pouze umístění, které jsou podporovány pro vytvoření datové továrny.
6. Zaškrtněte **Připnout na řídicí panel**.     
7. Klikněte na možnost **Vytvořit**.
8. Na řídicím panelu vidíte následující dlaždice se statusem: **Nasazování datové továrny**. 

    ![nasazování dlaždice datové továrny](media/tutorial-create-azure-ssis-runtime-portal/deploying-data-factory.png)
9. Po dokončení vytvoření se zobrazí **Data Factory** stránky, jak je znázorněno na obrázku.
   
   ![Domovská stránka objektu pro vytváření dat](./media/tutorial-create-azure-ssis-runtime-portal/data-factory-home-page.png)
10. Klikněte na tlačítko **Autor & monitorování** ke spuštění na Data objektu pro vytváření uživatele rozhraní (UI) na samostatné kartě. 

### <a name="provision-an-azure-ssis-integration-runtime"></a>Zřídit modulu runtime integrace Azure SSIS

1. Na stránku Začínáme, klikněte na **konfigurace Runtime integrační služby SSIS** dlaždici. 

   ![Konfigurace služby SSIS integrace Runtime dlaždice](./media/tutorial-create-azure-ssis-runtime-portal/configure-ssis-integration-runtime-tile.png)
2. V **obecné nastavení** stránka **nastavení integrace modulu Runtime**, proveďte následující kroky: 

   ![Obecná nastavení](./media/tutorial-create-azure-ssis-runtime-portal/general-settings.png)

    1. Zadejte **název** pro integraci modulu runtime.
    2. Vyberte **umístění** pro integraci modulu runtime. Zobrazí se pouze podporovaných umístění.
    3. Vyberte **velikost uzlu** který má být nakonfigurován s modulem runtime SSIS.
    4. Zadejte **počet uzlů** v clusteru.
    5. Klikněte na **Další**. 
1. V **nastavení SQL**, proveďte následující kroky: 

    ![Nastavení SQL](./media/tutorial-create-azure-ssis-runtime-portal/sql-settings.png)

    1. Zadejte Azure **předplatné** který má server Azure SQL. 
    2. Vyberte server Azure SQL pro **koncový bod serveru databáze katalogu**.
    3. Zadejte **správce** uživatelské jméno.
    4. Zadejte **heslo** pro správce.  
    5. Vyberte **vrstvy služby** pro databázi SSISDB. Výchozí hodnota je Basic.
    6. Klikněte na **Další**. 
1.  V **Upřesnit nastavení** vyberte hodnotu **maximální paralelní spuštěních podle uzlu**.   

    ![Upřesnit nastavení](./media/tutorial-create-azure-ssis-runtime-portal/advanced-settings.png)    
5. Tento krok je **volitelné**. Pokud máte klasickou virtuální síť (VNet), který chcete runtime integrace se chcete připojit, vyberte **vyberte virtuální síť pro vaše runtime integrace Azure SSIS připojit a povolit službám Azure ke konfiguraci oprávnění nebo nastavení sítě VNet** možnost a proveďte následující kroky: 

    ![Pokročilé nastavení s virtuální sítě](./media/tutorial-create-azure-ssis-runtime-portal/advanced-settings-vnet.png)    

    1. Zadejte **předplatné** má klasické virtuální sítě. 
    2. Vyberte **VNet**. <br/>
    4. Vyberte **podsítě**.<br/> 
1. Klikněte na tlačítko **Dokončit** zahájíte vytváření runtime integrace Azure SSIS. 

    > [!IMPORTANT]
    > - Tento proces trvá přibližně 20 minut
    > - Služba Data Factory se připojuje k vaší databázi SQL Azure Příprava databáze katalogu služby SSIS (SSISDB). Skript také nakonfiguruje oprávnění a nastavení vaší virtuální sítě, pokud je zadáte, a připojí k této virtuální síti novou instanci prostředí Azure SSIS Integration Runtime.
7. V **připojení** okně přepínač tak, aby **integrační moduly runtime** v případě potřeby. Klikněte na tlačítko **aktualizovat** aktualizovat stav. 

    ![Stav vytváření](./media/tutorial-create-azure-ssis-runtime-portal/azure-ssis-ir-creation-status.png)
8. Použijte odkazy v části **akce** sloupce, které chcete monitorovat, zastavit a spustit, upravit nebo odstranit modul runtime integrace. Použijte poslední odkaz zobrazíte kód JSON pro integraci modulu runtime. Tlačítka Upravit a odstranit jsou povolené jenom v případě, že Reakcí je zastavena. 

    ![Azure SSIS IR - akce](./media/tutorial-create-azure-ssis-runtime-portal/azure-ssis-ir-actions.png)        
9. Klikněte na tlačítko **monitorování** v části **akce**.  

    ![Azure IR SSIS – podrobnosti](./media/tutorial-create-azure-ssis-runtime-portal/azure-ssis-ir-details.png)
10. Pokud dojde **chyba** spojené s Reakcí SSIS Azure, uvidíte počet chyb na této stránce a na odkaz zobrazíte podrobnosti o této chybě. Například pokud katalog služby SSIS již existuje na serveru databáze, zobrazí chybu, která určuje existenci databázi SSISDB.  
11. Klikněte na tlačítko **integrační moduly runtime** v horní části přejděte zpět na předchozí stránku zobrazíte všechny moduly integrace runtime přidružené k objektu pro vytváření dat k položce.  

### <a name="azure-ssis-integration-runtimes-in-the-portal"></a>Azure integrační moduly runtime SSIS na portálu

1. V uživatelském rozhraní Azure Data Factory přepnout **upravit** , klikněte na **připojení**a potom přepnout na **integrační moduly runtime** zobrazte existující integrační moduly Runtime v vaší služby data factory. 
    ![Finanční úřad stávajících zobrazení](./media/tutorial-create-azure-ssis-runtime-portal/view-azure-ssis-integration-runtimes.png)
1. Klikněte na tlačítko **nový** k vytvoření nové infračerveného signálu Azure SSIS. 

    ![Modul runtime integrace pomocí nabídky](./media/tutorial-create-azure-ssis-runtime-portal/edit-connections-new-integration-runtime-button.png)
2. Chcete-li vytvořit modulu runtime integrace Azure SSIS, klikněte na tlačítko **nový** jak je znázorněno na obrázku. 
3. V okně Nastavení integrace modulu Runtime, vyberte **navýšení a shift existující balíčky SSIS spuštění v Azure**a potom klikněte na **Další**.

    ![Zadejte typ integrace modulu runtime](./media/tutorial-create-azure-ssis-runtime-portal/integration-runtime-setup-options.png)
4. Najdete v článku [zřídit modulu runtime integrace Azure SSIS](#provision-an-azure-ssis-integration-runtime) části pro zbývající kroky k nastavení služby Azure SSIS infračerveného signálu.

## <a name="azure-powershell"></a>Azure PowerShell
V této části použijte prostředí Azure PowerShell k vytvoření služby Azure SSIS infračerveného signálu.

### <a name="create-variables"></a>Vytvoření proměnných
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
# In public preview, only Standard_A4_v2|Standard_A8_v2|Standard_D1_v2|Standard_D2_v2|Standard_D3_v2|Standard_D4_v2 are supported.
$AzureSSISNodeSize = "Standard_A4_v2" 
# In public preview, only 1-10 nodes are supported.
$AzureSSISNodeNumber = 2 
# In public preview, only 1-8 parallel executions per node are supported.
$AzureSSISMaxParallelExecutionsPerNode = 2 

# SSISDB info
$SSISDBServerEndpoint = "[your Azure SQL Database server name.database.windows.net or your Azure SQL Managed Instance (private preview) server endpoint]"
$SSISDBServerAdminUserName = "[your server admin username]"
$SSISDBServerAdminPassword = "[your server admin password]"

# Remove the SSISDBPricingTier variable if you are using Azure SQL Managed Instance (private preview)
# This parameter applies only to Azure SQL Database. For the basic pricing tier, specify "Basic", not "B". For standard tiers, specify "S0", "S1", "S2", 'S3", etc.
$SSISDBPricingTier = "[your Azure SQL Database pricing tier. Examples: Basic, S0, S1, S2, S3, etc.]"

# Remove these the following two OPTIONAL variables if you are using Azure SQL Database. 
# These two parameters apply if you are using VNet and Azure SQL Managed Instance (private preview). 
# Get the following information from the properties page for your Classic Virtual Network in the Azure portal
# It should be in the format: $VnetId = "/subscriptions/<Azure Subscription ID>/resourceGroups/<Azure Resource Group>/providers/Microsoft.ClassicNetwork/virtualNetworks/<Class Virtual Network Name>"

# OPTIONAL: In public preview, only classic virtual network (VNet) is supported.
$VnetId = "[your VNet resource ID or leave it empty]" 
$SubnetName = "[your subnet name or leave it empty]" 

```
### <a name="log-in-and-select-subscription"></a>Přihlášení a výběr předplatného
Přidejte následující kód skriptu se přihlaste a vyberte předplatné Azure: 

```powershell
Login-AzureRmAccount
Select-AzureRmSubscription -SubscriptionName $SubscriptionName
```

### <a name="validate-the-connection-to-database"></a>Ověření připojení k databázi
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

### <a name="configure-virtual-network"></a>Konfigurace virtuální sítě
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

### <a name="create-a-resource-group"></a>Vytvoření skupiny prostředků
Vytvořte [skupinu prostředků Azure](../azure-resource-manager/resource-group-overview.md) pomocí příkazu [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup). Skupina prostředků je logický kontejner, ve kterém se nasazují a spravují prostředky jako skupina. Následující příklad vytvoří skupinu prostředků s názvem `myResourceGroup` v umístění `westeurope`.

```powershell
New-AzureRmResourceGroup -Location $DataFactoryLocation -Name $ResourceGroupName
```

### <a name="create-a-data-factory"></a>Vytvoření datové továrny
Spuštěním následujícího příkazu vytvořte datovou továrnu:

```powershell
Set-AzureRmDataFactoryV2 -ResourceGroupName $ResourceGroupName `
                         -Location $DataFactoryLocation `
                         -Name $DataFactoryName
```

### <a name="create-an-integration-runtime"></a>Vytvoření prostředí Integration Runtime
Spusťte následující příkaz k vytvoření modulu runtime integrace Azure SSIS, který běží v Azure balíčky SSIS: pomocí tohoto skriptu z části na základě typu databáze (Azure SQL Database vs. Azure SQL spravované Instance (soukromém náhledu)) používáte. 

#### <a name="azure-sql-database-to-host-the-ssisdb-database-ssis-catalog"></a>Databáze SQL Azure k hostování databáze SSISDB (SSIS katalog) 

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

#### <a name="azure-sql-managed-instance-private-preview-to-host-the-ssisdb-database"></a>Azure spravované Instance SQL (soukromém náhledu) k hostování databáze SSISDB

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

### <a name="start-integration-runtime"></a>Spuštění prostředí Integration Runtime
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


### <a name="full-script"></a>Celý skript
Zde je úplná skript, který vytvoří Azure SSIS reakcí na Incidenty a připojí k virtuální síti. Tento skript předpokládá, že používáte Azure SQL spravované Instance (MI) pro hostování katalogu služby SSIS. 

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
# In public preview, only Standard_A4_v2|Standard_A8_v2|Standard_D1_v2|Standard_D2_v2|Standard_D3_v2|Standard_D4_v2 are supported.
$AzureSSISNodeSize = "Standard_A4_v2" 
# In public preview, only 1-10 nodes are supported.
$AzureSSISNodeNumber = 2 
# In public preview, only 1-8 parallel executions per node are supported.
$AzureSSISMaxParallelExecutionsPerNode = 2 

# SSISDB info
$SSISDBServerEndpoint = "[your Azure SQL Database server name.database.windows.net or your Azure SQL Managed Instance (private preview) server endpoint]"
$SSISDBServerAdminUserName = "[your server admin username]"
$SSISDBServerAdminPassword = "[your server admin password]"

# Remove the SSISDBPricingTier variable if you are using Azure SQL Managed Instance (private preview)
# This parameter applies only to Azure SQL Database. For the basic pricing tier, specify "Basic", not "B". For standard tiers, specify "S0", "S1", "S2", 'S3", etc.
$SSISDBPricingTier = "[your Azure SQL Database pricing tier. Examples: Basic, S0, S1, S2, S3, etc.]"

## Remove these two OPTIONAL variables if you are using Azure SQL Database. 
## These two parameters apply if you are using VNet and Azure SQL Managed Instance (private preview). 
# In public preview, only classic virtual network (VNet) is supported.
$VnetId = "[your VNet resource ID or leave it empty]" 
$SubnetName = "[your subnet name or leave it empty]" 

Login-AzureRmAccount
Select-AzureRmSubscription -SubscriptionName $SubscriptionName

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
                                            -Description $AzureSSISDescription `
                                            -Location $AzureSSISLocation `
                                            -NodeSize $AzureSSISNodeSize `
                                            -NodeCount $AzureSSISNodeNumber `
                                            -MaxParallelExecutionsPerNode $AzureSSISMaxParallelExecutionsPerNode `
                                            -VnetId $VnetId `
                                            -Subnet $SubnetName

write-host("##### Starting #####")
Start-AzureRmDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                             -DataFactoryName $DataFactoryName `
                                             -Name $AzureSSISName `
                                             -Force

write-host("##### Completed #####")
write-host("If any cmdlet is unsuccessful, please consider using -Debug option for diagnostics.")
```

## <a name="azure-resource-manager-template"></a>Šablona Azure Resource Manageru
V této části použijete k vytvoření modulu runtime integrace Azure SSIS šablonu Azure Resource Manager. Zde je ukázka návod: 

1. Vytvořte soubor JSON pomocí následující šablony Resource Manageru. Nahraďte vlastními hodnotami hodnoty v lomené závorky (zástupného). 

    ```json
    {
        "contentVersion": "1.0.0.0",
        "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "parameters": {},
        "variables": {},
        "resources": [{
            "name": "<Specify a name for your data factory>",
            "apiVersion": "2017-09-01-preview",
            "type": "Microsoft.DataFactory/factories",
            "location": "East US",
            "properties": {},
            "resources": [{
                "type": "integrationruntimes",
                "name": "<Specify a name for the Azure SSIS IR>",
                "dependsOn": [ "<The name of the data factory you specified at the beginning>" ],
                "apiVersion": "2017-09-01-preview",
                "properties": {
                    "type": "Managed",
                    "typeProperties": {
                        "computeProperties": {
                            "location": "East US",
                            "nodeSize": "Standard_D1_v2",
                            "numberOfNodes": 1,
                            "maxParallelExecutionsPerNode": 1
                        },
                        "ssisProperties": {
                            "catalogInfo": {
                                "catalogServerEndpoint": "<Azure SQL server>.database.windows.net",
                                "catalogAdminUserName": "<Azure SQL user",
                                "catalogAdminPassword": {
                                    "type": "SecureString",
                                    "value": "<Azure SQL Password>"
                                },
                                "catalogPricingTier": "Basic"
                            }
                        }
                    }
                }
            }]
        }]
    }
    ```
2. Pokud chcete nasadit šablonu Resource Manager, spuštěním příkazu New-AzureRmResourceGroupDeployment jak je znázorněno v následujícím exmaple. V tomto příkladu je ADFTutorialResourceGroup název skupiny prostředků. C:\adfgetstarted je soubor, který obsahuje definici JSON pro vytváření dat a infračerveného signálu Azure SSIS. 

    ```powershell
    New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile ADFTutorialARM.json
    ```

    Tento příkaz vytvoří objekt pro vytváření dat a vytvoří Azure SSIS IR v něm, ale nespustí infračerveného signálu. 
3. Spusťte IR Azure SSIS, spusťte příkaz Start-AzureRmDataFactoryV2IntegrationRuntime: 

    ```powershell
    Start-AzureRmDataFactoryV2IntegrationRuntime -ResourceGroupName "<Resource Group Name> `
                                             -DataFactoryName <Data Factory Name> `
                                             -Name <Azure SSIS IR Name> `
                                             -Force
    ``` 

## <a name="deploy-ssis-packages"></a>Nasazení balíčků SSIS
Teď použijte SQL Server Data Tools (SSDT) nebo aplikaci SQL Server Management Studio (SSMS) k nasazení vašich balíčků SSIS do Azure. Připojte se k serveru SQL Azure, který hostuje katalog služby SSIS (SSISDB). Název serveru SQL Azure je ve formátu &lt;název_serveru&gt;.database.windows.net (pro službu Azure SQL Database). Pokyny najdete v článku věnovaném [nasazení balíčků](/sql/integration-services/packages/deploy-integration-services-ssis-projects-and-packages#deploy-packages-to-integration-services-server). 

## <a name="next-steps"></a>Další postup
Najdete v dalších tématech IR Azure SSIS v této dokumentaci:

- [Modul Runtime integrace Azure SSIS](concepts-integration-runtime.md#azure-ssis-integration-runtime). Tento článek obsahuje koncepční informace o integraci runtimes obecně včetně infračerveného signálu Azure SSIS. 
- [Kurz: Nasazení balíčků SSIS do Azure](tutorial-deploy-ssis-packages-azure.md) Tento článek obsahuje podrobné pokyny pro vytvoření Azure-SSIS IR a využívá databázi Azure SQL k hostování katalogu SSIS. 
- [Monitorování Azure-SSIS IR](monitor-integration-runtime.md#azure-ssis-integration-runtime). Tento článek ukazuje, jak načíst informace o Azure-SSIS IR a popisy stavů ve vrácených informacích. 
- [Správa Azure-SSIS IR](manage-azure-ssis-integration-runtime.md). Tento článek ukazuje, jak zastavit, spustit nebo odebrat Azure-SSIS IR. Ukazuje také postup horizontálního navýšení kapacity Azure-SSIS IR přidáním více uzlů. 
- [Připojení Azure-SSIS IR k virtuální síti](join-azure-ssis-integration-runtime-virtual-network.md) Tento článek obsahuje koncepční informace o připojení Azure-SSIS IR k virtuální síti Azure (VNet). Poskytuje také kroky pro využití webu Azure Portal ke konfiguraci virtuální sítě, aby se k ní prostředí Azure-SSIS IR mohlo připojit. 