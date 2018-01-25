---
title: "Nasadit do Azure kontejner instancí z registru kontejner Azure"
description: "Informace o nasazení kontejnerů v Azure kontejner instancí pomocí bitové kopie kontejneru v registru kontejneru služby Azure."
services: container-instances
author: seanmck
manager: timlt
ms.service: container-instances
ms.topic: article
ms.date: 01/24/2018
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: c69b95f66bf2eaf4975961da5b25f5ac6172798c
ms.sourcegitcommit: 79683e67911c3ab14bcae668f7551e57f3095425
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/25/2018
---
# <a name="deploy-to-azure-container-instances-from-azure-container-registry"></a>Nasadit do Azure kontejner instancí z registru kontejner Azure

Registr kontejner Azure je založené na Azure, privátní registru, pro kontejner Docker bitové kopie. Tento článek vysvětluje postup nasazení bitových kopií kontejneru uložené v registru kontejner Azure do Azure kontejner instancí.

## <a name="deploy-with-azure-cli"></a>Nasazení pomocí rozhraní příkazového řádku Azure

Rozhraní příkazového řádku Azure obsahuje příkazy pro vytváření a Správa kontejnerů v Azure kontejner instancí. Pokud zadáte privátní bitové kopie v [vytvořit kontejner az] [ az-container-create] příkazu, můžete také zadat heslo registru image vyžadovaný k ověření s registrem kontejneru.

```azurecli-interactive
az container create --resource-group myResourceGroup --name myprivatecontainer --image mycontainerregistry.azurecr.io/mycontainerimage:v1 --registry-password myRegistryPassword
```

[Vytvořit kontejner az] [ az-container-create] příkaz také podporuje určení `--registry-login-server` a `--registry-username`. Však server přihlášení pro registru kontejner Azure, je vždycky *registryname*. azurecr.io a výchozí uživatelské jméno je *registryname*, takže pokud není, jsou tyto hodnoty odvodit z název bitové kopie explicitně zadat.

## <a name="deploy-with-azure-resource-manager-template"></a>Nasazení pomocí šablony Azure Resource Manageru

Můžete zadat vlastnosti registr kontejner Azure v šablonu Azure Resource Manager včetně `imageRegistryCredentials` vlastnost v definici skupiny kontejneru:

```JSON
"imageRegistryCredentials": [
  {
    "server": "imageRegistryLoginServer",
    "username": "imageRegistryUsername",
    "password": "imageRegistryPassword"
  }
]
```

Abyste se vyhnuli ukládání heslo registru kontejneru přímo v šabloně, doporučujeme uložit jako tajný klíč v [Azure Key Vault](../key-vault/key-vault-manage-with-cli2.md) a odkazovat pomocí šablony [nativní integrace mezi službou Azure Resource Manager a trezoru klíčů](../azure-resource-manager/resource-manager-keyvault-parameter.md).

## <a name="deploy-with-azure-portal"></a>Nasazení pomocí portálu Azure

Pokud chcete zachovat kontejneru bitové kopie v registru kontejner Azure, můžete snadno vytvořit kontejner v instancí kontejneru Azure pomocí portálu Azure.

1. Na portálu Azure přejděte do kontejneru registr.

1. Vyberte **úložiště**, pak vyberte úložiště, který chcete nasadit, klikněte pravým tlačítkem na značce pro kontejner bitovou kopii, kterou chcete nasadit a vyberte **spustí instanci**.

    !["Spustit instance" v registru kontejner Azure na portálu Azure][acr-runinstance-contextmenu]

1. Zadejte název kontejneru a název skupiny prostředků. Pokud chcete můžete taky změnit výchozí hodnoty.

    ![Vytvořit nabídku pro instance kontejner Azure][acr-create-deeplink]

1. Po dokončení nasazení můžete přejít do kontejneru skupiny z podokna oznámení najít IP adresu a další vlastnosti.

    ![Zobrazení podrobností pro skupinu kontejner instancí kontejnerů Azure][aci-detailsview]

## <a name="service-principal-authentication"></a>Ověřování instančních objektů

Pokud správce pro kontejner Azure registru je zakázaná, můžete použít Azure Active Directory [instanční objekt](../container-registry/container-registry-auth-service-principal.md) k ověření registru při vytváření instance kontejneru. Ověřování pomocí hlavní název služby je také vhodné v bezobslužných scénáře, jako je skript nebo aplikaci, která vytvoří kontejner instancí v bezobslužném.

Další informace najdete v tématu [ověřit pomocí registru kontejner Azure z instancí kontejneru Azure](../container-registry/container-registry-auth-aci.md).

## <a name="next-steps"></a>Další postup

Naučte se vytvářet kontejnery, vložit je privátní kontejneru registru a jejich nasazení do instance kontejner Azure podle [dokončení tohoto kurzu](container-instances-tutorial-prepare-app.md).

<!-- IMAGES -->
[acr-create-deeplink]: ./media/container-instances-using-azure-container-registry/acr-create-deeplink.png
[aci-detailsview]: ./media/container-instances-using-azure-container-registry/aci-detailsview.png
[acr-runinstance-contextmenu]: ./media/container-instances-using-azure-container-registry/acr-runinstance-contextmenu.png

<!-- LINKS - Internal -->
[az-container-create]: /cli/azure/container?view=azure-cli-latest#az_container_create