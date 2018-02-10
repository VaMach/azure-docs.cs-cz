---
title: "Analýza události prostředků infrastruktury služby Azure pomocí služby Application Insights | Microsoft Docs"
description: "Další informace o vizualizaci a analýzu událostí pomocí Application Insights pro monitorování a Diagnostika Azure Service Fabric clusterů."
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 10/15/2017
ms.author: dekapur
ms.openlocfilehash: 479e486dca432020d5fcbaf98971a9803888bf98
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/09/2018
---
# <a name="event-analysis-and-visualization-with-application-insights"></a>Analýza události a vizualizace s Application Insights

Azure Application Insights je rozšiřitelná platforma pro monitorování aplikací a diagnostiku. Zahrnuje výkonné analýzy a zadávat dotazy na nástroj, přizpůsobitelné řídicích panelů a vizualizací a další možnosti, včetně automatizované výstrahy. Je doporučené platforma pro monitorování a Diagnostika pro Service Fabric aplikace a služby.

## <a name="setting-up-application-insights"></a>Nastavení služby Application Insights

### <a name="creating-an-ai-resource"></a>Vytváření prostředek AI

Vytvořte prostředek AI, přejděte přes Azure Marketplace, a vyhledejte "Application Insights". Měl by se zobrazit jako první řešení (je v kategorii "Web + mobilní"). Klikněte na tlačítko **vytvořit** při hledání v pravém prostředků (potvrďte, že cestu k odpovídá následující obrázek).

![Nový prostředek Application Insights](media/service-fabric-diagnostics-event-analysis-appinsights/create-new-ai-resource.png)

Musíte se k vyplnění nějaké informace, které zřídit prostředek správně. V *typ aplikace* pole, použijte "Webové aplikace ASP.NET" Pokud budete používat některé z Service Fabric je programovací modely nebo publikování aplikace .NET do clusteru. Pokud budete nasazovat hosta spustitelných souborů a kontejnery, použijte "Obecné". Obecně platí, výchozích nastavení používat "Webové aplikace ASP.NET" nechat možnosti otevřené v budoucnu. Název je až vaši volbu a skupina prostředků i předplatného jsou změnit po nasazení prostředku. Doporučujeme vám, že vaše AI prostředek je ve stejné skupině prostředků jako cluster. Pokud potřebujete další informace, najdete v tématu [vytvořte prostředek Application Insights](../application-insights/app-insights-create-new-resource.md)

Je nutné klíč instrumentace AI konfigurace AI nástrojem agregace událostí. AI prostředku je nastaveno (trvá několik minut po nasazení je ověření), přejděte k němu a najděte **vlastnosti** části na levém navigačním panelu. Nové okno se otevře zobrazující *klíč INSTRUMENTACE*. Pokud potřebujete změnit předplatné nebo skupinu prostředků prostředku, je možné ji provést zde také.

### <a name="configuring-ai-with-wad"></a>Konfigurace s WAD AI

>[!NOTE]
>Používá se pouze pro clustery s Windows v tuto chvíli.

Existují dva primární způsoby odesílání dat z WAD AI Azure, které se dosáhne přidáním jímky AI WAD konfigurace, jak je podrobně uvedeno v [v tomto článku](../monitoring-and-diagnostics/azure-diagnostics-configure-application-insights.md).

#### <a name="add-an-ai-instrumentation-key-when-creating-a-cluster-in-azure-portal"></a>Při vytváření clusteru na portálu Azure přidejte kód instrumentace AI

![Přidání AIKey](media/service-fabric-diagnostics-event-analysis-appinsights/azure-enable-diagnostics.png)

Při vytváření clusteru, pokud je vypnuté Diagnostika "Na", se zobrazí volitelné pole k zadání klíč instrumentace Application Insights. Pokud můžete vložit vaše AI IKey zde, podřízený AI se automaticky nakonfiguruje pro vás v šabloně Resource Manager, který se používá k nasazení clusteru.

#### <a name="add-the-ai-sink-to-the-resource-manager-template"></a>Přidat do šablony Resource Manageru jímky AI

V "WadCfg" šablony Resource Manageru přidejte "Podřízený" zahrnutím následující dvě změny:

1. Přidat konfiguraci podřízený přímo po deklarování z `DiagnosticMonitorConfiguration` dokončení:

    ```json
    "SinksConfig": {
        "Sink": [
            {
                "name": "applicationInsights",
                "ApplicationInsights": "***ADD INSTRUMENTATION KEY HERE***"
            }
        ]
    }

    ```

2. Zahrnout jímky v `DiagnosticMonitorConfiguration` přidáním následujícího řádku `DiagnosticMonitorConfiguration` z `WadCfg` (bezprostředně před `EtwProviders` jsou deklarovány):

    ```json
    "sinks": "applicationInsights"
    ```

V obou fragmenty kódu výše byl použit název "applicationInsights" k popisu jímky. Toto není požadavek a také název jímky je součástí "jímky", název můžete nastavit na libovolný řetězec.

