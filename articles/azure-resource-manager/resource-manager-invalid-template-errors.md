---
title: "Chyby neplatný šablony Azure | Microsoft Docs"
description: "Popisuje, jak vyřešit chyby neplatný šablony."
services: azure-resource-manager,azure-portal
documentationcenter: 
author: tfitzmac
manager: timlt
editor: 
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: support-article
ms.date: 09/13/2017
ms.author: tomfitz
ms.openlocfilehash: 87bc6e4def624785c5052a9a25f579b022c940ec
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/09/2018
---
# <a name="resolve-errors-for-invalid-template"></a>Řešení chyb neplatný šablony

Tento článek popisuje, jak vyřešit chyby neplatný šablony.

## <a name="symptom"></a>Příznaky

Při nasazení šablony, se zobrazí chyba oznamující:

```
Code=InvalidTemplate
Message=<varies>
```

Chybová zpráva, závisí na typu chyby.

## <a name="cause"></a>Příčina

Tato chyba může způsobit z několika různých typů chyb. Obvykle zahrnují syntaxe nebo strukturální chyby v šabloně.

## <a name="solution"></a>Řešení

### <a name="solution-1---syntax-error"></a>Řešení 1 - Chyba syntaxe

Pokud se zobrazí chybovou zprávu, která určuje ověření šablony se nezdařilo, bude pravděpodobně problém syntaxe ve vaší šabloně.

```
Code=InvalidTemplate
Message=Deployment template validation failed
```

Tato chyba je snadné provést, protože může být složité výrazy šablony. Například následující přiřazení název pro účet úložiště obsahuje jednu sadu závorky, tři funkce, tři sady závorky, jednu sadu jednoduchých uvozovek a jednu vlastnost:

```json
"name": "[concat('storage', uniqueString(resourceGroup().id))]",
```

Pokud nezadáte syntaxe porovnávání, vytvoří šablona hodnotu, která se liší od svůj záměr.

Po zobrazení tohoto typu chyby, pečlivě zkontrolujte syntaxi výrazu. Zvažte použití editor JSON jako [Visual Studio](vs-azure-tools-resource-groups-deployment-projects-create-deploy.md) nebo [Visual Studio Code](resource-manager-vs-code.md), která vás upozorní chyby syntaxe.

### <a name="solution-2---incorrect-segment-lengths"></a>Řešení 2 – nesprávný segment délky

Další neplatný šablony chyba nastane, když název prostředku není ve správném formátu.

```
Code=InvalidTemplate
Message=Deployment template validation failed: 'The template resource {resource-name}'
for type {resource-type} has incorrect segment lengths.
```

Kořenové úrovně prostředku musí mít jeden menší segmentu v názvu než v typ prostředku. Každý segment je rozlišené pomocí lomítko. V následujícím příkladu, typ má dva segmenty a název jednoho segmentu, tak, aby byl **platný název**.

```json
{
  "type": "Microsoft.Web/serverfarms",
  "name": "myHostingPlanName",
  ...
}
```

Dalším příkladem je, ale **není platný název** protože má stejný počet segmentů jako typ.

```json
{
  "type": "Microsoft.Web/serverfarms",
  "name": "appPlan/myHostingPlanName",
  ...
}
```

Pro podřízené prostředky typ a název mít stejný počet segmentů. Tento počet segmentů dává smysl, protože úplný název a typ pro podřízený objekt zahrnuje název nadřazeného a typu. Úplný název proto má stále jednoho menší segmentu než úplné typu.

```json
"resources": [
    {
        "type": "Microsoft.KeyVault/vaults",
        "name": "contosokeyvault",
        ...
        "resources": [
            {
                "type": "secrets",
                "name": "appPassword",
                ...
            }
        ]
    }
]
```

Získávání segmentů správné může být složité s typy Resource Manager, které se použijí napříč zprostředkovatelé prostředků. Například použití prostředků zámku na web vyžaduje typ s čtyři segmenty. Název je tedy tři segmenty:

```json
{
    "type": "Microsoft.Web/sites/providers/locks",
    "name": "[concat(variables('siteName'),'/Microsoft.Authorization/MySiteLock')]",
    ...
}
```

### <a name="solution-3---parameter-is-not-valid"></a>Řešení 3 – parametr není platný

Pokud šablona specifikuje povolené hodnoty pro parametr, a zadáte hodnotu, která není jedním z těchto hodnot, zobrazí se zpráva podobná následující chybě:

```
Code=InvalidTemplate;
Message=Deployment template validation failed: 'The provided value {parameter value}
for the template parameter {parameter name} is not valid. The parameter value is not
part of the allowed values
```

Double zkontrolujte povolené hodnoty v šabloně a zadejte jeden během nasazení.

### <a name="solution-4---too-many-target-resource-groups"></a>Řešení 4 - příliš mnoho skupin prostředků cíle

Pokud zadáte více než pět skupin prostředků cíle v jednom nasazení, zobrazí tato chyba. Zvažte buď konsolidace počet skupin prostředků ve vašem nasazení nebo nasazení některé šablony jako samostatná nasazení. Další informace najdete v tématu [prostředky Azure nasazení na více než jedno předplatné nebo skupinu prostředků](resource-manager-cross-resource-group-deployment.md).

### <a name="solution-5---circular-dependency-detected"></a>Řešení 5 - cyklická závislost zjistil

Tato chyba se zobrazí, když prostředky jsou vzájemně závislé způsobem, který zabrání spuštění nasazení. Kombinace vzájemné závislosti díky dvou nebo více prostředků čekat na další prostředky, které jsou také čekání. Například resource1 závisí na resource3, resource2 závisí na resource1 a resource3 závisí na resource2. Obvykle můžete tento problém vyřešit odstraněním nepotřebných závislosti.
