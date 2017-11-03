---
title: "Analýzy protokolů protokolu vyhledávání REST API | Microsoft Docs"
description: "Tato příručka obsahuje základní kurz popisující, jak můžete použít vyhledávání analýzy protokolů REST API v Operations Management Suite (OMS) a poskytuje příklady, které ukazují, jak používat příkazy."
services: log-analytics
documentationcenter: 
author: bwren
manager: carmonm
editor: 
ms.assetid: b4e9ebe8-80f0-418e-a855-de7954668df7
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/06/2017
ms.author: bwren
ms.openlocfilehash: 5b51c6fcc69c8dff6579a1a1221e88822eccc1a3
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="log-analytics-log-search-rest-api"></a>Hledání protokolu analýzy protokolů REST API
Tato příručka obsahuje základní kurz, včetně příkladů, jak můžete pomocí rozhraní REST API Log Analytics Search. Analýzy protokolů je součástí Operations Management Suite (OMS).

> [!NOTE]
> Pokud pracovní prostor byl upgradován na verzi [nové analýzy protokolů dotazu jazyka](log-analytics-log-search-upgrade.md), pak se seznamte s [dokumentace pro novou verzi protokolu hledání rozhraní API](https://dev.loganalytics.io/).

> [!NOTE]
> Analýzy protokolů volala dřív Operational Insights, proto je název používaný v poskytovatele prostředků.
>
>

## <a name="overview-of-the-log-search-rest-api"></a>Přehled protokolu hledání rozhraní REST API
Rozhraní REST API Log Analytics Search je dosáhl standardu RESTful a je přístupný prostřednictvím rozhraní API služby Azure Resource Manager. Tento článek obsahuje příklady přístup k rozhraní API prostřednictvím [ARMClient](https://github.com/projectkudu/ARMClient), nástroje příkazového řádku s otevřeným zdrojem, který zjednodušuje volání rozhraní API služby Azure Resource Manager. Použití ARMClient je jedním z mnoha možností pro přístup k rozhraní API pro vyhledávání Analytics protokolu. Další možností je použít modul Azure PowerShell pro OperationalInsights, který obsahuje rutiny pro přístup k vyhledávání. Pomocí těchto nástrojů můžete využít rozhraní API služby Azure Resource Manager provádět volání do OMS pracovních prostorů a provádět příkazy vyhledávání v nich. Rozhraní API výstupy výsledků vyhledávání ve formátu JSON, budete moci použít výsledky hledání v mnoha různými způsoby prostřednictvím kódu programu.

Azure Resource Manager je možné prostřednictvím [knihovna pro .NET](https://msdn.microsoft.com/library/azure/dn910477.aspx) a [REST API](https://msdn.microsoft.com/library/azure/mt163658.aspx). Další informace, zkontrolujte propojené webové stránky.

> [!NOTE]
> Pokud například použijete příkaz agregace `|measure count()` nebo `distinct`, každé volání hledání může vrátit maximálně 500 000 záznamů. Hledání, které neobsahují příkaz agregace vrátit maximálně 5 000 záznamů.
>
>

## <a name="basic-log-analytics-search-rest-api-tutorial"></a>Základní kurz Log Analytics vyhledávání REST API
### <a name="to-use-armclient"></a>Chcete-li použít ARMClient
1. Nainstalujte [Chocolatey](https://chocolatey.org/), což je otevřete Správce balíčků zdroje pro systém Windows. Otevřete okno příkazového řádku jako správce a spusťte následující příkaz:

    ```
    @powershell -NoProfile -ExecutionPolicy unrestricted -Command "iex ((new-object net.webclient).DownloadString('https://chocolatey.org/install.ps1'))" && SET PATH=%PATH%;%ALLUSERSPROFILE%\chocolatey\bin
    ```
2. Instalace ARMClient spuštěním následujícího příkazu:

    ```
    choco install armclient
    ```

### <a name="to-perform-a-search-using-armclient"></a>K provedení vyhledávání pomocí ARMClient
1. Přihlaste se pomocí účtu Microsoft nebo pracovní nebo školní účet:

    ```
    armclient login
    ```

    Úspěšné přihlášení zobrazí všechny odběry vázaný na daný účet:

    ```
    PS C:\Users\SampleUserName> armclient login
    Welcome YourEmail@ORG.com (Tenant: zzzzzzzz-zzzz-zzzz-zzzz-zzzzzzzzzzzz)
    User: YourEmail@ORG.com, Tenant: zzzzzzzz-zzzz-zzzz-zzzz-zzzzzzzzzzzz (Example org)
    There are 3 subscriptions
    Subscription xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx (Example Name 1)
    Subscription xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx (Example Name 2)
    Subscription xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx (Example Name 3)
    ```
2. Získáte pracovní prostory služby Operations Management Suite:

    ```
    armclient get /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces?api-version=2015-03-20
    ```

    Úspěšné volání Get by výstup všechny pracovní prostory, které jsou vázané na předplatné:

    ```
    {
    "value": [
    {
      "properties": {
    "source": "External",
    "customerId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
    "portalUrl": "https://eus.login.mms.microsoft.com/SignIn.aspx?returnUrl=https%3a%2f%2feus.mms.microsoft.com%2fMain.aspx%3fcid%xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
      },
      "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourcegroups/oi-default-east-us/providers/microsoft.operationalinsights/workspaces/{WORKSPACE NAME}",
      "name": "{WORKSPACE NAME}",
      "type": "Microsoft.OperationalInsights/workspaces",
      "location": "East US"
       ]
    }
    ```
3. Vytvořte vaše proměnná vyhledávání:

    ```
    $mySearch = "{ 'top':150, 'query':'Error'}";
    ```
4. Vyhledávání pomocí vaší novou proměnnou vyhledávání:

    ```
    armclient post /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{WORKSPACE NAME}/search?api-version=2015-03-20 $mySearch
    ```

## <a name="log-analytics-search-rest-api-reference-examples"></a>Příklady referenční dokumentace rozhraní API REST Analytics vyhledávání protokolu
Následující příklady ukazují, jak můžete použít rozhraní API služby Search.

### <a name="search---actionread"></a>Hledání - akce/čtení
**Ukázka adresa Url:**

```
    /subscriptions/{SubscriptionId}/resourceGroups/{ResourceGroupId}/providers/Microsoft.OperationalInsights/workspaces/{WorkspaceName}/search?api-version=2015-03-20
```

**Žádost:**

```
    $savedSearchParametersJson =
    {
      "top":150,
      "highlight":{
        "pre":"{[hl]}",
        "post":"{[/hl]}"
      },
      "query":"*",
      "start":"2015-02-04T21:03:29.231Z",
      "end":"2015-02-11T21:03:29.231Z"
    }
    armclient post /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace ID}/search?api-version=2015-03-20 $searchParametersJson
```
Následující tabulka popisuje vlastnosti, které jsou k dispozici.

| **Vlastnost** | **Popis** |
| --- | --- |
| Horní |Maximální počet výsledků vrátit. |
| Zvýrazněte |Obsahuje předběžné a post parametry, obvykle použité pro zvýraznění odpovídající pole |
| Před |Předpony daný řetězec, který má vaše odpovídající pole. |
| POST |Daný řetězec připojí k vaší odpovídající pole. |
| query |Vyhledávací dotaz, použít ke shromažďování a vrátí výsledky. |
| start |Začátek časový interval, který chcete výsledky, která se má najít z. |
| End |Konec časový interval, který chcete výsledky, která se má najít z. |

**Odpověď:**

```
    {
      "id" : "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
      "__metadata" : {
        "resultType" : "raw",
        "total" : 1455,
        "top" : 150,
        "StartTime" : "2015-02-11T21:09:07.0345815Z",
        "Status" : "Successful",
        "LastUpdated" : "2015-02-11T21:09:07.331463Z",
        "CoreResponses" : [],
        "sort" : [{
          "name" : "TimeGenerated",
          "order" : "desc"
        }],
        "requestTime" : 450
      },
      "value": [{
        "SourceSystem" : "OpsManager",
        "TimeGenerated" : "2015-02-07T14:07:33Z",
        "Source" : "SideBySide",
        "EventLog" : "Application",
        "Computer" : "BAMBAM",
        "EventCategory" : 0,
        "EventLevel" : 1,
        "EventLevelName" : "Error",
        "UserName" : "N/A",
        "EventID" : 78,
        "MG" : "00000000-0000-0000-0000-000000000001",
        "TimeCollected" : "2015-02-07T14:10:04.69Z",
        "ManagementGroupName" : "AOI-5bf9a37f-e841-462b-80d2-1d19cd97dc40",
        "id" : "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
        "Type" : "Event",
        "__metadata" : {
          "Type" : "Event",
          "TimeGenerated" : "2015-02-07T14:07:33Z",
          "highlighting" : {
          "EventLevelName" : ["{[hl]}Error{[/hl]}"]
        }
      }]
    ],
            "start" : "2015-02-04T21:03:29.231Z",
            "end" : "2015-02-11T21:03:29.231Z"
          }
        }
      }]
    }
```

### <a name="searchid---actionread"></a>Hledání / {ID} - akce/čtení
**Požadavek na obsah uložené výsledky hledání:**

```
    armclient post /subscriptions/{SubId}/resourceGroups/{ResourceGroupId}/providers/Microsoft.OperationalInsights/workspaces/{WorkspaceName}/search/{SearchId}?api-version=2015-03-20
```

> [!NOTE]
> Pokud hledání vrátí se stavem "Čekající na vyřízení", pak dotazování aktualizované výsledky lze provést prostřednictvím tohoto rozhraní API. Po 6 minimum výsledek hledání budou odstraněna z mezipaměti a zmizel HTTP bude vrácen. Pokud požadavek počáteční vyhledávání vrátí, úspěšné, stav okamžitě, výsledky nejsou přidány do mezipaměti způsobuje toto rozhraní API vrátit zmizel HTTP, pokud dotaz. Obsah výsledek HTTP 200 jsou ve stejném formátu jako počáteční vyhledávání požadavek právě s aktualizovanými hodnotami.
>
>

### <a name="saved-searches"></a>Uložená hledání
**Žádosti o seznam uložených hledání:**

```
    armclient post /subscriptions/{SubId}/resourceGroups/{ResourceGroupId}/providers/Microsoft.OperationalInsights/workspaces/{WorkspaceName}/savedSearches?api-version=2015-03-20
```

Podporované metody: GET, PUT odstranit

Podporované metody kolekce: získání

Následující tabulka popisuje vlastnosti, které jsou k dispozici.

| Vlastnost | Popis |
| --- | --- |
| ID |Jedinečný identifikátor. |
| Značka Etag |**Vyžaduje se pro opravu**. Server aktualizaci na každý zápis. Hodnota musí být stejná jako aktuální hodnota uložené nebo ' *' aktualizovat. 409 vrátil hodnoty starý nebo neplatná. |
| Properties.Query |**Požadované**. Vyhledávací dotaz. |
| properties.displayName |**Požadované**. Uživatelem definované zobrazovaný název dotazu. |
| Properties.category |**Požadované**. Uživatelem definované kategorie dotaz. |

> [!NOTE]
> Rozhraní API pro vyhledávání analýzy protokolů aktuálně vrací hodnotu vytvořené uživatelem uložená hledání při dotazování pro uložená hledání v pracovním prostoru. Rozhraní API nevrací uložená hledání poskytované řešení.
>
>

### <a name="create-saved-searches"></a>Vytvoření uložených hledání
**Žádost:**

```
    $savedSearchParametersJson = "{'properties': { 'Category': 'myCategory', 'DisplayName':'myDisplayName', 'Query':'* | measure Count() by Source', 'Version':'1'  }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace ID}/savedSearches/thisismyid?api-version=2015-03-20 $savedSearchParametersJson
```

> [!NOTE]
> Název pro všechny uložená hledání, plány a akce, které jsou vytvořené pomocí rozhraní API Log Analytics musí být malými písmeny.

### <a name="delete-saved-searches"></a>Odstraňte uložená hledání
**Žádost:**

```
    armclient delete /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace ID}/savedSearches/thisIsMyId?api-version=2015-03-20
```

### <a name="update-saved-searches"></a>Aktualizace uložených hledání
 **Žádost:**

```
    $savedSearchParametersJson = "{'etag': 'W/`"datetime\'2015-04-16T23%3A35%3A35.3182423Z\'`"', 'properties': { 'Category': 'myCategory', 'DisplayName':'myDisplayName', 'Query':'* | measure Count() by Source', 'Version':'1'  }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace ID}/savedSearches/thisIsMyId?api-version=2015-03-20 $savedSearchParametersJson
```

### <a name="metadata---json-only"></a>Metadata – pouze JSON
Tady je způsob, jak je uvedeno v poli pro všechny typy protokolu pro data shromážděná v pracovním prostoru. Například pokud chcete, že znáte, pokud má tento typ události pole s názvem počítače, pak tento dotaz je jeden způsob kontroly.

**Žádost o pole:**

```
    armclient get /subscriptions/{SubId}/resourceGroups/{ResourceGroupId}/providers/Microsoft.OperationalInsights/workspaces/{WorkspaceName}/schema?api-version=2015-03-20
```

**Odpověď:**

```
    {
      "__metadata" : {
        "schema" : {
          "name" : "Example Name",
          "version" : 2
        },
        "resultType" : "schema",
        "requestTime" : 35
      },
      "value" : [{
          "name" : "MG",
          "displayName" : "MG",
          "type" : "Guid",
          "facetable" : true,
          "display" : false,
          "ownerType" : ["PerfHourly", "ProtectionStatus", "Capacity_SMBUtilizationByHost", "Capacity_ArrayUtilization", "Capacity_SMBShareUtilization", "SQLAssessmentRecommendation", "Event", "ConfigurationChange", "ConfigurationAlert", "ADAssessmentRecommendation", "ConfigurationObject", "ConfigurationObjectProperty"]
        }, {
          "name" : "ManagementGroupName",
          "displayName" : "ManagementGroupName",
          "type" : "String",
          "facetable" : true,
          "display" : true,
          "ownerType" : ["PerfHourly", "ProtectionStatus", "Event", "ConfigurationChange", "ConfigurationAlert", "W3CIISLog", "AlertHistory", "Recommendation", "Alert", "SecurityEvent", "UpdateAgent", "RequiredUpdate", "ConfigurationObject", "ConfigurationObjectProperty"]
        }
      ]
    }
```

Následující tabulka popisuje vlastnosti, které jsou k dispozici.

| **Vlastnost** | **Popis** |
| --- | --- |
| jméno |Název pole. |
| displayName |Zobrazovaný název pole. |
| type |Typ v poli hodnota. |
| facetable (kategorizovatelné) |Kombinace aktuální 'indexované', ' uložené ' a 'omezující vlastnost' vlastnosti. |
| Zobrazení |Aktuální vlastnost 'display'. Hodnota TRUE, pokud pole je viditelné ve vyhledávání. |
| ownerType |Snížit na pouze typy, které patří do zařazený nemá IP. |

## <a name="optional-parameters"></a>Volitelné parametry
Následující informace popisují volitelné parametry, které jsou k dispozici.

### <a name="highlighting"></a>Zvýraznění
"Zvýraznění" parametr je volitelný parametr, který můžete použít k vyžádání subsystém hledání obsahují sadu značek v odpovědi.

Tyto značky upozornění na zahájení a ukončení zvýrazněný text, který odpovídá podmínky uvedené v vyhledávací dotaz.
Můžete určit počáteční a koncové značky, které jsou používány vyhledávání obtékat zvýrazněná termín.

**Příklad vyhledávací dotaz.**

```
    $savedSearchParametersJson =
    {
      "top":150,
      "highlight":{
        "pre":"{[hl]}",
        "post":"{[/hl]}"
      },
      "query":"*",
      "start":"2015-02-04T21:03:29.231Z",
      "end":"2015-02-11T21:03:29.231Z"
    }
    armclient post /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace ID}/search?api-version=2015-03-20 $searchParametersJson
```

**Ukázkové výsledky:**

```
    {
        "TimeGenerated":
        "2015-05-18T23:55:59Z", "Source":
        "EventLog": "Application",
        "Computer": "smokedturkey.net",
        "EventCategory": 0,
        "EventLevel":1,
        "EventLevelName":
        "Error"
        "Manager ", "__metadata":
        {
            "Type": "Event",
            "TimeGenerated": "2015-05-18T23:55:59Z",
            "highlighting": {
                "EventLevelName":
                ["{[hl]}Error{[/hl]}"]
            }
        }
    }
```

Všimněte si, že předchozí výsledek obsahuje chybovou zprávu, která má předponu a připojí.

## <a name="computer-groups"></a>Skupiny počítačů
Skupiny počítačů jsou speciální uložená hledání, které vrátí sadu počítačů.  Skupina počítačů můžete použít v jiné dotazy omezit výsledky do počítače ve skupině.  Skupina počítačů je implementovaný jako uložené hledání se skupiny značku s hodnotou počítače.

Následuje ukázková odpověď pro skupinu počítačů.

```
    "etag": "W/\"datetime'2016-04-01T13%3A38%3A04.7763203Z'\"",
    "properties": {
        "Category": "My Computer Groups",
        "DisplayName": "My Computer Group",
        "Query": "srv* | Distinct Computer",
        "Tags": [{
            "Name": "Group",
            "Value": "Computer"
          }],
    "Version": 1
    }
```

### <a name="retrieving-computer-groups"></a>Načítání skupin počítačů
Chcete-li načíst skupinu počítačů, použijte metodu Get s ID skupiny.

```
armclient get /subscriptions/{Subscription ID}/resourceGroups/{Resource Group Name}/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Group ID}`?api-version=2015-03-20
```

### <a name="creating-or-updating-a-computer-group"></a>Vytváření nebo aktualizaci skupinu počítačů
Chcete-li vytvořit skupinu počítačů, použijte metodu Put s ID jedinečný uloženého hledání. Pokud chcete použít existující skupinu ID počítače, než je upravit. Když vytvoříte skupinu počítačů na portálu analýzy protokolů, ID je vytvořen ze skupiny a název.

Dotaz, použít pro definice skupiny musí vrátit sadu počítačů pro skupinu ke správnému fungování.  Doporučuje se, že vám stát, že váš dotaz s `| Distinct Computer` zajistit správné data jsou vrácena.

Definice uloženého hledání musí obsahovat značku s hodnotou počítače pro hledání být označeny jako skupinu počítačů s názvem skupiny.

```
    $etag=Get-Date -Format yyyy-MM-ddThh:mm:ss.msZ
    $groupName="My Computer Group"
    $groupQuery = "Computer=srv* | Distinct Computer"
    $groupCategory="My Computer Groups"
    $groupID = "My Computer Groups | My Computer Group"

    $groupJson = "{'etag': 'W/`"datetime\'" + $etag + "\'`"', 'properties': { 'Category': '" + $groupCategory + "', 'DisplayName':'"  + $groupName + "', 'Query':'" + $groupQuery + "', 'Tags': [{'Name': 'Group', 'Value': 'Computer'}], 'Version':'1'  }"

    armclient put /subscriptions/{Subscription ID}/resourceGroups/{Resource Group Name}/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/$groupId`?api-version=2015-03-20 $groupJson
```

### <a name="deleting-computer-groups"></a>Odstranění skupiny počítačů
Chcete-li odstranit skupinu počítačů, použijte metodu Delete s ID skupiny.

```
armclient delete /subscriptions/{Subscription ID}/resourceGroups/{Resource Group Name}/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/$groupId`?api-version=2015-03-20
```


## <a name="next-steps"></a>Další kroky
* Další informace o [protokolu hledání](log-analytics-log-searches.md) k vytvoření dotazů pomocí vlastních polí pro kritéria.
