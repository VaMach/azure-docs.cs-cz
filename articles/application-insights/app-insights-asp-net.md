---
title: "Nastavení analýzy webových aplikací pro ASP.NET pomocí Azure Application Insights | Dokumentace Microsoftu"
description: "Nakonfigurujte výkon, dostupnost a analýzy využití pro váš web ASP.NET hostovaný místně nebo v Azure."
services: application-insights
documentationcenter: .net
author: NumberByColors
manager: carmonm
ms.assetid: d0eee3c0-b328-448f-8123-f478052751db
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: get-started-article
ms.date: 02/28/2017
ms.author: awills
translationtype: Human Translation
ms.sourcegitcommit: 05b642949205d7698fbcf791a5d2f06528ff239e
ms.openlocfilehash: e827bddfa0bcc1c1e46d06c6856033e77859c7fb
ms.lasthandoff: 03/01/2017


---
# <a name="set-up-application-insights-for-your-aspnet-website"></a>Nastavení Application Insights pro web ASP.NET
[Azure Application Insights](app-insights-overview.md) monitoruje živé aplikace a pomáhá [detekovat a diagnostikovat problémy s výkonem a výjimky](app-insights-detect-triage-diagnose.md). Zároveň pomáhá [zjistit, jak se aplikace používá](app-insights-overview-usage.md). Funguje jak pro funkci Web Apps služby Azure App Service, tak pro aplikace hostované na místních serverech služby IIS nebo na cloudových virtuálních počítačích.

## <a name="before-you-start"></a>Než začnete
Budete potřebovat:

