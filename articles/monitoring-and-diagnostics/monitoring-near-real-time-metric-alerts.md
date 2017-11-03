---
title: "Téměř v reálném čase metriky výstrahy v Azure monitorování | Microsoft Docs"
description: "Téměř v reálném čase metrika výstrahy umožňují monitorovat metriky prostředků Azure s četností až 1 min."
author: snehithm
manager: kmadnani1
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/16/2017
ms.author: snmuvva
ms.custom: 
ms.openlocfilehash: aeeb6c2fb87e6c19991ef243ee7230f4e8f4e251
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="near-real-time-metric-alerts-preview"></a>Téměř v reálném čase metriky výstrahy (Preview)
Azure monitorování teď podporuje nový typ metriky výstrah názvem téměř v reálném čase metrika výstrahy (Preview). Tato funkce je aktuálně ve verzi public preview.
Tyto výstrahy se liší od regulární metriky výstrahy několika způsoby.

- **Vylepšené latence** -téměř v reálném čase metrika výstrahy sledování změn v metriky hodnoty co nejdříve 1 min.
- **Větší kontrolu nad metriky podmínky** -téměř v reálném čase metrika výstrahy povoluje uživatelům definovat bohatší pravidla výstrah. Výstrahy teď podporují monitorování maximální, minimální, průměrná a celkové hodnoty metrik.
- **Kombinaci monitorování více metrik** -téměř v reálném čase metrika výstrah můžete sledovat více metriky (aktuálně dvě) s jedním pravidlem. Získá aktivuje výstraha, pokud obě metriky narušit jejich příslušné prahové hodnoty pro zadané časové období.
- **Modulární oznámení systému** – téměř v reálném čase metriky výstrahy použití [skupiny akcí](monitoring-action-groups.md). Tato funkce umožňuje uživatelům vytvářet akce způsobem modulární a znovu je použít pro mnoho pravidla výstrah.

> [!NOTE]
> Téměř v reálném čase metriky výstrahy funkce je aktuálně ve verzi public preview. Funkce a uživatelské prostředí je mohou podléhat změnám.
>

## <a name="what-resources-can-i-create-near-real-time-metric-alerts-for"></a>Jaké prostředky můžete vytvořit téměř v reálném čase metriky výstrahy pro?
Úplný seznam typů prostředků, které jsou podporovány téměř v reálném čase metriky výstrahy:

* Microsoft.ApiManagement/service
* Microsoft.Batch/batchAccounts
* Microsoft.Cache/Redis
* Microsoft.Compute/virtualMachines
* Microsoft.Compute/virtualMachineScaleSets
* Microsoft.DataFactory/factories
* Microsoft.DBforMySQL/servers
* Microsoft.DBforPostgreSQL/servers
* Microsoft.EventHub/namespaces
* Microsoft.Logic/workflows
* Microsoft.Network/applicationGateways
* Microsoft.Network/publicipaddresses
* Microsoft.Search/searchServices
* Microsoft.ServiceBus/namespaces
* Microsoft.Sql/servers/elasticpools
* Microsoft.StreamAnalytics/streamingjobs
* Microsoft.Timeseriesinsights
* Microsoft.CognitiveServices/accounts


## <a name="create-a-near-real-time-metric-alert"></a>Vytvořit výstrahu metriky Near v reálném čase
V současné době téměř v reálném čase metrika výstrahy lze vytvořit pouze prostřednictvím portálu Azure. Podpora konfigurace téměř v reálném čase metriky výstrah pomocí prostředí PowerShell, rozhraní příkazového řádku (CLI) a rozhraní REST API Azure monitorování tu bude brzo dostupná.

1. V [portál](https://portal.azure.com/), vyhledejte prostředků, které vás zajímají monitorování a vyberte ho. Tento prostředek by měl být jeden z typů prostředků, které jsou uvedené v [předchozí části](#what-resources-can-i-create-near-real-time-metric-alerts-for). Můžete také provést stejný pro všechny typy podporovaných prostředky centrálně z monitorování > výstrahy.

2. Vyberte **výstrahy** nebo **výstrah pravidla** části monitorování. Text a ikona se mohou mírně lišit pro různé prostředky.
   ![Monitorování](./media/insights-alerts-portal/AlertRulesButton.png)

3. Klikněte **přidat v reálném čase metriky upozornění (preview) na brzký** příkaz. Pokud příkaz není k dispozici, ověřte, že prostředek je vybrán ve filtru.

    ![Přidat téměř v reálném čase metrik výstrah tlačítko](./media/monitoring-near-real-time-metric-alerts/AddNRTAlertButton.png)

4. **Název** upozornění pravidlo a vyberte **popis**, který také zobrazuje v oznámení e-mailů.
5. Vyberte **metrika** chcete monitorovat, a potom vyberte **podmínku**, **čas agregace**, a **prahová hodnota** hodnoty pro metriku. Volitelně vyberte jiný **metrika** chcete monitorovat, a potom vyberte **podmínku**, **čas agregace**, a **prahová hodnota** hodnota druhý metriku. 

    ![Přidat téměř v reálném čase metriky Alert1](./media/monitoring-near-real-time-metric-alerts/AddNRTAlert1.png) ![přidat téměř v reálném čase metriky Alert2](./media/monitoring-near-real-time-metric-alerts/AddNRTAlert2.png)
6. Vyberte **období** času, který metriky pravidla je nutné splnit před výstrahy aktivačních událostí. Tak například používáte období "za posledních 5 minut" a vaše výstrahy vypadá pro procesoru nad 80 % (a NetworkIn výše 500 MB), můžete výstrahy se aktivuje při procesoru bylo konzistentně výše 80 % 5 minut. Jakmile dojde k první aktivační událost, se znovu spustí, když procesoru zůstává nižší než 80 % 5 minut. Výstraha je vyhodnocena podle **četnost vyhodnocení**


6. Vyberte odpovídající **závažnost** v rozevíracím seznamu.

7. Zadejte, pokud budete chtít používat nový nebo existující **akce skupiny**.

8. Pokud vyberete možnost vytvořit **nový** akce skupiny udělit skupině Akce název a krátký název, zadejte akce (Webhook služby SMS, e-mailu,) a zadat příslušné podrobnosti.


8. Vyberte **OK** po dokončení vytvoření výstrahy.   

Během několika minut výstraha je aktivní a jak se popisuje výš se aktivuje.

## <a name="managing-near-real-time-metric-alerts"></a>Správa téměř v reálném čase metriky výstrahy
Po vytvoření výstrahy, můžete ji vybrat a:

* Zobrazte graf zobrazující prahovou hodnotu metriky a skutečnými hodnotami z předchozí den.
* Upravit nebo odstranit.
* **Zakázat** nebo **povolit** ji, pokud chcete dočasně zastavit nebo obnovit příjem oznámení pro výstrahy.



