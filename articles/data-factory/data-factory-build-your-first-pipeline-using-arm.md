<properties
    pageTitle="Sestavení prvního objektu pro vytváření dat (šablona ARM) | Microsoft Azure"
    description="V tomto kurzu vytvoříte pomocí šablony Azure Resource Manageru ukázkový kanál služby Azure Data Factory."
    services="data-factory"
    documentationCenter=""
    authors="spelluru"
    manager="jhubbard"
    editor="monicar"/>

<tags
    ms.service="data-factory"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="05/16/2016"
    ms.author="spelluru"/>

# Kurz: Sestavení prvního objektu pro vytváření dat Azure pomocí šablony Azure Resource Manageru
> [AZURE.SELECTOR]
- [Přehled kurzu](data-factory-build-your-first-pipeline.md)
- [Pomocí editoru služby Data Factory](data-factory-build-your-first-pipeline-using-editor.md)
- [Pomocí prostředí PowerShell](data-factory-build-your-first-pipeline-using-powershell.md)
- [Pomocí sady Visual Studio](data-factory-build-your-first-pipeline-using-vs.md)
- [Pomocí šablony Resource Manageru](data-factory-build-your-first-pipeline-using-arm.md)


V tomto článku se dozvíte, jak vytvořit první objekt pro vytváření dat Azure pomocí šablony Azure Resource Manageru (ARM). 


## Požadavky
Vedle požadavků uvedených v tématu Přehled kurzu musíte nainstalovat tyto položky:

- Než budete pokračovat, **musíte** si přečíst článek [Přehled kurzu](data-factory-build-your-first-pipeline.md) a provést nutné kroky. 
- **Nainstalujte prostředí Azure PowerShell**. Podle pokynů v článku [Instalace a konfigurace prostředí Azure PowerShell](../powershell-install-configure.md) si na počítač nainstalujte nejnovější verzi prostředí Azure PowerShell.
- Tento článek neposkytuje koncepční přehled služby Azure Data Factory. Podrobnější přehled služby najdete v článku [Úvod do Azure Data Factory](data-factory-introduction.md). 
- Informace o šablonách Azure Resource Manageru (ARM) najdete v článku [Vytváření šablon Azure Resource Manageru](../resource-group-authoring-templates.md). 

> [AZURE.IMPORTANT] Pokud chcete postupovat podle návodu v tomto článku, musíte nejdřív provést nutné kroky popsané v článku [Přehled kurzu](data-factory-build-your-first-pipeline.md). 

## Vytvoření šablony ARM

Ve složce **C:\ADFGetStarted** vytvořte soubor JSON s názvem **ADFTutorialARM.json** s následujícím obsahem: 

Šablona umožňuje vytvořit následující entity služby Data Factory.

1. **Objekt pro vytváření dat** s názvem **TutorialDataFactoryARM**. Objekt pro vytváření dat může mít jeden nebo víc kanálů. Kanál může obsahovat jednu nebo víc aktivit. Může obsahovat třeba aktivitu kopírování, která slouží ke kopírování dat ze zdrojového do cílového úložiště dat, a aktivitu HDInsight Hive pro spuštění skriptu Hive, který umožňuje transformovat vstupní data na výstupní data produktu. 
2. Dvě **propojené služby**: **StorageLinkedService** a **HDInsightOnDemandLinkedService**. Tyto propojené služby připojují k vašemu objektu pro vytváření dat účet služby Azure Storage a cluster Azure HDInsight na vyžádání. Účet služby Azure Storage bude v této ukázce obsahovat vstupní a výstupní data pro kanál. Propojená služba HDInsight slouží v této ukázce ke spuštění skriptu Hive určeného v aktivitě kanálu. Musíte určit, jaké úložiště dat / výpočetní služby se mají ve vašem scénáři používat, a vytvořením propojených služeb spojit tyto služby s objektem pro vytváření dat. 
3. Dvě **datové sady** (vstupní/výstupní): **AzureBlobInput** a **AzureBlobOutput**. Tyto datové sady představují vstupní a výstupní data pro zpracování Hive. Tyto datové sady odkazují na službu **StorageLinkedService**, kterou už jste v tomto kurzu vytvořili. Propojená služba odkazuje na účet služby Azure Storage a datové sady určují kontejner, složku a název souboru v úložišti, který obsahuje vstupní a výstupní data.   

