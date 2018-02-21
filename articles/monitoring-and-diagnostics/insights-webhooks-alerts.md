---
title: "Konfigurace webhooky na Azure metriky výstrahy | Microsoft Docs"
description: "Zjistěte, jak přesměrovat Azure výstrahy k systémům jiných, mimo Azure."
author: johnkemnetz
manager: carmonm
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 8b3ae540-1d19-4f3d-a635-376042f8a5bb
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/03/2017
ms.author: johnkem
ms.openlocfilehash: 049803e7701c68559103d9b1fa5dfacf820d0548
ms.sourcegitcommit: 95500c068100d9c9415e8368bdffb1f1fd53714e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/14/2018
---
# <a name="configure-a-webhook-on-an-azure-metric-alert"></a>Konfigurace webhook, jehož na výstrahu Azure metriky
Webhooky můžete používat ke směrování Azure oznámení výstrahy k jiným systémům pro následné zpracování nebo vlastní akce. Webhook, jehož na výstrahu můžete použít ke směrování ke službám, které odesílají zprávy SMS, do protokolu chyb, upozornit na tým prostřednictvím chatu nebo služby zasílání zpráv nebo pro různé další akce. 

Tento článek popisuje, jak nastavit webhooku pro výstrahu Azure metriky. Také vám ukazuje datové části pro HTTP POST na webhook, jehož vypadá jako. Informace o nastavení a schématu pro Azure aktivitu výstrahu protokolu (výstraha na událostech), najdete v části [volat webhook, jehož na výstrahu protokolu aktivita Azure](insights-auditlog-to-webhook-email.md).

Výstrahy Azure používat k odesílání výstrah obsah ve formátu JSON do webhooku identifikátor URI, který je zadat při vytváření výstrahu HTTP POST. Schéma je definována později v tomto článku. Identifikátor URI musí být platný koncový bod HTTP nebo HTTPS. Azure účtuje jeden záznam za požadavek při aktivaci výstrahy.

