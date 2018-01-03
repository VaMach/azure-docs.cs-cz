---
title: "Chyba a zpracování výjimek pro Logic Apps v Azure | Microsoft Docs"
description: "Vzory pro chybové události a zpracování výjimek v Logic Apps."
services: logic-apps
documentationcenter: .net,nodejs,java
author: derek1ee
manager: anneta
editor: 
ms.assetid: e50ab2f2-1fdc-4d2a-be40-995a6cc5a0d4
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 10/18/2016
ms.author: LADocs; deli
ms.openlocfilehash: a74c7d18306359c9152f139299de1208b5932fe5
ms.sourcegitcommit: f46cbcff710f590aebe437c6dd459452ddf0af09
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/20/2017
---
# <a name="handle-errors-and-exceptions-in-logic-apps"></a>Zpracování chyb a výjimek v Logic Apps

Služba Logic Apps v Azure poskytuje bohaté nástroje a vzory vám pomohou zajistit, že jsou vaše integrace robustní a odolný proti selhání. Všechny Architektura integrace představuje výzvu správně zpracovávat výpadku nebo problémů na závislé systémy. Služba Logic Apps zajišťuje zpracování chyb první třídy. Nabízí nástroje, které potřebujete tak, aby fungoval na výjimek a chyb v vaše pracovní postupy.

## <a name="retry-policies"></a>Opakujte zásady

Zásady opakování je nejzákladnější typ výjimky a zpracování chyb. Pokud počáteční požadavek časového limitu nebo selže (každá žádost, jejímž výsledkem 429 nebo 5xx odpověď), zásady opakování určuje, zda a jak je třeba opakovat akci. 

Existují čtyři typy zásady opakování: výchozí, žádná pevná intervalu a exponenciální intervalu. Pokud zásady opakovaných pokusů není k dispozici v definici pracovního postupu, použije se výchozí zásady podle definice služby. 

Můžete nakonfigurovat zásady opakování v *vstupy* pro konkrétní akci nebo aktivovat, pokud je opakovatelné. Podobně můžete nakonfigurovat opakování zásady (pokud existuje) v návrháři aplikace logiky. Chcete-li nastavit zásady opakovaných pokusů, v návrháři aplikace logiky, přejděte na **nastavení** konkrétní akce.

Informace o omezeních zásady opakování najdete v tématu [Logic Apps omezení a konfigurace](../logic-apps/logic-apps-limits-and-config.md). Další informace o podporovaných syntaxi najdete v tématu [opakujte část zásad v akce pracovního postupu a aktivační události][retryPolicyMSDN].

### <a name="default"></a>Výchozí

Pokud nedefinujete zásady opakovaných pokusů (**retryPolicy** není definován), se používá výchozí zásady. Výchozí zásada je zásadu exponenciální interval, která odešle až čtyř opakovaných pokusů, které v exponenciálnímu zvýšení intervaly škálovat podle 7.5 sekund. Interval je limitován od 5 do 45 sekund. Tato výchozí zásada je ekvivalentní zásad v tomto příkladu HTTP definice pracovního postupu:

```json
"HTTP":
{
    "inputs": {
        "method": "GET",
        "uri": "http://myAPIendpoint/api/action",
        "retryPolicy" : {
            "type": "exponential",
            "count": 4,
            "interval": "PT7.5S",
            "minimumInterval": "PT5S",
            "maximumInterval": "PT45S"
        }
    },
    "runAfter": {},
    "type": "Http"
}
```

### <a name="none"></a>Žádné

Pokud **retryPolicy** je nastaven na **žádné**, není opakovat chybné žádosti.

| Název elementu | Požaduje se | Typ | Popis |
| ------------ | -------- | ---- | ----------- |
| type | Ano | Řetězec | **None** |

### <a name="fixed-interval"></a>Pevně zadaném intervalu

Pokud **retryPolicy** je nastaven na **pevné**, zásady opakování chybné žádosti tím, že zadaný interval času před odesláním další požadavek.

