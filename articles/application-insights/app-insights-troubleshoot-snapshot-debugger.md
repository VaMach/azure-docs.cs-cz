---
title: "Ladicí program Průvodce odstraňováním potíží s snímek služby Azure Application Insights | Microsoft Docs"
description: "Časté otázky k Azure Application Insights snímku ladicí program."
services: application-insights
documentationcenter: .net
author: mrbullwinkle
manager: carmonm
ms.assetid: 
ms.service: application-insights
ms.workload: 
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 11/15/2017
ms.author: mbullwin
ms.openlocfilehash: 5d6a2fe4c3ee373172f5324b6c7a39e4f94f4652
ms.sourcegitcommit: 29bac59f1d62f38740b60274cb4912816ee775ea
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/29/2017
---
# <a name="snapshot-debugger-troubleshooting-guide"></a>Snímku ladicí program: Průvodce odstraňováním potíží

Ladicí program snímku Application Insights umožňuje automatické shromažďování snímků ladění z provozu webových aplikací. Snímek zobrazuje stav zdrojového kódu a proměnné v okamžiku, kdy došlo k výjimce. Pokud máte potíže s ladicím programem snímku Application Insights zprovoznění Tento článek vás provede procesem jak ladicího programu funguje, společně s řešení pro běžné scénáře řešení potíží. 

## <a name="how-does-application-insights-snapshot-debugger-work"></a>Jak funguje Application Insights snímku ladicí program

Ladicí program snímku Application Insights je součástí kanálu telemetrie Application Insights (instance ITelemetryProcessor), kolekce snímku monitoruje výjimky vydané v kódu (AppDomain.FirstChanceException) a výjimky které získat sleduje kanálu Telemetrii Application Insights výjimka. Po úspěšném přidání kolekce snímku do projektu a zjistil, že je jedinou výjimkou v telemetrii Application Insights kanálu, Application Insights vlastní událost s názvem 'AppInsightsSnapshotCollectorLogs' a ' SnapshotCollectorEnabled' v vlastní Data budou odeslány. Ve stejnou dobu bude spuštěn proces s názvem "MinidumpUploader.exe' k nahrání souborů dat shromážděných snímků do Application Insights.  Když MinidumpUploader.exe zpracovat spustí, vlastní události s názvem 'UploaderStart' odeslána. Po provedení předchozích kroků bude snímek kolekce zadejte své běžné monitorování chování.

Při snímek kolekce je monitorování telemetrie výjimek Application Insights, používá parametry (například ThresholdForSnapshotting, MaximumSnapshotsRequired, MaximumCollectionPlanSize, ProblemCounterResetInterval) definovaný v nakonfigurovat a určit, kdy se mají shromažďovat snímku. Pokud jsou splněny všechna pravidla, kolekce se bude požadovat snímku pro další výjimky na jednom místě. Budou odeslány současně, služby Application Insights vlastní události s názvem 'AppInsightsSnapshotCollectorLogs' a 'RequestSnapshots'. Vzhledem k tomu, že kompilátor bude optimalizace kódu "Verze", místní proměnné nemusí být zobrazeny ve shromážděných snímku. Kolekce snímku se pokusí deoptimize metodu, která vrátila výjimku, když jej požaduje snímky. Během této doby budou odeslány v vlastních dat Application Insights vlastní událostí s názvem 'AppInsightsSnapshotCollectorLogs' a 'ProductionBreakpointsDeOptimizeMethod'.  Když jsou shromažďována snímku další výjimky, místní proměnné bude k dispozici. Až se shromáždí snímku, ji budou reoptimize kód k zajištění výkonu. 

> [!NOTE]
> Deoptimization vyžaduje rozšíření lokality služby Application Insights k instalaci.

Pokud snímek je požadována pro konkrétní výjimku, kolekce snímku spustí monitorování zpracování kanálu (AppDomain.FirstChanceException) výjimek vaší aplikace. Pokud výjimka zase objeví, kolekce začne snímku (Application Insights vlastní události s názvem 'AppInsightsSnapshotCollectorLogs' a 'SnapshotStart' ve vlastních dat). Potom vytvoří stínovou kopii běžící proces se provádí (tabulky stránky se duplicitní). Obvykle bude to trvat 10 až 20 milisekundách. Poté budou odeslány služby Application Insights vlastní události s názvem 'AppInsightsSnapshotCollectorLogs' a 'SnapshotStop' v vlastní data. Při vytváření procesu forked celkovou velikost paměti stránkové se zvýší o stejnou hodnotu jako stránkové paměť spuštěné aplikace (pracovní sady budou mnohem menší). Když váš proces aplikace za normálních okolností běží stínové kopie bude paměti procesu zálohované na disk a nahrán do Application Insights. Po nahrání snímku, bude odeslána Application Insights vlastní událost s názvem 'UploadSnapshotFinish'.

