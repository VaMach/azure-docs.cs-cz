---
title: "Rychlý start - vytvořit privátní registru Docker v Azure pomocí rozhraní příkazového řádku Azure"
description: "Rychle se Naučte se vytvořit privátní registru kontejner Docker pomocí Azure CLI."
services: container-registry
documentationcenter: 
author: neilpeterson
manager: timlt
editor: tysonn
tags: 
keywords: 
ms.assetid: 29e20d75-bf39-4f7d-815f-a2e47209be7d
ms.service: container-registry
ms.devlang: azurecli
ms.topic: quicksart
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/16/2017
ms.author: nepeters
ms.custom: H1Hack27Feb2017, mvc
ms.openlocfilehash: 6804a3e6933a467fc57b960676ada800a9ed22a4
ms.sourcegitcommit: 7136d06474dd20bb8ef6a821c8d7e31edf3a2820
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/05/2017
---
# <a name="create-a-container-registry-using-the-azure-cli"></a>Vytvoření registru kontejnerů pomocí Azure CLI

Azure Container Registry je spravovaná služba registru kontejnerů Dockeru sloužící k ukládání privátních imagí kontejnerů Dockeru. Tato příručka údaje vytvoření instance registru kontejner Azure pomocí rozhraní příkazového řádku Azure.

Tento rychlý start vyžaduje, že používáte Azure CLI verze 2.0.20 nebo novější. Verzi zjistíte spuštěním příkazu `az --version`. Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI 2.0](/cli/azure/install-azure-cli).

Je také nutné mít Docker nainstalovány místně. Docker nabízí balíčky pro snadnou konfiguraci Dockeru na jakémkoli systému [Mac](https://docs.docker.com/docker-for-mac/), [Windows](https://docs.docker.com/docker-for-windows/) nebo [Linux](https://docs.docker.com/engine/installation/#supported-platforms).

## <a name="create-a-resource-group"></a>Vytvoření skupiny prostředků

Vytvořte skupinu prostředků pomocí příkazu [az group create](/cli/azure/group#create). Skupina prostředků Azure je logický kontejner, ve kterém se nasazují a spravují prostředky Azure.

Následující příklad vytvoří skupinu prostředků *myResourceGroup* v umístění *eastus*.

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

## <a name="create-a-container-registry"></a>Vytvoření registru kontejnerů

V tento rychlý start, vytvoříme *základní* registru. Azure kontejneru registru je k dispozici v několika různých SKU stručně popsány v následující tabulce. Rozšířené informace o jednotlivých v [kontejneru registru SKU](container-registry-skus.md).

[!INCLUDE [container-registry-sku-matrix](../../includes/container-registry-sku-matrix.md)]

Pomocí příkazu [az acr create](/cli/azure/acr#create) vytvořte instanci služby ACR.

Název registru **musí být jedinečné**. V následujícím příkladu *myContainerRegistry007* se používá. Aktualizujte na jedinečnou hodnotu.

```azurecli
az acr create --name myContainerRegistry007 --resource-group myResourceGroup --sku Basic
```

Po vytvoření registru je výstup podobný tomuto:

```json
{
  "adminUserEnabled": false,
  "creationDate": "2017-09-08T22:32:13.175925+00:00",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.ContainerRegistry/registries/myContainerRegistry007",
  "location": "eastus",
  "loginServer": "myContainerRegistry007.azurecr.io",
  "name": "myContainerRegistry007",
  "provisioningState": "Succeeded",
  "resourceGroup": "myResourceGroup",
  "sku": {
    "name": "Basic",
    "tier": "Basic"
  },
  "storageAccount": {
    "name": "mycontainerregistr223140"
  },
  "tags": {},
  "type": "Microsoft.ContainerRegistry/registries"
}
```

Po celý zbytek tento rychlý start, používáme `<acrname>` jako zástupný symbol pro název kontejneru registru.

## <a name="log-in-to-acr"></a>Přihlaste se k ACR

Před odesíláním a vyžadováním imagí kontejnerů se musíte přihlásit k instanci služby ACR. K tomu použijte příkaz [az acr login](/cli/azure/acr#login).

```azurecli
az acr login --name <acrname>
```

Příkaz po dokončení vrátí zprávu Login Succeeded (Přihlášení proběhlo úspěšně).

## <a name="push-image-to-acr"></a>Nabízená ACR bitové kopie

Tak, aby nabízel bitovou kopii registru kontejneru Azure, musíte nejprve mít bitovou kopii. V případě potřeby spusťte následující příkaz načítat z úložiště Docker Hub předem vytvořené bitové kopie.

```bash
docker pull microsoft/aci-helloworld
```

Obrázek musí být označené název ACR přihlášení serveru. Spusťte následující příkaz, který vrátí název serveru přihlášení instance ACR.

```azurecli
az acr list --resource-group myResourceGroup --query "[].{acrLoginServer:loginServer}" --output table
```

Značka pomocí bitové kopie [docker značky](https://docs.docker.com/engine/reference/commandline/tag/) příkaz. Nahraďte  *<acrLoginServer>*  s přihlašovacím jménem serveru vaší instance ACR.

```bash
docker tag microsoft/aci-helloworld <acrLoginServer>/aci-helloworld:v1
```

Nakonec použijte [docker nabízené](https://docs.docker.com/engine/reference/commandline/push/) tak, aby nabízel bitovou kopii k instanci ACR. Nahraďte  *<acrLoginServer>*  s přihlašovacím jménem serveru vaší instance ACR.

```bash
docker push <acrLoginServer>/aci-helloworld:v1
```

## <a name="list-container-images"></a>Výpis imagí kontejnerů

Následující příklad uvádí úložiště v registru:

```azurecli
az acr repository list -n <acrname> -o table
```

Výstup:

```bash
Result
----------------
aci-helloworld
```

Následující příklad vypíše značky na **aci helloworld** úložiště.

```azurecli
az acr repository show-tags -n <acrname> --repository aci-helloworld -o table
```

Výstup:

```bash
Result
--------
v1
```

## <a name="clean-up-resources"></a>Vyčištění prostředků

Pokud již nepotřebujete, můžete použít [odstranění skupiny az](/cli/azure/group#delete) příkaz k odebrání skupiny prostředků, ACR instanci a všechny Image kontejneru.

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Další kroky

V tento rychlý start vytvořili kontejner registru Azure pomocí Azure CLI. Pokud chcete používat registru kontejner Azure s Azure kontejner instancí, nadále kurzu instancí kontejnerů Azure.

> [!div class="nextstepaction"]
> [Kurz pro Azure instancí kontejnerů](../container-instances/container-instances-tutorial-prepare-app.md)