---
title: "Práce s triggerů a vazeb v Azure Functions"
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
ms.openlocfilehash: ab5550ee0c057c9abc4b706929d780a495aaff65
ms.sourcegitcommit: 4256ebfe683b08fedd1a63937328931a5d35b157
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/23/2017
---
# <a name="azure-functions-triggers-and-bindings-concepts"></a>Azure funkce triggerů a vazeb koncepty
Azure Functions umožňuje psaní kódu v reakci na události v Azure a dalším službám prostřednictvím *aktivační události* a *vazby*. Tento článek obsahuje přehled služby aktivačních událostí a vazby pro všechny podporované programovací jazyky. Tady jsou popsané funkce, které jsou společné pro všechny vazby.

## <a name="overview"></a>Přehled

Triggerů a vazeb jsou deklarativní způsob, jak definovat, jak je volána funkce a co data funguje s. A *aktivační událost* definuje způsob volání funkce. Funkce musí mít přesně jeden aktivační události. Aktivační události mají související data, která je obvykle datové části, která aktivuje funkce.

Vstup a výstup *vazby* poskytnout deklarativní způsob, jak se připojit k datům z vašeho kódu. Podobně jako u aktivačních událostí, je zadat připojovací řetězce a další vlastnosti v konfiguraci funkce. Vazby jsou volitelné a funkci můžete mít více vstup a výstup vazby. 

Pomocí triggerů a vazeb, můžete napsat kód, který je instalace a další obecná závislé podrobnosti o službách, pomocí které komunikuje. Dat pocházejících z služby jednoduše stát vstupní hodnoty kódu funkce. Chcete-li výstupní data do jiné služby (jako je vytvoření nového řádku v Azure Table Storage), použijte návratovou hodnotu metody. Nebo pokud potřebujete výstup více hodnot, použijte pomocný objekt. Mít triggerů a vazeb **název** vlastnost, která je identifikátor používáte ve vašem kódu pro přístup k vazby.

Můžete nakonfigurovat triggerů a vazeb v **integrací** na portálu Azure Functions. V pozadí, uživatelské rozhraní upraví soubor s názvem *function.json* soubor v adresáři funkce. Tento soubor můžete upravit změnou na **pokročilé editor**.

## <a name="supported-bindings"></a>Podporované vazby

[!INCLUDE [Full bindings table](../../includes/functions-bindings.md)]

Informace o tom, které jsou ve verzi preview vazby, nebo jsou schváleny pro použití v provozním prostředí najdete v tématu [podporované jazyky](supported-languages.md).

## <a name="example-queue-trigger-and-table-output-binding"></a>Příklad: Aktivace fronty a tabulky Výstupní vazby

Předpokládejme, že chcete k zápisu nového řádku do Azure Table Storage, vždy, když v Azure Queue Storage se objeví nová zpráva. Tento scénář může být implementovaná pomocí Azure Queue aktivační události a Azure Table Storage výstupní vazby. 

Aktivační událost INSTEAD Azure Queue Storage vyžaduje následující informace v **integrací** karty:

* Název nastavení aplikace, který obsahuje připojovací řetězec pro účet úložiště Azure pro Azure Queue Storage
* Název fronty
* Identifikátor ve vašem kódu číst obsah zprávy ve frontě, jako například `order`.

Zapsat do Azure Table Storage, použijte vazbu výstup s následujícími podrobnostmi:

* Název nastavení aplikace, který obsahuje připojovací řetězec pro účet úložiště Azure pro úložiště tabulek Azure
* Název tabulky
* Identifikátor ve vašem kódu k vytvoření výstupní položky nebo návratovou hodnotou z funkce.

Vazby použijte řetězce připojení s hodnotami uloženými v nastavení aplikace vynutit nejlepší praxi, který *function.json* neobsahuje tajné klíče služby a místo toho jednoduše obsahovat názvy nastavení aplikace.

Potom pomocí identifikátorů, které jste zadali pro integraci s Azure Storage v kódu.

