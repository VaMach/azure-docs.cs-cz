---
title: Azure vazby HTTP funkce a webhooku | Microsoft Docs
description: "Pochopit, jak používat protokol HTTP a webhooku triggerů a vazeb v Azure Functions."
services: functions
documentationcenter: na
author: mattchenderson
manager: cfowler
editor: 
tags: 
keywords: "Funkce Azure, funkce, událostí zpracování, webhooků, dynamické výpočetní, bez serveru Architektura protokolu HTTP, API REST"
ms.assetid: 2b12200d-63d8-4ec1-9da8-39831d5a51b1
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 08/26/2017
ms.author: mahender
ms.openlocfilehash: 3c3247592cbe2bc382d220264b0c646ee566b8a7
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="azure-functions-http-and-webhook-bindings"></a>Azure funkce protokolu HTTP a webhooku vazby
[!INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

Tento článek vysvětluje postup konfigurace a práce s HTTP triggerů a vazeb v Azure Functions.
Pomocí těchto můžete Azure Functions sestavení bez serveru rozhraní API a reagovat na webhooky.

Azure Functions nabízí následující vazby:
- [Triggeru protokolu HTTP](#httptrigger) umožňuje vyvolají funkci s žádostí HTTP. To lze přizpůsobit reagovat na [webhooky](#hooktrigger).
- [HTTP výstup vazby](#output) umožňuje odpovědět na požadavek.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

[!INCLUDE [HTTP client best practices](../../includes/functions-http-client-best-practices.md)]

<a name="httptrigger"></a>

## <a name="http-trigger"></a>Trigger HTTP
Aktivace protokolu HTTP spustí funkce v odpovědi na požadavek HTTP. Můžete přizpůsobit ho reagovat na konkrétní adresu URL nebo sady metod HTTP. Aktivační událost INSTEAD HTTP lze také nakonfigurovat reagovat na webhooky. 

Pokud používáte portál funkce, můžete také začít používat hned použití předem vytvořené šablony. Vyberte **novou funkci** a zvolte "Rozhraní API a Webhooky" z **scénář** rozevíracího seznamu. Vyberte jednu z šablon a klikněte na **vytvořit**.

Ve výchozím nastavení aktivační procedury HTTP odpoví na žádost s stavový kód HTTP 200 OK a prázdným textem zprávy. Chcete-li upravit odpověď, nakonfigurovat [HTTP výstup vazby](#output)

### <a name="configuring-an-http-trigger"></a>Konfigurace aktivace protokolu HTTP
Aktivační událost INSTEAD HTTP je objekt JSON v definované `bindings` pole function.json, jak je znázorněno v následujícím příkladu:

```json
{
    "name": "req",
    "type": "httpTrigger",
    "direction": "in",
    "authLevel": "function",
    "methods": [ "get" ],
    "route": "values/{id}"
},
```
Vazba podporuje následující vlastnosti:

|Vlastnost  |Popis  |
|---------|---------|
| **Jméno** | Požadovaná proměnná používá v kódu funkce pro požadavek nebo textu požadavku. V tématu [práce s aktivační procedury HTTP z kódu](#httptriggerusage). |
| **Typ** | Vyžaduje - musí být nastavena na `httpTrigger`. |
| **směr** | Vyžaduje - musí být nastavena na `in`. |
| **authLevel** | Určuje, co klíče, pokud existuje, musí být přítomen v požadavku k vyvolání funkce. Hodnota může být jeden z následujících hodnot: <ul><li><code>anonymous</code>&mdash;Je vyžadován žádný klíč rozhraní API.</li><li><code>function</code>&mdash;Je požadován klíč rozhraní API specifických funkcí. Toto je výchozí hodnota, pokud žádný je k dispozici.</li><li><code>admin</code>&mdash;Je nezbytný hlavní klíč.</li></ul> Další informace najdete v tématu [práci s klíči](#keys). |
| **metody** | Pole metody HTTP, na které funkce odpoví. Pokud není zadaný, funkce odpoví na všechny metody HTTP. V tématu [přizpůsobení koncový bod HTTP](#url). |
| **trasy** | Definuje šablonu trasy řízení, které žádosti o funkce odpoví adresy URL. Výchozí hodnota, pokud je zadaný žádný je `<functionname>`. Další informace najdete v tématu [přizpůsobení koncový bod HTTP](#url). |
| **webHookType** | Nakonfiguruje tak, aby fungoval jako přijímač webhooku pro zadaného zprostředkovatele triggeru protokolu HTTP. Nepoužívejte _metody_ při použití tohoto nastavení. Hodnota může být jeden z následujících hodnot:<ul><li><code>genericJson</code>&mdash;Koncový bod pro obecné účely webhooku bez logiku pro konkrétního zprostředkovatele.</li><li><code>github</code>&mdash;Funkce odpoví na Githubu webhooky. Nepoužívejte _authLevel_ vlastnost při použití této hodnoty.</li><li><code>slack</code>&mdash;Funkce odpoví na Slack webhooky. Nepoužívejte _authLevel_ vlastnost při použití této hodnoty.</li></ul> Další informace najdete v tématu [neodpovídá na požadavky webhooky](#hooktrigger). |

<a name="httptriggerusage"></a>
### <a name="working-with-an-http-trigger-from-code"></a>Práce s aktivační procedury HTTP z kódu
Pro funkce C# a F #, můžou deklarovat typ aktivační událost vstupu být buď `HttpRequestMessage` nebo vlastní typ formátu .NET. Pokud se rozhodnete `HttpRequestMessage`, získat úplný přístup k objektu žádosti. Pro vlastní typ rozhraní .NET funkce pokusí analyzovat datovou část požadavku JSON a nastavte vlastnosti objektu. 

Pro funkce Node.js poskytuje Functions runtime textu žádosti namísto objektu žádosti. Další informace najdete v tématu [HTTP aktivační událost ukázky](#httptriggersample).


<a name="output"></a>
## <a name="http-response-output-binding"></a>Odpověď HTTP výstup vazby
Použijte protokol HTTP výstup vazby reagovat na odesílatel požadavku HTTP. Tato vazba vyžaduje aktivační procedury protokolu HTTP a umožňuje přizpůsobit odpověď přidružená k požadavku aktivační událost. Pokud výstup vazba HTTP není zadáno, aktivační procedury HTTP vrátí s prázdným textem zprávy HTTP 200 OK. 

### <a name="configuring-an-http-output-binding"></a>Konfigurace HTTP výstup vazby
Výstup vazba HTTP je definovaný v objektu JSON `bindings` pole function.json, jak je znázorněno v následujícím příkladu:

```json
{
    "name": "res",
    "type": "http",
    "direction": "out"
}
```
Vazba podporuje následující požadované vlastnosti:

|Vlastnost  |Popis  |
|---------|---------|
|**Jméno** | Název proměnné používá v kódu funkce pro odpověď. V tématu [práci s HTTP výstup vazba z kódu](#outputusage). |
| **Typ** |musí být nastavena na `http`. |
| **směr** | musí být nastavena na `out`. |

<a name="outputusage"></a>
### <a name="working-with-an-http-output-binding-from-code"></a>Práce s HTTP výstup vazba z kódu
Výstupní parametr můžete reagovat na protokolu http nebo webhooku volajícího. Můžete také použít vzory language standard odpovědi. Například odpovědí, najdete v části [HTTP aktivační událost ukázky](#httptriggersample) a [ukázky aktivační události Webhooku](#hooktriggersample).


<a name="hooktrigger"></a>
## <a name="responding-to-webhooks"></a>Odpovídá k webhookům
Aktivační událost INSTEAD HTTP s _webHookType_ je nakonfigurovaný tak, aby odpovídal na [webhooky](https://en.wikipedia.org/wiki/Webhook). Základní konfigurace používá nastavení "genericJson". To omezuje požadavky pouze na ty pomocí protokolu HTTP POST a s `application/json` typ obsahu.

Aktivační událost, můžete dále přizpůsobit konkrétní webhooku zprostředkovatele, jako například [Githubu](https://developer.github.com/webhooks/) nebo [Slack](https://api.slack.com/outgoing-webhooks). Pokud je zadána poskytovatele, Functions runtime může zpracovávat logiku zprostředkovatele ověření pro vás.  

### <a name="configuring-github-as-a-webhook-provider"></a>Konfigurace jako zprostředkovatel webhook Githubu
Chcete-li reagovat na Githubu webhooků, nejprve vytvoření funkce s aktivační procedury protokolu HTTP a nastavte **webHookType** vlastnost `github`. Zkopírujte jeho [URL](#url) a [klíč rozhraní API](#keys) do **přidat webhooku** stránky ve vašem úložišti GitHub. 

![](./media/functions-bindings-http-webhook/github-add-webhook.png)

Příklad, naleznete v části [vytvořit funkci aktivovány webhook Githubu](functions-create-github-webhook-triggered-function.md).

### <a name="configuring-slack-as-a-webhook-provider"></a>Konfigurace systému Slack jako zprostředkovatel webhooku
Slack webhooku generuje token pro vás místo umožňují určit, takže je nutné nakonfigurovat specifické funkce klíč pomocí tokenu z Slack. V tématu [práci s klíči](#keys).

<a name="url"></a>
## <a name="customizing-the-http-endpoint"></a>Přizpůsobení koncový bod HTTP
Ve výchozím nastavení vytvořit funkci pro triggeru protokolu HTTP, nebo Webhooku, funkce při adresovatelné s trasou ve tvaru:

    http://<yourapp>.azurewebsites.net/api/<funcname> 

Můžete přizpůsobit tuto trasu pomocí volitelné `route` vstup vazbu vlastnosti na triggeru protokolu HTTP. Jako příklad následující *function.json* soubor definuje `route` vlastnost pro aktivační procedury HTTP:

```json
{
    "bindings": [
    {
        "type": "httpTrigger",
        "name": "req",
        "direction": "in",
        "methods": [ "get" ],
        "route": "products/{category:alpha}/{id:int?}"
    },
    {
        "type": "http",
        "name": "res",
        "direction": "out"
    }
    ]
}
```

Pomocí této konfigurace, funkce je nyní adresovatelné s následující trasa místo původní trasy.

    http://<yourapp>.azurewebsites.net/api/products/electronics/357

To umožňuje kódu funkce, které podporují dva parametry v adrese, _kategorie_ a _id_. Můžete použít libovolnou [omezení trasy webové rozhraní API](https://www.asp.net/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2#constraints) s parametry. Následující kód funkce jazyka C# využívá oba parametry.

```csharp
public static Task<HttpResponseMessage> Run(HttpRequestMessage req, string category, int? id, 
                                                TraceWriter log)
{
    if (id == null)
        return  req.CreateResponse(HttpStatusCode.OK, $"All {category} items were requested.");
    else
        return  req.CreateResponse(HttpStatusCode.OK, $"{category} item with id = {id} has been requested.");
}
```

Zde je kód Node.js funkce používat stejné parametry trasy.

```javascript
module.exports = function (context, req) {

    var category = context.bindingData.category;
    var id = context.bindingData.id;

    if (!id) {
        context.res = {
            // status: 200, /* Defaults to 200 */
            body: "All " + category + " items were requested."
        };
    }
    else {
        context.res = {
            // status: 200, /* Defaults to 200 */
            body: category + " item with id = " + id + " was requested."
        };
    }

    context.done();
} 
```

Standardně jsou všechny funkce trasy předponou *rozhraní api*. Můžete také upravit nebo odebrat pomocí předpony `http.routePrefix` vlastnost ve vaší *host.json* souboru. Následující příklad odebere *rozhraní api* předpona trasy pomocí prázdný řetězec pro předponu v *host.json* souboru.

```json
{
    "http": {
    "routePrefix": ""
    }
}
```

Podrobné informace o tom, jak aktualizovat *host.json* funkce, najdete v souboru [jak aktualizovat soubory aplikace funkce](functions-reference.md#fileupdate). 

Informace o dalších vlastností můžete nakonfigurovat v vaše *host.json* souborů najdete v tématu [host.json odkaz](functions-host-json.md).


<a name="keys"></a>
## <a name="working-with-keys"></a>Práce s klíči
Aktivace protokolu HTTP vám umožňují používat klíče pro zvýšení zabezpečení. Standardní triggeru protokolu HTTP můžete použít jako klíč rozhraní API nutnosti klíč nacházet v žádosti. Webhooky slouží k autorizaci požadavků v mnoha různými způsoby v závislosti na tom, co poskytovatel podporuje klíče.

Klíče jsou uložené v rámci funkce aplikace v Azure a jsou zašifrovaná přinejmenším. Chcete-li zobrazit vaše klíče, vytvořit nové, nebo vrátit klíče na nové hodnoty, přejděte do jednoho z funkcí v rámci portálu a vyberte "Manage". 

Existují dva typy klíčů:
- **Klíče hostitele**: tyto klíče jsou sdíleny všechny funkce v rámci funkce aplikace. Když se použije jako klíč rozhraní API, tyto rutiny umožňují přístup k žádné funkce v rámci funkce aplikace.
- **Funkční klávesy**: tyto klíče se vztahují pouze na určité funkce, pod kterým jsou definovány. Když se použije jako klíč rozhraní API, tyto pouze povolí přístup k této funkce.

Každý klíč jmenuje pro referenci a je výchozí klíč (s názvem "Výchozí") na úrovni funkce a hostitele. **Hlavní klíč** je výchozí hostitele klíč s názvem "_master", která je definována pro každou aplikaci funkce. Nejde odvolat, tento klíč. Poskytuje přístup pro modul runtime rozhraní API pro správu. Pomocí `"authLevel": "admin"` vazba JSON vyžaduje tento klíč předkládané na vyžádání; další klíč výsledkem ověření se nezdařilo.

> [!IMPORTANT]  
> Z důvodu vyšší úroveň oprávnění udělená pomocí hlavního klíče nesmí sdílet s třetími stranami tento klíč nebo distribuovat v nativní klientské aplikace. Buďte opatrní při výběru úroveň oprávnění správce.

### <a name="api-key-authorization"></a>Autorizace pro klíč rozhraní API
Ve výchozím nastavení aktivační procedury HTTP vyžaduje klíč rozhraní API v požadavku HTTP. Proto požadavku HTTP obvykle vypadá takto:

    https://<yourapp>.azurewebsites.net/api/<function>?code=<ApiKey>

Klíč může být součástí proměnnou řetězce dotazu s názvem `code`, jak je uvedeno výše, nebo ho můžou být součástí `x-functions-key` hlavičky protokolu HTTP. Hodnota klíče může být jakékoli funkce definované pro tuto funkci, nebo všechny hostitele klíče.

Můžete povolit anonymní žádosti, které nevyžadují klíče. Můžete také vyžaduje, aby se používané hlavního klíče. Změnit výchozí úroveň ověřování pomocí `authLevel` vlastnost Vazba JSON. Další informace najdete v tématu [triggeru protokolu HTTP](#httptrigger).

### <a name="keys-and-webhooks"></a>Klíče a pomocí webhooků
Autorizace Webhooku se zpracovává souborem komponentu příjemce webhooku součástí triggeru protokolu HTTP, a tento mechanismus se může lišit podle typu webhooku. Jednotlivé mechanismy neodpovídá, ale závisí na klíč. Ve výchozím nastavení se používá klíč funkce s názvem "Výchozí". Pokud chcete používat jiný klíč, konfiguraci poskytovatele webhooku odeslat název klíče s požadavkem v jednom z následujících způsobů:

- **Řetězec dotazu**: Zprostředkovatel předá název klíče v `clientid` parametr řetězce dotazu, jako `https://<yourapp>.azurewebsites.net/api/<funcname>?clientid=<keyname>`.
- **Hlavička požadavku**: Zprostředkovatel předá název klíče v `x-functions-clientid` záhlaví.

> [!NOTE]
> Funkční klávesy mají přednost před klíče hostitele. Když dva klíče jsou definovány se stejným názvem, je vždy použít funkční klávesy.


<a name="httptriggersample"></a>
## <a name="http-trigger-samples"></a>Ukázky aktivace protokolu HTTP
Předpokládejme, že máte následující triggeru protokolu HTTP `bindings` pole function.json:

```json
{
    "name": "req",
    "type": "httpTrigger",
    "direction": "in",
    "authLevel": "function"
},
```

Viz ukázka konkrétní jazyk, které hledá `name` parametr buď v řetězci dotazu nebo textu požadavku HTTP.

* [C#](#httptriggercsharp)
* [F#](#httptriggerfsharp)
* [Node.js](#httptriggernodejs)


<a name="httptriggercsharp"></a>
### <a name="http-trigger-sample-in-c"></a>Ukázka aktivace protokolu HTTP v jazyce C# #
```csharp
using System.Net;
using System.Threading.Tasks;

public static async Task<HttpResponseMessage> Run(HttpRequestMessage req, TraceWriter log)
{
    log.Info($"C# HTTP trigger function processed a request. RequestUri={req.RequestUri}");

    // parse query parameter
    string name = req.GetQueryNameValuePairs()
        .FirstOrDefault(q => string.Compare(q.Key, "name", true) == 0)
        .Value;

    // Get request body
    dynamic data = await req.Content.ReadAsAsync<object>();

    // Set name to query string or body data
    name = name ?? data?.name;

    return name == null
        ? req.CreateResponse(HttpStatusCode.BadRequest, "Please pass a name on the query string or in the request body")
        : req.CreateResponse(HttpStatusCode.OK, "Hello " + name);
}
```

Můžete také vytvořit vazbu na objekt .NET místo **HttpRequestMessage**. Tento objekt se vytvoří z textu požadavku, analyzovat jako JSON. Podobně typu můžete předat výstup vazby a vrátí jako text odpovědi, společně s 200 stavový kód odpovědi HTTP.

```csharp
using System.Net;
using System.Threading.Tasks;

public static string Run(CustomObject req, TraceWriter log)
{
    return "Hello " + req?.name;
}

public class CustomObject {
     public String name {get; set;}
}
}
```

<a name="httptriggerfsharp"></a>
### <a name="http-trigger-sample-in-f"></a>Ukázka aktivační události protokolu HTTP v jazyce F # #
```fsharp
open System.Net
open System.Net.Http
open FSharp.Interop.Dynamic

let Run(req: HttpRequestMessage) =
    async {
        let q =
            req.GetQueryNameValuePairs()
                |> Seq.tryFind (fun kv -> kv.Key = "name")
        match q with
        | Some kv ->
            return req.CreateResponse(HttpStatusCode.OK, "Hello " + kv.Value)
        | None ->
            let! data = Async.AwaitTask(req.Content.ReadAsAsync<obj>())
            try
                return req.CreateResponse(HttpStatusCode.OK, "Hello " + data?name)
            with e ->
                return req.CreateErrorResponse(HttpStatusCode.BadRequest, "Please pass a name on the query string or in the request body")
    } |> Async.StartAsTask
```

Je nutné `project.json` souboru, který používá NuGet tak, aby odkazovaly `FSharp.Interop.Dynamic` a `Dynamitey` sestavení, a to takto:

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

To se používá NuGet načíst svoje závislosti a na ně odkazuje ve vašem skriptu.

<a name="httptriggernodejs"></a>
### <a name="http-trigger-sample-in-nodejs"></a>Ukázka aktivace protokolu HTTP v Node.JS
```javascript
module.exports = function(context, req) {
    context.log('Node.js HTTP trigger function processed a request. RequestUri=%s', req.originalUrl);

    if (req.query.name || (req.body && req.body.name)) {
        context.res = {
            // status: 200, /* Defaults to 200 */
            body: "Hello " + (req.query.name || req.body.name)
        };
    }
    else {
        context.res = {
            status: 400,
            body: "Please pass a name on the query string or in the request body"
        };
    }
    context.done();
};
```
     
<a name="hooktriggersample"></a>
## <a name="webhook-samples"></a>Ukázky Webhooku
Předpokládejme, že máte následující aktivační události webhooku `bindings` pole function.json:

```json
{
    "webHookType": "github",
    "name": "req",
    "type": "httpTrigger",
    "direction": "in",
},
```

Naleznete v ukázce pro specifický jazyk, který zaznamenává komentáře problém Githubu.

* [C#](#hooktriggercsharp)
* [F#](#hooktriggerfsharp)
* [Node.js](#hooktriggernodejs)

<a name="hooktriggercsharp"></a>

### <a name="webhook-sample-in-c"></a>Ukázka Webhooku v jazyce C# #
```csharp
#r "Newtonsoft.Json"

using System;
using System.Net;
using System.Threading.Tasks;
using Newtonsoft.Json;

public static async Task<object> Run(HttpRequestMessage req, TraceWriter log)
{
    string jsonContent = await req.Content.ReadAsStringAsync();
    dynamic data = JsonConvert.DeserializeObject(jsonContent);

    log.Info($"WebHook was triggered! Comment: {data.comment.body}");

    return req.CreateResponse(HttpStatusCode.OK, new {
        body = $"New GitHub comment: {data.comment.body}"
    });
}
```

<a name="hooktriggerfsharp"></a>

### <a name="webhook-sample-in-f"></a>Ukázka Webhooku v jazyce F # #
```fsharp
open System.Net
open System.Net.Http
open FSharp.Interop.Dynamic
open Newtonsoft.Json

type Response = {
    body: string
}

let Run(req: HttpRequestMessage, log: TraceWriter) =
    async {
        let! content = req.Content.ReadAsStringAsync() |> Async.AwaitTask
        let data = content |> JsonConvert.DeserializeObject
        log.Info(sprintf "GitHub WebHook triggered! %s" data?comment?body)
        return req.CreateResponse(
            HttpStatusCode.OK,
            { body = sprintf "New GitHub comment: %s" data?comment?body })
    } |> Async.StartAsTask
```

<a name="hooktriggernodejs"></a>

### <a name="webhook-sample-in-nodejs"></a>Ukázka Webhooku v Node.JS
```javascript
module.exports = function (context, data) {
    context.log('GitHub WebHook triggered!', data.comment.body);
    context.res = { body: 'New GitHub comment: ' + data.comment.body };
    context.done();
};
```


## <a name="next-steps"></a>Další kroky
[!INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]

