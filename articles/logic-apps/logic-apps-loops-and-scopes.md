---
title: "Vytvoření smyčky a obory nebo debatch data v pracovních postupech - Azure Logic Apps | Microsoft Docs"
description: "Vytvoření smyčky k iteraci v rámci dat, akce skupiny do oborů, nebo debatch data ke spuštění více pracovních postupů v Azure Logic Apps."
services: logic-apps
documentationcenter: .net,nodejs,java
author: jeffhollan
manager: anneta
editor: 
ms.assetid: 75b52eeb-23a7-47dd-a42f-1351c6dfebdc
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/29/2016
ms.author: LADocs; jehollan
ms.openlocfilehash: 9cdbe4a12a0b16341a1e52f176901045baf327b5
ms.sourcegitcommit: 68aec76e471d677fd9a6333dc60ed098d1072cfc
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/18/2017
---
# <a name="logic-apps-loops-scopes-and-debatching"></a>Smyčky, obory a rozdělení dávek v Logic Apps
  
Služba Logic Apps poskytuje několik způsobů, jak pracovat s poli, kolekcí, dávky a smyčky v pracovním postupu.
  
## <a name="foreach-loop-and-arrays"></a>Pole a smyčka typu ForEach
  
Služba Logic Apps umožňuje smyčky v rámci sady dat a provedení akce pro každou položku.  Ve smyčce přes kolekce je možné prostřednictvím `foreach` akce.  V návrháři, můžete přidat pro každou smyčku.  Až vyberete pole, která si přejete iterace, můžete začít přidáním akce.  Můžete přidat více akcí za smyčka typu foreach.  Jednou v rámci smyčky, můžete začít k určení, co má probíhat na každou hodnotu pole.

  Tento příklad odešle e-mailu pro každý e-mailovou adresu, která obsahuje 'microsoft.com. Pokud používáte zobrazení kódu, můžete zadat, pro každou smyčku jako v následujícím příkladu:

``` json
{
    "email_filter": {
        "type": "query",
        "inputs": {
            "from": "@triggerBody()['emails']",
            "where": "@contains(item()['email'], 'microsoft.com')"
        }
    },
    "forEach_email": {
        "type": "foreach",
        "foreach": "@body('email_filter')",
        "actions": {
            "send_email": {
                "type": "ApiConnection",
                "inputs": {
                "body": {
                    "to": "@item()",
                    "from": "me@contoso.com",
                    "message": "Hello, thank you for ordering"
                },
                "host": {
                    "connection": {
                        "id": "@parameters('$connections')['office365']['connection']['id']"
                    }
                },
                }
            }
        },
        "runAfter":{
            "email_filter": [ "Succeeded" ]
        }
    }
}
```
  
  A `foreach` akce můžete iterace v polích s tisíci entitami.  Iterace provést paralelní ve výchozím nastavení.  V tématu [omezení a konfigurace](logic-apps-limits-and-config.md) podrobnosti o pole a souběžnost omezení.

### <a name="sequential-foreach-loops"></a>Sekvenční smyčky ForEach

Chcete-li povolit smyčka typu foreach provést postupně, `Sequential` by měla být přidána možnost operaci.

``` json
"forEach_email": {
        "type": "foreach",
        "foreach": "@body('email_filter')",
        "operationOptions": "Sequential",
        "..."
}
```
  
## <a name="until-loop"></a>Dokud smyčky
  
  Dokud je splněna podmínka, můžete provést akci nebo posloupnost akcí.  Nejběžnější scénáře použití dokud smyčky je volání koncový bod, dokud nezískáte odpovědi, které hledáte.  V návrháři, můžete přidat dokud smyčky.  Po přidání akce uvnitř smyčky, můžete nastavit ukončovací podmínky, jakož i smyčky omezení.
  
  Tento příklad zavolá koncový bod protokolu HTTP, dokud text odpovědi má hodnotu "Dokončeno".  Při dokončení buď: 
  
  * Odpověď HTTP má stav "dokončeno.
  * To nezkusí hodinu
  * Smyčce 100krát
  
  Pokud používáte zobrazení kódu, můžete zadat dokud smyčky jako v následujícím příkladu:
  
  ``` json
  {
      "until_successful":{
        "type": "until",
        "expression": "@equals(actions('http')['status'], 'Completed')",
        "limit": {
            "count": 100,
            "timeout": "PT1H"
        },
        "actions": {
            "create_resource": {
                "type": "http",
                "inputs": {
                    "url": "http://provisionRseource.com",
                    "body": {
                        "resourceId": "@triggerBody()"
                    }
                }
            }
        }
      }
  }
  ```
  
## <a name="spliton-and-debatching"></a>SplitOn a debatching

Někdy se může zobrazit pole položek, které chcete debatch a spustit pracovní postup, na položku aktivační událost.  Tato debatching můžete provést pomocí `spliton` příkaz.  Ve výchozím nastavení, pokud vaše swagger aktivační událost určuje datové části, která je pole `spliton` je přidána. `spliton` Příkaz spustí spustit na každou položku v poli.  SplitOn lze přidat pouze k aktivační události, které mohou být ručně nakonfigurované nebo přepsat. Nemůže mít `spliton` a také implementovat vzor synchronní odpovědi.  Jakýkoli pracovní postup, který volá má `response` akce kromě `spliton` běží asynchronně a odešle okamžitého `202 Accepted` odpovědi.  

  Tento příklad přijímá pole položek a debatches na každém řádku. SplitOn lze zadat v zobrazení kódu jako v následujícím příkladu:

```
{
    "myDebatchTrigger": {
        "type": "Http",
        "inputs": {
            "url": "http://getNewCustomers",
        },
        "recurrence": {
            "frequencey": "Second",
            "interval": 15
        },
        "spliton": "@triggerBody()['rows']"
    }
}
```

## <a name="scopes"></a>Obory

Je možné seskupit sérii akcí společně s použitím oboru.  Obory jsou užitečné pro implementace zpracování výjimek.  V Návrháři můžete přidat nový obor a začnete přidávat všechny akce v rámci ho.  Můžete definovat obory v zobrazení kódu jako v následujícím příkladu:


```
{
    "myScope": {
        "type": "scope",
        "actions": {
            "call_bing": {
                "type": "http",
                "inputs": {
                    "url": "http://www.bing.com"
                }
            }
        }
    }
}
```
