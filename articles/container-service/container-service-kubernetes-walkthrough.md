---
title: "Rychlý start clusteru Kubernetes v Azure | Dokumentace Microsoftu"
description: "Nasazení clusteru Kubernetes ve službě Azure Container Service a zahájení práce"
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
ms.date: 03/01/2017
ms.author: anhowe
ms.custom: H1Hack27Feb2017
translationtype: Human Translation
ms.sourcegitcommit: 2464901d22bb91cbf396ef60f4bda6d979b578b7
ms.openlocfilehash: 49602804cc6b4d4f98c802c1a3b651dda2634bb7
ms.lasthandoff: 03/02/2017

---

# <a name="get-started-with-a-kubernetes-cluster-in-container-service"></a>Zahájení práce s clusterem Kubernetes ve službě Container Service


Pokyny v tomto článku ukazují, jak pomocí příkazů Azure CLI 2.0 vytvořit cluster Kubernetes. Následně můžete pomocí nástroje příkazového řádku `kubectl` začít pracovat s kontejnery v clusteru.

Následující obrázek ukazuje architekturu clusteru kontejnerové služby s jedním hlavním uzlem a dvěma agenty. Hlavní uzel obsluhuje pro Kubernetes rozhraní REST API. Agentské uzly jsou seskupené ve skupině dostupnosti Azure a spouští vaše kontejnery. Všechny virtuální počítače jsou ve stejné privátní virtuální síti a jsou vzájemně plně přístupné.

![Obrázek clusteru Kubernetes v Azure](media/container-service-kubernetes-walkthrough/kubernetes.png)

## <a name="prerequisites"></a>Požadavky
Tento názorný průvodce předpokládá, že máte nainstalované a nastavené rozhraní [Azure CLI v. 2.0](/cli/azure/install-az-cli2). Také musíte mít veřejný klíč SSH RSA v souboru `~/.ssh/id_rsa.pub`. Pokud jej nemáte, podívejte se na postup pro [OS X a Linux](../virtual-machines/virtual-machines-linux-mac-create-ssh-keys.md) nebo [Windows](../virtual-machines/virtual-machines-linux-ssh-from-windows.md).






## <a name="create-your-kubernetes-cluster"></a>Vytvoření clusteru Kubernetes

Zde je popsáno vytvoření clusteru pomocí stručných příkazů prostředí v Azure CLI 2.0. Další informace najdete v tématu [Vytvoření clusteru Azure Container Service pomocí Azure CLI 2.0](container-service-create-acs-cluster-cli.md).

### <a name="create-a-resource-group"></a>Vytvoření skupiny prostředků
Pokud chcete vytvořit cluster, musíte si nejprve v konkrétním umístění vytvořit skupinu prostředků. Spusťte příkazy podobné těmto:

```console
RESOURCE_GROUP=my-resource-group
LOCATION=westus
az group create --name=$RESOURCE_GROUP --location=$LOCATION
```

### <a name="create-a-cluster"></a>Vytvoření clusteru
Jakmile máte skupinu prostředků, můžete v této skupině vytvořit cluster:

```console
DNS_PREFIX=some-unique-value
CLUSTER_NAME=any-acs-cluster-name
az acs create --orchestrator-type=kubernetes --resource-group $RESOURCE_GROUP --name=$CLUSTER_NAME --dns-prefix=$DNS_PREFIX
```

> [!NOTE]
> Rozhraní příkazového řádku během nasazení nahraje do virtuálních počítačů s Linuxem soubor `~/.ssh/id_rsa.pub`.
>

Po dokončení tohoto příkazu byste měli mít funkční cluster Kubernetes.

### <a name="connect-to-the-cluster"></a>Připojení ke clusteru

Následující příkazy Azure CLI slouží k připojení ke clusteru Kubernetes z klientského počítače pomocí klienta příkazového řádku Kubernetes `kubectl`. Další informace najdete v [Připojení ke clusteru služby Azure Container Service](container-service-connect.md).

Pokud `kubectl` ještě nemáte nainstalovaného, můžete k instalaci použít:

```console
az acs kubernetes install-cli
```

Jakmile je `kubectl` nainstalovaný, spuštěním následujícího příkazu stáhněte hlavní konfiguraci clusteru Kubernetes do souboru ~/.kube/config:

```console
az acs kubernetes get-credentials --resource-group=$RESOURCE_GROUP --name=$CLUSTER_NAME
```

