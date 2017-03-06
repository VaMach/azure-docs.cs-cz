---
title: "Sledování aplikace Node.js pomocí sady Azure Application Insights SDK | Dokumentace Microsoftu"
description: "Analýza místního využití, dostupnosti a výkonu nebo webová aplikace Microsoft Azure pomocí Application Insights."
services: application-insights
documentationcenter: 
author: alancameronwills
manager: carmonm
ms.assetid: 2ec7f809-5e1a-41cf-9fcd-d0ed4bebd08c
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: get-started-article
ms.date: 02/23/2017
ms.author: awills
translationtype: Human Translation
ms.sourcegitcommit: 46b829ce52994a5112494145a02e78859c5fae2d
ms.openlocfilehash: d4c7fa2058b1c07671329304c37630d2e6e8e8a7
ms.lasthandoff: 02/23/2017


---
# <a name="add-application-insights-sdk-to-monitor-your-nodejs-app"></a>Přidejte Application Insights SDK pro sledování aplikace Node.js


[Azure Application Insights](app-insights-overview.md) monitoruje vaše živé aplikace a pomáhá vám [najít a diagnostikovat problémy s výkonem a výjimkami](app-insights-detect-triage-diagnose.md) a [zjistit, jak se vaše aplikace používá](app-insights-overview-usage.md). Funguje pro aplikace, které jsou hostovány na místních serverech IIS nebo na virtuálních počítačích Azure a také webových aplikacích Azure.

Sada SDK poskytuje automatický sběr hodnot příchozích požadavků HTTP a odpovědí, čítačů výkonu (procesor, paměť, RPS) a neošetřených výjimek. Kromě toho můžete přidat vlastní volání ke sledování závislostí, metrik a dalších událostí.

![Příklady tabulek sledování výkonu](./media/app-insights-nodejs/10-perf.png)

#### <a name="before-you-start"></a>Než začnete
Budete potřebovat:

