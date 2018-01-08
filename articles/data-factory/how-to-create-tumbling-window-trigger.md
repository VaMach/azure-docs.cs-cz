---
title: "Postup vytvoření přeskakující okno aktivační události v Azure Data Factory | Microsoft Docs"
description: "Naučte se vytvořit aktivační událost v Azure Data Factory, která běží na přeskakující okno kanálu."
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
ms.date: 01/05/2018
ms.author: shlo
ms.openlocfilehash: a3b056ae4bb4eda26fec58ca3b6bed7f0744e36e
ms.sourcegitcommit: 1d423a8954731b0f318240f2fa0262934ff04bd9
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/05/2018
---
# <a name="how-to-create-a-trigger-that-runs-a-pipeline-on-a-tumbling-window"></a>Jak vytvořit aktivační událost, která běží na přeskakující okno kanálu
Tento článek obsahuje kroky k vytvoření, spuštění a monitorování aktivační událost přeskakující okno. Obecné informace o aktivační události a typy podporujeme najdete v tématu [kanálu spouštěcí a aktivační události](concepts-pipeline-execution-triggers.md).

> [!NOTE]
> Tento článek se týká verze 2 služby Data Factory, která je aktuálně ve verzi Preview. Pokud používáte verzi 1 služby Data Factory, který je všeobecně dostupná (GA), prostudujte si [úvod do služby Data Factory verze 1](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).

Přeskakující okno aktivační události jsou typ aktivační událost, která aktivuje se v pravidelných časovém intervalu z zadaný počáteční čas, při zachování stavu. Přeskakující windows jsou řadu pevnou velikostí, které se nepřekrývají a souvislý časové intervaly. Přeskakující okno aktivační událost je v relaci 1:1 se zřetězením příkazů a může odkazovat pouze na singulární kanálu.

## <a name="tumbling-window-trigger-type-properties"></a>Přeskakujícího okna vlastnosti typ aktivační události
```  
{
    "name": "MyTriggerName",
    "properties": {
        "type": "TumblingWindowTrigger",
        "runtimeState": "<<Started/Stopped/Disabled - readonly>>",
        "typeProperties": {
            "frequency": "<<Minute/Hour>>",
            "interval": <<int>>,
            "startTime": "<<datetime>>",
            "endTime: "<<datetime – optional>>"",
            "delay": "<<timespan – optional>>",
            “maxConcurrency”: <<int>> (required, max allowed: 50),
            "retryPolicy": {
                "count":  <<int - optional, default: 0>>,
                “intervalInSeconds”: <<int>>,
            }
        },
        "pipeline":
            {
                "pipelineReference": {
                    "type": "PipelineReference",
                    "referenceName": "MyPipelineName"
                },
                "parameters": {
                    "parameter1": {
                        "type": "Expression",
                        "value": "@{concat('output',formatDateTime(trigger().outputs.windowStartTime,'-dd-MM-yyyy-HH-mm-ss-ffff'))}"
                    },
                    "parameter2": {
                        "type": "Expression",
                        "value": "@{concat('output',formatDateTime(trigger().outputs.windowEndTime,'-dd-MM-yyyy-HH-mm-ss-ffff'))}"
                    },
                    "parameter3": "https://mydemo.azurewebsites.net/api/demoapi"
                }
            }
      }    
}
```  

Následující tabulka obsahuje přehled důležité elementy související s opakování a plánování v aktivační události přeskakující okno.

