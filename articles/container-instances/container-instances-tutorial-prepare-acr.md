---
title: "Kurz služby Azure Container Instances – Příprava služby Azure Container Registry"
description: "Kurz služby Azure Container Instances, část 2 ze 3 – Příprava služby Azure Container Registry"
services: container-instances
author: neilpeterson
manager: timlt
ms.service: container-instances
ms.topic: tutorial
ms.date: 01/02/2018
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: 94ecba44b8281460da4518c146aab814d2eaa850
ms.sourcegitcommit: 1fbaa2ccda2fb826c74755d42a31835d9d30e05f
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/22/2018
---
# <a name="deploy-and-use-azure-container-registry"></a>Nasazení a použití služby Azure Container Registry

Toto je druhá část třídílného kurzu. V [předchozím kroku](container-instances-tutorial-prepare-app.md) byla vytvořena image kontejneru pro jednoduchou webovou aplikaci napsanou v prostředí [Node.js][nodejs]. V tomto kurzu nahrajete image do služby Azure Container Registry. Pokud jste image kontejneru ještě nevytvořili, vraťte se ke [kurzu 1 – Vytvoření image kontejneru](container-instances-tutorial-prepare-app.md).

Azure Container Registry je privátní registr prostředí Azure pro image kontejneru Dockeru. Tento kurz vás provede nasazením instance služby Azure Container Registry a nahráním image kontejneru do ní.

V tomto článku, který je druhou částí série, se naučíte:

> [!div class="checklist"]
> * Nasadit instanci služby Azure Container Registry
> * Označit image kontejneru pro svůj registr kontejneru Azure
> * Odeslat image do svého registru

V následujícím článku, který je posledním dílem série, nasadíte kontejner z privátního registru do služby Azure Container Instances.

## <a name="before-you-begin"></a>Než začnete

Tento kurz vyžaduje použití Azure CLI verze 2.0.23 nebo novější. Verzi zjistíte spuštěním příkazu `az --version`. Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI 2.0][azure-cli-install].

K dokončení tohoto kurzu potřebujete vývojové prostředí pro Docker nainstalované místně. Docker nabízí balíčky pro snadnou konfiguraci Dockeru na jakémkoli [Macu][docker-mac] nebo systému [Windows][docker-windows] či [Linux][docker-linux].

Azure Cloud Shell neobsahuje součásti Dockeru nutné pro dokončení všech kroků v tomto kurzu. Je nutné nainstalovat do místního počítače rozhraní příkazového řádku Azure a vývojové prostředí Dockeru, aby bylo možné tento kurz dokončit.

## <a name="deploy-azure-container-registry"></a>Nasazení služby Azure Container Registry

Pokud chcete nasadit službu Azure Container Registry, nejprve potřebujete skupinu prostředků. Skupina prostředků Azure je logická kolekce, ve které se nasazují a spravují prostředky Azure.

Vytvořte skupinu prostředků pomocí příkazu [az group create][az-group-create]. V tomto příkladu se vytvoří skupina prostředků s názvem *myResourceGroup* v oblasti *eastus*.

```azurecli
az group create --name myResourceGroup --location eastus
```

Pomocí příkazu [az acr create][az-acr-create] vytvořte registr kontejneru Azure. Název registru kontejneru musí být v rámci prostředí Azure jedinečný a musí obsahovat 5 až 50 alfanumerických znaků. Nahraďte položku `<acrName>` jedinečným názvem pro svůj registr:

```azurecli
az acr create --resource-group myResourceGroup --name <acrName> --sku Basic
```

Příklad – vytvoření registru kontejneru Azure s názvem *mycontainerregistry082*:

```azurecli
az acr create --resource-group myResourceGroup --name mycontainerregistry082 --sku Basic --admin-enabled true
```

V celé zbývající části tohoto kurzu používáme položku `<acrName>` jako zástupný symbol pro název registru kontejneru, který jste zvolili.

## <a name="container-registry-login"></a>Přihlášení k registru kontejneru

Před nahráváním imagí do instance služby Azure Container Registry se k ní musíte přihlásit. Dokončete operaci pomocí příkazu [az acr login][az-acr-login]. Je třeba uvést jedinečný název, který jste zadali pro registr kontejneru při jeho vytváření.

```azurecli
az acr login --name <acrName>
```

Příkaz po dokončení vrátí zprávu `Login Succeeded` (Přihlášení bylo úspěšné).

## <a name="tag-container-image"></a>Označení image kontejneru

Pokud chcete nasadit image kontejneru z privátního registru, je třeba označit image názvem `loginServer` registru.

Seznam aktuálních imagí můžete zobrazit pomocí příkazu [docker images][docker-images].

```bash
docker images
```

Výstup:

```bash
REPOSITORY                   TAG                 IMAGE ID            CREATED              SIZE
aci-tutorial-app             latest              5c745774dfa9        39 seconds ago       68.1 MB
```

