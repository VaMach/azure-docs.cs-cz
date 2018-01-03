---
title: "Kurz pro Azure instancí kontejnerů – Příprava registru kontejner Azure"
description: "Azure instancí kontejnerů kurz část 2 / 3 – Příprava registru kontejner Azure"
services: container-instances
author: neilpeterson
manager: timlt
ms.service: container-instances
ms.topic: tutorial
ms.date: 01/02/2018
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: c0aad1f9bbaac9a456b34f75633faba92f57f498
ms.sourcegitcommit: 85012dbead7879f1f6c2965daa61302eb78bd366
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/02/2018
---
# <a name="deploy-and-use-azure-container-registry"></a>Nasazení a používání Azure kontejneru registru

Toto je část dva kurzu třemi částmi. V [předchozího kroku](container-instances-tutorial-prepare-app.md), bitovou kopii kontejner byl vytvořen pro jednoduché webové aplikace napsané v [Node.js][nodejs]. V tomto kurzu push bitovou kopii do registru kontejneru služby Azure. Pokud jste ještě nevytvořili bitovou kopii kontejneru, vrátit [kurzu 1 – Vytvoření kontejneru image](container-instances-tutorial-prepare-app.md).

Registr kontejner Azure je založenou na Azure a privátní registru pro kontejner Docker bitové kopie. V tomto kurzu vás provede nasazením instanci Azure Container registru a když zavedete bitovou kopii kontejneru.

V tomto článku část dvě řady, můžete:

> [!div class="checklist"]
> * Nasaďte instanci Azure Container registru
> * Značka bitovou kopii kontejner pro vaše registru kontejner Azure
> * Nahrajte image na vaše registru

V následující článek konečné kurz v této sérii, můžete nasadit kontejner z vaší privátní registru pro kontejner instancí Azure.

## <a name="before-you-begin"></a>Než začnete

Tento kurz vyžaduje, že používáte Azure CLI verze 2.0.23 nebo novější. Verzi zjistíte spuštěním příkazu `az --version`. Pokud je potřeba nainstalovat nebo upgradovat najdete v tématu [nainstalovat Azure CLI 2.0][azure-cli-install].

K dokončení tohoto kurzu potřebujete prostředí pro vývoj Docker nainstalovány místně. Docker poskytuje balíčky, které můžete snadno konfigurovat Docker na žádném [Mac][docker-mac], [Windows][docker-windows], nebo [Linux] [ docker-linux] systému.

Prostředí Azure Cloud neobsahuje součásti Docker nutné pro dokončení každý krok v tomto kurzu. Je nutné nainstalovat rozhraní příkazového řádku Azure a nástrojem Docker vývojového prostředí v místním počítači k dokončení tohoto kurzu.

## <a name="deploy-azure-container-registry"></a>Nasadit kontejner Azure registru

Pokud nasazujete registru kontejneru služby Azure, musíte nejprve skupinu prostředků. Skupinu prostředků Azure je logické kolekce do Azure, které jsou nasazené a spravované prostředky.

Vytvořte skupinu prostředků pomocí příkazu [az group create][az-group-create]. V tomto příkladu skupinu prostředků s názvem *myResourceGroup* je vytvořen v *eastus* oblast.

```azurecli
az group create --name myResourceGroup --location eastus
```

Vytvoření služby Azure kontejneru registru s [vytvořit az acr] [ az-acr-create] příkaz. Název kontejneru registru **musí být jedinečné** v rámci Azure a musí obsahovat alfanumerické znaky 5-50. Nahraďte `<acrName>` s jedinečným názvem pro vaše registru:

```azurecli
az acr create --resource-group myResourceGroup --name <acrName> --sku Basic
```

Například k vytvoření služby Azure kontejneru registru s názvem *mycontainerregistry082*:

```azurecli
az acr create --resource-group myResourceGroup --name mycontainerregistry082 --sku Basic --admin-enabled true
```

Po celý zbytek v tomto kurzu používáme `<acrName>` jako zástupný symbol pro název kontejneru registru, který jste zvolili.

## <a name="container-registry-login"></a>Kontejner registru přihlášení

