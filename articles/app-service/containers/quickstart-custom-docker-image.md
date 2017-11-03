---
title: "Spusťte vlastní image úložiště Docker Hub ve webové aplikaci pro kontejnery | Microsoft Docs"
description: "Jak používat vlastní image Docker pro webovou aplikaci pro kontejnery."
keywords: "služby Azure app service, webové aplikace, linux, docker, kontejneru"
services: app-service
documentationcenter: 
author: naziml
manager: cfowler
editor: 
ms.assetid: b97bd4e6-dff0-4976-ac20-d5c109a559a8
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 09/05/2017
ms.author: wesmc
ms.custom: mvc
ms.openlocfilehash: c85f79cc14cdcecd2a05fc0ff91c4864b9fba277
ms.sourcegitcommit: 3e3a5e01a5629e017de2289a6abebbb798cec736
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/27/2017
---
# <a name="run-a-custom-docker-hub-image-in-web-app-for-containers"></a>Spusťte vlastní image úložiště Docker Hub ve webové aplikaci pro kontejnery

Služba App Service poskytuje zásobníky předem definované aplikací v systému Linux s podporou pro konkrétní verze, jako je například PHP 7.0 a Node.js 4.5. Vlastní image Docker můžete také použít k nasazení vaší webové aplikace do zásobník aplikací, které již nejsou definované v Azure. Tento rychlý start ukazuje, jak vytvořit webovou aplikaci a nasazení na základě Python Docker bitové kopie do ní. Vytvořit webovou aplikaci pomocí [rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli).

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

[!INCLUDE [Configure deployment user](../../../includes/configure-deployment-user.md)]

[!INCLUDE [Create resource group](../../../includes/app-service-web-create-resource-group.md)]

[!INCLUDE [Create app service plan](../../../includes/app-service-web-create-app-service-plan-linux.md)]

## <a name="create-a-web-app-for-container"></a>Vytvoření webové aplikace pro kontejner

Pomocí příkazu [az webapp create](/cli/azure/webapp#create) vytvořte [webovou aplikaci](../app-service-web-overview.md) v plánu služby App Service `myAppServicePlan`. Nezapomeňte nahradit `<app name>` s jedinečným názvem aplikace.

```azurecli-interactive
az webapp create --resource-group myResourceGroup --plan myAppServicePlan --name <app name> --deployment-container-image-name elnably/dockerimagetest
```

V předchozím příkazu `--deployment-container-image-name` odkazuje na bitovou kopii veřejného úložiště Docker Hub [https://hub.docker.com/r/elnably/dockerimagetest/](https://hub.docker.com/r/elnably/dockerimagetest/). Si můžete prohlédnout její obsah na [https://github.com/ahmedelnably/dockerimagetest](https://github.com/ahmedelnably/dockerimagetest).

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
> [Vytvoření webové aplikace Docker Python a PostgreSQL v Azure](tutorial-docker-python-postgresql-app.md)
