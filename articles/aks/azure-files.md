---
title: "Použít Azure soubor s AKS | Microsoft Docs"
description: "Disky systému Azure pomocí AKS"
services: container-service
documentationcenter: 
author: neilpeterson
manager: timlt
editor: 
tags: aks, azure-container-service
keywords: 
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/11/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 11457e6556e6400d8f58f71c71ab1e790bcef8f1
ms.sourcegitcommit: e38120a5575ed35ebe7dccd4daf8d5673534626c
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/13/2017
---
# <a name="using-azure-files-with-kubernetes"></a>Soubory Azure pomocí Kubernetes

Aplikace typu kontejner na základě často potřebují přístup a uchovávat data v svazku externí data. Soubory Azure můžete použít jako tohle úložiště dat externí. Tento článek údaje pomocí Azure souborů jako svazek Kubernetes v Azure Container Service.

Další informace o svazcích Kubernetes najdete v tématu [Kubernetes svazky][kubernetes-volumes].

## <a name="creating-a-file-share"></a>Vytvoření sdílené složky

S Azure Container Service můžete použít existující sdílenou složku Azure File. Pokud potřebujete vytvořit, použijte následující sadu příkazů.

Vytvořte skupinu prostředků pro použití sdílené složky Azure File [vytvořit skupinu az] [ az-group-create] příkaz. Skupiny prostředků účtu úložiště a Kubernetes cluster se musí nacházet ve stejné oblasti.

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

Použití [vytvořit účet úložiště az] [ az-storage-create] příkaz pro vytvoření účtu úložiště Azure. Název účtu úložiště musí být jedinečný. Aktualizujte hodnotu `--name` argument jedinečnou hodnotu.

```azurecli-interactive
az storage account create --name mystorageaccount --resource-group myResourceGroup --sku Standard_LRS
```

Použití [seznam klíčů účtu úložiště az ] [ az-storage-key-list] příkaz vrátí klíč úložiště. Aktualizujte hodnotu `--account-name` argument s názvem účtu úložiště jedinečný.

Všimněte si z jedné z hodnot klíče, to se používá v postupných krocích.

```azurecli-interactive
az storage account keys list --account-name mystorageaccount --resource-group myResourceGroup --output table
```

Použití [vytvořit sdílenou složku úložiště az] [ az-storage-share-create] příkazu vytvořte sdílenou složku Azure File. Aktualizace `--account-key` hodnotu s hodnotou shromážděné v posledním kroku.

```azurecli-interactive
az storage share create --name myfileshare --account-name mystorageaccount --account-key <key>
```

## <a name="create-kubernetes-secret"></a>Vytvoření tajného klíče Kubernetes

Kubernetes potřebuje přihlašovací údaje pro přístup ke sdílené složce. Místo uložení název účtu úložiště Azure a klíč s každou pod, uloží se jednou [tajný klíč Kubernetes] [ kubernetes-secret] a odkazuje na každý svazek Azure Files. 

Hodnoty v manifestu tajný Kubernetes musí mít kódování base64. Použijte následující příkazy pro vrácení kódovaného hodnot.

Nejprve kódování názvu účtu úložiště. Nahraďte `storage-account` s názvem účtu úložiště Azure.

```azurecli-interactive
echo -n <storage-account> | base64
```

Dále je potřeba přístupový klíč účtu úložiště. Spusťte následující příkaz, který vrátí kódovaného klíč. Nahraďte `storage-key` klíčem shromážděny v jednom z předchozích kroků

```azurecli-interactive
echo -n <storage-key> | base64
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

Použití [kubectl použít] [ kubectl-apply] příkaz pro vytvoření tajný klíč.

```azurecli-interactive
kubectl apply -f azure-secret.yml
```

## <a name="mount-file-share-as-volume"></a>Připojení sdílené složky jako svazek

Do sdílené složky souborů Azure můžete připojit do vaší pod nakonfigurováním svazek v jeho specifikace. Vytvořte nový soubor s názvem `azure-files-pod.yml` s tímto obsahem. Aktualizace `share-name` s názvem uvedeným k Azure Files sdílet.

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
      shareName: <share-name>
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
[kubectl-apply]: https://kubernetes.io/docs/user-guide/kubectl/v1.8/#apply
[kubernetes-secret]: https://kubernetes.io/docs/concepts/configuration/secret/
[az-group-create]: /cli/azure/group#az_group_create