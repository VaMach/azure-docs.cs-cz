---
title: "Kontejnery Azure Service Fabric monitorování a Diagnostika | Microsoft Docs"
description: "Zjistěte, jak sledovat a diagnostikovat kontejnery orchestrovat na Microsoft Azure Service Fabric na OMS kontejnery řešení."
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
ms.openlocfilehash: 319ee2c0f7492389bc1767aa2669dd273f8cfa1b
ms.sourcegitcommit: a7c01dbb03870adcb04ca34745ef256414dfc0b3
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/17/2017
---
# <a name="monitoring-windows-server-containers-with-oms"></a>Monitorování systému Windows Server kontejnery s OMS

## <a name="oms-containers-solution"></a>Řešení kontejnery OMS

Analýzy protokolů Operations Management Suite (OMS) má kontejnery řešení, které lze použít pro monitorování kontejnery. Vedle řešení Service Fabric toto řešení je skvělý nástroj pro monitorování nasazení kontejnerů orchestrovat v Service Fabric. Tady je jednoduchý příklad řídicího panelu řešení, která bude vypadat takto:

![Řídicí panel základní OMS](./media/service-fabric-diagnostics-containers-windowsserver/oms-containers-dashboard.png)

Shromažďuje taky různé druhy protokoly, které může být dotazována v nástroj OMS analýzy protokolů a může sloužit k vizualizaci všechny metriky nebo události generován. Typy protokolu, které byly shromážděny jsou:

1. ContainerInventory: obsahuje informace o umístění kontejneru, název a obrázků
2. ContainerImageInventory: informace o nasazené bitové kopie, včetně ID nebo velikosti
3. ContainerLog: specifické chybové protokoly, protokoly docker (stdout atd.) a ostatní položky
4. ContainerServiceLog: docker démon příkazy, které byly spuštěny
5. Výkonu: čítače včetně kontejneru vstupně-výstupních operací a vlastní metriky z hostitelských počítačích disku procesoru, paměti, síťového provozu,

Tento článek popisuje kroky potřebné k nastavení kontejneru monitorování pro váš cluster. Další informace o řešení kontejnery na OMS, podívejte se na jejich [dokumentaci](../log-analytics/log-analytics-containers.md).

## <a name="1-set-up-a-service-fabric-cluster"></a>1. Nastavení clusteru Service Fabric

Vytvoření clusteru s podporou pomocí šablony Azure Resource Manager najít [zde](https://github.com/ChackDan/Service-Fabric/tree/master/ARM%20Templates/SF%20OMS%20Samples/Windows). Nezapomeňte přidat jedinečný název pracovní prostor OMS. Tato šablona také výchozí nasazení clusteru ve verzi preview sestavení Service Fabric (v255.255), což znamená, že nelze použít v produkčním prostředí a nelze upgradovat na jinou verzi Service Fabric. Pokud se rozhodnete pro tuto šablonu použít dlouhodobé nebo produkční použít, změňte verzi číslo stabilní verze.

Až nastavíte clusteru, zkontrolujte, jestli jste nainstalovali příslušný certifikát a ujistěte se, že budete moct připojit ke clusteru.

Zkontrolujte, zda vaší skupiny prostředků nastavená správně pomocí záhlaví k [portál Azure](https://portal.azure.com/) a hledání nasazení. Skupina prostředků by měl obsahovat všechny prostředky Service Fabric a také mít řešení Log Analytics, jakož i řešení Service Fabric.

Úpravy stávajícího clusteru Service Fabric:
* Zkontrolujte, zda je povoleno diagnostiky (Pokud ne, povolit pomocí [aktualizace škálovací sadu virtuálních počítačů](/rest/api/virtualmachinescalesets/create-or-update-a-set))
* Přidat pracovním prostorem OMS vytvořením "Service Fabric Analytics" řešení prostřednictvím Azure Marketplace
* Upravit řešení Service Fabric načíst data z tabulky služby Azure Storage (nastavit tak, že WAD) odpovídající zdroje dat ve skupině prostředků, který je cluster v
* Přidat agenta jako [rozšíření škálovací sadu virtuálních počítačů](/powershell/module/azurerm.compute/add-azurermvmssextension) prostřednictvím prostředí PowerShell nebo prostřednictvím aktualizace virtuálního počítače sady škálování (jak je uvedeno výše, k úpravě šablony Resource Manageru stejné odkaz)

## <a name="2-deploy-a-container"></a>2. Nasazení kontejneru

Jakmile clusteru je připravené a ověříte, že budete mít přístup, nasaďte kontejner k němu. Pokud jste se rozhodli používat verzi preview nastavená šablony, můžete si také prostudovat Service Fabric nové docker compose funkce. Berte v úvahu, že při prvním nasazení kontejneru image do clusteru, ho trvá několik minut stáhnout bitovou kopii v závislosti na jeho velikosti.

## <a name="3-add-the-containers-solution"></a>3. Přidat řešení kontejnery

Na portálu Azure vytvořit prostředek kontejnery (v oddíle monitorování a správu kategorie) prostřednictvím Azure Marketplace. 

![Přidání kontejnery řešení](./media/service-fabric-diagnostics-containers-windowsserver/containers-solution.png)

V kroku vytváření požaduje pracovním prostorem OMS. Vyberte ten, který byl vytvořen s nasazením výše. Tento krok přidává řešení kontejnery v rámci pracovní prostor OMS a je automaticky zjišťován agentem OMS nasazení šablony. Agent začne shromažďování dat na kontejnery procesů v clusteru, a v asi 10 až 15 minut, měli byste vidět světlým řešení se s daty jako obrázek na řídicím panelu výše.

## <a name="4-next-steps"></a>4. Další kroky

OMS nabízí různé nástroje v pracovním prostoru, aby pro vás provedl Pokud užitečnější. Prozkoumejte následující možnosti přizpůsobení řešení vašim potřebám:
- Získat familiarized s [vyhledávání a dotazování protokolu](../log-analytics/log-analytics-log-searches.md) funkcím poskytovaným jako součást analýzy protokolů
- Konfigurace agenta OMS na vyzvednutí konkrétních čítačích výkonu (na domovskou stránku pracovní prostor > Nastavení > Data > čítačů výkonu systému Windows)
- Konfigurace OMS nastavit [automatizované výstrahy](../log-analytics/log-analytics-alerts.md) pravidla, která pomáhají při zjišťování a Diagnostika