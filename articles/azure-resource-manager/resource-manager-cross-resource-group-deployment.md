---
title: "Nasazení prostředků Azure do několika skupin prostředků | Microsoft Docs"
description: "Ukazuje, jak mít více než jedné skupiny prostředků Azure během nasazení."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: 
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/15/2017
ms.author: tomfitz
ms.openlocfilehash: d8b041213b269775175a810e585103d3c538557f
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="deploy-azure-resources-to-more-than-one-resource-group"></a>Nasadit Azure prostředky do více než jedné skupiny prostředků

Zpravidla nasazujete, všechny prostředky ve vaší šablony jedna skupina prostředků. Existují však scénáře, ve které chcete nasadit sadu prostředků společně, ale je umístit do jiné skupiny prostředků. Můžete například nasazení zálohování virtuálního počítače pro Azure Site Recovery na skupinu samostatné prostředků a umístění. Resource Manager umožňuje použití vnořených šablon pro různé skupiny prostředků než použití v šabloně nadřazené skupiny prostředků.

Skupina prostředků je kontejner životního cyklu pro aplikace a jeho kolekce prostředků. Vytvořte skupinu prostředků mimo šablonu a zadejte skupinu prostředků pro během nasazení. Úvod do skupiny prostředků, najdete v části [přehled Azure Resource Manageru](resource-group-overview.md).

## <a name="example-template"></a>Příklad šablony

Pokud chcete zacílit na různé zdroje, musíte použít šablonu vnořené nebo propojené během nasazení. `Microsoft.Resources/deployments` Nabízí typ prostředku `resourceGroup` parametr, který umožňuje určit jinou skupinu prostředků pro vnořené nasazení. Všechny skupiny prostředků musí existovat před spuštěním nasazení. Následující příklad nasadí dva účty úložiště – jeden ve skupině prostředků, které se zadávají během nasazení a po jednom v skupinu prostředků s názvem `crossResourceGroupDeployment`:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "StorageAccountName1": {
            "type": "string"
        },
        "StorageAccountName2": {
            "type": "string"
        }
    },
    "variables": {},
    "resources": [
        {
            "apiVersion": "2017-05-10",
            "name": "nestedTemplate",
            "type": "Microsoft.Resources/deployments",
            "resourceGroup": "crossResourceGroupDeployment",
            "properties": {
                "mode": "Incremental",
                "template": {
                    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
                    "contentVersion": "1.0.0.0",
                    "parameters": {},
                    "variables": {},
                    "resources": [
                        {
                            "type": "Microsoft.Storage/storageAccounts",
                            "name": "[parameters('StorageAccountName2')]",
                            "apiVersion": "2015-06-15",
                            "location": "West US",
                            "properties": {
                                "accountType": "Standard_LRS"
                            }
                        }
                    ]
                },
                "parameters": {}
            }
        },
        {
            "type": "Microsoft.Storage/storageAccounts",
            "name": "[parameters('StorageAccountName1')]",
            "apiVersion": "2015-06-15",
            "location": "West US",
            "properties": {
                "accountType": "Standard_LRS"
            }
        }
    ]
}
```

Pokud nastavíte `resourceGroup` název skupiny prostředků, který ještě neexistuje, nasazení se nezdaří. Pokud nezadáte hodnotu `resourceGroup`, správce prostředků používá nadřazené skupiny prostředků.  

## <a name="deploy-the-template"></a>Nasazení šablony

Pokud chcete nasadit šablonu příklad, můžete portálu, prostředí Azure PowerShell nebo rozhraní příkazového řádku Azure. Pro prostředí Azure PowerShell nebo rozhraní příkazového řádku Azure musíte použít verzi z může 2017 nebo novější. Příklady předpokládají, že šablona byla uložena místně jako soubor s názvem **crossrgdeployment.json**.

Pro prostředí PowerShell:

```powershell
Login-AzureRmAccount

New-AzureRmResourceGroup -Name mainResourceGroup -Location "South Central US"
New-AzureRmResourceGroup -Name crossResourceGroupDeployment -Location "Central US"
New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName mainResourceGroup `
  -TemplateFile c:\MyTemplates\crossrgdeployment.json
```

Pokud používáte Azure CLI:

```azurecli
az login

az group create --name mainResourceGroup --location "South Central US"
az group create --name crossResourceGroupDeployment --location "Central US"
az group deployment create \
    --name ExampleDeployment \
    --resource-group mainResourceGroup \
    --template-file crossrgdeployment.json
```

Po dokončení nasazení uvidíte dvě skupiny prostředků. Každé skupině prostředků obsahuje účet úložiště.

## <a name="use-resourcegroup-function"></a>Pomocí funkce resourceGroup()

Pro různé nasazení skupiny prostředků, [resouceGroup() funkce](resource-group-template-functions-resource.md#resourcegroup) vyřeší různě v závislosti na tom, jak je zadat vnořené šablony. 

Pokud vložit jednu šablonu v rámci jiné šablony resouceGroup() v šabloně vnořené přeloží do nadřazené skupiny prostředků. Šablonu embedded používá následující formát:

```json
"apiVersion": "2017-05-10",
"name": "embeddedTemplate",
"type": "Microsoft.Resources/deployments",
"resourceGroup": "crossResourceGroupDeployment",
"properties": {
    "mode": "Incremental",
    "template": {
        ...
        resourceGroup() refers to parent resource group
    }
}
```

Pokud jste odkaz na šablonu samostatné, resouceGroup() v šabloně propojené přeloží na skupině vnořeného prostředku. Propojené šablona používá následující formát:

```json
"apiVersion": "2017-05-10",
"name": "linkedTemplate",
"type": "Microsoft.Resources/deployments",
"resourceGroup": "crossResourceGroupDeployment",
"properties": {
    "mode": "Incremental",
    "templateLink": {
        ...
        resourceGroup() in linked template refers to linked resource group
    }
}
```

## <a name="next-steps"></a>Další kroky

* Chcete-li pochopit, jak definovat parametry v šabloně, přečtěte si téma [pochopit strukturu a syntaxe šablon Azure Resource Manager](resource-group-authoring-templates.md).
* Tipy k řešení běžných chyb při nasazení, naleznete v části [odstraňování běžných chyb nasazení Azure pomocí Azure Resource Manageru](resource-manager-common-deployment-errors.md).
* Informace o nasazení šablony, která vyžaduje tokenu SAS naleznete v tématu [privátní šablony nasazení s tokenem SAS](resource-manager-powershell-sas-token.md).
