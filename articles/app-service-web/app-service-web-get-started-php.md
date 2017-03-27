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
ms.date: 03/17/2017
ms.author: cephalin
translationtype: Human Translation
ms.sourcegitcommit: 0d8472cb3b0d891d2b184621d62830d1ccd5e2e7
ms.openlocfilehash: afecc8997631bf507c797e56a9e3fc0d1df27614
ms.lasthandoff: 03/21/2017


---
# <a name="create-your-first-php-web-app-in-azure-in-five-minutes"></a>Vytvoření první webové aplikace v PHP v Azure během pěti minut
[!INCLUDE [app-service-web-selector-get-started](../../includes/app-service-web-selector-get-started.md)]

Tento Rychlý start vám pomůže nasadit první webovou aplikaci v PHP do služby [Azure App Service](../app-service/app-service-value-prop-what-is.md) během pár minut.

Než začnete, ujistěte se, že je rozhraní Azure CLI nainstalované. Další informace najdete v tématu [Průvodce instalací Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli).

## <a name="log-in-to-azure"></a>Přihlaste se k Azure.
Přihlaste se k Azure spuštěním příkazu `az login` a postupem podle pokynů na obrazovce.
   
```azurecli
az login
```
   
## <a name="create-a-resource-group"></a>Vytvoření skupiny prostředků   
Vytvořte [skupinu prostředků](../azure-resource-manager/resource-group-overview.md). To je místo, kam dáte všechny prostředky Azure, které chcete spravovat společně, jako je webová aplikace a její back-end SQL Database.

```azurecli
az group create --location "West Europe" --name myResourceGroup
```

K zobrazení možných hodnot, které se dají použít pro `---location`, použijte příkaz `az appservice list-locations` rozhraní příkazového řádku Azure CLI.

## <a name="create-an-app-service-plan"></a>Vytvoření plánu služby App Service
Vytvořte [plán služby App Service](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) úrovně Standard, ve kterém se bude spouštět kontejner Linuxu. 

```azurecli
az appservice plan create --name my-free-appservice-plan --resource-group myResourceGroup --is-linux --sku S1
```

## <a name="create-a-web-app"></a>Vytvoření webové aplikace
Vytvořte webovou aplikaci s jedinečným názvem ve značce `<app_name>`.

```azurecli
az appservice web create --name <app_name> --resource-group myResourceGroup --plan my-free-appservice-plan
```

## <a name="configure-the-linux-container"></a>Konfigurace kontejneru Linuxu
Nakonfigurujte kontejner Linuxu, aby používal výchozí image PHP 7.0.6.

```azurecli
az appservice web config update --php-version 7.0.6 --name <app_name> --resource-group myResourceGroup
```
## <a name="deploy-sample-application"></a>Nasazení ukázkové aplikace
Nasaďte ukázkovou aplikaci v PHP z GitHubu.

```azurecli
az appservice web source-control config --name <app_name> --resource-group myResourceGroup \
--repo-url "https://github.com/Azure-Samples/app-service-web-php-get-started.git" --branch master --manual-integration 
```

## <a name="browse-to-web-app"></a>Přechod do webové aplikace
Pokud chcete vidět svou aplikaci živě běžet v Azure, spusťte tento příkaz.

```azurecli
az appservice web browse --name <app_name> --resource-group myResourceGroup
```

Blahopřejeme, vaše první webová aplikace v PHP běží živě v Azure App Service.

[!INCLUDE [cli-samples-clean-up](../../includes/cli-samples-clean-up.md)]

## <a name="next-steps"></a>Další kroky

Prozkoumejte předem vytvořené [skripty rozhraní příkazového řádku pro webové aplikace](app-service-cli-samples.md).

