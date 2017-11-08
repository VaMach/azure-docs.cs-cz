---
title: "Vytvoření datové továrny Azure pomocí PowerShellu | Dokumentace Microsoftu"
description: "Vytvořte datovou továrnu Azure ke zkopírování dat z jednoho umístění ve službě Azure Blob Storage do jiného umístění v stejné službě."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: 
ms.devlang: powershell
ms.topic: hero-article
ms.date: 09/26/2017
ms.author: jingwang
ms.openlocfilehash: 82c6f69e886bfe86b2839e7efc3579782c8c3dce
ms.sourcegitcommit: c50171c9f28881ed3ac33100c2ea82a17bfedbff
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/26/2017
---
# <a name="create-a-data-factory-and-pipeline-using-powershell"></a>Vytvoření datové továrny a kanálu pomocí PowerShellu
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Verze 1 – GA](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
> * [Verze 2 – Preview](quickstart-create-data-factory-powershell.md)

Azure Data Factory je cloudová služba pro integraci dat umožňující vytváření pracovních postupů řízených daty v cloudu za účelem orchestrace a automatizace přesunu a transformace dat. Pomocí služby Azure Data Factory můžete vytvářet a plánovat pracovní postupy řízené daty (nazývané kanály) se schopností ingestovat data z různorodých úložišť dat, zpracovat a transformovat tato data pomocí výpočetních služeb, jako je Azure HDInsight Hadoop, Spark, Azure Data Lake Analytics a Azure Machine Learning, a publikovat výstupní data do úložišť dat, jako je Azure SQL Data Warehouse, aby je mohly využívat aplikace business intelligence (BI). 

Tento rychlý start popisuje použití PowerShellu k vytvoření datové továrny Azure. Kanál v této datové továrně kopíruje data z jednoho umístění do jiného umístění v úložišti objektů blob v Azure.

> [!NOTE]
> Tento článek se týká verze 2 služby Data Factory, která je aktuálně ve verzi Preview. Pokud používáte verzi 1 služby Data Factory, který je všeobecně dostupná (GA), prostudujte si [úvod do služby Data Factory verze 1](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).

Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/) před tím, než začnete.

## <a name="prerequisites"></a>Požadavky

