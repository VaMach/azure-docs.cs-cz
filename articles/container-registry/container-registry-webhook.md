---
title: Azure webhooky registru kontejneru
description: "Další informace o použití webhooky aktivační události, pokud dojde k určité akce ve vaší registru úložiště."
services: container-registry
documentationcenter: 
author: neilpeterson
manager: timlt
editor: 
tags: acr, azure-container-registry
keywords: Docker, kontejnery, ACR
ms.assetid: 
ms.service: container-registry
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/08/2017
ms.author: nepeters
ms.openlocfilehash: 5a9dab91aafb92f944b473f05144242143e36477
ms.sourcegitcommit: ccb84f6b1d445d88b9870041c84cebd64fbdbc72
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/14/2017
---
# <a name="using-azure-container-registry-webhooks"></a>Pomocí webhooků registru kontejner Azure

Azure kontejneru registru uchovává a spravuje privátní kontejner imagí Dockeru, podobně jako úložiště Docker Hub ukládá veřejné imagí Dockeru. Můžete použít webhooky aktivační události při určité akce provést v jednom z vašeho úložiště registru. Webhooky můžete reakce na události na úrovni registru, nebo může být snížit značku konkrétní úložiště.

Pozadí a koncepty najdete v tématu [o Azure kontejneru registru](./container-registry-intro.md).

## <a name="prerequisites"></a>Požadavky

- Kontejner Azure spravované registru - vytvořit spravované kontejneru registru ve vašem předplatném Azure. Například použít [portál Azure](container-registry-get-started-portal.md) nebo [rozhraní příkazového řádku Azure](container-registry-get-started-azure-cli.md).
- Instalace rozhraní příkazového řádku - nastavení místního počítače jako hostitele Docker a přístup k příkazy příkazového řádku Dockeru docker [modulu Docker](https://docs.docker.com/engine/installation/).

## <a name="create-webhook-azure-portal"></a>Vytvoření webhooku portálu Azure

1. Přihlaste se k [portál Azure](https://portal.azure.com) a přejděte do registru, ve kterém chcete vytvořit webhooky.

2. V okně kontejner vyberte **Webhooky** pod **služby**.

3. Vyberte **přidat** na panelu nástrojů okna webhooku.

4. Dokončení *vytvoření webhooku* formuláře s následujícími informacemi:

| Hodnota | Popis |
|---|---|
| Name (Název) | Název, který chcete poskytnout k webhooku. Může obsahovat jenom malá písmena a čísla a musí být 5 – 50 znaků. |
| URI služby | Identifikátor URI, kde má webhooku odeslat oznámení POST. |
| Vlastní hlavičky | Hlavičky, kterou chcete předat společně s požadavek POST. Musí být v "klíč: hodnota" formátu. |
| Akce aktivace | Akce, které aktivují webhooku. Webhooky mohou být aktivovány nabízené bitové kopie nebo odstranění akcí. |
| Status | Stav webhooku po jejím vytvoření. Ve výchozím nastavení je povolená. |
| Rozsah | Obor, ve kterém funguje webhooku. Ve výchozím nastavení rozsah je pro všechny události v registru. Můžete zadat pro úložiště nebo značku ve formátu "úložiště: značka". |

Příklad webhooku formuláře:

![Vytvoření webhooku ACR uživatelského rozhraní na portálu Azure](./media/container-registry-webhook/webhook.png)

## <a name="create-webhook-azure-cli"></a>Vytvoření webhooku rozhraní příkazového řádku Azure

Chcete-li vytvořit webhook, jehož pomocí rozhraní příkazového řádku Azure, použijte [vytvoření webhooku acr az](/cli/azure/acr/webhook#create) příkaz.

```azurecli-interactive
az acr webhook create --registry mycontainerregistry --name myacrwebhook01 --actions delete --uri http://webhookuri.com
```

## <a name="test-webhook"></a>Test webhooku

### <a name="azure-portal"></a>portál Azure

Před použitím webhooku na kontejneru bitové kopie nabízené a odstranění akcí, můžete otestovat ji s **Ping** tlačítko. Příkaz ping odešle požadavek POST obecné zadaný koncový bod a protokoly odpovědi. Můžete ověřit, zda že jste správně nakonfigurovali webhooku.

1. Vyberte webhooku, kterou chcete otestovat.
2. V horním panelu nástrojů vyberte **Ping**.
3. Zkontrolujte v odpovědi pro koncový bod **stav protokolu HTTP** sloupce.

![Vytvoření webhooku ACR uživatelského rozhraní na portálu Azure](./media/container-registry-webhook/webhook-02.png)

### <a name="azure-cli"></a>Azure CLI

Chcete-li otestovat webhook ACR pomocí Azure CLI, použijte [az acr webhooku ping](/cli/azure/acr/webhook#ping) příkaz.

```azurecli-interactive
az acr webhook ping --registry mycontainerregistry --name myacrwebhook01
```

Pokud chcete zobrazit výsledky, použijte [az acr webhooku seznam události](/cli/azure/acr/webhook#list-events) příkaz.

```azurecli-interactive
az acr webhook list-events --registry mycontainerregistry08 --name myacrwebhook01
```

## <a name="delete-webhook"></a>Odstranit webhook

### <a name="azure-portal"></a>portál Azure

Každý webhooku odstraníte výběrem webhooku a potom **odstranit** tlačítka na portálu Azure.

### <a name="azure-cli"></a>Azure CLI

```azurecli-interactive
az acr webhook delete --registry mycontainerregistry --name myacrwebhook01
```
