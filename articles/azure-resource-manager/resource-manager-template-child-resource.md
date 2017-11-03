---
title: "Definování podřízených prostředků v šablony Azure | Microsoft Docs"
description: "Ukazuje, jak nastavit typ prostředku a název pro podřízený prostředek v šablonu Azure Resource Manager"
services: azure-resource-manager
documentationcenter: 
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 
ms.service: azure-resource-manager
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/01/2017
ms.author: tomfitz
ms.openlocfilehash: 5b6ce5526f354008eb4a697deec737876f22391f
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="set-name-and-type-for-child-resource-in-resource-manager-template"></a>Nastavte název a typ pro podřízený prostředek v šabloně Resource Manager
Při vytváření šablony, musíte často obsahují podřízený prostředek, který souvisí se nadřazený prostředek. Vaše šablona může obsahovat třeba SQL server a databáze. SQL server je nadřazený prostředek a databáze je podřízený prostředek. 

Není ve formátu podřízený typ prostředku:`{resource-provider-namespace}/{parent-resource-type}/{child-resource-type}`

Formát názvu podřízené prostředků je:`{parent-resource-name}/{child-resource-name}`

Však zadejte název a typ v šabloně různě v závislosti na tom, jestli je vnořen do nadřazený prostředek, nebo na vlastní na nejvyšší úrovni. Toto téma ukazuje, jak zpracovat obou přístupů.

Při vytváření plně kvalifikovaný odkaz na prostředek, není jednoduše zřetězení těchto dvou pořadí kombinovat segmenty z typu a název.  Místo toho po oboru názvů, použijte posloupnost *nebo název typu* dvojice z nejméně specifická k nejvíce:

```json
{resource-provider-namespace}/{parent-resource-type}/{parent-resource-name}[/{child-resource-type}/{child-resource-name}]*
```

Například:

`Microsoft.Compute/virtualMachines/myVM/extensions/myExt`správnost `Microsoft.Compute/virtualMachines/extensions/myVM/myExt` není správný

## <a name="nested-child-resource"></a>Vnořené podřízené prostředků
Nejjednodušší způsob, jak definovat podřízených prostředků je vnořit v nadřazeném prostředku. Následující příklad ukazuje vnořené v systému SQL Server databáze SQL.

```json
{
  "name": "exampleserver",
  "type": "Microsoft.Sql/servers",
  "apiVersion": "2014-04-01",
  ...
  "resources": [
    {
      "name": "exampledatabase",
      "type": "databases",
      "apiVersion": "2014-04-01",
      ...
    }
  ]
}
```

Pro podřízený prostředek, je typ nastaven `databases` , ale jeho typ prostředku úplné `Microsoft.Sql/servers/databases`. Nezadáte `Microsoft.Sql/servers/` se předpokládá z nadřazeného typu prostředku. Název prostředku podřízené je nastaven `exampledatabase` ale úplný název obsahuje název nadřazené. Nezadáte `exampleserver` se předpokládá z nadřazené prostředku.

## <a name="top-level-child-resource"></a>Nejvyšší úrovně podřízené prostředků
Můžete definovat podřízených prostředků na nejvyšší úrovni. Tento postup můžete použít, pokud nadřazený prostředek není nasazený ve stejné šablony, nebo pokud chcete použít `copy` vytvořit více podřízené prostředky. S tímto přístupem musíte zadat typ prostředku úplné a zahrnout název nadřazené prostředku v názvu prostředku podřízené.

```json
{
  "name": "exampleserver",
  "type": "Microsoft.Sql/servers",
  "apiVersion": "2014-04-01",
  "resources": [ 
  ],
  ...
},
{
  "name": "exampleserver/exampledatabase",
  "type": "Microsoft.Sql/servers/databases",
  "apiVersion": "2014-04-01",
  ...
}
```

Databáze je podřízený prostředek k serveru, i když jsou definovány na stejné úrovni v šabloně.

## <a name="next-steps"></a>Další kroky
* Doporučení, jak vytvořit šablony, najdete v části [osvědčené postupy pro vytváření šablon Azure Resource Manager](resource-manager-template-best-practices.md).
* Příklad vytvoření několika podřízenými prostředky, najdete v části [nasadit několik instancí prostředků v šablonách Azure Resource Manager](resource-group-create-multiple.md).
