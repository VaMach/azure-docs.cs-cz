---
title: "Azure vazby fronty úložiště pro Azure Functions"
description: "Pochopit, jak použít aktivační událost Azure Queue storage a výstupní vazby v Azure Functions."
services: functions
documentationcenter: na
author: ggailey777
manager: cfowler
editor: 
tags: 
keywords: "Funkce Azure, funkce zpracování událostí, dynamické výpočetní architektura bez serveru"
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 10/23/2017
ms.author: glenga
ms.openlocfilehash: 258393884c156b2cff00559d3cedfde1380c6c28
ms.sourcegitcommit: 85012dbead7879f1f6c2965daa61302eb78bd366
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/02/2018
---
# <a name="azure-queue-storage-bindings-for-azure-functions"></a>Azure vazby fronty úložiště pro Azure Functions

Tento článek vysvětluje, jak pracovat s Azure Queue storage vazeb v Azure Functions. Azure Functions podporuje aktivaci a výstupní vazby pro fronty.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="trigger"></a>Trigger

Použijte aktivační událost fronty ke spuštění funkce při přijetí nové položky ve frontě. Zprávy ve frontě je k dispozici jako vstup do funkce.

## <a name="trigger---example"></a>Aktivační událost – příklad

Podívejte se na konkrétní jazyk příklad:

* [C#](#trigger---c-example)
* [C# skript (.csx)](#trigger---c-script-example)
* [JavaScript](#trigger---javascript-example)

### <a name="trigger---c-example"></a>Aktivační událost – příklad jazyka C#

Následující příklad ukazuje [funkce jazyka C#](functions-dotnet-class-library.md) který posuzuje `myqueue-items` fronty a zapíše do protokolu pokaždé, když položka fronty je zpracovat.

```csharp
public static class QueueFunctions
{
    [FunctionName("QueueTrigger")]
    public static void QueueTrigger(
        [QueueTrigger("myqueue-items")] string myQueueItem, 
        TraceWriter log)
    {
        log.Info($"C# function processed: {myQueueItem}");
    }
}
```

### <a name="trigger---c-script-example"></a>Aktivační událost – příklad skriptu jazyka C#

Následující příklad ukazuje vazby v aktivační události objektu blob *function.json* souboru a [C# skript (.csx)](functions-reference-csharp.md) kód, který používá vazby. Funkce dotazování `myqueue-items` zařadit do fronty a zapíše do protokolu pokaždé, když položka fronty je zpracovat.

Tady je *function.json* souboru:

```json
{
    "disabled": false,
    "bindings": [
        {
            "type": "queueTrigger",
            "direction": "in",
            "name": "myQueueItem",
            "queueName": "myqueue-items",
            "connection":"MyStorageConnectionAppSetting"
        }
    ]
}
```

[Konfigurace](#trigger---configuration) část vysvětluje tyto vlastnosti.

Tady je kód skriptu jazyka C#:

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

[Využití](#trigger---usage) část vysvětluje `myQueueItem`, která má název podle `name` vlastnost function.json.  [Zprávy oddílem metadat](#trigger---message-metadata) vysvětluje všechny ostatní proměnné vidět.

### <a name="trigger---javascript-example"></a>Aktivační událost – příklad v jazyce JavaScript

Následující příklad ukazuje vazby v aktivační události objektu blob *function.json* souboru a [funkce JavaScript, která](functions-reference-node.md) používající vazby. Funkce dotazování `myqueue-items` zařadit do fronty a zapíše do protokolu pokaždé, když položka fronty je zpracovat.

Tady je *function.json* souboru:

```json
{
    "disabled": false,
    "bindings": [
        {
            "type": "queueTrigger",
            "direction": "in",
            "name": "myQueueItem",
            "queueName": "myqueue-items",
            "connection":"MyStorageConnectionAppSetting"
        }
    ]
}
```

[Konfigurace](#trigger---configuration) část vysvětluje tyto vlastnosti.

Tady je kód jazyka JavaScript:

```javascript
module.exports = function (context) {
    context.log('Node.js queue trigger function processed work item', context.bindings.myQueueItem);
    context.log('queueTrigger =', context.bindingData.queueTrigger);
    context.log('expirationTime =', context.bindingData.expirationTime);
    context.log('insertionTime =', context.bindingData.insertionTime);
    context.log('nextVisibleTime =', context.bindingData.nextVisibleTime);
    context.log('id =', context.bindingData.id);
    context.log('popReceipt =', context.bindingData.popReceipt);
    context.log('dequeueCount =', context.bindingData.dequeueCount);
    context.done();
};
```

[Využití](#trigger---usage) část vysvětluje `myQueueItem`, která má název podle `name` vlastnost function.json.  [Zprávy oddílem metadat](#trigger---message-metadata) vysvětluje všechny ostatní proměnné vidět.

## <a name="trigger---attributes"></a>Aktivační událost – atributy
 
V [knihovny tříd jazyka C#](functions-dotnet-class-library.md), použijte následující atributy ke konfiguraci aktivační procedury fronty:

* [QueueTriggerAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/QueueTriggerAttribute.cs), definované v balíčku NuGet [Microsoft.Azure.WebJobs](http://www.nuget.org/packages/Microsoft.Azure.WebJobs)

  Konstruktoru atributu přebírá název fronty, které chcete sledovat, jak je znázorněno v následujícím příkladu:

  ```csharp
  [FunctionName("QueueTrigger")]
  public static void Run(
      [QueueTrigger("myqueue-items")] string myQueueItem, 
      TraceWriter log)
  {
      ...
  }
  ```

  Můžete nastavit `Connection` vlastnosti a určit účet úložiště, který chcete použít, jak je znázorněno v následujícím příkladu:

  ```csharp
  [FunctionName("QueueTrigger")]
  public static void Run(
      [QueueTrigger("myqueue-items", Connection = "StorageConnectionAppSetting")] string myQueueItem, 
      TraceWriter log)
  {
      ....
  }
  ```
 
  Úplný příklad najdete v tématu [aktivační událost - C# příklad](#trigger---c-example).

* [StorageAccountAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/StorageAccountAttribute.cs), definované v balíčku NuGet [Microsoft.Azure.WebJobs](http://www.nuget.org/packages/Microsoft.Azure.WebJobs)

  Poskytuje další možnost zadejte účet úložiště, který chcete použít. Konstruktor přebírá název nastavení aplikace, který obsahuje připojovací řetězec úložiště. Atribut lze použít na parametr, metoda nebo na úrovni třídy. Následující příklad ukazuje úroveň třídy a metody:

  ```csharp
  [StorageAccount("ClassLevelStorageAppSetting")]
  public static class AzureFunctions
  {
      [FunctionName("QueueTrigger")]
      [StorageAccount("FunctionLevelStorageAppSetting")]
      public static void Run( //...
  {
      ...
  }
  ```

Účet úložiště, který chcete použít, je určen v následujícím pořadí:

* `QueueTrigger` Atributu `Connection` vlastnost.
* `StorageAccount` Atribut použitý pro parametr stejné jako `QueueTrigger` atribut.
* `StorageAccount` Atribut použitý k funkci.
* `StorageAccount` Atribut použitý k třídě.
* Nastavení aplikace "AzureWebJobsStorage".

## <a name="trigger---configuration"></a>Aktivační událost - konfigurace

Následující tabulka popisuje vlastnosti konfigurace vazby, které jste nastavili v *function.json* souboru a `QueueTrigger` atribut.

|Vlastnost Function.JSON | Vlastnost atributu |Popis|
|---------|---------|----------------------|
|**Typ** | neuvedeno| musí být nastavena na `queueTrigger`. Tato vlastnost nastavena automaticky při vytváření aktivační události na portálu Azure.|
|**směr**| neuvedeno | V *function.json* pouze souboru. musí být nastavena na `in`. Tato vlastnost nastavena automaticky při vytváření aktivační události na portálu Azure. |
|**Jméno** | neuvedeno |Název proměnné, která představuje ve frontě v kód funkce.  | 
|**queueName** | **QueueName**| Název fronty pro cyklické dotazování. | 
|**připojení** | **Připojení** |Název nastavení aplikace, který obsahuje připojovací řetězec úložiště k použití pro tuto vazbu. Název nastavení aplikace začíná "AzureWebJobs", můžete zadat pouze zbytku názvu sem. Například pokud nastavíte `connection` na "MyStorage" Functions runtime vypadá pro aplikaci nastavení, která je s názvem "AzureWebJobsMyStorage." Pokud necháte `connection` prázdný, funkce používá modul runtime výchozí úložiště připojovací řetězec v nastavení aplikace, který je pojmenován `AzureWebJobsStorage`.|

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="trigger---usage"></a>Aktivační událost - využití
 
V jazyce C# a C# skript, přístup k datům objektu blob pomocí parametru metody `Stream paramName`. V jazyce C# skript `paramName` je hodnota zadaná v `name` vlastnost *function.json*. Můžete vázat na některý z následujících typů:

* Objekt – modul runtime Functions objektů POCO deserializuje datovou část JSON do objektů POCO objektu. 
* `string`
* `byte[]`
* [CloudQueueMessage]

V jazyce JavaScript, použijte `context.bindings.<name>` pro přístup k datové položky fronty. Pokud datové části JSON, je deserializovat do objektu.

## <a name="trigger---message-metadata"></a>Aktivační událost – zpráva metadat

Aktivační událost fronty nabízí několik vlastností metadat. Tyto vlastnosti lze použít jako součást vazby výrazy v jiných vazby nebo jako parametry v kódu. Hodnoty mají stejnou sémantiku jako [CloudQueueMessage](https://docs.microsoft.com/dotnet/api/microsoft.windowsazure.storage.queue.cloudqueuemessage).

|Vlastnost|Typ|Popis|
|--------|----|-----------|
|`QueueTrigger`|`string`|Datová část frontu (pokud platný řetězec). Pokud fronty zpráv datové části jako řetězec, `QueueTrigger` má stejnou hodnotu jako proměnné s názvem podle `name` vlastnost *function.json*.|
|`DequeueCount`|`int`|Počet časy, kdy se tato zpráva má bylo vyřazeno z fronty.|
|`ExpirationTime`|`DateTimeOffset?`|Čas, kdy vyprší platnost zprávy.|
|`Id`|`string`|ID zprávy fronty|
|`InsertionTime`|`DateTimeOffset?`|Doba, která zpráva byla přidána do fronty.|
|`NextVisibleTime`|`DateTimeOffset?`|Čas, zprávy budou vedle viditelné.|
|`PopReceipt`|`string`|Pop přijetí zprávy.|

## <a name="trigger---poison-messages"></a>Aktivační událost - poškozených zpráv

Pokud se nezdaří aktivační funkce fronty, Azure Functions opakuje funkce až pětkrát pro danou frontu zprávy, včetně první pokus. Pokud selžou všechny pět pokusy, functions runtime přidá zprávu do fronty s názvem  *&lt;originalqueuename >-poškozených*. Můžete napsat, že je funkce, která se zpracování zpráv z fronty poškozených jejich protokolování nebo odesílání oznámení, že ruční pozornost.

Pro zpracování poškozených zpráv ručně, zkontrolujte [dequeueCount](#trigger---message-metadata) zprávy fronty.

## <a name="trigger---hostjson-properties"></a>Aktivační událost - host.json vlastnosti

[Host.json](functions-host-json.md#queues) soubor obsahuje nastavení, které řídí chování frontě aktivační události.

[!INCLUDE [functions-host-json-queues](../../includes/functions-host-json-queues.md)]

## <a name="output"></a>Výstup

Pomocí Azure Queue storage výstup vazby pro zápis zpráv do fronty.

## <a name="output---example"></a>Výstup – příklad

Podívejte se na konkrétní jazyk příklad:

* [C#](#output---c-example)
* [C# skript (.csx)](#output---c-script-example)
* [JavaScript](#output---javascript-example)

### <a name="output---c-example"></a>Výstup – příklad jazyka C#

Následující příklad ukazuje [C# funkce](functions-dotnet-class-library.md) , vytvoří zprávu fronty pro každý požadavek HTTP přijatých.

```csharp
[StorageAccount("AzureWebJobsStorage")]
public static class QueueFunctions
{
    [FunctionName("QueueOutput")]
    [return: Queue("myqueue-items")]
    public static string QueueOutput([HttpTrigger] dynamic input,  TraceWriter log)
    {
        log.Info($"C# function processed: {input.Text}");
        return input.Text;
    }
}
```

### <a name="output---c-script-example"></a>Výstup – příklad skriptu jazyka C#

Následující příklad ukazuje vazby v aktivační události objektu blob *function.json* souboru a [C# skript (.csx)](functions-reference-csharp.md) kód, který používá vazby. Funkce vytvoří položka fronty s datovou objektů POCO pro každý požadavek HTTP přijatých.

Tady je *function.json* souboru:

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
      "connection": "MyStorageConnectionAppSetting",
    }
  ]
}
``` 

[Konfigurace](#output---configuration) část vysvětluje tyto vlastnosti.

Tady je C# kód skriptu, který vytvoří jedna fronta zprávu:

```cs
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

Můžete odeslat více zpráv najednou pomocí `ICollector` nebo `IAsyncCollector` parametr. Zde uvádíme C# kód skriptu, který odešle více zpráv, jednu s data požadavku HTTP a druhou s pevně zakódovaným hodnotami:

```cs
public static void Run(
    CustomQueueMessage input, 
    ICollector<CustomQueueMessage> myQueueItem, 
    TraceWriter log)
{
    myQueueItem.Add(input);
    myQueueItem.Add(new CustomQueueMessage { PersonName = "You", Title = "None" });
}
```

### <a name="output---javascript-example"></a>Výstup – příklad v jazyce JavaScript

Následující příklad ukazuje vazby v aktivační události objektu blob *function.json* souboru a [funkce JavaScript, která](functions-reference-node.md) používající vazby. Funkce vytvoří položka fronty pro každý požadavek HTTP přijatých.

Tady je *function.json* souboru:

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
      "connection": "MyStorageConnectionAppSetting",
    }
  ]
}
``` 

[Konfigurace](#output---configuration) část vysvětluje tyto vlastnosti.

Tady je kód jazyka JavaScript:

```javascript
module.exports = function (context, input) {
    context.done(null, input.body);
};
```

Můžete odeslat více zpráv najednou tak, že definujete pole zpráv pro `myQueueItem` výstup vazby. Následující kód v JavaScriptu odešle dvě zprávy fronty s pevně zakódovaným hodnoty pro každý požadavek HTTP přijatých.

```javascript
module.exports = function(context) {
    context.bindings.myQueueItem = ["message 1","message 2"];
    context.done();
};
```

## <a name="output---attributes"></a>Výstup – atributy
 
V [knihovny tříd jazyka C#](functions-dotnet-class-library.md), použijte [QueueAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/QueueAttribute.cs), která je definována v balíčku NuGet [Microsoft.Azure.WebJobs](http://www.nuget.org/packages/Microsoft.Azure.WebJobs).

Atribut se vztahuje na `out` parametr nebo vrací hodnotu funkce. Konstruktoru atributu přebírá název fronty, jak je znázorněno v následujícím příkladu:

```csharp
[FunctionName("QueueOutput")]
[return: Queue("myqueue-items")]
public static string Run([HttpTrigger] dynamic input,  TraceWriter log)
{
    ...
}
```

Můžete nastavit `Connection` vlastnosti a určit účet úložiště, který chcete použít, jak je znázorněno v následujícím příkladu:

```csharp
[FunctionName("QueueOutput")]
[return: Queue("myqueue-items, Connection = "StorageConnectionAppSetting")]
public static string Run([HttpTrigger] dynamic input,  TraceWriter log)
{
    ...
}
```

Úplný příklad najdete v tématu [výstup - C# příklad](#output---c-example).

Můžete použít `StorageAccount` atribut určete účet úložiště na úrovni třídy, metoda nebo parametr. Další informace najdete v tématu [aktivační událost – atributy](#trigger---attribute).

## <a name="output---configuration"></a>Výstup – konfigurace

Následující tabulka popisuje vlastnosti konfigurace vazby, které jste nastavili v *function.json* souboru a `Queue` atribut.

|Vlastnost Function.JSON | Vlastnost atributu |Popis|
|---------|---------|----------------------|
|**Typ** | neuvedeno | musí být nastavena na `queue`. Tato vlastnost nastavena automaticky při vytváření aktivační události na portálu Azure.|
|**směr** | neuvedeno | musí být nastavena na `out`. Tato vlastnost nastavena automaticky při vytváření aktivační události na portálu Azure. |
|**Jméno** | neuvedeno | Název proměnné, která představuje ve frontě v kód funkce. Nastavte na `$return` Chcete-li funkce návratovou hodnotu.| 
|**queueName** |**QueueName** | Název fronty | 
|**připojení** | **Připojení** |Název nastavení aplikace, který obsahuje připojovací řetězec úložiště k použití pro tuto vazbu. Název nastavení aplikace začíná "AzureWebJobs", můžete zadat pouze zbytku názvu sem. Například pokud nastavíte `connection` na "MyStorage" Functions runtime vypadá pro aplikaci nastavení, která je s názvem "AzureWebJobsMyStorage." Pokud necháte `connection` prázdný, funkce používá modul runtime výchozí úložiště připojovací řetězec v nastavení aplikace, který je pojmenován `AzureWebJobsStorage`.|

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="output---usage"></a>Výstup – použití
 
V jazyce C# a C# skript, zápis zpráva s jednou frontou pomocí parametru metody `out T paramName`. V jazyce C# skript `paramName` je hodnota zadaná v `name` vlastnost *function.json*. Návratový typ metody místo můžete použít `out` parametr, a `T` může být libovolná z následujících typů:

* Serializovatelné jako JSON objektů POCO
* `string`
* `byte[]`
* [CloudQueueMessage] 

V jazyce C# a C# skript zápis více fronty zpráv pomocí jedné z následujících typů: 

* `ICollector<T>` nebo `IAsyncCollector<T>`
* [CloudQueue](/dotnet/api/microsoft.windowsazure.storage.queue.cloudqueue)

V funkce jazyka JavaScript, použijte `context.bindings.<name>` pro přístup ke zprávě výstupní fronty. Řetězec nebo objekt JSON serializovatelný můžete použít pro datová položka fronty.

## <a name="next-steps"></a>Další postup

> [!div class="nextstepaction"]
> [Přejděte na rychlé spuštění, který používá aktivační procedury fronty úložiště](functions-create-storage-queue-triggered-function.md)

> [!div class="nextstepaction"]
> [Přejděte na kurz, který používá Queue storage výstup vazby](functions-integrate-storage-queue-output-binding.md)

> [!div class="nextstepaction"]
> [Další informace o Azure functions triggerů a vazeb](functions-triggers-bindings.md)

<!-- LINKS -->

[CloudQueueMessage]: /dotnet/api/microsoft.windowsazure.storage.queue.cloudqueuemessage