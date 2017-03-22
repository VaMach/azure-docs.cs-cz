---
title: "Vytvoření první webové aplikace v Javě v Azure během pěti minut | Dokumentace Microsoftu"
description: "Nasazením ukázkové aplikace zjistíte, jak snadné je spustit webové aplikace ve službě App Service."
services: app-service\web
documentationcenter: 
author: cephalin
manager: wpickett
editor: 
ms.assetid: 8bacfe3e-7f0b-4394-959a-a88618cb31e1
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 03/08/2017
ms.author: cephalin
translationtype: Human Translation
ms.sourcegitcommit: 97acd09d223e59fbf4109bc8a20a25a2ed8ea366
ms.openlocfilehash: e48e03e86a325b8f39809a49cdd19820dfa78bdc
ms.lasthandoff: 03/10/2017


---
# <a name="create-your-first-java-web-app-in-azure-in-five-minutes"></a>Vytvoření první webové aplikace v Javě v Azure během pěti minut
[!INCLUDE [app-service-web-selector-get-started](../../includes/app-service-web-selector-get-started.md)]

Tento Rychlý start vám pomůže nasadit první webovou aplikaci v Javě do služby [Azure App Service](../app-service/app-service-value-prop-what-is.md) během pár minut.

Před zahájením tohoto Rychlého startu zajistěte, že je na vašem počítači [nainstalované rozhraní příkazového řádku Azure CLI](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli).

## <a name="create-a-java-web-app-in-azure"></a>Vytvoření webové aplikace v Javě v Azure
2. Přihlaste se k Azure spuštěním příkazu `az login` a postupem podle pokynů na obrazovce.
   
    ```azurecli
    az login
    ```
   
3. Vytvořte [skupinu prostředků](../azure-resource-manager/resource-group-overview.md). To je místo, kam dáte všechny prostředky Azure, které chcete spravovat společně, jako je webová aplikace a její back-end SQL Database.

    ```azurecli
    az group create --location "West Europe" --name myResourceGroup
    ```

    K zobrazení možných hodnot, které se dají použít pro `---location`, použijte příkaz `az appservice list-locations` rozhraní příkazového řádku Azure CLI.

3. Vytvořte [plán služby App Service](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) Free. 

    ```azurecli
    az appservice plan create --name my-free-appservice-plan --resource-group myResourceGroup --sku FREE
    ```

4. Vytvořte novou webovou aplikaci s jedinečným názvem ve značce `<app_name>`.

    ```azurecli
    az appservice web create --name <app_name> --resource-group myResourceGroup --plan my-free-appservice-plan
    ```

4. Nasaďte ukázkovou aplikaci v Javě z GitHubu.

    ```azurecli
    az appservice web source-control config --name <app_name> --resource-group myResourceGroup \
    --repo-url "https://github.com/azure-appservice-samples/JavaCoffeeShopTemplate.git" --branch master --manual-integration 
    ```


5. Pokud chcete vidět svou aplikaci živě běžet v Azure, spusťte tento příkaz.

    ```azurecli
    az appservice web browse --name <app_name> --resource-group myResourceGroup
    ```

Blahopřejeme, vaše první webová aplikace v Javě běží živě v Azure App Service.

[!INCLUDE [cli-samples-clean-up](../../includes/cli-samples-clean-up.md)]

## <a name="next-steps"></a>Další kroky

Prozkoumejte předem vytvořené [skripty rozhraní příkazového řádku pro webové aplikace](app-service-cli-samples.md).

