---
title: "Vytvoření datové továrny Azure pomocí .NET | Dokumentace Microsoftu"
description: "Vytvořte datovou továrnu Azure ke zkopírování dat z jednoho umístění ve službě Azure Blob Storage do jiného umístění v stejné službě."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: 
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 09/06/2017
ms.author: jingwang
ms.openlocfilehash: 579311b28abb650c6527fe1160ebf875ce7e8c82
ms.sourcegitcommit: c50171c9f28881ed3ac33100c2ea82a17bfedbff
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/26/2017
---
# <a name="create-a-data-factory-and-pipeline-using-net-sdk"></a>Vytvoření datové továrny a kanálu s využitím .NET SDK
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Verze 1 – GA](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
> * [Verze 2 – Preview](quickstart-create-data-factory-dot-net.md)

Azure Data Factory je cloudová služba pro integraci dat umožňující vytváření pracovních postupů řízených daty v cloudu za účelem orchestrace a automatizace přesunu a transformace dat. Pomocí služby Azure Data Factory můžete vytvářet a plánovat pracovní postupy řízené daty (nazývané kanály) se schopností ingestovat data z různorodých úložišť dat, zpracovat a transformovat tato data pomocí výpočetních služeb, jako je Azure HDInsight Hadoop, Spark, Azure Data Lake Analytics a Azure Machine Learning, a publikovat výstupní data do úložišť dat, jako je Azure SQL Data Warehouse, aby je mohly využívat aplikace business intelligence (BI). 

Tento rychlý start popisuje použití sady .NET SDK k vytvoření datové továrny Azure. Kanál v této datové továrně kopíruje data z jedné složky do jiné složky v úložišti objektů blob Azure.

> [!NOTE]
> Tento článek se týká verze 2 služby Data Factory, která je aktuálně ve verzi Preview. Pokud používáte verzi 1 služby Data Factory, který je všeobecně dostupná (GA), prostudujte si [úvod do služby Data Factory verze 1](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).

Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/) před tím, než začnete.

