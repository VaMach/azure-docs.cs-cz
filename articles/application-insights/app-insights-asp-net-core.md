---
title: Azure Application Insights pro ASP.NET Core | Microsoft Docs
description: "Sledování webových aplikací pro dostupnosti, výkonu a využití."
services: application-insights
documentationcenter: .net
author: mrbullwinkle
manager: carmonm
ms.assetid: 3b722e47-38bd-4667-9ba4-65b7006c074c
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 03/14/2017
ms.author: mbullwin
ms.openlocfilehash: 74f99dd6f31ecff7c838d8f710a7fe4279ce0ea9
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/01/2017
---
# <a name="application-insights-for-aspnet-core"></a>Application Insights pro ASP.NET Core
[Application Insights](app-insights-overview.md) umožňuje monitorování vaší webové aplikace pro dostupnosti, výkonu a využití. Na základě zpětné vazby ohledně výkonu a efektivity vaší aplikace při běžném používání můžete informovaně rozhodovat o směrování návrhu v každé fázi vývoje.

![Příklad](./media/app-insights-asp-net-core/sample.png)

Budete potřebovat předplatné s [Microsoft Azure](http://azure.com). Přihlaste se pomocí účtu Microsoft, který můžete mít zřízen pro Windows, XBox Live nebo jiné cloudové služby Microsoftu. Tým může mít předplatné pro společnosti do Azure: Požádejte vlastníka, aby je přidejte do ní pomocí svého účtu Microsoft.

## <a name="getting-started"></a>Začínáme

* V Průzkumníku řešení Visual Studio, klikněte pravým tlačítkem na projekt a vyberte **konfigurovat Application Insights**, nebo **Přidat > Application Insights**. [Další informace](app-insights-asp-net.md).
* Pokud nevidíte tyto příkazy nabídky, postupujte podle kroků [ruční získávání Příručka Začínáme](https://github.com/Microsoft/ApplicationInsights-aspnetcore/wiki/Getting-Started). Můžete to udělat, pokud projekt byl vytvořen s verzí sady Visual Studio před 2017.

## <a name="using-application-insights"></a>Použití Application Insights
Přihlaste se k [portálu Microsoft Azure](https://portal.azure.com), vyberte **všechny prostředky** nebo **Application Insights**a pak vyberte prostředek, který jste vytvořili pro sledování aplikace.

V samostatném okně prohlížeče používají vaši aplikaci nějakou dobu. Zobrazí se data zobrazí v grafech Application Insights. (Možná bude muset klikněte na tlačítko Aktualizovat.) Budou existovat jenom malé množství dat při vyvíjíte, ale tyto grafy skutečně pocházet zachování připojení při publikování aplikace a mít mnoho uživatelů. 

Na stránce Přehled se zobrazí grafy výkonu klíče: doba odezvy serveru, čas načítání stránky a počet neúspěšných požadavků. Klikněte na libovolný graf zobrazíte další grafy a data.

Zobrazení portálu spadají do tří kategorií:

* [Průzkumníku metrik](app-insights-metrics-explorer.md) zobrazuje grafů a tabulek metriky a počty, například dobu odezvy, selhání sazby nebo metriky můžete vytvořit sami pomocí [rozhraní API](app-insights-api-custom-events-metrics.md). Filtrovat a rozdělit data pomocí hodnoty vlastností pro dokázali lépe porozumět vaší aplikace a jeho uživatelů.
* [Hledání Explorer](app-insights-diagnostic-search.md) uvádí jednotlivé události, jako jsou konkrétní požadavky, výjimky, trasování protokolu nebo události, které jste vytvořili sami pomocí [rozhraní API](app-insights-api-custom-events-metrics.md). Filtrovat a hledání v událostech a pohyb mezi souvisejících událostí k prozkoumání problémů.
* [Analýza](app-insights-analytics.md) umožňuje spouštět dotazy podobné jazyku SQL přes telemetrie a je výkonný nástroj pro analýzu a diagnostiky.

## <a name="alerts"></a>Výstrahy
* Automaticky získáte [proaktivní diagnostické výstrahy](app-insights-proactive-diagnostics.md) , vás informovat o neobvyklé změny v sazby selhání a další metriky.
* Nastavit [testy dostupnosti](app-insights-monitor-web-app-availability.md) otestovat váš web průběžně z umístění po celém světě, a také všechny testovací nezdaří získat e-mailů.
* Nastavit [metriky výstrahy](app-insights-monitor-web-app-availability.md) vědět, pokud přejděte metriky, například dobu odezvy nebo výjimka sazby mimo přijatelný omezení.

## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/100/player] 

## <a name="open-source"></a>Open source
[Přečtěte si a přispívat ke kódu](https://github.com/Microsoft/ApplicationInsights-aspnetcore#recent-updates)


## <a name="next-steps"></a>Další kroky
* [Přidání telemetrických údajů do webových stránek](app-insights-javascript.md) k monitorování stránky využití a výkonu.
* [Monitorování závislostí](app-insights-asp-net-dependencies.md) chcete zobrazit, pokud REST, SQL nebo jiné externí prostředky zpomalují můžete.
* [Použít rozhraní API](app-insights-api-custom-events-metrics.md) k odeslání vlastní události a metriky pro další podrobné zobrazení výkonu a využití vaší aplikace.
* [Testy dostupnosti](app-insights-monitor-web-app-availability.md) zkontrolujte aplikaci neustále z po celém světě. 

