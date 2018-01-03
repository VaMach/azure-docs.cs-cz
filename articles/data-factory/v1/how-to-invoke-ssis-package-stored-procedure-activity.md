---
title: "Vyvolání balíčku služby SSIS pomocí Azure Data Factory - aktivity uložené procedury | Microsoft Docs"
description: "Tento článek popisuje, jak má být vyvolán balíček integrační služby SSIS (SQL Server) z kanál služby Azure Data Factory pomocí aktivity uložené procedury."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: 
ms.devlang: powershell
ms.topic: article
ms.date: 12/07/2017
ms.author: jingwang
ms.openlocfilehash: 8aabe45a1627b1a897ca9fe4bda581c7a3f6bc03
ms.sourcegitcommit: 901a3ad293669093e3964ed3e717227946f0af96
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/21/2017
---
# <a name="invoke-an-ssis-package-using-stored-procedure-activity-in-azure-data-factory"></a>Vyvolání balíčku služby SSIS pomocí aktivity uložené procedury v Azure Data Factory
Tento článek popisuje, jak má být vyvolán balíčku služby SSIS z kanál služby Azure Data Factory pomocí aktivity uložené procedury. 

> [!NOTE]
> Tento článek se týká verze 1 objektu pro vytváření dat, která je obvykle dostupná. Pokud používáte verze 2 služby Data Factory, který je ve verzi Public Preview, najdete v části [balíčky SSIS vyvolání pomocí aktivity uložené procedury v verze 2](../how-to-invoke-ssis-package-stored-procedure-activity.md).

## <a name="prerequisites"></a>Požadavky

### <a name="azure-sql-database"></a>Azure SQL Database 
Návod v tomto článku používá databázi Azure SQL, který je hostitelem služby SSIS katalogu. Můžete také použít Azure SQL spravované Instance (soukromém náhledu).

### <a name="create-an-azure-ssis-integration-runtime"></a>Vytvoření prostředí Azure-SSIS Integration Runtime
Vytvoření modulu runtime integrace Azure SSIS, pokud nemáte dodržením podrobných pokynů v [kurz: balíčky nasazení SSIS](../tutorial-deploy-ssis-packages-azure.md). Je nutné vytvořit objekt pro vytváření dat verze 2: vytvoření modulu runtime integrace Azure SSIS. 

### <a name="azure-powershell"></a>Azure PowerShell
Nainstalujte nejnovější moduly Azure PowerShellu podle pokynů v tématu [Instalace a konfigurace Azure PowerShellu](/powershell/azure/install-azurerm-ps).

## <a name="create-a-data-factory"></a>Vytvoření datové továrny
Následující postup popisuje kroky k vytvoření služby data factory. Vytvoření kanálu s aktivitou uložené procedury v této datové továrně. Aktivita uložené procedury spustí uloženou proceduru v databázi SSISDB ke spuštění vašeho balíčku služby SSIS.

1. Definujte proměnnou pro název skupiny prostředků, kterou použijete později v příkazech PowerShellu. Zkopírujte do PowerShellu následující text příkazu, zadejte název [skupiny prostředků Azure](../../azure-resource-manager/resource-group-overview.md) v uvozovkách a pak příkaz spusťte. Například: `"adfrg"`. 
   
     ```powershell
    $resourceGroupName = "ADFTutorialResourceGroup";
    ```

    Pokud již skupina prostředků existuje, nepřepisujte ji. Přiřaďte proměnné `$ResourceGroupName` jinou hodnotu a spusťte tento příkaz znovu.
2. Pokud chcete vytvořit skupinu prostředků Azure, spusťte následující příkaz: 

    ```powershell
    $ResGrp = New-AzureRmResourceGroup $resourceGroupName -location 'eastus'
    ``` 
    Pokud již skupina prostředků existuje, nepřepisujte ji. Přiřaďte proměnné `$ResourceGroupName` jinou hodnotu a spusťte tento příkaz znovu. 
