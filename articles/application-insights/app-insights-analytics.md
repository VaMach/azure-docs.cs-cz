---
title: "Analýza - výkonné vyhledávání a nástroj pro dotazování služby Azure Application Insights | Microsoft Docs"
description: "Přehled analýzy, nástroj výkonné diagnostické vyhledávání služby Application Insights. "
services: application-insights
documentationcenter: 
author: mrbullwinkle
manager: carmonm
ms.assetid: 0a2f6011-5bcf-47b7-8450-40f284274b24
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 10/04/2017
ms.author: mbullwin
ms.openlocfilehash: 80a9e248ca50c11ef61a5c50c4986c4f8f4ead9d
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/01/2017
---
# <a name="analytics-in-application-insights"></a>Analýza ve službě Application Insights
Analytics je výkonný nástroj vyhledávání a dotaz z [Application Insights](app-insights-overview.md). Analytics je nástroj pro webové, takže není zapotřebí žádné nastavení. Pokud jste již nakonfigurovali Application Insights pro některé z aplikací, pak můžete analyzovat data této aplikaci otevřením Analytics z vaší aplikace [okno Přehled](app-insights-dashboards.md).

![Otevřete portal.azure.com otevřete prostředek Application Insights a klikněte na Analytics.](./media/app-insights-analytics/001.png)

Můžete také [Analytics playground](https://go.microsoft.com/fwlink/?linkid=859557) tedy volné ukázkové prostředí s velkým množstvím ukázková data.
<br>
<br>
> [!VIDEO https://channel9.msdn.com/events/Connect/2016/123/player] 

## <a name="query-data-in-analytics"></a>Dotaz na data v Analytics
Typické dotazu začíná název tabulky, za nímž následuje řadu *operátory* oddělených `|`.
Například umožňuje zjistit, kolik požadavků vaší aplikace, obdržel z různých zemí během posledních 3 hodiny:
```AIQL
requests
| where timestamp > ago(3h)
| summarize count() by client_CountryOrRegion
| render piechart
```

Začneme s názvem tabulky *požadavky* a podle potřeby přidejte vytvoření kanálu elementy.  Nejprve jsme definovali filtr času ke kontrole pouze záznamy z poslední 3 hodiny.
Jsme pak určený počet záznamů za zemi (aby dat je nalezen ve sloupci *client_CountryOrRegion*). Nakonec jsme vykreslovat výsledky výsečového grafu.
<br>

![Výsledky dotazu](./media/app-insights-analytics/030.png)

Jazyk obsahuje mnoho atraktivní funkcí:

* [Filtr](https://docs.loganalytics.io/queryLanguage/query_language_whereoperator.html) telemetrie nezpracovaná aplikace podle všechna pole, včetně vlastní vlastnosti a metriky.
* [Připojení k](https://docs.loganalytics.io/queryLanguage/query_language_joinoperator.html) více tabulek – korelace požadavky s zobrazení stránky, volání závislostí, výjimek a protokolu trasování.
* Efektivní statistické [agregace](https://docs.loganalytics.io/learn/tutorials/aggregations.html).
* Okamžité a výkonné vizualizace.
* [Rozhraní REST API](https://dev.applicationinsights.io/) používané ke spouštění dotazů prostřednictvím kódu programu, například z prostředí PowerShell.

[Úplné referenční informace k jazyku](https://go.microsoft.com/fwlink/?linkid=856079) údaje každý příkaz podporován a pravidelně aktualizuje.

## <a name="next-steps"></a>Další kroky
* Začínáme s [portálu analýza](https://go.microsoft.com/fwlink/?linkid=856587)
* Začínáme [zápis dotazů](https://go.microsoft.com/fwlink/?linkid=856078)
* Zkontrolujte [SQL-uživatelů tahák](https://aka.ms/sql-analytics) pro překlady nejběžnější idioms.
* Vyzkoušejte Analytics na našem [playground](https://analytics.applicationinsights.io/demo) Pokud aplikace není odesílání dat do služby Application Insights ještě.
* Sledování [úvodní video](https://applicationanalytics-media.azureedge.net/home_page_video.mp4).