---
title: "Pomocí OMS Log Analytics výstrahy REST API"
description: "Log Analytics výstrahy REST API umožňuje vytvářet a spravovat výstrahy v analýzy protokolů, která je součástí služby Operations Management Suite (OMS).  Tento článek obsahuje podrobné informace o rozhraní API a několik příkladů pro provádění různých akcí."
services: log-analytics
documentationcenter: 
author: bwren
manager: jwhit
editor: tysonn
ms.assetid: 628ad256-7181-4a0d-9e68-4ed60c0f3f04
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/12/2017
ms.author: bwren
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 5ce72ffef4394bf3bbe39fa420c4fcaa965ae35c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-and-manage-alert-rules-in-log-analytics-with-rest-api"></a>Vytvářet a spravovat pravidla výstrah v analýzy protokolů pomocí rozhraní REST API
Log Analytics výstrahy REST API umožňuje vytvářet a spravovat výstrahy v Operations Management Suite (OMS).  Tento článek obsahuje podrobné informace o rozhraní API a několik příkladů pro provádění různých akcí.

Rozhraní REST API Log Analytics Search je dosáhl standardu RESTful a je přístupný prostřednictvím rozhraní REST API Azure Resource Manager. V tomto dokumentu najdete příklady kterých je přístup k rozhraní API z příkazového řádku pomocí prostředí PowerShell [ARMClient](https://github.com/projectkudu/ARMClient), nástroj pro příkazový řádek s otevřeným zdrojem, který zjednodušuje volání rozhraní API služby Azure Resource Manager. Použití ARMClient a prostředí PowerShell je jedním z mnoha možností pro přístup k rozhraní API pro vyhledávání Analytics protokolu. Pomocí těchto nástrojů můžete využít rozhraní RESTful API Správce prostředků Azure provádět volání do OMS pracovních prostorů a provádět příkazy vyhledávání v nich. Rozhraní API výstup výsledků vyhledávání do je ve formátu JSON, budete moci použít výsledky hledání v mnoha různými způsoby prostřednictvím kódu programu.

## <a name="prerequisites"></a>Požadavky
V současné době mohou výstrahy vytvořeny pouze s uloženého hledání v analýzy protokolů.  Můžete se podívat do [rozhraní API REST vyhledávání protokolu](log-analytics-log-search-api.md) Další informace.

## <a name="schedules"></a>Plány
Uložené hledání může mít jeden nebo více plánů. Plán definuje, jak často hledání je spustit a časový interval, za které se identifikuje kritéria.
Plány mít vlastnosti v následující tabulce.

| Vlastnost | Popis |
|:--- |:--- |
| Interval |Jak často je spustit hledání. Měří v minutách. |
| QueryTimeSpan |Časový interval, za které se vyhodnotí kritéria. Musí být rovna nebo větší než Interval. Měří v minutách. |
| Verze |Verze rozhraní API používá.  V současné době to musí být vždy nastavená na hodnotu 1. |

Představte si třeba dotazu událostí s Interval 15 minut a časový interval 30 minut. V takovém případě by se spustí dotaz každých 15 minut a výstraha by spustí, pokud kritéria dál odkazující na hodnotu true přes rozpětí 30 minut.

### <a name="retrieving-schedules"></a>Načítání plány
Umožňuje načíst všechny plány uložených hledání metodu Get.

    armclient get /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search  ID}/schedules?api-version=2015-03-20

Pomocí této metody Get s ID plánu načíst konkrétní plán uložených hledání.

    armclient get /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Subscription ID}/schedules/{Schedule ID}?api-version=2015-03-20

Následuje ukázková odpověď pro plán.

```json
{
    "value": [{
        "id": "subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/MyWorkspace/savedSearches/0f0f4853-17f8-4ed1-9a03-8e888b0d16ec/schedules/a17b53ef-bd70-4ca4-9ead-83b00f2024a8",
        "etag": "W/\"datetime'2016-02-25T20%3A54%3A49.8074679Z'\"",
        "properties": {
            "Interval": 15,
            "QueryTimeSpan": 15
        }
    }]
}
```

### <a name="creating-a-schedule"></a>Vytvoření plánu
K vytvoření nového plánu použijte metodu Put s ID plánu jedinečný.  Všimněte si, že dvě plány nemůže mít stejné ID i v případě, že jsou spojeny s jinou uložená hledání.  Při vytváření plánu v konzole OMS, se vytvoří identifikátor GUID pro ID plánu.

