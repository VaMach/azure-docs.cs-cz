---
title: "Zásady prostředků Azure pro značky | Microsoft Docs"
description: "Obsahuje příklady prostředků zásad pro správu značky na prostředky"
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
ms.date: 08/24/2017
ms.author: tomfitz
ms.openlocfilehash: 469bd8d637337e5900ea84c6bfaf88064695fb7e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="apply-resource-policies-for-tags"></a>Použít zásady prostředků pro značky

Toto téma obsahuje běžné zásady pravidla, že které můžete použít k zajištění konzistentní využití značky na prostředky.

Použití zásad značky pro skupiny prostředků nebo předplatného se stávajícími prostředky nevztahuje zpětně zásady na tyto prostředky. K vynucení zásad na tyto prostředky, spustíte aktualizaci existující prostředky. Tento článek obsahuje příklad PowerShell pro spuštění aktualizace.

## <a name="ensure-all-resources-in-a-resource-group-have-a-tagvalue"></a>Ověřte, zda že mají všechny prostředky ve skupině prostředků a jejich hodnoty

Běžným požadavkem je, že mají všechny prostředky ve skupině prostředků konkrétní značku a hodnotu. Tento požadavek je často potřeba ke sledování nákladů oddělení. Musí být splněny následující podmínky:

* Požadované značky a hodnota se připojí k nové a aktualizované prostředky, které nemají značky.
* Požadované značky a hodnotu nelze odebrat ze všech stávajících prostředků.

Tento požadavek můžete dosáhnout použitím dvě předdefinované zásady do skupiny prostředků.

| ID | Popis |
| ---- | ---- |
| 2a0e14a6-b0a6-4fab-991a-187a4f81c498 | Když není zadaný uživatel se vztahuje požadované značky a jeho výchozí hodnotu. |
| 1e30110a-5ceb-460c-a204-c1c3969c6d62 | Vynucuje požadované značky a jeho hodnotu. |

### <a name="powershell"></a>PowerShell

Následující skript prostředí PowerShell přiřadí dva definice předdefinovaných zásad skupiny prostředků. Před spuštěním skriptu, přiřaďte všechny požadované značky do skupiny prostředků. Jednotlivé značky ve skupině prostředků. je vyžadována pro prostředky ve skupině. Přiřadit do všech skupin prostředků v rámci vašeho předplatného, neposkytují `-Name` parametr při získávání skupin prostředků.

```powershell
$appendpolicy = Get-AzureRmPolicyDefinition | Where-Object {$_.Name -eq '2a0e14a6-b0a6-4fab-991a-187a4f81c498'}
$denypolicy = Get-AzureRmPolicyDefinition | Where-Object {$_.Name -eq '1e30110a-5ceb-460c-a204-c1c3969c6d62'}

$rgs = Get-AzureRMResourceGroup -Name ExampleGroup

foreach($rg in $rgs)
{
    $tags = $rg.Tags
    foreach($key in $tags.Keys){
        $key 
        $tags[$key]
        New-AzureRmPolicyAssignment -Name ("append"+$key+"tag") -PolicyDefinition $appendpolicy -Scope $rg.ResourceId -tagName $key -tagValue  $tags[$key]
        New-AzureRmPolicyAssignment -Name ("denywithout"+$key+"tag") -PolicyDefinition $denypolicy -Scope $rg.ResourceId -tagName $key -tagValue  $tags[$key]
    }
}
```

Po přiřazení zásady, můžete aktivovat aktualizace všechny existující prostředky k vynucení zásad značky, které jste přidali. Následující skript uchovává všechny značky, které byly na prostředky:

```powershell
$group = Get-AzureRmResourceGroup -Name "ExampleGroup" 

$resources = Find-AzureRmResource -ResourceGroupName $group.ResourceGroupName 

foreach($r in $resources)
{
    try{
        $r | Set-AzureRmResource -Tags ($a=if($r.Tags -eq $NULL) { @{}} else {$r.Tags}) -Force -UsePatchSemantics
    }
    catch{
        Write-Host  $r.ResourceId + "can't be updated"
    }
}
```

## <a name="require-tags-for-a-resource-type"></a>Vyžadovat značky pro typ prostředku
Následující příklad ukazuje, jak lze vnořit logické operátory tak, aby vyžadovala značku aplikace pro typ (v této případu, účty úložiště) jenom zadaný prostředek.

```json
{
    "if": {
        "allOf": [
          {
            "not": {
              "field": "tags",
              "containsKey": "application"
            }
          },
          {
            "field": "type",
            "equals": "Microsoft.Storage/storageAccounts"
          }
        ]
    },
    "then": {
        "effect": "audit"
    }
}
```

## <a name="require-tag"></a>Vyžadovat značky
Tyto zásady odmítne požadavky, které nemají značku obsahující klíč "costCenter" (můžete použít libovolná hodnota):

```json
{
  "if": {
    "not" : {
      "field" : "tags",
      "containsKey" : "costCenter"
    }
  },
  "then" : {
    "effect" : "deny"
  }
}
```

## <a name="next-steps"></a>Další kroky
* Po definování zásad pravidlo (jak je znázorněno v předchozích ukázkách), musíte k vytvoření definice zásady a přiřadit obor. Obor může být předplatné, skupinu prostředků nebo prostředek. K přiřazení zásad prostřednictvím portálu, najdete v části [portálu Azure použijte přiřadit a spravovat zásady prostředků](resource-manager-policy-portal.md). K přiřazení zásad pomocí rozhraní REST API, Powershellu nebo příkazového řádku Azure CLI, najdete v části [přiřadit a spravovat zásady prostřednictvím skriptu](resource-manager-policy-create-assign.md).
* Úvod do zásad prostředků, najdete v části [přehled zásad prostředků](resource-manager-policy.md).
* Pokyny k tomu, jak můžou podniky používat Resource Manager k efektivní správě předplatných, najdete v části [Základní kostra Azure Enterprise – zásady správného řízení pro předplatná](resource-manager-subscription-governance.md).

