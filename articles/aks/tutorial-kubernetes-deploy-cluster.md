---
title: "Kubernertes na kurz pro Azure – nasazení clusteru | Microsoft Docs"
description: "Kurz AKS – nasazení clusteru"
services: container-service
documentationcenter: 
author: neilpeterson
manager: timlt
editor: 
tags: aks, azure-container-service
keywords: "Docker, Kontejnery, mikroslužby, Kubernetes, DC/OS, Azure"
ms.assetid: 
ms.service: container-service
ms.devlang: azurecli
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/24/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 7f9991d2254011080185a555f5351dce85f73704
ms.sourcegitcommit: 9c3150e91cc3075141dc2955a01f47040d76048a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/26/2017
---
# <a name="deploy-an-azure-container-service-aks-cluster"></a>Nasazení clusteru Azure Container Service (AKS)

Kubernetes poskytuje distribuovanou platformu pro kontejnerizované aplikace. Zřizování clusteru výroby připravené Kubernetes s AKS, je usnadňují a urychlují. V tomto kurzu, součástí tři osm, Kubernetes cluster nasadí AKS. Dokončit krokům patří:

> [!div class="checklist"]
> * Nasazení clusteru s podporou Kubernetes AKS
> * Instalace rozhraní příkazového řádku Kubernetes (kubectl)
> * Konfigurace kubectl

V následujících kurzech aplikace Azure hlas je nasadit do clusteru, škálovat, aktualizovat a Operations Management Suite je nakonfigurované pro monitorování Kubernetes clusteru.

## <a name="before-you-begin"></a>Než začnete

V předchozí kurzy byl bitovou kopii kontejner vytvořit a nahrát do Azure kontejneru registru instance. Pokud se ještě provést tyto kroky a chcete sledovat, vrátit [kurzu 1 – Vytvoření kontejneru image](./tutorial-kubernetes-prepare-app.md).

## <a name="enabling-aks-preview-for-your-azure-subscription"></a>Povolení AKS náhled pro vaše předplatné Azure
Při AKS je ve verzi preview, vytváření nových clusterů vyžaduje funkce příznak vaše předplatné. Může požádat o tuto funkci pro libovolný počet předplatná, která se má použít. Použití `az provider register` příkaz pro registraci zprostředkovatele AKS:

```azurecli-interactive
az provider register -n Microsoft.ContainerService
```

Po registraci, nyní jste připraveni k vytvoření clusteru s podporou Kubernetes s AKS.

## <a name="create-kubernetes-cluster"></a>Vytvoření clusteru Kubernetes

Následující příklad vytvoří cluster s názvem `myK8sCluster` ve skupině prostředků s názvem `myResourceGroup`. Tato skupina prostředků byl vytvořen v [předchozí kurzu](./tutorial-kubernetes-prepare-acr.md).

```azurecli
az aks create --resource-group myResourceGroup --name myK8sCluster --agent-count 1 --generate-ssh-keys
```

Po několika minutách se nasazení dokončí a formátu json vrátí informace o nasazení AKS.

## <a name="install-the-kubectl-cli"></a>Nainstalujte kubectl rozhraní příkazového řádku

Chcete-li připojit ke clusteru Kubernetes z klientského počítače, použijte [kubectl](https://kubernetes.io/docs/user-guide/kubectl/), klient příkazového řádku Kubernetes.

Pokud používáte Azure Cloud Shell, kubectl je už nainstalován. Pokud chcete nainstalovat místně, spusťte následující příkaz:

```azurecli
az aks install-cli
```

## <a name="connect-with-kubectl"></a>Připojení přes kubectl

Konfigurovat kubectl se připojit ke svému clusteru Kubernetes, spusťte následující příkaz:

```azurecli
az aks get-credentials --resource-group=myResourceGroup --name=myK8sCluster
```

Chcete-li ověřit připojení ke clusteru, spusťte [kubectl získat uzly](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#get) příkaz.

```azurecli
kubectl get nodes
```

Výstup:

```
NAME                          STATUS    AGE       VERSION
k8s-myk8scluster-36346190-0   Ready     49m       v1.7.7
```

V kurzu dokončení máte připravené pro úlohy AKS cluster. V následujících kurzech aplikace s více kontejnerů je nasadit do tohoto clusteru, škálovat na více systémů, aktualizovat a sledovat.

## <a name="next-steps"></a>Další kroky

V tomto kurzu Kubernetes cluster nasazená v AKS. Dokončili jste následující kroky:

> [!div class="checklist"]
> * Nasazení clusteru s podporou Kubernetes AKS
> * Nainstalovat rozhraní příkazového řádku Kubernetes (kubectl)
> * Nakonfigurované kubectl

Přechodu na v dalším kurzu se dozvíte o spuštění aplikace v clusteru.

> [!div class="nextstepaction"]
> [Nasazení aplikace v Kubernetes](./tutorial-kubernetes-deploy-application.md)
