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
ms.openlocfilehash: fc34cfbab796c6e1e4cd25ce13dcc63c39c6699d
ms.sourcegitcommit: 79683e67911c3ab14bcae668f7551e57f3095425
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/25/2018
---
# <a name="pipeline-execution-and-triggers-in-azure-data-factory"></a>Spouštění kanálů a aktivační události v Azure Data Factory
> [!div class="op_single_selector" title1="Select the version of the Data Factory service that you're using:"]
> * [Verze 1 – GA](v1/data-factory-scheduling-and-execution.md)
> * [Verze 2 – Preview](concepts-pipeline-execution-triggers.md)

_Spuštění kanálu_ je termín používaný v Azure Data Factory verze 2, který definuje instanci spuštění kanálu. Například si představte, že máte kanál, který se spouští v 8:00, 9:00 a 10:00. V tomto případě existují tři samostatná spuštění kanálu. Každé spuštění kanálu má jedinečné ID spuštění kanálu, což je identifikátor GUID, který jedinečně určuje konkrétní spuštění kanálu. Spuštění kanálu se obvykle inicializuje předáním argumentů parametrům definovaným v kanálech. Existují dva způsoby spuštění kanálu: ručně nebo prostřednictvím _aktivační události_. Tento článek obsahuje podrobnosti o obou způsobech spuštění kanálu.

> [!NOTE]
> Tento článek se týká služby Azure Data Factory verze 2, která je aktuálně ve verzi Preview. Pokud používáte službu Azure Data Factory verze 1, která je všeobecně dostupná (GA), přečtěte si téma o [plánování a provádění ve službě Azure Data Factory verze 1](v1/data-factory-scheduling-and-execution.md).

## <a name="manual-execution-on-demand"></a>Ruční spuštění (na vyžádání)
Ruční spuštění kanálu se označuje také jako spuštění _na vyžádání_.

Předpokládejme například, že máte jednoduchý kanál **copyPipeline**, který chcete spustit. Tento kanál obsahuje jednu aktivitu, která provádí kopírování ze zdrojové složky ve službě Azure Blob Storage do cílové složky ve stejném úložišti. Následující definice JSON ukazuje tento ukázkový kanál:

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

V definici JSON přijímá kanál dva parametry: **sourceBlobContainer** a **sinkBlobContainer**. Do těchto parametrů za běhu předáváte hodnoty.

Kanál můžete ručně spustit následujícími způsoby:
- Pomocí sady .NET SDK.
- Pomocí modulu Azure PowerShellu.
- Pomocí rozhraní REST API.
- Pomocí sady Python SDK.

### <a name="the-rest-api"></a>Rozhraní REST API
Následující ukázkový příkaz ukazuje ruční spuštění kanálu pomocí rozhraní REST API:  

```
POST
https://management.azure.com/subscriptions/mySubId/resourceGroups/myResourceGroup/providers/Microsoft.DataFactory/factories/myDataFactory/pipelines/copyPipeline/createRun?api-version=2017-03-01-preview
```

Úplnou ukázku najdete v tématu [Rychlý start: Vytvoření datové továrny pomocí rozhraní REST API](quickstart-create-data-factory-rest-api.md).

### <a name="azure-powershell"></a>Azure PowerShell
Následující ukázkový příkaz ukazuje ruční spuštění kanálu pomocí Azure PowerShellu:

```powershell
Invoke-AzureRmDataFactoryV2Pipeline -DataFactory $df -PipelineName "Adfv2QuickStartPipeline" -ParameterFile .\PipelineParameters.json
```

Parametry předáváte v těle datové části požadavku. V sadě .NET SDK, Azure Powershellu a sadě Python SDK předáváte hodnoty ve slovníku, který se předává do volání jako argument:

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

Úplnou ukázku najdete v tématu [Rychlý start: Vytvoření datové továrny pomocí Azure Powershellu](quickstart-create-data-factory-powershell.md).

### <a name="the-net-sdk"></a>Sada .NET SDK
Následující ukázkové volání ukazuje ruční spuštění kanálu pomocí sady .NET SDK:

```csharp
client.Pipelines.CreateRunWithHttpMessagesAsync(resourceGroup, dataFactoryName, pipelineName, parameters)
```

Úplnou ukázku najdete v tématu [Rychlý start: Vytvoření datové továrny pomocí sady .NET SDK](quickstart-create-data-factory-dot-net.md).

