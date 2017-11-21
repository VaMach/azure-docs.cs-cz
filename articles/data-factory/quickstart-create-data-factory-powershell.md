---
title: "Kopírování dat v Blob Storage pomocí Azure Data Factory | Dokumentace Microsoftu"
description: "Vytvořte datovou továrnu Azure ke zkopírování dat z jedné složky ve službě Azure Blob Storage do jiné složky ve stejné službě."
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
ms.date: 11/14/2017
ms.author: jingwang
ms.openlocfilehash: 8ee2f48db009da4660a03f91194c4e99f6ecac4a
ms.sourcegitcommit: afc78e4fdef08e4ef75e3456fdfe3709d3c3680b
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/16/2017
---
# <a name="create-an-azure-data-factory-using-powershell"></a>Vytvoření datové továrny Azure pomocí PowerShellu 
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Verze 1 – GA](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
> * [Verze 2 – Preview](quickstart-create-data-factory-powershell.md)

Tento rychlý start popisuje použití PowerShellu k vytvoření datové továrny Azure. Kanál, který vytvoříte v této datové továrně, kopíruje data z jedné složky do jiné složky v úložišti objektů blob Azure. Kurz předvádějící způsoby transformace dat pomocí Azure Data Factory najdete v tématu [Kurz: Transformace dat pomocí Sparku](transform-data-using-spark.md). 

Tento článek neposkytuje podrobný úvod do služby Data Factory. Úvod do služby Azure Data Factory najdete v tématu [Úvod do Azure Data Factory](introduction.md).

> [!NOTE]
> Tento článek se týká verze 2 služby Data Factory, která je aktuálně ve verzi Preview. Pokud používáte verzi 1 služby Data Factory, který je všeobecně dostupná (GA), prostudujte si [úvod do služby Data Factory verze 1](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).


## <a name="prerequisites"></a>Požadavky

### <a name="azure-subscription"></a>Předplatné Azure
Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/) před tím, než začnete.

