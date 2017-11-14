---
title: "Čítače výkonu ve službě Application Insights | Microsoft Docs"
description: "Systém monitorování a vlastní čítače výkonu .NET ve službě Application Insights."
services: application-insights
documentationcenter: 
author: mrbullwinkle
manager: carmonm
ms.assetid: 5b816f4c-a77a-4674-ae36-802ee3a2f56d
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 10/11/2016
ms.author: mbullwin
ms.openlocfilehash: 40821d32c5bdfe51bb3cb205660d6f25b2c3fadc
ms.sourcegitcommit: e38120a5575ed35ebe7dccd4daf8d5673534626c
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/13/2017
---
# <a name="system-performance-counters-in-application-insights"></a>Čítače výkonu systému ve službě Application Insights
Systém Windows nabízí širokou škálu [čítače výkonu](http://www.codeproject.com/Articles/8590/An-Introduction-To-Performance-Counters) například obsazení procesoru, paměti, disku a využití sítě. Můžete také definovat vlastní. [Application Insights](app-insights-overview.md) můžete zobrazit tyto čítače výkonu, pokud vaše aplikace běží v rámci služby IIS na místního hostitele nebo virtuální počítač, ke kterému mají přístup pro správu. Grafy znamenat prostředky k dispozici pro vaše živé aplikace a může pomoct identifikovat nevyváženou zatížení mezi instancemi serveru.

Čítače výkonu se zobrazí v okně servery, které obsahuje tabulku této segmenty instance serveru.

![Čítače výkonu, které jsou hlášeny ve službě Application Insights](./media/app-insights-performance-counters/counters-by-server-instance.png)

(Čítače výkonu nejsou k dispozici pro webové aplikace Azure. Ale můžete [odesílání Azure Diagnostics Application Insights](app-insights-azure-diagnostics.md).)

## <a name="view-counters"></a>Zobrazení čítačů
V okně servery ukazuje výchozí sadu čítačů výkonu. 

Informace o jiných čítačů, buď upravit grafy v okně servery nebo otevřete nové [Průzkumníku metrik](app-insights-metrics-explorer.md) okno a přidejte nové grafy. 

K dispozici čítače jsou označeny jako metriky, když se upravit graf.

![Čítače výkonu, které jsou hlášeny ve službě Application Insights](./media/app-insights-performance-counters/choose-performance-counters.png)

Chcete-li zobrazit všechny nejužitečnější grafy na jednom místě, vytvořte [řídicí panel](app-insights-dashboards.md) a kód pin je k němu.

## <a name="add-counters"></a>Přidání čítačů
Není-li čítač výkonu, který má být zobrazena v seznamu metriky, je to způsobeno Application Insights SDK není shromažďování ve vašem webovém serveru. Můžete nakonfigurovat ji tak.

1. Zjistěte, jaké čítače jsou k dispozici na vašem serveru pomocí tohoto příkazu Powershellu na serveru:
   
    `Get-Counter -ListSet *`
   
    (Viz [ `Get-Counter` ](https://technet.microsoft.com/library/hh849685.aspx).)
2. Otevřete soubor ApplicationInsights.config.
   
   * Pokud jste přidali Application Insights do vaší aplikace během vývoje, upravit soubor ApplicationInsights.config ve vašem projektu a pak znovu nasadit na vaše servery.
   * Pokud jste použili monitorování stavu instrumentovat webové aplikace za běhu, najít soubor ApplicationInsights.config v kořenovém adresáři aplikace ve službě IIS. Aktualizaci existuje v každé instanci serveru.
3. Úpravy – direktiva kolekce výkonu:
   
```XML
   
    <Add Type="Microsoft.ApplicationInsights.Extensibility.PerfCounterCollector.PerformanceCollectorModule, Microsoft.AI.PerfCounterCollector">
      <Counters>
        <Add PerformanceCounter="\Objects\Processes"/>
        <Add PerformanceCounter="\Sales(photo)\# Items Sold" ReportAs="Photo sales"/>
      </Counters>
    </Add>

```

Můžete zaznamenat standardní čítače i těch, které že jste implementovali sami. `\Objects\Processes`Příkladem standardní čítač je k dispozici na všechny systémy Windows. `\Sales(photo)\# Items Sold`je příklad vlastní čítače, který může být implementována ve webové službě. 

Formát je `\Category(instance)\Counter"`, nebo kategorie, které nemají instancí, právě `\Category\Counter`.

`ReportAs`je vyžadována pro názvy čítačů, které se neshodují `[a-zA-Z()/-_ \.]+` – to znamená, že obsahovat znaky, které nejsou v následujících sadách: písmena, kulaté závorky, lomítkem, pomlčky, podtržítka, místo, tečka.

Pokud zadáte instance, budou shromažďovány jako dimenze "CounterInstanceName" hlášené metriky.

### <a name="collecting-performance-counters-in-code"></a>Shromažďování čítače výkonu v kódu
Pokud chcete shromáždit čítače výkonu systému a jejich odeslání do služby Application Insights, můžete přizpůsobit fragmentu níže:


``` C#

    var perfCollectorModule = new PerformanceCollectorModule();
    perfCollectorModule.Counters.Add(new PerformanceCounterCollectionRequest(
      @"\.NET CLR Memory([replace-with-application-process-name])\# GC Handles", "GC Handles")));
    perfCollectorModule.Initialize(TelemetryConfiguration.Active);
```
Nebo můžete provést totéž s vlastní metriky, které jste vytvořili:

``` C#
    var perfCollectorModule = new PerformanceCollectorModule();
    perfCollectorModule.Counters.Add(new PerformanceCounterCollectionRequest(
      @"\Sales(photo)\# Items Sold", "Photo sales"));
    perfCollectorModule.Initialize(TelemetryConfiguration.Active);
```

## <a name="performance-counters-in-analytics"></a>Čítače výkonu v Analytics
Můžete vyhledat a zobrazit sestavy čítače výkonu v [Analytics](app-insights-analytics.md).

**Čítače výkonu** schématu zpřístupní `category`, `counter` název, a `instance` název jednotlivých čítačů výkonu.  V telemetrii pro každou aplikaci zobrazí se pouze čítačů pro tuto aplikaci. Například pokud chcete zobrazit jsou k dispozici co čítače: 

![Čítače výkonu v analytics Application Insights](./media/app-insights-performance-counters/analytics-performance-counters.png)

('Instance, zde označují instance čítače výkonu, není instance počítače role nebo serveru. Název instance čítače výkonu obvykle segmenty čítače například využití procesoru podle názvu procesu nebo aplikace.)

Pokud chcete získat graf dostupné paměti za poslední období: 

![Paměť timechart v analytics Application Insights](./media/app-insights-performance-counters/analytics-available-memory.png)

Jako další telemetrií **čítače výkonu** také má sloupec `cloud_RoleInstance` určující identita instance hostitele serveru, na kterém aplikace běží. Chcete-li například porovnat výkon vaší aplikace na různé počítače: 

![Výkon oddělených instance role ve službě Application Insights analytics](./media/app-insights-performance-counters/analytics-metrics-role-instance.png)

## <a name="aspnet-and-application-insights-counts"></a>Technologie ASP.NET a počty Application Insights
*Jaký je rozdíl mezi rychlost výjimky a výjimky metriky?*

* *Míra výjimka* je čítače výkonu systému. Modul CLR spočítá všechny zpracovávaný a neošetřené výjimky, které jsou vyvolány a součet vydělí v intervalu vzorkování prahovou hodnotou délku intervalu. Application Insights SDK shromažďuje tento výsledek a odešle ji do portálu.
* *Výjimky* je počet přijatých portálu v intervalu vzorkování grafu TrackException sestavy. Obsahuje pouze zpracovávaný výjimky, kde jste napsali TrackException zavolá do vašeho kódu a neobsahuje všechny [neošetřené výjimky](app-insights-asp-net-exceptions.md). 

## <a name="performance-counters-in-aspnet-core-applications"></a>Čítače výkonu v aplikacích Asp.Net Core
Čítače výkonu jsou podporovány pouze v případě, že aplikace je cílení úplné rozhraní .NET Framework. Neexistuje žádné schopnost shromažďovat čítače výkonu pro .net základní aplikace.

## <a name="alerts"></a>Výstrahy
Jako další metriky můžete [nastavit upozornění](app-insights-alerts.md) varovat, pokud čítače výkonu přejde mimo omezení zadáte. Otevřete okno Výstrahy a klikněte na tlačítko Přidat výstrahy.

## <a name="next"></a>Další kroky
* [Sledování závislostí](app-insights-asp-net-dependencies.md)
* [Sledování výjimek](app-insights-asp-net-exceptions.md)