| **Název JSON** | **Popis** | **Povolené hodnoty** | **Požadované** |
|:--- |:--- |:--- |:--- |
| **Typ** | Typ aktivační události. Tento problém je vyřešený jako "TumblingWindowTrigger." | Řetězec | Ano |
| **runtimeState** | <readOnly>Možné hodnoty: Spuštěna, zastavena, zakázán | Řetězec | Ano, jen pro čtení |
| **frekvence** |*Frekvence* řetězec představující frekvence jednotka, kdy dojde znovu aktivační událost. Podporované hodnoty jsou "minutu" a "hodinu." Pokud čas zahájení má částí data, která jsou podrobnější než je četnost, se provedou v úvahu k výpočtu okno hranice. Pro ex: Pokud frekvence se každou hodinu a čas spuštění je 2016-04-01T10:10:10Z, je první okno (2017-09-01T10:10:10Z 2017-09-01T11:10:10Z.)  | Řetězec. Podporované typy "minut", "hodina" | Ano |
| **interval** |*Interval* je kladné celé číslo a označuje je interval *frekvence* který určuje, jak často se spustí aktivační událost. Například pokud *interval* 3 a *frekvence* je "hodina", aktivační událost opakovat každých 3 hodiny. | Integer | Ano |
| **čas spuštění**|*startTime* je datum a čas. *startTime* je první výskyt a může být v minulosti. První interval aktivační události bude (čas spuštění, startTime + interval). | Datum a čas | Ano |
| **čas ukončení**|*čas ukončení* je datum a čas. *čas ukončení* je poslední výskyt a může být v minulosti. | Datum a čas | Ano |
| **zpoždění** | Zadejte zpoždění před zpracováním dat spustí okno. Kanál spustit spustí po očekávaný čas provádění + zpoždění. Zpoždění definuje, jak dlouho se má aktivační procedura čeká po splatnosti čas před aktivací nové spuštění. Čas zahájení okna ho nebude změnit. | Časový interval (Příklad: 00:10:00, znamená zpoždění 10 minut) |  Ne. Výchozí hodnota je "00: 00:00" |
| **maximální souběžnosti** | Počet souběžných aktivační událost spustí, které se aktivuje například pro windows, které jsou připravené. Příklad: Pokud se s vámi snažíme k obnovení dat každou hodinu pro včerejšího dne, který by 24 windows. Pokud souběžnosti = 10, aktivační události při vyvolání pouze pro prvních 10 windows (00:00-01:00 - 09:00 – 10:00). Po dokončení prvních 10 běží spouštěná kanálu se spustí aktivační událost při vyvolání pro další 10 (10:00 – 11:00 – 19:00 až 20:00). Pokračováním příklad souběžnosti = 10, pokud existují 10 windows připravený, bude 10 běží kanálu. Pokud je pouze 1 okno připraven, budou existovat jenom 1 spustit kanál. | Integer | Ano. Možné hodnoty 1 – 50 |
| **retryPolicy: počet** | Počet opakovaných pokusů před spuštěním kanálu je označena jako "Failed"  | Integer |  Ne. Výchozí hodnota je 0 opakování |
| **retryPolicy: intervalInSeconds** | Prodleva mezi pokusy o opakování v sekundách. | Integer |  Ne. Výchozí hodnota je 30 sekund. |

### <a name="using-system-variables-windowstart-and-windowend"></a>Pomocí systémové proměnné: WindowStart a WindowEnd

Pokud chcete použít WindowStart a WindowEnd přeskakující okno triggeru ve vaší **kanálu** definice (tj. pro část dotazu), musí proměnné předat jako parametry do kanálu v **aktivační událost**definice, například takto:
```  
{
    "name": "MyTriggerName",
    "properties": {
        "type": "TumblingWindowTrigger",
            ...
        "pipeline":
            {
                "pipelineReference": {
                    "type": "PipelineReference",
                    "referenceName": "MyPipelineName"
                },
                "parameters": {
                    "MyWindowStart": {
                        "type": "Expression",
                        "value": "@{concat('output',formatDateTime(trigger().outputs.windowStartTime,'-dd-MM-yyyy-HH-mm-ss-ffff'))}"
                    },
                    "MyWindowEnd": {
                        "type": "Expression",
                        "value": "@{concat('output',formatDateTime(trigger().outputs.windowEndTime,'-dd-MM-yyyy-HH-mm-ss-ffff'))}"
                    }
                }
            }
      }    
}
```  

Potom v definici kanálu WindowStart a WindowEnd hodnoty, použijte parametry odpovídajícím způsobem "MyWindowStart" a "MyWindowEnd"

### <a name="notes-on-backfill"></a>Poznámky k obnovení dat
Pokud nejsou k dispozici více windows ke spuštění (esp. ve scénáři obnovení dat), pořadí spouštění systému windows je deterministická a bude z nejstarší nejnovější intervalech. Neexistuje žádný způsob, jak toto chování od nyní změnit.

