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
ms.date: 05/08/2017
ms.author: anhowe
ms.custom: H1Hack27Feb2017
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: 2ec155129374c03ba7e0ecaa5d2bf29a1d3111aa
ms.contentlocale: cs-cz
ms.lasthandoff: 05/10/2017

---

# <a name="get-started-with-a-kubernetes-cluster-in-container-service"></a>Zahájení práce s clusterem Kubernetes ve službě Container Service


Tento názorný postup ukazuje, jak používat příkazy rozhraní příkazového řádku Azure CLI 2.0 k vytvoření clusteru Kubernetes ve službě Azure Container Service. Následně můžete pomocí nástroje příkazového řádku `kubectl` začít pracovat s kontejnery v clusteru.

Následující obrázek ukazuje architekturu clusteru služby Container Service s jedním hlavním linuxovým uzlem a dvěma agenty systému Linux. Hlavní uzel obsluhuje pro Kubernetes rozhraní REST API. Agentské uzly jsou seskupené ve skupině dostupnosti Azure a spouští vaše kontejnery. Všechny virtuální počítače jsou ve stejné privátní virtuální síti a jsou vzájemně plně přístupné.

![Obrázek clusteru Kubernetes v Azure](media/container-service-kubernetes-walkthrough/kubernetes.png)

