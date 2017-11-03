---
title: "Ukázka skriptu Azure CLI - vazby SSL vlastní certifikát do webové aplikace | Microsoft Docs"
description: "Ukázka skriptu Azure CLI - vazby vlastní certifikát SSL pro webovou aplikaci"
services: app-service\web
documentationcenter: 
author: cephalin
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: eb95d350-81ea-4145-a1e2-6eea3b7469b2
ms.service: app-service-web
ms.workload: web
ms.devlang: azurecli
ms.tgt_pltfrm: na
ms.topic: sample
ms.date: 06/19/2017
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: ab4e211bc2c3f7e99ab0246e7abba57795318245
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="bind-a-custom-ssl-certificate-to-a-web-app"></a>Vlastní certifikát SSL vazbu do webové aplikace

Tento ukázkový skript vytvoří webovou aplikaci ve službě App Service se jeho souvisejících prostředků a potom vytvoří vazbu certifikátu SSL vlastního názvu domény. Pro tuto ukázku budete potřebovat:

* Přístup na stránku konfigurace DNS doménového registrátora.
* Platné. Soubor PFX a heslo pro certifikát SSL chcete nahrát a jejich vazby.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Pokud se rozhodnete nainstalovat a používat rozhraní příkazového řádku (CLI) místně, musíte mít spuštěnou verzi Azure CLI 2.0 nebo novější. Verzi zjistíte spuštěním příkazu `az --version`. Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI 2.0]( /cli/azure/install-azure-cli). 


## <a name="sample-script"></a>Ukázkový skript

[!code-azurecli-interactive[main](../../../cli_scripts/app-service/configure-ssl-certificate/configure-ssl-certificate.sh?highlight=3-5 "Bind a custom SSL certificate to a web app")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Vysvětlení skriptu

Tento skript používá následující příkazy. Každý příkaz v tabulce odkazy na dokumentaci konkrétní příkaz.

| Příkaz | Poznámky |
|---|---|
| [Vytvoření skupiny az](https://docs.microsoft.com/cli/azure/group#az_group_create) | Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky. |
| [Vytvořit plán aplikační služby az](https://docs.microsoft.com/cli/azure/appservice/plan#az_appservice_plan_create) | Vytvoří plán služby App Service. |
| [Vytvoření az webapp](https://docs.microsoft.com/cli/azure/webapp#az_webapp_create) | Vytvoří webové aplikace Azure. |
| [Přidejte název az webapp konfigurace hostitele](https://docs.microsoft.com/cli/azure/webapp/config/hostname#az_webapp_config_hostname_add) | Mapuje vlastní doménu do webové aplikace. |
| [AZ webapp konfigurace ssl nahrávání](https://docs.microsoft.com/cli/azure/webapp/config/ssl#az_webapp_config_ssl_upload) | Odešle certifikát SSL pro webovou aplikaci. |
| [AZ webapp konfigurace protokolu ssl vazby](https://docs.microsoft.com/cli/azure/webapp/config/ssl#az_webapp_config_ssl_bind) | Váže nahraný certifikát SSL pro webovou aplikaci. |

## <a name="next-steps"></a>Další kroky

Další informace o rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).

Další ukázky skript aplikace služby rozhraní příkazového řádku najdete v [dokumentaci služby Azure App Service](../app-service-cli-samples.md).
