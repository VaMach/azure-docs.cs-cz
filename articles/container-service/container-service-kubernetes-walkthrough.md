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
ms.date: 04/05/2017
ms.author: anhowe
ms.custom: H1Hack27Feb2017
translationtype: Human Translation
ms.sourcegitcommit: 538f282b28e5f43f43bf6ef28af20a4d8daea369
ms.openlocfilehash: 5c529ae41b42d276d37e6103305e33ed04694e18
ms.lasthandoff: 04/07/2017

---

# <a name="get-started-with-a-kubernetes-cluster-in-container-service"></a>Zahájení práce s clusterem Kubernetes ve službě Container Service


Tento názorný postup ukazuje, jak používat příkazy rozhraní příkazového řádku Azure CLI 2.0 k vytvoření clusteru Kubernetes ve službě Azure Container Service. Následně můžete pomocí nástroje příkazového řádku `kubectl` začít pracovat s kontejnery v clusteru.

Následující obrázek ukazuje architekturu clusteru služby Container Service s jedním hlavním uzlem a dvěma agenty. Hlavní uzel obsluhuje pro Kubernetes rozhraní REST API. Agentské uzly jsou seskupené ve skupině dostupnosti Azure a spouští vaše kontejnery. Všechny virtuální počítače jsou ve stejné privátní virtuální síti a jsou vzájemně plně přístupné.

![Obrázek clusteru Kubernetes v Azure](media/container-service-kubernetes-walkthrough/kubernetes.png)

## <a name="prerequisites"></a>Požadavky
Tento názorný průvodce předpokládá, že máte nainstalované a nastavené rozhraní [Azure CLI 2.0](/cli/azure/install-az-cli2). 

U příkladů příkazů se předpokládá, že spouštíte rozhraní Azure CLI v prostředí bash, které je běžné v systémech Linux a macOS. Pokud spustíte Azure CLI na klientovi Windows, syntaxe skriptování a souborů se může lišit v závislosti na příkazovém prostředí. 

## <a name="create-your-kubernetes-cluster"></a>Vytvoření clusteru Kubernetes

Zde je popsáno vytvoření clusteru pomocí stručných příkazů prostředí v Azure CLI 2.0. 

### <a name="create-a-resource-group"></a>Vytvoření skupiny prostředků
Pokud chcete vytvořit cluster, musíte si nejprve v konkrétním umístění vytvořit skupinu prostředků. Spusťte příkazy podobné těmto:

```azurecli
RESOURCE_GROUP=my-resource-group
LOCATION=westus
az group create --name=$RESOURCE_GROUP --location=$LOCATION
```

### <a name="create-a-cluster"></a>Vytvoření clusteru
Jakmile máte skupinu prostředků, můžete v této skupině vytvořit cluster. Následující příklad používá možnost `--generate-ssh-keys`, která generuje soubory veřejného a soukromého klíče SSH potřebné pro nasazení, pokud dosud neexistují ve výchozím adresáři `~/.ssh/`. 

Tento příkaz také automaticky vygeneruje [instanční objekt služby Azure Active Directory](container-service-kubernetes-service-principal.md), který cluster Kubernetes v Azure používá.

```azurecli
DNS_PREFIX=some-unique-value
CLUSTER_NAME=any-acs-cluster-name
az acs create --orchestrator-type=kubernetes --resource-group $RESOURCE_GROUP --name=$CLUSTER_NAME --dns-prefix=$DNS_PREFIX --generate-ssh-keys
```


Po několika minutách se příkaz dokončí a měli byste mít funkční cluster Kubernetes.

### <a name="connect-to-the-cluster"></a>Připojení ke clusteru

Pokud se připojujete ke clusteru Kubernetes z klientského počítače, použijte klienta příkazového řádku Kubernetes [`kubectl`](https://kubernetes.io/docs/user-guide/kubectl/). 

Pokud `kubectl` ještě nemáte nainstalovaného, můžete k instalaci použít:

```azurecli
sudo az acs kubernetes install-cli
```
> [!TIP]
> Ve výchozím nastavení tento příkaz nainstaluje binární soubor `kubectl` do adresáře `/usr/local/bin/kubectl` v systému Linux nebo macOS, nebo do adresáře `C:\Program Files (x86)\kubectl.exe` ve Windows. Pokud chcete zadat jinou instalační cestu, použijte parametr `--install-location`.
>

Po instalaci `kubectl` se ujistěte, že jeho adresář je v systémové cestě, nebo ho tam přidejte. 


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

### <a name="start-a-simple-container"></a>Spuštění jednoduchého kontejneru
Jednoduchý kontejner (v tomto případě webový server Nginx) můžete spustit pomocí příkazu:

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
Tento příkaz spustí v místním hostiteli jednoduchý ověřený proxy server, který můžete použít k zobrazení webového uživatelského rozhraní Kubernetes běžícího na adrese [http://localhost:8001/ui](http://localhost:8001/ui). Další informace najdete v tématu [Používání webového uživatelského rozhraní Kubernetes s Azure Container Service](container-service-kubernetes-ui.md).

![Obrázek řídicího panelu Kubernetes](media/container-service-kubernetes-walkthrough/kubernetes-dashboard.png)

### <a name="remote-sessions-inside-your-containers"></a>Vzdálené relace uvnitř kontejnerů
Kubernetes umožňuje spouštět příkazy ve vzdáleném kontejneru Dockeru spuštěném ve vašem clusteru.

```bash
# Get the name of your nginx pods
kubectl get pods
```

S využitím názvu podu můžete v podu spustit vzdálený příkaz.  Například:

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

