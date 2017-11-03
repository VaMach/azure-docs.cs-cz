---
title: Azure Cosmos DB vazby pro funkce | Microsoft Docs
description: "Pochopit, jak používat Azure Cosmos DB triggerů a vazeb v Azure Functions."
services: functions
documentationcenter: na
author: christopheranderson
manager: cfowler
editor: 
tags: 
keywords: "Funkce Azure, funkce zpracování událostí, dynamické výpočetní architektura bez serveru"
ms.assetid: 3d8497f0-21f3-437d-ba24-5ece8c90ac85
ms.service: functions; cosmos-db
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 09/19/2017
ms.author: glenga
ms.openlocfilehash: d05c0342e771e229a7175570ad227c4359980990
ms.sourcegitcommit: 6acb46cfc07f8fade42aff1e3f1c578aa9150c73
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/18/2017
---
# <a name="azure-cosmos-db-bindings-for-functions"></a>Azure Cosmos DB vazby pro funkce
[!INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

Tento článek vysvětluje, jak nakonfigurovat a vazeb Azure Cosmos DB kódu v Azure Functions. Podporuje funkce aktivovat, vstup a výstup vazby pro Azure Cosmos DB.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

Další informace o výpočetní bez serveru s Azure Cosmos DB najdete v tématu [Cosmos databázi Azure: bez serveru databáze computing pomocí Azure Functions](..\cosmos-db\serverless-computing-database.md).

<a id="trigger"></a>
<a id="cosmosdbtrigger"></a>

## <a name="azure-cosmos-db-trigger"></a>Aktivační událost Azure Cosmos DB

Aktivační událost DB Cosmos Azure používá [Azure Cosmos DB změnu kanálu](../cosmos-db/change-feed.md) tak, aby naslouchala na změny napříč oddíly. Aktivační událost vyžaduje druhé kolekci, která se používá k ukládání _zapůjčení_ přes oddíly.

Kolekce, který je monitorován a kolekce, která obsahuje zapůjčení musí být k dispozici pro aktivační událost pro práci.

Aktivační událost Azure Cosmos DB podporuje následující vlastnosti:

|Vlastnost  |Popis  |
|---------|---------|
|**Typ** | musí být nastavena na `cosmosDBTrigger`. |
|**Jméno** | Název proměnné používá v kódu funkce, která představuje seznam dokumentů se změnami. | 
|**směr** | musí být nastavena na `in`. Tento parametr je nastaven automaticky při vytváření aktivační události na portálu Azure. |
|**connectionStringSetting** | Název nastavení aplikace, který obsahuje připojovací řetězec použitý pro připojení k účtu Azure Cosmos DB monitorovány. |
|**Název databáze** | Název databáze Azure Cosmos DB s kolekcí monitorovány. |
|**Název_kolekce** | Název kolekce, který je monitorován. |
| **leaseConnectionStringSetting** | (Volitelné) Název nastavení aplikace, který obsahuje připojovací řetězec k službě, která kolekci zapůjčení. Pokud není nastavena, `connectionStringSetting` hodnota se používá. Tento parametr je automaticky nastaven při vytvoření vazby na portálu. |
| **leaseDatabaseName** | (Volitelné) Název databáze, který obsahuje kolekci používá k ukládání zapůjčení. Pokud není nastavena hodnota `databaseName` nastavení se používá. Tento parametr je automaticky nastaven při vytvoření vazby na portálu. |
| **leaseCollectionName** | (Volitelné) Název kolekce používá k ukládání zapůjčení. Pokud není nastavena hodnota `leases` se používá. |
| **createLeaseCollectionIfNotExists** | (Volitelné) Pokud nastavíte hodnotu `true`, kolekci zapůjčení se automaticky vytvoří, pokud ještě neexistuje. Výchozí hodnota je `false`. |
| **leaseCollectionThroughput** | (Volitelné) Definuje množství jednotky žádosti přiřadit při vytvoření kolekce zapůjčení. Toto nastavení je pouze použité při `createLeaseCollectionIfNotExists` je nastaven na `true`. Tento parametr je automaticky nastaven při vytvoření vazby na portálu.

>[!NOTE] 
>Připojovací řetězec použitý pro připojení ke kolekci zapůjčení musí mít oprávnění k zápisu.

Tyto vlastnosti lze nastavit v kartě integrací pro funkce na portálu Azure nebo úpravou `function.json` souboru projektu.

## <a name="using-an-azure-cosmos-db-trigger"></a>Pomocí aktivační procedury Azure Cosmos DB

Tato část obsahuje příklady použití Azure Cosmos DB aktivační událost. Příklady předpokládají metadata aktivační událost, která vypadá takto:

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
 
Příklad toho, jak vytvořit aktivační událost Azure Cosmos DB z funkce aplikace na portálu, naleznete v části [vytvořit funkci aktivovány Azure Cosmos DB](functions-create-cosmos-db-triggered-function.md). 

### <a name="trigger-sample-in-c"></a>Ukázka aktivační události v jazyce C# #
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


### <a name="trigger-sample-in-javascript"></a>Ukázka aktivační události v jazyce JavaScript
```javascript
    module.exports = function (context, documents) {
        context.log('First document Id modified : ', documents[0].id);

        context.done();
    }
```

<a id="docdbinput"></a>

## <a name="documentdb-api-input-binding"></a>Rozhraní API DocumentDB vstupní vazby
Vstupní vazba rozhraní API DocumentDB načte dokument Azure Cosmos DB a předá do pojmenované vstupní parametr funkce. Dokument, který se dá určit ID založené na aktivační událost, která volá funkci. 

Vstupní vazba rozhraní API DocumentDB má následující vlastnosti *function.json*:

|Vlastnost  |Popis  |
|---------|---------|
|**Jméno**     | Název parametru vazby, který představuje dokumentu ve funkci.  |
|**Typ**     | musí být nastavena na `documentdb`.        |
|**Název databáze** | Databáze obsahující dokumentu.        |
|**Název_kolekce**  | Název kolekce, která obsahuje dokument. |
|**ID**     | ID dokumentu pro načtení. Tato vlastnost podporuje vazby parametrů. Další informace najdete v tématu [vazby na vlastní vstupní vlastnosti ve výrazu vazby](functions-triggers-bindings.md#bind-to-custom-input-properties-in-a-binding-expression). |
|**sqlQuery**     | Dotaz služby Azure Cosmos DB SQL použitý k načtení více dokumentů. Dotaz podporuje runtime vazby, například v příkladu: `SELECT * FROM c where c.departmentId = {departmentId}`.        |
|**připojení**     |Název nastavení aplikace obsahující připojovacího řetězce Azure Cosmos DB.        |
|**směr**     | musí být nastavena na `in`.         |

Nelze nastavit i **id** a **sqlQuery** vlastnosti. Pokud jsou nastavená ani jeden z nich, se načte celou kolekci.

## <a name="using-a-documentdb-api-input-binding"></a>Použití rozhraní API DocumentDB vstupní vazby

* V jazyce C# a F # funkce při ukončení funkce úspěšně, některé změny provedené vstupní dokument prostřednictvím pojmenované vstupní parametry jsou automaticky nastavené jako trvalé. 
* V funkce jazyka JavaScript nejsou automaticky provedeny aktualizace po ukončení funkce. Místo toho použijte `context.bindings.<documentName>In` a `context.bindings.<documentName>Out` pro nastavení aktualizací. Najdete v článku [JavaScript ukázka](#injavascript).

<a name="inputsample"></a>

## <a name="input-sample-for-single-document"></a>Vstupní vzorek pro jednotlivý dokument
Předpokládejme, že máte následující rozhraní API DocumentDB vstupní vazby v `bindings` pole function.json:

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

V tématu vzorku pro specifický jazyk, který používá tuto vstupní vazbu k aktualizaci dokumentu textovou hodnotu.

* [C#](#incsharp)
* [F#](#infsharp)
* [JavaScript](#injavascript)

<a name="incsharp"></a>
### <a name="input-sample-in-c"></a>Vstupní ukázka v jazyce C# #

```cs
// Change input document contents using DocumentDB API input binding 
public static void Run(string myQueueItem, dynamic inputDocument)
{   
  inputDocument.text = "This has changed.";
}
```
<a name="infsharp"></a>

### <a name="input-sample-in-f"></a>Vstupní ukázka v jazyce F # #

```fsharp
(* Change input document contents using DocumentDB API input binding *)
open FSharp.Interop.Dynamic
let Run(myQueueItem: string, inputDocument: obj) =
  inputDocument?text <- "This has changed."
```

Tato ukázka vyžaduje `project.json` soubor, který určuje `FSharp.Interop.Dynamic` a `Dynamitey` NuGet závislosti:

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

<a name="injavascript"></a>

### <a name="input-sample-in-javascript"></a>Vstupní ukázka v jazyce JavaScript

```javascript
// Change input document contents using DocumentDB API input binding, using context.bindings.inputDocumentOut
module.exports = function (context) {   
  context.bindings.inputDocumentOut = context.bindings.inputDocumentIn;
  context.bindings.inputDocumentOut.text = "This was updated!";
  context.done();
};
```

## <a name="input-sample-with-multiple-documents"></a>Vstupní ukázkové s více dokumenty

Předpokládejme, které chcete načíst více dokumentů určeného dotaz SQL, chcete-li přizpůsobit parametry dotazu pomocí aktivační procedury fronty. 

V tomto příkladu obsahuje aktivační událost fronty parametr `departmentId`. Fronty zpráv z `{ "departmentId" : "Finance" }` by vrátit všechny záznamy pro finančního oddělení. Použijte následující *function.json*:

```
{
    "name": "documents",
    "type": "documentdb",
    "direction": "in",
    "databaseName": "MyDb",
    "collectionName": "MyCollection",
    "sqlQuery": "SELECT * from c where c.departmentId = {departmentId}",
    "connection": "CosmosDBConnection"
}
```

### <a name="input-sample-with-multiple-documents-in-c"></a>Vstupní ukázkové s více dokumenty v jazyce C#

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

### <a name="input-sample-with-multiple-documents-in-javascript"></a>Vstupní ukázkové s více dokumenty v jazyce JavaScript

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

## <a id="docdboutput"></a>Rozhraní API DocumentDB výstup vazby
Rozhraní API DocumentDB výstup vazby umožňuje zapsat nový dokument k databázi Azure Cosmos DB. Má následující vlastnosti v *function.json*:

|Vlastnost  |Popis  |
|---------|---------|
|**Jméno**     | Název parametru vazby, který představuje dokumentu ve funkci.  |
|**Typ**     | musí být nastavena na `documentdb`.        |
|**Název databáze** | Databáze obsahující kolekci, kde se má vytvořit dokumentu.     |
|**Název_kolekce**  | Název kolekce, kde se má vytvořit dokumentu. |
|**createIfNotExists**     | Logická hodnota označující, zda kolekce se vytvoří při neexistuje. Výchozí hodnota je *false*. Je to proto, že jsou vytvořeny nové kolekce s vyhrazenou propustností, který obsahuje náklady dopad. Další podrobnosti naleznete [stránce s cenami](https://azure.microsoft.com/pricing/details/documentdb/).  |
|**připojení**     |Název nastavení aplikace obsahující připojovacího řetězce Azure Cosmos DB.        |
|**směr**     | musí být nastavena na `out`.         |

## <a name="using-a-documentdb-api-output-binding"></a>Pomocí rozhraní API DocumentDB výstup vazby
V této části se dozvíte, jak používat rozhraní API DocumentDB výstupu vazby v kódu funkce.

Ve výchozím nastavení když zapíšete do výstupního parametru ve funkci, se dokumentu vytvoří v databázi. Tento dokument má automaticky vytvářenému identifikátoru GUID jako ID dokumentu. Můžete zadat ID dokumentu výstup dokumentu zadáním `id` vlastnost v objektu JSON předaný výstupní parametr. 

>[!Note]  
>Pokud zadáte ID stávající dokument, získá přepsány nový dokument výstup. 

Výstup více dokumentů, můžete také vázat na `ICollector<T>` nebo `IAsyncCollector<T>` kde `T` je jedním z podporovaných typů.

<a name="outputsample"></a>

## <a name="documentdb-api-output-binding-sample"></a>Ukázka výstupu vazby DocumentDB rozhraní API
Předpokládejme, že máte následující rozhraní API DocumentDB výstup vazby v `bindings` pole function.json:

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

A máte vazbu vstupní fronty pro frontu, která přijímá JSON v následujícím formátu:

```json
{
  "name": "John Henry",
  "employeeId": "123456",
  "address": "A town nearby"
}
```

A chcete vytvořit Azure Cosmos DB dokumenty ve formátu pro každý záznam:

```json
{
  "id": "John Henry-123456",
  "name": "John Henry",
  "employeeId": "123456",
  "address": "A town nearby"
}
```

V tématu vzorku pro specifický jazyk, který používá tuto vazbu výstup k přidání dokumenty k vaší databázi.

* [C#](#outcsharp)
* [F#](#outfsharp)
* [JavaScript](#outjavascript)

<a name="outcsharp"></a>

### <a name="output-sample-in-c"></a>Ukázka výstupu v jazyce C# #

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

<a name="outfsharp"></a>

### <a name="output-sample-in-f"></a>Ukázka výstupu v jazyce F # #

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

Tato ukázka vyžaduje `project.json` soubor, který určuje `FSharp.Interop.Dynamic` a `Dynamitey` NuGet závislosti:

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

<a name="outjavascript"></a>

### <a name="output-sample-in-javascript"></a>Ukázka výstupu v jazyce JavaScript

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
