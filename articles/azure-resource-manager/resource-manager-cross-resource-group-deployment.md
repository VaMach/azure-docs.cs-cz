---
title: "Nasazení prostředků Azure do několika skupin předplatného a prostředků | Microsoft Docs"
description: "Ukazuje, jak mít více než jeden Azure předplatném nebo skupině prostředků během nasazení."
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
ms.date: 12/01/2017
ms.author: tomfitz
ms.openlocfilehash: 763f46b9b5be7edf06ee0604bfc51a2482405b60
ms.sourcegitcommit: 7136d06474dd20bb8ef6a821c8d7e31edf3a2820
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/05/2017
---
# <a name="deploy-azure-resources-to-more-than-one-subscription-or-resource-group"></a>Nasazení prostředků Azure k více než jedno předplatné nebo skupinu prostředků

Zpravidla nasazujete, všechny prostředky ve vaší šablony jedna skupina prostředků. Existují však scénáře, ve které chcete nasadit sadu prostředků společně ale umístěte je v různých skupinách prostředků nebo předplatných. Můžete například nasazení zálohování virtuálního počítače pro Azure Site Recovery na skupinu samostatné prostředků a umístění. Resource Manager umožňuje použití vnořených šablon cíl různých předplatných a skupin prostředků než předplatného a použití v šabloně nadřazené skupiny prostředků.

Skupina prostředků je kontejner životního cyklu pro aplikace a jeho kolekce prostředků. Vytvořte skupinu prostředků mimo šablonu a zadejte skupinu prostředků pro během nasazení. Úvod do skupiny prostředků, najdete v části [přehled Azure Resource Manageru](resource-group-overview.md).

## <a name="specify-a-subscription-and-resource-group"></a>Zadejte skupinu předplatného a prostředků

Pokud chcete zacílit na různé zdroje, musíte použít šablonu vnořené nebo propojené během nasazení. `Microsoft.Resources/deployments` Typ prostředku poskytuje parametry pro `subscriptionId` a `resourceGroup`. Tyto vlastnosti umožňují zadat jiné předplatné a prostředek skupiny pro vnořené nasazení. Všechny skupiny prostředků musí existovat před spuštěním nasazení. Pokud nezadáte skupině ID nebo prostředků předplatného, předplatné a skupina prostředků z nadřazené šablony se používá.

Následující příklad nasadí dva účty úložiště – jeden ve skupině prostředků, které se zadávají během nasazení, a v zadat jeden ve skupině prostředků `secondResourceGroup` parametr:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "storagePrefix": {
            "type": "string",
            "maxLength": 11
        },
        "secondResourceGroup": {
            "type": "string"
        },
        "secondSubscriptionID": {
            "type": "string",
            "defaultValue": ""
        },
        "secondStorageLocation": {
            "type": "string",
            "defaultValue": "[resourceGroup().location]"
        }
    },
    "variables": {
        "firstStorageName": "[concat(parameters('storagePrefix'), uniqueString(resourceGroup().id))]",
        "secondStorageName": "[concat(parameters('storagePrefix'), uniqueString(parameters('secondSubscriptionID'), parameters('secondResourceGroup')))]"
    },
    "resources": [
        {
            "apiVersion": "2017-05-10",
            "name": "nestedTemplate",
            "type": "Microsoft.Resources/deployments",
            "resourceGroup": "[parameters('secondResourceGroup')]",
            "subscriptionId": "[parameters('secondSubscriptionID')]",
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
                            "name": "[variables('secondStorageName')]",
                            "apiVersion": "2017-06-01",
                            "location": "[parameters('secondStorageLocation')]",
                            "sku":{
                                "name": "Standard_LRS"
                            },
                            "kind": "Storage",
                            "properties": {
                            }
                        }
                    ]
                },
                "parameters": {}
            }
        },
        {
            "type": "Microsoft.Storage/storageAccounts",
            "name": "[variables('firstStorageName')]",
            "apiVersion": "2017-06-01",
            "location": "[resourceGroup().location]",
            "sku":{
                "name": "Standard_LRS"
            },
            "kind": "Storage",
            "properties": {
            }
        }
    ]
}
```

Pokud nastavíte `resourceGroup` název skupiny prostředků, který ještě neexistuje, nasazení se nezdaří.

## <a name="deploy-the-template"></a>Nasazení šablony

Pokud chcete nasadit šablonu příklad, použijte verzi prostředí Azure PowerShell nebo rozhraní příkazového řádku Azure z může 2017 nebo novější. Pro tyto příklady, použijte [křížové předplatné šablony](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/crosssubscription.json) v Githubu.

### <a name="two-resource-groups-in-the-same-subscription"></a>Dvě skupiny prostředků v rámci stejného předplatného

Pro prostředí PowerShell abyste mohli nasadit dva účty úložiště na dvě skupiny prostředků ve stejném předplatném, použijte:

```powershell
$firstRG = "primarygroup"
$secondRG = "secondarygroup"

New-AzureRmResourceGroup -Name $firstRG -Location southcentralus
New-AzureRmResourceGroup -Name $secondRG -Location eastus

