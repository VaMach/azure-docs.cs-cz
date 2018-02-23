---
title: "Provádět dotazy na pracovních prostorů Azure Log Analytics | Microsoft Docs"
description: "Tento článek popisuje, jak můžete dotazovat mezi různými pracovních prostorů a konkrétní aplikaci App Insights ve vašem předplatném."
services: log-analytics
documentationcenter: 
author: MGoedtel
manager: carmonm
editor: 
ms.assetid: 
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/15/2018
ms.author: magoedte
ms.openlocfilehash: 403448995c28ff7172d2c3abbf3b9d67341017b4
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/21/2018
---
# <a name="how-to-perform-queries-across-multiple-log-analytics-workspaces"></a>Jak provádět dotazy napříč více pracovní prostory analýzy protokolů

Dříve s Azure Log Analytics může pouze analyzujete data přímo z aktuální pracovní prostor a je omezený možnost dotazu napříč více pracovní prostory definované v rámci vašeho předplatného.  

Teď se můžete dotazovat nejen napříč více analýzy protokolů pracovních prostorů, ale také data z konkrétní aplikaci Application Insights ve stejné skupině prostředků, jiné skupině prostředků nebo jiné předplatné. To poskytuje systémová zobrazení vaše data.  Tento typ dotazu v lze provést pouze [pokročilé portál](log-analytics-log-search-portals.md#advanced-analytics-portal), není v portálu Azure.  

## <a name="querying-across-log-analytics-workspaces-and-from-application-insights"></a>Dotazování napříč pracovních prostorů analýzy protokolů a z Application Insights
Chcete-li jiný pracovní prostor v dotazu, použijte [ *prostoru* ](https://docs.loganalytics.io/docs/Language-Reference/Scope-functions/workspace()) identifikátor a pro aplikace z Application Insights, použijte [ *aplikace* ](https://docs.loganalytics.io/docs/Language-Reference/Scope-functions/app())identifikátor.  

Například první dotaz vrátí souhrnnou počty podle jejich klasifikace z tabulky aktualizace z aktuální pracovní prostor i jiné prostoru s názvem potřebné aktualizace *contosoretail it*.  V druhém příkladu dotaz vrátí souhrnnou počet požadavků na aplikaci s názvem přístup *fabrikamapp* ve službě Application Insights. 

### <a name="identifying-workspace-resources"></a>Identifikační prostředky prostoru
Identifikace pracovního prostoru lze provést několika způsoby:

* Název prostředku - je popisný název pracovního prostoru, označovaných také jako *název komponenty*. 

    `workspace("contosoretail").Update | count`
 
    >[!NOTE]
    >Identifikace pracovního prostoru jeho název předpokládá, že je jedinečné ve všech předplatných přístupné. Pokud máte více aplikací se zadaným názvem, se dotaz nezdaří z důvodu nejednoznačnosti. V takovém případě musíte použít jeden z dalších identifikátorů.

* Kvalifikovaný název – je "úplný název" pracovního prostoru tvořený název odběru, skupinu prostředků a název součásti v tomto formátu: *Název_předplatného, resourceGroup nebo componentName*. 

    `workspace('contoso/contosoretail/development').requests | count `

    >[!NOTE]
    >Vzhledem k tomu, že nejsou jedinečné názvy předplatné Azure, tento identifikátor může být nejednoznačný. 
    >

* ID pracovního prostoru - ID pracovního prostoru je jedinečný, neměnné, identifikátor přiřazený k každém pracovním prostoru vyjádřené globálně jedinečný identifikátor (GUID).

    `workspace("b438b4f6-912a-46d5-9cb1-b44069212ab4").Update | count`

* Azure ID prostředku – definované Azure jedinečnou identitu pracovního prostoru. ID prostředku se používají, když název prostředku je nejednoznačný.  Pro pracovní prostory, není ve formátu: */subscriptions/subscriptionId/resourcegroups/resourceGroup/providers/microsoft. Pracovní prostory/OperationalInsights/componentName*.  

    Příklad:
    ``` 
    workspace("/subscriptions/e427267-5645-4c4e-9c67-3b84b59a6982/resourcegroups/ContosoAzureHQ/providers/Microsoft.OperationalInsights/workspaces/contosoretail").Event | count
    ```

### <a name="identifying-an-application"></a>Identifikace aplikace

Identifikace aplikace ve službě Application Insights lze provést *app(Identifier)* výraz.  *Identifikátor* argument určuje aplikace pomocí jedné z následujících akcí:

* Název prostředku - je lidské čitelný název aplikace, označovaných také jako *název komponenty*.  

    `app("fabrikamapp")`

* Kvalifikovaný název – je "úplný název" aplikace skládá z název odběru, skupinu prostředků, název komponenty v tomto formátu: *Název_předplatného, resourceGroup nebo componentName*. 

    `app("AI-Prototype/Fabrikam/fabrikamapp").requests | count`

     >[!NOTE]
    >Vzhledem k tomu, že nejsou jedinečné názvy předplatné Azure, tento identifikátor může být nejednoznačný. 
    >

* ID – identifikátor GUID aplikace aplikace.

    `app("b438b4f6-912a-46d5-9cb1-b44069212ab4").requests | count`

* Azure ID prostředku - definované Azure jedinečnou identitu aplikace. ID prostředku se používají, když název prostředku je nejednoznačný. Formát je: */subscriptions/subscriptionId/resourcegroups/resourceGroup/providers/microsoft. OperationalInsights nebo součástí nebo componentName*.  

    Příklad:
    ```
    app("/subscriptions/7293b69-db12-44fc-9a66-9c2005c3051d/resourcegroups/Fabrikam/providers/microsoft.insights/components/fabrikamapp").requests | count
    ```

## <a name="next-steps"></a>Další postup

Zkontrolujte [analýzy protokolů protokolu vyhledávání odkaz](https://docs.loganalytics.io/docs/Language-Reference) zobrazíte všechny možnosti syntaxe dotazu, která je k dispozici v analýzy protokolů.    