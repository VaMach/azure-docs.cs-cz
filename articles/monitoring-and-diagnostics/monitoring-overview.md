---
title: "Monitorování aplikací Azure a prostředky | Microsoft Docs"
description: "Přehled služby společnosti Microsoft a funkce, které přispívají k dokončení monitorování strategii pro Azure služby a aplikace."
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
ms.openlocfilehash: d8da175a551f7c589c313b2289b2a0209dbd2b56
ms.sourcegitcommit: 782d5955e1bec50a17d9366a8e2bf583559dca9e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/02/2018
---
# <a name="monitoring-azure-applications-and-resources"></a>Monitorování aplikací Azure a prostředky

Sledování je v rámci shromažďování a analýzy dat o výkonu, stavu a dostupnosti obchodní aplikace a prostředky, které závisí na. Účinné strategie sledování vám pomůže pochopit podrobné činnost součástí vaší aplikace. Také pomáhá zvýšit vaši provozu proaktivně vás upozorní na zásadních problémů, aby předtím, než začnou způsobovat problémy, můžete je vyřešit.

Azure obsahuje více služeb, které jednotlivě provádět určité role nebo úkolu v prostoru pro monitorování. Společně tyto služby poskytovat komplexní řešení pro shromažďování, analýze a funguje na telemetrie z vaší aplikace a prostředky Azure, které je podporují. Může spolupracovat taky monitorovat kritické místních prostředků s cílem poskytnout hybridním monitorování prostředí. Pochopení nástroje a data, která jsou k dispozici je prvním krokem při vývoji dokončení strategie monitorování pro vaši aplikaci. 

Následující diagram znázorňuje koncepční zobrazení součásti, které společně poskytují monitorování prostředků Azure. Následující části popisují tyto součásti a obsahují odkazy na podrobné technické informace.

![Přehled monitorování](media/monitoring-overview/overview.png)

## <a name="basic-monitoring"></a>Základní monitorování
Základní monitorování poskytuje základní, vyžaduje monitorování napříč prostředků Azure. Tyto služby vyžadují minimální konfigurace a shromažďovat základní telemetrii, kterou monitorování služby premium používají.    

### <a name="azure-monitor"></a>Azure Monitor
[Azure monitorování](../monitoring-and-diagnostics/monitoring-overview-azure-monitor.md) umožňuje základní monitorování pro služby Azure tím, že kolekce [metriky](../monitoring-and-diagnostics/monitoring-overview-metrics.md), [protokoly aktivity](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md), a [diagnostické protokoly](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md). Například protokol aktivit zjistíte při vytvoření nebo úpravě nové prostředky. 

Metriky, které jsou k dispozici, zadejte statistiku výkonu pro různé prostředky a i operační systém uvnitř virtuálního počítače. Můžete zobrazit tato data s jedním z průzkumníci na portálu Azure, odešle Azure Log Analytics k analýze trendů a podrobné nebo vytvořit pravidla výstrah proaktivně upozornění kritických problémů.

### <a name="service-health"></a>Stav služby
Stav aplikace spoléhá na služby Azure, které závisí na. [Azure stavu služby](../service-health/service-health-overview.md) identifikuje všechny problémy se službami Azure, které mohou ovlivnit vaše aplikace. Stav služby také vám pomůže naplánovat plánované údržby.

### <a name="azure-advisor"></a>Azure Advisor
[Azure Advisor](../advisor/advisor-overview.md) neustále monitoruje telemetrie konfiguraci a využití prostředků. Potom nabízí přizpůsobené doporučení na základě osvědčených postupů. Těmito doporučeními pomáhá zvýšit výkon, zabezpečení a dostupnost prostředků, které podporují vaší aplikace.


## <a name="premium-monitoring-services"></a>Monitorování služby Premium
Následující služby Azure nabízí bohaté možnosti pro shromažďování a analýzy dat monitorování. Tyto služby stavět na základní monitorování a využít výhod běžné funkce v Azure. Poskytují výkonný analytics s shromážděná data tak, abyste získali jedinečný insights do své aplikace a infrastrukturu. Jejich prezentují data v rámci scénáře, které jsou cíleny na různé cílové skupiny.

