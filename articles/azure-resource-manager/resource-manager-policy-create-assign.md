---
title: "Přiřadit a spravovat zásady prostředků Azure | Microsoft Docs"
description: "Popisuje, jak chcete použít zásady prostředků Azure pro skupiny prostředků a předplatná a postup zobrazení zásad prostředků."
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
ms.date: 09/19/2017
ms.author: tomfitz
ms.openlocfilehash: 64bdd6ed41e98079c8d4112e895aaeddcd629282
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="assign-and-manage-resource-policies"></a>Přiřadit a spravovat zásady prostředků

Chcete-li implementovat zásady, musíte provést tyto kroky:

1. Definice zásad (včetně integrovaných zásad poskytovaný platformou Azure) zkontrolujte, zda jeden již existuje v rámci vašeho předplatného, která splňuje vaše požadavky.
2. Pokud existuje, získáte její název.
3. Pokud žádný neexistuje, definovat pravidlo zásad textem JSON a přidejte jej jako definici zásady v rámci vašeho předplatného. Tento krok zpřístupní zásady pro přiřazení, ale není použití pravidel pro vaše předplatné.
4. V obou případech přiřaďte zásady oboru (například předplatné nebo prostředek skupiny). Pravidla zásad se teď vynucují.

Tento článek se zaměřuje na kroky k vytvoření definice zásady a přiřazení této definici obor prostřednictvím REST API, Powershellu nebo rozhraní příkazového řádku Azure. Pokud byste radši chtěli použít portálu přiřadit zásady, najdete v článku [portálu Azure použijte přiřadit a spravovat zásady prostředků](resource-manager-policy-portal.md). V tomto článku není soustředit na syntaxi pro vytvoření definice zásady. Informace o syntaxi zásad najdete v tématu [přehled zásad prostředků](resource-manager-policy.md).

## <a name="exclusion-scopes"></a>Rozsahy vyloučení

Můžete vyloučit obor, při přiřazování zásady. Tato funkce zjednodušuje přiřazení zásad, protože můžete přiřadit zásady na úrovni předplatného, ale zadejte, kdy není používá zásady. Například v rámci vašeho předplatného, máte skupinu prostředků, který je určený pro síťové infrastruktury. Týmy aplikací nasadit své prostředky do jiných skupin prostředků. Nechcete, aby tyto týmy pro vytvoření síťové prostředky, které může způsobit problémy se zabezpečením. Ale ve skupině prostředků sítě, budete chtít povolit síťovým prostředkům. Přiřazení zásad na úrovni předplatného, ale vyloučit skupina síťových prostředků. Můžete určit více oborů sub.

```json
{
    "properties":{
        "policyDefinitionId":"<ID for policy definition>",
        "notScopes":[
            "/subscriptions/<subid>/resourceGroups/networkresourceGroup1"
        ]
    }
}
```

Pokud zadáte rozsah vyloučení v přiřazení, použijte **2017-06-01-preview** verze rozhraní API.

## <a name="rest-api"></a>REST API

### <a name="create-policy-definition"></a>Vytvoření definice zásady

Můžete vytvořit a zásadou [REST API pro definice zásady](/rest/api/resources/policydefinitions). Rozhraní REST API umožňuje vytvářet a odstraňovat definice zásady a získat informace o existující definice.

K vytvoření definice zásady, spusťte příkaz:

```HTTP
PUT https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.authorization/policydefinitions/{policyDefinitionName}?api-version={api-version}
```

Zahrnout obsah žádosti podobně jako v následujícím příkladu:

```json
{
  "properties": {
    "parameters": {
      "allowedLocations": {
        "type": "array",
        "metadata": {
          "description": "The list of locations that can be specified when deploying resources",
          "strongType": "location",
          "displayName": "Allowed locations"
        }
      }
    },
    "displayName": "Allowed locations",
    "description": "This policy enables you to restrict the locations your organization can specify when deploying resources.",
    "policyRule": {
      "if": {
        "not": {
          "field": "location",
          "in": "[parameters('allowedLocations')]"
        }
      },
      "then": {
        "effect": "deny"
      }
    }
  }
}
```