Zjistěte název loginServer spuštěním příkazu [az acr show][az-acr-show]. Nahraďte položku `<acrName>` názvem svého registru kontejneru.

```azurecli
az acr show --name <acrName> --query loginServer --output table
```

Příklad výstupu:

```
Result
------------------------
mycontainerregistry082.azurecr.io
```

Označte image *aci-tutorial-app* názvem loginServer svého registru kontejneru. Na konec názvu image také přidejte řetězec `:v1`. Tato značka označuje číslo verze image. Nahraďte položku `<acrLoginServer>` výsledkem příkazu [az acr show] [ az-acr-show], který jste právě spustili.

```bash
docker tag aci-tutorial-app <acrLoginServer>/aci-tutorial-app:v1
```

Po označení operaci ověřte spuštěním příkazu `docker images`.

```bash
docker images
```

Výstup:

```bash
REPOSITORY                                                TAG                 IMAGE ID            CREATED             SIZE
aci-tutorial-app                                          latest              5c745774dfa9        39 seconds ago      68.1 MB
mycontainerregistry082.azurecr.io/aci-tutorial-app        v1                  a9dace4e1a17        7 minutes ago       68.1 MB
```

## <a name="push-image-to-azure-container-registry"></a>Nahrání image do služby Azure Container Registry

Nahrajte image *aci-tutorial-app* do registru příkazem [docker push][docker-push]. Nahraďte položku `<acrLoginServer>` úplným názvem serveru pro přihlášení získaným v dřívějším kroku.

```bash
docker push <acrLoginServer>/aci-tutorial-app:v1
```

Operace `push` by měla trvat několik sekund až několik minut v závislosti na připojení k internetu a výstup je podobný následujícímu:

```bash
The push refers to a repository [mycontainerregistry082.azurecr.io/aci-tutorial-app]
3db9cac20d49: Pushed
13f653351004: Pushed
4cd158165f4d: Pushed
d8fbd47558a8: Pushed
44ab46125c35: Pushed
5bef08742407: Pushed
v1: digest: sha256:ed67fff971da47175856505585dcd92d1270c3b37543e8afd46014d328f05715 size: 1576
```

## <a name="list-images-in-azure-container-registry"></a>Vypsání seznamu imagí ve službě Azure Container Registry

Pokud chcete vrátit seznam imagí, které byly nahrány do vašeho registru kontejneru Azure, použijte příkaz [az acr repository list][az-acr-repository-list]. Aktualizujte příkaz zadáním názvu registru kontejneru.

```azurecli
az acr repository list --name <acrName> --output table
```

Výstup:

```azurecli
Result
----------------
aci-tutorial-app
```

A pak můžete pomocí příkazu [az acr repository show-tags][az-acr-repository-show-tags] zobrazit značky pro konkrétní image.

```azurecli
az acr repository show-tags --name <acrName> --repository aci-tutorial-app --output table
```

Výstup:

```azurecli
Result
--------
v1
```

## <a name="next-steps"></a>Další kroky

V tomto kurzu jste připravili službu Azure Container Registry pro použití se službou Azure Container Instances a nahráli image kontejneru do registru. Dokončili jste následující kroky:

> [!div class="checklist"]
> * Nasazení instance služby Azure Container Registry
> * Označení image kontejneru pro službu Azure Container Registry
> * Odeslání image do služby Azure Container Registry

Přejděte k dalšímu kurzu, ve kterém se naučíte nasadit kontejner do prostředí Azure s použitím služby Azure Container Instances.

> [!div class="nextstepaction"]
> [Nasazování kontejnerů do služby Azure Container Instances](./container-instances-tutorial-deploy-app.md)

<!-- LINKS - External -->
[docker-build]: https://docs.docker.com/engine/reference/commandline/build/
[docker-get-started]: https://docs.docker.com/get-started/
[docker-hub-nodeimage]: https://store.docker.com/images/node
[docker-images]: https://docs.docker.com/engine/reference/commandline/images/
[docker-linux]: https://docs.docker.com/engine/installation/#supported-platforms
[docker-login]: https://docs.docker.com/engine/reference/commandline/login/
[docker-mac]: https://docs.docker.com/docker-for-mac/
[docker-push]: https://docs.docker.com/engine/reference/commandline/push/
[docker-tag]: https://docs.docker.com/engine/reference/commandline/tag/
[docker-windows]: https://docs.docker.com/docker-for-windows/
[nodejs]: http://nodejs.org

<!-- LINKS - Internal -->
[az-acr-create]: /cli/azure/acr#az_acr_create
[az-acr-login]: /cli/azure/acr#az_acr_login
[az-acr-repository-list]: /cli/azure/acr/repository#az_acr_list
[az-acr-repository-show-tags]: /cli/azure/acr/repository#az_acr_repository_show_tags
[az-acr-show]: /cli/azure/acr#az_acr_show
[az-group-create]: /cli/azure/group#az_group_create
[azure-cli-install]: /cli/azure/install-azure-cli