> [!NOTE]
> Sadu .NET SDK můžete použít k volání kanálů Azure Data Factory ze služby Azure Functions, vlastních webových služeb atd.

<h2 id="triggers">Spuštění pomocí aktivační události</h2>
Aktivační události představují druhý způsob spuštění kanálu. Aktivační události jsou jednotkou zpracování, která určuje, kdy se má zahájit provádění kanálu. V současné době služba Azure Data Factory podporuje dva typy aktivačních událostí:
- Aktivační událost plánovače: Aktivační událost, která volá kanál podle časového plánu.
- Aktivační událost pro přeskakující okno: Aktivační událost, která funguje v pravidelných intervalech a přitom si zachovává svůj stav. Azure Data Factory v současné době nepodporuje aktivační události na základě událostí. Například aktivační událost pro spuštění kanálu jako reakce na událost přijetí souboru.

Mezi kanály a aktivačními událostmi existuje vztah n-m. Více aktivačních událostí může aktivovat jeden kanál a jedna aktivační událost může aktivovat více kanálů. V následující definici aktivační události vlastnost **pipelines** odkazuje na seznam kanálů aktivovaných konkrétní aktivační událostí. Definice vlastnosti obsahuje hodnoty parametrů kanálu.

### <a name="basic-trigger-definition"></a>Definice základní aktivační události

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
Aktivační událost plánovače spouští kanály podle časového plánu. Tato aktivační událost podporuje pravidelné spouštění i rozšířené možnosti kalendáře. Tato aktivační událost podporuje například intervaly, jako „weekly“ (týdně) nebo „Monday at 5:00 PM and Thursday at 9:00 PM“ (V pondělí v 17:00 a ve čtvrtek ve 21:00). Aktivační událost plánovače je flexibilní, protože je nezávislá na vzorech datových sad a nerozlišuje mezi daty časových řad a daty bez časových řad.

Další informace o aktivačních událostech plánovače a příklady najdete v tématu [Vytvoření aktivační události plánovače](how-to-create-schedule-trigger.md).

## <a name="tumbling-window-trigger"></a>Aktivační událost pro přeskakující okno
Aktivační události pro přeskakující okno jsou typem aktivačních událostí, které se aktivuje v pravidelných časových intervalech od určeného počátečního okamžiku a které zachovávají stav. Přeskakující okna jsou řada nepřekrývajících se souvislých časových intervalů s pevnou velikostí. Další informace o aktivačních událostech pro přeskakující okno a příklady najdete v tématu [Vytvoření aktivační události pro přeskakující okno](how-to-create-tumbling-window-trigger.md).

## <a name="schedule-trigger-definition"></a>Definice aktivační události plánovače
Při vytváření aktivační události plánovače určíte plánování a opakování pomocí definice JSON. 

Pokud chcete, aby aktivační událost plánovače aktivovala spuštění kanálu, zahrňte do definice aktivační události odkaz na příslušný kanál. Mezi kanály a aktivačními událostmi existuje vztah n-m. Víc aktivačních událostí může aktivovat jeden kanál. Jedna aktivační událost může aktivovat více kanálů.

