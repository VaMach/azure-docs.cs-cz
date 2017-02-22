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
ms.date: 11/15/2016
ms.author: anhowe
translationtype: Human Translation
ms.sourcegitcommit: 7ed8fb75f057d5a7cfde5436e72e8fec52d07156
ms.openlocfilehash: f3b2fc301bf7083f192c0ec872c4e032472eef97


---

# <a name="microsoft-azure-container-service-engine---kubernetes-walkthrough"></a>Modul Microsoft Azure Container Service – názorný průvodce pro Kubernetes

## <a name="prerequisites"></a>Požadavky
Tento názorný průvodce předpokládá, že máte nainstalovaný [nástroj rozhraní příkazového řádku azure-cli](https://github.com/azure/azure-cli#installation) a vytvořili jste [veřejný klíč SSH](../virtual-machines/virtual-machines-linux-mac-create-ssh-keys.md) v `~/.ssh/id_rsa.pub`.

## <a name="overview"></a>Přehled

Pomocí pokynů uvedených níže vytvoříte cluster Kubernetes s jedním hlavním a dvěma pracovními uzly.
Hlavní uzel obsluhuje pro Kubernetes rozhraní REST API.  Pracovní uzly jsou seskupené ve skupině dostupnosti Azure a spouští vaše kontejnery. Všechny virtuální počítače jsou ve stejné virtuální privátní síti a jsou vzájemně plně přístupné.

> [!NOTE]
> Podpora pro Kubernetes je v Azure Container Service momentálně ve verzi preview.
>

Následující obrázek ukazuje architekturu clusteru kontejnerové služby s jedním hlavním uzlem a dvěma agenty:

![Obrázek clusteru Kubernetes v Azure](media/container-service-kubernetes-walkthrough/kubernetes.png)

## <a name="creating-your-kubernetes-cluster"></a>Vytvoření clusteru Kubernetes

### <a name="create-a-resource-group"></a>Vytvoření skupiny prostředků
Pokud chcete vytvořit cluster, musíte si nejdřív na konkrétním místě vytvořit skupinu prostředků. Můžete postupovat takto:
```console
RESOURCE_GROUP=my-resource-group
LOCATION=westus
az group create --name=$RESOURCE_GROUP --location=$LOCATION
```

### <a name="create-a-cluster"></a>Vytvoření clusteru
Jakmile máte skupinu prostředků, můžete v této skupině vytvořit cluster pomocí příkazů:
```console
DNS_PREFIX=some-unique-value
SERVICE_NAME=any-acs-service-name
az acs create --orchestrator-type=kubernetes --resource-group $RESOURCE_GROUP --name=$SERVICE_NAME --dns-prefix=$DNS_PREFIX
```

> [!NOTE]
> azure-cli načte `~/.ssh/id_rsa.pub` do virtuálních počítačů s Linuxem.
>

Po dokončení tohoto příkazu byste měli mít funkční cluster Kubernetes.

### <a name="configure-kubectl"></a>Konfigurace kubectl
`kubectl` je klient příkazového řádku Kubernetes.  Pokud ho ještě nemáte nainstalovaný, můžete k instalaci použít:

```console
az acs kubernetes install-cli
```

Jakmile je `kubectl` nainstalovaný, spuštění příkazu uvedeného níž stáhne hlavní konfiguraci clusteru kubernetes do souboru ~/.kube/config.
```console
az acs kubernetes get-credentials --resource-group=$RESOURCE_GROUP --name=$SERVICE_NAME
```

V tomto okamžiku byste měli být připravení přistoupit ke clusteru ze svého počítače. Zkuste spustit:
```console
kubectl get nodes
```

A ověřte, že vidíte počítače v clusteru.

## <a name="create-your-first-kubernetes-service"></a>Vytvoření první služby Kubernetes

Po dokončení tohoto názorného průvodce budete umět:
 * nasadit aplikaci v Dockeru a zpřístupnit ji světu,
 * použít `kubectl exec` ke spouštění příkazů v kontejneru a 
 * využívat přístup k řídicímu panelu Kubernetes.

### <a name="start-a-simple-container"></a>Spuštění jednoduchého kontejneru
Jednoduchý kontejner (v tomto případě webový server `nginx`) můžete spustit pomocí příkazu:

```console
kubectl run nginx --image nginx
```

Tento příkaz spustí kontejner Dockeru nginx v podu na jednom z uzlů.

Po spuštění
```console
kubectl get pods
```

uvidíte spuštěný kontejner.

### <a name="expose-the-service-to-the-world"></a>Zpřístupnění služby ostatním
Pokud chcete službu zpřístupnit ostatním,  vytvoříte Kubernetes `Service` typu `LoadBalancer`:

```console
kubectl expose deployments nginx --port=80 --type=LoadBalancer
```

To teď způsobí, že Kubernetes vytvoří Azure Load Balancer s veřejnou IP adresou. Rozšíření této změny do nástroje pro vyrovnávání zatížení trvá asi 2 až 3 minuty.

Pokud chcete sledovat změnu služby (ze stavu čekající na externí IP adresu), zadejte:
```console
watch 'kubectl get svc'
```

  ![Obrázek sledování přechodu ze stavu čekající na externí IP adresu](media/container-service-kubernetes-walkthrough/kubernetes-nginx3.png)

Jakmile se zobrazí externí IP adresa, můžete na ni přejít pomocí prohlížeče:

  ![Obrázek přechodu na nginx](media/container-service-kubernetes-walkthrough/kubernetes-nginx4.png)  


### <a name="browse-the-kubernetes-ui"></a>Procházení uživatelského rozhraní Kubernetes
K zobrazení webového rozhraní Kubernetes můžete použít příkaz:

```console
kubectl proxy
```
Spustí v místním hostiteli jednoduchý ověřený proxy server, který můžete použít k zobrazení [uživatelského rozhraní Kubernetes](http://localhost:8001/ui).

![Obrázek řídicího panelu Kubernetes](media/container-service-kubernetes-walkthrough/kubernetes-dashboard.png)

### <a name="remote-sessions-inside-your-containers"></a>Vzdálené relace uvnitř kontejnerů
Kubernetes umožňuje spouštět příkazy ve vzdáleném kontejneru Dockeru spuštěném ve vašem clusteru.

```console
# Get the name of your nginx pod
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

![Obrázek nástroje curl pro podIP](media/container-service-kubernetes-walkthrough/kubernetes-remote.png)


## <a name="details"></a>Podrobnosti
### <a name="installing-the-kubectl-configuration-file"></a>Instalace konfiguračního souboru kubectl
Když jste spustili `az acs kubernetes get-credentials`, konfigurační soubor kube pro vzdálený přístup se uložil do domovského adresáře ~/.kube/config.

Pokud byste ho potřebovali stáhnout přímo, můžete využít `ssh` v Linuxu nebo OS X, nebo `Putty` ve Windows:

#### <a name="windows"></a>Windows
Pokud chcete použít pscp z [putty](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html),  zajistěte, aby váš certifikát byl dostupný prostřednictvím modulu [pageant](https://github.com/Azure/acs-engine/blob/master/docs/ssh.md#key-management-and-agent-forwarding-with-windows-pageant):
  ```
  # MASTERFQDN is obtained in step1
  pscp azureuser@MASTERFQDN:.kube/config .
  SET KUBECONFIG=%CD%\config
  kubectl get nodes
  ```

#### <a name="os-x-or-linux"></a>OS X nebo Linux:
  ```
  # MASTERFQDN is obtained in step1
  scp azureuser@MASTERFQDN:.kube/config .
  export KUBECONFIG=`pwd`/config
  kubectl get nodes
  ```
## <a name="learning-more"></a>Další informace

### <a name="azure-container-service"></a>Azure Container Service

1. [Dokumentace ke službě Azure Container Service](https://azure.microsoft.com/en-us/documentation/services/container-service/)
2. [Opensourcový modul služby Azure Container Service](https://github.com/azure/acs-engine)

### <a name="kubernetes-community-documentation"></a>Dokumentace komunity Kubernetes

1. [Kubernetes Bootcamp](https://katacoda.com/embed/kubernetes-bootcamp/1/) – ukazuje, jak nasadit, škálovat, aktualizovat a ladit kontejnerizované aplikace.
2. [Uživatelská příručka Kubernetes](http://kubernetes.io/docs/user-guide/) – poskytuje informace o spouštění programů v existujícím clusteru Kubernetes.
3. [Příklady Kubernetes](https://github.com/kubernetes/kubernetes/tree/master/examples) – poskytuje řadu příkladů spouštění skutečných aplikací s využitím Kubernetes.



<!--HONumber=Jan17_HO4-->


