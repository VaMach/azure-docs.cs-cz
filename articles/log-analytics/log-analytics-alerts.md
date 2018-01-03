---
title: "Vysvětlení výstrah v Azure Log Analytics | Microsoft Docs"
description: "Výstrahy v Log Analytics identifikovat důležité informace ve svém úložišti OMS a můžete proaktivně upozorňují na problémy nebo vyvolání akce se pokusit o opravte je.  Tento článek popisuje různé druhy pravidla výstrah a jak jsou definovány."
services: log-analytics
documentationcenter: 
author: bwren
manager: jwhit
editor: tysonn
ms.assetid: 6cfd2a46-b6a2-4f79-a67b-08ce488f9a91
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/13/2017
ms.author: bwren
ms.openlocfilehash: a0897113660f764cb23239b066bc93c479a9a553
ms.sourcegitcommit: 6f33adc568931edf91bfa96abbccf3719aa32041
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/22/2017
---
# <a name="understanding-alerts-in-log-analytics"></a>Vysvětlení výstrah v analýzy protokolů

Výstrahy v Log Analytics identifikovat důležité informace do úložiště analýzy protokolů.  Tento článek obsahuje podrobnosti o tom, jak výstrahy pravidlech pracovního analýzy protokolů a popisuje rozdíly mezi různé typy pravidla výstrah.

Proces vytváření pravidla výstrah najdete v následujících článcích:

- Vytvořit pravidla výstrah pomocí [portálu Azure](log-analytics-alerts-creating.md)
- Vytvořit pravidla výstrah pomocí [šablony Resource Manageru](../operations-management-suite/operations-management-suite-solutions-resources-searches-alerts.md)
- Vytvořit pravidla výstrah pomocí [REST API](log-analytics-api-alerts.md)


## <a name="alert-rules"></a>Pravidla výstrah

Výstrahy jsou vytvářeny pravidla výstrah, které automaticky spustit vyhledávání protokolu v pravidelných intervalech.  Pokud výsledky hledání protokolů odpovídají konkrétním kritériím se vytvoří záznam výstrahy.  Pravidlo lze poté automaticky spouštět jednu nebo více akcí proaktivně oznámíme vám výstrahy nebo vyvolání jiný proces.  Různé typy pravidla výstrah pomocí různých logiku k provedení této analýze.

![Výstrahy Log Analytics](media/log-analytics-alerts/overview.png)

Pravidla výstrah jsou definovány následující podrobnosti:

- **Hledání protokolů**.  Dotaz, který se spustí pokaždé, když se aktivuje pravidlo výstrahy.  Vrácené tímto dotazem záznamy slouží k určení, jestli se má vytvořit výstrahu.
- **Časový interval**.  Určuje časový rozsah pro dotaz.  Dotaz vrátí pouze záznamy, které byly vytvořeny v tomto rozsahu aktuální čas.  To může být libovolná hodnota 5 minut až 24 hodin. Například pokud časový interval je nastavena na 60 minut a spuštění dotazu: 15: 00, je vrácena pouze záznamy vytvořené 12:15:00 až 1:15 hodin.
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

### <a name="threshold"></a>Prahová hodnota
Prahová hodnota pro **počet výsledků** pravidlo výstrahy je jednoduše větší nebo menší než určitou hodnotu.  Pokud počet záznamů protokolu nalezené splňují tato kritéria, se vytvoří výstraha.

### <a name="scenarios"></a>Scénáře

#### <a name="events"></a>Události
Tento typ pravidla výstrah je ideální pro práci s událostmi, jako je například protokol událostí systému Windows, Syslog, a vlastní protokoly.  Můžete vytvořit výstrahu, když se vytvoří událost konkrétní chyby, nebo když jsou v rámci konkrétní časové okno vytvořit více událostí chyby.

Chcete-li výstraha na jednu událost, nastavte počet výsledků na hodnotu větší než 0 a četnost i časové okno na 5 minut.  Která se spustí dotaz každých 5 minut a zkontrolujte výskyt jedna událost, která byla vytvořena od posledního spuštění dotazu.  Doba mezi událostí shromažďovaných a vytváří výstrahy může zdržet frekvencí delší.

