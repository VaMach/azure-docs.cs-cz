---
title: "Monitorování aplikací Azure a prostředky | Microsoft Docs"
description: "Přehled jiné služby společnosti Microsoft a funkce které přispívají k dokončení monitorování strategii pro Azure služby a aplikace."
author: robb
manager: carmonm
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 1b962c74-8d36-4778-b816-a893f738f92d
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/30/2018
ms.author: robb,bwren
ms.openlocfilehash: 505e92b5fc63f570bc4d0f8899ae977b93850356
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/09/2018
---
# <a name="monitoring-azure-applications-and-resources"></a>Monitorování aplikací Azure a prostředky

Sledování je v rámci shromažďování a analýzy dat o výkonu, stavu a dostupnosti obchodní aplikace a prostředky, které závisí na. Účinné strategie sledování vám pomůže porozumět podrobné operaci různé součásti aplikace a zvýšit vaši provozu proaktivně vás upozorní na zásadních problémů, aby předtím, než začnou způsobovat problémy, můžete je vyřešit.

Azure obsahuje více služeb, které jednotlivě provádět určité role nebo úkolu v prostoru pro monitorování a současně poskytovat komplexní řešení pro shromažďování, analýze a funguje na telemetrie z vaší aplikace a základní prostředky Azure podpora.  Může spolupracovat taky monitorovat kritické místních prostředků s cílem poskytnout hybridním monitorování prostředí.   Pochopení nástroje a data, která jsou k dispozici je prvním krokem při vývoji dokončení strategie monitorování pro vaši aplikaci. 

Následující diagram znázorňuje koncepční zobrazení různých komponent, které vzájemně spolupracují a poskytovat monitorování prostředků Azure.  Každá z těchto je popsána v následujících částech s odkazy na podrobné technické informace.

![Přehled monitorování](media/monitoring-overview/overview.png)

## <a name="basic-monitoring"></a>Základní monitorování
Základní monitorování poskytuje základní požadované monitorování napříč prostředků Azure.  Tyto služby vyžadují minimální konfigurace a shromažďovat základní telemetrii, kterou je využít monitorovat služby premium.    

### <a name="azure-monitor"></a>Azure Monitor
[Azure monitorování](../monitoring-and-diagnostics/monitoring-overview-azure-monitor.md) umožňuje základní monitorování pro služby Azure tím, že kolekce [metriky](../monitoring-and-diagnostics/monitoring-overview-metrics.md), [protokoly aktivity](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md), a [diagnostické protokoly](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md).  Například protokol aktivit zjistíte při vytvoření nebo úpravě nové prostředky.  Metriky, které jsou k dispozici, zadejte statistiku výkonu pro různé prostředky a i operační systém v rámci virtuálního počítače.  Můžete zobrazit tato data s jedním z průzkumníci na portálu Azure, odešle analýzy protokolů pro analýzu trendů a podrobné nebo vytvořit pravidla výstrah proaktivně upozornění kritických problémů.

### <a name="service-health"></a>Service Health
Stav aplikace spoléhá na služby Azure, které závisí na.  [Azure stavu služby](../service-health/service-health-overview.md) identifikuje všechny problémy se službami Azure, které může mít vliv na vaše aplikace a také vám pomůže s plánováním pro všechny plánu údržby.

### <a name="azure-advisor"></a>Azure Advisor
[Azure Advisor](../advisor/advisor-overview.md) neustále monitoruje prostředků konfiguraci a využití telemetrie zajistit přizpůsobené doporučení na základě osvědčených postupů.  Následující tato doporučení můžete zvýšit výkon, zabezpečení a dostupnost prostředků podporu vaší aplikací.


## <a name="premium-monitoring-services"></a>Monitorování služby Premium
Následující služby Azure nabízí bohaté možnosti pro shromažďování a analýzy dat monitorování.  Tyto stavět na základní monitorování a využívání běžné funkce v Azure a výkonné analytics poskytnout shromážděná data a poskytuje vám jedinečný insights do vaší aplikace a infrastrukturu.  Jejich prezentují data v rámci konkrétní scénáře, které jsou cíleny na různé cílové skupiny.

