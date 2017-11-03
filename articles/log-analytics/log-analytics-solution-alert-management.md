---
title: "Výstrahy řešení pro správu v Operations Management Suite (OMS) | Microsoft Docs"
description: "Řešení pro správu výstrah v analýzy protokolů umožňuje analyzovat všechny výstrahy ve vašem prostředí.  Kromě konsolidace výstrahy generované v rámci OMS, se importuje výstrahy z připojených skupin pro správu System Center Operations Manager do analýzy protokolů."
services: log-analytics
documentationcenter: 
author: bwren
manager: jwhit
editor: tysonn
ms.assetid: fe5d534e-0418-4e2f-9073-8025e13271a8
ms.service: operations-management-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/13/2017
ms.author: bwren
ms.openlocfilehash: 4a394ead09794c52e92dbd2db8be8166d1fab83b
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="alert-management-solution-in-operations-management-suite-oms"></a>Výstrahy řešení pro správu v Operations Management Suite (OMS)

![Ikona správy výstrah](media/log-analytics-solution-alert-management/icon.png)

Řešení pro správu výstrah pomáhá analyzovat všechny výstrahy v úložiště analýzy protokolů.  Tyto výstrahy mohou mít pocházet z různých zdrojů, včetně těchto zdrojů [vytvořené analýzy protokolů](log-analytics-alerts.md) nebo [importovat z Nagios nebo Zabbix](log-analytics-linux-agents.md).  Řešení také importuje výstrahy z libovolného [připojené skupiny pro správu System Center Operations Manager](log-analytics-om-agents.md).

## <a name="prerequisites"></a>Požadavky
Toto řešení funguje se všechny záznamy v úložišti analýzy protokolů s typem **výstrahy**, takže je třeba provést, ať konfigurace je potřeba shromáždit tyto záznamy.

- Pro analýzy protokolů výstrahy [vytvořit pravidla výstrah](log-analytics-alerts.md) vytvářet výstrahy záznamy přímo v úložišti.
- Pro výstrahy Nagios a Zabbix [konfiguraci těchto serverů](log-analytics-linux-agents.md) k odesílání upozornění k analýze protokolů.
- Pro System Center Operations Manager výstrahy [připojte svoji skupinu správy nástroje Operations Manager do pracovního prostoru analýzy protokolů](log-analytics-om-agents.md).  Všechny výstrahy vytvořené v nástroji System Center Operations Manager jsou importovány do analýzy protokolů.  

## <a name="configuration"></a>Konfigurace
Přidat do řešení pro správu výstrah do pracovního prostoru OMS pomocí procesu popsaného v tématu [přidat řešení](log-analytics-add-solutions.md).  Není nutná žádná další konfigurace.

## <a name="management-packs"></a>Sady Management Pack
Pokud vaše skupina pro správu System Center Operations Manager je připojený k vaším pracovním prostorem OMS, jsou při přidání tohoto řešení následující sady management Pack nainstalované v System Center Operations Manager.  Neexistuje žádná konfigurace ani údržby sad management Pack vyžaduje.  

* Microsoft System Center Advisor správu výstrah (Microsoft.IntelligencePacks.AlertManagement)

Další informace o způsobu, jakým se aktualizují sady pro správu řešení, najdete v tématu [Připojení Operations Manageru ke službě Log Analytics](log-analytics-om-agents.md).

## <a name="data-collection"></a>Shromažďování dat
### <a name="agents"></a>Agenti
Následující tabulka popisuje připojené zdroje, které toto řešení podporuje.

| Připojený zdroj | Podpora | Popis |
|:--- |:--- |:--- |
| [Agenti systému Windows](log-analytics-windows-agents.md) | Ne |Přímé agenty se systémem Windows negenerují výstrahy.  Log Analytics výstrahy lze vytvořit na základě události a údaje o výkonu shromážděných ze systému Windows agenty. |
| [Agenti systému Linux](log-analytics-linux-agents.md) | Ne |Přímé agenty Linux negenerují výstrahy.  Log Analytics výstrahy mohou být vytvořeny z události a data výkonu shromážděných z agentů systému Linux.  Nagios a Zabbix výstrahy se shromažďují z těchto serverů, které vyžadují agenta systému Linux. |
| [Skupina pro správu System Center Operations Manager](log-analytics-om-agents.md) |Ano |Výstrahy, které jsou vytvořeny na agenty nástroje Operations Manager jsou doručeny do skupiny pro správu a pak předají k analýze protokolů.<br><br>Přímé připojení z agenty nástroje Operations Manager k analýze protokolů se nevyžaduje. Do úložiště analýzy protokolů se předají výstrahy data ze skupiny pro správu. |


