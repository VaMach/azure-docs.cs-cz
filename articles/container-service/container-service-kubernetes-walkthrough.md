---
title: "Rychlý start – Azure Kubernetes cluster pro Linux | Dokumentace Microsoftu"
description: "Rychle se naučíte, jak pomocí rozhraní příkazového řádku Azure vytvářet cluster Kubernetes pro linuxové kontejnery ve službě Azure Container Service."
services: container-service
documentationcenter: 
author: neilpeterson
manager: timlt
editor: 
tags: acs, azure-container-service, kubernetes
keywords: 
ms.assetid: 8da267e8-2aeb-4c24-9a7a-65bdca3a82d6
ms.service: container-service
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/18/2017
ms.author: nepeters
ms.custom: H1Hack27Feb2017
ms.translationtype: HT
ms.sourcegitcommit: 26c07d30f9166e0e52cb396cdd0576530939e442
ms.openlocfilehash: 3be2079d205d6bfd4c796e5f6abcd7ac5fe595a2
ms.contentlocale: cs-cz
ms.lasthandoff: 07/19/2017

---

# <a name="deploy-kubernetes-cluster-for-linux-containers"></a>Nasazení clusteru Kubernetes pro linuxové kontejnery

Azure CLI slouží k vytváření a správě prostředků Azure z příkazového řádku nebo ve skriptech. Tato příručka podrobně popisuje použití rozhraní příkazového řádku Azure k nasazení clusteru [Kubernetes](https://kubernetes.io/docs/home/) ve službě [Azure Container Service](container-service-intro.md). Po nasazení clusteru se k němu připojíte pomocí nástroje pro příkazový řádek Kubernetes `kubectl` a nasadíte svůj první linuxový kontejner.

Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) před tím, než začnete.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Pokud se rozhodnete nainstalovat a používat rozhraní příkazového řádku místně, musíte mít rozhraní příkazového řádku Azure ve verzi 2.0.4 nebo novější. Verzi zjistíte spuštěním příkazu `az --version`. Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI 2.0]( /cli/azure/install-azure-cli). 

## <a name="create-a-resource-group"></a>Vytvoření skupiny prostředků