### <a name="assign-policy"></a>Přiřazení zásad

Můžete použít definice zásady na požadovaný rozsah prostřednictvím [REST API pro přiřazení zásad](/rest/api/resources/policyassignments). Rozhraní REST API umožňuje vytvářet a odstraňovat přiřazení zásad a získat informace o existující přiřazení.

Chcete-li vytvořit přiřazení zásady, spusťte:

```HTTP
PUT https://management.azure.com /subscriptions/{subscription-id}/providers/Microsoft.authorization/policyassignments/{policyAssignmentName}?api-version={api-version}
```

{-Přiřazení zásady} je název přiřazení zásady.

Zahrnout obsah žádosti podobně jako v následujícím příkladu:

```json
{
  "properties":{
    "displayName":"West US only policy assignment on the subscription ",
    "description":"Resources can only be provisioned in West US regions",
    "parameters": {
      "allowedLocations": { "value": ["northeurope", "westus"] }
     },
    "policyDefinitionId":"/subscriptions/{subscription-id}/providers/Microsoft.Authorization/policyDefinitions/{definition-name}",
      "scope":"/subscriptions/{subscription-id}"
  },
}
```

### <a name="view-policy"></a>Zobrazení zásad
Zásady, použijte [získat definice zásady](https://docs.microsoft.com/rest/api/resources/policydefinitions#PolicyDefinitions_Get) operaci.

### <a name="get-aliases"></a>Získat aliasy
Můžete načíst aliasy přes rozhraní REST API:

```HTTP
GET /subscriptions/{id}/providers?$expand=resourceTypes/aliases&api-version=2015-11-01
```

Následující příklad ukazuje definici alias. Jak vidíte, definuje alias cesty v různých verzích rozhraní API, i když dojde ke změně názvu vlastnosti. 

```json
"aliases": [
    {
      "name": "Microsoft.Storage/storageAccounts/sku.name",
      "paths": [
        {
          "path": "properties.accountType",
          "apiVersions": [
            "2015-06-15",
            "2015-05-01-preview"
          ]
        },
        {
          "path": "sku.name",
          "apiVersions": [
            "2016-01-01"
          ]
        }
      ]
    }
]
```

## <a name="powershell"></a>PowerShell

Než budete pokračovat s příklady prostředí PowerShell, zajistěte, aby byla [nainstalovali nejnovější verzi](/powershell/azure/install-azurerm-ps) prostředí Azure PowerShell. Ve verzi 3.6.0 byly přidány zásady parametry. Pokud máte starší verzi, příklady vrátí chybu oznamující, že parametr nebyl nalezen.

### <a name="view-policy-definitions"></a>Definice zásad zobrazení
Pokud chcete zobrazit všechny definice zásady v rámci vašeho předplatného, použijte následující příkaz:

```powershell
Get-AzureRmPolicyDefinition
```

Vrátí všechny dostupné zásady definice, včetně integrovaných zásad. Každá zásada se vrátí v následujícím formátu:

```powershell
Name               : e56962a6-4747-49cd-b67b-bf8b01975c4c
ResourceId         : /providers/Microsoft.Authorization/policyDefinitions/e56962a6-4747-49cd-b67b-bf8b01975c4c
ResourceName       : e56962a6-4747-49cd-b67b-bf8b01975c4c
ResourceType       : Microsoft.Authorization/policyDefinitions
Properties         : @{displayName=Allowed locations; policyType=BuiltIn; description=This policy enables you to
                     restrict the locations your organization can specify when deploying resources. Use to enforce
                     your geo-compliance requirements.; parameters=; policyRule=}
PolicyDefinitionId : /providers/Microsoft.Authorization/policyDefinitions/e56962a6-4747-49cd-b67b-bf8b01975c4c
```

Než budete pokračovat k vytvoření definice zásady, podívejte se na integrovaných zásad. Pokud zjistíte předdefinované zásady, které platí omezení, které potřebujete, můžete přeskočit vytvoření definice zásady. Předdefinované zásady místo toho přiřadíte požadovaný rozsah.

### <a name="create-policy-definition"></a>Vytvoření definice zásady
Můžete vytvořit pomocí definice zásady `New-AzureRmPolicyDefinition` rutiny.

K vytvoření definice zásad ze souboru, předejte cestu k souboru. Pro externí soubor použijte:

```powershell
$definition = New-AzureRmPolicyDefinition `
    -Name denyCoolTiering `
    -DisplayName "Deny cool access tiering for storage" `
    -Policy 'https://raw.githubusercontent.com/Azure/azure-policy-samples/master/samples/Storage/storage-account-access-tier/azurepolicy.rules.json'
```

Pro místní soubor použijte:

```powershell
$definition = New-AzureRmPolicyDefinition `
    -Name denyCoolTiering `
    -Description "Deny cool access tiering for storage" `
    -Policy "c:\policies\coolAccessTier.json"
```

K vytvoření definice zásady s vložené pravidlo, použijte:

```powershell
$definition = New-AzureRmPolicyDefinition -Name denyCoolTiering -Description "Deny cool access tiering for storage" -Policy '{
  "if": {
    "allOf": [
      {
        "field": "type",
        "equals": "Microsoft.Storage/storageAccounts"
      },
      {
        "field": "kind",
        "equals": "BlobStorage"
      },
      {
        "not": {
          "field": "Microsoft.Storage/storageAccounts/accessTier",
          "equals": "cool"
        }
      }
    ]
  },
  "then": {
    "effect": "deny"
  }
}'
```            

Výstup je uložen v `$definition` objekt, který se používá při přiřazování zásady. 

Následující příklad vytvoří definici zásady, který obsahuje parametry:

```powershell
$policy = '{
    "if": {
        "allOf": [
            {
                "field": "type",
                "equals": "Microsoft.Storage/storageAccounts"
            },
            {
                "not": {
                    "field": "location",
                    "in": "[parameters(''allowedLocations'')]"
                }
            }
        ]
    },
    "then": {
        "effect": "Deny"
    }
}'

$parameters = '{
    "allowedLocations": {
        "type": "array",
        "metadata": {
          "description": "The list of locations that can be specified when deploying storage accounts.",
          "strongType": "location",
          "displayName": "Allowed locations"
        }
    }
}' 

$definition = New-AzureRmPolicyDefinition -Name storageLocations -Description "Policy to specify locations for storage accounts." -Policy $policy -Parameter $parameters 
```

### <a name="assign-policy"></a>Přiřazení zásad

Použít v souladu se zásadami požadovaný rozsah pomocí `New-AzureRmPolicyAssignment` rutiny. Následující příklad přiřadí zásady do skupiny prostředků.

```powershell
$rg = Get-AzureRmResourceGroup -Name "ExampleGroup"
New-AzureRMPolicyAssignment -Name accessTierAssignment -Scope $rg.ResourceId -PolicyDefinition $definition
```

Přiřazení zásad, která vyžaduje parametry, vytvořte a objekt s těmito hodnotami. Následující příklad načte předdefinovaných zásad a předá hodnoty parametrů:

```powershell
$rg = Get-AzureRmResourceGroup -Name "ExampleGroup"
$definition = Get-AzureRmPolicyDefinition -Id /providers/Microsoft.Authorization/policyDefinitions/e5662a6-4747-49cd-b67b-bf8b01975c4c
$array = @("West US", "West US 2")
$param = @{"listOfAllowedLocations"=$array}
New-AzureRMPolicyAssignment -Name locationAssignment -Scope $rg.ResourceId -PolicyDefinition $definition -PolicyParameterObject $param
```

### <a name="view-policy-assignment"></a>Přiřazení zásady zobrazení

Přiřazení konkrétní zásady, použijte:

```powershell
$rg = Get-AzureRmResourceGroup -Name "ExampleGroup"
(Get-AzureRmPolicyAssignment -Name accessTierAssignment -Scope $rg.ResourceId
```

Chcete-li zobrazit pravidlo zásad pro definici zásady, použijte:

```powershell
(Get-AzureRmPolicyDefinition -Name coolAccessTier).Properties.policyRule | ConvertTo-Json
```

### <a name="remove-policy-assignment"></a>Odebrat přiřazení zásady 

Chcete-li odebrat přiřazení zásady, použijte:

```powershell
Remove-AzureRmPolicyAssignment -Name regionPolicyAssignment -Scope /subscriptions/{subscription-id}/resourceGroups/{resource-group-name}
```

## <a name="azure-cli"></a>Azure CLI

### <a name="view-policy-definitions"></a>Definice zásad zobrazení
Pokud chcete zobrazit všechny definice zásady v rámci vašeho předplatného, použijte následující příkaz:

```azurecli
az policy definition list
```

Vrátí všechny dostupné zásady definice, včetně integrovaných zásad. Každá zásada se vrátí v následujícím formátu:

```azurecli
{                                                            
  "description": "This policy enables you to restrict the locations your organization can specify when deploying resources. Use to enforce your geo-compliance requirements.",                      
  "displayName": "Allowed locations",
  "id": "/providers/Microsoft.Authorization/policyDefinitions/e56962a6-4747-49cd-b67b-bf8b01975c4c",
  "name": "e56962a6-4747-49cd-b67b-bf8b01975c4c",
  "policyRule": {
    "if": {
      "not": {
        "field": "location",
        "in": "[parameters('listOfAllowedLocations')]"
      }
    },
    "then": {
      "effect": "Deny"
    }
  },
  "policyType": "BuiltIn"
}
```

Než budete pokračovat k vytvoření definice zásady, podívejte se na integrovaných zásad. Pokud zjistíte předdefinované zásady, které platí omezení, které potřebujete, můžete přeskočit vytvoření definice zásady. Předdefinované zásady místo toho přiřadíte požadovaný rozsah.

### <a name="create-policy-definition"></a>Vytvoření definice zásady

Můžete vytvořit definici zásady pomocí rozhraní příkazového řádku Azure pomocí příkazu definice zásady.

K vytvoření definice zásady s vložené pravidlo, použijte:

```azurecli
az policy definition create --name denyCoolTiering --description "Deny cool access tiering for storage" --rules '{
  "if": {
    "allOf": [
      {
        "field": "type",
        "equals": "Microsoft.Storage/storageAccounts"
      },
      {
        "field": "kind",
        "equals": "BlobStorage"
      },
      {
        "not": {
          "field": "Microsoft.Storage/storageAccounts/accessTier",
          "equals": "cool"
        }
      }
    ]
  },
  "then": {
    "effect": "deny"
  }
}'    
```

### <a name="assign-policy"></a>Přiřazení zásad

Zásady můžete použít k požadovaný rozsah pomocí příkazu přiřazení zásad. Následující příklad přiřadí zásadu do skupiny prostředků.

```azurecli
az policy assignment create --name coolAccessTierAssignment --policy coolAccessTier --scope /subscriptions/{subscription-id}/resourceGroups/{resource-group-name}
```

### <a name="view-policy-assignment"></a>Přiřazení zásady zobrazení

Chcete-li zobrazit přiřazení zásady, zadejte název přiřazení zásady a oboru:

```azurecli
az policy assignment show --name coolAccessTierAssignment --scope "/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}"
```

### <a name="remove-policy-assignment"></a>Odebrat přiřazení zásady 

Chcete-li odebrat přiřazení zásady, použijte:

```azurecli
az policy assignment delete --name coolAccessTier --scope /subscriptions/{subscription-id}/resourceGroups/{resource-group-name}
```

## <a name="next-steps"></a>Další kroky
* Pokyny k tomu, jak můžou podniky používat Resource Manager k efektivní správě předplatných, najdete v části [Základní kostra Azure Enterprise – zásady správného řízení pro předplatná](resource-manager-subscription-governance.md).

