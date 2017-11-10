---
title: "Tajný klíč Key Vault pomocí šablony Azure Resource Manager | Microsoft Docs"
description: "Ukazuje, jak předat tajného klíče z trezoru klíčů jako parametr během nasazení."
services: azure-resource-manager,key-vault
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/09/2017
ms.author: tomfitz
ms.openlocfilehash: e789a234979be877d990665902fd6219ae7ec40b
ms.sourcegitcommit: dcf5f175454a5a6a26965482965ae1f2bf6dca0a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
# <a name="use-azure-key-vault-to-pass-secure-parameter-value-during-deployment"></a>Použití Azure Key Vault předejte hodnotu parametru zabezpečení při nasazení

Když potřebujete předat hodnotu zabezpečení (třeba heslo) jako parametr během nasazení, můžete získat hodnotu z [Azure Key Vault](../key-vault/key-vault-whatis.md). Načíst hodnotu odkazem trezoru klíčů a tajný klíč v souboru parametrů. Hodnota je nikdy vystavena, protože budete odkazovat pouze na jeho ID trezoru klíčů. Nemusíte ručně zadat hodnotu pro tajný klíč pokaždé, když nasazujete prostředky. Trezor klíčů může existovat v jiném předplatném. než skupině prostředků, které nasazujete. Při odkazování na trezor klíčů, můžete zahrnout ID předplatného.

Při vytváření trezoru klíčů, nastavte *enabledForTemplateDeployment* vlastnost *true*. Nastavením této hodnoty na hodnotu true, můžete povolit přístup ze šablon Resource Manager během nasazení.

## <a name="deploy-a-key-vault-and-secret"></a>Nasazení trezoru klíčů a tajný klíč

Vytvoření trezoru klíčů a tajný klíč, pomocí rozhraní příkazového řádku Azure nebo PowerShell. Všimněte si, že key vault je povolena pro šablonu nasazení. 

Pokud používáte Azure CLI, použijte:

```azurecli-interactive
vaultname={your-unique-vault-name}
password={password-value}

az group create --name examplegroup --location 'South Central US'
az keyvault create \
  --name $vaultname \
  --resource-group examplegroup \
  --location 'South Central US' \
  --enabled-for-template-deployment true
az keyvault secret set --vault-name $vaultname --name examplesecret --value $password
```

Pokud používáte PowerShell, použijte:

```powershell
$vaultname = "{your-unique-vault-name}"
$password = "{password-value}"

New-AzureRmResourceGroup -Name examplegroup -Location "South Central US"
New-AzureRmKeyVault `
  -VaultName $vaultname `
  -ResourceGroupName examplegroup `
  -Location "South Central US" `
  -EnabledForTemplateDeployment
$secretvalue = ConvertTo-SecureString $password -AsPlainText -Force
Set-AzureKeyVaultSecret -VaultName $vaultname -Name "examplesecret" -SecretValue $secretvalue
```

## <a name="enable-access-to-the-secret"></a>Povolení přístupu k tajný klíč

Ať používáte nového trezoru klíčů nebo stávající, zajistí, že uživatel nasazení šablony můžete přístup tajný klíč. Uživatel nasazení šablonu, která odkazuje na tajný klíč musí mít `Microsoft.KeyVault/vaults/deploy/action` oprávnění pro trezor klíčů. [Vlastníka](../active-directory/role-based-access-built-in-roles.md#owner) a [Přispěvatel](../active-directory/role-based-access-built-in-roles.md#contributor) role obou udělit přístup.

## <a name="reference-a-secret-with-static-id"></a>Referenční tajný klíč s ID statické

Šablony, která přijímá tajný klíč trezoru klíčů je jako libovolné jiné šablony. Je to způsobeno **odkazujete trezoru klíčů v souboru parametrů, není šablona.** Například následující šablony nasadí databázi SQL, která zahrnuje heslo správce. Parametr hesla je nastaven na zabezpečený řetězec. Ale šablony neurčuje, kde tato hodnota pochází z.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "adminLogin": {
      "type": "string"
    },
    "adminPassword": {
      "type": "securestring"
    },
    "sqlServerName": {
      "type": "string"
    }
  },
  "resources": [
    {
      "name": "[parameters('sqlServerName')]",
      "type": "Microsoft.Sql/servers",
      "apiVersion": "2015-05-01-preview",
      "location": "[resourceGroup().location]",
      "tags": {},
      "properties": {
        "administratorLogin": "[parameters('adminLogin')]",
        "administratorLoginPassword": "[parameters('adminPassword')]",
        "version": "12.0"
      }
    }
  ],
  "outputs": {
  }
}
```

Teď vytvořte soubor parametrů pro předchozí šablonu. V souboru parametrů zadejte parametr, který odpovídá názvu parametru v šabloně. Pro hodnotu parametru odkazovat tajného klíče z trezoru klíčů. Tajný klíč odkazujete předáním identifikátor prostředku služby key vault a název tajný klíč. V následujícím příkladu tajný klíč trezoru klíčů již musí existovat a zadejte statickou hodnotu pro jeho ID prostředku.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "adminLogin": {
            "value": "exampleadmin"
        },
        "adminPassword": {
            "reference": {
              "keyVault": {
                "id": "/subscriptions/<subscription-id>/resourceGroups/examplegroup/providers/Microsoft.KeyVault/vaults/<vault-name>"
              },
              "secretName": "examplesecret"
            }
        },
        "sqlServerName": {
            "value": "<your-server-name>"
        }
    }
}
```

