---
title: "Vytváření aktivačních událostí v Azure Data Factory | Microsoft Docs"
description: "Naučte se vytvořit aktivační událost v Azure Data Factory, který kanál spouští podle plánu."
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
ms.date: 12/11/2017
ms.author: shlo
ms.openlocfilehash: eed286c01604ab0e9ac2113d56cbce268503668d
ms.sourcegitcommit: aaba209b9cea87cb983e6f498e7a820616a77471
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/12/2017
---
# <a name="how-to-create-a-trigger-that-runs-a-pipeline-on-a-schedule"></a>Jak vytvořit aktivační událost, která spouští kanál podle plánu
Tento článek obsahuje kroky k vytvoření, spuštění a monitorování aktivační událost. Koncepční informace o aktivační události najdete v tématu [kanálu spouštěcí a aktivační události](concepts-pipeline-execution-triggers.md).

> [!NOTE]
> Tento článek se týká verze 2 služby Data Factory, která je aktuálně ve verzi Preview. Pokud používáte verzi 1 služby Data Factory, který je všeobecně dostupná (GA), prostudujte si [úvod do služby Data Factory verze 1](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).


## <a name="use-azure-powershell"></a>Použití Azure Powershell
V této části se dozvíte, jak pomocí prostředí Azure PowerShell k vytvoření, spuštění a monitorování aktivační událost. Pokud chcete zobrazit tento ukázkový pracovní, nejdřív projít [rychlý start: Vytvořte objekt pro vytváření dat pomocí Azure PowerShell](quickstart-create-data-factory-powershell.md). Hlavní metodu, která se vytvoří a spustí plán aktivační událost, která se spouští každých 15 minut, přidejte následující kód. Aktivační událost souvisí s kanálu (**Adfv2QuickStartPipeline**), které vytvoříte jako součást rychlý start.

1. Vytvořte soubor JSON s názvem MyTrigger.json ve složce C:\ADFv2QuickStartPSH\ s následujícím obsahem: 

    > [!IMPORTANT]
    > Nastavit **startTime** na aktuální čas UTC a **endTime** na jednu hodinu po aktuální čas UTC Čas před uložením souboru JSON.

    ```json   
    {
        "properties": {
            "name": "MyTrigger",
            "type": "ScheduleTrigger",
            "typeProperties": {
                "recurrence": {
                    "frequency": "Minute",
                    "interval": 15,
                    "startTime": "2017-12-08T00:00:00",
                    "endTime": "2017-12-08T01:00:00"
                }
            },
            "pipelines": [{
                    "pipelineReference": {
                        "type": "PipelineReference",
                        "referenceName": "Adfv2QuickStartPipeline"
                    },
                    "parameters": {
                        "inputPath": "adftutorial/input",
                        "outputPath": "adftutorial/output"
                    }
                }
            ]
        }
    }
    ```
    
    V tomto fragmentu kódu JSON: 
    - **Typ** aktivační události je nastaven na **ScheduleTrigger**. 
    - **Frekvence** je nastaven na **minutu** a **interval** je nastaven na **15**. Proto aktivační událost spustí kanálu každých 15 minut mezi počáteční a koncový čas. 
    - **EndTime** je jedna hodina po **startTime**, takže aktivační událost spouští kanálu 15 minut, 30 minut a 45 minut po čas spuštění. Nezapomeňte aktualizovat na aktuální čas UTC a čas ukončení jedné hodině po čas spuštění čas spuštění.  
    - Aktivační událost je přidružena **Adfv2QuickStartPipeline** kanálu. Chcete-li přidružit více kanálů aktivační událost, přidejte další **pipelineReference** oddíly. 
    - Kanál v rychlé spuštění trvá dva **parametry**. Proto předat hodnoty pro tyto parametry z aktivační událost. 
2. Vytvořit aktivační událost pomocí **Set-AzureRmDataFactoryV2Trigger** rutiny.

    ```powershell
    Set-AzureRmDataFactoryV2Trigger -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name "MyTrigger" -DefinitionFile "C:\ADFv2QuickStartPSH\MyTrigger.json"
    ```
3. Zkontrolujte, zda je stav aktivační události **Zastaveno** pomocí **Get-AzureRmDataFactoryV2Trigger** rutiny. 

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

## <a name="use-net-sdk"></a>Použití sady .NET SDK
V této části ukazuje, jak používat sadu .NET SDK k vytvoření, spuštění a monitorování aktivační událost. Pokud chcete zobrazit tento kód práce, nejdřív projít [rychlý start pro vytvoření objekt pro vytváření dat pomocí sady .NET SDK](quickstart-create-data-factory-dot-net.md). Hlavní metodu, která se vytvoří a spustí plán aktivační událost, která se spouští každých 15 minut, přidejte následující kód. Aktivační událost souvisí s kanálu (**Adfv2QuickStartPipeline**), které vytvoříte jako součást rychlý start. 

```csharp
            //create the trigger
            Console.WriteLine("Creating the trigger");

            // set the start time to the current UTC time
            DateTime startTime = DateTime.UtcNow;

            // specify values for the inputPath and outputPath parameters
            Dictionary<string, object> pipelineParameters = new Dictionary<string, object>();
            pipelineParameters.Add("inputPath", "adftutorial/input");
            pipelineParameters.Add("outputPath", "adftutorial/output");

            // create a schedule trigger
            string triggerName = "MyTrigger";
            ScheduleTrigger myTrigger = new ScheduleTrigger()
            {
                Pipelines = new List<TriggerPipelineReference>()
                {
                    // associate the Adfv2QuickStartPipeline pipeline with the trigger
                    new TriggerPipelineReference()
                    {
                        PipelineReference = new PipelineReference(pipelineName),
                        Parameters = pipelineParameters,
                    }
                },
                Recurrence = new ScheduleTriggerRecurrence()
                {
                    // set the start time to current UTC time and the end time to be one hour after.
                    StartTime = startTime,
                    TimeZone = "UTC",
                    EndTime = startTime.AddHours(1),
                    Frequency = RecurrenceFrequency.Minute,
                    Interval = 15,
                }
            };

            // now, create the trigger by invoking CreateOrUpdate method. 
            TriggerResource triggerResource = new TriggerResource()
            {
                Properties = myTrigger
            };
            client.Triggers.CreateOrUpdate(resourceGroup, dataFactoryName, triggerName, triggerResource);

            //start the trigger
            Console.WriteLine("Starting the trigger");
            client.Triggers.Start(resourceGroup, dataFactoryName, triggerName);

```

