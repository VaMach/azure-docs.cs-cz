---
title: "Práce s proxy Azure Functions | Microsoft Docs"
description: "Přehled o tom, jak používat Azure funkce proxy"
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
ms.date: 04/11/2017
ms.author: alkarche
ms.openlocfilehash: 870dab3770f4595aa8b98e7f2dd18cf666b6dc67
ms.sourcegitcommit: 4ac89872f4c86c612a71eb7ec30b755e7df89722
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/07/2017
---
# <a name="work-with-azure-functions-proxies"></a>Práce s Azure Functions proxy

Tento článek vysvětluje postup konfigurace a práce s Azure funkce proxy. Pomocí této funkce můžete určovat koncové body ve vaší aplikaci funkce, které jsou implementované pomocí jiný prostředek. Tyto servery proxy pro přerušení velké rozhraní API do více aplikací – funkce (jako architektury mikroslužby), můžete použít při stále poskytovat jeden prostor rozhraní API pro klienty.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

> [!NOTE] 
> Standardní funkce fakturace se vztahuje na spuštěních proxy serveru. Další informace najdete v tématu [ceny Azure Functions](https://azure.microsoft.com/pricing/details/functions/).

## <a name="create"></a>Vytvořit proxy

V této části se dozvíte, jak vytvořit proxy na portálu funkce.

1. Otevřete [portál Azure]a poté přejděte k vaší aplikaci funkce.
2. V levém podokně vyberte **nového proxy**.
3. Zadejte název pro proxy.
4. Konfigurace koncového bodu, který je vystaven na tuto aplikaci funkce zadáním **šablonu trasy** a **metody HTTP**. Tyto parametry se chovat podle pravidel pro [aktivace protokolu HTTP].
5. Nastavte **URL back-end** na jiný koncový bod. Tento koncový bod může být funkce v jiné aplikaci funkce, nebo to může být ostatní rozhraní API. Hodnota nemusí být statické, a může obsahovat odkazy na [nastavení aplikace] a [parametry z původního požadavku klienta].
6. Klikněte na možnost **Vytvořit**.

Proxy nyní existuje jako nový koncový bod v aplikaci funkce. Z hlediska klienta je ekvivalentní HttpTrigger v Azure Functions. Váš nový proxy server můžete vyzkoušet tak, že adresa URL proxy serveru a testování pomocí vašeho oblíbeného klienta HTTP.

## <a name="modify-requests-responses"></a>Upravit požadavky a odpovědi

S funkcí proxy Azure můžete upravit žádostí a odpovědí z back-end. Tyto transformace můžete používat proměnné, jak jsou definovány v [použijte proměnné].

### <a name="modify-backend-request"></a>Upravit požadavek back-end

Ve výchozím nastavení je požadavek back-end inicializován jako kopii původního požadavku. Kromě nastavení adresy URL back-end, můžete provést změny metoda HTTP, hlaviček a parametrů řetězce dotazu. Změny hodnot, můžete odkazovat [nastavení aplikace] a [parametry z původního požadavku klienta].

V současné době není k dispozici žádné portálu prostředí pro úpravy požadavky back-end. Zjistěte, jak použít tuto možnost z proxies.json, najdete v tématu [definovat objekt requestOverrides].

### <a name="modify-response"></a>Upravit odpověď

Ve výchozím nastavení je odpověď klienta jako kopii odpovědi na back-end inicializován. Můžete změnit stavový kód, frázi důvodu, hlavičky a text z odpovědi. Změny hodnot, můžete odkazovat [nastavení aplikace], [parametry z původního požadavku klienta], a [parametry z back-end odpovědi].

V současné době není k dispozici žádné portálu prostředí pro úpravy odpovědi. Zjistěte, jak použít tuto možnost z proxies.json, najdete v tématu [definovat objekt responseOverrides].

## <a name="using-variables"></a>Použití proměnných

Konfigurace proxy serveru nemusí být statická. Podmínka vyhodnocena ho na použití proměnné z původního požadavku klienta, back-end odpovědi nebo nastavení aplikace.

### <a name="request-parameters"></a>Parametry žádosti referenční dokumentace

Parametry žádosti můžete použít jako vstupy pro vlastnost adresa URL back-end nebo jako součást úprava požadavky a odpovědi. Některé parametry mohou být vázány z šablony trasy, která je určená v konfiguraci proxy serveru základní a jiné mohou pocházet z vlastnosti příchozího požadavku.

#### <a name="route-template-parameters"></a>Parametry šablony trasy
Parametry, které se používají v šabloně trasy jsou k dispozici bude odkazovat podle názvu. Názvy parametrů jsou uzavřené do složených závorek ({}).

Pokud proxy server má šablonu trasy, jako například `/pets/{petId}`, adresa URL back-end může obsahovat hodnotu `{petId}`jako v `https://<AnotherApp>.azurewebsites.net/api/pets/{petId}`. Pokud šablonu trasy ukončí v zástupný znak, jako například `/api/{*restOfPath}`, hodnota `{restOfPath}` je řetězcová reprezentace zbývající segmenty cesty z příchozího požadavku.

#### <a name="additional-request-parameters"></a>Parametry další žádosti
Kromě parametry šablony trasy lze použít následující hodnoty v hodnotách konfigurace:

* **{request.method}** : Metoda protokolu HTTP, který se používá u původního požadavku.
* **{request.headers. \<HeaderName\>}**: hlavičku, který může číst z původního požadavku. Nahraďte  *\<HeaderName\>*  s názvem záhlaví, který chcete číst. Pokud není k dispozici hlavičky v požadavku, bude hodnota prázdný řetězec.
* **{request.querystring. \<ParameterName\>}**: parametr řetězce dotazu, který může číst z původního požadavku. Nahraďte  *\<ParameterName\>*  s názvem parametr, který chcete číst. Pokud parametr není zahrnut v žádosti, bude hodnota prázdný řetězec.

### <a name="response-parameters"></a>Odkaz na back-end odpovědi parametry

Odpověď parametry můžete použít jako součást úprava odpověď klientovi. Následující hodnoty lze použít v hodnotách konfigurace:

* **{backend.response.statusCode}** : Stavový kód protokolu HTTP, která je vrácena v odpovědi back-end.
* **{backend.response.statusReason}** : Frázi důvodu protokolu HTTP, která je vrácena v odpovědi back-end.
* **{backend.response.headers. \<HeaderName\>}**: hlavičku, který může číst z back-end odpovědi. Nahraďte  *\<HeaderName\>*  s názvem hlavičky, která si chcete přečíst. Pokud není k dispozici hlavičky v požadavku, bude hodnota prázdný řetězec.

### <a name="use-appsettings"></a>Odkaz nastavení aplikace

Můžete taky odkazovat [nastavení aplikace, které jsou definovány pro funkce aplikace](https://docs.microsoft.com/azure/azure-functions/functions-how-to-use-azure-function-app-settings#develop) tím, že název nastavení znaky procenta (%).

Například adresu URL back-end z *https://%ORDER_PROCESSING_HOST%/api/orders* by měla mít "% ORDER_PROCESSING_HOST %" nahrazeno hodnotou ORDER_PROCESSING_HOST nastavení.

> [!TIP] 
> Použít nastavení aplikace pro hostitele back-end, když máte více nasazení nebo testovací prostředí. Tímto způsobem můžete zajistit, aby vždy mluvení do správné back-end pro prostředí.

## <a name="advanced-configuration"></a>Pokročilá konfigurace

Proxy servery, které nakonfigurujete jsou uloženy v souboru proxies.json, který se nachází v kořenovém adresáři aplikace funkce. Můžete ručně upravit tento soubor a nasadit jako součást aplikace, když použijete některou z [metody nasazení](https://docs.microsoft.com/azure/azure-functions/functions-continuous-deployment) této funkce podporuje. Tato funkce musí být [povoleno](#enable) pro soubor, který má být zpracována. 

> [!TIP] 
> Pokud jste nenastavili jednu z metod nasazení, můžete také pracovat se soubor proxies.json na portálu. Přejděte do funkce aplikace, vyberte **funkce**a potom vyberte **App Service Editor**. Díky tomu můžete zobrazit celý soubor strukturu funkce aplikace a proveďte změny.

Proxies.JSON je definován objekt proxy, který se skládá z pojmenované proxy serverů a jejich definice. Případně, pokud vaše editor podporuje, můžete odkazovat [schématu JSON](http://json.schemastore.org/proxies) pro doplňování kódu. Příklad souboru může vypadat následovně:

```json
{
    "$schema": "http://json.schemastore.org/proxies",
    "proxies": {
        "proxy1": {
            "matchCondition": {
                "methods": [ "GET" ],
                "route": "/api/{test}"
            },
            "backendUri": "https://<AnotherApp>.azurewebsites.net/api/<FunctionName>"
        }
    }
}
```

Každý proxy má popisný název, například *proxy1* v předchozím příkladu. Objekt odpovídající definice proxy serveru je definováno následující vlastnosti:

* **matchCondition**: požadované – objekt definování požadavků, které aktivace provádění tento proxy server. Obsahuje dvě vlastnosti, které jsou sdíleny s [aktivace protokolu HTTP]:
    * _metody_: metody HTTP, které proxy serveru reaguje na pole. Pokud není zadaný, proxy server odpoví na všechny metody HTTP na trasy.
    * _trasy_: požadované – definuje šablonu trasy řízení, které adresy URL žádosti proxy odpoví. Na rozdíl od v aktivační události protokolu HTTP, není žádná výchozí hodnota.
* **backendUri**: adresa URL back-end prostředku, na kterou by měla být žádost směrovány přes proxy server. Tato hodnota může odkazovat nastavení aplikace a parametry z původního požadavku klienta. Pokud tuto vlastnost nezadáte, odpoví Azure Functions HTTP 200 OK.
* **requestOverrides**: objekt, který definuje transformace na požadavek back-end. V tématu [definovat objekt requestOverrides].
* **responseOverrides**: objekt, který definuje transformace na odpověď klienta. V tématu [definovat objekt responseOverrides].

> [!NOTE] 
> Vlastnost trasy proxy funkce Azure nectí vlastnost routePrefix konfigurace hostitele funkce. Pokud chcete zahrnout předpona například /api, musí být zahrnut ve vlastnosti trasy.

### <a name="requestOverrides"></a>Definování objekt requestOverrides

Objekt requestOverrides definuje změny provedené na žádost o při volání prostředků back-end. Objekt je definováno následující vlastnosti:

* **backend.Request.Method**: Metoda protokolu HTTP, který se používá k volání back-end.
* **backend.Request.QueryString. \<ParameterName\>**: parametr řetězce dotazu, který je možné nastavit pro volání back-end. Nahraďte  *\<ParameterName\>*  s názvem parametr, který chcete nastavit. Pokud je zadán prázdný řetězec, není parametr součástí požadavek back-end.
* **backend.Request.Headers. \<HeaderName\>**: hlavičku, která se dá nastavit pro volání back-end. Nahraďte  *\<HeaderName\>*  s názvem záhlaví, který chcete nastavit. Pokud zadáte prázdný řetězec, není k dispozici hlavičky v požadavku back-end.

Hodnoty můžete odkazovat nastavení aplikace a parametry z původního požadavku klienta.

Příklad konfigurace může vypadat následovně:

```json
{
    "$schema": "http://json.schemastore.org/proxies",
    "proxies": {
        "proxy1": {
            "matchCondition": {
                "methods": [ "GET" ],
                "route": "/api/{test}"
            },
            "backendUri": "https://<AnotherApp>.azurewebsites.net/api/<FunctionName>",
            "requestOverrides": {
                "backend.request.headers.Accept": "application/xml",
                "backend.request.headers.x-functions-key": "%ANOTHERAPP_API_KEY%"
            }
        }
    }
}
```

### <a name="responseOverrides"></a>Definování objekt responseOverrides

Objekt requestOverrides definuje změny provedené v odpovědi, který je předán zpět do klienta. Objekt je definováno následující vlastnosti:

* **response.statusCode**: stavový kód protokolu HTTP má být vrácen do klienta.
* **response.statusReason**: frázi důvodu protokolu HTTP má být vrácen do klienta.
* **Response.body**: řetězcovou reprezentaci těla má být vrácen do klienta.
* **Response.Headers. \<HeaderName\>**: hlavičku, která se dá nastavit pro odpověď klientovi. Nahraďte  *\<HeaderName\>*  s názvem záhlaví, který chcete nastavit. Pokud zadáte prázdný řetězec, záhlaví není zahrnutý v odpovědi.

Hodnoty můžete odkazovat nastavení aplikace, parametry z původního požadavku klienta a parametry z back-end odpovědi.

Příklad konfigurace může vypadat následovně:

```json
{
    "$schema": "http://json.schemastore.org/proxies",
    "proxies": {
        "proxy1": {
            "matchCondition": {
                "methods": [ "GET" ],
                "route": "/api/{test}"
            },
            "responseOverrides": {
                "response.body": "Hello, {test}",
                "response.headers.Content-Type": "text/plain"
            }
        }
    }
}
```
> [!NOTE] 
> V tomto příkladu text se nastavuje přímo, takže ne `backendUri` vlastnost je vyžadována. Příklad ukazuje, jak je možné použít Azure funkce proxy pro mocking rozhraní API.

## <a name="enable"></a>Povolit proxy Azure Functions

Ve výchozím nastavení jsou nyní k dispozici proxy! Pokud byly pomocí starší verze preview proxy servery a zakázaná proxy servery, musíte povolit proxy jednou v pořadí pro proxy provést ručně.

1. Otevřete [portál Azure]a poté přejděte k vaší aplikaci funkce.
2. Vyberte **funkce nastavení aplikace**.
3. Přepínač **povolit funkce proxy Azure (preview)** k **na**.

Můžete se taky vrátit zde o aktualizaci runtime proxy serveru je k dispozici nové funkce.

[portál Azure]: https://portal.azure.com
[aktivace protokolu HTTP]: https://docs.microsoft.com/azure/azure-functions/functions-bindings-http-webhook#http-trigger
[Modify the back-end request]: #modify-backend-request
[Modify the response]: #modify-response
[definovat objekt requestOverrides]: #requestOverrides
[definovat objekt responseOverrides]: #responseOverrides
[nastavení aplikace]: #use-appsettings
[použijte proměnné]: #using-variables
[parametry z původního požadavku klienta]: #request-parameters
[parametry z back-end odpovědi]: #response-parameters