| Název elementu | Požaduje se | Typ | Popis |
| ------------ | -------- | ---- | ----------- |
| type | Ano | Řetězec | **Pevná** |
| Počet | Ano | Integer | Počet opakovaných pokusů. Musí být mezi 1 a 90. |
| interval | Ano | Řetězec | Interval v opakování [formátu ISO 8601](https://en.wikipedia.org/wiki/ISO_8601#Combined_date_and_time_representations). Musí být mezi PT5S a PT1D. |

### <a name="exponential-interval"></a>Exponenciální intervalu

Pokud **retryPolicy** je nastaven na **exponenciální**, zásady opakování po náhodném časovém intervalu z exponenciálnímu rostoucí rozsahu chybné žádosti. Jednotlivé pokusy o opakování záruku, že se k odeslání v náhodném intervalu, který je větší než **minimumInterval** a menší než **maximumInterval**. Jednoznačnou náhodnou proměnnou v rozsahu uvedené v následující tabulce se generuje pro každou opakování, včetně **počet**:

**Náhodná proměnná rozsahu**

| Opakujte číslo | Minimální interval | Maximální interval |
| ------------ |  ------------ |  ------------ |
| 1 | Maximální počet (0, **minimumInterval**) | Min (interval, **maximumInterval**) |
| 2 | Maximální počet (interval, **minimumInterval**) | Min (2 * interval **maximumInterval**) |
| 3 | Maximální počet (2 * interval **minimumInterval**) | Min (4 * interval **maximumInterval**) |
| 4 | Maximální počet (4 * interval **minimumInterval**) | Min (8 * interval **maximumInterval**) |
| ... |

Pro typ exponenciální zásady **počet** a **interval** jsou povinné. Hodnoty pro **minimumInterval** a **maximumInterval** jsou volitelné. Můžete přidat je do přepsat výchozí hodnoty PT5S a PT1D, v uvedeném pořadí.

| Název elementu | Požaduje se | Typ | Popis |
| ------------ | -------- | ---- | ----------- |
| type | Ano | Řetězec | **exponenciální** |
| Počet | Ano | Integer | Počet opakovaných pokusů. Musí být mezi 1 a 90.  |
| interval | Ano | Řetězec | Interval v opakování [formátu ISO 8601](https://en.wikipedia.org/wiki/ISO_8601#Combined_date_and_time_representations). Musí být mezi PT5S a PT1D. |
| minimumInterval | Ne | Řetězec | Minimální interval v opakování [formátu ISO 8601](https://en.wikipedia.org/wiki/ISO_8601#Combined_date_and_time_representations). Musí být v rozmezí PT5S a **interval**. |
| maximumInterval | Ne | Řetězec | Minimální interval v opakování [formátu ISO 8601](https://en.wikipedia.org/wiki/ISO_8601#Combined_date_and_time_representations). Musí být v rozmezí **interval** a PT1D. |

## <a name="catch-failures-with-the-runafter-property"></a>Catch – selhání s vlastností runAfter

Každá akce logic app deklaruje akce, které musíte dokončit před spuštěním akce. Je to podobné řazení podle kroků v pracovním postupu. V definici akce toto řazení se označuje jako **runAfter** vlastnost. 

**RunAfter** vlastnost je objekt, který popisuje, které akce a akce stavy provést akci. Ve výchozím nastavení, všechny akce, které jste přidali pomocí návrháře logiku aplikace nastaven na spouštění po předchozím kroku, pokud je výsledek předchozí krok **úspěšné**. 

Ale můžete přizpůsobit **runAfter** hodnota má provést akce, pokud mají předchozí akce výsledku **se nezdařilo**, **vynecháno**, nebo sadu možné z těchto hodnot. Pokud chcete přidat položku do určené téma Azure Service Bus po určité akci **Insert_Row** selže, můžete použít následující **runAfter** konfigurace:

```json
"Send_message": {
    "inputs": {
        "body": {
            "ContentData": "@{encodeBase64(body('Insert_Row'))}",
            "ContentType": "{ \"content-type\" : \"application/json\" }"
        },
        "host": {
            "api": {
                "runtimeUrl": "https://logic-apis-westus.azure-apim.net/apim/servicebus"
            },
            "connection": {
                "name": "@parameters('$connections')['servicebus']['connectionId']"
            }
        },
        "method": "post",
        "path": "/@{encodeURIComponent('failures')}/messages"
    },
    "runAfter": {
        "Insert_Row": [
            "Failed"
        ]
    }
}
```

Všimněte si, že **runAfter** má provést, pokud je nastavena **Insert_Row** je výsledek akce **se nezdařilo**. Akci spustit, pokud je stav akce **úspěšné**, **se nezdařilo**, nebo **vynecháno**, použijte následující syntaxi:

```json
"runAfter": {
        "Insert_Row": [
            "Failed", "Succeeded", "Skipped"
        ]
    }
```

> [!TIP]
> Akce, které spustit a úspěšně dokončit po předchozí akce se nezdařila, které jsou označeny jako **úspěšné**. To znamená, že pokud jste úspěšně catch všechny chyby v pracovním postupu spustit samotné je označen jako **úspěšné**.

## <a name="scopes-and-results-to-evaluate-actions"></a>Obory a výsledky, které slouží k vyhodnocení, akce

Můžete seskupit akce uvnitř [oboru](../logic-apps/logic-apps-loops-and-scopes.md), podobně jako spustíte po jednotlivé akce. Obor funguje logické seskupení akce. 

Obory jsou užitečné pro uspořádání vaše akce aplikace logiky i pro provádění agregační hodnocení na stav oboru. Obor samotné obdrží stav po dokončení všech akcí v oboru. Stav oboru je určen s stejná kritéria jako spustit. Pokud je poslední akce v větev provádění **se nezdařilo** nebo **bylo přerušeno.**, je stav **se nezdařilo**.

Chcete-li provést určité akce pro všechny chyby, které došlo k chybě v rámci oboru, můžete použít **runAfter** s oborem, který je označen **se nezdařilo**. Pokud *žádné* akce v oboru selže, pokud používáte **runAfter** pro obor, můžete vytvořit jednu akci k zachycení selhání.

### <a name="get-the-context-of-failures-with-results"></a>Získat kontext chyb s výsledky.

I když zachytávání chyb z oboru je užitečné, můžete také kontextu, které vám pomohou pochopit, přesně akce, které se nezdařilo a pochopit žádné chyby nebo stavové kódy, které byly vráceny.  **@result()** Funkce workflowu poskytuje kontext o výsledek všechny akce v oboru.

 **@result()** Funkce přijímá jeden parametr (název oboru) a vrátí pole všech akce výsledků v rámci tohoto oboru. Tyto objekty akce zahrnují stejné atributy, jako  **@actions()** výstupy objektu, včetně čas spuštění akce, akce koncový čas, stav akce, akce vstupy, akce korelace ID a akce. 

Kontext všechny akce, které se nezdařilo odeslat v rámci oboru, můžete snadno spárujte  **@result()** fungovat s **runAfter** vlastnost.

K provedení akce *pro každou* akce v oboru, který má **se nezdařilo** výsledek, a pokud chcete filtrovat pole výsledky na akce, které se nezdařilo, pair  **@result()** s [Filter_array](../connectors/connectors-native-query.md) akce a [foreach](../logic-apps/logic-apps-loops-and-scopes.md) smyčky. S polem filtrované výsledek, můžete provést akci pro každé selhání pomocí **foreach** smyčky. 

Zde uvádíme příklad, který odešle HTTP POST žádosti s text odpovědi o všechny akce, které se nezdařila v oboru My_Scope:

```json
"Filter_array": {
    "inputs": {
        "from": "@result('My_Scope')",
        "where": "@equals(item()['status'], 'Failed')"
    },
    "runAfter": {
        "My_Scope": [
            "Failed"
        ]
    },
    "type": "Query"
},
"For_each": {
    "actions": {
        "Log_Exception": {
            "inputs": {
                "body": "@item()['outputs']['body']",
                "method": "POST",
                "headers": {
                    "x-failed-action-name": "@item()['name']",
                    "x-failed-tracking-id": "@item()['clientTrackingId']"
                },
                "uri": "http://requestb.in/"
            },
            "runAfter": {},
            "type": "Http"
        }
    },
    "foreach": "@body('Filter_array')",
    "runAfter": {
        "Filter_array": [
            "Succeeded"
        ]
    },
    "type": "Foreach"
}
```

Zde je podrobný postup popisující, co se stane, že v předchozím příkladu:

1. Chcete-li získat výsledek všechny akce v rámci My_Scope, **Filter_array** filtrů Akce  **@result(My_Scope)**.

2. Podmínky pro **Filter_array** libovolnou  **@result()** položku, která je ve stavu, která je rovna **se nezdařilo**. Tuto podmínku filtrování pole z všechny výsledky akce z My_Scope, do pole s výsledky pouze akce se nezdařilo.

3. Provedení **foreach** akce *filtrované pole* výstupy. Tento krok provede akci *pro každou* výsledek akce, který byl dříve nefiltruje se nezdařilo.

    Pokud se nezdařila jednu akci v oboru, akce v **foreach** spustit jenom jednou. Několik neúspěšných akce způsobí, že jednu akci za selhání.

4. Odeslání požadavku HTTP POST na **foreach** položky text odpovědi, nebo  **@item() ['výstupy'] ['text']**.  **@result()** Tvar položka je stejný jako  **@actions()** tvaru. Lze ji analyzovat stejným způsobem.

5. Patří dva vlastní hlavičky s názvem selhání akce  **@item() [name]** a neúspěšný spusťte klienta, ID sledování  **@item() [clientTrackingId]**.

Pro referenci tady je příklad jednoho  **@result()** položky. Zobrazuje **název**, **textu**, a **clientTrackingId** vlastnosti, které jsou analyzovány v předchozím příkladu. Mimo **foreach** akce,  **@result()** vrátí pole z těchto objektů.

```json
{
    "name": "Example_Action_That_Failed",
    "inputs": {
        "uri": "https://myfailedaction.azurewebsites.net",
        "method": "POST"
    },
    "outputs": {
        "statusCode": 404,
        "headers": {
            "Date": "Thu, 11 Aug 2016 03:18:18 GMT",
            "Server": "Microsoft-IIS/8.0",
            "X-Powered-By": "ASP.NET",
            "Content-Length": "68",
            "Content-Type": "application/json"
        },
        "body": {
            "code": "ResourceNotFound",
            "message": "/docs/folder-name/resource-name does not exist"
        }
    },
    "startTime": "2016-08-11T03:18:19.7755341Z",
    "endTime": "2016-08-11T03:18:20.2598835Z",
    "trackingId": "bdd82e28-ba2c-4160-a700-e3a8f1a38e22",
    "clientTrackingId": "08587307213861835591296330354",
    "code": "NotFound",
    "status": "Failed"
}
```

Pro zpracování vzory různých výjimek můžete použít výrazy popsané dříve v článku. Můžete zvolit provedení jednoho výjimka zpracování akce mimo rozsah, který přijímá pole celý filtrované chyb a odebrat **foreach**. Můžete použít také další užitečné vlastnosti z  **@result()** odpovědi, jak je popsáno výše.

## <a name="azure-diagnostics-and-telemetry"></a>Azure diagnostiky a telemetrii

Vzory popsané v tomto článku poskytují skvělý způsoby, jak zpracování chyb a výjimek uvnitř spuštění, ale můžete také určit a reagují na chyby, které jsou nezávislé na spuštění sám sebe. [Azure Diagnostics](../logic-apps/logic-apps-monitor-your-logic-apps.md) poskytuje jednoduchý způsob, jak odeslat všechny události pracovního postupu (včetně všech stavů spustit a akce) na účet úložiště Azure nebo do centra událostí v Azure Event Hubs. 

Vyhodnotit spuštění stavy, můžete monitorovat protokoly a metriky nebo publikovat je do jakýkoli monitorování nástroj, který preferujete. Potenciální možnost je k vysílání datového proudu všechny události prostřednictvím centra událostí pro [Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/). Do služby Stream Analytics lze zapsat za provozu dotazy na základě jakékoli anomálie, průměry nebo selhání z diagnostické protokoly. Stream Analytics můžete použít k odeslání informací do jiných zdrojů dat, jako je do fronty, témata, SQL, Azure Cosmos DB nebo Power BI.

## <a name="next-steps"></a>Další kroky

* V tématu Jak zákazník [sestavení s Logic Apps v Azure pro zpracování chyb](../logic-apps/logic-apps-scenario-error-and-exception-handling.md).
* Najít další [Logic Apps příkladů a scénářů](../logic-apps/logic-apps-examples-and-scenarios.md).
* Naučte se vytvářet [automatizované nasazení pro aplikace logiky](../logic-apps/logic-apps-create-deploy-template.md).
* Zjistěte, jak [sestavení a nasazení aplikace logiky pomocí sady Visual Studio](logic-apps-deploy-from-vs.md).

<!-- References -->
[retryPolicyMSDN]: https://docs.microsoft.com/rest/api/logic/actions-and-triggers#Anchor_9
