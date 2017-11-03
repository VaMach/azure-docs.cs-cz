---
title: "Kubernetes na kurz pro Azure – nasazení aplikace | Microsoft Docs"
description: "Kurz AKS – nasazení aplikace"
services: container-service
documentationcenter: 
author: neilpeterson
manager: timlt
editor: 
tags: aks, azure-container-service
keywords: "Docker, Kontejnery, mikroslužby, Kubernetes, DC/OS, Azure"
ms.assetid: 
ms.service: container-service
ms.devlang: aurecli
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/24/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 73b943709fb3846058d4b1c09bc76b09460855ed
ms.sourcegitcommit: b979d446ccbe0224109f71b3948d6235eb04a967
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/25/2017
---
# <a name="run-applications-in-azure-container-service-aks"></a>Spuštění aplikace v Azure Container Service (AKS)

V tomto kurzu součástí čtyři osm, vzorová aplikace je nasazený do clusteru s podporou Kubernetes. Dokončit krokům patří:

> [!div class="checklist"]
> * Soubory manifestu Kubernetes aktualizace
> * Spuštění aplikace v Kubernetes
> * Testování aplikace

V následujících kurzech této aplikace je škálovat na více systémů, aktualizovat, a Operations Management Suite konfigurované pro monitorování Kubernetes clusteru.

Tento kurz předpokládá základní znalosti o Kubernetes koncepty, podrobné informace o Kubernetes najdete [Kubernetes dokumentaci](https://kubernetes.io/docs/home/).

## <a name="before-you-begin"></a>Než začnete

V předchozí kurzy aplikace byla zabalené do kontejneru image, tuto bitovou kopii byl odeslán do registru kontejner Azure a Kubernetes cluster byla vytvořena. 

K dokončení tohoto kurzu, budete potřebovat předem vytvořené `azure-vote-all-in-one-redis.yml` Kubernetes soubor manifestu. Tento soubor byl stažen se zdrojovým kódem aplikace v předchozí kurzu. Ověřte, že máte klonovat úložiště a že jste změnili adresáře do klonovaný úložišti.

Pokud se ještě provést tyto kroky a chcete sledovat, vrátit [kurzu 1 – Vytvoření kontejneru image](./tutorial-kubernetes-prepare-app.md). 

## <a name="update-manifest-file"></a>Aktualizace souboru manifestu

V tomto kurzu jsou využívány Azure kontejneru registru (ACR) pro uložení image kontejneru. Než spustíte aplikaci, je třeba aktualizovat v souboru manifestu Kubernetes název ACR přihlášení serveru.

Získat název ACR přihlášení serveru s [az acr seznamu](/cli/azure/acr#list) příkaz.

```azurecli
az acr list --resource-group myResourceGroup --query "[].{acrLoginServer:loginServer}" --output table
```

Soubor manifestu musí být předem vytvořené s názvem serveru přihlášení `microsoft`. V každém textovém editoru otevřete soubor. V tomto příkladu je soubor otevřít s `vi`.

```console
vi azure-vote-all-in-one-redis.yml
```

Nahraďte `microsoft` s název ACR přihlášení serveru. Tato hodnota je nalezen na řádku **47** souboru manifestu.

```yaml
containers:
- name: azure-vote-front
  image: microsoft/azure-vote-front:redis-v1
```

Uložte a zavřete soubor.

## <a name="deploy-application"></a>Nasazení aplikace

Pomocí příkazu [kubectl create](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#create) spusťte aplikaci. Tento příkaz analyzuje souboru manifestu a vytvořit objekty definované Kubernetes.

```azurecli
kubectl create -f azure-vote-all-in-one-redis.yml
```

Výstup:

```
deployment "azure-vote-back" created
service "azure-vote-back" created
deployment "azure-vote-front" created
service "azure-vote-front" created
```

## <a name="test-application"></a>Testování aplikace

A [Kubernetes služby](https://kubernetes.io/docs/concepts/services-networking/service/) se vytvoří, který zpřístupňuje aplikace k Internetu. Tento proces může trvat několik minut. 

Pomocí příkazu [kubectl get service](https://kubernetes.io/docs/user-guide/kubectl/v1.7/#get) s argumentem `--watch` můžete sledovat průběh.

```azurecli
kubectl get service azure-vote-front --watch
```

Zpočátku se *EXTERNAL-IP* (Externí IP adresa) pro službu *azure-vote-front* bude zobrazovat ve stavu *probíhá*.
  
```
azure-vote-front   10.0.34.242   <pending>     80:30676/TCP   7s
```

Jednou *externí IP* adres se změnil z *čekající* k *IP adresu*, použít `CTRL-C` kubectl sledovat proces zastavíte. 

```
azure-vote-front   10.0.34.242   52.179.23.131   80:30676/TCP   2m
```

Informace o aplikaci, přejděte na externí IP adresu.

![Obrázek clusteru Kubernetes v Azure](media/container-service-kubernetes-tutorials/azure-vote.png)

## <a name="next-steps"></a>Další kroky

V tomto kurzu aplikace Azure hlas nasazená do clusteru s podporou Kubernetes v AKS. Dokončené úkoly patří:  

> [!div class="checklist"]
> * Stáhnout soubory manifestu Kubernetes
> * Spusťte aplikaci v Kubernetes
> * Testování aplikace

Přechodu na v dalším kurzu se dozvíte o škálování Kubernetes aplikace a podpůrné infrastruktuře Kubernetes. 

> [!div class="nextstepaction"]
> [Škálování Kubernetes aplikace a infrastrukturu](./tutorial-kubernetes-scale.md)