New-AzureRmResourceGroupDeployment `
  -ResourceGroupName $firstRG `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/crosssubscription.json `
  -storagePrefix storage `
  -secondResourceGroup $secondRG `
  -secondStorageLocation eastus
```

Pro rozhraní příkazového řádku Azure Pokud chcete nasadit dva účty úložiště na dvě skupiny prostředků v rámci stejného předplatného, použijte:

```azurecli-interactive
firstRG="primarygroup"
secondRG="secondarygroup"

az group create --name $firstRG --location southcentralus
az group create --name $secondRG --location eastus
az group deployment create \
  --name ExampleDeployment \
  --resource-group $firstRG \
  --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/crosssubscription.json \
  --parameters storagePrefix=tfstorage secondResourceGroup=$secondRG secondStorageLocation=eastus
```

Po dokončení nasazení uvidíte dvě skupiny prostředků. Každé skupině prostředků obsahuje účet úložiště.

### <a name="two-resource-groups-in-different-subscriptions"></a>Dvě skupiny prostředků v různých předplatných

Pro prostředí PowerShell Chcete-li nasadit dva účty úložiště pro obě předplatná, použijte:

```powershell
$firstRG = "primarygroup"
$secondRG = "secondarygroup"

$firstSub = "<first-subscription-id>"
$secondSub = "<second-subscription-id>"

Select-AzureRmSubscription -Subscription $secondSub
New-AzureRmResourceGroup -Name $secondRG -Location eastus

Select-AzureRmSubscription -Subscription $firstSub
New-AzureRmResourceGroup -Name $firstRG -Location southcentralus

New-AzureRmResourceGroupDeployment `
  -ResourceGroupName $firstRG `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/crosssubscription.json `
  -storagePrefix storage `
  -secondResourceGroup $secondRG `
  -secondStorageLocation eastus `
  -secondSubscriptionID $secondSub
```

Pokud používáte Azure CLI nasadit dva účty úložiště pro obě předplatná, použijte:

```azurecli-interactive
firstRG="primarygroup"
secondRG="secondarygroup"

firstSub="<first-subscription-id>"
secondSub="<second-subscription-id>"

az account set --subscription $secondSub
az group create --name $secondRG --location eastus

az account set --subscription $firstSub
az group create --name $firstRG --location southcentralus

az group deployment create \
  --name ExampleDeployment \
  --resource-group $firstRG \
  --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/crosssubscription.json \
  --parameters storagePrefix=storage secondResourceGroup=$secondRG secondStorageLocation=eastus secondSubscriptionID=$secondSub
```

## <a name="use-the-resourcegroup-function"></a>Použijte funkci resourceGroup()

Pro různé nasazení skupiny prostředků, [resourceGroup() funkce](resource-group-template-functions-resource.md#resourcegroup) vyřeší různě v závislosti na tom, jak je zadat vnořené šablony. 

Pokud vložit jednu šablonu v rámci jiné šablony resourceGroup() v šabloně vnořené přeloží do nadřazené skupiny prostředků. Šablonu embedded používá následující formát:

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

Pokud jste odkaz na šablonu samostatné, resourceGroup() v šabloně propojené přeloží na skupině vnořeného prostředku. Propojené šablona používá následující formát:

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

K testování různé způsoby `resourceGroup()` vyřeší, nasazení [příklad šablony](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/crossresourcegroupproperties.json) která vrací objekt skupiny prostředků pro nadřazený prvek šablony, šablony vložené a propojené šablony. Nadřízené a vložené šablonu, kterou obě přeložit na stejnou skupinu prostředků. Přeloží propojené šablony do skupiny propojeného prostředku.

Pokud používáte PowerShell, použijte:

```powershell
New-AzureRmResourceGroup -Name parentGroup -Location southcentralus
New-AzureRmResourceGroup -Name inlineGroup -Location southcentralus
New-AzureRmResourceGroup -Name linkedGroup -Location southcentralus

New-AzureRmResourceGroupDeployment `
  -ResourceGroupName parentGroup `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/crossresourcegroupproperties.json
```

Pokud používáte Azure CLI, použijte:

```azurecli-interactive
az group create --name parentGroup --location southcentralus
az group create --name inlineGroup --location southcentralus
az group create --name linkedGroup --location southcentralus

az group deployment create \
  --name ExampleDeployment \
  --resource-group parentGroup \
  --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/crossresourcegroupproperties.json 
```

## <a name="next-steps"></a>Další kroky

* Chcete-li pochopit, jak definovat parametry v šabloně, přečtěte si téma [pochopit strukturu a syntaxe šablon Azure Resource Manager](resource-group-authoring-templates.md).
* Tipy k řešení běžných chyb při nasazení, naleznete v části [odstraňování běžných chyb nasazení Azure pomocí Azure Resource Manageru](resource-manager-common-deployment-errors.md).
* Informace o nasazení šablony, která vyžaduje tokenu SAS naleznete v tématu [privátní šablony nasazení s tokenem SAS](resource-manager-powershell-sas-token.md).