### <a name="collection-frequency"></a>Četnost shromažďování dat
- Výstrahy záznamy jsou k dispozici řešení, jakmile jsou uložené v úložišti.
- Výstrahy odeslání dat ze skupiny pro správu nástroje Operations Manager k analýze protokolů každé tři minuty.  

## <a name="using-the-solution"></a>Použití řešení
Když přidáte do řešení pro správu výstrah do pracovního prostoru OMS **správu výstrah** dlaždice se přidá na řídicí panel OMS.  Tuto dlaždici zobrazí počet a grafické znázornění počet aktuálně aktivních výstrah, které byly vygenerovány během posledních 24 hodin.  Tento časový rozsah nelze změnit.

![Dlaždice výstrah správy](media/log-analytics-solution-alert-management/tile.png)

Klikněte na **správu výstrah** dlaždici otevřete **správu výstrah** řídicí panel.  Řídicí panel obsahuje sloupce v následující tabulce.  Každý sloupec uvádí top 10 výstrahy podle počtu odpovídající kritériím tento sloupec pro zadaný obor a časový rozsah.  Můžete spustit hledání protokolu, které poskytuje celý seznam kliknutím **zobrazit všechny** v dolní části sloupce nebo kliknutím na záhlaví sloupce.

| Sloupec | Popis |
|:--- |:--- |
| Kritické výstrahy |Všechny výstrahy se závažností kritický seskupené podle název výstrahy.  Klikněte na název výstrahy. ke spuštění vyhledávání protokolu vrátit všechny záznamy pro tuto výstrahu. |
| Varovné výstrahy. |Všechny výstrahy se závažností upozornění seskupené podle název výstrahy.  Klikněte na název výstrahy. ke spuštění vyhledávání protokolu vrátit všechny záznamy pro tuto výstrahu. |
| Aktivní SCOM výstrahy |Všechny výstrahy shromážděné z nástroje Operations Manager pomocí nějaký stav, jiné než *uzavřeno* seskupené podle zdroje, které výstrahu vygenerovalo. |
| Všechny aktivní výstrahy |Všechny výstrahy s všechny závažnosti seskupené podle název výstrahy. Zahrnuje výstrahy nástroje Operations Manager s jakýkoli stav jenom jiné než *uzavřeno*. |

Pokud jste se posuňte doprava, řídicí panel uvádí několik běžných dotazů, které můžete provést kliknutím na [hledání protokolů](log-analytics-log-searches.md) pro dat výstrah.

![Výstrahy řídicí panel správy](media/log-analytics-solution-alert-management/dashboard.png)


## <a name="log-analytics-records"></a>Záznamy služby Log Analytics
Řešení pro správu výstrah analyzuje všech záznamů s typem **výstrahy**.  Výstrahy vytvořené analýzy protokolů nebo shromážděných ze Nagios nebo Zabbix nejsou shromážděna přímo v řešení.

Řešení importovat výstrahy z nástroje System Center Operations Manager a vytvoří odpovídající záznam pro každý typ **výstrahy** a SourceSystem z **OpsManager**.  Tyto záznamy mají vlastnosti v následující tabulce:  

| Vlastnost | Popis |
|:--- |:--- |
| Typ |*Výstrahy* |
| SourceSystem |*OpsManager* |
| AlertContext |Podrobnosti o datová položka, která způsobila výstrahu, kterou chcete vygenerovat ve formátu XML. |
| AlertDescription |Podrobný popis výstrahy. |
| AlertId |Identifikátor GUID výstrahy. |
| AlertName |Název výstrahy. |
| AlertPriority |Úroveň priority výstrahy. |
| AlertSeverity |Úroveň závažnosti výstrahy. |
| AlertState |Nejnovější stav řešení výstrahy. |
| LastModifiedBy |Jméno uživatele, který poslední změny výstrahy. |
| ManagementGroupName |Název skupiny pro správu kde byla výstraha vygenerována. |
| RepeatCount |Kolikrát stejnou výstrahu vygenerovalo pro stejnou monitorovat objekt od přeloženy. |
| ResolvedBy |Jméno uživatele, který vyřešit výstrahu. Prázdný, pokud je výstraha dosud nebyl vyřešen. |
| SourceDisplayName |Zobrazovaný název objektu monitorování, které výstrahu vygenerovalo. |
| SourceFullName |Úplný název objektu monitorování, které výstrahu vygenerovalo. |
| TicketId |ID lístku pro upozornění, pokud je prostředí nástroje System Center Operations Manager je integrovaná s procesem pro přidělování lístků pro výstrahy.  Prázdný žádné lístku ID je přiřazen. |
| TimeGenerated |Datum a čas vytvoření výstrahy. |
| TimeLastModified |Datum a čas poslední změny výstrahy. |
| TimeRaised |Datum a čas, který byla výstraha vygenerována. |
| TimeResolved |Datum a čas, který se vyřešil výstrahy. Prázdný, pokud je výstraha dosud nebyl vyřešen. |

