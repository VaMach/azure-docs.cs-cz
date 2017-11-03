---
title: Azure Event Hubs funkce vazby | Microsoft Docs
description: "Pochopit, jak používat Azure Event Hubs vazby v Azure Functions."
services: functions
documentationcenter: na
author: wesmc7777
manager: cfowler
editor: 
tags: 
keywords: "Funkce Azure, funkce zpracování událostí, dynamické výpočetní architektura bez serveru"
ms.assetid: daf81798-7acc-419a-bc32-b5a41c6db56b
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 06/20/2017
ms.author: wesmc
ms.openlocfilehash: 85eb6985ef3579b1b2313db3ce5f91c3471da72f
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="azure-functions-event-hubs-bindings"></a>Azure Event Hubs funkce vazby
[!INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

Tento článek vysvětluje, jak konfigurovat a používat [Azure Event Hubs](../event-hubs/event-hubs-what-is-event-hubs.md) vazby pro Azure Functions.
Azure Functions podporuje aktivaci a výstupní vazeb pro služby Event Hubs.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

Pokud nový Azure Event Hubs, podívejte se [Přehled služby Event Hubs](../event-hubs/event-hubs-what-is-event-hubs.md).

<a name="trigger"></a>

## <a name="event-hub-trigger"></a>Aktivační událost rozbočovače
Použijte aktivační událost Event Hubs reagovat na událost odeslaná datového proudu událostí centra událostí. Musíte mít přístup pro čtení do centra událostí vytvořit aktivační událost.

Aktivační událost Event Hubs funkce používá následující objekt JSON v `bindings` pole function.json:

```json
{
    "type": "eventHubTrigger",
    "name": "<Name of trigger parameter in function signature>",
    "direction": "in",
    "path": "<Name of the event hub>",
    "consumerGroup": "Consumer group to use - see below",
    "connection": "<Name of app setting with connection string - see below>"
}
```

`consumerGroup`je volitelná vlastnost lze nastavit [skupiny příjemců](../event-hubs/event-hubs-features.md#event-consumers) používá přihlásit k odběru událostí v centru. Pokud tento parametr vynechán, `$Default` skupina uživatelů slouží.  
`connection`musí být název nastavení aplikace, který obsahuje připojovací řetězec k Centru událostí oboru názvů.
Zkopírujte tento připojovací řetězec kliknutím **informace o připojení** tlačítko pro *obor názvů*, ne samotný centra událostí. Tento připojovací řetězec musí mít alespoň oprávnění ke čtení pro aktivační událost.

[Další nastavení](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json) lze zadat do souboru host.json další vyladění aktivační události Event Hubs.  

<a name="triggerusage"></a>

## <a name="trigger-usage"></a>Aktivační události využití
Když se aktivuje funkce aktivační událost Event Hubs, zprávu, která ji spouští je předán do funkce jako řetězec.

<a name="triggersample"></a>

## <a name="trigger-sample"></a>Ukázka aktivační události
Předpokládejme, že máte následující služby Event Hubs aktivovat v `bindings` pole function.json:

```json
{
  "type": "eventHubTrigger",
  "name": "myEventHubMessage",
  "direction": "in",
  "path": "MyEventHub",
  "connection": "myEventHubReadConnectionString"
}
```

Naleznete v ukázce pro specifický jazyk, který zaznamenává tělo zprávy aktivační události rozbočovače.

* [C#](#triggercsharp)
* [F#](#triggerfsharp)
* [Node.js](#triggernodejs)

<a name="triggercsharp"></a>

### <a name="trigger-sample-in-c"></a>Ukázka aktivační události v jazyce C# #

```cs
using System;

public static void Run(string myEventHubMessage, TraceWriter log)
{
    log.Info($"C# Event Hub trigger function processed a message: {myEventHubMessage}");
}
```

Můžete také získat událost jako [EventData](/dotnet/api/microsoft.servicebus.messaging.eventdata) objekt, který umožňuje přístup k metadatům událostí.

```cs
#r "Microsoft.ServiceBus"
using System.Text;
using Microsoft.ServiceBus.Messaging;

public static void Run(EventData myEventHubMessage, TraceWriter log)
{
    log.Info($"{Encoding.UTF8.GetString(myEventHubMessage.GetBytes())}");
}
```

Chcete-li přijímat události v dávce, změňte podpis metody k `string[]` nebo `EventData[]`.

```cs
public static void Run(string[] eventHubMessages, TraceWriter log)
{
    foreach (var message in eventHubMessages)
    {
        log.Info($"C# Event Hub trigger function processed a message: {message}");
    }
}
```

<a name="triggerfsharp"></a>

### <a name="trigger-sample-in-f"></a>Ukázka aktivační události v jazyce F # #

```fsharp
let Run(myEventHubMessage: string, log: TraceWriter) =
    log.Info(sprintf "F# eventhub trigger function processed work item: %s" myEventHubMessage)
```

<a name="triggernodejs"></a>

### <a name="trigger-sample-in-nodejs"></a>Ukázka aktivační události v Node.js

```javascript
module.exports = function (context, myEventHubMessage) {
    context.log('Node.js eventhub trigger function processed work item', myEventHubMessage);    
    context.done();
};
```

<a name="output"></a>

## <a name="event-hubs-output-binding"></a>Služba Event Hubs výstup vazby
Použijte službu Event Hubs výstup vytvoření vazby na zapsat události do datového proudu událostí centra událostí. Musíte mít oprávnění odesílat do centra událostí zapsat události do ní.

Vazba výstup používá následující objekt JSON v `bindings` pole function.json:

```json
{
    "type": "eventHub",
    "name": "<Name of output parameter in function signature>",
    "path": "<Name of event hub>",
    "connection": "<Name of app setting with connection string - see below>"
    "direction": "out"
}
```

`connection`musí být název nastavení aplikace, který obsahuje připojovací řetězec k Centru událostí oboru názvů.
Zkopírujte tento připojovací řetězec kliknutím **informace o připojení** tlačítko pro *obor názvů*, ne samotný centra událostí. Tento připojovací řetězec musí mít oprávnění pro odesílání k odeslání zprávy do datového proudu událostí.

## <a name="output-usage"></a>Využití výstupní
V této části se dozvíte, jak používat službu Event Hubs výstupu vazby v kódu funkce.

Výstup můžete zprávy do centra událostí nakonfigurované s následujícími typy parametrů:

* `out string`
* `ICollector<string>`(pro výstup více zpráv)
* `IAsyncCollector<string>`(asynchronní verzi `ICollector<T>`)

<a name="outputsample"></a>

## <a name="output-sample"></a>Ukázkový výstup
Předpokládejme, že máte následující služby Event Hubs výstup vazby v `bindings` pole function.json:

```json
{
    "type": "eventHub",
    "name": "outputEventHubMessage",
    "path": "myeventhub",
    "connection": "MyEventHubSend",
    "direction": "out"
}
```

Naleznete v ukázce konkrétní jazyk, který zapíše se událost do i datový proud.

* [C#](#outcsharp)
* [F#](#outfsharp)
* [Node.js](#outnodejs)

<a name="outcsharp"></a>

### <a name="output-sample-in-c"></a>Ukázka výstupu v jazyce C# #

```cs
using System;

public static void Run(TimerInfo myTimer, out string outputEventHubMessage, TraceWriter log)
{
    String msg = $"TimerTriggerCSharp1 executed at: {DateTime.Now}";
    log.Verbose(msg);   
    outputEventHubMessage = msg;
}
```

Nebo, chcete-li vytvořit více zpráv:

```cs
public static void Run(TimerInfo myTimer, ICollector<string> outputEventHubMessage, TraceWriter log)
{
    string message = $"Event Hub message created at: {DateTime.Now}";
    log.Info(message);
    outputEventHubMessage.Add("1 " + message);
    outputEventHubMessage.Add("2 " + message);
}
```

<a name="outfsharp"></a>

### <a name="output-sample-in-f"></a>Ukázka výstupu v jazyce F # #

```fsharp
let Run(myTimer: TimerInfo, outputEventHubMessage: byref<string>, log: TraceWriter) =
    let msg = sprintf "TimerTriggerFSharp1 executed at: %s" DateTime.Now.ToString()
    log.Verbose(msg);
    outputEventHubMessage <- msg;
```

<a name="outnodejs"></a>

### <a name="output-sample-for-nodejs"></a>Ukázka výstupu pro Node.js

```javascript
module.exports = function (context, myTimer) {
    var timeStamp = new Date().toISOString();
    context.log('Event Hub message created at: ', timeStamp);   
    context.bindings.outputEventHubMessage = "Event Hub message created at: " + timeStamp;
    context.done();
};
```

Nebo, pokud chcete odeslat více zpráv

```javascript
module.exports = function(context) {
    var timeStamp = new Date().toISOString();
    var message = 'Event Hub message created at: ' + timeStamp;

    context.bindings.outputEventHubMessage = [];

    context.bindings.outputEventHubMessage.push("1 " + message);
    context.bindings.outputEventHubMessage.push("2 " + message);
    context.done();
};
```

## <a name="next-steps"></a>Další kroky
[!INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]
