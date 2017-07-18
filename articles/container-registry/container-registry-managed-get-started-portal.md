---
title: "Vytvoření soukromého registru Dockeru – Azure Portal | Dokumentace Microsoftu"
description: "Začínáme vytvářet a spravovat soukromé registry kontejnerů Dockeru pomocí webu Azure Portal"
services: container-registry
documentationcenter: 
author: neilpeterson
manager: timlt
editor: na
tags: 
keywords: 
ms.assetid: 53a3b3cb-ab4b-4560-bc00-366e2759f1a1
ms.service: container-registry
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/11/2017
ms.author: nepeters
ms.custom: na
ms.translationtype: HT
ms.sourcegitcommit: 2ad539c85e01bc132a8171490a27fd807c8823a4
ms.openlocfilehash: 560aee42b0c5a61c37c594d7937f833ab7183d49
ms.contentlocale: cs-cz
ms.lasthandoff: 07/12/2017

---

# <a name="create-a-managed-container-registry-using-the-azure-portal"></a>Vytvoření spravovaného registru kontejnerů pomocí webu Azure Portal

Azure Container Registry je spravovaná služba registru kontejnerů Dockeru sloužící k ukládání privátních imagí kontejnerů Dockeru. Tato příručka podrobně popisuje vytvoření spravované instance služby Azure Container Registry pomocí webu Azure Portal.

Spravované registry kontejnerů Azure jsou ve verzi Preview a nejsou dostupné ve všech oblastech.

## <a name="log-in-to-azure"></a>Přihlaste se k Azure.

Přihlaste se k webu Azure Portal na adrese http://portal.azure.com.

## <a name="create-a-container-registry"></a>Vytvoření registru kontejnerů

1. Klikněte na tlačítko **Nový** v levém horním rohu portálu Azure Portal.

2. Na webu Marketplace vyhledejte a vyberte **Azure Container Registry**.

3. Klikněte na **Vytvořit**. Otevře se okno pro vytvoření služby ACR.

    ![Nastavení registru kontejnerů](./media/container-registry-get-started-portal/managed-container-registry-settings.png)

4. V okně **Azure Container Registry** zadejte následující informace. Až budete hotovi, klikněte na **Vytvořit**.

    a. **Název registru:** Globálně jedinečný název domény nejvyšší úrovně konkrétního registru. V tomto příkladu je název registru *myAzureContainerRegistry1*, ale nahraďte jej vlastním jedinečným názvem. Název může obsahovat pouze písmena a číslice.

    b. **Skupina prostředků:** Vyberte existující [skupinu prostředků](../azure-resource-manager/resource-group-overview.md#resource-groups) nebo zadejte název nové skupiny prostředků.

    c. **Umístění:** Vyberte umístění datového centra Azure, ve kterém je tato služba [dostupná](https://azure.microsoft.com/regions/services/), například **Střed USA – jih**.

    d. **Uživatel s právy pro správu:** Pokud chcete, povolte uživateli s právy pro správu přístup k registru. Toto nastavení můžete po vytvoření registru změnit.

    e. **Použít spravovaný registr:** Vyberte Ano, pokud chcete, aby služba ACR automaticky spravovala úložiště registru a používala webhooky a ověřování AAD.

    f. **Cenová úroveň:** Vyberte cenovou úroveň; další informace najdete tady v cenách služby ACR.

## <a name="log-in-to-acr-instance"></a>Přihlášení k instanci služby ACR

Před odesíláním a vyžadováním imagí kontejnerů se musíte přihlásit k instanci služby ACR. 

Použijte k tomu Azure CLI 2.0. Nejprve se pomocí příkazu [az login](/cli/azure/#login) přihlaste do Azure, pokud je to potřeba. 

```azurecli
az login
```

Potom se pomocí příkazu [az acr login](/cli/azure/acr#login) přihlaste ke službě Azure Container Registry.

```azurecli-interactive
az acr login --name myAzureContainerRegistry1
```

## <a name="use-azure-container-registry"></a>Použití služby Azure Container Registry

### <a name="list-container-images"></a>Výpis imagí kontejnerů

Pomocí příkazů rozhraní příkazového řádku `az acr` se můžete dotazovat na obrázky a značky v úložišti.

> [!NOTE]
> Container Registry v současné době nepodporuje použití příkazu `docker search` k dotazování obrázků a značek.

### <a name="list-repositories"></a>Výpis úložišť

Následující příklad zobrazí seznam úložišť v registru ve formátu JSON (JavaScript Object Notation):

```azurecli
az acr repository list -n myContainerRegistry1 -o json
```

### <a name="list-tags"></a>Výpis značek

Následující příklad zobrazí seznam značek na úložišti **samples/nginx** ve formátu JSON:

```azurecli
az acr repository show-tags -n myContainerRegistry1 --repository samples/nginx -o json
```

## <a name="next-steps"></a>Další kroky

V tomto rychlém startu jste vytvořili spravovanou instanci služby Azure Container Registry pomocí webu Azure Portal.

> [!div class="nextstepaction"]
> [Nahrání první image pomocí rozhraní příkazového řádku Dockeru](container-registry-get-started-docker-cli.md)
