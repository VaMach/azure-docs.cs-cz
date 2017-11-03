---
title: "Chyby prostředků Azure nadřazené | Microsoft Docs"
description: "Popisuje postup řešení chyb při práci s nadřazený prostředek."
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
ms.openlocfilehash: e59147c4ac18f730f27b9d4aa9c008f219881065
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="resolve-errors-for-parent-resources"></a>Vyřešte chyby pro nadřazené prostředky

Tento článek popisuje chyb, které se můžete setkat při nasazení na prostředek, který závisí na nadřazený prostředek.

## <a name="symptom"></a>Příznaky

Při nasazení na prostředek, který je podřízená na jiný prostředek, může se zobrazit chybová zpráva:

```
Code=ParentResourceNotFound;
Message=Can not perform requested operation on nested resource. Parent resource 'exampleserver' not found."
```

## <a name="cause"></a>Příčina

Pokud jeden prostředek podřízené jiný prostředek, musí existovat nadřazený prostředek před vytvořením podřízené prostředků. Název prostředku podřízené obsahuje název nadřazené. Například může být definovaná SQL Database jako:

```json
{
  "type": "Microsoft.Sql/servers/databases",
  "name": "[concat(variables('databaseServerName'), '/', parameters('databaseName'))]",
  ...
```

Ale pokud nezadáte závislost na serveru, může před server nasadil spustit nasazení databáze.

## <a name="solution"></a>Řešení

Chcete-li tuto chybu vyřešit, obsahovat závislost.

```json
"dependsOn": [
    "[variables('databaseServerName')]"
]
```

Další informace najdete v tématu [definovat pořadí pro nasazení prostředků v šablonách Azure Resource Manager](resource-group-define-dependencies.md).