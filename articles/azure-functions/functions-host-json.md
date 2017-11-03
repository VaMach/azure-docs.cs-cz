---
title: Host.JSON odkazu pro Azure Functions
description: "Referenční dokumentace pro soubor host.json Azure Functions."
services: functions
author: tdykstra
manager: cfowler
editor: 
tags: 
keywords: 
ms.service: functions
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 10/12/2017
ms.author: tdykstra
ms.openlocfilehash: b3e5976a84e0ec91a41d683a426b58635fd5abd6
ms.sourcegitcommit: 6acb46cfc07f8fade42aff1e3f1c578aa9150c73
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/18/2017
---
# <a name="hostjson-reference-for-azure-functions"></a>Host.JSON odkazu pro Azure Functions

*Host.json* soubor metadat obsahuje možnosti globální konfigurace, které ovlivňují všechny funkce pro funkce aplikace. Tento článek obsahuje seznam nastavení, které jsou k dispozici. Schéma JSON je na http://json.schemastore.org/host.

Existují další možnosti globální konfigurace v [nastavení aplikace](functions-app-settings.md) a v [local.settings.json](functions-run-local.md#local-settings-file) souboru.

## <a name="sample-hostjson-file"></a>Ukázkový soubor host.json

Následující ukázka *host.json* soubor obsahuje všechny možné možností.

```json
{
    "aggregator": {
        "batchSize": 1000,
        "flushTimeout": "00:00:30"
    },
    "applicationInsights": {
        "sampling": {
          "isEnabled": true,
          "maxTelemetryItemsPerSecond" : 5
        }
    },
    "eventHub": {
      "maxBatchSize": 64,
      "prefetchCount": 256,
      "batchCheckpointFrequency": 1
    },
    "functions": [ "QueueProcessor", "GitHubWebHook" ],
    "functionTimeout": "00:05:00",
    "http": {
        "routePrefix": "api",
        "maxOutstandingRequests": 20,
        "maxConcurrentRequests": 10,
        "dynamicThrottlesEnabled": false
    },
    "id": "9f4ea53c5136457d883d685e57164f08",
    "logger": {
        "categoryFilter": {
            "defaultLevel": "Information",
            "categoryLevels": {
                "Host": "Error",
                "Function": "Error",
                "Host.Aggregator": "Information"
            }
        }
    },
    "queues": {
      "maxPollingInterval": 2000,
      "visibilityTimeout" : "00:00:30",
      "batchSize": 16,
      "maxDequeueCount": 5,
      "newBatchThreshold": 8
    },
    "serviceBus": {
      "maxConcurrentCalls": 16,
      "prefetchCount": 100,
      "autoRenewTimeout": "00:05:00"
    },
    "singleton": {
      "lockPeriod": "00:00:15",
      "listenerLockPeriod": "00:01:00",
      "listenerLockRecoveryPollingInterval": "00:01:00",
      "lockAcquisitionTimeout": "00:01:00",
      "lockAcquisitionPollingInterval": "00:00:03"
    },
    "tracing": {
      "consoleLevel": "verbose",
      "fileLoggingMode": "debugOnly"
    },
    "watchDirectories": [ "Shared" ],
}
```

Následující části tohoto článku popisují každou vlastnost nejvyšší úrovně. Všechny jsou volitelné, pokud není uvedeno jinak.

## <a name="aggregator"></a>Agregátor

Určuje, kolik volání funkce agregovat při [výpočet metriky pro službu Application Insights](functions-monitoring.md#configure-the-aggregator). 

```json
{
    "aggregator": {
        "batchSize": 1000,
        "flushTimeout": "00:00:30"
    }
}
```

|Vlastnost  |Výchozí | Popis |
|---------|---------|---------| 
|batchSize|1000|Maximální počet požadavků, které k agregaci.| 
|flushTimeout|00:00:30|Maximální doba období k agregaci.| 

Volání funkce jsou agregovat při první dva omezuje se dosáhne.

## <a name="applicationinsights"></a>applicationInsights

Ovládací prvky [vzorkování funkce ve službě Application Insights](functions-monitoring.md#configure-sampling).

```json
{
    "applicationInsights": {
        "sampling": {
          "isEnabled": true,
          "maxTelemetryItemsPerSecond" : 5
        }
    }
}
```

|Vlastnost  |Výchozí | Popis |
|---------|---------|---------| 
|Hodnotu IsEnabled|False|Povolí nebo zakáže vzorkování.| 
|maxTelemetryItemsPerSecond|5|Prahová hodnota, na které vzorkování začne.| 

## <a name="eventhub"></a>Centrum EventHub

Nastavení konfigurace pro [centra událostí triggerů a vazeb](functions-bindings-event-hubs.md).

```json
{
    "eventHub": {
      "maxBatchSize": 64,
      "prefetchCount": 256,
      "batchCheckpointFrequency": 1
    }
}
```

|Vlastnost  |Výchozí | Popis |
|---------|---------|---------| 
|maxBatchSize|64|Maximální velikost události počet přijatých za receive smyčky.|
|prefetchCount|neuvedeno|Výchozí hodnota PrefetchCount, který se použije základní knihovny EventProcessorHost.| 
|batchCheckpointFrequency|1|Počet událostí zpracovávaných dávek před vytvořením kontrolního bodu Centrum EventHub kurzoru.| 

## <a name="functions"></a>Funkce

Seznam funkcí, které budou spouštět úlohy hostitele.  Prázdné pole znamená spustit všechny funkce.  Určený k použití pouze tehdy, když [spuštěn místně](functions-run-local.md). V aplikacích pro funkce, použijte *function.json* `disabled` vlastnost spíše než tuto vlastnost v *host.json*.

```json
{
    "functions": [ "QueueProcessor", "GitHubWebHook" ]
}
```

## <a name="functiontimeout"></a>functionTimeout

Určuje dobu trvání časového limitu pro všechny funkce. V rámci plánů spotřebu platný rozsah je od 1 sekundy do 10 minut a výchozí hodnota je 5 minut. V plánech služby App Service není omezen a výchozí hodnota je null, což značí bez časového limitu.

```json
{
    "functionTimeout": "00:05:00"
}
```

## <a name="http"></a>http

Nastavení konfigurace pro [http triggerů a vazeb](functions-bindings-http-webhook.md).

```json
{
    "http": {
        "routePrefix": "api",
        "maxOutstandingRequests": 20,
        "maxConcurrentRequests": 10,
        "dynamicThrottlesEnabled": false
    }
}
```

|Vlastnost  |Výchozí | Popis |
|---------|---------|---------| 
|Parametr routePrefix|rozhraní api|Předpona trasy, která platí pro všechny trasy. K odebrání výchozí předponu použijte prázdný řetězec. |
|maxOutstandingRequests|-1|Maximální počet nevyřízených požadavků, které bude možné uchovávat v každém okamžiku (-1 znamená bez vazby). Limit zahrnuje požadavky, které jsou zařazeny do fronty, ale nebyla spuštěna, a také všechny probíhající spuštěních. Všechny příchozí požadavky přes tento limit byly zamítnuty 429 "Zaneprázdněn" odpovědi. Volající můžete použít tuto odpověď využívat strategie opakování založené na čase. Určuje toto nastavení pouze služby Řízení front, dojde v této cestě hostitele provádění úlohy. Další fronty, jako je například fronty požadavků ASP.NET, je toto nastavení nemá vliv. |
|maxConcurrentRequests|-1|Maximální počet funkce protokolu HTTP, které budou spuštěny současně (-1 znamená bez vazby). Například můžete nastavit omezení Pokud funkce protokolu HTTP používat příliš mnoho systémových prostředků, když je souběžnosti vysoké. Nebo pokud funkcí provést odchozí požadavky služby třetích stran, těchto volání může být nutné míra limited.|
|dynamicThrottlesEnabled|False|Způsobí, že zpracování kanálu požadavku na pravidelně kontrolovat, čítače výkonu systému. Čítače zahrnují připojení, vláken, procesy, paměti a procesoru. Pokud některá z počítadla předdefinované nadměrného (80 %), požadavky byly zamítnuty 429 "Zaneprázdněn" odpovědi, dokud counter(s) vrátit do normálního úrovně.|

## <a name="id"></a>id

Jedinečné ID pro úlohu hostitele. Mohou být malé písmeno GUID s pomlčkami odebrány. Při místním spuštění vyžaduje. Při spuštění v Azure Functions, je-li automaticky vygeneruje ID `id` je vynechán.

```json
{
    "id": "9f4ea53c5136457d883d685e57164f08"
}
```

## <a name="logger"></a>Protokoly

Ovládací prvky filtrování podle zapisují protokoly [objekt objektu ILogger](functions-monitoring.md#write-logs-in-c-functions) nebo [context.log](functions-monitoring.md#write-logs-in-javascript-functions).

```json
{
    "logger": {
        "categoryFilter": {
            "defaultLevel": "Information",
            "categoryLevels": {
                "Host": "Error",
                "Function": "Error",
                "Host.Aggregator": "Information"
            }
        }
    }
}
```

|Vlastnost  |Výchozí | Popis |
|---------|---------|---------| 
|categoryFilter|neuvedeno|Určuje filtrování podle kategorie| 
|defaultLevel|Informace|Pro všechny kategorie, nebyly zadány v `categoryLevels` pole, odeslání protokolů na této úrovni a vyšší Application Insights.| 
|categoryLevels|neuvedeno|Pole kategorií, které určuje úroveň minimální protokolu odeslat do služby Application Insights pro každou kategorii. Kategorie zde zadané řídí všechny kategorie, které začínají stejnou hodnotu a mají přednost před delší hodnoty. V předchozím příkladu *host.json* souboru, všechny kategorie, které začínají "Host.Aggregator" protokolu v `Information` úroveň. Přihlaste se na všech ostatních kategorií, které začínají "Hostitel", jako je například "Host.Executor" `Error` úroveň.| 

## <a name="queues"></a>Fronty

Nastavení konfigurace pro [úložiště fronty triggerů a vazeb](functions-bindings-storage-queue.md).

```json
{
    "queues": {
      "maxPollingInterval": 2000,
      "visibilityTimeout" : "00:00:30",
      "batchSize": 16,
      "maxDequeueCount": 5,
      "newBatchThreshold": 8
    }
}
```

|Vlastnost  |Výchozí | Popis |
|---------|---------|---------| 
|maxPollingInterval|60000|Maximální dobu v milisekundách mezi dvěma fronty.| 
|visibilityTimeout|0|Časový interval mezi opakovanými pokusy při zpracování zprávy, které se nezdaří.| 
|batchSize|16|Počet zpráv fronty k načtení a zpracovávají paralelně. Maximum je 32.| 
|maxDequeueCount|5|Počet pokusů, před přesunutím do fronty poškozených zpracování zprávy.| 
|newBatchThreshold|batchSize/2|Prahová hodnota, na kterém jsou načtených novou dávku zpráv.| 

## <a name="servicebus"></a>Sběrnice

Nastavení konfigurace pro [Service Bus triggerů a vazeb](functions-bindings-service-bus.md).

```json
{
    "serviceBus": {
      "maxConcurrentCalls": 16,
      "prefetchCount": 100,
      "autoRenewTimeout": "00:05:00"
    }
}
```

|Vlastnost  |Výchozí | Popis |
|---------|---------|---------| 
|maxConcurrentCalls|16|Maximální počet souběžných volání zpětné volání, které by měla iniciovat message pump. | 
|prefetchCount|neuvedeno|Výchozí hodnota PrefetchCount, který se použije základní MessageReceiver.| 
|autoRenewTimeout|00:05:00|Maximální doba, ve kterém bude automaticky obnoveno zámek zprávy.| 

## <a name="singleton"></a>singleton

Nastavení konfigurace pro jednotlivý prvek uzamčení chování. Další informace najdete v tématu [potíže Githubu o podpoře singleton](https://github.com/Azure/azure-webjobs-sdk-script/issues/912).

```json
    "singleton": {
      "lockPeriod": "00:00:15",
      "listenerLockPeriod": "00:01:00",
      "listenerLockRecoveryPollingInterval": "00:01:00",
      "lockAcquisitionTimeout": "00:01:00",
      "lockAcquisitionPollingInterval": "00:00:03"
    }
}
```

|Vlastnost  |Výchozí | Popis |
|---------|---------|---------| 
|lockPeriod|00:00:15|Dobu, po kterou jsou funkce úrovni zámky prováděné na. Zámky automatického obnovení.| 
|listenerLockPeriod|00:01:00|Období, které se provádějí zámky naslouchací proces pro.| 
|listenerLockRecoveryPollingInterval|00:01:00|Časový interval pro naslouchací proces obnovení zámku použit v případě, že při spuštění se nepodařilo získat zámek naslouchací proces.| 
|lockAcquisitionTimeout|00:01:00|Maximální množství času modulu runtime se pokusí získat zámek.| 
|lockAcquisitionPollingInterval|neuvedeno|Interval mezi pokusy o získání zámku.| 

## <a name="tracing"></a>trasování

Nastavení konfigurace pro protokoly, které vytvoříte pomocí `TraceWriter` objektu. V tématu [C# protokolování](functions-reference-csharp.md#logging) a [Node.js protokolování](functions-reference-node.md#writing-trace-output-to-the-console). 

```json
{
    "tracing": {
      "consoleLevel": "verbose",
      "fileLoggingMode": "debugOnly"
    }
}
```

|Vlastnost  |Výchozí | Popis |
|---------|---------|---------| 
|consoleLevel|Informace o|Úroveň trasování pro protokolování konzoly. Možnosti jsou: `off`, `error`, `warning`, `info`, a `verbose`.|
|fileLoggingMode|debugOnly|Úroveň trasování pro protokolování do souboru. Možnosti jsou `never`, `always`, `debugOnly`.| 

## <a name="watchdirectories"></a>watchDirectories

Sadu [sdíleného adresáře kód](functions-reference-csharp.md#watched-directories) , je nutné sledovat změny.  Zajišťuje, že při změně kódu v tyto adresáře změny jsou zachyceny pomocí funkcí.

```json
{
    "watchDirectories": [ "Shared" ]
}
```

## <a name="durabletask"></a>durableTask

[Úloha rozbočovače](durable-functions-task-hubs.md) název [trvanlivý funkce](durable-functions-overview.md).

```json
{
  "durableTask": {
    "HubName": "MyTaskHub"
  }
}
```

Úloha rozbočovače názvy musí začínat písmenem a obsahovat jenom písmena a čísla. Pokud není zadáno, je výchozí název rozbočovače úlohy pro funkce aplikace **DurableFunctionsHub**. Další informace najdete v tématu [úloh centra](durable-functions-task-hubs.md).


## <a name="next-steps"></a>Další kroky

> [!div class="nextstepaction"]
> [Zjistěte, jak aktualizovat soubor host.json](functions-reference.md#fileupdate)

> [!div class="nextstepaction"]
> [Najdete v části globální nastavení v seznamu proměnných prostředí](functions-app-settings.md)
