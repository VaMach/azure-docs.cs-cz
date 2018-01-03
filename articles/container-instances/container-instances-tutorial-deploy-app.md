---
title: "Kurz pro Azure instancí kontejnerů – nasazení aplikace"
description: "Kurz pro Azure instancí kontejnerů část 3 ze 3 – nasazení aplikace"
services: container-instances
author: seanmck
manager: timlt
ms.service: container-instances
ms.topic: tutorial
ms.date: 01/02/2018
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: 471caa1b24dc7017c70782c072b2068f9635244b
ms.sourcegitcommit: 85012dbead7879f1f6c2965daa61302eb78bd366
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/02/2018
---
# <a name="deploy-a-container-to-azure-container-instances"></a>Nasazení do Azure kontejner instancí kontejneru

Toto je poslední kurz v řadě třemi částmi. Starší v řadě, [bitovou kopii kontejner byl vytvořen](container-instances-tutorial-prepare-app.md) a [nabídnutých do služby Azure kontejneru registru](container-instances-tutorial-prepare-acr.md). Tento článek dokončení kurzu řady nasazením kontejneru na kontejner instancí Azure.

V tomto kurzu jste:

> [!div class="checklist"]
> * Nasaďte kontejner z registru kontejner Azure pomocí rozhraní příkazového řádku Azure
> * Zobrazení aplikace v prohlížeči
> * Zobrazit protokoly kontejneru

## <a name="before-you-begin"></a>Než začnete

Tento kurz vyžaduje, že používáte Azure CLI verze 2.0.23 nebo novější. Verzi zjistíte spuštěním příkazu `az --version`. Pokud je potřeba nainstalovat nebo upgradovat najdete v tématu [nainstalovat Azure CLI 2.0][azure-cli-install].

K dokončení tohoto kurzu potřebujete prostředí pro vývoj Docker nainstalovány místně. Docker poskytuje balíčky, které můžete snadno konfigurovat Docker na žádném [Mac][docker-mac], [Windows][docker-windows], nebo [Linux] [ docker-linux] systému.

Prostředí Azure Cloud neobsahuje součásti Docker nutné pro dokončení každý krok v tomto kurzu. Je nutné nainstalovat rozhraní příkazového řádku Azure a nástrojem Docker vývojového prostředí v místním počítači k dokončení tohoto kurzu.

## <a name="deploy-the-container-using-the-azure-cli"></a>Nasaďte kontejner pomocí rozhraní příkazového řádku Azure

Rozhraní příkazového řádku Azure umožňuje nasazení kontejner Azure kontejner instancí v jednom příkazu. Vzhledem k tomu, že je kontejner image umístěná v privátní registru kontejner Azure, musí zahrnovat přihlašovací údaje potřebné pro přístup k ní. Získejte přihlašovací údaje pomocí následujících příkazů příkazového řádku Azure CLI.

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
az container create --resource-group myResourceGroup --name aci-tutorial-app --image <acrLoginServer>/aci-tutorial-app:v1 --cpu 1 --memory 1 --registry-password <acrPassword> --ip-address public --ports 80
```

Během pár sekund měli byste obdržet počáteční reakce ze Správce prostředků Azure. Chcete-li zobrazit stav nasazení, použijte [az kontejneru zobrazit][az-container-show]:

```azurecli
az container show --resource-group myResourceGroup --name aci-tutorial-app --query instanceView.state
```

Opakujte [az kontejneru zobrazit] [ az-container-show] příkaz, dokud se stav změní z *čekající* k *systémem*, která by za několik minut trvat. Pokud je kontejner *systémem*, přejděte k dalšímu kroku.

## <a name="view-the-application-and-container-logs"></a>Zobrazit protokoly aplikací a kontejneru

Po úspěšné nasazení, zobrazí kontejneru veřejnou IP adresu s [az kontejneru zobrazit] [ az-container-show] příkaz:

```bash
az container show --resource-group myResourceGroup --name aci-tutorial-app --query ipAddress.ip
```

Příklad výstupu:`"13.88.176.27"`

Informace o běžící aplikaci, přejděte na veřejnou IP adresu v oblíbeném prohlížeči.

![Hello world aplikace v prohlížeči][aci-app-browser]

Můžete také zobrazit výstup protokolu kontejneru:

```azurecli
az container logs --resource-group myResourceGroup --name aci-tutorial-app
```

Výstup:

```bash
listening on port 80
::ffff:10.240.0.4 - - [21/Jul/2017:06:00:02 +0000] "GET / HTTP/1.1" 200 1663 "-" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_12_5) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/59.0.3071.115 Safari/537.36"
::ffff:10.240.0.4 - - [21/Jul/2017:06:00:02 +0000] "GET /favicon.ico HTTP/1.1" 404 150 "http://13.88.176.27/" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_12_5) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/59.0.3071.115 Safari/537.36"
```

## <a name="clean-up-resources"></a>Vyčištění prostředků

Pokud již nepotřebujete žádné prostředky vytvořené z této série kurz, můžete provést [odstranění skupiny az] [ az-group-delete] příkaz k odebrání skupiny prostředků a všechny prostředky, které obsahuje. Tento příkaz odstraní kontejner registru, kterou jste vytvořili, a také kontejneru spuštěné a všechny související prostředky.

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Další postup

V tomto kurzu jste dokončili proces nasazení kontejnerů do Azure kontejner instancí. Dokončili jste následující kroky:

> [!div class="checklist"]
> * Nasazení kontejneru z registru kontejner Azure pomocí rozhraní příkazového řádku Azure
> * Zobrazit aplikaci v prohlížeči
> * Zobrazit protokoly kontejneru

<!-- IMAGES -->
[aci-app-browser]: ./media/container-instances-quickstart/aci-app-browser.png

<!-- LINKS - external -->
[docker-linux]: https://docs.docker.com/engine/installation/#supported-platforms
[docker-login]: https://docs.docker.com/engine/reference/commandline/login/
[docker-mac]: https://docs.docker.com/docker-for-mac/
[docker-push]: https://docs.docker.com/engine/reference/commandline/push/
[docker-tag]: https://docs.docker.com/engine/reference/commandline/tag/
[docker-windows]: https://docs.docker.com/docker-for-windows/

<!-- LINKS - internal -->
[az-container-show]: /cli/azure/container#az_container_show
[az-group-delete]: /cli/azure/group#az_group_delete
[azure-cli-install]: /cli/azure/install-azure-cli
[prepare-app]: ./container-instances-tutorial-prepare-app.md