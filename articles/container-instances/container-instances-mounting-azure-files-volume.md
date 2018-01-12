---
title: "Připojit Azure Files svazek v Azure kontejner instancí"
description: "Zjistěte, jak připojit Azure Files svazek k uchování stavu s instancemi Azure kontejneru"
services: container-instances
author: seanmck
manager: timlt
ms.service: container-instances
ms.topic: article
ms.date: 01/02/2018
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: be502e6aef39ee4ed8cfc1f8926cb556dc1defb1
ms.sourcegitcommit: 9292e15fc80cc9df3e62731bafdcb0bb98c256e1
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/10/2018
---
# <a name="mount-an-azure-file-share-with-azure-container-instances"></a>Připojit sdílenou složku Azure s instancemi Azure kontejneru

Ve výchozím nastavení jsou bezstavové instancí kontejnerů Azure. Pokud kontejner dojde k chybě nebo zastaví, všechny její stav bude ztracena. K zachování stavu nad rámec životnost kontejneru, je nutné připojit svazek z externího úložiště. Tento článek ukazuje, jak připojit sdílenou složku Azure pro použití s instancemi Azure kontejneru.

> [!NOTE]
> Připojení sdílenou složku Azure je aktuálně omezeno na kontejnery Linux. Při pracujeme, aby všechny funkce Windows kontejnery, můžete najít aktuální platformy rozdíly v [kvóty a dostupnost v oblastech Azure kontejner instancí](container-instances-quotas.md).

## <a name="create-an-azure-file-share"></a>Vytvořte sdílenou složku Azure

Před použitím sdílenou složku Azure s Azure kontejner instancí, je třeba ji vytvořit. Spusťte následující skript pro vytvoření účtu úložiště pro hostování sdílené složky a sdílené složky sám sebe. Název účtu úložiště musí být globálně jedinečné, takže skript přidá do základní řetězec náhodná hodnota.

```azurecli-interactive
# Change these four parameters as needed
ACI_PERS_RESOURCE_GROUP=myResourceGroup
ACI_PERS_STORAGE_ACCOUNT_NAME=mystorageaccount$RANDOM
ACI_PERS_LOCATION=eastus
ACI_PERS_SHARE_NAME=acishare

# Create the storage account with the parameters
az storage account create \
    --resource-group $ACI_PERS_RESOURCE_GROUP \
    --name $ACI_PERS_STORAGE_ACCOUNT_NAME \
    --location $ACI_PERS_LOCATION \
    --sku Standard_LRS

# Export the connection string as an environment variable. The following 'az storage share create' command
# references this environment variable when creating the Azure file share.
export AZURE_STORAGE_CONNECTION_STRING=`az storage account show-connection-string --resource-group $ACI_PERS_RESOURCE_GROUP --name $ACI_PERS_STORAGE_ACCOUNT_NAME --output tsv`

# Create the file share
az storage share create -n $ACI_PERS_SHARE_NAME
```

## <a name="get-storage-credentials"></a>Získání přihlašovacích údajů úložiště

Chcete-li jako svazek v instancí kontejnerů Azure připojit sdílenou složku Azure, je třeba tří hodnot: název účtu úložiště, název sdílené složky a přístupový klíč úložiště.

Pokud jste použili výše uvedený skript, název účtu úložiště byl vytvořen s hodnotou náhodných na konci. Chcete-li prohledávat posledním řetězci (včetně náhodných část), použijte následující příkazy:

```azurecli-interactive
STORAGE_ACCOUNT=$(az storage account list --resource-group $ACI_PERS_RESOURCE_GROUP --query "[?contains(name,'$ACI_PERS_STORAGE_ACCOUNT_NAME')].[name]" --output tsv)
echo $STORAGE_ACCOUNT
```

Název sdílené složky je již znám (definován jako *acishare* ve skriptu výše), takže všechny, že zůstanou je klíč účtu úložiště, které lze najít pomocí následujícího příkazu:

```azurecli-interactive
STORAGE_KEY=$(az storage account keys list --resource-group $ACI_PERS_RESOURCE_GROUP --account-name $STORAGE_ACCOUNT --query "[0].value" --output tsv)
echo $STORAGE_KEY
```

