---
title: "Protokol výstrah v monitorování Azure – výstrahy (Preview) | Microsoft Docs"
description: "Aktivační událost e-mailů, oznámení, adresy URL weby volání (webhooky) nebo automatizace při splnění zadané podmínky komplexní dotazu pro výstrahy Azure (Preview)."
author: msvijayn
manager: kmadnani1
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: f7457655-ced6-4102-a9dd-7ddf2265c0e2
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/02/2018
ms.author: vinagara
ms.openlocfilehash: f6072e4e8a9ab72f677c35e498e31b5218579f1b
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/09/2018
---
# <a name="log-alerts-in-azure-monitor---alerts-preview"></a>Protokol výstrah v monitorování Azure – výstrahy (Preview)
Tento článek poskytuje podrobné informace o tom, jak výstrahy pravidla v pracovní dotazy Analytics ve výstrahách Azure (Preview) a popisuje rozdíly mezi různé typy protokolu pravidla výstrah.

Aktuálně výstrahy Azure (Preview), podporuje protokolu výstrahy na dotazy z [Azure Log Analytics](../log-analytics/log-analytics-tutorial-viewdata.md) a [Application Insights](../application-insights/app-insights-cloudservices.md#view-azure-diagnostic-events).

> [!WARNING]

> V současné době protokolu výstrahy ve výstrahách Azure (Preview) nepodporuje dotazů mezi prostoru nebo napříč aplikacemi.

## <a name="log-alert-rules"></a>Pravidla výstrah protokolu

Výstrahy jsou vytvářeny automaticky v pravidelných intervalech spouštět dotazy protokolu výstrahy Azure (Preview).  Pokud výsledky dotazu protokolu konkrétním kritériím, se vytvoří záznam výstrahy. Pravidlo může automaticky spusťte jednu nebo více akcí proaktivně oznámíme vám výstrahy nebo vyvolání jiný proces, jako je spuštění sady runbook, pomocí [skupiny akcí](monitoring-action-groups.md).  Různé typy pravidla výstrah pomocí různých logiku k provedení této analýze.

Pravidla výstrah jsou definovány následující podrobnosti:

- **Protokolu dotazu**.  Dotaz, který se spustí pokaždé, když se aktivuje pravidlo výstrahy.  Vrácené tímto dotazem záznamy se používají k určení, jestli se má vytvořit výstrahu.
- **Časový interval**.  Určuje časový rozsah pro dotaz.  Dotaz vrátí pouze záznamy, které byly vytvořeny v tomto rozsahu aktuální čas.  Časový interval může být libovolná hodnota mezi 5 minut a 1 440 minut nebo 24 hodin. Například pokud časový interval je nastavena na 60 minut a spuštění dotazu: 15: 00, je vrácena pouze záznamy vytvořené 12:15:00 až 1:15 hodin.
- **Frekvence**.  Určuje, jak často se má spustit dotaz. Může být libovolná hodnota 5 minut až 24 hodin. Musí být rovna nebo menší než časový interval.  Pokud hodnota je větší než časový interval, riskujete záznamů je vynechán.<br>Představte si třeba časové okno 30 minut a četnost 60 minut.  Pokud je v 1:00 spustit dotaz, vrátí záznamy 12:30 až 1:00 PM.  Při příštím spuštění dotazu by je 2:00, když měla by vrátit záznamy 1:30 až 2:00.  Všechny záznamy vytvořené 1:00 až 1:30 by nikdy vyhodnotí.
- **Prahová hodnota**.  Výsledky hledání protokolů se vyhodnocují k určení, zda má být vytvořena výstraha.  Prahová hodnota se liší pro různé typy pravidla výstrah.

Každé pravidlo výstrahy v analýzy protokolů je jedním ze dvou typů.  Každý z těchto typů je podrobně popsány v následujících částech.

- **[Počet výsledků](#number-of-results-alert-rules)**. Vytvoří, když počet záznamů protokolu nalezené překračuje zadaný počet jedna výstraha.
- **[Metriky měření](#metric-measurement-alert-rules)**.  Výstraha byla vytvořena pro každý objekt ve výsledcích hledání protokolu s hodnotami, které překročí zadanou prahovou hodnotu.

Rozdíl mezi typy pravidlo výstrahy se následujícím způsobem.

- **Počet výsledků** pravidlo výstrahy vytvoří vždy jeden výstrahy chvíli **metriky měření** pravidlo výstrahy vytvoří výstrahu pro každý objekt, který překračuje prahovou hodnotu.
- **Počet výsledků** pravidla výstrah vytvářejí výstrahu, když je prahová hodnota počtu současně. **Metriky měření** pravidla výstrah může vytvořit upozornění, pokud je překročena prahová hodnota počtu časy v konkrétním časovém intervalu.

## <a name="number-of-results-alert-rules"></a>Počet výsledků pravidla výstrah
**Počet výsledků** pravidla výstrah vytvořit jednu výstrahu, když počet záznamů vrácených dotazem vyhledávání překročí zadanou prahovou hodnotu.

**Prahová hodnota**: prahová hodnota pro **počet výsledků** pravidlo výstrahy je větší nebo menší než určitou hodnotu.  Pokud počet záznamů protokolu nalezené splňují tato kritéria, se vytvoří výstraha.

### <a name="scenarios"></a>Scénáře

#### <a name="events"></a>Události
Tento typ pravidla výstrah je ideální pro práci s událostmi, jako je například protokol událostí systému Windows, Syslog, a vlastní protokoly.  Můžete vytvořit výstrahu, když se vytvoří událost konkrétní chyby, nebo když jsou v rámci konkrétní časové okno vytvořit více událostí chyby.

Chcete-li výstraha na jednu událost, nastavte počet výsledků na hodnotu větší než 0 a četnost i časové okno až pět minut.  Která se spustí dotaz každých pět minut a zkontrolujte výskyt jedna událost, která byla vytvořena od posledního spuštění dotazu.  Doba mezi událostí shromažďovaných a vytváří výstrahy může zdržet frekvencí delší.

Některé aplikace mohou být zaznamenány občasné chyby, který by neměl být nutně vygeneruje výstrahu.  Aplikace může například opakujte procesu, který vytvořil událost chyby a pak úspěšné příště.  V takovém případě nemusí budete chtít vytvořit výstrahu, pokud jsou v rámci konkrétní časové okno vytvořit více událostí.  

V některých případech můžete vytvořit výstrahu při absenci událost.  Tento proces se může například protokolu běžné události indikující, že funguje správně.  Pokud není protokolu jednu z těchto událostí v rámci konkrétní časové okno, je třeba vytvořit výstrahu.  V takovém případě by nastavit prahovou hodnotu **menší než 1**.

## <a name="metric-measurement-alert-rules"></a>Pravidla výstrah metriky měření

**Metriky měření** pravidla výstrah vytvářejí výstrahu pro každý objekt v dotazu s hodnotou, který je delší než zadaná prahová hodnota.  Mají odlišné následující rozdíly proti **počet výsledků** pravidla výstrah.

**Agregační funkce**: Určuje výpočet, který provádí a potenciálně číselného pole k agregaci.  Například **count()** vrátí počet záznamů v dotazu, **avg(CounterValue)** Vrátí průměrnou hodnotu pole přepočtené průběhu intervalu.

> [!NOTE]

> Agregační funkci v dotazu musí být s názvem volat: AggregatedValue a zadejte číselnou hodnotu.


**Pole Seskupit**: pro každou instanci v tomto poli se vytvoří záznam s hodnotou agregované a výstraha může vygenerovat pro každý.  Například pokud chcete generovat výstrahy pro každý počítač, byste použili **počítačem.**   

> [!NOTE]

> Metriky měření výstrah pravidla, které jsou založeny na Application Insights můžete zadat pole pro seskupení dat. Chcete-li to provést, použijte **agregační na** možnost v definici pravidla.   

**Interval**: definuje časový interval, za které agregovaná data.  Například pokud jste zadali **pět minut**, by se vytvoří záznam pro každou instanci pole skupiny v okně čas zadaný pro výstrahu agregován v intervalech 5 minut.

**Prahová hodnota**: prahová hodnota pro pravidla výstrah metriky měření je definována agregovaná hodnota a celou řadu.  Pokud žádné datového bodu v hledání protokolů překročí tuto hodnotu, považuje za porušení.  Pokud počet porušení v u všech objektů ve výsledcích překročí zadanou hodnotu, se vytvoří výstraha pro tento objekt.

#### <a name="example"></a>Příklad:
Vezměte v úvahu scénář, kde jste chtěli výstrahu překračování libovolného počítače využití procesoru 90 % třikrát více než 30 minut.  Vytvoříte pravidlo výstrahy s následujícími podrobnostmi:  

**Dotaz:** výkonu | kde ObjectName == "Procesor" a název_čítače == "% času procesoru" | shrnout AggregatedValue = avg(CounterValue) podle bin (TimeGenerated, 5 m), počítač<br>
**Časový interval:** 30 minut<br>
**Frekvence výstrah:** pět minut<br>
**Agregace hodnota:** skvělé než 90<br>
**Aktivační událost upozornění na základě:** celkem poruší větší než 5<br>

Dotaz by vytvořit průměrnou hodnotu pro každý počítač v intervalech 5 minut.  Tento dotaz by spustit každých 5 minut datech shromážděných za předchozí 30 minut.  Ukázková data jsou uvedené dole pro tři počítače.

![Ukázkové výsledky dotazu](../log-analytics/media/log-analytics-alerts/metrics-measurement-sample-graph.png)

V tomto příkladu by se vytvořit samostatné výstrahy pro srv02 a srv03, protože jejich nichž nebyla dodržena prahová hodnota 90 % 3krát přes časový interval.  Pokud **aktivační událost upozornění na základě:** byly změněny na **za sebou** výstrahu by vytvořen pouze pro srv03, od nichž nebyla dodržena prahová hodnota pro tři po sobě jdoucích vzorků.


## <a name="next-steps"></a>Další postup
* [Získat přehled o výstrahách Azure (Preview)](monitoring-overview-unified-alerts.md)
* Další informace o [pomocí výstrah Azure (preview)](monitor-alerts-unified-usage.md)
* Další informace o [analýzy protokolů](../log-analytics/log-analytics-overview.md).    
