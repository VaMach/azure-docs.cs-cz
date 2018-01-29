---
title: "Azure Disk pomocí AKS"
description: "Disky systému Azure pomocí AKS"
services: container-service
author: neilpeterson
manager: timlt
ms.service: container-service
ms.topic: article
ms.date: 1/25/2018
ms.author: nepeters
ms.openlocfilehash: e1f5b68d5d39dd846ebec525d1e83a6c0ef4971a
ms.sourcegitcommit: 99d29d0aa8ec15ec96b3b057629d00c70d30cfec
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/25/2018
---
# <a name="persistent-volumes-with-azure-disks---dynamic-provisioning"></a>Trvalé svazky s Azure disky - dynamické zajišťování

Trvalé svazku představuje část úložiště, které se zřizují pro použití v clusteru s podporou Kubernetes. Trvalé svazku může používat jednoho nebo mnoha pracovními stanicemi soustředěnými kolem a můžete dynamicky nebo staticky zřídit. Tento dokument podrobně popisuje dynamické zřizování Azure disk jako trvalé svazek Kubernetes v clusteru služby AKS. 

Další informace o Kubernetes trvalé svazky, najdete v části [trvalé svazky Kubernetes][kubernetes-volumes].

## <a name="built-in-storage-classes"></a>Součástí třídy úložiště

Třídy úložiště se používá k definování konfigurace dynamicky vytvořený trvalé svazku. Další informace o Kubernetes třídy úložiště najdete v tématu [třídy úložiště Kubernetes][kubernetes-storage-classes].

Každý cluster AKS zahrnuje dvě třídy předem vytvořené úložiště, jak nakonfigurováno pro práci s disky systému Azure. Použití `kubectl get storageclass` příkazu zobrazte tyto.

```console
NAME                PROVISIONER                AGE
default (default)   kubernetes.io/azure-disk   1h
managed-premium     kubernetes.io/azure-disk   1h
```

Pokud tyto třídy úložiště fungovat pro vaše potřeby, není potřeba vytvořit novou.

## <a name="create-storage-class"></a>Vytvoření třídy úložiště

Pokud chcete vytvořit novou třídu úložiště nakonfigurovaný pro disky systému Azure, použijte následující ukázka manifestu. 

`storageaccounttype` Hodnotu `Standard_LRS` vyplývá, že je standardní disku. Tuto hodnotu můžete změnit na `Premium_LRS` k vytvoření [premium disku][premium-storage]. Pokud chcete použít prémiové disky, musí mít AKS uzlu virtuální počítač s velikostí, které jsou kompatibilní s prémiové disky. V tématu [tento dokument] [ premium-storage] seznam kompatibilní velikosti.

```yaml
apiVersion: storage.k8s.io/v1beta1
kind: StorageClass
metadata:
  name: azure-managed-disk
provisioner: kubernetes.io/azure-disk
parameters:
  kind: Managed
  storageaccounttype: Standard_LRS
```

## <a name="create-persistent-volume-claim"></a>Vytvoření svazku trvalé deklarace identity

Deklarace identity trvalé svazek používá objekt třídy úložiště dynamicky zřídit část úložiště. Pokud používáte Azure disk, disk se vytvoří ve stejné skupině prostředků jako AKS prostředky.

Manifest tento příklad vytvoří deklarace identity trvalé svazku pomocí `azure-managed-disk` třídy úložiště pro vytvoření disku `5GB` velikost `ReadWriteOnce` přístup. Další informace o PVC režimy přístupu najdete v tématu [režimy přístupu][access-modes].

> [!NOTE]
> Azure disk může být připojen pouze s typem režim přístupu ReadWriteOnce, že bude k dispozici pouze do jednoho uzlu AKS. Pokud museli sdílet mezi několika uzly trvalé svazku, zvažte použití [Azure Files][azure-files-pvc]. 

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: azure-managed-disk
  annotations:
    volume.beta.kubernetes.io/storage-class: azure-managed-disk
spec:
  accessModes:
  - ReadWriteOnce
  resources:
    requests:
      storage: 5Gi
```

## <a name="using-the-persistent-volume"></a>Použití trvalé svazku

Jakmile vytvořila deklaraci identity trvalé svazku a disku úspěšném zřízení, pod lze vytvořit s přístupem na disk. Následující manifest vytvoří pod, která používá deklarace trvalé svazku `azure-managed-disk` připojit disk v Azure `/var/www/html` cesta. 

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
        claimName: azure-managed-disk
```

## <a name="next-steps"></a>Další postup

Další informace o Kubernetes trvalé svazky s využitím disky systému Azure.

> [!div class="nextstepaction"]
> [Modul plug-in Kubernetes pro disky systému Azure][kubernetes-disk]

<!-- LINKS - external -->
[access-modes]: https://kubernetes.io/docs/concepts/storage/persistent-volumes/#access-modes
[kubernetes-disk]: https://kubernetes.io/docs/concepts/storage/storage-classes/#new-azure-disk-storage-class-starting-from-v172
[kubernetes-storage-classes]: https://kubernetes.io/docs/concepts/storage/storage-classes/
[kubernetes-volumes]: https://kubernetes.io/docs/concepts/storage/persistent-volumes/

<!-- LINKS - internal -->
[azure-files-pvc]: azure-files-dynamic-pv.md
[premium-storage]: ../virtual-machines/windows/premium-storage.md