3. Definujte proměnnou název datové továrny. 

    > [!IMPORTANT]
    >  Aktualizujte název datové továrny tak, aby byl globálně jedinečný. 

    ```powershell
    $DataFactoryName = "ADFTutorialFactory";
    ```

5. K vytvoření objektu pro vytváření dat, spusťte následující **New-AzureRmDataFactory** rutiny pomocí vlastnosti umístění a název skupiny prostředků z $ResGrp proměnné: 
    
    ```powershell       
    $df = New-AzureRmDataFactory -ResourceGroupName $ResourceGroupName -Name $dataFactoryName -Location "East US"
    ```

Je třeba počítat s následujícím:

* Název objektu pro vytváření dat Azure musí být globálně jedinečný. Pokud se zobrazí následující chyba, změňte název a zkuste to znovu.

    ```
    The specified Data Factory name 'ADFTutorialFactory' is already in use. Data Factory names must be globally unique.
    ```
* Pro vytvoření instancí Data Factory musí být uživatelský účet, který použijete pro přihlášení k Azure, členem rolí **přispěvatel** nebo **vlastník** nebo **správcem** předplatného Azure.

### <a name="create-an-azure-sql-database-linked-service"></a>Vytvoření propojené služby Azure SQL Database
Vytvoření propojené služby propojení Azure SQL database, který je hostitelem služby SSIS katalogu datovou továrnu. Objekt pro vytváření dat používá informace v rámci této propojené služby pro připojení k databázi SSISDB a spustí uloženou proceduru spustit balíčku služby SSIS. 

1. Vytvořte soubor JSON s názvem **AzureSqlDatabaseLinkedService.json** v **C:\ADF\RunSSISPackage** složku s následujícím obsahem: 

    > [!IMPORTANT]
    > Nahraďte &lt;servername&gt;, &lt;uživatelské jméno&gt;@&lt;servername&gt; a &lt;heslo&gt; s hodnotami před databázi SQL Azure ukládání souboru.

    ```json
    {
        "name": "AzureSqlDatabaseLinkedService",
        "properties": {
            "type": "AzureSqlDatabase",
            "typeProperties": {
                "connectionString": "Server=tcp:<servername>.database.windows.net,1433;Database=SSISDB;User ID=<username>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
            }
        }
        }
    ```
2. V **prostředí Azure PowerShell**, přepnout **C:\ADF\RunSSISPackage** složky.
3. Spustit **New-AzureRmDataFactoryLinkedService** rutiny pro vytvoření propojené služby: **AzureSqlDatabaseLinkedService**. 

    ```powershell
    New-AzureRmDataFactoryLinkedService $df -File ".\AzureSqlDatabaseLinkedService.json"
    ```

## <a name="create-an-output-dataset"></a>Vytvoření výstupní datové sady
Tato výstupní datové sady je fiktivní datovou sadu, která určuje plán kanálu. Všimněte si, že je nastavena frekvence na hodinu a interval je nastavena na hodnotu 1. Proto kanálu spouští jednou za hodinu v rámci kanálu spuštění a ukončení. 

1. Vytvořte soubor OuputDataset.json s následujícím obsahem: 
    
    ```json
    {
        "name": "sprocsampleout",
        "properties": {
            "type": "AzureSqlTable",
            "linkedServiceName": "AzureSqlLinkedService",
            "typeProperties": { },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            }
        }
    }
    ```
2. Spustit **New-AzureRmDataFactoryDataset** vytvořte datovou sadu. 

    ```powershell
    New-AzureRmDataFactoryDataset $df -File ".\OutputDataset.json"
    ```

## <a name="create-a-pipeline-with-stored-procedure-activity"></a>Vytvoření kanálu s aktivitou uložené procedury 
V tomto kroku vytvoříte kanál s aktivitou uložené procedury. Aktivity vyvolá sp_executesql uložený postup spuštění vašeho balíčku služby SSIS. 

