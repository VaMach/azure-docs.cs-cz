---
title: "Zásady prostředků Azure pro zásady vytváření názvů | Microsoft Docs"
description: "Popisuje zásady správce prostředků Azure pro zásady vytváření názvů prostředků."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/03/2017
ms.author: tomfitz
ms.openlocfilehash: 955b04517c3ccdbe530eb982e6aa1255b69e1edd
ms.sourcegitcommit: 3df3fcec9ac9e56a3f5282f6c65e5a9bc1b5ba22
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/04/2017
---
# <a name="apply-resource-policies-for-names-and-text"></a>Použít zásady prostředků pro názvy a text.
Tento článek ukazuje několik [zásady prostředků](resource-manager-policy.md) můžete použít k vytvoření konvence pojmenování a text. Tyto zásady zajistit konzistenci pro názvy prostředků a hodnoty značky. 

## <a name="set-naming-convention-with-wildcard"></a>Nastavit zásady vytváření názvů se zástupnými znaky
Následující příklad ukazuje použití zástupných znaků, který je podporovaný rozhraním **jako** podmínku. Podmínka uvádí, že pokud název neodpovídá vzoru uvedených (namePrefix\*nameSuffix) pak daný požadavek je odepřen:

```json
{
  "if": {
    "not": {
      "field": "name",
      "like": "namePrefix*nameSuffix"
    }
  },
  "then": {
    "effect": "deny"
  }
}
```

## <a name="set-naming-convention-with-pattern"></a>Nastavit zásady vytváření názvů pomocí vzoru

Chcete-li určit, že názvy prostředků odpovídá vzorku, použijte podmínku shodu. Následující příklad vyžaduje názvy začínat `contoso` a obsahovat šesti další písmena:

```json
{
  "if": {
    "not": {
      "field": "name",
      "match": "contoso??????"
    }
  },
  "then": {
    "effect": "deny"
  }
}
```

## <a name="set-date-pattern-for-tag-value"></a>Nastavit datum vzor pro hodnotu značky

Tak, aby vyžadovala datum vzorec dvě číslice, pomlčky, tři písmena, dash a čtyři číslice, použijte:

```json
{
  "if": {
    "field": "tags.date",
    "match": "##-???-####"
  },
  "then": {
    "effect": "deny"
  }
}
```

## <a name="set-multiple-naming-patterns"></a>Nastavit více vzory pojmenování

Chcete-li zadat více než jeden povolené zásady vytváření názvů, použijte **allOf** a **není** operátory. V následujícím příkladu Pokud zadaný název neodpovídá buď vzoru se nezdařilo.

```json
{
    "if": {
        "allOf": [
            {
                "not": {
                    "field": "name",
                    "match": "contoso??????"
                }
            },
            {
                "not": {
                    "field": "name",
                    "match": "contoso-???-##"
                }
            }
        ]
    },
    "then": {
        "effect": "deny"
    }
}
```

## <a name="next-steps"></a>Další kroky
* Po definování zásad pravidlo (jak je znázorněno v předchozích ukázkách), musíte k vytvoření definice zásady a přiřadit obor. Obor může být předplatné, skupinu prostředků nebo prostředek. K přiřazení zásad prostřednictvím portálu, najdete v části [portálu Azure použijte přiřadit a spravovat zásady prostředků](resource-manager-policy-portal.md). K přiřazení zásad pomocí rozhraní REST API, Powershellu nebo příkazového řádku Azure CLI, najdete v části [přiřadit a spravovat zásady prostřednictvím skriptu](resource-manager-policy-create-assign.md). 
* Pokyny k tomu, jak můžou podniky používat Resource Manager k efektivní správě předplatných, najdete v části [Základní kostra Azure Enterprise – zásady správného řízení pro předplatná](resource-manager-subscription-governance.md).

