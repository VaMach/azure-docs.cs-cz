---
title: "Sdílení vlastních zobrazení Azure Time Series Insights pomocí parametrizovaných adres URL | Dokumentace Microsoftu"
description: "Tento článek popisuje vývoj parametrizovaných adres URL ve službě Azure Time Series Insights za účelem snadného sdílení zobrazení zákazníka."
services: time-series-insights
ms.service: time-series-insights
author: MarkMcGeeAtAquent
ms.author: MarkMcGeeAtAquent
manager: jhubbard
editor: MicrosoftDocs/tsidocs
ms.reviewer: v-mamcge, jasonh, kfile, anshan
ms.devlang: rest-api
ms.topic: get-started-article
ms.workload: big-data
ms.date: 11/21/2017
ms.openlocfilehash: ac48969a9166080384dccf606f0401a82016a60a
ms.sourcegitcommit: 62eaa376437687de4ef2e325ac3d7e195d158f9f
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/22/2017
---
# <a name="share-a-custom-view-using-a-parameterized-url"></a>Sdílení vlastního zobrazení pomocí parametrizovaných adres URL

Pokud chcete sdílet vlastní zobrazení v průzkumníku Time Series Insights, můžete prostřednictvím kódu programu vytvořit parametrizovanou adresu URL tohoto vlastního zobrazení.

Průzkumník Time Series Insights podporuje parametry dotazů adresy URL, které určují zobrazení v prostředí přímo v adrese URL.  Pouze pomocí adresy URL můžete například určit cílové prostředí, predikát vyhledávání a požadované časové období. Když uživatel klikne na přizpůsobenou adresu URL, v rozhraní se zobrazí přímý odkaz na příslušný prostředek na portálu Time Series Insights.  Platí zde zásady přístupu k datům. 

## <a name="environment-id"></a>ID prostředí

Parametr `environmentId=<guid>` určuje ID cílového prostředí.  Jedná se o součást plně kvalifikovaného názvu domény pro přístup k datům a najdete jej v pravém horním rohu přehledu prostředí na webu Azure Portal.  Je to vše před `env.timeseries.azure.com`. Příkladem parametru ID prostředí je `?environmentId=10000000-0000-0000-0000-100000000108`.

## <a name="time"></a>Čas

V parametrizované adrese URL můžete zadat absolutní nebo relativní časové hodnoty.

### <a name="absolute-time-values"></a>Absolutní časové hodnoty

Pro absolutní časové hodnoty použijte parametry `from=<integer>` a `to=<integer>`. 

`from=<integer>` je hodnota v milisekundách JavaScriptu určující čas spuštění pro rozsah hledání.

`to=<integer>` je hodnota v milisekundách JavaScriptu určující čas ukončení pro rozsah hledání. 

Pokud chcete pro nějaké datum určit čas v milisekundách JavaScriptu, přečtěte si článek [Epoch & Unix Timestamp Converter](https://www.freeformatter.com/epoch-timestamp-to-date-converter.html).

### <a name="relative-time-values"></a>Relativní časové hodnoty

Pro relativní časovou hodnotu použijte parametr `relativeMillis=<value>`, kde *value* je časové období nejnovějších dat v back-endu v milisekundách JavaScriptu.

Například `&relativeMillis=3600000` zobrazí data za posledních 60 minut.

Přijatelné hodnoty odpovídají nabídce **quick time** (rychlý čas) průzkumníka Time Series Insights a patří mezi ně následující hodnoty:

- 1800000 (posledních 30 minut)
- 3600000 (posledních 60 minut)
- 10800000 (poslední 3 hodiny)
- 21600000 (posledních 6 hodin)
- 43200000 (posledních 12 hodin)
- 86400000 (posledních 24 hodin)
- 604800000 (posledních 7 dnů)
- 2592000000 (posledních 30 dnů)

### <a name="optional-parameters"></a>Volitelné parametry

Parametr `timeSeriesDefinitions=<collection of term objects>` určuje podmínky zobrazení Time Series Insights, kde:

- `name=<string>`
  - Název *podmínky*.
- `splitBy=<string>`
  - Název sloupce, podle kterého se bude *rozdělovat*.
- `measureName=<string>`
  - Název sloupce obsahujícího *míru*.
- `predicate=<string>`
  - Klauzule *kde* pro filtrování na straně serveru.

 
### <a name="examples"></a>Příklady

Například pokud chcete jako parametr adresy URL přidat definice časových řad, můžete použít následující:

```https
&timeSeriesDefinitions=[{"name":"F1PressureId","splitBy":"Id","measureName":"Pressure","predicate":"'Factory1'"},{"name":"F2TempStation","splitBy":"Station","measureName":"Temperature","predicate":"'Factory2'"},
{"name":"F3VibrationPL","splitBy":"ProductionLine","measureName":"Vibration","predicate":"'Factory3'"}]
```

Pomocí těchto ukázkových definic časových řad pro 

- ID prostředí,
- data za posledních 60 minut,
- podmínky (F1PressureID, F2TempStation a F3VibrationPL), které tvoří volitelné parametry,
 
můžete pro zobrazení vytvořit následující parametrizovanou adresu URL:

```https
https://insights.timeseries.azure.com/samples?environmentId=10000000-0000-0000-0000-100000000108&relativeMillis=3600000&timeSeriesDefinitions=[{"name":"F1PressureId","splitBy":"Id","measureName":"Pressure","predicate":"'Factory1'"},{"name":"F2TempStation","splitBy":"Station","measureName":"Temperature","predicate":"'Factory2'"},{"name":"F3VibrationPL","splitBy":"ProductionLine","measureName":"Vibration","predicate":"'Factory3'"}]
```

Pokud jste k sestavení zobrazení popsaného předchozí adresou URL použili průzkumníka Time Series Insights, bude vypadat takto:

![Podmínky v průzkumníku Time Series Insights](media/parameterized-url/url1.png)

Úplné zobrazení (včetně grafu) bude vypadat takto:

![Zobrazení grafu](media/parameterized-url/url2.png)

## <a name="next-steps"></a>Další kroky
[Dotazování dat pomocí jazyka C#](time-series-insights-query-data-csharp.md)
