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
ms.openlocfilehash: 08467aed4e1601b32598fc42515d9c38b601a9d4
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/19/2018
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

```json
{
    "WebhookName": "Alert1515526229589",
    "RequestBody": {
        "schemaId": "Microsoft.Insights/activityLogs",
        "data": {
            "status": "Activated",
            "context": {
                "activityLog": {
                    "authorization": {
                        "action": "Microsoft.Compute/virtualMachines/deallocate/action",
                        "scope": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/ContosoVM/providers/Microsoft.Compute/virtualMachines/ContosoVM1"
                    },
                    "channels": "Operation",
                    "claims": {
                        "aud": "https://management.core.windows.net/",
                        "iss": "https://sts.windows.net/00000000-0000-0000-0000-000000000000/",
                        "iat": "1234567890",
                        "nbf": "1234567890",
                        "exp": "1234567890",
                        "aio": "Y2NgYBD8ZLlhu27JU6WZsXemMIvVAAA=",
                        "appid": "00000000-0000-0000-0000-000000000000",
                        "appidacr": "2",
                        "e_exp": "262800",
                        "http://schemas.microsoft.com/identity/claims/identityprovider": "https://sts.windows.net/00000000-0000-0000-0000-000000000000/",
                        "http://schemas.microsoft.com/identity/claims/objectidentifier": "00000000-0000-0000-0000-000000000000",
                        "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier": "00000000-0000-0000-0000-000000000000",
                        "http://schemas.microsoft.com/identity/claims/tenantid": "00000000-0000-0000-0000-000000000000",
                        "uti": "XnCk46TrDkOQXwo49Y8fAA",
                        "ver": "1.0"
                    },
                    "caller": "00000000-0000-0000-0000-000000000000",
                    "correlationId": "00000000-0000-0000-0000-000000000000",
                    "description": "",
                    "eventSource": "Administrative",
                    "eventTimestamp": "2018-01-09T20:11:25.8410967+00:00",
                    "eventDataId": "00000000-0000-0000-0000-000000000000",
                    "level": "Informational",
                    "operationName": "Microsoft.Compute/virtualMachines/deallocate/action",
                    "operationId": "00000000-0000-0000-0000-000000000000",
                    "resourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/ContosoVM/providers/Microsoft.Compute/virtualMachines/ContosoVM1",
                    "resourceGroupName": "ContosoVM",
                    "resourceProviderName": "Microsoft.Compute",
                    "status": "Succeeded",
                    "subStatus": "",
                    "subscriptionId": "00000000-0000-0000-0000-000000000000",
                    "submissionTimestamp": "2018-01-09T20:11:40.2986126+00:00",
                    "resourceType": "Microsoft.Compute/virtualMachines"
                }
            },
            "properties": {}
        }
    },
    "RequestHeader": {
        "Expect": "100-continue",
        "Host": "s1events.azure-automation.net",
        "User-Agent": "IcMBroadcaster/1.0",
        "X-CorrelationContext": "RkkKACgAAAACAAAAEADlBbM7x86VTrHdQ2JlmlxoAQAQALwazYvJ/INPskb8S5QzgDk=",
        "x-ms-request-id": "00000000-0000-0000-0000-000000000000"
    }
}
```

| Název elementu | Popis |
| --- | --- |
| status |Používá pro výstrahy, metriky. Vždy nastaven na "aktivovat" pro protokol aktivit výstrahy. |
| Kontext |Kontext události. |
| activityLog | Vlastnosti protokolu události.|
| Autorizace |Vlastnosti RBAC události. Obvykle patří "action", "role" a "obor". |
| akce | Akce zachycenou výstrahy. |
| Obor | Obor výstrahy (tj. prostředek).|
| kanály | Operace |
| Deklarace identity | Kolekce informací je v má vztah k deklarace identity. |
| volající |Identifikátor GUID nebo uživatelské jméno uživatele, který provádí operace, deklarace hlavní název uživatele nebo název SPN deklarace identity na základě dostupnosti. Může mít hodnotu null pro určité systémová volání. |
| correlationId |Obvykle GUID ve formátu řetězce. Události se correlationId patřit do stejné větší akce a obvykle sdílet correlationId. |
| description |Popis výstrahy jako sada během vytváření výstrahy |
| EventSource |Název služby Azure nebo infrastruktury, které vygenerovalo událost. |
| eventTimestamp |Čas, kdy k události došlo. |
| eventDataId |Jedinečný identifikátor pro událost. |
| úroveň |Jeden z následujících hodnot: "Kritická", "Chyba", "Upozornění", "Informační" a "Verbose." |
| operationName |Název operace. |
| operationId |Obvykle GUID sdílen události odpovídající jedné operace. |
| resourceId |ID prostředku ovlivněné prostředku. |
| resourceGroupName |Název skupiny prostředků ovlivněné prostředku |
| resourceProviderName |Zprostředkovatel prostředků ovlivněné prostředku. |
| status |Řetězec. Stav operace. Běžné hodnoty jsou: "Spustit", "Probíhá", "Bylo úspěšně dokončeno", "Se nezdařilo", "Aktivní", "Přeložit". |
| subStatus |Obvykle zahrnuje stavový kód HTTP odpovídající volání REST. Může také obsahovat jiných řetězců popisující podřízeného stavu. Běžné substatus hodnoty patří: OK (stavový kód HTTP: 200), které byly vytvořeny (stavový kód HTTP: 201), platné (stavový kód HTTP: 202), ne obsahu (stavový kód HTTP: 204), chybný požadavek (stavový kód HTTP: 400), nebyl nalezen (stavový kód HTTP: 404), konflikt (stavový kód HTTP: 409), vnitřní chybu serveru (kód stavu HTTP: 500), služba není k dispozici (kód stavu HTTP: 503), vypršel časový limit brány (kód stavu HTTP: 504) |
| subscriptionId |ID předplatného Azure. |
| submissionTimestamp |Čas, kdy byla generována událost pomocí služby Azure, který požadavek zpracoval. |
| resourceType | Typ prostředku, které vygenerovalo událost.|
| properties |Sada `<Key, Value>` páry (tj. `Dictionary<String, String>`) obsahující podrobnosti o události. |

## <a name="next-steps"></a>Další postup
* [Další informace o protokolu aktivit](monitoring-overview-activity-logs.md)
* [Spustit skripty Azure Automation (Runbooky) na Azure výstrahy](http://go.microsoft.com/fwlink/?LinkId=627081)
* [Pomocí aplikace logiky můžete odeslat zprávu SMS prostřednictvím Twilio z Azure výstrahy](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-text-message-with-logic-app). V tomto příkladu je metriky výstrahy, ale je možné upravovat pro práci s výstrahu protokolu aktivit.
* [Použití aplikace logiky odeslat zprávu Slack z Azure výstrahy](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-slack-with-logic-app). V tomto příkladu je metriky výstrahy, ale je možné upravovat pro práci s výstrahu protokolu aktivit.
* [Použití aplikace logiky odeslat zprávu do fronty Azure z Azure výstrahy](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-queue-with-logic-app). V tomto příkladu je metriky výstrahy, ale je možné upravovat pro práci s výstrahu protokolu aktivit.
