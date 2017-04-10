---
title: "Azure Application Insights pro server Windows a role pracovního procesu | Dokumentace Microsoftu"
description: "Ručně přidejte do aplikace ASP.NET sadu SDK Application Insights k analýze využití, dostupnosti a výkonu."
services: application-insights
documentationcenter: .net
author: alancameronwills
manager: douge
ms.assetid: 106ba99b-b57a-43b8-8866-e02f626c8190
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: get-started-article
ms.date: 11/01/2016
ms.author: awills
translationtype: Human Translation
ms.sourcegitcommit: 197ebd6e37066cb4463d540284ec3f3b074d95e1
ms.openlocfilehash: 97d750035e79d4780738e660d05e1a41085d51f1
ms.lasthandoff: 03/31/2017


---
# <a name="manually-configure-application-insights-for-aspnet-applications"></a>Ruční konfigurace Application Insights pro aplikace ASP.NET
[Application Insights](app-insights-overview.md) představují rozšiřitelný nástroj, který vývojářům pomáhá pochopit výkon a využití živých aplikací. Můžete je ručně nakonfigurovat k monitorování serveru Windows, rolí pracovního procesu a dalších aplikací ASP.NET. U webových aplikací je ruční konfigurace alternativou k [automatickému nastavení](app-insights-asp-net.md), které nabízí sada Visual Studio.

![Příklady tabulek sledování výkonu](./media/app-insights-windows-services/10-perf.png)

#### <a name="before-you-start"></a>Než začnete
Budete potřebovat:

* Předplatné [Microsoft Azure](http://azure.com). Pokud váš tým nebo společnost má předplatné Azure, vlastník vás do něj může přidat pomocí vašeho [účtu Microsoft](http://live.com).
* Visual Studio 2013 nebo novější.

## <a name="add"></a>1. Vytvořte prostředek Application Insights
Přihlaste se k webu [Azure Portal](https://portal.azure.com/) a vytvořte nový prostředek Application Insights. Vyberte jako typ aplikace ASP.NET.

![Klikněte na tlačítko Nový, Application Insights](./media/app-insights-windows-services/01-new-asp.png)

[Prostředek](app-insights-resources-roles-access-control.md) v Azure je instancí služby. Tento prostředek je místo, kde se analyzuje a prezentuje telemetrie z vaší aplikace.

Volba typu aplikace nastaví výchozí obsah oken prostředků a vlastnosti viditelné v [Průzkumníku metrik](app-insights-metrics-explorer.md).

#### <a name="copy-the-instrumentation-key"></a>Zkopírovat klíč instrumentace
Klíč identifikuje prostředek a nainstalujte ho brzy do sady SDK pro přímá data do prostředku.

![Klikněte na tlačítko Vlastnosti, vyberte klíč a stiskněte klávesy ctrl + C](./media/app-insights-windows-services/02-props-asp.png)

Kroky, které jste právě provedli pro vytvoření nového prostředku, jsou vhodný způsob, jak začít monitorovat všechny aplikace. Teď do prostředku můžete odesílat data.

## <a name="sdk"></a>2. Instalace balíčku Application Insights v aplikaci
Instalace a konfigurace balíčku Application Insights se liší podle platformy, se kterou právě pracujete. Pro aplikace ASP.NET je to snadné.

1. V sadě Visual Studio upravte balíčky NuGet projektu webové aplikace.
   
    ![Klikněte pravým tlačítkem myši a vyberte možnost Spravovat balíčky Nuget](./media/app-insights-windows-services/03-nuget.png)
2. Nainstalujte balíček Application Insights pro aplikace serveru Windows.
   
    ![Vyhledejte „Application Insights“](./media/app-insights-windows-services/04-ai-nuget.png)
   
    *Mohu použít jiné balíčky?*
   
    Ano. Pokud chcete rozhraní API používat pouze k odesílání vlastní telemetrie, zvolte základní rozhraní API (Microsoft.ApplicationInsights). Balíček Windows Serveru automaticky zahrnuje základní rozhraní API spolu s dalšími balíčky, jako jsou například kolekce čítačů výkonu a monitorování závislostí. 

#### <a name="to-upgrade-to-future-package-versions"></a>Postup upgradu na budoucí verze balíčku
Čas od času vydáváme nové verze sady SDK.

K upgradu [novou verzi balíčku](https://github.com/Microsoft/ApplicationInsights-dotnet-server/releases/) otevřete znovu Správce balíčků NuGet a filtrujte nainstalované balíčky. Vyberte **Microsoft.ApplicationInsights.WindowsServer** a zvolte **Upgradovat**.

Pokud jste provedli jakékoli úpravy souboru ApplicationInsights.config, uložte jeho kopii před upgradem a následně slučte změny do nové verze.

## <a name="3-send-telemetry"></a>3. Odeslání telemetrie
**Pokud jste nainstalovali pouze balíček základního rozhraní API:**

* Nastavte v kódu klíč instrumentace, například v metodě `main()`: 
  
    `TelemetryConfiguration.Active.InstrumentationKey = "` *váš klíč* `";` 
* [Napište si vlastní telemetrii pomocí rozhraní API](app-insights-api-custom-events-metrics.md#ikey).

**Pokud jste nainstalovali jiné balíčky Application Insights**, můžete k nastavení klíče instrumentace použít soubor .config, pokud tomu dáváte přednost:

* Upravit soubor ApplicationInsights.config (který byl nainstalován nástrojem NuGet). Vložte tuto položku těsně před uzavírací značku:
  
    `<InstrumentationKey>` *zkopírovaný klíč instrumentace* `</InstrumentationKey>`
* Ujistěte se, že jsou vlastnosti souboru ApplicationInsights.config v Průzkumníku řešení nastavené na: **Build Action = Content, Copy to Output Directory = Copy**.

Nastavení klíče instrumentace v kódu je užitečné v případě, že chcete [přepínat mezi klíči pro různé konfigurace sestavení](app-insights-separate-resources.md). Pokud klíč nastavíte v kódu, nemusíte ho nastavovat v souboru `.config`.

## <a name="run"></a> Spuštění projektu
Použijte **F5** ke spuštění aplikace a vyzkoušejte: otevření různých stránek k vygenerování nějaké telemetrie.

V sadě Visual Studio zobrazí počet událostí, které byly odeslány.

![Počet událostí v sadě Visual Studio](./media/app-insights-windows-services/appinsights-09eventcount.png)

## <a name="monitor"></a> Zobrazení telemetrických dat
Přihlaste se do [portálu Azure](https://portal.azure.com/) a procházejte do zdroje Application Insights.

Vyhledejte data v tabulkách Přehled. Na první pohled uvidíte pouze jeden nebo dva body. Příklad:

![Proklikejte se k dalším datům](./media/app-insights-windows-services/12-first-perf.png)

Proklikejte se prostřednictvím jakékoli grafu pro zobrazení podrobnějších metrik. [Další informace o metrikách.](app-insights-web-monitor-performance.md)

#### <a name="no-data"></a>Žádná data?
* Použijte aplikaci a otevřete různé stránky tak, aby došlo k vygenerování nějaké telemetrie.
* Otevřete dlaždici [Vyhledávání](app-insights-diagnostic-search.md) a zobrazte jednotlivé události. Někdy trvá událostem trochu déle, než se dostanou skrz kanály metriky.
* Počkejte několik sekund a klikněte na tlačítko **Aktualizovat**. Grafy se pravidelně samy aktualizují, ale můžete je aktualizovat ručně, pokud čekáte na zobrazení některých dat.
* Viz [Poradce při potížích](app-insights-troubleshoot-faq.md).

## <a name="publish-your-app"></a>Publikování aplikace
Nyní nasaďte aplikaci na server nebo do Azure a sledujte shromažďování dat.

![K publikování aplikace použijte sadu Visual Studio](./media/app-insights-windows-services/15-publish.png)

Při spuštění v režimu ladění prochází telemetrie skrz kanál, takže byste měli vidět zobrazení dat během několika sekund. Při nasazení aplikace v rámci konfigurace verze se data hromadí pomaleji.

#### <a name="no-data-after-you-publish-to-your-server"></a>Žádná data po publikování na serveru?
Otevřete tyto porty pro odchozí přenosy v bráně firewall serveru:

* `dc.services.visualstudio.com:443`
* `f5.services.visualstudio.com:443`

#### <a name="trouble-on-your-build-server"></a>Potíže na vašem serveru sestavení?
Naleznete v tématu [tato položka Poradce při potížích](app-insights-asp-net-troubleshoot-no-data.md#NuGetBuild).

> [!NOTE]
> Pokud vaše aplikace generuje mnoho telemetrie (a používáte verzi sady SDK technologie ASP.NET 2.0.0-beta3 nebo novější), modul adaptivního vzorkování automaticky sníží objem, který je odesílán na portál odesláním pouze reprezentativní části události. Události, které se vztahují ke stejnému požadavku, však budou vybrány nebo nevybrány jako skupina, takže mohou procházet mezi souvisejícími událostmi. 
> [Další informace o vzorkování](app-insights-sampling.md).
> 
> 

## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/100/player]

## <a name="next-steps"></a>Další kroky
* [Přidejte více telemetrie](app-insights-asp-net-more.md) a získejte komplexní náhled na svoji aplikaci.