Nyní se nasazení šablony a předejte soubor parametru. Pokud používáte Azure CLI, použijte:

```azurecli-interactive
az group create --name datagroup --location "Central US"
az group deployment create \
    --name exampledeployment \
    --resource-group datagroup \
    --template-file sqlserver.json \
    --parameters @sqlserver.parameters.json
```

Pokud používáte PowerShell, použijte:

```powershell
New-AzureRmResourceGroup -Name datagroup -Location "Central US"
New-AzureRmResourceGroupDeployment `
  -Name exampledeployment `
  -ResourceGroupName datagroup `
  -TemplateFile sqlserver.json `
  -TemplateParameterFile sqlserver.parameters.json
```

## <a name="reference-a-secret-with-dynamic-id"></a>Referenční tajný klíč s dynamické ID

V předchozí části vám ukázal, jak předat ID statické prostředku pro tajný klíč trezoru klíčů. Ale v některých scénářích musíte tak, aby odkazovaly trezor klíčů tajný klíč, který se liší podle aktuální nasazení. V takovém případě nemůžete pevně ID prostředku v souboru parametrů. Bohužel nelze generovat dynamicky ID prostředku v souboru parametrů vzhledem k tomu, že šablona výrazy nejsou povoleny v souboru parametrů.

K dynamickému generování ID prostředku pro tajný klíč trezoru klíčů, musíte přesunout prostředek, který potřebuje tajný klíč do vnořené šablony. V šabloně hlavní přidání vnořené šablony a předat v parametru, který obsahuje ID dynamicky generovaném prostředku. Vnořené šablony musí být k dispozici prostřednictvím externí identifikátor URI. Zbývající část tohoto článku předpokládá šablona v předchozím příkladu jste přidali do účtu úložiště a je k dispozici prostřednictvím identifikátor URI - `https://<storage-name>.blob.core.windows.net/templatecontainer/sqlserver.json`.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
      "vaultName": {
        "type": "string"
      },
      "vaultResourceGroup": {
        "type": "string"
      },
      "secretName": {
        "type": "string"
      },
      "adminLogin": {
        "type": "string"
      },
      "sqlServerName": {
        "type": "string"
      }
    },
    "resources": [
    {
      "apiVersion": "2015-01-01",
      "name": "nestedTemplate",
      "type": "Microsoft.Resources/deployments",
      "properties": {
        "mode": "incremental",
        "templateLink": {
          "uri": "https://<storage-name>.blob.core.windows.net/templatecontainer/sqlserver.json",
          "contentVersion": "1.0.0.0"
        },
        "parameters": {
          "adminPassword": {
            "reference": {
              "keyVault": {
                "id": "[resourceId(subscription().subscriptionId,  parameters('vaultResourceGroup'), 'Microsoft.KeyVault/vaults', parameters('vaultName'))]"
              },
              "secretName": "[parameters('secretName')]"
            }
          },
          "adminLogin": { "value": "[parameters('adminLogin')]" },
          "sqlServerName": {"value": "[parameters('sqlServerName')]"}
        }
      }
    }],
    "outputs": {}
}
```

Nasadit předchozí šablonu a zadejte hodnoty pro parametry.

## <a name="next-steps"></a>Další kroky
* Obecné informace o trezorů klíčů najdete v tématu [Začínáme s Azure Key Vault](../key-vault/key-vault-get-started.md).
* Dokončení příklady odkazující na klíče tajné klíče, naleznete v tématu [Key Vault příklady](https://github.com/rjmax/ArmExamples/tree/master/keyvaultexamples).
