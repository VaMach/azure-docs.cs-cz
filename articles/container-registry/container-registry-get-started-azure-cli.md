---
title: "Rychlý start - vytvořit privátní registru Docker v Azure pomocí rozhraní příkazového řádku Azure"
description: "Rychle se Naučte se vytvořit privátní registru kontejner Docker pomocí Azure CLI."
services: container-registry
author: neilpeterson
manager: timlt
ms.service: container-registry
ms.topic: quicksart
ms.date: 12/07/2017
ms.author: nepeters
ms.custom: H1Hack27Feb2017, mvc
ms.openlocfilehash: f31f4e5e2b3fe5db85873894a7f2fa9c415392c1
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/08/2017
---
# <a name="create-a-container-registry-using-the-azure-cli"></a>Vytvoření registru kontejnerů pomocí Azure CLI

Azure Container Registry je spravovaná služba registru kontejnerů Dockeru sloužící k ukládání privátních imagí kontejnerů Dockeru. Tato příručka údaje vytvoření instance registru kontejner Azure pomocí rozhraní příkazového řádku Azure.

Tento rychlý start vyžaduje, že používáte Azure CLI verze 2.0.21 nebo novější. Verzi zjistíte spuštěním příkazu `az --version`. Pokud je potřeba nainstalovat nebo upgradovat najdete v tématu [nainstalovat Azure CLI 2.0][azure-cli].

Je také nutné mít Docker nainstalovány místně. Docker poskytuje balíčky, které můžete snadno konfigurovat Docker na žádném [Mac][docker-mac], [Windows][docker-windows], nebo [Linux] [ docker-linux] systému.

## <a name="create-a-resource-group"></a>Vytvoření skupiny prostředků

Vytvořte skupinu prostředků pomocí příkazu [az group create][az-group-create]. Skupina prostředků Azure je logický kontejner, ve kterém se nasazují a spravují prostředky Azure.

Následující příklad vytvoří skupinu prostředků *myResourceGroup* v umístění *eastus*.

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

## <a name="create-a-container-registry"></a>Vytvoření registru kontejnerů

V tento rychlý start, vytvoříme *základní* registru. Azure kontejneru registru je k dispozici v několika různých SKU stručně popsány v následující tabulce. Rozšířené informace o jednotlivých v [kontejneru registru SKU][container-registry-skus].

[!INCLUDE [container-registry-sku-matrix](../../includes/container-registry-sku-matrix.md)]

Vytvořte instanci ACR pomocí [vytvořit az acr] [ az-acr-create] příkaz.

Název registru **musí být jedinečné**. V následujícím příkladu *myContainerRegistry007* se používá. Aktualizujte na jedinečnou hodnotu.

```azurecli
az acr create --resource-group myResourceGroup --name myContainerRegistry007 --sku Basic
```

Po vytvoření registru je výstup podobný tomuto:

```json
{
  "adminUserEnabled": false,
  "creationDate": "2017-09-08T22:32:13.175925+00:00",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.ContainerRegistry/registries/myContainerRegistry007",
  "location": "eastus",
  "loginServer": "myContainerRegistry007.azurecr.io",
  "name": "myContainerRegistry007",
  "provisioningState": "Succeeded",
  "resourceGroup": "myResourceGroup",
  "sku": {
    "name": "Basic",
    "tier": "Basic"
  },
  "status": null,
  "storageAccount": null,
  "tags": {},
  "type": "Microsoft.ContainerRegistry/registries"
}
```

Po celý zbytek tento rychlý start, používáme `<acrName>` jako zástupný symbol pro název kontejneru registru.

## <a name="log-in-to-acr"></a>Přihlaste se k ACR

Před odesíláním a vyžadováním imagí kontejnerů se musíte přihlásit k instanci služby ACR. Chcete-li to provést, použijte [az acr přihlášení] [ az-acr-login] příkaz.

```azurecli
az acr login --name <acrName>
```

Příkaz vrátí `Login Succeeded` zprávu po dokončení.

## <a name="push-image-to-acr"></a>Nabízená ACR bitové kopie

Tak, aby nabízel bitovou kopii registru kontejneru Azure, musíte nejprve mít bitovou kopii. Pokud ještě nemáte žádné místní kontejneru bitové kopie, spusťte následující příkaz načítat stávající image z úložiště Docker Hub.

```bash
docker pull microsoft/aci-helloworld
```

Předtím, než můžete posouvat bitovou kopii do registru, musíte ji označit s plně kvalifikovaný název ACR přihlášení serveru. Spusťte následující příkaz k získání úplné přihlašovací jméno serveru ACR instance.

```azurecli
az acr list --resource-group myResourceGroup --query "[].{acrLoginServer:loginServer}" --output table
```

Značka pomocí bitové kopie [docker značka] [ docker-tag] příkaz. Nahraďte `<acrLoginServer>` s přihlašovacím jménem serveru vaší instance ACR.

```bash
docker tag microsoft/aci-helloworld <acrLoginServer>/aci-helloworld:v1
```

Nakonec použijte [docker nabízené] [ docker-push] tak, aby nabízel bitovou kopii k instanci ACR. Nahraďte `<acrLoginServer>` s přihlašovacím jménem serveru vaší instance ACR.

```bash
docker push <acrLoginServer>/aci-helloworld:v1
```

## <a name="list-container-images"></a>Výpis imagí kontejnerů

Následující příklad uvádí úložiště v registru:

```azurecli
az acr repository list --name <acrName> --output table
```

Výstup:

```bash
Result
----------------
aci-helloworld
```

Následující příklad vypíše značky na **aci helloworld** úložiště.

```azurecli
az acr repository show-tags --name <acrName> --repository aci-helloworld --output table
```

Výstup:

```bash
Result
--------
v1
```

## <a name="clean-up-resources"></a>Vyčištění prostředků

Pokud již nepotřebujete, můžete použít [odstranění skupiny az] [ az-group-delete] příkaz k odebrání skupiny prostředků, ACR instanci a všechny Image kontejneru.

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Další kroky

V tento rychlý start vytvořili kontejner registru Azure pomocí Azure CLI. Pokud chcete používat registru kontejner Azure s Azure kontejner instancí, nadále kurzu instancí kontejnerů Azure.

> [!div class="nextstepaction"]
> [Kurz pro Azure instancí kontejnerů][container-instances-tutorial-prepare-app]

<!-- LINKS - external -->
[docker-linux]: https://docs.docker.com/engine/installation/#supported-platforms
[docker-login]: https://docs.docker.com/engine/reference/commandline/login/
[docker-mac]: https://docs.docker.com/docker-for-mac/
[docker-push]: https://docs.docker.com/engine/reference/commandline/push/
[docker-tag]: https://docs.docker.com/engine/reference/commandline/tag/
[docker-windows]: https://docs.docker.com/docker-for-windows/

<!-- LINKS - internal -->
[az-acr-create]: /cli/azure/acr#az_acr_create
[az-acr-login]: /cli/azure/acr#az_acr_login
[az-group-create]: /cli/azure/group#az_group_create
[az-group-delete]: /cli/azure/group#az_group_delete
[azure-cli]: /cli/azure/install-azure-cli
[container-instances-tutorial-prepare-app]: ../container-instances/container-instances-tutorial-prepare-app.md
[container-registry-skus]: container-registry-skus.md