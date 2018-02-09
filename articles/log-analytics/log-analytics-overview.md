---
title: "Co je služba Log Analytics v Azure? | Dokumenty Microsoft"
description: "Log Analytics je služba v Azure, která pomáhá shromažďovat a analyzovat provozní data vygenerovaná prostředky ve vašem cloudovém a místním prostředí.  Tento článek poskytuje stručný přehled různých komponent služby Log Analytics a odkazy na podrobný obsah."
services: log-analytics
documentationcenter: 
author: bwren
manager: jwhit
editor: tysonn
ms.assetid: bd90b460-bacf-4345-ae31-26e155beac0e
ms.service: log-analytics
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/24/2018
ms.author: bwren
ms.openlocfilehash: a95528f5bd259a36ea96c7bc0660ca082c09d6e6
ms.sourcegitcommit: 99d29d0aa8ec15ec96b3b057629d00c70d30cfec
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/25/2018
---
# <a name="what-is-log-analytics"></a>Co je služba Log Analytics?
Log Analytics je služba v Azure, která monitoruje cloudové a místní prostředí s cílem zachovat jejich dostupnost a výkon.  Shromažďuje data generovaná prostředky ve vašem cloudovém a místním prostředí a také data z dalších nástrojů pro monitorování a poskytuje analýzy napříč zdroji.  Tento článek obsahuje stručný popis toho, co služba Log Analytics poskytuje, přehled toho, jak funguje, a odkazy na podrobnější obsah, abyste ji mohli blíž prozkoumat.

## <a name="is-log-analytics-for-you"></a>Je pro vás služba Log Analytics vhodná?
Pokud ještě nemáte pro prostředí Azure nastavené žádné monitorování, měli byste začít se službou [Azure Monitor](../monitoring-and-diagnostics/monitoring-overview.md), která shromažďuje a analyzuje data monitorování prostředků Azure.  Log Analytics může [shromažďovat data z Azure Monitoru](log-analytics-azure-storage.md), dát je do souvislosti s ostatními daty a poskytovat další analýzy.

Pokud chcete monitorovat místní prostředí nebo už využíváte monitorování pomocí služeb, jako je Azure Monitor nebo System Center Operations Manager, může využití služby Log Analytics výrazně zlepšit situaci.  Může do jednoho úložiště shromažďovat data přímo z agentů a také z těchto dalších nástrojů.  Analytické nástroje služby Log Analytics, jako je prohledávání protokolu, zobrazení a řešení, využívají všechna shromážděná data a poskytují centralizovanou analýzu celého prostředí.


## <a name="using-log-analytics"></a>Použití Log Analytics
Pro přístup ke službě Log Analytics můžete využít web Azure Portal, který se dá otevřít v jakémkoli prohlížeči a který poskytuje přístup k nastavení konfigurace a několika nástrojům pro analýzy shromážděných dat a práci s nimi.  Na portálu můžete využívat [prohledávání protokolů](log-analytics-log-searches.md), kde je možné vytvářet dotazy pro analýzy shromážděných dat, [řídicí panely](log-analytics-dashboards.md), které můžete přizpůsobit pomocí grafických zobrazení nejdůležitějších hledání, a [řešení](log-analytics-add-solutions.md), která poskytují další funkce a analytické nástroje.

