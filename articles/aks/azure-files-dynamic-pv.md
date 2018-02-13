---
title: "Použít Azure soubor s AKS"
description: "Disky systému Azure pomocí AKS"
services: container-service
author: neilpeterson
manager: timlt
ms.service: container-service
ms.topic: article
ms.date: 1/04/2018
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: ce37cfdd70f95822a912f6ea71b9e4a3f9a30a14
ms.sourcegitcommit: b32d6948033e7f85e3362e13347a664c0aaa04c1
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/13/2018
---
# <a name="persistent-volumes-with-azure-files"></a>Trvalé svazky s soubory Azure

Trvalé svazku představuje část úložiště, které se zřizují pro použití v clusteru s podporou Kubernetes. Trvalé svazku může používat jednoho nebo mnoha pracovními stanicemi soustředěnými kolem a můžete dynamicky nebo staticky zřídit. Tento dokument podrobně popisuje dynamické zřizování sdílenou složku Azure jako trvalé svazek Kubernetes v clusteru služby AKS. 

Další informace o Kubernetes trvalé svazky, najdete v části [trvalé svazky Kubernetes][kubernetes-volumes].

## <a name="prerequisites"></a>Požadavky

Při zřizování dynamicky sdílenou složku Azure jako Kubernetes svazek, můžete tak dlouho, dokud je obsažena ve stejné skupině prostředků jako AKS cluster použít libovolný účet úložiště. V případě potřeby vytvořte účet úložiště ve stejné skupině prostředků jako AKS cluster. 

Chcete-li identifikovat odpovídající prostředek skupiny, použijte [seznam skupiny az] [ az-group-list] příkaz.

```azurecli-interactive
az group list --output table
```

Výstupu v následujícím příkladu zobrazuje skupiny prostředků že přidruženy AKS clusteru. Skupinu prostředků s názvem, jako *MC_myAKSCluster_myAKSCluster_eastus* obsahuje AKS prostředků clusteru a je, kde je potřeba vytvořit účet úložiště. 

```
Name                                 Location    Status
-----------------------------------  ----------  ---------
MC_myAKSCluster_myAKSCluster_eastus  eastus      Succeeded
myAKSCluster                         eastus      Succeeded
```

Jakmile byl nalezen skupině prostředků se vytvořit účet úložiště se [vytvořit účet úložiště az] [ az-storage-account-create] příkaz.

```azurecli-interactive
az storage account create --resource-group  MC_myAKSCluster_myAKSCluster_eastus --name mystorageaccount --location eastus --sku Standard_LRS
```

## <a name="create-storage-class"></a>Vytvoření třídy úložiště

Třídy úložiště se používá k definování konfigurace dynamicky vytvořený trvalé svazku. Položky, jako je například název účtu úložiště Azure, SKU a oblasti jsou definované v objektu třídy úložiště. Další informace o Kubernetes třídy úložiště najdete v tématu [třídy úložiště Kubernetes][kubernetes-storage-classes].

Následující příklad určuje, že všechny účtu úložiště SKU zadejte `Standard_LRS` v `eastus` oblast lze použít, pokud se požaduje úložiště. 

```yaml
kind: StorageClass
apiVersion: storage.k8s.io/v1
metadata:
  name: azurefile
provisioner: kubernetes.io/azure-file
parameters:
  skuName: Standard_LRS
```

Pro použití účtu konkrétní úložiště, `storageAccount` může být použit parametr.

```yaml
kind: StorageClass
apiVersion: storage.k8s.io/v1
metadata:
  name: azurefile
provisioner: kubernetes.io/azure-file
parameters:
  storageAccount: azure_storage_account_name
```

## <a name="create-persistent-volume-claim"></a>Vytvoření svazku trvalé deklarace identity

Deklaraci identity svazku trvalé používá objektu třídy úložiště pro dynamicky zajišťují část úložiště. Pokud používáte Azure Files, se vytvoří sdílenou složku Azure v účtu úložiště vybraný nebo zadaný v objektu třídy úložiště.

