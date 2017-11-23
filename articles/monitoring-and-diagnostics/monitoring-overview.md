---
title: "Monitorování v Microsoft Azure | Microsoft Docs"
description: "Možnosti, když chcete monitorovat nic v Microsoft Azure. Azure monitorování, Application Insights a analýzy protokolů"
author: rboucher
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
ms.date: 10/04/2017
ms.author: robb
ms.openlocfilehash: c34211e0c55c10defaa32f1e0a2195514ff3ae5f
ms.sourcegitcommit: 62eaa376437687de4ef2e325ac3d7e195d158f9f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/22/2017
---
# <a name="overview-of-monitoring-in-microsoft-azure"></a>Přehled monitorování v Microsoft Azure
Tento článek obsahuje přehled nástroje a služby, které se účastní komplexní monitorování Microsoft Azure. Platí pro:
- Pomocí služeb Azure ke sledování infrastruktury Azure a aplikace
- Pomocí služby Azure pro hybridní monitorování a infrastruktury mimo Azure a aplikace
- Použití mimo platformu Azure services pro monitorování infrastruktury Azure a aplikace

Tento článek popisuje různé produkty a služby, které jsou k dispozici a jak pracují společně. Může pomoci určit, které nástroje jsou pro vás nejvhodnější v jaké případech.  

## <a name="why-use-azures-monitoring-services"></a>Proč používat služby monitorování Azure?

Problémy s výkonem v cloudové aplikace může mít vliv na vaši firmu. S více vzájemně propojena součástmi a často verzích může dojít, degradations kdykoli. A pokud vyvíjíte aplikace, uživatelé obvykle zjistit problémy, které nebyl nalezen v testování. Měli vědět o tyto problémy okamžitě a mít nástroje pro diagnostiku a řešení problémů. Kromě toho od základní infrastruktury, na kterém tyto aplikace spouštět, může způsobit problémy v aplikaci tak komplexní pohled na vaše aplikace a infrastrukturu je klíč k monitorování prostředí Azure. Microsoft Azure obsahuje řadu nástrojů pro identifikaci a řešení těchto problémů.

## <a name="how-do-i-monitor-my-azure-environment"></a>Jak se monitorování Moje prostředí Azure?

Existují řadu nástrojů pro monitorování prostředí Azure z kódu aplikace spuštěné v Azure ke službám a infrastruktuře hostování vaší aplikace. Tyto nástroje vzájemně spolupracují a nabízí komplexní cloudové monitorování a zahrnují:

-   **Azure monitorování** – služba Azure, která funguje jako konsolidované kanálu pro všechna monitorování data ze služby Azure. Umožňuje vám přístup k metriky výkonu a událostí, které popisují operaci infrastrukturu Azure a služby Azure, kterou používáte. Azure monitorování je monitorování kanálu dat pro prostředí Azure a nabízí data přímo do Log Analytics, jakož i 3. stran nástroje, kde můžete proniknout do dat a kombinovat s daty z na místní nebo jiným prostředkům cloudu.

-   **Application Insights** – služba Azure, která nabízí monitorování a uživatel Analýza výkonu aplikace. Sleduje napsání kódu a aplikace, které jste nasadili v Azure, a místní nebo ostatních cloudů. Podle instrumentaci vaší aplikace pomocí Application Insights SDK můžete získat přístup k rozsahu dat včetně doby odezvy, závislosti, výjimka trasování, ladění snímky a provádění profily. Poskytuje výkonné nástroje pro analýzu tuto telemetrii aplikace při vývoji a provozování vaší aplikace. Je úzce se integruje s Visual Studio umožňují získat zprava problém řádky kódu, takže ho můžete řešit a nabízí analýzy využití pro analýzu využití zákazníka aplikací také manažerům produktu.

-   **Analýza protokolu** -dříve označované jako analýzy protokolů OMS, je služba Azure, která ingestuje protokolu a metriku, data ze služby Azure (přes Azure monitorování), virtuální počítače Azure a místní nebo jiné cloudové infrastruktury a nabízí flexibilní protokol hledání a na více systémů v pole analýzy nad tato data. Nabízí bohaté nástroje k analýze dat napříč zdrojů, umožňuje komplexní dotazy napříč všechny protokoly a může aktivně upozornit na zadaných podmínek.  Můžete i shromáždění vlastních dat do své centrální úložiště, takže se můžete dotazovat a vizualizovat ho. Můžete taky využít výhod integrované řešení protokolu analýzy a okamžitě získáte přehled o zabezpečení a funkce vaší infrastruktury.

## <a name="accessing-monitoring-in-the-azure-portal"></a>Přístup k monitorování na portálu Azure
Monitorování všech služeb Azure jsou nyní k dispozici v jednom podokně uživatelského rozhraní. Další informace o tom, jak získat přístup k této oblasti najdete v tématu [Začínáme s Azure monitorování](monitoring-get-started.md). 

Monitorování funkce pro konkrétní prostředky Azure můžete také přejít pomocí zvýrazňování tyto prostředky a podrobnějším informacím o jejich možnosti monitorování. 

## <a name="examples-of-when-to-use-which-tool"></a>Příklady použití který nástroj 

Následující části vysvětlují některé základní scénáře a které nástroje by měl použít společně. 

