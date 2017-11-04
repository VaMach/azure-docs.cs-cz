---
title: "Průběžné nasazování registr, kontejner Docker s webovou aplikaci pro kontejnery - Azure | Microsoft Docs"
description: "Jak nastavit průběžné nasazování registr, kontejner Docker ve webové aplikaci pro kontejnery."
keywords: "služby Azure app service, linux, docker, acr, operačních systémů"
services: app-service
documentationcenter: 
author: ahmedelnably
manager: cfowler
editor: 
ms.assetid: a47fb43a-bbbd-4751-bdc1-cd382eae49f8
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2017
ms.author: aelnably;wesmc
ms.openlocfilehash: cccbd4952c66d3d8140e2a03e3b76afaa5ba3fbf
ms.sourcegitcommit: b979d446ccbe0224109f71b3948d6235eb04a967
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/25/2017
---
# <a name="continuous-deployment-with-web-app-for-containers"></a>Průběžné nasazování s webovou aplikaci pro kontejnery

V tomto kurzu nakonfigurujete průběžné nasazování pro bitovou kopii vlastní kontejner ze spravované [registru kontejner Azure](https://azure.microsoft.com/services/container-registry/) úložiště nebo [úložiště Docker Hub](https://hub.docker.com).

## <a name="sign-in-to-azure"></a>Přihlášení k Azure

Přihlaste se k [portálu Azure](https://portal.azure.com)

## <a name="enable-container-continuous-deployment-feature"></a>Povolit funkci průběžné nasazování kontejneru

Můžete povolit pomocí funkce průběžné nasazování [rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/install-azure-cli) a spuštěním následujícího příkazu

```azurecli-interactive
az webapp deployment container config -n sname -g rgname -e true
```

V  **[portál Azure](https://portal.azure.com/)**, klikněte **služby App Service** možnost na levé straně stránky.

Klikněte na název vaší aplikace, který chcete provést konfiguraci úložiště Docker Hub průběžné nasazování pro.

V **nastavení aplikace**, přidat aplikaci názvem `DOCKER_ENABLE_CI` s hodnotou `true`.

![Vložit obrázek nastavení aplikace](./media/app-service-webapp-service-linux-ci-cd/step2.png)

## <a name="prepare-webhook-url"></a>Příprava adresa URL Webhooku

Můžete získat pomocí adresy URL Webhooku [rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/install-azure-cli) a spuštěním následujícího příkazu

```azurecli-interactive
az webapp deployment container show-cd-url -n sname1 -g rgname
```

Pro adresu URL Webhooku, musíte mít následující koncový bod: `https://<publishingusername>:<publishingpwd>@<sitename>.scm.azurewebsites.net/docker/hook`.

Můžete získat vaše `publishingusername` a `publishingpwd` stažením webové aplikace profil publikování se pomocí portálu Azure.

![Vložit obrázek přidání webhooku 2](./media/app-service-webapp-service-linux-ci-cd/step3-3.png)

## <a name="add-a-web-hook"></a>Přidat web háku

### <a name="azure-container-registry"></a>Azure Container Registry

V okně portálu registru, klikněte na tlačítko **Webhooky**, kliknutím na vytvořit nový webhooku **přidat**. V **vytvoření webhooku** okno, zadejte název vaší webhooku. Pro identifikátor URI pro Webhook, budete muset zadat adresu URL získané z **krok 3**

Zajistěte, aby definujte rozsah jako úložiště obsahující bitové kopie kontejneru.

![Vložit obrázek webhooku](./media/app-service-webapp-service-linux-ci-cd/step3ACRWebhook-1.png)

Při aktualizaci získá bitovou kopii, získat webové aplikace automaticky aktualizován s novou bitovou kopii.

### <a name="docker-hub"></a>Úložiště docker Hub

Na stránce úložiště Docker Hub klikněte na tlačítko **Webhooky**, pak **vytvoření WEBHOOKU A**.

![Vložit obrázek přidání webhooku 1](./media/app-service-webapp-service-linux-ci-cd/step3-1.png)

Pro adresu URL Webhooku, budete muset zadat adresu URL získané z **krok 3**

![Vložit obrázek přidání webhooku 2](./media/app-service-webapp-service-linux-ci-cd/step3-2.png)

Při aktualizaci získá bitovou kopii, získat webové aplikace automaticky aktualizován s novou bitovou kopii.

## <a name="next-steps"></a>Další kroky

* [Co je Azure App Service v systému Linux?](./app-service-linux-intro.md)
* [Kontejner Azure registru](https://azure.microsoft.com/services/container-registry/)
* [Pomocí .NET Core v Azure App Service v systému Linux](quickstart-dotnetcore.md)
* [Pomocí Ruby v Azure App Service v systému Linux](quickstart-ruby.md)
* [Jak používat vlastní image Docker pro webovou aplikaci pro kontejnery](quickstart-custom-docker-image.md)
* [Webové aplikace Azure App Service pro kontejnery – nejčastější dotazy](./app-service-linux-faq.md)
* [Správa webové aplikace pro kontejnery pomocí Azure CLI 2.0](./app-service-linux-cli.md)