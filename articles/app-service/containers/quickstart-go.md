---
title: "Nasazení aplikace v jazyce Go ve službě Azure Web App for Containers | Microsoft Docs"
description: "Postup, jak nasadit image Dockeru s aplikací v jazyce Go pro službu Azure Web App for Containers."
keywords: azure app service, web app, go, docker, kontejner
services: app-service
author: sptramer
manager: cfowler
ms.assetid: b97bd4e6-dff0-4976-ac20-d5c109a559a8
ms.service: app-service
ms.devlang: go
ms.topic: quickstart
ms.date: 01/17/2018
ms.author: sttramer
ms.custom: mvc
ms.openlocfilehash: dbe4d7bc6f5f1d83a079993c9616206fd82a1b9d
ms.sourcegitcommit: 95500c068100d9c9415e8368bdffb1f1fd53714e
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/14/2018
---
# <a name="deploy-a-go-app-in-azure-web-app-for-containers"></a>Nasazení aplikace v jazyce Go ve službě Azure Web App for Containers

App Service poskytuje předdefinované zásobníky aplikací v Linuxu s podporou konkrétních verzí, jako například PHP 7.0 a Node.js 4.5. Můžete také použít vlastní image Dockeru a spouštět webovou aplikaci v zásobníku aplikací, který ještě není v Azure definovaný. V tomto návodu pro rychlý start se dozvíte, jak vybrat existující kontejner Docker s aplikací jazyka Go a spustit ho v Azure App Service. Webovou aplikaci vytvoříte pomocí [Azure CLI](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli).

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

[!INCLUDE [Configure deployment user](../../../includes/configure-deployment-user.md)]

[!INCLUDE [Create resource group](../../../includes/app-service-web-create-resource-group.md)]

[!INCLUDE [Create app service plan](../../../includes/app-service-web-create-app-service-plan-linux.md)]

## <a name="create-a-web-app-for-the-container"></a>Vytvoření webové aplikace pro kontejner

Pomocí příkazu [az webapp create](/cli/azure/webapp?view=azure-cli-latest#az_webapp_create) vytvořte [webovou aplikaci](../app-service-web-overview.md) v plánu služby App Service `myAppServicePlan`. Nezapomeňte nahradit `<app name>` globálně jedinečným názvem aplikace.

```azurecli-interactive
az webapp create --resource-group myResourceGroup --plan myAppServicePlan --name <app name> --deployment-container-image-name microsoft/appservice-go-quickstart
```

Parametr `--deployment-container-image-name` v předchozím příkazu odkazuje na veřejnou image z Docker Hubu [microsoft/appservice-go-quickstart](https://hub.docker.com/r/microsoft/appservice-go-quickstart).

Po vytvoření webové aplikace Azure CLI zobrazí výstup podobný následujícímu příkladu:

```json
{
  "availabilityState": "Normal",
  "clientAffinityEnabled": true,
  "clientCertEnabled": false,
  "cloningInfo": null,
  "containerSize": 0,
  "dailyMemoryTimeQuota": 0,
  "defaultHostName": "<app name>.azurewebsites.net",
  "deploymentLocalGitUrl": "https://<username>@<app name>.scm.azurewebsites.net/<app name>.git",
  "enabled": true,
  < JSON data removed for brevity. >
}
```

## <a name="get-data-from-the-apps-endpoint"></a>Načtení dat z koncového bodu aplikace

Pomocí příkazu `curl` kontaktujte koncový bod rozhraní REST API z aplikace kontejneru.

```bash
curl -X GET http://<app_name>.azurewebsites.net:8080/hello
```

```output
Hello world!
```

**Blahopřejeme!** Do služby Azure Web App for Containers jste nasadili vlastní image Dockeru s aplikací v jazyce Go.

## <a name="next-steps"></a>Další kroky

> [!div class="nextstepaction"]
> [Použití vlastní image Dockeru](tutorial-custom-docker-image.md)
