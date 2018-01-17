---
title: "Spouštění kanálů a aktivační události v Azure Data Factory | Dokumentace Microsoftu"
description: "Tento článek obsahuje informace o tom, jak spustit kanál v Azure Data Factory, a to buď na vyžádání, nebo pomocí aktivační události."
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
ms.date: 01/03/2018
ms.author: shlo
ms.openlocfilehash: 88ae5dfbf6246ecf92d6528ad3d9a8e5fb57e4b0
ms.sourcegitcommit: 0e1c4b925c778de4924c4985504a1791b8330c71
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/06/2018
---
# <a name="pipeline-execution-and-triggers-in-azure-data-factory"></a>Spouštění kanálů a aktivační události v Azure Data Factory
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Verze 1 – GA](v1/data-factory-scheduling-and-execution.md)
> * [Verze 2 – Preview](concepts-pipeline-execution-triggers.md)

**Spuštění kanálu** je termín používaný v Azure Data Factory verze 2, který definuje instanci provádění kanálu. Například předpokládejme, že máte kanál, který se provádí v 8:00, 9:00 a 10:00. V tomto případě by tedy existovala tři samostatná spuštění kanálu. Každé spuštění kanálu má jedinečné ID spuštění kanálu, což je identifikátor GUID, který jedinečně určuje konkrétní spuštění kanálu. Instance spuštění kanálu se obvykle vytvářejí předáváním argumentů do parametrů definovaných v kanálech. Existují dva způsoby provedení kanálu: **ručně** nebo prostřednictvím **aktivační události**. Tento článek obsahuje podrobné informace o obou způsobech provedení kanálu.

> [!NOTE]
> Tento článek se týká verze 2 služby Data Factory, která je aktuálně ve verzi Preview. Pokud používáte verzi 1 služby Data Factory, který je všeobecně dostupná (GA), přečtěte si téma o [plánování a provádění ve službě Data Factory verze 1](v1/data-factory-scheduling-and-execution.md).

## <a name="run-pipeline-on-demand"></a>Spuštění kanálu na vyžádání
Tato metoda slouží k ručnímu spuštění kanálu. Také se považuje za provedení kanálu na požádání.

Například předpokládejme, že máte kanál s názvem **copyPipeline**, který chcete provést. Kanál je jednoduchý kanál s jednou aktivitou, která se zkopíruje ze zdrojové složky v Azure Blob Storage do cílové složky ve stejném úložišti. Toto je ukázková definice kanálu:

```json
{
  "name": "copyPipeline",
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
        "name": "CopyBlobtoBlob",
        "inputs": [
          {
            "referenceName": "sourceBlobDataset",
            "type": "DatasetReference"
          }
        ],
        "outputs": [
          {
            "referenceName": "sinkBlobDataset",
            "type": "DatasetReference"
          }
        ]
      }
    ],
    "parameters": {
      "sourceBlobContainer": {
        "type": "String"
      },
      "sinkBlobContainer": {
        "type": "String"
      }
    }
  }
}

```
Kanál má dva parametry:sourceBlobContainer a sinkBlobContainer, jak ukazuje definice JSON. Do těchto parametrů za běhu předáváte hodnoty.

Pokud chcete kanál spustit ručně, můžete to udělat jedním z těchto způsobů: rozhraní .NET, prostředí PowerShell, REST a Python.

### <a name="rest-api"></a>REST API
Toto je ukázka příkazu REST:  

```
POST
https://management.azure.com/subscriptions/mySubId/resourceGroups/myResourceGroup/providers/Microsoft.DataFactory/factories/myDataFactory/pipelines/copyPipeline/createRun?api-version=2017-03-01-preview
```
Úplnou ukázku najdete v tématu [Rychlý start: Vytvoření datové továrny pomocí rozhraní REST API](quickstart-create-data-factory-rest-api.md).

### <a name="powershell"></a>PowerShell
Toto je ukázka příkazu prostředí PowerShell:

```powershell
Invoke-AzureRmDataFactoryV2Pipeline -DataFactory $df -PipelineName "Adfv2QuickStartPipeline" -ParameterFile .\PipelineParameters.json
```

Parametry předáváte v těle datové části požadavku. V prostředích .NET, Powershell a Python předáváte hodnoty ve slovníku, který se předává do volání jako argument.

