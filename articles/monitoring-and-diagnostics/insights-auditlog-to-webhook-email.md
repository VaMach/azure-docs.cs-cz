---
title: "Volat webhook, jehož protokol činnosti Azure výstrah | Microsoft Docs"
description: "Události protokolu aktivit trasy k jiným službám pro vlastní akce. Například odeslání serveru SMS, protokolu chyby nebo upozornění tým prostřednictvím chatu nebo zasílání zpráv služby service."
author: johnkemnetz
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 64d333d1-7f37-4a00-9d16-dda6e69a113b
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: johnkem
ms.openlocfilehash: 341ab32ad0ec691285fbf1537ee298ab30156a5d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="call-a-webhook-on-azure-activity-log-alerts"></a>Volat webhook, jehož protokol činnosti Azure výstrah
Webhooky umožňují směrovat Azure oznámení výstrahy k jiným systémům pro následné zpracování nebo vlastní akce. Webhook, jehož na výstrahu slouží k směrovat do služeb, které odeslání serveru SMS, protokolu chyb, upozornění tým prostřednictvím služby zasílání zpráv nebo chatu nebo provádět řadu dalších akcí. Tento článek popisuje, jak nastavit webhooku se volá, když se aktivuje upozornění protokol činnosti Azure. Také ukazuje, jak vypadá pro HTTP POST na webhook, jehož datové části. Informace o instalaci a schéma pro výstrahu Azure metriky [místo toho najdete na této stránce](insights-webhooks-alerts.md). Můžete také nastavit tak výstrahu protokolu aktivit k odesílání e-mailu, pokud je aktivován.

> [!NOTE]
> Tato funkce je aktuálně ve verzi preview a odeberou se v určitém okamžiku v budoucnu.
>
>

