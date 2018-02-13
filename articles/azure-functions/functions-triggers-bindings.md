---
title: "Triggerů a vazeb v Azure Functions"
description: "Další informace o použití triggerů a vazeb v Azure Functions k připojení vašeho provádění kódu k online události a cloudové služby."
services: functions
documentationcenter: na
author: ggailey777
manager: cfowler
editor: 
tags: 
keywords: "funkce azure, funkce, zpracování událostí, webhook, dynamické výpočty, architektura bez serverů"
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 11/21/2017
ms.author: glenga
ms.openlocfilehash: 90a192f58f0e4b285f7aece8a3555c08df051f38
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/09/2018
---
# <a name="azure-functions-triggers-and-bindings-concepts"></a>Azure funkce triggerů a vazeb koncepty

Tento článek je koncepční přehled triggerů a vazeb v Azure Functions. Funkce, které jsou společné pro všechny vazby a všechny podporované jazyky jsou popsané v tomto poli.

## <a name="overview"></a>Přehled

A *aktivační událost* definuje způsob volání funkce. Funkce musí mít přesně jeden aktivační události. Aktivační události mají související data, která je obvykle datové části, která aktivuje funkce.

Vstup a výstup *vazby* poskytnout deklarativní způsob, jak se připojit k datům z vašeho kódu. Vazby jsou volitelné a funkci můžete mít více vstup a výstup vazby. 

