<properties 
    pageTitle="Nastavte analýzy webových aplikací pro ASP.NET pomocí Application Insights" 
    description="Nakonfigurujte výkon, dostupnost a analýzy využití pro váš web ASP.NET hostovaný místně nebo v Azure." 
    services="application-insights" 
    documentationCenter=".net"
    authors="alancameronwills" 
    manager="douge"/>

<tags 
    ms.service="application-insights" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="get-started-article" 
    ms.date="05/25/2016" 
    ms.author="awills"/>


# Nastavte Application Insights pro ASP.NET

[Visual Studio Application Insights](app-insights-overview.md) monitoruje vaše živé aplikace a pomáhá vám [najít a diagnostikovat problémy s výkonem a výjimkami](app-insights-detect-triage-diagnose.md) a [zjistit, jak se vaše aplikace používá](app-insights-overview-usage.md).  Funguje pro aplikace, které jsou hostovány na místních serverech IIS nebo na cloudových virtuálních počítačích a také webových aplikacích Azure.


## Než začnete

Budete potřebovat:

* Visual Studio 2013 s aktualizací Update 3 nebo vyšší. Později je lepší.
* Předplatné [Microsoft Azure](http://azure.com). Pokud váš tým nebo společnost má předplatné Azure, vlastník vás do něj může přidat pomocí vašeho [účtu Microsoft](http://live.com). 

Nejsou k dispozici alternativní články pro sledování, pokud vás zajímají:

* [Instrumentace webové aplikace za běhu](app-insights-monitor-performance-live-website-now.md)
* [Jádro ASP.NET](app-insights-asp-net-core.md)
* [Služby Azure Cloud](app-insights-cloudservices.md)

## <a name="ide"></a> 1. Přidání Application Insights SDK


### Pokud se jedná o nový projekt...

Když vytvoříte nový projekt v sadě Visual Studio, ověřte, zda je zvolena možnost Application Insights. 


![Vytvoření projektu ASP.NET](./media/app-insights-asp-net/appinsights-01-vsnewp1.png)


### ... nebo pokud se jedná o existující projekt

Klikněte pravým tlačítkem myši na Průzkumníka řešení a vyberte položku **Přidat telemetrii Application Insights** nebo **Konfigurovat Application Insights**.

![Vyberte možnost Přidat Application Insights](./media/app-insights-asp-net/appinsights-03-addExisting.png)




## <a name="run"></a> 2. Spuštění aplikace

Spusťte aplikaci pomocí F5 a vyzkoušejte ji: otevřete různé stránky k vygenerování nějaké telemetrie.

V sadě Visual Studio zobrazí počet událostí, které byly zaprotokolovány. 

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

* Jednotlivé události se zobrazí ve **Vyhledávání** (1). Nejprve se data zobrazí zde (a v Live Stream). Kliknutím na libovolnou událost zobrazíte její vlastnosti. 
* Agregovaná metrika se zobrazí v grafech (2). Zobrazení dat na tomto místě může trvat minutu nebo dvě. Klikněte na libovolný graf a otevřete okno s dalšími podrobnostmi.

[Další informace o používání Application Insights na portálu Azure](app-insights-dashboards.md).

##<a name="land"></a> Co dělají Application Insights?

Application Insights odesílá telemetrii z vaší aplikace na portál Application Insights (který je hostován v Microsoft Azure):

![](./media/app-insights-asp-net/01-scheme.png)

Takže příkaz provedl tři věci:

1. Přidal balíček Application Insights Web SDK NuGet do projektu. Chcete-li ji zobrazit v sadě Visual Studio, klikněte pravým tlačítkem na projekt a zvolte spravovat balíčky NuGet.
2. Vytvořte prostředek Application Insights na [portálu Azure](https://portal.azure.com/). Toto je místo, kde uvidíte vaše data. Načítá *klíč instrumentace*, který identifikuje prostředek.
3. Vloží klíč instrumentace do `ApplicationInsights.config` tak, aby sada SDK mohla odesílat telemetrii do portálu.

Pokud chcete, můžete [tyto kroky provést ručně](app-insights-asp-net-manual.md).


## Co dále?

| | 
|---|---
|**[Práce s Application Insights v sadě Visual Studio](app-insights-visual-studio.md)**<br/>Ladění pomocí telemetrie, diagnostická hledání, podrobné procházení kódem.|![Visual Studio](./media/app-insights-asp-net/61.png)
|**[Práce s portálem Application Insights](app-insights-dashboards.md)**<br/>Řídicí panely, výkonné nástroje pro diagnostiku a analýzy, výstrahy, aktivní mapa závislostí vaší aplikace a export telemetrie. |![Visual Studio](./media/app-insights-asp-net/62.png)
|**[Přidat další data](app-insights-asp-net-more.md)**<br/>Sledování využití, dostupnosti, závislostí, výjimek. Integrujte trasování z rozhraní protokolování. Zapisuje vlastní telemetrii. | ![Visual Studio](./media/app-insights-asp-net/64.png)









<!--HONumber=Jun16_HO2-->