* Předplatné [Microsoft Azure](http://azure.com). Pokud váš tým nebo společnost má předplatné Azure, vlastník vás do něj může přidat pomocí vašeho [účtu Microsoft](http://live.com).

## <a name="a-nameaddacreate-an-application-insights-resource"></a><a name="add"></a> Vytvoření prostředku Application Insights
Přihlaste se k webu [Azure Portal][portal] a vytvořte nový prostředek Application Insights. [Prostředek][roles] v Azure je instancí služby. Tento prostředek je místo, kde se analyzuje a prezentuje telemetrie z vaší aplikace.

![Klikněte na tlačítko Nový, Application Insights](./media/app-insights-nodejs/01-new-asp.png)

Vyberte jako typ aplikace Obecný. Volba typu aplikace nastaví výchozí obsah oken prostředků a vlastnosti viditelné v [Průzkumníku metrik][metrics].

#### <a name="copy-the-instrumentation-key"></a>Zkopírovat klíč instrumentace
Klíč identifikuje prostředek a nainstalujte ho brzy do sady SDK pro přímá data do prostředku.

![Klikněte na tlačítko Vlastnosti, vyberte klíč a stiskněte klávesy ctrl + C](./media/app-insights-nodejs/02-props-asp.png)

## <a name="a-namesdka-install-the-sdk-in-your-application"></a><a name="sdk"></a> Instalace sady SDK do aplikace
```
npm install applicationinsights --save
```

## <a name="usage"></a>Využití
Tím povolíte sledování požadavku, sledování neošetřených výjimek a sledování výkonu systému (procesoru a paměti nebo RPS).

```javascript

var appInsights = require("applicationinsights");
appInsights.setup("<instrumentation_key>").start();
```

Klíč instrumentace můžete také nastavit v proměnném prostředí APPINSIGHTS_INSTRUMENTATIONKEY. Když toto dokončíte, není třeba žádný argument při volání metody `appInsights.setup()` nebo `appInsights.getClient()`.

Sadu SDK můžete vyzkoušet bez odesílání telemetrie: nastavte klíč instrumentace na neprázdný řetězec.

## <a name="a-nameruna-run-your-project"></a><a name="run"></a> Spuštění projektu
Spusťte aplikaci a vyzkoušejte ji: otevřete různé stránky k vygenerování nějaké telemetrie.

## <a name="a-namemonitora-view-your-telemetry"></a><a name="monitor"></a> Zobrazení telemetrických dat
Přihlaste se do [portálu Azure](https://portal.azure.com) a procházejte do zdroje Application Insights.

Vyhledejte data na stránce Přehled. Na první pohled uvidíte pouze jeden nebo dva body. Příklad:

![Proklikejte se k dalším datům](./media/app-insights-nodejs/12-first-perf.png)

Proklikejte se prostřednictvím jakékoli grafu pro zobrazení podrobnějších metrik. [Další informace o metrikách.][perf]

#### <a name="no-data"></a>Žádná data?
* Použijte aplikaci a otevřete různé stránky tak, aby došlo k vygenerování nějaké telemetrie.
* Otevřete dlaždici [Vyhledávání](app-insights-diagnostic-search.md) a zobrazte jednotlivé události. Někdy trvá událostem trochu déle, než se dostanou skrz kanály metriky.
* Počkejte několik sekund a klikněte na tlačítko **Aktualizovat**. Grafy se pravidelně samy aktualizují, ale můžete je aktualizovat ručně, pokud čekáte na zobrazení některých dat.
* Další informace najdete v tématu [Poradce při potížích][qna].

## <a name="publish-your-app"></a>Publikování aplikace
Teď nasaďte aplikaci do služby IIS nebo do Azure a sledujte shromažďování dat.

#### <a name="no-data-after-you-publish-to-your-server"></a>Žádná data po publikování na serveru?
Otevřete tyto porty pro odchozí přenosy v bráně firewall serveru:

* `dc.services.visualstudio.com:443`
* `f5.services.visualstudio.com:443`

#### <a name="trouble-on-your-build-server"></a>Potíže na vašem serveru sestavení?
Naleznete v tématu [tato položka Poradce při potížích](app-insights-asp-net-troubleshoot-no-data.md#NuGetBuild).

## <a name="customized-usage"></a>Přizpůsobené využití
### <a name="disabling-auto-collection"></a>Zakázání automatického sběru
```javascript
import appInsights = require("applicationinsights");
appInsights.setup("<instrumentation_key>")
    .setAutoCollectRequests(false)
    .setAutoCollectPerformance(false)
    .setAutoCollectExceptions(false)
    // no telemetry will be sent until .start() is called
    .start();
```

### <a name="custom-monitoring"></a>Vlastní sledování
```javascript
import appInsights = require("applicationinsights");
var client = appInsights.getClient();

client.trackEvent("custom event", {customProperty: "custom property value"});
client.trackException(new Error("handled exceptions can be logged with this method"));
client.trackMetric("custom metric", 3);
client.trackTrace("trace message");
```

[Další informace o rozhraní API telemetrie](app-insights-api-custom-events-metrics.md).

### <a name="using-multiple-instrumentation-keys"></a>Použití více klíčů instrumentace
```javascript
import appInsights = require("applicationinsights");

// configure auto-collection with one instrumentation key
appInsights.setup("<instrumentation_key>").start();

// get a client for another instrumentation key
var otherClient = appInsights.getClient("<other_instrumentation_key>");
otherClient.trackEvent("custom event");
```

## <a name="examples"></a>Příklady
### <a name="tracking-dependency"></a>Sledování závislostí
```javascript
import appInsights = require("applicationinsights");
var client = appInsights.getClient();

var startTime = Date.now();
// execute dependency call
var endTime = Date.now();

var elapsedTime = endTime - startTime;
var success = true;
client.trackDependency("dependency name", "command name", elapsedTime, success);
```



### <a name="manual-request-tracking-of-all-get-requests"></a>Sledování ručního požadavku všech žádostí o "GET"
```javascript
var http = require("http");
var appInsights = require("applicationinsights");
appInsights.setup("<instrumentation_key>")
    .setAutoCollectRequests(false) // disable auto-collection of requests for this example
    .start();

// assign common properties to all telemetry sent from the default client
appInsights.client.commonProperties = {
    environment: process.env.SOME_ENV_VARIABLE
};

// track a system startup event
appInsights.client.trackEvent("server start");

// create server
var port = process.env.port || 1337
var server = http.createServer(function (req, res) {
    // track all "GET" requests
    if(req.method === "GET") {
        appInsights.client.trackRequest(req, res);
    }

    res.writeHead(200, { "Content-Type": "text/plain" });
    res.end("Hello World\n");
}).listen(port);

// track startup time of the server as a custom metric
var start = +new Date;
server.on("listening", () => {
    var end = +new Date;
    var duration = end - start;
    appInsights.client.trackMetric("StartupTime", duration);
});
```

## <a name="next-steps"></a>Další kroky
* [Monitorování telemetrických dat na portálu](app-insights-dashboards.md)
* [Psaní analytických dotazů do telemetrických dat](app-insights-analytics-tour.md)

<!--Link references-->

[knowUsers]: app-insights-overview-usage.md
[metrics]: app-insights-metrics-explorer.md
[perf]: app-insights-web-monitor-performance.md
[portal]: http://portal.azure.com/
[qna]: app-insights-troubleshoot-faq.md
[roles]: app-insights-resources-roles-access-control.md

