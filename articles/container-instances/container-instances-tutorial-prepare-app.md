---
title: "Kurz pro Azure instancí kontejnerů – Příprava aplikace"
description: "Příprava aplikace pro nasazení do služby Azure Container Instances"
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
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/26/2017
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: 52d99411b2dc9ae9c3f2ebd3b9f346973a91e7c9
ms.sourcegitcommit: 3ab5ea589751d068d3e52db828742ce8ebed4761
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/27/2017
---
# <a name="create-container-for-deployment-to-azure-container-instances"></a>Vytvoření kontejneru pro nasazení do služby Azure Container Instances

Azure Container Instances umožňuje nasazení kontejnerů Dockeru na infrastrukturu Azure bez zřizování virtuálních počítačů nebo využívání služby vyšší úrovně. V tomto kurzu sestavíte malé webové aplikace v Node.js a balíčku v kontejneru, který lze spouštět s využitím Azure kontejner instancí. Kurz zahrnuje:

> [!div class="checklist"]
> * Klonování zdroje aplikace z GitHubu
> * Vytváření imagí kontejnerů ze zdroje aplikace
> * Testování imagí v místním prostředí Dockeru

V následujících kurzech odeslání image do registru kontejneru služby Azure a poté ji nasadit do Azure kontejner instancí.

## <a name="before-you-begin"></a>Než začnete

Tento kurz vyžaduje, že používáte Azure CLI verze 2.0.20 nebo novější. Verzi zjistíte spuštěním příkazu `az --version`. Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI 2.0](/cli/azure/install-azure-cli).

Tento kurz předpokládá základní znalosti o základní koncepty Docker jako kontejnery, kontejner bitové kopie a basic `docker` příkazy. V případě potřeby najdete základní informace o kontejnerech v článku [Get started with Docker]( https://docs.docker.com/get-started/) (Začínáme s Dockerem).

K dokončení tohoto kurzu potřebujete vývojové prostředí pro Docker. Docker nabízí balíčky pro snadnou konfiguraci Dockeru na jakémkoli systému [Mac](https://docs.docker.com/docker-for-mac/), [Windows](https://docs.docker.com/docker-for-windows/) nebo [Linux](https://docs.docker.com/engine/installation/#supported-platforms).

Prostředí Azure Cloud neobsahuje součásti Docker nutné pro dokončení každý krok v tomto kurzu. Proto doporučujeme místní instalace rozhraní příkazového řádku Azure a nástrojem Docker vývojového prostředí.

## <a name="get-application-code"></a>Získání kódu aplikace

Ukázka v tomto kurzu zahrnuje jednoduchou webovou aplikaci vytvořenou v [Node.js](http://nodejs.org). Tato aplikace slouží jako statická stránka HTML a vypadá takto:

![Ukázková aplikace zobrazená v prohlížeči][aci-tutorial-app]

Pomocí gitu si stáhněte ukázku:

```bash
git clone https://github.com/Azure-Samples/aci-helloworld.git
```

## <a name="build-the-container-image"></a>Sestavení image kontejneru

Soubor Dockerfile, který je součástí ukázkového úložiště, ukazuje postup sestavení kontejneru. Začíná od [oficiální image Node.js][dockerhub-nodeimage] založené na systému [Alpine Linux](https://alpinelinux.org/), malé distribuci vhodné pro použití s kontejnery. Potom zkopíruje soubory aplikace do kontejneru, nainstaluje závislosti pomocí Node Package Manageru a nakonec aplikaci spustí.

```Dockerfile
FROM node:8.2.0-alpine
RUN mkdir -p /usr/src/app
COPY ./app/* /usr/src/app/
WORKDIR /usr/src/app
RUN npm install
CMD node /usr/src/app/index.js
```

Pomocí příkazu `docker build` vytvořte image kontejneru a označte ji jako *aci-tutorial-app*:

```bash
docker build ./aci-helloworld -t aci-tutorial-app
```

Pomocí příkazu `docker images` zobrazte sestavenou image:

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

## <a name="next-steps"></a>Další kroky

V tomto kurzu jste vytvořili image kontejneru, kterou je možné nasadit do služby Azure Container Instances. Dokončili jste následující kroky:

> [!div class="checklist"]
> * Klonovat zdroj aplikace z webu GitHub
> * Vytvořený kontejner obrázky z aplikací zdroje
> * Testování kontejneru místně

Přejděte k dalšímu kurzu, ve kterém se seznámíte s ukládáním imagí kontejnerů ve službě Azure Container Registry.

> [!div class="nextstepaction"]
> [Nahrávání imagí do služby Azure Container Registry](./container-instances-tutorial-prepare-acr.md)

<!-- LINKS -->
[dockerhub-nodeimage]: https://store.docker.com/images/node

<!--- IMAGES --->
[aci-tutorial-app]:./media/container-instances-quickstart/aci-app-browser.png
[aci-tutorial-app-local]: ./media/container-instances-tutorial-prepare-app/aci-app-browser-local.png