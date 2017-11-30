---
title: Azure Event Hubs vazby pro Azure Functions
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
ms.date: 11/08/2017
ms.author: wesmc
ms.openlocfilehash: 70219ada2f4886f40d088486063afda2bc489611
ms.sourcegitcommit: 29bac59f1d62f38740b60274cb4912816ee775ea
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/29/2017
---
# <a name="azure-event-hubs-bindings-for-azure-functions"></a>Azure Event Hubs vazby pro Azure Functions

Tento článek vysvětluje, jak pracovat s [Azure Event Hubs](../event-hubs/event-hubs-what-is-event-hubs.md) vazby pro Azure Functions. Azure Functions podporuje aktivaci a výstupní vazeb pro služby Event Hubs.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="trigger"></a>Trigger

Použijte aktivační událost Event Hubs reagovat na událost odeslaná datového proudu událostí centra událostí. Musíte mít přístup pro čtení do centra událostí vytvořit aktivační událost.

Když se aktivuje funkce aktivační událost Event Hubs, zprávu, která ji spouští je předán do funkce jako řetězec.

## <a name="trigger---example"></a>Aktivační událost – příklad

Podívejte se na konkrétní jazyk příklad:

* [Předkompilované C#](#trigger---c-example)
* [Skript jazyka C#](#trigger---c-script-example)
* [F#](#trigger---f-example)
* [JavaScript](#trigger---javascript-example)

### <a name="trigger---c-example"></a>Aktivační událost – příklad jazyka C#

Následující příklad ukazuje [předkompilovaných C#](functions-dotnet-class-library.md) kód, který protokoluje tělo zprávy aktivační události rozbočovače.

```csharp
[FunctionName("EventHubTriggerCSharp")]
public static void Run([EventHubTrigger("samples-workitems", Connection = "EventHubConnection")] string myEventHubMessage, TraceWriter log)
{
    log.Info($"C# Event Hub trigger function processed a message: {myEventHubMessage}");
}
```

Chcete-li získat přístup k metadatům událostí, vytvořit vazbu k [EventData](/dotnet/api/microsoft.servicebus.messaging.eventdata) objektu (vyžaduje `using` příkaz pro `Microsoft.ServiceBus.Messaging`).

```csharp
[FunctionName("EventHubTriggerCSharp")]
public static void Run([EventHubTrigger("samples-workitems", Connection = "EventHubConnection")] EventData myEventHubMessage, TraceWriter log)
{
    log.Info($"{Encoding.UTF8.GetString(myEventHubMessage.GetBytes())}");
}
```
Chcete-li přijímat události v dávce, ujistěte se, `string` nebo `EventData` pole:

```cs
[FunctionName("EventHubTriggerCSharp")]
public static void Run([EventHubTrigger("samples-workitems", Connection = "EventHubConnection")] string[] eventHubMessages, TraceWriter log)
{
    foreach (var message in eventHubMessages)
    {
        log.Info($"C# Event Hub trigger function processed a message: {message}");
    }
}
```

### <a name="trigger---c-script-example"></a>Aktivační událost – příklad skriptu jazyka C#

Následující příklad ukazuje vazby v aktivační signál události rozbočovače *function.json* souboru a [funkce skriptu jazyka C#](functions-reference-csharp.md) používající vazby. Funkce zaznamená tělo zprávy aktivační události rozbočovače.

Zde je vazba dat v *function.json* souboru:

```json
{
  "type": "eventHubTrigger",
  "name": "myEventHubMessage",
  "direction": "in",
  "path": "MyEventHub",
  "connection": "myEventHubReadConnectionString"
}
```
Tady je kód skriptu jazyka C#:

```cs
using System;

public static void Run(string myEventHubMessage, TraceWriter log)
{
    log.Info($"C# Event Hub trigger function processed a message: {myEventHubMessage}");
}
```

Pokud chcete získat přístup k metadatům událostí, vytvořit vazbu k [EventData](/dotnet/api/microsoft.servicebus.messaging.eventdata) objektu (vyžaduje, pomocí příkazu pro `Microsoft.ServiceBus.Messaging`).

```cs
#r "Microsoft.ServiceBus"
using System.Text;
using Microsoft.ServiceBus.Messaging;

public static void Run(EventData myEventHubMessage, TraceWriter log)
{
    log.Info($"{Encoding.UTF8.GetString(myEventHubMessage.GetBytes())}");
}
```

Chcete-li přijímat události v dávce, ujistěte se, `string` nebo `EventData` pole:

```cs
public static void Run(string[] eventHubMessages, TraceWriter log)
{
    foreach (var message in eventHubMessages)
    {
        log.Info($"C# Event Hub trigger function processed a message: {message}");
    }
}
```

### <a name="trigger---f-example"></a>Aktivační událost – příklad F #

Následující příklad ukazuje vazby v aktivační signál události rozbočovače *function.json* souboru a [funkce F #](functions-reference-fsharp.md) používající vazby. Funkce zaznamená tělo zprávy aktivační události rozbočovače.

Zde je vazba dat v *function.json* souboru:

```json
{
  "type": "eventHubTrigger",
  "name": "myEventHubMessage",
  "direction": "in",
  "path": "MyEventHub",
  "connection": "myEventHubReadConnectionString"
}
```

Tady je kód F #:

```fsharp
let Run(myEventHubMessage: string, log: TraceWriter) =
    log.Info(sprintf "F# eventhub trigger function processed work item: %s" myEventHubMessage)
```

### <a name="trigger---javascript-example"></a>Aktivační událost – příklad v jazyce JavaScript

Následující příklad ukazuje vazby v aktivační signál události rozbočovače *function.json* souboru a [funkce JavaScript, která](functions-reference-node.md) používající vazby. Funkce zaznamená tělo zprávy aktivační události rozbočovače.

Zde je vazba dat v *function.json* souboru:

```json
{
  "type": "eventHubTrigger",
  "name": "myEventHubMessage",
  "direction": "in",
  "path": "MyEventHub",
  "connection": "myEventHubReadConnectionString"
}
```

Tady je kód jazyka JavaScript:

```javascript
module.exports = function (context, myEventHubMessage) {
    context.log('Node.js eventhub trigger function processed work item', myEventHubMessage);    
    context.done();
};
```

## <a name="trigger---attributes"></a>Aktivační událost – atributy

Pro [předkompilovaných C#](functions-dotnet-class-library.md) používat funkce, [EventHubTriggerAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/EventHubs/EventHubTriggerAttribute.cs) atribut, který je definován v balíčku NuGet [Microsoft.Azure.WebJobs.ServiceBus](http://www.nuget.org/packages/Microsoft.Azure.WebJobs.ServiceBus).

Konstruktoru atributu přebírá název centra událostí, název skupiny uživatelů a název nastavení aplikace, který obsahuje připojovací řetězec. Další informace o těchto nastaveních najdete v tématu [aktivovat konfigurační oddíl](#trigger---configuration). Tady je `EventHubTriggerAttribute` atribut příklad:

```csharp
[FunctionName("EventHubTriggerCSharp")]
public static void Run([EventHubTrigger("samples-workitems", Connection = "EventHubConnection")] string myEventHubMessage, TraceWriter log)
{
    ...
}
```

Úplný příklad najdete v tématu [aktivační událost - předkompilovaných C# příklad](#trigger---c-example).

## <a name="trigger---configuration"></a>Aktivační událost - konfigurace

Následující tabulka popisuje vlastnosti konfigurace vazby, které jste nastavili v *function.json* souboru a `EventHubTrigger` atribut.

|Vlastnost Function.JSON | Vlastnost atributu |Popis|
|---------|---------|----------------------|
|**Typ** | neuvedeno | musí být nastavena na `eventHubTrigger`. Tato vlastnost nastavena automaticky při vytváření aktivační události na portálu Azure.|
|**směr** | neuvedeno | musí být nastavena na `in`. Tato vlastnost nastavena automaticky při vytváření aktivační události na portálu Azure. |
|**Jméno** | neuvedeno | Název proměnné, která představuje položku událostí v kódu funkce. | 
|**Cesta** |**EventHubName** | Název centra událostí. | 
|**možnost consumerGroup** |**Možnost ConsumerGroup** | Volitelná vlastnost, která nastavuje [skupiny příjemců](../event-hubs/event-hubs-features.md#event-consumers) používá přihlásit k odběru událostí v centru. Pokud tento parametr vynechán, `$Default` skupina uživatelů slouží. | 
|**připojení** |**Připojení** | Název nastavení aplikace, který obsahuje připojovací řetězec k Centru událostí oboru názvů. Zkopírujte tento připojovací řetězec kliknutím **informace o připojení** tlačítko pro *obor názvů*, ne samotný centra událostí. Tento připojovací řetězec musí mít alespoň oprávnění ke čtení pro aktivační událost.|

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="trigger---hostjson-properties"></a>Aktivační událost - host.json vlastnosti

[Host.json](functions-host-json.md#eventhub) soubor obsahuje nastavení, které řídí chování aktivační událost Event Hubs.

[!INCLUDE [functions-host-json-event-hubs](../../includes/functions-host-json-event-hubs.md)]

## <a name="output"></a>Výstup

Použijte službu Event Hubs výstup vytvoření vazby na zapsat události do datového proudu událostí. Musíte mít oprávnění odesílat do centra událostí zapsat události do ní.

## <a name="output---example"></a>Výstup – příklad

Podívejte se na konkrétní jazyk příklad:

* [Předkompilované C#](#output---c-example)
* [Skript jazyka C#](#output---c-script-example)
* [F#](#output---f-example)
* [JavaScript](#output---javascript-example)

### <a name="output---c-example"></a>Výstup – příklad jazyka C#

Následující příklad ukazuje [předkompilovaných C# funkce](functions-dotnet-class-library.md) , zapíše zprávu do centra událostí, pomocí návratovou hodnotu metody jako výstup:

```csharp
[FunctionName("EventHubOutput")]
[return: EventHub("outputEventHubMessage", Connection = "EventHubConnection")]
public static string Run([TimerTrigger("0 */5 * * * *")] TimerInfo myTimer, TraceWriter log)
{
    log.Info($"C# Timer trigger function executed at: {DateTime.Now}");
    return $"{DateTime.Now}";
}
```

### <a name="output---c-script-example"></a>Výstup – příklad skriptu jazyka C#

Následující příklad ukazuje vazby v aktivační signál události rozbočovače *function.json* souboru a [funkce skriptu jazyka C#](functions-reference-csharp.md) používající vazby. Funkce zapíše zprávu do centra událostí.

Zde je vazba dat v *function.json* souboru:

```json
{
    "type": "eventHub",
    "name": "outputEventHubMessage",
    "path": "myeventhub",
    "connection": "MyEventHubSend",
    "direction": "out"
}
```

Tady je C# kód skriptu, který vytvoří jednu zprávu:

```cs
using System;

public static void Run(TimerInfo myTimer, out string outputEventHubMessage, TraceWriter log)
{
    String msg = $"TimerTriggerCSharp1 executed at: {DateTime.Now}";
    log.Verbose(msg);   
    outputEventHubMessage = msg;
}
```

Zde uvádíme C# kód skriptu, který vytvoří více zpráv:

```cs
public static void Run(TimerInfo myTimer, ICollector<string> outputEventHubMessage, TraceWriter log)
{
    string message = $"Event Hub message created at: {DateTime.Now}";
    log.Info(message);
    outputEventHubMessage.Add("1 " + message);
    outputEventHubMessage.Add("2 " + message);
}
```

### <a name="output---f-example"></a>Výstup – příklad F #

Následující příklad ukazuje vazby v aktivační signál události rozbočovače *function.json* souboru a [funkce F #](functions-reference-fsharp.md) používající vazby. Funkce zapíše zprávu do centra událostí.

Zde je vazba dat v *function.json* souboru:

```json
{
    "type": "eventHub",
    "name": "outputEventHubMessage",
    "path": "myeventhub",
    "connection": "MyEventHubSend",
    "direction": "out"
}
```

Tady je kód F #:

```fsharp
let Run(myTimer: TimerInfo, outputEventHubMessage: byref<string>, log: TraceWriter) =
    let msg = sprintf "TimerTriggerFSharp1 executed at: %s" DateTime.Now.ToString()
    log.Verbose(msg);
    outputEventHubMessage <- msg;
```

### <a name="output---javascript-example"></a>Výstup – příklad v jazyce JavaScript

Následující příklad ukazuje vazby v aktivační signál události rozbočovače *function.json* souboru a [funkce JavaScript, která](functions-reference-node.md) používající vazby. Funkce zapíše zprávu do centra událostí.

Zde je vazba dat v *function.json* souboru:

```json
{
    "type": "eventHub",
    "name": "outputEventHubMessage",
    "path": "myeventhub",
    "connection": "MyEventHubSend",
    "direction": "out"
}
```

Zde uvádíme kódu jazyka JavaScript, který odesílá do jedné zprávy:

```javascript
module.exports = function (context, myTimer) {
    var timeStamp = new Date().toISOString();
    context.log('Event Hub message created at: ', timeStamp);   
    context.bindings.outputEventHubMessage = "Event Hub message created at: " + timeStamp;
    context.done();
};
```

Zde uvádíme kódu jazyka JavaScript, který odesílá více zpráv:

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

## <a name="output---attributes"></a>Výstup – atributy

Pro [předkompilovaných C#](functions-dotnet-class-library.md) používat funkce, [EventHubAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/EventHubs/EventHubAttribute.cs) atribut, který je definován v balíčku NuGet [Microsoft.Azure.WebJobs.ServiceBus](http://www.nuget.org/packages/Microsoft.Azure.WebJobs.ServiceBus).

Konstruktoru atributu přebírá název centra událostí a název nastavení aplikace, který obsahuje připojovací řetězec. Další informace o těchto nastaveních najdete v tématu [výstup - konfigurace](#output---configuration). Tady je `EventHub` atribut příklad:

```csharp
[FunctionName("EventHubOutput")]
[return: EventHub("outputEventHubMessage", Connection = "EventHubConnection")]
public static string Run([TimerTrigger("0 */5 * * * *")] TimerInfo myTimer, TraceWriter log)
{
    ...
}
```

Úplný příklad najdete v tématu [výstup - předkompilovaných C# příklad](#output---c-example).

## <a name="output---configuration"></a>Výstup – konfigurace

Následující tabulka popisuje vlastnosti konfigurace vazby, které jste nastavili v *function.json* souboru a `EventHub` atribut.

|Vlastnost Function.JSON | Vlastnost atributu |Popis|
|---------|---------|----------------------|
|**Typ** | neuvedeno | Musí být nastavena na "eventHub". |
|**směr** | neuvedeno | Musí být nastavena na "out". Tento parametr je nastaven automaticky při vytvoření vazby na portálu Azure. |
|**Jméno** | neuvedeno | Název proměnné používá v kódu funkce, která představuje událost. | 
|**Cesta** |**EventHubName** | Název centra událostí. | 
|**připojení** |**Připojení** | Název nastavení aplikace, který obsahuje připojovací řetězec k Centru událostí oboru názvů. Zkopírujte tento připojovací řetězec kliknutím **informace o připojení** tlačítko pro *obor názvů*, ne samotný centra událostí. Tento připojovací řetězec musí mít oprávnění pro odesílání k odeslání zprávy do datového proudu událostí.|

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="output---usage"></a>Výstup – použití

V jazyce C# a C# skript, odesílání zpráv pomocí parametru metody `out string paramName`. V jazyce C# skript `paramName` je hodnota zadaná v `name` vlastnost *function.json*. Zápis více zpráv, můžete použít `ICollector<string>` nebo `IAsyncCollector<string>` místě `out string`.

V jazyce JavaScript, přístup k výstupu událostí pomocí `context.bindings.<name>`. `<name>`Hodnota zadaná v `name` vlastnost *function.json*.

## <a name="next-steps"></a>Další kroky

> [!div class="nextstepaction"]
> [Další informace o Azure functions triggerů a vazeb](functions-triggers-bindings.md)
