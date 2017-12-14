---
title: "Analýza události služby Azure Service Fabric s OMS | Microsoft Docs"
description: "Další informace o vizualizaci a analýzu událostí pomocí OMS pro monitorování a Diagnostika Azure Service Fabric clusterů."
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
ms.openlocfilehash: 977c5d64a32157b39aa6b618196dde20c4c3cc8e
ms.sourcegitcommit: 922687d91838b77c038c68b415ab87d94729555e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/13/2017
---
# <a name="event-analysis-and-visualization-with-oms"></a>Analýza události a vizualizace s OMS

Operations Management Suite (OMS) je kolekce služby správy, které pomáhají s monitorování a Diagnostika pro aplikace a služby hostované v cloudu. Chcete-li získat podrobnější přehled OMS a co nabízí, přečtěte si [co je OMS?](../operations-management-suite/operations-management-suite-overview.md)

## <a name="log-analytics-and-the-oms-workspace"></a>Analýzy protokolů a pracovním prostorem OMS

Analýzy protokolů shromažďuje data z spravované prostředky, včetně úložiště Azure table nebo agenta a udržuje v centrálním úložišti. Data lze poté použít pro analýzy, výstrahy a vizualizace nebo další export. Analýzy protokolů podporuje události, údaje o výkonu nebo jiná vlastní data.

Pokud je nakonfigurovaná OMS, budete mít přístup na konkrétní *pracovním prostorem OMS*, ze kterých můžete data dotaz nebo vizualizována ve řídicí panely.

Po přijetí dat podle analýzy protokolů OMS má několik *řešení pro správu* hotových řešení ke sledování příchozích dat, upravit tak, aby několik scénářů, které jsou. Mezi ně patří *Service Fabric Analytics* řešení a *kontejnery* řešení, které jsou dvě nejdůležitější ty diagnostiky a monitorování při použití clusterů Service Fabric. Existuje několik ostatní, a které je vhodné využít a OMS také umožňuje používat pro vytváření vlastních řešení, které další informace o [zde](../operations-management-suite/operations-management-suite-solutions.md). Každé řešení, které chcete použít pro cluster s podporou lze konfigurovat ve stejném pracovním prostorem OMS spolu s analýzy protokolů. Pracovní prostory umožňují vlastní řídicí panely a vizualizace dat a změny, které chcete shromažďovat, proces a analyzovat data.

## <a name="setting-up-an-oms-workspace-with-the-service-fabric-analytics-solution"></a>Nastavení pracovní prostor OMS s řešení analýzy Service Fabric
Je doporučeno, můžete zahrnout řešení prostředků infrastruktury služby vaším pracovním prostorem OMS – obsahuje řídicí panel, který ukazuje různé příchozí kanály protokolu z úrovně platformy a aplikace a poskytuje možnost dotazu Service Fabric určitých protokolů. Tady je poměrně jednoduché řešení prostředků infrastruktury služby vzhled, s jedné aplikace nasazené na clusteru:

![OMS SF řešení](media/service-fabric-diagnostics-event-analysis-oms/service-fabric-solution.png)

V tématu [nastavení analýzy protokolů OMS](service-fabric-diagnostics-oms-setup.md) začít pracovat s to pro váš cluster.

## <a name="using-the-oms-agent"></a>Pomocí agenta OMS

Je doporučené použít EventFlow a WAD jako řešení agregace, protože umožňují pro více modulární přístup k diagnostiky a monitorování. Například pokud chcete změnit vaše výstupy z EventFlow, vyžaduje nijak nemění skutečné instrumentace, právě jednoduchých úprav konfiguračního souboru. Pokud však rozhodnete investovat do pomocí analýzy protokolů OMS, měli byste nastavit [agenta OMS](../log-analytics/log-analytics-windows-agent.md). Byste měli použít také OMS agent při nasazení kontejnerů do clusteru, jak je popsáno níže. 

Přejděte přes [přidat agenta OMS do clusteru s podporou](service-fabric-diagnostics-oms-agent.md) kroky v tomto.

Výhody tohoto jsou následující:

* Data širší na straně čítače a metriky výkonu
* Jednoduchá konfigurace metriky shromažďovaných z clusteru a bez nutnosti aktualizovat konfiguraci vašeho clusteru. Změny nastavení agenta lze provést na portálu OMS a agent automaticky restartuje tak, aby odpovídala požadované konfigurace. Konfigurace agenta OMS na vyzvednutí konkrétních čítačích výkonu, přejděte do pracovního prostoru **Domů > Nastavení > Data > čítačů výkonu systému Windows** a vyberte chcete najdete v části shromážděných dat
* Data rychleji, než ho museli být uloženy před se zachyceny OMS objeví / analýzy protokolů
* Monitorování kontejnery je mnohem jednodušší, protože rozpoznal docker protokoly (stdout, stderr) a statistiky (metriky výkonu na kontejner a uzel úrovně)

Hlavní význam tady je, že vzhledem k tomu, že agent nasadí cluster společně se všechny aplikace, mohou být některé dopad na výkon vaší aplikace v clusteru.

## <a name="monitoring-containers"></a>Monitorování kontejnery

Při nasazení kontejnerů do clusteru Service Fabric, doporučujeme, aby clusteru byla nastavena s agentem OMS a že kontejnery řešení byl přidán k vašim pracovním prostorem OMS povolení monitorování a Diagnostika. Zde je, jak řešení kontejnery vypadá v pracovním prostoru:

![Řídicí panel základní OMS](./media/service-fabric-diagnostics-event-analysis-oms/oms-containers-dashboard.png)

Agenta umožňuje kolekce několik specifické pro kontejner protokoly, které mohou být dotazována v OMS, nebo použít k ukazatele vizualizovaných výkonu. Typy protokolu, které byly shromážděny jsou:

* ContainerInventory: obsahuje informace o umístění kontejneru, název a obrázků
* ContainerImageInventory: informace o nasazené bitové kopie, včetně ID nebo velikosti
* ContainerLog: specifické chybové protokoly, protokoly docker (stdout atd.) a ostatní položky
* ContainerServiceLog: docker démon příkazy, které byly spuštěny
* Výkonu: čítače včetně kontejneru vstupně-výstupních operací a vlastní metriky z hostitelských počítačích disku procesoru, paměti, síťového provozu,

[Monitorování kontejnery s analýzy protokolů OMS](service-fabric-diagnostics-oms-containers.md) popisuje kroky potřebné k nastavení kontejneru monitorování pro váš cluster. Další informace o řešení kontejnery na OMS, podívejte se na jejich [dokumentaci](../log-analytics/log-analytics-containers.md).

## <a name="next-steps"></a>Další kroky

Prozkoumejte následující OMS nástrojů a možností pro přizpůsobení pracovního prostoru vašim potřebám:

* Pro místní clusterů nabízí OMS brány (dál server Proxy protokolu HTTP), který slouží k odesílání dat do OMS. Další informace o v [propojíte počítače bez připojení k Internetu pomocí brány OMS OMS](../log-analytics/log-analytics-oms-gateway.md)
* Konfigurace OMS nastavit [automatizované výstrahy](../log-analytics/log-analytics-alerts.md) které pomáhají při zjišťování a Diagnostika
* Získat familiarized s [vyhledávání a dotazování protokolu](../log-analytics/log-analytics-log-searches.md) funkcím poskytovaným jako součást analýzy protokolů