```json
{
  "properties": {
    "type": "ScheduleTrigger",
    "typeProperties": {
      "recurrence": {
        "frequency": <<Minute, Hour, Day, Week, Year>>,
        "interval": <<int>>,             // How often to fire
        "startTime": <<datetime>>,
        "endTime": <<datetime>>,
        "timeZone": "UTC"
        "schedule": {                    // Optional (advanced scheduling specifics)
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
> Vlastnost **parameters** elementu **pipelines** je povinná. Pokud váš kanál nepřijímá žádné parametry, musíte pro vlastnost **parameters** zahrnout prázdnou definici JSON.

### <a name="schema-overview"></a>Přehled schématu
Následující tabulka obsahuje přehled hlavních elementů schématu souvisejících s opakováním a plánováním aktivační události:

| Vlastnost JSON | Popis |
|:--- |:--- |
| **startTime** | Hodnota data a času. V případě jednoduchých plánů se hodnota vlastnosti **startTime** vztahuje pouze na první výskyt. U složitějších plánů aktivační událost nezačíná dřív než v čas určený hodnotou **startTime**. |
| **endTime** | Koncové datum a čas pro aktivační událost. Aktivační událost se nebude spouštět po zadaném koncovém datu a času. Hodnota této vlastnosti nemůže být v minulosti. <!-- This property is optional. --> |
| **timeZone** | Časové pásmo. V současné době se podporuje pouze časové pásmo UTC. |
| **recurrence** | Objekt recurrence, který určuje pravidla opakování aktivační události. Objekt recurrence podporuje elementy **frequency** (frekvence), **interval** (interval), **endTime** (koncový čas), **count** (počet) a **schedule** (plán). Když je definovaný objekt recurrence, vyžaduje se element **frequency** (frekvence). Další elementy objektu recurrence jsou volitelné. |
| **frequency** | Jednotka frekvence, s jakou se aktivační událost opakuje. Mezi podporované hodnoty patří „minute“ (minuta), „hour“ (hodina), „day“ (den), „week“ (týden) a „month“ (měsíc). |
| **interval** | Kladné celé číslo označující interval pro hodnotu **frequency**, která určuje, jak často se má aktivační událost spouštět. Pokud má například **interval** hodnotu 3 a **frequency** hodnotu „week“ (týden), aktivační událost se opakuje každé 3 týdny. |
| **schedule** | Plán opakování pro aktivační událost. Aktivační události se zadanou hodnotou **frequency** mění své opakování na základě plánu opakování. Vlastnost **schedule** obsahuje úpravy opakování na základě minut, hodin, dní v týdnu, dní v měsíci a čísla týdne.

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

### <a name="schema-defaults-limits-and-examples"></a>Výchozí hodnoty, omezení a příklady schématu

| Vlastnost JSON | Typ | Požaduje se | Výchozí hodnota | Platné hodnoty | Příklad |
|:--- |:--- |:--- |:--- |:--- |:--- |
| **startTime** | Řetězec | Ano | Žádný | Data a časy podle normy ISO 8601 | `"startTime" : "2013-01-09T09:30:00-08:00"` |
| **recurrence** | Objekt | Ano | Žádný | Objekt opakování | `"recurrence" : { "frequency" : "monthly", "interval" : 1 }` |
| **interval** | Číslo | Ne | 1 | 1 až 1 000 | `"interval":10` |
| **endTime** | Řetězec | Ano | Žádný | Hodnota data a času představující čas v budoucnosti. | `"endTime" : "2013-02-09T09:30:00-08:00"` |
| **schedule** | Objekt | Ne | Žádný | Objekt plánu | `"schedule" : { "minute" : [30], "hour" : [8,17] }` |

### <a name="starttime-property"></a>Vlastnost startTime
Následující tabulka ukazuje, jakým způsobem vlastnost **startTime** ovládá spouštění aktivační události:

| Hodnota startTime | Opakování bez plánu | Opakování s plánem |
|:--- |:--- |:--- |
| Čas začátku v minulosti | Vypočítá první čas spuštění v budoucnosti následující po čase začátku a v tomto čase se spustí.<br/><br/>Zahájí další spuštění na základě výpočtu z posledního času spuštění.<br/><br/>Prohlédněte si příklad pod touto tabulkou. | Aktivační událost se spustí _až po_ zadaném čase začátku. První výskyt vychází z plánu vypočítaného z času začátku.<br/><br/>Zahájí další spuštění na základě plánu opakování. |
| Čas začátku v budoucnosti nebo přítomnosti | Spustí se jednou v zadaný čas začátku.<br/><br/>Zahájí další spuštění na základě výpočtu z posledního času spuštění. | Aktivační událost se spustí _až po_ zadaném čase začátku. První výskyt vychází z plánu vypočítaného z času začátku.<br/><br/>Zahájí další spuštění na základě plánu opakování. |

Podívejme se na příklad toho, co se stane, když je čas začátku v minulosti a je nastaveno opakování, ale žádný plán. Předpokládejme, že aktuální čas je `2017-04-08 13:00`, čas začátku je `2017-04-07 14:00` a opakování je nastaveno na každé dva dny. (Hodnota **recurrence** je definovaná nastavením vlastnosti **frequency** na hodnotu „day“ (den) vlastnosti **interval** na hodnotu 2.) Všimněte si, že hodnota **startTime** je v minulosti a předchází aktuálnímu času.

Za těchto podmínek dojde k prvnímu spuštění v `2017-04-09 at 14:00`. Modul plánovače vypočítá výskyty spuštění na základě času začátku. Všechny instance v minulosti se zahodí. Modul použije další instanci, která nastane v budoucnosti. V tomto scénáři je čas začátku `2017-04-07 at 2:00pm`, takže další instance nastane o dva dny později, tedy v čase `2017-04-09 at 2:00pm`.

První čas spuštění je stejný, i když má vlastnost **startTime** hodnotu `2017-04-05 14:00` nebo `2017-04-01 14:00`. Po prvním spuštění se další spuštění vypočítají na základě plánu. Proto další spuštění proběhnou v čase `2017-04-11 at 2:00pm`, pak v `2017-04-13 at 2:00pm`, pak v `2017-04-15 at 2:00pm` atd.

A nakonec, pokud v plánu aktivační události nejsou nastavené hodiny nebo minuty, jako výchozí hodnoty se použijí hodiny nebo minuty prvního spuštění.

### <a name="schedule-property"></a>Vlastnost schedule
Na jedné straně může použití plánu omezit počet spuštění aktivační události. Pokud má například aktivační událost s měsíční frekvencí naplánované spouštění jenom v 31. den, tato aktivační událost se spustí jenom v měsících, které mají 31 dní.

Na druhou stranu může plán způsobit také zvýšení počtu spuštění aktivační události. Například aktivační událost s měsíční frekvencí, která má naplánované spouštění na 1. a 2. den v měsíci se nespustí jednou za měsíc, ale v 1. i 2. den v měsíci.

Pokud je zadaných více elementů **schedule**, pořadí jejich vyhodnocování postupuje od největšího k nejmenšímu nastavení plánu. Vyhodnocování začne číslem týdne a pak pokračuje dnem v měsíci, dnem v týdnu, hodinou a nakonec minutou.

Následující tabulka obsahuje podrobný popis elementů **schedule**:

| Element JSON | Popis | Platné hodnoty |
|:--- |:--- |:--- |
| **minutes** | Minuty v hodině, ve kterých se aktivační událost spouští. | <ul><li>Integer</li><li>Pole celých čísel</li></ul>
| **hours** | Hodiny dne, ve kterých se aktivační událost spouští. | <ul><li>Integer</li><li>Pole celých čísel</li></ul> |
| **weekDays** | Dny v týdnu, ve kterých se aktivační událost spouští. Tuto hodnotu je možné zadat jenom při týdenní frekvenci. | <ul><li>Monday (pondělí), Tuesday (úterý), Wednesday (středa), Thursday (čtvrtek), Friday (pátek), Saturday (sobota), Sunday (neděle)</li><li>Pole hodnot dní (maximální velikost pole je 7)</li><li>V hodnotách dní se nerozlišují malá a velká písmena</li></ul> |
| **monthlyOccurrences** | Dny v měsíci, ve kterých se aktivační událost spouští. Tuto hodnotu je možné zadat jenom při měsíční frekvenci. | <ul><li>Pole objektů **monthlyOccurrence**: `{ "day": day,  "occurrence": occurence }`.</li><li>Atribut **day** představuje den v týdnu, ve kterém se aktivační událost spouští. Například vlastnost **monthlyOccurrences** s atributem **day** s hodnotou `{Sunday}` znamená každou neděli v měsíci. Atribut **day** je povinný.</li><li>Atribut **occurrence** představuje výskyt zadaného dne (**day**) v měsíci. Například vlastnost **monthlyOccurrences** s atributy **day** a **occurrence** s hodnotami `{Sunday, -1}` znamená poslední neděli v měsíci. Atribut **occurrence** je volitelný.</li></ul> |
| **monthDays** | Dan v měsíci, ve kterém se aktivační událost spouští. Tuto hodnotu je možné zadat jenom při měsíční frekvenci. | <ul><li>Libovolná hodnota < = -1 a > =-31</li><li>Libovolná hodnota >= 1 a <= 31</li><li>Pole hodnot</li></ul> |

## <a name="examples-of-trigger-recurrence-schedules"></a>Příklady plánů opakování aktivačních událostí
Tato část obsahuje příklady plánů opakování se zaměřením na objekt **schedule** a jeho elementy.

V příkladech se předpokládá, že vlastnost **interval** má hodnotu 1 a vlastnost **frequency** má správnou hodnotu s ohledem na definici plánu. Například není možné, aby vlastnost **frequency** měla hodnotu „day“ a zároveň objekt **schedule** obsahoval úpravu atributu „monthDays“. Tato a podobná omezení jsou popsaná v tabulce v předchozí části.

| Příklad | Popis |
|:--- |:--- |
| `{"hours":[5]}` | Spuštění každý den v 5:00. |
| `{"minutes":[15], "hours":[5]}` | Spuštění každý den v 5:15. |
| `{"minutes":[15], "hours":[5,17]}` | Spuštění každý den v 5:15 a 17:15. |
| `{"minutes":[15,45], "hours":[5,17]}` | Spuštění každý den v 5:15, 5:45 a 17:15 a 17:45. |
| `{"minutes":[0,15,30,45]}` | Spuštění každých 15 minut. |
| `{hours":[0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23]}` | Spuštění každou hodinu. Tato aktivační událost se spouští každou hodinu. Minuty se řídí hodnotou **startTime**, pokud je zadaná. Pokud hodnotu nezadáte, minuty se řídí časem vytvoření. Například pokud je čas začátku nebo čas vytvoření (podle situace) 0:25, aktivační událost se spustí v 0:25, 1:25, 2:25 atd. až do 23:25.<br/><br/>Tento plán je stejný jako aktivační událost s vlastností **frequency** s hodnotou „hour“ (hodina), vlastností **interval** s hodnotou 1 a žádným objektem **schedule**. Tento plán se dá s jinými hodnotami **frequency** a **interval** použít k vytvoření jiných aktivačních událostí. Když má například **frequency** hodnotu „month“ (měsíc), plán se spustí pouze jednou za měsíc, a ne každý den, jako když má **frequency** hodnotu „day“ (den). |
| `{"minutes":[0]}` | Spuštění v každou celou hodinu. Tato aktivační událost se spouští každou celou hodinu počínaje 0:00, 1:00, 2:00 atd.<br/><br/>Tento plán je stejný jako aktivační událost s vlastností **frequency** s hodnotou „hour“ (hodina) a vlastností **startTime** s hodnotou nula minut nebo žádným objektem **schedule**, ale s vlastností **frequency** s hodnotou „day“ (den). Pokud má **frequency** hodnotu „week“ (týden) nebo „month“ (měsíc), plán se spustí pouze v jeden den v týdnu nebo jeden den v měsíci. |
| `{"minutes":[15]}` | Spuštění 15 minut po každé celé hodině. Tato aktivační událost se spouští 15 minut po každé celé hodině počínaje 0:15, 1:15, 2:15 atd. až do 23:15. |
| `{"hours":[17], "weekDays":["saturday"]}` | Spuštění každý týden v sobotu v 17:00. |
| `{"hours":[17], "weekDays":["monday", "wednesday", "friday"]}` | Spuštění každý týden v pondělí, středu a pátek v 17:00. |
| `{"minutes":[15,45], "hours":[17], "weekDays":["monday", "wednesday", "friday"]}` | Spuštění každý týden v pondělí, středu a pátek v 17:15 a 17:45. |
| `{"minutes":[0,15,30,45], "weekDays":["monday", "tuesday", "wednesday", "thursday", "friday"]}` | Spuštění ve všední dny každých 15 minut. |
| `{"minutes":[0,15,30,45], "hours": [9, 10, 11, 12, 13, 14, 15, 16] "weekDays":["monday", "tuesday", "wednesday", "thursday", "friday"]}` | Spuštění ve všední dny každých 15 minut mezi 9:00 a 16:45. |
| `{"weekDays":["tuesday", "thursday"]}` | Spuštění každé úterý a čtvrtek v zadaný čas začátku. |
| `{"minutes":[0], "hours":[6], "monthDays":[28]}` | Spuštění 28. dne každého měsíce v 6:00 (za předpokladu, že **frequency** má hodnotu „month“ (měsíc)). |
| `{"minutes":[0], "hours":[6], "monthDays":[-1]}` | Spuštění poslední den v měsíci v 6:00. Pokud chcete spustit aktivační událost poslední den v měsíci, použijte místo hodnot 28, 29, 30 nebo 31 hodnotu -1. |
| `{"minutes":[0], "hours":[6], "monthDays":[1,-1]}` | Spuštění v každý první a poslední den v měsíci v 6:00. |
| `{monthDays":[1,14]}` | Spuštění v každý první a 14. den v měsíci v zadaný čas začátku. |
| `{"minutes":[0], "hours":[5], "monthlyOccurrences":[{"day":"friday", "occurrence":1}]}` | Spuštění v každý první pátek v měsíci v 5:00. |
| `{"monthlyOccurrences":[{"day":"friday", "occurrence":1}]}` | Spuštění v každý první pátek v měsíci v zadaný čas začátku. |
| `{"monthlyOccurrences":[{"day":"friday", "occurrence":-3}]}` | Spuštění v každý třetí pátek od konce měsíce v zadaný čas začátku. |
| `{"minutes":[15], "hours":[5], "monthlyOccurrences":[{"day":"friday", "occurrence":1},{"day":"friday", "occurrence":-1}]}` | Spuštění v každý první a poslední pátek v měsíci v 5:15. |
| `{"monthlyOccurrences":[{"day":"friday", "occurrence":1},{"day":"friday", "occurrence":-1}]}` | Spuštění v každý první a poslední pátek v měsíci v zadaný čas začátku. |
| `{"monthlyOccurrences":[{"day":"friday", "occurrence":5}]}` | Spuštění v každý pátý pátek v měsíci v zadaný čas začátku. Pokud určitý měsíc nemá pátý pátek, kanál se nespustí, protože je jeho spouštění naplánováno jenom na páté pátky. Pokud chcete, aby se aktivační událost spouštěla v poslední pátek v měsíci, můžete jako **occurrence** místo hodnoty 5 zadat hodnotu -1. |
| `{"minutes":[0,15,30,45], "monthlyOccurrences":[{"day":"friday", "occurrence":-1}]}` | Spuštění v poslední pátek v měsíci každých 15 minut. |
| `{"minutes":[15,45], "hours":[5,17], "monthlyOccurrences":[{"day":"wednesday", "occurrence":3}]}` | Spuštění každou třetí středu v měsíci v 5:15, 5:45, 17:15 a 17:45. |

## <a name="trigger-type-comparison"></a>Porovnání typů aktivačních událostí
Aktivační událost pro přeskakující okno i aktivační událost plánovače pracují s časovými intervaly, jaký je mezi nimi tedy vlastně rozdíl?

Následující tabulka obsahuje porovnání aktivační události pro přeskakující okno a aktivační události plánovače:

|  | Aktivační událost&nbsp;pro&nbsp;přeskakující okno | Aktivační událost&nbsp;plánovače |
|:--- |:--- |:--- |
| **Scénáře&nbsp;obnovení dat** | Podporuje se. Spuštění kanálu je možné naplánovat i pro okna v minulosti. | Nepodporuje se. Spuštění kanálu je možné provést pouze v časová období od současnosti do budoucnosti. |
| **Spolehlivost** | 100% spolehlivost. Spuštění kanálu je možné plánovat pro všechna okna od zadaného času začátku, a to bez mezer. | Menší spolehlivost. |
| **Možnost&nbsp;opakování** | Podporuje se. Spuštění kanálu, která selžou, mají výchozí zásadu opakování 0 nebo zásadu zadanou uživatelem v definici aktivační události. Automaticky opakuje pokus, když spuštění kanálu selže kvůli omezením souběžnosti, serveru nebo využití sítě (tedy stavové kódy 400: Chyba uživatele, 429: Příliš mnoho požadavků a 500: Vnitřní chyba serveru). | Nepodporuje se. |
| **Souběžnost** | Podporuje se. Uživatelé můžou pro aktivační událost explicitně nastavit omezení souběžnosti. Umožňuje 1 až 50 souběžně aktivovaných spuštění kanálu. | Nepodporuje se. |
| **Systémové&nbsp;proměnné** | Podporuje použití systémových proměnných **WindowStart** a **WindowEnd**. Uživatelé mají v definici aktivační události přístup k `triggerOutputs().windowStartTime` a `triggerOutputs().windowEndTime` jako systémovým proměnným aktivační události. Tyto hodnoty se používají v čase začátku okna a v čase konce okna. Například pro aktivační událost pro přeskakující okno, která se spouští každou hodinu, je definice okna od 1:00 do 2:00 následující: `triggerOutputs().WindowStartTime = 2017-09-01T01:00:00Z` a `triggerOutputs().WindowEndTime = 2017-09-01T02:00:00Z`. | Nepodporuje se. |
| **Vztah mezi kanálem a aktivační událostí** | Podporuje vztah 1:1. Je možné aktivovat pouze jeden kanál. | Podporuje vztahy M:N. Víc aktivačních událostí může aktivovat jeden kanál. Jedna aktivační událost může aktivovat více kanálů. | 

## <a name="next-steps"></a>Další kroky
Projděte si tyto kurzy:

- [Rychlý start: Vytvoření datové továrny pomocí sady .NET SDK](quickstart-create-data-factory-dot-net.md)
- [Vytvoření aktivační události plánovače](how-to-create-schedule-trigger.md)
- [Vytvoření aktivační události pro přeskakující okno](how-to-create-tumbling-window-trigger.md)