Kliknutím na kartu **Pomocí editoru služby Data Factory** přepnete na článek, který obsahuje podrobnosti o vlastnostech formátu JSON použitých v této šabloně.

> [AZURE.IMPORTANT] Změňte hodnoty proměnných **storageAccountName** a **storageAccountKey**. Změňte i proměnnou **dataFactoryName**, protože název musí být jedinečný.


    {
        "contentVersion": "1.0.0.0",
        "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "parameters": {
        },
        "variables": {
            "dataFactoryName":  "TutorialDataFactoryARM",
            "storageAccountName":  "<AZURE STORAGE ACCOUNT NAME>" ,
            "storageAccountKey":  "<AZURE STORAGE ACCOUNT KEY>",
            "apiVersion": "2015-10-01",
            "storageLinkedServiceName": "AzureStorageLinkedService",
            "hdInsightOnDemandLinkedServiceName": "HDInsightOnDemandLinkedService",
            "blobInputDataset": "AzureBlobInput",
            "blobOutputDataset": "AzureBlobOutput",
            "singleQuote": "'"
        },
        "resources": [
            {
                "name": "[variables('dataFactoryName')]",
                "apiVersion": "[variables('apiVersion')]",
                "type": "Microsoft.DataFactory/datafactories",
                "location": "westus",
                "resources": [
                    {
                        "dependsOn": [ "[concat('Microsoft.DataFactory/dataFactories/', variables('dataFactoryName'))]" ],
                        "type": "linkedservices",
                        "name": "[variables('storageLinkedServiceName')]",
                        "apiVersion": "[variables('apiVersion')]",
                        "properties": {
                            "type": "AzureStorage",
                            "typeProperties": {
                                "connectionString": "[concat('DefaultEndpointsProtocol=https;AccountName=',variables('storageAccountName'),';AccountKey=',variables('storageAccountKey'))]"
                            }
                        }
                    },
                    {
                        "dependsOn": [
                            "[concat('Microsoft.DataFactory/dataFactories/', variables('dataFactoryName'))]",
                            "[concat('Microsoft.DataFactory/dataFactories/', variables('dataFactoryName'), '/linkedservices/', variables('storageLinkedServiceName'))]"
                        ],
                        "type": "linkedservices",
                        "name": "[variables('hdInsightOnDemandLinkedServiceName')]",
                        "apiVersion": "[variables('apiVersion')]",
                        "properties": {
                            "type": "HDInsightOnDemand",
                            "typeProperties": {
                                "clusterSize": 4,
                                "version":  "3.2",
                                "timeToLive": "00:05:00",
                                "osType": "windows",
                                "linkedServiceName": "[variables('storageLinkedServiceName')]",
                            }
                        }
                    },
                    {
                        "dependsOn": [
                            "[concat('Microsoft.DataFactory/dataFactories/', variables('dataFactoryName'))]",
                            "[concat('Microsoft.DataFactory/dataFactories/', variables('dataFactoryName'), '/linkedServices/', variables('storageLinkedServiceName'))]"
                        ],
                        "type": "datasets",
                        "name": "[variables('blobInputDataset')]",
                        "apiVersion": "[variables('apiVersion')]",
                            "properties": {
                                "type": "AzureBlob",
                                "linkedServiceName": "[variables('storageLinkedServiceName')]",
                                "typeProperties": {
                                    "fileName": "input.log",
                                    "folderPath": "adfgetstarted/inputdata",
                                    "format": {
                                        "type": "TextFormat",
                                        "columnDelimiter": ","
                                    }
                                },
                                "availability": {
                                    "frequency": "Month",
                                    "interval": 1
                                },
                                "external": true,
                                "policy": {}
                            }
                        },
                    {
                        "dependsOn": [
                            "[concat('Microsoft.DataFactory/dataFactories/', variables('dataFactoryName'))]",
                            "[concat('Microsoft.DataFactory/dataFactories/', variables('dataFactoryName'), '/linkedServices/', variables('storageLinkedServiceName'))]"
                        ],
                        "type": "datasets",
                        "name": "[variables('blobOutputDataset')]",
                        "apiVersion": "[variables('apiVersion')]",
                            "properties": {
                                "published": false,
                                "type": "AzureBlob",
                                "linkedServiceName": "[variables('storageLinkedServiceName')]",
                                "typeProperties": {
                                    "folderPath": "adfgetstarted/partitioneddata",
                                    "format": {
                                        "type": "TextFormat",
                                        "columnDelimiter": ","
                                    }
                                },
                                "availability": {
                                    "frequency": "Month",
                                    "interval": 1
                                }
                            }
                        },
                        {
                            "dependsOn": [
                                "[concat('Microsoft.DataFactory/dataFactories/', variables('dataFactoryName'))]",
                                "[concat('Microsoft.DataFactory/dataFactories/', variables('dataFactoryName'), '/linkedServices/', variables('storageLinkedServiceName'))]",
                                "[concat('Microsoft.DataFactory/dataFactories/', variables('dataFactoryName'), '/linkedServices/', variables('hdInsightOnDemandLinkedServiceName'))]",
                                "[concat('Microsoft.DataFactory/dataFactories/', variables('dataFactoryName'), '/datasets/', variables('blobInputDataset'))]",
                                "[concat('Microsoft.DataFactory/dataFactories/', variables('dataFactoryName'), '/datasets/', variables('blobOutputDataset'))]"
                            ],
                            "type": "datapipelines",
                            "name": "[variables('dataFactoryName')]",
                            "apiVersion": "[variables('apiVersion')]",
                            "properties": {
                                "description": "My first Azure Data Factory pipeline",
                                "activities": [
                                    {
                                        "type": "HDInsightHive",
                                        "typeProperties": {
                                            "scriptPath": "adfgetstarted/script/partitionweblogs.hql",
                                            "scriptLinkedService": "[variables('storageLinkedServiceName')]",
                                            "defines": {
                                                "inputtable": "[concat('wasb://adfgetstarted@', variables('storageAccountName'), '.blob.core.windows.net/inputdata')]",
                                                "partitionedtable": "[concat('wasb://adfgetstarted@', variables('storageAccountName'), '.blob.core.windows.net/partitioneddata')]"
                                            }
                                        },
                                        "inputs": [
                                            {
                                                "name": "AzureBlobInput"
                                            }
                                        ],
                                        "outputs": [
                                            {
                                                "name": "AzureBlobOutput"
                                            }
                                        ],
                                        "policy": {
                                            "concurrency": 1,
                                            "retry": 3
                                        },
                                        "scheduler": {
                                            "frequency": "Month",
                                            "interval": 1
                                        },
                                        "name": "RunSampleHiveActivity",
                                        "linkedServiceName": "HDInsightOnDemandLinkedService"
                                    }
                                ],
                                "start": "2016-04-01T00:00:00Z",
                                "end": "2016-04-02T00:00:00Z",
                                "isPaused": false
                            }
                        }
                ]
            }
        ]
    }