### <a name="azure-storage-account"></a>Účet služby Azure Storage
V tomto rychlém startu použijete účet služby Azure Storage (konkrétně služby Blob Storage) pro obecné účely jako **zdrojové úložiště dat** i **cílové úložiště dat nebo úložiště dat jímky**. Pokud nemáte účet úložiště Azure pro obecné účely, přečtěte si téma popisující [Vytvoření účtu úložiště](../storage/common/storage-create-storage-account.md#create-a-storage-account). 

#### <a name="get-storage-account-name-and-account-key"></a>Získání názvu a klíče účtu úložiště
V tomto rychlém startu použijete název a klíč svého účtu úložiště Azure. Následující postup předvádí kroky k získání názvu a klíče vašeho účtu úložiště. 

1. Spusťte webový prohlížeč a přejděte na [Azure Portal](https://portal.azure.com). Přihlaste se pomocí svého uživatelského jména a hesla Azure. 
2. V nabídce vlevo klikněte na **Další služby >**, použijte filtr s klíčovým slovem **úložiště** a vyberte **Účty úložiště**.

    ![Vyhledání účtu úložiště](media/quickstart-create-data-factory-powershell/search-storage-account.png)
3. V seznamu účtů úložiště vyfiltrujte váš účet úložiště (pokud je to potřeba) a pak vyberte **váš účet úložiště**. 
4. Na stránce **Účet úložiště** vyberte v nabídce **Přístupové klíče**.

    ![Získání názvu a klíče účtu úložiště](media/quickstart-create-data-factory-powershell/storage-account-name-key.png)
5. Zkopírujte do schránky hodnoty z polí **Název účtu úložiště** a **klíč1**. Vložte je do Poznámkového bloku nebo jiného editoru a uložte je.  

#### <a name="create-input-folder-and-files"></a>Vytvoření vstupní složky a souborů
V této části vytvoříte ve svém úložišti objektů blob v Azure kontejner objektů blob s názvem adftutorial. Potom v kontejneru vytvoříte složku input a nahrajete do ní ukázkový soubor. 

1. Pokud jej ve svém počítači ještě nemáte, nainstalujte [Průzkumníka služby Azure Storage](https://azure.microsoft.com/features/storage-explorer/). 
2. Spusťte na svém počítači **Průzkumníka služby Microsoft Azure Storage**.   
3. V okně **Připojit ke službě Azure Storage** vyberte **Použít název a klíč účtu úložiště** a klikněte na **Další**. Pokud se okno **Připojit ke službě Azure Storage** nezobrazí, ve stromovém zobrazení klikněte pravým tlačítkem na **Účty úložiště** a pak klikněte na **Připojit k úložišti Azure**. 

    ![Připojení k úložišti Azure](media/quickstart-create-data-factory-powershell/storage-explorer-connect-azure-storage.png)
4. Do okna **Připojit s použitím názvu a klíče** vložte **Název účtu** a **Klíč účtu**, které jste uložili v předchozím kroku. Pak klikněte na **Další**. 
5. V okně **Souhrn připojení** klikněte na **Připojit**.
6. Ověřte, že se váš účet úložiště zobrazí ve stromovém zobrazení v části **(Místní a připojené)** -> **Účty úložiště**. 
7. Rozbalte **Kontejnery objektů blob** a ověřte, že neexistuje kontejner objektů blob s názvem **adftutorial**. Pokud již existuje, přeskočte další kroky pro vytvoření tohoto kontejneru. 
8. Klikněte pravým tlačítkem na **Kontejnery objektů blob** a vyberte **Vytvořit kontejner objektů blob**.

    ![Vytvoření kontejneru objektů blob](media/quickstart-create-data-factory-powershell/stroage-explorer-create-blob-container-menu.png)
9. Jako název zadejte **adftutorial** a stiskněte **ENTER**. 
10. Potvrďte, že je ve stromovém zobrazení vybraný kontejner **adftutorial**. 
11. Na panelu nástrojů klikněte na **Nová složka**. 

    ![Tlačítko Vytvořit složku](media/quickstart-create-data-factory-powershell/stroage-explorer-new-folder-button.png)
12. V okně **Vytvořit nový virtuální adresář** jako **Název** zadejte **input** a klikněte na **OK**. 

    ![Dialogové okno Vytvořit adresář](media/quickstart-create-data-factory-powershell/storage-explorer-create-new-directory-dialog.png)
13. Spusťte **Poznámkový blok** a vytvořte soubor **emp.txt** s následujícím obsahem: 
    
    ```
    John, Doe
    Jane, Doe
    ```    
    Uložte jej do složky **c:\ADFv2QuickStartPSH**. Pokud složka **ADFv2QuickStartPSH** ještě neexistuje, vytvořte ji. 
14. Na panelu nástrojů klikněte na tlačítko **Nahrát** a vyberte **Nahrát soubory**. 

    ![Tlačítko Nahrát](media/quickstart-create-data-factory-powershell/storage-explorer-upload-button.png)
15. V okně **Nahrát soubory** v části **Soubory** vyberte `...`. 
16. V okně **Vybrat složku pro nahrání** přejděte do složky se souborem **emp.txt** a vyberte tento soubor. 

    ![Dialogové okno Nahrát soubory](media/quickstart-create-data-factory-powershell/storage-explorer-upload-files-dialog.png)
17. V okně **Nahrát soubory** klikněte na **Nahrát**. 

### <a name="azure-powershell"></a>Azure PowerShell

#### <a name="install-azure-powershell"></a>Instalace prostředí Azure PowerShell
Nainstalujte nejnovější Azure PowerShell, pokud jej ve svém počítači nemáte. 

1. Ve webovém prohlížeči přejděte na stránku [Sady Azure SDK ke stažení a sady SDK](https://azure.microsoft.com/downloads/). 
2. V části **Nástroje příkazového řádku** -> **PowerShell** klikněte na **Instalace pro Windows**. 
3. Pokud chcete nainstalovat Azure PowerShell, spusťte soubor **MSI**. 

Podrobné pokyny najdete v tématu [Instalace a konfigurace prostředí Azure PowerShell](/powershell/azure/install-azurerm-ps). 

#### <a name="log-in-to-azure-powershell"></a>Přihlášení do prostředí Azure PowerShell
Spusťte na svém počítači **PowerShell**. Nechte prostředí Azure PowerShell otevřené až do konce tohoto kurzu Rychlý start. Pokud ho zavřete a znovu otevřete, tyto příkazy bude potřeba znovu spustit.

1. Spusťte následující příkaz a zadejte uživatelské jméno a heslo Azure, které používáte k přihlášení na Azure Portal:
       
    ```powershell
    Login-AzureRmAccount
    ```        
2. Pokud máte více předplatných Azure, spuštěním následujícího příkazu zobrazíte všechna předplatná pro tento účet:

    ```powershell
    Get-AzureRmSubscription
    ```
3. Spuštěním následujícího příkazu vyberte předplatné, se kterým chcete pracovat. Místo **SubscriptionId** použijte ID vašeho předplatného Azure:

    ```powershell
    Select-AzureRmSubscription -SubscriptionId "<SubscriptionId>"       
    ```

## <a name="create-a-data-factory"></a>Vytvoření datové továrny
1. Definujte proměnnou pro název skupiny prostředků, kterou použijete později v příkazech PowerShellu. Zkopírujte do PowerShellu následující text příkazu, zadejte název [skupiny prostředků Azure](../azure-resource-manager/resource-group-overview.md) v uvozovkách a pak příkaz spusťte. 
   
     ```powershell
    $resourceGroupName = "<Specify a name for the Azure resource group>";
    ```
2. Definujte proměnnou pro název datové továrny, kterou můžete později použít v příkazech prostředí PowerShell. 

    ```powershell
    $dataFactoryName = "<Specify a name for the data factory. It must be globally unique.>";
    ```
1. Definujte proměnnou pro umístění datové továrny: 

    ```powershell
    $location = "East US"
    ```
4. Pokud chcete vytvořit skupinu prostředků Azure, spusťte následující příkaz: 

    ```powershell
    New-AzureRmResourceGroup $resourceGroupName $location
    ``` 
    Pokud již skupina prostředků existuje, nepřepisujte ji. Přiřaďte proměnné `$resourceGroupName` jinou hodnotu a zkuste to znovu. Pokud chcete skupinu prostředků sdílet s ostatními, pokračujte k dalšímu kroku. 
5. Pokud chcete vytvořit datovou továrnu, spusťte následující rutinu **Set-AzureRmDataFactoryV2**: 
    
    ```powershell       
    Set-AzureRmDataFactoryV2 -ResourceGroupName $resourceGroupName -Location "East US" -Name $dataFactoryName 
    ```

Je třeba počítat s následujícím:

* Název objektu pro vytváření dat Azure musí být globálně jedinečný. Pokud se zobrazí následující chyba, změňte název a zkuste to znovu.

    ```
    The specified Data Factory name 'ADFv2QuickStartDataFactory' is already in use. Data Factory names must be globally unique.
    ```

* Instance služby Data Factory můžete vytvářet jenom tehdy, když jste **přispěvatelem** nebo **správcem** předplatného Azure.
* Data Factory verze 2 v současné době umožňuje vytváření datových továren jenom v oblastech Východní USA, Východní USA 2 a Západní Evropa. Úložiště dat (Azure Storage, Azure SQL Database atd.) a výpočetní prostředí (HDInsight atd.) používané datovou továrnou mohou být v jiných oblastech.

## <a name="create-a-linked-service"></a>Vytvoření propojené služby

V datové továrně vytvořte propojené služby, abyste svá úložiště dat a výpočetní služby spojili s datovou továrnou. V tomto rychlém startu stačí vytvořit jednu propojenou službu Azure Storage s názvem AzureStorageLinkedService, která se použije jako zdroj i úložiště jímky.

1. Ve složce **C:\ADFv2QuickStartPSH** vytvořte soubor JSON s názvem **AzureStorageLinkedService.json** s následujícím obsahem: (pokud ještě neexistuje, složku ADFv2QuickStartPSH vytvořte). 

    > [!IMPORTANT]
    > Než soubor uložíte, položky &lt;accountName&gt; a &lt;accountKey&gt; nahraďte názvem svého účtu úložiště Azure a jeho klíčem.

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

Nadefinujete datovou sadu, která představuje data ke kopírování ze zdroje do jímky. Tato datová sada objektů blob v tomto příkladu odkazuje na propojenou službu Azure Storage, kterou jste vytvořili v předchozím kroku. Datová sada přebírá parametr, jehož hodnota je nastavená v aktivitě, která tuto datovou sadu využívá. Tento parametr se používá ke konstrukci **folderPath** s odkazem na místo uložení dat.

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
  
V tomto příkladu tento kanál obsahuje jednu aktivitu a přebírá dva parametry – cestu ke vstupnímu objektu blob a cestu k výstupnímu objektu blob. Hodnoty pro tyto parametry se nastaví při aktivaci nebo spuštění kanálu. Aktivita kopírování používá stejnou datovou sadu objektů blob, kterou jste vytvořili v předchozím kroku jako vstup a výstup. Když se tato datová sada použije jako vstupní, zadá se vstupní cesta. A když se tato datová sada použije jako výstupní, zadá se výstupní cesta. 

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

    Pokud používáte jiné kontejnery a složky, nahraďte hodnoty položek **inputPath** a **outputPath** cestami k objektům blob zdroje a jímky.

    ```json
    {
        "inputPath": "adftutorial/input",
        "outputPath": "adftutorial/output"
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
Kanál v kontejneru objektů blob adftutorial automaticky vytvoří výstupní složku. Potom do výstupní složky zkopíruje soubor emp.txt ze vstupní složky. Pomocí [Průzkumníka služby Azure Storage](https://azure.microsoft.com/features/storage-explorer/) zkontrolujte, že se objekty blob v inputBlobPath zkopírovaly do outputBlobPath. 

## <a name="clean-up-resources"></a>Vyčištění prostředků
Prostředky, které jste vytvořili v rámci tohoto rychlého startu, můžete vyčistit dvěma způsoby. Můžete odstranit [skupinu prostředků Azure](../azure-resource-manager/resource-group-overview.md), což zahrnuje odstranění všech prostředků v této skupině prostředků. Pokud chcete ostatní prostředky zachovat beze změny, odstraňte pouze datovou továrnu, kterou jste vytvořili v tomto kurzu.

Spuštěním následujícího příkazu odstraníte celou skupinu prostředků: 
```powershell
Remove-AzureRmResourceGroup -ResourceGroupName $resourcegroupname
```

Spuštěním následujícího příkazu odstraníte pouze datovou továrnu: 

```powershell
Remove-AzureRmDataFactoryV2 -Name $dataFactoryName -ResourceGroupName $resourceGroupName
```

## <a name="next-steps"></a>Další kroky
Kanál v této ukázce kopíruje data z jednoho umístění do jiného umístění v úložišti objektů blob v Azure. Projděte si [kurzy](tutorial-copy-data-dot-net.md), kde se dozvíte o použití služby Data Factory ve více scénářích. 
