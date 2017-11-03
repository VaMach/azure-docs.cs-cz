---
title: "Azure Service Bus funkce triggerů a vazeb | Microsoft Docs"
description: "Pochopit, jak používat Azure Service Bus triggerů a vazeb v Azure Functions."
services: functions
documentationcenter: na
author: christopheranderson
manager: cfowler
editor: 
tags: 
keywords: "Funkce Azure, funkce zpracování událostí, dynamické výpočetní architektura bez serveru"
ms.assetid: daedacf0-6546-4355-a65c-50873e74f66b
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 04/01/2017
ms.author: glenga
ms.openlocfilehash: 71149aaacc940a62e085cf1ce103a0214d05bd1c
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="azure-functions-service-bus-bindings"></a>Azure Service Bus funkce vazby
[!INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

Tento článek vysvětluje postup konfigurace a práce s vazeb Azure Service Bus v Azure Functions. 

Azure Functions podporuje aktivaci a výstupní vazby pro fronty sběrnice a témata.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

<a name="trigger"></a>

## <a name="service-bus-trigger"></a>Aktivace služby Service Bus
Použijte aktivační událost Service Bus reagovat na zprávy z fronty sběrnice nebo téma. 

Následující objekty JSON v jsou definovány aktivační události pro frontu a téma sběrnice `bindings` pole function.json:

* *fronty* aktivační události:

    ```json
    {
        "name" : "<Name of input parameter in function signature>",
        "queueName" : "<Name of the queue>",
        "connection" : "<Name of app setting that has your queue's connection string - see below>",
        "accessRights" : "<Access rights for the connection string - see below>",
        "type" : "serviceBusTrigger",
        "direction" : "in"
    }
    ```

* *téma* aktivační události:

    ```json
    {
        "name" : "<Name of input parameter in function signature>",
        "topicName" : "<Name of the topic>",
        "subscriptionName" : "<Name of the subscription>",
        "connection" : "<Name of app setting that has your topic's connection string - see below>",
        "accessRights" : "<Access rights for the connection string - see below>",
        "type" : "serviceBusTrigger",
        "direction" : "in"
    }
    ```

Je třeba počítat s následujícím:

* Pro `connection`, [vytvoření nastavení aplikace v aplikaci funkce](functions-how-to-use-azure-function-app-settings.md) obsahující připojovací řetězec k oboru názvů Service Bus, pak zadejte název nastavení aplikace v `connection` vlastnost aktivační událost. Získat připojovací řetězec pomocí následujících kroků, zobrazuje se v [získat přihlašovací údaje správu](../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md#obtain-the-management-credentials).
  Připojovací řetězec musí být v případě oboru názvů Service Bus, bez omezení konkrétní fronty nebo téma.
  Pokud necháte `connection` prázdná, aktivační událost se předpokládá, že je zadán připojovací řetězec sběrnice výchozí v aplikaci nastavení s názvem `AzureWebJobsServiceBus`.
* Pro `accessRights`, dostupné jsou hodnoty `manage` a `listen`. Výchozí hodnota je `manage`, což znamená, že `connection` má **spravovat** oprávnění. Pokud použijete připojovací řetězec, který nemá **spravovat** nastavit oprávnění, `accessRights` k `listen`. Funkce modulu runtime může selhat pokouší o provedení operace, které vyžadují, jinak hodnota spravovat práva.

## <a name="trigger-behavior"></a>Aktivační událost chování
* **Dělení na vlákna jedním** – ve výchozím nastavení funkce procesy runtime více zpráv současně. Přímé modulu runtime zpracovat jenom jeden fronta nebo téma zprávy najednou, nastavte `serviceBus.maxConcurrentCalls` 1 v *host.json*. 
  Informace o *host.json*, najdete v části [struktura složek](functions-reference.md#folder-structure) a [host.json](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json).
* **Zpracování škodlivých zpráv** -Service Bus nepodporuje svůj vlastní zpracování poškozených zpráv, které nelze řídí nebo nakonfigurované v Azure Functions konfigurace nebo kódu. 
* **Chování PeekLock** – modul runtime Functions přijme nějakou zprávu v [ `PeekLock` režimu](../service-bus-messaging/service-bus-performance-improvements.md#receive-mode) a volání `Complete` na zprávu, pokud funkci skončí úspěšně, nebo volání `Abandon` Pokud funkce se nezdaří. 
  Pokud je funkce spuštěná déle, než `PeekLock` automaticky obnovují vždy vypršel časový limit, zámek.

<a name="triggerusage"></a>

## <a name="trigger-usage"></a>Aktivační události využití
V této části se dozvíte, jak používat vaše aktivační události služby Service Bus ve vašem kódu funkce. 

V jazyce C# a F # lze deserializovat zpráva aktivační události služby Service Bus ke kterékoli z následujících typů vstupu:

* `string`-užitečné pro řetězec zprávy
* `byte[]`-vhodné pro binární data
* Všechny [objekt](https://msdn.microsoft.com/library/system.object.aspx) – vhodné pro data serializací JSON.
  Pokud je deklarovat vlastní typ vstupu, například `CustomType`, Azure Functions se pokusí rekonstruovat JSON data do zadaného typu.
* `BrokeredMessage`-vám deserializovat zprávu s [BrokeredMessage.GetBody<T>()](https://msdn.microsoft.com/library/hh144211.aspx) metoda.

V Node.js aktivační událost zprávy služby Service Bus je předán do funkce jako řetězec nebo objekt JSON.

<a name="triggersample"></a>

## <a name="trigger-sample"></a>Ukázka aktivační události
Předpokládejme, že máte následující function.json:

```json
{
"bindings": [
    {
    "queueName": "testqueue",
    "connection": "MyServiceBusConnection",
    "name": "myQueueItem",
    "type": "serviceBusTrigger",
    "direction": "in"
    }
],
"disabled": false
}
```

Naleznete v ukázce pro specifický jazyk, který zpracuje zprávu fronty Service Bus.

* [C#](#triggercsharp)
* [F#](#triggerfsharp)
* [Node.js](#triggernodejs)

<a name="triggercsharp"></a>

### <a name="trigger-sample-in-c"></a>Ukázka aktivační události v jazyce C# #

```cs
public static void Run(string myQueueItem, TraceWriter log)
{
    log.Info($"C# ServiceBus queue trigger function processed message: {myQueueItem}");
}
```

<a name="triggerfsharp"></a>

### <a name="trigger-sample-in-f"></a>Ukázka aktivační události v jazyce F # #

```fsharp
let Run(myQueueItem: string, log: TraceWriter) =
    log.Info(sprintf "F# ServiceBus queue trigger function processed message: %s" myQueueItem)
```

<a name="triggernodejs"></a>

### <a name="trigger-sample-in-nodejs"></a>Ukázka aktivační události v Node.js

```javascript
module.exports = function(context, myQueueItem) {
    context.log('Node.js ServiceBus queue trigger function processed message', myQueueItem);
    context.done();
};
```

<a name="output"></a>

## <a name="service-bus-output-binding"></a>Service Bus výstup vazby
Výstup frontu a téma sběrnice pro funkci použití následujících objektů JSON `bindings` pole function.json:

* *fronty* výstup:

    ```json
    {
        "name" : "<Name of output parameter in function signature>",
        "queueName" : "<Name of the queue>",
        "connection" : "<Name of app setting that has your queue's connection string - see below>",
        "accessRights" : "<Access rights for the connection string - see below>",
        "type" : "serviceBus",
        "direction" : "out"
    }
    ```
* *téma* výstup:

    ```json
    {
        "name" : "<Name of output parameter in function signature>",
        "topicName" : "<Name of the topic>",
        "subscriptionName" : "<Name of the subscription>",
        "connection" : "<Name of app setting that has your topic's connection string - see below>",
        "accessRights" : "<Access rights for the connection string - see below>",
        "type" : "serviceBus",
        "direction" : "out"
    }
    ```

Je třeba počítat s následujícím:

* Pro `connection`, [vytvoření nastavení aplikace v aplikaci funkce](functions-how-to-use-azure-function-app-settings.md) obsahující připojovací řetězec k oboru názvů Service Bus, pak zadejte název nastavení aplikace v `connection` vlastnost Výstupní vazba. Získat připojovací řetězec pomocí následujících kroků, zobrazuje se v [získat přihlašovací údaje správu](../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md#obtain-the-management-credentials).
  Připojovací řetězec musí být v případě oboru názvů Service Bus, bez omezení konkrétní fronty nebo téma.
  Pokud necháte `connection` prázdná, vazba výstup předpokládá, že připojovací řetězec sběrnice výchozí je zadán v aplikaci nastavení s názvem `AzureWebJobsServiceBus`.
* Pro `accessRights`, dostupné jsou hodnoty `manage` a `listen`. Výchozí hodnota je `manage`, což znamená, že `connection` má **spravovat** oprávnění. Pokud použijete připojovací řetězec, který nemá **spravovat** nastavit oprávnění, `accessRights` k `listen`. Funkce modulu runtime může selhat pokouší o provedení operace, které vyžadují, jinak hodnota spravovat práva.

<a name="outputusage"></a>

## <a name="output-usage"></a>Využití výstupní
V jazyce C# a F # můžete Azure Functions vytvořit zprávu fronty Service Bus z libovolného z následujících typů:

* Všechny [objekt](https://msdn.microsoft.com/library/system.object.aspx) -definici parametru vypadá jako `out T paramName` (C#).
  Funkce deserializuje objekt do formátu JSON zprávy. Pokud jsou výstupní hodnotu null při ukončení funkce, funkce vytvoří zprávu s objektem hodnotu null.
* `string`-Vypadá parametr definice `out string paraName` (C#). Pokud je hodnota parametru jinou hodnotu než null při ukončení funkce, funkce vytvoří zprávu.
* `byte[]`-Vypadá parametr definice `out byte[] paraName` (C#). Pokud je hodnota parametru jinou hodnotu než null při ukončení funkce, funkce vytvoří zprávu.
* `BrokeredMessage`Definici parametru vypadá jako `out BrokeredMessage paraName` (C#). Pokud je hodnota parametru jinou hodnotu než null při ukončení funkce, funkce vytvoří zprávu.

Pro vytvoření více zpráv ve funkci jazyka C#, můžete použít `ICollector<T>` nebo `IAsyncCollector<T>`. Zprávu se vytvoří při volání `Add` metoda.

V Node.js, můžete přiřadit řetězec, bajtové pole nebo objekt jazyka Javascript (deserializovat do formátu JSON) na `context.binding.<paramName>`.

<a name="outputsample"></a>

## <a name="output-sample"></a>Ukázkový výstup
Předpokládejme, že máte následující function.json, která definuje výstupní fronty Service Bus:

```json
{
    "bindings": [
        {
            "schedule": "0/15 * * * * *",
            "name": "myTimer",
            "runsOnStartup": true,
            "type": "timerTrigger",
            "direction": "in"
        },
        {
            "name": "outputSbQueue",
            "type": "serviceBus",
            "queueName": "testqueue",
            "connection": "MyServiceBusConnection",
            "direction": "out"
        }
    ],
    "disabled": false
}
```

Naleznete v ukázce pro specifický jazyk, který odešle zprávu do fronty service bus.

* [C#](#outcsharp)
* [F#](#outfsharp)
* [Node.js](#outnodejs)

<a name="outcsharp"></a>

### <a name="output-sample-in-c"></a>Ukázka výstupu v jazyce C# #

```cs
public static void Run(TimerInfo myTimer, TraceWriter log, out string outputSbQueue)
{
    string message = $"Service Bus queue message created at: {DateTime.Now}";
    log.Info(message); 
    outputSbQueue = message;
}
```

Nebo, chcete-li vytvořit více zpráv:

```cs
public static void Run(TimerInfo myTimer, TraceWriter log, ICollector<string> outputSbQueue)
{
    string message = $"Service Bus queue message created at: {DateTime.Now}";
    log.Info(message); 
    outputSbQueue.Add("1 " + message);
    outputSbQueue.Add("2 " + message);
}
```

<a name="outfsharp"></a>

### <a name="output-sample-in-f"></a>Ukázka výstupu v jazyce F # #

```fsharp
let Run(myTimer: TimerInfo, log: TraceWriter, outputSbQueue: byref<string>) =
    let message = sprintf "Service Bus queue message created at: %s" (DateTime.Now.ToString())
    log.Info(message)
    outputSbQueue = message
```

<a name="outnodejs"></a>

### <a name="output-sample-in-nodejs"></a>Ukázka výstupu v Node.js

```javascript
module.exports = function (context, myTimer) {
    var message = 'Service Bus queue message created at ' + timeStamp;
    context.log(message);   
    context.bindings.outputSbQueueMsg = message;
    context.done();
};
```

Nebo, chcete-li vytvořit více zpráv:

```javascript
module.exports = function (context, myTimer) {
    var message = 'Service Bus queue message created at ' + timeStamp;
    context.log(message);   
    context.bindings.outputSbQueueMsg = [];
    context.bindings.outputSbQueueMsg.push("1 " + message);
    context.bindings.outputSbQueueMsg.push("2 " + message);
    context.done();
};
```

## <a name="next-steps"></a>Další kroky
[!INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]

