---
title: "Stream Azure diagnostické protokoly na Namespace centra událostí | Microsoft Docs"
description: "Zjistěte, jak k vysílání datového proudu Azure diagnostické protokoly na obor názvů služby Event Hubs."
author: johnkemnetz
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 42bc4845-c564-4568-b72d-0614591ebd80
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/21/2017
ms.author: johnkem
ms.openlocfilehash: 01ba8ddfcf90e1368ac147296fd180f99420d96f
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="stream-azure-diagnostic-logs-to-an-event-hubs-namespace"></a>Stream Azure diagnostické protokoly na Namespace centra událostí
**[Azure diagnostické protokoly](monitoring-overview-of-diagnostic-logs.md)**  Streamovat skoro v reálném čase pro všechny aplikace pomocí předdefinované možnosti "Export do služby Event Hubs" na portálu nebo povolením ID Service Bus pravidla v nastavení diagnostiky prostřednictvím rutin prostředí Azure PowerShell nebo rozhraní příkazového řádku Azure.

## <a name="what-you-can-do-with-diagnostics-logs-and-event-hubs"></a>Co můžete dělat s protokolů diagnostiky a Event Hubs
Můžete použít k diagnostickým protokolům streamování schopností několika způsoby:

* **Stream protokoluje události do 3. stran protokolování a telemetrie systémy** – v čase, streamování Event Hubs se stane mechanismus kanálem k diagnostickým protokolům v jiných systémů Siem a řešení pro analýzu protokolu.
* **Zobrazit stav služby streamování "aktivní cesta" dat do PowerBI** – pomocí služby Event Hubs, Stream Analytics a PowerBI, můžete snadno transformovat data diagnostiky v poblíž přehledy v reálném čase na služeb Azure. [V tomto článku dokumentace podává přehled o tom, jak nastavit službu Event Hubs, zpracování dat pomocí služby Stream Analytics a použít PowerBI jako výstup](../stream-analytics/stream-analytics-power-bi-dashboard.md). Zde je několik tipů pro získání nastavení diagnostické protokoly.
  
  * Centra událostí pro kategorii diagnostické protokoly se vytvoří automaticky, když zaškrtnutí políčka na portálu nebo povolit pomocí prostředí PowerShell, kterou chcete vybrat centra událostí v oboru názvů s názvem, který začíná **insights -**.
  * Následující kód SQL je ukázkový dotaz služby Stream Analytics, který můžete použít k analýze všechna data protokolu v do PowerBI tabulky:

    ```sql
    SELECT
    records.ArrayValue.[Properties you want to track]
    INTO
    [OutputSourceName – the PowerBI source]
    FROM
    [InputSourceName] AS e
    CROSS APPLY GetArrayElements(e.records) AS records
    ```

