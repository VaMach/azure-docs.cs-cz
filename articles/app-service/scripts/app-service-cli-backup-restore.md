---
title: "Ukázka skriptu Azure CLI – obnovení webové aplikace ze zálohy | Microsoft Docs"
description: "Ukázka skriptu Azure CLI – obnovení ze zálohy, webové aplikace"
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
ms.date: 12/07/2017
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: bec915f3e321cf2422f7cc2cad3ce1dfb6b3317a
ms.sourcegitcommit: 094061b19b0a707eace42ae47f39d7a666364d58
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/08/2017
---
# <a name="restore-a-web-app-from-a-backup"></a>Obnovení ze zálohy, webové aplikace

Tento ukázkový skript vytvoří webovou aplikaci ve službě App Service se jeho souvisejících prostředků a potom vytvoří jednorázové zálohy pro ni. 

Pokud chcete spustit tento skript, musíte existující zálohy pro webovou aplikaci. Chcete-li vytvořit, přečtěte si téma [zálohování webovou aplikaci](app-service-cli-backup-onetime.md) nebo [vytvoření naplánovaného zálohování pro webovou aplikaci](app-service-cli-backup-scheduled.md).

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Pokud si zvolíte instalaci a použití rozhraní příkazového řádku místně, je třeba Azure CLI verze 2.0 nebo novější. Verzi zjistíte spuštěním příkazu `az --version`. Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI 2.0]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Ukázkový skript

[!code-azurecli-interactive[main](../../../cli_scripts/app-service/backup-restore/backup-restore.sh?highlight=3-4,9 "Restore a web app from a backup")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Vysvětlení skriptu

Tento skript používá následující příkazy. Každý příkaz v tabulce odkazy na dokumentaci konkrétní příkaz.

| Příkaz | Poznámky |
|---|---|
| [`az webapp config backup list`](/cli/azure/webapp/config/backup?view=azure-cli-latest#az_webapp_config_backup_list) | Získá seznam záloh pro webovou aplikaci. |
| [`az webapp config backup restore`](/cli/azure/webapp/config/backup?view=azure-cli-latest#az_webapp_config_backup_restore) | Webové aplikace se obnoví ze zálohy. |

## <a name="next-steps"></a>Další kroky

Další informace o rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).

Další ukázky skript aplikace služby rozhraní příkazového řádku najdete v [dokumentaci služby Azure App Service](../app-service-cli-samples.md).
