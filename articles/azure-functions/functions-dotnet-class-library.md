---
title: "Azure funkcí jazyka C# referenční informace pro vývojáře"
description: "Pochopit, jak vyvíjet Azure Functions pomocí jazyka C#."
services: functions
documentationcenter: na
author: ggailey777
manager: cfowler
editor: 
tags: 
keywords: "funkce azure, funkce, zpracování událostí, webhook, dynamické výpočty, architektura bez serverů"
ms.service: functions
ms.devlang: dotnet
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 12/12/2017
ms.author: glenga
ms.openlocfilehash: 8bd46adc475af35d32b9e329a3765e064120a6e3
ms.sourcegitcommit: 1d423a8954731b0f318240f2fa0262934ff04bd9
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/05/2018
---
# <a name="azure-functions-c-developer-reference"></a>Azure funkcí jazyka C# referenční informace pro vývojáře

<!-- When updating this article, make corresponding changes to any duplicate content in functions-reference-csharp.md -->

Tento článek je úvodem do Azure Functions vývoj s použitím jazyka C# v knihovny tříd rozhraní .NET.

Azure Functions podporuje C# a C# skript programovacích jazyků. Pokud hledáte pokyny [pomocí jazyka C# na portálu Azure](functions-create-function-app-portal.md), najdete v části [C# skript (.csx) referenční informace pro vývojáře](functions-reference-csharp.md).

Tento článek předpokládá, že jste si přečetli již v následujících článcích:

* [Příručka pro vývojáře Azure funkce](functions-reference.md)
* [Nástroje sady Visual Studio 2017 funkcí Azure](functions-develop-vs.md)

## <a name="functions-class-library-project"></a>Funkce projektu knihovny tříd

V sadě Visual Studio **Azure Functions** vytvoří šablona projektu C# projektu knihovny tříd obsahující následující soubory:

* [Host.JSON](functions-host-json.md) – ukládá nastavení konfigurace, které ovlivní všechny funkce v projektu, když běží místně nebo v Azure.
* [Local.Settings.JSON](functions-run-local.md#local-settings-file) – ukládá nastavení aplikace a připojovacích řetězců, které se používají při místním spuštění.

### <a name="functionname-and-trigger-attributes"></a>Atributy %{FunctionName/ a aktivační události

Knihovny tříd, funkce je statickou metodu s `FunctionName` a aktivační události atributu, jak je znázorněno v následujícím příkladu:

```csharp
public static class SimpleExample
{
    [FunctionName("QueueTrigger")]
    public static void Run(
        [QueueTrigger("myqueue-items")] string myQueueItem, 
        TraceWriter log)
    {
        log.Info($"C# function processed: {myQueueItem}");
    }
} 
```

`FunctionName` Atribut určí metodu jako vstupní bod funkce. Název musí být jedinečný v rámci projektu.

Aktivační událost atribut určuje typ aktivační události a sváže vstupní data parametru metody. Příklad funkce se aktivuje zprávu fronty a zprávy ve frontě, je předaná metodě v `myQueueItem` parametr.

### <a name="additional-binding-attributes"></a>Atributy další vazby

Může použít další vstupní a výstupní vazby atributy. Následující příklad změní předchozí jeden přidáním vazbu výstupní fronty. Funkce zapíše zprávu vstupní fronty do nové zprávy fronty v jiné fronty.

```csharp
public static class SimpleExampleWithOutput
{
    [FunctionName("CopyQueueMessage")]
    public static void Run(
        [QueueTrigger("myqueue-items-source")] string myQueueItem, 
        [Queue("myqueue-items-destination")] out string myQueueItemCopy,
        TraceWriter log)
    {
        log.Info($"CopyQueueMessage function processed: {myQueueItem}");
        myQueueItemCopy = myQueueItem;
    }
}
```

### <a name="conversion-to-functionjson"></a>Převod na function.json

Vytvoří procesu sestavení *function.json* souboru ve složce funkce ve složce sestavení. Tento soubor není určen k upravovat přímo. Nelze změnit konfiguraci vazby nebo zakázat funkci úpravou tohoto souboru. 

Účelem tohoto souboru je poskytují informace, které řadič škálování pro [škálování rozhodnutí plánu spotřeby](functions-scale.md#how-the-consumption-plan-works). Z tohoto důvodu soubor má jenom informace o aktivační událost není vstupem nebo výstupem vazby.

Generovaný objekt *function.json* soubor obsahuje `configurationSource` vlastnost, která sděluje modulu runtime .NET atributy použít u vazeb, místo *function.json* konfigurace. Tady je příklad:

{"generatedBy": "Microsoft.NET.Sdk.Functions-1.0.0.0", "configurationSource": "atributy", "vazby": [{"typ": "queueTrigger", "queueName": "% název vstupní fronty %", "název": "myQueueItem"}], "zakázáno": false, "Soubor_skriptu": ".. \\bin\\FunctionApp1.dll ","entryPoint":"FunctionApp1.QueueTrigger.Run"}

*Function.json* generování souboru se provádí pomocí balíčku NuGet [Microsoft\.NET\.Sdk\.funkce](http://www.nuget.org/packages/Microsoft.NET.Sdk.Functions). Zdrojový kód je k dispozici v úložišti GitHub [azure\-funkce\-vs\-sestavení\-sdk](https://github.com/Azure/azure-functions-vs-build-sdk).

## <a name="supported-types-for-bindings"></a>Podporované typy u vazeb

Každé vazby má svou vlastní podporované typy; pro instanci atribut aktivační události objektu blob lze použít na řetězcový parametr, parametr objektů POCO, `CloudBlockBlob` parametr, nebo libovolná z některé další podporované typy. [Článku vazby u vazeb objektů blob](functions-bindings-storage-blob.md#trigger---usage) zobrazí všechny podporované typy parametrů. Další informace najdete v tématu [triggerů a vazeb](functions-triggers-bindings.md) a [vazby referenční dokumentace pro každý typ vazby](functions-triggers-bindings.md#next-steps).

[!INCLUDE [HTTP client best practices](../../includes/functions-http-client-best-practices.md)]

## <a name="binding-to-method-return-value"></a>Vytvoření vazby na návratovou hodnotu – Metoda

Můžete návratovou hodnotu metoda pro vazbu výstup, jak je znázorněno v následujícím příkladu:

```csharp
public static class ReturnValueOutputBinding
{
    [FunctionName("CopyQueueMessageUsingReturnValue")]
    [return: Queue("myqueue-items-destination")]
    public static string Run(
        [QueueTrigger("myqueue-items-source-2")] string myQueueItem,
        TraceWriter log)
    {
        log.Info($"C# function processed: {myQueueItem}");
        return myQueueItem;
    }
}
```

## <a name="writing-multiple-output-values"></a>Zápis více hodnot výstup

K vytvoření více hodnot pro vazbu výstup, použijte [ `ICollector` ](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/ICollector.cs) nebo [ `IAsyncCollector` ](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/IAsyncCollector.cs) typy. Tyto typy jsou jen pro zápis kolekce, které se zapisují do výstupu vazby po dokončení metody.

Tento příklad zapisuje více fronty zpráv do fronty ke stejné pomocí `ICollector`:

```csharp
public static class ICollectorExample
{
    [FunctionName("CopyQueueMessageICollector")]
    public static void Run(
        [QueueTrigger("myqueue-items-source-3")] string myQueueItem,
        [Queue("myqueue-items-destination")] ICollector<string> myQueueItemCopy,
        TraceWriter log)
    {
        log.Info($"C# function processed: {myQueueItem}");
        myQueueItemCopy.Add($"Copy 1: {myQueueItem}");
        myQueueItemCopy.Add($"Copy 2: {myQueueItem}");
    }
}
```

## <a name="logging"></a>Protokolování

Do protokolu výstup váš datový proud protokolů v jazyce C#, zahrnují argument typu `TraceWriter`. Doporučujeme, abyste pojmenujte ji `log`. Nepoužívejte `Console.Write` v Azure Functions. 

`TraceWriter`je definována v [Azure WebJobs SDK](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.Host/TraceWriter.cs). Úroveň protokolu pro `TraceWriter` se dá nakonfigurovat v [host.json](functions-host-json.md).

```csharp
public static class SimpleExample
{
    [FunctionName("QueueTrigger")]
    public static void Run(
        [QueueTrigger("myqueue-items")] string myQueueItem, 
        TraceWriter log)
    {
        log.Info($"C# function processed: {myQueueItem}");
    }
} 
```

> [!NOTE]
> Informace o novější rozhraní protokolování, který můžete použít místo `TraceWriter`, najdete v části [zápisu protokolů v C# funkce](functions-monitoring.md#write-logs-in-c-functions) v **monitorování Azure Functions** článku.

## <a name="async"></a>Asynchronní

Chcete-li funkci asynchronní, použijte `async` – klíčové slovo a vraťte se `Task` objektu.

```csharp
public static class AsyncExample
{
    [FunctionName("BlobCopy")]
    public static async Task RunAsync(
        [BlobTrigger("sample-images/{blobName}")] Stream blobInput,
        [Blob("sample-images-copies/{blobName}", FileAccess.Write)] Stream blobOutput,
        CancellationToken token,
        TraceWriter log)
    {
        log.Info($"BlobCopy function processed.");
        await blobInput.CopyToAsync(blobOutput, 4096, token);
    }
}
```

## <a name="cancellation-tokens"></a>Zrušení tokenů

Některé operace vyžadují řádné vypnutí. I když je vždy nejlepší napsat kód, který dokáže zpracovat chybám, v případech, ve které chcete zpracovávat požadavky vypnutí definovat [CancellationToken](https://msdn.microsoft.com/library/system.threading.cancellationtoken.aspx) zadali argument.  A `CancellationToken` je k dispozici signál, že se aktivuje vypnutí hostitele.

```csharp
public static class CancellationTokenExample
{
    [FunctionName("BlobCopy")]
    public static async Task RunAsync(
        [BlobTrigger("sample-images/{blobName}")] Stream blobInput,
        [Blob("sample-images-copies/{blobName}", FileAccess.Write)] Stream blobOutput,
        CancellationToken token)
    {
        await blobInput.CopyToAsync(blobOutput, 4096, token);
    }
}
```

## <a name="environment-variables"></a>Proměnné prostředí

Proměnné prostředí nebo nastavení hodnoty aplikace, použijte `System.Environment.GetEnvironmentVariable`, jak ukazuje následující příklad kódu:

```csharp
public static class EnvironmentVariablesExample
{
    [FunctionName("GetEnvironmentVariables")]
    public static void Run([TimerTrigger("0 */5 * * * *")]TimerInfo myTimer, TraceWriter log)
    {
        log.Info($"C# Timer trigger function executed at: {DateTime.Now}");
        log.Info(GetEnvironmentVariable("AzureWebJobsStorage"));
        log.Info(GetEnvironmentVariable("WEBSITE_SITE_NAME"));
    }

    public static string GetEnvironmentVariable(string name)
    {
        return name + ": " +
            System.Environment.GetEnvironmentVariable(name, EnvironmentVariableTarget.Process);
    }
}
```

## <a name="binding-at-runtime"></a>Vazba za běhu

V jazyce C# a jinými jazyky rozhraní .NET, můžete použít [imperativní](https://en.wikipedia.org/wiki/Imperative_programming) vazby vzor, nikoli [ *deklarativní* ](https://en.wikipedia.org/wiki/Declarative_programming) vazby v atributech. Imperativní vazba je užitečné, když vázané parametry muset počítaný v době běhu spíše než návrhu. Tento vzor je možné svázat podporované vstup a výstup vazby na průběžně ve vašem kódu funkce.

Definujte imperativní vazby následujícím způsobem:

- **Nechcete** zahrnout atribut podpis funkce pro vaše požadované imperativní vazby.
- Předejte jí vstupní parametr [ `Binder binder` ](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.Host/Bindings/Runtime/Binder.cs) nebo [ `IBinder binder` ](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/IBinder.cs).
- K provedení vazby dat použijte následující vzor C#.

  ```cs
  using (var output = await binder.BindAsync<T>(new BindingTypeAttribute(...)))
  {
      ...
  }
  ```

  `BindingTypeAttribute`je atribut rozhraní .NET, která definuje vazbu, a `T` je vstupní nebo výstupní typ, který podporuje tento typ vazby. `T`nelze `out` typ parametru (například `out JObject`). Například v tabulce Mobile Apps výstupu vazba podporuje [šest výstup typy](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.MobileApps/MobileTableAttribute.cs#L17-L22), ale můžete použít pouze [ICollector<T> ](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/ICollector.cs) nebo [IAsyncCollector<T> ](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/IAsyncCollector.cs)imperativní vazbou.

### <a name="single-attribute-example"></a>Příklad jeden atribut

Následující příklad kódu vytvoří [objektu blob Storage výstup vazby](functions-bindings-storage-blob.md#input--output) s objektem blob cestu, která je definována v době běhu, pak zapíše řetězec do objektu blob.

```cs
public static class IBinderExample
{
    [FunctionName("CreateBlobUsingBinder")]
    public static void Run(
        [QueueTrigger("myqueue-items-source-4")] string myQueueItem,
        IBinder binder,
        TraceWriter log)
    {
        log.Info($"CreateBlobUsingBinder function processed: {myQueueItem}");
        using (var writer = binder.Bind<TextWriter>(new BlobAttribute(
                    $"samples-output/{myQueueItem}", FileAccess.Write)))
        {
            writer.Write("Hello World!");
        };
    }
}
```

[BlobAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/BlobAttribute.cs) definuje [objektu blob Storage](functions-bindings-storage-blob.md) vstupem nebo výstupem vazby a [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter.aspx) je typ vazby podporované výstup.

### <a name="multiple-attribute-example"></a>Příklad více atributů

V předchozím příkladu získá nastavení aplikace pro aplikaci funkce hlavní účet připojovacího řetězce úložiště (což je `AzureWebJobsStorage`). Můžete zadat nastavení vlastní aplikace použít pro účet úložiště tak, že přidáte [StorageAccountAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/StorageAccountAttribute.cs) a předání atribut pole do `BindAsync<T>()`. Použití `Binder` parametr není `IBinder`.  Příklad:

```cs
public static class IBinderExampleMultipleAttributes
{
    [FunctionName("CreateBlobInDifferentStorageAccount")]
    public async static Task RunAsync(
            [QueueTrigger("myqueue-items-source-binder2")] string myQueueItem,
            Binder binder,
            TraceWriter log)
    {
        log.Info($"CreateBlobInDifferentStorageAccount function processed: {myQueueItem}");
        var attributes = new Attribute[]
        {
        new BlobAttribute($"samples-output/{myQueueItem}", FileAccess.Write),
        new StorageAccountAttribute("MyStorageAccount")
        };
        using (var writer = await binder.BindAsync<TextWriter>(attributes))
        {
            await writer.WriteAsync("Hello World!!");
        }
    }
}
```

## <a name="triggers-and-bindings"></a>Triggery a vazby 

Následující tabulka uvádí aktivační události a vazba atributy, které jsou k dispozici v projektu knihovny tříd Azure Functions. Všechny atributy jsou v oboru názvů `Microsoft.Azure.WebJobs`.

| Trigger | Vstup | Výstup|
|------   | ------    | ------  |
| [BlobTrigger](functions-bindings-storage-blob.md#trigger---attributes)| [Objekt BLOB](functions-bindings-storage-blob.md#input--output---attributes)| [Objekt BLOB](functions-bindings-storage-blob.md#input--output---attributes)|
| [CosmosDBTrigger](functions-bindings-documentdb.md#trigger---attributes)| [DocumentDB](functions-bindings-documentdb.md#input---attributes)| [DocumentDB](functions-bindings-documentdb.md#output---attributes) |
| [EventHubTrigger](functions-bindings-event-hubs.md#trigger---attributes)|| [Centrum EventHub](functions-bindings-event-hubs.md#output---attributes) |
| [HTTPTrigger](functions-bindings-http-webhook.md#trigger---attributes)|||
| [QueueTrigger](functions-bindings-storage-queue.md#trigger---attributes)|| [Fronty](functions-bindings-storage-queue.md#output---attributes) |
| [ServiceBusTrigger](functions-bindings-service-bus.md#trigger---attributes)|| [Sběrnice](functions-bindings-service-bus.md#output---attributes) |
| [TimerTrigger](functions-bindings-timer.md#attributes) | ||
| |[ApiHubFile](functions-bindings-external-file.md)| [ApiHubFile](functions-bindings-external-file.md)|
| |[MobileTable](functions-bindings-mobile-apps.md#input---attributes)| [MobileTable](functions-bindings-mobile-apps.md#output---attributes) | 
| |[Tabulka](functions-bindings-storage-table.md#input---attributes)| [Tabulka](functions-bindings-storage-table.md#output---attributes)  | 
| ||[NotificationHub](functions-bindings-notification-hubs.md#attributes) |
| ||[SendGrid](functions-bindings-sendgrid.md#attributes) |
| ||[Twilio](functions-bindings-twilio.md#attributes)| 

## <a name="next-steps"></a>Další postup

> [!div class="nextstepaction"]
> [Další informace o triggerů a vazeb](functions-triggers-bindings.md)

> [!div class="nextstepaction"]
> [Další informace o osvědčených postupech pro Azure Functions](functions-best-practices.md)
