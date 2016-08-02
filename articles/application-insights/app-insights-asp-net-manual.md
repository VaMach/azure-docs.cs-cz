<properties
    pageTitle="Přidejte Application Insights SDK pro sledování aplikace ASP.NET | Microsoft Azure"
    description="Analýza místního využití, dostupnosti a výkonu nebo webová aplikace Microsoft Azure s nástrojem Application Insights."
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
    ms.date="05/19/2016"
    ms.author="awills"/>


# Přidejte Application Insights SDK pro sledování aplikace ASP.NET

*Application Insights je ve verzi Preview.*

[AZURE.INCLUDE [app-insights-selector-get-started](../../includes/app-insights-selector-get-started.md)]


Visual Studio Application Insights monitoruje vaše živé aplikace a pomáhá vám [najít a diagnostikovat problémy s výkonem a výjimkami][ zjistit] a [objevit, jak vaši aplikaci používají][známí uživatelé]. Lze ji používat s různými typy aplikací. Funguje pro aplikace, které jsou hostovány na místních serverech IIS nebo na virtuálních počítačích Azure a také webových aplikacích Azure.



![Příklady tabulek sledování výkonu](./media/app-insights-asp-net-manual/10-perf.png)

*Viz také:*

* [ASP.NET 5](app-insights-asp-net-five.md)
* [Aplikace zařízení a platformy][Java serveru]

#### Než začnete

