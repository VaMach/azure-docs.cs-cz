---
title: "Chyba & výjimek - Azure Logic Apps | Microsoft Docs"
description: "Vzory pro chybové události a zpracování výjimek v Azure Logic Apps"
services: logic-apps
documentationcenter: .net,nodejs,java
author: jeffhollan
manager: anneta
editor: 
ms.assetid: e50ab2f2-1fdc-4d2a-be40-995a6cc5a0d4
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 10/18/2016
ms.author: LADocs; jehollan
ms.openlocfilehash: 4eb6f743479886374692eadcf218b77b4bfcc933
ms.sourcegitcommit: 62eaa376437687de4ef2e325ac3d7e195d158f9f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/22/2017
---
# <a name="handle-errors-and-exceptions-in-azure-logic-apps"></a>Zpracování chyb a výjimek v Azure Logic Apps

Azure Logic Apps nabízí bohaté nástroje a vzory pro vám pomůže zajistit, že vaše integrace robustní a je odolný proti selhání. Všechny Architektura integrace představuje výzvu služby a zkontrolujte, zda správně zpracovat problémy ze závislých systémů nebo výpadek. Díky Logic Apps je zpracování chyb v první třídy rozhraní, která poskytuje nástroje, které potřebujete tak, aby fungoval na výjimek a chyb v vaše pracovní postupy.

## <a name="retry-policies"></a>Opakujte zásady

Zásady opakování je nejzákladnější typ výjimky a zpracování chyb. Pokud počáteční požadavek vypršel časový limit nebo se nezdařilo (každá žádost, jejímž výsledkem 429 nebo 5xx odpověď), tato zásada určuje, zda a jak opakujte akci. Existují tři typy zásady opakování `exponential`, `fixed`, a `none`. Pokud zásady opakovaných pokusů není k dispozici v definici pracovního postupu, použije se výchozí zásady. Můžete nakonfigurovat zásady opakování v **vstupy** pro konkrétní akci nebo aktivovat, pokud je opakovatelné. Podobně v opakovaném logiku aplikace Návrhář zásady lze konfigurovat (pokud existuje) v části **nastavení** pro daný blok.

Informace o omezení zásady opakování najdete v tématu [Logic Apps omezení a konfigurace](../logic-apps/logic-apps-limits-and-config.md) a další informace o podporovaných syntaxi najdete v tématu [části zásady opakování akce pracovního postupu a aktivační události][retryPolicyMSDN].

### <a name="exponential-interval"></a>Exponenciální intervalu
`exponential` Typ zásad se po náhodném časovém intervalu z exponenciálnímu rostoucí rozsahu opakujte chybné žádosti. Jednotlivé pokusy o opakování záruku, že se k odeslání v náhodném intervalu, který je větší než **minimumInterval** a menší než **maximumInterval**. Jednoznačnou náhodnou proměnnou v pod oblastí vygeneruje pro každý opakování včetně **počet**:
<table>
<tr><th> Náhodné proměnné rozsahu </th></tr>
<tr><td>

| Opakujte číslo | Minimální Interval | Maximální Interval |
| ------------ |  ------------ |  ------------ |
| 1 | Maximální počet (0, **minimumInterval**) | Min (interval, **maximumInterval**) |
| 2 | Maximální počet (interval, **minimumInterval**) | Min (2 * interval **maximumInterval**) |
| 3 | Maximální počet (2 * interval **minimumInterval**) | Min (4 * interval **maximumInterval**) |
| 4 | Maximální počet (4 * interval **minimumInterval**) | Min (8 * interval **maximumInterval**) |
| Tlačítka ... |

</td></tr></table>

Pro `exponential` zadejte zásady, **počet** a **interval** jsou požadovány při **minimumInterval** a **maximumInterval** může být Volitelně můžete zadat přepsat výchozí hodnoty PT5S a PT1D v uvedeném pořadí.

