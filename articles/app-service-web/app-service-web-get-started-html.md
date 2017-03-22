---
title: "Vytvoření webové aplikace ve statickém HTML v Azure během pěti minut | Dokumentace Microsoftu"
description: "Nasazením ukázkové aplikace zjistíte, jak snadné je spustit webové aplikace ve službě App Service."
services: app-service\web
documentationcenter: 
author: cephalin
manager: wpickett
editor: 
ms.assetid: 60495cc5-6963-4bf0-8174-52786d226c26
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 03/08/2017
ms.author: cephalin
translationtype: Human Translation
ms.sourcegitcommit: a087df444c5c88ee1dbcf8eb18abf883549a9024
ms.openlocfilehash: 3a618e4dfed5e941fb0200f4f21a6a45e33d948e
ms.lasthandoff: 03/15/2017


---
# <a name="create-a-static-html-web-app-in-azure-in-five-minutes"></a>Vytvoření webové aplikace ve statickém HTML v Azure během pěti minut
[!INCLUDE [app-service-web-selector-get-started](../../includes/app-service-web-selector-get-started.md)] 

Tento Rychlý start vám pomůže nasadit jednoduchý web HTML+CSS do služby [Azure App Service](../app-service/app-service-value-prop-what-is.md) během pár minut.

Před zahájením tohoto Rychlého startu zajistěte, že je na vašem počítači [nainstalované rozhraní příkazového řádku Azure CLI](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli).

## <a name="create-a-static-html-site-in-azure"></a>Vytvoření webu ve statickém HTML v Azure
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

4. Nasaďte ukázkový web HTML z Githubu.

    ```azurecli
    az appservice web source-control config --name <app_name> --resource-group myResourceGroup \
    --repo-url "https://github.com/Azure-Samples/app-service-web-html-get-started.git" --branch master --manual-integration 
    ```

5. Pokud chcete vidět svou aplikaci živě běžet v Azure, spusťte tento příkaz.

    ```azurecli
    az appservice web browse --name <app_name> --resource-group myResourceGroup
    ```

Blahopřejeme, váš první web ve statickém HTML běží živě v Azure App Service.

[!INCLUDE [cli-samples-clean-up](../../includes/cli-samples-clean-up.md)]

## <a name="next-steps"></a>Další kroky

Prozkoumejte předem vytvořené [skripty rozhraní příkazového řádku pro webové aplikace](app-service-cli-samples.md).

