---
title: "Přehled diagnostiky a Azure Service Fabric monitorování | Microsoft Docs"
description: "Další informace o monitorování a Diagnostika pro clustery, aplikace a služby Azure Service Fabric."
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
ms.date: 07/17/2017
ms.author: dekapur
ms.openlocfilehash: 88f4a23f89a1c8fd88db1df3a7ff03ae5df64c0f
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="monitoring-and-diagnostics-for-azure-service-fabric"></a>Monitorovací a diagnostické pro Azure Service Fabric

Monitorovací a diagnostické jsou důležité k vývoji, testování a nasazení aplikací a služeb v jakémkoli prostředí. Řešení Service Fabric fungují lépe, když plánování a implementace monitorování a diagnostiky, které pomáhají zajistit aplikace a služby jsou funguje podle očekávání v místním vývojovém prostředí, nebo v produkčním prostředí.

Hlavní cíle monitorování a Diagnostika se:
* Najít a diagnostikovat problémy s hardwarem a infrastruktury
* Rozpoznat problémy s softwaru a aplikace, zkrátit dobu prostojů při služby
* Informace k prostředku využívání a nápovědy jednotky operations rozhodování o
* Optimalizace výkonu aplikace, služby a infrastruktury
* Generovat podnikových statistik a identifikujte oblasti zlepšování


Celkové pracovní postup monitorování a Diagnostika zahrnuje tři kroky:

1. **Generování událostí**: Jedná se o událostí (protokoly, trasování, vlastní události) na infrastrukturu (cluster), platformy a na úrovni aplikace / služby
2. **Agregace událostí**: vygenerovaných událostí je třeba shromažďovat a agregovat předtím, než lze zobrazit
3. **Analýza**: události musí být vizualizovaných a v některých formátu, aby bylo možné pro analýzu a zobrazit podle potřeby

Více produkty jsou k dispozici, které zahrnují tyto tři oblasti, a můžete vybrat různé technologie pro každou. Je důležité se ujistit, že různých částí spolupracovat a přinést řešení monitorování v začátku do konce pro váš cluster.

## <a name="event-generation"></a>Generování událostí

Prvním krokem při monitorování a Diagnostika pracovního postupu je vytváření a generování událostí a protokoly. Tyto události, protokoly a trasování jsou generovány na dvou úrovních: vrstvy platformy (včetně clusteru, počítače nebo akce Service Fabric) nebo aplikační vrstvu (všechny instrumentace přidat do aplikace a služby, které jsou nasazené na clusteru). Události na všech těchto úrovních jsou přizpůsobitelné, i když Service Fabric poskytuje některé instrumentace ve výchozím nastavení.

Další informace o [úroveň události platformy](service-fabric-diagnostics-event-generation-infra.md) a [události na úrovni aplikace](service-fabric-diagnostics-event-generation-app.md) pochopit, co je k dispozici a jak přidat další instrumentace.

Po rozhodování o protokolování zprostředkovatele byste chtěli použít, musíte zajistit, bude vaše protokoly agregovat a uložené správně.

## <a name="event-aggregation"></a>Agregace událostí

Pro shromažďování protokolů a událostí generovaných aplikací a cluster, obvykle doporučujeme používat [Azure Diagnostics](service-fabric-diagnostics-event-aggregation-wad.md) (podobně jako více do kolekce založené na agentovi protokolu) nebo [EventFlow](service-fabric-diagnostics-event-aggregation-eventflow.md) () v procesu protokolu kolekce).

Shromažďování protokolů aplikací pomocí rozšíření diagnostiky Azure je vhodný pro služby Service Fabric, pokud je sada protokolu zdroje a cíle se nemění často a přehledné mapování mezi zdroji a cíli. Z důvodu pro toto je konfigurace Azure Diagnostics se odehrává na vrstvě Resource Manager, takže provedení významných změn konfigurace vyžaduje aktualizaci nebo opětovného nasazení clusteru. Kromě toho je nejlépe použity při ověřování, protokoly se ukládají někde další trvalé, ze které lze k nim podle různých platformách analysis. To znamená, že končí až se kanálu méně efektivní, než budete s možností jako EventFlow.