Můžete nastavit k protokolu aktivit výstrah pomocí [rutin prostředí Azure PowerShell](insights-powershell-samples.md#create-metric-alerts), [rozhraní příkazového řádku a platformy](insights-cli-samples.md#work-with-alerts), nebo [REST API služby Azure monitorování](https://msdn.microsoft.com/library/azure/dn933805.aspx). V současné době nelze nastavit ho vytvořit pomocí portálu Azure.

## <a name="authenticating-the-webhook"></a>Ověřování webhooku
Webhooku můžete ověřit pomocí některé z těchto metod:

1. **Na základě tokenu autorizace** -webhooku identifikátor URI je uložit s tokenu ID, například`https://mysamplealert/webcallback?tokenid=sometokenid&someparameter=somevalue`
2. **Základní ověřování** -webhooku identifikátor URI je uložit pomocí uživatelského jména a hesla, například`https://userid:password@mysamplealert/webcallback?someparamater=somevalue&foo=bar`

## <a name="payload-schema"></a>Datová část schématu
Operaci POST obsahuje následující datové části JSON a schéma pro všechny výstrahy na základě protokolu aktivit. Toto schéma je podobné používá metrika na základě výstrahy.

```
{
        "status": "Activated",
        "context": {
                "resourceProviderName": "Microsoft.Web",
                "event": {
                        "$type": "Microsoft.WindowsAzure.Management.Monitoring.Automation.Notifications.GenericNotifications.Datacontracts.InstanceEventContext, Microsoft.WindowsAzure.Management.Mon.Automation",
                        "authorization": {
                                "action": "Microsoft.Web/sites/start/action",
                                "scope": "/subscriptions/s1/resourcegroups/rg1/providers/Microsoft.Web/sites/leoalerttest"
                        },
                        "eventDataId": "327caaca-08d7-41b1-86d8-27d0a7adb92d",
                        "category": "Administrative",
                        "caller": "myname@mycompany.com",
                        "httpRequest": {
                                "clientRequestId": "f58cead8-c9ed-43af-8710-55e64def208d",
                                "clientIpAddress": "104.43.166.155",
                                "method": "POST"
                        },
                        "status": "Succeeded",
                        "subStatus": "OK",
                        "level": "Informational",
                        "correlationId": "4a40beaa-6a63-4d92-85c4-923a25abb590",
                        "eventDescription": "",
                        "operationName": "Microsoft.Web/sites/start/action",
                        "operationId": "4a40beaa-6a63-4d92-85c4-923a25abb590",
                        "properties": {
                                "$type": "Microsoft.WindowsAzure.Management.Common.Storage.CasePreservedDictionary, Microsoft.WindowsAzure.Management.Common.Storage",
                                "statusCode": "OK",
                                "serviceRequestId": "f7716681-496a-4f5c-8d14-d564bcf54714"
                        }
                },
                "timestamp": "Friday, March 11, 2016 9:13:23 PM",
                "id": "/subscriptions/s1/resourceGroups/rg1/providers/microsoft.insights/alertrules/alertonevent2",
                "name": "alertonevent2",
                "description": "test alert on event start",
                "conditionType": "Event",
                "subscriptionId": "s1",
                "resourceId": "/subscriptions/s1/resourcegroups/rg1/providers/Microsoft.Web/sites/leoalerttest",
                "resourceGroupName": "rg1"
        },
        "properties": {
                "key1": "value1",
                "key2": "value2"
        }
}
```

| Název elementu | Popis |
| --- | --- |
| status |Používá pro výstrahy, metriky. Vždy nastaven na "aktivovat" pro protokol aktivit výstrahy. |
| Kontext |Kontext události. |
| resourceProviderName |Zprostředkovatel prostředků ovlivněné prostředku. |
| conditionType |Vždy "událost". |
| jméno |Název pravidla výstrahy. |
| id |ID prostředku výstrahy. |
| description |Popis výstrahy jako sada během vytváření výstrahy |
| subscriptionId |ID předplatného Azure. |
| časové razítko |Čas, kdy byla generována událost pomocí služby Azure, který požadavek zpracoval. |
| resourceId |ID prostředku ovlivněné prostředku. |
| Název skupiny prostředků |Název skupiny prostředků ovlivněné prostředku |
| properties |Sada `<Key, Value>` páry (tj. `Dictionary<String, String>`) obsahující podrobnosti o události. |
| Události |Element obsahující metadata o události. |
| Autorizace |Vlastnosti RBAC události. Obvykle patří "action", "role" a "obor". |
| category |Kategorie události. Podporované hodnoty jsou: pro správu, výstrahy, zabezpečení, ServiceHealth, doporučení. |
| volající |E-mailovou adresu uživatele, který provádí operace, deklarace hlavní název uživatele nebo název SPN deklarace identity na základě dostupnosti. Může mít hodnotu null pro určité systémová volání. |
| correlationId |Obvykle GUID ve formátu řetězce. Události se correlationId patřit do stejné větší akce a obvykle sdílet correlationId. |
| eventDescription |Statický text popis události. |
| eventDataId |Jedinečný identifikátor pro událost. |
| EventSource |Název služby Azure nebo infrastruktury, které vygenerovalo událost. |
| požadavku HTTP |Obvykle zahrnuje "clientRequestId", "clientIpAddress" a "metodu" (například PUT metoda HTTP). |
| úroveň |Jeden z následujících hodnot: "Kritická", "Chyba", "Upozornění", "Informační" a "Verbose." |
| operationId |Obvykle GUID sdílen události odpovídající jedné operace. |
| operationName |Název operace. |
| properties |Vlastnosti události. |
| status |Řetězec. Stav operace. Běžné hodnoty jsou: "Spustit", "Probíhá", "Bylo úspěšně dokončeno", "Se nezdařilo", "Aktivní", "Přeložit". |
| Podřízený stav |Obvykle zahrnuje stavový kód HTTP odpovídající volání REST. Může také obsahovat jiných řetězců popisující podřízeného stavu. Běžné substatus hodnoty patří: OK (stavový kód HTTP: 200), které byly vytvořeny (stavový kód HTTP: 201), platné (stavový kód HTTP: 202), ne obsahu (stavový kód HTTP: 204), chybný požadavek (stavový kód HTTP: 400), nebyl nalezen (stavový kód HTTP: 404), konflikt (stavový kód HTTP: 409), vnitřní chybu serveru (kód stavu HTTP: 500), služba není k dispozici (kód stavu HTTP: 503), vypršel časový limit brány (kód stavu HTTP: 504) |

## <a name="next-steps"></a>Další kroky
* [Další informace o protokolu aktivit](monitoring-overview-activity-logs.md)
* [Spustit skripty Azure Automation (Runbooky) na Azure výstrahy](http://go.microsoft.com/fwlink/?LinkId=627081)
* [Pomocí aplikace logiky můžete odeslat zprávu SMS prostřednictvím Twilio z Azure výstrahy](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-text-message-with-logic-app). V tomto příkladu je metriky výstrahy, ale je možné upravovat pro práci s výstrahu protokolu aktivit.
* [Použití aplikace logiky odeslat zprávu Slack z Azure výstrahy](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-slack-with-logic-app). V tomto příkladu je metriky výstrahy, ale je možné upravovat pro práci s výstrahu protokolu aktivit.
* [Použití aplikace logiky odeslat zprávu do fronty Azure z Azure výstrahy](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-queue-with-logic-app). V tomto příkladu je metriky výstrahy, ale je možné upravovat pro práci s výstrahu protokolu aktivit.
