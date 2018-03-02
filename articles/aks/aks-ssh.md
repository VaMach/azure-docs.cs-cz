---
title: "SSH do uzlů clusteru Azure Container Service (AKS)"
description: "Vytvoření připojení SSH pomocí clusteru služby Azure Container Service (AKS) uzly"
services: container-service
author: neilpeterson
manager: timlt
ms.service: container-service
ms.topic: article
ms.date: 2/28/2018
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 00affc3d1c02c477826261aeac6e092934037e81
ms.sourcegitcommit: 83ea7c4e12fc47b83978a1e9391f8bb808b41f97
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/28/2018
---
# <a name="ssh-into-azure-container-service-aks-cluster-nodes"></a>SSH do uzlů clusteru Azure Container Service (AKS)

V některých případech může potřebujete přístup k uzlu Azure Container Service (AKS) pro údržby, shromáždění protokolů nebo jiné řešení potíží operace. Uzlů Azure Container Service (AKS) nejsou vystaveny v Internetu. Vytvoření připojení SSH pomocí do uzlu AKS použijte kroky popsané v tomto dokumentu.

## <a name="configure-ssh-access"></a>Konfigurace SSH přístupu

 SSH na konkrétním uzlu, pod je vytvořena `hostNetwork` přístup. Služby se vytvoří také pod přístup. Tato konfigurace je privilegovaný a měla by být odebrána po použití.

Vytvořte soubor s názvem `aks-ssh.yaml` a zkopírujte tento manifestu. Aktualizujte název uzlu s názvem AKS cílový uzel.

```yaml
apiVersion: v1
kind: Service
metadata:
  name: aks-ssh
spec:
  selector:
    app: aks-ssh
  type: LoadBalancer
  ports:
  - protocol: TCP
    port: 22
    targetPort: 22
---
apiVersion: extensions/v1beta1
kind: Deployment
metadata:
  name: aks-ssh
  labels:
    app: aks-ssh
spec:
  replicas: 1
  selector:
    matchLabels:
      app: aks-ssh
  template:
    metadata:
      labels:
        app: aks-ssh
    spec:
      containers:
      - name: alpine
        image: alpine:latest
        ports:
        - containerPort: 22
        command: ["/bin/sh", "-c", "--"]
        args: ["while true; do sleep 30; done;"]
      hostNetwork: true
      nodeName: aks-nodepool1-42032720-0
```

Spusťte manifest vytvoření pod a služby.

```azurecli-interactive
$ kubectl apply -f aks-ssh.yaml
```

Získáte externí IP adresu zveřejněné služby. Může trvat několik minut pro konfiguraci IP adres k dokončení. 

```azurecli-interactive
$ kubectl get service

NAME               TYPE           CLUSTER-IP    EXTERNAL-IP     PORT(S)        AGE
kubernetes         ClusterIP      10.0.0.1      <none>          443/TCP        1d
aks-ssh            LoadBalancer   10.0.51.173   13.92.154.191   22:31898/TCP   17m
```

Vytvořte ssh připojení. 

Je výchozí uživatelské jméno pro cluster služby AKS `azureuser`. Pokud tento účet byl změněn v okamžiku vytvoření clusteru, nahraďte správce správné uživatelské jméno. 

Pokud váš klíč není v `~/ssh/id_rsa`, zadejte správné umístění, pomocí `ssh -i` argument.

```azurecli-interactive
$ ssh azureuser@13.92.154.191

Welcome to Ubuntu 16.04.3 LTS (GNU/Linux 4.11.0-1016-azure x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  Get cloud support with Ubuntu Advantage Cloud Guest:
    http://www.ubuntu.com/business/services/cloud

48 packages can be updated.
0 updates are security updates.


*** System restart required ***
Last login: Tue Feb 27 20:10:38 2018 from 167.220.99.8
To run a command as administrator (user "root"), use "sudo <command>".
See "man sudo_root" for details.

azureuser@aks-nodepool1-42032720-0:~$
```

## <a name="remove-ssh-access"></a>Odebrat přístup SSH

Až budete hotoví, odstraňte pod přístup SSH a služby.

```azurecli-interactive
kubectl delete -f aks-ssh.yaml
```