Triggerů a vazeb umožňují vyhnout hardcoding podrobnosti o služby, které pracujete s. Funkce je přijímá data (například obsah zprávy fronty) v parametry funkce. Odesílat data (například pro vytvoření zprávy fronty) pomocí návratovou hodnotu funkce, `out` parametr nebo [objekt kolekce](functions-reference-csharp.md#writing-multiple-output-values).

Při vývoji funkce pomocí portálu Azure, triggerů a vazeb konfigurované v *function.json* souboru. Na portálu poskytuje uživatelské rozhraní pro tuto konfiguraci, ale můžete upravit soubor přímo změna **pokročilé editor**.

Při vývoji funkce pomocí sady Visual Studio k vytvoření knihovny tříd, konfigurujete triggerů a vazeb a architekturu metody a parametry s atributy.

## <a name="supported-bindings"></a>Podporované vazby

[!INCLUDE [Full bindings table](../../includes/functions-bindings.md)]

Informace o tom, které jsou ve verzi preview vazby, nebo jsou schváleny pro použití v provozním prostředí najdete v tématu [podporované jazyky](supported-languages.md).

## <a name="example-queue-trigger-and-table-output-binding"></a>Příklad: Aktivace fronty a tabulky Výstupní vazby

Předpokládejme, že chcete k zápisu nového řádku do Azure Table storage, vždy, když v rámci Azure Queue storage se objeví nová zpráva. Tento scénář může být implementovaná pomocí Azure Queue aktivační událost úložiště a Azure Table storage výstupní vazby. 

Tady je *function.json* souboru pro tento scénář. 

```json
{
  "bindings": [
    {
      "name": "order",
      "type": "queueTrigger",
      "direction": "in",
      "queueName": "myqueue-items",
      "connection": "MY_STORAGE_ACCT_APP_SETTING"
    },
    {
      "name": "$return",
      "type": "table",
      "direction": "out",
      "tableName": "outTable",
      "connection": "MY_TABLE_STORAGE_ACCT_APP_SETTING"
    }
  ]
}
```

Prvním elementem v `bindings` pole je aktivační událost fronty úložiště. `type` a `direction` vlastnosti identifikovat aktivační událost. `name` Vlastnost identifikuje parametr funkce, která bude přijímat obsah zprávy fronty. Název fronty k monitorování se `queueName`, a připojovací řetězec je v nastavení aplikace identifikovaný `connection`.

Druhý prvkem v `bindings` pole je Azure Table Storage výstup vazby. `type` a `direction` vlastnosti identifikovat vazby. `name` Vlastnost určuje, jak funkce zajistí řádku nové tabulky v tomto případě pomocí funkce návratovou hodnotu. Název tabulky se `tableName`, a připojovací řetězec je v nastavení aplikace identifikovaný `connection`.

K zobrazení a úprava obsahu *function.json* na portálu Azure klikněte na tlačítko **pokročilé editor** možnost **integrací** kartě funkce.

> [!NOTE]
> Hodnota `connection` je název nastavení aplikace, který obsahuje připojovací řetězec, ne samotný připojovací řetězec. Vazby používat připojení řetězce, které jsou uložené v nastavení aplikace vynutit nejlepší praxi, který *function.json* neobsahuje tajné klíče služby.

Zde je C# kód skriptu, který funguje s této aktivační události a vazby. Všimněte si, že je název parametru, který poskytuje obsah zprávy fronty `order`; tento název je povinný, protože `name` hodnoty vlastností v *function.json* je`order` 

```cs
#r "Newtonsoft.Json"

using Newtonsoft.Json.Linq;

// From an incoming queue message that is a JSON object, add fields and write to Table storage
// The method return value creates a new row in Table Storage
public static Person Run(JObject order, TraceWriter log)
{
    return new Person() { 
            PartitionKey = "Orders", 
            RowKey = Guid.NewGuid().ToString(),  
            Name = order["Name"].ToString(),
            MobileNumber = order["MobileNumber"].ToString() };  
}
 
public class Person
{
    public string PartitionKey { get; set; }
    public string RowKey { get; set; }
    public string Name { get; set; }
    public string MobileNumber { get; set; }
}
```

Pomocí funkce JavaScript lze použít ke stejnému souboru function.json:

```javascript
// From an incoming queue message that is a JSON object, add fields and write to Table Storage
// The second parameter to context.done is used as the value for the new row
module.exports = function (context, order) {
    order.PartitionKey = "Orders";
    order.RowKey = generateRandomId(); 

    context.done(null, order);
};

function generateRandomId() {
    return Math.random().toString(36).substring(2, 15) +
        Math.random().toString(36).substring(2, 15);
}
```

Knihovny tříd, stejné aktivační události a informace o vazbě &mdash; názvu tabulky a fronty, účty úložiště, funkce parametry pro vstup a výstup &mdash; poskytuje atributy:

```csharp
 public static class QueueTriggerTableOutput
 {
     [FunctionName("QueueTriggerTableOutput")]
     [return: Table("outTable", Connection = "MY_TABLE_STORAGE_ACCT_APP_SETTING")]
     public static Person Run(
         [QueueTrigger("myqueue-items", Connection = "MY_STORAGE_ACCT_APP_SETTING")]JObject order, 
         TraceWriter log)
     {
         return new Person() {
                 PartitionKey = "Orders",
                 RowKey = Guid.NewGuid().ToString(),
                 Name = order["Name"].ToString(),
                 MobileNumber = order["MobileNumber"].ToString() };
     }
 }

 public class Person
 {
     public string PartitionKey { get; set; }
     public string RowKey { get; set; }
     public string Name { get; set; }
     public string MobileNumber { get; set; }
 }
```

## <a name="binding-direction"></a>Směr vazby

Mají všechny triggerů a vazeb `direction` vlastnost *function.json* souboru:

- Pro aktivační události směr je vždy`in`
- Vstupní a výstupní vazby používat `in` a`out`
- Některé vazby podporují speciální směr `inout`. Pokud používáte `inout`, jenom **pokročilé editor** je k dispozici v **integrací** kartě.

Při použití [atributy v knihovně tříd](functions-dotnet-class-library.md) konfigurace triggerů a vazeb, směr uvedené v atributu konstruktoru nebo odvozené od typu parametru.

## <a name="using-the-function-return-type-to-return-a-single-output"></a>Použití návratový typ funkce pro vrácení jediného výstupu

Předchozí příklad ukazuje, jak používat funkce návratovou hodnotu k poskytování výstup vazbu, která je určena v *function.json* pomocí speciální hodnoty `$return` pro `name` vlastnost. (Toto je jediná hodnota podporovaná v jazycích, které mají návratovou hodnotu, jako je skript jazyka C#, JavaScript a F #.) Pokud funkce má několik vazeb výstup, použijte `$return` pouze pro jeden z výstupu vazby. 

```json
// excerpt of function.json
{
    "name": "$return",
    "type": "blob",
    "direction": "out",
    "path": "output-container/{id}"
}
```

Příklady níže ukazují jak návratové typy se používají s výstup vazeb v C# skript, JavaScript a F #.

```cs
// C# example: use method return value for output binding
public static string Run(WorkItem input, TraceWriter log)
{
    string json = string.Format("{{ \"id\": \"{0}\" }}", input.Id);
    log.Info($"C# script processed queue message. Item={json}");
    return json;
}
```

```cs
// C# example: async method, using return value for output binding
public static Task<string> Run(WorkItem input, TraceWriter log)
{
    string json = string.Format("{{ \"id\": \"{0}\" }}", input.Id);
    log.Info($"C# script processed queue message. Item={json}");
    return Task.FromResult(json);
}
```

```javascript
// JavaScript: return a value in the second parameter to context.done
module.exports = function (context, input) {
    var json = JSON.stringify(input);
    context.log('Node.js script processed queue message', json);
    context.done(null, json);
}
```

```fsharp
// F# example: use return value for output binding
let Run(input: WorkItem, log: TraceWriter) =
    let json = String.Format("{{ \"id\": \"{0}\" }}", input.Id)   
    log.Info(sprintf "F# script processed queue message '%s'" json)
    json
```

## <a name="binding-datatype-property"></a>Vlastnost dataType vazby

V rozhraní .NET použijte k definování typu dat pro vstupní data, typ parametru. Například použijte `string` k vytvoření vazby na text aktivační procedury fronty, bajtové pole pro čtení jako binární a vlastní typ k deserializaci na objekt objektů POCO.

Pro jazyky, které jsou zadány dynamicky například JavaScript, použijte `dataType` vlastnost *function.json* souboru. Například číst obsah požadavku HTTP v binárním formátu, nastavte `dataType` k `binary`:

```json
{
    "type": "httpTrigger",
    "name": "req",
    "direction": "in",
    "dataType": "binary"
}
```

Další možnosti pro `dataType` jsou `stream` a `string`.

## <a name="resolving-app-settings"></a>Řešení nastavení aplikace

Jako osvědčený postup tajné klíče a připojovací řetězce musí být řízen pomocí nastavení aplikace, nikoli konfigurační soubory. To omezuje přístup na těchto tajných klíčů a umožňuje bezpečné uložení *function.json* v úložišti veřejné zdroj ovládacího prvku.

Nastavení aplikace jsou užitečné také vždy, když chcete změnit konfiguraci na základě prostředí. Například v testovacím prostředí, můžete monitorovat jiný kontejner fronty nebo objekt blob úložiště.

Nastavení aplikace jsou vyřešeny vždy, když hodnota je uzavřena mezi znaky procenta, jako například `%MyAppSetting%`. Všimněte si, že `connection` vlastnost triggerů a vazeb je zvláštní případ a automaticky vyřeší hodnoty jako nastavení aplikace. 

Následující příklad je aktivační událost INSTEAD Azure Queue Storage, který používá nastavení aplikace `%input-queue-name%` k definování fronty k aktivaci na.

```json
{
  "bindings": [
    {
      "name": "order",
      "type": "queueTrigger",
      "direction": "in",
      "queueName": "%input-queue-name%",
      "connection": "MY_STORAGE_ACCT_APP_SETTING"
    }
  ]
}
```

Můžete použít ve stejný přístup v knihovny tříd:

```csharp
[FunctionName("QueueTrigger")]
public static void Run(
    [QueueTrigger("%input-queue-name%")]string myQueueItem, 
    TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");
}
```

## <a name="trigger-metadata-properties"></a>Metadata vlastnosti aktivační události

Kromě datová poskytované aktivační událost (například zprávy ve frontě, která aktivuje funkci) zadejte mnoho aktivační události hodnoty dalších metadat. Tyto hodnoty lze použít jako vstupní parametry v C# a F # nebo vlastnosti na `context.bindings` objektu v jazyce JavaScript. 

Například aktivační procedury fronty Azure storage podporuje následující vlastnosti:

* QueueTrigger - aktivován obsah zprávy, pokud platný řetězec
* DequeueCount
* ExpirationTime
* ID
* InsertionTime
* NextVisibleTime
* PopReceipt

Tyto hodnoty metadata jsou dostupné v *function.json* vlastnosti souboru. Předpokládejme například, použít aktivační událost fronty a zprávy ve frontě obsahuje název objektu blob, které chcete číst. V *function.json* souboru, můžete použít `queueTrigger` metadata vlastnost v objektu blob `path` vlastnost, jak je znázorněno v následujícím příkladu:

```json
  "bindings": [
    {
      "name": "myQueueItem",
      "type": "queueTrigger",
      "queueName": "myqueue-items",
      "connection": "MyStorageConnection",
    },
    {
      "name": "myInputBlob",
      "type": "blob",
      "path": "samples-workitems/{queueTrigger}",
      "direction": "in",
      "connection": "MyStorageConnection"
    }
  ]
```

Podrobnosti o vlastnosti metadat pro jednotlivé aktivační události jsou popsané v článku na odpovídající odkaz. Příklad, naleznete v části [frontě aktivační události metadata](functions-bindings-storage-queue.md#trigger---message-metadata). Je také dostupná v dokumentaci **integrací** karta portálu v **dokumentace** části níže oblast konfigurace vazby.  

## <a name="binding-expressions-and-patterns"></a>Výrazy vazba a vzory

Jedním z nejúčinnějších funkce triggerů a vazeb je *vazby výrazy*. V konfiguraci pro vazbu můžete definovat vzor výrazy, které se dají použít v jiné vazby nebo kódu. Metadata aktivační události lze také ve výrazech vazby, jak je uvedeno v předchozí části.

Předpokládejme například, kterou chcete změnit velikost bitové kopie v konkrétním kontejneru objektů blob úložiště, podobně jako **Úprava velikosti obrázku** šablony v **novou funkci** stránce portálu Azure (najdete v článku **ukázky**  scénář). 

Tady je *function.json* definice:

```json
{
  "bindings": [
    {
      "name": "image",
      "type": "blobTrigger",
      "path": "sample-images/{filename}",
      "direction": "in",
      "connection": "MyStorageConnection"
    },
    {
      "name": "imageSmall",
      "type": "blob",
      "path": "sample-images-sm/{filename}",
      "direction": "out",
      "connection": "MyStorageConnection"
    }
  ],
}
```

Všimněte si, že `filename` parametr se používá v definici aktivační události objektu blob a objekt blob výstup vazby. Tento parametr můžete použít také v kódu funkce.

```csharp
// C# example of binding to {filename}
public static void Run(Stream image, string filename, Stream imageSmall, TraceWriter log)  
{
    log.Info($"Blob trigger processing: {filename}");
    // ...
} 
```

<!--TODO: add JavaScript example -->
<!-- Blocked by bug https://github.com/Azure/Azure-Functions/issues/248 -->

Stejné možnost používat výrazy vazba a vzory se vztahuje na atributy v knihovny tříd. Zde je například bitová kopie Změna velikosti funkce v knihovně tříd:

```csharp
[FunctionName("ResizeImage")]
[StorageAccount("AzureWebJobsStorage")]
public static void Run(
    [BlobTrigger("sample-images/{name}")] Stream image, 
    [Blob("sample-images-sm/{name}", FileAccess.Write)] Stream imageSmall, 
    [Blob("sample-images-md/{name}", FileAccess.Write)] Stream imageMedium)
{
    var imageBuilder = ImageResizer.ImageBuilder.Current;
    var size = imageDimensionsTable[ImageSize.Small];

    imageBuilder.Build(image, imageSmall,
        new ResizeSettings(size.Item1, size.Item2, FitMode.Max, null), false);

    image.Position = 0;
    size = imageDimensionsTable[ImageSize.Medium];

    imageBuilder.Build(image, imageMedium,
        new ResizeSettings(size.Item1, size.Item2, FitMode.Max, null), false);
}

public enum ImageSize { ExtraSmall, Small, Medium }

private static Dictionary<ImageSize, (int, int)> imageDimensionsTable = new Dictionary<ImageSize, (int, int)>() {
    { ImageSize.ExtraSmall, (320, 200) },
    { ImageSize.Small,      (640, 400) },
    { ImageSize.Medium,     (800, 600) }
};
```

### <a name="create-guids"></a>Vytvoření identifikátory GUID

`{rand-guid}` Vazby výraz vytvoří identifikátor GUID. Následující příklad používá k vytvoření název objektu blob jedinečný identifikátor GUID: 

```json
{
  "type": "blob",
  "name": "blobOutput",
  "direction": "out",
  "path": "my-output-container/{rand-guid}"
}
```

### <a name="current-time"></a>Aktuální čas

Výraz vazby `DateTime` přeloží na `DateTime.UtcNow`.

```json
{
  "type": "blob",
  "name": "blobOutput",
  "direction": "out",
  "path": "my-output-container/{DateTime}"
}
```

## <a name="bind-to-custom-input-properties"></a>Vytvoření vazby na vlastní vstupní vlastnosti

Vazba výrazy můžete taky odkazovat vlastnosti, které jsou definovány v datové části aktivační událost sám sebe. Například můžete dynamicky vázat na soubor úložiště objektů blob ze součástí webhook, jehož název souboru.

Například následující *function.json* používá vlastnost s názvem `BlobName` z datové části aktivační události:

```json
{
  "bindings": [
    {
      "name": "info",
      "type": "httpTrigger",
      "direction": "in",
      "webHookType": "genericJson"
    },
    {
      "name": "blobContents",
      "type": "blob",
      "direction": "in",
      "path": "strings/{BlobName}",
      "connection": "AzureWebJobsStorage"
    },
    {
      "name": "res",
      "type": "http",
      "direction": "out"
    }
  ]
}
```

K tomu v C# a F #, je nutné zadat objektů POCO, která definuje pole, která bude deserializovat v datové části aktivační události.

```csharp
using System.Net;

public class BlobInfo
{
    public string BlobName { get; set; }
}
  
public static HttpResponseMessage Run(HttpRequestMessage req, BlobInfo info, string blobContents)
{
    if (blobContents == null) {
        return req.CreateResponse(HttpStatusCode.NotFound);
    } 

    return req.CreateResponse(HttpStatusCode.OK, new {
        data = $"{blobContents}"
    });
}
```

V jazyce JavaScript se provádí automaticky deserializaci JSON a vlastností lze používat přímo.

```javascript
module.exports = function (context, info) {
    if ('BlobName' in info) {
        context.res = {
            body: { 'data': context.bindings.blobContents }
        }
    }
    else {
        context.res = {
            status: 404
        };
    }
    context.done();
}
```

## <a name="configuring-binding-data-at-runtime"></a>Konfigurace vazba dat za běhu

V jazyce C# a jinými jazyky rozhraní .NET, můžete použít imperativní vazby vzoru oproti deklarativní vazeb v *function.json* a atributy. Imperativní vazba je užitečné, když vázané parametry muset počítaný v době běhu spíše než návrhu. Další informace najdete v tématu [vazby za běhu prostřednictvím imperativní vazby](functions-reference-csharp.md#imperative-bindings) v referenci vývojáře jazyka C#.

## <a name="functionjson-file-schema"></a>Schéma souboru Function.JSON

*Function.json* schéma souboru je k dispozici na [http://json.schemastore.org/function](http://json.schemastore.org/function).

## <a name="handling-binding-errors"></a>Zpracování chyb vazby

[!INCLUDE [bindings errors intro](../../includes/functions-bindings-errors-intro.md)]

Odkazy na všechny relevantní chyby pro různé služby nepodporuje funkce, najdete v článku [vazby kódy chyb](functions-bindings-error-pages.md#binding-error-codes) části [zpracování chyb Azure Functions](functions-bindings-error-pages.md) přehledu.  

## <a name="next-steps"></a>Další postup

Další informace o konkrétní vazbu najdete v následujících článcích:

- [HTTP a webhooky](functions-bindings-http-webhook.md)
- [Časovač](functions-bindings-timer.md)
- [Queue Storage](functions-bindings-storage-queue.md)
- [Blob Storage](functions-bindings-storage-blob.md)
- [Table Storage](functions-bindings-storage-table.md)
- [Centrum událostí](functions-bindings-event-hubs.md)
- [Service Bus](functions-bindings-service-bus.md)
- [Azure Cosmos DB](functions-bindings-cosmosdb.md)
- [Microsoft Graph](functions-bindings-microsoft-graph.md)
- [SendGrid](functions-bindings-sendgrid.md)
- [Twilio](functions-bindings-twilio.md)
- [Notification Hubs](functions-bindings-notification-hubs.md)
- [Mobile Apps](functions-bindings-mobile-apps.md)
- [Externí soubor](functions-bindings-external-file.md)
