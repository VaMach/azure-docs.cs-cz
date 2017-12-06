---
title: "Nasazení do Azure kontejner instancí z registru kontejner Azure"
description: "Nasazení Azure kontejner instancí z registru kontejner Azure"
services: container-instances
author: seanmck
manager: timlt
ms.service: container-instances
ms.topic: article
ms.date: 08/02/2017
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: 9667a5b840d6c1fab5087cfcf3ede34a732fbe01
ms.sourcegitcommit: a48e503fce6d51c7915dd23b4de14a91dd0337d8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/05/2017
---
# <a name="deploy-to-azure-container-instances-from-the-azure-container-registry"></a>Nasazení do Azure kontejner instancí z registru kontejner Azure

Registr kontejner Azure je založené na Azure, privátní registru, pro kontejner Docker bitové kopie. Tento článek vysvětluje postup nasazení bitových kopií kontejneru uložené v registru kontejner Azure do Azure kontejner instancí.

## <a name="using-the-azure-cli"></a>Použití Azure CLI

Rozhraní příkazového řádku Azure obsahuje příkazy pro vytváření a Správa kontejnerů v Azure kontejner instancí. Pokud zadáte privátní bitové kopie v `create` příkazu, můžete také zadat heslo registru image vyžadovaný k ověření s registrem kontejneru.

```azurecli-interactive
az container create --name myprivatecontainer --image mycontainerregistry.azurecr.io/mycontainerimage:v1 --registry-password myRegistryPassword --resource-group myresourcegroup
```

`create` Příkaz také podporuje určení `registry-login-server` a `registry-username`. Však server přihlášení pro registru kontejner Azure, je vždycky *registryname*. azurecr.io a výchozí uživatelské jméno je *registryname*, takže pokud není, jsou tyto hodnoty odvodit z název bitové kopie explicitně zadat.

## <a name="using-an-azure-resource-manager-template"></a>Pomocí šablony Azure Resource Manager

Můžete zadat vlastnosti registr kontejner Azure v šablonu Azure Resource Manager včetně `imageRegistryCredentials` vlastnost v definici skupiny kontejneru:

```json
"imageRegistryCredentials": [
  {
    "server": "imageRegistryLoginServer",
    "username": "imageRegistryUsername",
    "password": "imageRegistryPassword"
  }
]
```

Abyste se vyhnuli ukládání heslo registru kontejneru přímo v šabloně, doporučujeme uložit jako tajný klíč v [Azure Key Vault](../key-vault/key-vault-manage-with-cli2.md) a odkazovat pomocí šablony [nativní integrace mezi službou Azure Resource Manager a trezoru klíčů](../azure-resource-manager/resource-manager-keyvault-parameter.md).

## <a name="using-the-azure-portal"></a>Použití webu Azure Portal

Pokud chcete zachovat kontejneru bitové kopie v registru kontejner Azure, můžete snadno vytvořit kontejner v instancí kontejneru Azure pomocí portálu Azure.

1. Na portálu Azure přejděte do kontejneru registr.

2. Vyberte úložiště.

    ![V nabídce registru kontejner Azure na portálu Azure][acr-menu]

3. Vyberte, kterou chcete nasadit z úložiště.

4. Klikněte pravým tlačítkem na značce pro kontejner bitovou kopii, kterou chcete nasadit.

    ![Kontextové nabídky pro spuštění kontejner s instancemi Azure kontejneru][acr-runinstance-contextmenu]

5. Zadejte název kontejneru a název skupiny prostředků. Pokud chcete můžete taky změnit výchozí hodnoty.

    ![Vytvořit nabídku pro instance kontejner Azure][acr-create-deeplink]

6. Po dokončení nasazení můžete přejít do kontejneru skupiny z podokna oznámení najít IP adresu a další vlastnosti.

    ![Zobrazení podrobností pro skupinu kontejner instancí kontejnerů Azure][aci-detailsview]

## <a name="next-steps"></a>Další kroky

Naučte se vytvářet kontejnery, vložit je privátní kontejneru registru a jejich nasazení do instance kontejner Azure podle [dokončení tohoto kurzu](container-instances-tutorial-prepare-app.md).

<!-- IMAGES -->
[acr-menu]: ./media/container-instances-using-azure-container-registry/acr-menu.png

[acr-runinstance-contextmenu]: ./media/container-instances-using-azure-container-registry/acr-runinstance-contextmenu.png

[acr-create-deeplink]: ./media/container-instances-using-azure-container-registry/acr-create-deeplink.png

[aci-detailsview]: ./media/container-instances-using-azure-container-registry/aci-detailsview.png
