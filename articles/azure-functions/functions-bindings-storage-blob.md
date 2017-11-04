---
title: Azure Blob Storage funkce vazby | Microsoft Docs
description: "Pochopit, jak používat Azure Storage triggerů a vazeb v Azure Functions."
services: functions
documentationcenter: na
author: ggailey777
manager: cfowler
editor: 
tags: 
keywords: "Funkce Azure, funkce zpracování událostí, dynamické výpočetní architektura bez serveru"
ms.assetid: aba8976c-6568-4ec7-86f5-410efd6b0fb9
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 10/27/2017
ms.author: glenga
ms.openlocfilehash: 79b9dbee89a4e33a1768343b9242d6b2b1118355
ms.sourcegitcommit: 804db51744e24dca10f06a89fe950ddad8b6a22d
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/30/2017
---
# <a name="azure-functions-blob-storage-bindings"></a>Vazby úložiště Azure Blob funkce
[!INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

Tento článek vysvětluje postup konfigurace a práce s Azure Blob storage vazeb v Azure Functions. Azure podporuje aktivaci funkce vstup a výstup vazby pro úložiště objektů Blob v Azure. Funkce, které jsou k dispozici v všechny vazby, najdete v části [Azure Functions triggerů a vazeb koncepty](functions-triggers-bindings.md).

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

> [!NOTE]
> A [pouze účet úložiště blob](../storage/common/storage-create-storage-account.md#blob-storage-accounts) není podporován. Objekt BLOB úložiště triggerů a vazeb se vyžaduje účet úložiště pro obecné účely. 
> 

<a name="trigger"></a>
<a name="storage-blob-trigger"></a>
## <a name="blob-storage-triggers-and-bindings"></a>Objekt BLOB úložiště triggerů a vazeb

Pomocí aktivační událost úložiště objektů Blob v Azure, kódu funkce je volána, když je zjištěna nová nebo aktualizovaná objektu blob. Obsahu objektu blob jsou uvedeny jako vstup do funkce.

Definovat pomocí aktivační události objektu blob úložiště **integrací** na portálu funkce. Vytvoří následující definice v portálu **vazby** části *function.json*:

```json
{
    "name": "<The name used to identify the trigger data in your code>",
    "type": "blobTrigger",
    "direction": "in",
    "path": "<container to monitor, and optionally a blob name pattern - see below>",
    "connection": "<Name of app setting - see below>"
}
```

Objekt BLOB vstup a výstup vazby jsou definovány pomocí `blob` jako typ vazby:

```json
{
  "name": "<The name used to identify the blob input in your code>",
  "type": "blob",
  "direction": "in", // other supported directions are "inout" and "out"
  "path": "<Path of input blob - see below>",
  "connection":"<Name of app setting - see below>"
},
```

* `path` Podporuje vlastnost vazby výrazy a parametry filtrování. V tématu [název vzory](#pattern).
* `connection` Vlastnost musí obsahovat název nastavení aplikace, který obsahuje připojovací řetězec úložiště. Na portálu Azure, standardního editoru v **integrací** kartě nakonfiguruje toto nastavení aplikace pro vás, když vyberete účet úložiště.

> [!NOTE]
> Pokud používáte aktivační události objektu blob na plánu spotřeby, může být až 10 minut zpoždění při zpracování nové objekty BLOB po aplikaci funkce přešel nečinnosti. Po aplikaci funkce běží, objekty BLOB jsou zpracovávány okamžitě. Abyste se vyhnuli Tato počáteční prodleva, zvažte jednu z následujících možností:
> - Plán služby App Service použijte s povolenou funkci Always On.
> - Použijte jiný mechanismus pro aktivaci objektu blob zpracování, např. zprávu fronty, který obsahuje název objektu blob. Příklad, naleznete v části [vstupní frontě aktivační události s objektem blob vazby](#input-sample).

<a name="pattern"></a>

### <a name="name-patterns"></a>Vzory názvů
Můžete zadat vzor názvu objektu blob v `path` vlastnosti, která může být výraz filtru nebo vazby. V tématu [vazby výrazy a vzory](functions-triggers-bindings.md#binding-expressions-and-patterns).

Například pokud chcete filtrovat, aby objekty BLOB, které začínají řetězcem "původní", použijte následující definice. Tuto cestu vyhledá objekt blob s názvem *původní Blob1.txt* v *vstupní* kontejneru a hodnota `name` proměnná v kódu funkce `Blob1`.

```json
"path": "input/original-{name}",
```

Pokud chcete vytvořit vazbu samostatně na název souboru objektů blob a příponu, použijte dva vzory. Tato cesta také vyhledá objekt blob s názvem *původní Blob1.txt*a hodnota `blobname` a `blobextension` proměnné v kódu funkce jsou *původní Blob1* a *txt*.

```json
"path": "input/{blobname}.{blobextension}",
```

Typ souboru objektů BLOB můžete omezit pomocí příkazu pevnou hodnotu pro tuto příponu. Například aktivovat pouze u souborů, PNG, používají následující vzorec:

```json
"path": "samples/{name}.png",
```

Složené závorky jsou speciální znaky v vzory názvů. Pokud chcete zadat názvy objektů blob, které mají v názvu složené závorky, můžete vyhnuli složené závorky pomocí dvou složené závorky. Následující příklad vyhledá objekt blob s názvem *{20140101}-soundfile.mp3* v *bitové kopie* kontejner a `name` hodnotu proměnné v kód funkce je *soundfile.mp3*. 

```json
"path": "images/{{20140101}}-{name}",
```

### <a name="trigger-metadata"></a>Aktivační událost metadat

Aktivační události objektu blob nabízí několik vlastností metadat. Tyto vlastnosti lze použít jako součást výrazů vazby v jiných vazby nebo jako parametry v kódu. Tyto hodnoty mají stejnou sémantiku jako [CloudBlob](https://docs.microsoft.com/en-us/dotnet/api/microsoft.windowsazure.storage.blob.cloudblob?view=azure-dotnet).

- **BlobTrigger**. Zadejte `string`. Spouštěcí objekt blob cesta
- **Identifikátor URI**. Zadejte `System.Uri`. Identifikátor URI objektu blob pro primární umístění.
- **Vlastnosti**. Zadejte `Microsoft.WindowsAzure.Storage.Blob.BlobProperties`. Vlastnosti systému objektu blob.
- **Metadata**. Zadejte `IDictionary<string,string>`. Metadata definovaná uživatelem pro tento objekt blob.

<a name="receipts"></a>

### <a name="blob-receipts"></a>Potvrzení objektů BLOB
Modulu runtime Azure Functions zajistí, že žádná funkce aktivační události objektu blob volala více než jednou pro stejný objekt blob nové nebo aktualizované. Chcete-li zjistit, pokud byla zpracována na verzi daného objektu blob, udržuje *blob potvrzení*.

Azure funkce úložiště objektů blob v kontejneru nazvaném potvrzení *azure. webové úlohy hostitelů* v účtu úložiště Azure pro aplikaci funkce (definované nastavení aplikace `AzureWebJobsStorage`). Potvrzení o objektu blob obsahuje následující informace:

* Funkci spouštěná ("*&lt;funkce název aplikace >*. Funkce.  *&lt;název funkce >*", například:"MyFunctionApp.Functions.CopyBlob")
* Název kontejneru
* Typ objektu blob ("BlockBlob" nebo "PageBlob")
* Název objektu blob
* Značky ETag (identifikátor objektu blob verze, například: "0x8D1DC6E70A277EF")

Pokud chcete vynutit opětovné zpracování objektu blob, odstranit objekt blob příjmu pro tento objekt blob z *azure. webové úlohy hostitelů* kontejneru ručně.

<a name="poison"></a>

### <a name="handling-poison-blobs"></a>Zpracování poškozených objektů BLOB
Pokud funkci aktivační události objektu blob se nezdaří pro daný objekt blob Azure Functions opakovaných pokusů, které funkce celkem 5krát ve výchozím nastavení. 

Pokud selžou všechny 5 pokusů, Azure Functions přidá zprávu do fronty úložiště s názvem *webjobs. blobtrigger poison*. Zprávy ve frontě pro poškozených objekty BLOB je objekt JSON, který obsahuje následující vlastnosti:

* FunctionId (ve formátu  *&lt;funkce název aplikace >*. Funkce.  *&lt;název funkce >*)
* BlobType ("BlockBlob" nebo "PageBlob")
* ContainerName
* BlobName
* Značka ETag (identifikátor objektu blob verze, například: "0x8D1DC6E70A277EF")

### <a name="blob-polling-for-large-containers"></a>Dotazování pro velké kontejnery objektů BLOB
Pokud kontejner objektů blob, který je monitorován obsahuje více než 10 000 objektů BLOB, přihlaste se kontroly runtime funkce soubory, které chcete sledovat pro nové nebo změněné objekty BLOB. Tento proces není v reálném čase. Funkce nemusí získat aktivuje až několik minut nebo déle po vytvoření objektu blob. Kromě toho [protokol úložiště jsou vytvořené na "best effort"](/rest/api/storageservices/About-Storage-Analytics-Logging) intervalech. Není zaručeno, že jsou zachyceny všechny události. Za určitých podmínek může být načteni protokoly. Pokud budete potřebovat rychlejší a spolehlivější blob zpracování, zvažte vytvoření [zprávu fronty](../storage/queues/storage-dotnet-how-to-use-queues.md) při vytváření objektu blob. Poté použijte [aktivační událost fronty](functions-bindings-storage-queue.md) místo aktivační událost objektu blob ke zpracování objektu blob.

<a name="triggerusage"></a>

## <a name="using-a-blob-trigger-and-input-binding"></a>Pomocí aktivační události objektu blob a vstupní vazby
V rozhraní .NET funkce, přístup k datům objektu blob pomocí parametru metody, jako třeba `Stream paramName`. Zde `paramName` je hodnota zadaná v [konfigurace aktivační události](#trigger). Ve funkcích Node.js, přístup k datům vstupního objektu blob pomocí `context.bindings.<name>`.

V rozhraní .NET můžete vázat na jakýkoli z typů v níže uvedeném seznamu. Pokud slouží jako vstupní vazby, přičemž některé z těchto typů vyžadují `inout` vazby směr v *function.json*. Tomto směru nepodporuje standardního editoru, je nutné použít rozšířené editoru.

* `TextReader`
* `Stream`
* `ICloudBlob`(vyžaduje "inout" vazba směr)
* `CloudBlockBlob`(vyžaduje "inout" vazba směr)
* `CloudPageBlob`(vyžaduje "inout" vazba směr)
* `CloudAppendBlob`(vyžaduje "inout" vazba směr)

Pokud se očekává text objekty BLOB, můžete také vázat na .NET `string` typu. Toto nastavení se doporučuje jenom Pokud velikost objektu blob je malá, protože obsah celý objekt blob jsou načtena do paměti. Obecně platí, je vhodnější použít `Stream` nebo `CloudBlockBlob` typu.

## <a name="trigger-sample"></a>Ukázka aktivační události
Předpokládejme, že máte následující function.json, která definuje aktivační událost úložiště objektů blob:

```json
{
    "disabled": false,
    "bindings": [
        {
            "name": "myBlob",
            "type": "blobTrigger",
            "direction": "in",
            "path": "samples-workitems",
            "connection":"MyStorageAccount"
        }
    ]
}
```

Naleznete v ukázce pro specifický jazyk, který zaznamenává obsah jednotlivých objektů blob, který je přidán do kontejneru monitorovaných.

* [C#](#triggercsharp)
* [Node.js](#triggernodejs)

<a name="triggercsharp"></a>

### <a name="blob-trigger-examples-in-c"></a>Příklady aktivační události objektu BLOB v jazyce C# #

```cs
// Blob trigger sample using a Stream binding
public static void Run(Stream myBlob, TraceWriter log)
{
   log.Info($"C# Blob trigger function Processed blob\n Name:{name} \n Size: {myBlob.Length} Bytes");
}
```

```cs
// Blob trigger binding to a CloudBlockBlob
#r "Microsoft.WindowsAzure.Storage"

using Microsoft.WindowsAzure.Storage.Blob;

public static void Run(CloudBlockBlob myBlob, string name, TraceWriter log)
{
    log.Info($"C# Blob trigger function Processed blob\n Name:{name}\nURI:{myBlob.StorageUri}");
}
```

<a name="triggernodejs"></a>

### <a name="trigger-example-in-nodejs"></a>Příklad aktivační události v Node.js

```javascript
module.exports = function(context) {
    context.log('Node.js Blob trigger function processed', context.bindings.myBlob);
    context.done();
};
```
<a name="outputusage"></a>
<a name="storage-blob-output-binding"></a>

## <a name="using-a-blob-output-binding"></a>Pomocí objektu blob výstup vazby

V rozhraní .NET funkce, měli byste buď použijte `out string` parametr v podpis funkce nebo použijte jeden z typů v následujícím seznamu. V Node.js funkce, přístup k výstupu blob pomocí `context.bindings.<name>`.

V rozhraní .NET funkce výstup můžete na některý z následujících typů:

* `out string`
* `TextWriter`
* `Stream`
* `CloudBlobStream`
* `ICloudBlob`
* `CloudBlockBlob` 
* `CloudPageBlob` 

<a name="input-sample"></a>

## <a name="queue-trigger-with-blob-input-and-output-sample"></a>Aktivační událost fronty s blob vstupní a výstupní ukázka
Předpokládejme, že máte následující function.json, který definuje [aktivační událost Queue Storage](functions-bindings-storage-queue.md)úložiště objektů blob vstup a výstup úložiště objektů blob. Všimněte si použití `queueTrigger` vlastnost metadat. v objektu blob vstupní a výstupní `path` vlastnosti:

```json
{
  "bindings": [
    {
      "queueName": "myqueue-items",
      "connection": "MyStorageConnection",
      "name": "myQueueItem",
      "type": "queueTrigger",
      "direction": "in"
    },
    {
      "name": "myInputBlob",
      "type": "blob",
      "path": "samples-workitems/{queueTrigger}",
      "connection": "MyStorageConnection",
      "direction": "in"
    },
    {
      "name": "myOutputBlob",
      "type": "blob",
      "path": "samples-workitems/{queueTrigger}-Copy",
      "connection": "MyStorageConnection",
      "direction": "out"
    }
  ],
  "disabled": false
}
``` 

Naleznete v ukázce pro specifický jazyk, který kopíruje vstupního objektu blob do výstupního objektu blob.

* [C#](#incsharp)
* [Node.js](#innodejs)

<a name="incsharp"></a>

### <a name="blob-binding-example-in-c"></a>Příklad vazby objektů BLOB v jazyce C# #

```cs
// Copy blob from input to output, based on a queue trigger
public static void Run(string myQueueItem, Stream myInputBlob, out Stream myOutputBlob, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");
    myOutputBlob = myInputBlob;
}
```

<a name="innodejs"></a>

### <a name="blob-binding-example-in-nodejs"></a>Příklad vazby objektů BLOB v Node.js

```javascript
// Copy blob from input to output, based on a queue trigger
module.exports = function(context) {
    context.log('Node.js Queue trigger function processed', context.bindings.myQueueItem);
    context.bindings.myOutputBlob = context.bindings.myInputBlob;
    context.done();
};
```

## <a name="next-steps"></a>Další kroky
[!INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]

