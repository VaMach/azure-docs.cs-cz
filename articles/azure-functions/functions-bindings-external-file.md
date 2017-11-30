---
title: "Externí soubor funkce vazby Azure (Preview) | Microsoft Docs"
description: "Používání vazeb externí soubor v Azure Functions"
services: functions
documentationcenter: 
author: alexkarcher-msft
manager: cfowler
editor: 
ms.assetid: 
ms.service: functions
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 11/27/2017
ms.author: alkarche
ms.openlocfilehash: c7a1ff4d4488b37b1969edfbb6f935eccd63413c
ms.sourcegitcommit: cfd1ea99922329b3d5fab26b71ca2882df33f6c2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/30/2017
---
# <a name="azure-functions-external-file-bindings-preview"></a>Externí soubor funkce vazby Azure (Preview)
Tento článek ukazuje, jak pracovat se soubory od různých poskytovatelů SaaS (například OneDrive, Dropbox) v rámci funkce využívá integrované vazby. Azure functions podporuje aktivaci, vstup a výstup vazby pro externí soubor.

Tuto vazbu vytvoří připojení rozhraní API pro SaaS poskytovatelů nebo použije existující API připojení ze skupiny prostředků aplikaci funkce.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="supported-file-connections"></a>Podporované připojení souboru