### <a name="scenario-1--fix-errors-in-an-azure-application-under-development"></a>Scénář 1 – oprava chyb v aplikaci Azure ve vývoji   

**Nejlepší možnost je společně použít Application Insights, sledování Azure a Visual Studio**

Azure teď poskytuje potenciál ladicího programu sady Visual Studio v cloudu. Konfigurace monitorování Azure k odesílání telemetrie Application insights. Povolte sady Visual Studio Application Insights SDK do aplikace zahrnout. Jednou ve službě Application Insights můžete použít aplikaci mapy zjistit vizuálně části spuštěné aplikace, které jsou v pořádku, či nikoli. Pro ty části, které nejsou v pořádku chyby a výjimky jsou již k dispozici pro zkoumání. Různé analytics ve službě Application Insights vám pomůže přejít hlubší. Pokud si nejste jisti o této chybě, můžete pro trasování do kódu a kódu pin bod problém další ladicího programu sady Visual Studio. 

Další informace najdete v tématu [monitorování webové aplikace](../application-insights/app-insights-azure-web-apps.md) a odkazovat na obsah na levé straně pokyny pro různé typy aplikací a jazyků.  

### <a name="scenario-2--debug-an-azure-net-web-application-for-errors-that-only-show-in-production"></a>Scénář 2 – ladění webové aplikace služby Azure .NET pro chyby, které se zobrazí jenom v produkčním prostředí 

> [!NOTE]
> Tyto funkce jsou ve verzi preview. 

**Nejlepší možnost je použít Application Insights a pokud možné Visual Studio pro úplné ladění prostředí.**

Ladění aplikace pomocí ladicího programu Application Insights snímku. Dojde-li k určitým prahem chyby s provozním součásti, systém automaticky zaznamená telemetrie v systému windows času názvem "snímků." Velikost zachytit je bezpečné pro přístup z cloudu produkční, protože je dostatečně malé, nechcete mít vliv na výkon, ale dostatečně významné umožňující trasování.  Systém můžete zaznamenat několik snímků. Můžete prohlížet bod v čase na portálu Azure nebo použijte sadu Visual Studio pro úplné prostředí. Pomocí sady Visual Studio můžou vývojáři provede tento snímek jako kdyby byly ladění v reálném čase. Lokální proměnné, parametry, paměti a rámce jsou všechny dostupné. Vývojáři musí mít udělen přístup k těmto datům produkční prostřednictvím [RBAC role](../active-directory/role-based-access-built-in-roles.md).  

Další informace najdete v tématu [snímku ladění](../application-insights/app-insights-snapshot-debugger.md). 

### <a name="scenario-3--debug-an-azure-application-that-uses-containers-or-microservices"></a>Scénář 3 – ladění aplikace Azure, která používá kontejnery nebo mikroslužeb 

**Stejné jako scénář 1. Společně použít Application Insights, sledování Azure a Visual Studio**

Application Insights podporuje taky shromažďování telemetrie z procesů běžících v rámci kontejnery a mikroslužeb (Kubernetes, Docker, Azure Service Fabric). Další informace najdete [najdete v tomto videu o ladění kontejnerů a mikroslužeb](https://go.microsoft.com/fwlink/?linkid=848184). 


### <a name="scenario-4--fix-performance-issues-in-your-azure-application"></a>Scénář 4 – oprava problémů s výkonem v aplikaci Azure

[Application Insights profileru](../application-insights/app-insights-profiler.md) slouží k řešení těchto typů problémů. Můžete identifikovat a řešit problémy s výkonem, pro aplikace běžící v App Services (webové aplikace, Logic Apps, mobilní aplikace, aplikace API, funkce aplikace) a další výpočetní prostředky, jako jsou virtuální počítače sady škálování virtuálního počítače (VMSS), cloudové služby a Service Fabric. 

> [!NOTE]
> Schopnost profil virtuální počítače, škálovací sady virtuálních počítačů (VMSS), cloudové služby a služby prostředků infrastruktury je ve verzi preview.   

Kromě toho proaktivně upozornění e-mailem o určitých typů chyb, jako je například časů načtení stránky pomalé, od nástroj inteligentní detekce.  Nepotřebujete provádět žádnou konfiguraci na tento nástroj. Další informace najdete v tématu [Inteligentní detekce - anomálie výkonu](../application-insights/app-insights-proactive-performance-diagnostics.md).



## <a name="next-steps"></a>Další kroky
Další informace o

* [Azure monitorování v videa z Ignite 2016](https://myignite.microsoft.com/videos/4977)
* [Začínáme s Azure monitorování](monitoring-get-started.md)
* [Azure Diagnostics](../azure-diagnostics.md) Pokud se pokoušíte diagnostikovat problémy s cloudové služby, virtuální počítač, virtuální počítač škálování aplikace Fabric nastaveny nebo služby.
* [Application Insights](https://azure.microsoft.com/documentation/services/application-insights/) Pokud se pokoušíte diagnostiky problémů v aplikaci služby webové aplikace.
* [Analýza protokolu](https://azure.microsoft.com/documentation/services/log-analytics/) a [Operations Management Suite](https://www.microsoft.com/oms/) produkční řešení monitorování
