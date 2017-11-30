---
title: Azure Cosmos DB vazby pro funkce
description: "Pochopit, jak používat Azure Cosmos DB triggerů a vazeb v Azure Functions."
services: functions
documentationcenter: na
author: ggailey777
manager: cfowler
editor: 
tags: 
keywords: "Funkce Azure, funkce zpracování událostí, dynamické výpočetní architektura bez serveru"
ms.service: functions; cosmos-db
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 11/21/2017
ms.author: glenga
ms.openlocfilehash: a725d6e08721107ddd83999dac85dddb88896ebf
ms.sourcegitcommit: cfd1ea99922329b3d5fab26b71ca2882df33f6c2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/30/2017
---
# <a name="azure-cosmos-db-bindings-for-azure-functions"></a>Azure Cosmos DB vazby pro Azure Functions

Tento článek vysvětluje, jak pracovat s [Azure Cosmos DB](..\cosmos-db\serverless-computing-database.md) vazeb v Azure Functions. Azure Functions podporuje aktivaci, vstup a výstup vazby pro Azure Cosmos DB.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="trigger"></a>Trigger

Aktivační událost DB Cosmos Azure používá [Azure Cosmos DB změnu kanálu](../cosmos-db/change-feed.md) tak, aby naslouchala na změny napříč oddíly. Aktivační událost vyžaduje druhé kolekci, která se používá k ukládání _zapůjčení_ přes oddíly.

Kolekce, který je monitorován a kolekce, která obsahuje zapůjčení musí být k dispozici pro aktivační událost pro práci.

## <a name="trigger---example"></a>Aktivační událost – příklad

Podívejte se na konkrétní jazyk příklad:

