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
ms.date: 12/12/2017
ms.author: spelluru
ms.openlocfilehash: f287b0287ad85ffe1654e0d574cd44aa4dd81a0f
ms.sourcegitcommit: 3fca41d1c978d4b9165666bb2a9a1fe2a13aabb6
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/15/2017
---
# <a name="lookup-activity-in-azure-data-factory"></a>Aktivita vyhledávání v Azure Data Factory
Aktivita vyhledávání slouží ke čtení nebo vyhledání záznamu / názvu tabulky / hodnoty z jakéhokoli externího zdroje. Na tento výstup mohou dále odkazovat následující aktivity. 

Vyhledávání aktivity je užitečné, pokud chcete dynamicky načíst seznam souborů nebo záznamy nebo tabulky z konfiguračního souboru nebo zdroj dat. Výstup z aktivity další lze ostatní aktivity k provedení určité zpracování na jen ty položky.

> [!NOTE]
> Tento článek se týká verze 2 služby Data Factory, která je aktuálně ve verzi Preview. Pokud používáte verzi 1 služby Data Factory, který je všeobecně dostupná (GA), přečtěte si téma [Data Factory V1 dokumentaci](v1/data-factory-introduction.md).

## <a name="supported-capabilities"></a>Podporované možnosti

Následující zdroje dat jsou aktuálně podporovány pro vyhledávání:
- Soubor JSON v Azure Blob
- Soubor JSON v systému souborů
- Azure SQL Database (JSON dat převedených z dotazu)
- Azure SQL Data Warehouse (JSON dat převedených z dotazu)
- SQL Server (JSON dat převedených z dotazu)
- Azure Table Storage (JSON dat převedených z dotazu)

## <a name="syntax"></a>Syntaxe

```json
{
    "name": "LookupActivity",
    "type": "Lookup",
    "typeProperties": {
        "source": {
            "type": "<source type>"
            <additional source specific properties (optional)>
        },
        "dataset": { 
            "referenceName": "<source dataset name>",
            "type": "DatasetReference"
        },
        "firstRowOnly": false
    }
}
```

## <a name="type-properties"></a>Vlastnosti typu
Name (Název) | Popis | Typ | Požaduje se
---- | ----------- | ---- | --------
Datové sady | Atribut datové sady je poskytnout odkaz na datovou sadu pro vyhledávání. V současné době jsou typy podporované datové sady:<ul><li>`AzureBlobDataset`pro [Azure Blob Storage](connector-azure-blob-storage.md#dataset-properties) jako zdroj</li><li>`FileShareDataset`pro [systém souborů](connector-file-system.md#dataset-properties) jako zdroj</li><li>`AzureSqlTableDataset`pro [Azure SQL Database](connector-azure-sql-database.md#dataset-properties) nebo [Azure SQL Data Warehouse](connector-azure-sql-data-warehouse.md#dataset-properties) jako zdroj</li><li>`SqlServerTable`pro [systému SQL Server](connector-sql-server.md#dataset-properties) jako zdroj</li><li>`AzureTableDataset`pro [Azure Table Storage](connector-azure-table-storage.md#dataset-properties) jako zdroj</li> | Dvojice klíč/hodnota | Ano
Zdroj | Vlastnosti specifické pro datové sady zdroje, stejně jako zdroj kopie aktivity. Další podrobnosti o z části "Zkopírovat vlastnosti aktivity" v jednotlivých odpovídající konektor článků. | Dvojice klíč/hodnota | Ano
firstRowOnly | Označuje, jestli se mají vracet pouze první řádek nebo všechny řádky. | Logická hodnota | Ne. Výchozí hodnota je `ture`.

## <a name="use-lookup-activity-result-in-subsequent-activity"></a>Výsledek vyhledávání aktivity použít následné aktivity

Výsledek vyhledávání je vrácený v `output` části v rámci aktivity výsledek spuštění.

**Když `firstRowOnly` je nastaven na `true` (výchozí)**, formát výstupu se následujícím způsobem. Výsledek vyhledávání je pod pevná `firstRow` klíč. Pokud chcete použít výsledek následné aktivity, použijte vzor `@{activity('MyLookupActivity').output.firstRow.TableName}`.

```json
{
    "firstRow":
    {
        "Id": "1",
        "TableName" : "Table1"
    }
}
```

**Když `firstRowOnly` je nastaven na `false`** , formát výstupu se následujícím způsobem. A `count` pole určuje, kolik záznamů se vrátí, a podrobné hodnoty jsou v části pevná `value` pole. V takovém případě je vyhledávání aktivity obvykle následuje [Foreach aktivity](control-flow-for-each-activity.md), můžete předat `value` pole pro aktivitu ForEach `items` pole pomocí vzor `@activity('MyLookupActivity').output.value`. Přístup k elementům ve `value`, použijte následující syntaxi: `@{activity('lookupActivity').output.value[zero based index].propertyname}`. Tady je příklad:`@{activity('lookupActivity').output.value[0].tablename}`

```json
{
    "count": "2",
    "value": [
        {
            "Id": "1",
            "TableName" : "Table1"
        },
        {
            "Id": "2",
            "TableName" : "Table2"
        }
    ]
} 
```

## <a name="example"></a>Příklad
V tomto příkladu aktivitě kopírování kopíruje data z tabulky SQL v databázi Azure SQL do Azure Blob Storage. Název tabulky SQL je uložené v souboru JSON ve službě Blob Storage. Aktivita vyhledávání vyhledá název tabulky za běhu. Tento přístup umožňuje JSON má být změněn dynamicky bez opětovného nasazení kanálů nebo datové sady. 

Tento příklad ukazuje vyhledávání pouze první řádek. Vyhledávání všechny řádky a řetězec s aktivitou ForEach, najdete v části [kurz – hromadné kopírování dat](tutorial-bulk-copy.md) ukázka.

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
                        "sqlReaderQuery": "select * from @{activity('LookupActivity').output.firstRow.tableName}" 
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
            "tableName": "@{activity('LookupActivity').output.firstRow.tableName}"
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

## <a name="next-steps"></a>Další kroky
Najdete v části Další aktivity toku řízení podporovaných službou Data Factory: 

- [Aktivita spuštění kanálu](control-flow-execute-pipeline-activity.md)
- [Pro každou aktivitu](control-flow-for-each-activity.md)
- [Aktivita GetMetadata](control-flow-get-metadata-activity.md)
- [Webové aktivity](control-flow-web-activity.md)
