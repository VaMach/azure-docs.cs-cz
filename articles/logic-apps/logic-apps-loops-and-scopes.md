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
ms.openlocfilehash: 413a2ba9107ca259ed577825bf0a17ff5622f1ac
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="logic-apps-loops-scopes-and-debatching"></a>Smyčky, obory a rozdělení dávek v Logic Apps
  
Služba Logic Apps poskytuje několik způsobů, jak pracovat s poli, kolekcí, dávky a smyčky v pracovním postupu.
  
## <a name="foreach-loop-and-arrays"></a>Pole a smyčka typu ForEach
  
Služba Logic Apps umožňuje smyčky v rámci sady dat a provedení akce pro každou položku.  To je možné prostřednictvím `foreach` akce.  V návrháři, můžete přidat pro každou smyčku.  Až vyberete pole, která si přejete iterace, můžete začít přidáním akce.  Aktuálně jste omezeni na jenom jednu akci za smyčka typu foreach, ale toto omezení se zruší, v následujících týdnech.  V rámci smyčky začnete jednou zadejte, co má probíhat na každou hodnotu pole.

Pokud používáte zobrazení kódu, můžete zadat, pro každou smyčku jako níže.  Toto je příklad pro každou smyčku, která odešle e-mail pro každý e-mailovou adresu, která obsahuje 'microsoft.com.:

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
  
  A `foreach` akce iterovat přes maticových až 5000 řádků.  Každé iteraci spustí paralelně ve výchozím nastavení.  

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
  
  Dokud je splněna podmínka, můžete provést akci nebo posloupnost akcí.  Nejběžnější scénáře je volání koncový bod, dokud nezískáte odpověď, kterou hledáte.  V návrháři, můžete přidat dokud smyčky.  Po přidání akce uvnitř smyčky, můžete nastavit ukončovací podmínky, jakož i smyčky omezení.  Mezi cykly smyčky dochází ke zpoždění 1 minuta.
  
  Pokud používáte zobrazení kódu, můžete zadat dokud smyčky jako níže.  Toto je příklad volání koncový bod protokolu HTTP, dokud text odpovědi má hodnotu "Dokončeno".  Když se dokončí buď 
  
  * Odpověď HTTP má stav "dokončeno.
  * To nezkusí 1 hodinu
  * Smyčce 100krát
  
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

Někdy se může zobrazit pole položek, které chcete debatch a spustit pracovní postup, na položku aktivační událost.  Můžete to provést prostřednictvím `spliton` příkaz.  Ve výchozím nastavení, pokud vaše swagger aktivační událost určuje datové části, která je pole `spliton` přidá a spustit na položku start.  SplitOn lze přidat pouze pro aktivační událost.  To lze ručně nakonfigurované nebo přepsání v definici zobrazení kódu.  Nyní můžete debatch SplitOn maticových až 5 000 položek.  Nemůže mít `spliton` a také implementovat vzor synchronní odpovědi.  Jakýkoli pracovní postup, který volá má `response` akce kromě `spliton` spustí asynchronně a odeslat okamžitého `202 Accepted` odpovědi.  

SplitOn lze zadat v zobrazení kódu jako v následujícím příkladu.  To přijímá pole položek a debatches na každém řádku.

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

Je možné seskupit sérii akcí společně s použitím oboru.  To je obzvláště užitečné pro implementace zpracování výjimek.  V Návrháři můžete přidat nový obor a začnete přidávat všechny akce v rámci ho.  Můžete definovat obory v zobrazení kódu takto:


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