### <a name="updating-an-existing-triggerresource"></a>Aktualizace stávající TriggerResource
* Frekvence (nebo velikost okna), aktivační události dojde ke změně, stav windows již zpracována bude *není* resetován. Aktivační událost bude pokračovat pálení pro windows z poslední ten, který se provedla pomocí nové velikost okna.
* Pokud je aktivační událost změny (přidán nebo aktualizován), stav windows již koncový čas zpracování bude *není* resetován. Aktivační událost se jednoduše nakládá nový koncový čas. Pokud vybraný konečný čas je před windows již proveden, aktivační událost se zastaví. Jinak se zastaví, jakmile narazí nový koncový čas.

## <a name="sample-using-azure-powershell"></a>Ukázka pomocí Azure PowerShell
V této části se dozvíte, jak pomocí prostředí Azure PowerShell k vytvoření, spuštění a monitorování aktivační událost.

1. Vytvořte soubor JSON s názvem MyTrigger.json ve složce C:\ADFv2QuickStartPSH\ s následujícím obsahem:

   > [!IMPORTANT]
   > Nastavit **startTime** na aktuální čas UTC a **endTime** na jednu hodinu po aktuální čas UTC Čas před uložením souboru JSON.

    ```json   
    {
      "name": "PerfTWTrigger",
      "properties": {
        "type": "TumblingWindowTrigger",
        "typeProperties": {
          "frequency": "Minute",
          "interval": "15",
          "startTime": "2017-09-08T05:30:00Z",
          "delay": "00:00:01",
          "retryPolicy": {
            "count": 2,
            "intervalInSeconds": 30
          },
          "maxConcurrency": 50
        },
        "pipeline": {
          "pipelineReference": {
            "type": "PipelineReference",
            "referenceName": "DynamicsToBlobPerfPipeline"
          },
          "parameters": {
            "windowStart": "@trigger().outputs.windowStartTime",
            "windowEnd": "@trigger().outputs.windowEndTime"
          }
        },
        "runtimeState": "Started"
      }
    }
    ```  
2. Vytvořit aktivační událost pomocí **Set-AzureRmDataFactoryV2Trigger** rutiny.

    ```powershell
    Set-AzureRmDataFactoryV2Trigger -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name "MyTrigger" -DefinitionFile "C:\ADFv2QuickStartPSH\MyTrigger.json"
    
3. Confirm that the status of the trigger is **Stopped** by using the **Get-AzureRmDataFactoryV2Trigger** cmdlet.

    ```powershell
    Get-AzureRmDataFactoryV2Trigger -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name "MyTrigger"
    ```
4. Spusťte aktivační událost pomocí **Start-AzureRmDataFactoryV2Trigger** rutiny:

    ```powershell
    Start-AzureRmDataFactoryV2Trigger -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name "MyTrigger"
    ```
5. Zkontrolujte, zda je stav aktivační události **Začínáme** pomocí **Get-AzureRmDataFactoryV2Trigger** rutiny.

    ```powershell
    Get-AzureRmDataFactoryV2Trigger -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name "MyTrigger"
    ```
6.  Získat spustí aktivační událost pomocí prostředí PowerShell pomocí **Get-AzureRmDataFactoryV2TriggerRun** rutiny. Chcete-li získat informace o aktivační události spuštění, spusťte následující příkaz pravidelně: aktualizace **TriggerRunStartedAfter** a **TriggerRunStartedBefore** hodnoty shodují s hodnotami v definici aktivační události .

    ```powershell
    Get-AzureRmDataFactoryV2TriggerRun -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -TriggerName "MyTrigger" -TriggerRunStartedAfter "2017-12-08T00:00:00" -TriggerRunStartedBefore "2017-12-08T01:00:00"
    ```
Monitorování spustí nebo kanálu aktivační událost se spustí v portálu Azure najdete v tématu [spouští monitorování kanálu](quickstart-create-data-factory-resource-manager-template.md#monitor-the-pipeline)

## <a name="next-steps"></a>Další postup
Podrobné informace o aktivační události najdete v tématu [kanálu spouštěcí a aktivační události](concepts-pipeline-execution-triggers.md#triggers).
