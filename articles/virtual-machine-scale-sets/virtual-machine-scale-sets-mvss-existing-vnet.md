---
title: "Odkazovat na existující virtuální síť v šablonu sady Azure škálování | Microsoft Docs"
description: "Informace o postupu přidání virtuální sítě do stávající šablony sadu škálování virtuálního počítače Azure"
services: virtual-machine-scale-sets
documentationcenter: 
author: gatneil
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 76ac7fd7-2e05-4762-88ca-3b499e87906e
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: negat
ms.openlocfilehash: 28117d467b491704aed8d45e5eba42530579dfa2
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="add-reference-to-an-existing-virtual-network-in-an-azure-scale-set-template"></a>Přidat odkaz na existující virtuální síť v šablonu sady Azure škálování

Tento článek ukazuje, jak upravit [minimální přijatelná měřítko nastavit šablonu](./virtual-machine-scale-sets-mvss-start.md) k nasazení do existující virtuální síť místo vytvoření nové.

## <a name="change-the-template-definition"></a>Změna definice šablony

Nakonfigurujte šablonu naše minimální přijatelná škálování si můžete prohlédnout [sem](https://raw.githubusercontent.com/gatneil/mvss/minimum-viable-scale-set/azuredeploy.json), a naše šablony pro nasazení do existující virtuální síť sad škálování si můžete prohlédnout [zde](https://raw.githubusercontent.com/gatneil/mvss/existing-vnet/azuredeploy.json). Podívejme se na rozdílové použít k vytvoření této šablony (`git diff minimum-viable-scale-set existing-vnet`) část podle část:

Nejprve přidáme `subnetId` parametr. Tento řetězec se předají do konfigurace sady škálování, povolení k identifikaci předem vytvořené podsítě k nasazení virtuálních počítačů do sad škálování. Tento řetězec musí být ve tvaru: `/subscriptions/<subscription-id>resourceGroups/<resource-group-name>/providers/Microsoft.Network/virtualNetworks/<virtual-network-name>/subnets/<subnet-name>`. Například nasazení měřítka nastavení do existující virtuální síť s názvem `myvnet`, podsíť `mysubnet`, skupinu prostředků `myrg`a předplatné `00000000-0000-0000-0000-000000000000`, by subnetId: `/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myrg/providers/Microsoft.Network/virtualNetworks/myvnet/subnets/mysubnet`.

```diff
     },
     "adminPassword": {
       "type": "securestring"
+    },
+    "subnetId": {
+      "type": "string"
     }
   },
```

V dalším kroku jsme odstranit virtuální síť prostředek z `resources` pole, protože jsme se použití existující virtuální sítě a není nutné nasazovat nové.

```diff
   "variables": {},
   "resources": [
-    {
-      "type": "Microsoft.Network/virtualNetworks",
-      "name": "myVnet",
-      "location": "[resourceGroup().location]",
-      "apiVersion": "2016-12-01",
-      "properties": {
-        "addressSpace": {
-          "addressPrefixes": [
-            "10.0.0.0/16"
-          ]
-        },
-        "subnets": [
-          {
-            "name": "mySubnet",
-            "properties": {
-              "addressPrefix": "10.0.0.0/16"
-            }
-          }
-        ]
-      }
-    },
```

Virtuální sítě už existuje před nasazením šablony, takže není nutné specifikovat klauzuli dependsOn od stupnice nastavit do virtuální sítě. Proto jsme odstranit tyto řádky:

```diff
     {
       "type": "Microsoft.Compute/virtualMachineScaleSets",
       "name": "myScaleSet",
       "location": "[resourceGroup().location]",
       "apiVersion": "2016-04-30-preview",
-      "dependsOn": [
-        "Microsoft.Network/virtualNetworks/myVnet"
-      ],
       "sku": {
         "name": "Standard_A1",
         "capacity": 2
```

Nakonec jsme předávat `subnetId` parametr nastavený uživatelem (místo použití `resourceId` získat id virtuální sítě v jednom nasazení, který je co minimální přijatelná měřítko nastavit šablonu nemá).

```diff
                       "name": "myIpConfig",
                       "properties": {
                         "subnet": {
-                          "id": "[concat(resourceId('Microsoft.Network/virtualNetworks', 'myVnet'), '/subnets/mySubnet')]"
+                          "id": "[parameters('subnetId')]"
                         }
                       }
                     }
```




## <a name="next-steps"></a>Další kroky

[!INCLUDE [mvss-next-steps-include](../../includes/mvss-next-steps.md)]