> [!NOTE]
> Název pro všechny uložená hledání, plány a akce, které jsou vytvořené pomocí rozhraní API Log Analytics musí být malými písmeny.

    $scheduleJson = "{'properties': { 'Interval': 15, 'QueryTimeSpan':15, 'Active':'true' } }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/mynewschedule?api-version=2015-03-20 $scheduleJson

### <a name="editing-a-schedule"></a>Úprava plánu
K úpravě tento plán, použijte metodu Put s existující ID plánu pro stejné uloženého hledání.  Text žádosti musí obsahovat značku etag plánu.

      $scheduleJson = "{'etag': 'W/\"datetime'2016-02-25T20%3A54%3A49.8074679Z'\""','properties': { 'Interval': 15, 'QueryTimeSpan':15, 'Active':'true' } }"
      armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/mynewschedule?api-version=2015-03-20 $scheduleJson


### <a name="deleting-schedules"></a>Odstraňování plány
Chcete-li odstranit plán pomocí ID plánu metodu Delete.

    armclient delete /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Subscription ID}/schedules/{Schedule ID}?api-version=2015-03-20


## <a name="actions"></a>Akce
Plán může mít více akcí. Akce může definovat jeden nebo více procesy provést například odesílání e-mailu nebo spuštění sady runbook, nebo se může definovat prahové hodnoty, která určuje, kdy výsledky hledání kritériím některé.  Některé akce obě bude definovat, aby procesy provede, když je splněna prahovou hodnotu.

Všechny akce mít vlastnosti v následující tabulce.  Různé typy výstrah mají různé další vlastnosti, které jsou popsány níže.

| Vlastnost | Popis |
|:--- |:--- |
| Typ |Typ akce.  Možné hodnoty jsou aktuálně upozornění a Webhooku. |
| Name (Název) |Zobrazovaný název výstrahy. |
| Verze |Verze rozhraní API používá.  V současné době to musí být vždy nastavená na hodnotu 1. |

### <a name="retrieving-actions"></a>Načítání akce
Umožňuje načíst všechny akce pro plán metodu Get.

    armclient get /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search  ID}/schedules/{Schedule ID}/actions?api-version=2015-03-20

Pomocí metody Get s ID akce pro načtení určité akce pro plán.

    armclient get /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Subscription ID}/schedules/{Schedule ID}/actions/{Action ID}?api-version=2015-03-20

### <a name="creating-or-editing-actions"></a>Vytvořením nebo úpravou akce
Použijte metodu Put s ID akce, které jsou jedinečné pro plán pro vytvoření nové akce.  Když vytvoříte v konzole OMS akce, je identifikátor GUID pro ID akce.

> [!NOTE]
> Název pro všechny uložená hledání, plány a akce, které jsou vytvořené pomocí rozhraní API Log Analytics musí být malými písmeny.

K úpravě tento plán, použijte metodu Put s existující ID akce pro stejné uloženého hledání.  Text žádosti musí obsahovat značku etag plánu.

Formát požadavku pro vytvoření nové akce se liší podle typu akce, takže tyto příklady jsou uvedeny v následujících částech.

### <a name="deleting-actions"></a>Odstranění akcí
Použijte metodu Delete s ID akce k odstranění akce.

    armclient delete /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Subscription ID}/schedules/{Schedule ID}/Actions/{Action ID}?api-version=2015-03-20

### <a name="alert-actions"></a>Akce výstrah
Plán by měl mít pouze jeden výstrahy akce.  Jeden nebo více oddílů v následující tabulce, které se mají výstrahy akce.  Každý je podrobně popsaná v další níže.

| Část | Popis |
|:--- |:--- |
| Prahová hodnota |Kritéria pro spuštění akce. |
| EmailNotification |Odesílat e-maily několika příjemcům. |
| Nápravy |Spuštění sady runbook ve službě Azure Automation se pokuste odstranit zjištěný problém. |

