---
title: "Připojení Azure Files svazku v Azure kontejner instancí"
description: "Zjistěte, jak připojit Azure Files svazek k uchování stavu s instancemi Azure kontejneru"
services: container-instances
documentationcenter: 
author: seanmck
manager: timlt
editor: 
tags: 
keywords: 
ms.assetid: 
ms.service: container-instances
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/31/2017
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: 41c3a449b39d6ef77e1dd0cf10699f8debcad475
ms.sourcegitcommit: 54fd091c82a71fbc663b2220b27bc0b691a39b5b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/12/2017
---
# <a name="mounting-an-azure-file-share-with-azure-container-instances"></a>Připojení sdílenou složku Azure s instancemi Azure kontejneru

Ve výchozím nastavení jsou bezstavové instancí kontejnerů Azure. Pokud kontejner dojde k chybě nebo zastaví, všechny její stav bude ztracena. K zachování stavu nad rámec životnost kontejneru, je nutné připojit svazek z externího úložiště. Tento článek ukazuje, jak připojit sdílenou složku Azure pro použití s instancemi Azure kontejneru.

## <a name="create-an-azure-file-share"></a>Vytvořte sdílenou složku Azure

Před použitím sdílenou složku Azure s Azure kontejner instancí, je třeba ji vytvořit. Spusťte následující skript pro vytvoření účtu úložiště pro hostování sdílené složky a sdílené složky sám sebe. Název účtu úložiště musí být globálně jedinečné, takže skript přidá do základní řetězec náhodná hodnota.

```azurecli-interactive
# Change these four parameters
ACI_PERS_STORAGE_ACCOUNT_NAME=mystorageaccount$RANDOM
ACI_PERS_RESOURCE_GROUP=myResourceGroup
ACI_PERS_LOCATION=eastus
ACI_PERS_SHARE_NAME=acishare

# Create the storage account with the parameters
az storage account create -n $ACI_PERS_STORAGE_ACCOUNT_NAME -g $ACI_PERS_RESOURCE_GROUP -l $ACI_PERS_LOCATION --sku Standard_LRS

# Export the connection string as an environment variable, this is used when creating the Azure file share
export AZURE_STORAGE_CONNECTION_STRING=`az storage account show-connection-string -n $ACI_PERS_STORAGE_ACCOUNT_NAME -g $ACI_PERS_RESOURCE_GROUP -o tsv`

# Create the share
az storage share create -n $ACI_PERS_SHARE_NAME
```

## <a name="acquire-storage-account-access-details"></a>Získat podrobnosti o přístup k účtu úložiště

Chcete-li jako svazek v instancí kontejnerů Azure připojit sdílenou složku Azure, je třeba tří hodnot: název účtu úložiště, název sdílené složky a přístupový klíč úložiště.

Pokud jste použili výše uvedený skript, název účtu úložiště byl vytvořen s hodnotou náhodných na konci. Chcete-li prohledávat posledním řetězci (včetně náhodných část), použijte následující příkazy:

```azurecli-interactive
STORAGE_ACCOUNT=$(az storage account list --resource-group $ACI_PERS_RESOURCE_GROUP --query "[?contains(name,'$ACI_PERS_STORAGE_ACCOUNT_NAME')].[name]" -o tsv)
echo $STORAGE_ACCOUNT
```

Název sdílené složky je již znám (je *acishare* ve skriptu výše), takže všechny, že zůstanou je klíč účtu úložiště, které lze najít pomocí následujícího příkazu:

```azurecli-interactive
STORAGE_KEY=$(az storage account keys list --resource-group $ACI_PERS_RESOURCE_GROUP --account-name $STORAGE_ACCOUNT --query "[0].value" -o tsv)
echo $STORAGE_KEY
```

## <a name="store-storage-account-access-details-with-azure-key-vault"></a>Ukládání podrobností pro přístup k účtu úložiště Azure klíče trezoru

Klíče účtu úložiště chránit přístup k datům, proto doporučujeme uložit je do v Azure klíče trezoru.

Vytvoření trezoru klíčů s Azure CLI:

```azurecli-interactive
KEYVAULT_NAME=aci-keyvault
az keyvault create -n $KEYVAULT_NAME --enabled-for-template-deployment -g $ACI_PERS_RESOURCE_GROUP
```

`enabled-for-template-deployment` Přepínač umožňuje Azure Resource Manageru pro vyžádání obsahu tajné klíče z trezoru klíčů v době nasazení.

Ukládání klíče účtu úložiště jako nový sdílený tajný klíč v trezoru klíčů:

```azurecli-interactive
KEYVAULT_SECRET_NAME=azurefilesstoragekey
az keyvault secret set --vault-name $KEYVAULT_NAME --name $KEYVAULT_SECRET_NAME --value $STORAGE_KEY
```

