---
title: "Kopírování místních dat do cloudu pomocí služby Azure Data Factory | Dokumentace Microsoftu"
description: "Zjistěte, jak kopírovat data z místního úložiště dat do cloudu Azure s využitím místního prostředí Integration Runtime ve službě Azure Data Factory."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 09/14/2017
ms.author: jingwang
ms.openlocfilehash: 9aac9c9bcc609a91415438279419d4cc8e237fcb
ms.sourcegitcommit: 6acb46cfc07f8fade42aff1e3f1c578aa9150c73
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/18/2017
---
# <a name="copy-data-between-on-premises-and-cloud"></a>Kopírování dat mezi místním prostředím a cloudem
Azure Data Factory je cloudová služba pro integraci dat umožňující vytváření pracovních postupů řízených daty v cloudu za účelem orchestrace a automatizace přesunu a transformace dat. Pomocí služby Azure Data Factory můžete vytvářet a plánovat pracovní postupy řízené daty (nazývané kanály) se schopností ingestovat data z různorodých úložišť dat, zpracovat a transformovat tato data pomocí výpočetních služeb, jako je Azure HDInsight Hadoop, Spark, Azure Data Lake Analytics a Azure Machine Learning, a publikovat výstupní data do úložišť dat, jako je Azure SQL Data Warehouse, aby je mohly využívat aplikace business intelligence (BI).

V tomto kurzu použijete Azure PowerShell k vytvoření kanálu Data Factory, který kopíruje data z místní databáze SQL Serveru do úložiště objektů blob v Azure. Vytvoříte a použijete místní prostředí Integration Runtime (IR) služby Azure Data Factory umožňující integraci místních a cloudových úložišť dat.  Další informace o vytvoření datové továrny pomocí jiných nástrojů nebo sad SDK najdete v tématu [Šablony Rychlý start](quickstart-create-data-factory-dot-net.md).

V tomto kurzu provedete následující kroky:

> [!div class="checklist"]
> * Vytvoření datové továrny
> * Vytvoření místního prostředí Integration Runtime
> * Vytvoření a šifrování propojené služby místního SQL Serveru v místním prostředí Integration Runtime
> * Vytvoření propojené služby Azure Storage
> * Vytvoření datových sad SQL Serveru a služby Azure Storage
> * Vytvoření kanálu s aktivitou kopírování pro přesun dat
> * Zahájení spuštění kanálu
> * Monitorování spuštění aktivit a kanálu

Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/) před tím, než začnete.

## <a name="prerequisites"></a>Požadavky

