---
title: "Co je služba Log Analytics? | Dokumentace Microsoftu"
description: "Log Analytics je služba v rámci sady Operations Management Suite (OMS), která pomáhá shromažďovat a analyzovat provozní data vygenerovaná prostředky ve vašem cloudovém a místním prostředí.  Tento článek poskytuje stručný přehled různých komponent služby Log Analytics a odkazy na podrobný obsah."
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
ms.date: 12/09/2016
ms.author: bwren
translationtype: Human Translation
ms.sourcegitcommit: bf1455cf3975b925d114acc3e1d4cba55f2a17a9
ms.openlocfilehash: 3e7e42e2ac2deedf936ffc4d246553f72ee4dcd3


---
# <a name="what-is-log-analytics"></a>Co je služba Log Analytics?
Log Analytics je služba v rámci sady [Operations Management Suite \(OMS\)](../operations-management-suite/operations-management-suite-overview.md), která pomáhá shromažďovat a analyzovat data vygenerovaná prostředky ve vašem cloudovém a místním prostředí. Poskytuje přehledy v reálném čase pomocí integrovaného vyhledávání a vlastních řídicích panelů, díky kterým můžete analyzovat miliony záznamů napříč všemi svými úlohami a servery bez ohledu na jejich umístění.

## <a name="log-analytics-components"></a>Komponenty služby Log Analytics
Centrem služby Log Analytics je úložiště OMS, které je hostované v cloudu Azure.  Data se do úložiště shromažďují z připojených zdrojů tak, že se konfigurují zdroje dat a přidávají řešení do vašeho předplatného.  Zdroje dat a řešení vytvářejí různé typy záznamů, které mají vlastní sady vlastností, ale dají se přesto analyzovat společně v dotazech zasílaných do úložiště.  To vám umožňuje používat stejné nástroje a metody pro práci s různými druhy dat shromážděných různými prostředky.

![Úložiště OMS](media/log-analytics-overview/overview.png)

Propojené zdroje jsou počítače a další prostředky, které generují data shromážděná službou Log Analytics.  Můžou sem patřit agenti nainstalovaní na počítačích s [Windows](log-analytics-windows-agents.md) a [Linuxem](log-analytics-linux-agents.md), kteří se připojují přímo, nebo agenti v [připojené skupině pro správu System Center Operations Manageru](log-analytics-om-agents.md).  Log Analytics může také shromažďovat data z [úložiště Azure](log-analytics-azure-storage.md).

[Zdroje dat](log-analytics-data-sources.md) jsou různé druhy dat shromážděných z každého připojeného zdroje.  Patří sem události a [údaje o výkonu](log-analytics-data-sources-performance-counters.md) z agentů [Windows](log-analytics-data-sources-windows-events.md) a Linux spolu se zdroji, jako jsou [protokoly IIS](log-analytics-data-sources-iis-logs.md) a [vlastní textové protokoly](log-analytics-data-sources-custom-logs.md).  Nakonfigurujete každý zdroj dat, který chcete shromáždit, a konfigurace se automaticky distribuuje každému připojenému zdroji.

## <a name="analyzing-log-analytics-data"></a>Analýzy dat služby Log Analytics
Většina vaší práce se službou Analytics probíhá prostřednictvím portálu OMS, který se dá otevřít v jakémkoli prohlížeči a který poskytuje přístup k nastavení konfigurace a více nástrojům pro analýzy shromážděných dat a práci s nimi.  Z portálu můžete využívat [prohledávání protokolů](log-analytics-log-searches.md), kde je možné vytvářet dotazy pro analýzy shromážděných dat, [řídicí panely](log-analytics-dashboards.md) které můžete přizpůsobit pomocí grafických zobrazení nejdůležitějších hledání, a [řešení](log-analytics-add-solutions.md) která poskytují další funkce a analytické nástroje.

![Portál OMS](media/log-analytics-overview/portal.png)