## <a name="deploy-container-and-mount-volume"></a>Nasazení kontejneru a připojení svazku

Chcete-li připojit sdílenou složku Azure jako svazek v kontejneru, zadejte do sdílené složky a svazek přípojný bod, když vytvoříte kontejner s [vytvořit kontejner az][az-container-create]. Pokud jste postupovali podle předchozích kroků, můžete připojit sdílenou složku, kterou jste dříve vytvořili pomocí následujícího příkazu k vytvoření kontejneru:

```azurecli-interactive
az container create \
    --resource-group $ACI_PERS_RESOURCE_GROUP \
    --name hellofiles \
    --image seanmckenna/aci-hellofiles \
    --ip-address Public \
    --ports 80 \
    --azure-file-volume-account-name $ACI_PERS_STORAGE_ACCOUNT_NAME \
    --azure-file-volume-account-key $STORAGE_KEY \
    --azure-file-volume-share-name $ACI_PERS_SHARE_NAME \
    --azure-file-volume-mount-path /aci/logs/
```

## <a name="manage-files-in-mounted-volume"></a>Správa souborů v připojeného svazku

Po spuštění kontejneru, můžete jednoduché webové aplikace nasazené prostřednictvím [seanmckenna/aci-hellofiles] [ aci-hellofiles] image ke správě souborů v Azure sdílené složky v cestě připojení jste zadali. Získat IP adresu pro webovou aplikaci pomocí [az kontejneru zobrazit] [ az-container-show] příkaz:

```azurecli-interactive
az container show --resource-group $ACI_PERS_RESOURCE_GROUP --name hellofiles --output table
```

Můžete použít [portál Azure] [ portal] nebo nástroje, jako [Microsoft Azure Storage Explorer] [ storage-explorer] k načtení a zkontrolujte soubor zapsána do sdílené složky.

## <a name="mount-multiple-volumes"></a>Připojení více svazků

Připojení více svazků v instanci kontejneru, je nutné nasadit pomocí [šablony Azure Resource Manageru](/azure/templates/microsoft.containerinstance/containergroups).

Nejdřív zadejte podrobnosti sdílené složky a definování svazky naplněním `volumes` pole v `properties` část šablony. Například pokud jste vytvořili dvou sdílených složek soubory Azure s názvem *sdílená_složka1* a *share2* v účtu úložiště *Můj_účet_úložiště*, `volumes` se zobrazí pole podobný následujícímu:

```json
"volumes": [{
  "name": "myvolume1",
  "azureFile": {
    "shareName": "share1",
    "storageAccountName": "myStorageAccount",
    "storageAccountKey": "<storage-account-key>"
  }
},
{
  "name": "myvolume2",
  "azureFile": {
    "shareName": "share2",
    "storageAccountName": "myStorageAccount",
    "storageAccountKey": "<storage-account-key>"
  }
}]
```

V dalším kroku pro každý kontejner ve skupině kontejner, ve kterém byste chtěli připojit svazky naplnit `volumeMounts` pole v `properties` části definici kontejneru. Například to připojí dva svazky *myvolume1* a *myvolume2*, dříve definovaném:

```json
"volumeMounts": [{
  "name": "myvolume1",
  "mountPath": "/mnt/share1/"
},
{
  "name": "myvolume2",
  "mountPath": "/mnt/share2/"
}]
```

Příklad nasazení kontejneru instance pomocí šablony Azure Resource Manager, najdete v sekci [nasazení skupiny více kontejnerů v Azure kontejner instancí](container-instances-multi-container-group.md).

## <a name="next-steps"></a>Další postup

Další informace o vztah mezi [Azure instancí kontejnerů a kontejner orchestrators](container-instances-orchestrator-relationship.md).

<!-- LINKS - External -->
[aci-hellofiles]: https://hub.docker.com/r/seanmckenna/aci-hellofiles/
[portal]: https://portal.azure.com
[storage-explorer]: https://storageexplorer.com

<!-- LINKS - Internal -->
[az-container-create]: /cli/azure/container#az_container_create
[az-container-show]: /cli/azure/container#az_container_show