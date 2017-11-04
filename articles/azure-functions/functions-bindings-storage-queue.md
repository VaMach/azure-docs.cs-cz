---
title: "Azure Functions fronty úložiště vazby | Microsoft Docs"
description: "Pochopit, jak používat Azure Storage triggerů a vazeb v Azure Functions."
services: functions
documentationcenter: na
author: ggailey777
manager: cfowler
editor: 
tags: 
keywords: "Funkce Azure, funkce zpracování událostí, dynamické výpočetní architektura bez serveru"
ms.assetid: 4e6a837d-e64f-45a0-87b7-aa02688a75f3
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 05/30/2017
ms.author: glenga
ms.openlocfilehash: b68ce106ceb25d19ee0bbde287891d553a448560
ms.sourcegitcommit: 963e0a2171c32903617d883bb1130c7c9189d730
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/20/2017
---
# <a name="azure-functions-queue-storage-bindings"></a>Azure Queue Storage funkce vazby
[!INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

Tento článek popisuje, jak nakonfigurovat a vazby Azure Queue storage kódu v Azure Functions. Azure Functions podporuje aktivaci a výstupní vazby pro Azure fronty. Funkce, které jsou k dispozici v všechny vazby, najdete v části [Azure Functions triggerů a vazeb koncepty](functions-triggers-bindings.md).

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

<a name="trigger"></a>

## <a name="queue-storage-trigger"></a>Aktivace fronty úložiště
Aktivační událost Azure Queue storage vám umožňuje monitorovat fronty úložiště pro nové zprávy a reagovat na ně. 

Zadejte aktivační události fronty pomocí **integrací** na portálu funkce. Vytvoří následující definice v portálu **vazby** části *function.json*:

```json
{
    "type": "queueTrigger",
    "direction": "in",
    "name": "<The name used to identify the trigger data in your code>",
    "queueName": "<Name of queue to poll>",
    "connection":"<Name of app setting - see below>"
}
```

* `connection` Vlastnost musí obsahovat název nastavení aplikace, který obsahuje připojovací řetězec úložiště. Na portálu Azure, standardního editoru v **integrací** kartě nakonfiguruje toto nastavení aplikace pro vás, když vyberete účet úložiště.

Další nastavení lze zadat do [host.json soubor](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json) a vyladit aktivační procedury fronty úložiště. Můžete například změnit fronty v host.json interval dotazování.

<a name="triggerusage"></a>

## <a name="using-a-queue-trigger"></a>Pomocí aktivační procedury fronty
Ve funkcích Node.js, přístup k frontě dat pomocí `context.bindings.<name>`.


V rozhraní .NET funkce, přístup k datové části fronty pomocí parametru metody, jako třeba `CloudQueueMessage paramName`. Zde `paramName` je hodnota zadaná v [konfigurace aktivační události](#trigger). Zprávy ve frontě lze deserializovat na některý z následujících typů:

* Objektů POCO objekt. Použijte, pokud datová část fronty je objekt JSON. Modul runtime funkce deserializuje datovou část do objektů POCO objektu. 
* `string`
* `byte[]`
* [`CloudQueueMessage`]

<a name="meta"></a>

### <a name="queue-trigger-metadata"></a>Metadata frontě aktivační události
Aktivační událost fronty nabízí několik vlastností metadat. Tyto vlastnosti lze použít jako součást vazby výrazy v jiných vazby nebo jako parametry v kódu. Hodnoty mají stejnou sémantiku jako [ `CloudQueueMessage` ].

* **QueueTrigger** -datové frontu (pokud platný řetězec)
* **DequeueCount** – typ `int`. Počet časy, kdy se tato zpráva má bylo vyřazeno z fronty.
* **ExpirationTime** – typ `DateTimeOffset?`. Čas, kdy vyprší platnost zprávy.
* **ID** – typ `string`. ID zprávy fronty
* **InsertionTime** – typ `DateTimeOffset?`. Doba, která zpráva byla přidána do fronty.
* **NextVisibleTime** – typ `DateTimeOffset?`. Čas, zprávy budou vedle viditelné.
* **Vlastnosti PopReceipt** – typ `string`. Pop přijetí zprávy.

V tématu Jak používat fronty metadat v [aktivační událost vzorku](#triggersample).

<a name="triggersample"></a>

## <a name="trigger-sample"></a>Ukázka aktivační události
Předpokládejme, že máte následující function.json, která definuje aktivační procedury fronty:

```json
{
    "disabled": false,
    "bindings": [
        {
            "type": "queueTrigger",
            "direction": "in",
            "name": "myQueueItem",
            "queueName": "myqueue-items",
            "connection":"MyStorageConnectionString"
        }
    ]
}
```

Viz ukázka pro specifický jazyk, který načte a protokoly ve frontě metadat.

* [C#](#triggercsharp)
* [Node.js](#triggernodejs)

<a name="triggercsharp"></a>

### <a name="trigger-sample-in-c"></a>Ukázka aktivační události v jazyce C# #
```csharp
#r "Microsoft.WindowsAzure.Storage"

using Microsoft.WindowsAzure.Storage.Queue;
using System;

public static void Run(CloudQueueMessage myQueueItem, 
    DateTimeOffset expirationTime, 
    DateTimeOffset insertionTime, 
    DateTimeOffset nextVisibleTime,
    string queueTrigger,
    string id,
    string popReceipt,
    int dequeueCount,
    TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem.AsString}\n" +
        $"queueTrigger={queueTrigger}\n" +
        $"expirationTime={expirationTime}\n" +
        $"insertionTime={insertionTime}\n" +
        $"nextVisibleTime={nextVisibleTime}\n" +
        $"id={id}\n" +
        $"popReceipt={popReceipt}\n" + 
        $"dequeueCount={dequeueCount}");
}
```

<!--
<a name="triggerfsharp"></a>
### Trigger sample in F# ## 
```fsharp

```
-->

<a name="triggernodejs"></a>

### <a name="trigger-sample-in-nodejs"></a>Ukázka aktivační události v Node.js

```javascript
module.exports = function (context) {
    context.log('Node.js queue trigger function processed work item', context.bindings.myQueueItem);
    context.log('queueTrigger =', context.bindingData.queueTrigger);
    context.log('expirationTime =', context.bindingData.expirationTime);
    context.log('insertionTime =', context.bindingData.insertionTime);
    context.log('nextVisibleTime =', context.bindingData.nextVisibleTime);
    context.log('id=', context.bindingData.id);
    context.log('popReceipt =', context.bindingData.popReceipt);
    context.log('dequeueCount =', context.bindingData.dequeueCount);
    context.done();
};
```

### <a name="handling-poison-queue-messages"></a>Zpracování poškozených fronty zpráv
Pokud se nezdaří aktivační funkce fronty, Azure Functions opakuje této funkce až pětkrát pro danou frontu zprávy, včetně první pokus. Pokud selžou všechny pět pokusy, functions runtime přidá zprávu do fronty úložiště s názvem  *&lt;originalqueuename >-poškozených*. Můžete napsat, že je funkce, která se zpracování zpráv z fronty poškozených jejich protokolování nebo odesílání oznámení, že ruční pozornost. 

Pro zpracování poškozených zpráv ručně, zkontrolujte `dequeueCount` zprávy fronty (v tématu [frontě aktivační události metadata](#meta)).

<a name="output"></a>

## <a name="queue-storage-output-binding"></a>Vazba výstupní fronty úložiště
Úložiště Azure queue výstup vazby umožňuje zápis zpráv do fronty. 

Definovat pomocí vazby fronty výstupu **integrací** na portálu funkce. Vytvoří následující definice v portálu **vazby** části *function.json*:

```json
{
   "type": "queue",
   "direction": "out",
   "name": "<The name used to identify the trigger data in your code>",
   "queueName": "<Name of queue to write to>",
   "connection":"<Name of app setting - see below>"
}
```

* `connection` Vlastnost musí obsahovat název nastavení aplikace, který obsahuje připojovací řetězec úložiště. Na portálu Azure, standardního editoru v **integrací** kartě nakonfiguruje toto nastavení aplikace pro vás, když vyberete účet úložiště.

<a name="outputusage"></a>

## <a name="using-a-queue-output-binding"></a>Pomocí fronty výstup vazby
V Node.js funkce, přístup k výstupu fronty pomocí `context.bindings.<name>`.

V rozhraní .NET funkce výstup můžete na některý z následujících typů. Pokud je parametr typu `T`, `T` musí být jeden z typů podporovaných výstupu, jako například `string` nebo objektů POCO.

* `out T`(serializovanou jako JSON)
* `out string`
* `out byte[]`
* `out` [`CloudQueueMessage`] 
* `ICollector<T>`
* `IAsyncCollector<T>`
* [`CloudQueue`](/dotnet/api/microsoft.windowsazure.storage.queue.cloudqueue)

Návratový typ metody můžete také použít jako výstup vazba.

<a name="outputsample"></a>

## <a name="queue-output-sample"></a>Ukázka výstupní fronty
Následující *function.json* definuje aktivační procedury HTTP s frontou výstup vazby:

```json
{
  "bindings": [
    {
      "type": "httpTrigger",
      "direction": "in",
      "authLevel": "function",
      "name": "input"
    },
    {
      "type": "http",
      "direction": "out",
      "name": "return"
    },
    {
      "type": "queue",
      "direction": "out",
      "name": "$return",
      "queueName": "outqueue",
      "connection": "MyStorageConnectionString",
    }
  ]
}
``` 

Naleznete v ukázce pro specifický jazyk, který produkuje zprávu fronty s příchozí datové části protokolu HTTP.

* [C#](#outcsharp)
* [Node.js](#outnodejs)

<a name="outcsharp"></a>

### <a name="queue-output-sample-in-c"></a>Ukázka výstupní fronty v jazyce C# #

```cs
// C# example of HTTP trigger binding to a custom POCO, with a queue output binding
public class CustomQueueMessage
{
    public string PersonName { get; set; }
    public string Title { get; set; }
}

public static CustomQueueMessage Run(CustomQueueMessage input, TraceWriter log)
{
    return input;
}
```

Chcete-li odeslat více zpráv, použijte `ICollector`:

```cs
public static void Run(CustomQueueMessage input, ICollector<CustomQueueMessage> myQueueItem, TraceWriter log)
{
    myQueueItem.Add(input);
    myQueueItem.Add(new CustomQueueMessage { PersonName = "You", Title = "None" });
}
```

<a name="outnodejs"></a>

### <a name="queue-output-sample-in-nodejs"></a>Ukázka výstupní fronty v Node.js

```javascript
module.exports = function (context, input) {
    context.done(null, input.body);
};
```

Nebo, pokud chcete odeslat více zpráv

```javascript
module.exports = function(context) {
    // Define a message array for the myQueueItem output binding. 
    context.bindings.myQueueItem = ["message 1","message 2"];
    context.done();
};
```

## <a name="next-steps"></a>Další kroky

Příklad, funkci, která používá fronty úložiště triggerů a vazeb, naleznete v části [vytvořit funkci aktivovány Azure Queue storage](functions-create-storage-queue-triggered-function.md).

[!INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]

<!-- LINKS -->

['CloudQueueMessage.]: /dotnet/api/microsoft.windowsazure.storage.queue.cloudqueuemessage
