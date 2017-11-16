---
title: "Kubernetes na kurz pro Azure – škálování aplikace | Microsoft Docs"
description: "Kurz AKS – škálování aplikace"
services: container-service
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: aks, azure-container-service
keywords: Docker, kontejnery, Micro-services, Kubernetes, Azure
ms.assetid: 
ms.service: container-service
ms.devlang: aurecli
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/15/2017
ms.author: danlep
ms.custom: mvc
ms.openlocfilehash: f060b37d5ae02dfd53f513b134692186024cf727
ms.sourcegitcommit: c25cf136aab5f082caaf93d598df78dc23e327b9
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/15/2017
---
# <a name="scale-application-in-azure-container-service-aks"></a>Škálování aplikací v Azure Container Service (AKS)

Pokud jste byli po kurzů k, máte funkční Kubernetes cluster v AKS a nasadit aplikaci Azure hlasování.

V tomto kurzu, součástí pět osm, škálovat pracovními stanicemi soustředěnými kolem v aplikaci a zkuste to pod automatické škálování. Můžete také zjistěte, jak se škálovat počet uzlů virtuálního počítače Azure, chcete-li změnit kapacity clusteru pro hostování úloh. Dokončené úkoly patří:

> [!div class="checklist"]
> * Škálování Kubernetes Azure uzly
> * Ruční škálování pracovními stanicemi soustředěnými kolem Kubernetes
> * Konfigurace automatického škálování pracovními stanicemi soustředěnými kolem spuštění aplikace front-endu

V následujících kurzech hlas Azure bude aktualizována a Operations Management Suite konfigurované pro monitorování Kubernetes clusteru.

## <a name="before-you-begin"></a>Než začnete

V předchozích kurzy byla aplikace zabalené do kontejneru image, tento image nahrané do registru kontejner Azure a cluster Kubernetes vytvořit. Aplikace pak byl na Kubernetes clusteru spusťte.

Pokud se ještě provést tyto kroky a chcete sledovat, vrátit [kurzu 1 – Vytvoření kontejneru image](./tutorial-kubernetes-prepare-app.md).

## <a name="scale-aks-nodes"></a>Škálování AKS uzly

Pokud jste vytvořili Kubernetes clusteru pomocí příkazů v předchozí kurzu, má jeden uzel. Počet uzlů můžete upravit ručně, pokud máte v plánu více nebo méně kontejneru zatížení v clusteru.

Následující příklad zvyšuje počet uzlů na tři v Kubernetes clusteru s názvem *myK8sCluster*. Příkaz trvá několik minut na dokončení.

```azurecli
az aks scale --resource-group=myResourceGroup --name=myK8SCluster --node-count 3
```

Výstup je podobný tomuto:

```
"agentPoolProfiles": [
  {
    "count": 3,
    "dnsPrefix": null,
    "fqdn": null,
    "name": "myK8sCluster",
    "osDiskSizeGb": null,
    "osType": "Linux",
    "ports": null,
    "storageProfile": "ManagedDisks",
    "vmSize": "Standard_D2_v2",
    "vnetSubnetId": null
  }
```

## <a name="manually-scale-pods"></a>Ručně škálovat, pracovními stanicemi soustředěnými kolem

Proto úplně, Azure hlas front-endu a Redis instance byly nasazené, každý s jednu repliku. Pokud chcete ověřit, spusťte [kubectl získat](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#get) příkaz.

```azurecli
kubectl get pods
```

Výstup:

```
NAME                               READY     STATUS    RESTARTS   AGE
azure-vote-back-2549686872-4d2r5   1/1       Running   0          31m
azure-vote-front-848767080-tf34m   1/1       Running   0          31m
```

Ručně změnit počet pracovními stanicemi soustředěnými kolem v `azure-vote-front` nasazení pomocí [kubectl škálování](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#scale) příkaz. Tento příklad zvyšuje počet 5.

```azurecli
kubectl scale --replicas=5 deployment/azure-vote-front
```

Spustit [kubectl získat pracovními stanicemi soustředěnými kolem](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#get) k ověření, že Kubernetes vytváří pracovními stanicemi soustředěnými kolem. Za minutu další pracovními stanicemi soustředěnými kolem běží:

```azurecli
kubectl get pods
```

Výstup:

```
NAME                                READY     STATUS    RESTARTS   AGE
azure-vote-back-2606967446-nmpcf    1/1       Running   0          15m
azure-vote-front-3309479140-2hfh0   1/1       Running   0          3m
azure-vote-front-3309479140-bzt05   1/1       Running   0          3m
azure-vote-front-3309479140-fvcvm   1/1       Running   0          3m
azure-vote-front-3309479140-hrbf2   1/1       Running   0          15m
azure-vote-front-3309479140-qphz8   1/1       Running   0          3m
```

## <a name="autoscale-pods"></a>Pracovními stanicemi soustředěnými kolem škálování

Podporuje Kubernetes [automatické škálování vodorovné pod](https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale/) upravit počet pracovními stanicemi soustředěnými kolem v nasazení v závislosti na využití procesoru nebo jiné vybrat metriky.

Pokud chcete použít autoscaler, musí mít vaše pracovními stanicemi soustředěnými kolem procesoru požadavky a omezení definovaná. V `azure-vote-front` nasazení, front-end kontejneru požadavky 0,25 procesor limit 0,5 procesoru. Nastavení vypadat podobně jako:

```YAML
resources:
  requests:
     cpu: 250m
  limits:
     cpu: 500m
```

Následující příklad používá [kubectl škálování](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#autoscale) příkaz pro škálování počtu pracovními stanicemi soustředěnými kolem v `azure-vote-front` nasazení. Sem pokud využití procesoru přesahuje 50 %, autoscaler zvyšuje pracovními stanicemi soustředěnými kolem maximálně 10.


```azurecli
kubectl autoscale deployment azure-vote-front --cpu-percent=50 --min=3 --max=10
```

Pokud chcete zobrazit stav autoscaler, spusťte následující příkaz:

```azurecli
kubectl get hpa
```

Výstup:

```
NAME               REFERENCE                     TARGETS    MINPODS   MAXPODS   REPLICAS   AGE
azure-vote-front   Deployment/azure-vote-front   0% / 50%   3         10        3          2m
```

Po několika minutách, s minimálním zatížením hlas Azure aplikace a snižuje počet replik pod automaticky na 3.

## <a name="next-steps"></a>Další kroky

V tomto kurzu použít v clusteru Kubernetes různé funkce škálování. Úlohy popsané součástí:

> [!div class="checklist"]
> * Ruční škálování pracovními stanicemi soustředěnými kolem Kubernetes
> * Konfigurace automatického škálování pracovními stanicemi soustředěnými kolem spuštění aplikace front-endu
> * Škálování Kubernetes Azure uzly

Přechodu na v dalším kurzu se dozvíte o aktualizaci aplikace v Kubernetes.

> [!div class="nextstepaction"]
> [Aktualizace aplikace v Kubernetes](./tutorial-kubernetes-app-update.md)