Některé aplikace mohou být zaznamenány občasné chyby, který by neměl být nutně vygeneruje výstrahu.  Aplikace může například opakujte procesu, který vytvořil událost chyby a pak úspěšné příště.  V takovém případě nemusí budete chtít vytvořit výstrahu, pokud jsou v rámci konkrétní časové okno vytvořit více událostí.  

V některých případech můžete vytvořit výstrahu při absenci událost.  Tento proces se může například protokolu běžné události indikující, že funguje správně.  Pokud není protokolu jednu z těchto událostí v rámci konkrétní časové okno, je třeba vytvořit výstrahu.  V takovém případě byste měli nastavit prahovou hodnotu na **menší než 1**.

#### <a name="performance-alerts"></a>Výstrahy výkonu
[Data výkonu](log-analytics-data-sources-performance-counters.md) se ukládají jako záznamy v úložišti OMS podobné události.  Pokud chcete upozornit, když čítače výkonu překračuje prahovou hodnotu konkrétní, by měl prahové hodnoty obsažen v dotazu.

Například, pokud chcete upozornit, když procesor běží víc než 90 %, byste použili dotazu jako následující s prahovou hodnotou pro pravidlo výstrahy **větší než 0**.

    Perf | where ObjectName=="Processor" and CounterName=="% Processor Time" and CounterValue>90

    

Pokud chcete upozornit, když procesor průměrem více než 90 % pro konkrétní časové okno, by použití dotazu jako následující s prahovou hodnotou pro pravidlo výstrahy **větší než 0**.

    Perf | where ObjectName=="Processor" and CounterName=="% Processor Time" | where CounterValue>90 | summarize avg(CounterValue) by Computer

    
