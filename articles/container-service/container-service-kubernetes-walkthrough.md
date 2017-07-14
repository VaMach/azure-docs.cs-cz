---
title: "Rychlý start – Azure Kubernetes cluster pro Linux | Dokumentace Microsoftu"
description: "Rychle se naučíte, jak pomocí rozhraní příkazového řádku Azure vytvářet cluster Kubernetes pro linuxové kontejnery ve službě Azure Container Service."
services: container-service
documentationcenter: 
author: anhowe
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
ms.date: 05/31/2017
ms.author: anhowe
ms.custom: H1Hack27Feb2017
ms.translationtype: Human Translation
ms.sourcegitcommit: 7948c99b7b60d77a927743c7869d74147634ddbf
ms.openlocfilehash: 25043f6bf5e5ab3def8563bd2c096b79706bfec1
ms.contentlocale: cs-cz
ms.lasthandoff: 06/20/2017

---

# Nasazení clusteru Kubernetes pro linuxové kontejnery
<a id="deploy-kubernetes-cluster-for-linux-containers" class="xliff"></a>

Azure CLI slouží k vytváření a správě prostředků Azure z příkazového řádku nebo ve skriptech. Tato příručka podrobně popisuje použití rozhraní příkazového řádku Azure k nasazení clusteru [Kubernetes](https://kubernetes.io/docs/home/) ve službě [Azure Container Service](container-service-intro.md). Po nasazení clusteru se k němu připojíte pomocí nástroje pro příkazový řádek Kubernetes `kubectl` a nasadíte svůj první linuxový kontejner.

Tento kurz vyžaduje Azure CLI verze 2.0.4 nebo novější. Verzi zjistíte spuštěním příkazu `az --version`. Pokud potřebujete upgrade, přečtěte si téma [Instalace Azure CLI 2.0]( /cli/azure/install-azure-cli). 

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/) před tím, než začnete.

## Přihlaste se k Azure.
<a id="log-in-to-azure" class="xliff"></a> 

Přihlaste se k předplatnému Azure pomocí příkazu [az login](/cli/azure/#login) a postupujte podle pokynů na obrazovce.

```azurecli-interactive
az login
```

## Vytvoření skupiny prostředků
<a id="create-a-resource-group" class="xliff"></a>

Vytvořte skupinu prostředků pomocí příkazu [az group create](/cli/azure/group#create). Skupina prostředků Azure je logická skupina, ve které se nasazují a spravují prostředky Azure. 

Následující příklad vytvoří skupinu prostředků *myResourceGroup* v umístění *eastus*.

```azurecli-interactive 
az group create --name myResourceGroup --location eastus
```

## Vytvoření clusteru Kubernetes
<a id="create-kubernetes-cluster" class="xliff"></a>
Vytvořte cluster Kubernetes ve službě Azure Container Service pomocí příkazu [az acs create](/cli/azure/acs#create). 

Následující příklad vytvoří cluster s názvem *myK8sCluster* s jedním hlavním linuxovým uzlem a dvěma agentskými linuxovými uzly. Tento příklad vytvoří klíče SSH (pokud už ve výchozích umístěních neexistují). Chcete-li použít konkrétní sadu klíčů, použijte možnost `--ssh-key-value`. Aktualizujte název clusteru, aby byl vhodný pro vaše prostředí. 



```azurecli-interactive 
az acs create --orchestrator-type=kubernetes \
    --resource-group myResourceGroup \
    --name=myK8sCluster \
    --generate-ssh-keys 
```

Po několika minutách se příkaz dokončí a zobrazí vám informace o nasazení.

## Instalace kubectl
<a id="install-kubectl" class="xliff"></a>

Pokud se chcete připojit ke clusteru Kubernetes z klientského počítače, použijte klienta příkazového řádku Kubernetes [`kubectl`](https://kubernetes.io/docs/user-guide/kubectl/). 

Pokud používáte Azure CloudShell, `kubectl` je už nainstalován. Pokud ho chcete nainstalovat místně, můžete použít příkaz [az acs kubernetes install-cli](/cli/azure/acs/kubernetes#install-cli).

Následující příklad rozhraní příkazového řádku Azure nainstaluje `kubectl` do vašeho systému. Pokud používáte rozhraní příkazového řádku Azure na počítači se systémem macOS nebo Linux, budete možná muset příkaz spustit s příkazem `sudo`.

```azurecli-interactive 
az acs kubernetes install-cli 
```

## Připojení přes kubectl
<a id="connect-with-kubectl" class="xliff"></a>

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


## Nasazení kontejneru NGINX
<a id="deploy-an-nginx-container" class="xliff"></a>

Kontejner Dockeru můžete spustit v *podu* Kubernetes, který obsahuje jeden nebo více kontejnerů. 

Následující příkaz spustí kontejner Dockeru NGINX v podu Kubernetes na jednom z uzlů. V tomto případě kontejner spustí webový server NGINX získaný z image v [Docker Hubu](https://hub.docker.com/_/nginx/).

```azurecli-interactive
kubectl run nginx --image nginx
```
Pokud si chcete ověřit spuštění kontejneru, použijte příkaz:

```azurecli-interactive
kubectl get pods
```

## Zobrazení úvodní stránky serveru NGINX
<a id="view-the-nginx-welcome-page" class="xliff"></a>
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


## Odstranění clusteru
<a id="delete-cluster" class="xliff"></a>
Pokud už cluster nepotřebujete, můžete k odebrání skupiny prostředků, služby kontejneru a všech souvisejících prostředků použít příkaz [az group delete](/cli/azure/group#delete).

```azurecli-interactive 
az group delete --name myResourceGroup
```


## Další kroky
<a id="next-steps" class="xliff"></a>

V tomto rychlém úvodním kurzu jste nasadili cluster Kubernetes, připojili se přes `kubectl` a nasadili pod s kontejnerem NGINX. Další informace o Azure Container Service získáte v kurzu o clusteru Kubernetes.

> [!div class="nextstepaction"]
> [Správa clusteru ACS Kubernetes](./container-service-tutorial-kubernetes-prepare-app.md)