* Visual Studio 2013 s aktualizací Update 3 nebo vyšší. Později je lepší.
* Předplatné [Microsoft Azure](http://azure.com). Pokud má váš tým nebo organizace předplatné Azure, vlastník vás do něj může přidat pomocí vašeho [účtu Microsoft](http://live.com).

K dispozici jsou i alternativní témata, na která se v případě zájmu můžete podívat:

* [Instrumentace webové aplikace za běhu](app-insights-monitor-performance-live-website-now.md)
* [Azure Cloud Services](app-insights-cloudservices.md)

## <a name="a-nameidea-step-1-add-the-application-insights-sdk"></a><a name="ide"></a>Krok 1: Přidání sady Application Insights SDK

Klikněte pravým tlačítkem myši na projekt webové aplikace v Průzkumníku řešení a vyberte postupně **Přidat**, **Telemetrie Application Insights...** nebo **Konfigurovat Application Insights**.

![Snímek obrazovky Průzkumníka řešení se zvýrazněnou možností Přidat telemetrii Application Insights](./media/app-insights-asp-net/appinsights-03-addExisting.png)

(V sadě Visual Studio 2015 je také možné přidat Application Insights v dialogovém okně Nový projekt.)

Přejděte na konfigurační stránku Application Insights:

![Snímek obrazovky stránky registrace vaší aplikace v Application Insights](./media/app-insights-asp-net/visual-studio-register-dialog.png)

1. Vyberte účet a předplatné, které používáte pro přístup k Azure.
2. V Azure vyberte prostředek, ve kterém chcete zobrazit data z vaší aplikace. Pro každou aplikaci obvykle vytvoříte samostatný prostředek. Pokud chcete nastavit skupinu prostředků nebo umístění, kde jsou vaše data uložená, klikněte na **Konfigurovat nastavení**. Skupiny prostředků slouží k řízení přístupu k datům. Pokud například máte několik aplikací, které tvoří součást stejného systému, můžete jejich data Application Insights ukládat do stejné skupiny prostředků.
3. Služba Application Insights je do určitého objemu telemetrie bezplatná. Je možné nastavit limit objemu dat a vyhnout se tak poplatkům. Po vytvoření prostředku můžete výběr na portálu změnit tak, že otevřete **Funkce a ceny**, **Správa dat**, **Denní limit objemu**.
4. Pokračujte kliknutím na **Registrovat** a nakonfigurujte Application Insights pro vaši webovou aplikaci. Telemetrie se bude posílat na web [Azure Portal](https://portal.azure.com), jak během ladění aplikace, tak po jejím publikování.
5. Další možností je přidat k aplikaci sadu Application Insights SDK. V takovém případě vám bude telemetrie zobrazovat v sadě Visual Studio během ladění. Později se můžete na tuto stránku konfigurace vrátit, nebo počkat až po nasazení aplikace a [přepnout na telemetrie za běhu](app-insights-monitor-performance-live-website-now.md).


## <a name="a-nameruna-step-2-run-your-app"></a><a name="run"></a>Krok 2: Spuštění aplikace
Spusťte aplikaci pomocí F5. Otevřete různé stránky k vygenerování nějaké telemetrie.

V sadě Visual Studio se zobrazí počet událostí, které byly zaprotokolovány.

![Snímek obrazovky sady Visual Studio. Během ladění se zobrazí tlačítko Application Insights.](./media/app-insights-asp-net/54.png)

## <a name="step-3-see-your-telemetry-in-visual-studio-or-application-insights"></a>Krok 3: Zobrazení telemetrie v sadě Visual Studio nebo v Application Insights
Telemetrii můžete zobrazit v sadě Visual Studio nebo na webovém portálu Application Insights.

**V sadě Visual Studio** otevřete okno Application Insights. Klikněte na tlačítko **Application Insights** nebo klikněte pravým tlačítkem na projekt v Průzkumníku řešení, vyberte **Application Insights** a potom klikněte na **Aktivní telemetrie služby Search**.

V okně Visual Studio Application Insights Search se v zobrazení **Data z relace ladění** zobrazí telemetrie vygenerovaná na straně serveru vaší aplikace. Experimentujte s filtry a klikněte na události, které chcete zobrazit podrobněji.

![Snímek obrazovky zobrazení Data z relace ladění v okně Application Insights.](./media/app-insights-asp-net/55.png)

> [!NOTE]
> Pokud se žádná data nezobrazí, ujistěte se, že je časový rozsah správný, a klikněte na ikonu Search.

[Další informace týkající se nástrojů Application Insights v sadě Visual Studio](app-insights-visual-studio.md).

<a name="monitor"></a>
### <a name="the-application-insights-web-portal"></a>Webový portál Application Insights
Telemetrii můžete zobrazit také na **webovém portálu Application Insights**, pokud jste se nerozhodli nainstalovat pouze sadu SDK. Portál obsahuje více grafů, analytických nástrojů a řídicích panelů než sada Visual Studio.

Otevřete prostředek Application Insights. Buď se přihlaste k webu [Azure Portal](https://portal.azure.com/) a vyhledejte jej, nebo klikněte pravým tlačítkem myši na projekt v sadě Visual Studio a ten vás navede.

![Snímek obrazovky sady Visual Studio ukazující, jak otevřít portál Application Insights](./media/app-insights-asp-net/appinsights-04-openPortal.png)

> [!NOTE]
> Pokud se zobrazí chyba přístupu, možná máte pro Microsoft více než jednu sadu přihlašovacích údajů a je možné, že jste přihlášení pomocí nesprávné sady. Odhlaste se z portálu a znovu se přihlaste.

Portál otevře zobrazení telemetrie z vaší aplikace.
![Snímek obrazovky stránky s přehledem Application Insights](./media/app-insights-asp-net/66.png)

Klikněte na jakoukoli dlaždici nebo graf, aby se zobrazily podrobnější údaje.

### <a name="more-details-in-the-application-insights-web-portal"></a>Další podrobnosti na webovém portálu Application Insights
Zde je několik příkladů toho, jak portál poskytuje další podrobnosti.

* [**Live Metrics Stream**](app-insights-live-stream.md) zobrazí telemetrii skoro okamžitě.

    ![Snímek obrazovky portálu. V okně Přehled klikněte na Live Stream.](./media/app-insights-asp-net/livestream.png)

    Otevřete službu Live Metrics Stream v době, kdy je spuštěná aplikace, aby se mohly propojit.

    Služba Live Metrics Stream zobrazuje telemetrii jenom minutu po odeslání. K dalším historickým zkoumáním použijte služby Search, Průzkumník metrik a Analytics. Zobrazení dat v těchto umístěních může zabrat několik minut.

* Služba [**Search**](app-insights-diagnostic-search.md) zobrazuje jednotlivé události, jako jsou požadavky, výjimky a zobrazení stránek. Můžete filtrovat podle typu události, shody pojmů a vlastností hodnoty. Kliknutím na libovolnou událost zobrazíte její vlastnosti a související události.

    ![Snímek obrazovky portálu. V okně Přehled klikněte na Search.](./media/app-insights-asp-net/search.png)

 * V režimu pro vývoj se může zobrazovat spousta událostí závislosti (AJAX). Jsou to synchronizace mezi prohlížečem a emulátorem serveru. Pokud je chcete skrýt, klikněte na filtr **Závislost**.
* V grafech se zobrazuje [**agregovaná metrika** ](app-insights-metrics-explorer.md), jako jsou požadavky a míra selhání. Klikněte na libovolný graf a otevřete okno s dalšími podrobnostmi. Pokud chcete nastavit filtry a velikost, klikněte na značku **Upravit** u kteréhokoli grafu.

    ![Snímek obrazovky okna agregovaných metrik na portálu](./media/app-insights-asp-net/metrics.png)

[Další informace o používání Application Insights na portálu Azure](app-insights-dashboards.md).

## <a name="step-4-publish-your-app"></a>Krok 4: Publikování aplikace
Publikování aplikace na serveru služby IIS nebo do Azure. Sledujte [Živé vysílání metrik](app-insights-metrics-explorer.md#live-metrics-stream) a ověřte, zda vše běží hladce.

Telemetrie vzniká na portálu Application Insights, kde můžete monitorovat metriky, vyhledávat telemetrii a nastavovat [řídicí panely](app-insights-dashboards.md). Můžete také použít účinný [Jazyk pro analýzy dotazů](app-insights-analytics.md) k analýze využívání a výkonu nebo k hledání konkrétních událostí.

Můžete také dále analyzovat telemetrii v sadě [Visual Studio](app-insights-visual-studio.md) pomocí nástrojů jako vyhledávání diagnostiky a [Trendy](app-insights-visual-studio-trends.md).

> [!NOTE]
> Pokud vaše aplikace odesílá dostatek telemetrie k dosažení [limitů omezení](app-insights-pricing.md#limits-summary), zapne se automatické [vzorkování](app-insights-sampling.md). Vzorkování snižuje množství telemetrie odesílané z vaší aplikace při zachování korelovaných dat k diagnostickým účelům.
>
>

## <a name="a-namelanda-what-does-the-add-application-insights-command-do"></a><a name="land"></a>Co dělá příkaz Add v Application Insights?
Application Insights odesílá telemetrii z vaší aplikace na portál Application Insights (který je hostován v Azure).

![Diagram přesunu telemetrie](./media/app-insights-asp-net/01-scheme.png)

Příkaz tedy dělá tři věci:

1. Přidá do projektu balíček Application Insights Web SDK NuGet. Chcete-li jej zobrazit v sadě Visual Studio, klikněte pravým tlačítkem na projekt a zvolte **Spravovat balíčky NuGet**.
2. Vytvoří prostředek Application Insights na webu [Azure Portal](https://portal.azure.com/). Tady vidíte vaše data. Načítá *klíč instrumentace*, který identifikuje prostředek.
3. Vloží klíč instrumentace do `ApplicationInsights.config` tak, aby sada SDK mohla odesílat telemetrii do portálu.

Pokud chcete, můžete tyto kroky provést ručně pro [ASP.NET 4](app-insights-windows-services.md) nebo [ASP.NET Core](https://github.com/Microsoft/ApplicationInsights-aspnetcore/wiki/Getting-Started).

### <a name="upgrade-to-future-sdk-versions"></a>Upgrade na budoucí verze sady SDK
Pokud chcete upgradovat na [novou verzi sady SDK](https://github.com/Microsoft/ApplicationInsights-dotnet-server/releases), otevřete znovu **Správce balíčků NuGet** a filtrujte nainstalované balíčky. Vyberte **Microsoft.ApplicationInsights.Web** a zvolte **Upgradovat**.

Pokud jste provedli jakékoli úpravy souboru ApplicationInsights.config, před upgradem si uložte jeho kopii. Potom slučte změny do nové verze.

## <a name="add-more-telemetry"></a>Přidání další telemetrie
Dále jsou uvedeny další typy telemetrie, které můžete přidat.
### <a name="dependencies-exceptions-and-performance-counters"></a>Závislosti, výjimky a čítače výkonu

[Nainstalujte monitor stavu](http://go.microsoft.com/fwlink/?LinkId=506648) na každý počítač se serverem služby IIS, abyste o webové aplikaci získali další telemetrické údaje. Pokud je už nainstalovaný, nemusíte dělat nic. (Monitorování stavu jste už možná použili k monitorování aplikace za běhu.)

Pokud kromě sestavovací sady SDK použijete i monitorování stavu, získáte úplnější sadu telemetrických údajů, která zahrnuje:

* [Čítače výkonu:](app-insights-performance-counters.md) čítače výkonu procesoru, paměti, disku a další čítače, které se týkají vaší aplikace.
* [Výjimky:](app-insights-asp-net-exceptions.md) podrobnější telemetrie o některých výjimkách.
* [Závislosti:](app-insights-asp-net-dependencies.md) včetně návratových hodnot.

### <a name="webpages-and-single-page-apps"></a>Webové stránky a jednostránkové aplikace
1. [Přidejte javascriptový fragment kódu](app-insights-javascript.md) do svých webových aplikací, aby bylo možné zobrazit údaje o zobrazení stránek, časech načtení, výjimkách prohlížeče, výkonu volání jazyka AJAX a počtu uživatelů a relací. Tyto údaje se zobrazují v oknech Prohlížeč a Využití.
2. [Naprogramujte vlastní události,](app-insights-api-custom-events-metrics.md) které počítají a měří čas nebo akce uživatelů.


### <a name="diagnostic-code"></a>Diagnostický kód
Vyskytl se v aplikaci problém? Pokud chcete, můžete do aplikace vložit kód, který vám pomůže ji diagnostikovat. Máte několik možností:

* [Záznam trasování protokolu:](app-insights-asp-net-trace-logs.md) Pokud k trasování událostí v protokolu používáte Log4N, NLog nebo System.Diagnostics.Trace, můžete výstup posílat do Application Insights. Tento výstup můžete porovnat s požadavky, hledat v něm a analyzovat ho.
* [Vlastní události a metriky:](app-insights-api-custom-events-metrics.md) V kódu serveru nebo v kódu webové stránky použijte funkce TrackEvent() a TrackMetric().
* [Označení telemetrie s použitím dalších vlastností](app-insights-api-filtering-sampling.md#add-properties).

K vyhledání a porovnání určitých událostí použijte funkci [Search](app-insights-diagnostic-search.md) a k provádění výkonnějších dotazů použijte funkci [Analytics](app-insights-analytics.md).

## <a name="alerts"></a>Výstrahy
Buďte první, kdo ví o problémech aplikace.

* [Testy dostupnosti:](app-insights-monitor-web-app-availability.md) Vytvářejte testy, abyste ověřili viditelnost svého webu na internetu.
* [Inteligentní diagnostika:](app-insights-proactive-diagnostics.md) Tyto testy se spouštějí automaticky, takže je nemusíte nijak nastavovat. Upozorní vás, pokud má aplikace nezvykle velký podíl neúspěšných požadavků.
* [Upozornění na metriku:](app-insights-alerts.md) Nastavte si je a nechte se upozornit, pokud metrika překročí mezní hodnotu. Upozornění můžete nastavit u vlastních metrik, které v aplikaci naprogramujete.

Ve výchozím nastavení se oznámení výstrah automaticky posílají vlastníkovi předplatného Azure.

![Snímek obrazovky ukázky e-mailového upozornění](./media/app-insights-asp-net/alert-email.png)

## <a name="version-and-release-tracking"></a>Sledování verzí a vydání
Pokud chcete sledovat verzi aplikace, ujistěte se, že proces Microsoft Build Engine vygeneroval soubor `buildinfo.config`. Do souboru .csproj přidejte:  

```XML

    <PropertyGroup>
      <GenerateBuildInfoConfigFile>true</GenerateBuildInfoConfigFile>    <IncludeServerNameInBuildInfo>true</IncludeServerNameInBuildInfo>
    </PropertyGroup>
```

Pokud obsahuje informace o sestavení, webový modul Application Insights automaticky přidá položku **Verze aplikace** jako vlastnost pro každý předmět telemetrie. Díky tomu můžete při provádění [diagnostických hledání](app-insights-diagnostic-search.md) nebo při [zkoumání metrik](app-insights-metrics-explorer.md) filtrovat podle verze.

Všimněte si však, že číslo verze sestavení je generováno pouze pomocí procesu Microsoft Build Engine, ne sestavením vývojáře v sadě Visual Studio.

### <a name="release-annotations"></a>Poznámky k verzi
Pokud používáte Visual Studio Team Services, můžete [získat značku poznámek](app-insights-annotations.md), kterou můžete přidat do svých grafů pokaždé, když vydáte novou verzi. Následující obrázek ukazuje, jak se tato značka zobrazuje.

![Snímek obrazovky grafu s ukázkovou poznámkou k verzi](./media/app-insights-asp-net/release-annotation.png)

## <a name="next-steps"></a>Další kroky
**[Práce s Application Insights v sadě Visual Studio](app-insights-visual-studio.md)**<br/>Zahrnuje informace o ladění pomocí telemetrie, diagnostických hledáních a podrobném procházení kódem.

**[Práce s portálem Application Insights](app-insights-dashboards.md)**<br/> Zahrnuje informace o řídicích panelech, výkonných nástrojích pro diagnostiku a analýzy, výstrahách, aktivních mapách závislostí vaší aplikace a exportu telemetrie.

