---
title: "Konfigurace webhooky na Azure metriky výstrahy | Microsoft Docs"
description: "Přesměrovat Azure výstrahy k jiným systémům mimo Azure."
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
ms.openlocfilehash: 1a885166e5c71f13da222bfc22b0fc579096c52f
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="configure-a-webhook-on-an-azure-metric-alert"></a>Konfigurace webhook, jehož na výstrahu Azure metriky
Webhooky umožňují směrovat Azure oznámení výstrahy k jiným systémům pro následné zpracování nebo vlastní akce. Webhook, jehož na výstrahu slouží k směrovat do služeb, které odeslání serveru SMS, protokolu chyb, upozornění tým prostřednictvím služby zasílání zpráv nebo chatu nebo provádět řadu dalších akcí. Tento článek popisuje, jak nastavit webhook, jehož na výstrahu Azure metriky a datové části pro HTTP POST na webhook, jehož vypadá jako. Informace o instalaci a schéma pro výstrahu protokol činnosti Azure (výstraha na událostech) [místo toho najdete na této stránce](insights-auditlog-to-webhook-email.md).

Azure výstrahy HTTP POST výstrahy obsah ve formátu JSON formátu, schéma definované níže na webhooku identifikátor URI, který je zadat při vytváření výstrahu. Tento identifikátor URI musí být platný koncový bod HTTP nebo HTTPS. Azure účtuje jeden záznam za požadavek při aktivaci výstrahy.

## <a name="configuring-webhooks-via-the-portal"></a>Konfigurace webhooky prostřednictvím portálu
Můžete přidat nebo aktualizovat webhook URI v dialogovém okně Vytvořit nebo aktualizovat výstrahy v [portál](https://portal.azure.com/).

![Přidání pravidla výstrahy](./media/insights-webhooks-alerts/Alertwebhook.png)

Můžete také nakonfigurovat výstrahy k vystavování příspěvků na webhooku URI pomocí [rutin prostředí Azure PowerShell](insights-powershell-samples.md#create-metric-alerts), [rozhraní příkazového řádku a platformy](insights-cli-samples.md#work-with-alerts), nebo [REST API služby Azure monitorování](https://msdn.microsoft.com/library/azure/dn933805.aspx).

## <a name="authenticating-the-webhook"></a>Ověřování webhooku
Webhooku můžete ověřit pomocí ověřování na základě tokenu. Webhook identifikátor URI je uložit s tokenu ID, např. `https://mysamplealert/webcallback?tokenid=sometokenid&someparameter=somevalue`

## <a name="payload-schema"></a>Datová část schématu
Operaci POST obsahuje následující datové části JSON a schéma pro všechny výstrahy na základě metriky.

```JSON
{
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
```


| Pole | Povinné | Opravené sadu hodnot | Poznámky |
|:--- |:--- |:--- |:--- |
| status |Ano |"Aktivovaná", "Přeložit" |Stav upozornění na základě podmínky, že jste nastavili. |
| Kontext |Ano | |Kontext výstrahy. |
| časové razítko |Ano | |Čas, kdy byla výstraha. |
| id |Ano | |Každé pravidlo výstrahy má jedinečné id. |
| jméno |Ano | |Název výstrahy. |
| Popis |Ano | |Popis výstrahy. |
| conditionType |Ano |"Metriky", "Událost" |Podporuje dva typy výstrah. Jeden založené na podmínce metriky a dalších založené na události v protokolu aktivit. Tato hodnota se používá ke kontrole, pokud je výstraha podle metrika nebo událostí. |
| Podmínka |Ano | |Na conditionType na základě konkrétních polí zkontrolujte. |
| metricName |Metriky výstrahy | |Název metriky, která definuje, co pravidlo monitoruje. |
| metricUnit |Metriky výstrahy |"Bajtů", "BytesPerSecond", "Count", "CountPerSecond", "Procenta", "Seconds" |Jednotka v metriku povoleny. [Povolené hodnoty jsou zde uvedeny](https://msdn.microsoft.com/library/microsoft.azure.insights.models.unit.aspx). |
| metricValue |Metriky výstrahy | |Aktuální hodnota metriku, která způsobila výstrahu. |
| Prahová hodnota |Metriky výstrahy | |Prahová hodnota, na které se aktivuje výstrahu. |
| velikost_okna |Metriky výstrahy | |Časový interval, který se používá k monitorování výstrah aktivity podle prahovou hodnotu. Musí být mezi 5 minutami a 1 dnem. Doba trvání formátu ISO 8601. |
| Agregace času |Metriky výstrahy |"Průměru", "Posledního", "Maximální", "Minimální", "Žádný", "celkové" |Data, která se shromažďují jak by měla být kombinovány časem. Výchozí hodnota je průměr. [Povolené hodnoty jsou zde uvedeny](https://msdn.microsoft.com/library/microsoft.azure.insights.models.aggregationtype.aspx). |
| Operátor |Metriky výstrahy | |Operátor použit k porovnání aktuální metriky data, která mají nastavenou prahovou hodnotu. |
| subscriptionId |Ano | |ID předplatného Azure. |
| Název skupiny prostředků |Ano | |Název skupiny prostředků pro prostředek dopad. |
| resourceName |Ano | |Název prostředku ovlivněné prostředku. |
| Typ prostředku |Ano | |Typ prostředku ovlivněné prostředku. |
| resourceId |Ano | |ID prostředku ovlivněné prostředku. |
| resourceRegion |Ano | |Oblast nebo umístění ovlivněné prostředku. |
| portalLink |Ano | |Přímý odkaz na stránce Souhrn portálu prostředků. |
| properties |N |Nepovinné |Sada `<Key, Value>` páry (tj. `Dictionary<String, String>`) obsahující podrobnosti o události. Vlastnosti pole je nepovinné. Do vlastní uživatelské rozhraní nebo logiku aplikace na základě pracovní postup mohou uživatelé zadat klíč/hodnota, které lze předat prostřednictvím datové části. Alternativní způsob předat vlastní vlastnosti webhooku je prostřednictvím uri webhooku (jako parametry dotazu) |

> [!NOTE]
> Pole vlastnosti lze nastavit pouze pomocí [REST API služby Azure monitorování](https://msdn.microsoft.com/library/azure/dn933805.aspx).
>
>

## <a name="next-steps"></a>Další kroky
* Další informace o Azure výstrahy a pomocí webhooků ve videu [integrovat Azure výstrahy s PagerDuty](http://go.microsoft.com/fwlink/?LinkId=627080)
* [Spustit skripty Azure Automation (Runbooky) na Azure výstrahy](http://go.microsoft.com/fwlink/?LinkId=627081)
* [Pomocí aplikace logiky můžete odeslat zprávu SMS prostřednictvím Twilio z Azure výstrahy](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-text-message-with-logic-app)
* [Použití aplikace logiky odeslat zprávu Slack z Azure výstrahy](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-slack-with-logic-app)
* [Použití aplikace logiky odeslat zprávu do fronty Azure z Azure výstrahy](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-queue-with-logic-app)