## <a name="sample-log-searches"></a>Ukázky hledání v protokolech
Následující tabulka obsahuje ukázkový protokol hledání výstrah záznamů shromažďují toto řešení: 

| Dotaz | Popis |
|:--- |:--- |
| Typ = výstrahy SourceSystem = OpsManager AlertSeverity = chyby TimeRaised > nyní 24 hodin |Kritické výstrahy vyvolané během posledních 24 hodin |
| Typ = výstrahy AlertSeverity = upozornění TimeRaised > nyní 24 hodin |Upozorňující výstrahy vyvolané během posledních 24 hodin |
| Typ = výstrahy SourceSystem = OpsManager AlertState! = uzavřené TimeRaised > nyní 24 hodin &#124; míra count() jako počet podle SourceDisplayName |Zdroje s aktivními výstrahami vyvolanými během posledních 24 hodin |
| Typ = výstrahy SourceSystem = OpsManager AlertSeverity = chyby TimeRaised > nyní 24 hodin AlertState! = uzavřeno |Kritické výstrahy vyvolané během posledních 24 hodin, které jsou stále aktivní |
| Typ = výstrahy SourceSystem = OpsManager TimeRaised > nyní 24 hodin AlertState = uzavřený |Výstrahy vyvolané během posledních 24 hodin, které už jsou uzavřené |
| Typ = výstrahy SourceSystem = OpsManager TimeRaised > nyní - 1 den &#124; míra count() jako počet podle AlertSeverity |Výstrahy vyvolané za poslední 1 den seskupené podle závažnosti |
| Typ = výstrahy SourceSystem = OpsManager TimeRaised > nyní - 1 den &#124; řazení RepeatCount desc |Výstrahy vyvolané za poslední 1 den seřazené podle počtu opakování |


>[!NOTE]
> Pokud pracovní prostor byl upgradován na verzi [nové analýzy protokolů dotazu jazyka](log-analytics-log-search-upgrade.md), pak předchozí dotazy by změnit na následující:
>
>| Dotaz | Popis |
|:---|:---|
| Upozornit &#124; kde SourceSystem == "OpsManager" a AlertSeverity == "Chyba" a TimeRaised > ago(24h) |Kritické výstrahy vyvolané během posledních 24 hodin |
| Upozornit &#124; kde AlertSeverity == "upozornění" a TimeRaised > ago(24h) |Upozorňující výstrahy vyvolané během posledních 24 hodin |
| Upozornit &#124; kde SourceSystem == "OpsManager" a AlertState! = "Uzavřeno" a TimeRaised > ago(24h) &#124; shrnout Count = count() podle SourceDisplayName |Zdroje s aktivními výstrahami vyvolanými během posledních 24 hodin |
| Upozornit &#124; kde SourceSystem == "OpsManager" a AlertSeverity == "Chyba" a TimeRaised > ago(24h) a AlertState! = "Uzavřeno" |Kritické výstrahy vyvolané během posledních 24 hodin, které jsou stále aktivní |
| Upozornit &#124; kde SourceSystem == "OpsManager" a TimeRaised > ago(24h) a AlertState == "Uzavřeno" |Výstrahy vyvolané během posledních 24 hodin, které už jsou uzavřené |
| Upozornit &#124; kde SourceSystem == "OpsManager" a TimeRaised > ago(1d) &#124; shrnout Count = count() podle AlertSeverity |Výstrahy vyvolané za poslední 1 den seskupené podle závažnosti |
| Upozornit &#124; kde SourceSystem == "OpsManager" a TimeRaised > ago(1d) &#124; Řadit podle RepeatCount desc |Výstrahy vyvolané za poslední 1 den seřazené podle počtu opakování |


## <a name="next-steps"></a>Další kroky
* Podrobnosti o generování upozornění ze služby Log Analytics najdete v tématu [Upozornění v Log Analytics](log-analytics-alerts.md).