Pro mnoho typů aplikací [Visual Studio může přidávat Application Insights do vaší aplikace](#ide) téměř bez vašeho povšimnutí. Ale vzhledem k tomu, že čtete tyto informace pro lepší pochopení, co se děje, provedeme vás těmito kroky ručně.


Budete potřebovat:

* Předplatné [Microsoft Azure](http://azure.com). Pokud váš tým nebo společnost má předplatné Azure, vlastník vás do něj může přidat pomocí vašeho [účtu Microsoft](http://live.com).
* Visual Studio 2013 nebo novější.

## <a name="add"></a>Vytvořte prostředek Application Insights

Přihlaste se k portálu [Azure][] a vytvořte nový prostředek Application Insights. Vyberte jako typ aplikace ASP.NET.

![Klikněte na tlačítko Nový, Application Insights](./media/app-insights-asp-net-manual/01-new-asp.png)

[Role][prostředků] v Azure je instance služby. Tento prostředek je místo, kde se analyzuje a prezentuje telemetrie z vaší aplikace.

Volba typu aplikace nastaví výchozí obsah oken prostředků a vlastnosti viditelné v metrikách [Průzkumníku metrik][].

#### Zkopírovat klíč instrumentace

Klíč identifikuje prostředek a nainstalujte ho brzy do sady SDK pro přímá data do prostředku.

![Klikněte na tlačítko Vlastnosti, vyberte klíč a stiskněte klávesy ctrl + C](./media/app-insights-asp-net-manual/02-props-asp.png)

Kroky, které jste právě provedli pro vytvoření nového prostředku, jsou vhodný způsob, jak začít monitorovat všechny aplikace. Teď do prostředku můžete odesílat data.

## <a name="sdk"></a> Nainstalujte sadu SDK do aplikace

Instalace a konfigurace Application Insights SDK se liší podle platformy, se kterou právě pracujete. Pro aplikace ASP.NET je to snadné.

1. V sadě Visual Studio upravte balíčky NuGet projektu webové aplikace.

    ![Klikněte pravým tlačítkem myši a vyberte možnost Spravovat balíčky Nuget](./media/app-insights-asp-net-manual/03-nuget.png)

2. Nainstalujte službu Application Insights SDK pro webové aplikace.

    ![Vyhledejte „Application Insights“](./media/app-insights-asp-net-manual/04-ai-nuget.png)

3. Upravit soubor ApplicationInsights.config (který byl nainstalován nástrojem NuGet). Vložte tuto položku těsně před uzavírací značku:

    `<InstrumentationKey>` *zkopírovaný klíč instrumentace* `</InstrumentationKey>`

    (Případně nastavte [klíč napsáním nějakého kódu][apikey] do vaší aplikace.)

#### Postup upgradu na budoucí verze sady SDK

Čas od času vydáváme nové verze sady SDK.

K upgradu [novou verzi sady SDK](app-insights-release-notes-dotnet.md) otevřete znovu Správce balíčků NuGet a filtrujte nainstalované balíčky. Vyberte **Microsoft.ApplicationInsights.Web** a zvolte **Upgradovat**.

Pokud jste provedli jakékoli úpravy souboru ApplicationInsights.config, uložte jeho kopii před upgradem a následně slučte změny do nové verze.


## <a name="run"></a> Spusťte projekt

Použijte **F5** ke spuštění aplikace a vyzkoušejte: otevření různých stránek k vygenerování nějaké telemetrie.

V sadě Visual Studio zobrazí počet událostí, které byly odeslány.

![](./media/app-insights-asp-net-manual/appinsights-09eventcount.png)

## <a name="monitor"></a> Zobrazení telemetrie

Přihlaste se do portálu [Azure][] a procházejte do zdroje Application Insights.


Vyhledejte data v tabulkách Přehled. Na první pohled uvidíte pouze jeden nebo dva body. Příklad:

![Proklikejte se k dalším datům](./media/app-insights-asp-net-manual/12-first-perf.png)

Proklikejte se prostřednictvím jakékoli grafu pro zobrazení podrobnějších metrik. [Další informace o metrikách.][výkon]

#### Žádná data?

* Použijte aplikaci a otevřete různé stránky tak, aby došlo k vygenerování nějaké telemetrie.
* Otevřete dlaždici [Vyhledávání][diagnostiky] a zobrazte jednotlivé události. Někdy trvá událostem trochu déle, než se dostanou skrz kanály metriky.
* Počkejte několik sekund a klikněte na tlačítko **Aktualizovat**. Grafy se pravidelně samy aktualizují, ale můžete je aktualizovat ručně, pokud čekáte na zobrazení některých dat.
* Viz [Poradce při potížích][qna].

## Publikování aplikace

Teď nasaďte aplikaci do služby IIS nebo do Azure a sledujte shromažďování dat.

![K publikování aplikace použijte sadu Visual Studio](./media/app-insights-asp-net-manual/15-publish.png)

Při spuštění v režimu ladění prochází telemetrie skrz kanál, takže byste měli vidět zobrazení dat během několika sekund. Při nasazení aplikace v rámci konfigurace verze se data hromadí pomaleji.

#### Žádná data po publikování na serveru?

Otevřete tyto porty pro odchozí přenosy v bráně firewall serveru:

+ `dc.services.visualstudio.com:443`
+ `f5.services.visualstudio.com:443`


#### Potíže na vašem serveru sestavení?

Naleznete v tématu [tato položka Poradce při potížích](app-insights-asp-net-troubleshoot-no-data.md#NuGetBuild).

> [AZURE.NOTE] Pokud vaše aplikace generuje mnoho telemetrie (a používáte verzi sady SDK technologie ASP.NET 2.0.0-beta3 nebo novější), modul adaptivního vzorkování automaticky sníží objem, který je odesílán na portál odesláním pouze reprezentativní části události. Události, které se vztahují ke stejnému požadavku, však budou vybrány nebo nevybrány jako skupina, takže mohou procházet mezi souvisejícími událostmi. 
> [Další informace o vzorkování](app-insights-sampling.md).


## Přidat závislost sledování (a čítače výkonu služby IIS)

Sada SDK potřebuje trochu pomoci pro získání přístupu k některým datům. Zejména budete potřebovat tento další krok k automatickému měření volání z aplikace do databáze, REST API a dalších externích součástí. Tyto metriky závislostí mohou být neocenitelnou pomocí při diagnostice problémů s výkonem.

Pokud ladíte vlastní server IIS, tento krok také umožní zobrazení systémů čítače výkonu v [průzkumníku metrik](app-insights-metrics-explorer.md).

#### Pokud vaše aplikace běží na serveru služby IIS

Přihlaste se na server pomocí oprávnění správce a nainstalujte [Monitorování stavu Application Insights](http://go.microsoft.com/fwlink/?LinkId=506648).

Možná budete muset [otevřít další odchozí porty v bráně firewall](app-insights-monitor-performance-live-website-now.md#troubleshooting).

Tento krok také umožní [hlášení čítačů výkonu](app-insights-web-monitor-performance.md#system-performance-counters), například CPU, paměti, síťového vytížení.

#### Pokud je vaše aplikace Azure Web App

V ovládacích panelech webové aplikace Azure přidejte rozšíření Application Insights.

![Ve vaší webové aplikaci, nastavení, rozšíření, přidat, Application Insights](./media/app-insights-asp-net-manual/05-extend.png)


#### Pokud se jedná o projekt cloudových služeb Azure

[Přidat skripty pro webové a pracovní role](app-insights-cloudservices.md).



## Přidat monitorování na straně klienta

Nainstalovali jste sadu SDK, která odesílá telemetrická data ze serveru (back-end) vaší aplikace. Teď můžete přidat monitorování na straně klienta. To poskytuje údaje o uživatelích, relacích, zobrazeních stránky a všech výjimkách nebo pádech, ke kterým došlo v prohlížeči. Také budete moci napsat vlastní kód pro sledování, jak uživatelé pracují s vaší aplikací, až po podrobnou úroveň používání myši a stisknutí kláves.


Přidá fragment kódu jazyka JavaScript na každou stránku. Získáte kód z vašich prostředků Application Insights:

![Ve vaší webové aplikaci otevřete rychlý Start a klikněte na příkaz „Načíst kód ke sledování mých webových stránek“](./media/app-insights-asp-net-manual/02-monitor-web-page.png)

Všimněte si, že kód obsahuje klíč instrumentace, který identifikuje prostředek vaší aplikace.

[Další informace o sledování webové stránky.](app-insights-web-track-usage.md)


## Sledování verze aplikace

Ujistěte se, že je `buildinfo.config` generován procesem MSBuild. Do souboru .csproj přidejte:  

```XML

    <PropertyGroup>
      <GenerateBuildInfoConfigFile>true</GenerateBuildInfoConfigFile>    <IncludeServerNameInBuildInfo>true</IncludeServerNameInBuildInfo>
    </PropertyGroup> 
```

Pokud obsahuje informace o sestavení, webový modul Application Insights automaticky přidá položku **Verze aplikace** jako vlastnost pro každý předmět telemetrie. To vám umožní filtrovat podle verze při provádění [diagnostických hledání][diagnostiky] nebo při [zkoumání metriky][metriky]. 

Všimněte si však, že číslo verze sestavení je generováno pouze pomocí MS Build, ne sestavením vývojáře v sadě Visual Studio.

## Dokončení instalace

Chcete-li získat úplné 360 stupňové zobrazení vaší aplikace, můžete udělat pár dalších věcí, které lze provést:

* [Nastavit testy webu][dostupnosti] a ujistěte se, že vaše aplikace zůstává aktivní a reagující.
* [Ukládání protokolů trasování][netlogs] z vašeho oblíbeného rozhraní protokolování
* [Sledování vlastních událostí a metrik][rozhraní api] v klientovi nebo serveru nebo obojím pro zjištění dalších informací o použití aplikace.

## <a name="ide"></a> Automatizovaný způsob

Na začátku tohoto článku jsme uvedli, že vám ukážeme manuální způsob, jak vytvořit prostředek služby Application Insights a pak nainstalovat sadu SDK. Věříme, že je vhodné pochopit dvě části tohoto postupu. Ale pro aplikace ASP.NET (a mnoho dalších) existuje ještě rychlejší automatizovaný způsob.

Budete potřebovat [Visual Studio](http://go.microsoft.com/fwlink/?linkid=397827&clcid=0x409) (2013 update 3 nebo novější) a účet v [Microsoft Azure](http://azure.com).

#### Vytvoření nového projektu...

Když vytvoříte nový projekt v sadě Visual Studio, je zvolena možnost **Přidat službu Application Insights**.


![Vytvoření projektu ASP.NET](./media/app-insights-asp-net-manual/appinsights-01-vsnewp1.png)

Visual Studio vytvoří prostředek ve službě Application Insights, přidá SDK do projektu a umístí klíč do souboru `.config`.

Pokud projekt obsahuje webové stránky, přidá také klienta [JavaScript SDK][] do hlavní webové stránky.

#### ... nebo pokud se jedná o existující projekt

Klikněte pravým tlačítkem myši na Průzkumníka řešení a zvolte **Přidat službu Application Insights**.

![Vyberte možnost Přidat Application Insights](./media/app-insights-asp-net-manual/appinsights-03-addExisting.png)

Visual Studio vytvoří prostředek ve službě Application Insights, přidá SDK do projektu a umístí klíč do souboru `.config`.

V takovém případě nepřidá klienta [JavaScript SDK][] na webové stránky – doporučujeme, abyste to provedli v dalším kroku.

#### Možnosti nastavení

Pokud je to poprvé, budete požádáni o přihlášení nebo registraci do Microsoft Azure Preview. 

Pokud je tato aplikace součástí větší aplikace, můžete chtít použít položku **Konfigurovat nastavení** pro její umístění do stejné skupiny prostředků, jako ostatní součásti.

*Žádná možnost Application Insights? Zkontrolujte, zda používáte Visual Studio 2013 Update 3 nebo novější a že jsou nástroje Application Insights povoleny v rozšířeních a aktualizacích.*

#### Otevřete službu Application Insights z projektu

![Klikněte pravým tlačítkem na projekt a otevřete portál Azure](./media/app-insights-asp-net-manual/appinsights-04-openPortal.png)




## <a name="video"></a>Video

> [AZURE.VIDEO getting-started-with-application-insights]


<!--Link references-->

[rozhraní api]: app-insights-api-custom-events-metrics.md
[apikey]: app-insights-api-custom-events-metrics.md#ikey
[dostupnosti]: app-insights-monitor-web-app-availability.md
[azure]: ../insights-perf-analytics.md
[klient]: app-insights-javascript.md
[ zjistit]: app-insights-detect-triage-diagnose.md
[diagnostiky]: app-insights-diagnostic-search.md
[knowUsers]: app-insights-overview-usage.md
[metriky]: app-insights-metrics-explorer.md
[netlogs]: app-insights-asp-net-trace-logs.md
[výkon]: app-insights-web-monitor-performance.md
[Java serveru]: app-insights-platforms.md
[portál]: http://portal.azure.com/
[qna]: app-insights-troubleshoot-faq.md
[redfield]: app-insights-monitor-performance-live-website-now.md
[prostředků]: app-insights-resources-roles-access-control.md
[zahájení]: app-insights-overview.md



<!--HONumber=Jun16_HO2-->


