---
title: "Monitorování služeb Node.js pomocí Azure Application Insights | Dokumentace Microsoftu"
description: "Monitorujte výkon a diagnostikujte problémy ve službách Node.js pomocí Application Insights."
services: application-insights
documentationcenter: nodejs
author: joshgav
manager: carmonm
ms.assetid: 2ec7f809-5e1a-41cf-9fcd-d0ed4bebd08c
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/01/2017
ms.author: sewhee
ms.translationtype: Human Translation
ms.sourcegitcommit: 64bd7f356673b385581c8060b17cba721d0cf8e3
ms.openlocfilehash: 76a8025cd2a67533beb321c88e924517c1977dfc
ms.contentlocale: cs-cz
ms.lasthandoff: 05/02/2017

---

# <a name="monitor-your-nodejs-services-and-apps-with-application-insights"></a>Monitorování služeb a aplikací Node.js pomocí Application Insights

[Azure Application Insights](app-insights-overview.md) monitoruje po nasazení vaše back-endové služby a komponenty a tím vám pomáhá [zjišťovat a rychle diagnostikovat problémy nejen s výkonem](app-insights-detect-triage-diagnose.md). Tuto službu můžete použít pro služby Node.js hostované kdekoli: ve vašem datacentru, na virtuálních počítačích nebo ve webových aplikacích Azure a dokonce i v dalších veřejných cloudech.

Pokud chcete přijímat, ukládat a prozkoumávat data monitorování, postupujte podle následujících pokynů a vložte do svého kódu agenta a nastavte v Azure odpovídající prostředek Application Insights. Agent do tohoto prostředku odesílá data pro další analýzy a prozkoumávání.

Agent Node.js dokáže automaticky monitorovat příchozí a odchozí požadavky HTTP, řadu systémových metrik a výjimky. Počínaje verzí 0.20 dokáže monitorovat také některé běžné balíčky třetích stran, jako například `mongodb`, `mysql` a `redis`. Všechny události související s příchozím požadavkem HTTP se korelují za účelem rychlejšího řešení potíží.

Můžete monitorovat více aspektů aplikace a systému, pokud je ručně nakonfigurujete pomocí rozhraní API agenta popsaného níže.

![Příklady tabulek sledování výkonu](./media/app-insights-nodejs/10-perf.png)

## <a name="getting-started"></a>Začínáme

Projděme si nastavení monitorování pro aplikaci nebo službu.

### <a name="resource"></a> Nastavení prostředku App Insights

**Než začnete**, ujistěte se, že máte předplatné Azure nebo [zdarma získejte nové předplatné][azure-free-offer]. Pokud vaše organizace již má předplatné Azure, správce vás do něj může přidat pomocí [těchto pokynů][add-aad-user].

[azure-free-offer]: https://azure.microsoft.com/en-us/free/
[add-aad-user]: https://docs.microsoft.com/en-us/azure/active-directory/active-directory-users-create-azure-portal

Nyní se přihlaste k webu [Azure Portal][portal] a vytvořte prostředek Application Insights, jak je znázorněno na následujícím obrázku – Klikněte na Nový > Vývojářské nástroje > Application Insights. Prostředek zahrnuje koncový bod pro příjem telemetrických dat, úložiště pro tato data, uložené sestavy a řídicí panely, konfigurace pravidel a upozornění a ještě více.

![Vytvoření prostředku App Insights](./media/app-insights-nodejs/03-new_appinsights_resource.png)

Na stránce vytváření prostředku vyberte z rozevírací nabídky typu aplikace možnost Aplikace Node.js. Typ aplikace určuje výchozí sadu řídicích panelů a sestav, které se pro vás vytvoří. Ale nebojte se, každý prostředek App Insights může ve skutečnosti shromažďovat data z jakéhokoli jazyka a libovolné platformy.

![Formulář Nový prostředek App Insights](./media/app-insights-nodejs/04-create_appinsights_resource.png)

### <a name="agent"></a> Nastavení agenta Node.js

Nyní je čas do aplikace vložit agenta, aby mohl shromažďovat data.
Začněte zkopírováním instrumentačního klíče vašeho prostředku (dále jen `ikey`) z portálu, jak je znázorněno níže. Systém App Insights pomocí tohoto klíče mapuje data na váš prostředek Azure, proto ho musíte zadat ve svém kódu jako proměnnou prostředí, aby ho agent mohl použít.  

![Zkopírování instrumentačního klíče](./media/app-insights-nodejs/05-appinsights_ikey_portal.png)

Dále přidejte do závislostí aplikace knihovnu agenta Node.js prostřednictvím souboru package.json. Z kořenové složky aplikace spusťte:

```bash
npm install applicationinsights --save
```

Nyní je potřeba knihovnu explicitně načíst v kódu. Vzhledem k tomu, že agent vkládá instrumentaci do mnoha dalších knihoven, musíte ho načíst co nejdříve, dokonce ještě před dalšími příkazy `require`. Začněte tak, že na začátek vašeho prvního souboru .js přidáte:

```javascript
const appInsights = require("applicationinsights");
appInsights.setup("<instrumentation_key>");
appInsights.start();
```

Metoda `setup` konfiguruje instrumentační klíč (a tedy prostředek Azure), který se použije jako výchozí pro všechny sledované položky. Po dokončení konfigurace zavolejte metodu `start`, která spustí shromažďování a odesílání telemetrických dat.

