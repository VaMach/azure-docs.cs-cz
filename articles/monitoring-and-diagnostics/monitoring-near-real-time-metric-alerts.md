---
title: "Téměř v reálném čase metriky výstrah v monitorování Azure | Microsoft Docs"
description: "Další informace o použití téměř v reálném čase metriky výstrahy monitorovat metriky prostředků Azure s frekvencí co 1 minuta."
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
ms.date: 12/06/2017
ms.author: snmuvva
ms.custom: 
ms.openlocfilehash: 6370f4596e6b20962c6de0dbcbd5f86c3b7777cc
ms.sourcegitcommit: b32d6948033e7f85e3362e13347a664c0aaa04c1
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/13/2018
---
# <a name="near-real-time-metric-alerts-preview"></a>Téměř v reálném čase metriky výstrahy (preview)
Azure monitorování podporuje nové výstrahy typu s názvem téměř v reálném čase metriky výstrahy (preview). Tato funkce je aktuálně ve verzi public preview.

Téměř v reálném čase metrika výstrahy se liší od regulární metriky výstrahy několika způsoby:

- **Vylepšené latence**: téměř v reálném čase metrika výstrah můžete sledovat změny v metriky hodnoty s frekvencí co 1 minuta.
- **Větší kontrolu nad metriky podmínky**: můžete definovat bohatší pravidla výstrah v téměř v reálném čase metriky výstrahy. Výstrahy podporovat monitorování maximální, minimální, průměrná a celkové hodnoty metrik.
- **Kombinaci monitorování více metrik**: téměř v reálném čase metrika výstrah můžete sledovat více metriky (v současné době až dvě metriky) s jedním pravidlem. Výstraha se spustí, pokud obě metriky narušit jejich příslušné prahové hodnoty pro zadané časové období.
- **Modulární oznámení systému**: téměř v reálném čase metrika výstrahy použít [skupiny akcí](monitoring-action-groups.md). Akce skupiny slouží k vytvoření modulární akce. Akce skupiny pro více pravidla výstrah můžete znovu použít.

> [!NOTE]
> Near v reálném čase metriky výstrah funkce je aktuálně ve verzi public preview. Funkce a uživatelské prostředí je mohou podléhat změnám.
>

## <a name="resources-you-can-use-with-near-real-time-metric-alerts"></a>Prostředky, které můžete použít s téměř v reálném čase metriky výstrahy
Tady je úplný seznam typů prostředků, které jsou podporovány pro téměř v reálném čase metriky výstrahy:

* Microsoft.ApiManagement/service
* Microsoft.Automation/automationAccounts
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
* Microsoft.Storage/storageAccounts.
* Microsoft.Storage/storageAccounts/services
* Microsoft.StreamAnalytics/streamingjobs
* Microsoft.CognitiveServices/accounts

## <a name="near-real-time-metric-alerts-for-metrics-that-use-dimensions"></a>Téměř v reálném čase metriky výstrahy pro metriky, které používají dimenze
Téměř v reálném čase metriky výstrahy podporují generování výstrah pro metriky, které používají dimenze. Dimenze můžete použít k filtrování vaší metrika se používá k správnou úroveň. Téměř v reálném čase metrika výstrahy pro metriky, které používají dimenze jsou podporovány pro následující typy prostředků:

* Microsoft.ApiManagement/service
* Microsoft.Storage/storageAccounts (podporováno pouze pro účty úložiště v oblastech USA)
* Microsoft.Storage/storageAccounts/services (podporováno pouze pro účty úložiště v oblastech USA)

## <a name="create-a-near-real-time-metric-alert"></a>Vytvořit výstrahu metriky near v reálném čase
V současné době můžete vytvořit téměř v reálném čase metriky výstrahy pouze na portálu Azure. Podpora konfigurace téměř v reálném čase metriky výstrah pomocí prostředí PowerShell, rozhraní příkazového řádku Azure (Azure CLI) a rozhraní API REST Azure monitorování tu bude brzo dostupná.

Prostředí pro vytváření near v reálném čase metriky výstraha se přesunul do nového **výstrahy (Preview)** stránky. I v případě, že stránka zobrazuje aktuální výstrahy **přidat téměř v reálném čase metrika výstraha**, budete přesměrováni na **výstrahy (Preview)** stránky.

Další postup vytvoření metriky upozornění na near v reálném čase najdete v tématu [vytvořit pravidlo výstrahy na portálu Azure](monitor-alerts-unified-usage.md#create-an-alert-rule-with-the-azure-portal).

## <a name="manage-near-real-time-metric-alerts"></a>Spravovat téměř v reálném čase metriky výstrahy
Po vytvoření metriky upozornění na near v reálném čase, výstrahy můžete spravovat pomocí kroků popsaných v [oznámení na portálu Azure spravovat](monitor-alerts-unified-usage.md#managing-your-alerts-in-azure-portal).

## <a name="payload-schema"></a>Datová část schématu

Operaci POST obsahuje následující datové části JSON a schématu pro všechny téměř v reálném čase metriky výstrahy:

```json
{
    "WebhookName": "Alert1510875839452",
    "RequestBody": {
        "status": "Activated",
        "context": {
            "condition": {
                "metricName": "Percentage CPU",
                "metricUnit": "Percent",
                "metricValue": "17.7654545454545",
                "threshold": "1",
                "windowSize": "10",
                "timeAggregation": "Average",
                "operator": "GreaterThan"
            },
            "resourceName": "ContosoVM1",
            "resourceType": "microsoft.compute/virtualmachines",
            "resourceRegion": "westus",
            "portalLink": "https://portal.azure.com/#resource/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/automationtest/providers/Microsoft.Compute/virtualMachines/ContosoVM1",
            "timestamp": "2017-11-16T23:54:03.9517451Z",
            "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/ContosoVM/providers/microsoft.insights/alertrules/VMMetricAlert1",
            "name": "VMMetricAlert1",
            "description": "A metric alert for the VM Win2012R2",
            "conditionType": "Metric",
            "subscriptionId": "00000000-0000-0000-0000-000000000000",
            "resourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/ContosoVM/providers/Microsoft.Compute/virtualMachines/ContosoVM1",
            "resourceGroupName": "ContosoVM"
        },
        "properties": {
                "key1": "value1",
                "key2": "value2"
        }
    },
    "RequestHeader": {
        "Connection": "Keep-Alive",
        "Host": "s1events.azure-automation.net",
        "User-Agent": "azure-insights/0.9",
        "x-ms-request-id": "00000000-0000-0000-0000-000000000000"
    }
}
```

## <a name="next-steps"></a>Další postup

* Další informace o nové [výstrahy (Preview) prostředí](monitoring-overview-unified-alerts.md).
* Další informace o [protokolu výstrahy ve výstrahách Azure (Preview)](monitor-alerts-unified-log.md).
* Další informace o [výstrahy v Azure](monitoring-overview-alerts.md).