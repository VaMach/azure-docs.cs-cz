---
title: "Přírůstkové kopírování dat pomocí Azure Data Factory | Dokumentace Microsoftu"
description: "V tomto kurzu vytvoříte kanál Azure Data Factory, který přírůstkově kopíruje data z Azure SQL Database do Azure Blob Storage."
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/10/2017
ms.author: shlo
ms.openlocfilehash: 91b632b6d2c2917acf17e9d89c1b5a4b0f8b1c33
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="incrementally-load-data-from-azure-sql-database-to-azure-blob-storage"></a>Přírůstkové načtení dat z Azure SQL Database do Azure Blob Storage
Azure Data Factory je cloudová služba pro integraci dat umožňující vytváření pracovních postupů řízených daty v cloudu za účelem orchestrace a automatizace přesunu a transformace dat. Pomocí služby Azure Data Factory můžete vytvářet a plánovat pracovní postupy řízené daty (nazývané kanály) se schopností ingestovat data z různorodých úložišť dat, zpracovat a transformovat tato data pomocí výpočetních služeb, jako je Azure HDInsight Hadoop, Spark, Azure Data Lake Analytics a Azure Machine Learning, a publikovat výstupní data do úložišť dat, jako je Azure SQL Data Warehouse, aby je mohly využívat aplikace business intelligence (BI). 

Jedním z častých scénářů během integrace dat je pravidelné přírůstkové načítání dat pro obnovení aktualizovaných výsledků analýz po počátečním načtení a analýze dat. V tomto kurzu se zaměříte na načítání jenom nových nebo aktualizovaných záznamů ze zdrojů dat do datových jímek. Tato operace je ve srovnání s kompletním načtením mnohem efektivnější, hlavně u rozsáhlých datových sad.    

Data Factory můžete využít k vytvoření řešení horní meze pro zajištění přírůstkového načítání dat s využitím aktivit vyhledávání, kopírování a uložených procedur v kanálu.  

V tomto kurzu provedete následující kroky:

> [!div class="checklist"]
> * Příprava úložiště dat pro uložení hodnoty meze   
> * Vytvoření datové továrny
> * Vytvoření propojených služeb 
> * Vytvoření zdroje, jímky a datových sad mezí
> * Vytvoření kanálu
> * Spuštění kanálu
> * Monitorování spuštění kanálu 

## <a name="overview"></a>Přehled
Diagram tohoto řešení na nejvyšší úrovni je: 

![Přírůstkové načtení dat](media\tutorial-Incrementally-load-data-from-azure-sql-to-blob\incrementally-load.png)

Tady jsou důležité kroky při vytváření tohoto řešení: 

1. **Vyberte sloupec meze**.
    Ve zdrojovém úložišti dat vyberte jeden sloupec, který je možné použít k rozlišení nových nebo aktualizovaných záznamů pro každé spuštění. Data v tomto vybraném sloupci (například čas_poslední_změny nebo ID) se při vytváření nebo aktualizaci řádků obvykle zvyšují. Maximální hodnota v tomto sloupci se používá jako horní mez.
2. **Připravte úložiště dat pro uložení hodnoty meze**.   
    V tomto kurzu uložíte hodnotu meze do databáze Azure SQL.
3. **Vytvořte kanál s následujícím pracovním postupem:** 
    
    Kanál v tomto řešení má následující aktivity:
  
    1. Vytvořte dvě aktivity **vyhledávání**. První aktivitu vyhledávání použijte k načtení poslední hodnoty meze. Druhou aktivitu vyhledávání použijte k načtení nové hodnoty meze. Tyto hodnoty meze se předají aktivitě kopírování. 
    2. Vytvořte **aktivitu kopírování**, která ze zdrojového úložiště dat zkopíruje řádky, které ve sloupci horní meze mají hodnotu vyšší, než je stará hodnota meze, a nižší, než je nová hodnota meze. Potom tato rozdílová data zkopíruje ze zdrojového úložiště dat do úložiště objektů blob jako nový soubor. 
    3. Vytvořte **aktivitu uložené procedury**, která aktualizuje hodnotu meze pro příští spuštění kanálu. 


Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/) před tím, než začnete.

