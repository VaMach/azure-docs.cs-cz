---
title: "Umístění prostředků Azure v šabloně | Microsoft Docs"
description: "Ukazuje, jak nastavit umístění pro prostředek v šablonu Azure Resource Manager"
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
ms.date: 02/03/2017
ms.author: tomfitz
ms.openlocfilehash: 73e50a593c41e841dcaf184abb895406ff5001e9
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="set-resource-location-in-azure-resource-manager-templates"></a>Nastavte umístění prostředku v šablonách Azure Resource Manager
Při nasazování šablony, je nutné zadat umístění každého prostředku. Toto téma ukazuje, jak k určení umístění, které jsou k dispozici pro vaše předplatné pro každý typ prostředku.

## <a name="determine-supported-locations"></a>Určení podporovaných umístění

Úplný seznam podporovaných umístění pro každý typ prostředku, najdete v části [produkty podle oblasti](https://azure.microsoft.com/regions/services/). Vaše předplatné však nemusí mít přístup do všech umístění v tomto seznamu. Pokud chcete zobrazit seznam umístění, které jsou k dispozici pro vaše předplatné přizpůsobené, použijte prostředí Azure PowerShell nebo rozhraní příkazového řádku Azure. 

Následující příklad používá prostředí PowerShell a získat umístění pro `Microsoft.Web\sites` typ prostředku:

```powershell
((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Web).ResourceTypes | Where-Object ResourceTypeName -eq sites).Locations
```

Následující příklad používá Azure CLI 2.0 získat umístění `Microsoft.Web\sites` typ prostředku:

```azurecli
az provider show -n Microsoft.Web --query "resourceTypes[?resourceType=='sites'].locations"
```

## <a name="set-location-in-template"></a>Nastavení umístění v šabloně

Po určení podporovaná umístění pro vaše prostředky, musíte nastavit toto umístění ve vaší šabloně. Nejjednodušší způsob, jak nastavit tuto hodnotu je vytvoření skupiny prostředků v místě, které podporuje tyto typy prostředků a nastavit každý umístění na `[resourceGroup().location]`. Můžete znovu nasaďte šablonu, kterou chcete skupiny prostředků v různých umístěních a nezmění žádné hodnoty v šabloně nebo parametry. 

Následující příklad ukazuje, účet úložiště, který je nasazen do stejného umístění jako pro skupinu prostředků:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "variables": {
      "storageName": "[concat('storage', uniqueString(resourceGroup().id))]"
    },
    "resources": [
    {
      "apiVersion": "2016-01-01",
      "type": "Microsoft.Storage/storageAccounts",
      "name": "[variables('storageName')]",
      "location": "[resourceGroup().location]",
      "tags": {
        "Dept": "Finance",
        "Environment": "Production"
      },
      "sku": {
        "name": "Standard_LRS"
      },
      "kind": "Storage",
      "properties": { }
    }
    ]
}
```

Pokud potřebujete používat pevné kódování umístění v šabloně, zadejte název jednoho z podporovaných oblastí. Následující příklad ukazuje, účet úložiště, který je vždy nasazený do Sever střední USA:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [
    {
      "apiVersion": "2016-01-01",
      "type": "Microsoft.Storage/storageAccounts",
      "name": "[concat('storageloc', uniqueString(resourceGroup().id))]",
      "location": "North Central US",
      "tags": {
        "Dept": "Finance",
        "Environment": "Production"
      },
      "sku": {
        "name": "Standard_LRS"
      },
      "kind": "Storage",
      "properties": { }
    }
    ]
}
```

## <a name="next-steps"></a>Další kroky
* Doporučení, jak vytvořit šablony, najdete v části [osvědčené postupy pro vytváření šablon Azure Resource Manager](resource-manager-template-best-practices.md).

