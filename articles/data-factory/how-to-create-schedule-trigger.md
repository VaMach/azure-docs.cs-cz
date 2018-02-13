---
title: "Vytvoření plánu aktivačních událostí v Azure Data Factory | Microsoft Docs"
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
ms.date: 01/23/2018
ms.author: shlo
ms.openlocfilehash: 51e2dddbe66ca372d89fc8efeb24bdab9fe6a442
ms.sourcegitcommit: 9890483687a2b28860ec179f5fd0a292cdf11d22
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
# <a name="create-a-trigger-that-runs-a-pipeline-on-a-schedule"></a>Vytvořit aktivační událost, která spouští kanál podle plánu
Tento článek obsahuje informace o aktivační události plán a kroky k vytvoření, spuštění a monitorování plánu aktivační událost. U jiných typů aktivačních událostí, najdete v části [kanálu spouštěcí a aktivační události](concepts-pipeline-execution-triggers.md).

Při vytváření aktivační události plánu, můžete zadat plán (počáteční datum opakování, koncové datum atd.) pro aktivační události a přidružení se zřetězením příkazů. Mezi kanály a aktivačními událostmi existuje vztah n-m. Víc aktivačních událostí může aktivovat jeden kanál. Jedna aktivační událost může aktivovat více kanálů.

> [!NOTE]
> Tento článek se týká do Azure Data Factory verze 2, který je aktuálně ve verzi preview. Pokud používáte Azure Data Factory verze 1, který je všeobecně dostupná (GA), najdete v části [Začínáme s Azure Data Factory verze 1](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).

Následující části popisují vytvořit aktivační událost plán různými způsoby. 

## <a name="data-factory-ui"></a>Uživatelské rozhraní Data Factory
Můžete vytvořit **aktivační událost plán** při plánování kanálu pravidelně spouštět (hodinový, denní, atd.). 

> [!NOTE]
> Kompletní a podrobný postup vytvoření kanálu a plán aktivační událost, aktivační událost přidružením kanál a spuštění a monitorování kanálu, najdete v části [rychlý start: Vytvořte objekt pro vytváření dat pomocí uživatelského rozhraní objektu pro vytváření dat](quickstart-create-data-factory-portal.md).

1. Přepněte na kartu **Upravit**. 

    ![Přepnutí na kartu Upravit](./media/how-to-create-schedule-trigger/switch-edit-tab.png)
1. V nabídce klikněte na **Aktivační událost** a pak klikněte na **Nová / upravit**. 

    ![Nabídka Nová aktivační událost](./media/how-to-create-schedule-trigger/new-trigger-menu.png)
2. Na stránce **Přidat aktivační události** klikněte na **Zvolit aktivační událost...** a pak na **Nová**. 

    ![Přidat aktivační události – nová aktivační událost](./media/how-to-create-schedule-trigger/add-trigger-new-button.png)
3. V **novou aktivační událost** proveďte následující kroky: 

    1. Potvrďte, že **plán** je vybraná **typu**. 
    2. Zadejte datum a čas spuštění aktivační události pro **spustit datum (UTC)**. Jinak je nastavená na aktuální datum a čas ve výchozím nastavení. 
    3. Zadejte **opakování** pro aktivační událost. Vyberte jednu z hodnot z rozevíracího seznamu (každou minutu, hodinu, denně, týdně a měsíční). Násobitel zadejte do textového pole. Například pokud chcete, aby aktivační událost spustit jednou pro každých 15 minut, vyberete **každých minutu**a zadejte **15** v textovém poli. 
    4. Pro **End** pole, pokud nechcete zadat koncovou hodnotou datetime pro aktivační událost, vyberte **ne End**. Chcete-li zadat element end datum a čas, vyberte **na datum**a zadejte koncové datum a čas a klikněte na tlačítko **použít**. Každé spuštění kanálu je zpoplatněno. Pokud testujete, můžete zajistit, že se aktivuje kanálu jenom pár časy. Zajistěte však, aby měl kanál mezi časem publikování a koncovým časem dostatek času na spuštění. Aktivační událost nabývá účinnosti po publikování řešení do služby Data Factory, a ne při uložení aktivační události v uživatelském rozhraní.

        ![Nastavení aktivační události](./media/how-to-create-schedule-trigger/trigger-settings.png)
