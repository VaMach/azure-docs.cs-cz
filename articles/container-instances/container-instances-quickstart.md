---
title: "Rychlý start - vytvoření vaší první instance kontejner Azure kontejneru"
description: "Nasazení služby Azure Container Instances a zahájení práce"
services: container-instances
documentationcenter: 
author: seanmck
manager: timlt
editor: mmacy
tags: 
keywords: 
ms.assetid: 
ms.service: container-instances
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/20/2017
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: 160ba84d2c022ca3af2eb13a9689a282b4a6b198
ms.sourcegitcommit: 1d8612a3c08dc633664ed4fb7c65807608a9ee20
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/20/2017
---
# <a name="create-your-first-container-in-azure-container-instances"></a>Vytvoření prvního kontejneru ve službě Azure Container Instances
Azure instancí kontejnerů umožňuje snadno vytvářet a spravovat Docker kontejnerů v Azure, aniž by museli zřizovat virtuální počítače nebo přijmou vyšší úrovně služby. V tento rychlý start vytvořit kontejner ve službě Azure a umístěte ji do internet s veřejnou IP adresu. K dokončení této operace stačí jediný příkaz. Během několika sekund se zobrazí tato v prohlížeči:

![Aplikace nasazená pomocí služby Azure Container Instances zobrazená v prohlížeči][aci-app-browser]

Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) před tím, než začnete.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Dokončete tento rychlý start můžete použít prostředí cloudové služby Azure nebo místní instalace rozhraní příkazového řádku Azure. Pokud si zvolíte instalaci a použití rozhraní příkazového řádku místně, vyžaduje tento rychlý start, že používáte Azure CLI verze 2.0.21 nebo novější. Verzi zjistíte spuštěním příkazu `az --version`. Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI 2.0]( /cli/azure/install-azure-cli).

## <a name="create-a-resource-group"></a>Vytvoření skupiny prostředků

Služba Azure Container Instances je prostředek Azure a musí být umístěná ve skupině prostředků Azure, což je logická kolekce, ve které se nasazují a spravují prostředky Azure.

Vytvořte skupinu prostředků s [vytvořit skupinu az] [ az-group-create] příkaz.

Následující příklad vytvoří skupinu prostředků *myResourceGroup* v umístění *eastus*.

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

## <a name="create-a-container"></a>Vytvoření kontejneru

Kontejner můžete vytvořit zadáním názvu, bitovou kopii Docker a skupinu prostředků Azure k [vytvořit kontejner az] [ az-container-create] příkaz. Volitelně můžete kontejner zveřejnit na internetu s použitím veřejné IP adresy. V tento rychlý start nasadit kontejner, který je hostitelem malé webové aplikace napsané v [Node.js](http://nodejs.org).

```azurecli-interactive
az container create --name mycontainer --image microsoft/aci-helloworld --resource-group myResourceGroup --ip-address public --ports 80
```

Během několika sekund byste měli obdržet odpověď na váš požadavek. Standardně je kontejner v **vytváření** stavu, ale by se měl spustit během několika sekund. Můžete zkontrolovat stav pomocí [az kontejneru zobrazit] [ az-container-show] příkaz:

```azurecli-interactive
az container show --name mycontainer --resource-group myResourceGroup
```

V dolní části výstupu se zobrazí stav zřizování kontejneru a jeho IP adresa:

```json
...
"ipAddress": {
      "ip": "13.88.176.27",
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

Jakmile kontejneru přesune do **úspěšné** stavu, můžete dosáhnout v prohlížeči IP adresa zadaná pomocí.

![Aplikace nasazená pomocí služby Azure Container Instances zobrazená v prohlížeči][aci-app-browser]

## <a name="pull-the-container-logs"></a>Vyžádání protokolů kontejneru

Může vyžádat protokoly pro kontejner, který jste vytvořili pomocí [az kontejneru protokoly] [ az-container-logs] příkaz:

```azurecli-interactive
az container logs --name mycontainer --resource-group myResourceGroup
```

Výstup:

```bash
Server running...
10.240.255.107 - - [20/Nov/2017:19:16:28 +0000] "GET / HTTP/1.1" 200 1663 "" "Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/62.0.3202.94 Safari/537.36"
10.240.255.107 - - [20/Nov/2017:19:16:28 +0000] "GET / HTTP/1.1" 200 1663
10.240.255.107 - - [20/Nov/2017:19:16:28 +0000] "GET /favicon.ico HTTP/1.1" 404 19
```

## <a name="delete-the-container"></a>Odstranění kontejneru

Až skončíte s kontejneru, můžete odebrat pomocí [odstranit kontejner az] [ az-container-delete] příkaz:

```azurecli-interactive
az container delete --name mycontainer --resource-group myResourceGroup
```

Pokud chcete ověřit, že kontejner byl odstraněn, spusťte [az kontejneru seznamu](/cli/azure/container#az_container_list) příkaz:

```azurecli-interactive
az container list --resource-group myResourceGroup -o table
```

**Můj_kontejner** kontejneru neměla v výstup příkazu. Pokud máte ve skupině prostředků žádné jiné kontejnery, zobrazí se žádný výstup.

## <a name="next-steps"></a>Další kroky

Kód pro kontejner použitý v tento rychlý start je k dispozici [na Githubu][app-github-repo], společně s jeho soubor Docker. Pokud byste si chtěli sami vyzkoušet jeho sestavení a nasazení do služby Azure Container Instances pomocí služby Azure Container Registry, pokračujte na kurz služby Azure Container Instances.

> [!div class="nextstepaction"]
> [Kurzy služby Azure Container Instances](./container-instances-tutorial-prepare-app.md)

Můžete vyzkoušet na možnosti spuštění kontejnery v systému orchestration v Azure, najdete v článku [Service Fabric] [ service-fabric] nebo [Azure Container Service (AKS)] [ container-service] – elementy quickstart.

<!-- LINKS -->
[app-github-repo]: https://github.com/Azure-Samples/aci-helloworld.git
[az-group-create]: /cli/azure/group?view=azure-cli-latest#az_group_create
[az-container-create]: /cli/azure/container?view=azure-cli-latest#az_container_create
[az-container-delete]: /cli/azure/container?view=azure-cli-latest#az_container_delete
[az-container-list]: /cli/azure/container?view=azure-cli-latest#az_container_list
[az-container-logs]: /cli/azure/container?view=azure-cli-latest#az_container_logs
[az-container-show]: /cli/azure/container?view=azure-cli-latest#az_container_show
[service-fabric]: ../service-fabric/service-fabric-quickstart-containers.md
[container-service]: ../aks/kubernetes-walkthrough.md


<!-- IMAGES -->
[aci-app-browser]: ./media/container-instances-quickstart/aci-app-browser.png