## <a name="prerequisites"></a>Požadavky
* **Azure SQL Database**. Tuto databázi použijete jako **zdrojové** úložiště dat. Pokud Azure SQL Database nemáte, přečtěte si článek věnovaný [vytvoření databáze Azure SQL](../sql-database/sql-database-get-started-portal.md), kde najdete kroky pro její vytvoření.
* **Účet služby Azure Storage**. Úložiště objektů blob použijete jako úložiště dat **jímky**. Pokud nemáte účet úložiště Azure, přečtěte si článek [Vytvoření účtu úložiště](../storage/common/storage-create-storage-account.md#create-a-storage-account), kde najdete kroky pro jeho vytvoření. Vytvořte kontejner s názvem **adftutorial**. 
* **Azure PowerShell**. Postupujte podle pokynů v tématu [Jak nainstalovat a nakonfigurovat Azure PowerShell](/powershell/azure/install-azurerm-ps).

### <a name="create-a-data-source-table-in-your-azure-sql-database"></a>Vytvoření tabulky zdroje dat v databázi Azure SQL
1. Otevřete **SQL Server Management Studio**, v **Průzkumníku serveru** klikněte pravým tlačítkem na databázi a potom zvolte **Nový dotaz**.
2. Spuštěním následujícího příkazu SQL na vaší databázi Azure SQL vytvořte tabulku s názvem `data_source_table` jako úložiště zdroje dat.  
    
    ```sql
    create table data_source_table
    (
        PersonID int,
        Name varchar(255),
        LastModifytime datetime
    );

    INSERT INTO data_source_table
    (PersonID, Name, LastModifytime)
    VALUES
    (1, 'aaaa','9/1/2017 12:56:00 AM'),
    (2, 'bbbb','9/2/2017 5:23:00 AM'),
    (3, 'cccc','9/3/2017 2:36:00 AM'),
    (4, 'dddd','9/4/2017 3:21:00 AM'),
    (5, 'eeee','9/5/2017 8:06:00 AM');
    ```
    V tomto kurzu jako sloupec **meze** použijete **LastModifytime**.  Data v úložišti zdroje dat zobrazuje následující tabulka:

    ```
    PersonID | Name | LastModifytime
    -------- | ---- | --------------
    1 | aaaa | 2017-09-01 00:56:00.000
    2 | bbbb | 2017-09-02 05:23:00.000
    3 | cccc | 2017-09-03 02:36:00.000
    4 | dddd | 2017-09-04 03:21:00.000
    5 | eeee | 2017-09-05 08:06:00.000
    ```

### <a name="create-another-table-in-sql-database-to-store-the-high-watermark-value"></a>Vytvoření další tabulky v databázi SQL pro ukládání hodnoty horní meze
1. Spuštěním následujícího příkazu SQL na vaší databázi Azure SQL vytvořte tabulku s názvem `watermarktable` pro uložení hodnoty meze.  
    
    ```sql
    create table watermarktable
    (
    
    TableName varchar(255),
    WatermarkValue datetime,
    );
    ```
3. Jako výchozí **hodnotu** horní meze nastavte název tabulky zdrojového úložiště dat.  (V tomto kurzu má tato tabulka název **data_source_table**.)

    ```sql
    INSERT INTO watermarktable
    VALUES ('data_source_table','1/1/2010 12:00:00 AM')    
    ```
4. Zkontrolujte data v tabulce `watermarktable`.
    
    ```sql
    Select * from watermarktable
    ```
    Výstup: 

    ```
    TableName  | WatermarkValue
    ----------  | --------------
    data_source_table | 2010-01-01 00:00:00.000
    ```

### <a name="create-a-stored-procedure-in-azure-sql-database"></a>Vytvoření uložené procedury v databázi Azure SQL 

Spuštěním následujícího příkazu ve vaší databázi Azure SQL vytvořte uloženou proceduru.

```sql
CREATE PROCEDURE sp_write_watermark @LastModifiedtime datetime, @TableName varchar(50)
AS

BEGIN
    
    UPDATE watermarktable
    SET [WatermarkValue] = @LastModifiedtime 
WHERE [TableName] = @TableName
    
END
```

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
    Set-AzureRmDataFactoryV2 -ResourceGroupName "<your resource group to create the factory>" -Location "East US" -Name "<specify the name of data factory to create. It must be globally unique.>" 
    ```

    Je třeba počítat s následujícím:

    * Název objektu pro vytváření dat Azure musí být globálně jedinečný. Pokud se zobrazí následující chyba, změňte název a zkuste to znovu.

        ```
        The specified Data Factory name '<data factory name>' is already in use. Data Factory names must be globally unique.
        ```

    * Instance služby Data Factory můžete vytvářet jenom tehdy, když jste přispěvatelem nebo správcem předplatného Azure.
    * Data Factory V2 v současné době umožňuje vytvoření datové továrny jenom v oblasti Východní USA. Úložiště dat (Azure Storage, Azure SQL Database atd.) a výpočetní prostředí (HDInsight atd.) používané datovou továrnou mohou být v jiných oblastech.


## <a name="create-linked-services"></a>Vytvoření propojených služeb
V datové továrně vytvoříte propojené služby, abyste svá úložiště dat a výpočetní služby spojili s datovou továrnou. V této části vytvoříte propojené služby pro účet Azure Storage a databázi Azure SQL. 

### <a name="create-azure-storage-linked-service"></a>Vytvořte propojenou službu pro Azure Storage
1. Vytvořte soubor JSON s názvem **AzureStorageLinkedService.json** ve složce **C:\ADF** s následujícím obsahem. (Pokud složka ADF ještě neexistuje, vytvořte ji.) Než soubor uložíte, položky `<accountName>` a `<accountKey>` nahraďte názvem svého účtu úložiště Azure a jeho klíčem.

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
2. V **Azure PowerShellu** přejděte do složky **ADF**.
3. Spuštěním rutiny **Set-AzureRmDataFactoryV2LinkedService** vytvořte propojenou službu **AzureStorageLinkedService**. V následujícím příkladu předáte hodnoty pro parametry **ResourceGroupName** a **DataFactoryName**. 

    ```powershell
    Set-AzureRmDataFactoryV2LinkedService -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -Name "AzureStorageLinkedService" -File ".\AzureStorageLinkedService.json"
    ```

    Zde je ukázkový výstup:

    ```json
    LinkedServiceName : AzureStorageLinkedService
    ResourceGroupName : <resourceGroupName>
    DataFactoryName   : <dataFactoryName>
    Properties        : Microsoft.Azure.Management.DataFactory.Models.AzureStorageLinkedService
    ```

### <a name="create-azure-sql-database-linked-service"></a>Vytvoření propojené služby Azure SQL Database
1. Vytvořte soubor JSON s názvem **AzureSQLDatabaseLinkedService.json** ve složce **C:\ADF** s následujícím obsahem. (Pokud složka ADF ještě neexistuje, vytvořte ji.) Než soubor uložíte, položky **&lt;server&gt;, &lt;id_uživatele&gt; a &lt;heslo&gt;** nahraďte názvem vašeho serveru Azure SQL, ID uživatele a heslem. 

    ```json
    {
        "name": "AzureSQLDatabaseLinkedService",
        "properties": {
            "type": "AzureSqlDatabase",
            "typeProperties": {
                "connectionString": {
                    "value": "Server = tcp:<server>.database.windows.net,1433;Initial Catalog=<database name>; Persist Security Info=False; User ID=<user name> ; Password=<password>; MultipleActiveResultSets = False; Encrypt = True; TrustServerCertificate = False; Connection Timeout = 30;",
                    "type": "SecureString"
                }
            }
        }
    }
    ```
2. V **Azure PowerShellu** přejděte do složky **ADF**.
3. Spuštěním rutiny **Set-AzureRmDataFactoryV2LinkedService** vytvořte propojenou službu **AzureSQLDatabaseLinkedService**. 

    ```powershell
    Set-AzureRmDataFactoryV2LinkedService -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -Name "AzureSQLDatabaseLinkedService" -File ".\AzureSQLDatabaseLinkedService.json"
    ```

    Zde je ukázkový výstup:

    ```json
    LinkedServiceName : AzureSQLDatabaseLinkedService
    ResourceGroupName : ADF
    DataFactoryName   : incrementalloadingADF
    Properties        : Microsoft.Azure.Management.DataFactory.Models.AzureSqlDatabaseLinkedService
    ProvisioningState :
    ```

## <a name="create-datasets"></a>Vytvoření datových sad
V tomto kroku vytvoříte datové sady, které reprezentují data zdroje a jímky. 

### <a name="create-a-source-dataset"></a>Vytvoření zdrojové datové sady

1. Ve stejné složce vytvořte soubor JSON s názvem SourceDataset.json a s následujícím obsahem: 

    ```json
    {
        "name": "SourceDataset",
        "properties": {
            "type": "AzureSqlTable",
            "typeProperties": {
                "tableName": "data_source_table"
            },
            "linkedServiceName": {
                "referenceName": "AzureSQLDatabaseLinkedService",
                "type": "LinkedServiceReference"
            }
        }
    }
   
    ```
    V tomto kurzu používáme tabulku s názvem **data_source_table**. Pokud používáte tabulku s jiným názvem, nahraďte ho. 
2.  Spuštěním rutiny Set-AzureRmDataFactoryV2Dataset vytvořte datovou sadu SourceDataset.
    
    ```powershell
    Set-AzureRmDataFactoryV2Dataset -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -Name "SourceDataset" -File ".\SourceDataset.json"
    ```

    Tady je ukázkový výstup této rutiny:
    
    ```json
    DatasetName       : SourceDataset
    ResourceGroupName : ADF
    DataFactoryName   : incrementalloadingADF
    Structure         :
    Properties        : Microsoft.Azure.Management.DataFactory.Models.AzureSqlTableDataset
    ```

### <a name="create-a-sink-dataset"></a>Vytvoření datové sady jímky

1. Ve stejné složce vytvořte soubor JSON s názvem SinkDataset.json a s následujícím obsahem: 

    ```json
    {
        "name": "SinkDataset",
        "properties": {
            "type": "AzureBlob",
            "typeProperties": {
                "folderPath": "adftutorial/incrementalcopy",
                "fileName": "@CONCAT('Incremental-', pipeline().RunId, '.txt')", 
                "format": {
                    "type": "TextFormat"
                }
            },
            "linkedServiceName": {
                "referenceName": "AzureStorageLinkedService",
                "type": "LinkedServiceReference"
            }
        }
    }   
    ```

    > [!IMPORTANT]
    > Tento fragment kódu předpokládá, že ve službě Azure Blob Storage máte kontejner objektů blob s názvem **adftutorial**. Pokud tento kontejner neexistuje, vytvořte ho nebo použijte název existujícího kontejneru. Výstupní složka `incrementalcopy` se v kontejneru vytvoří automaticky, pokud ještě neexistuje. V tomto kurzu se název souboru generuje dynamicky pomocí výrazu `@CONCAT('Incremental-', pipeline().RunId, '.txt')`.
2.  Spuštěním rutiny Set-AzureRmDataFactoryV2Dataset vytvořte datovou sadu SinkDataset.
    
    ```powershell
    Set-AzureRmDataFactoryV2Dataset -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -Name "SinkDataset" -File ".\SinkDataset.json"
    ```

    Tady je ukázkový výstup této rutiny:
    
    ```json
    DatasetName       : SinkDataset
    ResourceGroupName : ADF
    DataFactoryName   : incrementalloadingADF
    Structure         :
    Properties        : Microsoft.Azure.Management.DataFactory.Models.AzureBlobDataset    
    ```

## <a name="create-a-dataset-for-watermark"></a>Vytvoření datové sady pro mez
V tomto kroku vytvoříte datovou sadu pro uložení hodnoty horní meze. 

1. Ve stejné složce vytvořte soubor JSON s názvem WatermarkDataset.json a s následujícím obsahem: 

    ```json
    {
        "name": " WatermarkDataset ",
        "properties": {
            "type": "AzureSqlTable",
            "typeProperties": {
                "tableName": "watermarktable"
            },
            "linkedServiceName": {
                "referenceName": "AzureSQLDatabaseLinkedService",
                "type": "LinkedServiceReference"
            }
        }
    }    
    ```
2.  Spuštěním rutiny Set-AzureRmDataFactoryV2Dataset vytvořte datovou sadu WatermarkDataset.
    
    ```powershell
    Set-AzureRmDataFactoryV2Dataset -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -Name "WatermarkDataset" -File ".\WatermarkDataset.json"
    ```

    Tady je ukázkový výstup této rutiny:
    
    ```json
    DatasetName       : WatermarkDataset
    ResourceGroupName : ADF
    DataFactoryName   : incrementalloadingADF
    Structure         :
    Properties        : Microsoft.Azure.Management.DataFactory.Models.AzureSqlTableDataset    
    ```

## <a name="create-a-pipeline"></a>Vytvoření kanálu
V tomto kurzu vytvoříte kanál se dvěma aktivitami vyhledávání, jednou aktivitou kopírování a jednou aktivitou uložené procedury, a to všechno zřetězené v jednom kanálu. 


1. Ve stejné složce vytvořte soubor JSON s názvem IncrementalCopyPipeline.json a s následujícím obsahem. 

    ```json
    {
        "name": "IncrementalCopyPipeline",
        "properties": {
            "activities": [
                {
                    "name": "LookupOldWaterMarkActivity",
                    "type": "Lookup",
                    "typeProperties": {
                        "source": {
                        "type": "SqlSource",
                        "sqlReaderQuery": "select * from watermarktable"
                        },
    
                        "dataset": {
                        "referenceName": "WatermarkDataset",
                        "type": "DatasetReference"
                        }
                    }
                },
                {
                    "name": "LookupNewWaterMarkActivity",
                    "type": "Lookup",
                    "typeProperties": {
                        "source": {
                            "type": "SqlSource",
                            "sqlReaderQuery": "select MAX(LastModifytime) as NewWatermarkvalue from data_source_table"
                        },
    
                        "dataset": {
                        "referenceName": "SourceDataset",
                        "type": "DatasetReference"
                        }
                    }
                },
                
                {
                    "name": "IncrementalCopyActivity",
                    "type": "Copy",
                    "typeProperties": {
                        "source": {
                            "type": "SqlSource",
                            "sqlReaderQuery": "select * from data_source_table where LastModifytime > '@{activity('LookupOldWaterMarkActivity').output.firstRow.WatermarkValue}' and LastModifytime <= '@{activity('LookupNewWaterMarkActivity').output.firstRow.NewWatermarkvalue}'"
                        },
                        "sink": {
                            "type": "BlobSink"
                        }
                    },
                    "dependsOn": [
                        {
                            "activity": "LookupNewWaterMarkActivity",
                            "dependencyConditions": [
                                "Succeeded"
                            ]
                        },
                        {
                            "activity": "LookupOldWaterMarkActivity",
                            "dependencyConditions": [
                                "Succeeded"
                            ]
                        }
                    ],
    
                    "inputs": [
                        {
                            "referenceName": "SourceDataset",
                            "type": "DatasetReference"
                        }
                    ],
                    "outputs": [
                        {
                            "referenceName": "SinkDataset",
                            "type": "DatasetReference"
                        }
                    ]
                },
    
                {
                    "name": "StoredProceduretoWriteWatermarkActivity",
                    "type": "SqlServerStoredProcedure",
                    "typeProperties": {
    
                        "storedProcedureName": "sp_write_watermark",
                        "storedProcedureParameters": {
                            "LastModifiedtime": {"value": "@{activity('LookupNewWaterMarkActivity').output.firstRow.NewWatermarkvalue}", "type": "datetime" },
                            "TableName":  { "value":"@{activity('LookupOldWaterMarkActivity').output.firstRow.TableName}", "type":"String"}
                        }
                    },
    
                    "linkedServiceName": {
                        "referenceName": "AzureSQLDatabaseLinkedService",
                        "type": "LinkedServiceReference"
                    },
    
                    "dependsOn": [
                        {
                            "activity": "IncrementalCopyActivity",
                            "dependencyConditions": [
                                "Succeeded"
                            ]
                        }
                    ]
                }
            ],
            
        }
    }
    ```json

    If you are using a source table with a name different from the one used in the tutorial (**data_source_table**), replace **data_source_table** in the sqlReaderQuery with the name of your source table. 
    

2. Run the Set-AzureRmDataFactoryV2Pipeline cmdlet to create the pipeline: IncrementalCopyPipeline.
    
   ```powershell
   Set-AzureRmDataFactoryV2Pipeline -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -Name "IncrementalCopyPipeline" -File ".\IncrementalCopyPipeline.json"
   ``` 

   Zde je ukázkový výstup: 

   ```json
    PipelineName      : IncrementalCopyPipeline
    ResourceGroupName : ADF
    DataFactoryName   : incrementalloadingADF
    Activities        : {LookupOldWaterMarkActivity, LookupNewWaterMarkActivity, IncrementalCopyActivity, StoredProceduretoWriteWatermarkActivity}
    Parameters        :
   ```
 
## <a name="run-the-pipeline"></a>Spuštění kanálu

1. Kanál **IncrementalCopyPipeline** spusťte pomocí rutiny **Invoke-AzureRmDataFactoryV2Pipeline**. Zástupné znaky nahraďte vlastním názvem skupiny prostředků a názvem datové továrny.

    ```powershell
    $RunId = Invoke-AzureRmDataFactoryV2Pipeline -PipelineName "IncrementalCopyPipeline" -ResourceGroup "<your resource group>" -dataFactoryName "<your data factory name>"
    ``` 
2. Kontrolujte stav kanálu spuštěním rutiny Get-AzureRmDataFactoryV2ActivityRun, dokud neuvidíte všechny aktivity úspěšně spuštěné. Zástupné znaky nahraďte vlastním odpovídajícím časem pro parametry RunStartedAfter a RunStartedBefore.  V tomto kurzu používáme -RunStartedAfter "2017/09/14" -RunStartedBefore "2017/09/15".

    ```powershell
    Get-AzureRmDataFactoryV2ActivityRun -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -PipelineRunId $RunId -RunStartedAfter "<start time>" -RunStartedBefore "<end time>"
    ```

    Zde je ukázkový výstup:
 
    ```json
    ResourceGroupName : ADF
    DataFactoryName   : incrementalloadingADF
    ActivityName      : LookupNewWaterMarkActivity
    PipelineRunId     : d4bf3ce2-5d60-43f3-9318-923155f61037
    PipelineName      : IncrementalCopyPipeline
    Input             : {source, dataset}
    Output            : {NewWatermarkvalue}
    LinkedServiceName :
    ActivityRunStart  : 9/14/2017 7:42:42 AM
    ActivityRunEnd    : 9/14/2017 7:42:50 AM
    DurationInMs      : 7777
    Status            : Succeeded
    Error             : {errorCode, message, failureType, target}
    
    ResourceGroupName : ADF
    DataFactoryName   : incrementalloadingADF
    ActivityName      : LookupOldWaterMarkActivity
    PipelineRunId     : d4bf3ce2-5d60-43f3-9318-923155f61037
    PipelineName      : IncrementalCopyPipeline
    Input             : {source, dataset}
    Output            : {TableName, WatermarkValue}
    LinkedServiceName :
    ActivityRunStart  : 9/14/2017 7:42:42 AM
    ActivityRunEnd    : 9/14/2017 7:43:07 AM
    DurationInMs      : 25437
    Status            : Succeeded
    Error             : {errorCode, message, failureType, target}
    
    ResourceGroupName : ADF
    DataFactoryName   : incrementalloadingADF
    ActivityName      : IncrementalCopyActivity
    PipelineRunId     : d4bf3ce2-5d60-43f3-9318-923155f61037
    PipelineName      : IncrementalCopyPipeline
    Input             : {source, sink}
    Output            : {dataRead, dataWritten, rowsCopied, copyDuration...}
    LinkedServiceName :
    ActivityRunStart  : 9/14/2017 7:43:10 AM
    ActivityRunEnd    : 9/14/2017 7:43:29 AM
    DurationInMs      : 19769
    Status            : Succeeded
    Error             : {errorCode, message, failureType, target}
    
    ResourceGroupName : ADF
    DataFactoryName   : incrementalloadingADF
    ActivityName      : StoredProceduretoWriteWatermarkActivity
    PipelineRunId     : d4bf3ce2-5d60-43f3-9318-923155f61037
    PipelineName      : IncrementalCopyPipeline
    Input             : {storedProcedureName, storedProcedureParameters}
    Output            : {}
    LinkedServiceName :
    ActivityRunStart  : 9/14/2017 7:43:32 AM
    ActivityRunEnd    : 9/14/2017 7:43:47 AM
    DurationInMs      : 14467
    Status            : Succeeded
    Error             : {errorCode, message, failureType, target}

    ```

## <a name="review-the-results"></a>Kontrola výsledků

1. V úložišti objektů blob Azure (úložiště jímky) byste měli vidět, že se data zkopírovala do souboru definovaného parametrem SinkDataset.  V aktuálním kurzu má tento soubor název `Incremental- d4bf3ce2-5d60-43f3-9318-923155f61037.txt`.  Otevřete tento soubor. Zobrazí se záznamy, které jsou stejné jako data v databázi Azure SQL.

    ```
    1,aaaa,2017-09-01 00:56:00.0000000
    2,bbbb,2017-09-02 05:23:00.0000000
    3,cccc,2017-09-03 02:36:00.0000000
    4,dddd,2017-09-04 03:21:00.0000000
    5,eeee,2017-09-05 08:06:00.0000000
    ``` 
2. Zkontrolujte poslední hodnotu z `watermarktable`. Vidíte, že se hodnota meze aktualizovala.

    ```sql
    Select * from watermarktable
    ```
    
    Zde je ukázkový výstup:
 
    TableName | WatermarkValue
    --------- | --------------
    data_source_table   2017-09-05  8:06:00.000

### <a name="insert-data-into-data-source-store-to-verify-delta-data-loading"></a>Vložení dat do úložiště zdroje dat pro ověření načtení rozdílových dat

1. Do databáze Azure SQL (úložiště zdroje dat) vložte nová data:

    ```sql
    INSERT INTO data_source_table
    VALUES (6, 'newdata','9/6/2017 2:23:00 AM')
    
    INSERT INTO data_source_table
    VALUES (7, 'newdata','9/7/2017 9:01:00 AM')
    ``` 

    Aktualizovaná data v databázi Azure SQL vypadají takto:

    ```
    PersonID | Name | LastModifytime
    -------- | ---- | --------------
    1 | aaaa | 2017-09-01 00:56:00.000
    2 | bbbb | 2017-09-02 05:23:00.000
    3 | cccc | 2017-09-03 02:36:00.000
    4 | dddd | 2017-09-04 03:21:00.000
    5 | eeee | 2017-09-05 08:06:00.000
    6 | newdata | 2017-09-06 02:23:00.000
    7 | newdata | 2017-09-07 09:01:00.000
    ```
2. Kanál **IncrementalCopyPipeline** znovu spusťte pomocí rutiny **Invoke-AzureRmDataFactoryV2Pipeline**. Zástupné znaky nahraďte vlastním názvem skupiny prostředků a názvem datové továrny.

    ```powershell
    $RunId = Invoke-AzureRmDataFactoryV2Pipeline -PipelineName "IncrementalCopyPipeline" -ResourceGroup "<your resource group>" -dataFactoryName "<your data factory name>"
    ```
3. Kontrolujte stav kanálu spuštěním rutiny **Get-AzureRmDataFactoryV2ActivityRun**, dokud neuvidíte všechny aktivity úspěšně spuštěné. Zástupné znaky nahraďte vlastním odpovídajícím časem pro parametry RunStartedAfter a RunStartedBefore.  V tomto kurzu používáme -RunStartedAfter "2017/09/14" -RunStartedBefore "2017/09/15".

    ```powershell
    Get-AzureRmDataFactoryV2ActivityRun -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -PipelineRunId $RunId -RunStartedAfter "<start time>" -RunStartedBefore "<end time>"
    ```

    Zde je ukázkový výstup:
 
    ```json
    ResourceGroupName : ADF
    DataFactoryName   : incrementalloadingADF
    ActivityName      : LookupNewWaterMarkActivity
    PipelineRunId     : 2fc90ab8-d42c-4583-aa64-755dba9925d7
    PipelineName      : IncrementalCopyPipeline
    Input             : {source, dataset}
    Output            : {NewWatermarkvalue}
    LinkedServiceName :
    ActivityRunStart  : 9/14/2017 8:52:26 AM
    ActivityRunEnd    : 9/14/2017 8:52:58 AM
    DurationInMs      : 31758
    Status            : Succeeded
    Error             : {errorCode, message, failureType, target}
    
    ResourceGroupName : ADF
    DataFactoryName   : incrementalloadingADF
    ActivityName      : LookupOldWaterMarkActivity
    PipelineRunId     : 2fc90ab8-d42c-4583-aa64-755dba9925d7
    PipelineName      : IncrementalCopyPipeline
    Input             : {source, dataset}
    Output            : {TableName, WatermarkValue}
    LinkedServiceName :
    ActivityRunStart  : 9/14/2017 8:52:26 AM
    ActivityRunEnd    : 9/14/2017 8:52:52 AM
    DurationInMs      : 25497
    Status            : Succeeded
    Error             : {errorCode, message, failureType, target}
    
    ResourceGroupName : ADF
    DataFactoryName   : incrementalloadingADF
    ActivityName      : IncrementalCopyActivity
    PipelineRunId     : 2fc90ab8-d42c-4583-aa64-755dba9925d7
    PipelineName      : IncrementalCopyPipeline
    Input             : {source, sink}
    Output            : {dataRead, dataWritten, rowsCopied, copyDuration...}
    LinkedServiceName :
    ActivityRunStart  : 9/14/2017 8:53:00 AM
    ActivityRunEnd    : 9/14/2017 8:53:20 AM
    DurationInMs      : 20194
    Status            : Succeeded
    Error             : {errorCode, message, failureType, target}
    
    ResourceGroupName : ADF
    DataFactoryName   : incrementalloadingADF
    ActivityName      : StoredProceduretoWriteWatermarkActivity
    PipelineRunId     : 2fc90ab8-d42c-4583-aa64-755dba9925d7
    PipelineName      : IncrementalCopyPipeline
    Input             : {storedProcedureName, storedProcedureParameters}
    Output            : {}
    LinkedServiceName :
    ActivityRunStart  : 9/14/2017 8:53:23 AM
    ActivityRunEnd    : 9/14/2017 8:53:41 AM
    DurationInMs      : 18502
    Status            : Succeeded
    Error             : {errorCode, message, failureType, target}

    ```
4.  V úložišti objektů blob Azure byste měli vidět, že se vytvořil další soubor. V tomto kurzu se tento nový jmenuje `Incremental-2fc90ab8-d42c-4583-aa64-755dba9925d7.txt`.  Otevřete tento soubor a uvidíte, že obsahuje 2 řádky záznamů:
5.  Zkontrolujte poslední hodnotu z `watermarktable`. Vidíte, že se hodnota meze znovu aktualizovala.

    ```sql
    Select * from watermarktable
    ```
    Ukázkový výstup: 
    
    TableName | WatermarkValue
    --------- | ---------------
    data_source_table | 2017-09-07 09:01:00.000

     
## <a name="next-steps"></a>Další kroky
V tomto kurzu jste provedli následující kroky: 

> [!div class="checklist"]
> * Definice sloupce **meze** a jeho uložení v Azure SQL Database  
> * Vytvoření datové továrny
> * Vytvoření propojených služeb pro SQL Database a Blob Storage 
> * Vytvoření zdrojové datové sady a datové sady jímky
> * Vytvoření kanálu
> * Spuštění kanálu
> * Monitorování spuštění kanálu 

Pokud se chcete dozvědět víc o transformaci dat pomocí clusteru Spark v Azure, přejděte k následujícímu kurzu:

> [!div class="nextstepaction"]
>[Transformace dat pomocí clusteru Spark v cloudu](tutorial-transform-data-spark-powershell.md)



