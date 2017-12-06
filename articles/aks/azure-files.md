---
title: "Použít Azure soubor s AKS"
description: "Disky systému Azure pomocí AKS"
services: container-service
author: neilpeterson
manager: timlt
ms.service: container-service
ms.topic: article
ms.date: 11/17/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 12c5d4985260c734ba813ace3143433883966712
ms.sourcegitcommit: 5d3e99478a5f26e92d1e7f3cec6b0ff5fbd7cedf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/06/2017
---
# <a name="using-azure-files-with-kubernetes"></a>Soubory Azure pomocí Kubernetes

Aplikace založené na kontejneru často potřebují přístup a uchovávat data v svazku externí data. Soubory Azure můžete použít jako tohle úložiště dat externí. Tento článek údaje pomocí Azure souborů jako svazek Kubernetes v Azure Container Service.

Další informace o svazcích Kubernetes najdete v tématu [Kubernetes svazky][kubernetes-volumes].

## <a name="create-an-azure-file-share"></a>Vytvořte sdílenou složku Azure

Před použitím sdílení souborů Azure jako svazek Kubernetes, je nutné vytvořit účet úložiště Azure a sdílené složky. K dokončení těchto úloh můžete použít následující skript. Všimněte si nebo aktualizujte hodnoty parametru, některé z nich je třeba při vytváření svazku Kubernetes.

```azurecli-interactive
# Change these four parameters
AKS_PERS_STORAGE_ACCOUNT_NAME=mystorageaccount$RANDOM
AKS_PERS_RESOURCE_GROUP=myAKSShare
AKS_PERS_LOCATION=eastus
AKS_PERS_SHARE_NAME=aksshare

# Create the Resource Group
az group create --name $AKS_PERS_RESOURCE_GROUP --location $AKS_PERS_LOCATION

# Create the storage account
az storage account create -n $AKS_PERS_STORAGE_ACCOUNT_NAME -g $AKS_PERS_RESOURCE_GROUP -l $AKS_PERS_LOCATION --sku Standard_LRS

# Export the connection string as an environment variable, this is used when creating the Azure file share
export AZURE_STORAGE_CONNECTION_STRING=`az storage account show-connection-string -n $AKS_PERS_STORAGE_ACCOUNT_NAME -g $AKS_PERS_RESOURCE_GROUP -o tsv`

# Create the file share
az storage share create -n $AKS_PERS_SHARE_NAME

# Get storage account key
STORAGE_KEY=$(az storage account keys list --resource-group $AKS_PERS_RESOURCE_GROUP --account-name $AKS_PERS_STORAGE_ACCOUNT_NAME --query "[0].value" -o tsv)
```

## <a name="create-kubernetes-secret"></a>Vytvoření tajného klíče Kubernetes

Kubernetes potřebuje přihlašovací údaje pro přístup ke sdílené složce. Tyto přihlašovací údaje jsou uložené v [tajný klíč Kubernetes][kubernetes-secret], který se odkazuje při vytváření Kubernetes pod.

Při vytváření Kubernetes tajný, tajný hodnoty musí být kódováním base64.

Nejprve kódování názvu účtu úložiště. V případě potřeby nahradit `$AKS_PERS_STORAGE_ACCOUNT_NAME` s názvem účtu úložiště Azure.

```azurecli-interactive
echo -n $AKS_PERS_STORAGE_ACCOUNT_NAME | base64
```

V dalším kroku kódování klíč účtu úložiště. V případě potřeby nahradit `$STORAGE_KEY` s názvem klíč účtu úložiště Azure.

```azurecli-interactive
echo -n $STORAGE_KEY | base64
```

Vytvořte soubor s názvem `azure-secret.yml` a zkopírujte následující YAML. Aktualizace `azurestorageaccountname` a `azurestorageaccountkey` kódováním base64, pomocí hodnoty hodnoty získané v předchozím kroku.

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: azure-secret
type: Opaque
data:
  azurestorageaccountname: <base64_encoded_storage_account_name>
  azurestorageaccountkey: <base64_encoded_storage_account_key>
```

Použití [kubectl vytvořit] [ kubectl-create] příkaz pro vytvoření tajný klíč.

```azurecli-interactive
kubectl create -f azure-secret.yml
```

## <a name="mount-file-share-as-volume"></a>Připojení sdílené složky jako svazek

Do sdílené složky souborů Azure můžete připojit do vaší pod nakonfigurováním svazek v jeho specifikace. Vytvořte nový soubor s názvem `azure-files-pod.yml` s tímto obsahem. Aktualizace `aksshare` s názvem uvedeným k Azure Files sdílet.

```yaml
apiVersion: v1
kind: Pod
metadata:
 name: azure-files-pod
spec:
 containers:
  - image: kubernetes/pause
    name: azure
    volumeMounts:
      - name: azure
        mountPath: /mnt/azure
 volumes:
  - name: azure
    azureFile:
      secretName: azure-secret
      shareName: aksshare
      readOnly: false
```

K vytvoření pod použijte kubectl.

```azurecli-interactive
kubectl apply -f azure-files-pod.yml
```

Nyní máte kontejner spuštěné s Azure sdílené složky připojené `/mnt/azure` adresáře. Zobrazí připojení při kontrole vaší pod prostřednictvím svazku `kubectl describe pod azure-files-pod`.

## <a name="next-steps"></a>Další kroky

Další informace o Kubernetes svazky s využitím Azure Files.

> [!div class="nextstepaction"]
> [Modul plug-in Kubernetes pro Azure Files](https://github.com/kubernetes/examples/blob/master/staging/volumes/azure_file/README.md)

<!-- LINKS -->
[kubernetes-volumes]: https://kubernetes.io/docs/concepts/storage/volumes/
[az-storage-create]: /cli/azure/storage/account#az_storage_account_create
[az-storage-key-list]: /cli/azure/storage/account/keys#az_storage_account_keys_list
[az-storage-share-create]: /cli/azure/storage/share#az_storage_share_create
[kubectl-create]: https://kubernetes.io/docs/user-guide/kubectl/v1.8/#create
[kubernetes-secret]: https://kubernetes.io/docs/concepts/configuration/secret/
[az-group-create]: /cli/azure/group#az_group_create