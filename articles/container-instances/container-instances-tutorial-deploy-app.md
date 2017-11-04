---
title: "Kurz pro Azure instancí kontejnerů – nasazení aplikace"
description: "Kurz pro Azure instancí kontejnerů – nasazení aplikace"
services: container-instances
documentationcenter: 
author: seanmck
manager: timlt
editor: 
tags: 
keywords: 
ms.assetid: 
ms.service: container-instances
ms.devlang: azurecli
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/26/2017
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: 3b651526f5ee3197e7d04accb6a87e2f10bf0791
ms.sourcegitcommit: 3ab5ea589751d068d3e52db828742ce8ebed4761
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/27/2017
---
# <a name="deploy-a-container-to-azure-container-instances"></a>Nasazení do Azure kontejner instancí kontejneru

Toto je poslední kurzu třemi částmi. V předchozích částech [bitovou kopii kontejner byl vytvořen](container-instances-tutorial-prepare-app.md) a [nabídnutých do služby Azure kontejneru registru](container-instances-tutorial-prepare-acr.md). Tato část dokončení tohoto kurzu nasazením kontejneru na kontejner instancí Azure. Dokončit krokům patří:

> [!div class="checklist"]
> * Nasazení kontejneru z registru kontejner Azure pomocí rozhraní příkazového řádku Azure
> * Zobrazení aplikace v prohlížeči
> * Prohlížení protokolů kontejneru

## <a name="before-you-begin"></a>Než začnete

Tento kurz vyžaduje, že používáte Azure CLI verze 2.0.20 nebo novější. Verzi zjistíte spuštěním příkazu `az --version`. Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI 2.0](/cli/azure/install-azure-cli).

K dokončení tohoto kurzu potřebujete vývojové prostředí pro Docker. Docker nabízí balíčky pro snadnou konfiguraci Dockeru na jakémkoli systému [Mac](https://docs.docker.com/docker-for-mac/), [Windows](https://docs.docker.com/docker-for-windows/) nebo [Linux](https://docs.docker.com/engine/installation/#supported-platforms).

Prostředí Azure Cloud neobsahuje součásti Docker nutné pro dokončení každý krok v tomto kurzu. Proto doporučujeme místní instalace rozhraní příkazového řádku Azure a nástrojem Docker vývojového prostředí.

## <a name="deploy-the-container-using-the-azure-cli"></a>Nasaďte kontejner pomocí rozhraní příkazového řádku Azure

Rozhraní příkazového řádku Azure umožňuje nasazení kontejner Azure kontejner instancí v jednom příkazu. Vzhledem k tomu, že je kontejner image umístěná v privátní registru kontejner Azure, musí zahrnovat přihlašovací údaje potřebné pro přístup k ní. V případě potřeby se můžete dotazovat je jak je uvedeno níže.

Kontejner registru přihlášení server (aktualizace nahraďte názvem registru):

```azurecli
az acr show --name <acrName> --query loginServer
```

Kontejner registru heslo:

```azurecli
az acr credential show --name <acrName> --query "passwords[0].value"
```

K nasazení bitové kopie kontejneru z registru kontejneru se žádost o prostředku procesorového 1 a 1 GB paměti, spusťte následující příkaz:

```azurecli
az container create --name aci-tutorial-app --image <acrLoginServer>/aci-tutorial-app:v1 --cpu 1 --memory 1 --registry-password <acrPassword> --ip-address public -g myResourceGroup
```

Během pár sekund měli byste obdržet počáteční reakce ze Správce prostředků Azure. Pokud chcete zobrazit stav nasazení, použijte:

```azurecli
az container show --name aci-tutorial-app --resource-group myResourceGroup --query state
```

Abychom mohli pokračovat, dokud se stav změní z spuštěním tohoto příkazu *čekající* k *systémem*. Potom jsme můžete pokračovat.

## <a name="view-the-application-and-container-logs"></a>Zobrazit protokoly aplikací a kontejneru

Po úspěšné nasazení, otevřete prohlížeč na IP adresu zobrazené ve výstupu příkazu:

```bash
az container show --name aci-tutorial-app --resource-group myResourceGroup --query ipAddress.ip
```

```json
"13.88.176.27"
```

![Hello world aplikace v prohlížeči][aci-app-browser]

Můžete také zobrazit výstup protokolu kontejneru:

```azurecli
az container logs --name aci-tutorial-app -g myResourceGroup
```

Výstup:

```bash
listening on port 80
::ffff:10.240.0.4 - - [21/Jul/2017:06:00:02 +0000] "GET / HTTP/1.1" 200 1663 "-" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_12_5) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/59.0.3071.115 Safari/537.36"
::ffff:10.240.0.4 - - [21/Jul/2017:06:00:02 +0000] "GET /favicon.ico HTTP/1.1" 404 150 "http://13.88.176.27/" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_12_5) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/59.0.3071.115 Safari/537.36"
```

## <a name="next-steps"></a>Další kroky

V tomto kurzu jste dokončili proces nasazení kontejnerů do Azure kontejner instancí. Dokončili jste následující kroky:

> [!div class="checklist"]
> * Nasazení kontejneru z registru kontejner Azure pomocí rozhraní příkazového řádku Azure
> * Zobrazení aplikace v prohlížeči
> * Prohlížení protokolů kontejneru

<!-- LINKS -->
[prepare-app]: ./container-instances-tutorial-prepare-app.md

<!-- IMAGES -->
[aci-app-browser]: ./media/container-instances-quickstart/aci-app-browser.png
