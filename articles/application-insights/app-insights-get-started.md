<properties
    pageTitle="Začínáme s Visual Studio Application Insights | Microsoft Azure"
    description="Analýza místního využití, dostupnosti a výkonu nebo webová aplikace Microsoft Azure s nástrojem Visual Studio Application Insights."
    services="application-insights"
    documentationCenter=""
    authors="alancameronwills"
    manager="douge"/>

<tags
    ms.service="application-insights"
    ms.workload="tbd"
    ms.tgt_pltfrm="ibiza"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="06/06/2016"
    ms.author="awills"/>

# Začínáme s Visual Studio Application Insights

*Application Insights je ve verzi Preview.*

Zjišťování problémů, řešení problémů a neustálé zlepšování aplikací. Rychle diagnostikuje problémy uvedené ve vaší živé aplikaci. Pochopte, co s ní vaši uživatelé dělají.

Konfigurace je velmi snadná a během minut se zobrazí výsledky.

Momentálně podporujeme aplikace iOS, Android a Windows , webové aplikace J2EE a ASP.NET a služby WCF. Webové aplikace můžete spustit v Azure nebo na místních serverech. Naše JavaScript SDK se spouští na libovolné webové stránce.

[Prohlédněte si úvod animace](https://www.youtube.com/watch?v=fX2NtGrh-Y0).

## Začínáme

Spustit s libovolnou kombinací v libovolném pořadí vstupních bodů, které se zobrazují v následujícím diagramu. Postupujte podle cesty, které vám vyhovuje.

Application Insights funguje přidáním sady SDK do aplikace, která odesílá telemetrii do [portálu Azure](https://portal.azure.com). Existují různé sady SDK pro různé kombinace platforem, jazyků a integrovaného vývojového prostředí, které jsou podporovány.

Budete potřebovat účet v [Microsoft Azure](http://azure.com). Již můžete mít přístup k účtu skupiny v rámci organizace, nebo můžete chtít získat účet průběžné platby. Application Insights obsahuje volné vrstvy, takže nebudete muset platit, dokud vaše aplikace nebude oblíbená. Zkontrolujte [stránku s cenami](https://azure.microsoft.com/pricing/details/application-insights/).

Co chcete použít | Co dělat | Co získáte
---|---|---
 <a href="app-insights-asp-net.md">![ASP.NET](./media/app-insights-get-started/appinsights-gs-i-01-perf.png)</a> | <a href="app-insights-asp-net.md">Přidejte sadu SDK Application Insights do svého webového projektu</a> <br/> ![získá](./media/app-insights-get-started/appinsights-00arrow.png) | <a href="app-insights-asp-net.md">![Výkon a sledování využití](./media/app-insights-get-started/appinsights-gs-r-01-perf.png)</a>
<a href="app-insights-monitor-performance-live-website-now.md">![Již živý web ASP.NET](./media/app-insights-get-started/appinsights-gs-i-04-red2.png)</a><br/><a href="app-insights-monitor-performance-live-website-now.md">![Monitorování výkonu a závislostí](./media/app-insights-get-started/appinsights-gs-i-03-red.png)</a>|<a href="app-insights-monitor-performance-live-website-now.md">Monitorování stavu instalace na serveru služby IIS</a> <br/> ![získá](./media/app-insights-get-started/appinsights-00arrow.png) | <a href="app-insights-monitor-performance-live-website-now.md">![Monitorování závislostí ASP.NET](./media/app-insights-get-started/appinsights-gs-r-03-red.png)</a>
<a href="insights-perf-analytics.md">![Webovou aplikaci nebo virtuální počítače Azure](./media/app-insights-get-started/appinsights-gs-i-10-azure.png)</a>|<a href="insights-perf-analytics.md">Povolte Insights ve webové aplikaci nebo virtuálních počítačích Azure</a> <br/> ![získá](./media/app-insights-get-started/appinsights-00arrow.png) | <a href="insights-perf-analytics.md">![Monitorování výkonu a závislostí](./media/app-insights-get-started/appinsights-gs-r-03-red.png)</a>
<a href="app-insights-java-get-started.md">![Java](./media/app-insights-get-started/appinsights-gs-i-11-java.png)</a>|<a href="app-insights-java-get-started.md">Přidejte sadu SDK do projektu jazyka Java</a><br/>![získá](./media/app-insights-get-started/appinsights-00arrow.png) | <a href="app-insights-java-get-started.md">![Výkon a sledování využití](./media/app-insights-get-started/appinsights-gs-r-10-java.png)</a>
<a href="app-insights-web-track-usage.md">![JavaScript](./media/app-insights-get-started/appinsights-gs-i-02-usage.png)</a>|<a href="app-insights-web-track-usage.md">Vložte skript Application Insights do webových stránek</a><br/>![získá](./media/app-insights-get-started/appinsights-00arrow.png) | <a href="app-insights-web-track-usage.md">![zobrazení stránky a výkon prohlížeče](./media/app-insights-get-started/appinsights-gs-r-02-usage.png)</a>
<a href="app-insights-monitor-web-app-availability.md">![Dostupnost](./media/app-insights-get-started/appinsights-gs-i-05-avail.png)</a>|<a href="app-insights-monitor-web-app-availability.md">Vytvářejte webové testy</a><br/>![získá](./media/app-insights-get-started/appinsights-00arrow.png) | <a href="app-insights-monitor-web-app-availability.md">![Dostupnost](./media/app-insights-get-started/appinsights-gs-r-05-avail.png)</a>
<a href="app-insights-platforms.md">![iOS, Android a Windows zařízení](./media/app-insights-get-started/appinsights-gs-i-07-device.png)</a>|<a href="http://hockeyapp.net">Použijte HockeyApp</a><br/>![získá](./media/app-insights-get-started/appinsights-00arrow.png) | <a href="http://hockeyapp.net">![Selhání a používání dat](./media/app-insights-get-started/appinsights-gs-r-06-device.png)</a>

## Podpora a zpětná vazba


* Otázky a problémy:
 * [Poradce při potížích][qna].
 * [Fórum MSDN](https://social.msdn.microsoft.com/Forums/vstudio/en-US/home?forum=ApplicationInsights)
 * [StackOverflow](http://stackoverflow.com/questions/tagged/ms-application-insights)
* Návrhy:
 * [UserVoice](https://visualstudio.uservoice.com/forums/357324)
* Ukázky kódů
 * [Ukázky kódů](app-insights-code-samples.md)



## <a name="video"></a>Video

[![A[animovaný úvod](./media/app-insights-get-started/video-front-1.png)](https://www.youtube.com/watch?v=fX2NtGrh-Y0)

> [AZURE.VIDEO 218]

> [AZURE.VIDEO usage-monitoring-application-insights]

> [AZURE.VIDEO performance-monitoring-application-insights]



<!--Link references-->

[qna]: app-insights-troubleshoot-faq.md



<!---HONumber=Aug16_HO4-->


