---
title: zahrnout soubor
description: zahrnout soubor
services: azure-resource-manager
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: include
ms.date: 02/16/2018
ms.author: tomfitz
ms.custom: include file
ms.openlocfilehash: 87b9bd74fc59c86bf177e45c18bfc4e104d9c996
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/21/2018
---
Pokud chcete přidat do skupiny prostředků dvě značky, použijte:

```powershell
Set-AzureRmResourceGroup -Name myResourceGroup -Tag @{ Dept="IT"; Environment="Test" }
```

Předpokládejme, že chcete přidat třetí značku. Pokaždé, když použijete značky na prostředek nebo skupinu prostředků, přepíšete pro daný prostředek nebo skupinu prostředků existující značky. Pokud chcete přidat novou značku bez ztráty stávajících značky, je nutné získat existující značky, přidat novou značku a znovu použít kolekci značky:

```powershell
$tags = (Get-AzureRmResourceGroup -Name myResourceGroup).Tags
$tags += @{Project="Documentation"}
Set-AzureRmResourceGroup -Tag $tags -Name myResourceGroup
```

Prostředky nedědí značky ze skupiny prostředků. V současné době vaší skupiny prostředků má tři značky, ale prostředky nemají žádné značky. Chcete-li použít všechny značky ze skupiny prostředků pro jeho zdroje a zachovat stávající značky na prostředcích, které nebudou duplikáty, použijte následující skript:

```powershell
$group = Get-AzureRmResourceGroup myResourceGroup
if ($group.Tags -ne $null) {
    $resources = $group | Find-AzureRmResource
    foreach ($r in $resources)
    {
        $resourcetags = (Get-AzureRmResource -ResourceId $r.ResourceId).Tags
        foreach ($key in $group.Tags.Keys)
        {
            if (($resourcetags) -AND ($resourcetags.ContainsKey($key))) { $resourcetags.Remove($key) }
        }
        $resourcetags += $group.Tags
        Set-AzureRmResource -Tag $resourcetags -ResourceId $r.ResourceId -Force
    }
}
```

Alternativně můžete použít značky ze skupiny prostředků pro prostředky bez zachovat stávající značky:

```powershell
$g = Get-AzureRmResourceGroup -Name myResourceGroup
Find-AzureRmResource -ResourceGroupNameEquals $g.ResourceGroupName | ForEach-Object {Set-AzureRmResource -ResourceId $_.ResourceId -Tag $g.Tags -Force }
```

Chcete-li sloučit několik hodnot v jedné značky, použijte řetězec formátu JSON.

```powershell
Set-AzureRmResourceGroup -Name myResourceGroup -Tag @{ CostCenter="{`"Dept`":`"IT`",`"Environment`":`"Test`"}" }
```

Pokud chcete odebrat všechny značky, předáte prázdný zatřiďovací tabulku.

```powershell
Set-AzureRmResourceGroup -Name myResourceGroup -Tag @{ }
```
