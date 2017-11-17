---
title: "Azure Service Bus funkce triggerů a vazeb"
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
ms.openlocfilehash: 5ef558f19bb88d208b0d224e30137ac237ab64bc
ms.sourcegitcommit: 7d107bb9768b7f32ec5d93ae6ede40899cbaa894
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/16/2017
---
# <a name="azure-functions-service-bus-bindings"></a>Azure Service Bus funkce vazby

Tento článek vysvětluje, jak pracovat s vazeb Azure Service Bus v Azure Functions. Azure Functions podporuje aktivaci a výstupní vazby pro fronty sběrnice a témata.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="service-bus-trigger"></a>Aktivace služby Service Bus

Použijte aktivační událost Service Bus reagovat na zprávy z fronty sběrnice nebo téma. 

## <a name="trigger---example"></a>Aktivační událost – příklad

Podívejte se na konkrétní jazyk příklad:

* [Předkompilované C#](#trigger---c-example)
* [Skript jazyka C#](#trigger---c-script-example)
* [F#](#trigger---f-example)
* [JavaScript](#trigger---javascript-example)

### <a name="trigger---c-example"></a>Aktivační událost – příklad jazyka C#

Následující příklad ukazuje [předkompilovaných C# funkce](functions-dotnet-class-library.md) , zaznamená zprávu fronty Service Bus.

```cs
[FunctionName("ServiceBusQueueTriggerCSharp")]                    
public static void Run(
    [ServiceBusTrigger("myqueue", AccessRights.Manage, Connection = "ServiceBusConnection")] 
    string myQueueItem, 
    TraceWriter log)
{
    log.Info($"C# ServiceBus queue trigger function processed message: {myQueueItem}");
}
```

### <a name="trigger---c-script-example"></a>Aktivační událost – příklad skriptu jazyka C#

Následující příklad ukazuje vazby v aktivační události služby Service Bus *function.json* souboru a [funkce skriptu jazyka C#](functions-reference-csharp.md) používající vazby. Funkce zaznamená zprávu fronty Service Bus.

Zde je vazba dat v *function.json* souboru:

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

Tady je kód skriptu jazyka C#:

```cs
public static void Run(string myQueueItem, TraceWriter log)
{
    log.Info($"C# ServiceBus queue trigger function processed message: {myQueueItem}");
}
```

### <a name="trigger---f-example"></a>Aktivační událost – příklad F #

Následující příklad ukazuje vazby v aktivační události služby Service Bus *function.json* souboru a [funkce F #](functions-reference-fsharp.md) používající vazby. Funkce zaznamená zprávu fronty Service Bus. 

Zde je vazba dat v *function.json* souboru:

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

Tady je kód skriptu F #:

```fsharp
let Run(myQueueItem: string, log: TraceWriter) =
    log.Info(sprintf "F# ServiceBus queue trigger function processed message: %s" myQueueItem)
```

### <a name="trigger---javascript-example"></a>Aktivační událost – příklad v jazyce JavaScript

Následující příklad ukazuje vazby v aktivační události služby Service Bus *function.json* souboru a [funkce JavaScript, která](functions-reference-node.md) používající vazby. Funkce zaznamená zprávu fronty Service Bus. 

Zde je vazba dat v *function.json* souboru:

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

Tady je kód JavaScript skriptu:

```javascript
module.exports = function(context, myQueueItem) {
    context.log('Node.js ServiceBus queue trigger function processed message', myQueueItem);
    context.done();
};
```

## <a name="trigger---attributes-for-precompiled-c"></a>Aktivační událost - předkompilovaných atributy pro C#

Pro [předkompilovaných C#](functions-dotnet-class-library.md) funkce dovoluje konfigurovat aktivační události služby Service Bus následující atributy:

* [ServiceBusTriggerAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/ServiceBusTriggerAttribute.cs), definované v balíčku NuGet [Microsoft.Azure.WebJobs.ServiceBus](http://www.nuget.org/packages/Microsoft.Azure.WebJobs.ServiceBus)

  Konstruktoru atributu převezme název fronta nebo téma a odběr. Můžete také zadat připojení přístupová práva. Pokud nezadáte přístupová práva, výchozí hodnota je `Manage`. Jak vybrat přístupová práva, nastavení je podrobně popsaný [aktivační událost - konfigurace](#trigger---configuration) části. Tady je příklad, který ukazuje atribut používaný s parametrem řetězec:

  ```csharp
  [FunctionName("ServiceBusQueueTriggerCSharp")]                    
  public static void Run(
      [ServiceBusTrigger("myqueue")] string myQueueItem, TraceWriter log)
  ```

  Můžete nastavit `Connection` vlastnosti a určit účet služby Service Bus, který chcete použít, jak je znázorněno v následujícím příkladu:

  ```csharp
  [FunctionName("ServiceBusQueueTriggerCSharp")]                    
  public static void Run(
      [ServiceBusTrigger("myqueue", Connection = "ServiceBusConnection")] 
      string myQueueItem, TraceWriter log)
  ```

* [ServiceBusAccountAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/ServiceBusAccountAttribute.cs), definované v balíčku NuGet [Microsoft.Azure.WebJobs.ServiceBus](http://www.nuget.org/packages/Microsoft.Azure.WebJobs.ServiceBus)

  Poskytuje další možnost zadat účet služby Service Bus, který chcete použít. Konstruktor přebírá název nastavení aplikace, který obsahuje připojovací řetězec sběrnice služeb. Atribut lze použít na parametr, metoda nebo na úrovni třídy. Následující příklad ukazuje úroveň třídy a metody:

  ```csharp
  [ServiceBusAccount("ClassLevelServiceBusAppSetting")]
  public static class AzureFunctions
  {
      [ServiceBusAccount("MethodLevelServiceBusAppSetting")]
      [FunctionName("ServiceBusQueueTriggerCSharp")]
      public static void Run(
          [ServiceBusTrigger("myqueue", AccessRights.Manage)] 
          string myQueueItem, TraceWriter log)
  ```

Účet služby Service Bus, který chcete použít, je určen v následujícím pořadí:

* `ServiceBusTrigger` Atributu `Connection` vlastnost.
* `ServiceBusAccount` Atribut použitý pro parametr stejné jako `ServiceBusTrigger` atribut.
* `ServiceBusAccount` Atribut použitý k funkci.
* `ServiceBusAccount` Atribut použitý k třídě.
* Nastavení aplikace "AzureWebJobsServiceBus".

## <a name="trigger---configuration"></a>Aktivační událost - konfigurace

Následující tabulka popisuje vlastnosti konfigurace vazby, které jste nastavili v *function.json* souboru a `ServiceBusTrigger` atribut.

|Vlastnost Function.JSON | Vlastnost atributu |Popis|
|---------|---------|----------------------|
|**Typ** | neuvedeno | Musí být nastavena na "serviceBusTrigger". Tato vlastnost nastavena automaticky při vytváření aktivační události na portálu Azure.|
|**směr** | neuvedeno | Musí být nastavena na "v". Tato vlastnost nastavena automaticky při vytváření aktivační události na portálu Azure. |
|**Jméno** | neuvedeno | Název proměnné, která představuje zprávu fronta nebo téma v kódu funkce. Chcete-li funkce návratovou hodnotu nastavte na "$return". | 
|**queueName**|**QueueName**|Název fronty k monitorování.  Nastavte jenom v případě, že monitorování fronty, ne pro téma.
|**topicName**|**TopicName**|Název tématu, které chcete monitorovat. Nastavte jenom v případě, že monitorování téma, ne pro frontu.|
|**Název_předplatného**|**Název_předplatného**|Název odběru k monitorování. Nastavte jenom v případě, že monitorování téma, ne pro frontu.|
|**připojení**|**Připojení**|Název nastavení aplikace, který obsahuje připojovací řetězec sběrnice služeb, který chcete použít pro tuto vazbu. Název nastavení aplikace začíná "AzureWebJobs", můžete zadat pouze zbytku názvu. Například pokud nastavíte `connection` na "MyServiceBus" Functions runtime vypadá pro aplikaci nastavení, která je s názvem "AzureWebJobsMyServiceBus." Pokud necháte `connection` prázdná, připojovací řetězec sběrnice služeb výchozí modul runtime funkce používá v nastavení aplikace, s názvem "AzureWebJobsServiceBus".<br><br>Pokud chcete získat připojovací řetězec, postupujte podle kroků, zobrazuje se v [získat přihlašovací údaje správu](../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md#obtain-the-management-credentials). Připojovací řetězec musí být v případě oboru názvů Service Bus, bez omezení konkrétní fronty nebo téma. <br/>Pokud vyvíjíte místně, nastavení aplikace, přejděte do hodnoty [local.settings.json soubor](functions-run-local.md#local-settings-file).|
|**accessRights**|**Přístup**|Přístupová práva pro připojovací řetězec. Dostupné jsou hodnoty `manage` a `listen`. Výchozí hodnota je `manage`, což znamená, že `connection` má **spravovat** oprávnění. Pokud použijete připojovací řetězec, který nemá **spravovat** nastavit oprávnění, `accessRights` "naslouchat". Funkce modulu runtime může selhat pokouší o provedení operace, které vyžadují, jinak hodnota spravovat práva.|

## <a name="trigger---usage"></a>Aktivační událost - využití

V jazyce C# a C# skript, přístup k fronta nebo téma zprávu pomocí parametru metody `string paramName`. V jazyce C# skript `paramName` je hodnota zadaná v `name` vlastnost *function.json*. Můžete použít některou z následujících typů místo `string`:

* `byte[]`-Vhodné pro binární data.
* Vlastní typ – Pokud zpráva obsahuje formát JSON, Azure Functions se pokusí rekonstruovat JSON data.
* `BrokeredMessage`-Vám deserializovat zprávu s [BrokeredMessage.GetBody<T>()](https://msdn.microsoft.com/library/hh144211.aspx) metoda.

V jazyce JavaScript, přístup k fronta nebo téma zprávy pomocí `context.bindings.<name>`. `<name>`Hodnota zadaná v `name` vlastnost *function.json*. Zprávy služby Service Bus je předán do funkce jako řetězec nebo objekt JSON.

## <a name="trigger---poison-messages"></a>Aktivační událost - poškozených zpráv

Zpracování poškozených zpráv nelze řídí nebo nakonfigurované v Azure Functions. Service Bus zpracovává poškozených zpráv sám sebe.

## <a name="trigger---peeklock-behavior"></a>Aktivační událost - PeekLock chování

Modul runtime funkce obdrží zprávu v [PeekLock režimu](../service-bus-messaging/service-bus-performance-improvements.md#receive-mode). Zavolá `Complete` na zprávu, pokud funkci skončí úspěšně, nebo volání `Abandon` Pokud funkce selže. Pokud je funkce spuštěná déle, než `PeekLock` automaticky obnovují vždy vypršel časový limit, zámek.

## <a name="trigger---hostjson-properties"></a>Aktivační událost - host.json vlastnosti

[Host.json](functions-host-json.md#servicebus) soubor obsahuje nastavení, které řídí chování aktivační události služby Service Bus.

[!INCLUDE [functions-host-json-event-hubs](../../includes/functions-host-json-service-bus.md)]

## <a name="service-bus-output-binding"></a>Service Bus výstup vazby

Použití Azure Service Bus Výstupní vazba odesílat zprávy fronty nebo téma.

## <a name="output---example"></a>Výstup – příklad

Podívejte se na konkrétní jazyk příklad:

* [Předkompilované C#](#output---c-example)
* [Skript jazyka C#](#output---c-script-example)
* [F#](#output---f-example)
* [JavaScript](#output---javascript-example)

### <a name="output---c-example"></a>Výstup – příklad jazyka C#

Následující příklad ukazuje [předkompilovaných C# funkce](functions-dotnet-class-library.md) , odešle zprávu fronty sběrnice:

```cs
[FunctionName("ServiceBusOutput")]
[return: ServiceBus("myqueue", Connection = "ServiceBusConnection")]
public static string ServiceBusOutput([HttpTrigger] dynamic input, TraceWriter log)
{
    log.Info($"C# function processed: {input.Text}");
    return input.Text;
}
```

### <a name="output---c-script-example"></a>Výstup – příklad skriptu jazyka C#

Následující příklad ukazuje výstup Service Bus vazby ve *function.json* souboru a [funkce skriptu jazyka C#](functions-reference-csharp.md) používající vazby. Funkce používá aktivaci časovačem odeslat zprávu fronty každých 15 sekund.

Zde je vazba dat v *function.json* souboru:

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

Tady je C# kód skriptu, který vytvoří do jedné zprávy:

```cs
public static void Run(TimerInfo myTimer, TraceWriter log, out string outputSbQueue)
{
    string message = $"Service Bus queue message created at: {DateTime.Now}";
    log.Info(message); 
    outputSbQueue = message;
}
```

Zde uvádíme C# kód skriptu, který vytvoří více zpráv:

```cs
public static void Run(TimerInfo myTimer, TraceWriter log, ICollector<string> outputSbQueue)
{
    string message = $"Service Bus queue messages created at: {DateTime.Now}";
    log.Info(message); 
    outputSbQueue.Add("1 " + message);
    outputSbQueue.Add("2 " + message);
}
```

### <a name="output---f-example"></a>Výstup – příklad F #

Následující příklad ukazuje výstup Service Bus vazby ve *function.json* souboru a [F # skript funkce](functions-reference-fsharp.md) používající vazby. Funkce používá aktivaci časovačem odeslat zprávu fronty každých 15 sekund.

Zde je vazba dat v *function.json* souboru:

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

Tady je F # kód skriptu, který vytvoří do jedné zprávy:

```fsharp
let Run(myTimer: TimerInfo, log: TraceWriter, outputSbQueue: byref<string>) =
    let message = sprintf "Service Bus queue message created at: %s" (DateTime.Now.ToString())
    log.Info(message)
    outputSbQueue = message
```

### <a name="output---javascript-example"></a>Výstup – příklad v jazyce JavaScript

Následující příklad ukazuje výstup Service Bus vazby ve *function.json* souboru a [funkce JavaScript, která](functions-reference-node.md) používající vazby. Funkce používá aktivaci časovačem odeslat zprávu fronty každých 15 sekund.

Zde je vazba dat v *function.json* souboru:

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

Zde je kód JavaScript, který vytvoří do jedné zprávy:

```javascript
module.exports = function (context, myTimer) {
    var message = 'Service Bus queue message created at ' + timeStamp;
    context.log(message);   
    context.bindings.outputSbQueueMsg = message;
    context.done();
};
```

Zde je kód skriptu jazyka JavaScript, který vytváří více zpráv:

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

## <a name="output---attributes-for-precompiled-c"></a>Výstup – atributy pro předkompilované C#

Pro [předkompilovaných C#](functions-dotnet-class-library.md) používat funkce, [ServiceBusAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/ServiceBusAttribute.cs), která je definována v balíčku NuGet [Microsoft.Azure.WebJobs.ServiceBus](http://www.nuget.org/packages/Microsoft.Azure.WebJobs.ServiceBus).

  Konstruktoru atributu převezme název fronta nebo téma a odběr. Můžete také zadat připojení přístupová práva. Jak vybrat přístupová práva, nastavení je podrobně popsaný [výstup - konfigurace](#output---configuration) části. Tady je příklad, který ukazuje atribut návratové hodnoty funkce:

  ```csharp
  [FunctionName("ServiceBusOutput")]
  [return: ServiceBus("myqueue")]
  public static string Run([HttpTrigger] dynamic input, TraceWriter log)
  ```

  Můžete nastavit `Connection` vlastnosti a určit účet služby Service Bus, který chcete použít, jak je znázorněno v následujícím příkladu:

  ```csharp
  [FunctionName("ServiceBusOutput")]
  [return: ServiceBus("myqueue", Connection = "ServiceBusConnection")]
  public static string Run([HttpTrigger] dynamic input, TraceWriter log)
  ```

Můžete použít `ServiceBusAccount` atribut zadat účet služby Service Bus, který chcete použít na úrovni třídy, metoda nebo parametr.  Další informace najdete v tématu [aktivační událost – atributy pro předkompilované C#](#trigger---attributes-for-precompiled-c).

## <a name="output---configuration"></a>Výstup – konfigurace

Následující tabulka popisuje vlastnosti konfigurace vazby, které jste nastavili v *function.json* souboru a `ServiceBus` atribut.

|Vlastnost Function.JSON | Vlastnost atributu |Popis|
|---------|---------|----------------------|
|**Typ** | neuvedeno | Musí být nastavena na "sběrnice". Tato vlastnost nastavena automaticky při vytváření aktivační události na portálu Azure.|
|**směr** | neuvedeno | Musí být nastavena na "out". Tato vlastnost nastavena automaticky při vytváření aktivační události na portálu Azure. |
|**Jméno** | neuvedeno | Název proměnné, která představuje fronta nebo téma v kódu funkce. Chcete-li funkce návratovou hodnotu nastavte na "$return". | 
|**queueName**|**QueueName**|Název fronty.  Nastavte jenom v případě, že odeslání zprávy do fronty, není pro téma.
|**topicName**|**TopicName**|Název tématu, které chcete monitorovat. Nastavte jenom v případě, že odesílání zpráv tématu, ne pro frontu.|
|**Název_předplatného**|**Název_předplatného**|Název odběru k monitorování. Nastavte jenom v případě, že odesílání zpráv tématu, ne pro frontu.|
|**připojení**|**Připojení**|Název nastavení aplikace, který obsahuje připojovací řetězec sběrnice služeb, který chcete použít pro tuto vazbu. Název nastavení aplikace začíná "AzureWebJobs", můžete zadat pouze zbytku názvu. Například pokud nastavíte `connection` na "MyServiceBus" Functions runtime vypadá pro aplikaci nastavení, která je s názvem "AzureWebJobsMyServiceBus." Pokud necháte `connection` prázdná, připojovací řetězec sběrnice služeb výchozí modul runtime funkce používá v nastavení aplikace, s názvem "AzureWebJobsServiceBus".<br><br>Pokud chcete získat připojovací řetězec, postupujte podle kroků, zobrazuje se v [získat přihlašovací údaje správu](../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md#obtain-the-management-credentials). Připojovací řetězec musí být v případě oboru názvů Service Bus, bez omezení konkrétní fronty nebo téma. <br/>Pokud vyvíjíte místně, nastavení aplikace, přejděte do hodnoty [local.settings.json soubor](functions-run-local.md#local-settings-file).|
|**accessRights**|**Přístup** |Přístupová práva pro připojovací řetězec. Dostupné jsou hodnoty "Správa" a "naslouchat". Výchozí hodnota je "manage", což naznačuje, že má připojení **spravovat** oprávnění. Pokud použijete připojovací řetězec, který nemá **spravovat** nastavit oprávnění, `accessRights` "naslouchat". Funkce modulu runtime může selhat pokouší o provedení operace, které vyžadují, jinak hodnota spravovat práva.|

## <a name="output---usage"></a>Výstup – použití

V jazyce C# a C# skript, přístup k fronta nebo téma pomocí parametru metody `out string paramName`. V jazyce C# skript `paramName` je hodnota zadaná v `name` vlastnost *function.json*. Můžete použít některou z následujících typů parametrů:

* `out T paramName` - `T`mohou být jakéhokoli typu JSON serializable. Pokud hodnota parametru má hodnotu null při ukončení funkce, funkce vytvoří zprávu s objektem hodnotu null.
* `out string`– Pokud je hodnota parametru má hodnotu null při ukončení funkce, funkce nevytváří zprávu.
* `out byte[]`– Pokud je hodnota parametru má hodnotu null při ukončení funkce, funkce nevytváří zprávu.
* `out BrokeredMessage`– Pokud je hodnota parametru má hodnotu null při ukončení funkce, funkce nevytváří zprávu.

Pro vytvoření více zpráv v C# nebo funkce skriptu jazyka C#, můžete použít `ICollector<T>` nebo `IAsyncCollector<T>`. Zprávu se vytvoří při volání `Add` metoda.

V jazyce JavaScript, přístup k fronta nebo téma pomocí `context.bindings.<name>`. `<name>`Hodnota zadaná v `name` vlastnost *function.json*. Řetězec, bajtové pole nebo objekt jazyka Javascript (deserializovat do formátu JSON) můžete přiřadit k `context.binding.<name>`.

## <a name="next-steps"></a>Další kroky

> [!div class="nextstepaction"]
> [Další informace o Azure functions triggerů a vazeb](functions-triggers-bindings.md)