Kliknutím na kartu **Pomocí editoru služby Data Factory** přepnete na článek, který obsahuje podrobnosti o vlastnostech formátu JSON použitých v této šabloně.

Je třeba počítat s následujícím: 

- Pomocí výše uvedeného kódu JSON služba Data Factory vytvoří cluster HDInsight **se systémem Windows**. Můžete ale také vytvořit cluster HDInsight **se systémem Linux**. Podrobnosti najdete v tématu [Propojená služba HDInsight na vyžádání](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service). 
- Místo clusteru HDInsight na vyžádání můžete použít také **vlastní cluster HDInsight**. Podrobnosti najdete v tématu [Propojená služba HDInsight](data-factory-compute-linked-services.md#azure-hdinsight-linked-service).
- Cluster HDInsight vytvoří **výchozí kontejner** ve službě Blob Storage, kterou jste určili v kódu JSON (**linkedServiceName**). Při odstranění clusteru HDInsight neprovede odstranění tohoto kontejneru. Toto chování je úmyslné. Díky propojené službě HDInsight na vyžádání se cluster HDInsight vytvoří pokaždé, když je potřeba zpracovat určitý řez, pokud neexistuje aktivní cluster (**timeToLive**), a po dokončení zpracování se zase odstraní.

    Po zpracování velkého množství řezů se ve službě Azure Blob Storage objeví velké množství kontejnerů. Pokud je nepotřebujete k řešení potíží s úlohami, můžete je odstranit, abyste snížili náklady na úložiště. Názvy těchto kontejnerů používají následující formát: „adf**název_vašeho_objektu_pro_vytváření_dat**-**název_propojené_služby**-razítko_data_a_času“. K odstranění kontejnerů ze služby Azure Blob Storage můžete použít nástroje, jako je třeba [Průzkumník úložišť od Microsoftu](http://storageexplorer.com/).

Podrobnosti najdete v tématu [Propojená služba HDInsight na vyžádání](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service).

> [AZURE.NOTE] Další příklad šablony ARM pro vytvoření služby Azure Data Factory najdete na webu [GitHub](https://github.com/Azure/azure-quickstart-templates/blob/master/101-data-factory-blob-to-sql/azuredeploy.json).  

## Vytvoření objektu pro vytváření dat

1. Otevřete prostředí **Azure PowerShell** a spusťte následující příkaz. 
    - Spusťte příkaz **Login-AzureRmAccount** a zadejte uživatelské jméno a heslo, které používáte k přihlášení na webu Azure Portal.  
    - Spuštěním následujícího příkazu vyberte předplatné, ve kterém chcete objekt pro vytváření dat vytvořit.
            Get-AzureRmSubscription -název_předplatného <SUBSCRIPTION NAME> | Set-AzureRmContext
1. Spuštěním následujícího příkazu nasaďte entity služby Data Factory pomocí šablony ARM, kterou jste vytvořili v kroku 1. 

        New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile C:\ADFGetStarted\ADFTutorialARM.json

## Monitorování kanálu
 
1.  Po přihlášení na web [Azure Portal](https://portal.azure.com/) klikněte na **Procházet** a vyberte **Datové továrny**.
        ![Procházet -> Datové továrny](./media/data-factory-build-your-first-pipeline-using-arm/BrowseDataFactories.png)
2.  V okně **Datové továrny** klikněte na objekt pro vytváření dat (**TutorialFactoryARM**), který jste vytvořili.   
2.  V okně **Objekt pro vytváření dat** vašeho objektu pro vytváření dat klikněte na **Diagram**.
        ![Dlaždice Diagram](./media/data-factory-build-your-first-pipeline-using-arm/DiagramTile.png)
4.  V **zobrazení diagramu** uvidíte přehled kanálů a datové sady použité v tomto kurzu.
    
    ![Zobrazení diagramu](./media/data-factory-build-your-first-pipeline-using-arm/DiagramView.png) 
8. V zobrazení diagramu dvakrát klikněte na datovou sadu **AzureBlobOutput**. Zobrazí se řez, který se právě zpracovává.

    ![Datová sada](./media/data-factory-build-your-first-pipeline-using-arm/AzureBlobOutput.png)
9. Po dokončení zpracování uvidíte, že je řez ve stavu **Připraveno**. Upozorňujeme, že vytváření clusteru HDInsight na vyžádání většinou nějakou dobu trvá (přibližně 20 minut). 

    ![Datová sada](./media/data-factory-build-your-first-pipeline-using-arm/SliceReady.png) 
10. Pokud je řez ve stavu **Připraveno**, zkontrolujte, jestli se ve složce **partitioneddata** v kontejneru **adfgetstarted** ve službě Blob Storage nachází výstupní data.  

Pokyny k použití oken Azure Portal k monitorování kanálu a datových sad, které jste vytvořili v tomto kurzu, najdete v článku [Monitorování datových sad a kanálu](data-factory-monitor-manage-pipelines.md).

K monitorování datových kanálů můžete také použít aplikaci pro monitorování a správu. Podrobnosti o použití této aplikace najdete v článku [Monitorování a správa kanálů služby Azure Data Factory pomocí monitorovací aplikace](data-factory-monitor-manage-app.md). 

> [AZURE.IMPORTANT] Po úspěšném zpracování řezu se vstupní soubor odstraní. Pokud tedy chcete spustit řez znovu nebo si znovu projít tento kurz, načtěte vstupní soubor (input.log) do složky inputdata v kontejneru adfgetstarted.

## Šablona ARM pro vytvoření brány
Tady je ukázka šablony ARM pro vytvoření logické brány v pozadí. Upozorňujeme, že je potřeba nainstalovat bránu na místní počítač nebo virtuální počítač Azure s modelem IaaS a pomocí klíče ji zaregistrovat ve službě Data Factory. Podrobnosti najdete v článku [Přesun dat mezi místním prostředím a cloudem](data-factory-move-data-between-onprem-and-cloud.md).

    {
        "contentVersion": "1.0.0.0",
        "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "parameters": {
        },
        "variables": {
            "dataFactoryName":  "GatewayUsingArmDF",
            "apiVersion": "2015-10-01",
            "singleQuote": "'"
        },
        "resources": [
            {
                "name": "[variables('dataFactoryName')]",
                "apiVersion": "[variables('apiVersion')]",
                "type": "Microsoft.DataFactory/datafactories",
                "location": "eastus",
                "resources": [
                    {
                        "dependsOn": [ "[concat('Microsoft.DataFactory/dataFactories/', variables('dataFactoryName'))]" ],
                        "type": "gateways",
                        "apiVersion": "[variables('apiVersion')]",
                        "name": "GatewayUsingARM",
                        "properties": {
                            "description": "my gateway"
                        }
                    }            
                ]
            }
        ]
    }

Tato šablona vytvoří objekt pro vytváření dat s názvem GatewayUsingArmDF, který má bránu s názvem GatewayUsingARM. 

## Viz také
| Téma | Popis |
| :---- | :---- |
| [Aktivity transformace dat](data-factory-data-transformation-activities.md) | Tento článek obsahuje seznam aktivit transformace dat (třeba transformaci HDInsight Hive, kterou jste použili v tomto kurzu) podporovaných službou Azure Data Factory. |
| [Plánování a provádění](data-factory-scheduling-and-execution.md) | Tento článek vysvětluje aspekty plánování a provádění aplikačního modelu služby Azure Data Factory. |
| [Kanály](data-factory-create-pipelines.md) | Tento článek vám pomůže pochopit kanály a aktivity ve službě Azure Data Factory a porozumět tomu, jak se dají ve vaší situaci nebo podniku použít k sestavení kompletních pracovních postupů založených na datech. |
| [Datové sady](data-factory-create-datasets.md) | Tento článek vám pomůže pochopit datové sady ve službě Azure Data Factory.
| [Monitorování a správa kanálů pomocí monitorovací aplikace](data-factory-monitor-manage-app.md) | Tento článek popisuje, jak monitorovat, spravovat a ladit kanály pomocí aplikace pro monitorování a správu. 

  




<!--HONumber=Jun16_HO2-->