```json
{
  “sourceBlobContainer”: “MySourceFolder”,
  “sinkBlobCountainer”: “MySinkFolder”
}
```

Datová část odpovědi představuje jedinečné ID spuštění kanálu:

```json
{
  "runId": "0448d45a-a0bd-23f3-90a5-bfeea9264aed"
}
```


Úplnou ukázku najdete v tématu [Rychlý start: Vytvoření datové továrny pomocí prostředí PowerShell](quickstart-create-data-factory-powershell.md).

### <a name="net"></a>.NET
Toto je ukázka volání rozhraní .NET:

```csharp
client.Pipelines.CreateRunWithHttpMessagesAsync(resourceGroup, dataFactoryName, pipelineName, parameters)
```

Úplnou ukázku najdete v tématu [Rychlý start: Vytvoření datové továrny pomocí rozhraní .NET](quickstart-create-data-factory-dot-net.md).

> [!NOTE]
> Rozhraní API .NET můžete použít k volání kanálů Data Factory ze služby Azure Functions, vlastních webových služeb atd.

## <a name="triggers"></a>Triggery
Aktivační události představují druhý způsob provedení spuštění kanálu. Aktivační události jsou jednotkou zpracování, která určuje, kdy se má zahájit provádění kanálu. Data Factory v současné době podporuje dva typy aktivačních událostí: 1)**Aktivační událost plánovače**, která volá kanál podle časového plánu, a 2)**Aktivační událost pro přeskakující okno**, která pracuje v periodických intervalech a zachovává stav. Data Factory v současné době nepodporuje aktivační události na základě událostí, jako například aktivační událost spuštění kanálu v případě přijetí souboru.

Mezi kanály a aktivačními událostmi existuje vztah n-m. Více aktivačních událostí může aktivovat jeden kanál a jedna aktivační událost může aktivovat více kanálů. V následující definici JSON aktivační události vlastnost **pipelines** odkazuje na seznam kanálů aktivovaných určitou aktivační událostí a hodnoty parametrů kanálu.

### <a name="basic-trigger-definition"></a>Definice základní aktivační události:
```json
    "properties": {
        "name": "MyTrigger",
        "type": "<type of trigger>",
        "typeProperties": {
            …
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
```

## <a name="schedule-trigger"></a>Aktivační událost plánovače
Aktivační událost plánovače spouští kanály podle časového plánu. Tato aktivační událost podporuje možnosti pravidelného opakování a pokročilé možnosti kalendáře (jednou týdně, v pondělí v 17:00 a ve čtvrtek ve 21:00). Je flexibilní, protože je nezávislá na vzorech datových sad a nerozlišuje mezi daty časových řad a daty bez časových řad.

Podrobnější informace o aktivačních událostech plánovače a příklady najdete v článku [Postupy: Vytvoření aktivační události plánovače](how-to-create-schedule-trigger.md).

## <a name="tumbling-window-trigger"></a>Aktivační událost pro přeskakující okno
Aktivační události pro přeskakující okno jsou typem aktivačních událostí, které se aktivuje v pravidelných časových intervalech od určeného počátečního okamžiku a které zachovávají stav. Přeskakující okna jsou řada nepřekrývajících se souvislých časových intervalů s pevnou velikostí.
Podrobnější informace a příklady o aktivačních událostech pro přeskakující okno najdete v článku [Postupy: Vytvoření aktivační události pro přeskakující okno](how-to-create-tumbling-window-trigger.md).

### <a name="scheduler-trigger-json-definition"></a>Definice JSON aktivační události plánovače
Když vytvoříte aktivační událost plánovače, můžete zadat plánování a opakování pomocí formátu JSON, jak znázorňuje příklad v této části. 

Pokud chcete, aby aktivační událost plánovače aktivovala spuštění kanálu, zahrňte do definice aktivační události odkaz na příslušný kanál. Mezi kanály a aktivačními událostmi existuje vztah n-m. Víc aktivačních událostí může aktivovat jeden kanál. Jedna aktivační událost může aktivovat více kanálů.

