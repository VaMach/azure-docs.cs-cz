---
title: "Postup vytvoření plánu aktivační události v Azure Data Factory | Microsoft Docs"
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
ms.openlocfilehash: 6b92e8402d372e29e264dc70b128124973d66bb9
ms.sourcegitcommit: 1d423a8954731b0f318240f2fa0262934ff04bd9
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/05/2018
---
# <a name="how-to-create-a-trigger-that-runs-a-pipeline-on-a-schedule"></a>Jak vytvořit aktivační událost, která spouští kanál podle plánu
Tento článek obsahuje informace o aktivační události plán a kroky k vytvoření, spuštění a monitorování aktivační událost. U jiných typů aktivačních událostí, najdete v části [kanálu spouštěcí a aktivační události](concepts-pipeline-execution-triggers.md).

> [!NOTE]
> Tento článek se týká verze 2 služby Data Factory, která je aktuálně ve verzi Preview. Pokud používáte verzi 1 služby Data Factory, který je všeobecně dostupná (GA), prostudujte si [úvod do služby Data Factory verze 1](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).

### <a name="schedule-trigger-json-definition"></a>Plán aktivace definici JSON
Když vytvoříte plán aktivační událost, můžete zadat plánování a opakování pomocí JSON, jak je znázorněno v příkladu v této části.

Pokud chcete, aby váš plán aktivační událost ji spustit kanál, obsahovat odkaz kanálu konkrétní kanálu v definici aktivační události. Mezi kanály a aktivačními událostmi existuje vztah n-m. Víc aktivačních událostí může aktivovat jeden kanál. Jedna aktivační událost může aktivovat více kanálů.