### <a name="application-insights"></a>Application Insights
Můžete použít [Azure Application Insights](http://azure.microsoft.com/documentation/services/application-insights) ke sledování dostupnosti a výkonu a využití vaší aplikace, zda je hostovaná v cloudu nebo místně. 

Instrumentaci vaší aplikace pro práci s Application Insights, můžete dosáhnout informace hluboko skryté. Můžete rychle identifikovat a diagnostikovat chyby bez čekání na uživatele a jejich sestavy. S informacemi, které shromáždíte můžete provést informované volby na údržbu a vylepšení vaší aplikace. 

Application Insights obsahuje rozsáhlé nástroje pro interakci s daty, která shromažďuje. Application Insights ukládá data do úložiště v běžné. Může trvat využívat sdílené funkce jako je například výstrahy, řídicí panely a hloubkovou analýzu pomocí dotazovacího jazyka pro analýzy protokolů.

### <a name="log-analytics"></a>Log Analytics
[Analýza protokolu](http://azure.microsoft.com/documentation/services/log-analytics) hraje centrální role v Azure monitorování tím, že shromažďování dat z různých zdrojů do jednoho úložiště. Zde můžete analyzovat data pomocí účinný dotazovací jazyk. 

Application Insights a Azure Security Center ukládají data v analýzy protokolů data uložit a použít jeho analytics motoru. Data jsou data shromážděná z Azure monitorování, řešení pro správu a agentů nainstalovaných na virtuálních počítačů v cloudu nebo místně. Tato funkce sdílené umožňuje formuláři úplný přehled o svém prostředí. 


### <a name="service-map"></a>Mapa služeb
[Mapa služeb](../operations-management-suite/operations-management-suite-service-map.md) poskytuje vhled do prostředí IaaS analýzou virtuálních počítačů s jejich různé procesy a závislosti na jiných počítačích a externí procesy. Integruje se službou události, údaje o výkonu a řešení pro správu v analýzy protokolů. Potom můžete zobrazit tato data v rámci každého počítače a jejich vztah se zbytkem prostředí. 

Mapa služeb je podobný [mapy aplikací ve službě Application Insights](../application-insights/app-insights-app-map.md). Zaměřuje se na součásti infrastruktury, které podporují vaší aplikace.

### <a name="network-watcher"></a>Network Watcher
[Sledovací proces sítě](../network-watcher/network-watcher-monitoring-overview.md) poskytuje na základě scénáře monitorování a Diagnostika pro jinou síť scénáře v Azure. Ukládá data do Azure metriky a diagnostiky k další analýze. Funguje s následující řešení pro monitorování různých aspektů vaší síti:
* [Sítě monitorování výkonu (NPM)](https://blogs.msdn.microsoft.com/azuregov/2017/09/05/network-performance-monitor-general-availability/): síť cloudové řešení monitorování, které monitoruje připojení mezi veřejných cloudů, datovými centry a místními prostředími.
* [Monitorování ExpressRoute](https://azure.microsoft.com/en-in/blog/monitoring-of-azure-expressroute-in-preview/): NPM funkci, která monitoruje připojení klient server a výkonu u okruhu Azure ExpressRoute.
* Analýza provozu: Cloudového řešení, které umožní získat přehled o činnosti uživatelů a aplikací ve vaší síti cloudu.
* [Analýza DNS](https://docs.microsoft.com/en-us/azure/log-analytics/log-analytics-dns): řešení, které poskytuje zabezpečení, výkon a operace související přehledy založené na vaše servery DNS.

### <a name="management-solutions"></a>Řešení pro správu
[Řešení pro správu](../log-analytics/log-analytics-add-solutions.md) jsou zabalené sady logiky, které nabízejí přehled pro konkrétní aplikaci nebo službě. Spoléhají na analýzy protokolů ukládat a analyzovat data monitorování, který budou shromažďovat. 

Řešení pro správu jsou k dispozici od společnosti Microsoft a partneři poskytovat monitorování pro různé Azure a služby třetích stran. Příklady monitorování řešení:
* [Kontejner monitorování](../log-analytics/log-analytics-containers.md), který umožňuje zobrazit a spravovat hostitele kontejneru.
* [Azure SQL Analytics](../log-analytics/log-analytics-azure-sql.md), který shromažďuje a vizualizuje metriky výkonu u databází Azure SQL.


## <a name="shared-functionality"></a>Sdílené funkce
Následující nástroje Azure poskytují důležité funkce Premium monitorování služeb. Více služeb sdílet, tak můžete využít výhod společných funkcí a konfigurace ve více službách.

### <a name="alerts"></a>Výstrahy
[Azure výstrahy](../monitoring-and-diagnostics/monitoring-overview-alerts.md) proaktivně oznámíme vám kritického stavu a potenciálně proveďte opravné akce. Pravidla výstrah, můžete použít data z více zdrojů, včetně metrik a protokoly. Používají [skupiny akcí](../monitoring-and-diagnostics/monitoring-action-groups.md), které obsahují jedinečnou sadu akcí v reakci na oznámení a příjemce. Podle potřeb, může mít výstrahy spusťte externí akce pomocí webhooků a integrovat pomocí nástrojů pro ITSM.

### <a name="dashboards"></a>Řídicí panely
Můžete použít [Azure řídicí panely](../azure-portal/azure-portal-dashboards.md) kombinovat různé druhy dat do jednoho podokně na portálu Azure. Řídicí panel pak můžete sdílet s jinými uživateli Azure. 

Můžete například vytvořit řídicí panel, který kombinuje:
- Dlaždice, které se zobrazí graf metrik
- Tabulku protokoly aktivity
- Graf využití z Application Insights
- Výstup hledání protokolů v analýzy protokolů

Můžete také exportovat data Log Analytics [Power BI](https://docs.microsoft.com/power-bi/). Zde můžete využít další vizualizace. Můžete provést také data pro ostatní dostupný v rámci i mimo vaši organizaci.

### <a name="metrics-explorer"></a>Průzkumník metrik
[Metriky](../monitoring-and-diagnostics/monitoring-overview-metrics.md) jsou číselné hodnoty generované prostředek služby Azure, které vám pomohou pochopit provoz a výkonu prostředku. Pomocí Průzkumníku metrik, můžete odeslat metriky k analýze protokolů pro analýzu s daty z jiných zdrojů.



### <a name="activity-log"></a>Protokol aktivit
[Protokol aktivit](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md) poskytuje data o operaci prostředek služby Azure. Tyto informace zahrnují:
- Změny konfigurace do prostředku.
- Služba stavu incidentů.
- Doporučení pro lepší využití prostředku.
- Informace týkající se operací škálování. 

Protokoly pro určitý prostředek můžete zobrazit na stránce s jejím na portálu Azure. Nebo protokoly z více prostředků můžete zobrazit v Průzkumníku protokolu aktivit. 

Můžete také odeslat protokoly aktivity k analýze protokolů. Zde můžete analyzovat protokolů pomocí data shromažďovaná společností řešení pro správu, agenty na virtuální počítače a další zdroje.


## <a name="example-scenarios"></a>Ukázkové scénáře
Následují příklady vysoké úrovně, které ilustrují, jak můžete použít jiné nástroje pro sledování v Azure pro různé scénáře.

### <a name="monitoring-a-web-application"></a>Monitorování webové aplikace
Vezměte v úvahu webové aplikace nasazené v Azure pomocí služby Azure App Service, Azure Storage a databázi SQL. Spuštění díky přístupu k [metriky](../monitoring-and-diagnostics/monitoring-overview-metrics.md) a [protokoly aktivity](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md) pro tyto prostředky na svých stránkách portálu Azure. Vyhledejte důležitých informací, jako je počet požadavků v aplikaci a průměrná doba odezvy. Můžete také identifikovat změny konfigurace.

Pak přejdete na monitorování na portálu Chcete-li zobrazit protokoly pro různé zdroje a metriky společně. Určení standardní parametry pro metriku, které [vytvořit pravidla výstrah](../monitoring-and-diagnostics/monitoring-overview-unified-alerts.md). Tato pravidla proaktivně vás informují, například průměrná doba odezvy zvýší nad prahovou hodnotou. Chcete-li získat rychlý přehled o denní výkon aplikace, můžete vytvořit řídicí panel Azure zobrazit grafy metriky, které představují důležité klíčových ukazatelů výkonu.

K provedení hlubší monitorování aplikace, můžete [ho nakonfigurovat pro službu Application Insights](../application-insights/quick-monitor-portal.md). Nyní můžete shromažďovat další data, která poskytuje další aspekty provoz a výkonu vaší aplikace. Application Insights zjišťuje základní vztahy mezi součástmi vaší aplikace. To umožňuje pro vizuální reprezentace prostřednictvím [aplikace mapy](../application-insights/app-insights-app-map.md) kombinaci s [začátku do konce trasování](../application-insights/app-insights-transaction-diagnostics.md) při diagnostice přesný součásti, závislostí nebo výjimky, kde došlo k potížím. 

Vytvoříte [testy dostupnosti](../application-insights/app-insights-monitor-web-app-availability.md) proaktivně testování vaší aplikace z různých oblastech. Chcete-li pomoci vývojářům, můžete [povolit profileru](../application-insights/enable-profiler-compute.md) , můžete sledovat žádosti a jakékoli výjimky na konkrétní řádek kódu. Chcete-li získat další přehled o služeb používaných ve vaší aplikaci, přidejte [řešení SQL analýzy](../log-analytics/log-analytics-azure-sql.md) shromažďovat další data v analýzy protokolů. 

Po určité době rozhodnete zjistěte základní příčinu dobu, kdy výkonu na webu klesla pod prahovou hodnotu. Můžete napsat dotaz pomocí analýzy protokolů. Pomáhá vazbu mezi využití a výkonu data shromážděná pomocí Application Insights s konfigurací a údaje o výkonu v prostředky Azure, které podporují aplikace.


### <a name="monitoring-virtual-machines"></a>Monitorování virtuálních počítačů
Máte směs Windows a Linux virtuální počítače běžící v Azure. Pomocí Azure monitorování zobrazíte [protokoly aktivity](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md) a [metriky na úrovni hostitele](../monitoring-and-diagnostics/monitoring-overview-metrics.md). Můžete přidat [rozšíření Azure Diagnostics](../virtual-machines/linux/tutorial-monitoring.md#install-diagnostics-extension) k virtuálním počítačům, aby bylo možné shromažďovat metriky ze hostovaného operačního systému. Potom můžete vytvářet [výstrah pravidla](../monitoring-and-diagnostics/monitoring-overview-unified-alerts.md) proaktivně upozornění v případě základní metriky, jako je využití procesoru a paměti mezi prahové hodnoty.

Shromažďování podrobných informací o virtuálních počítačů spuštěných obchodní aplikace, můžete [vytvořit pracovní prostor analýzy protokolů a povolit rozšíření virtuálního počítače](../log-analytics/log-analytics-quick-collect-azurevm.md) na každém počítači. Můžete nakonfigurovat [kolekce různých datových zdrojů](../log-analytics/log-analytics-data-sources.md) pro vaši aplikaci a [vytvořte zobrazení](../log-analytics/log-analytics-view-designer.md) chcete sestavu podle jeho každodenní operace a výkonu. Potom můžete [vytvořit pravidla výstrah](../monitoring-and-diagnostics/monitoring-overview-unified-alerts.md) upozornění v případě přijetí události určité chybě. 

Neustále monitorovat stav nainstalovaného agenta, přidejte [řešení pro správu agenta stavu](../operations-management-suite/oms-solution-agenthealth.md). Chcete-li získat další přehled o aplikaci, můžete [Přidat závislost agenta](../operations-management-suite/operations-management-suite-service-map-configure.md) k virtuálním počítačům, aby bylo možné přidat je do [mapy služeb](../operations-management-suite/operations-management-suite-service-map.md). Mapa služeb zjistí důležité procesy a identifikuje připojení mezi počítači s jinými službami. 

Po výpadku hlášené pomocí mapy služeb provádět forenzní můžete identifikovat konkrétní počítače, které nastal problém. Potom můžete vytvářet [dotaz na data Log Analytics](../log-analytics/log-analytics-log-search-new.md) k identifikaci problému v budoucnosti. A vytvořte pravidlo výstrahy pro proaktivně vás upozornit, když je zjištěna podmínku.



## <a name="next-steps"></a>Další postup
Další informace o:

* [Azure monitorování v videa z Ignite 2016](https://myignite.microsoft.com/videos/4977).
* [Začínáme s Azure monitorování](monitoring-get-started.md).
* [Azure Diagnostics](../azure-diagnostics.md) Pokud se pokoušíte diagnostikovat problémy s cloudovou službu, virtuální počítač, škálovací sadu virtuálních počítačů nebo aplikace Azure Service Fabric.
* [Application Insights](https://azure.microsoft.com/documentation/services/application-insights/) Pokud se snažíte diagnostikovat problémy ve vaší webové aplikaci služby App Service.
* [Analýza protokolu](https://azure.microsoft.com/documentation/services/log-analytics/) pro analýzu shromážděná data monitorování.
