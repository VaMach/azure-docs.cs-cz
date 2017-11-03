---
title: "Provádět dotazy na pracovních prostorů Azure Log Analytics | Microsoft Docs"
description: "Tento článek popisuje, jak můžete dotazovat napříč více pracovní prostory v rámci vašeho předplatného s příklady provést."
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
ms.date: 10/04/2017
ms.author: magoedte
ms.openlocfilehash: 6b6dc6f472a87178c006301f4f692aeff15e257e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="perform-queries-across-multiple-log-analytics-workspaces"></a>Provádět dotazy na pracovní prostory více analýzy protokolů

Dříve s Azure Log Analytics je může pouze analyzovat data přímo z aktuální pracovní prostor a této omezené možnost dotazu napříč více pracovní prostory definované v rámci vašeho předplatného.  

Teď se můžete dotazovat napříč více pracovní prostory, že poskytuje systémová zobrazení dat.  Tento typ dotazu v lze provést pouze [pokročilé portál](log-analytics-log-search-portals.md#advanced-analytics-portal), není v portálu Azure.  

## <a name="querying-across-log-analytics-workspaces"></a>Dotazování napříč pracovních prostorů analýzy protokolů
Chcete-li jiný pracovní prostor v dotazu, použijte *prostoru* identifikátor.  Například následující dotaz vrátí souhrnnou počty podle jejich klasifikace z tabulky aktualizace z aktuální pracovní prostor i jiné prostoru s názvem potřebné aktualizace *contosoretail it*.  


## <a name="identifying-resources"></a>Identifikujete zdroje
Identifikace pracovního prostoru lze provést několika způsoby:

* Název prostředku - je popisný název pracovního prostoru, označovaných také jako *název komponenty*. 

    `workspace("contosoretail").Update | count`
 
    >[!NOTE]
    >Identifikace pracovního prostoru jeho název předpokládá, že je jedinečné ve všech předplatných přístupné. Pokud máte více aplikací se zadaným názvem, se dotaz nezdaří z důvodu nejednoznačnosti. V takovém případě musí používat jednu z dalších identifikátorů.

* Kvalifikovaný název – je "úplný název" pracovního prostoru tvořený název odběru, skupinu prostředků a součást název v tomto formátu: *Název_předplatného, resourceGroup nebo componentName*. 

    `workspace('contoso/contosoretail/development').requests | count `

    >[!NOTE]
    >Vzhledem k tomu, že nejsou jedinečné názvy předplatné Azure, tento identifikátor může být nejednoznačný. 
    >

* ID pracovního prostoru - ID pracovního prostoru je jedinečný, neměnné, identifikátor přiřazený k každém pracovním prostoru vyjádřené globálně jedinečný identifikátor (GUID).

    `workspace("b438b4f6-912a-46d5-9cb1-b44069212ab4").Update | count`

* Azure ID prostředku – definované Azure jedinečnou identitu pracovního prostoru. Můžete použít při název prostředku je nejednoznačný.  Pro pracovní prostory, není ve formátu: */subscriptions/subscriptionId/resourcegroups/resourceGroup/providers/microsoft. Pracovní prostory/OperationalInsights/componentName*.  

    Například:
    ``` 
    `workspace("/subscriptions/e427267-5645-4c4e-9c67-3b84b59a6982/resourcegroups/ContosoAzureHQ/providers/Microsoft.OperationalInsights/workspaces/contosoretail").Event | count
    ```

## <a name="next-steps"></a>Další kroky

Zkontrolujte [analýzy protokolů protokolu vyhledávání odkaz](https://docs.loganalytics.io/docs/Language-Reference) zobrazíte všechny možnosti syntaxe dotazu, která je k dispozici v analýzy protokolů.    