## <a name="mount-the-volume"></a>Připojte svazek

Připojení sdílenou složku Azure jako svazek v kontejneru je dvoustupňový proces. Nejdřív zadejte podrobnosti o sdílenou složku jako součást definice skupina kontejneru, pak je určit, jakým způsobem chcete svazek připojený do jedné nebo více kontejnerů ve skupině.

Chcete-li definovat svazky, které mají být k dispozici pro připojení, přidejte `volumes` pole do kontejneru skupiny definice v šabloně Azure Resource Manager a pak na ně odkazovat v definici jednotlivých kontejnerů.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "storageaccountname": {
      "type": "string"
    },
    "storageaccountkey": {
      "type": "securestring"
    }
  },
  "resources":[{
    "name": "hellofiles",
    "type": "Microsoft.ContainerInstance/containerGroups",
    "apiVersion": "2017-08-01-preview",
    "location": "[resourceGroup().location]",
    "properties": {
      "containers": [{
        "name": "hellofiles",
        "properties": {
          "image": "seanmckenna/aci-hellofiles",
          "resources": {
            "requests": {
              "cpu": 1,
              "memoryInGb": 1.5
            }
          },
          "ports": [{
            "port": 80
          }],
          "volumeMounts": [{
            "name": "myvolume",
            "mountPath": "/aci/logs/"
          }]
        }
      }],
      "osType": "Linux",
      "ipAddress": {
        "type": "Public",
        "ports": [{
          "protocol": "tcp",
          "port": "80"
        }]
      },
      "volumes": [{
        "name": "myvolume",
        "azureFile": {
          "shareName": "acishare",
          "storageAccountName": "[parameters('storageaccountname')]",
          "storageAccountKey": "[parameters('storageaccountkey')]"
        }
      }]
    }
  }]
}
```

Šablona obsahuje název účtu úložiště a klíč jako parametry, které lze zadat do souboru samostatné parametry. K naplnění souboru parametrů, je nutné tří hodnot: název účtu úložiště, ID prostředku trezoru klíčů Azure a tajný název trezoru klíčů, který jste použili k uložení klíče úložiště. Pokud jste postupovali podle předchozích kroků, můžete získat tyto hodnoty s následující skript:

```azurecli-interactive
echo $STORAGE_ACCOUNT
echo $KEYVAULT_SECRET_NAME
az keyvault show --name $KEYVAULT_NAME --query [id] -o tsv
```

Vložení hodnoty do souboru parametrů:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "storageaccountname": {
      "value": "<my_storage_account_name>"
    },
   "storageaccountkey": {
      "reference": {
        "keyVault": {
          "id": "<my_keyvault_id>"
        },
        "secretName": "<my_storage_account_key_secret_name>"
      }
    }
  }
}
```

## <a name="deploy-the-container-and-manage-files"></a>Nasaďte kontejner a správa souborů

Pomocí šablony definovaný můžete vytvořit kontejner a jeho svazek pomocí rozhraní příkazového řádku Azure připojit. Za předpokladu, že je název souboru šablony *azuredeploy.json* a s názvem souboru parametrů *azuredeploy.parameters.json*, pak na příkazovém řádku:

```azurecli-interactive
az group deployment create --name hellofilesdeployment --template-file azuredeploy.json --parameters @azuredeploy.parameters.json --resource-group myResourceGroup
```

Po spuštění kontejneru, můžete jednoduché webové aplikace nasazené prostřednictvím **seanmckenna/aci-hellofiles** bitovou kopii, ke správě souborů v Azure sdílené složky v cestě připojení, který jste zadali. Získejte IP adresu pro webové aplikace pomocí následující:

```azurecli-interactive
az container show --resource-group myResourceGroup --name hellofiles -o table
```

Můžete použít nástroje, jako [Microsoft Azure Storage Explorer](http://storageexplorer.com) k načtení a zkontrolujte soubor zapisovat do sdílené složky.

>[!NOTE]
> Další informace o používání šablon Azure Resource Manageru najdete v tématu soubory parametrů a nasazení pomocí rozhraní příkazového řádku Azure [nasazení prostředků pomocí šablony Resource Manageru a rozhraní příkazového řádku Azure](../azure-resource-manager/resource-group-template-deploy-cli.md).

## <a name="next-steps"></a>Další kroky

- Nasazení pro váš první kontejner pomocí instancí kontejneru Azure [rychlý start](container-instances-quickstart.md)
- Další informace o [vztah mezi instancemi kontejner Azure a orchestrators kontejneru](container-instances-orchestrator-relationship.md)
