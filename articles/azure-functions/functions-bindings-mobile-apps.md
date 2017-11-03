---
title: Azure vazby funkce Mobile Apps | Microsoft Docs
description: "Pochopit, jak používat Azure Mobile Apps vazby v Azure Functions."
services: functions
documentationcenter: na
author: ggailey777
manager: cfowler
editor: 
tags: 
keywords: "Funkce Azure, funkce zpracování událostí, dynamické výpočetní architektura bez serveru"
ms.assetid: faad1263-0fa5-41a9-964f-aecbc0be706a
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 10/31/2016
ms.author: glenga
ms.openlocfilehash: d2c0e4e233761584bad2df05a8e702e4fc77e84f
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="azure-functions-mobile-apps-bindings"></a>Azure Mobile Apps funkce vazby
[!INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

Tento článek vysvětluje, jak nakonfigurovat a kódu [Azure Mobile Apps](../app-service-mobile/app-service-mobile-value-prop.md) vazeb v Azure Functions. Azure Functions podporuje vstup a výstup vazby pro Mobile Apps.

Mobile Apps vstup a výstup vazby umožňují [číst a zapisovat do datových tabulek](../app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#TableOperations) v mobilní aplikaci.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

<a name="input"></a>

## <a name="mobile-apps-input-binding"></a>Vstupní vazba Mobile Apps
Vstupní vazba Mobile Apps načte záznam z koncového bodu mobilní tabulky a předá ji do funkce. C# a F # funkce všechny změny provedené v záznamu se automaticky odešlou zpět k tabulce při ukončení funkce úspěšně.

Mobile Apps vstup do funkce používá následující objekt JSON v `bindings` pole function.json:

```json
{
    "name": "<Name of input parameter in function signature>",
    "type": "mobileTable",
    "tableName": "<Name of your mobile app's data table>",
    "id" : "<Id of the record to retrieve - see below>",
    "connection": "<Name of app setting that has your mobile app's URL - see below>",
    "apiKey": "<Name of app setting that has your mobile app's API key - see below>",
    "direction": "in"
}
```

Je třeba počítat s následujícím:

* `id`může být statické nebo může být založen na aktivační událost, která volá funkci. Například, pokud používáte [aktivační událost fronty]() pro funkce, pak `"id": "{queueTrigger}"` používá s řetězcovou hodnotou obsahující zprávy ve frontě jako ID záznamu pro načtení.
* `connection`by mělo obsahovat název nastavení aplikace v aplikaci funkce, která obsahuje adresu URL této mobilní aplikace. Funkce používá tuto adresu URL k vytvoření požadované operace REST pro mobilní aplikace. Můžete [vytvoření nastavení aplikace v aplikaci funkce]() obsahující adresu URL mobilních aplikací (který vypadá podobně jako `http://<appname>.azurewebsites.net`), pak zadejte název nastavení aplikace v `connection` vlastnost Vstupní vazba. 
* Je třeba zadat `apiKey` Pokud jste [implementovat klíč rozhraní API vašeho back-end mobilní aplikace Node.js](https://github.com/Azure/azure-mobile-apps-node/tree/master/samples/api-key), nebo [implementovat klíč rozhraní API vašeho back-end mobilní aplikace .NET](https://github.com/Azure/azure-mobile-apps-net-server/wiki/Implementing-Application-Key). K tomu, můžete [vytvoření nastavení aplikace v aplikaci funkce]() obsahující klíč rozhraní API, přidejte `apiKey` vlastnost vaše Vstupní vazba s názvem nastavení aplikace. 
  
  > [!IMPORTANT]
  > Tento klíč rozhraní API nesmí sdílet s klienty mobilní aplikace. Je měli jenom zajistit jeho distribuci bezpečně klientům straně služby, jako je Azure Functions. 
  > 
  > [!NOTE]
  > Azure Functions ukládá informace o připojení a klíče rozhraní API jako nastavení aplikace, tak, aby se změnami do vašeho úložiště správy zdrojového kódu. To chrání vaše citlivé informace.
  > 
  > 

<a name="inputusage"></a>

## <a name="input-usage"></a>Vstupní využití
V této části se dozvíte, jak používat vaše mobilní aplikace vstupní vazby v kódu funkce. 

Když se najde záznam se zadané tabulky a ID záznamu, je předaná do pojmenované [JObject](http://www.newtonsoft.com/json/help/html/t_newtonsoft_json_linq_jobject.htm) parametr (nebo v Node.js, je předaná do `context.bindings.<name>` objekt). Při záznamu není nalezen, je parametr `null`. 

Funkcí jazyka C# a F #, provedené změny do vstupní záznamu (vstupní parametr) je automaticky odeslán zpět k tabulce mobilní aplikace při ukončení funkce úspěšně. Ve funkcích Node.js, použijte `context.bindings.<name>` pro přístup k vstupního záznamu. Nelze upravit záznam v Node.js.

<a name="inputsample"></a>

## <a name="input-sample"></a>Vstupní ukázka
Předpokládejme, že máte následující function.json, který načte záznam tabulky mobilní aplikace s id zprávy ve frontě aktivační události:

```json
{
"bindings": [
    {
    "name": "myQueueItem",
    "queueName": "myqueue-items",
    "connection":"",
    "type": "queueTrigger",
    "direction": "in"
    },
    {
        "name": "record",
        "type": "mobileTable",
        "tableName": "MyTable",
        "id" : "{queueTrigger}",
        "connection": "My_MobileApp_Url",
        "apiKey": "My_MobileApp_Key",
        "direction": "in"
    }
],
"disabled": false
}
```

V tématu vzorku pro specifický jazyk, který používá vstupního záznamu z vazby. Ukázky jazyka C# a F # také upravit na záznam `text` vlastnost.

* [C#](#inputcsharp)
* [Node.js](#inputnodejs)

<a name="inputcsharp"></a>

### <a name="input-sample-in-c"></a>Vstupní ukázka v jazyce C# #

```cs
#r "Newtonsoft.Json"    
using Newtonsoft.Json.Linq;

public static void Run(string myQueueItem, JObject record)
{
    if (record != null)
    {
        record["Text"] = "This has changed.";
    }    
}
```

<!--
<a name="inputfsharp"></a>
### Input sample in F# ## 

```fsharp
#r "Newtonsoft.Json"    
open Newtonsoft.Json.Linq
let Run(myQueueItem: string, record: JObject) =
  inputDocument?text <- "This has changed."
```
-->

<a name="inputnodejs"></a>

### <a name="input-sample-in-nodejs"></a>Vstupní ukázka v Node.js

```javascript
module.exports = function (context, myQueueItem) {    
    context.log(context.bindings.record);
    context.done();
};
```

<a name="output"></a>

## <a name="mobile-apps-output-binding"></a>Mobile Apps výstup vazby
Použijte výstup Mobile Apps vazby k zápisu nového záznamu do tabulky koncový bod mobilní aplikace.  

Mobile Apps výstup funkce používá následující objekt JSON v `bindings` pole function.json:

```json
{
    "name": "<Name of output parameter in function signature>",
    "type": "mobileTable",
    "tableName": "<Name of your mobile app's data table>",
    "connection": "<Name of app setting that has your mobile app's URL - see below>",
    "apiKey": "<Name of app setting that has your mobile app's API key - see below>",
    "direction": "out"
}
```

Je třeba počítat s následujícím:

* `connection`by mělo obsahovat název nastavení aplikace v aplikaci funkce, která obsahuje adresu URL této mobilní aplikace. Funkce používá tuto adresu URL k vytvoření požadované operace REST pro mobilní aplikace. Můžete [vytvoření nastavení aplikace v aplikaci funkce]() obsahující adresu URL mobilních aplikací (který vypadá podobně jako `http://<appname>.azurewebsites.net`), pak zadejte název nastavení aplikace v `connection` vlastnost Vstupní vazba. 
* Je třeba zadat `apiKey` Pokud jste [implementovat klíč rozhraní API vašeho back-end mobilní aplikace Node.js](https://github.com/Azure/azure-mobile-apps-node/tree/master/samples/api-key), nebo [implementovat klíč rozhraní API vašeho back-end mobilní aplikace .NET](https://github.com/Azure/azure-mobile-apps-net-server/wiki/Implementing-Application-Key). K tomu, můžete [vytvoření nastavení aplikace v aplikaci funkce]() obsahující klíč rozhraní API, přidejte `apiKey` vlastnost vaše Vstupní vazba s názvem nastavení aplikace. 
  
  > [!IMPORTANT]
  > Tento klíč rozhraní API nesmí sdílet s klienty mobilní aplikace. Je měli jenom zajistit jeho distribuci bezpečně klientům straně služby, jako je Azure Functions. 
  > 
  > [!NOTE]
  > Azure Functions ukládá informace o připojení a klíče rozhraní API jako nastavení aplikace, tak, aby se změnami do vašeho úložiště správy zdrojového kódu. To chrání vaše citlivé informace.
  > 
  > 

<a name="outputusage"></a>

## <a name="output-usage"></a>Využití výstupní
V této části se dozvíte, jak používat Mobile Apps výstupu vazby v kódu funkce. 

V C# funkce, použijte parametr s názvem výstup typu `out object` pro přístup k výstupu záznamu. Ve funkcích Node.js, použijte `context.bindings.<name>` pro přístup k výstupu záznamu.

<a name="outputsample"></a>

## <a name="output-sample"></a>Ukázkový výstup
Předpokládejme, že máte následující function.json, která definuje aktivační procedury fronty a výstup mobilní aplikace:

```json
{
"bindings": [
    {
    "name": "myQueueItem",
    "queueName": "myqueue-items",
    "connection":"",
    "type": "queueTrigger",
    "direction": "in"
    },
    {
    "name": "record",
    "type": "mobileTable",
    "tableName": "MyTable",
    "connection": "My_MobileApp_Url",
    "apiKey": "My_MobileApp_Key",
    "direction": "out"
    }
],
"disabled": false
}
```

Naleznete v ukázce pro specifický jazyk, který vytvoří záznam v koncovém bodě Mobile Apps tabulku s obsahem zprávy ve frontě.

* [C#](#outcsharp)
* [Node.js](#outnodejs)

<a name="outcsharp"></a>

### <a name="output-sample-in-c"></a>Ukázka výstupu v jazyce C# #

```cs
public static void Run(string myQueueItem, out object record)
{
    record = new {
        Text = $"I'm running in a C# function! {myQueueItem}"
    };
}
```

<!--
<a name="outfsharp"></a>
### Output sample in F# ## 
```fsharp

```
-->
<a name="outnodejs"></a>

### <a name="output-sample-in-nodejs"></a>Ukázka výstupu v Node.js

```javascript
module.exports = function (context, myQueueItem) {

    context.bindings.record = {
        text : "I'm running in a Node function! Data: '" + myQueueItem + "'"
    }   

    context.done();
};
```

## <a name="next-steps"></a>Další kroky
[!INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]