>[!NOTE]
> Pokud pracovního prostoru ještě nebyla upgradována na [nové analýzy protokolů dotazu jazyka](log-analytics-log-search-upgrade.md), pak výše dotazy by změnit na následující pozdější prostřednictvím [měření příkaz](log-analytics-search-reference.md#commands):`Type=Perf ObjectName=Processor CounterName="% Processor Time" CounterValue>90`
> `Type=Perf ObjectName=Processor CounterName="% Processor Time" | measure avg(CounterValue) by Computer | where AggregatedValue>90`


## <a name="metric-measurement-alert-rules"></a>Pravidla výstrah metriky měření

>[!NOTE]
> Pravidla výstrah metriky měření jsou aktuálně ve verzi public preview.

**Metriky měření** pravidla výstrah vytvářejí výstrahu pro každý objekt v dotazu s hodnotou, který je delší než zadaná prahová hodnota.  Mají odlišné následující rozdíly proti **počet výsledků** pravidla výstrah.

#### <a name="log-search"></a>Prohledávání protokolů
I když můžete používat pro jakýkoli dotaz **počet výsledků** pravidlo výstrahy, existují požadavky na dotaz pro pravidlo výstrahy metriky měření.  Musí obsahovat [měření příkaz](log-analytics-search-reference.md#commands) seskupí výsledky podle určitého pole. Tento příkaz musí zahrnovat následující prvky.

- **Agregační funkce**.  Určuje výpočet, který provádí a potenciálně číselného pole k agregaci.  Například **count()** vrátí počet záznamů v dotazu, **avg(CounterValue)** Vrátí průměrnou hodnotu pole přepočtené průběhu intervalu.
- **Pole Seskupit**.  Pro každou instanci v tomto poli se vytvoří záznam s hodnotou agregované a výstraha může vygenerovat pro každý.  Například pokud chcete generovat výstrahy pro každý počítač, byste použili **počítačem**.   
- **Interval**.  Definuje časový interval, za které agregovaná data.  Například pokud jste zadali **5minutes**, by se vytvoří záznam pro každou instanci pole skupiny v okně čas zadaný pro výstrahu agregován v intervalech 5 minut.

#### <a name="threshold"></a>Prahová hodnota
Prahová hodnota pro pravidla výstrah metriky měření je definována agregovaná hodnota a celou řadu.  Pokud žádné datového bodu v hledání protokolů překročí tuto hodnotu, považuje za porušení.  Pokud počet porušení v u všech objektů ve výsledcích překročí zadanou hodnotu, se vytvoří výstraha pro tento objekt.

#### <a name="example"></a>Příklad:
Vezměte v úvahu scénář, kde jste chtěli výstrahu překračování libovolného počítače využití procesoru 90 % třikrát více než 30 minut.  Vytvoříte pravidlo výstrahy s následujícími podrobnostmi.  

**Dotaz:** výkonu | kde ObjectName == "Procesor" a název_čítače == "% času procesoru" | shrnout AggregatedValue = avg(CounterValue) podle bin (TimeGenerated, 5 m), počítač<br>
**Časový interval:** 30 minut<br>
**Frekvence výstrah:** 5 minut<br>
**Agregace hodnota:** větší než 90<br>
**Aktivační událost upozornění na základě:** celkem poruší větší než 2<br>

Dotaz by vytvořit průměrnou hodnotu pro každý počítač v intervalech 5 minut.  Tento dotaz by spustit každých 5 minut datech shromážděných za předchozí 30 minut.  Ukázková data jsou uvedené dole pro tři počítače.

![Ukázkové výsledky dotazu](media/log-analytics-alerts/metrics-measurement-sample-graph.png)

V tomto příkladu by se vytvořit samostatné výstrahy pro srv02 a srv03, protože jejich nichž nebyla dodržena prahová hodnota 90 % 3krát přes časový interval.  Pokud **aktivační událost upozornění na základě:** byly změněny na **za sebou** výstrahu by vytvořen pouze pro srv03, od nichž nebyla dodržena prahová hodnota pro 3 po sobě jdoucích vzorků.

## <a name="alert-records"></a>Výstrahy záznamů
Mít výstrahy záznamy vytvořené pravidla výstrah v analýzy protokolů **typ** z **výstraha** a **SourceSystem** z **OMS**.  V následující tabulce mají vlastnosti.

| Vlastnost | Popis |
|:--- |:--- |
| Typ |*Výstrahy* |
| SourceSystem |*OMS* |
| *Objekt*  | [Metriky měření výstrahy](#metric-measurement-alert-rules) bude mít vlastnost pro pole skupiny.  Například pokud protokol hledání skupin v počítači, výstrahy záznam s mít počítač pole s názvem počítače jako hodnotu.
| AlertName |Název výstrahy. |
| AlertSeverity |Úroveň závažnosti výstrahy. |
| LinkToSearchResults |Propojit vyhledávání protokolu analýzy protokolů, který vrátí záznamy v dotazu, který vytvořili výstrahu. |
| Dotaz |Text dotazu, který byl spuštěn. |
| QueryExecutionEndTime |Konec časový rozsah pro dotaz. |
| QueryExecutionStartTime |Začátek časový rozsah pro dotaz. |
| ThresholdOperator | Operátor, který byl používán pravidlo výstrahy. |
| ThresholdValue | Hodnota, která byla používána pro pravidlo výstrahy. |
| TimeGenerated |Datum a čas vytvoření výstrahy. |

Existují jiné typy záznamů výstrahy vytvořené [řešení pro správu výstrahu](log-analytics-solution-alert-management.md) a [Power BI exportuje](log-analytics-powerbi.md).  Tyto jsou vybavené **typ** z **výstrahy** , ale jsou rozlišené jejich **SourceSystem**.


## <a name="next-steps"></a>Další postup
* Nainstalujte [řešení pro správu výstrah](log-analytics-solution-alert-management.md) k analýze výstrahy vytvořené v analýzy protokolů společně s výstrahy shromážděných z System Center Operations Manager.
* Další informace o [protokolu hledání](log-analytics-log-searches.md) , mohou generovat výstrahy.
* Dokončete průvodce pro [konfigurace webhook, jehož](log-analytics-alerts-webhooks.md) s pravidlo výstrahy.  
* Další informace o zápisu [sady runbook ve službě Azure Automation](https://azure.microsoft.com/documentation/services/automation) k nápravě problémů identifikovaný výstrahy.