Další související informace najdete v tématu [Úvod do Azure Container Service](container-service-intro.md) a v [dokumentaci ke Kubernetes](https://kubernetes.io/docs/home/).

## <a name="prerequisites"></a>Požadavky
K vytvoření clusteru Azure Container Service pomocí Azure CLI 2.0 musíte mít:
* účet Azure ([získejte bezplatnou zkušební verzi](https://azure.microsoft.com/pricing/free-trial/)),
* nainstalované a nastavené [Azure CLI 2.0](/cli/azure/install-az-cli2)

Kromě toho potřebujete (nebo můžete pomocí Azure CLI vygenerovat automaticky během nasazování clusteru):

* **Veřejný klíč SSH RSA:** Pokud chcete vytvořit klíče SSH (Secure Shell) RSA předem, postupujte podle pokynů pro systémy [macOS a Linux](../virtual-machines/linux/mac-create-ssh-keys.md) nebo [Windows](../virtual-machines/linux/ssh-from-windows.md). 

* **ID a tajný klíč klienta instančního objektu:** Postup vytvoření instančního objektu služby Azure Active Directory a další informace najdete v tématu [O instančním objektu pro cluster Kubernetes](container-service-kubernetes-service-principal.md).

 Příklad příkazu v tomto článku automaticky vygeneruje klíče SSH a instanční objekt.

## <a name="create-your-kubernetes-cluster"></a>Vytvoření clusteru Kubernetes

Zde je popsáno vytvoření clusteru pomocí stručných příkazů prostředí Bash v Azure CLI 2.0. 

### <a name="create-a-resource-group"></a>Vytvoření skupiny prostředků
Abyste mohli vytvořit cluster, nejprve potřebujete vytvořit skupinu prostředků v umístění, kde je [dostupná](https://azure.microsoft.com/regions/services/) služba Azure Container Service. Spusťte příkazy podobné těmto:

```azurecli
RESOURCE_GROUP=my-resource-group
LOCATION=westus
az group create --name=$RESOURCE_GROUP --location=$LOCATION
```

### <a name="create-a-cluster"></a>Vytvoření clusteru
Vytvořte ve skupině prostředků cluster Kubernetes pomocí příkazu `az acs create` s použitím `--orchestrator-type=kubernetes`. Syntaxi příkazu najdete v [nápovědě](/cli/azure/acs#create) k příkazu `az acs create`.

Tato verze příkazu pro cluster Kubernetes automaticky vygeneruje klíče SSH RSA a instanční objekt.



```azurecli
DNS_PREFIX=some-unique-value
CLUSTER_NAME=any-acs-cluster-name
az acs create --orchestrator-type=kubernetes --resource-group $RESOURCE_GROUP --name=$CLUSTER_NAME --dns-prefix=$DNS_PREFIX --generate-ssh-keys
```

Po několika minutách se příkaz dokončí a měli byste mít funkční cluster Kubernetes.

> [!IMPORTANT]
> Pokud váš účet nemá oprávnění k vytvoření instančního objektu služby Azure AD, příkaz vygeneruje chybu podobnou této: `Insufficient privileges to complete the operation.`. Další informace najdete v tématu [O instančním objektu pro cluster Kubernetes](container-service-kubernetes-service-principal.md).
> 



### <a name="connect-to-the-cluster"></a>Připojení ke clusteru

Pokud se připojujete ke clusteru Kubernetes z klientského počítače, použijte klienta příkazového řádku Kubernetes [`kubectl`](https://kubernetes.io/docs/user-guide/kubectl/). 

Pokud nástroj `kubectl` ještě nemáte nainstalovaný, můžete ho nainstalovat pomocí příkazu `az acs kubernetes install-cli`. (Můžete si ho také stáhnout z [webu Kubernetes](https://kubernetes.io/docs/tasks/kubectl/install/).)

```azurecli
sudo az acs kubernetes install-cli
```

> [!TIP]
> Ve výchozím nastavení tento příkaz nainstaluje binární soubor `kubectl` do adresáře `/usr/local/bin/kubectl` v systému Linux nebo macOS, nebo do adresáře `C:\Program Files (x86)\kubectl.exe` ve Windows. Pokud chcete zadat jinou instalační cestu, použijte parametr `--install-location`.
>
> Po instalaci `kubectl` se ujistěte, že jeho adresář je v systémové cestě, nebo ho tam přidejte. 
>


Pak spuštěním následujícího příkazu stáhněte hlavní konfiguraci clusteru Kubernetes do souboru `~/.kube/config`:

```azurecli
az acs kubernetes get-credentials --resource-group=$RESOURCE_GROUP --name=$CLUSTER_NAME
```

Další možnosti instalace a konfigurace `kubectl` najdete v části [Připojení ke clusteru Azure Container Service](container-service-connect.md).

V tomto okamžiku byste měli být připravení přistoupit ke clusteru ze svého počítače. Zkuste spustit:

```bash
kubectl get nodes
```

Ověřte, že vidíte seznam počítačů v clusteru.

## <a name="create-your-first-kubernetes-service"></a>Vytvoření první služby Kubernetes

Po dokončení tohoto názorného průvodce budete umět:
* nasadit aplikaci v Dockeru a zpřístupnit ji světu,
* použít `kubectl exec` ke spouštění příkazů v kontejneru a 
* využívat přístup k řídicímu panelu Kubernetes.

### <a name="start-a-container"></a>Spuštění kontejneru
Kontejner (v tomto případě webový server Nginx) můžete spustit pomocí příkazu:

```bash
kubectl run nginx --image nginx
```

Tento příkaz spustí kontejner Dockeru Nginx v podu na jednom z uzlů.

Pokud chcete zobrazit spuštěný kontejner, spusťte příkaz:

```bash
kubectl get pods
```

### <a name="expose-the-service-to-the-world"></a>Zpřístupnění služby ostatním
Pokud chcete službu zpřístupnit ostatním, vytvořte Kubernetes `Service` typu `LoadBalancer`:

```bash
kubectl expose deployments nginx --port=80 --type=LoadBalancer
```

Tento příkaz způsobí, že Kubernetes vytvoří pravidlo nástroje pro vyrovnávání zatížení Azure s veřejnou IP adresou. Rozšíření této změny do nástroje pro vyrovnávání zatížení trvá několik minut. Další informace najdete v tématu [Kontejnery pro vyrovnávání zatížení v clusteru Kubernetes v Azure Container Service](container-service-kubernetes-load-balancing.md).

Spusťte následující příkaz a sledujte, jak se stav služby změní z `pending` (čekající) na externí IP adresu:

```bash
watch 'kubectl get svc'
```

  ![Obrázek sledování přechodu ze stavu čekající na externí IP adresu](media/container-service-kubernetes-walkthrough/kubernetes-nginx3.png)

Jakmile se zobrazí externí IP adresa, můžete na ni přejít pomocí prohlížeče:

  ![Obrázek přechodu na Nginx](media/container-service-kubernetes-walkthrough/kubernetes-nginx4.png)  


### <a name="browse-the-kubernetes-ui"></a>Procházení uživatelského rozhraní Kubernetes
K zobrazení webového rozhraní Kubernetes můžete použít příkaz:

```bash
kubectl proxy
```
Tento příkaz spustí v místním hostiteli ověřený proxy server, který můžete použít k zobrazení webového uživatelského rozhraní Kubernetes na adrese [http://localhost:8001/ui](http://localhost:8001/ui). Další informace najdete v tématu [Používání webového uživatelského rozhraní Kubernetes s Azure Container Service](container-service-kubernetes-ui.md).

![Obrázek řídicího panelu Kubernetes](media/container-service-kubernetes-walkthrough/kubernetes-dashboard.png)

### <a name="remote-sessions-inside-your-containers"></a>Vzdálené relace uvnitř kontejnerů
Kubernetes umožňuje spouštět příkazy ve vzdáleném kontejneru Dockeru spuštěném ve vašem clusteru.

```bash
# Get the name of your nginx pods
kubectl get pods
```

S využitím názvu podu můžete v podu spustit vzdálený příkaz. Například:

```bash
kubectl exec <pod name> date
```

Pomocí příznaků `-it` můžete také získat plně interaktivní relaci:

```bash
kubectl exec <pod name> -it bash
```

![Vzdálená relace uvnitř kontejneru](media/container-service-kubernetes-walkthrough/kubernetes-remote.png)



## <a name="next-steps"></a>Další kroky

Informace o lepším využívání clusteru Kubernetes najdete v následujících zdrojích:

* [Kubernetes Bootcamp](https://katacoda.com/embed/kubernetes-bootcamp/1/) – ukazuje, jak nasadit, škálovat, aktualizovat a ladit kontejnerizované aplikace.
* [Uživatelská příručka Kubernetes](http://kubernetes.io/docs/user-guide/) – poskytuje informace o spouštění programů v existujícím clusteru Kubernetes.
* [Příklady Kubernetes](https://github.com/kubernetes/kubernetes/tree/master/examples) – poskytuje příklady spouštění skutečných aplikací s využitím Kubernetes.