## <a name="configure-webhooks-via-the-azure-portal"></a>Konfigurace webhooky prostřednictvím portálu Azure
Přidat nebo aktualizovat webhook identifikátor URI, [portál Azure](https://portal.azure.com/), přejděte na **vytvořit nebo aktualizovat výstrahy**.

![Přidání podokna pravidlo výstrahy](./media/insights-webhooks-alerts/Alertwebhook.png)

Můžete také nakonfigurovat výstrahy k vystavování příspěvků na webhooku URI pomocí [rutin prostředí Azure PowerShell](insights-powershell-samples.md#create-metric-alerts), [rozhraní příkazového řádku a platformy](insights-cli-samples.md#work-with-alerts), nebo [rozhraní REST API Azure monitorování](https://msdn.microsoft.com/library/azure/dn933805.aspx).

## <a name="authenticate-the-webhook"></a>Ověření webhooku
Webhooku můžete ověřit pomocí ověřování na základě tokenu. Webhook identifikátor URI je uložit s ID tokenu. Příklad: `https://mysamplealert/webcallback?tokenid=sometokenid&someparameter=somevalue`

## <a name="payload-schema"></a>Datová část schématu
Operaci POST obsahuje následující datové části JSON a schéma pro všechny výstrahy na základě metrika:

```JSON
{
    "WebhookName": "Alert1515515157799",
    "RequestBody": {
        "status": "Activated",
        "context": {
            "timestamp": "2015-08-14T22:26:41.9975398Z",
            "id": "/subscriptions/s1/resourceGroups/useast/providers/microsoft.insights/alertrules/ruleName1",
            "name": "ruleName1",
            "description": "some description",
            "conditionType": "Metric",
            "condition": {
                "metricName": "Requests",
                "metricUnit": "Count",
                "metricValue": "10",
                "threshold": "10",
                "windowSize": "15",
                "timeAggregation": "Average",
                "operator": "GreaterThanOrEqual"
            },
            "subscriptionId": "s1",
            "resourceGroupName": "useast",
            "resourceName": "mysite1",
            "resourceType": "microsoft.foo/sites",
            "resourceId": "/subscriptions/s1/resourceGroups/useast/providers/microsoft.foo/sites/mysite1",
            "resourceRegion": "centralus",
            "portalLink": "https://portal.azure.com/#resource/subscriptions/s1/resourceGroups/useast/providers/microsoft.foo/sites/mysite1"
        },
        "properties": {
            "key1": "value1",
            "key2": "value2"
        }
    }
}
```


| Pole | Povinné | Opravené sadu hodnot | Poznámky |
|:--- |:--- |:--- |:--- |
| status |Ano |Aktivovat, vyřešit |Stav upozornění na základě podmínek, můžete nastavit. |
| Kontext |Ano | |Kontext výstrahy. |
| časové razítko |Ano | |Čas, kdy byla výstraha. |
| id |Ano | |Každé pravidlo výstrahy má jedinečný identifikátor. |
| jméno |Ano | |Název výstrahy. |
| description |Ano | |Popis výstrahy. |
| conditionType |Ano |Metrika událostí |Podporuje dva typy výstrah: metriky a události. Metriky výstrahy jsou založené na podmínce metriky. Událost výstrahy jsou založené na události v protokolu aktivit. Tato hodnota se používá ke kontrole, jestli je výstraha založené na metriky nebo na událost. |
| podmínka |Ano | |Na základě konkrétních polí zkontrolujte **conditionType** hodnotu. |
| metricName |Metriky výstrahy | |Název metriky, která definuje, co pravidlo monitoruje. |
| metricUnit |Metriky výstrahy |Bytes, BytesPerSecond, Count, CountPerSecond, Percent, Seconds |Jednotka v metriku povoleny. V tématu [povolené hodnoty](https://msdn.microsoft.com/library/microsoft.azure.insights.models.unit.aspx). |
| metricValue |Metriky výstrahy | |Aktuální hodnota metriku, která způsobila výstrahu. |
| Prahová hodnota |Metriky výstrahy | |Prahová hodnota, na které se aktivuje výstrahu. |
| velikost_okna |Metriky výstrahy | |Časový interval, který se používá k monitorování výstrah aktivity podle prahovou hodnotu. Hodnota musí být mezi 5 minutami a 1 den. Hodnota musí být ve formátu ISO 8601 doba trvání. |
| Agregace času |Metriky výstrahy |Průměrná, poslední, maximální a minimální, None, celkový počet |Data, která se shromažďují jak by měla být kombinovány časem. Výchozí hodnota je průměr. V tématu [povolené hodnoty](https://msdn.microsoft.com/library/microsoft.azure.insights.models.aggregationtype.aspx). |
| Operátor |Metriky výstrahy | |Operátor, který se používá k porovnání aktuální metriky data, která mají nastavenou prahovou hodnotu. |
| subscriptionId |Ano | |ID předplatného Azure. |
| resourceGroupName |Ano | |Název skupiny prostředků pro ovlivněných prostředků. |
| resourceName |Ano | |Název prostředku ovlivněných prostředků. |
| resourceType |Ano | |Typ prostředku ovlivněných prostředků. |
| resourceId |Ano | |ID prostředku ovlivněných prostředků. |
| resourceRegion |Ano | |Oblast nebo umístění zdroje. |
| portalLink |Ano | |Přímý odkaz na stránce Souhrn portálu prostředků. |
| properties |N |Nepovinné |Sada dvojic klíč/hodnota, který obsahuje podrobnosti o události. Například, `Dictionary<String, String>`. Vlastnosti pole je nepovinné. Do vlastní uživatelské rozhraní nebo pracovních postupů založených na aplikaci logiky mohou uživatelé zadat dvojice klíč/hodnota, které lze předat prostřednictvím datové části. Jiný způsob, jak předat vlastní vlastnosti webhooku je prostřednictvím webhooku URI samotné (jako parametry dotazu). |

> [!NOTE]
> Můžete nastavit **vlastnosti** pole pouze pomocí [rozhraní REST API Azure monitorování](https://msdn.microsoft.com/library/azure/dn933805.aspx).
>
>

## <a name="next-steps"></a>Další postup
* Další informace o Azure výstrahy a pomocí webhooků ve videu [integrovat Azure výstrahy s PagerDuty](http://go.microsoft.com/fwlink/?LinkId=627080).
* Zjistěte, jak [spustit skripty Azure Automation (runbooky) na Azure výstrahy](http://go.microsoft.com/fwlink/?LinkId=627081).
* Zjistěte, jak [použití aplikace logiky k odeslání zprávy SMS prostřednictvím Twilio z Azure výstrahy](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-text-message-with-logic-app).
* Zjistěte, jak [použití aplikace logiky odeslat zprávu Slack z Azure výstrahy](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-slack-with-logic-app).
* Zjistěte, jak [použití aplikace logiky k odeslání zprávy do fronty Azure z Azure výstrahy](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-queue-with-logic-app).