```cs
#r "Newtonsoft.Json"

using Newtonsoft.Json.Linq;

// From an incoming queue message that is a JSON object, add fields and write to Table Storage
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

Tady je *function.json* odpovídající předchozí kód. Všimněte si, že stejnou konfiguraci můžete použít, bez ohledu na jazyk implementace funkce.

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
K zobrazení a úprava obsahu *function.json* na portálu Azure klikněte na tlačítko **pokročilé editor** možnost **integrací** kartě funkce.

Další příklady kódu a informace o integraci s Azure Storage najdete v tématu [Azure Functions triggerů a vazeb pro Azure Storage](functions-bindings-storage.md).

## <a name="binding-direction"></a>Směr vazby

Mají všechny triggerů a vazeb `direction` vlastnost *function.json* souboru:

- Pro aktivační události směr je vždy`in`
- Vstupní a výstupní vazby používat `in` a`out`
- Některé vazby podporují speciální směr `inout`. Pokud používáte `inout`, jenom **pokročilé editor** je k dispozici v **integrací** kartě.

## <a name="using-the-function-return-type-to-return-a-single-output"></a>Použití návratový typ funkce pro vrácení jediného výstupu

Předchozí příklad ukazuje, jak používat funkce návratovou hodnotu k poskytování výstup vazbu, což je dosaženo pomocí parametru speciální název `$return`. (Toto je jediná hodnota podporovaná v jazycích, které mají návratovou hodnotu, například C#, JavaScript a F #.) Pokud funkce má několik vazeb výstup, použijte `$return` pouze pro jeden z výstupu vazby. 

```json
// excerpt of function.json
{
    "name": "$return",
    "type": "blob",
    "direction": "out",
    "path": "output-container/{id}"
}
```

Příklady níže ukazují jak návratové typy se používají s výstup vazeb v C#, JavaScript a F #.

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

V rozhraní .NET použijte k definování datový typ pro vstupní data typy. Například použijte `string` k vytvoření vazby na text aktivační procedury fronty, bajtové pole pro čtení jako binární a vlastní typ k deserializaci na objekt objektů POCO.

Pro jazyky, které jsou zadány dynamicky například JavaScript, použijte `dataType` vlastnost v definici vazby. Například pokud chcete číst obsah požadavku HTTP v binárním formátu, použijte typ `binary`:

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

## <a name="trigger-metadata-properties"></a>Metadata vlastnosti aktivační události

Kromě datová poskytované aktivační událost (například zprávy ve frontě, která aktivuje funkci) zadejte mnoho aktivační události hodnoty dalších metadat. Tyto hodnoty lze použít jako vstupní parametry v C# a F # nebo vlastnosti na `context.bindings` objektu v jazyce JavaScript. 

Například aktivační procedury fronty Azure Storage podporuje následující vlastnosti:

* QueueTrigger - aktivován obsah zprávy, pokud platný řetězec
* DequeueCount
* ExpirationTime
* ID
* InsertionTime
* NextVisibleTime
* Vlastnosti PopReceipt

Podrobnosti o vlastnosti metadat pro jednotlivé aktivační události jsou popsané v odpovídajícího tématu. Je také dostupná v dokumentaci **integrací** karta portálu v **dokumentace** části níže oblast konfigurace vazby.  

Například vzhledem k tomu, že aktivační události objektu blob mají některé zpoždění, můžete použít aktivační procedury fronty ke spuštění funkce (viz [aktivační událost úložiště objektů Blob](functions-bindings-storage-blob.md#trigger)). Zprávy ve frontě by obsahovat název souboru objektů blob k aktivaci na. Pomocí `queueTrigger` vlastnost metadat, toto chování můžete zadat všechny v konfiguraci, nikoli kódu.

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

Metadata vlastnosti aktivační události lze také v *vazby výraz* pro jiné vazbu, jak je popsáno v následující části.

## <a name="binding-expressions-and-patterns"></a>Výrazy vazba a vzory

Jedním z nejúčinnějších funkce triggerů a vazeb je *vazby výrazy*. V rámci vaší vazba, můžete definovat vzor výrazy, které lze poté použít v jiné vazby nebo kódu. Aktivační událost metadata mohou sloužit také v vazby výrazy, jak je vidět v ukázce v předchozí části.

Předpokládejme například, kterou chcete změnit velikost bitové kopie v kontejneru konkrétní objektu blob úložiště, podobně jako **Úprava velikosti obrázku** šablony v **novou funkci** stránky. Přejděte na **novou funkci** -> jazyka **C#** -> scénář **ukázky** -> **ImageResizer CSharp**. 

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

Všimněte si, že `filename` parametr se používá v definici aktivační události objektu blob jak objektu blob výstup vazby. Tento parametr můžete použít také v kódu funkce.

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


### <a name="random-guids"></a>Náhodné identifikátory GUID
Azure Functions nabízí pohodlí syntaxe pro generování identifikátory GUID v vazby, prostřednictvím `{rand-guid}` vazby výraz. Následující příklad používá ke generování objektů blob jedinečný název toto: 

```json
{
  "type": "blob",
  "name": "blobOutput",
  "direction": "out",
  "path": "my-output-container/{rand-guid}"
}
```

### <a name="current-time"></a>Aktuální čas

Můžete použít výraz vazby `DateTime`, který přeloží na `DateTime.UtcNow`.

```json
{
  "type": "blob",
  "name": "blobOutput",
  "direction": "out",
  "path": "my-output-container/{DateTime}"
}
```

## <a name="bind-to-custom-input-properties-in-a-binding-expression"></a>Vytvoření vazby na vlastní vstupní vlastnosti ve výrazu vazby

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

V jazyce C# a jinými jazyky rozhraní .NET, můžete použít imperativní vazby vzoru oproti deklarativní vazeb v *function.json*. Imperativní vazba je užitečné, když vázané parametry muset počítaný v době běhu spíše než návrhu. Další informace najdete v tématu [vazby za běhu prostřednictvím imperativní vazby](functions-reference-csharp.md#imperative-bindings) v referenci vývojáře jazyka C#.

## <a name="functionjson-file-schema"></a>Schéma souboru Function.JSON

*Function.json* schéma souboru je k dispozici na [http://json.schemastore.org/function](http://json.schemastore.org/function).

## <a name="next-steps"></a>Další postup
Další informace o konkrétní vazbu najdete v následujících článcích:

- [HTTP a webhooky](functions-bindings-http-webhook.md)
- [Časovač](functions-bindings-timer.md)
- [Queue Storage](functions-bindings-storage-queue.md)
- [Blob Storage](functions-bindings-storage-blob.md)
- [Table Storage](functions-bindings-storage-table.md)
- [Centrum událostí](functions-bindings-event-hubs.md)
- [Service Bus](functions-bindings-service-bus.md)
- [Azure Cosmos DB](functions-bindings-documentdb.md)
- [Microsoft Graph](functions-bindings-microsoft-graph.md)
- [SendGrid](functions-bindings-sendgrid.md)
- [Twilio](functions-bindings-twilio.md)
- [Notification Hubs](functions-bindings-notification-hubs.md)
- [Mobile Apps](functions-bindings-mobile-apps.md)
- [Externí soubor](functions-bindings-external-file.md)
