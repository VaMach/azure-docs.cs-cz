---
title: "Nasazení prostředků do Azure | Dokumentace Microsoftu"
description: "Nasazení prostředků do Azure pomocí Azure PowerShellu nebo Azure CLI. Prostředky jsou definovány v šabloně Resource Manageru."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/16/2017
ms.author: tomfitz
translationtype: Human Translation
ms.sourcegitcommit: aaf97d26c982c1592230096588e0b0c3ee516a73
ms.openlocfilehash: 3b3162c335be43c9bf2a3d1c14b86cdd9b5d46b9
ms.lasthandoff: 04/27/2017


---
# <a name="deploy-resources-to-azure"></a>Nasazení prostředků do Azure

Toto téma ukazuje, jak nasadit prostředky do předplatného Azure. K nasazení šablony Resource Manageru, která definuje infrastrukturu řešení, můžete použít Azure PowerShell nebo Azure CLI.

Úvod ke konceptům Resource Manageru najdete v tématu [Přehled Azure Resource Manageru](resource-group-overview.md).

## <a name="steps-for-deployment"></a>Postup nasazení

Toto téma předpokládá, že nasazujete [ukázkovou šablonu úložiště](#example-storage-template) z tohoto tématu. Můžete použít jinou šablonu, ale předávané parametry budou jiné než v tomto tématu.

Po vytvoření šablony je obecný postup nasazení šablony následující:

1. Přihlášení k účtu
2. Výběr předplatného, které chcete použít (nutné pouze v případě, že máte více předplatných a chcete použít jiné než výchozí)
3. Vytvoření skupiny prostředků
4. Nasazení šablony
5. Kontrola stavu nasazení

Následující části ukazují provedení těchto kroků pomocí [PowerShellu](#powershell) nebo [Azure CLI](#azure-cli).

## <a name="powershell"></a>PowerShell

1. Informace o instalaci Azure PowerShellu najdete v tématu [Začínáme s rutinami Azure PowerShellu](/powershell/azure/overview).

2. Pokud chcete rychle začít s nasazením, použijte následující rutiny:

  ```powershell
  Login-AzureRmAccount
  Set-AzureRmContext -SubscriptionID {subscription-id}

  New-AzureRmResourceGroup -Name ExampleGroup -Location "Central US"
  New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleGroup -TemplateFile c:\MyTemplates\azuredeploy.json 
  ```

  Rutina `Set-AzureRmContext` je potřeba pouze v případě, že chcete použít jiné předplatné, než je vaše výchozí. Pokud chcete zobrazit všechna předplatná a jejich ID, použijte:

  ```powershell
  Get-AzureRmSubscription
  ```

3. Dokončení nasazení může trvat několik minut. Po dokončení se zobrazí zpráva podobná této:

  ```powershell
  DeploymentName          : ExampleDeployment
  CorrelationId           : 07b3b233-8367-4a0b-b9bc-7aa189065665
  ResourceGroupName       : ExampleGroup
  ProvisioningState       : Succeeded
  ...
  ```

4. Pokud chcete ověřit nasazení skupiny prostředků a účtu úložiště do předplatného, použijte:

  ```powershell
  Get-AzureRmResourceGroup -Name ExampleGroup
  Find-AzureRmResource -ResourceGroupNameEquals ExampleGroup
  ```

5. Při nasazování šablony můžete zadat parametry šablony jako parametry PowerShellu. Předchozí příklad nezahrnoval žádné parametry šablony, takže se použily výchozí hodnoty z šablony. Pokud chcete nasadit další účet úložiště a zadat hodnoty parametrů pro předponu názvu úložiště a skladovou položku úložiště, použijte:

  ```powershell
  New-AzureRmResourceGroupDeployment -Name ExampleDeployment2 -ResourceGroupName ExampleGroup -TemplateFile c:\MyTemplates\azuredeploy.json -storageNamePrefix "contoso" -storageSKU "Standard_GRS"
  ```

  Nyní máte ve skupině prostředků dva účty úložiště. 

## <a name="azure-cli"></a>Azure CLI

1. Informace o instalaci Azure CLI najdete v tématu [Instalace Azure CLI 2.0](/cli/azure/install-az-cli2).

2. Pokud chcete rychle začít s nasazením, použijte následující příkazy:

  ```azurecli
  az login
  az account set --subscription {subscription-id}

  az group create --name ExampleGroup --location "Central US"
  az group deployment create --name ExampleDeployment --resource-group ExampleGroup --template-file c:\MyTemplates\azuredeploy.json
  ```

  Příkaz `az account set` je potřeba pouze v případě, že chcete použít jiné předplatné, než je vaše výchozí. Pokud chcete zobrazit všechna předplatná a jejich ID, použijte:

  ```azurecli
  az account list
  ```

3. Dokončení nasazení může trvat několik minut. Po dokončení se zobrazí zpráva podobná této:

  ```azurecli
  "provisioningState": "Succeeded",
  ```

4. Pokud chcete ověřit nasazení skupiny prostředků a účtu úložiště do předplatného, použijte:

  ```azurecli
  az group show --name ExampleGroup
  az resource list --resource-group ExampleGroup
  ```

5. Při nasazování šablony můžete zadat parametry šablony jako parametry PowerShellu. Předchozí příklad nezahrnoval žádné parametry šablony, takže se použily výchozí hodnoty z šablony. Pokud chcete nasadit další účet úložiště a zadat hodnoty parametrů pro předponu názvu úložiště a skladovou položku úložiště, použijte:

  ```azurecli
  az group deployment create --name ExampleDeployment2 --resource-group ExampleGroup --template-file c:\MyTemplates\azuredeploy.json --parameters '{"storageNamePrefix":{"value":"contoso"},"storageSKU":{"value":"Standard_GRS"}}'
  ```

  Nyní máte ve skupině prostředků dva účty úložiště. 

## <a name="example-storage-template"></a>Příklad šablony úložiště

Následující příklad šablony můžete použít k nasazení účtu úložiště do předplatného:

```json
{
  "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "storageNamePrefix": {
      "type": "string",
      "maxLength": 11,
      "defaultValue": "storage",
      "metadata": {
        "description": "The value to use for starting the storage account name."
      }
    },
    "storageSKU": {
      "type": "string",
      "allowedValues": [
        "Standard_LRS",
        "Standard_ZRS",
        "Standard_GRS",
        "Standard_RAGRS",
        "Premium_LRS"
      ],
      "defaultValue": "Standard_LRS",
      "metadata": {
        "description": "The type of replication to use for the storage account."
      }
    }
  },
  "variables": {
    "storageName": "[concat(parameters('storageNamePrefix'), uniqueString(resourceGroup().id))]"
  },
  "resources": [
    {
      "name": "[variables('storageName')]",
      "type": "Microsoft.Storage/storageAccounts",
      "apiVersion": "2016-05-01",
      "sku": {
        "name": "[parameters('storageSKU')]"
      },
      "kind": "Storage",
      "location": "[resourceGroup().location]",
      "tags": {},
      "properties": {
      }
    }
  ],
  "outputs": {  }
}
```

## <a name="next-steps"></a>Další kroky

* Podrobné informace o použití PowerShellu k nasazení šablon najdete v tématu [Nasazení prostředků pomocí šablon Resource Manageru a Azure PowerShellu](/azure/azure-resource-manager/resource-group-template-deploy).
* Podrobné informace o použití Azure CLI k nasazení šablon najdete v tématu [Nasazení prostředků pomocí šablon Resource Manageru a Azure CLI](/azure/azure-resource-manager/resource-group-template-deploy-cli).