1. Vytvořte soubor JSON s názvem **MyPipeline.json** v **C:\ADF\RunSSISPackage** složku s následujícím obsahem:

    > [!IMPORTANT]
    > Nahraďte &lt;název složky&gt;, &lt;název projektu&gt;, &lt;název balíčku&gt; s názvy složek, projekt a balíčku v katalogu služby SSIS před uložením souboru.

    ```json
    {
        "name": "MyPipeline",
        "properties": {
            "activities": [{
                "name": "SprocActivitySample",
                "type": "SqlServerStoredProcedure",
                "typeProperties": {
                    "storedProcedureName": "sp_executesql",
                    "storedProcedureParameters": {
                        "stmt": "DECLARE @return_value INT, @exe_id BIGINT, @err_msg NVARCHAR(150)    EXEC @return_value=[SSISDB].[catalog].[create_execution] @folder_name=N'<folder name>', @project_name=N'<project name>', @package_name=N'<package name>', @use32bitruntime=0, @runinscaleout=1, @useanyworker=1, @execution_id=@exe_id OUTPUT    EXEC [SSISDB].[catalog].[set_execution_parameter_value] @exe_id, @object_type=50, @parameter_name=N'SYNCHRONIZED', @parameter_value=1    EXEC [SSISDB].[catalog].[start_execution] @execution_id=@exe_id, @retry_count=0    IF(SELECT [status] FROM [SSISDB].[catalog].[executions] WHERE execution_id=@exe_id)<>7 BEGIN SET @err_msg=N'Your package execution did not succeed for execution ID: ' + CAST(@exe_id AS NVARCHAR(20)) RAISERROR(@err_msg,15,1) END"
                    }
                },
                "outputs": [{
                    "name": "sprocsampleout"
                }],
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                }
            }],
            "start": "2017-10-01T00:00:00Z",
            "end": "2017-10-01T05:00:00Z",
            "isPaused": false
        }
    }    
    ```

2. K vytvoření kanálu: **RunSSISPackagePipeline**spusťte **New-AzureRmDataFactoryPipeline** rutiny.

    ```powershell
    $DFPipeLine = New-AzureRmDataFactoryPipeline -DataFactoryName $DataFactory.DataFactoryName -ResourceGroupName $ResGrp.ResourceGroupName -Name "RunSSISPackagePipeline" -DefinitionFile ".\RunSSISPackagePipeline.json"
    ```

## <a name="monitor-the-pipeline-run"></a>Monitorování spuštění kanálu

2. Spustit **Get-AzureRmDataFactorySlice** získat tak podrobné údaje o všech řezech tabulky Výstupní datovou sadu **, která je výstupní tabulkou kanálu.

    ```PowerShell
    Get-AzureRmDataFactorySlice $df -DatasetName sprocsampleout -StartDateTime 2017-10-01T00:00:00Z
    ```
    Všimněte si, že hodnota StartDateTime, kterou tady určíte, je stejná jako počáteční čas uvedený v kódu JSON kanálu. 
3. Spuštěním rutiny **Get-AzureRmDataFactoryRun** získáte podrobnosti o spouštění aktivity pro určitý řez.

    ```PowerShell
    Get-AzureRmDataFactoryRun $df -DatasetName sprocsampleout -StartDateTime 2017-10-01T00:00:00Z
    ```

    Rutinu můžete spouštět opakovaně, dokud se u řezu neobjeví stav **Připraveno** nebo **Nezdařilo se**. 

    Můžete spustit následující dotaz na databázi SSISDB v serveru Azure SQL ověřit, jestli balíček provést. 

    ```sql
    select * from catalog.executions
    ```

## <a name="next-steps"></a>Další postup
Podrobnosti o aktivitě uložené procedury najdete v tématu [aktivity uložené procedury](data-factory-stored-proc-activity.md) článku.