* [Předkompilované C#](#trigger---c-example)
* [Skript jazyka C#](#trigger---c-script-example)
* [JavaScript](#trigger---javascript-example)

### <a name="trigger---c-example"></a>Aktivační událost – příklad jazyka C#

Následující příklad ukazuje [předkompilovaných C# funkce](functions-dotnet-class-library.md) , aktivuje z konkrétní databáze a kolekce.

```cs
[FunctionName("DocumentUpdates")]
public static void Run(
    [CosmosDBTrigger("database", "collection", ConnectionStringSetting = "myCosmosDB")]
IReadOnlyList<Document> documents,
    TraceWriter log)
{
        log.Info("Documents modified " + documents.Count);
        log.Info("First document Id " + documents[0].Id);
}
```

### <a name="trigger---c-script"></a>Aktivační událost - C# skript

Následující příklad ukazuje, aktivační událost Cosmos DB vazby ve *function.json* souboru a [funkce skriptu jazyka C#](functions-reference-csharp.md) používající vazby. Funkce zapíše zprávy protokolu při změně záznamy Cosmos DB.

Zde je vazba dat v *function.json* souboru:

```json
{
  "type": "cosmosDBTrigger",
  "name": "documents",
  "direction": "in",
  "leaseCollectionName": "leases",
  "connectionStringSetting": "<connection-app-setting>",
  "databaseName": "Tasks",
  "collectionName": "Items",
  "createLeaseCollectionIfNotExists": true
}
```

Tady je kód skriptu jazyka C#:
 
```cs 
    #r "Microsoft.Azure.Documents.Client"
    using Microsoft.Azure.Documents;
    using System.Collections.Generic;
    using System;
    public static void Run(IReadOnlyList<Document> documents, TraceWriter log)
    {
        log.Verbose("Documents modified " + documents.Count);
        log.Verbose("First document Id " + documents[0].Id);
    }
```

### <a name="trigger---javascript"></a>Aktivační událost - JavaScript

Následující příklad ukazuje, aktivační událost Cosmos DB vazby ve *function.json* souboru a [funkce JavaScript, která](functions-reference-node.md) používající vazby. Funkce zapíše zprávy protokolu při změně záznamy Cosmos DB.

Zde je vazba dat v *function.json* souboru:

```json
{
  "type": "cosmosDBTrigger",
  "name": "documents",
  "direction": "in",
  "leaseCollectionName": "leases",
  "connectionStringSetting": "<connection-app-setting>",
  "databaseName": "Tasks",
  "collectionName": "Items",
  "createLeaseCollectionIfNotExists": true
}
```

Tady je kód jazyka JavaScript:

```javascript
    module.exports = function (context, documents) {
        context.log('First document Id modified : ', documents[0].id);

        context.done();
    }
```

## <a name="trigger---attributes"></a>Aktivační událost – atributy

Pro [předkompilovaných C#](functions-dotnet-class-library.md) používat funkce, [CosmosDBTrigger](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.DocumentDB/Trigger/CosmosDBTriggerAttribute.cs) atribut, který je definován v balíčku NuGet [Microsoft.Azure.WebJobs.Extensions.DocumentDB](http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.DocumentDB).

Konstruktoru atributu přebírá název databáze a název kolekce. Informace o těchto nastaveních a další vlastnosti, které můžete konfigurovat, najdete v článku [aktivační událost - konfigurace](#trigger---configuration). Tady je `CosmosDBTrigger` atribut příkladu podpis metody:

```csharp
[FunctionName("DocumentUpdates")]
public static void Run(
    [CosmosDBTrigger("database", "collection", ConnectionStringSetting = "myCosmosDB")]
IReadOnlyList<Document> documents,
    TraceWriter log)
{
    ...
}
```

Úplný příklad najdete v tématu [aktivační událost - předkompilovaných C# příklad](#trigger---c-example).

## <a name="trigger---configuration"></a>Aktivační událost - konfigurace

Následující tabulka popisuje vlastnosti konfigurace vazby, které jste nastavili v *function.json* souboru a `CosmosDBTrigger` atribut.

|Vlastnost Function.JSON | Vlastnost atributu |Popis|
|---------|---------|----------------------|
|**Typ** || musí být nastavena na `cosmosDBTrigger`. |
|**směr** || musí být nastavena na `in`. Tento parametr je nastaven automaticky při vytváření aktivační události na portálu Azure. |
|**Jméno** || Název proměnné používá v kódu funkce, která představuje seznam dokumentů se změnami. | 
|**connectionStringSetting**|**ConnectionStringSetting** | Název nastavení aplikace, který obsahuje připojovací řetězec použitý pro připojení k účtu Azure Cosmos DB monitorovány. |
|**Název databáze**|**Název databáze**  | Název databáze Azure Cosmos DB s kolekcí monitorovány. |
|**Název_kolekce** |**Název_kolekce** | Název kolekce, který je monitorován. |
| **leaseConnectionStringSetting** | **LeaseConnectionStringSetting** | (Volitelné) Název nastavení aplikace, který obsahuje připojovací řetězec k službě, která kolekci zapůjčení. Pokud není nastavena, `connectionStringSetting` hodnota se používá. Tento parametr je automaticky nastaven při vytvoření vazby na portálu. |
| **leaseDatabaseName** |**LeaseDatabaseName** | (Volitelné) Název databáze, který obsahuje kolekci používá k ukládání zapůjčení. Pokud není nastavena hodnota `databaseName` nastavení se používá. Tento parametr je automaticky nastaven při vytvoření vazby na portálu. |
| **leaseCollectionName** | **LeaseCollectionName** | (Volitelné) Název kolekce používá k ukládání zapůjčení. Pokud není nastavena hodnota `leases` se používá. |
| **createLeaseCollectionIfNotExists** | **CreateLeaseCollectionIfNotExists** | (Volitelné) Pokud nastavíte hodnotu `true`, kolekci zapůjčení se automaticky vytvoří, pokud ještě neexistuje. Výchozí hodnota je `false`. |
| **leaseCollectionThroughput**| | (Volitelné) Definuje množství jednotky žádosti přiřadit při vytvoření kolekce zapůjčení. Toto nastavení je pouze použité při `createLeaseCollectionIfNotExists` je nastaven na `true`. Tento parametr je automaticky nastaven při vytvoření vazby na portálu.
| |**LeaseOptions** | Konfigurace možností zapůjčení nastavením vlastnosti v instanci systému [ChangeFeedHostOptions](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.changefeedprocessor.changefeedhostoptions) třídy.

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

>[!NOTE] 
>Připojovací řetězec pro kolekci zapůjčení musí mít oprávnění k zápisu.

## <a name="input"></a>Vstup

Vstupní vazba rozhraní API DocumentDB načte jeden nebo více dokumentů Azure Cosmos DB a předává je do vstupní parametr funkce. Parametry dotazu nebo ID dokumentu lze určit podle aktivační událost, která volá funkci. 

## <a name="input---example-1"></a>(Vstup) – Příklad 1

Podívejte se na příklad pro specifický jazyk, který čte jednoho dokumentu:

* [Skript jazyka C#](#input---c-script-example)
* [F#](#input---f-example)
* [JavaScript](#input---javascript-example)

### <a name="input---c-script-example"></a>(Vstup) – příklad skriptu jazyka C#

Následující příklad ukazuje vstupní vazbu Cosmos DB v *function.json* souboru a [funkce skriptu jazyka C#](functions-reference-csharp.md) používající vazby. Funkce přečte jednotlivý dokument a aktualizuje dokumentu textovou hodnotu.

Zde je vazba dat v *function.json* souboru:

```json
{
  "name": "inputDocument",
  "type": "documentDB",
  "databaseName": "MyDatabase",
  "collectionName": "MyCollection",
  "id" : "{queueTrigger}",
  "connection": "MyAccount_COSMOSDB",     
  "direction": "in"
}
```
[Konfigurace](#input---configuration) část vysvětluje tyto vlastnosti.

Tady je kód skriptu jazyka C#:

```cs
// Change input document contents using DocumentDB API input binding 
public static void Run(string myQueueItem, dynamic inputDocument)
{   
  inputDocument.text = "This has changed.";
}
```

<a name="infsharp"></a>

### <a name="input---f-example"></a>(Vstup) – příklad F #

Následující příklad ukazuje vstupní vazbu Cosmos DB v *function.json* souboru a [F # funkce](functions-reference-fsharp.md) používající vazby. Funkce přečte jednotlivý dokument a aktualizuje dokumentu textovou hodnotu.

Zde je vazba dat v *function.json* souboru:

```json
{
  "name": "inputDocument",
  "type": "documentDB",
  "databaseName": "MyDatabase",
  "collectionName": "MyCollection",
  "id" : "{queueTrigger}",
  "connection": "MyAccount_COSMOSDB",     
  "direction": "in"
}
```

[Konfigurace](#input---configuration) část vysvětluje tyto vlastnosti.

Tady je kód F #:

```fsharp
(* Change input document contents using DocumentDB API input binding *)
open FSharp.Interop.Dynamic
let Run(myQueueItem: string, inputDocument: obj) =
  inputDocument?text <- "This has changed."
```

Tento příklad vyžaduje, `project.json` soubor, který určuje `FSharp.Interop.Dynamic` a `Dynamitey` NuGet závislosti:

```json
{
  "frameworks": {
    "net46": {
      "dependencies": {
        "Dynamitey": "1.0.2",
        "FSharp.Interop.Dynamic": "3.0.0"
      }
    }
  }
}
```

Chcete-li přidat `project.json` souborů najdete v tématu [správy balíčků F #](functions-reference-fsharp.md#package).

### <a name="input---javascript-example"></a>(Vstup) – příklad v jazyce JavaScript

Následující příklad ukazuje vstupní vazbu Cosmos DB v *function.json* souboru a [funkce JavaScript, která](functions-reference-node.md) používající vazby. Funkce přečte jednotlivý dokument a aktualizuje dokumentu textovou hodnotu.

Zde je vazba dat v *function.json* souboru:

```json
{
  "name": "inputDocument",
  "type": "documentDB",
  "databaseName": "MyDatabase",
  "collectionName": "MyCollection",
  "id" : "{queueTrigger}",
  "connection": "MyAccount_COSMOSDB",     
  "direction": "in"
}
```
[Konfigurace](#input---configuration) část vysvětluje tyto vlastnosti.

Tady je kód jazyka JavaScript:

```javascript
// Change input document contents using DocumentDB API input binding, using context.bindings.inputDocumentOut
module.exports = function (context) {   
  context.bindings.inputDocumentOut = context.bindings.inputDocumentIn;
  context.bindings.inputDocumentOut.text = "This was updated!";
  context.done();
};
```

## <a name="input---example-2"></a>(Vstup) – příklad 2

Podívejte se na příklad pro specifický jazyk, který čte více dokumentů:

* [Předkompilované C#](#input---c-example-2)
* [Skript jazyka C#](#input---c-script-example-2)
* [JavaScript](#input---javascript-example-2)

### <a name="input---c-example-2"></a>Vstup - C# příklad 2

Následující příklad ukazuje [předkompilovaných C# funkce](functions-dotnet-class-library.md) , provede dotaz SQL.

```csharp
[FunctionName("CosmosDBSample")]
public static HttpResponseMessage Run(
    [HttpTrigger(AuthorizationLevel.Anonymous, "get")] HttpRequestMessage req,
    [DocumentDB("test", "test", ConnectionStringSetting = "CosmosDB", sqlQuery = "SELECT top 2 * FROM c order by c._ts desc")] IEnumerable<object> documents)
{
    return req.CreateResponse(HttpStatusCode.OK, documents);
}
```

### <a name="input---c-script-example-2"></a>(Vstup) – příklad skriptu jazyka C# 2

Následující příklad ukazuje vazbu vstupní v DocumentDB *function.json* souboru a [funkce skriptu jazyka C#](functions-reference-csharp.md) vazby, který používá. Funkce načte více dokumentů určeného dotaz SQL, chcete-li přizpůsobit parametry dotazu pomocí aktivační procedury fronty.

Aktivační událost fronty poskytuje parametr `departmentId`. Fronty zpráv z `{ "departmentId" : "Finance" }` by vrátit všechny záznamy pro finančního oddělení. 

Zde je vazba dat v *function.json* souboru:

```
{
    "name": "documents",
    "type": "documentdb",
    "direction": "in",
    "databaseName": "MyDb",
    "collectionName": "MyCollection",
    "sqlQuery": "SELECT * from c where c.departmentId = {departmentId}"
    "connection": "CosmosDBConnection"
}
```

[Konfigurace](#input---configuration) část vysvětluje tyto vlastnosti.

Tady je kód skriptu jazyka C#:

```csharp
public static void Run(QueuePayload myQueueItem, IEnumerable<dynamic> documents)
{   
    foreach (var doc in documents)
    {
        // operate on each document
    }    
}

public class QueuePayload
{
    public string departmentId { get; set; }
}
```

### <a name="input---javascript-example-2"></a>(Vstup) – příklad v jazyce JavaScript 2

Následující příklad ukazuje vazbu vstupní v DocumentDB *function.json* souboru a [funkce JavaScript, která](functions-reference-node.md) používající vazby. Funkce načte více dokumentů určeného dotaz SQL, chcete-li přizpůsobit parametry dotazu pomocí aktivační procedury fronty.

Aktivační událost fronty poskytuje parametr `departmentId`. Fronty zpráv z `{ "departmentId" : "Finance" }` by vrátit všechny záznamy pro finančního oddělení. 

Zde je vazba dat v *function.json* souboru:

```
{
    "name": "documents",
    "type": "documentdb",
    "direction": "in",
    "databaseName": "MyDb",
    "collectionName": "MyCollection",
    "sqlQuery": "SELECT * from c where c.departmentId = {departmentId}"
    "connection": "CosmosDBConnection"
}
```

[Konfigurace](#input---configuration) část vysvětluje tyto vlastnosti.

Tady je kód jazyka JavaScript:

```javascript
module.exports = function (context, input) {    
    var documents = context.bindings.documents;
    for (var i = 0; i < documents.length; i++) {
        var document = documents[i];
        // operate on each document
    }       
    context.done();
};
```

## <a name="input---attributes"></a>(Vstup) – atributy

Pro [předkompilovaných C#](functions-dotnet-class-library.md) používat funkce, [DocumentDB](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.DocumentDB/DocumentDBAttribute.cs) atribut, který je definován v balíčku NuGet [Microsoft.Azure.WebJobs.Extensions.DocumentDB](http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.DocumentDB).

Konstruktoru atributu přebírá název databáze a název kolekce. Informace o těchto nastaveních a další vlastnosti, které můžete konfigurovat, najdete v článku [následující konfigurační oddíl](#input---configuration). 

## <a name="input---configuration"></a>Vstup - konfigurace

Následující tabulka popisuje vlastnosti konfigurace vazby, které jste nastavili v *function.json* souboru a `DocumentDB` atribut.

|Vlastnost Function.JSON | Vlastnost atributu |Popis|
|---------|---------|----------------------|
|**Typ**     || musí být nastavena na `documentdb`.        |
|**směr**     || musí být nastavena na `in`.         |
|**Jméno**     || Název parametru vazby, který představuje dokumentu ve funkci.  |
|**Název databáze** |**Název databáze** |Databáze obsahující dokumentu.        |
|**Název_kolekce** |**Název_kolekce** | Název kolekce, která obsahuje dokument. |
|**ID**    | **ID** | ID dokumentu pro načtení. Tato vlastnost podporuje vazby parametrů. Další informace najdete v tématu [vazby na vlastní vstupní vlastnosti ve výrazu vazby](functions-triggers-bindings.md#bind-to-custom-input-properties-in-a-binding-expression). Obě není nastavený **id** a **sqlQuery** vlastnosti. Pokud není nastavený buď jednu, je načíst celou kolekci. |
|**sqlQuery**  |**SqlQuery**  | Dotaz služby Azure Cosmos DB SQL použitý k načtení více dokumentů. Vlastnost podporuje runtime vazby, jako v následujícím příkladě: `SELECT * FROM c where c.departmentId = {departmentId}`. Obě není nastavený **id** a **sqlQuery** vlastnosti. Pokud není nastavený buď jednu, je načíst celou kolekci.|
|**připojení**     |**ConnectionStringSetting**|Název nastavení aplikace obsahující připojovacího řetězce Azure Cosmos DB.        |
||**Klíč oddílu**|Určuje hodnotu klíče oddílu pro vyhledávání. Může zahrnovat vázané parametry.|

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="input---usage"></a>(Vstup) – použití

V jazyce C# a F # funkce při ukončení funkce úspěšně, některé změny provedené vstupní dokument prostřednictvím pojmenované vstupní parametry jsou automaticky nastavené jako trvalé. 

V funkce jazyka JavaScript nejsou automaticky provedeny aktualizace po ukončení funkce. Místo toho použijte `context.bindings.<documentName>In` a `context.bindings.<documentName>Out` pro nastavení aktualizací. Najdete v článku [příklad v jazyce JavaScript](#input---javascript-example).

## <a name="output"></a>Výstup

Rozhraní API DocumentDB výstup vazby umožňuje zapsat nový dokument k databázi Azure Cosmos DB. 

## <a name="output---example"></a>Výstup – příklad

Podívejte se na konkrétní jazyk příklad:

* [Předkompilované C#](#trigger---c-example)
* [Skript jazyka C#](#trigger---c-script-example)
* [F#](#trigger---f-example)
* [JavaScript](#trigger---javascript-example)

### <a name="output---c-example"></a>Výstup – příklad jazyka C#

Následující příklad ukazuje [předkompilovaných C# funkce](functions-dotnet-class-library.md) doplňuje dokument k databázi, pomocí data zadaná v zpráv z fronty úložiště.

```cs
[FunctionName("QueueToDocDB")]        
public static void Run(
    [QueueTrigger("myqueue-items", Connection = "AzureWebJobsStorage")] string myQueueItem,
    [DocumentDB("ToDoList", "Items", Id = "id", ConnectionStringSetting = "myCosmosDB")] out dynamic document)
{
    document = new { Text = myQueueItem, id = Guid.NewGuid() };
}
```

### <a name="output---c-script-example"></a>Výstup – příklad skriptu jazyka C#

Následující příklad ukazuje výstup DocumentDB vazby ve *function.json* souboru a [funkce skriptu jazyka C#](functions-reference-csharp.md) používající vazby. Funkce, která používá vazbu vstupní fronty pro frontu, která přijímá JSON v následujícím formátu:

```json
{
  "name": "John Henry",
  "employeeId": "123456",
  "address": "A town nearby"
}
```

Funkce vytvoří Azure Cosmos DB dokumenty ve formátu pro každý záznam:

```json
{
  "id": "John Henry-123456",
  "name": "John Henry",
  "employeeId": "123456",
  "address": "A town nearby"
}
```

Zde je vazba dat v *function.json* souboru:

```json
{
  "name": "employeeDocument",
  "type": "documentDB",
  "databaseName": "MyDatabase",
  "collectionName": "MyCollection",
  "createIfNotExists": true,
  "connection": "MyAccount_COSMOSDB",     
  "direction": "out"
}
```

[Konfigurace](#output---configuration) část vysvětluje tyto vlastnosti.

Tady je kód skriptu jazyka C#:

```cs
#r "Newtonsoft.Json"

using System;
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;

public static void Run(string myQueueItem, out object employeeDocument, TraceWriter log)
{
  log.Info($"C# Queue trigger function processed: {myQueueItem}");

  dynamic employee = JObject.Parse(myQueueItem);

  employeeDocument = new {
    id = employee.name + "-" + employee.employeeId,
    name = employee.name,
    employeeId = employee.employeeId,
    address = employee.address
  };
}
```

Pokud chcete vytvořit více dokumentů, můžete vázat na `ICollector<T>` nebo `IAsyncCollector<T>` kde `T` je jedním z podporovaných typů.

### <a name="output---f-example"></a>Výstup – příklad F #

Následující příklad ukazuje výstup DocumentDB vazby ve *function.json* souboru a [F # funkce](functions-reference-fsharp.md) používající vazby. Funkce, která používá vazbu vstupní fronty pro frontu, která přijímá JSON v následujícím formátu:

```json
{
  "name": "John Henry",
  "employeeId": "123456",
  "address": "A town nearby"
}
```

Funkce vytvoří Azure Cosmos DB dokumenty ve formátu pro každý záznam:

```json
{
  "id": "John Henry-123456",
  "name": "John Henry",
  "employeeId": "123456",
  "address": "A town nearby"
}
```

Zde je vazba dat v *function.json* souboru:

```json
{
  "name": "employeeDocument",
  "type": "documentDB",
  "databaseName": "MyDatabase",
  "collectionName": "MyCollection",
  "createIfNotExists": true,
  "connection": "MyAccount_COSMOSDB",     
  "direction": "out"
}
```
[Konfigurace](#output---configuration) část vysvětluje tyto vlastnosti.

Tady je kód F #:

```fsharp
open FSharp.Interop.Dynamic
open Newtonsoft.Json

type Employee = {
  id: string
  name: string
  employeeId: string
  address: string
}

let Run(myQueueItem: string, employeeDocument: byref<obj>, log: TraceWriter) =
  log.Info(sprintf "F# Queue trigger function processed: %s" myQueueItem)
  let employee = JObject.Parse(myQueueItem)
  employeeDocument <-
    { id = sprintf "%s-%s" employee?name employee?employeeId
      name = employee?name
      employeeId = employee?employeeId
      address = employee?address }
```

Tento příklad vyžaduje, `project.json` soubor, který určuje `FSharp.Interop.Dynamic` a `Dynamitey` NuGet závislosti:

```json
{
  "frameworks": {
    "net46": {
      "dependencies": {
        "Dynamitey": "1.0.2",
        "FSharp.Interop.Dynamic": "3.0.0"
      }
    }
  }
}
```

Chcete-li přidat `project.json` souborů najdete v tématu [správy balíčků F #](functions-reference-fsharp.md#package).

### <a name="output---javascript-example"></a>Výstup – příklad v jazyce JavaScript

Následující příklad ukazuje výstup DocumentDB vazby ve *function.json* souboru a [funkce JavaScript, která](functions-reference-node.md) používající vazby. Funkce, která používá vazbu vstupní fronty pro frontu, která přijímá JSON v následujícím formátu:

```json
{
  "name": "John Henry",
  "employeeId": "123456",
  "address": "A town nearby"
}
```

Funkce vytvoří Azure Cosmos DB dokumenty ve formátu pro každý záznam:

```json
{
  "id": "John Henry-123456",
  "name": "John Henry",
  "employeeId": "123456",
  "address": "A town nearby"
}
```

Zde je vazba dat v *function.json* souboru:

```json
{
  "name": "employeeDocument",
  "type": "documentDB",
  "databaseName": "MyDatabase",
  "collectionName": "MyCollection",
  "createIfNotExists": true,
  "connection": "MyAccount_COSMOSDB",     
  "direction": "out"
}
```

[Konfigurace](#output---configuration) část vysvětluje tyto vlastnosti.

Tady je kód jazyka JavaScript:

```javascript
module.exports = function (context) {

  context.bindings.employeeDocument = JSON.stringify({ 
    id: context.bindings.myQueueItem.name + "-" + context.bindings.myQueueItem.employeeId,
    name: context.bindings.myQueueItem.name,
    employeeId: context.bindings.myQueueItem.employeeId,
    address: context.bindings.myQueueItem.address
  });

  context.done();
};
```

## <a name="output---attributes"></a>Výstup – atributy

Pro [předkompilovaných C#](functions-dotnet-class-library.md) používat funkce, [DocumentDB](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.DocumentDB/DocumentDBAttribute.cs) atribut, který je definován v balíčku NuGet [Microsoft.Azure.WebJobs.Extensions.DocumentDB](http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.DocumentDB).

Konstruktoru atributu přebírá název databáze a název kolekce. Informace o těchto nastaveních a další vlastnosti, které můžete konfigurovat, najdete v článku [výstup - konfigurace](#output---configuration). Tady je `DocumentDB` atribut příkladu podpis metody:

```csharp
[FunctionName("QueueToDocDB")]        
public static void Run(
    [QueueTrigger("myqueue-items", Connection = "AzureWebJobsStorage")] string myQueueItem,
    [DocumentDB("ToDoList", "Items", Id = "id", ConnectionStringSetting = "myCosmosDB")] out dynamic document)
{
    ...
}
```

Úplný příklad najdete v tématu [výstup - předkompilovaných C# příklad](#output---c-example).

## <a name="output---configuration"></a>Výstup – konfigurace

Následující tabulka popisuje vlastnosti konfigurace vazby, které jste nastavili v *function.json* souboru a `DocumentDB` atribut.

|Vlastnost Function.JSON | Vlastnost atributu |Popis|
|---------|---------|----------------------|
|**Typ**     || musí být nastavena na `documentdb`.        |
|**směr**     || musí být nastavena na `out`.         |
|**Jméno**     || Název parametru vazby, který představuje dokumentu ve funkci.  |
|**Název databáze** | **Název databáze**|Databáze obsahující kolekci, kde se má vytvořit dokumentu.     |
|**Název_kolekce** |**Název_kolekce**  | Název kolekce, kde se má vytvořit dokumentu. |
|**createIfNotExists**  |**CreateIfNotExists**    | Logická hodnota označující, zda kolekce se vytvoří při neexistuje. Výchozí hodnota je *false* s vyhrazenou propustností, který obsahuje náklady důsledky se vytváří nové kolekce. Další informace najdete na [stránce s cenami](https://azure.microsoft.com/pricing/details/documentdb/).  |
||**Klíč oddílu** |Když `CreateIfNotExists` hodnotu true, definuje cestu ke klíči oddílu pro vytvořenou kolekci.|
||**CollectionThroughput**| Když `CreateIfNotExists` hodnotu true, definuje [propustnost](../cosmos-db/set-throughput.md) vytvořené kolekce.|
|**připojení**    |**ConnectionStringSetting** |Název nastavení aplikace obsahující připojovacího řetězce Azure Cosmos DB.        |

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="output---usage"></a>Výstup – použití

Ve výchozím nastavení když zapíšete do výstupního parametru ve funkci, se dokumentu vytvoří v databázi. Tento dokument má automaticky vytvářenému identifikátoru GUID jako ID dokumentu. Můžete zadat ID dokumentu výstupního dokumentu zadáním `id` vlastnost v objektu JSON předaný výstupní parametr. 

> [!Note]  
> Pokud zadáte ID stávající dokument, získá přepsány nový dokument výstup. 

## <a name="next-steps"></a>Další kroky

> [!div class="nextstepaction"]
> [Přejděte na rychlé spuštění, používající aktivační událost Cosmos DB](functions-create-cosmos-db-triggered-function.md)

> [!div class="nextstepaction"]
> [Další informace o výpočetní s Cosmos DB bez serveru databáze](..\cosmos-db\serverless-computing-database.md)

> [!div class="nextstepaction"]
> [Další informace o Azure functions triggerů a vazeb](functions-triggers-bindings.md)