### <a name="application-insights"></a>Application Insights
[Application Insights](http://azure.microsoft.com/documentation/services/application-insights) umožňuje monitorování dostupnosti, výkonu a využití aplikace, zda je hostovaná v cloudu nebo místně.  Instrumentaci vaší aplikace pro práci s Application Insights, můžete dosáhnout hlubšímu porozumění, který vám umožní rychle identifikovat a diagnostikovat chyby bez čekání na uživatele a jejich sestavy. S informacemi, které shromáždíte můžete provést informované volby na údržbu a vylepšení vaší aplikace.  Application Insights kromě rozsáhlé nástroje pro interakci s daty, která shromažďuje, ukládá data do úložiště v běžné využít sdílené funkce, jako jsou výstrahy, řídicí panely a hloubkovou analýzu pomocí dotazovacího jazyka pro analýzy protokolů.

### <a name="log-analytics"></a>Log Analytics
[Analýza protokolu](http://azure.microsoft.com/documentation/services/log-analytics) hraje centrální role v Azure monitorování tím, že shromažďování dat z různých zdrojů do jednoho úložiště, kde mohou být analyzovány s účinný dotazovací jazyk.  Application Insights a Azure Security Center ukládají data v analýzy protokolů data ukládat a využívat jeho analytics motoru.  To v kombinaci s daty shromážděnými z Azure monitorování, řešení pro správu a agentů nainstalovaných na virtuálních počítačů v cloudu nebo místně umožňují vytvořit úplný přehled o celé prostředí. 


### <a name="service-map"></a>Mapa služeb
[Mapa služeb](../operations-management-suite/operations-management-suite-service-map.md) poskytuje vhled do prostředí IaaS analýzou virtuálních počítačů s jejich různé procesy a závislosti na jiných počítačích a externí procesy.  Se integruje se službou události, údaje o výkonu a řešení pro správu v analýzy protokolů tak, aby tato data můžete zobrazit v rámci každého počítače a jejich vztah se zbytkem prostředí.  Mapa služeb je podobný [mapy aplikací ve službě Application Insights](../application-insights/app-insights-app-map.md) , ale zaměřuje se na podporu aplikace součásti infrastruktury.

### <a name="network-watcher"></a>Network Watcher
[Sledovací proces sítě](../network-watcher/network-watcher-monitoring-overview.md) poskytuje na základě scénáře monitorování a Diagnostika pro jinou síť scénáře v Azure.  Ukládá data do Azure metriky a Diagnostika pro další analýzu a funguje s následující sítě sledování řešení pro monitorování různých aspektů vaší síti:
* [Sítě monitorování výkonu (NPM)](https://blogs.msdn.microsoft.com/azuregov/2017/09/05/network-performance-monitor-general-availability/) – síť cloudové řešení monitorování, které monitoruje připojení mezi veřejných cloudů, datových center a místními prostředími
* [Monitorování ExpressRoute](https://azure.microsoft.com/en-in/blog/monitoring-of-azure-expressroute-in-preview/) -NPM funkci, která monitoruje připojení klient server a výkonu přes okruhy ExpressRoute.
* Analýza provozu - cloudové řešení, která poskytuje přehled o činnosti uživatelů a aplikací na vaší cloudové síťové.
* [Analýza DNS](https://docs.microsoft.com/en-us/azure/log-analytics/log-analytics-dns) -poskytuje zabezpečení, výkonu a operací souvisejících přehledy založené na vaše servery DNS.

### <a name="management-solutions"></a>Řešení pro správu
[Řešení pro správu](../log-analytics/log-analytics-add-solutions.md) jsou zabalené sady logiky, které nabízejí přehled pro konkrétní aplikaci nebo službě.  Spoléhají na analýzy protokolů ukládat a analyzovat shromážděná data monitorování.  Řešení pro správu jsou k dispozici od společnosti Microsoft a partnery, které poskytují monitorování pro různé služby Azure a třetích stran. Příklad sledování řešení zahrnují [kontejneru monitorování](../log-analytics/log-analytics-containers.md) který umožňuje zobrazit a spravovat hostitele kontejneru a [Azure SQL Analytics](../log-analytics/log-analytics-azure-sql.md) který shromažďuje a vizualizuje metriku výkonu pro SQL Azure databáze.


## <a name="shared-functionality"></a>Sdílené funkce
Následující nástroje Azure poskytují důležité funkce Premium monitorování služeb.  Že jsou sdíleny více služeb díky tomu můžete využít běžné funkce a konfigurace ve více službách.

### <a name="alerts"></a>Výstrahy
[Azure výstrahy](../monitoring-and-diagnostics/monitoring-overview-alerts.md) proaktivně oznámíme vám kritického stavu a potenciálně proveďte opravné akce.  Pravidla výstrah můžete využít data z více zdrojů, včetně metrik a protokoly. Používají [skupiny akcí](../monitoring-and-diagnostics/monitoring-action-groups.md) který obsahují jedinečnou sadu akcí v reakci na oznámení a příjemce.  Podle potřeb, může mít výstrahy spusťte externí akce s použitím webhooky a integrovat pomocí nástrojů pro ITSM.

### <a name="dashboards"></a>Řídicí panely
[Azure řídicí panely](../azure-portal/azure-portal-dashboards.md) umožňují kombinovat různé druhy dat do jednoho podokno v portálu Azure a sdílenou složku s jinými uživateli Azure.  Můžete například vytvořit řídicí panel, který kombinuje dlaždice zobrazující graf metriky, tabulku protokoly aktivity, graf využití z Application Insights a výstup hledání protokolů v analýzy protokolů.

Můžete také exportovat data Log Analytics [Power BI](https://docs.microsoft.com/power-bi/) využít další vizualizace a také aby data k dispozici jiným uživatelům v rámci i mimo vaši organizaci.

### <a name="metrics-explorer"></a>Průzkumníku metrik
[Metriky](../monitoring-and-diagnostics/monitoring-overview-metrics.md) jsou číselné hodnoty generované prostředky Azure, které vám pomohou porozumět operace a výkonu prostředku. Můžete odeslat metriky k analýze protokolů pro analýzu s daty z jiných zdrojů.



### <a name="activity-logs"></a>Protokoly aktivit
[Protokoly aktivity](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md) poskytují informace o operaci prostředků Azure.  To zahrnuje takové informace jako změny konfigurace prostředků, služba stavu incidentů, doporučení na lepší využití prostředku a informace týkající se operací škálování.  Protokoly pro určitý prostředek můžete zobrazit na stránce s jejím Azure portal nebo zobrazení protokolů z více prostředků v Průzkumníku protokolu aktivit.  Můžete také odeslat protokoly aktivity k analýze protokolů, mohou být analyzovány s daty shromážděnými při řešení pro správu, agenty na virtuální počítače a další zdroje.


## <a name="example-scenarios"></a>Ukázkové scénáře
Následují příklady vysoké úrovně, které ilustrují, jak by využívají různé nástroje monitorování v Azure pro různé scénáře.

### <a name="monitoring-a-web-application"></a>Monitorování webové aplikace
Vezměte v úvahu webové aplikace nasazené v Azure pomocí App Services, Azure Storage a databázi SQL.  Můžete začít díky přístupu k [metriky](../monitoring-and-diagnostics/monitoring-overview-metrics.md) a [protokoly aktivity](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md) pro každý z těchto jednotlivých prostředků na svých stránkách na portálu Azure.  To zahrnuje důležité informace, jako je počet požadavků v aplikaci a průměrná doba odezvy kromě identifikace změny konfigurace.

Může pak přejdete na monitorování na portálu Chcete-li zobrazit protokoly pro různé zdroje a metriky společně.  Určení standardní parametry pro metriku, které [vytvořit pravidla výstrah](../monitoring-and-diagnostics/monitoring-overview-unified-alerts.md) proaktivně upozornění v případě, například průměrná doba odezvy zvýší nad prahovou hodnotou.  Chcete-li získat rychlý přehled o denní výkon aplikace, můžete vytvořit řídicí panel Azure zobrazit grafy metrik představující důležité klíčových ukazatelů výkonu.

K provedení hlubší monitorování aplikace, můžete [ho nakonfigurovat pro službu Application Insights](../application-insights/quick-monitor-portal.md).  Nyní můžete shromažďovat další data, která poskytuje další aspekty provoz a výkonu vaší aplikace.  Application Insights zjistí základní vztahy mezi součástmi vaší aplikace, aby vám umožnil vizuální reprezentace prostřednictvím [aplikace mapy](../application-insights/app-insights-app-map.md) kombinaci s [začátku do konce trasování](../application-insights/app-insights-transaction-diagnostics.md) diagnostiky přesné součásti, závislostí nebo výjimky, kde došlo k potížím.  Vytvoříte [testy dostupnosti](../application-insights/app-insights-monitor-web-app-availability.md) proaktivně testování vaší aplikace z různých oblastech.  Chcete-li pomoci vývojářům, můžete [povolit profileru](../application-insights/enable-profiler-compute.md) , můžete sledovat žádosti a jakékoli výjimky na konkrétní řádek kódu.  

Chcete-li získat další přehled o služeb používaných ve vaší aplikaci, můžete přidat [řešení SQL analýzy](../log-analytics/log-analytics-azure-sql.md) shromažďovat další data Log Analytics. Po určité době rozhodnete zjistěte základní příčinu pro časová období, kdy výkonu na webu klesla pod prahovou hodnotou.  Můžete zapsat dotazu pomocí analýzy protokolů ke korelaci dat využití a výkonu shromážděné pomocí Application Insights pomocí konfigurace a výkonu data mezi prostředky Azure podporu aplikace.


### <a name="monitoring-virtual-machines"></a>Monitorování virtuálních počítačů
Máte směs Windows a Linux virtuální počítače běžící v Azure.  Pomocí Azure monitorování zobrazíte [protokoly aktivity](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md) a [metriky na úrovni hostitele](../monitoring-and-diagnostics/monitoring-overview-metrics.md) a poté přidejte [rozšíření Azure Diagnostics](../virtual-machines/linux/tutorial-monitoring.md#install-diagnostics-extension) k virtuálním počítačům, aby bylo možné shromažďovat metriky z hostovaného operačního systému.  Potom můžete vytvářet [výstrah pravidla](../monitoring-and-diagnostics/monitoring-overview-unified-alerts.md) pro proaktivní upozornění, pokud základní metriky takové využití procesoru a paměti mezi prahové hodnoty.

Shromažďování podrobných informací o virtuálních počítačů spuštěných obchodní aplikace, můžete [vytvořit pracovní prostor analýzy protokolů a povolit rozšíření virtuálního počítače](../log-analytics/log-analytics-quick-collect-azurevm.md) na každém počítači.  Nakonfigurujete [kolekce různých datových zdrojů](../log-analytics/log-analytics-data-sources.md) pro vaši aplikaci a [vytvořte zobrazení](../log-analytics/log-analytics-view-designer.md) chcete sestavu podle jeho každodenní operace a výkonu.  Potom můžete [vytvořit pravidla výstrah](../monitoring-and-diagnostics/monitoring-overview-unified-alerts.md) upozornění v případě přijetí události určité chybě.  Chcete-li neustále monitorovat stav s nainstalovaným agentem, můžete přidat [řešení pro správu agenta stavu](../operations-management-suite/oms-solution-agenthealth.md).

Chcete-li získat další informace o aplikaci, můžete [Přidat závislost agenta](../operations-management-suite/operations-management-suite-service-map-configure.md) k virtuálním počítačům, aby bylo možné přidat je do [mapy služeb](../operations-management-suite/operations-management-suite-service-map.md).  Ji zjistí důležité procesy a identifikuje připojení mezi počítači s jinými službami.  Po výpadku hlášené pomocí mapy služeb provádět forenzní můžete identifikovat konkrétní počítače, které nastal problém.  Potom můžete vytvářet [dotaz na data Log Analytics](../log-analytics/log-analytics-log-search-new.md) identifikovat problém v budoucnosti a vytvořit pravidlo výstrahy pro proaktivně vás upozornit, když byla zjištěna podmínku.



## <a name="next-steps"></a>Další postup
Další informace o

* [Azure monitorování v videa z Ignite 2016](https://myignite.microsoft.com/videos/4977)
* [Začínáme s Azure monitorování](monitoring-get-started.md)
* [Azure Diagnostics](../azure-diagnostics.md) Pokud se pokoušíte diagnostikovat problémy s cloudové služby, virtuální počítač, virtuální počítač škálování aplikace Fabric nastaveny nebo služby.
* [Application Insights](https://azure.microsoft.com/documentation/services/application-insights/) Pokud se pokoušíte diagnostiky problémů v aplikaci služby webové aplikace.
* [Analýza protokolu](https://azure.microsoft.com/documentation/services/log-analytics/) pro analýzu shromážděná data monitorování.