## <a name="prerequisites"></a>Požadavky
* **Účet služby Azure Storage**. Úložiště objektů blob použijete jako úložiště dat pro **zdroj** i **jímku**. Pokud nemáte účet úložiště Azure, přečtěte si článek [Vytvoření účtu úložiště](../storage/common/storage-create-storage-account.md#create-a-storage-account). 
* Vytvořte **kontejner objektů blob** ve službě Blob Storage, v tomto kontejneru vytvořte vstupní **složku** a uložte do ní nějaké soubory. Nástroje, jako je [Průzkumník služby Azure Storage](https://azure.microsoft.com/features/storage-explorer/), můžete použít k připojení k úložišti objektů blob v Azure, k vytvoření kontejneru objektů blob, nahrání vstupního souboru a ověření výstupního souboru.
* **Visual Studio** 2013, 2015 nebo 2017. Názorný postup v tomto článku využívá Visual Studio 2017.
* **Stáhněte a nainstalujte sadu [Azure .NET SDK](http://azure.microsoft.com/downloads/)**.
* **V Azure Active Directory** vytvořte aplikaci s využitím [těchto pokynů](../azure-resource-manager/resource-group-create-service-principal-portal.md#create-an-azure-active-directory-application). Poznamenejte následující hodnoty, které použijete později: **ID aplikace**, **ověřovací klíč** a **ID tenanta**. Podle pokynů ve stejném článku přiřaďte aplikaci roli **Přispěvatel**. 
*  

## <a name="create-a-visual-studio-project"></a>Vytvoření projektu ve Visual Studiu

Pomocí sady Visual Studio 2013/2015/2017 vytvořte konzolovou aplikaci v C# .NET.

1. Spusťte **Visual Studio**.
2. Klikněte na **Soubor**, přejděte na **Nový** a klikněte na **Projekt**.
3. V seznamu typů projektů napravo vyberte **Visual C#** -> **Aplikace konzoly (.NET Framework)**. Vyžaduje se .NET verze 4.5.2 nebo novější.
4. Jako název zadejte **ADFv2QuickStart**.
5. Kliknutím na tlačítko **OK** vytvořte projekt.

## <a name="install-nuget-packages"></a>Instalace balíčků NuGet

1. Klikněte na **Nástroje**  ->  **Správce balíčků NuGet**  ->  **Konzola správce balíčků**.
2. V **konzole Správce balíčků** spusťte následující příkazy pro instalaci balíčků:

    ```
    Install-Package Microsoft.Azure.Management.DataFactory -Prerelease
    Install-Package Microsoft.Azure.Management.ResourceManager -Prerelease
    Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory

    ```

## <a name="create-a-data-factory-client"></a>Vytvoření klienta datové továrny

1. Otevřete soubor **Program.cs** a vložte do něj následující příkazy. Přidáte tak odkazy na obory názvů.

    ```csharp
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using Microsoft.Rest;
    using Microsoft.Azure.Management.ResourceManager;
    using Microsoft.Azure.Management.DataFactory;
    using Microsoft.Azure.Management.DataFactory.Models;
    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    ```

2. Do metody **Main** přidejte následující kód, který nastaví proměnné. Zástupné znaky nahraďte vlastními hodnotami.

    ```csharp
    // Set variables
    string tenantID = "<your tenant ID>";
    string applicationId = "<your application ID>";
    string authenticationKey = "<your authentication key for the application>";
    string subscriptionId = "<your subscription ID where the data factory resides>";
    string resourceGroup = "<your resource group where the data factory resides>";
    // Currently, Data Factory V2 allows you to create data factories only in the East US and East US2 regions. 
    // Note that the data stores (Azure Storage, Azure SQL Database, etc.) and computes (HDInsight, etc.) used by data factory can be in other regions
    string region = "East US 2";
    string dataFactoryName = "<specify the name of data factory to create. It must be globally unique.>";
    string storageAccount = "<your storage account name to copy data>";
    string storageKey = "<your storage account key>";
    // specify the container and input folder from which all files need to be copied to the output folder. 
    string inputBlobPath = "<the path to existing blob(s) to copy data from, e.g. containername/foldername>";
    //specify the contains and output folder where the files are copied
    string outputBlobPath = "<the blob path to copy data to, e.g. containername/foldername>";

    string storageLinkedServiceName = "AzureStorageLinkedService";  // name of the Azure Storage linked service
    string blobDatasetName = "BlobDataset";             // name of the blob dataset
    string pipelineName = "Adfv2QuickStartPipeline";    // name of the pipeline
    ```

3. Do metody **Main** přidejte následující kód, který vytvoří instanci třídy **DataFactoryManagementClient**. Tento objekt použijete k vytvoření datové továrny, propojené služby, datových sad a kanálu. Použijete ho také k monitorování podrobných informací o spuštění kanálu.

    ```csharp
    // Authenticate and create a data factory management client
    var context = new AuthenticationContext("https://login.windows.net/" + tenantID);
    ClientCredential cc = new ClientCredential(applicationId, authenticationKey);
    AuthenticationResult result = context.AcquireTokenAsync("https://management.azure.com/", cc).Result;
    ServiceClientCredentials cred = new TokenCredentials(result.AccessToken);
    var client = new DataFactoryManagementClient(cred) { SubscriptionId = subscriptionId };
    ```

## <a name="create-a-data-factory"></a>Vytvoření datové továrny

Do metody **Main** přidejte následující kód, který vytvoří **datovou továrnu**. 

```csharp
// Create a data factory
Console.WriteLine("Creating data factory " + dataFactoryName + "...");
Factory dataFactory = new Factory
{
    Location = region,
    Identity = new FactoryIdentity()
};
client.Factories.CreateOrUpdate(resourceGroup, dataFactoryName, dataFactory);
Console.WriteLine(SafeJsonConvert.SerializeObject(dataFactory, client.SerializationSettings));

while (client.Factories.Get(resourceGroup, dataFactoryName).ProvisioningState == "PendingCreation")
{
    System.Threading.Thread.Sleep(1000);
}
```

## <a name="create-a-linked-service"></a>Vytvoření propojené služby

Do metody **Main** přidejte následující kód, který vytvoří **propojenou službu Azure Storage**.

V datové továrně vytvoříte propojené služby, abyste svá úložiště dat a výpočetní služby spojili s datovou továrnou. V tomto kurzu Rychlý start stačí vytvořit jednu propojenou službu Azure Storage pro zdroj kopírování i úložiště jímky s názvem AzureStorageLinkedService.

```csharp
// Create an Azure Storage linked service
Console.WriteLine("Creating linked service " + storageLinkedServiceName + "...");

LinkedServiceResource storageLinkedService = new LinkedServiceResource(
    new AzureStorageLinkedService
    {
        ConnectionString = new SecureString("DefaultEndpointsProtocol=https;AccountName=" + storageAccount + ";AccountKey=" + storageKey)
    }
);
client.LinkedServices.CreateOrUpdate(resourceGroup, dataFactoryName, storageLinkedServiceName, storageLinkedService);
Console.WriteLine(SafeJsonConvert.SerializeObject(storageLinkedService, client.SerializationSettings));
```

## <a name="create-a-dataset"></a>Vytvoření datové sady

Do metody **Main** přidejte následující kód, který vytvoří **datovou sadu objektů blob Azure**.

Definujete datovou sadu, která představuje data pro kopírování ze zdroje do jímky. Tato datová sada objektů blob v tomto příkladu odkazuje na propojenou službu Azure Storage, kterou jste vytvořili v předchozím kroku: Datová sada přebírá parametr, jehož hodnota je nastavená v aktivitě, která tuto datovou sadu využívá. Tento parametr se používá ke konstrukci folderPath s odkazem na místo uložení dat.

```csharp
// Create a Azure Blob dataset
Console.WriteLine("Creating dataset " + blobDatasetName + "...");
DatasetResource blobDataset = new DatasetResource(
    new AzureBlobDataset
    {
        LinkedServiceName = new LinkedServiceReference
        {
            ReferenceName = storageLinkedServiceName
        },
        FolderPath = new Expression { Value = "@{dataset().path}" },
        Parameters = new Dictionary<string, ParameterSpecification>
        {
            { "path", new ParameterSpecification { Type = ParameterType.String } }

        }
    }
);
client.Datasets.CreateOrUpdate(resourceGroup, dataFactoryName, blobDatasetName, blobDataset);
Console.WriteLine(SafeJsonConvert.SerializeObject(blobDataset, client.SerializationSettings));
```

## <a name="create-a-pipeline"></a>Vytvoření kanálu

Do metody **Main** přidejte následující kód, který vytvoří **kanál s aktivitou kopírování**.

V tomto příkladu tento kanál obsahuje jednu aktivitu a přebírá dva parametry – vstupní cestu objektů blob a výstupní cestu objektů blob. Hodnoty pro tyto parametry se nastaví při aktivaci nebo spuštění kanálu. Aktivita kopírování odkazuje na stejnou datovou sadu objektů blob, kterou jste vytvořili v předchozím kroku jako vstup a výstup. Když se tato datová sada použije jako vstupní, zadá se vstupní cesta. A když se tato datová sada použije jako výstupní, zadá se výstupní cesta. 

```csharp
// Create a pipeline with a copy activity
Console.WriteLine("Creating pipeline " + pipelineName + "...");
PipelineResource pipeline = new PipelineResource
{
    Parameters = new Dictionary<string, ParameterSpecification>
    {
        { "inputPath", new ParameterSpecification { Type = ParameterType.String } },
        { "outputPath", new ParameterSpecification { Type = ParameterType.String } }
    },
    Activities = new List<Activity>
    {
        new CopyActivity
        {
            Name = "CopyFromBlobToBlob",
            Inputs = new List<DatasetReference>
            {
                new DatasetReference()
                {
                    ReferenceName = blobDatasetName,
                    Parameters = new Dictionary<string, object>
                    {
                        { "path", "@pipeline().parameters.inputPath" }
                    }
                }
            },
            Outputs = new List<DatasetReference>
            {
                new DatasetReference
                {
                    ReferenceName = blobDatasetName,
                    Parameters = new Dictionary<string, object>
                    {
                        { "path", "@pipeline().parameters.outputPath" }
                    }
                }
            },
            Source = new BlobSource { },
            Sink = new BlobSink { }
        }
    }
};
client.Pipelines.CreateOrUpdate(resourceGroup, dataFactoryName, pipelineName, pipeline);
Console.WriteLine(SafeJsonConvert.SerializeObject(pipeline, client.SerializationSettings));
```

## <a name="create-a-pipeline-run"></a>Vytvoření spuštění kanálu

Do metody **Main** přidejte následující kód, který **aktivuje spuštění kanálu**.

Tento kód také jako hodnoty parametrů **inputPath** a **outputPath** zadaných v kanálu nastaví skutečné cesty k objektům blob zdroje a jímky.

```csharp
// Create a pipeline run
Console.WriteLine("Creating pipeline run...");
Dictionary<string, object> parameters = new Dictionary<string, object>
{
    { "inputPath", inputBlobPath },
    { "outputPath", outputBlobPath }
};
CreateRunResponse runResponse = client.Pipelines.CreateRunWithHttpMessagesAsync(resourceGroup, dataFactoryName, pipelineName, parameters).Result.Body;
Console.WriteLine("Pipeline run ID: " + runResponse.RunId);
```

## <a name="monitor-a-pipeline-run"></a>Monitorování spuštění kanálu

1. Do metody **Main** přidejte následující kód, který nepřetržitě kontroluje stav spuštění kanálu, dokud se kopírování dat nedokončí.

    ```csharp
    // Monitor the pipeline run
    Console.WriteLine("Checking pipeline run status...");
    PipelineRun pipelineRun;
    while (true)
    {
        pipelineRun = client.PipelineRuns.Get(resourceGroup, dataFactoryName, runResponse.RunId);
        Console.WriteLine("Status: " + pipelineRun.Status);
        if (pipelineRun.Status == "InProgress")
            System.Threading.Thread.Sleep(15000);
        else
            break;
    }
    ```

2. Do metody **Main** přidejte následující kód, který načte podrobnosti o spuštění aktivity kopírování, například velikost načtených/zapsaných dat.

    ```csharp
    // Check the copy activity run details
    Console.WriteLine("Checking copy activity run details...");
   
    List<ActivityRun> activityRuns = client.ActivityRuns.ListByPipelineRun(
    resourceGroup, dataFactoryName, runResponse.RunId, DateTime.UtcNow.AddMinutes(-10), DateTime.UtcNow.AddMinutes(10)).ToList(); 
    if (pipelineRun.Status == "Succeeded")
        Console.WriteLine(activityRuns.First().Output);
    else
        Console.WriteLine(activityRuns.First().Error);
    Console.WriteLine("\nPress any key to exit...");
    Console.ReadKey();
    ```

## <a name="run-the-code"></a>Spuštění kódu

Sestavte a spusťte aplikaci a potom ověřte spuštění kanálu.

Konzola vytiskne průběh vytváření datové továrny, propojených služeb, datových sad, kanálu a spuštění kanálu. Potom zkontroluje stav spuštění kanálu. Počkejte, dokud aktivita kopírování nezobrazí údaje o velikosti načtených/zapsaných dat. Potom použijte nástroj, jako je [Průzkumník služby Azure Storage](https://azure.microsoft.com/features/storage-explorer/), a zkontrolujte, že se objekty blob zkopírovaly z inputBlobPath do outputBlobPath, jak jste zadali v proměnných.

### <a name="sample-output"></a>Ukázkový výstup: 
```json
Creating data factory SPv2Factory0907...
{
  "identity": {
    "type": "SystemAssigned"
  },
  "location": "East US"
}
Creating linked service AzureStorageLinkedService...
{
  "properties": {
    "type": "AzureStorage",
    "typeProperties": {
      "connectionString": {
        "value": "DefaultEndpointsProtocol=https;AccountName=<storageAccountName>;AccountKey=<storageAccountKey>",
        "type": "SecureString"
      }
    }
  }
}
Creating dataset BlobDataset...
{
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
Creating pipeline Adfv2QuickStartPipeline...
{
  "properties": {
    "activities": [
      {
        "type": "Copy",
        "typeProperties": {
          "source": {
            "type": "BlobSource"
          },
          "sink": {
            "type": "BlobSink"
          }
        },
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
        "name": "CopyFromBlobToBlob"
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
Creating pipeline run...
Pipeline run ID: 308d222d-3858-48b1-9e66-acd921feaa09
Checking pipeline run status...
Status: InProgress
Status: InProgress
Checking copy activity run details...
{
    "dataRead": 331452208,
    "dataWritten": 331452208,
    "copyDuration": 23,
    "throughput": 14073.209,
    "errors": [],
    "effectiveIntegrationRuntime": "DefaultIntegrationRuntime (West US)",
    "usedCloudDataMovementUnits": 2,
    "billedDuration": 23
}

Press any key to exit...

```

## <a name="clean-up-resources"></a>Vyčištění prostředků
Pokud chcete datovou továrnu odstranit prostřednictvím kódu programu, přidejte do programu následující řádky kódu: 

```csharp
            Console.WriteLine("Deleting the data factory");
            client.Factories.Delete(resourceGroup, dataFactoryName);
```

## <a name="next-steps"></a>Další kroky
Kanál v této ukázce kopíruje data z jednoho umístění do jiného umístění v úložišti objektů blob v Azure. Projděte si [kurzy](tutorial-copy-data-dot-net.md), kde se dozvíte o použití služby Data Factory ve více scénářích. 