## <a name="is-the-snapshot-collector-working-properly"></a>Kolekce snímek funguje správně?

### <a name="how-to-find-snapshot-collector-logs"></a>Jak najít protokoly snímek kolekce
Protokoly kolekce snímku se odesílají do účtu přehled aplikace, pokud [balíček NuGet kolekce snímku](https://www.nuget.org/packages/Microsoft.ApplicationInsights.SnapshotCollector) verze 1.1.0 nebo novější je nainstalován. Zajistěte, aby *ProvideAnonymousTelemetry* není nastaven na hodnotu false (hodnota je true ve výchozím nastavení).

* Přejděte do zdroje Application Insights na portálu Azure.
* Klikněte na tlačítko *vyhledávání* v oddílu Přehled.
* Do vyhledávacího pole zadejte následující řetězec:
    ```
    AppInsightsSnapshotCollectorLogs OR AppInsightsSnapshotUploaderLogs OR UploadSnapshotFinish OR UploaderStart OR UploaderStop
    ```
* Poznámka: Změna *čas rozsah* v případě potřeby.

![Snímek obrazovky vyhledávání snímek kolekce protokoly](./media/app-insights-troubleshoot-snapshot-debugger/001.png)


### <a name="examine-snapshot-collector-logs"></a>Zkontrolujte protokoly kolekce snímku
Při hledání kolekce snímku protokoly, musí být 'UploadSnapshotFinish' událostí v rozsahu cílový čas. Pokud stále nevidíte, otevřete ladění snímku tlačítko Otevřít snímku, pošlete e-mail na snapshothelp@microsoft.com s klíč instrumentace Application Insights.

![Snímek obrazovky Zkontrolujte protokoly kolekce snímku](./media/app-insights-troubleshoot-snapshot-debugger/005.png)

## <a name="i-cannot-find-a-snapshot-to-open"></a>Nelze najít snímek, který chcete otevřít
Pokud tyto kroky nejsou vám tento problém vyřešit, pošlete e-mail na snapshothelp@microsoft.com s klíč instrumentace Application Insights.

### <a name="step-1-make-sure-your-application-is-sending-telemetry-data-and-exception-data-to-application-insights"></a>Krok 1: Zajistěte, aby že vaše aplikace odesílá telemetrická data a data výjimky Application insights
Přejděte do prostředku Application Insights, zkontrolujte, zda je data odeslaná z vaší aplikace.

### <a name="step-2-make-sure-snapshot-collector-is-added-correctly-to-your-applications-application-insights-telemetry-pipeline"></a>Krok 2: Zkontrolujte, zda kolekce snímku se správně přidá do kanálu vaší aplikace Telemetrii Application Insights
Protokoly můžete najít v kroku, jak najít protokoly snímek kolekce, kolekce snímku se správně přidá do projektu, můžete tento krok ignorovat.

Pokud neexistují žádné protokoly kolekce snímku, ověřte následující:
* Pro aplikace ASP.NET, classic, zkontrolujte pro tento řádek  *<Add Type="Microsoft.ApplicationInsights.SnapshotCollector.SnapshotCollectorTelemetryProcessor, Microsoft.ApplicationInsights.SnapshotCollector">*  v *souboru ApplicationInsights.config* souboru.

* Pro aplikace ASP.NET Core, zajistěte, aby *ITelemetryProcessorFactory* s *SnapshotCollectorTelemetryProcessor* se přidá do *IServiceCollection* služby .

* Také zkontrolujte, že používáte klíč instrumentace správné v k publikované aplikaci.

* Snímek kolekce nepodporuje více klíčů instrumentace v rámci jedné aplikace, se bude odesílat snímky klíč instrumentace první výjimky, které ho dodržuje.

* Pokud nastavíte *InstrmentationKey* ručně v kódu, proveďte aktualizaci *InstrumentationKey* element z *souboru ApplicationInsights.config*.

### <a name="step-3-make-sure-the-minidump-uploader-is-started"></a>Krok 3: Zkontrolujte, zda že je spuštěna osoba minimální výpis
Vyhledejte v protokolech kolekce snímku, *UploaderStart* (do textového pole hledání zadejte UploaderStart). Měla by existovat událost v případě, že kolekce snímku monitorovat první výjimka. Pokud tato událost neexistuje, zkontrolujte další podrobnosti v protokolech. Jeden z možných způsobů řešení tohoto problému je restartování aplikace.

### <a name="step-4-make-sure-snapshot-collector-expressed-its-intent-to-collect-snapshots"></a>Krok 4: Zkontrolujte, zda že kolekce snímku vyjádřit svůj záměr shromažďování snímků
Vyhledejte v protokolech kolekce snímku, *RequestSnapshots* (typ ```RequestSnapshots``` do textového pole hledání).  Pokud není k dispozici žádné, Zkontrolujte prosím konfiguraci, například *ThresholdForSnapshotting* což označuje číslo, které konkrétních výjimek, které může dojít, než začne shromažďování snímků.

### <a name="step-5-make-sure-that-snapshot-is-not-disabled-due-to-memory-protection"></a>Krok 5: Ujistěte se, že není snímek zakázán z důvodu ochrany paměti
K ochraně výkon aplikace, bude jde snímek zachytit po vyrovnávací paměti funkční. V protokolech kolekce snímku vyhledejte 'CannotSnapshotDueToMemoryUsage'. V události vlastních dat bude mít podrobné důvod. Pokud vaše aplikace běží ve webové aplikaci Azure, může být omezení strict. Od webové aplikace Azure bude restartování aplikace, pokud jsou splněny určitá pravidla paměti. Můžete zkusit škálování plánu služby do počítačů s více paměti, jak tento problém vyřešit.

### <a name="step-6-make-sure-snapshots-were-captured"></a>Krok 6: Zkontrolujte, zda zaznamenalo snímky
Vyhledejte v protokolech kolekce snímku, ```RequestSnapshots```.  Pokud žádný není přítomen, zkontrolujte konfiguraci, například ```ThresholdForSnapshotting``` označuje počet specifických výjimek před shromažďování snímků.

### <a name="step-7-make-sure-snapshots-are-uploaded-correctly"></a>Krok 7: Ověřte, zda snímky jsou nahrány správně
Vyhledejte v protokolech kolekce snímku, ```UploadSnapshotFinish```.  Pokud není zadán, pošlete e-mail na snapshothelp@microsoft.com s klíč instrumentace Application Insights. Pokud tato událost existuje, otevřete jednoho z protokolů a zkopírujte hodnotu 'SnapshotId' v vlastní Data. Poté vyhledejte hodnotu. To zjistí výjimka odpovídající snímku. Klikněte na tlačítko výjimku a otevřete ladění snímku. (Pokud neexistuje žádná odpovídající výjimka, může být telemetrie výjimek vzorků, z důvodu vyšší objemy, zkuste to prosím jinou snapshotId.)

![Snímek obrazovky SnapshotId najít](./media/app-insights-troubleshoot-snapshot-debugger/002.png)

![Snímek obrazovky otevřete výjimky](./media/app-insights-troubleshoot-snapshot-debugger/004b.png)

![Snímek obrazovky otevřete snímku ladění](./media/app-insights-troubleshoot-snapshot-debugger/003.png)

## <a name="snapshot-view-local-variables-are-not-complete"></a>Lokální proměnné zobrazení snímků nebyly dokončeny.

Chybí některé lokální proměnné. Pokud vaše aplikace běží verze kódu, bude kompilátor optimalizovat některé proměnné rychle. Například:

```csharp
    const int a = 1; // a will be discarded by compiler and the value 1 will be inline.
    Random rand = new Random();
    int b = rand.Next() % 300; // b will be discarded and the value will be directly put to the 'FindNthPrimeNumber' call stack.
    long primeNumber = FindNthPrimeNumber(b);
```

Pokud váš případ se liší, může to být chyby. Pošlete e-mail na snapshothelp@microsoft.com s klíč instrumentace Application Insights společně s fragmentu kódu.

## <a name="snapshot-view-cannot-obtain-value-of-the-local-variable-or-argument"></a>Zobrazení snímku: Nelze získat hodnotu místní proměnné nebo argument
Zkontrolujte, zda [Application Insights lokality rozšíření](https://www.siteextensions.net/packages/Microsoft.ApplicationInsights.AzureWebSites/) je nainstalovaná. Pokud potíže potrvají, obraťte se na adresu snapshothelp@microsoft.com s klíč instrumentace Application Insights.