```json
{
  "properties": {
    "type": "ScheduleTrigger",
    "typeProperties": {
      "recurrence": {
        "frequency": <<Minute, Hour, Day, Week, Year>>,
        "interval": <<int>>,             // how often to fire
        "startTime": <<datetime>>,
        "endTime": <<datetime>>,
        "timeZone": "UTC"
        "schedule": {                    // optional (advanced scheduling specifics)
          "hours": [<<0-24>>],
          "weekDays": ": [<<Monday-Sunday>>],
          "minutes": [<<0-60>>],
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


### <a name="overview-scheduler-trigger-schema"></a>Přehled: schéma aktivační události plánovače
Následující tabulka obsahuje přehled hlavních elementů souvisejících s opakováním a plánováním aktivační události:

Vlastnost JSON |     Popis
------------- | -------------
startTime | Položka startTime je ve formátu datum-čas. U jednoduchých plánů představuje položka startTime první výskyt. U složitějších plánů aktivační událost nezačíná dřív než čas určený hodnotou startTime.
endTime | Určuje datum a čas konce aktivační události. Po uplynutí tohoto času se už aktivační událost neprovede. Hodnota endTime v minulosti není platná.
timeZone | Aktuálně se podporuje pouze UTC. 
recurrence | Objekt recurrence určuje pravidla opakování aktivační události. Objekt recurrence podporuje tyto elementy: frekvence, interval, čas konce, počet a plán. Pokud se definuje opakování, je frekvence povinná. Ostatní elementy opakování jsou volitelné.
frequency | Představuje jednotku frekvence, s jakou se aktivační událost opakuje. Podporované hodnoty: `minute`, `hour`, `day`, `week` nebo `month`.
interval | Interval je kladné celé číslo. Označuje interval pro frekvenci, který určuje, jak často se má aktivační událost spouštět. Pokud má například interval hodnotu 3 a frekvence hodnotu „týden“, aktivační událost se opakuje každé 3 týdny.
schedule | Aktivační události se zadanou frekvencí mění své opakování na základě plánu opakování. Plán obsahuje úpravy na základě minut, hodin, dní v týdnu, dní v měsíci a čísla týdne.


## <a name="tumbling-window-trigger-vs-schedule-trigger"></a>Aktivační událost pro přeskakující okno vs. aktivační událost plánovače
Když aktivační události plánovače i pro přeskakující okno pracují s časovými intervaly, jaký je mezi nimi vlastně rozdíl?
V případě aktivační události pro přeskakující okno:
* **Scénáře obnovení dat:** Aktivační události pro přeskakující okno podporují scénáře obnovení dat, takže je možné je naplánovat ke spuštění pro okna v minulosti. Aktivační událost plánovače je možné spustit jen v časových obdobích v budoucnosti.
* **Spolehlivost:** Aktivační události pro přeskakující okno naplánují spuštění kanálu pro všechna okna od počátečního okamžiku bez mezer a se 100% spolehlivostí.
* **Opakování pokusu:** Aktivační události pro přeskakující okno mají možnost opakování pokusu. Nezdařená spuštění kanálu mají výchozí zásadu opakování pokusu 0, ale uživatel ji může při definování aktivační události změnit. K opakování pokusu dojde automaticky u těch instancí, u kterých došlo k selhání z důvodu omezení souběžnosti, serveru nebo využití sítě, tj. v případě stavových kódů 400 (chyba uživatele), 429 (příliš mnoho požadavků) nebo 500 (vnitřní chyba serveru).
* **Souběžnost:** Aktivační události pro přeskakující okno umožňují uživatelům explicitně nastavit limity souběžnosti pro aktivační událost (max. 1–50 souběžných spouštění kanálu)
* **Proměnné počátku a konce okna:** V případě aktivačních událostí pro přeskakující okno mají uživatelé v definici aktivační události přístup k systémovým proměnným triggerOutputs().windowStartTime a triggerOutputs().windowEndTime, které obsahují začátek a konec časového okna. Například pokud máte aktivační událost pro přeskakující okno spouštěnou každou hodinu, v případě okna 01:00 – 02:00 bude triggerOutputs().WindowStartTime = 2017-09-01T01:00:00Z a triggerOutputs().WindowEndTime = 2017-09-01T02:00:00Z.
* **Vztah mezi kanálem a aktivační událostí:** Mezi aktivačními událostmi plánovače a kanály je vztah n:m. Aktivační událost plánovače může spouštět více kanálů. Mezi aktivačními událostmi pro přeskakující okno a kanály je vztah 1:1. Každá aktivační událost pro přeskakující okno může spouštět jen jeden kanál.

### <a name="schedule-trigger-example"></a>Příklad plánování aktivační události

```json
{
    "properties": {
        "name": "MyTrigger",
        "type": "ScheduleTrigger",
        "typeProperties": {
            "recurrence": {
                "frequency": "Hour",
                "interval": 1,
                "startTime": "2017-11-01T09:00:00-08:00",
                "endTime": "2017-11-02T22:00:00-08:00"
            }
        },
        "pipelines": [{
                "pipelineReference": {
                    "type": "PipelineReference",
                    "referenceName": "SQLServerToBlobPipeline"
                },
                "parameters": {}
            },
            {
                "pipelineReference": {
                    "type": "PipelineReference",
                    "referenceName": "SQLServerToAzureSQLPipeline"
                },
                "parameters": {}
            }
        ]
    }
}
```

### <a name="overview-scheduler-trigger-schema-defaults-limits-and-examples"></a>Přehled: výchozí hodnoty schématu aktivační události plánovače, omezení a příklady

Název JSON | Typ hodnoty | Povinné? | Výchozí hodnota | Platné hodnoty | Příklad
--------- | ---------- | --------- | ------------- | ------------ | -------
startTime | Řetězec | Ano | Žádný | Data a časy podle normy ISO 8601 | ```"startTime" : "2013-01-09T09:30:00-08:00"```
recurrence | Objekt | Ano | Žádná | Objekt opakování | ```"recurrence" : { "frequency" : "monthly", "interval" : 1 }```
interval | Číslo | Ano | Žádný | 1 až 1000 | ```"interval":10```
endTime | Řetězec | Ano | Žádný | Hodnota data a času představující čas v budoucnosti | `"endTime" : "2013-02-09T09:30:00-08:00"`
schedule | Objekt | Ne | Žádná | Objekt plánu | `"schedule" : { "minute" : [30], "hour" : [8,17] }`

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
minutes | Minuty v hodině, ve kterých se aktivační událost spouští. | <ul><li>Pole celých čísel</li></ul>
hours | Hodiny dne, ve kterých se aktivační událost spouští. | <ul><li>Pole celých čísel</li></ul>
weekDays | Dny v týdnu, ve kterých se aktivační událost spouští. Tuto položku je možné zadat jenom při týdenní frekvenci. | <ul><li>Pole jakýchkoli z následujících hodnot (maximální velikost pole je 7)<ul><li>Pondělí</li><li>Úterý</li><li>Středa</li><li>Čtvrtek</li><li>Pátek</li><li>Sobota</li><li>Neděle</li></ul></li></p>Nerozlišují se malá a velká písmena.</p>
monthlyOccurrences | Určuje, které dny v měsíci se má aktivační událost spouštět. Tuto položku je možné zadat jenom při měsíční frekvenci. | Pole objektů monthlyOccurence: `{ "day": day,  "occurrence": occurence }`. <p> Den je den v týdnu, kdy se spouští aktivační událost, například hodnota `{Sunday}` určuje každou neděli v měsíci. Povinná hodnota.<p>Výskyt je výskyt dne v měsíci, například `{Sunday, -1}` je poslední neděle v měsíci. Volitelné.
monthDays | Den v měsíci, kdy se aktivační událost spouští. Tuto položku je možné zadat jenom při měsíční frekvenci. | <ul><li>Pole následujících hodnot</li><ul><li>Libovolná hodnota < = -1 a > =-31</li><li>Libovolná hodnota >= 1 a <= 31</li></ul></ul> |


## <a name="examples-recurrence-schedules"></a>Příklady: plány opakování
Tato část obsahuje příklady plánů opakování se zaměřením na objekt plánu a jeho dílčí elementy.

Příklady plánů vycházejí z předpokladu, že je interval nastavený na hodnotu 1. Také předpokládejme správnou frekvenci odpovídající obsahu plánu – například nemůžete použít frekvenci „den“, a přitom mít v plánu úpravu elementu „monthDays“. Tato omezení se nachází v tabulce v předchozí části. 

Příklad | Popis
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




## <a name="next-steps"></a>Další kroky
Projděte si tyto kurzy:

- [Rychlý start: Vytvoření datové továrny pomocí rozhraní .NET](quickstart-create-data-factory-dot-net.md)
- [Postup: Vytvoření aktivační události plánovače](how-to-create-schedule-trigger.md)
- [Postup: Vytvoření aktivační události pro přeskakující okno](how-to-create-tumbling-window-trigger.md)