* **Vytvoření vlastní telemetrii a protokolování platformy** – Pokud už máte uživatelské telemetrie platformy nebo jsou jenom přemýšlíte o vytváření jeden vysoce škálovatelné, publikování a odběru na povaze služby Event Hubs umožňuje flexibilně ingestování diagnostické protokoly. [Naleznete v Průvodci Dana Rosanova pomocí služby Event Hubs telemetrie platformy globálním měřítku zde](https://azure.microsoft.com/documentation/videos/build-2015-designing-and-sizing-a-global-scale-telemetry-platform-on-azure-event-Hubs/).

## <a name="enable-streaming-of-diagnostic-logs"></a>Povolení diagnostických protokolů streamování
Můžete povolit vysílání datového proudu diagnostické protokoly prostřednictvím kódu programu, prostřednictvím portálu nebo pomocí [rozhraní REST API Azure monitorování](https://docs.microsoft.com/rest/api/monitor/servicediagnosticsettings). V obou případech vytvoříte nastavení diagnostiky v němž jsou uvedeny na obor názvů služby Event Hubs a protokolu kategorií a metriky, které se mají posílat do oboru názvů. Centra událostí je vytvořen v oboru názvů pro každou kategorii protokolu, které povolíte. Diagnostika **kategorie protokolu** je typ protokolu, který může shromažďovat prostředku.

> [!WARNING]
> Povolení a vysílání datového proudu diagnostických protokolů z výpočetní prostředky (například virtuální počítače nebo Service Fabric) [vyžaduje jinou sadu kroků](../event-hubs/event-hubs-streaming-azure-diags-data.md).
> 
> 

Obor názvů Service Bus nebo Event Hubs nemusí být ve stejném předplatném jako prostředek emitování protokoly tak dlouho, dokud uživatel, který konfiguruje nastavení, má odpovídající přístup RBAC do oba odběry.

## <a name="stream-diagnostic-logs-using-the-portal"></a>Diagnostické protokoly datového proudu pomocí portálu
1. Na portálu, přejděte do monitorování Azure a klikněte na **nastavení diagnostiky**

    ![Monitorování části monitorování Azure](media/monitoring-stream-diagnostic-logs-to-event-hubs/diagnostic-settings-blade.png)

2. Volitelně můžete seznam filtrovat podle skupiny prostředků nebo typ prostředku, a potom klikněte na prostředek, pro kterou chcete provést nastavení diagnostiky.

3. Pokud neexistuje žádné nastavení na prostředku jste vybrali, zobrazí se výzva k vytvoření nastavení. Klikněte na tlačítko "Zapnout diagnostiky."

   ![Přidat nastavení diagnostiky - žádná existující nastavení](media/monitoring-stream-diagnostic-logs-to-event-hubs/diagnostic-settings-none.png)

   Pokud máte stávající nastavení na prostředek, zobrazí se seznam nastavení, které jsou již nakonfigurována na tomto prostředku. Klikněte na tlačítko "Přidat nastavení diagnostiky."

   ![Přidat nastavení diagnostiky - stávající nastavení](media/monitoring-stream-diagnostic-logs-to-event-hubs/diagnostic-settings-multiple.png)

3. Zadejte název nastavení a zaškrtněte políčko pro **datový proud do centra událostí**, pak vyberte obor názvů Event Hubs.
   
   ![Přidat nastavení diagnostiky - stávající nastavení](media/monitoring-stream-diagnostic-logs-to-event-hubs/diagnostic-settings-configure.png)
    
   Obor názvů vybrané bude kde vytvoření (pokud to vaše první čas diagnostické protokoly streamování) nebo prostřednictvím datového proudu do centra událostí (Pokud již existují prostředky, které jsou streamování této kategorie protokolu na tento obor názvů), a definuje zásady oprávnění, která má streamování mechanismus. V současné době streamování do centra událostí vyžadují oprávnění spravovat, odeslání a naslouchání. Můžete vytvářet nebo upravovat Event Hubs obor názvů sdílených zásad přístupu na portálu na kartě Konfigurace oboru názvů. Pokud chcete aktualizovat jednu z těchto nastavení diagnostiky, klient musí mít oprávnění ListKey na autorizační pravidlo Event Hubs.

4. Klikněte na **Uložit**.

Po chvíli se nové nastavení se zobrazí v seznamu nastavení pro tento prostředek a diagnostické protokoly jsou datového proudu k tomuto účtu úložiště, také se vygeneruje nová data události.

### <a name="via-powershell-cmdlets"></a>Pomocí rutin prostředí PowerShell
Povolit vysílání datového proudu prostřednictvím [rutin prostředí Azure PowerShell](insights-powershell-samples.md), můžete použít `Set-AzureRmDiagnosticSetting` rutiny s těmito parametry:

```powershell
Set-AzureRmDiagnosticSetting -ResourceId [your resource ID] -ServiceBusRuleId [your Service Bus rule ID] -Enabled $true
```

ID pravidla Service Bus je řetězec s Tento formát: `{Service Bus resource ID}/authorizationrules/{key name}`, například `/subscriptions/{subscription ID}/resourceGroups/Default-ServiceBus-WestUS/providers/Microsoft.ServiceBus/namespaces/{Service Bus namespace}/authorizationrules/RootManageSharedAccessKey`.

### <a name="via-azure-cli"></a>Prostřednictvím rozhraní příkazového řádku Azure
Povolit vysílání datového proudu prostřednictvím [rozhraní příkazového řádku Azure](insights-cli-samples.md), můžete použít `insights diagnostic set` příkaz takto:

```azurecli
azure insights diagnostic set --resourceId <resourceID> --serviceBusRuleId <serviceBusRuleID> --enabled true
```

Jak je popsáno pro rutinu prostředí PowerShell, použijte stejný formát pro ID pravidla Service Bus.

## <a name="how-do-i-consume-the-log-data-from-event-hubs"></a>Způsob, jakým využívají data protokolu ze služby Event Hubs?
Zde je ukázka výstupní data ze služby Event Hubs:

```json
{
    "records": [
        {
            "time": "2016-07-15T18:00:22.6235064Z",
            "workflowId": "/SUBSCRIPTIONS/DF602C9C-7AA0-407D-A6FB-EB20C8BD1192/RESOURCEGROUPS/JOHNKEMTEST/PROVIDERS/MICROSOFT.LOGIC/WORKFLOWS/JOHNKEMTESTLA",
            "resourceId": "/SUBSCRIPTIONS/DF602C9C-7AA0-407D-A6FB-EB20C8BD1192/RESOURCEGROUPS/JOHNKEMTEST/PROVIDERS/MICROSOFT.LOGIC/WORKFLOWS/JOHNKEMTESTLA/RUNS/08587330013509921957/ACTIONS/SEND_EMAIL",
            "category": "WorkflowRuntime",
            "level": "Error",
            "operationName": "Microsoft.Logic/workflows/workflowActionCompleted",
            "properties": {
                "$schema": "2016-04-01-preview",
                "startTime": "2016-07-15T17:58:55.048482Z",
                "endTime": "2016-07-15T18:00:22.4109204Z",
                "status": "Failed",
                "code": "BadGateway",
                "resource": {
                    "subscriptionId": "df602c9c-7aa0-407d-a6fb-eb20c8bd1192",
                    "resourceGroupName": "JohnKemTest",
                    "workflowId": "243aac67fe904cf195d4a28297803785",
                    "workflowName": "JohnKemTestLA",
                    "runId": "08587330013509921957",
                    "location": "westus",
                    "actionName": "Send_email"
                },
                "correlation": {
                    "actionTrackingId": "29a9862f-969b-4c70-90c4-dfbdc814e413",
                    "clientTrackingId": "08587330013509921958"
                }
            }
        },
        {
            "time": "2016-07-15T18:01:15.7532989Z",
            "workflowId": "/SUBSCRIPTIONS/DF602C9C-7AA0-407D-A6FB-EB20C8BD1192/RESOURCEGROUPS/JOHNKEMTEST/PROVIDERS/MICROSOFT.LOGIC/WORKFLOWS/JOHNKEMTESTLA",
            "resourceId": "/SUBSCRIPTIONS/DF602C9C-7AA0-407D-A6FB-EB20C8BD1192/RESOURCEGROUPS/JOHNKEMTEST/PROVIDERS/MICROSOFT.LOGIC/WORKFLOWS/JOHNKEMTESTLA/RUNS/08587330012106702630/ACTIONS/SEND_EMAIL",
            "category": "WorkflowRuntime",
            "level": "Information",
            "operationName": "Microsoft.Logic/workflows/workflowActionStarted",
            "properties": {
                "$schema": "2016-04-01-preview",
                "startTime": "2016-07-15T18:01:15.5828115Z",
                "status": "Running",
                "resource": {
                    "subscriptionId": "df602c9c-7aa0-407d-a6fb-eb20c8bd1192",
                    "resourceGroupName": "JohnKemTest",
                    "workflowId": "243aac67fe904cf195d4a28297803785",
                    "workflowName": "JohnKemTestLA",
                    "runId": "08587330012106702630",
                    "location": "westus",
                    "actionName": "Send_email"
                },
                "correlation": {
                    "actionTrackingId": "042fb72c-7bd4-439e-89eb-3cf4409d429e",
                    "clientTrackingId": "08587330012106702632"
                }
            }
        }
    ]
}
```

| Název elementu | Popis |
| --- | --- |
| záznamy |Pole všechny protokolu události v této datové části. |
| time |Čas, kdy došlo k události. |
| category |Kategorie protokolu pro tuto událost. |
| resourceId |ID prostředku prostředku, který vytvořil tuto událost. |
| operationName |Název operace. |
| úroveň |Volitelné. Určuje úroveň protokolu událostí. |
| properties |Vlastnosti události. |

Můžete zobrazit seznam všech poskytovatelů prostředků, které podporují streamování Event Hubs [zde](monitoring-overview-of-diagnostic-logs.md).

## <a name="stream-data-from-compute-resources"></a>Datový proud dat z výpočetní prostředky
Můžete také stream diagnostických protokolů z výpočetní prostředky pomocí agenta Windows Azure Diagnostics. [Najdete v článku](../event-hubs/event-hubs-streaming-azure-diags-data.md) jak k tomuto nastavení.

## <a name="next-steps"></a>Další kroky
* [Další informace o diagnostických protokolů Azure.](monitoring-overview-of-diagnostic-logs.md)
* [Začínáme s Event Hubs](../event-hubs/event-hubs-csharp-ephcs-getstarted.md)