Istrumentační klíč můžete místo ručního předávání do metod `setup()` nebo`getClient()` zadat také přes proměnnou prostředí APPINSIGHTS\_INSTRUMENTATIONKEY. Tento postup umožňuje oddělit instrumentační klíče od potvrzeného zdrojového kódu a určit různé instrumentační klíče pro různá prostředí.

Další možnosti konfigurace jsou popsány níže.

Agenta můžete vyzkoušet bez odesílání telemetrie tak, že instrumentační klíč nastavíte na neprázdný řetězec.

### <a name="monitor"></a> Monitorování aplikace

Agent automaticky shromažďuje telemetrii o modulu runtime Node.js a některých dalších modulech třetích stran. Nyní použijte svou aplikaci k vygenerování nějakých dat.

Potom na webu [Azure Portal][portal] přejděte do prostředku Application Insights, který jste vytvořili dříve, a na časové ose Přehled vyhledejte vaše první datové body, jak je znázorněno na následujícím obrázku. Přes grafy se můžete proklikat k dalším podrobnostem.

![První datové body](./media/app-insights-nodejs/12-first-perf.png)

Kliknutím na tlačítko Mapa aplikace zobrazte zjištěnou topologii vaší aplikace, jak je znázorněno na následujícím obrázku. Přes komponenty na mapě se můžete proklikat k dalším podrobnostem.

![Mapa jedné aplikace](./media/app-insights-nodejs/06-appinsights_appmap.png)

Pomocí dalších zobrazení dostupných v části Prozkoumávání můžete zjistit další informace o vaší aplikaci a řešit problémy.

![Část Prozkoumávání](./media/app-insights-nodejs/07-appinsights_investigate_blades.png)

#### <a name="no-data"></a>Žádná data?

Vzhledem k tomu, že agent seskupuje data do dávek pro odesílání, může docházet k prodlevám v zobrazení položek na portálu. Pokud ve svém prostředku nevidíte data, vyzkoušejte některou z následujících oprav:

* Používejte aplikaci trochu více a proveďte více akcí pro vygenerování další telemetrie.
* V zobrazení prostředku na portálu klikněte na **Aktualizovat**. Grafy se samy pravidelně automaticky aktualizují, ale aktualizace vynutí, že k tomu dojde okamžitě.
* Ověřte, že jsou otevřené [požadované výchozí porty](app-insights-ip-addresses.md).
* Otevřete dlaždici [Vyhledávání](app-insights-diagnostic-search.md) a vyhledejte jednotlivé události.
* Přečtěte si část [Nejčastější dotazy][].


## <a name="agent-configuration"></a>Konfigurace agenta

Dále jsou uvedeny metody konfigurace agenta a jejich výchozí hodnoty.

Pro úplnou korelaci událostí ve službě nezapomeňte nastavit `.setAutoDependencyCorrelation(true)`. To agentovi umožní sledovat kontext napříč asynchronními zpětnými voláními v Node.js.

```javascript
const appInsights = require("applicationinsights");
appInsights.setup("<instrumentation_key>")
    .setAutoDependencyCorrelation(false)
    .setAutoCollectRequests(true)
    .setAutoCollectPerformance(true)
    .setAutoCollectExceptions(true)
    .setAutoCollectDependencies(true)
    .start();
```

## <a name="agent-api"></a>Rozhraní API agenta

<!-- TODO: Fully document agent API. -->

Rozhraní API agenta pro .NET je podrobně popsané [tady](app-insights-api-custom-events-metrics.md).

Pomocí klienta Application Insights pro Node.js můžete sledovat jakékoli žádosti, události, metriky nebo výjimky. Následující příklad ukazuje některá dostupná rozhraní API.

```javascript
let appInsights = require("applicationinsights");
appInsights.setup().start(); // assuming ikey in env var
let client = appInsights.getClient();

client.trackEvent("my custom event", {customProperty: "custom property value"});
client.trackException(new Error("handled exceptions can be logged with this method"));
client.trackMetric("custom metric", 3);
client.trackTrace("trace message");

let http = require("http");
http.createServer( (req, res) => {
  client.trackRequest(req, res); // Place at the beginning of your request handler
});
```

### <a name="track-your-dependencies"></a>Sledování závislostí

```javascript
let appInsights = require("applicationinsights");
let client = appInsights.getClient();

var success = false;
let startTime = Date.now();
// execute dependency call here....
let duration = Date.now() - startTime;
success = true;

client.trackDependency("dependency name", "command name", duration, success);
```

### <a name="add-a-custom-property-to-all-events"></a>Přidání vlastní vlastnosti do všech událostí

```javascript
appInsights.client.commonProperties = {
    environment: process.env.SOME_ENV_VARIABLE
};
```

### <a name="track-http-get-requests"></a>Sledování požadavků HTTP GET

```javascript
var server = http.createServer((req, res) => {
    if ( req.method === "GET" ) {
            appInsights.client.trackRequest(req, res);
    }
    // other work here....
    res.end();
});
```

### <a name="track-server-startup-time"></a>Sledování času spuštění serveru

```javascript
let start = Date.now();
server.on("listening", () => {
    let duration = Date.now() - start;
    appInsights.client.trackMetric("server startup time", duration);
});
```

## <a name="more-resources"></a>Další zdroje informací

* [Monitorování telemetrických dat na portálu](app-insights-dashboards.md)
* [Psaní analytických dotazů do telemetrických dat](app-insights-analytics-tour.md)

<!--references-->

[portal]: https://portal.azure.com/
[Nejčastější dotazy]: app-insights-troubleshoot-faq.md