V současné době se protokoly z clusteru zobrazí jako trasování v prohlížeči na AI protokolu. Vzhledem k tomu, že většina trasování pocházejících z platformy je úrovně "Informační", byste také zvážit změna konfigurace podřízený jenom k odesílání protokolů typu "Kritický" nebo "Chyba". Stačí přidáním "Kanály" podřízený, jak je předvedeno v [v tomto článku](../monitoring-and-diagnostics/azure-diagnostics-configure-application-insights.md).

>[!NOTE]
>Pokud používáte nesprávné IKey AI portálu nebo v šabloně Resource Manager, budete muset ručně změnit klíč a aktualizovat cluster / zavést jej znovu. 

### <a name="configuring-ai-with-eventflow"></a>Konfigurace s EventFlow AI

Pokud používáte EventFlow pro agregaci událostí, je třeba importovat `Microsoft.Diagnostics.EventFlow.Output.ApplicationInsights`balíček NuGet. Následující musí být součástí *výstupy* části *eventFlowConfig.json*:

```json
"outputs": [
    {
        "type": "ApplicationInsights",
        // (replace the following value with your AI resource's instrumentation key)
        "instrumentationKey": "00000000-0000-0000-0000-000000000000"
    }
]
```

Zajistěte, aby proveďte požadované změny v filtry a také zahrnovat všechny ostatní vstupy (spolu s jejich odpovídajících balíčky NuGet).

## <a name="aisdk"></a>AI.SDK

Obecně doporučujeme používat EventFlow a WAD jako řešení agregace, protože umožňují pro více modulární přístup k diagnostiky a monitorování, tj. Pokud chcete změnit vaše výstupy z EventFlow, vyžaduje nijak nemění skutečné instrumentace, právě jednoduchých úprav konfiguračního souboru. Pokud však rozhodnete investovat do pomocí Application Insights a pravděpodobně nedojde ke změně na jiné platformě, by měla vypadat do aplikace pomocí AI na novou sadu SDK pro agregaci událostí a jejich odesláním AI. To znamená, že se již muset nakonfigurovat EventFlow odeslání dat do AI, ale místo toho nainstaluje balíček Service Fabric NuGet ApplicationInsight. Informace o balíčku naleznete [zde](https://github.com/Microsoft/ApplicationInsights-ServiceFabric).

[Podpora služby Application Insights pro Mikroslužeb a kontejnery](https://azure.microsoft.com/en-us/blog/app-insights-microservices/) uvedeny některé nové funkce, které se pracuje (aktuálně stále v beta), které umožňují, abyste měli bohatší možnosti monitorování se na pole s AI. Jedná se o závislost sledování (používá se při vytváření AppMap služeb a aplikací v clusteru a komunikace mezi nimi) a lepší korelace trasování pocházejících z vašich služeb (pomáhá v lepší přesným rozpoznáním problém v pracovním postupu aplikace nebo služby).

Pokud jsou vývoj v rozhraní .NET a budou pravděpodobně používat některé z Service Fabric programovací modely a jsou ochotni použít AI jako svou platformu pro vizualizaci a analýzu dat událostí a protokolů, pak doporučujeme přejít prostřednictvím sady SDK AI trasy jako pracovní postup monitorování a Diagnostika. Čtení [to](../application-insights/app-insights-asp-net-more.md) a [to](../application-insights/app-insights-asp-net-trace-logs.md) začít pracovat s pomocí AI shromáždit a zobrazit protokoly.

## <a name="navigating-the-ai-resource-in-azure-portal"></a>Navigace AI prostředků na portálu Azure

Po nakonfigurování AI jako výstupu událostí a protokoly informace začněte objeví v prostředku AI za pár minut. Přejděte k prostředku AI, kterým se dostanete na řídicí panel AI prostředků. Klikněte na tlačítko **vyhledávání** na hlavním panelu AI chcete zobrazit nejnovější trasování, které obdržela a možné filtrovat pomocí nich.

*Průzkumníku metrik* je užitečným nástrojem pro vytvoření vlastní řídicí panely podle metriky, které vaše aplikace, služby a cluster může být vytváření sestav. V tématu [zkoumat metriky ve službě Application Insights](../application-insights/app-insights-metrics-explorer.md) nastavit několik grafy pro sami na základě dat shromažďujete.

Kliknutím na tlačítko **Analytics** se dostanete na portál Application Insights analýzy, kde může dotazovat událostí a trasování s větší rozsah a volitelnost. Další informace o to v [Analytics ve službě Application Insights](../application-insights/app-insights-analytics.md).

## <a name="next-steps"></a>Další postup

* [Nastavení výstrah v AI](../application-insights/app-insights-alerts.md) oznámení o změnách v výkonu a využití
* [Inteligentní detekce ve službě Application Insights](../application-insights/app-insights-proactive-diagnostics.md) provede proaktivní analýzu telemetrie odesílána AI varovat před potenciálním problémům s výkonem
