---
title: "Tajný klíč Key Vault pomocí šablony Resource Manageru | Microsoft Docs"
description: "Ukazuje, jak předat tajného klíče z trezoru klíčů jako parametr během nasazení."
services: azure-resource-manager,key-vault
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: c582c144-4760-49d3-b793-a3e1e89128e2
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/25/2017
ms.author: tomfitz
ms.openlocfilehash: 1ca72599e67e79d42a3d430dbb13e89ea7265334
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="use-key-vault-to-pass-secure-parameter-value-during-deployment"></a>Předejte hodnotu parametru zabezpečení při nasazení pomocí Key Vault

Když potřebujete předat hodnotu zabezpečení (třeba heslo) jako parametr během nasazení, můžete získat hodnotu z [Azure Key Vault](../key-vault/key-vault-whatis.md). Načíst hodnotu odkazem trezoru klíčů a tajný klíč v souboru parametrů. Hodnota je nikdy vystavena, protože budete odkazovat pouze na jeho ID trezoru klíčů. Nemusíte ručně zadat hodnotu pro tajný klíč pokaždé, když nasazujete prostředky. Trezor klíčů může existovat v jiném předplatném. než skupině prostředků, které nasazujete. Při odkazování na trezor klíčů, můžete zahrnout ID předplatného.

Při vytváření trezoru klíčů, nastavte *enabledForTemplateDeployment* vlastnost *true*. Nastavením této hodnoty na hodnotu true, můžete povolit přístup ze šablon Resource Manager během nasazení.  

## <a name="deploy-a-key-vault-and-secret"></a>Nasazení trezoru klíčů a tajný klíč

Vytvoření trezoru klíčů a tajný klíč, pomocí rozhraní příkazového řádku Azure nebo PowerShell. Všimněte si, že key vault je povolena pro šablonu nasazení. 

Pokud používáte Azure CLI, použijte:

```azurecli
vaultname={your-unique-vault-name}
password={password-value}

az group create --name examplegroup --location 'South Central US'
az keyvault create --name $vaultname --resource-group examplegroup --location 'South Central US' --enabled-for-template-deployment true
az keyvault secret set --vault-name $vaultname --name examplesecret --value $password
```

Pokud používáte PowerShell, použijte:

```powershell
$vaultname = "{your-unique-vault-name}"
$password = "{password-value}"

New-AzureRmResourceGroup -Name examplegroup -Location "South Central US"
New-AzureRmKeyVault -VaultName $vaultname -ResourceGroupName examplegroup -Location "South Central US" -EnabledForTemplateDeployment
$secretvalue = ConvertTo-SecureString $password -AsPlainText -Force
Set-AzureKeyVaultSecret -VaultName $vaultname -Name "examplesecret" -SecretValue $secretvalue
```

## <a name="enable-access-to-the-secret"></a>Povolení přístupu k tajný klíč

