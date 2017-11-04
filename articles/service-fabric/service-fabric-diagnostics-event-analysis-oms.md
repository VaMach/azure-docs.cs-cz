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
ms.openlocfilehash: f0cefab15a115719ea9c378546a7e6004bd06187
ms.sourcegitcommit: a7c01dbb03870adcb04ca34745ef256414dfc0b3
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/17/2017
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

Existují dva způsoby, jak zřídit a nakonfigurovat pracovním prostorem OMS prostřednictvím šablony Resource Manageru nebo přímo z Azure Marketplace. Používejte první, když nasazujete cluster a pozdější Pokud již máte nasazení s diagnostikou clusteru povolena.

### <a name="deploying-oms-using-a-resource-management-template"></a>Nasazení OMS pomocí šablony Správa prostředků

Při nasazování clusteru pomocí šablony Resource Manageru, šablony můžete také vytvořit nový pracovní prostor OMS, přidejte řešení prostředků infrastruktury služby k němu a nakonfigurujte ji číst data z tabulky odpovídající úložiště.

>[!NOTE]
>Pro tento postup musí být povoleno v pořadí pro úložiště Azure tabulky existovat pro OMS diagnostiky nebo pro čtení informací v z protokolu analýzy.

[Zde](https://azure.microsoft.com/resources/templates/service-fabric-oms/) je ukázka šablony, která můžete použít a změnit podle požadavků, který provádí výše akce. V případě, že chcete další volitelnost, jsou k dispozici několik další šablony, které poskytují různé možnosti v závislosti na tom, kde v procesu je možné nastavení pracovním prostorem OMS – najdete na [Service Fabric a OMS šablony](https://azure.microsoft.com/resources/templates/?term=service+fabric+OMS).

### <a name="deploying-oms-using-through-azure-marketplace"></a>Nasazení pomocí OMS prostřednictvím Azure Marketplace

Pokud chcete přidat pracovním prostorem OMS po nasazení clusteru, přejděte na Azure Marketplace a vyhledejte *"Service Fabric Analytics"*.

![Analýza SF OMS v Marketplace.](media/service-fabric-diagnostics-event-analysis-oms/service-fabric-analytics.png)

* Klikněte na **vytvořit**
* V okně vytváření Service Fabric Analytics klikněte na **vyberte pracovní prostor** pro *pracovním prostorem OMS* pole a potom **vytvořit nový pracovní prostor**. Vyplnit požadované položky – tady Jediným požadavkem je, že předplatné pro cluster Service Fabric a pracovním prostorem OMS musí být stejná. Po ověření vaše záznamy vaším pracovním prostorem OMS spustí nasazení. To by měl jenom pár minut trvat.
* Po dokončení klikněte na tlačítko **vytvořit** znovu v dolní části okna Vytvoření Service Fabric Analytics. Ujistěte se, že nový pracovní prostor se zobrazí v části *pracovním prostorem OMS*. Řešení se přidá do pracovního prostoru, který jste právě vytvořili.


I když toto řešení přidá do pracovního prostoru, pracovním prostoru stále musí být připojen k diagnostiky dat pocházejících z clusteru. Přejděte do skupiny prostředků, kterou jste vytvořili řešení Service Fabric analýzy v. Měli byste vidět *ServiceFabric (\<nameOfOMSWorkspace\>)*.

* Klikněte na řešení, přejděte na stránku s jeho přehled, ze kterých můžete změnit nastavení řešení, nastavení pracovního prostoru a přejděte na portálu OMS.
* V levém navigační nabídce klikněte na **účtů úložiště protokolů**v části *zdroje dat pracovního prostoru*.

    ![Řešení Service Fabric analýzy na portálu](media/service-fabric-diagnostics-event-analysis-oms/service-fabric-analytics-portal.png)

* Na *protokol účtu úložiště* klikněte na tlačítko **přidat** v horní části přidat váš cluster protokoly do pracovního prostoru.
* Klikněte na tlačítko do **účet úložiště** přidat odpovídající účet vytvořený v clusteru. Pokud jste použili výchozí název, název účtu úložiště *sfdg\<resourceGroupName\>*. Můžete to ověřit také kontrolou šablony Azure Resource Manageru, které jsou používány k nasazení kontrolou hodnota používaná pro cluster, `applicationDiagnosticsStorageAccountName`. Možná budete také muset přejděte dolů a klikněte na tlačítko **načtěte více** Pokud název účtu nezobrazuje. Klikněte na název účtu úložiště správné, když se zobrazí vyberte.
* Další, budete muset zadat *datový typ*, což by mělo být **události prostředků infrastruktury služby**.
* *Zdroj* automaticky musí být nastavena na *WADServiceFabric\*EventTable*.
* Klikněte na tlačítko **OK** připojit pracovního prostoru do protokolů vašeho clusteru.

    ![Přidat protokol účtu úložiště do OMS](media/service-fabric-diagnostics-event-analysis-oms/add-storage-account.png)

* Účet by měl nyní zobrazují jako součást vaší *protokol účtu úložiště* ve zdrojích dat pracovního prostoru.

Pomocí této nyní jste přidali řešení Service Fabric analýzy v pracovním prostoru analýzy protokolů OMS, který je nyní správně připojené k platformě váš cluster a tabulku protokolu aplikace. Další zdroje můžete přidat do pracovního prostoru stejným způsobem.

## <a name="using-the-oms-agent"></a>Pomocí agenta OMS

Je doporučené použít EventFlow a WAD jako řešení agregace, protože umožňují pro více modulární přístup k diagnostiky a monitorování. Například pokud chcete změnit vaše výstupy z EventFlow, vyžaduje nijak nemění skutečné instrumentace, právě jednoduchých úprav konfiguračního souboru. Pokud, ale můžete rozhodnout investovat do pomocí OMS a chcete-li pokračovat v používání pro analýzu událostí (nemusí být jediným platformy, můžete použít, ale to spíše bude alespoň jeden z platformy), doporučujeme, abyste se seznámili nastavení [OMS ag trola](../log-analytics/log-analytics-windows-agents.md). Byste měli použít také OMS agent při nasazení kontejnerů do clusteru, jak je popsáno níže.

Proces tohoto postupu je poměrně jednoduché, protože právě musíte přidat agenta škálování virtuálního počítače nastavit rozšíření do šablony Resource Manageru jako zajistíte, že získá nainstalované na všech uzlů. Ukázka šablonu Resource Manager, která nasadí pracovní prostor OMS s řešením Service Fabric (jak je uvedeno výše) a přidá agenta do uzlů pro clustery se systémem nalezen [Windows](https://github.com/ChackDan/Service-Fabric/tree/master/ARM%20Templates/SF%20OMS%20Samples/Windows) nebo [Linux](https://github.com/ChackDan/Service-Fabric/tree/master/ARM%20Templates/SF%20OMS%20Samples/Linux).

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

Tento článek popisuje kroky potřebné k nastavení kontejneru monitorování pro váš cluster. Další informace o řešení kontejnery na OMS, podívejte se na jejich [dokumentaci](../log-analytics/log-analytics-containers.md).

Nastavit řešení kontejneru v pracovním prostoru, ujistěte se, že máte agenta OMS nasazené na uzly clusteru na podle kroků uvedených výše. Jakmile clusteru je připraven, nasaďte kontejner k němu. Berte v úvahu, že při prvním nasazení kontejneru image do clusteru, ho trvá několik minut stáhnout bitovou kopii v závislosti na jeho velikosti.

V Azure Marketplace vyhledejte *řešení monitorování kontejneru* a vytvořte **řešení monitorování kontejneru** výsledek, který by měla být dřívější, v oddíle monitorování a správu kategorie.

![Přidání kontejnery řešení](./media/service-fabric-diagnostics-event-analysis-oms/containers-solution.png)

V kroku vytváření požaduje pracovním prostorem OMS. Vyberte ten, který byl vytvořen s nasazením výše. Tento krok přidává řešení kontejnery v rámci pracovní prostor OMS a je automaticky zjišťován agentem OMS nasazení šablony. Agent začne shromažďování dat na kontejnery procesů v clusteru a méně než 15 minut nebo Ano, měli byste vidět světla až s daty, jako obrázek na řídicím panelu výše uvedené řešení.


## <a name="next-steps"></a>Další kroky

Prozkoumejte následující OMS nástrojů a možností pro přizpůsobení pracovního prostoru vašim potřebám:

* Pro místní clusterů nabízí OMS brány (dál server Proxy protokolu HTTP), který slouží k odesílání dat do OMS. Další informace o v [propojíte počítače bez připojení k Internetu pomocí brány OMS OMS](../log-analytics/log-analytics-oms-gateway.md)
* Konfigurace OMS nastavit [automatizované výstrahy](../log-analytics/log-analytics-alerts.md) které pomáhají při zjišťování a Diagnostika
* Získat familiarized s [vyhledávání a dotazování protokolu](../log-analytics/log-analytics-log-searches.md) funkcím poskytovaným jako součást analýzy protokolů