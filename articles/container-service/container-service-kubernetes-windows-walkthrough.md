---
title: "Rychlý start – Azure Kubernetes cluster pro Windows | Dokumentace Microsoftu"
description: "Rychle se naučíte, jak pomocí rozhraní příkazového řádku Azure vytvářet cluster Kubernetes pro kontejnery Windows ve službě Azure Container Service."
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: acs, azure-container-service, kubernetes
keywords: 
ms.assetid: 
ms.service: container-service
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/31/2017
ms.author: danlep
ms.custom: H1Hack27Feb2017
ms.translationtype: Human Translation
ms.sourcegitcommit: 7948c99b7b60d77a927743c7869d74147634ddbf
ms.openlocfilehash: 929a4dec638da9488dd0b43fd123ed0cce77bcf3
ms.contentlocale: cs-cz
ms.lasthandoff: 06/20/2017


---

# Nasazení clusteru Kubernetes pro kontejnery Windows
<a id="deploy-kubernetes-cluster-for-windows-containers" class="xliff"></a>

Azure CLI slouží k vytváření a správě prostředků Azure z příkazového řádku nebo ve skriptech. Tato příručka podrobně popisuje použití rozhraní příkazového řádku Azure k nasazení clusteru [Kubernetes](https://kubernetes.io/docs/home/) ve službě [Azure Container Service](container-service-intro.md). Po nasazení clusteru se k němu připojíte pomocí nástroje pro příkazový řádek Kubernetes `kubectl` a nasadíte svůj první kontejner Windows.

Tento kurz vyžaduje Azure CLI verze 2.0.4 nebo novější. Verzi zjistíte spuštěním příkazu `az --version`. Pokud potřebujete upgrade, přečtěte si téma [Instalace Azure CLI 2.0]( /cli/azure/install-azure-cli). 

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/) před tím, než začnete.

> [!NOTE]
> Podpora pro kontejnery Windows v Kubernetes ve službě Azure Container Service je ve verzi Preview. 
>

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

Následující příklad vytvoří cluster s názvem *myK8sCluster* s jedním hlavním linuxovým uzlem a dvěma agentskými uzly Windows. Tento příklad vytvoří klíče SSH potřebné pro připojení k hlavnímu serveru Linux. Tento příklad používá na uzlech Windows uživatelské jméno správce *azureuser* a heslo *myPassword12*. Aktualizujte tyto hodnoty na nějaké vhodné pro vaše prostředí. 



```azurecli-interactive 
az acs create --orchestrator-type=kubernetes \
    --resource-group myResourceGroup \
    --name=myK8sCluster \
    --agent-count=2 \
    --generate-ssh-keys \
    --windows --admin-username azureuser \
    --admin-password myPassword12
```

Po několika minutách se příkaz dokončí a zobrazí vám informace o nasazení.

## Instalace kubectl
<a id="install-kubectl" class="xliff"></a>

Pokud se chcete připojit ke clusteru Kubernetes z klientského počítače, použijte klienta příkazového řádku Kubernetes [`kubectl`](https://kubernetes.io/docs/user-guide/kubectl/). 

Pokud používáte Azure CloudShell, `kubectl` je už nainstalován. Pokud ho chcete nainstalovat místně, můžete použít příkaz [az acs kubernetes install-cli](/cli/azure/acs/kubernetes#install-cli).

Následující příklad rozhraní příkazového řádku Azure nainstaluje `kubectl` do vašeho systému. V systému Windows tento příkaz spusťte jako správce.

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

## Nasazení kontejneru Windows služby IIS
<a id="deploy-a-windows-iis-container" class="xliff"></a>

Kontejner Dockeru můžete spustit v *podu* Kubernetes, který obsahuje jeden nebo více kontejnerů. 

Tento základní příklad používá soubor JSON k určení kontejneru se serverem služby Microsoft IIS a pak vytvoří pod pomocí příkazu `kubctl apply`. 

Vytvořte místní soubor `iis.json` a zkopírujte do něj následující text. Tento soubor říká Kubernetes, aby spustil službu IIS v systému Windows Server 2016 Nano Server s použitím veřejné image kontejneru z [Docker Hubu](https://hub.docker.com/r/nanoserver/iis/). Kontejner používá port 80, ale zpočátku je přístupný pouze v rámci sítě s clustery.

 ```JSON
 {
  "apiVersion": "v1",
  "kind": "Pod",
  "metadata": {
    "name": "iis",
    "labels": {
      "name": "iis"
    }
  },
  "spec": {
    "containers": [
      {
        "name": "iis",
        "image": "nanoserver/iis",
        "ports": [
          {
          "containerPort": 80
          }
        ]
      }
    ],
    "nodeSelector": {
     "beta.kubernetes.io/os": "windows"
     }
   }
 }
 ```

Pokud chcete pod spustit, zadejte:
  
```azurecli-interactive
kubectl apply -f iis.json
```  

Pokud chcete sledovat nasazení, zadejte:
  
```azurecli-interactive
kubectl get pods
```

Zatímco se pod nasazuje, je jeho stav `ContainerCreating` (Vytváření kontejneru). Přechod kontejneru do stavu `Running` (Spuštěný) může trvat několik minut.

```azurecli-interactive
NAME     READY        STATUS        RESTARTS    AGE
iis      1/1          Running       0           32s
```

## Zobrazení úvodní stránky služby IIS
<a id="view-the-iis-welcome-page" class="xliff"></a>

Pokud chcete zpřístupnit pod celému světu prostřednictvím veřejné IP adresy, zadejte následující příkaz:

```azurecli-interactive
kubectl expose pods iis --port=80 --type=LoadBalancer
```

Tento příkaz způsobí, že Kubernetes vytvoří službu a [pravidlo nástroje pro vyrovnávání zatížení Azure](container-service-kubernetes-load-balancing.md) s veřejnou IP adresou pro tuto službu. 

Spuštěním následujícího příkazu zobrazte stav služby.

```azurecli-interactive
kubectl get svc
```

Zpočátku se IP adresa bude zobrazovat jako `pending` (čekající). Po několika minutách se externí IP adresa podu `iis` nastaví takto:
  
```azurecli-interactive
NAME         CLUSTER-IP     EXTERNAL-IP     PORT(S)        AGE       
kubernetes   10.0.0.1       <none>          443/TCP        21h       
iis          10.0.111.25    13.64.158.233   80/TCP         22m
```

Pokud si chcete zobrazit úvodní stránku ISS na externí IP adrese, můžete použít libovolný webový prohlížeč:

![Obrázek přechodu na službu IIS](media/container-service-kubernetes-windows-walkthrough/kubernetes-iis.png)  


## Odstranění clusteru
<a id="delete-cluster" class="xliff"></a>
Pokud už cluster nepotřebujete, můžete k odebrání skupiny prostředků, služby kontejneru a všech souvisejících prostředků použít příkaz [az group delete](/cli/azure/group#delete).

```azurecli-interactive 
az group delete --name myResourceGroup
```


## Další kroky
<a id="next-steps" class="xliff"></a>

V tomto rychlém úvodním kurzu jste nasadili cluster Kubernetes, připojili se přes `kubectl` a nasadili pod s kontejnerem ISS. Další informace o Azure Container Service získáte v kurzu o Kubernetes.

> [!div class="nextstepaction"]
> [Správa clusteru ACS Kubernetes](./container-service-tutorial-kubernetes-prepare-app.md)

