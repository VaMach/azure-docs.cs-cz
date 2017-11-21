---
title: "Kurz pro Azure instancí kontejnerů – nasazení aplikace"
description: "Kurz pro Azure instancí kontejnerů – nasazení aplikace"
services: container-instances
documentationcenter: 
author: seanmck
manager: timlt
editor: mmacy
tags: 
keywords: 
ms.assetid: 
ms.service: container-instances
ms.devlang: azurecli
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/20/2017
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: 5222c43fe381c93527ec6c8b927d723f75dd58f9
ms.sourcegitcommit: 1d8612a3c08dc633664ed4fb7c65807608a9ee20
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/20/2017
---
# <a name="deploy-a-container-to-azure-container-instances"></a>Nasazení do Azure kontejner instancí kontejneru

Toto je poslední kurzu třemi částmi. V předchozích částech [bitovou kopii kontejner byl vytvořen](container-instances-tutorial-prepare-app.md) a [nabídnutých do služby Azure kontejneru registru](container-instances-tutorial-prepare-acr.md). Tato část dokončení tohoto kurzu nasazením kontejneru na kontejner instancí Azure. Dokončit krokům patří:

> [!div class="checklist"]
> * Nasazení kontejneru z registru kontejner Azure pomocí rozhraní příkazového řádku Azure
> * Zobrazení aplikace v prohlížeči
> * Prohlížení protokolů kontejneru

## <a name="before-you-begin"></a>Než začnete

Tento kurz vyžaduje, že používáte Azure CLI verze 2.0.21 nebo novější. Verzi zjistíte spuštěním příkazu `az --version`. Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI 2.0](/cli/azure/install-azure-cli).

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

K nasazení bitové kopie kontejneru z registru kontejneru se žádost o prostředku procesorového 1 a 1 GB paměti, spusťte následující příkaz. Nahraďte `<acrLoginServer>` a `<acrPassword>` s hodnotami, které jste získali z předchozích dvou příkazů.

```azurecli
az container create --name aci-tutorial-app --image <acrLoginServer>/aci-tutorial-app:v1 --cpu 1 --memory 1 --registry-password <acrPassword> --ip-address public --ports 80 -g myResourceGroup
```

Během pár sekund měli byste obdržet počáteční reakce ze Správce prostředků Azure. Chcete-li zobrazit stav nasazení, použijte [az kontejneru zobrazit](/cli/azure/container#az_container_show):

```azurecli
az container show --name aci-tutorial-app --resource-group myResourceGroup --query instanceView.state
```

Opakujte `az container show` příkaz, dokud se stav změní z *čekající* k *systémem*, která by za několik minut trvat. Pokud je kontejner *systémem*, přejděte k dalšímu kroku.

## <a name="view-the-application-and-container-logs"></a>Zobrazit protokoly aplikací a kontejneru

Po úspěšné nasazení, zobrazí kontejneru veřejnou IP adresu s [az kontejneru zobrazit](/cli/azure/container#az_container_show) příkaz:

```bash
az container show --name aci-tutorial-app --resource-group myResourceGroup --query ipAddress.ip
```

Příklad výstupu:`"13.88.176.27"`

Informace o běžící aplikaci, přejděte na veřejnou IP adresu v oblíbeném prohlížeči.

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

## <a name="clean-up-resources"></a>Vyčištění prostředků

Pokud již nepotřebujete žádné prostředky vytvořené z této série kurz, můžete provést [odstranění skupiny az](/cli/azure/group#delete) příkaz k odebrání skupiny prostředků a všechny prostředky, které obsahuje. Tento příkaz odstraní kontejner registru, kterou jste vytvořili, a také kontejneru spuštěné a všechny související prostředky.

```azurecli-interactive
az group delete --name myResourceGroup
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
