---
title: "Azure CLI skriptu ukázkové – webovou aplikaci připojit k mezipaměti redis | Microsoft Docs"
description: "Azure CLI skriptu ukázkové – webovou aplikaci připojit k mezipaměti redis"
services: appservice
documentationcenter: appservice
author: syntaxc4
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: bc8345b2-8487-40c6-a91f-77414e8688e6
ms.service: app-service
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: web
ms.date: 08/30/2017
ms.author: cfowler
ms.custom: mvc
ms.openlocfilehash: b8ae453ca73ae69c34ff785dc619433035257a6a
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/08/2017
---
# <a name="connect-a-web-app-to-a-redis-cache"></a>Připojení webové aplikace do mezipaměti redis

V tomto scénáři se dozvíte, jak vytvořit webové aplikace Azure a Azure redis cache. Potom propojíte mezipaměť redis pro webovou aplikaci pomocí nastavení aplikace.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Ukázkový skript

[!code-azurecli-interactive[main](../../../cli_scripts/app-service/connect-to-redis/connect-to-redis.sh "Azure Redis Cache")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Vysvětlení skriptu

Tento skript používá následující příkazy k vytvoření skupiny prostředků, webové aplikace, redis cache a všechny související prostředky. Každý příkaz v tabulce odkazy na dokumentaci konkrétní příkaz.

| Příkaz | Poznámky |
|---|---|
| [Vytvoření skupiny az](https://docs.microsoft.com/cli/azure/group#az_group_create) | Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky. |
| [Vytvořit plán aplikační služby az](https://docs.microsoft.com/cli/azure/appservice/plan#az_appservice_plan_create) | Vytvoří plán služby App Service. Toto je jako serverové farmy pro Azure webové aplikace. |
| [Vytvoření az webapp](https://docs.microsoft.com/cli/azure/webapp#az_webapp_create) | Vytvoří webové aplikace Azure. |
| [Vytvoření az redis](https://docs.microsoft.com/cli/azure/redis#az_redis_create) | Vytvořte novou instanci služby Redis Cache. Toto je, kde bude uložena data. |
| [AZ redis seznamu klíčů](https://docs.microsoft.com/cli/azure/redis#az_redis_list_keys) | Uvádí přístupové klíče pro instanci služby redis cache. |
| [AZ webapp konfigurace appsettings sady](https://docs.microsoft.com/cli/azure/webapp/config/appsettings#az_webapp_config_appsettings_set) | Vytvoří nebo aktualizuje nastavení aplikace pro webové aplikace Azure. Nastavení aplikace jsou zveřejněné jako proměnné prostředí pro vaši aplikaci. |

## <a name="next-steps"></a>Další kroky

Další informace o rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).

Další ukázky skript aplikace služby rozhraní příkazového řádku najdete v [dokumentaci služby Azure App Service](../app-service-cli-samples.md).
