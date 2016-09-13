<properties 
    pageTitle="Nastavení analýzy webových aplikací pro ASP.NET pomocí Application Insights | Microsoft Azure" 
    description="Nakonfigurujte výkon, dostupnost a analýzy využití pro váš web ASP.NET hostovaný místně nebo v Azure." 
    services="application-insights" 
    documentationCenter=".net"
    authors="NumberByColors" 
    manager="douge"/>

<tags 
    ms.service="application-insights" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="get-started-article" 
    ms.date="08/09/2016" 
    ms.author="daviste"/>


# Nastavte Application Insights pro ASP.NET

[Visual Studio Application Insights](app-insights-overview.md) monitoruje vaše živé aplikace a pomáhá vám [najít a diagnostikovat problémy s výkonem a výjimkami](app-insights-detect-triage-diagnose.md) a [zjistit, jak se vaše aplikace používá](app-insights-overview-usage.md).  Funguje pro aplikace, které jsou hostovány na místních serverech IIS nebo na cloudových virtuálních počítačích a také webových aplikacích Azure.


## Než začnete

Budete potřebovat:

* Visual Studio 2013 s aktualizací Update 3 nebo vyšší. Později je lepší.
* Předplatné [Microsoft Azure](http://azure.com). Pokud váš tým nebo společnost má předplatné Azure, vlastník vás do něj může přidat pomocí vašeho [účtu Microsoft](http://live.com). 

Nejsou k dispozici alternativní články pro sledování, pokud vás zajímají:

* [Instrumentace webové aplikace za běhu](app-insights-monitor-performance-live-website-now.md)
* [Služby Azure Cloud](app-insights-cloudservices.md)

## <a name="ide"></a> 1. Přidání Application Insights SDK


### Pokud se jedná o nový projekt...

Když vytvoříte nový projekt v sadě Visual Studio, ověřte, zda je zvolena možnost Application Insights. 


![Vytvoření projektu ASP.NET](./media/app-insights-asp-net/appinsights-01-vsnewp1.png)


### ... nebo pokud se jedná o existující projekt

Klikněte pravým tlačítkem myši na Průzkumníka řešení a vyberte položku **Přidat telemetrii Application Insights** nebo **Konfigurovat Application Insights**.

![Vyberte možnost Přidat Application Insights](./media/app-insights-asp-net/appinsights-03-addExisting.png)

* Projekt jádra ASP.NET? - [Pro opravu několika řádků kódu postupujte podle těchto pokynů](https://github.com/Microsoft/ApplicationInsights-aspnetcore/wiki/Getting-Started#add-application-insights-instrumentation-code-to-startupcs). 



## <a name="run"></a> 2. Spuštění aplikace

Spusťte aplikaci pomocí F5 a vyzkoušejte ji: otevřete různé stránky k vygenerování nějaké telemetrie.

V sadě Visual Studio se zobrazí počet událostí, které byly zaprotokolovány. 

![V sadě Visual Studio se zobrazí tlačítko Application Insights během ladění.](./media/app-insights-asp-net/54.png)

## 3. Viz vaše telemetrie...

### ...v sadě Visual Studio

Otevřete okno Application Insights v sadě Visual Studio: klikněte na tlačítko Application Insights nebo klikněte pravým tlačítkem na projekt v Průzkumníku řešení:

![V sadě Visual Studio se zobrazí tlačítko Application Insights během ladění.](./media/app-insights-asp-net/55.png)

Toto zobrazení obsahuje telemetrii vygenerovanou na serverové straně vaší aplikace. Experimentujte s filtry a klikněte na události, které chcete zobrazit podrobněji.

[Další informace týkající se nástrojů Application Insights v sadě Visual Studio](app-insights-visual-studio.md).

<a name="monitor"></a> 
### ...na portálu

Pokud jste zvolili *instalovat pouze sadu SDK,* můžete také zobrazit telemetrii na webovém portálu Application Insights. 

Portál obsahuje více grafů, analytických nástrojů a řídicích panelů než Visual Studio. 


Otevřete prostředek Application Insights na [portálu Azure](https://portal.azure.com/).

![Klikněte pravým tlačítkem na projekt a otevřete portál Azure](./media/app-insights-asp-net/appinsights-04-openPortal.png)

Portál se otevře v zobrazení telemetrie z vaší aplikace:
![](./media/app-insights-asp-net/66.png)

* První telemetrie se zobrazí ve [Živé vysílání metrik](app-insights-metrics-explorer.md#live-metrics-stream).
* Jednotlivé události se zobrazí ve **Vyhledávání** (1). Zobrazení dat může zabrat několik minut. Kliknutím na libovolnou událost zobrazíte její vlastnosti. 
* Agregovaná metrika se zobrazí v grafech (2). Zobrazení dat na tomto místě může trvat minutu nebo dvě. Klikněte na libovolný graf a otevřete okno s dalšími podrobnostmi.

[Další informace o používání Application Insights na portálu Azure](app-insights-dashboards.md).

## 4. Publikování aplikace

Publikování aplikace na serveru služby IIS nebo do Azure. Sledujte [Živé vysílání metrik](app-insights-metrics-explorer.md#live-metrics-stream) a ověřte, zda vše běží hladce.

Uvidíte telemetrii vznikající na portálu služby Application Insights, kde můžete monitorovat metriky, hledat telemetrie a nastavovat [řídicí panely](app-insights-dashboards.md). Můžete také použít účinný [Jazyk pro analýzy dotazů](app-insights-analytics.md) k analýze využívání a výkonu nebo hledat konkrétní události. 

Můžete také dále analyzovat telemetrie ve [Visual Studiu](app-insights-visual-studio.md) pomocí nástrojů jako vyhledávání diagnostiky a [Trendy](app-insights-visual-studio-trends.md).

> [AZURE.NOTE] Pokud vaše aplikace odesílá dostatek telemetrie k dosažení [limitů omezení](app-insights-pricing.md#limits-summary), zapne se automatické [vzorkování](app-insights-sampling.md). Vzorkování snižuje množství telemetrie odesílané z vaší aplikace při zachování korelovaných dat k diagnostickým účelům.


##<a name="land"></a> Co dělají Application Insights?

Application Insights odesílá telemetrii z vaší aplikace na portál Application Insights (který je hostován v Microsoft Azure):

![](./media/app-insights-asp-net/01-scheme.png)

Takže příkaz provedl tři věci:

1. Přidal balíček Application Insights Web SDK NuGet do projektu. Chcete-li ji zobrazit v sadě Visual Studio, klikněte pravým tlačítkem na projekt a zvolte spravovat balíčky NuGet.
2. Vytvořte prostředek Application Insights na [portálu Azure](https://portal.azure.com/). Toto je místo, kde uvidíte vaše data. Načítá *klíč instrumentace*, který identifikuje prostředek.
3. Vloží klíč instrumentace do `ApplicationInsights.config` tak, aby sada SDK mohla odesílat telemetrii do portálu.

Pokud chcete, můžete tyto kroky provést ručně pro [ASP.NET 4](app-insights-windows-services.md) nebo [ASP.NET Core](https://github.com/Microsoft/ApplicationInsights-aspnetcore/wiki/Getting-Started).

### Postup upgradu na budoucí verze sady SDK

K upgradu [novou verzi sady SDK](https://github.com/Microsoft/ApplicationInsights-dotnet-server/releases) otevřete znovu Správce balíčků NuGet a filtrujte nainstalované balíčky. Vyberte Microsoft.ApplicationInsights.Web a zvolte Upgradovat.

Pokud jste provedli jakékoli úpravy souboru ApplicationInsights.config, uložte jeho kopii před upgradem a následně slučte změny do nové verze.



## Další kroky

| | 
|---|---
|**[Práce s Application Insights v sadě Visual Studio](app-insights-visual-studio.md)**<br/>Ladění pomocí telemetrie, diagnostická hledání, podrobné procházení kódem.|![Visual Studio](./media/app-insights-asp-net/61.png)
|**[Práce s portálem Application Insights](app-insights-dashboards.md)**<br/>Řídicí panely, výkonné nástroje pro diagnostiku a analýzy, výstrahy, aktivní mapa závislostí vaší aplikace a export telemetrie. |![Visual Studio](./media/app-insights-asp-net/62.png)
|**[Přidat další data](app-insights-asp-net-more.md)**<br/>Sledování využití, dostupnosti, závislostí, výjimek. Integrujte trasování z rozhraní protokolování. Zapisuje vlastní telemetrii. | ![Visual Studio](./media/app-insights-asp-net/64.png)









<!--HONumber=sep16_HO1-->


