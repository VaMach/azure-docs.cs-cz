---
title: "Vytvoření prvního kontejneru služby Azure Container Instances | Dokumentace Azure"
description: "Nasazení služby Azure Container Instances a zahájení práce"
services: container-service
documentationcenter: 
author: seanmck
manager: timlt
editor: 
tags: 
keywords: 
ms.assetid: 
ms.service: 
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/26/2017
ms.author: seanmck
ms.custom: 
ms.translationtype: HT
ms.sourcegitcommit: 3b15d6645b988f69f1f05b27aff6f726f34786fc
ms.openlocfilehash: 933299ce5a5d6f5b2262d40ae768019ccaf8796a
ms.contentlocale: cs-cz
ms.lasthandoff: 07/26/2017

---

# <a name="create-your-first-container-in-azure-container-instances"></a>Vytvoření prvního kontejneru ve službě Azure Container Instances

Azure Container Instances zjednodušuje vytváření a správu kontejnerů v Azure. V tomto rychlém startu vytvoříte kontejner v Azure a zveřejníte ho na internetu s použitím veřejné IP adresy. K dokončení této operace stačí jediný příkaz. Během několika sekund uvidíte ve svém prohlížeči toto:

![Aplikace nasazená pomocí služby Azure Container Instances zobrazená v prohlížeči][aci-app-browser]

Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) před tím, než začnete.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Příkazy rozhraní příkazového řádku Azure Container Instances jsou aktuálně dostupné pouze ve službě Azure Cloud Shell.

## <a name="create-a-resource-group"></a>Vytvoření skupiny prostředků

Služba Azure Container Instances je prostředek Azure a musí být umístěná ve skupině prostředků Azure, což je logická kolekce, ve které se nasazují a spravují prostředky Azure.

Vytvořte skupinu prostředků pomocí příkazu [az group create](/cli/azure/group#create). 

Následující příklad vytvoří skupinu prostředků *myResourceGroup* v umístění *eastus*.

```azurecli-interactive 
az group create --name myResourceGroup --location eastus
```

## <a name="create-a-container"></a>Vytvoření kontejneru

Kontejner můžete vytvořit zadáním názvu, image Dockeru a skupiny prostředků Azure. Volitelně můžete kontejner zveřejnit na internetu s použitím veřejné IP adresy. V tomto případě použijeme kontejner, který je hostitelem velmi jednoduché webové aplikace napsané v [Node.js](http://nodejs.org).

Příkazy rozhraní příkazového řádku Azure Container Instances jsou aktuálně dostupné pouze ve službě Azure Cloud Shell.

```azurecli-interactive
az container create --name mycontainer --image microsoft/aci-helloworld --resource-group myResourceGroup --ip-address public 
```

Během několika sekund byste měli obdržet odpověď na váš požadavek. Zpočátku bude kontejner ve stavu **Vytváření**, ale během několika sekund by se měl spustit. Stav můžete zkontrolovat pomocí příkazu `show`:

```azurecli-interactive
az container show --name mycontainer --resource-group myResourceGroup
```

V dolní části výstupu se zobrazí stav zřizování kontejneru a jeho IP adresa:

```json
...
"ipAddress": {
      "ip": "13.88.8.148",
      "ports": [
        {
          "port": 80,
          "protocol": "TCP"
        }
      ]
    },
    "osType": "Linux",
    "provisioningState": "Succeeded"
...
```

Jakmile se kontejner přesune do stavu **Úspěšné**, můžete k němu přistoupit v prohlížeči pomocí zadané IP adresy. 

![Aplikace nasazená pomocí služby Azure Container Instances zobrazená v prohlížeči][aci-app-browser]

## <a name="pull-the-container-logs"></a>Vyžádání protokolů kontejneru

Protokoly pro vytvořený kontejner si můžete vyžádat pomocí příkazu `logs`:

```azurecli-interactive
az container logs --name mycontainer --resource-group myResourceGroup
```

Výstup:

```bash
listening on port 80
::ffff:10.240.255.105 - - [21/Jul/2017:00:01:46 +0000] "GET / HTTP/1.1" 200 1663 "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/59.0.3071.115 Safari/537.36"
::ffff:10.240.255.105 - - [21/Jul/2017:00:01:46 +0000] "GET /favicon.ico HTTP/1.1" 404 150 "http://104.210.39.122/" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/59.0.3071.115 Safari/537.36"
```

## <a name="delete-the-container"></a>Odstranění kontejneru

Až s kontejnerem skončíte, můžete ho odebrat pomocí příkazu `delete`:

```azurecli-interactive
az container delete --name mycontainer --resource-group myResourceGroup
```

## <a name="next-steps"></a>Další kroky

Veškerý kód pro kontejner použitý v tomto rychlém startu je k dispozici [na GitHubu][app-github-repo] společně s příslušným souborem Dockerfile. Pokud byste si chtěli sami vyzkoušet jeho sestavení a nasazení do služby Azure Container Instances pomocí služby Azure Container Registry, pokračujte na kurz služby Azure Container Instances.

> [!div class="nextstepaction"]
> [Kurzy služby Azure Container Instances](./container-instances-tutorial-prepare-app.md)


<!-- LINKS -->
[app-github-repo]: https://github.com/Azure-Samples/aci-helloworld.git

<!-- IMAGES -->
[aci-app-browser]: ./media/container-instances-quickstart/aci-app-browser.png