Log Analytics poskytuje syntaxi dotazů k rychlému načítání a slučování dat v úložišti.  Můžete vytvořit a uložit [hledání v protokolu](log-analytics-log-searches.md) pro přímou analýzu dat na portálu OMS, nebo nechat prohledávání protokolů běžet automaticky, aby se vytvořila výstraha v případě, kdy budou výsledky dotazu indikovat důležitou podmínku.

![Prohledávání protokolů](media/log-analytics-overview/log-search.png)

Pro získání rychlého grafického přehledu stavu vašeho celkového prostředí můžete do [řídicího panelu](log-analytics-dashboards.md) přidat vizualizace uložených hledání v protokolu.   

![Řídicí panel](media/log-analytics-overview/dashboard.png)

Pro účely analýzy dat mimo službu Log Analytics můžete exportovat data z úložiště OMS do nástrojů jako [Power BI](log-analytics-powerbi.md) nebo Excel.  Můžete také využít [rozhraní API hledání v protokolu](log-analytics-log-search-api.md) k vytvoření vlastních řešení, která využívají data služby Log Analytics nebo se integrují s ostatními systémy.

## <a name="solutions"></a>Řešení
Řešení do služby Log Analytics přidávají funkčnost.  Primárně běží v cloudu a poskytují analýzu dat shromážděných v úložišti OMS. Můžou také definovat nové typy záznamů, které se mají shromáždit a které se dají analyzovat pomocí hledání v protokolu nebo prostřednictvím dalšího uživatelského rozhraní poskytnutého řešením v řídicím panelu OMS.  

![Řešení pro sledování změn](media/log-analytics-overview/change-tracking.png)

Řešení jsou k dispozici pro celou řadu funkcí a je možné snadno procházet dostupná řešení a [přidávat je do pracovního prostoru OMS](log-analytics-add-solutions.md) z galerie řešení.  Spousta jich bude nasazených automaticky a začnou fungovat hned, zatímco jiná můžou vyžadovat určitou konfiguraci.

![Galerie řešení](media/log-analytics-overview/solution-gallery.png)

## <a name="log-analytics-architecture"></a>Architektura služby Log Analytics
Požadavky na nasazení služby Log Analytics jsou minimální, protože základní součásti jsou hostované v cloudu Azure.  Kromě služeb, které umožňují korelaci a analýzy shromážděných údajů, sem patří také úložiště.  Portál je přístupný z libovolného prohlížeče, takže není potřeba žádný klientský software.

Musíte nainstalovat agenty na počítače s [Windows](log-analytics-windows-agents.md) a [Linuxem](log-analytics-linux-agents.md), ale nepožaduje se žádný další agent pro počítače, které jsou už členy [připojené skupiny pro správu SCOM](log-analytics-om-agents.md).  Agenti nástroje SCOM nadále komunikují se servery pro správu, které přeposílají jejich údaje do služby Log Analytics.  Některá řešení ale budou pro přímou komunikaci se službou Log Analytics vyžadovat agenty.  Požadavky na komunikaci jednotlivých řešení uvádí dokumentace k těmto řešením.

Když [se zaregistrujete ke službě Log Analytics](log-analytics-get-started.md), vytvoří se pracovní prostor OMS.  Ten si můžete představit jako jedinečné prostředí OMS s vlastním úložištěm dat, zdroji dat a řešeními. V rámci svého předplatného můžete vytvořit několik pracovních prostorů na podporu několika prostředí, jako je například výrobní a testovací prostředí.

![Architektura služby Log Analytics](media/log-analytics-overview/architecture.png)

## <a name="next-steps"></a>Další kroky
* [Zaregistrujte se k bezplatnému účtu Log Analytics](log-analytics-get-started.md), abyste mohli provádět testování ve vlastním prostředí.
* Zobrazte různé dostupné [zdroje dat](log-analytics-data-sources.md) ke shromažďování dat do úložiště OMS.
* Pokud chcete přidávat funkce do služby Log Analytics, [procházejte dostupná řešení v galerii řešení](log-analytics-add-solutions.md).




<!--HONumber=Dec16_HO2-->