Vytvořte skupinu prostředků pomocí příkazu [az group create](/cli/azure/group#create). Skupina prostředků Azure je logická skupina, ve které se nasazují a spravují prostředky Azure. 

Následující příklad vytvoří skupinu prostředků *myResourceGroup* v umístění *eastus*.

```azurecli-interactive 
az group create --name myResourceGroup --location eastus
```

## <a name="create-kubernetes-cluster"></a>Vytvoření clusteru Kubernetes
Vytvořte cluster Kubernetes ve službě Azure Container Service pomocí příkazu [az acs create](/cli/azure/acs#create). 

Následující příklad vytvoří cluster s názvem *myK8sCluster* s jedním hlavním linuxovým uzlem a dvěma agentskými linuxovými uzly. Tento příklad vytvoří klíče SSH (pokud už ve výchozích umístěních neexistují). Chcete-li použít konkrétní sadu klíčů, použijte možnost `--ssh-key-value`. Aktualizujte název clusteru, aby byl vhodný pro vaše prostředí. 



```azurecli-interactive 
az acs create --orchestrator-type=kubernetes \
    --resource-group myResourceGroup \
    --name=myK8sCluster \
    --generate-ssh-keys 
```

Po několika minutách se příkaz dokončí a zobrazí vám informace o nasazení.

## <a name="install-kubectl"></a>Instalace kubectl

Pokud se chcete připojit ke clusteru Kubernetes z klientského počítače, použijte klienta příkazového řádku Kubernetes [`kubectl`](https://kubernetes.io/docs/user-guide/kubectl/). 

Pokud používáte Azure CloudShell, `kubectl` je už nainstalován. Pokud ho chcete nainstalovat místně, můžete použít příkaz [az acs kubernetes install-cli](/cli/azure/acs/kubernetes#install-cli).

Následující příklad rozhraní příkazového řádku Azure nainstaluje `kubectl` do vašeho systému. Pokud používáte rozhraní příkazového řádku Azure na počítači se systémem macOS nebo Linux, budete možná muset příkaz spustit s příkazem `sudo`.

```azurecli-interactive 
az acs kubernetes install-cli 
```

## <a name="connect-with-kubectl"></a>Připojení přes kubectl

Abyste nakonfigurovali `kubectl` pro připojení ke svému clusteru Kubernetes, spusťte příkaz [az acs kubernetes get-credentials](/cli/azure/acs/kubernetes#get-credentials). Následující příklad stáhne konfiguraci clusteru pro cluster Kubernetes.

```azurecli-interactive 
az acs kubernetes get-credentials --resource-group=myResourceGroup --name=myK8sCluster
```

Abyste ověřili připojení ke clusteru ze svého počítače, zkuste spustit:

```azurecli-interactive
kubectl get nodes
```

`kubectl` zobrazuje seznam hlavních a agentských uzlů.

```azurecli-interactive
NAME                    STATUS                     AGE       VERSION
k8s-agent-98dc3136-0    Ready                      5m        v1.5.3
k8s-agent-98dc3136-1    Ready                      5m        v1.5.3
k8s-master-98dc3136-0   Ready,SchedulingDisabled   5m        v1.5.3

```


## <a name="deploy-an-nginx-container"></a>Nasazení kontejneru NGINX

Kontejner Dockeru můžete spustit v *podu* Kubernetes, který obsahuje jeden nebo více kontejnerů. 

Následující příkaz spustí kontejner Dockeru NGINX v podu Kubernetes na jednom z uzlů. V tomto případě kontejner spustí webový server NGINX získaný z image v [Docker Hubu](https://hub.docker.com/_/nginx/).

```azurecli-interactive
kubectl run nginx --image nginx
```
Pokud si chcete ověřit spuštění kontejneru, použijte příkaz:

```azurecli-interactive
kubectl get pods
```

## <a name="view-the-nginx-welcome-page"></a>Zobrazení úvodní stránky serveru NGINX
Pokud chcete zpřístupnit server NGINX celému světu prostřednictvím veřejné IP adresy, zadejte následující příkaz:

```azurecli-interactive
kubectl expose deployments nginx --port=80 --type=LoadBalancer
```

Tento příkaz způsobí, že Kubernetes vytvoří službu a [pravidlo nástroje pro vyrovnávání zatížení Azure](container-service-kubernetes-load-balancing.md) s veřejnou IP adresou pro tuto službu. 

Spuštěním následujícího příkazu zobrazte stav služby.

```azurecli-interactive
kubectl get svc
```

Zpočátku se IP adresa bude zobrazovat jako `pending` (čekající). Po několika minutách se externí IP adresa služby nastaví takto:
  
```azurecli-interactive
NAME         CLUSTER-IP     EXTERNAL-IP     PORT(S)        AGE       
kubernetes   10.0.0.1       <none>          443/TCP        21h       
nginx        10.0.111.25    52.179.3.96     80/TCP         22m
```

Pokud si chcete zobrazit úvodní stránku NGINX na externí IP adrese, můžete použít libovolný webový prohlížeč:

![Obrázek přechodu na Nginx](media/container-service-kubernetes-walkthrough/kubernetes-nginx4.png)  


## <a name="delete-cluster"></a>Odstranění clusteru
Pokud už cluster nepotřebujete, můžete k odebrání skupiny prostředků, služby kontejneru a všech souvisejících prostředků použít příkaz [az group delete](/cli/azure/group#delete).

```azurecli-interactive 
az group delete --name myResourceGroup
```


## <a name="next-steps"></a>Další kroky

V tomto rychlém úvodním kurzu jste nasadili cluster Kubernetes, připojili se přes `kubectl` a nasadili pod s kontejnerem NGINX. Další informace o Azure Container Service získáte v kurzu o clusteru Kubernetes.

> [!div class="nextstepaction"]
> [Správa clusteru ACS Kubernetes](./container-service-tutorial-kubernetes-prepare-app.md)