Pomocí [EventFlow](https://github.com/Azure/diagnostics-eventflow) umožňuje mít služby odeslat protokoly přímo na platformu pro analýzu a vizualizace nebo do úložiště. K tomuto účelu se dají používat další knihovny (objektu ILogger, Serilog atd.), ale EventFlow má výhodu s byly navrženy speciálně pro kolekce v procesu protokolu a k podpoře služby Service Fabric. To může mít několik potenciální výhody:

* Jednoduchá konfigurace a nasazení
    * Konfigurace shromažďování diagnostických dat je právě součástí konfigurace služby. Je snadné vždy synchronizujte jej "" s zbývající aplikace
    * Konfigurace pro aplikaci nebo službě,-je snadno dosažitelný
    * Konfigurace cíle dat prostřednictvím EventFlow stačí odpovídající balíček NuGet pro přidání a změna *eventFlowConfig.json* souboru
* Flexibilita
    * Aplikace může posílat data, bez ohledu na účelu, také je zde knihovna klienta, který podporuje systém cílových dat úložiště. Nový port nelze přidat podle potřeby
    * Komplexní zachycení, filtrování a agregace dat pravidla mohou být implementována
* Přístup k datům interní aplikace a kontext
    * Subsystém diagnostiky běžících v rámci procesu aplikace nebo služba může snadno posílení trasování s kontextové informace

Poznámka je, že tyto dvě možnosti se navzájem nevylučují a i když je možné získat podobné úlohu provést používat jeden z nich, může také dávají smysl pro obě nastavení. Ve většině případů kombinování agenta se v procesu kolekce může vést k větší spolehlivost monitorování pracovního postupu. Rozšíření Azure Diagnostics (agent) může být vaše vybrané cesty pro platformu úrovně protokoly při můžete použít EventFlow (v rámci procesu shromažďování) pro svoje protokoly úrovni aplikace. Jakmile budete mít započítáno se co nejlépe vám vyhovuje, je čas přemýšlet o tom, jak chcete zobrazit a analyzovat vaše data.

## <a name="event-analysis"></a>Analýza události

Existuje několik skvělé platforem, které existují na trhu, pokud jde o analýzy a vizualizace dat monitorování a diagnostiky. Jsou dva, které doporučujeme [OMS](service-fabric-diagnostics-event-analysis-oms.md) a [Application Insights](service-fabric-diagnostics-event-analysis-appinsights.md) z důvodu jejich lepší integraci s Service Fabric, ale by měl vypadat také do [elastické zásobníku](https://www.elastic.co/products) () zejména pokud uvažujete o spuštění clusteru v režimu offline), [Splunk](https://www.splunk.com/), nebo jakoukoli jinou platformu vaši volbu.

Klíčové body pro libovolnou platformu, že si vyberete by měla obsahovat jak pohodlné jste s uživatelským rozhraním a dotazování umožňuje vizualizovat data a vytvářet snadno čitelné řídicí panely a dalších nástrojů poskytuje k vylepšení monitorování, možnosti například automatizované výstrahy.

Kromě platformou si zvolíte když jste nastavili cluster Service Fabric jako prostředek služby Azure, můžete také získat přístup k Azure se na pole monitorování pro počítače, které může být užitečná pro konkrétní výkonu a metriky monitorování.

### <a name="azure-monitor"></a>Azure Monitor

Můžete použít [Azure monitorování](../monitoring-and-diagnostics/monitoring-overview.md) monitorovat mnoho prostředků Azure, na kterých je vytvořen cluster Service Fabric. Sadu metriky pro [škálovací sadu virtuálních počítačů](../monitoring-and-diagnostics/monitoring-supported-metrics.md#microsoftcomputevirtualmachinescalesets) a jednotlivých [virtuální počítače](../monitoring-and-diagnostics/monitoring-supported-metrics.md#microsoftcomputevirtualmachinescalesetsvirtualmachines) se automaticky shromažďovat a zobrazí na portálu Azure. Chcete-li zobrazit shromážděných informací na portálu Azure, vyberte skupinu prostředků, která obsahuje clusteru Service Fabric. Pak vyberte škálovací sadu virtuálních počítačů, které chcete zobrazit. V **monitorování** vyberte **metriky** k zobrazení grafu hodnot.

![Zobrazení portálu Azure shromažďovat informace o metrikách](media/service-fabric-diagnostics-overview/azure-monitoring-metrics.png)

Chcete-li přizpůsobit grafy, postupujte podle pokynů v [metriky v Microsoft Azure](../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md). Také můžete vytvořit oznámení založené na tyto metriky, jak je popsáno v [vytvoření výstrahy v Azure monitorování pro služby Azure](../monitoring-and-diagnostics/insights-alerts-portal.md). Výstrahy můžete odeslat oznámení služby pomocí webové háky, jak je popsáno v [nakonfigurovat webové háku na výstrahu Azure metriky](../monitoring-and-diagnostics/insights-webhooks-alerts.md). Azure monitorování podporuje pouze jedno předplatné. Pokud je nutné sledovat více předplatných, nebo pokud potřebujete další funkce, [analýzy protokolů](https://azure.microsoft.com/documentation/services/log-analytics/), součást nástroje Microsoft Operations Management Suite nabízí komplexní řešení pro správu IT pro místní i cloudové infrastruktura. Data z Azure monitorování přímo k Log Analytics může směrovat, abyste viděli metriky a protokoly pro celé prostředí na jednom místě.

## <a name="next-steps"></a>Další kroky

### <a name="watchdogs"></a>Watchdogs

Sledovací zařízení je samostatný služba, která může sledovat stav a zatížení v rámci služeb a sestavy stavu pro všechno, co je v hierarchii stavu modelu. To může pomoct zabránit chybám, které by se na základě zobrazení jedné služby. Watchdogs jsou také vhodné místo pro hostitele kód, který provede nápravné akce bez zásahu uživatele (například čištění souborů protokolů v úložiště v určitých časových intervalech). Můžete najít implementaci služby sledovací zařízení ukázka [zde](https://github.com/Azure-Samples/service-fabric-watchdog-service).

Začínáme s pochopení, jak získat generovány událostí a protokolů v [platformy úroveň](service-fabric-diagnostics-event-generation-infra.md) a [úrovni aplikace](service-fabric-diagnostics-event-generation-app.md).