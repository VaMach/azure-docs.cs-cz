---
title: "Nasazení clusteru kontejneru Dockeru – Azure CLI | Dokumentace Microsoftu"
description: "Nasazení řešení Kubernetes, DC/OS nebo Docker Swarm ve službě Azure Container Service pomocí Azure CLI 2.0"
services: container-service
documentationcenter: 
author: sauryadas
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: 
ms.assetid: 8da267e8-2aeb-4c24-9a7a-65bdca3a82d6
ms.service: container-service
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/01/2017
ms.author: saudas
ms.custom: H1Hack27Feb2017
translationtype: Human Translation
ms.sourcegitcommit: 2464901d22bb91cbf396ef60f4bda6d979b578b7
ms.openlocfilehash: a0cbf24c2e2f1e6f3a1d2097e6146c09b4eee4fe
ms.lasthandoff: 03/02/2017


---
# <a name="deploy-a-docker-container-hosting-solution-using-the-azure-cli-20"></a>Nasazení řešení hostování kontejnerů Dockeru pomocí Azure CLI 2.0

Pomocí příkazů `az acs` v Azure CLI 2.0 můžete vytvořit a spravovat clustery ve službě Azure Container Service. Cluster Azure Container Service můžete také nasadit pomocí webu [Azure Portal](container-service-deployment.md) nebo rozhraní API služby Azure Container Service.

Nápovědu k příkazům `az acs` získáte předáním parametru `-h` příslušnému příkazu. Například: `az acs create -h`.



## <a name="prerequisites"></a>Požadavky
K vytvoření clusteru Azure Container Service pomocí Azure CLI 2.0 musíte mít:
* účet Azure ([získejte bezplatnou zkušební verzi](https://azure.microsoft.com/pricing/free-trial/)),
* nainstalované a nastavené [Azure CLI 2.0](/cli/azure/install-az-cli2)

## <a name="get-started"></a>Začínáme 
### <a name="log-in-to-your-account"></a>Přihlášení k účtu
```azurecli
az login 
```

Postupujte podle zobrazených výzev a interaktivně se přihlaste. Další způsoby přihlášení najdete v tématu [Začínáme s Azure CLI 2.0](/cli/azure/get-started-with-az-cli2).

### <a name="set-your-azure-subscription"></a>Nastavení předplatného Azure

Pokud máte více než jedno předplatné Azure, nastavte výchozí předplatné. Například:

```
az account set --subscription "f66xxxxx-xxxx-xxxx-xxx-zgxxxx33cha5"
```


### <a name="create-a-resource-group"></a>Vytvoření skupiny prostředků
Doporučujeme vytvořit skupinu prostředků pro každý cluster. Zadejte oblast Azure, ve které je Azure Container Service [k dispozici](https://azure.microsoft.com/en-us/regions/services/). Například:

```azurecli
az group create -n acsrg1 -l "westus"
```
Výstup je podobný tomuto:

![Vytvoření skupiny prostředků](media/container-service-create-acs-cluster-cli/rg-create.png)


## <a name="create-an-azure-container-service-cluster"></a>Vytvoření clusteru Azure Container Service

K vytvoření clusteru použijte příkaz `az acs create`.
Název clusteru a název skupiny prostředků vytvořené v předchozím kroku jsou povinné parametry. 

Ostatní vstupy jsou nastavené na výchozí hodnoty (viz následující obrazovku), pokud nejsou přepsané pomocí příslušných přepínačů. Například orchestrátor je standardně nastaven na DC/OS. A pokud nezadáte předponu názvu DNS, vytvoří se na základě názvu clusteru.

![použití příkazu az acs create](media/container-service-create-acs-cluster-cli/create-help.png)


### <a name="quick-acs-create-using-defaults"></a>Rychlý příkaz `acs create` s využitím výchozích hodnot
Pokud máte soubor s veřejným klíčem SSH RSA `id_rsa.pub` ve výchozím umístění (nebo jste jej vytvořili pro [OS X a Linux](../virtual-machines/virtual-machines-linux-mac-create-ssh-keys.md) nebo [Windows](../virtual-machines/virtual-machines-linux-ssh-from-windows.md)), použijte příkaz podobný tomuto:

```azurecli
az acs create -n acs-cluster -g acsrg1 -d applink789
```
Pokud veřejný klíč SSH nemáte, použijte druhý příkaz. Tento příkaz ho pro vás pomocí přepínače `--generate-ssh-keys` vytvoří.

```azurecli
az acs create -n acs-cluster -g acsrg1 -d applink789 --generate-ssh-keys
```

Po zadání příkazu počkejte asi 10 minut, než se cluster vytvoří. Výstup příkazu zahrnuje plně kvalifikované názvy domén hlavních i agentských uzlů a příkaz SSH pro připojení k prvnímu hlavnímu uzlu. Tady je zkrácený výstup:

![Obrázek s příkazem ACS create](media/container-service-create-acs-cluster-cli/cluster-create.png)

> [!TIP]
> [Názorný průvodce pro Kubernetes](container-service-kubernetes-walkthrough.md) ukazuje, jak pomocí příkazu `az acs create` s použitím výchozích hodnot vytvořit cluster Kubernetes.
>

## <a name="manage-acs-clusters"></a>Správa clusterů ACS

Cluster můžete spravovat pomocí dalších příkazů `az acs`. Zde je několik příkladů:

### <a name="list-clusters-under-a-subscription"></a>Výpis clusterů v rámci předplatného

```azurecli
az acs list --output table
```

### <a name="list-clusters-in-a-resource-group"></a>Výpis clusterů ve skupině prostředků

```azurecli
az acs list -g acsrg1 --output table
```

![příkaz acs list](media/container-service-create-acs-cluster-cli/acs-list.png)


### <a name="display-details-of-a-container-service-cluster"></a>Zobrazení podrobností o clusteru Container Service

```azurecli
az acs show -g acsrg1 -n acs-cluster --output list
```

![příkaz acs show](media/container-service-create-acs-cluster-cli/acs-show.png)


### <a name="scale-the-cluster"></a>Škálování clusteru
Je povolené horizontální navýšení i snížení kapacity agentských uzlů. Parametr `new-agent-count` určuje nový počet agentů v clusteru ACS.

```azurecli
az acs scale -g acsrg1 -n acs-cluster --new-agent-count 4
```

![příkaz acs scale](media/container-service-create-acs-cluster-cli/acs-scale.png)

## <a name="delete-a-container-service-cluster"></a>Odstranění clusteru služby Container Service
```azurecli
az acs delete -g acsrg1 -n acs-cluster 
```
Tento příkaz neodstraní všechny prostředky (sítě a úložiště), které byly vytvořené během vytváření služby Container Service. Pokud chcete jednoduše odstranit všechny prostředky, doporučujeme, abyste každý cluster nasadili do jiné skupiny prostředků. Když už cluster nebudete potřebovat, odstraňte příslušnou skupinu prostředků.

## <a name="next-steps"></a>Další kroky
Nyní když máte funkční cluster, nahlédněte do těchto dokumentů, kde naleznete podrobnosti týkající se připojení a správy:

* [Připojení ke clusteru Azure Container Service](container-service-connect.md)
* [Práce se službou Azure Container Service a DC/OS](container-service-mesos-marathon-rest.md)
* [Práce se službou Azure Container Service a nástrojem Docker Swarm](container-service-docker-swarm.md)
* [Práce s Azure Container Service a Kubernetes](container-service-kubernetes-walkthrough.md)
