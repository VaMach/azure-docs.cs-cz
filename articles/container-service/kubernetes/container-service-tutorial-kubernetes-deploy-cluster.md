---
title: "Kurz pro Azure Container Service – nasazení clusteru | Microsoft Docs"
description: "Kurz pro Azure Container Service – nasazení clusteru"
services: container-service
documentationcenter: 
author: neilpeterson
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: "Docker, Kontejnery, mikroslužby, Kubernetes, DC/OS, Azure"
ms.assetid: 
ms.service: container-service
ms.devlang: azurecli
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/14/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: dcced32925ecd109be8dde6324cca4899923679b
ms.sourcegitcommit: c5eeb0c950a0ba35d0b0953f5d88d3be57960180
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/24/2017
---
# <a name="deploy-a-kubernetes-cluster-in-azure-container-service"></a>Nasazení clusteru Kubernetes v Azure Container Service

[!INCLUDE [aks-preview-redirect.md](../../../includes/aks-preview-redirect.md)]

Kubernetes poskytuje distribuovanou platformu pro kontejnerizované aplikace. Zřizování clusteru výroby připravené Kubernetes s Azure Container Service je usnadňují a urychlují. V tomto kurzu, část 3 7, je nasazení clusteru Azure Container Service Kubernetes služby. Dokončit krokům patří:

> [!div class="checklist"]
> * Nasazení služby ACS Kubernetes clusteru
> * Instalace rozhraní příkazového řádku Kubernetes (kubectl)
> * Konfigurace kubectl

V následujících kurzech aplikace Azure hlas je nasadit do clusteru, škálovat, aktualizovat a Operations Management Suite je nakonfigurované pro monitorování Kubernetes clusteru.

## <a name="before-you-begin"></a>Než začnete

V předchozí kurzy byl bitovou kopii kontejner vytvořit a nahrát do Azure kontejneru registru instance. Pokud se ještě provést tyto kroky a chcete sledovat, vrátit [kurzu 1 – Vytvoření kontejneru image](./container-service-tutorial-kubernetes-prepare-app.md).

## <a name="create-kubernetes-cluster"></a>Vytvoření clusteru Kubernetes

Vytvořte cluster Kubernetes ve službě Azure Container Service pomocí příkazu [az acs create](/cli/azure/acs#create). 

Následující příklad vytvoří cluster s názvem `myK8sCluster` ve skupině prostředků s názvem `myResourceGroup`. Tato skupina prostředků byl vytvořen v [předchozí kurzu](./container-service-tutorial-kubernetes-prepare-acr.md).

```azurecli-interactive 
az acs create --orchestrator-type kubernetes --resource-group myResourceGroup --name myK8SCluster --generate-ssh-keys 
```

V některých případech, například s omezenou zkušební verzí, má předplatné Azure omezený přístup k prostředkům Azure. Pokud se nasazení nezdaří kvůli omezenému počtu dostupných jader, snižte výchozí počet agentů přidáním možnosti `--agent-count 1` do příkazu [az acs create](/cli/azure/acs#create). 

Po několika minutách se nasazení dokončí a formátu json vrátí informace o nasazení služby ACS.

## <a name="install-the-kubectl-cli"></a>Nainstalujte kubectl rozhraní příkazového řádku

Chcete-li připojit ke clusteru Kubernetes z klientského počítače, použijte [kubectl](https://kubernetes.io/docs/user-guide/kubectl/), klient příkazového řádku Kubernetes. 

Pokud používáte Azure Cloud Shell, kubectl je už nainstalován. Pokud chcete nainstalovat místně, použijte [az acs kubernetes instalace rozhraní příkazového řádku](/cli/azure/acs/kubernetes#install-cli) příkaz.

Pokud provozujete v systému Linux nebo systému macOS, můžete spustit pomocí příkazu "sudo". V systému Windows Ujistěte se, že vaše prostředí byl spuštěn jako správce.

```azurecli-interactive 
az acs kubernetes install-cli 
```

V systému Windows, je výchozí instalace *c:\program files (x86)\kubectl.exe*. Potřebujete přidejte tento soubor do cesty k systému Windows. 

## <a name="connect-with-kubectl"></a>Připojení přes kubectl

Abyste nakonfigurovali kubectl pro připojení ke svému clusteru Kubernetes, spusťte příkaz [az acs kubernetes get-credentials](/cli/azure/acs/kubernetes#get-credentials).

```azurecli-interactive 
az acs kubernetes get-credentials --resource-group myResourceGroup --name myK8SCluster
```

Chcete-li ověřit připojení ke clusteru, spusťte [kubectl získat uzly](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#get) příkaz.

```azurecli-interactive
kubectl get nodes
```

Výstup:

```bash
NAME                    STATUS                     AGE       VERSION
k8s-agent-98dc3136-0    Ready                      5m        v1.6.2
k8s-agent-98dc3136-1    Ready                      5m        v1.6.2
k8s-agent-98dc3136-2    Ready                      5m        v1.6.2
k8s-master-98dc3136-0   Ready,SchedulingDisabled   5m        v1.6.2
```

V kurzu dokončení máte připravené pro zatížení clusteru služby ACS Kubernetes. V následujících kurzech aplikace s více kontejnerů je nasadit do tohoto clusteru, škálovat na více systémů, aktualizovat a sledovat.

## <a name="next-steps"></a>Další kroky

V tomto kurzu byl nasazen clusteru Azure Container Service Kubernetes. Dokončili jste následující kroky:

> [!div class="checklist"]
> * Nasazení clusteru s podporou Kubernetes ACS
> * Nainstalovat rozhraní příkazového řádku Kubernetes (kubectl)
> * Nakonfigurované kubectl

Přechodu na v dalším kurzu se dozvíte o spuštění aplikace v clusteru.

> [!div class="nextstepaction"]
> [Nasazení aplikace v Kubernetes](./container-service-tutorial-kubernetes-deploy-application.md)
