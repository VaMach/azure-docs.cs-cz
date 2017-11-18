---
title: "Aktivita vyhledávání v Azure Data Factory | Microsoft Docs"
description: "Zjistěte, jak pomocí aktivity vyhledávání najít hodnotu z externího zdroje. Na tento výstup mohou dále odkazovat následující aktivity."
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: shlo
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/31/2017
ms.author: spelluru
ms.openlocfilehash: d498705ef7f714b4f15b8d2722053bf3081b5045
ms.sourcegitcommit: a036a565bca3e47187eefcaf3cc54e3b5af5b369
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/17/2017
---
# <a name="lookup-activity-in-azure-data-factory"></a>Aktivita vyhledávání v Azure Data Factory
Aktivita vyhledávání slouží ke čtení nebo vyhledání záznamu / názvu tabulky / hodnoty z jakéhokoli externího zdroje. Na tento výstup mohou dále odkazovat následující aktivity. 

Následující zdroje dat jsou aktuálně podporovány pro vyhledávání:
- Soubor JSON v Azure Blob
- Místní soubor JSON
- Azure SQL Database (JSON dat převedených z dotazu)
- Azure Table Storage (JSON dat převedených z dotazu)

Vyhledávání aktivity je užitečné, pokud chcete dynamicky načíst seznam souborů nebo záznamy nebo tabulky z konfiguračního souboru nebo zdroj dat. Výstup z aktivity další lze ostatní aktivity k provedení určité zpracování na jen ty položky.

> [!NOTE]
> Tento článek se týká verze 2 služby Data Factory, která je aktuálně ve verzi Preview. Pokud používáte verzi 1 služby Data Factory, který je všeobecně dostupná (GA), přečtěte si téma [Data Factory V1 dokumentaci](v1/data-factory-introduction.md).


## <a name="example"></a>Příklad
V tomto příkladu aktivitě kopírování kopíruje data z tabulky SQL v databázi Azure SQL do Azure Blob Storage. Název tabulky SQL je uložené v souboru JSON ve službě Blob Storage. Aktivita vyhledávání vyhledá název tabulky za běhu. Tento přístup umožňuje JSON má být změněn dynamicky bez opětovného nasazení kanálů nebo datové sady. 

### <a name="pipeline"></a>Kanál
Tento kanál obsahuje dvě aktivity: **vyhledat** a **kopie**. 

- Aktivita vyhledávání je nakonfigurovaný na použití LookupDataset, který odkazuje na umístění v Azure Blob Storage. Aktivita vyhledávání načte název tabulky SQL ze souboru JSON v tomto umístění. 
- Aktivita kopírování používá výstup aktivity vyhledávání (název tabulky SQL). Název tabulky v datové sadě zdroje (SourceDataset) je nakonfigurován pro použití výstupu z vyhledávání aktivity. Aktivita kopírování kopíruje data z tabulky SQL do umístění v Azure Blob Storage, která je zadána SinkDataset. 


```json
{
    "name": "LookupPipelineDemo",
    "properties": {
        "activities": [
            {
                "name": "LookupActivity",
                "type": "Lookup",
                "typeProperties": {
                    "source": {
                        "type": "BlobSource"
                    },
                    "dataset": { 
                        "referenceName": "LookupDataset", 
                        "type": "DatasetReference" 
                    }
                }
            },
            {
                "name": "CopyActivity",
                "type": "Copy",
                "typeProperties": {
                    "source": { 
                        "type": "SqlSource", 
                        "sqlReaderQuery": "select * from @{activity('LookupActivity').output.tableName}" 
                    },
                    "sink": { 
                        "type": "BlobSink" 
                    }
                },                
                "dependsOn": [ 
                    { 
                        "activity": "LookupActivity", 
                        "dependencyConditions": [ "Succeeded" ] 
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
            }
        ]
    }
}
```

### <a name="lookup-dataset"></a>Vyhledávání datové sady
Vyhledávání datová sada odkazuje na soubor sourcetable.json ve složce vyhledávání ve službě Azure Storage určeného AzureStorageLinkedService. 