Následující obrázek ukazuje obrazovku s přehledem, na které se zobrazují souhrnné informace o [řešeních](#add-functionality-with-management-solutions), která jsou v pracovním prostoru nainstalovaná.  Kliknutím na libovolnou dlaždici můžete přejít k podrobným datům pro příslušné řešení.

![Portál OMS](media/log-analytics-overview/portal.png)

Log Analytics poskytuje dotazovací jazyk pro rychlé načítání a slučování dat v úložišti.  Můžete vytvořit a uložit [prohledávání protokolů](log-analytics-log-searches.md) pro přímou analýzu dat na portálu, nebo nechat prohledávání protokolů běžet automaticky, aby se vytvořila výstraha v případě, že výsledky dotazu indikují důležitou podmínku.

![Prohledávání protokolů](media/log-analytics-overview/log-search.png)

Pro účely analýzy dat mimo službu Log Analytics můžete exportovat data do nástrojů jako [Power BI](log-analytics-powerbi.md) nebo Excel.  Můžete také využít [rozhraní API hledání v protokolu](log-analytics-log-search-api.md) k vytvoření vlastních řešení, která využívají data služby Log Analytics nebo se integrují s ostatními systémy.

## <a name="add-functionality-with-management-solutions"></a>Přidání funkcí s využitím řešení pro správu
[Řešení pro správu](log-analytics-add-solutions.md) doplňují do Log Analytics další funkce a poskytují další data a analytické nástroje pro Log Analytics.  Můžou také definovat nové typy záznamů, které se mají shromáždit a které se dají analyzovat pomocí prohledávání protokolu nebo prostřednictvím dalšího uživatelského rozhraní poskytnutého řešením v řídicím panelu.  Na následujícím obrázku je příklad [řešení pro sledování změn](log-analytics-change-tracking.md)

![Řešení pro sledování změn](media/log-analytics-overview/change-tracking.png)

Řešení jsou k dispozici pro celou řadu funkcí a průběžně se přidávají další řešení.  Dostupná řešení můžete snadno procházet a [přidávat je do pracovního prostoru](log-analytics-add-solutions.md) z Azure Marketplace.  Řada jich bude nasazených automaticky a začnou fungovat hned, zatímco jiná můžou vyžadovat určitou konfiguraci.

![Galerie řešení](media/log-analytics-overview/solution-gallery.png)

## <a name="log-analytics-components"></a>Komponenty služby Log Analytics
Centrem služby Log Analytics je úložiště shromážděných dat, které je hostované v cloudu Azure.  Data se shromažďují z připojených zdrojů tak, že se konfigurují zdroje dat a přidávají řešení do vašeho předplatného.  Zdroje dat a řešení vytvářejí různé typy záznamů, které mají vlastní sady vlastností, ale dají se přesto analyzovat společně v dotazech zasílaných do úložiště.  To vám umožňuje používat stejné nástroje a metody pro práci s různými druhy dat shromážděných různými prostředky.

![Komponenty služby Log Analytics](media/log-analytics-overview/overview.png)

Propojené zdroje jsou počítače a další prostředky, které generují data shromážděná službou Log Analytics.  Můžou sem patřit agenti nainstalovaní na počítačích s [Windows](log-analytics-windows-agent.md) a [Linuxem](log-analytics-linux-agents.md), kteří se připojují přímo, nebo agenti v [připojené skupině pro správu System Center Operations Manageru](log-analytics-om-agents.md).  V případě prostředků Azure služba Log Analytics shromažďuje data z [Azure Monitoru a Azure Diagnostics](log-analytics-azure-storage.md).

[Zdroje dat](log-analytics-data-sources.md) jsou různé druhy dat shromážděných z každého připojeného zdroje.  Patří sem údaje o [událostech](log-analytics-data-sources-windows-events.md) a [výkonu](log-analytics-data-sources-performance-counters.md) z agentů [Windows](log-analytics-data-sources-windows-events.md) a Linux spolu se zdroji, jako jsou [protokoly IIS](log-analytics-data-sources-iis-logs.md) a [vlastní textové protokoly](log-analytics-data-sources-custom-logs.md).  Nakonfigurujete každý zdroj dat, který chcete shromáždit, a konfigurace se automaticky distribuuje každému připojenému zdroji.

Pokud máte vlastní požadavky, můžete použít [rozhraní API kolekce dat HTTP](log-analytics-data-collector-api.md) a zapsat data do úložiště z klienta REST API.

## <a name="log-analytics-architecture"></a>Architektura služby Log Analytics
Požadavky na nasazení služby Log Analytics jsou minimální, protože základní součásti jsou hostované v cloudu Azure.  Kromě služeb, které umožňují korelaci a analýzy shromážděných údajů, sem patří také úložiště.  Portál je přístupný z libovolného prohlížeče, takže není potřeba žádný klientský software.

Musíte nainstalovat agenty na počítače s [Windows](log-analytics-windows-agent.md) a [Linuxem](log-analytics-linux-agents.md), ale nepožaduje se žádný další agent pro počítače, které jsou už členy [připojené skupiny pro správu System Center Operations Manageru](log-analytics-om-agents.md).  Agenti Operations Manageru nadále komunikují se servery pro správu, které přeposílají jejich údaje do služby Log Analytics.  Některá řešení ale budou pro přímou komunikaci se službou Log Analytics vyžadovat agenty.  Požadavky na komunikaci jednotlivých řešení uvádí dokumentace k těmto řešením.

Když [se zaregistrujete ke službě Log Analytics](log-analytics-get-started.md), vytvoří se pracovní prostor.  Ten si můžete představit jako jedinečné prostředí Log Analytics s vlastním úložištěm dat, zdroji dat a řešeními. V rámci svého předplatného můžete vytvořit několik pracovních prostorů na podporu několika prostředí, jako je například výrobní a testovací prostředí.

![Architektura služby Log Analytics](media/log-analytics-overview/architecture.png)

## <a name="next-steps"></a>Další kroky
* [Zaregistrujte se k bezplatnému účtu Log Analytics](log-analytics-get-started.md), abyste mohli provádět testování ve vlastním prostředí.
* Prohlédněte si různé dostupné [zdroje dat](log-analytics-data-sources.md) ke shromažďování dat do služby Log Analytics.
* Pokud chcete přidávat funkce do služby Log Analytics, [procházejte dostupná řešení v galerii řešení](log-analytics-add-solutions.md).

