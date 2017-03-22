---
title: "Vytvoření první webové aplikace v PHP v Azure během pěti minut | Dokumentace Microsoftu"
description: "Nasazením ukázkové aplikace v PHP zjistíte, jak snadné je spustit webové aplikace ve službě App Service."
services: app-service\web
documentationcenter: 
author: cephalin
manager: wpickett
editor: 
ms.assetid: 6feac128-c728-4491-8b79-962da9a40788
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 03/08/2017
ms.author: cephalin
translationtype: Human Translation
ms.sourcegitcommit: a087df444c5c88ee1dbcf8eb18abf883549a9024
ms.openlocfilehash: 9a756618549cafc41c4f09683fd710748bf7b411
ms.lasthandoff: 03/15/2017


---
# <a name="create-your-first-php-web-app-in-azure-in-five-minutes"></a>Vytvoření první webové aplikace v PHP v Azure během pěti minut
[!INCLUDE [app-service-web-selector-get-started](../../includes/app-service-web-selector-get-started.md)]

Tento Rychlý start vám pomůže nasadit první webovou aplikaci v PHP do služby [Azure App Service](../app-service/app-service-value-prop-what-is.md) během pár minut.

Před zahájením tohoto Rychlého startu zajistěte, že je na vašem počítači [nainstalované rozhraní příkazového řádku Azure CLI](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli).

## <a name="create-a-php-web-app-in-azure"></a>Vytvoření webové aplikace v PHP v Azure
   
2. Přihlaste se k Azure spuštěním příkazu `az login` a postupem podle pokynů na obrazovce.
   
    ```azurecli
    az login
    ```
   
3. Vytvořte [skupinu prostředků](../azure-resource-manager/resource-group-overview.md). To je místo, kam dáte všechny prostředky Azure, které chcete spravovat společně, jako je webová aplikace a její back-end SQL Database.

    ```azurecli
    az group create --location "West Europe" --name myResourceGroup
    ```

    K zobrazení možných hodnot, které se dají použít pro `---location`, použijte příkaz `az appservice list-locations` rozhraní příkazového řádku Azure CLI.

3. Vytvořte [plán služby App Service](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) Standard. Úroveň Standard je nutná ke spuštění kontejnerů Linuxu.

    ```azurecli
    az appservice plan create --name my-free-appservice-plan --resource-group myResourceGroup --sku S1 --is-linux 
    ```

4. Vytvořte webovou aplikaci s jedinečným názvem ve značce `<app_name>`.

    ```azurecli
    az appservice web create --name <app_name> --resource-group myResourceGroup --plan my-free-appservice-plan
    ```

4. Nakonfigurujte kontejner Linuxu, aby používal výchozí image PHP 7.0.6.

    ```azurecli
    az appservice web config update --php-version 7.0.6 --name <app_name> --resource-group myResourceGroup
    ```

4. Nasaďte ukázkovou aplikaci v PHP z GitHubu.

    ```azurecli
    az appservice web source-control config --name <app_name> --resource-group myResourceGroup \
    --repo-url "https://github.com/Azure-Samples/app-service-web-php-get-started.git" --branch master --manual-integration 
    ```

5. Pokud chcete vidět svou aplikaci živě běžet v Azure, spusťte tento příkaz.

    ```azurecli
    az appservice web browse --name <app_name> --resource-group myResourceGroup
    ```

Blahopřejeme, vaše první webová aplikace v PHP běží živě v Azure App Service.

[!INCLUDE [cli-samples-clean-up](../../includes/cli-samples-clean-up.md)]

## <a name="next-steps"></a>Další kroky

Prozkoumejte předem vytvořené [skripty rozhraní příkazového řádku pro webové aplikace](app-service-cli-samples.md).

