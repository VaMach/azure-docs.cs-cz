---
title: "Definovat pracovní postupy s JSON - Azure Logic Apps | Microsoft Docs"
description: "Jak napsat definice pracovního postupu ve formátu JSON pro logic apps"
author: jeffhollan
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: d565873c-6b1b-4057-9250-cf81a96180ae
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.custom: H1Hack27Feb2017
ms.date: 03/29/2017
ms.author: LADocs; jehollan
ms.openlocfilehash: 7f9e5a10066df8a464c285273e77a85c0d562ebb
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="create-workflow-definitions-for-logic-apps-using-json"></a>Vytvoření definice pracovního postupu pro logic apps pomocí JSON

Vytvořením definice pracovního postupu pro [Azure Logic Apps](logic-apps-what-are-logic-apps.md) jednoduchý a deklarativní jazyce JSON. Pokud jste to ještě neudělali, přečtěte si nejprve [postup vytvoření první aplikace logiky pomocí návrháře aplikace logiky](logic-apps-create-a-logic-app.md). Další informace naleznete [úplné referenční dokumentace pro jazyk definic workflowů](http://aka.ms/logicappsdocs).

## <a name="repeat-steps-over-a-list"></a>Opakujte kroky pro seznam

K iteraci v rámci pole, které má až 10 000 položek a provedení akce pro každou položku, použijte [typu foreach](logic-apps-loops-and-scopes.md).

## <a name="handle-failures-if-something-goes-wrong"></a>Zpracování chyb, pokud dojde k chybě

Obvykle, které chcete zahrnout *nápravy krok* – některé logiky, která provede *jenom v případě* jeden nebo více voláními nezdaří. Tento příklad načte data z různých míst, ale pokud volání selže, chceme, takže jsme můžete sledovat tohoto selhání později někde odeslat zprávu:  

```
{
  "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {},
  "triggers": {
    "Request": {
      "type": "request",
      "kind": "http"
    }
  },
  "actions": {
    "readData": {
      "type": "Http",
      "inputs": {
        "method": "GET",
        "uri": "http://myurl"
      }
    },
    "postToErrorMessageQueue": {
      "type": "ApiConnection",
      "inputs": "...",
      "runAfter": {
        "readData": [
          "Failed"
        ]
      }
    }
  },
  "outputs": {}
}
```

Chcete-li určit, že `postToErrorMessageQueue` spouští pouze `readData` má `Failed`, použít `runAfter` vlastnosti, například pro zadání seznamu možných hodnot, tak, aby `runAfter` může být `["Succeeded", "Failed"]`.

Nakonec, protože v tomto příkladu teď zpracovává chyby, jsme už označit spustit jako `Failed`. Vzhledem k tomu, že jsme přidali v kroku pro zpracování této chyby v tomto příkladu, má spustit `Succeeded` i když jeden krok `Failed`.

## <a name="execute-two-or-more-steps-in-parallel"></a>Paralelní spuštění dvou nebo více kroků

Ke spouštění více akcí paralelně, `runAfter` vlastnost musí být shodná za běhu. 

```
{
  "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {},
  "triggers": {
    "Request": {
      "kind": "http",
      "type": "Request"
    }
  },
  "actions": {
    "readData": {
      "type": "Http",
      "inputs": {
        "method": "GET",
        "uri": "http://myurl"
      }
    },
    "branch1": {
      "type": "Http",
      "inputs": {
        "method": "GET",
        "uri": "http://myurl"
      },
      "runAfter": {
        "readData": [
          "Succeeded"
        ]
      }
    },
    "branch2": {
      "type": "Http",
      "inputs": {
        "method": "GET",
        "uri": "http://myurl"
      },
      "runAfter": {
        "readData": [
          "Succeeded"
        ]
      }
    }
  },
  "outputs": {}
}
```

V tomto příkladu obě `branch1` a `branch2` jsou nastaveny na spouštění `readData`. V důsledku toho obou poboček spustit souběžně. Časové razítko pro obě pobočky se shoduje.

![Paralelní](media/logic-apps-author-definitions/parallel.png)

## <a name="join-two-parallel-branches"></a>Připojení dvě paralelních větvích

Toho se můžete zapojit dvě akce, které jsou nastaveny na spouštění paralelní přidáním položky `runAfter` vlastnost jako v předchozím příkladu.

```
{
  "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-04-01-preview/workflowdefinition.json#",
  "actions": {
    "readData": {
      "type": "Http",
      "inputs": {
        "method": "GET",
        "uri": "http://myurl"
      },
      "runAfter": {}
    },
    "branch1": {
      "type": "Http",
      "inputs": {
        "method": "GET",
        "uri": "http://myurl"
      },
      "runAfter": {
        "readData": [
          "Succeeded"
        ]
      }
    },
    "branch2": {
      "type": "Http",
      "inputs": {
        "method": "GET",
        "uri": "http://myurl"
      },
      "runAfter": {
        "readData": [
          "Succeeded"
        ]
      }
    },
    "join": {
      "type": "Http",
      "inputs": {
        "method": "GET",
        "uri": "http://myurl"
      },
      "runAfter": {
        "branch1": [
          "Succeeded"
        ],
        "branch2": [
          "Succeeded"
        ]
      }
    }
  },
  "parameters": {},
  "triggers": {
    "Request": {
      "type": "Request",
      "kind": "Http",
      "inputs": {
        "schema": {}
      }
    }
  },
  "contentVersion": "1.0.0.0",
  "outputs": {}
}
```

![Paralelní](media/logic-apps-author-definitions/join.png)

## <a name="map-list-items-to-a-different-configuration"></a>Položky seznamu mapy do jiné konfigurace

Dále Řekněme, že má získat jiný obsah na základě hodnoty vlastnosti. Jako parametr jsme vytvořit mapování hodnot na cíle:  

```
{
  "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "specialCategories": {
      "defaultValue": [
        "science",
        "google",
        "microsoft",
        "robots",
        "NSA"
      ],
      "type": "Array"
    },
    "destinationMap": {
      "defaultValue": {
        "science": "http://www.nasa.gov",
        "microsoft": "https://www.microsoft.com/en-us/default.aspx",
        "google": "https://www.google.com",
        "robots": "https://en.wikipedia.org/wiki/Robot",
        "NSA": "https://www.nsa.gov/"
      },
      "type": "Object"
    }
  },
  "triggers": {
    "Request": {
      "type": "Request",
      "kind": "http"
    }
  },
  "actions": {
    "getArticles": {
      "type": "Http",
      "inputs": {
        "method": "GET",
        "uri": "https://ajax.googleapis.com/ajax/services/feed/load?v=1.0&q=http://feeds.wired.com/wired/index"
      }
    },
    "forEachArticle": {
      "type": "foreach",
      "foreach": "@body('getArticles').responseData.feed.entries",
      "actions": {
        "ifGreater": {
          "type": "if",
          "expression": "@greater(length(intersection(item().categories, parameters('specialCategories'))), 0)",
          "actions": {
            "getSpecialPage": {
              "type": "Http",
              "inputs": {
                "method": "GET",
                "uri": "@parameters('destinationMap')[first(intersection(item().categories, parameters('specialCategories')))]"
              }
            }
          }
        }
      },
      "runAfter": {
        "getArticles": [
          "Succeeded"
        ]
      }
    }
  }
}
```

V takovém případě nám nejdřív získat seznam článků. Podle kategorie, která byla definována jako parametr, druhý krok používá mapu vyhledat adresu URL pro získání obsahu.

Některé časy si zde: 

*   [ `intersection()` ](https://msdn.microsoft.com/library/azure/mt643789.aspx#intersection) Funkce kontroluje, zda kategorii odpovídá jedné ze známé definovaných kategorií.

*   Po kategorie se nám získat, jsme načítat položky od mapy pomocí hranatými závorkami:`parameters[...]`

## <a name="process-strings"></a>Proces řetězce

Různé funkce můžete použít k manipulaci s řetězci. Předpokládejme například, máme řetězec, který chcete předat do systému, ale nemůžeme nejsou jisti, o správné zpracování pro kódování znaků. Jednou z možností je ve formátu Base64 kódování tento řetězec. Ale abyste se vyhnuli uvozovací znaky v adrese URL, přidáme nahrazení pár znaků. 

Chceme také dílčí řetězec názvu pořadí, protože se nepoužívají prvních 5 znaků.

```
{
  "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "order": {
      "defaultValue": {
        "quantity": 10,
        "id": "myorder1",
        "orderer": "NAME=Contoso"
      },
      "type": "Object"
    }
  },
  "triggers": {
    "request": {
      "type": "request",
      "kind": "http"
    }
  },
  "actions": {
    "order": {
      "type": "Http",
      "inputs": {
        "method": "GET",
        "uri": "http://www.example.com/?id=@{replace(replace(base64(substring(parameters('order').orderer,5,sub(length(parameters('order').orderer), 5) )),'+','-') ,'/' ,'_' )}"
      }
    }
  },
  "outputs": {}
}
```

Pracovní z uvnitř k mimo:

1. Získat [ `length()` ](https://msdn.microsoft.com/library/azure/mt643789.aspx#length) pro název orderer, takže se nám získat zpět celkový počet znaků.

2. Odečtena 5, protože chceme kratší řetězec.

3. Ve skutečnosti, proveďte [ `substring()` ](https://msdn.microsoft.com/library/azure/mt643789.aspx#substring). Začneme v indexu `5` a přejděte zbytek řetězce.

4. Převést tento dílčí [ `base64()` ](https://msdn.microsoft.com/library/azure/mt643789.aspx#base64) řetězec.

5. [`replace()`](https://msdn.microsoft.com/library/azure/mt643789.aspx#replace)všechny `+` znaků a obsahující `-` znaků.

6. [`replace()`](https://msdn.microsoft.com/library/azure/mt643789.aspx#replace)všechny `/` znaků a obsahující `_` znaků.

## <a name="work-with-date-times"></a>Práce s data a času

Hodnoty data a času může být užitečná, zejména v případě, že se pokoušíte vyžádá data ze zdroje dat, která nepodporuje přirozeně *aktivační události*. Můžete taky data a času pro hledání, jak dlouho jednotlivých kroků trvá.

```
{
  "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "order": {
      "defaultValue": {
        "quantity": 10,
        "id": "myorder1"
      },
      "type": "Object"
    }
  },
  "triggers": {
    "Request": {
      "type": "request",
      "kind": "http"
    }
  },
  "actions": {
    "order": {
      "type": "Http",
      "inputs": {
        "method": "GET",
        "uri": "http://www.example.com/?id=@{parameters('order').id}"
      }
    },
    "ifTimingWarning": {
      "type": "If",
      "expression": "@less(actions('order').startTime,addseconds(utcNow(),-1))",
      "actions": {
        "timingWarning": {
          "type": "Http",
          "inputs": {
            "method": "GET",
            "uri": "http://www.example.com/?recordLongOrderTime=@{parameters('order').id}&currentTime=@{utcNow('r')}"
          }
        }
      },
      "runAfter": {
        "order": [
          "Succeeded"
        ]
      }
    }
  },
  "outputs": {}
}
```

V tomto příkladu jsme extrahujte `startTime` z předchozího kroku. Potom jsme získat aktuální čas a odečítání sekundu:

[`addseconds(..., -1)`](https://msdn.microsoft.com/library/azure/mt643789.aspx#addseconds) 

Můžete použít jiné jednotky doby, jako je třeba `minutes` nebo `hours`. Nakonec jsme můžete porovnat tyto dvě hodnoty. Pokud první hodnota je menší než druhá hodnota, která pak více než jedna sekunda byla úspěšná, protože byl nejprve umístit pořadí.

K formátování kalendářních dat, můžeme použít formátování řetězce. Například RFC1123 získáte používáme [ `utcnow('r')` ](https://msdn.microsoft.com/library/azure/mt643789.aspx#utcnow). Další informace o formátování data, najdete v části [jazyk definic workflowů](https://msdn.microsoft.com/library/azure/mt643789.aspx#utcnow).

## <a name="deployment-parameters-for-different-environments"></a>Parametry nasazení pro různá prostředí

Běžně mají životní cykly nasazení prostředí pro vývoj, pracovní prostředí a provozním prostředí. Například můžete použít stejné definice v těchto prostředích ale použití různých databází. Podobně můžete chtít použít stejné definice v různých oblastech pro vysokou dostupnost, ale má každá instance aplikace logiky ke komunikaci s danou oblast databáze.
Tento scénář se liší od trvá parametry v *runtime* kde místo toho by měla použít `trigger()` fungovat jako v předchozím příkladu.

Můžete začít s základní definice následujícím způsobem:

```
{
    "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "uri": {
            "type": "string"
        }
    },
    "triggers": {
        "request": {
          "type": "request",
          "kind": "http"
        }
    },
    "actions": {
        "readData": {
            "type": "Http",
            "inputs": {
                "method": "GET",
                "uri": "@parameters('uri')"
            }
        }
    },
    "outputs": {}
}
```

V skutečnou `PUT` požadavku pro logic apps, můžete zadat parametr `uri`. Protože výchozí hodnota už existuje, datové části aplikace logiky vyžaduje tento parametr:

```
{
    "properties": {},
        "definition": {
          // Use the definition from above here
        },
        "parameters": {
            "connection": {
                "value": "https://my.connection.that.is.per.enviornment"
            }
        }
    },
    "location": "westus"
}
``` 

V každém prostředí, můžete zadat jinou hodnotu pro `connection` parametr. 

Všechny možnosti, které máte k vytváření a správě aplikací logiky najdete v tématu [dokumentace k REST API](https://msdn.microsoft.com/library/azure/mt643787.aspx). 