|konektor|Trigger|Vstup|Výstup|
|:-----|:---:|:---:|:---:|
|[Pole](https://www.box.com)|x|x|x
|[Dropbox](https://www.dropbox.com)|x|x|x
|[FTP](https://docs.microsoft.com/azure/app-service/app-service-deploy-ftp)|x|x|x
|[OneDrive](https://onedrive.live.com)|x|x|x
|[OneDrive pro firmy](https://onedrive.live.com/about/business/)|x|x|x
|[SFTP](https://docs.microsoft.com/azure/connectors/connectors-create-api-sftp)|x|x|x
|[Google Drive](https://www.google.com/drive/)||x|x|

> [!NOTE]
> Externí připojení souboru lze použít také v [Azure Logic Apps](https://docs.microsoft.com/azure/connectors/apis-list)

## <a name="external-file-trigger-binding"></a>Externí soubor aktivovat vazby

Aktivační událost Azure externí soubor umožňuje monitorovat to vzdálená složka a spustíte kód funkce při zjištění změny.

Aktivační událost externí soubor používá těchto objektů JSON `bindings` pole function.json

```json
{
  "type": "apiHubFileTrigger",
  "name": "<Name of input parameter in function signature>",
  "direction": "in",
  "path": "<folder to monitor, and optionally a name pattern - see below>",
  "connection": "<name of external file connection - see above>"
}
```
<!---
See one of the following subheadings for more information:

* [Name patterns](#pattern)
* [File receipts](#receipts)
* [Handling poison files](#poison)
--->

<a name="pattern"></a>

### <a name="name-patterns"></a>Vzory názvů
Můžete zadat vzor názvů souborů v `path` vlastnost. Složka odkazuje musí existovat ve zprostředkovateli SaaS.
Příklady:

```json
"path": "input/original-{name}",
```

Tato cesta by vyhledat soubor s názvem *původní File1.txt* v *vstupní* složku a hodnota `name` proměnné v kódu funkce by `File1.txt`.

Další příklad:

```json
"path": "input/{filename}.{fileextension}",
```

Tato cesta by také vyhledat soubor s názvem *původní File1.txt*a hodnota `filename` a `fileextension` proměnných v kódu funkce by *původní File1* a *txt* .

Typ souboru souborů můžete omezit pomocí příkazu pevnou hodnotu pro tuto příponu. Například:

```json
"path": "samples/{name}.png",
```

V tomto případě pouze *.png* soubory *ukázky* složky aktivovat funkci.

Složené závorky jsou speciální znaky v vzory názvů. Pokud chcete zadat názvy souborů, které mají v názvu složené závorky, dvakrát složené závorky.
Například:

```json
"path": "images/{{20140101}}-{name}",
```

Tato cesta by vyhledat soubor s názvem *{20140101}-soundfile.mp3* v *bitové kopie* složku a `name` hodnotu proměnné v kód funkce by *soundfile.mp3*.

<a name="receipts"></a>

<!--- ### File receipts
The Azure Functions runtime makes sure that no external file trigger function gets called more than once for the same new or updated file.
It does so by maintaining *file receipts* to determine if a given file version has been processed.

File receipts are stored in a folder named *azure-webjobs-hosts* in the Azure storage account for your function app
(specified by the `AzureWebJobsStorage` app setting). A file receipt has the following information:

* The triggered function ("*&lt;function app name>*.Functions.*&lt;function name>*", for example: "functionsf74b96f7.Functions.CopyFile")
* The folder name
* The file type ("BlockFile" or "PageFile")
* The file name
* The ETag (a file version identifier, for example: "0x8D1DC6E70A277EF")

To force reprocessing of a file, delete the file receipt for that file from the *azure-webjobs-hosts* folder manually.
--->
<a name="poison"></a>

### <a name="handling-poison-files"></a>Zpracování poškozených souborů
Pokud se aktivace funkce externí soubor nezdaří, Azure Functions opakuje této funkce až 5 výskyty ve výchozím nastavení (včetně prvního pokusu) pro daný soubor.
Pokud selžou všechny 5 pokusů, funkce přidá zprávu do fronty úložiště s názvem *webjobs. apihubtrigger poison*. Zprávy ve frontě poškozených souborů je objekt JSON, který obsahuje následující vlastnosti:

* FunctionId (ve formátu  *&lt;funkce název aplikace >*. Funkce.  *&lt;název funkce >*)
* Typ souboru
* Název složky
* Název souboru
* Značka ETag (identifikátor verze souboru, například: "0x8D1DC6E70A277EF")


<a name="triggerusage"></a>

## <a name="trigger-usage"></a>Aktivační události využití
V jazyce C# funkce, vytvoření vazby na data vstupní soubor pomocí parametr s názvem v podpisu funkce, jako je třeba `<T> <name>`.
Kde `T` je datový typ chcete deserializaci dat do, a `paramName` je název, který jste zadali v [aktivovat JSON](#trigger). V Node.js funkce získáte přístup, data vstupní soubor pomocí `context.bindings.<name>`.

Soubor lze deserializovat do jakéhokoli z těchto typů:

* Všechny [objekt](https://msdn.microsoft.com/library/system.object.aspx) – vhodné pro data souboru serializací JSON.
  Pokud je deklarovat vlastní vstupní typ (například `FooType`), Azure Functions se pokusí rekonstruovat JSON data do zadaného typu.
* String – vhodné pro data textového souboru.

V jazyce C# funkce můžete také vytvořit vazbu na některý z následujících typů a Functions runtime pokusí k deserializaci dat souboru pomocí typu:

* `string`
* `byte[]`
* `Stream`
* `StreamReader`
* `TextReader`

## <a name="trigger-sample"></a>Ukázka aktivační události
Předpokládejme, že máte následující function.json, která definuje aktivační procedury externí soubor:

```json
{
    "disabled": false,
    "bindings": [
        {
            "name": "myFile",
            "type": "apiHubFileTrigger",
            "direction": "in",
            "path": "samples-workitems",
            "connection": "<name of external file connection>"
        }
    ]
}
```

Naleznete v ukázce pro specifický jazyk, který zaznamenává obsah každý soubor, který je přidán do sledované složky.

* [C#](#triggercsharp)
* [Node.js](#triggernodejs)

<a name="triggercsharp"></a>

### <a name="trigger-usage-in-c"></a>Aktivační události využití v jazyce C# #

```cs
public static void Run(string myFile, TraceWriter log)
{
    log.Info($"C# File trigger function processed: {myFile}");
}
```

<!--
<a name="triggerfsharp"></a>
### Trigger usage in F# ##
```fsharp

```
-->

<a name="triggernodejs"></a>

### <a name="trigger-usage-in-nodejs"></a>Aktivační události využití v Node.js

```javascript
module.exports = function(context) {
    context.log('Node.js File trigger function processed', context.bindings.myFile);
    context.done();
};
```

<a name="input"></a>

## <a name="external-file-input-binding"></a>Externí soubor vstupní vazby
Vstupní vazba Azure externí soubor můžete použít soubor z externí složky ve vaší funkci.

Externí soubor vstup do funkce používá těchto objektů JSON `bindings` pole function.json:

```json
{
  "name": "<Name of input parameter in function signature>",
  "type": "apiHubFile",
  "direction": "in",
  "path": "<Path of input file - see below>",
  "connection": "<name of external file connection>"
},
```

Je třeba počítat s následujícím:

* `path`musí obsahovat název složky a název souboru. Pokud máte například [aktivační událost fronty](functions-bindings-storage-queue.md) ve funkci, můžete použít `"path": "samples-workitems/{queueTrigger}"` tak, aby odkazoval na soubor v `samples-workitems` složku s názvem, který odpovídá názvu souboru, který je uvedený v aktivační události zprávě.   

<a name="inputusage"></a>

## <a name="input-usage"></a>Vstupní využití
V jazyce C# funkce, vytvoření vazby na data vstupní soubor pomocí parametr s názvem v podpisu funkce, jako je třeba `<T> <name>`.
Kde `T` je datový typ chcete deserializaci dat do, a `paramName` je název, který jste zadali v [vstupní vazby](#input). V Node.js funkce získáte přístup, data vstupní soubor pomocí `context.bindings.<name>`.

Soubor lze deserializovat do jakéhokoli z těchto typů:

* Všechny [objekt](https://msdn.microsoft.com/library/system.object.aspx) – vhodné pro data souboru serializací JSON.
  Pokud je deklarovat vlastní vstupní typ (například `InputType`), Azure Functions se pokusí rekonstruovat JSON data do zadaného typu.
* String – vhodné pro data textového souboru.

V jazyce C# funkce můžete také vytvořit vazbu na některý z následujících typů a Functions runtime pokusí k deserializaci dat souboru pomocí typu:

* `string`
* `byte[]`
* `Stream`
* `StreamReader`
* `TextReader`


<a name="output"></a>

## <a name="external-file-output-binding"></a>Externí soubor výstup vazby
Azure externí soubor výstup vazby umožňuje zapisovat soubory do složky externí ve vaší funkci.

Externí soubor výstup funkce používá těchto objektů JSON `bindings` pole function.json:

```json
{
  "name": "<Name of output parameter in function signature>",
  "type": "apiHubFile",
  "direction": "out",
  "path": "<Path of input file - see below>",
  "connection": "<name of external file connection>"
}
```

Je třeba počítat s následujícím:

* `path`musí obsahovat název složky a název souboru k zápisu. Pokud máte například [aktivační událost fronty](functions-bindings-storage-queue.md) ve funkci, můžete použít `"path": "samples-workitems/{queueTrigger}"` tak, aby odkazoval na soubor v `samples-workitems` složku s názvem, který odpovídá názvu souboru, který je uvedený v aktivační události zprávě.   

<a name="outputusage"></a>

## <a name="output-usage"></a>Využití výstupní
V C# funkce, můžete vytvořit vazbu k výstupnímu souboru pomocí pojmenované `out` jako parametr ve vaší podpis funkce `out <T> <name>`, kde `T` je datový typ chcete serializovat data do, a `paramName` je název, který jste zadali v [výstup vazby](#output). V Node.js funkce, přístup k výstupu pomocí souboru `context.bindings.<name>`.

Můžete napsat do výstupního souboru pomocí kteréhokoli z následujících typů:

* Všechny [objekt](https://msdn.microsoft.com/library/system.object.aspx) – užitečné pro serializaci JSON.
  Pokud je deklarovat vlastní výstupní typ (například `out OutputType paramName`), Azure Functions se pokusí o serializaci objektu do formátu JSON. Pokud výstupní parametr hodnotu null při ukončení funkce, funkce runtime vytvoří soubor jako objekt s hodnotou null.
* String – (`out string paramName`) vhodné pro data textového souboru. modul runtime funkce vytvoří soubor pouze v případě, že parametr řetězce má jinou hodnotu než null, při ukončení funkce.

V jazyce C# funkce také výstup můžete na některý z následujících typů:

* `TextWriter`
* `Stream`
* `CloudFileStream`
* `ICloudFile`
* `CloudBlockFile`
* `CloudPageFile`

<a name="outputsample"></a>

<a name="sample"></a>

## <a name="input--output-sample"></a>Vstup + ukázkový výstup
Předpokládejme, že máte následující function.json, který definuje [aktivace fronty úložiště](functions-bindings-storage-queue.md), externí soubor vstup a výstup externí soubor:

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
      "name": "myInputFile",
      "type": "apiHubFile",
      "path": "samples-workitems/{queueTrigger}",
      "connection": "<name of external file connection>",
      "direction": "in"
    },
    {
      "name": "myOutputFile",
      "type": "apiHubFile",
      "path": "samples-workitems/{queueTrigger}-Copy",
      "connection": "<name of external file connection>",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

Viz ukázka pro specifický jazyk, který kopíruje vstupní soubor do výstupního souboru.

* [C#](#incsharp)
* [Node.js](#innodejs)

<a name="incsharp"></a>

### <a name="usage-in-c"></a>Použití v jazyce C# #

```cs
public static void Run(string myQueueItem, string myInputFile, out string myOutputFile, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");
    myOutputFile = myInputFile;
}
```

<!--
<a name="infsharp"></a>
### Input usage in F# ##
```fsharp

```
-->

<a name="innodejs"></a>

### <a name="usage-in-nodejs"></a>Použití v Node.js

```javascript
module.exports = function(context) {
    context.log('Node.js Queue trigger function processed', context.bindings.myQueueItem);
    context.bindings.myOutputFile = context.bindings.myInputFile;
    context.done();
};
```

## <a name="next-steps"></a>Další kroky

> [!div class="nextstepaction"]
> [Další informace o Azure functions triggerů a vazeb](functions-triggers-bindings.md)
