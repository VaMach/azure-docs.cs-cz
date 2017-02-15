---
title: "Nastavení analýzy webových aplikací pro ASP.NET pomocí Application Insights | Dokumentace Microsoftu"
description: "Nakonfigurujte výkon, dostupnost a analýzy využití pro váš web ASP.NET hostovaný místně nebo v Azure."
services: application-insights
documentationcenter: .net
author: NumberByColors
manager: douge
ms.assetid: d0eee3c0-b328-448f-8123-f478052751db
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: get-started-article
ms.date: 10/13/2016
ms.author: awills
translationtype: Human Translation
ms.sourcegitcommit: dea21a59b189d1d3d474cbc5e67f64df485a1981
ms.openlocfilehash: a97e20b208d92e03bd4458605aaa48ef7c389e32


---
# <a name="set-up-application-insights-for-aspnet"></a>Nastavte Application Insights pro ASP.NET
[Azure Application Insights](app-insights-overview.md) monitoruje vaše živé aplikace a pomáhá vám [najít a diagnostikovat problémy s výkonem a výjimkami](app-insights-detect-triage-diagnose.md) a [zjistit, jak se vaše aplikace používá](app-insights-overview-usage.md).  Funguje pro aplikace, které jsou hostovány na místních serverech IIS nebo na cloudových virtuálních počítačích a také webových aplikacích Azure.

## <a name="before-you-start"></a>Než začnete
Budete potřebovat:

* Visual Studio 2013 s aktualizací Update 3 nebo vyšší. Později je lepší.
* Předplatné [Microsoft Azure](http://azure.com). Pokud váš tým nebo společnost má předplatné Azure, vlastník vás do něj může přidat pomocí vašeho [účtu Microsoft](http://live.com). 

Nejsou k dispozici alternativní články pro sledování, pokud vás zajímají:

* [Instrumentace webové aplikace za běhu](app-insights-monitor-performance-live-website-now.md)
* [Azure Cloud Services](app-insights-cloudservices.md)

## <a name="a-nameidea-1-add-application-insights-sdk"></a><a name="ide"></a> 1. Přidání Application Insights SDK
### <a name="if-its-a-new-project"></a>Pokud se jedná o nový projekt...
Když vytvoříte nový projekt v sadě Visual Studio, ověřte, zda je zvolena možnost Application Insights. 

![Vytvoření projektu ASP.NET](./media/app-insights-asp-net/appinsights-01-vsnewp1.png)

### <a name="-or-if-its-an-existing-project"></a>... nebo pokud se jedná o existující projekt
Klikněte pravým tlačítkem myši na Průzkumníka řešení a vyberte položku **Přidat telemetrii Application Insights** nebo **Konfigurovat Application Insights**.

![Vyberte možnost Přidat Application Insights](./media/app-insights-asp-net/appinsights-03-addExisting.png)

* Projekt jádra ASP.NET? - [Pro opravu několika řádků kódu postupujte podle těchto pokynů](https://github.com/Microsoft/ApplicationInsights-aspnetcore/wiki/Getting-Started#add-application-insights-instrumentation-code-to-startupcs). 

## <a name="a-nameruna-2-run-your-app"></a><a name="run"></a> 2. Spuštění aplikace
Spusťte aplikaci pomocí F5 a vyzkoušejte ji: otevřete různé stránky k vygenerování nějaké telemetrie.

V sadě Visual Studio se zobrazí počet událostí, které byly zaprotokolovány. 

![V sadě Visual Studio se zobrazí tlačítko Application Insights během ladění.](./media/app-insights-asp-net/54.png)

## <a name="3-see-your-telemetry"></a>3. Viz vaše telemetrie...
### <a name="-in-visual-studio"></a>...v sadě Visual Studio
Otevřete okno Application Insights v sadě Visual Studio: klikněte na tlačítko Application Insights nebo klikněte pravým tlačítkem na projekt v Průzkumníku řešení:

![V sadě Visual Studio se zobrazí tlačítko Application Insights během ladění.](./media/app-insights-asp-net/55.png)

Toto zobrazení obsahuje telemetrii vygenerovanou na serverové straně vaší aplikace. Experimentujte s filtry a klikněte na události, které chcete zobrazit podrobněji.

[Další informace týkající se nástrojů Application Insights v sadě Visual Studio](app-insights-visual-studio.md).

<a name="monitor"></a> 

### <a name="-in-the-portal"></a>...na portálu
Pokud jste zvolili *instalovat pouze sadu SDK,* můžete také zobrazit telemetrii na webovém portálu Application Insights. 

Portál obsahuje více grafů, analytických nástrojů a řídicích panelů než Visual Studio. 

Otevřete prostředek Application Insights na [portálu Azure](https://portal.azure.com/).

![Klikněte pravým tlačítkem na projekt a otevřete portál Azure](./media/app-insights-asp-net/appinsights-04-openPortal.png)

Portál otevře zobrazení telemetrie z vaší aplikace: ![](./media/app-insights-asp-net/66.png)

* První telemetrie se zobrazí ve [Živé vysílání metrik](app-insights-metrics-explorer.md#live-metrics-stream).
* Jednotlivé události se zobrazí ve **Vyhledávání** (1). Zobrazení dat může zabrat několik minut. Kliknutím na libovolnou událost zobrazíte její vlastnosti. 
* Agregovaná metrika se zobrazí v grafech (2). Zobrazení dat na tomto místě může trvat minutu nebo dvě. Klikněte na libovolný graf a otevřete okno s dalšími podrobnostmi.

[Další informace o používání Application Insights na portálu Azure](app-insights-dashboards.md).

## <a name="4-publish-your-app"></a>4. Publikování aplikace
Publikování aplikace na serveru služby IIS nebo do Azure. Sledujte [Živé vysílání metrik](app-insights-metrics-explorer.md#live-metrics-stream) a ověřte, zda vše běží hladce.

Uvidíte telemetrii vznikající na portálu služby Application Insights, kde můžete monitorovat metriky, hledat telemetrie a nastavovat [řídicí panely](app-insights-dashboards.md). Můžete také použít účinný [Jazyk pro analýzy dotazů](app-insights-analytics.md) k analýze využívání a výkonu nebo hledat konkrétní události. 

Můžete také dále analyzovat telemetrie ve [Visual Studiu](app-insights-visual-studio.md) pomocí nástrojů jako vyhledávání diagnostiky a [Trendy](app-insights-visual-studio-trends.md).

> [!NOTE]
> Pokud vaše aplikace odesílá dostatek telemetrie k dosažení [limitů omezení](app-insights-pricing.md#limits-summary), zapne se automatické [vzorkování](app-insights-sampling.md). Vzorkování snižuje množství telemetrie odesílané z vaší aplikace při zachování korelovaných dat k diagnostickým účelům.
> 
> 

## <a name="a-namelanda-what-did-add-application-insights-do"></a><a name="land"></a> Co dělá příkaz Přidat Application Insights?
Application Insights odesílá telemetrii z vaší aplikace na portál Application Insights (který je hostován v Microsoft Azure):

![](./media/app-insights-asp-net/01-scheme.png)

Takže příkaz provedl tři věci:

1. Přidal balíček Application Insights Web SDK NuGet do projektu. Chcete-li ji zobrazit v sadě Visual Studio, klikněte pravým tlačítkem na projekt a zvolte spravovat balíčky NuGet.
2. Vytvořte prostředek Application Insights na [portálu Azure](https://portal.azure.com/). Toto je místo, kde uvidíte vaše data. Načítá *klíč instrumentace*, který identifikuje prostředek.
3. Vloží klíč instrumentace do `ApplicationInsights.config` tak, aby sada SDK mohla odesílat telemetrii do portálu.

Pokud chcete, můžete tyto kroky provést ručně pro [ASP.NET 4](app-insights-windows-services.md) nebo [ASP.NET Core](https://github.com/Microsoft/ApplicationInsights-aspnetcore/wiki/Getting-Started).

### <a name="to-upgrade-to-future-sdk-versions"></a>Postup upgradu na budoucí verze sady SDK
K upgradu [novou verzi sady SDK](https://github.com/Microsoft/ApplicationInsights-dotnet-server/releases) otevřete znovu Správce balíčků NuGet a filtrujte nainstalované balíčky. Vyberte Microsoft.ApplicationInsights.Web a zvolte Upgradovat.

Pokud jste provedli jakékoli úpravy souboru ApplicationInsights.config, uložte jeho kopii před upgradem a následně slučte změny do nové verze.

## <a name="add-more-telemetry"></a>Přidání další telemetrie
### <a name="web-pages-and-single-page-apps"></a>Webové stránky a jednostránkové aplikace
1. [Přidejte javascriptový fragment kódu](app-insights-javascript.md) do svých webových aplikací, aby bylo možné do oken prohlížeče a využití doplnit údaje o zobrazení stránek, časech načtení, výjimkách prohlížeče, výkonu volání jazyka AJAX, počtu uživatelů a relací.
2. [Naprogramujte vlastní události,](app-insights-api-custom-events-metrics.md) které počítají a měří čas nebo akce uživatelů.

### <a name="dependencies-exceptions-and-performance-counters"></a>Závislosti, výjimky a čítače výkonu
[Nainstalujte monitor stavu](app-insights-monitor-performance-live-website-now.md) na každý serverový počítač, abyste o své aplikaci získali další telemetrické údaje. Co získáte:

* [Čítače výkonu](app-insights-performance-counters.md) - 
   procesoru, paměti, disku a další čítače, které se týkají vaší aplikace. 
* [Výjimky](app-insights-asp-net-exceptions.md) – podrobnější telemetrické údaje o některých výjimkách.
* [Závislosti](app-insights-asp-net-dependencies.md) – volání REST API nebo služeb SQL. Zjistíte, jestli problémy s výkonem vaší aplikace způsobuje pomalá odezva externích komponent (pokud aplikace běží na platformě .NET 4.6, nepotřebujete k získání těchto telemetrických údajů monitor stavu).

### <a name="diagnostic-code"></a>Diagnostický kód
Vyskytl se v aplikaci problém? Pokud chcete, můžete do aplikace vložit kód, který vám pomůže ji diagnostikovat. Máte několik možností:

* [Záznam trasování protokolu:](app-insights-asp-net-trace-logs.md) Pokud k trasování událostí v protokolu používáte Log4N, NLog nebo System.Diagnostics.Trace, můžete výstup poslat do služby Application Insights, kde ho můžete porovnat s požadavky, hledat v něm nebo ho analyzovat. 
* [Vlastní události a metriky:](app-insights-api-custom-events-metrics.md) Na serveru nebo v kódu webové stránky použijte funkce TrackEvent() a TrackMetric().
* [Telemetrie značek s dalšími vlastnostmi](app-insights-api-filtering-sampling.md#add-properties)

K vyhledání a porovnání určitých událostí použijte funkci [Search](app-insights-diagnostic-search.md) a k provádění výkonnějších dotazů použijte funkci [Analytics](app-insights-analytics.md).

## <a name="alerts"></a>Výstrahy
Buďte první, kdo ví o problémech aplikace. (Nečekejte, až vám o nich řeknou uživatelé.) 

* [Vytvářejte webové testy](app-insights-monitor-web-app-availability.md), abyste ověřili viditelnost svého webu.
* [Preventivní diagnostika](app-insights-proactive-diagnostics.md) se spouští automaticky (pokud má aplikace určitý minimální provoz). Tyto výstrahy nemusíte nastavovat. Upozorní vás, pokud má aplikace nezvykle velký podíl neúspěšných požadavků.
* [Nastavte si upozornění na metriku](app-insights-alerts.md) a nechte se upozornit, pokud metrika překročí mezní hodnotu. Upozornění můžete nastavit u vlastních metrik, které v aplikaci naprogramujete.

Ve výchozím nastavení se oznámení výstrah automaticky posílají vlastníkovi předplatného Azure. 

![Ukázka e-mailového upozornění](./media/app-insights-asp-net/alert-email.png)

## <a name="version-and-release-tracking"></a>Sledování verzí a vydání
### <a name="track-application-version"></a>Sledování verze aplikace
Ujistěte se, že je `buildinfo.config` generován procesem MSBuild. Do souboru .csproj přidejte:  

```XML

    <PropertyGroup>
      <GenerateBuildInfoConfigFile>true</GenerateBuildInfoConfigFile>    <IncludeServerNameInBuildInfo>true</IncludeServerNameInBuildInfo>
    </PropertyGroup> 
```

Pokud obsahuje informace o sestavení, webový modul Application Insights automaticky přidá položku **Verze aplikace** jako vlastnost pro každý předmět telemetrie. Díky tomu můžete při provádění [diagnostických hledání](app-insights-diagnostic-search.md) nebo při [zkoumání metrik](app-insights-metrics-explorer.md) filtrovat podle verze. 

Všimněte si však, že číslo verze sestavení je generováno pouze pomocí MS Build, ne sestavením vývojáře v sadě Visual Studio.

### <a name="release-annotations"></a>Poznámky k verzi
Pokud používáte Visual Studio Team Services, můžete [získat značku poznámek](app-insights-annotations.md), kterou můžete přidat do svých grafů pokaždé, když vydáte novou verzi.

![Ukázka poznámek k verzi](./media/app-insights-asp-net/release-annotation.png)

## <a name="next-steps"></a>Další kroky
|  |
| --- | --- |
| **[Práce s Application Insights v sadě Visual Studio](app-insights-visual-studio.md)**<br/>Ladění pomocí telemetrie, diagnostická hledání, podrobné procházení kódem. |
| **[Práce s portálem Application Insights](app-insights-dashboards.md)**<br/>Řídicí panely, výkonné nástroje pro diagnostiku a analýzy, výstrahy, aktivní mapa závislostí vaší aplikace a export telemetrie. |
| **[Přidání dalších dat](app-insights-asp-net-more.md)**<br/>Sledování využití, dostupnosti, závislostí, výjimek. Integrujte trasování z rozhraní protokolování. Zapisuje vlastní telemetrii. |




<!--HONumber=Nov16_HO3-->