#### <a name="thresholds"></a>Prahové hodnoty
Výstrahy akce by měl mít pouze jednu prahovou hodnotu.  Pokud výsledky uloženého hledání neodpovídají prahovou hodnotu v akci spojené s toto hledání, jsou spuštěny žádné další procesy, které jsou v této akce.  Akce může také obsahovat pouze prahovou hodnotu, aby se může použít s akcemi jiných typů, které neobsahují žádný prahové hodnoty.

Prahové hodnoty mít vlastnosti v následující tabulce.

| Vlastnost | Popis |
|:--- |:--- |
| Operátor |Operátor pro porovnání prahovou hodnotu. <br> gt = větší než <br> lt = menší než |
| Hodnota |Hodnota pro mezní hodnotu. |

Představte si třeba dotazu událostí se v intervalu 15 minut, časový interval 30 minut a prahové hodnoty větší než 10. V takovém případě by se spustí dotaz každých 15 minut a výstraha by spustí, pokud je vrácen 10 události, které byly vytvořeny během určitého 30 minut.

Následuje ukázková odpověď pro akce s pouze prahovou hodnotu.  

    "etag": "W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"",
    "properties": {
        "Type": "Alert",
        "Name": "My threshold action",
        "Threshold": {
            "Operator": "gt",
            "Value": 10
        },
        "Version": 1
    }

Chcete-li vytvořit novou akci prahovou hodnotu pro harmonogram pomocí akce jedinečné ID metodu Put.  

    $thresholdJson = "{'properties': { 'Name': 'My Threshold', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 } }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/mythreshold?api-version=2015-03-20 $thresholdJson

K úpravě akce prahové hodnoty pro plán, použijte metodu Put s existující ID akce.  Text žádosti musí obsahovat značku etag akce.

    $thresholdJson = "{'etag': 'W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"','properties': { 'Name': 'My Threshold', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 } }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/mythreshold?api-version=2015-03-20 $thresholdJson

#### <a name="email-notification"></a>E-mailových oznámení
E-mailová oznámení odesílat e-mailu na jeden nebo více příjemců.  Patří mezi ně vlastnosti v následující tabulce.

| Vlastnost | Popis |
|:--- |:--- |
| Příjemce |Seznam adres e-mailu. |
| Předmět |Předmět e-mailu. |
| Přílohy |Přílohy nejsou aktuálně podporovány, takže to bude mít vždy hodnotu "Žádný". |

Následuje ukázková odpověď pro akci oznámení e-mailu s prahovou hodnotou.  

    "etag": "W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"",
    "properties": {
        "Type": "Alert",
        "Name": "My email action",
        "Threshold": {
            "Operator": "gt",
            "Value": 10
        },
        "EmailNotification": {
            "Recipients": [
                "recipient1@contoso.com",
                "recipient2@contoso.com"
            ],
            "Subject": "This is the subject",
            "Attachment": "None"
        },
        "Version": 1
    }

Chcete-li vytvořit novou e-mailu akci pro plán pomocí akce jedinečné ID metodu Put.  Následující příklad vytvoří e-mailové oznámení s prahovou hodnotou, takže odešle e-mailu v případě, že výsledky uloženého hledání překročit prahovou hodnotu.

    $emailJson = "{'properties': { 'Name': 'MyEmailAction', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 }, 'EmailNotification': {'Recipients': ['recipient1@contoso.com', 'recipient2@contoso.com'], 'Subject':'This is the subject', 'Attachment':'None'} }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/myemailaction?api-version=2015-03-20 $emailJson

K úpravě akce e-mailu pro plán, použijte metodu Put s existující ID akce.  Text žádosti musí obsahovat značku etag akce.

    $emailJson = "{'etag': 'W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"','properties': { 'Name': 'MyEmailAction', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 }, 'EmailNotification': {'Recipients': ['recipient1@contoso.com', 'recipient2@contoso.com'], 'Subject':'This is the subject', 'Attachment':'None'} }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/myemailaction?api-version=2015-03-20 $emailJson

#### <a name="remediation-actions"></a>Akce nápravy
Nápravy spuštění sady runbook ve službě Azure Automation, který se pokouší odstranit problém identifikovaný výstrahy.  Musíte vytvořit webhooku pro sadu runbook použít v akci automatické nápravy a pak zadejte identifikátor URI ve vlastnosti WebhookUri.  Když vytvoříte tuto akci pomocí konzole OMS, se automaticky vytvoří nové webhooku pro sadu runbook.