4. V **novou aktivační událost** okno, zkontrolujte **aktivované** a klikněte na **Další**. Toto políčko můžete deaktivovat aktivační událost později. 

    ![Nastavení aktivační události – tlačítko Další](./media/how-to-create-schedule-trigger/trigger-settings-next.png)
5. Na stránce **Nová aktivační událost** si přečtěte zprávu upozornění a klikněte na **Dokončit**.

    ![Nastavení aktivační události – tlačítko Dokončit](./media/how-to-create-schedule-trigger/new-trigger-finish.png)
6. Kliknutím na **Publikovat** publikujte změny do služby Data Factory. Až budete publikovat změny objektu pro vytváření dat, nespustí aktivační událost aktivuje spuštění kanálu. 

    ![Tlačítko Publikovat](./media/how-to-create-schedule-trigger/publish-2.png)
8. Vlevo přepněte na kartu **Monitorování**. Kliknutím na **Aktualizovat** seznam aktualizujte. Uvidíte, že se kanál spustí spouštěná naplánované aktivační procedura. Všimněte si hodnot ve sloupci **Aktivoval(a)**. Pokud používáte **aktivační událost teď** možnost, zobrazí ruční aktivační událost spouštět v seznamu. 

    ![Monitorování aktivovaných spuštění](./media/how-to-create-schedule-trigger/monitor-triggered-runs.png)
9. Kliknutím na šipku dolů vedle **Spuštění kanálu** přepněte na zobrazení **Spuštění aktivační události**. 

    ![Monitorování spuštění aktivační události](./media/how-to-create-schedule-trigger/monitor-trigger-runs.png)

## <a name="azure-powershell"></a>Azure PowerShell
V této části ukazuje, jak pomocí prostředí Azure PowerShell k vytvoření, spuštění a monitorování plánu aktivační událost. Tato ukázka pracovní najdete nejdřív projít [rychlý start: Vytvořte objekt pro vytváření dat pomocí Azure PowerShell](quickstart-create-data-factory-powershell.md). Hlavní metodu, která se vytvoří a spustí plán aktivační událost, která se spouští každých 15 minut, přidejte následující kód. Aktivační událost je přidružena kanál s názvem **Adfv2QuickStartPipeline** , které vytvoříte jako součást rychlý start.

1. Vytvořte soubor JSON s názvem **MyTrigger.json** ve složce C:\ADFv2QuickStartPSH\ s následujícím obsahem:

    > [!IMPORTANT]
    > Před uložením souboru JSON, nastavte hodnotu **startTime** element na aktuální čas UTC. Nastavte hodnotu **endTime** element na jednu hodinu po aktuálním čase UTC.

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
    - **Typ** element aktivační události je nastaven na hodnotu "ScheduleTrigger."
    - **Frekvence** element je nastaven na hodnotu "Minutu" a **interval** element je nastaven na 15. Proto aktivační událost spustí kanálu každých 15 minut mezi počáteční a koncový čas.
    - **EndTime** element je jedna hodina po hodnotu **startTime** elementu. Proto aktivační událost spustí kanálu 15 minut, 30 minut a 45 minut po času zahájení. Nezapomeňte aktualizovat počáteční čas aktuální čas UTC a koncový čas na jednu hodinu po čase zahájení. 
    - Aktivační událost je přidružena **Adfv2QuickStartPipeline** kanálu. Chcete-li přidružit více kanálů aktivační událost, přidejte další **pipelineReference** oddíly.
    - Kanál v rychlé spuštění trvá dva **parametry** hodnoty: **inputPath** a **outputPath**. Proto předat hodnoty pro tyto parametry z aktivační událost.

