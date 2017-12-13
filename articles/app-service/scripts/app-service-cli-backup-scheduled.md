---
title: "Azure CLI skriptu ukázkové – vytvoření naplánovaného zálohování pro webovou aplikaci | Microsoft Docs"
description: "Azure CLI skriptu ukázkové – vytvoření naplánovaného zálohování pro webovou aplikaci"
services: app-service\web
documentationcenter: 
author: cephalin
manager: cfowler
editor: 
tags: azure-service-management
ms.service: app-service-web
ms.workload: web
ms.devlang: na
ms.topic: sample
ms.date: 12/11/2017
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: fe5d2ca6fe5327f40adf134e94cf9b00ad98d243
ms.sourcegitcommit: aaba209b9cea87cb983e6f498e7a820616a77471
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/12/2017
---
# <a name="create-a-scheduled-backup-for-a-web-app"></a>Vytvoření naplánovaného zálohování pro webovou aplikaci

Tento ukázkový skript vytvoří webovou aplikaci ve službě App Service se jeho souvisejících prostředků a potom vytvoří pro ni plánované zálohování. 

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Pokud si zvolíte instalaci a použití rozhraní příkazového řádku místně, je třeba Azure CLI verze 2.0 nebo novější. Verzi zjistíte spuštěním příkazu `az --version`. Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI 2.0]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Ukázkový skript

[!code-azurecli-interactive[main](../../../cli_scripts/app-service/backup-scheduled/backup-scheduled.sh?highlight=3-7 "Create a scheduled backup for a web app")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Vysvětlení skriptu

Tento skript používá následující příkazy. Každý příkaz v tabulce odkazy na dokumentaci konkrétní příkaz.

| Příkaz | Poznámky |
|---|---|
| [`az group create`](/cli/azure/group?view=azure-cli-latest#az_group_create) | Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky. |
| [`az storage account create`](/cli/azure/storage/account?view=azure-cli-latest#az_storage_account_create) | Vytvoří účet úložiště. |
| [`az storage container create`](/cli/azure/storage/container?view=azure-cli-latest#az_storage_container_create) | Vytvoří kontejner úložiště Azure. |
| [`az storage container generate-sas`](/cli/azure/storage/container?view=azure-cli-latest#az_storage_container_generate_sas) | Generuje token SAS pro kontejner úložiště Azure.  |
| [`az appservice plan create`](/cli/azure/appservice/plan?view=azure-cli-latest#az_appservice_plan_create) | Vytvoří plán služby App Service. |
| [`az webapp create`](/cli/azure/webapp?view=azure-cli-latest#az_webapp_create) | Vytvoří webové aplikace Azure. |
| [`az webapp config backup update`](/cli/azure/webapp/config/backup?view=azure-cli-latest#az_webapp_config_backup_update) | Konfiguruje nový plán zálohování pro webovou aplikaci. |
| [`az webapp config backup show`](/cli/azure/webapp/config/backup?view=azure-cli-latest#az_webapp_config_backup_show) | Zobrazuje plán zálohování pro webovou aplikaci. |
| [`az webapp config backup list`](/cli/azure/webapp/config/backup?view=azure-cli-latest#az_webapp_config_backup_list) | Získá seznam záloh pro webovou aplikaci. |

## <a name="next-steps"></a>Další kroky

Další informace o rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).

Další ukázky skript aplikace služby rozhraní příkazového řádku najdete v [dokumentaci služby Azure App Service](../app-service-cli-samples.md).