Nápravami, které zahrnují vlastnosti v následující tabulce.

| Vlastnost | Popis |
|:--- |:--- |
| RunbookName |Název sady runbook. To se musí shodovat publikované sady runbook v účtu automation konfigurované v řešení služby Automation v pracovním prostoru OMS. |
| WebhookUri |Identifikátor URI služby webhooku. |
| Vypršení platnosti |Datum vypršení platnosti a čas webhooku.  Pokud webhooku nemá vypršení platnosti, může to být žádné platné budoucí datum. |

Následuje ukázková odpověď pro akci automatické nápravy s prahovou hodnotou.

    "etag": "W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"",
    "properties": {
        "Type": "Alert",
        "Name": "My remediation action",
        "Threshold": {
            "Operator": "gt",
            "Value": 10
        },
        "Remediation": {
            "RunbookName": "My-Runbook",
            "WebhookUri": "https://s1events.azure-automation.net/webhooks?token=4jCibOjO3w4W2Cfg%2b2NkjLYdafnusaG6i8tnP8h%2fNNg%3d",
            "Expiry": "2018-02-25T18:27:20"
            },
        "Version": 1
    }

Chcete-li vytvořit novou akci nápravy pro plán pomocí akce jedinečné ID metodu Put.  Následující příklad vytvoří nápravy s prahovou hodnotou, takže spuštění runbooku při výsledky uloženého hledání překročit prahovou hodnotu.

    $remediateJson = "{'properties': { 'Type':'Alert', 'Name': 'My Remediation Action', 'Version':'1', 'Threshold': { 'Operator': 'gt', 'Value': 10 }, 'Remediation': {'RunbookName': 'My-Runbook', 'WebhookUri':'https://s1events.azure-automation.net/webhooks?token=4jCibOjO3w4W2Cfg%2b2NkjLYdafnusaG6i8tnP8h%2fNNg%3d', 'Expiry':'2018-02-25T18:27:20Z'} }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/myremediationaction?api-version=2015-03-20 $remediateJson

K úpravě akci automatické nápravy pro plán, použijte metodu Put s existující ID akce.  Text žádosti musí obsahovat značku etag akce.

    $remediateJson = "{'etag': 'W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"','properties': { 'Type':'Alert', 'Name': 'My Remediation Action', 'Version':'1', 'Threshold': { 'Operator': 'gt', 'Value': 10 }, 'Remediation': {'RunbookName': 'My-Runbook', 'WebhookUri':'https://s1events.azure-automation.net/webhooks?token=4jCibOjO3w4W2Cfg%2b2NkjLYdafnusaG6i8tnP8h%2fNNg%3d', 'Expiry':'2018-02-25T18:27:20Z'} }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/myremediationaction?api-version=2015-03-20 $remediateJson

#### <a name="example"></a>Příklad
Toto je kompletní příklad k vytvoření nové e-mailové výstrahy.  Tím se vytvoří nový plán spolu s akce obsahující prahovou hodnotu a e-mailu.

    $subscriptionId = "3d56705e-5b26-5bcc-9368-dbc8d2fafbfc"
    $resourceGroup  = "MyResourceGroup"    
    $workspaceName    = "MyWorkspace"
    $searchId       = "MySearch"
    $scheduleId     = "MySchedule"
    $thresholdId    = "MyThreshold"
    $actionId       = "MyEmailAction"

    $scheduleJson = "{'properties': { 'Interval': 15, 'QueryTimeSpan':15, 'Active':'true' }"
    armclient put /subscriptions/$subscriptionId/resourceGroups/$resourceGroup/providers/Microsoft.OperationalInsights/workspaces/$workspaceName/savedSearches/$searchId/schedules/$scheduleId/?api-version=2015-03-20 $scheduleJson

    $emailJson = "{'properties': { 'Name': 'MyEmailAction', 'Version':'1', 'Severity':'Warning', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 }, 'EmailNotification': {'Recipients': ['recipient1@contoso.com', 'recipient2@contoso.com'], 'Subject':'This is the subject', 'Attachment':'None'} }"
    armclient put /subscriptions/$subscriptionId/resourceGroups/$resourceGroup/providers/Microsoft.OperationalInsights/workspaces/$workspaceName/savedSearches/$searchId/schedules/$scheduleId/actions/$actionId/?api-version=2015-03-20 $emailJson

