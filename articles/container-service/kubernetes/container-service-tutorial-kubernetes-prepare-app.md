---
title: "Kurz pro Azure Container Service – Příprava aplikace"
description: "Kurz pro Azure Container Service – Příprava aplikace"
services: container-service
author: neilpeterson
manager: timlt
ms.service: container-service
ms.topic: tutorial
ms.date: 09/14/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 3dcfc0d454926d6040692b0e8a9d44b13e7603c5
ms.sourcegitcommit: 5d3e99478a5f26e92d1e7f3cec6b0ff5fbd7cedf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/06/2017
---
# <a name="create-container-images-to-be-used-with-azure-container-service"></a>Vytvoření bitové kopie kontejneru, který se má použít s Azure Container Service

[!INCLUDE [aks-preview-redirect.md](../../../includes/aks-preview-redirect.md)]

V tomto kurzu, 7, první část je více kontejnerové aplikace připravené pro použití v Kubernetes. Dokončit krokům patří:  

> [!div class="checklist"]
> * Klonování zdroje aplikace z GitHubu  
> * Vytvoření kontejneru image ze zdroje aplikace
> * Testování aplikace v místním prostředí Docker

Po dokončení následující aplikace je dostupné ve vašem místním vývojovém prostředí.

![Obrázek clusteru Kubernetes v Azure](media/container-service-kubernetes-tutorials/azure-vote.png)

V následujících kurzech bitovou kopii kontejneru se nahraje registru kontejneru služby Azure a pak spustit v Azure hostovaná Kubernetes clusteru.

## <a name="before-you-begin"></a>Než začnete

V tomto kurzu se předpokládá základní znalost klíčových konceptů Dockeru, jako jsou kontejnery, image kontejnerů a základní příkazy Dockeru. V případě potřeby najdete základní informace o kontejnerech v článku [Get started with Docker]( https://docs.docker.com/get-started/) (Začínáme s Dockerem). 

K dokončení tohoto kurzu potřebujete vývojové prostředí pro Docker. Docker nabízí balíčky pro snadnou konfiguraci Dockeru na jakémkoli systému [Mac](https://docs.docker.com/docker-for-mac/), [Windows](https://docs.docker.com/docker-for-windows/) nebo [Linux](https://docs.docker.com/engine/installation/#supported-platforms).

Prostředí Azure Cloud neobsahuje součásti Docker nutné pro dokončení každý krok v tomto kurzu. Proto doporučujeme používat úplnou Docker vývojovém prostředí.

## <a name="get-application-code"></a>Získání kódu aplikace

Ukázková aplikace používá v tomto kurzu je základní hlasovací aplikaci. Aplikace se skládá z front-endové webové součásti a instanci Redis back-end. Součást webové je zabalené do bitové kopie vlastní kontejner. Redis instance používá image beze změny z úložiště Docker Hub.  

Pomocí git stáhnout kopii aplikace na svoje vývojové prostředí.

```bash
git clone https://github.com/Azure-Samples/azure-voting-app-redis.git
```

Změňte adresáře tak, aby při práci z klonovaného adresáře.

```
cd azure-voting-app-redis
```

V adresáři je zdrojový kód aplikace, předem vytvořené Docker compose soubor a soubor manifestu Kubernetes. Tyto soubory se používají v celé sadě kurzu. 

## <a name="create-container-images"></a>Vytvořit kontejner bitové kopie

[Docker Compose](https://docs.docker.com/compose/) můžete použít k automatizaci sestavení mimo kontejner bitové kopie a nasazení aplikací s více kontejneru.

Spustit `docker-compose.yml` soubor pro vytvoření bitové kopie kontejneru, stáhněte bitovou kopii Redis a spusťte aplikaci.

```bash
docker-compose up -d
```

Po dokončení použít [imagí dockeru](https://docs.docker.com/engine/reference/commandline/images/) příkazu zobrazte vytvořené bitové kopie.

```bash
docker images
```

Všimněte si, že tři bitové kopie byly staženy nebo vytvořeny. `azure-vote-front` Image obsahuje aplikace a používá `nginx-flask` bitovou kopii jako základ. `redis` Image se použije ke spuštění Redis instance.

```bash
REPOSITORY                   TAG        IMAGE ID            CREATED             SIZE
azure-vote-front             latest     9cc914e25834        40 seconds ago      694MB
redis                        latest     a1b99da73d05        7 days ago          106MB
tiangolo/uwsgi-nginx-flask   flask      788ca94b2313        9 months ago        694MB
```

Spustit [docker ps](https://docs.docker.com/engine/reference/commandline/ps/) příkazu zobrazte spuštěných kontejnerů.

```bash
docker ps
```

Výstup:

```bash
CONTAINER ID        IMAGE             COMMAND                  CREATED             STATUS              PORTS                           NAMES
82411933e8f9        azure-vote-front  "/usr/bin/supervisord"   57 seconds ago      Up 30 seconds       443/tcp, 0.0.0.0:8080->80/tcp   azure-vote-front
b68fed4b66b6        redis             "docker-entrypoint..."   57 seconds ago      Up 30 seconds       0.0.0.0:6379->6379/tcp          azure-vote-back
```

## <a name="test-application-locally"></a>Testovací aplikace místně

Přejděte na adrese http://localhost: 8080 zobrazíte běžící aplikaci.

![Obrázek clusteru Kubernetes v Azure](media/container-service-kubernetes-tutorials/azure-vote.png)

## <a name="clean-up-resources"></a>Vyčištění prostředků

Teď, když funkce aplikace byl ověřen, může spuštěných kontejnerů zastavena a odebrat. Neodstraňujte kontejneru bitové kopie. `azure-vote-front` Bitové kopie se nahraje instanci Azure Container registru v dalším kurzu.

Spusťte následující příkaz k zastavení spuštěných kontejnerů.

```bash
docker-compose stop
```

Odstraňte zastaven kontejnery a prostředky pomocí následujícího příkazu.

```bash
docker-compose down
```

Při dokončení máte kontejneru bitovou kopii, která obsahuje aplikaci Azure hlas.

## <a name="next-steps"></a>Další kroky

V tomto kurzu aplikace byla testována a vytvořit kontejner bitových kopií pro aplikaci. Dokončili jste následující kroky:

> [!div class="checklist"]
> * Klonování zdroje aplikace z GitHubu  
> * Vytvořené bitové kopie kontejneru z zdroj aplikace
> * Testování aplikace v místním prostředí Docker

Přejděte k dalšímu kurzu, ve kterém se seznámíte s ukládáním imagí kontejnerů ve službě Azure Container Registry.

> [!div class="nextstepaction"]
> [Nahrávání imagí do služby Azure Container Registry](./container-service-tutorial-kubernetes-prepare-acr.md)
