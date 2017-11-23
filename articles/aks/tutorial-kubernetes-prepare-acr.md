---
title: "Kubernetes na kurz pro Azure – Příprava ACR | Microsoft Docs"
description: "Kurz AKS – Příprava ACR"
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
ms.date: 11/11/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 1848e15a2be8d89315657a6eabdb94617bd1b5bf
ms.sourcegitcommit: 8aa014454fc7947f1ed54d380c63423500123b4a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/23/2017
---
# <a name="deploy-and-use-azure-container-registry"></a>Nasazení a používání Azure kontejneru registru

Azure kontejneru registru (ACR) je založené na Azure, privátní registru, pro kontejner Docker bitové kopie. Tato dvě tutoriál, část z osm, provede nasazení instanci Azure Container registru a když zavedete bitovou kopii kontejneru. Dokončit krokům patří:

> [!div class="checklist"]
> * Nasazení instanci Azure Container registru (ACR)
> * Označování bitovou kopii kontejner pro ACR
> * Nahrávání bitovou kopii do ACR

V následujících kurzech této instance ACR je integrovaná do clusteru s podporou Kubernetes v AKS.

## <a name="before-you-begin"></a>Než začnete

V [předchozí kurzu](./tutorial-kubernetes-prepare-app.md), bitovou kopii kontejner byl vytvořen pro jednoduchou aplikaci Azure hlasování. Pokud jste ještě nevytvořili obrázek aplikace Azure hlasování, vrátit [kurzu 1 – Vytvoření kontejneru image](./tutorial-kubernetes-prepare-app.md).

Tento kurz vyžaduje, že používáte Azure CLI verze 2.0.21 nebo novější. Verzi zjistíte spuštěním příkazu `az --version`. Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI]( /cli/azure/install-azure-cli).

## <a name="deploy-azure-container-registry"></a>Nasadit kontejner Azure registru

Pokud nasazujete registru kontejneru služby Azure, musíte nejprve skupinu prostředků. Skupina prostředků Azure je logický kontejner, ve kterém se nasazují a spravují prostředky Azure.

Vytvořte skupinu prostředků pomocí příkazu [az group create](/cli/azure/group#create). V tomto příkladu skupinu prostředků s názvem `myResourceGroup` je vytvořen v `eastus` oblast.

```azurecli
az group create --name myResourceGroup --location eastus
```

Vytvoření kontejneru Azure registr s využitím [az acr vytvořit](/cli/azure/acr#create) příkaz. Název kontejneru registru **musí být jedinečné**.

```azurecli
az acr create --resource-group myResourceGroup --name <acrName> --sku Basic
```

Po celý zbytek v tomto kurzu používáme `<acrName>` jako zástupný symbol pro název kontejneru registru.

## <a name="container-registry-login"></a>Kontejner registru přihlášení

Použití [az acr přihlášení](https://docs.microsoft.com/cli/azure/acr#az_acr_login) příkaz k přihlášení do ACR instance. Je třeba zadat jedinečný název zadané registru kontejneru v okamžiku vytvoření.

```azurecli
az acr login --name <acrName>
```

Příkaz vrátí zprávu, byla úspěšná přihlášení po dokončení.

## <a name="tag-container-images"></a>Značka kontejneru obrázků

Chcete-li zobrazit seznam aktuální bitové kopie, použijte [imagí dockeru](https://docs.docker.com/engine/reference/commandline/images/) příkaz.

```console
docker images
```

Výstup:

```
REPOSITORY                   TAG                 IMAGE ID            CREATED             SIZE
azure-vote-front             latest              4675398c9172        13 minutes ago      694MB
redis                        latest              a1b99da73d05        7 days ago          106MB
tiangolo/uwsgi-nginx-flask   flask               788ca94b2313        9 months ago        694MB
```

Každé bitové kopie kontejneru musí být označené loginServer název registru. Tato značka se používá pro směrování při nabízení kontejneru bitové kopie do registru bitovou kopii.

Získat název loginServer, spusťte následující příkaz.

```azurecli
az acr list --resource-group myResourceGroup --query "[].{acrLoginServer:loginServer}" --output table
```

Nyní, označit `azure-vote-front` bitovou kopii s loginServer registru kontejneru. Navíc přidat `:redis-v1` na konec název bitové kopie. Tato značka označuje verzi bitové kopie.

```console
docker tag azure-vote-front <acrLoginServer>/azure-vote-front:redis-v1
```

Jakmile příznakem, spustit [imagí dockeru] (https://docs.docker.com/engine/reference/commandline/images/) k ověření operaci.

```console
docker images
```

Výstup:

```
REPOSITORY                                           TAG                 IMAGE ID            CREATED             SIZE
azure-vote-front                                     latest              eaf2b9c57e5e        8 minutes ago       716 MB
mycontainerregistry082.azurecr.io/azure-vote-front   redis-v1            eaf2b9c57e5e        8 minutes ago       716 MB
redis                                                latest              a1b99da73d05        7 days ago          106MB
tiangolo/uwsgi-nginx-flask                           flask               788ca94b2313        8 months ago        694 MB
```

## <a name="push-images-to-registry"></a>Push bitové kopie do registru.

Push `azure-vote-front` bitovou kopii do registru.

Pomocí následujícího příkladu, nahraďte název ACR loginServer loginServer ze svého prostředí.

```console
docker push <acrLoginServer>/azure-vote-front:redis-v1
```

Tato akce trvá několik minut na dokončení.

## <a name="list-images-in-registry"></a>Seznam obrázků v registru

K zobrazení seznamu bitové kopie, které se nabídne do vašeho kontejneru Azure registru uživatele [az acr úložiště seznamu](/cli/azure/acr/repository#list) příkaz. Aktualizujte příkaz s názvem instance ACR.

```azurecli
az acr repository list --name <acrName> --output table
```

Výstup:

```azurecli
Result
----------------
azure-vote-front
```

A pak najdete v části značky pro konkrétní image, pomocí [az acr úložiště zobrazit značky](/cli/azure/acr/repository#show-tags) příkaz.

```azurecli
az acr repository show-tags --name <acrName> --repository azure-vote-front --output table
```

Výstup:

```azurecli
Result
--------
redis-v1
```

V kurzu dokončení bitovou kopii kontejneru byla uložena v privátní instanci Azure Container registru. V následujících kurzech je nasadit tuto bitovou kopii z ACR do clusteru s podporou Kubernetes.

## <a name="next-steps"></a>Další kroky

V tomto kurzu registru kontejner Azure připravené pro použití v clusteru služby AKS. Dokončili jste následující kroky:

> [!div class="checklist"]
> * Nasazení instanci Azure Container registru
> * Označí image kontejner pro ACR
> * Odeslat bitovou kopii do ACR

Přechodu na v dalším kurzu se dozvíte o nasazení clusteru s podporou Kubernetes v Azure.

> [!div class="nextstepaction"]
> [Nasazení clusteru Kubernetes](./tutorial-kubernetes-deploy-cluster.md)
