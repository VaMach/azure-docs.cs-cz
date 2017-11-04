---
title: Konfigurace zdroje dat v OMS Log Analytics | Microsoft Docs
description: "Zdroje dat zadat data, analýzy protokolů shromažďuje z agentů a dalších připojené zdroje.  Tento článek popisuje základní informace o tom, jak analýzy protokolů používá zdroje dat, vysvětluje podrobnosti o tom, jak je nakonfigurovat a poskytuje k dispozici různé datové zdroje."
services: log-analytics
documentationcenter: 
author: bwren
manager: carmonm
editor: tysonn
ms.assetid: 67710115-c861-40f8-a377-57c7fa6909b4
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/23/2017
ms.author: bwren
ms.openlocfilehash: 00d030a502cf70ea9a5dea767f560cdf2919573e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="data-sources-in-log-analytics"></a>Zdroje dat v analýzy protokolů
Analýzy protokolů shromažďuje data z připojené zdroje v pracovním prostoru OMS a ukládá je v úložišti OMS.  Data, která se shromažďují z každé je definována zdroje dat, který nakonfigurujete.  Data v úložišti OMS se ukládají jako sada záznamů.  Všechny zdroje dat, vytvoří záznamy určitého typu pomocí jednotlivých typů má svou vlastní sadu vlastností.

![Přihlaste se shromažďování dat Analytics](./media/log-analytics-data-sources/overview.png)

Zdroje dat se liší od OMS řešení, které také shromažďovat data z připojené zdroje a vytvářet záznamy v úložišti OMS.  Řešení můžete přidat do pracovního prostoru z Galerie řešení a obvykle poskytne nástrojů pro další analýzu na portálu OMS.  

## <a name="summary-of-data-sources"></a>Souhrn datových zdrojů
Zdroje dat, které jsou aktuálně k dispozici v analýzy protokolů jsou uvedeny v následující tabulce.  Každý má odkaz na samostatný článek poskytuje podrobnosti pro tento zdroj dat.

| Zdroj dat | Typ události | Popis |
|:--- |:--- |:--- |
| [Vlastní protokoly](log-analytics-data-sources-custom-logs.md) |\<Název protokolu\>_CL |Textové soubory v systému Windows nebo Linux agenty obsahujícího informace o protokolu. |
| [Protokoly událostí systému Windows](log-analytics-data-sources-windows-events.md) |Událost |Události shromážděné z protokolu událostí na počítačích s Windows. |
| [Čítače výkonu systému Windows](log-analytics-data-sources-performance-counters.md) |Výkonu |Čítače výkonu shromážděných z počítače se systémem Windows. |
| [Čítače výkonu systému Linux](log-analytics-data-sources-performance-counters.md) |Výkonu |Čítače výkonu shromážděné z počítače se systémem Linux. |
| [Protokoly IIS](log-analytics-data-sources-iis-logs.md) |W3CIISLog |Internetová informační služba protokoly ve formátu W3C. |
| [Syslog](log-analytics-data-sources-syslog.md) |Syslog |Události procesu Syslog na počítačích systému Windows nebo Linux. |

## <a name="configuring-data-sources"></a>Konfigurace zdroje dat
Konfigurace zdroje dat z **Data** nabídky v analýzy protokolů **nastavení**.  Jakákoli konfigurace se doručí na všech připojených zdrojů v pracovním prostoru OMS.  Veškeré agenty nelze aktuálně vyloučit z této konfigurace.

![Konfigurace události systému Windows](./media/log-analytics-data-sources/configure-events.png)

1. V konzole OMS klikněte na **nastavení** dlaždici nebo **nastavení** tlačítka v horní části obrazovky.
2. Vyberte **Data**.
3. Klikněte na zdroj dat konfigurace.
4. Použijte odkaz na dokumentaci pro každý zdroj dat v tabulce Podrobnosti na jejich konfiguraci.

> [!NOTE]
> Momentálně nelze nakonfigurovat analýzy protokolů zdroje dat na portálu Azure.

## <a name="data-collection"></a>Shromažďování dat
Konfigurace zdroje dat se dodávají s agenty, které jsou připojeny přímo k Log Analytics během několika minut.  Zadaná data se shromažďují z agenta a doručeny přímo k Log Analytics v intervalech, které jsou specifické pro každý zdroj dat.  Najdete v dokumentaci pro každý zdroj dat pro tyto konkrétní.

Konfigurace zdroje dat pro System Center Operations Manager (SCOM) agenty v připojené skupiny pro správu, jsou přeložit na sady management Pack a doručit do skupiny pro správu každých 5 minut, ve výchozím nastavení.  Agent soubory ke stažení sady management pack jako libovolný jiný a shromažďuje zadaná data. V závislosti na zdroji dat, že buď odeslána na server pro správu, který předává data k analýze protokolů budou data nebo bude agent posílat data k analýze protokolů bez průchodu přes server pro správu. Odkazovat na [podrobnosti kolekce dat pro OMS funkcí a řešení](log-analytics-add-solutions.md#data-collection-details) podrobnosti.  Další informace o podrobnosti o připojení SCOM a OMS a úprava frekvence tato konfigurace je dodána na [konfigurace integrace s nástrojem System Center Operations Manager](log-analytics-om-agents.md).

Pokud agenta nemůže připojit k analýze protokolů nebo Operations Manager, bude pokračovat ke shromažďování dat, která bude poskytovat, když se naváže připojení.  Data mohou být ztracena, pokud objem dat dosáhne maximální velikost mezipaměti klienta, nebo pokud agent není schopen navázat připojení do 24 hodin.

## <a name="log-analytics-records"></a>Záznamy služby Log Analytics
Všechna data shromažďovaná společností analýzy protokolů je uložen v úložišti OMS jako záznamy.  Zaznamenává shromážděné z různých zdrojů dat. bude mít vlastní sadu vlastností a identifikovat podle jejich **typ** vlastnost.  Najdete v dokumentaci pro každý zdroj dat a řešení podrobnosti pro každý typ záznamu.

## <a name="next-steps"></a>Další kroky
* Další informace o [řešení](log-analytics-add-solutions.md) , přidání funkce do analýzy protokolů a také shromažďovat data do úložiště OMS.
* Další informace o [protokolu hledání](log-analytics-log-searches.md) analyzovat data shromážděná ze zdrojů dat a řešení.  
* Konfigurace [výstrahy](log-analytics-alerts.md) jako proaktivně upozornění na kritický data shromážděná ze zdrojů dat a řešení.
