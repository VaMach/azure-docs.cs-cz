---
title: "Jak číst nebo zapisovat na oddíly dat v Azure Data Factory | Microsoft Docs"
description: "Zjistěte, jak číst nebo zapisovat oddílů dat Azure Data Factory verze 2."
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: 
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/09/2017
ms.author: shlo
ms.openlocfilehash: ee83fce3eeef4bde6dc8e0ea6f17b40396619412
ms.sourcegitcommit: 6a22af82b88674cd029387f6cedf0fb9f8830afd
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/11/2017
---
# <a name="how-to-read-or-write-partitioned-data-in-azure-data-factory-version-2"></a>Jak číst nebo zapisovat data v Azure Data Factory verze 2 rozdělena na oddíly
Azure Data Factory v verze 1, podporované čtení nebo zápis oddílů dat pomocí SliceStart/SliceEnd/WindowStart/WindowEnd systémové proměnné. Ve verzi 2 můžete dosáhnout toto chování pomocí parametru kanálu a čas nebo naplánovaný čas spuštění aktivační události jako hodnotu parametru. 

## <a name="use-a-pipeline-parameter"></a>Pomocí parametru kanálu 
Ve verzi 1 můžete použít vlastnost partitionedBy a SliceStart systémové proměnné, jak je znázorněno v následujícím příkladu: 

```json
"folderPath": "adfcustomerprofilingsample/logs/marketingcampaigneffectiveness/yearno={Year}/monthno={Month}/dayno={Day}/",
"partitionedBy": [
    { "name": "Year", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyy" } },
    { "name": "Month", "value": { "type": "DateTime", "date": "SliceStart", "format": "%M" } },
    { "name": "Day", "value": { "type": "DateTime", "date": "SliceStart", "format": "%d" } }
],
```

Další informace o vlastnosti partitonedBy najdete v tématu [konektor Azure Blob verze 1](v1/data-factory-azure-blob-connector.md#dataset-properties) článku. 

V verze 2 je způsob, jak dosáhnout toto chování provést následující akce: 

1. Definování **kanálu parametr** typu řetězec. V následujícím příkladu je název parametru kanálu **ScheduledRunTime**. 
2. Nastavit **folderPath** v definici datové sady na hodnotu parametru kanálu pro jak je znázorněno v příkladu. 
3. Před spuštěním kanálu předáte pevně zakódované hodnotu pro parametr. Nebo předejte aktivační událost počáteční čas nebo naplánovaném čase dynamicky za běhu. 

```json
"folderPath": {
      "value": "@concat(pipeline().parameters.blobContainer, '/logs/marketingcampaigneffectiveness/yearno=', formatDateTime(pipeline().parameters.ScheduledRunTime, 'yyyy'), '/monthno=', formatDateTime(pipeline().parameters.ScheduledRunTime, '%M'), '/dayno=', formatDateTime(pipeline().parameters.ScheduledRunTime, '%d'), '/')",
      "type": "Expression"
},
```

## <a name="pass-in-value-from-a-trigger"></a>Předejte hodnotu z aktivační události
V následující definici aktivace naplánovaném čase aktivační události je předán jako hodnotu pro parametr ScheduledRunTime kanál: 

```json
{
    "name": "MyTrigger",
    "properties": {
       ...
        },
        "pipeline": {
            "pipelineReference": {
                "type": "PipelineReference",
                "referenceName": "MyPipeline"
            },
            "parameters": {
                "ScheduledRunTime": "@trigger().scheduledTime"
            }
        }
    }
}
```

## <a name="example"></a>Příklad

Zde je ukázka definice datové sady (která používá parametr s názvem: `date`):

```json
{
  "type": "AzureBlob",
  "typeProperties": {
    "folderPath": {
      "value": "@concat(pipeline().parameters.blobContainer, '/logs/marketingcampaigneffectiveness/yearno=', formatDateTime(pipeline().parameters.date, 'yyyy'), '/monthno=', formatDateTime(pipeline().parameters.date, '%M'), '/dayno=', formatDateTime(pipeline().parameters.date, '%d'), '/')",
      "type": "Expression"
    },
    "format": {
      "type": "TextFormat",
      "columnDelimiter": ","
    }
  },
  "structure": [
    { "name": "ProfileID", "type": "String" },
    { "name": "SessionStart", "type": "String" },
    { "name": "Duration", "type": "Int32" },
    { "name": "State", "type": "String" },
    { "name": "SrcIPAddress", "type": "String" },
    { "name": "GameType", "type": "String" },
    { "name": "Multiplayer", "type": "String" },
    { "name": "EndRank", "type": "String" },
    { "name": "WeaponsUsed", "type": "Int32" },
    { "name": "UsersInteractedWith", "type": "String" },
    { "name": "Impressions", "type": "String" }
  ],
  "linkedServiceName": {
    "referenceName": "churnStorageLinkedService",
    "type": "LinkedServiceReference"
  }
}
```

Definice kanál: 

```json
{
    "properties": {
        "activities": [{
            "type": "HDInsightHive",
            "typeProperties": {
                "scriptPath": {
                    "value": "@concat(pipeline().parameters.blobContainer, '/scripts/', pipeline().parameters.partitionHiveScriptFile)",
                    "type": "Expression"
                },
                "scriptLinkedService": {
                    "referenceName": "churnStorageLinkedService",
                    "type": "LinkedServiceReference"
                },
                "defines": {
                    "RAWINPUT": {
                        "value": "@concat('wasb://', pipeline().parameters.blobContainer, '@', pipeline().parameters.blobStorageAccount, '.blob.core.windows.net/logs/', pipeline().parameters.inputRawLogsFolder, '/')",
                        "type": "Expression"
                    },
                    "PARTITIONEDOUTPUT": {
                        "value": "@concat('wasb://', pipeline().parameters.blobContainer, '@', pipeline().parameters.blobStorageAccount, '.blob.core.windows.net/logs/partitionedgameevents/')",
                        "type": "Expression"
                    },
                    "Year": {
                        "value": "@formatDateTime(pipeline().parameters.date, 'yyyy')",
                        "type": "Expression"
                    },
                    "Month": {
                        "value": "@formatDateTime(pipeline().parameters.date, '%M')",
                        "type": "Expression"
                    },
                    "Day": {
                        "value": "@formatDateTime(pipeline().parameters.date, '%d')",
                        "type": "Expression"
                    }
                }
            },
            "linkedServiceName": {
                "referenceName": "HdiLinkedService",
                "type": "LinkedServiceReference"
            },
            "name": "HivePartitionGameLogs"
        }],
        "parameters": {
            "date": {
                "type": "String"
            },
            "blobStorageAccount": {
                "type": "String"
            },
            "blobContainer": {
                "type": "String"
            },
            "inputRawLogsFolder": {
                "type": "String"
            },
            "partitionHiveScriptFile": {
                "type": "String"
            }
        }
    }
}
```

## <a name="next-steps"></a>Další kroky
Kompletní návod, jak vytvořit objekt pro vytváření dat se zřetězením příkazů, najdete v části [rychlý start: Vytvořte objekt pro vytváření dat](quickstart-create-data-factory-powershell.md). 