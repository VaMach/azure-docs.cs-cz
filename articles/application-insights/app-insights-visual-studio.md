---
title: "Práce s Application Insights v sadě Visual Studio"
description: "Analýza výkonu a Diagnostika během ladění a v produkčním prostředí."
services: application-insights
documentationcenter: .net
author: alancameronwills
manager: douge
ms.assetid: 2059802b-1131-477e-a7b4-5f70fb53f974
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: get-started-article
ms.date: 06/21/2016
ms.author: awills
translationtype: Human Translation
ms.sourcegitcommit: 41ce9b0e323c0938b6db98b99d8d687d1ed0f0ef
ms.openlocfilehash: 2334a85402049eb8654039e98cf2c3131a43de62


---
# <a name="working-with-application-insights-in-visual-studio"></a>Práce s Application Insights v sadě Visual Studio
V sadě Visual Studio (2015 a novější) můžete analyzovat výkon a diagnostikovat problémy v ladění i v produkčním prostředí pomocí telemetrie z [Azure Application Insights](app-insights-overview.md).

Pokud jste ještě [nenainstalovali Application Insights do své aplikace](app-insights-asp-net.md), proveďte to nyní.

## <a name="a-nameruna-debug-your-project"></a><a name="run"></a> Ladění projektu
Spusťte aplikaci pomocí F5 a vyzkoušejte ji: otevřete různé stránky k vygenerování nějaké telemetrie.

V sadě Visual Studio zobrazí počet událostí, které byly zaprotokolovány.

![V sadě Visual Studio se zobrazí tlačítko Application Insights během ladění.](./media/app-insights-visual-studio/appinsights-09eventcount.png)

Kliknutím na toto tlačítko otevřete diagnostické vyhledávání. 

## <a name="diagnostic-search"></a>Diagnostické vyhledávání
V okně Vyhledávání se zobrazí události, které byly zaprotokolovány. (Pokud jste přihlášeni do Azure při nastavení Application Insights, budete moci vyhledávat stejné události na portálu.)

![Klikněte pravým tlačítkem myši na projekt a vyberte Application Insights, Vyhledávání](./media/app-insights-visual-studio/34.png)

Textové vyhledávání funguje na všechna pole v událostech. Například vyhledejte část adresy URL stránky nebo hodnotu vlastnosti, například města klienta; nebo určitá slova v protokolu trasování.

Kliknutím na libovolnou událost zobrazíte podrobné vlastnosti.

Můžete také otevřít kartu související položky a pomocí diagnostikovat neúspěšné požadavky nebo výjimky.

![](./media/app-insights-visual-studio/41.png)

## <a name="diagnostics-hub"></a>Centrum diagnostiky
Centrum diagnostiky (v sadě Visual Studio 2015 nebo novější) zobrazuje telemetrii serveru Application Insights v průběhu jejího vzniku. Tento postup funguje, i když jste se rozhodli pouze k instalaci sady SDK, bez připojení k prostředku na portálu Azure.

![Otevřete okno Diagnostické nástroje a prozkoumejte události Application Insights.](./media/app-insights-visual-studio/31.png)

## <a name="exceptions"></a>Výjimky
Pokud jste [nastavili monitorování výjimek](app-insights-asp-net-exceptions.md), sestavy výjimek se zobrazí v okně Hledání. 

Klikněte na výjimku a získejte trasování zásobníku. Pokud je kód aplikace otevřen v sadě Visual Studio, můžete kliknutím z trasování zásobníku přejít na příslušný řádek kódu.

![Trasování zásobníku výjimky](./media/app-insights-visual-studio/17.png)

Navíc v řádku Code Lens nad jednotlivými metodami uvidíte počet výjimek zaznamenaných nástrojem Application Insights za posledních 24 hodin.

![Trasování zásobníku výjimky](./media/app-insights-visual-studio/21.png)

## <a name="local-monitoring"></a>Místní monitorování
(Z aktualizace Visual Studio 2015 Update 2) Pokud jste nenakonfigurovali SDK k odesílání telemetrie do služby Application Insights (takže se v souboru ApplicationInsights.config nenachází žádný klíč instrumentace), zobrazí okno diagnostiky telemetrii z nejnovější relace ladění. 

Toto je žádoucí, pokud jste již publikovali předchozí verzi aplikace. Nechcete, aby se telemetrie z vaší relace ladění promíchala s telemetrií na portálu služby Application Insights z publikované aplikace.

Je také užitečné, pokud máte některou [vlastní telemetrii](app-insights-api-custom-events-metrics.md), kterou chcete ladit před odesláním telemetrie na portál.

* *Napřed jsem službu Application Insights plně nakonfiguroval/a tak, aby posílala telemetrické údaje na portál. Ale teď chci telemetrické údaje zobrazovat jen v sadě Visual Studio.*
  
  * V okně hledání nastavení je možnost vyhledávání místní diagnostiky i v případě, že vaše aplikace odesílá telemetrii na portál.
  * Odesílání telemetrie na portál zastavíte okomentováním řádku `<instrumentationkey>...` ze souboru ApplicationInsights.config. Jakmile budete připraveni k opětovnému odeslání telemetrie na portál, komentář zrušte.

## <a name="trends"></a>Trendy
Trendy představují nástroj pro vizualizaci chování aplikace v čase. 

Vybírejte z **Trendů zkoumání telemetrie** z tlačítka panelu nástrojů Application Insights nebo okna hledání Application Insights. Zvolte jeden z pěti běžných dotazů, abyste mohli začít. Na základě typů telemetrie, časových rozsahů a dalších vlastností můžete analyzovat různé datové sady. 

Pokud chcete vyhledat anomálie v datech, vyberte jednu z možností anomálií v rozevíracím seznamu „Typ zobrazení“. Možnosti filtrování v dolní části okna usnadňují zdokonalování v konkrétních podmnožinách vaší telemetrie.

![Trendy](./media/app-insights-visual-studio/51.png)

[Další informace o trendech](app-insights-visual-studio-trends.md).

## <a name="whats-next"></a>Co dále?
|  |  |
| --- | --- |
| **[Přidání dalších dat](app-insights-asp-net-more.md)**<br/>Sledování využití, dostupnosti, závislostí, výjimek. Integrujte trasování z rozhraní protokolování. Zapisuje vlastní telemetrii. |![Visual Studio](./media/app-insights-visual-studio/64.png) |
| **[Práce s portálem Application Insights](app-insights-dashboards.md)**<br/>Řídicí panely, výkonné nástroje pro diagnostiku a analýzy, výstrahy, aktivní mapa závislostí vaší aplikace a export telemetrie. |![Visual Studio](./media/app-insights-visual-studio/62.png) |




<!--HONumber=Nov16_HO3-->


