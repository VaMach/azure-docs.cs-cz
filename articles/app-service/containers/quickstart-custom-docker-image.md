---
title: "Spuštění vlastní image z Docker Hubu v Azure Web App for Containers | Dokumentace Microsoftu"
description: "Jak používat vlastní image Dockeru pro službu Azure Web App for Containers."
keywords: azure app service, web app, linux, docker, container
services: app-service
documentationcenter: 
author: cephalin
manager: cfowler
editor: 
ms.assetid: b97bd4e6-dff0-4976-ac20-d5c109a559a8
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 11/02/2017
ms.author: cephalin;wesmc
ms.custom: mvc
ms.openlocfilehash: a95a8435e4ecef201ad0f6d9ecda68e94f06ea80
ms.sourcegitcommit: 0e4491b7fdd9ca4408d5f2d41be42a09164db775
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/14/2017
---
# <a name="run-a-custom-docker-hub-image-in-azure-web-app-for-containers"></a>Spuštění vlastní image z Docker Hubu v Azure Web App for Containers

App Service poskytuje předdefinované zásobníky aplikací v Linuxu s podporou konkrétních verzí, jako například PHP 7.0 a Node.js 4.5. Můžete také použít vlastní image Dockeru a spouštět webovou aplikaci v zásobníku aplikací, který ještě není v Azure definovaný. Tento rychlý start ukazuje, jak vytvořit webovou aplikaci a nasadit do ní [oficiální image Dockeru Nginx](https://hub.docker.com/r/_/nginx/). Webovou aplikaci vytvoříte pomocí [Azure CLI](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli).

![Ukázková aplikace spuštěná ve službě Azure](media/quickstart-custom-docker-image/hello-world-in-browser.png)

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

[!INCLUDE [Configure deployment user](../../../includes/configure-deployment-user.md)]

[!INCLUDE [Create resource group](../../../includes/app-service-web-create-resource-group.md)]

[!INCLUDE [Create app service plan](../../../includes/app-service-web-create-app-service-plan-linux.md)]

## <a name="create-a-web-app-for-container"></a>Vytvoření webové aplikace pro kontejner

Pomocí příkazu [az webapp create](/cli/azure/webapp?view=azure-cli-latest#az_webapp_create) vytvořte [webovou aplikaci](../app-service-web-overview.md) v plánu služby App Service `myAppServicePlan`. Nezapomeňte nahradit `<app name>` jedinečným názvem aplikace.

```azurecli-interactive
az webapp create --resource-group myResourceGroup --plan myAppServicePlan --name <app name> --deployment-container-image-name nginx
```

Parametr `--deployment-container-image-name` v předchozím příkazu odkazuje na veřejnou image z Docker Hubu [https://hub.docker.com/r/_/nginx/](https://hub.docker.com/r/_/nginx/).

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

## <a name="browse-to-the-app"></a>Přechod do aplikace

Ve webovém prohlížeči přejděte na následující adresu URL.

```bash
http://<app_name>.azurewebsites.net
```

![Ukázková aplikace spuštěná ve službě Azure](media/quickstart-custom-docker-image/hello-world-in-browser.png)

**Blahopřejeme!** Nasadili jste vlastní image Dockeru do služby Web App for Containers.

## <a name="next-steps"></a>Další kroky

> [!div class="nextstepaction"]
> [Použití vlastní image Dockeru](tutorial-custom-docker-image.md)
