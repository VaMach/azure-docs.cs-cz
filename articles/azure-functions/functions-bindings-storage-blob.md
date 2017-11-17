---
title: "Vazby úložiště Azure Blob funkce"
description: "Pochopit, jak používat Azure Blob storage triggerů a vazeb v Azure Functions."
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
ms.date: 10/27/2017
ms.author: glenga
ms.openlocfilehash: 31a2fa3d3c87c16109514b130c95e731f401f8bd
ms.sourcegitcommit: c7215d71e1cdeab731dd923a9b6b6643cee6eb04
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/17/2017
---
# <a name="azure-functions-blob-storage-bindings"></a>Vazby úložiště Azure Blob funkce

Tento článek vysvětluje, jak pro práci s vazbami úložiště objektů Azure Blob v Azure Functions. Azure Functions podporuje aktivaci, vstup a výstup vazby pro objekty BLOB.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

> [!NOTE]
> [Účty úložiště BLOB jen](../storage/common/storage-create-storage-account.md#blob-storage-accounts) nejsou podporovány. Objekt BLOB úložiště triggerů a vazeb se vyžaduje účet úložiště pro obecné účely. 

## <a name="blob-storage-trigger"></a>Aktivační událost úložiště objektů BLOB

Spusťte funkci při zjištění do nové nebo aktualizované objektu blob pomocí aktivační událost úložiště objektů Blob. Obsahu objektu blob jsou uvedeny jako vstup do funkce.

> [!NOTE]
> Pokud používáte aktivační události objektu blob na plánu spotřeby, může být až 10 minut zpoždění při zpracování nové objekty BLOB po aplikaci funkce přešel nečinnosti. Po aplikaci funkce běží, objekty BLOB jsou zpracovávány okamžitě. Abyste se vyhnuli Tato počáteční prodleva, zvažte jednu z následujících možností:
> - Plán služby App Service použijte s povolenou funkci Always On.
> - Použijte jiný mechanismus pro aktivaci objektu blob zpracování, např. zprávu fronty, který obsahuje název objektu blob. Příklad, naleznete v části [příklad vazby vstupní a výstupní objekt blob později v tomto článku](#input--output---example).

## <a name="trigger---example"></a>Aktivační událost – příklad

Podívejte se na konkrétní jazyk příklad:

* [Předkompilované C#](#trigger---c-example)
* [Skript jazyka C#](#trigger---c-script-example)
* [JavaScript](#trigger---javascript-example)

### <a name="trigger---c-example"></a>Aktivační událost – příklad jazyka C#

Následující příklad ukazuje [předkompilovaných C#](functions-dotnet-class-library.md) kód, který zapíše do protokolu, když objekt blob je přidán nebo aktualizován v `samples-workitems` kontejneru.

```csharp
[FunctionName("BlobTriggerCSharp")]        
public static void Run([BlobTrigger("samples-workitems/{name}")] Stream myBlob, string name, TraceWriter log)
{
    log.Info($"C# Blob trigger function Processed blob\n Name:{name} \n Size: {myBlob.Length} Bytes");
}
```

Další informace o `BlobTrigger` atributů najdete v tématu [aktivační událost – atributy pro předkompilované C#](#trigger---attributes-for-precompiled-c).

### <a name="trigger---c-script-example"></a>Aktivační událost – příklad skriptu jazyka C#

Následující příklad ukazuje vazby v aktivační události objektu blob *function.json* souboru a [C# skript](functions-reference-csharp.md) kód, který používá vazby. Funkce zapíše do protokolu v případě, že objekt blob je přidán nebo aktualizován v `samples-workitems` kontejneru.

Zde je vazba dat v *function.json* souboru:

```json
{
    "disabled": false,
    "bindings": [
        {
            "name": "myBlob",
            "type": "blobTrigger",
            "direction": "in",
            "path": "samples-workitems",
            "connection":"MyStorageAccountAppSetting"
        }
    ]
}
```

[Konfigurace](#trigger---configuration) část vysvětluje tyto vlastnosti.

Tady je C# kód skriptu, který vytvoří vazbu `Stream`:

```cs
public static void Run(Stream myBlob, TraceWriter log)
{
   log.Info($"C# Blob trigger function Processed blob\n Name:{name} \n Size: {myBlob.Length} Bytes");
}
```

Tady je C# kód skriptu, který vytvoří vazbu `CloudBlockBlob`:

```cs
#r "Microsoft.WindowsAzure.Storage"

using Microsoft.WindowsAzure.Storage.Blob;

public static void Run(CloudBlockBlob myBlob, string name, TraceWriter log)
{
    log.Info($"C# Blob trigger function Processed blob\n Name:{name}\nURI:{myBlob.StorageUri}");
}
```

### <a name="trigger---javascript-example"></a>Aktivační událost – příklad v jazyce JavaScript

Následující příklad ukazuje vazby v aktivační události objektu blob *function.json* soubor a [kód JavaScript] (funkce node.md odkaz), který používá vazby. Funkce zapíše do protokolu v případě, že objekt blob je přidán nebo aktualizován v `samples-workitems` kontejneru.

Tady je *function.json* souboru:

```json
{
    "disabled": false,
    "bindings": [
        {
            "name": "myBlob",
            "type": "blobTrigger",
            "direction": "in",
            "path": "samples-workitems",
            "connection":"MyStorageAccountAppSetting"
        }
    ]
}
```

[Konfigurace](#trigger---configuration) část vysvětluje tyto vlastnosti.

Tady je kód jazyka JavaScript:

```javascript
module.exports = function(context) {
    context.log('Node.js Blob trigger function processed', context.bindings.myBlob);
    context.done();
};
```

## <a name="trigger---attributes-for-precompiled-c"></a>Aktivační událost - předkompilovaných atributy pro C#

Pro [předkompilovaných C#](functions-dotnet-class-library.md) funkce dovoluje konfigurovat aktivační události objektu blob následující atributy:

* [BlobTriggerAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/BlobTriggerAttribute.cs), definované v balíčku NuGet [Microsoft.Azure.WebJobs](http://www.nuget.org/packages/Microsoft.Azure.WebJobs)

  Cesta řetězec, který označuje kontejner si chcete přehrát trvá konstruktoru atributu a volitelně [vzor názvu objektu blob](#trigger---blob-name-patterns). Tady je příklad:

  ```csharp
  [FunctionName("ResizeImage")]
  public static void Run(
      [BlobTrigger("sample-images/{name}")] Stream image, 
      [Blob("sample-images-md/{name}", FileAccess.Write)] Stream imageSmall)
  ```

  Můžete nastavit `Connection` vlastnosti a určit účet úložiště, který chcete použít, jak je znázorněno v následujícím příkladu:

   ```csharp
  [FunctionName("ResizeImage")]
  public static void Run(
      [BlobTrigger("sample-images/{name}", Connection = "StorageConnectionAppSetting")] Stream image, 
      [Blob("sample-images-md/{name}", FileAccess.Write)] Stream imageSmall)
  ```

* [StorageAccountAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/StorageAccountAttribute.cs), definované v balíčku NuGet [Microsoft.Azure.WebJobs](http://www.nuget.org/packages/Microsoft.Azure.WebJobs)

  Poskytuje další možnost zadejte účet úložiště, který chcete použít. Konstruktor přebírá název nastavení aplikace, který obsahuje připojovací řetězec úložiště. Atribut lze použít na parametr, metoda nebo na úrovni třídy. Následující příklad ukazuje úroveň třídy a metody:

  ```csharp
  [StorageAccount("ClassLevelStorageAppSetting")]
  public static class AzureFunctions
  {
      [FunctionName("BlobTrigger")]
      [StorageAccount("FunctionLevelStorageAppSetting")]
      public static void Run( //...
  ```

Účet úložiště, který chcete použít, je určen v následujícím pořadí:

* `BlobTrigger` Atributu `Connection` vlastnost.
* `StorageAccount` Atribut použitý pro parametr stejné jako `BlobTrigger` atribut.
* `StorageAccount` Atribut použitý k funkci.
* `StorageAccount` Atribut použitý k třídě.
* Výchozí účet úložiště pro funkce aplikace (nastavení aplikace "AzureWebJobsStorage").

## <a name="trigger---configuration"></a>Aktivační událost - konfigurace

Následující tabulka popisuje vlastnosti konfigurace vazby, které jste nastavili v *function.json* souboru a `BlobTrigger` atribut.

|Vlastnost Function.JSON | Vlastnost atributu |Popis|
|---------|---------|----------------------|
|**Typ** | neuvedeno | musí být nastavena na `blobTrigger`. Tato vlastnost nastavena automaticky při vytváření aktivační události na portálu Azure.|
|**směr** | neuvedeno | musí být nastavena na `in`. Tato vlastnost nastavena automaticky při vytváření aktivační události na portálu Azure. Výjimky jsou uvedeny v [využití](#trigger---usage) části. |
|**Jméno** | neuvedeno | Název proměnné, která představuje objektů blob v kódu funkce. | 
|**Cesta** | **BlobPath** |Kontejner pro monitorování.  Může být [vzor názvu objektu blob](#trigger-blob-name-patterns). | 
|**připojení** | **Připojení** | Název nastavení aplikace, který obsahuje připojovací řetězec úložiště k použití pro tuto vazbu. Název nastavení aplikace začíná "AzureWebJobs", můžete zadat pouze zbytku názvu sem. Například pokud nastavíte `connection` na "MyStorage" Functions runtime vypadá pro aplikaci nastavení, která je s názvem "AzureWebJobsMyStorage." Pokud necháte `connection` prázdný, funkce používá modul runtime výchozí úložiště připojovací řetězec v nastavení aplikace, který je pojmenován `AzureWebJobsStorage`.<br><br>Připojovací řetězec nesmí být pro účet úložiště pro obecné účely [účet pouze objekt blob úložiště](../storage/common/storage-create-storage-account.md#blob-storage-accounts).<br>Pokud vyvíjíte místně, nastavení aplikace, přejděte do hodnoty [local.settings.json soubor](functions-run-local.md#local-settings-file).|

## <a name="trigger---usage"></a>Aktivační událost - využití

V jazyce C# a C# skript, přístup k datům objektu blob pomocí parametru metody `Stream paramName`. V jazyce C# skript `paramName` je hodnota zadaná v `name` vlastnost *function.json*. Můžete vázat na některý z následujících typů:

* `TextReader`
* `Stream`
* `ICloudBlob`(vyžaduje směr "inout" vazby v *function.json*)
* `CloudBlockBlob`(vyžaduje směr "inout" vazby v *function.json*)
* `CloudPageBlob`(vyžaduje směr "inout" vazby v *function.json*)
* `CloudAppendBlob`(vyžaduje směr "inout" vazby v *function.json*)

Jak jsme uvedli, vyžadují některé z těchto typů `inout` vazby směr v *function.json*. Tento směr nepodporuje standardního editoru na portálu Azure, je nutné použít rozšířené editor.

Pokud se očekává text objekty BLOB, můžete vázat na `string` typu. Toto nastavení se doporučuje jenom Pokud velikost objektu blob je malá, protože obsah celý objekt blob jsou načtena do paměti. Obecně platí, je vhodnější použít `Stream` nebo `CloudBlockBlob` typu.

V jazyce JavaScript, přístup k datům vstupního objektu blob pomocí `context.bindings.<name>`.

## <a name="trigger---blob-name-patterns"></a>Aktivační událost - vzory názvů objektů blob

Můžete zadat vzor názvu objektu blob v `path` vlastnost *function.json* nebo `BlobTrigger` konstruktoru atributu. Může být vzor názvů [výraz filtru nebo vazby](functions-triggers-bindings.md#binding-expressions-and-patterns).

### <a name="filter-on-blob-name"></a>Filtrování na název objektu blob

Následující příklad aktivace pouze na objekty BLOB v `input` kontejneru, která začínají řetězcem "původní-":

```json
"path": "input/original-{name}",
```
 
Pokud je název objektu blob *původní Blob1.txt*, hodnota `name` proměnná v kódu funkce `Blob1`.

### <a name="filter-on-file-type"></a>Filtrovat podle typu souboru

Následující příklad spustí jenom v *.png* soubory:

```json
"path": "samples/{name}.png",
```

### <a name="filter-on-curly-braces-in-file-names"></a>Filtrovat složené závorky v názvech souborů

Hledání složené závorky v názvech souborů, vyhnuli složené závorky pomocí dvou složené závorky. Následující příklad filtry pro objekty BLOB, které mají složené závorky v názvu:

```json
"path": "images/{{20140101}}-{name}",
```

Pokud je název objektu blob *{20140101}-soundfile.mp3*, `name` hodnotu proměnné v kód funkce je *soundfile.mp3*. 

### <a name="get-file-name-and-extension"></a>Získat název souboru a rozšíření

Následující příklad ukazuje, jak vytvořit vazbu na název souboru objektů blob a příponu samostatně:

```json
"path": "input/{blobname}.{blobextension}",
```
Pokud je název objektu blob *původní Blob1.txt*, hodnota `blobname` a `blobextension` proměnné v kódu funkce jsou *původní Blob1* a *txt*.

## <a name="trigger---metadata"></a>Aktivační událost - metadat

Aktivační události objektu blob nabízí několik vlastností metadat. Tyto vlastnosti lze použít jako součást vazby výrazy v jiných vazby nebo jako parametry v kódu. Tyto hodnoty mají stejnou sémantiku jako [CloudBlob](https://docs.microsoft.com/dotnet/api/microsoft.windowsazure.storage.blob.cloudblob?view=azure-dotnet) typu.


|Vlastnost  |Typ  |Popis  |
|---------|---------|---------|
|`BlobTrigger`|`string`|Cesta k spouštěcí objekt blob.|
|`Uri`|`System.Uri`|Identifikátor URI objektu blob pro primární umístění.|
|`Properties` |[BlobProperties](https://docs.microsoft.com/dotnet/api/microsoft.windowsazure.storage.blob.blobproperties)|Vlastnosti systému objektu blob. |
|`Metadata` |`IDictionary<string,string>`|Metadata definovaná uživatelem pro tento objekt blob.|

## <a name="trigger---blob-receipts"></a>Aktivační událost – objekt blob potvrzení

Modulu runtime Azure Functions zajistí, že žádná funkce aktivační události objektu blob volala více než jednou pro stejný objekt blob nové nebo aktualizované. Chcete-li zjistit, pokud byla zpracována na verzi daného objektu blob, udržuje *blob potvrzení*.

Azure funkce úložiště objektů blob v kontejneru nazvaném potvrzení *azure. webové úlohy hostitelů* v účtu úložiště Azure pro aplikaci funkce (definované nastavení aplikace `AzureWebJobsStorage`). Potvrzení o objektu blob obsahuje následující informace:

* Funkci spouštěná ("*&lt;funkce název aplikace >*. Funkce.  *&lt;název funkce >*", například:"MyFunctionApp.Functions.CopyBlob")
* Název kontejneru
* Typ objektu blob ("BlockBlob" nebo "PageBlob")
* Název objektu blob
* Značky ETag (identifikátor objektu blob verze, například: "0x8D1DC6E70A277EF")

Pokud chcete vynutit opětovné zpracování objektu blob, odstranit objekt blob příjmu pro tento objekt blob z *azure. webové úlohy hostitelů* kontejneru ručně.

## <a name="trigger---poison-blobs"></a>Aktivační událost - poškozených objektů BLOB

Pokud funkci aktivační události objektu blob se nezdaří pro daný objekt blob Azure Functions opakovaných pokusů, které funkce celkem 5krát ve výchozím nastavení. 

Pokud selžou všechny 5 pokusů, Azure Functions přidá zprávu do fronty úložiště s názvem *webjobs. blobtrigger poison*. Zprávy ve frontě pro poškozených objekty BLOB je objekt JSON, který obsahuje následující vlastnosti:

* FunctionId (ve formátu  *&lt;funkce název aplikace >*. Funkce.  *&lt;název funkce >*)
* BlobType ("BlockBlob" nebo "PageBlob")
* ContainerName
* BlobName
* Značka ETag (identifikátor objektu blob verze, například: "0x8D1DC6E70A277EF")

## <a name="trigger---polling-for-large-containers"></a>Aktivační událost - dotazování pro velké kontejnery

Pokud kontejner objektů blob, který je monitorován obsahuje více než 10 000 objektů BLOB, přihlaste se kontroly runtime funkce soubory, které chcete sledovat pro nové nebo změněné objekty BLOB. Tento proces může způsobit zpoždění. Funkce nemusí získat aktivuje až několik minut nebo déle po vytvoření objektu blob. Kromě toho [protokol úložiště jsou vytvořené na "best effort"](/rest/api/storageservices/About-Storage-Analytics-Logging) intervalech. Není zaručeno, že jsou zachyceny všechny události. Za určitých podmínek může být načteni protokoly. Pokud budete potřebovat rychlejší a spolehlivější blob zpracování, zvažte vytvoření [zprávu fronty](../storage/queues/storage-dotnet-how-to-use-queues.md) při vytváření objektu blob. Potom pomocí [aktivační událost fronty](functions-bindings-storage-queue.md) místo aktivační událost objektu blob ke zpracování objektu blob. Další možností je použít událost mřížky; Zobrazit kurz [automatické změně velikosti nahrán obrázky pomocí mřížky událostí](../event-grid/resize-images-on-storage-blob-upload-event.md).

## <a name="blob-storage-input--output-bindings"></a>Úložiště objektů BLOB vstup a výstup vazby

Použití objektů Blob storage vstup a výstup vazby ke čtení a zápisu objektů BLOB.

## <a name="input--output---example"></a>Vstup a výstup – příklad

Podívejte se na konkrétní jazyk příklad:

* [Předkompilované C#](#input--output---c-example)
* [Skript jazyka C#](#input--output---c-script-example)
* [JavaScript](#input--output---javascript-example)

### <a name="input--output---c-example"></a>Vstup a výstup – příklad jazyka C#

Následující příklad je [předkompilovaných C#](functions-dotnet-class-library.md) funkce, která používá aktivační události objektu blob a dvě vazby výstupního objektu blob. Funkce se aktivuje při vytváření objektu blob bitové kopie v *ukázkové obrázky* kontejneru. Vytvoří se kopie malé a střední velikost objektu blob bitové kopie. 

```csharp
[FunctionName("ResizeImage")]
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

### <a name="input--output---c-script-example"></a>Vstup a výstup – příklad skriptu jazyka C#

Následující příklad ukazuje, objektů blob vstup a výstup vazeb v *function.json* souboru a [C# skript](functions-reference-csharp.md) kód, který používá vazby. Funkce vytvoří kopii text objektu blob. Funkce se aktivuje zpráva fronty, který obsahuje název objektu blob pro kopírování. Nový objekt blob je s názvem *{originalblobname}-kopie*.

V *function.json* souboru `queueTrigger` vlastnost metadat slouží k určení názvu objektu blob v `path` vlastnosti:

```json
{
  "bindings": [
    {
      "queueName": "myqueue-items",
      "connection": "MyStorageConnectionAppSetting",
      "name": "myQueueItem",
      "type": "queueTrigger",
      "direction": "in"
    },
    {
      "name": "myInputBlob",
      "type": "blob",
      "path": "samples-workitems/{queueTrigger}",
      "connection": "MyStorageConnectionAppSetting",
      "direction": "in"
    },
    {
      "name": "myOutputBlob",
      "type": "blob",
      "path": "samples-workitems/{queueTrigger}-Copy",
      "connection": "MyStorageConnectionAppSetting",
      "direction": "out"
    }
  ],
  "disabled": false
}
``` 

[Konfigurace](#input--output---configuration) část vysvětluje tyto vlastnosti.

Tady je kód skriptu jazyka C#:

```cs
public static void Run(string myQueueItem, string myInputBlob, out string myOutputBlob, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");
    myOutputBlob = myInputBlob;
}
```

### <a name="input--output---javascript-example"></a>Vstup a výstup – příklad v jazyce JavaScript

Následující příklad ukazuje, objektů blob vstup a výstup vazeb v *function.json* soubor a [kód JavaScript] (funkce node.md odkaz), který používá vazby. Funkce vytvoří kopii objektu blob. Funkce se aktivuje zpráva fronty, který obsahuje název objektu blob pro kopírování. Nový objekt blob je s názvem *{originalblobname}-kopie*.

V *function.json* souboru `queueTrigger` vlastnost metadat slouží k určení názvu objektu blob v `path` vlastnosti:

```json
{
  "bindings": [
    {
      "queueName": "myqueue-items",
      "connection": "MyStorageConnectionAppSetting",
      "name": "myQueueItem",
      "type": "queueTrigger",
      "direction": "in"
    },
    {
      "name": "myInputBlob",
      "type": "blob",
      "path": "samples-workitems/{queueTrigger}",
      "connection": "MyStorageConnectionAppSetting",
      "direction": "in"
    },
    {
      "name": "myOutputBlob",
      "type": "blob",
      "path": "samples-workitems/{queueTrigger}-Copy",
      "connection": "MyStorageConnectionAppSetting",
      "direction": "out"
    }
  ],
  "disabled": false
}
``` 

[Konfigurace](#input--output---configuration) část vysvětluje tyto vlastnosti.

Tady je kód jazyka JavaScript:

```javascript
module.exports = function(context) {
    context.log('Node.js Queue trigger function processed', context.bindings.myQueueItem);
    context.bindings.myOutputBlob = context.bindings.myInputBlob;
    context.done();
};
```

## <a name="input--output---attributes-for-precompiled-c"></a>Vstup a výstup - atributy pro předkompilované C#

Pro [předkompilovaných C#](functions-dotnet-class-library.md) používat funkce, [BlobAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/BlobAttribute.cs), která je definována v balíčku NuGet [Microsoft.Azure.WebJobs](http://www.nuget.org/packages/Microsoft.Azure.WebJobs).

Konstruktoru atributu má cestu k objektu blob a `FileAccess` parametr určující číst nebo zapisovat, jak je znázorněno v následujícím příkladu:

```csharp
[FunctionName("ResizeImage")]
public static void Run(
    [BlobTrigger("sample-images/{name}")] Stream image, 
    [Blob("sample-images-md/{name}", FileAccess.Write)] Stream imageSmall)
```

Můžete nastavit `Connection` vlastnosti a určit účet úložiště, který chcete použít, jak je znázorněno v následujícím příkladu:

```csharp
[FunctionName("ResizeImage")]
public static void Run(
    [BlobTrigger("sample-images/{name}")] Stream image, 
    [Blob("sample-images-md/{name}", FileAccess.Write, Connection = "StorageConnectionAppSetting")] Stream imageSmall)
```

Můžete použít `StorageAccount` atribut určete účet úložiště na úrovni třídy, metoda nebo parametr. Další informace najdete v tématu [aktivační událost – atributy pro předkompilované C#](#trigger---attributes-for-precompiled-c).

## <a name="input--output---configuration"></a>Vstup a výstup - konfigurace

Následující tabulka popisuje vlastnosti konfigurace vazby, které jste nastavili v *function.json* souboru a `Blob` atribut.

|Vlastnost Function.JSON | Vlastnost atributu |Popis|
|---------|---------|----------------------|
|**Typ** | neuvedeno | musí být nastavena na `blob`. |
|**směr** | neuvedeno | Musí být nastavena na `in` pro vazbu vstupní nebo na více systémů pro vazbu výstup. Výjimky jsou uvedeny v [využití](#input--output---usage) části. |
|**Jméno** | neuvedeno | Název proměnné, která představuje objektů blob v kódu funkce.  Nastavte na `$return` Chcete-li funkce návratovou hodnotu.|
|**Cesta** |**BlobPath** | Cesta k objektu blob. | 
|**připojení** |**Připojení**| Název nastavení aplikace, který obsahuje připojovací řetězec úložiště k použití pro tuto vazbu. Název nastavení aplikace začíná "AzureWebJobs", můžete zadat pouze zbytku názvu sem. Například pokud nastavíte `connection` na "MyStorage" Functions runtime vypadá pro aplikaci nastavení, která je s názvem "AzureWebJobsMyStorage." Pokud necháte `connection` prázdný, funkce používá modul runtime výchozí úložiště připojovací řetězec v nastavení aplikace, který je pojmenován `AzureWebJobsStorage`.<br><br>Připojovací řetězec nesmí být pro účet úložiště pro obecné účely [účet pouze objekt blob úložiště](../storage/common/storage-create-storage-account.md#blob-storage-accounts).<br>Pokud vyvíjíte místně, nastavení aplikace, přejděte do hodnoty [local.settings.json soubor](functions-run-local.md#local-settings-file).|
|neuvedeno | **Přístup** | Určuje, zda jste se čtení nebo zápis. |

## <a name="input--output---usage"></a>Vstup a výstup - využití

V předkompilovaných C# a C# skript, přístup k objektu blob pomocí parametru metody `Stream paramName`. V jazyce C# skript `paramName` je hodnota zadaná v `name` vlastnost *function.json*. Můžete vázat na některý z následujících typů:

* `out string`
* `TextWriter` 
* `TextReader`
* `Stream`
* `ICloudBlob`(vyžaduje směr "inout" vazby v *function.json*)
* `CloudBlockBlob`(vyžaduje směr "inout" vazby v *function.json*)
* `CloudPageBlob`(vyžaduje směr "inout" vazby v *function.json*)
* `CloudAppendBlob`(vyžaduje směr "inout" vazby v *function.json*)

Jak jsme uvedli, vyžadují některé z těchto typů `inout` vazby směr v *function.json*. Tento směr nepodporuje standardního editoru na portálu Azure, je nutné použít rozšířené editor.

Při čtení objektů BLOB text, můžete vázat na `string` typu. Tento typ se doporučuje jenom, když velikost objektu blob je malá, protože obsah celý objekt blob jsou načtena do paměti. Obecně platí, je vhodnější použít `Stream` nebo `CloudBlockBlob` typu.

V jazyce JavaScript, přístup k objektu blob dat pomocí `context.bindings.<name>`.

## <a name="next-steps"></a>Další kroky

> [!div class="nextstepaction"]
> [Přejděte na rychlé spuštění, který používá aktivační událost úložiště objektů Blob](functions-create-storage-blob-triggered-function.md)

> [!div class="nextstepaction"]
> [Další informace o Azure functions triggerů a vazeb](functions-triggers-bindings.md)