V tomto okamžiku byste měli být připravení přistoupit ke clusteru ze svého počítače. Zkuste spustit:
```console
kubectl get nodes
```

Ověřte, že vidíte seznam počítačů v clusteru.

## <a name="create-your-first-kubernetes-service"></a>Vytvoření první služby Kubernetes

Po dokončení tohoto názorného průvodce budete umět:
 * nasadit aplikaci v Dockeru a zpřístupnit ji světu,
 * použít `kubectl exec` ke spouštění příkazů v kontejneru a 
 * využívat přístup k řídicímu panelu Kubernetes.

### <a name="start-a-simple-container"></a>Spuštění jednoduchého kontejneru
Jednoduchý kontejner (v tomto případě webový server Nginx) můžete spustit pomocí příkazu:

```console
kubectl run nginx --image nginx
```

Tento příkaz spustí kontejner Dockeru Nginx v podu na jednom z uzlů.

Pokud chcete zobrazit spuštěný kontejner, spusťte příkaz:

```console
kubectl get pods
```

### <a name="expose-the-service-to-the-world"></a>Zpřístupnění služby ostatním
Pokud chcete službu zpřístupnit ostatním, vytvořte Kubernetes `Service` typu `LoadBalancer`:

```console
kubectl expose deployments nginx --port=80 --type=LoadBalancer
```

To způsobí, že Kubernetes vytvoří pravidlo nástroje pro vyrovnávání zatížení Azure s veřejnou IP adresou. Rozšíření této změny do nástroje pro vyrovnávání zatížení trvá několik minut. Další informace najdete v tématu [Kontejnery pro vyrovnávání zatížení v clusteru Kubernetes v Azure Container Service](container-service-kubernetes-load-balancing.md).

Spusťte následující příkaz a sledujte, jak se stav služby změní z `pending` (čekající) na externí IP adresu:

```console
watch 'kubectl get svc'
```

  ![Obrázek sledování přechodu ze stavu čekající na externí IP adresu](media/container-service-kubernetes-walkthrough/kubernetes-nginx3.png)

Jakmile se zobrazí externí IP adresa, můžete na ni přejít pomocí prohlížeče:

  ![Obrázek přechodu na Nginx](media/container-service-kubernetes-walkthrough/kubernetes-nginx4.png)  


### <a name="browse-the-kubernetes-ui"></a>Procházení uživatelského rozhraní Kubernetes
K zobrazení webového rozhraní Kubernetes můžete použít příkaz:

```console
kubectl proxy
```
Spustí v místním hostiteli jednoduchý ověřený proxy server, který můžete použít k zobrazení [webového uživatelského rozhraní Kubernetes](http://localhost:8001/ui). Další informace najdete v tématu [Používání webového uživatelského rozhraní Kubernetes s Azure Container Service](container-service-kubernetes-ui.md).

![Obrázek řídicího panelu Kubernetes](media/container-service-kubernetes-walkthrough/kubernetes-dashboard.png)

### <a name="remote-sessions-inside-your-containers"></a>Vzdálené relace uvnitř kontejnerů
Kubernetes umožňuje spouštět příkazy ve vzdáleném kontejneru Dockeru spuštěném ve vašem clusteru.

```console
# Get the name of your nginx pods
kubectl get pods
```

S využitím názvu podu můžete v podu spustit vzdálený příkaz.  Například:

```console
kubectl exec nginx-701339712-retbj date
```

Pomocí příznaků `-it` můžete také získat plně interaktivní relaci:

```console
kubectl exec nginx-701339712-retbj -it bash
```

![Vzdálená relace uvnitř kontejneru](media/container-service-kubernetes-walkthrough/kubernetes-remote.png)



## <a name="next-steps"></a>Další kroky

Informace o lepším využívání clusteru Kubernetes najdete v následujících zdrojích:

* [Kubernetes Bootcamp](https://katacoda.com/embed/kubernetes-bootcamp/1/) – ukazuje, jak nasadit, škálovat, aktualizovat a ladit kontejnerizované aplikace.
* [Uživatelská příručka Kubernetes](http://kubernetes.io/docs/user-guide/) – poskytuje informace o spouštění programů v existujícím clusteru Kubernetes.
* [Příklady Kubernetes](https://github.com/kubernetes/kubernetes/tree/master/examples) – poskytuje příklady spouštění skutečných aplikací s využitím Kubernetes.