>  [!NOTE]
>   Ujistěte se, že byl účet úložiště vhodný předem vytvořené ve stejné skupině prostředků jako AKS prostředků clusteru. Tato skupina prostředků má název, jako je *MC_myAKSCluster_myAKSCluster_eastus*. Deklarace identity trvalé svazku se nepodaří zřízení Azure sdílená Pokud účet úložiště není k dispozici. 

Následující manifest je použít k vytvoření svazku trvalé deklarace `5GB` velikost `ReadWriteOnce` přístup. Další informace o PVC režimy přístupu najdete v tématu [režimy přístupu][access-modes].

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: azurefile
spec:
  accessModes:
    - ReadWriteOnce
  storageClassName: azurefile
  resources:
    requests:
      storage: 5Gi
```

## <a name="using-the-persistent-volume"></a>Použití trvalé svazku

Jakmile se vytvořila deklaraci identity trvalé svazku a úložiště úspěšném zřízení, pod lze vytvořit s přístupem ke svazku. Následující manifest vytvoří pod, která používá deklarace trvalé svazku `azurefile` připojit sdílenou složkou Azure v `/var/www/html` cesta. 

```yaml
kind: Pod
apiVersion: v1
metadata:
  name: mypod
spec:
  containers:
    - name: myfrontend
      image: nginx
      volumeMounts:
      - mountPath: "/var/www/html"
        name: volume
  volumes:
    - name: volume
      persistentVolumeClaim:
        claimName: azurefile
```

## <a name="mount-options"></a>Možnosti připojení

Výchozí hodnoty fileMode a dirMode liší mezi verzemi Kubernetes, jak je popsáno v následující tabulce. 

| verze | hodnota |
| ---- | ---- |
| v1.6.x, v1.7.x | 0777 |
| v1.8.0-v1.8.5 | 0700 |
| V1.8.6 nebo novější | 0755 |
| v1.9.0 | 0700 |
| V1.9.1 nebo novější | 0755 |

Pokud používáte cluster verze 1.8.5 nebo větší, přípojných možnosti lze zadat v objektu třídy úložiště. Následující příklad sady `0777`. 

```yaml
kind: StorageClass
apiVersion: storage.k8s.io/v1
metadata:
  name: azurefile
provisioner: kubernetes.io/azure-file
mountOptions:
  - dir_mode=0777
  - file_mode=0777
  - uid=1000
  - gid=1000
parameters:
  skuName: Standard_LRS
```

Pokud používáte cluster verze 1.8.0 - 1.8.4, lze určit kontext zabezpečení `runAsUser` nastavena na hodnotu `0`. Další informace o kontextu zabezpečení Pod najdete v tématu [konfigurace kontextu zabezpečení][kubernetes-security-context].

## <a name="next-steps"></a>Další postup

Další informace o Kubernetes trvalé svazky s využitím Azure Files.

> [!div class="nextstepaction"]
> [Modul plug-in Kubernetes pro Azure Files][kubernetes-files]

<!-- LINKS - external -->
[access-modes]: https://kubernetes.io/docs/concepts/storage/persistent-volumes/#access-modes
[kubectl-create]: https://kubernetes.io/docs/user-guide/kubectl/v1.8/#create
[kubectl-describe]: https://kubernetes-v1-4.github.io/docs/user-guide/kubectl/kubectl_describe/
[kubernetes-files]: https://github.com/kubernetes/examples/blob/master/staging/volumes/azure_file/README.md
[kubernetes-secret]: https://kubernetes.io/docs/concepts/configuration/secret/
[kubernetes-security-context]: https://kubernetes.io/docs/tasks/configure-pod-container/security-context/
[kubernetes-storage-classes]: https://kubernetes.io/docs/concepts/storage/storage-classes/
[kubernetes-volumes]: https://kubernetes.io/docs/concepts/storage/persistent-volumes/

<!-- LINKS - internal -->
[az-group-create]: /cli/azure/group#az_group_create
[az-group-list]: /cli/azure/group#az_group_list
[az-storage-account-create]: /cli/azure/storage/account#az_storage_account_create
[az-storage-create]: /cli/azure/storage/account#az_storage_account_create
[az-storage-key-list]: /cli/azure/storage/account/keys#az_storage_account_keys_list
[az-storage-share-create]: /cli/azure/storage/share#az_storage_share_create