Ke sledování aktivační události spuštění, přidejte následující kód před poslední `Console.WriteLine` příkaz: 

```csharp
            // Check the trigger runs every 15 minutes
            Console.WriteLine("Trigger runs. You see the output every 15 minutes");

            for (int i = 0; i < 3; i++)
            {
                System.Threading.Thread.Sleep(TimeSpan.FromMinutes(15));
                List<TriggerRun> triggerRuns = client.Triggers.ListRuns(resourceGroup, dataFactoryName, triggerName, DateTime.UtcNow.AddMinutes(-15 * (i + 1)), DateTime.UtcNow.AddMinutes(2)).ToList();
                Console.WriteLine("{0} trigger runs found", triggerRuns.Count);

                foreach (TriggerRun run in triggerRuns)
                {
                    foreach (KeyValuePair<string, string> triggeredPipeline in run.TriggeredPipelines)
                    {
                        PipelineRun triggeredPipelineRun = client.PipelineRuns.Get(resourceGroup, dataFactoryName, triggeredPipeline.Value);
                        Console.WriteLine("Pipeline run ID: {0}, Status: {1}", triggeredPipelineRun.RunId, triggeredPipelineRun.Status);
                        List<ActivityRun> runs = client.ActivityRuns.ListByPipelineRun(resourceGroup, dataFactoryName, triggeredPipelineRun.RunId, run.TriggerRunTimestamp.Value, run.TriggerRunTimestamp.Value.AddMinutes(20)).ToList();
                    }
                }
            }
```

Monitorování spustí nebo kanálu aktivační událost se spustí v portálu Azure najdete v tématu [spouští monitorování kanálu](quickstart-create-data-factory-resource-manager-template.md#monitor-the-pipeline)

## <a name="use-python-sdk"></a>Používat jazyk Python SDK
V této části se dozvíte, jak používat sadu Python SDK k vytvoření, spuštění a monitorování aktivační událost. Pokud chcete zobrazit tento kód práce, nejdřív projít [rychlý start pro vytvoření objekt pro vytváření dat pomocí sady SDK pro Python](quickstart-create-data-factory-python.md). Pak přidejte následující blok kódu po "spustit kanál monitorovat" blok kódu ve skriptu jazyka python. Tento kód vytvoří plán aktivační událost, která spouští každých 15 minut mezi zadaným počátečním a koncovým časem. Aktualizujte start_time na aktuální čas UTC a end_time na jednu hodinu po aktuálním čase UTC. 

```python
    # Create a trigger
    tr_name = 'mytrigger'
    scheduler_recurrence = ScheduleTriggerRecurrence(frequency='Minute', interval='15',start_time='2017-12-12T04:00:00', end_time='2017-12-12T05:00:00', time_zone='UTC') 
    pipeline_parameters = {'inputPath':'adftutorial/input', 'outputPath':'adftutorial/output'}
    pipelines_to_run = []
    pipeline_reference = PipelineReference('copyPipeline')
    pipelines_to_run.append(TriggerPipelineReference(pipeline_reference, pipeline_parameters))
    tr_properties = ScheduleTrigger(description='My scheduler trigger', pipelines = pipelines_to_run, recurrence=scheduler_recurrence)    
    adf_client.triggers.create_or_update(rg_name, df_name, tr_name, tr_properties)

    # start the trigger
    adf_client.triggers.start(rg_name, df_name, tr_name)
```
Monitorování spustí nebo kanálu aktivační událost se spustí v portálu Azure najdete v tématu [spouští monitorování kanálu](quickstart-create-data-factory-resource-manager-template.md#monitor-the-pipeline)

## <a name="use-resource-manager-template"></a>Pomocí Správce prostředků šablony
Šablonu Azure Resource Manager můžete vytvořit aktivační událost. Podrobné pokyny najdete v tématu [vytvořit objekt pro vytváření dat Azure pomocí šablony Resource Manageru](quickstart-create-data-factory-resource-manager-template.md).  

## <a name="pass-the-trigger-start-time-to-a-pipeline"></a>Čas spuštění aktivační události předat kanálu
Azure Data Factory v verze 1, podporované čtení nebo zápis oddílů dat pomocí SliceStart/SliceEnd/WindowStart/WindowEnd systémové proměnné. Ve verzi 2 můžete dosáhnout toto chování pomocí parametru kanálu a čas nebo naplánovaný čas spuštění aktivační události jako hodnotu parametru. V následujícím příkladu je naplánovaném čase aktivační události předat jako hodnota scheduledRunTime parametr kanálu. 

```json
"parameters": {
    "scheduledRunTime": "@trigger().scheduledTime"
}
```    
Další informace najdete v tématu [jak pro čtení nebo zápis dat rozdělena na oddíly](how-to-read-write-partitioned-data.md).

## <a name="next-steps"></a>Další kroky
Podrobné informace o aktivační události najdete v tématu [kanálu spouštěcí a aktivační události](concepts-pipeline-execution-triggers.md#triggers).