---
title: "Kubernetes na kurz pro Azure – nasazení aplikace"
description: "Kurz AKS – nasazení aplikace"
services: container-service
author: neilpeterson
manager: timlt
ms.service: container-service
ms.topic: tutorial
ms.date: 10/24/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 1433fce61bc26a6aa0e4742195e623329eb5f0cc
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/11/2017
---
# <a name="run-applications-in-azure-container-service-aks"></a>Spuštění aplikace v Azure Container Service (AKS)

V tomto kurzu součástí čtyři osm, vzorová aplikace je nasazený do clusteru s podporou Kubernetes. Dokončit krokům patří:

> [!div class="checklist"]
> * Soubory manifestu Kubernetes aktualizace
> * Spuštění aplikace v Kubernetes
> * Testování aplikace

V následujících kurzech této aplikace je škálovat na více systémů, aktualizovat, a Operations Management Suite konfigurované pro monitorování Kubernetes clusteru.

Tento kurz předpokládá základní znalosti o Kubernetes koncepty, podrobné informace o Kubernetes najdete [Kubernetes dokumentace][kubernetes-documentation].

## <a name="before-you-begin"></a>Než začnete

V předchozí kurzy aplikace byla zabalené do kontejneru image, tuto bitovou kopii byl odeslán do registru kontejner Azure a Kubernetes cluster byla vytvořena. 

K dokončení tohoto kurzu, budete potřebovat předem vytvořené `azure-vote-all-in-one-redis.yml` Kubernetes soubor manifestu. Tento soubor byl stažen se zdrojovým kódem aplikace v předchozí kurzu. Ověřte, že máte klonovat úložiště a že jste změnili adresáře do klonovaný úložišti.

Pokud se ještě provést tyto kroky a chcete sledovat, vrátit [kurzu 1 – Vytvoření kontejneru image][aks-tutorial-prepare-app].

## <a name="update-manifest-file"></a>Aktualizace souboru manifestu

V tomto kurzu jsou využívány Azure kontejneru registru (ACR) pro uložení image kontejneru. Než spustíte aplikaci, je třeba aktualizovat v souboru manifestu Kubernetes název ACR přihlášení serveru.

Získat název ACR přihlášení serveru s [az acr seznamu] [ az-acr-list] příkaz.

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

Použití [kubectl vytvořit] [ kubectl-create] příkaz ke spuštění aplikace. Tento příkaz analyzuje souboru manifestu a vytvořit objekty definované Kubernetes.

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

A [Kubernetes služby] [ kubernetes-service] je vytvořen, který zpřístupňuje aplikace k Internetu. Tento proces může trvat několik minut. 

Chcete-li sledovat průběh, použijte [kubectl získat služby] [ kubectl-get] s `--watch` argument.

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
> [Škálování Kubernetes aplikace a infrastrukturu][aks-tutorial-scale]

<!-- LINKS - external -->
[kubectl-create]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#create
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[kubernetes-documentation]: https://kubernetes.io/docs/home/
[kubernetes-service]: https://kubernetes.io/docs/concepts/services-networking/service/

<!-- LINKS - internal -->
[aks-tutorial-prepare-app]: ./tutorial-kubernetes-prepare-app.md
[aks-tutorial-scale]: ./tutorial-kubernetes-scale.md
[az-acr-list]: /cli/azure/acr#list