Ať používáte nového trezoru klíčů nebo stávající, zajistí, že uživatel nasazení šablony můžete přístup tajný klíč. Uživatel nasazení šablonu, která odkazuje na tajný klíč musí mít `Microsoft.KeyVault/vaults/deploy/action` oprávnění pro trezor klíčů. [Vlastníka](../active-directory/role-based-access-built-in-roles.md#owner) a [Přispěvatel](../active-directory/role-based-access-built-in-roles.md#contributor) role obou udělit přístup. Můžete také vytvořit [vlastní role](../active-directory/role-based-access-control-custom-roles.md) který uděluje toto oprávnění a přidejte uživatele do této role. Informace o přidání uživatele k roli najdete v tématu [přiřazení uživatele k rolí správce ve službě Azure Active Directory](../active-directory/active-directory-users-assign-role-azure-portal.md).


## <a name="reference-a-secret-with-static-id"></a>Referenční tajný klíč s ID statické

Šablony, která přijímá tajný klíč trezoru klíčů je jako libovolné jiné šablony. Je to způsobeno **odkazujete trezoru klíčů v souboru parametrů, není šablona.** Například následující šablony nasadí databázi SQL, která zahrnuje heslo správce. Parametr hesla je nastaven na zabezpečený řetězec. Ale šablony neurčuje, kde tato hodnota pochází z.

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "administratorLogin": {
            "type": "string"
        },
        "administratorLoginPassword": {
            "type": "securestring"
        },
        "collation": {
            "type": "string"
        },
        "databaseName": {
            "type": "string"
        },
        "edition": {
            "type": "string"
        },
        "requestedServiceObjectiveId": {
            "type": "string"
        },
        "location": {
            "type": "string"
        },
        "maxSizeBytes": {
            "type": "string"
        },
        "serverName": {
            "type": "string"
        },
        "sampleName": {
            "type": "string",
            "defaultValue": ""
        }
    },
    "resources": [
        {
            "apiVersion": "2015-05-01-preview",
            "location": "[parameters('location')]",
            "name": "[parameters('serverName')]",
            "properties": {
                "administratorLogin": "[parameters('administratorLogin')]",
                "administratorLoginPassword": "[parameters('administratorLoginPassword')]",
                "version": "12.0"
            },
            "resources": [
                {
                    "apiVersion": "2014-04-01-preview",
                    "dependsOn": [
                        "[concat('Microsoft.Sql/servers/', parameters('serverName'))]"
                    ],
                    "location": "[parameters('location')]",
                    "name": "[parameters('databaseName')]",
                    "properties": {
                        "collation": "[parameters('collation')]",
                        "edition": "[parameters('edition')]",
                        "maxSizeBytes": "[parameters('maxSizeBytes')]",
                        "requestedServiceObjectiveId": "[parameters('requestedServiceObjectiveId')]",
                        "sampleName": "[parameters('sampleName')]"
                    },
                    "type": "databases"
                },
                {
                    "apiVersion": "2014-04-01-preview",
                    "dependsOn": [
                        "[concat('Microsoft.Sql/servers/', parameters('serverName'))]"
                    ],
                    "location": "[parameters('location')]",
                    "name": "AllowAllWindowsAzureIps",
                    "properties": {
                        "endIpAddress": "0.0.0.0",
                        "startIpAddress": "0.0.0.0"
                    },
                    "type": "firewallrules"
                }
            ],
            "type": "Microsoft.Sql/servers"
        }
    ]
}
```

Teď vytvořte soubor parametrů pro předchozí šablonu. V souboru parametrů zadejte parametr, který odpovídá názvu parametru v šabloně. Pro hodnotu parametru odkazovat tajného klíče z trezoru klíčů. Tajný klíč odkazujete předáním identifikátor prostředku služby key vault a název tajný klíč. V následujícím příkladu tajný klíč trezoru klíčů již musí existovat a zadejte statickou hodnotu pro jeho ID prostředku.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "administratorLogin": {
            "value": "exampleadmin"
        },
        "administratorLoginPassword": {
            "reference": {
              "keyVault": {
                "id": "/subscriptions/{guid}/resourceGroups/examplegroup/providers/Microsoft.KeyVault/vaults/{vault-name}"
              },
              "secretName": "examplesecret"
            }
        },
        "collation": {
            "value": "SQL_Latin1_General_CP1_CI_AS"
        },
        "databaseName": {
            "value": "exampledb"
        },
        "edition": {
            "value": "Standard"
        },
        "location": {
            "value": "southcentralus"
        },
        "maxSizeBytes": {
            "value": "268435456000"
        },
        "requestedServiceObjectiveId": {
            "value": "455330e1-00cd-488b-b5fa-177c226f28b7"
        },
        "sampleName": {
            "value": ""
        },
        "serverName": {
            "value": "exampleserver"
        }
    }
}
```

## <a name="reference-a-secret-with-dynamic-id"></a>Referenční tajný klíč s dynamické ID

V předchozí části vám ukázal, jak předat ID statické prostředku pro tajný klíč trezoru klíčů. Ale v některých scénářích musíte tak, aby odkazovaly trezor klíčů tajný klíč, který se liší podle aktuální nasazení. V takovém případě nemůžete pevně ID prostředku v souboru parametrů. Bohužel nelze generovat dynamicky ID prostředku v souboru parametrů vzhledem k tomu, že šablona výrazy nejsou povoleny v souboru parametrů.

K dynamickému generování ID prostředku pro tajný klíč trezoru klíčů, musíte přesunout prostředek, který potřebuje tajný klíč do vnořené šablony. V šabloně hlavní přidání vnořené šablony a předat v parametru, který obsahuje ID dynamicky generovaném prostředku.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
      "vaultName": {
        "type": "string"
      },
      "secretName": {
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
          "uri": "https://www.contoso.com/AzureTemplates/newVM.json",
          "contentVersion": "1.0.0.0"
        },
        "parameters": {
          "adminPassword": {
            "reference": {
              "keyVault": {
                "id": "[concat(resourceGroup().id, '/providers/Microsoft.KeyVault/vaults/', parameters('vaultName'))]"
              },
              "secretName": "[parameters('secretName')]"
            }
          }
        }
      }
    }],
    "outputs": {}
}
```

## <a name="next-steps"></a>Další kroky
* Obecné informace o trezorů klíčů najdete v tématu [Začínáme s Azure Key Vault](../key-vault/key-vault-get-started.md).
* Dokončení příklady odkazující na klíče tajné klíče, naleznete v tématu [Key Vault příklady](https://github.com/rjmax/ArmExamples/tree/master/keyvaultexamples).