| Název elementu | Požaduje se | Typ | Popis |
| ------------ | -------- | ---- | ----------- |
| type | Ano | Řetězec | `exponential` |
| Počet | Ano | Integer | počet pokusy o opakování, musí být mezi 1 a 90  |
| interval | Ano | Řetězec | interval v opakování [formátu ISO 8601](https://en.wikipedia.org/wiki/ISO_8601#Combined_date_and_time_representations), musí být mezi PT5S a PT1D |
| minimumInterval | Ne| Řetězec | minimální interval v opakování [formátu ISO 8601](https://en.wikipedia.org/wiki/ISO_8601#Combined_date_and_time_representations), musí být v rozmezí PT5S a **intervalu** |
| maximumInterval | Ne| Řetězec | minimální interval v opakování [formátu ISO 8601](https://en.wikipedia.org/wiki/ISO_8601#Combined_date_and_time_representations), musí být v rozmezí **interval** a PT1D |

### <a name="fixed-interval"></a>Pevně zadaném intervalu

`fixed` Typ zásad se tím, že zadaný interval času před odesláním další požadavek opakujte chybné žádosti.

| Název elementu | Požaduje se | Typ | Popis |
| ------------ | -------- | ---- | ----------- |
| type | Ano | Řetězec | `fixed`|
| Počet | Ano | Integer | počet pokusy o opakování, musí být mezi 1 a 90 |
| interval | Ano | Řetězec | interval v opakování [formátu ISO 8601](https://en.wikipedia.org/wiki/ISO_8601#Combined_date_and_time_representations), musí být mezi PT5S a PT1D |

### <a name="none"></a>Žádný
`none` Typ zásad nebude akci opakovat chybné žádosti.

| Název elementu | Požaduje se | Typ | Popis |
| ------------ | -------- | ---- | ----------- |
| type | Ano | Řetězec | `none`|

### <a name="default"></a>Výchozí
Pokud je zadán žádné zásady opakovaných pokusů, se používá výchozí zásady. Výchozí zásada je zásadu exponenciální interval, který bude odesílat až 4 opakovaných pokusů, které v exponenciálnímu zvýšení intervaly škálovat podle 7.5 sekund a omezená od 5 do 45 sekund. Tato výchozí zásada (použít, když **retryPolicy** není definován) je ekvivalentní zásad v tomto příkladu HTTP definice pracovního postupu:

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

## <a name="catch-failures-with-the-runafter-property"></a>Catch – selhání s vlastností RunAfter

Každá akce logic app deklaruje akce, které musíte dokončit před spuštěním akce, jako je řazení podle kroků v pracovním postupu. V definici akce toto řazení se označuje jako `runAfter` vlastnost. Tato vlastnost je objekt, který popisuje, které akce a akce stavy provést akci. Ve výchozím nastavení, jsou všechny akce, které jsou přidány prostřednictvím návrháře aplikace logiky hodnotu `runAfter` v předchozím kroku Pokud v předchozím kroku `Succeeded`. Ale můžete přizpůsobit, tato hodnota má provést akce, pokud mají předchozí akce `Failed`, `Skipped`, nebo sadu možné z těchto hodnot. Pokud chcete přidat položku do tématu Service Bus určené po určité akci `Insert_Row` selže, můžete použít následující `runAfter` konfigurace:

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

Upozornění `runAfter` má provést, pokud je hodnota nastavena `Insert_Row` akce je `Failed`. Akci spustit, pokud je stav akce `Succeeded`, `Failed`, nebo `Skipped`, použijte následující syntaxi:

```json
"runAfter": {
        "Insert_Row": [
            "Failed", "Succeeded", "Skipped"
        ]
    }
```

> [!TIP]
> Akce, které jsou spuštěny a dokončeny úspěšně po předchozí akce se nezdařila, jsou označeny jako `Succeeded`. Toto chování znamená, že pokud jste úspěšně catch všechny chyby v pracovním postupu spustit samotné je označen jako `Succeeded`.

## <a name="scopes-and-results-to-evaluate-actions"></a>Obory a výsledky, které slouží k vyhodnocení, akce

Podobná jak můžete spustit po jednotlivé akce, můžete taky seskupit akce uvnitř [oboru](../logic-apps/logic-apps-loops-and-scopes.md), které fungují jako logické seskupení akce. Obory jsou užitečné pro uspořádání vaše akce aplikace logiky i pro provádění agregační hodnocení na stav oboru. Obor samotné obdrží stav po dokončení všech akcí v oboru. Stav oboru je určen s stejná kritéria jako spustit. Pokud je poslední akce v větev provádění `Failed` nebo `Aborted`, je stav `Failed`.

Chcete-li provést určité akce pro všechny chyby, které bylo provedeno v rámci oboru, můžete použít `runAfter` s oborem, který je označen `Failed`. Pokud *žádné* selhání akce v oboru, spuštění po obor selže umožňuje vytvořit jednu akci k zachycení selhání.

### <a name="getting-the-context-of-failures-with-results"></a>Získávání kontextu selhání s výsledky.

I když zachytávání chyb z oboru je užitečné, můžete také kontextu, které vám pomohou pochopit přesně akce, které se nezdařila a všechny chyby nebo stavové kódy, které byly vráceny. `@result()` Funkce workflowu poskytuje kontext o výsledek všechny akce v oboru.

`@result()`přijímá jeden parametr, název oboru a vrátí pole všech akce výsledků v rámci tohoto oboru. Tyto objekty akce zahrnují stejné atributy, jako `@actions()` výstupy objektu, včetně čas spuštění akce, akce koncový čas, stav akce, akce vstupy, akce korelace ID a akce. Kontext všechny akce, které se nezdařilo odeslat v rámci oboru, můžete snadno spárujte `@result()` fungovat s `runAfter`.

K provedení akce *pro každou* akce v oboru, `Failed`, filtrovat pole výsledky na akce, které se nezdařilo, může párovat `@result()` s  **[pole filtru](../connectors/connectors-native-query.md)**  akce a  **[ForEach](../logic-apps/logic-apps-loops-and-scopes.md)**  smyčky. Můžete provést pole filtrované výsledek a provedení akce pro každé selhání pomocí **ForEach** smyčky. Tady je příklad, za nímž následuje podrobné vysvětlení, který odesílá požadavek HTTP POST s text odpovědi o všechny akce, které se nepodařilo v rámci oboru `My_Scope`.

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

Zde je podrobný postup popisující, co se stane:

1. Chcete-li získat výsledek všechny akce v rámci `My_Scope`, **pole filtru** filtrů Akce `@result('My_Scope')`.

2. Podmínky pro **pole filtru** libovolnou `@result()` položku, která je rovna stavu `Failed`. Tato podmínka filtry pole s všechny výsledky akce z `My_Scope` do pole s pouze se nezdařilo výsledky akce.

3. Provedení **pro každou** akce **filtrovat pole** výstupy. Tento krok provede akci *pro každou* výsledek akce, který byl dříve nefiltruje se nezdařilo.

    Pokud v oboru jednu akci se nezdařilo, akce v `foreach` spustit jenom jednou. 
    Mnoho selhání akce způsobí, že jednu akci za selhání.

4. Odeslání požadavku HTTP POST na `foreach` položky text odpovědi, nebo `@item()['outputs']['body']`. `@result()` Tvar položka je stejný jako `@actions()` utvářejí a lze analyzovat stejným způsobem.

5. Patří dva vlastní hlavičky s názvem selhání akce `@item()['name']` a neúspěšný spusťte klienta, ID sledování `@item()['clientTrackingId']`.

Pro referenci tady je příklad jednoho `@result()` položky, zobrazuje `name`, `body`, a `clientTrackingId` vlastnosti, které jsou analyzovány v předchozím příkladu. Mimo `foreach`, `@result()` vrátí pole z těchto objektů.

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

K provedení různých zpracování vzory výjimek, můžete použít výrazy uvedený výše. Můžete zvolit provedení jednoho výjimka zpracování akce mimo rozsah, který přijímá pole celý filtrované chyb a odebrat `foreach`. Můžete použít také další užitečné vlastnosti z `@result()` odpovědi uvedený výše.

## <a name="azure-diagnostics-and-telemetry"></a>Azure diagnostiky a telemetrii

Předchozích jsou skvělý způsob, jak zpracování chyb a výjimek v rámci spuštění, ale můžete také určit a reagují na chyby, které jsou nezávislé na spuštění sám sebe. 
[Azure Diagnostics](../logic-apps/logic-apps-monitor-your-logic-apps.md) poskytuje jednoduchý způsob, jak odeslat všechny události pracovního postupu (včetně všech stavů spustit a akce) účtu služby Azure Storage nebo centra událostí Azure. Abyste mohli vyhodnotit spuštění stavy, můžete sledování metrik a protokolování nebo publikovat je do libovolného monitorování nástroje, kterému dáváte přednost. Jednou z možných možností je k vysílání datového proudu všechny události prostřednictvím centra událostí Azure do [Stream Analytics](https://azure.microsoft.com/services/stream-analytics/). Do služby Stream Analytics lze zapsat za provozu dotazy vypnout všechny anomálií, průměry nebo selhání z diagnostické protokoly. Stream Analytics můžete snadno výstup do jiných zdrojů dat, jako jsou fronty, témata, SQL, databáze Cosmos Azure a Power BI.

## <a name="next-steps"></a>Další kroky

* [V tématu Jak zákazník sestavení službou Azure Logic Apps zpracování chyb](../logic-apps/logic-apps-scenario-error-and-exception-handling.md)
* [Najít další aplikace logiky příkladů a scénářů](../logic-apps/logic-apps-examples-and-scenarios.md)
* [Naučte se vytvořit automatické nasazení pro logic apps](../logic-apps/logic-apps-create-deploy-template.md)
* [Vytvoření a nasazení aplikací logiky s využitím sady Visual Studio](logic-apps-deploy-from-vs.md)

<!-- References -->
[retryPolicyMSDN]: https://docs.microsoft.com/rest/api/logic/actions-and-triggers#Anchor_9