* **Účet služby Azure Storage**. Úložiště objektů blob použijete jako úložiště dat pro **zdroj** i **jímku**. Pokud nemáte účet úložiště Azure, přečtěte si článek [Vytvoření účtu úložiště](../storage/common/storage-create-storage-account.md#create-a-storage-account). 
* Vytvořte **kontejner objektů blob** ve službě Blob Storage, v tomto kontejneru vytvořte vstupní **složku** a uložte do ní nějaké soubory. Nástroje, jako je [Průzkumník služby Azure Storage](https://azure.microsoft.com/features/storage-explorer/), můžete použít k připojení k úložišti objektů blob v Azure, k vytvoření kontejneru objektů blob, nahrání vstupního souboru a ověření výstupního souboru.
* **Azure PowerShell**. Postupujte podle pokynů v tématu [Jak nainstalovat a nakonfigurovat Azure PowerShell](/powershell/azure/install-azurerm-ps).

## <a name="create-a-data-factory"></a>Vytvoření datové továrny

1. Spusťte **PowerShell**. Nechte prostředí Azure PowerShell otevřené až do konce tohoto kurzu Rychlý start. Pokud ho zavřete a znovu otevřete, bude potřeba tyto příkazy spustit znovu.

    Spusťte následující příkaz a zadejte uživatelské jméno a heslo, které používáte k přihlášení na web Azure Portal:
        
    ```powershell
    Login-AzureRmAccount
    ```        
    Spuštěním následujícího příkazu zobrazíte všechna předplatná pro tento účet:

    ```powershell
    Get-AzureRmSubscription
    ```
    Spuštěním následujícího příkazu vyberte předplatné, se kterým chcete pracovat. Místo **SubscriptionId** použijte ID vašeho předplatného Azure:

    ```powershell
    Select-AzureRmSubscription -SubscriptionId "<SubscriptionId>"       
    ```
2. Spusťte rutinu **Set-AzureRmDataFactoryV2** pro vytvoření datové továrny. Před spuštěním tohoto příkazu zástupné znaky nahraďte vlastními hodnotami. **Zástupné znaky** nahraďte vlastními hodnotami. 

    Definujte proměnnou pro název skupiny prostředků, kterou můžete později použít v příkazech prostředí PowerShell. 
    ```powershell
    $resourceGroupName = "<your resource group to create the factory>";
    ```

    Definujte proměnnou pro název datové továrny, kterou můžete později použít v příkazech prostředí PowerShell. 

    ```powershell
    $dataFactoryName = "<specify the name of data factory to create. It must be globally unique.>";
    ```

    Spuštěním následujícího příkazu vytvořte datovou továrnu. 
    ```powershell       
    Set-AzureRmDataFactoryV2 -ResourceGroupName $resourceGroupName -Location "East US" -Name $dataFactoryName 
    ```

    Je třeba počítat s následujícím:

    * Název objektu pro vytváření dat Azure musí být globálně jedinečný. Pokud se zobrazí následující chyba, změňte název a zkuste to znovu.

        ```
        The specified Data Factory name 'ADFv2QuickStartDataFactory' is already in use. Data Factory names must be globally unique.
        ```

    * Instance služby Data Factory můžete vytvářet jenom tehdy, když jste přispěvatelem nebo správcem předplatného Azure.
    * Data Factory V2 v současné době umožňuje vytvoření datové továrny jenom v oblasti **Východní USA** nebo **Východní USA 2**. Úložiště dat (Azure Storage, Azure SQL Database atd.) a výpočetní prostředí (HDInsight atd.) používané datovou továrnou mohou být v jiných oblastech.

## <a name="create-a-linked-service"></a>Vytvoření propojené služby

V datové továrně vytvořte propojené služby, abyste svá úložiště dat a výpočetní služby spojili s datovou továrnou. V tomto rychlém startu stačí vytvořit jednu propojenou službu Azure Storage s názvem AzureStorageLinkedService, která se použije jako zdroj i úložiště jímky.

1. Ve složce **C:\ADFv2QuickStartPSH** vytvořte soubor JSON s názvem **AzureStorageLinkedService.json** s následujícím obsahem: (pokud ještě neexistuje, složku ADFv2QuickStartPSH vytvořte). Než soubor uložíte, položky &lt;accountName&gt; a &lt;accountKey&gt; nahraďte názvem svého účtu úložiště Azure a jeho klíčem.

    ```json
    {
        "name": "AzureStorageLinkedService",
        "properties": {
            "type": "AzureStorage",
            "typeProperties": {
                "connectionString": {
                    "value": "DefaultEndpointsProtocol=https;AccountName=<accountName>;AccountKey=<accountKey>",
                    "type": "SecureString"
                }
            }
        }
    }
    ```

2. V prostředí **Azure PowerShell** přejděte do složky **ADFv2QuickStartPSH**.

3. Spuštěním rutiny **Set-AzureRmDataFactoryV2LinkedService** vytvořte propojenou službu **AzureStorageLinkedService**. 

    ```powershell
    Set-AzureRmDataFactoryV2LinkedService -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -Name "AzureStorageLinkedService" -DefinitionFile ".\AzureStorageLinkedService.json"
    ```

    Zde je ukázkový výstup:

    ```
    LinkedServiceName : AzureStorageLinkedService
    ResourceGroupName : <resourceGroupName>
    DataFactoryName   : <dataFactoryName>
    Properties        : Microsoft.Azure.Management.DataFactory.Models.AzureStorageLinkedService
    ```

## <a name="create-a-dataset"></a>Vytvoření datové sady

Nadefinujete datovou sadu, která představuje data ke kopírování ze zdroje do jímky. Tato datová sada objektů blob v tomto příkladu odkazuje na propojenou službu Azure Storage, kterou jste vytvořili v předchozím kroku. Datová sada přebírá parametr, jehož hodnota je nastavená v aktivitě, která tuto datovou sadu využívá. Tento parametr se používá ke konstrukci folderPath s odkazem na místo uložení dat.

1. Ve složce **C:\ADFv2QuickStartPSH** vytvořte soubor JSON s názvem **BlobDataset.json** s následujícím obsahem:

    ```json
    {
        "name": "BlobDataset",
        "properties": {
            "type": "AzureBlob",
            "typeProperties": {
                "folderPath": {
                    "value": "@{dataset().path}",
                    "type": "Expression"
                }
            },
            "linkedServiceName": {
                "referenceName": "AzureStorageLinkedService",
                "type": "LinkedServiceReference"
            },
            "parameters": {
                "path": {
                    "type": "String"
                }
            }
        }
    }
    ```

2. Spuštěním rutiny **Set-AzureRmDataFactoryV2Dataset** vytvořte datovou sadu **BlobDataset**.

    ```powershell
    Set-AzureRmDataFactoryV2Dataset -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -Name "BlobDataset" -DefinitionFile ".\BlobDataset.json"
    ```

    Zde je ukázkový výstup:

    ```
    DatasetName       : BlobDataset
    ResourceGroupName : <resourceGroupname>
    DataFactoryName   : <dataFactoryName>
    Structure         :
    Properties        : Microsoft.Azure.Management.DataFactory.Models.AzureBlobDataset
    ```

## <a name="create-a-pipeline"></a>Vytvoření kanálu
  
V tomto příkladu tento kanál obsahuje jednu aktivitu a přebírá dva parametry – cestu ke vstupnímu objektu blob a cestu k výstupnímu objektu blob. Hodnoty pro tyto parametry se nastaví při aktivaci nebo spuštění kanálu. Aktivita kopírování odkazuje na stejnou datovou sadu objektů blob, kterou jste vytvořili v předchozím kroku jako vstup a výstup. Když se tato datová sada použije jako vstupní, zadá se vstupní cesta. A když se tato datová sada použije jako výstupní, zadá se výstupní cesta. 

1. Ve složce **C:\ADFv2QuickStartPSH** vytvořte soubor JSON s názvem **Adfv2QuickStartPipeline.json** s následujícím obsahem:

    ```json
    {
        "name": "Adfv2QuickStartPipeline",
        "properties": {
            "activities": [
                {
                    "name": "CopyFromBlobToBlob",
                    "type": "Copy",
                    "inputs": [
                        {
                            "referenceName": "BlobDataset",
                            "parameters": {
                                "path": "@pipeline().parameters.inputPath"
                            },
                            "type": "DatasetReference"
                        }
                    ],
                    "outputs": [
                        {
                            "referenceName": "BlobDataset",
                            "parameters": {
                                "path": "@pipeline().parameters.outputPath"
                            },
                            "type": "DatasetReference"
                        }
                    ],
                    "typeProperties": {
                        "source": {
                            "type": "BlobSource"
                        },
                        "sink": {
                            "type": "BlobSink"
                        }
                    }
                }
            ],
            "parameters": {
                "inputPath": {
                    "type": "String"
                },
                "outputPath": {
                    "type": "String"
                }
            }
        }
    }
    ```

2. Spuštěním rutiny **Set-AzureRmDataFactoryV2Pipeline** vytvořte kanál **Adfv2QuickStartPipeline**.

    ```powershell
    Set-AzureRmDataFactoryV2Pipeline -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -Name "Adfv2QuickStartPipeline" -DefinitionFile ".\Adfv2QuickStartPipeline.json"
    ```

    Zde je ukázkový výstup:

    ```
    PipelineName      : Adfv2QuickStartPipeline
    ResourceGroupName : <resourceGroupName>
    DataFactoryName   : <dataFactoryName>
    Activities        : {CopyFromBlobToBlob}
    Parameters        : {[inputPath, Microsoft.Azure.Management.DataFactory.Models.ParameterSpecification], [outputPath, Microsoft.Azure.Management.DataFactory.Models.ParameterSpecification]}
    ```

## <a name="create-a-pipeline-run"></a>Vytvoření spuštění kanálu

V tomto kroku nastavíte hodnoty pro parametry kanálu **inputPath** a **outputPath** na skutečné cesty k objektům blob zdroje a jímky. Potom s použitím těchto argumentů vytvoříte spuštění kanálu. 

1. Ve složce **C:\ADFv2QuickStartPSH** vytvořte soubor JSON s názvem **PipelineParameters.json** s následujícím obsahem:

    Než soubor uložíte, hodnoty položek inputPath a outputPath nahraďte cestami k objektům blob zdroje a jímky, ze kterých a do kterých se data mají kopírovat.

    ```json
    {
        "inputPath": "<the path to existing blob(s) to copy data from, e.g. containername/foldername>",
        "outputPath": "<the blob path to copy data to, e.g. containername/foldername>"
    }
    ```

2. Spuštěním rutiny **Invoke-AzureRmDataFactoryV2Pipeline** vytvořte spuštění kanálu a předejte mu hodnoty parametrů. Zaznamená se také ID spuštění kanálu pro budoucí monitorování.

    ```powershell
    $runId = Invoke-AzureRmDataFactoryV2Pipeline -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -PipelineName "Adfv2QuickStartPipeline" -ParameterFile .\PipelineParameters.json
    ```

## <a name="monitor-a-pipeline-run"></a>Monitorování spuštění kanálu

1. Spusťte následující skript, který bude nepřetržitě kontrolovat stav spuštění kanálu, dokud nedokončí kopírování dat.

    ```powershell
    while ($True) {
        $run = Get-AzureRmDataFactoryV2PipelineRun -ResourceGroupName $resourceGroupName -DataFactoryName $DataFactoryName -PipelineRunId $runId

        if ($run) {
            if ($run.Status -ne 'InProgress') {
                Write-Host "Pipeline run finished. The status is: " $run.Status -foregroundcolor "Yellow"
                $run
                break
            }
            Write-Host  "Pipeline is running...status: InProgress" -foregroundcolor "Yellow"
        }

        Start-Sleep -Seconds 30
    }
    ```

    Zde je ukázkový výstup spuštění kanálu:

    ```
    Pipeline is running...status: InProgress
    Pipeline run finished. The status is:  Succeeded
    
    ResourceGroupName : ADFTutorialResourceGroup
    DataFactoryName   : SPTestFactory0928
    RunId             : 0000000000-0000-0000-0000-0000000000000
    PipelineName      : Adfv2QuickStartPipeline
    LastUpdated       : 9/28/2017 8:28:38 PM
    Parameters        : {[inputPath, adftutorial/input], [outputPath, adftutorial/output]}
    RunStart          : 9/28/2017 8:28:14 PM
    RunEnd            : 9/28/2017 8:28:38 PM
    DurationInMs      : 24151
    Status            : Succeeded
    Message           :
    ```

2. Spusťte následující skript, který načte podrobnosti o spuštění aktivity kopírování, například velikost načtených/zapsaných dat.

    ```powershell
    Write-Host "Activity run details:" -foregroundcolor "Yellow"
    $result = Get-AzureRmDataFactoryV2ActivityRun -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -PipelineRunId $runId -RunStartedAfter (Get-Date).AddMinutes(-30) -RunStartedBefore (Get-Date).AddMinutes(30)
    $result
    
    Write-Host "Activity 'Output' section:" -foregroundcolor "Yellow"
    $result.Output -join "`r`n"
    
    Write-Host "\nActivity 'Error' section:" -foregroundcolor "Yellow"
    $result.Error -join "`r`n"
    ```
3. Zkontrolujte, jestli se zobrazí výstup podobný následujícímu ukázkovému výstupu výsledku spuštění aktivity:

    ```json
    ResourceGroupName : ADFTutorialResourceGroup
    DataFactoryName   : SPTestFactory0928
    ActivityName      : CopyFromBlobToBlob
    PipelineRunId     : 00000000000-0000-0000-0000-000000000000
    PipelineName      : Adfv2QuickStartPipeline
    Input             : {source, sink}
    Output            : {dataRead, dataWritten, copyDuration, throughput...}
    LinkedServiceName :
    ActivityRunStart  : 9/28/2017 8:28:18 PM
    ActivityRunEnd    : 9/28/2017 8:28:36 PM
    DurationInMs      : 18095
    Status            : Succeeded
    Error             : {errorCode, message, failureType, target}
    
    Activity 'Output' section:
    "dataRead": 38
    "dataWritten": 38
    "copyDuration": 7
    "throughput": 0.01
    "errors": []
    "effectiveIntegrationRuntime": "DefaultIntegrationRuntime (West US)"
    "usedCloudDataMovementUnits": 2
    "billedDuration": 14
    ```

## <a name="verify-the-output"></a>Ověření výstupu
Použijte nástroj, jako je [Průzkumník služby Azure Storage](https://azure.microsoft.com/features/storage-explorer/), a zkontrolujte, že se objekty blob v inputBlobPath zkopírovaly do outputBlobPath.

## <a name="clean-up-resources"></a>Vyčištění prostředků
Prostředky, které jste vytvořili v rámci tohoto rychlého startu, můžete vyčistit dvěma způsoby. Můžete odstranit [skupinu prostředků Azure](../azure-resource-manager/resource-group-overview.md), což zahrnuje odstranění všech prostředků v této skupině prostředků. Pokud chcete ostatní prostředky zachovat beze změny, odstraňte pouze datovou továrnu, kterou jste vytvořili v tomto kurzu.

Spuštěním následujícího příkazu odstraníte celou skupinu prostředků: 
```powershell
Remove-AzureRmResourceGroup -ResourceGroupName $resourcegroupname
```

Spuštěním následujícího příkazu odstraníte pouze datovou továrnu: 

```powershell
Remove-AzureRmDataFactoryV2 -Name "<NameOfYourDataFactory>" -ResourceGroupName "<NameOfResourceGroup>"
```

## <a name="next-steps"></a>Další kroky
Kanál v této ukázce kopíruje data z jednoho umístění do jiného umístění v úložišti objektů blob v Azure. Projděte si [kurzy](tutorial-copy-data-dot-net.md), kde se dozvíte o použití služby Data Factory ve více scénářích. 