* **SQL Server**. V tomto kurzu použijete místní databázi SQL Serveru jako **zdrojové** úložiště dat.
* **Účet služby Azure Storage**. V tomto kurzu použijete úložiště objektů blob v Azure jako **cílové úložiště dat nebo úložiště dat jímky**. Pokud nemáte účet úložiště Azure, přečtěte si článek [Vytvoření účtu úložiště](../storage/common/storage-create-storage-account.md#create-a-storage-account), kde najdete kroky pro jeho vytvoření.
* **Azure PowerShell**. Postupujte podle pokynů v tématu [Jak nainstalovat a nakonfigurovat Azure PowerShell](/powershell/azure/install-azurerm-ps).

## <a name="create-a-data-factory"></a>Vytvoření datové továrny

1. Spusťte **PowerShell**. Nechte prostředí Azure PowerShell otevřené až do konce tohoto kurzu. Pokud ho zavřete a znovu otevřete, bude potřeba tyto příkazy spustit znovu.

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
2. Spusťte rutinu **Set-AzureRmDataFactoryV2** pro vytvoření datové továrny. Před spuštěním tohoto příkazu zástupné znaky nahraďte vlastními hodnotami.

    ```powershell
    $resourceGroupName = "<your resource group to create the factory>"
    $dataFactoryName = "<specify the name of data factory to create. It must be globally unique.>"
    $df = Set-AzureRmDataFactoryV2 -ResourceGroupName $resourceGroupName -Location "East US" -Name $dataFactoryName
    ```

    Je třeba počítat s následujícím:

    * Název objektu pro vytváření dat Azure musí být globálně jedinečný. Pokud se zobrazí následující chyba, změňte název a zkuste to znovu.

        ```
        Data factory name "<data factory name>" is not available.
        ```

    * Instance služby Data Factory můžete vytvářet jenom tehdy, když jste přispěvatelem nebo správcem předplatného Azure.
    * Data Factory v současné době umožňuje vytvoření datové továrny jenom v oblastech Východní USA a Východní USA 2. Úložiště dat (Azure Storage, Azure SQL Database atd.) a výpočetní prostředí (HDInsight atd.) používané datovou továrnou mohou být v jiných oblastech.

## <a name="create-a-self-hosted-ir"></a>Vytvoření místního prostředí IR

V této části můžete vytvořit místní prostředí Integration Runtime a přidružit ho k místnímu uzlu (počítači).

1. Vytvořte místní prostředí Integration Runtime. Pokud už existuje jiné prostředí Integration Runtime se stejným názvem, použijte jedinečný název.

   ```powershell
   $integrationRuntimeName = "<your integration runtime name>"
   Set-AzureRmDataFactoryV2IntegrationRuntime -Name $integrationRuntimeName -Type SelfHosted -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName
   ```

   Zde je ukázkový výstup:

   ```json
    Id                : /subscriptions/<subscription ID>/resourceGroups/ADFTutorialResourceGroup/providers/Microsoft.DataFactory/factories/onpremdf0914/integrationruntimes/myonpremirsp0914
    Type              : SelfHosted
    ResourceGroupName : ADFTutorialResourceGroup
    DataFactoryName   : onpremdf0914
    Name              : myonpremirsp0914
    Description       :
    ```
 

2. Spusťte následující příkaz, který načte stav vytvořeného prostředí Integration Runtime.

   ```powershell
   Get-AzureRmDataFactoryV2IntegrationRuntime -name $integrationRuntimeName -ResourceGroupName $resourceGroupName -DataFactoryName $dataFactoryName -Status
   ```

   Zde je ukázkový výstup:

   ```json
   Nodes                     : {}
   CreateTime                : 9/14/2017 10:01:21 AM
   InternalChannelEncryption :
   Version                   :
   Capabilities              : {}
   ScheduledUpdateDate       :
   UpdateDelayOffset         :
   LocalTimeZoneOffset       :
   AutoUpdate                :
   ServiceUrls               : {eu.frontend.clouddatahub.net, *.servicebus.windows.net}
   ResourceGroupName         : <ResourceGroup name>
   DataFactoryName           : <DataFactory name>
   Name                      : <Integration Runtime name>
   State                     : NeedRegistration
   ```

3. Spusťte následující příkaz, který načte ověřovací klíče pro registraci místního prostředí Integration Runtime ve službě Data Factory v cloudu. Jeden z těchto klíčů zkopírujte pro registraci místního prostředí Integration Runtime.

   ```powershell
   Get-AzureRmDataFactoryV2IntegrationRuntimeKey -Name $integrationRuntimeName -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName | ConvertTo-Json
   ```

   Zde je ukázkový výstup:

   ```json
   {
       "AuthKey1":  "IR@8437c862-d6a9-4fb3-87dd-7d4865a9e845@ab1@eu@VDnzgySwUfaj3pfSUxpvfsXXXXXXx4GHiyF4wboad0Y=",
       "AuthKey2":  "IR@8437c862-d6a9-4fb3-85dd-7d4865a9e845@ab1@eu@sh+k/QNJGBltXL46vXXXXXXXXOf/M1Gne5aVqPtbweI="
   }
   ```

4. Na místním počítači s Windows [stáhněte](https://www.microsoft.com/download/details.aspx?id=39717) místní prostředí Integration Runtime a pomocí ověřovacího klíče, který jste získali v předchozím kroku, toto místní prostředí Integration Runtime ručně zaregistrujte.

   ![Registrace prostředí Integration Runtime](media/tutorial-hybrid-copy-powershell/register-integration-runtime.png)

   Po úspěšném dokončení registrace místního prostředí Integration Runtime se zobrazí následující zpráva:

   ![Úspěšně zaregistrováno](media/tutorial-hybrid-copy-powershell/registered-successfully.png)

   Jakmile se uzel připojí ke cloudové službě, zobrazí se následující stránka:

   ![Uzel je připojen](media/tutorial-hybrid-copy-powershell/node-is-connected.png)

## <a name="create-linked-services"></a>Vytvoření propojených služeb

### <a name="create-an-azure-storage-linked-service-destinationsink"></a>Vytvoření propojené služby Azure Storage (cíl/jímka)

1. Ve složce **C:\ADFv2Tutorial** vytvořte soubor JSON s názvem **AzureStorageLinkedService.json** s následujícím obsahem. Pokud složka ADFv2Tutorial neexistuje, vytvořte ji.  Položky &lt;accountname&gt; a &lt;accountkey&gt; nahraďte názvem svého účtu Azure Storage a jeho klíčem.

   ```json
    {
        "properties": {
            "type": "AzureStorage",
            "typeProperties": {
                "connectionString": {
                    "type": "SecureString",
                    "value": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
                }
            }
        },
        "name": "AzureStorageLinkedService"
    }
   ```

2. V **Azure PowerShellu** přejděte do složky **ADFv2Tutorial**.

   Spuštěním rutiny **Set-AzureRmDataFactoryV2LinkedService** vytvořte propojenou službu **AzureStorageLinkedService**. Rutiny používané v tomto kurzu přebírají hodnoty pro parametry **ResourceGroupName** a **DataFactoryName**. Alternativně můžete předat objekt **DataFactory** vrácený rutinou Set-AzureRmDataFactoryV2, abyste nemuseli při každém spuštění rutiny zadávat hodnoty parametrů ResourceGroupName a DataFactoryName.

   ```powershell
   Set-AzureRmDataFactoryV2LinkedService -DataFactoryName $dataFactoryName -ResourceGroupName $ResourceGroupName -Name "AzureStorageLinkedService" -File ".\AzureStorageLinkedService.json"
   ```

   Zde je ukázkový výstup:

    ```json
    LinkedServiceName : AzureStorageLinkedService
    ResourceGroupName : ADFTutorialResourceGroup
    DataFactoryName   : onpremdf0914
    Properties        : Microsoft.Azure.Management.DataFactory.Models.AzureStorageLinkedService
    ```

### <a name="create-and-encrypt-a-sql-server-linked-service-source"></a>Vytvoření a šifrování propojené služby SQL Serveru (zdroj)

1. Ve složce **C:\ADFv2Tutorial** vytvořte soubor JSON s názvem **SqlServerLinkedService.json** s následujícím obsahem: Než soubor uložíte, položky **&lt;servername>** (název serveru), **&lt;databasename>** (název databáze), **&lt;username>** (uživatelské jméno), **&lt;servername>** (název serveru) a **&lt;password>** (heslo) nahraďte hodnotami vašeho SQL Serveru. Položku **&lt;integration** **runtime** **name>** nahraďte názvem vašeho prostředí Integration Runtime.

    ```json
    {
        "properties": {
            "type": "SqlServer",
            "typeProperties": {
                "connectionString": {
                    "type": "SecureString",
                    "value": "Server=<servername>;Database=<databasename>;User ID=<username>;Password=<password>;Timeout=60"
                }
            },
            "connectVia": {
                "type": "integrationRuntimeReference",
                "referenceName": "<integration runtime name>"
            }
        },
        "name": "SqlServerLinkedService"
    }
   ```
2. Pro zajištění šifrování citlivých dat z datové části JSON v místním prostředí Integration Runtime můžeme spustit rutinu **New-AzureRmDataFactoryV2LinkedServiceEncryptedCredential** a předat jí výše uvedenou datovou část JSON. Toto šifrování zajišťuje šifrování přihlašovacích údajů pomocí rozhraní Data Protection API a jejich lokální uložení v uzlu místního prostředí Integration Runtime. Výstupní datovou část je možné přesměrovat do jiného souboru JSON (v tomto případě encryptedLinkedService.json), který obsahuje zašifrované přihlašovací údaje.

    Před spuštěním tohoto příkazu nahraďte **&lt;integration runtime name&gt;** názvem vašeho prostředí Integration Runtime.

   ```powershell
   New-AzureRmDataFactoryV2LinkedServiceEncryptedCredential -DataFactoryName $dataFactoryName -ResourceGroupName $ResourceGroupName -IntegrationRuntimeName <integration runtime name> -File ".\SQLServerLinkedService.json" > encryptedSQLServerLinkedService.json
   ```

3. Spuštěním následujícího příkazu s použitím souboru JSON z předchozího kroku vytvořte propojenou službu **SqlServerLinkedService**:

   ```powershell
   Set-AzureRmDataFactoryV2LinkedService -DataFactoryName $dataFactoryName -ResourceGroupName $ResourceGroupName -Name "EncryptedSqlServerLinkedService" -File ".\encryptedSqlServerLinkedService.json"
   ```


## <a name="create-datasets"></a>Vytvoření datových sad

### <a name="prepare-an-on-premises-sql-server-for-the-tutorial"></a>Příprava místního SQL Serveru pro tento kurz

V tomto kroku vytvoříte vstupní a výstupní datové sady, které představují vstupní a výstupní data pro operaci kopírování (místní databáze SQL Serveru => úložiště objektů blob v Azure). Před vytvořením datových sad proveďte následující kroky (podrobný postup je uvedený pod seznamem):

- V databázi SQL Serveru, kterou jste do datové továrny přidali jako propojenou službu, vytvořte tabulku **emp** a vložte do ní několik ukázkových záznamů.
- V účtu služby Azure Blob Storage, který jste do datové továrny přidali jako propojenou službu, vytvořte kontejner objektů blob **adftutorial**.


1. V databázi, kterou jste zadali pro propojenou službu místního SQL Serveru (**SqlServerLinkedService**), pomocí následujícího skriptu SQL vytvořte tabulku **emp**.

   ```sql   
     CREATE TABLE dbo.emp
     (
         ID int IDENTITY(1,1) NOT NULL,
         FirstName varchar(50),
         LastName varchar(50),
         CONSTRAINT PK_emp PRIMARY KEY (ID)
     )
     GO
   ```

2. Vložte do tabulky několik ukázkových hodnot:

   ```sql
     INSERT INTO emp VALUES ('John', 'Doe')
     INSERT INTO emp VALUES ('Jane', 'Doe')
   ```



### <a name="create-a-dataset-for-source-sql-database"></a>Vytvoření datové sady pro zdrojovou databázi SQL Database

1. Ve složce **C:\ADFv2Tutorial** vytvořte soubor JSON s názvem **SqlServerDataset.json** s následujícím obsahem:  

    ```json
    {
       "properties": {
            "type": "SqlServerTable",
            "typeProperties": {
                "tableName": "dbo.emp"
            },
            "structure": [
                 {
                    "name": "ID",
                    "type": "String"
                },
                {
                    "name": "FirstName",
                    "type": "String"
                },
                {
                    "name": "LastName",
                    "type": "String"
                }
            ],
            "linkedServiceName": {
                "referenceName": "EncryptedSqlServerLinkedService",
                "type": "LinkedServiceReference"
            }
        },
        "name": "SqlServerDataset"
    }
    ```

2. Spuštěním rutiny **Set-AzureRmDataFactoryV2Dataset** vytvořte datovou sadu **SqlServerDataset**.

    ```powershell
    Set-AzureRmDataFactoryV2Dataset -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -Name "SqlServerDataset" -File ".\SqlServerDataset.json"
    ```

    Zde je ukázkový výstup:

    ```json
    DatasetName       : SqlServerDataset
    ResourceGroupName : ADFTutorialResourceGroup
    DataFactoryName   : onpremdf0914
    Structure         : {"name": "ID" "type": "String", "name": "FirstName" "type": "String", "name": "LastName" "type": "String"}
    Properties        : Microsoft.Azure.Management.DataFactory.Models.SqlServerTableDataset
    ```

### <a name="create-a-dataset-for-sink-azure-blob-storage"></a>Vytvoření datové sady pro službu Azure Blob Storage pro jímku

1. Ve složce **C:\ADFv2Tutorial** vytvořte soubor JSON s názvem **AzureBlobDataset.json** s následujícím obsahem:

    > [!IMPORTANT]
    > Tento vzorový kód předpokládá, že ve službě Azure Blob Storage máte kontejner s názvem **adftutorial**.

    ```json
    {
        "properties": {
            "type": "AzureBlob",
            "typeProperties": {
                "folderPath": "adftutorial/fromonprem",
                "format": {
                    "type": "TextFormat"
                }
            },
            "linkedServiceName": {
                "referenceName": "AzureStorageLinkedService",
                "type": "LinkedServiceReference"
            }
        },
        "name": "AzureBlobDataset"
    }
    ```

2. Spuštěním rutiny **Set-AzureRmDataFactoryV2Dataset** vytvořte datovou sadu **AzureBlobDataset**.

    ```powershell
    Set-AzureRmDataFactoryV2Dataset -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -Name "AzureBlobDataset" -File ".\AzureBlobDataset.json"
    ```

    Zde je ukázkový výstup:

    ```json
    DatasetName       : AzureBlobDataset
    ResourceGroupName : ADFTutorialResourceGroup
    DataFactoryName   : onpremdf0914
    Structure         :
    Properties        : Microsoft.Azure.Management.DataFactory.Models.AzureBlobDataset
    ```

## <a name="create-pipelines"></a>Vytvoření kanálů

### <a name="create-the-pipeline-sqlservertoblobpipeline"></a>Vytvoření kanálu SqlServerToBlobPipeline

1. Ve složce **C:\ADFv2Tutorial** vytvořte soubor JSON s názvem **SqlServerToBlobPipeline.json** s následujícím obsahem:

    ```json
    {
       "name": "SQLServerToBlobPipeline",
        "properties": {
            "activities": [       
                {
                    "type": "Copy",
                    "typeProperties": {
                        "source": {
                            "type": "SqlSource"
                        },
                        "sink": {
                            "type":"BlobSink"
                        }
                    },
                    "name": "CopySqlServerToAzureBlobActivity",
                    "inputs": [
                        {
                            "referenceName": "SqlServerDataset",
                            "type": "DatasetReference"
                        }
                    ],
                    "outputs": [
                        {
                            "referenceName": "AzureBlobDataset",
                            "type": "DatasetReference"
                        }
                    ]
                }
            ]
        }
    }
    ```

2. Spuštěním rutiny **Set-AzureRmDataFactoryV2Pipeline** vytvořte kanál **SQLServerToBlobPipeline**.

    ```powershell
    Set-AzureRmDataFactoryV2Pipeline -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -Name "SQLServerToBlobPipeline" -File ".\SQLServerToBlobPipeline.json"
    ```

    Zde je ukázkový výstup:

    ```json
    PipelineName      : SQLServerToBlobPipeline
    ResourceGroupName : ADFTutorialResourceGroup
    DataFactoryName   : onpremdf0914
    Activities        : {CopySqlServerToAzureBlobActivity}
    Parameters        :  
    ```



## <a name="start-and-monitor-a-pipeline-run"></a>Spuštění kanálu a jeho monitorování

1. Zahajte spuštění pro kanál SQLServerToBlobPipeline a zaznamenejte ID spuštění kanálu pro budoucí monitorování.  

    ```powershell
    $runId = Invoke-AzureRmDataFactoryV2Pipeline -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -PipelineName 'SQLServerToBlobPipeline'
    ```

2. Spusťte následující skript, který bude nepřetržitě kontrolovat stav spuštění kanálu **SQLServerToBlobPipeline** a vytiskne konečný výsledek.

    ```powershell
    while ($True) {
        $result = Get-AzureRmDataFactoryV2ActivityRun -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -PipelineRunId $runId -RunStartedAfter (Get-Date).AddMinutes(-30) -RunStartedBefore (Get-Date).AddMinutes(30)

        if (($result | Where-Object { $_.Status -eq "InProgress" } | Measure-Object).count -ne 0) {
            Write-Host "Pipeline run status: In Progress" -foregroundcolor "Yellow"
            Start-Sleep -Seconds 30
        }
        else {
            Write-Host "Pipeline 'SQLServerToBlobPipeline' run finished. Result:" -foregroundcolor "Yellow"
            $result
            break
        }
    }
    ```

    Zde je výstup tohoto ukázkového spuštění:

    ```jdon
    ResourceGroupName : <resourceGroupName>
    DataFactoryName   : <dataFactoryName>
    ActivityName      : copy
    PipelineRunId     : 4ec8980c-62f6-466f-92fa-e69b10f33640
    PipelineName      : SQLServerToBlobPipeline
    Input             :  
    Output            :  
    LinkedServiceName :
    ActivityRunStart  : 9/13/2017 1:35:22 PM
    ActivityRunEnd    : 9/13/2017 1:35:42 PM
    DurationInMs      : 20824
    Status            : Succeeded
    Error             : {errorCode, message, failureType, target}
    ```

3. Následujícím způsobem můžete získat ID spuštění kanálu **SQLServerToBlobPipeline** a zkontrolovat podrobné výsledky spuštění aktivit.

    ```powershell
    Write-Host "Pipeline 'SQLServerToBlobPipeline' run result:" -foregroundcolor "Yellow"
    ($result | Where-Object {$_.ActivityName -eq "CopySqlServerToAzureBlobActivity"}).Output.ToString()
    ```

    Zde je výstup tohoto ukázkového spuštění:

    ```json
    {
      "dataRead": 36,
      "dataWritten": 24,
      "rowsCopied": 2,
      "copyDuration": 4,
      "throughput": 0.01,
      "errors": []
    }
    ```
4. Připojte se k úložišti objektů blob v Azure pro jímku a potvrďte, že se data z Azure SQL Database zkopírovala správně.

## <a name="next-steps"></a>Další kroky
Kanál v této ukázce kopíruje data z jednoho umístění do jiného umístění v úložišti objektů blob v Azure. Naučili jste se tyto postupy:

> [!div class="checklist"]
> * Vytvoření datové továrny
> * Vytvoření místního prostředí Integration Runtime
> * Vytvoření a šifrování propojené služby místního SQL Serveru v místním prostředí Integration Runtime
> * Vytvoření propojené služby Azure Storage
> * Vytvoření datových sad SQL Serveru a služby Azure Storage
> * Vytvoření kanálu s aktivitou kopírování pro přesun dat
> * Zahájení spuštění kanálu
> * Monitorování spuštění aktivit a kanálu

V článku o [podporovaných úložištích dat](copy-activity-overview.md#supported-data-stores-and-formats) najdete seznam úložišť dat podporovaných službou Azure Data Factory jako zdroje a jímky.

Pokud se chcete dozvědět víc o hromadném kopírování dat ze zdroje do cíle, přejděte k následujícímu kurzu:

> [!div class="nextstepaction"]
>[Hromadné kopírování dat](tutorial-bulk-copy.md)