### <a name="webhook-actions"></a>Akce Webhooku
Akce Webhooku spuštění procesu voláním adresu URL a volitelně poskytuje datové části k odeslání.  Jsou podobná nápravné akce s výjimkou jsou určené pro webhooků, který může vyvolat procesy než Azure Automation runbook.  Obsahují taky další možnost poskytnout datové části který bude doručen do vzdálený proces.

Akce Webhooku nemáte prahovou hodnotu, ale místo toho musí být přidaní do plánu, který má výstrahy akce s prahovou hodnotou.  Můžete přidat více Webhooku akcí, které se všechny spustí při splnění prahovou hodnotu.

Akce Webhooku zahrnují vlastnosti v následující tabulce.

| Vlastnost | Popis |
|:--- |:--- |
| WebhookUri |Předmět e-mailu. |
| CustomPayload |Vlastní datovou část k odeslání do webhooku.  Formát bude záviset na co webhooku očekává. |

Následuje ukázková odpověď pro akce webhooku a přidružené akce výstrah s prahovou hodnotou.

    {
        "__metadata": {},
        "value": [
            {
                "id": "subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/bwren/savedSearches/2d1b30fb-7f48-4de5-9614-79ee244b52de/schedules/b80f5621-7217-4007-b32d-165d14377093/Actions/72884702-acf9-4653-bb67-f42436b342b4",
                "etag": "W/\"datetime'2016-02-26T20%3A25%3A00.6862124Z'\"",
                "properties": {
                    "Type": "Webhook",
                    "Name": "My Webhook Action",
                    "WebhookUri": "https://oaaswebhookdf.cloudapp.net/webhooks?token=VfkYTIlpk%2fc%2bJBP",
                    "CustomPayload": "{\"fielld1\":\"value1\",\"field2\":\"value2\"}",
                    "Version": 1
                }
            },
            {
                "id": "subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/bwren/savedSearches/2d1b30fb-7f48-4de5-9614-79ee244b52de/schedules/b80f5621-7217-4007-b32d-165d14377093/Actions/90a27cf8-71b7-4df2-b04f-54ed01f1e4b6",
                "etag": "W/\"datetime'2016-02-26T20%3A25%3A00.565204Z'\"",
                "properties": {
                    "Type": "Alert",
                    "Name": "Threshold for my webhook action",
                    "Threshold": {
                        "Operator": "gt",
                        "Value": 10
                    },
                    "Version": 1
                }
            }
        ]
    }

#### <a name="create-or-edit-a-webhook-action"></a>Vytvořte nebo upravte akce webhooku
K vytvoření nové akce webhooku pro plán použijte metodu Put s ID jedinečný akce.  Následující příklad vytvoří akce Webhooku a výstrah akce s prahovou hodnotu, aby webhooku se aktivuje, když výsledky uloženého hledání překročit prahovou hodnotu.

    $thresholdAction = "{'properties': { 'Name': 'My Threshold', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 } }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/mythreshold?api-version=2015-03-20 $thresholdAction

    $webhookAction = "{'properties': {'Type': 'Webhook', 'Name': 'My Webhook", 'WebhookUri': 'https://oaaswebhookdf.cloudapp.net/webhooks?token=VrkYTKlhk%2fc%2bKBP', 'CustomPayload': '{\"field1\":\"value1\",\"field2\":\"value2\"}', 'Version': 1 }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/mywebhookaction?api-version=2015-03-20 $webhookAction

K úpravě akce webhooku pro plán, použijte metodu Put s existující ID akce.  Text žádosti musí obsahovat značku etag akce.

    $webhookAction = "{'etag': 'W/\"datetime'2016-02-26T20%3A25%3A00.6862124Z'\"','properties': {'Type': 'Webhook', 'Name': 'My Webhook", 'WebhookUri': 'https://oaaswebhookdf.cloudapp.net/webhooks?token=VrkYTKlhk%2fc%2bKBP', 'CustomPayload': '{\"field1\":\"value1\",\"field2\":\"value2\"}', 'Version': 1 }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/mywebhookaction?api-version=2015-03-20 $webhookAction

## <a name="next-steps"></a>Další kroky
* Použití [rozhraní API REST k vyhledávání protokolu](log-analytics-log-search-api.md) v analýzy protokolů.

