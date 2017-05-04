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
ms.date: 03/17/2017
ms.author: cephalin
translationtype: Human Translation
ms.sourcegitcommit: be3ac7755934bca00190db6e21b6527c91a77ec2
ms.openlocfilehash: ba9b9b780da74c44f6314fa289f1d6b8c231dd30
ms.lasthandoff: 05/03/2017


---
# <a name="create-a-static-html-web-app-in-azure-in-five-minutes"></a>Vytvoření webové aplikace ve statickém HTML v Azure během pěti minut
[!INCLUDE [app-service-web-selector-get-started](../../includes/app-service-web-selector-get-started.md)] 

Tento Rychlý start vám pomůže nasadit jednoduchý web HTML+CSS do služby [Azure App Service](../app-service/app-service-value-prop-what-is.md) během pár minut.

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

K zobrazení možných hodnot, které se dají použít pro `--location`, použijte příkaz `az appservice list-locations` rozhraní příkazového řádku Azure CLI.


## <a name="create-an-app-service-plan"></a>Vytvoření plánu služby App Service
Vytvořte [plán služby App Service](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) Free. 

```azurecli
az appservice plan create --name my-free-appservice-plan --resource-group myResourceGroup --sku FREE
```

## <a name="create-a-web-app"></a>Vytvoření webové aplikace
Vytvořte webovou aplikaci s jedinečným názvem ve značce `<app_name>`.

```azurecli
az appservice web create --name <app_name> --resource-group myResourceGroup --plan my-free-appservice-plan
```

## <a name="deploy-sample-application"></a>Nasazení ukázkové aplikace
Nasaďte ukázkový web HTML z Githubu.

```azurecli
az appservice web source-control config --name <app_name> --resource-group myResourceGroup \
--repo-url "https://github.com/Azure-Samples/app-service-web-html-get-started.git" --branch master --manual-integration 
```

## <a name="browse-to-web-app"></a>Přechod do webové aplikace
Pokud chcete vidět svou aplikaci živě běžet v Azure, spusťte tento příkaz.

```azurecli
az appservice web browse --name <app_name> --resource-group myResourceGroup
```

Blahopřejeme, váš první web ve statickém HTML běží živě v Azure App Service.

[!INCLUDE [cli-samples-clean-up](../../includes/cli-samples-clean-up.md)]

## <a name="next-steps"></a>Další kroky

Prozkoumejte předem vytvořené [skripty rozhraní příkazového řádku pro webové aplikace](app-service-cli-samples.md).