Musíte se přihlásit k vaší instanci Azure Container registru před odesláním bitové kopie do ní. Použití [az acr přihlášení] [ az-acr-login] příkaz k dokončení operace. Je nutné zadat jedinečný název, který jste zadali pro kontejner registru, když jste ji vytvořili.

```azurecli
az acr login --name <acrName>
```

Příkaz vrátí `Login Succeeded` zprávu po dokončení.

## <a name="tag-container-image"></a>Obrázek značky kontejneru

K nasazení bitové kopie kontejneru z privátní registru, musíte označit bitové kopie s `loginServer` název registru.

Chcete-li zobrazit seznam aktuální bitové kopie, použijte [imagí dockeru] [ docker-images] příkaz.

```bash
docker images
```

Výstup:

```bash
REPOSITORY                   TAG                 IMAGE ID            CREATED              SIZE
aci-tutorial-app             latest              5c745774dfa9        39 seconds ago       68.1 MB
```

Chcete-li získat název loginServer, spusťte [az acr zobrazit] [ az-acr-show] příkaz. Nahraďte `<acrName>` s názvem vašeho kontejneru registru.

```azurecli
az acr show --name <acrName> --query loginServer --output table
```

Příklad výstupu:

```
Result
------------------------
mycontainerregistry082.azurecr.io
```

Značka *aci – kurz aplikace* bitovou kopii s loginServer registru systému kontejneru. Navíc přidat `:v1` na konec název bitové kopie. Tato značka označuje číslo verze bitové kopie. Nahraďte `<acrLoginServer>` s výsledkem [az acr zobrazit] [ az-acr-show] příkaz právě spuštěnou.

```bash
docker tag aci-tutorial-app <acrLoginServer>/aci-tutorial-app:v1
```

Jakmile příznakem, spustit `docker images` ověření operaci.

```bash
docker images
```

Výstup:

```bash
REPOSITORY                                                TAG                 IMAGE ID            CREATED             SIZE
aci-tutorial-app                                          latest              5c745774dfa9        39 seconds ago      68.1 MB
mycontainerregistry082.azurecr.io/aci-tutorial-app        v1                  a9dace4e1a17        7 minutes ago       68.1 MB
```

## <a name="push-image-to-azure-container-registry"></a>Push bitové kopie do registru kontejner Azure

Nabízené *aci – kurz aplikace* bitovou kopii do registru se [docker nabízené] [ docker-push] příkaz. Nahraďte `<acrLoginServer>` s názvem serveru úplná přihlášení získáte v předchozím kroku.

```bash
docker push <acrLoginServer>/aci-tutorial-app:v1
```

`push` Operace by měla trvat několik sekund až několik minut v závislosti na připojení k Internetu a výstup se podobá následující:

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

## <a name="list-images-in-azure-container-registry"></a>Seznam obrázků v registru kontejner Azure

Chcete-li vrátit seznam bitové kopie, které se nabídne do registru kontejneru Azure, použijte [az acr úložiště seznamu] [ az-acr-repository-list] příkaz. Aktualizujte příkaz s názvem registru kontejneru.

```azurecli
az acr repository list --name <acrName> --output table
```

Výstup:

```azurecli
Result
----------------
aci-tutorial-app
```

A pak najdete v části značky pro konkrétní image, pomocí [az acr úložiště zobrazit značky] [ az-acr-repository-show-tags] příkaz.

```azurecli
az acr repository show-tags --name <acrName> --repository aci-tutorial-app --output table
```

Výstup:

```azurecli
Result
--------
v1
```

## <a name="next-steps"></a>Další postup

V tomto kurzu připravená registru kontejner Azure pro použití s instancemi Azure kontejneru a instaluje bitovou kopii kontejneru do registru. Dokončili jste následující kroky:

> [!div class="checklist"]
> * Nasazení instanci Azure Container registru
> * Označí image kontejneru pro registru kontejner Azure
> * Odeslat bitovou kopii do registru kontejner Azure

Přechodu na další kurzu se dozvíte o nasazení do Azure pomocí Azure kontejner instancí kontejneru.

> [!div class="nextstepaction"]
> [Kontejnery můžete nasadit do Azure kontejner instancí](./container-instances-tutorial-deploy-app.md)

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
