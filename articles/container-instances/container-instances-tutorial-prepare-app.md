---
title: "Kurz pro Azure instancí kontejnerů – Příprava aplikace"
description: "Azure instancí kontejnerů kurz část 1 ze 3 – Příprava aplikace pro nasazení do Azure kontejner instancí"
services: container-instances
author: seanmck
manager: timlt
ms.service: container-instances
ms.topic: tutorial
ms.date: 01/02/2018
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: fc16be80e776d1472be775fa32354ba157d16545
ms.sourcegitcommit: 85012dbead7879f1f6c2965daa61302eb78bd366
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/02/2018
---
# <a name="create-container-for-deployment-to-azure-container-instances"></a>Vytvoření kontejneru pro nasazení do služby Azure Container Instances

Azure Container Instances umožňuje nasazení kontejnerů Dockeru na infrastrukturu Azure bez zřizování virtuálních počítačů nebo využívání služby vyšší úrovně. V tomto kurzu sestavíte malé webové aplikace v Node.js a balíčku v kontejneru, který lze spouštět s využitím Azure kontejner instancí.

V tomto článku část jedna řada, můžete:

> [!div class="checklist"]
> * Klonování zdrojovému kódu aplikace z webu GitHub
> * Vytvořit bitovou kopii kontejneru z aplikace zdroje.
> * Testování bitovou kopii v místním prostředí Docker

V následujících kurzech odeslání image do registru kontejneru služby Azure a poté ji nasadit do Azure kontejner instancí.

## <a name="before-you-begin"></a>Než začnete

Tento kurz vyžaduje, že používáte Azure CLI verze 2.0.23 nebo novější. Verzi zjistíte spuštěním příkazu `az --version`. Pokud je potřeba nainstalovat nebo upgradovat najdete v tématu [nainstalovat Azure CLI 2.0][azure-cli-install].

Tento kurz předpokládá základní znalosti o základní koncepty Docker jako kontejnery, kontejner bitové kopie a basic `docker` příkazy. V případě potřeby, najdete v části [začít pracovat s Docker] [ docker-get-started] pro úvod do na základní informace o kontejneru.

K dokončení tohoto kurzu potřebujete prostředí pro vývoj Docker nainstalovány místně. Docker poskytuje balíčky, které můžete snadno konfigurovat Docker na žádném [Mac][docker-mac], [Windows][docker-windows], nebo [Linux] [ docker-linux] systému.

Prostředí Azure Cloud neobsahuje součásti Docker nutné pro dokončení každý krok v tomto kurzu. Je nutné nainstalovat rozhraní příkazového řádku Azure a nástrojem Docker vývojového prostředí v místním počítači k dokončení tohoto kurzu.

## <a name="get-application-code"></a>Získání kódu aplikace

Ukázka v tomto kurzu zahrnuje jednoduchou webovou aplikaci součástí [Node.js][nodejs]. Tato aplikace slouží jako statická stránka HTML a vypadá takto:

![Ukázková aplikace zobrazená v prohlížeči][aci-tutorial-app]

Pomocí gitu si stáhněte ukázku:

```bash
git clone https://github.com/Azure-Samples/aci-helloworld.git
```

## <a name="build-the-container-image"></a>Sestavení image kontejneru

Soubor Dockerfile, který je součástí ukázkového úložiště, ukazuje postup sestavení kontejneru. Spuštění z [oficiální Node.js image] [ docker-hub-nodeimage] na základě [Alpine Linux][alpine-linux], malé distribuce, která dobře hodí pro použití s kontejnery. Potom zkopíruje soubory aplikace do kontejneru, nainstaluje závislosti pomocí Node Package Manageru a nakonec aplikaci spustí.

```Dockerfile
FROM node:8.9.3-alpine
RUN mkdir -p /usr/src/app
COPY ./app/* /usr/src/app/
WORKDIR /usr/src/app
RUN npm install
CMD node /usr/src/app/index.js
```

Použití [docker sestavení] [ docker-build] příkaz pro vytvoření bitové kopie kontejneru, označování jej jako *aci – kurz aplikace*:

```bash
docker build ./aci-helloworld -t aci-tutorial-app
```

Výstup z [docker sestavení] [ docker-build] příkaz je podobný následujícímu (zkrácená čitelnější):

```bash
Sending build context to Docker daemon  119.3kB
Step 1/6 : FROM node:8.9.3-alpine
8.9.3-alpine: Pulling from library/node
88286f41530e: Pull complete
84f3a4bf8410: Pull complete
d0d9b2214720: Pull complete
Digest: sha256:c73277ccc763752b42bb2400d1aaecb4e3d32e3a9dbedd0e49885c71bea07354
Status: Downloaded newer image for node:8.9.3-alpine
 ---> 90f5ee24bee2
...
Step 6/6 : CMD node /usr/src/app/index.js
 ---> Running in f4a1ea099eec
 ---> 6edad76d09e9
Removing intermediate container f4a1ea099eec
Successfully built 6edad76d09e9
Successfully tagged aci-tutorial-app:latest
```

Použití [imagí dockeru] [ docker-images] příkazu zobrazte vytvořené bitové kopie:

```bash
docker images
```

Výstup:

```bash
REPOSITORY                   TAG                 IMAGE ID            CREATED              SIZE
aci-tutorial-app             latest              5c745774dfa9        39 seconds ago       68.1 MB
```

## <a name="run-the-container-locally"></a>Místní spuštění kontejneru

Než se pokusíte kontejner nasadit do služby Azure Container Instances, spusťte ho místně, abyste ověřili, že funguje. Přepínač `-d` umožňuje spuštění kontejneru na pozadí, zatímco přepínač `-p` umožňuje mapování libovolného portu vašeho počítače na port 80 kontejneru.

```bash
docker run -d -p 8080:80 aci-tutorial-app
```

Otevřete v prohlížeči adresu http://localhost:8080 a ověřte, že je kontejner spuštěný.

![Místní spuštění aplikace v prohlížeči][aci-tutorial-app-local]

## <a name="next-steps"></a>Další postup

V tomto kurzu jste vytvořili image kontejneru, kterou je možné nasadit do služby Azure Container Instances. Dokončili jste následující kroky:

> [!div class="checklist"]
> * Klonovat zdroj aplikace z webu GitHub
> * Vytvořený kontejner obrázky z aplikací zdroje
> * Testování kontejneru místně

Přejděte k dalšímu kurzu, ve kterém se seznámíte s ukládáním imagí kontejnerů ve službě Azure Container Registry.

> [!div class="nextstepaction"]
> [Nahrávání imagí do služby Azure Container Registry](./container-instances-tutorial-prepare-acr.md)

<!--- IMAGES --->
[aci-tutorial-app]:./media/container-instances-quickstart/aci-app-browser.png
[aci-tutorial-app-local]: ./media/container-instances-tutorial-prepare-app/aci-app-browser-local.png

<!-- LINKS - External -->
[alpine-linux]: https://alpinelinux.org/
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
[azure-cli-install]: /cli/azure/install-azure-cli
