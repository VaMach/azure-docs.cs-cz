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
ms.date: 06/27/2017
ms.author: tomfitz
ms.openlocfilehash: 51b3519bbba8cb4c768bfdd7dadf92fced434f22
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="apply-resource-policies-for-names-and-text"></a>Použít zásady prostředků pro názvy a text.
Toto téma ukazuje několik [zásady prostředků](resource-manager-policy.md) můžete použít k vytvoření konvence pojmenování a text. Tyto zásady zajistit konzistenci pro názvy prostředků a hodnoty značky. 

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

## <a name="next-steps"></a>Další kroky
* Po definování zásad pravidlo (jak je znázorněno v předchozích ukázkách), musíte k vytvoření definice zásady a přiřadit obor. Obor může být předplatné, skupinu prostředků nebo prostředek. K přiřazení zásad prostřednictvím portálu, najdete v části [portálu Azure použijte přiřadit a spravovat zásady prostředků](resource-manager-policy-portal.md). K přiřazení zásad pomocí rozhraní REST API, Powershellu nebo příkazového řádku Azure CLI, najdete v části [přiřadit a spravovat zásady prostřednictvím skriptu](resource-manager-policy-create-assign.md). 
* Pokyny k tomu, jak můžou podniky používat Resource Manager k efektivní správě předplatných, najdete v části [Základní kostra Azure Enterprise – zásady správného řízení pro předplatná](resource-manager-subscription-governance.md).