```json
{
    "name": "LookupDataset",
    "properties": {
        "type": "AzureBlob",
        "typeProperties": {
            "folderPath": "lookup",
            "fileName": "sourcetable.json",
            "format": {
                "type": "JsonFormat",
                "filePattern": "SetOfObjects"
            }
        },
        "linkedServiceName": {
            "referenceName": "AzureStorageLinkedService",
            "type": "LinkedServiceReference"
        }
    }
}
```

### <a name="source-dataset-for-the-copy-activity"></a>Datové sady zdroje pro aktivitu kopírování
Datové sady zdroje používá výstup aktivity vyhledávání, což je název tabulky SQL. Aktivita kopírování kopíruje data z této tabulky SQL do umístění ve službě Azure Blob Storage určeného podřízený datovou sadu. 

```json
{
    "name": "SourceDataset",
    "properties": {
        "type": "AzureSqlTable",
        "typeProperties":{
            "tableName": "@{activity('LookupActivity').output.tableName}"
        },
        "linkedServiceName": {
            "referenceName": "AzureSqlLinkedService",
            "type": "LinkedServiceReference"
        }
    }
}
```

### <a name="sink-dataset-for-the-copy-activity"></a>Jímky datovou sadu pro aktivitu kopírování
Aktivita kopírování kopíruje data z tabulky SQL do filebylookup.csv soubor ve složce sdíleného svazku clusteru ve službě Azure Storage určeného AzureStorageLinkedService. 

```json
{
    "name": "SinkDataset",
    "properties": {
        "type": "AzureBlob",
        "typeProperties": {
            "folderPath": "csv",
            "fileName": "filebylookup.csv",
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

### <a name="azure-storage-linked-service"></a>Propojená služba Azure Storage
Tento účet úložiště obsahuje soubor JSON s názvy tabulek SQL. 

```json
{
    "properties": {
        "type": "AzureStorage",
        "typeProperties": {
            "connectionString": {
                "value": "DefaultEndpointsProtocol=https;AccountName=<StorageAccountName>;AccountKey=<StorageAccountKey>",
                "type": "SecureString"
            }
        }
    },
        "name": "AzureStorageLinkedService"
}
```

### <a name="azure-sql-database-linked-service"></a>Propojená služba Azure SQL Database
Tato databáze Azure SQL obsahuje data, která mají být zkopírovány do úložiště objektů blob. 

```json
{
    "name": "AzureSqlLinkedService",
    "properties": {
        "type": "AzureSqlDatabase",
        "description": "",
        "typeProperties": {
            "connectionString": {
                "value": "Server=<server>;Initial Catalog=<database>;User ID=<user>;Password=<password>;",
                "type": "SecureString"
            }
        }
    }
}
```

### <a name="sourcetablejson"></a>SourceTable.JSON

#### <a name="set-of-objects"></a>Sada objektů

```json
{
  "Id": "1",
  "tableName": "Table1",
}
{
   "Id": "2",
  "tableName": "Table2",
}
```
#### <a name="array-of-objects"></a>Pole objektů

```json
[ 
    {
        "Id": "1",
          "tableName": "Table1",
    }
    {
        "Id": "2",
        "tableName": "Table2",
    }
]
```



## <a name="type-properties"></a>Vlastnosti typu
Name (Název) | Popis | Typ | Požaduje se
---- | ----------- | ---- | --------
Datové sady | Atribut datové sady je poskytnout odkaz na datovou sadu pro vyhledávání. V současné době jsou typy podporované datové sady:<ul><li>FileShareDataset</li><li>AzureBlobDataset</li><li>AzureSqlTableDataset</li><li>AzureTableDataset</li> | Dvojice klíč/hodnota | Ano
Zdroj | Vlastnosti specifické pro datové sady zdroje, stejně jako zdroj kopie aktivity | Dvojice klíč/hodnota | Ne
firstRowOnly | Vrátí první řádek nebo všechny řádky. | Logická hodnota | Ne

## <a name="next-steps"></a>Další kroky
Najdete v části Další aktivity toku řízení podporovaných službou Data Factory: 

- [Aktivita spuštění kanálu](control-flow-execute-pipeline-activity.md)
- [Pro každou aktivitu](control-flow-for-each-activity.md)
- [Aktivita GetMetadata](control-flow-get-metadata-activity.md)
- [Webové aktivity](control-flow-web-activity.md)
