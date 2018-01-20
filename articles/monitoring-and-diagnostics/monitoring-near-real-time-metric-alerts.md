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
ms.date: 12/06/2017
ms.author: snmuvva
ms.custom: 
ms.openlocfilehash: d3e88a98e0ba93a630d131c25ca4dd5cb16f1b1a
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/19/2018
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

## <a name="near-real-time-metric-alerts-on-metrics-with-dimensions"></a>NEAR výstrahy metriky v reálném čase na metriky s dimenzí
Téměř v reálném čase metrika výstrahy podporuje výstrahy na metriky s dimenzí. Dimenze představují způsob, jak filtrovat vaší metrika se používá k správnou úroveň. Téměř v reálném čase metrika výstrahy na metriky s dimenzí podporují pro následující typy prostředků

* Microsoft.ApiManagement/service
* Microsoft.Storage/storageAccounts (podporováno pouze pro účty úložiště v oblastech USA)
* Microsoft.Storage/storageAccounts/services (podporováno pouze pro účty úložiště v oblastech USA)


## <a name="create-a-near-real-time-metric-alert"></a>Vytvořit výstrahu metriky Near v reálném čase
V současné době téměř v reálném čase metrika výstrahy lze vytvořit pouze prostřednictvím portálu Azure. Podpora konfigurace téměř v reálném čase metriky výstrah pomocí prostředí PowerShell, rozhraní příkazového řádku (CLI) a rozhraní REST API Azure monitorování tu bude brzo dostupná.

Výstrahy prostředí téměř v reálném čase metrika výstrahy se přesunul do nového **Alerts(Preview)** prostředí. Přestože aktuální výstrahy stránka zobrazuje **přidat téměř v reálném čase metrika výstraha**, budete přesměrováni na nové prostředí.

Můžete vytvořit near v reálném čase metriky výstrahu pomocí kroků popsaných [zde](monitor-alerts-unified-usage.md#create-an-alert-rule-with-the-azure-portal).

## <a name="managing-near-real-time-metric-alerts"></a>Správa téměř v reálném čase metriky výstrahy
Po vytvoření **téměř v reálném čase metrika výstraha**, je možné spravovat pomocí kroků popsaných [zde](monitor-alerts-unified-usage.md#managing-your-alerts-in-azure-portal).

## <a name="payload-schema"></a>Datová část schématu

Operaci POST obsahuje následující datové části JSON a schématu pro všechny téměř v reálném čase metriky výstrahy.

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

* [Další informace o nové rozhraní výstrahy (preview)](monitoring-overview-unified-alerts.md)
* [Další informace o protokolu výstrahy ve výstrahách Azure (preview)](monitor-alerts-unified-log.md)
* [Další informace o výstrahách v Azure](monitoring-overview-alerts.md)