2. Vytvořit aktivační událost pomocí **Set-AzureRmDataFactoryV2Trigger** rutiny:

    ```powershell
    Set-AzureRmDataFactoryV2Trigger -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name "MyTrigger" -DefinitionFile "C:\ADFv2QuickStartPSH\MyTrigger.json"
    ```

3. Zkontrolujte, zda je stav aktivační události **Zastaveno** pomocí **Get-AzureRmDataFactoryV2Trigger** rutiny:

    ```powershell
    Get-AzureRmDataFactoryV2Trigger -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name "MyTrigger"
    ```

4. Spusťte aktivační událost pomocí **Start-AzureRmDataFactoryV2Trigger** rutiny:

    ```powershell
    Start-AzureRmDataFactoryV2Trigger -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name "MyTrigger"
    ```

5. Zkontrolujte, zda je stav aktivační události **Začínáme** pomocí **Get-AzureRmDataFactoryV2Trigger** rutiny:

    ```powershell
    Get-AzureRmDataFactoryV2Trigger -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name "MyTrigger"
    ```

6.  Aktivační událost se spustí v prostředí Azure PowerShell Get **Get-AzureRmDataFactoryV2TriggerRun** rutiny. Chcete-li získat informace o spuštění aktivační události, spusťte následující příkaz pravidelně. Aktualizace **TriggerRunStartedAfter** a **TriggerRunStartedBefore** hodnoty k hodnotám ve vaší definice aktivační události:

    ```powershell
    Get-AzureRmDataFactoryV2TriggerRun -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -TriggerName "MyTrigger" -TriggerRunStartedAfter "2017-12-08T00:00:00" -TriggerRunStartedBefore "2017-12-08T01:00:00"
    ```
    
    Ke sledování aktivační událost spustí a kanál se spustí na portálu Azure, najdete v tématu [spouští monitorování kanálu](quickstart-create-data-factory-resource-manager-template.md#monitor-the-pipeline).


## <a name="net-sdk"></a>.NET SDK
V této části ukazuje, jak používat sadu .NET SDK k vytvoření, spuštění a monitorování aktivační událost. Tato ukázka pracovní najdete nejdřív projít [rychlý start: Vytvořte objekt pro vytváření dat pomocí .NET SDK](quickstart-create-data-factory-dot-net.md). Hlavní metodu, která se vytvoří a spustí plán aktivační událost, která se spouští každých 15 minut, přidejte následující kód. Aktivační událost je přidružena kanál s názvem **Adfv2QuickStartPipeline** , které vytvoříte jako součást rychlý start.

Pro vytvoření a spuštění plánu aktivační událost, která se spouští každých 15 minut, přidejte následující kód do hlavní metodu:

```csharp
            // Create the trigger
            Console.WriteLine("Creating the trigger");

            // Set the start time to the current UTC time
            DateTime startTime = DateTime.UtcNow;

            // Specify values for the inputPath and outputPath parameters
            Dictionary<string, object> pipelineParameters = new Dictionary<string, object>();
            pipelineParameters.Add("inputPath", "adftutorial/input");
            pipelineParameters.Add("outputPath", "adftutorial/output");

            // Create a schedule trigger
            string triggerName = "MyTrigger";
            ScheduleTrigger myTrigger = new ScheduleTrigger()
            {
                Pipelines = new List<TriggerPipelineReference>()
                {
                    // Associate the Adfv2QuickStartPipeline pipeline with the trigger
                    new TriggerPipelineReference()
                    {
                        PipelineReference = new PipelineReference(pipelineName),
                        Parameters = pipelineParameters,
                    }
                },
                Recurrence = new ScheduleTriggerRecurrence()
                {
                    // Set the start time to the current UTC time and the end time to one hour after the start time
                    StartTime = startTime,
                    TimeZone = "UTC",
                    EndTime = startTime.AddHours(1),
                    Frequency = RecurrenceFrequency.Minute,
                    Interval = 15,
                }
            };

            // Now, create the trigger by invoking the CreateOrUpdate method
            TriggerResource triggerResource = new TriggerResource()
            {
                Properties = myTrigger
            };
            client.Triggers.CreateOrUpdate(resourceGroup, dataFactoryName, triggerName, triggerResource);

            // Start the trigger
            Console.WriteLine("Starting the trigger");
            client.Triggers.Start(resourceGroup, dataFactoryName, triggerName);
```

Ke sledování aktivační události spuštění, přidejte následující kód před poslední `Console.WriteLine` příkaz v ukázce:

```csharp
            // Check that the trigger runs every 15 minutes
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

Ke sledování aktivační událost spustí a kanál se spustí na portálu Azure, najdete v tématu [spouští monitorování kanálu](quickstart-create-data-factory-resource-manager-template.md#monitor-the-pipeline).


## <a name="python-sdk"></a>Python SDK
V této části se dozvíte, jak používat sadu SDK Python k vytvoření, spuštění a monitorování aktivační událost. Tato ukázka pracovní najdete nejdřív projít [rychlý start: Vytvořte objekt pro vytváření dat pomocí sady SDK pro Python](quickstart-create-data-factory-python.md). Pak přidejte následující blok kódu po "spustit kanál monitorovat" blok kódu ve skriptu jazyka Python. Tento kód vytvoří plán aktivační událost, která spouští každých 15 minut mezi zadaným počátečním a koncovým časem. Aktualizace **start_time** proměnné na aktuální čas UTC a **end_time** proměnnou na jednu hodinu po aktuálním čase UTC.

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

    # Start the trigger
    adf_client.triggers.start(rg_name, df_name, tr_name)
```

Ke sledování aktivační událost spustí a kanál se spustí na portálu Azure, najdete v tématu [spouští monitorování kanálu](quickstart-create-data-factory-resource-manager-template.md#monitor-the-pipeline).

## <a name="azure-resource-manager-template"></a>Šablona Azure Resource Manageru
Šablonu Azure Resource Manager můžete vytvořit aktivační událost. Podrobné pokyny najdete v tématu [vytvořit objekt pro vytváření dat Azure pomocí šablony Resource Manageru](quickstart-create-data-factory-resource-manager-template.md).  

## <a name="pass-the-trigger-start-time-to-a-pipeline"></a>Čas spuštění aktivační události předat kanálu
Azure Data Factory verze 1 podporuje čtení nebo zápisu dat. oddílů pomocí systému proměnných: **SliceStart**, **SliceEnd**, **WindowStart**a **WindowEnd**. V Azure Data Factory verze 2 můžete toto chování dosáhnout pomocí parametru kanálu. Čas spuštění a naplánovaném čase pro aktivační událost jsou nastavená jako hodnota pro parametr kanálu. V následujícím příkladu je naplánovaném čase pro aktivační událost jako hodnotu předaný kanálu **scheduledRunTime** parametr:

```json
"parameters": {
    "scheduledRunTime": "@trigger().scheduledTime"
}
```    

Další informace naleznete v pokynech v [jak pro čtení nebo zápis dat rozdělena na oddíly](how-to-read-write-partitioned-data.md).

## <a name="json-schema"></a>Schématu JSON
Na následující definici JSON ukazuje, jak vytvořit aktivační událost plán s plánování a opakování:

```json
{
  "properties": {
    "type": "ScheduleTrigger",
    "typeProperties": {
      "recurrence": {
        "frequency": <<Minute, Hour, Day, Week, Month>>,
        "interval": <<int>>,             // Optional, specifies how often to fire (default to 1)
        "startTime": <<datetime>>,
        "endTime": <<datetime - optional>>,
        "timeZone": "UTC"
        "schedule": {                    // Optional (advanced scheduling specifics)
          "hours": [<<0-23>>],
          "weekDays": : [<<Monday-Sunday>>],
          "minutes": [<<0-59>>],
          "monthDays": [<<1-31>>],
          "monthlyOccurences": [
               {
                    "day": <<Monday-Sunday>>,
                    "occurrence": <<1-5>>
               }
           ]
        }
      }
    },
   "pipelines": [
            {
                "pipelineReference": {
                    "type": "PipelineReference",
                    "referenceName": "<Name of your pipeline>"
                },
                "parameters": {
                    "<parameter 1 Name>": {
                        "type": "Expression",
                        "value": "<parameter 1 Value>"
                    },
                    "<parameter 2 Name>" : "<parameter 2 Value>"
                }
           }
      ]
  }
}
```

> [!IMPORTANT]
>  **Parametry** vlastnost je povinná vlastnost **kanály** elementu. Pokud vaše kanálu neberou žádné parametry, je nutné zahrnout prázdný definici JSON pro **parametry** vlastnost.


### <a name="schema-overview"></a>Schéma – přehled
Následující tabulka obsahuje přehled hlavních schématu prvků, které se vztahují k opakování a plánování aktivační události:

| Vlastnost JSON | Popis |
|:--- |:--- |
| **startTime** | Hodnota, datum a čas. Pro jednoduché plány, hodnota **startTime** vlastnost se vztahuje na první výskyt. Pro komplexní plány, spustí aktivační událost dřív než zadaný **startTime** hodnotu. |
| **endTime** | Koncové datum a čas pro aktivační událost. Aktivační událost není spustit po uplynutí zadané koncové datum a čas. Hodnota pro vlastnost nemůže být v minulosti. Tato vlastnost je nepovinná. |
| **timeZone** | Časové pásmo. V současné době je podporována pouze v časovém pásmu UTC. |
| **opakování** | Objekt opakování, který určuje opakování pravidla pro aktivační událost. Objekt opakování podporuje **frekvence**, **interva**l, **endTime**, **počet**, a **plán**elementy. Pokud je definován objekt opakování, **frekvence** prvek je nutný. Další prvky objekt opakování jsou volitelné. |
| **frekvence** | Jednotka frekvence, při které aktivační událost objeví znovu. Podporované hodnoty zahrnují "patnáctiminutových", "hodina," "dne", "týden" a "měsíc". |
| **interval** | Kladné celé číslo, které označuje interval **frekvence** hodnotu, která určuje, jak často se spustí aktivační událost. Například pokud **interval** 3 a **frekvence** je "týden," aktivační událost opakuje každé 3 týdny. |
| **schedule** | Plán opakování pro aktivační událost. Aktivační událost se zadaným **frekvence** hodnotu mění jeho opakování podle plánu opakování. **Plán** vlastnost obsahuje úpravy opakování, které jsou založeny na minuty, hodiny, dny v týdnu, dny v měsíci a číslo týdne.


### <a name="schema-defaults-limits-and-examples"></a>Výchozí schéma, omezení a příklady

| Vlastnost JSON | Typ | Požaduje se | Výchozí hodnota | Platné hodnoty | Příklad: |
|:--- |:--- |:--- |:--- |:--- |:--- |
| **startTime** | Řetězec | Ano | Žádné | Data a časy podle normy ISO 8601 | `"startTime" : "2013-01-09T09:30:00-08:00"` |
| **opakování** | Objekt | Ano | Žádný | Objekt opakování | `"recurrence" : { "frequency" : "monthly", "interval" : 1 }` |
| **interval** | Číslo | Ne | 1 | 1 – 1000 | `"interval":10` |
| **endTime** | Řetězec | Ano | Žádné | Hodnota data a času, který představuje čas v budoucnosti. | `"endTime" : "2013-02-09T09:30:00-08:00"` |
| **schedule** | Objekt | Ne | Žádný | Objekt plánu | `"schedule" : { "minute" : [30], "hour" : [8,17] }` |

### <a name="starttime-property"></a>Vlastnost startTime
V následující tabulce se dozvíte, jak **startTime** vlastnost řídí aktivační událost spustit:

| Hodnota startTime | Opakování bez plánu | Opakování s plánem |
|:--- |:--- |:--- |
| Čas začátku v minulosti | Vypočítá budoucí prvním spuštění po čase zahájení a spouští v daném čase.<br/><br/>Spustí dalších spuštěních podle výpočet při posledním spuštění.<br/><br/>Prohlédněte si příklad pod touto tabulkou. | Spustí aktivační událost _ne sooner než_ zadaným časem spuštění. První výskyt je podle plánu, která je vypočtená z čas spuštění.<br/><br/>Spustí dalších spuštěních podle plánu opakování. |
| Čas začátku v budoucnosti nebo přítomnosti | Spustí jednou v určený čas spuštění.<br/><br/>Spustí dalších spuštěních podle výpočet při posledním spuštění. | Spustí aktivační událost _již dříve_ než zadaný počáteční čas. První výskyt je podle plánu, která je vypočtená z čas spuštění.<br/><br/>Spustí dalších spuštěních podle plánu opakování. |

Podíváme se na příklad co se stane, když čas spuštění je v minulosti s opakování, ale žádný plán. Předpokládejme, že je aktuální čas `2017-04-08 13:00`, čas spuštění je `2017-04-07 14:00`, a opakování je dvou dní. ( **Opakování** hodnota je definována nastavením **frekvence** vlastnost "den" a **interval** vlastnost na hodnotu 2.) Všimněte si, že **startTime** hodnota je v minulosti a dojde k před aktuálním časem.

Za těchto podmínek dojde k prvnímu spuštění v `2017-04-09 at 14:00`. Modul plánovače vypočítá výskyty spuštění na základě času začátku. Všechny instance v minulosti se zahodí. Modul použije další instanci, která nastane v budoucnosti. V tomto scénáři je čas spuštění `2017-04-07 at 2:00pm`, takže další instance je o dva dny z této doby, což je `2017-04-09 at 2:00pm`.

Při prvním spuštění je stejné i když **startTime** hodnota je `2017-04-05 14:00` nebo `2017-04-01 14:00`. Po prvním spuštění dalších spuštěních se počítají pomocí plán. Proto jsou dalších spuštěních `2017-04-11 at 2:00pm`, pak `2017-04-13 at 2:00pm`, pak `2017-04-15 at 2:00pm`a tak dále.

Nakonec v plánu pro aktivační událost není nastavena hodinách nebo minutách, hodinách nebo minutách první spuštění se používají jako výchozí hodnoty.

### <a name="schedule-property"></a>Vlastnost plán
Na jedné straně použití plánu můžete omezit počet spuštění aktivační události. Například pokud aktivační událost s měsíční četností je naplánována na spuštění pouze v den 31, aktivační událost se spustí pouze v těchto měsících, které mají 31 dní.

Na druhou stranu může plán způsobit také zvýšení počtu spuštění aktivační události. Například aktivační událost s měsíční frekvencí, která je naplánována na spuštění v dny v měsíci 1 a 2, se spouští na 1 a 2 dny v měsíci, než jednou za měsíc.

Pokud je to více **plán** elementy jsou zadán, pořadí vyhodnocování od největších nejmenší nastavení plánu. Hodnocení začíná týden číslo a potom měsíc a den, den v týdnu, hodiny a nakonec minutu.

V následující tabulce jsou popsány **plán** elementy podrobně:


| JSON – element | Popis | Platné hodnoty |
|:--- |:--- |:--- |
| **minut** | Minuty v hodině, ve kterých se aktivační událost spouští. | <ul><li>Integer</li><li>Pole celých čísel</li></ul>
| **hodiny** | Hodiny dne, ve kterých se aktivační událost spouští. | <ul><li>Integer</li><li>Pole celých čísel</li></ul> |
| **weekDays** | Dny v týdnu, ve kterém se spustí aktivační událost. S frekvencí týdenní pouze lze zadat hodnotu. | <ul><li>Pondělí, úterý, středu, čtvrtek, pátek, sobota, neděle</li><li>Pole hodnot den (7 je velikost maximální pole)</li><li>Den hodnoty nejsou malá a velká písmena</li></ul> |
| **monthlyOccurrences** | Dny v měsíci, ve kterém se spustí aktivační událost. Hodnota lze s měsíční četností. | <ul><li>Pole **monthlyOccurence** objekty: `{ "day": day,  "occurrence": occurence }`.</li><li>**Den** atribut je den v týdnu, ve kterém se spustí aktivační událost. Například **monthlyOccurrences** vlastnost s **den** hodnotu `{Sunday}` znamená každou neděli v měsíci. **Den** atribut je požadován.</li><li>**Výskyt** atribut je výskyt zadaného **den** v měsíci. Například **monthlyOccurrences** vlastnost s **den** a **výskyt** hodnoty `{Sunday, -1}` znamená neděli poslední den v měsíci. **Výskyt** atribut je volitelný.</li></ul> |
| **Prescribed** | Den v měsíci, ve kterém se spustí aktivační událost. Hodnota lze s měsíční četností. | <ul><li>Libovolná hodnota < = -1 a > =-31</li><li>Libovolná hodnota >= 1 a <= 31</li><li>Pole hodnot</li></ul> |


## <a name="examples-of-trigger-recurrence-schedules"></a>Příklady plány opakování aktivační události
Tato část obsahuje příklady opakování plány a se zaměřuje na **plán** objekt a jejích elementů.

V příkladech předpokládáme, že **interval** hodnota je 1 a že **frekvence** hodnota je správný podle definice plánu. Například nemůže mít **frekvence** hodnotu "dne" a "Prescribed" změny mají také **plán** objektu. Omezení takovéto jsou uvedený v tabulce v předchozí části.

| Příklad: | Popis |
|:--- |:--- |
| `{"hours":[5]}` | Spusťte na 5:00 každý den. |
| `{"minutes":[15], "hours":[5]}` | Spusťte na 5:15:00 každý den. |
| `{"minutes":[15], "hours":[5,17]}` | Spusťte na 5:15:00 a 17:15:00 každý den. |
| `{"minutes":[15,45], "hours":[5,17]}` | Spustit na 5:15:00, 5:45 AM, 17:15:00 a 17:45:00 každý den. |
| `{"minutes":[0,15,30,45]}` | Spusťte každých 15 minut. |
| `{hours":[0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23]}` | Spouštět každou hodinu. Tato aktivační událost se spouští každou hodinu. Dobu, po které jsou řízeny **startTime** hodnotu, pokud je zadaná hodnota. Pokud hodnotu nezadáte, dobu, po které jsou řízené čas vytvoření. Například pokud je čas spuštění nebo čas vytvoření (podle toho, co platí) 12:25 hodin, aktivační události, které se spouští v 00:25, 01:25 02:25,... a 23:25.<br/><br/>Tento plán je ekvivalentní s aktivační událost s **frekvence** hodnotu "hodina," **interval** hodnotu 1 a ne **plán**.  Tento plán lze použít s jinou **frekvence** a **interval** hodnotami pro vytvoření další aktivační události. Například, když **frekvence** hodnota je "měsíc", je plán spuštěn pouze jednou za měsíc, nikoli každý den, kdy **frekvence** hodnota je "den". |
| `{"minutes":[0]}` | Spouštět každou hodinu hodině. Této aktivační události spouští každou hodinu na hodinu od 12:00:00, 1:00 AM, 2:00 AM, a tak dále.<br/><br/>Tento plán je ekvivalentní aktivační událost s **frekvence** hodnotu "hodina" a **startTime** hodnotu nula minut, nebo Ne **plán** ale **frekvence**  hodnotu "dne." Pokud **frekvence** hodnota je "týden" nebo "měsíc," plán provede jeden den, týden nebo jednoho dne a měsíce pouze, v uvedeném pořadí. |
| `{"minutes":[15]}` | Spusťte 15 minut po každou hodinu. Této aktivační události spouští každou hodinu v 15 minut po hodině, počínaje od 00:15 AM, 1:15:00, 2:15:00 a tak dále a koncovou ve 23:15:00. |
| `{"hours":[17], "weekDays":["saturday"]}` | Spusťte v 17:00:00 v sobotu každý týden. |
| `{"hours":[17], "weekDays":["monday", "wednesday", "friday"]}` | Spusťte v 17:00:00 v pondělí, středu a pátek každý týden. |
| `{"minutes":[15,45], "hours":[17], "weekDays":["monday", "wednesday", "friday"]}` | Spusťte v 17:15:00 a 17:45:00 v pondělí, středu a pátek každý týden. |
| `{"minutes":[0,15,30,45], "weekDays":["monday", "tuesday", "wednesday", "thursday", "friday"]}` | Spusťte každých 15 minut na dny v týdnu. |
| `{"minutes":[0,15,30,45], "hours": [9, 10, 11, 12, 13, 14, 15, 16] "weekDays":["monday", "tuesday", "wednesday", "thursday", "friday"]}` | Spusťte každých 15 minut ve všední dny mezi 9:00 do 4:45. |
| `{"weekDays":["tuesday", "thursday"]}` | Spusťte na každé úterý a čtvrtek v určený čas spuštění. |
| `{"minutes":[0], "hours":[6], "monthDays":[28]}` | Spustit v 6:00:00 28th den každý měsíc (za předpokladu, že **frekvence** hodnotu "měsíc"). |
| `{"minutes":[0], "hours":[6], "monthDays":[-1]}` | Spusťte v 6:00:00 poslední den v měsíci. Aktivační událost spustit poslední den v měsíci, použijte místo den 28, 29, 30 a 31 -1. |
| `{"minutes":[0], "hours":[6], "monthDays":[1,-1]}` | Spusťte v 6:00:00 na první a poslední den v měsíci. |
| `{monthDays":[1,14]}` | Spusťte na první a 14. den v každém měsíci v určený čas spuštění. |
| `{"minutes":[0], "hours":[5], "monthlyOccurrences":[{"day":"friday", "occurrence":1}]}` | Spusťte na prvním pátek v každém měsíci na 5:00. |
| `{"monthlyOccurrences":[{"day":"friday", "occurrence":1}]}` | Spusťte na prvním pátek v každém měsíci v určený čas spuštění. |
| `{"monthlyOccurrences":[{"day":"friday", "occurrence":-3}]}` | Spusťte na třetí pátek od konce měsíce, v každém měsíci, v určený čas spuštění. |
| `{"minutes":[15], "hours":[5], "monthlyOccurrences":[{"day":"friday", "occurrence":1},{"day":"friday", "occurrence":-1}]}` | Spusťte na první a poslední pátek v každém měsíci v 5:15:00. |
| `{"monthlyOccurrences":[{"day":"friday", "occurrence":1},{"day":"friday", "occurrence":-1}]}` | Spusťte na první a poslední pátek v každém měsíci v určený čas spuštění. |
| `{"monthlyOccurrences":[{"day":"friday", "occurrence":5}]}` | Spusťte v páté pátek v každém měsíci v určený čas spuštění. Když žádné páté pátek v měsíci, kanál neběží, vzhledem k tomu, že je naplánována na spuštění pouze v páté pátek. Ke spuštění aktivační události na posledního výskytu pátek v měsíci, zvažte použití -1 namísto 5 pro **výskyt** hodnotu. |
| `{"minutes":[0,15,30,45], "monthlyOccurrences":[{"day":"friday", "occurrence":-1}]}` | Spusťte každých 15 minut na poslední pátek v měsíci. |
| `{"minutes":[15,45], "hours":[5,17], "monthlyOccurrences":[{"day":"wednesday", "occurrence":3}]}` | Spustit na 5:15:00, 5:45 AM, 17:15:00 a 17:45:00 ve třetí středu každého měsíce. |


## <a name="next-steps"></a>Další postup
Podrobné informace o aktivační události najdete v tématu [kanálu spouštěcí a aktivační události](concepts-pipeline-execution-triggers.md#triggers).