```json
{
  "properties": {
    "type": "ScheduleTrigger",
    "typeProperties": {
      "recurrence": {
        "frequency": <<Minute, Hour, Day, Week, Month>>,
        "interval": <<int>>,             // optional, how often to fire (default to 1)
        "startTime": <<datetime>>,
        "endTime": <<datetime - optional>>,
        "timeZone": "UTC"
        "schedule": {                    // optional (advanced scheduling specifics)
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
>  Vlastnost **parameters** je uvnitř vlastnosti **pipelines** povinná. I v případě, že váš kanál nepřijímá žádné parametry, zahrňte pro vlastnost parameters prázdný objekt JSON, protože tato vlastnost musí existovat.


### <a name="overview-schedule-trigger-schema"></a>Přehled: Plán aktivační událost schématu
Následující tabulka obsahuje přehled hlavních elementů souvisejících s opakováním a plánováním aktivační události:

Vlastnost JSON |     Popis
------------- | -------------
startTime | Položka startTime je ve formátu datum-čas. U jednoduchých plánů představuje položka startTime první výskyt. U složitějších plánů aktivační událost nezačíná dřív než čas určený hodnotou startTime.
endTime | Určuje datum a čas konce aktivační události. Po uplynutí tohoto času se už aktivační událost neprovede. Hodnota endTime v minulosti není platná. Tato vlastnost je volitelná.
timeZone | Aktuálně se podporuje pouze UTC.
recurrence | Objekt recurrence určuje pravidla opakování aktivační události. Objekt recurrence podporuje tyto elementy: frekvence, interval, čas konce, počet a plán. Pokud se definuje opakování, je frekvence povinná. Ostatní elementy opakování jsou volitelné.
frequency | Představuje jednotku frekvence, s jakou se aktivační událost opakuje. Podporované hodnoty: `minute`, `hour`, `day`, `week` nebo `month`.
interval | Interval je kladné celé číslo. Označuje interval pro frekvenci, který určuje, jak často se má aktivační událost spouštět. Pokud má například interval hodnotu 3 a frekvence hodnotu „týden“, aktivační událost se opakuje každé 3 týdny.
schedule | Aktivační události se zadanou frekvencí mění své opakování na základě plánu opakování. Plán obsahuje úpravy na základě minut, hodin, dní v týdnu, dní v měsíci a čísla týdne.


### <a name="overview-schedule-trigger-schema-defaults-limits-and-examples"></a>Přehled: Plánování výchozí schéma aktivační událost, omezení a příklady

Název JSON | Typ hodnoty | Povinné? | Výchozí hodnota | Platné hodnoty | Příklad:
--------- | ---------- | --------- | ------------- | ------------ | -------
startTime | Řetězec | Ano | Žádné | Data a časy podle normy ISO 8601 | ```"startTime" : "2013-01-09T09:30:00-08:00"```
recurrence | Objekt | Ano | Žádné | Objekt opakování | ```"recurrence" : { "frequency" : "monthly", "interval" : 1 }```
interval | Číslo | Ne | 1 | 1 až 1000 | ```"interval":10```
endTime | Řetězec | Ano | Žádný | Hodnota data a času představující čas v budoucnosti | `"endTime" : "2013-02-09T09:30:00-08:00"`
schedule | Objekt | Ne | Žádné | Objekt plánu | `"schedule" : { "minute" : [30], "hour" : [8,17] }`

### <a name="deep-dive-starttime"></a>Podrobné informace: položka startTime
Následující tabulka udává, jak položka startTime řídí spouštění aktivační události:

Hodnota startTime | Opakování bez plánu | Opakování s plánem
--------------- | --------------------------- | ------------------------
Čas začátku v minulosti | Vypočítá první čas spuštění v budoucnosti následující po čase začátku a v tomto čase se spustí.<p>Zahájí další spuštění na základě výpočtu z posledního času spuštění.</p><p>Prohlédněte si příklad pod touto tabulkou.</p> | Aktivační událost se spustí _až po_ zadaném čase začátku. První výskyt vychází z plánu vypočítaného z času začátku. <p>Zahájí další spuštění na základě plánu opakování.</p>
Čas začátku v budoucnosti nebo přítomnosti | Spustí se jednou v zadaný čas začátku. <p>Zahájí další spuštění na základě výpočtu z posledního času spuštění.</p> | Aktivační událost se spustí _až po_ zadaném čase začátku. První výskyt vychází z plánu vypočítaného z času začátku.<p>Zahájí další spuštění na základě plánu opakování.</p>

Podívejme se na příklad toho, co se stane, když je položka startTime v minulosti a je nastaveno opakování, ale žádný plán. Předpokládejme, že aktuální čas je `2017-04-08 13:00`, položka startTime má hodnotu `2017-04-07 14:00` a je nastaveno opakování každé dva dny (definuje se prostřednictvím frekvence „den“ a intervalu 2). Všimněte si, že položka startTime je v minulosti a předchází aktuálnímu času.

Za těchto podmínek dojde k prvnímu spuštění v `2017-04-09 at 14:00`. Modul plánovače vypočítá výskyty spuštění na základě času začátku. Všechny instance v minulosti se zahodí. Modul použije další instanci, která nastane v budoucnosti. V tomto případě má položka startTime hodnotu `2017-04-07 at 2:00pm`, takže další instance nastane o dva dny později, tedy v čase `2017-04-09 at 2:00pm`.

První čas spuštění je stejný, i když má položka startTime hodnotu `2017-04-05 14:00` nebo `2017-04-01 14:00`. Po prvním spuštění se další spuštění vypočítají na základě plánu. To znamená, že k nim dojde v čase `2017-04-11 at 2:00pm`, potom `2017-04-13 at 2:00pm`, potom `2017-04-15 at 2:00pm` atd.

Nakonec platí, že pokud má aktivační událost plán, ve kterém nejsou nastavené hodiny nebo minuty, použije se výchozí hodnota hodin nebo minut prvního spuštění.

### <a name="deep-dive-schedule"></a>Podrobné informace: plán
Na jedné straně může plán omezit počet spuštění aktivační události. Pokud má například aktivační událost s frekvencí „měsíc“ plán, který se spouští jenom v 31. den, tato aktivační událost se spustí jenom v měsících, které mají 31 dní.

Na druhou stranu může plán způsobit také zvýšení počtu spuštění aktivační události. Například pokud má aktivační událost s frekvencí „měsíc“ plán, který se spouští v 1. a 2. den v měsíci, tato aktivační událost se spustí v 1. i 2. den v měsíci, ne jenom jednou měsíčně.

Pokud plán obsahuje víc zadaných elementů, jejich vyhodnocování postupuje od největšího k nejmenšímu – od čísla týdne přes den v měsíci, den v týdnu a hodinu až k minutě.

Následující tabulka obsahuje podrobný popis elementů plánu:


Název JSON | Popis | Platné hodnoty
--------- | ----------- | ------------
minutes | Minuty v hodině, ve kterých se aktivační událost spouští. | <ul><li>Integer</li><li>Pole celých čísel</li></ul>
hours | Hodiny dne, ve kterých se aktivační událost spouští. | <ul><li>Integer</li><li>Pole celých čísel</li></ul>
weekDays | Dny v týdnu, ve kterých se aktivační událost spouští. Tuto položku je možné zadat jenom při týdenní frekvenci. | <ul><li>Pondělí, úterý, středa, čtvrtek, pátek, sobota nebo neděle</li><li>Pole jakýchkoli z uvedených hodnot (maximální velikost pole je 7)</li></p>Nerozlišují se malá a velká písmena.</p>
monthlyOccurrences | Určuje, které dny v měsíci se má aktivační událost spouštět. Tuto položku je možné zadat jenom při měsíční frekvenci. | Pole objektů monthlyOccurence: `{ "day": day,  "occurrence": occurence }`. <p> Den je den v týdnu, kdy se spouští aktivační událost, například hodnota `{Sunday}` určuje každou neděli v měsíci. Povinná hodnota.<p>Výskyt je výskyt dne v měsíci, například `{Sunday, -1}` je poslední neděle v měsíci. Volitelné.
monthDays | Den v měsíci, kdy se aktivační událost spouští. Tuto položku je možné zadat jenom při měsíční frekvenci. | <ul><li>Libovolná hodnota < = -1 a > =-31</li><li>Libovolná hodnota >= 1 a <= 31</li><li>Pole hodnot</li>


## <a name="examples-recurrence-schedules"></a>Příklady: plány opakování
Tato část obsahuje příklady plánů opakování se zaměřením na objekt plánu a jeho dílčí elementy.

Příklady plánů vycházejí z předpokladu, že je interval nastavený na hodnotu 1. Také předpokládejme správnou frekvenci odpovídající obsahu plánu – například nemůžete použít frekvenci „den“, a přitom mít v plánu úpravu elementu „monthDays“. Tato omezení se nachází v tabulce v předchozí části.

Příklad: | Popis
------- | -----------
`{"hours":[5]}` | Spustí se každý den v 5:00.
`{"minutes":[15], "hours":[5]}` | Spustí se každý den v 5:15.
`{"minutes":[15], "hours":[5,17]}` | Spustí na každý den v 5:15 a 17:15.
`{"minutes":[15,45], "hours":[5,17]}` | Spustí na každý den v 5:15, 5:45, 17:15 a 17:45.
`{"minutes":[0,15,30,45]}` | Spustí se každých 15 minut.
`{hours":[0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23]}` | Spustí se každou hodinu. Tato aktivační událost se spouští každou hodinu. Minutu řídí hodnota startTime, pokud je zadaná, a pokud ne, řídí ji čas vytvoření. Například pokud je čas začátku nebo čas vytvoření (podle situace) 00:25, aktivační událost se spustí v 00:25, 01:25, 02:25 atd. až do 23:25. Tento plán je stejný jako aktivační událost s frekvencí „hodina“, intervalem 1 a žádným plánem. Rozdíl je v tom, že tento plán se dá s jinou frekvencí a intervalem použít také k vytvoření jiných aktivačních událostí. Například pokud by byla frekvence „měsíc“, plán by se spouštěl jenom jednou za měsíc, a ne každý den, jako kdyby byla nastavená frekvence „den“.
`{"minutes":[0]}` | Spouští se v každou celou hodinu. Tato aktivační událost se také spouští každou hodinu, ale vždycky přesně v celou hodinu (například v 00:00, 1:00, 2:00 atd.). Toto nastavení je stejné jako aktivační událost s frekvencí „hodina“, položkou startTime s hodnotou nula minut a žádným plánem, pokud je nastavená frekvence „den“, ale při frekvenci „týden“ nebo „měsíc“ by se plán spouštěl jenom jeden den v týdnu, případně jeden den v měsíci.
`{"minutes":[15]}` | Spustí se 15 minut po každé celé hodině. Spouští se každou hodinu od 00:15, tedy v 1:15, 2:15 atd., a končí ve 22:15 a 23:15.
`{"hours":[17], "weekDays":["saturday"]}` | Spustí se každý týden v sobotu v 17:00.
`{"hours":[17], "weekDays":["monday", "wednesday", "friday"]}` | Spustí se každý týden v pondělí, středu a pátek v 17:00.
`{"minutes":[15,45], "hours":[17], "weekDays":["monday", "wednesday", "friday"]}` | Spustí se každý týden v pondělí, středu a pátek v 17:15 a 17:45.
`{"minutes":[0,15,30,45], "weekDays":["monday", "tuesday", "wednesday", "thursday", "friday"]}` | Spustí se ve všední dny každých 15 minut.
`{"minutes":[0,15,30,45], "hours": [9, 10, 11, 12, 13, 14, 15, 16] "weekDays":["monday", "tuesday", "wednesday", "thursday", "friday"]}` | Spustí se ve všední dny každých 15 minut mezi 9:00 a 16:45.
`{"weekDays":["tuesday", "thursday"]}` | Spustí se vždycky v úterý a čtvrtek v zadaný čas začátku.
`{"minutes":[0], "hours":[6], "monthDays":[28]}` | Spustí se vždycky 28. dne v každém měsíci v 6:00 (za předpokladu, že je nastavená měsíční frekvence).
`{"minutes":[0], "hours":[6], "monthDays":[-1]}` | Spustí se poslední den v měsíci v 6:00. Pokud chcete spustit aktivační událost poslední den v měsíci, použijte místo hodnot 28, 29, 30 nebo 31 hodnotu -1.
`{"minutes":[0], "hours":[6], "monthDays":[1,-1]}` | Spustí se v první a poslední den v měsíci v 6:00.
`{monthDays":[1,14]}` | Spustí se vždycky v první a čtrnáctý den v měsíci v zadaný čas začátku.
`{"minutes":[0], "hours":[5], "monthlyOccurrences":[{"day":"friday", "occurrence":1}]}` | Spustí se vždycky první pátek v měsíci v 5:00.
`{"monthlyOccurrences":[{"day":"friday", "occurrence":1}]}` | Spustí se vždycky první pátek v měsíci v zadaný čas začátku.
`{"monthlyOccurrences":[{"day":"friday", "occurrence":-3}]}` | Spustí se třetí pátek od konce každého měsíce v zadaný čas začátku.
`{"minutes":[15], "hours":[5], "monthlyOccurrences":[{"day":"friday", "occurrence":1},{"day":"friday", "occurrence":-1}]}` | Spustí se vždycky první a poslední pátek v měsíci v 5:15.
`{"monthlyOccurrences":[{"day":"friday", "occurrence":1},{"day":"friday", "occurrence":-1}]}` | Spustí se vždycky první a poslední pátek v měsíci v zadaný čas začátku.
`{"monthlyOccurrences":[{"day":"friday", "occurrence":5}]}` | Spustí se vždycky pátý pátek v měsíci v zadaný čas začátku. Pokud určitý měsíc nemá pátý pátek, kanál se nespustí, protože je jeho spouštění naplánováno jenom na páté pátky.  Pokud chcete, aby se aktivační událost spustila vždycky poslední pátek v měsíci, můžete místo hodnoty výskytu 5 zadat hodnotu -1.
`{"minutes":[0,15,30,45], "monthlyOccurrences":[{"day":"friday", "occurrence":-1}]}` | Spustí se vždycky poslední pátek v měsíci každých 15 minut.
`{"minutes":[15,45], "hours":[5,17], "monthlyOccurrences":[{"day":"wednesday", "occurrence":3}]}` | Spustí se vždycky třetí středu v měsíci v 5:15, 5:45, 17:15 a 17:45.


## <a name="use-azure-powershell"></a>Použití Azure Powershell
V této části ukazuje, jak pomocí prostředí Azure PowerShell k vytvoření, spuštění a monitorování plánovače aktivační událost. Pokud chcete zobrazit tento ukázkový pracovní, nejdřív projít [rychlý start: Vytvořte objekt pro vytváření dat pomocí Azure PowerShell](quickstart-create-data-factory-powershell.md). Hlavní metodu, která se vytvoří a spustí plán aktivační událost, která se spouští každých 15 minut, přidejte následující kód. Aktivační událost souvisí s kanálu (**Adfv2QuickStartPipeline**), které vytvoříte jako součást rychlý start.

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

## <a name="next-steps"></a>Další postup
Podrobné informace o aktivační události najdete v tématu [kanálu spouštěcí a aktivační události](concepts-pipeline-execution-triggers.md#triggers).
