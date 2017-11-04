---
title: "Spusťte vlastní image úložiště Docker Hub v Azure Web App pro kontejnery | Microsoft Docs"
description: "Jak používat vlastní image Docker pro webové aplikace Azure pro kontejnery."
keywords: "služby Azure app service, webové aplikace, linux, docker, kontejneru"
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
ms.openlocfilehash: 8e7afd89def170ce756aae9e76daf91d78cc20e0
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/03/2017
---
# <a name="run-a-custom-docker-hub-image-in-azure-web-app-for-containers"></a>Spusťte vlastní image úložiště Docker Hub v Azure Web App pro kontejnery

Služba App Service poskytuje zásobníky předem definované aplikací v systému Linux s podporou pro konkrétní verze, jako je například PHP 7.0 a Node.js 4.5. Můžete také vlastní image Docker ke spouštění vaší webové aplikace v zásobníku aplikace, které již nejsou definované v Azure. Tento rychlý start ukazuje, jak vytvořit webovou aplikaci a nasadit [oficiální image Nginx Docker](https://hub.docker.com/r/_/nginx/) k němu. Vytvořit webovou aplikaci pomocí [rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli).

![Ukázková aplikace spuštěná ve službě Azure](media/quickstart-custom-docker-image/hello-world-in-browser.png)

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

[!INCLUDE [Configure deployment user](../../../includes/configure-deployment-user.md)]

[!INCLUDE [Create resource group](../../../includes/app-service-web-create-resource-group.md)]

[!INCLUDE [Create app service plan](../../../includes/app-service-web-create-app-service-plan-linux.md)]

## <a name="create-a-web-app-for-container"></a>Vytvoření webové aplikace pro kontejner

Pomocí příkazu [az webapp create](/cli/azure/webapp#create) vytvořte [webovou aplikaci](../app-service-web-overview.md) v plánu služby App Service `myAppServicePlan`. Nezapomeňte nahradit `<app name>` s jedinečným názvem aplikace.

```azurecli-interactive
az webapp create --resource-group myResourceGroup --plan myAppServicePlan --name <app name> --deployment-container-image-name nginx
```

V předchozím příkazu `--deployment-container-image-name` odkazuje na bitovou kopii veřejného úložiště Docker Hub [https://hub.docker.com/r/_/nginx/](https://hub.docker.com/r/_/nginx/).

Po vytvoření webové aplikace Azure CLI ukazuje výstup podobně jako v následujícím příkladu:

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

Přejděte na následující adresu URL pomocí webového prohlížeče.

```bash
http://<app_name>.azurewebsites.net
```

![Ukázková aplikace spuštěná ve službě Azure](media/quickstart-custom-docker-image/hello-world-in-browser.png)

**Blahopřejeme!** Vlastní image Docker jste nasadili do webové aplikace pro kontejnery.

## <a name="next-steps"></a>Další kroky

> [!div class="nextstepaction"]
> [Použít vlastní image Docker](tutorial-custom-docker-image.md)
