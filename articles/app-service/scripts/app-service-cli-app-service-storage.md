---
title: "Azure CLI skriptu ukázkové – webovou aplikaci připojit k účtu úložiště | Microsoft Docs"
description: "Azure CLI skriptu ukázkové – webovou aplikaci připojit k účtu úložiště"
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
ms.date: 06/19/2017
ms.author: cfowler
ms.custom: mvc
ms.openlocfilehash: a07c2ae64be2081a2dba172ada810b76bfd7dfd7
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="connect-a-web-app-to-a-storage-account"></a>Webovou aplikaci připojit k účtu úložiště

V tomto scénáři se dozvíte, jak vytvořit účet úložiště Azure a webové aplikace Azure. Potom propojíte účet úložiště pro webovou aplikaci pomocí nastavení aplikace.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Pokud se rozhodnete nainstalovat a používat rozhraní příkazového řádku (CLI) místně, musíte mít spuštěnou verzi Azure CLI 2.0 nebo novější. Verzi zjistíte spuštěním příkazu `az --version`. Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI 2.0]( /cli/azure/install-azure-cli). 


## <a name="sample-script"></a>Ukázkový skript

[!code-azurecli-interactive[main](../../../cli_scripts/app-service/connect-to-storage/connect-to-storage.sh "Azure Storage")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Vysvětlení skriptu

Tento skript používá následující příkazy k vytvoření skupiny prostředků, webové aplikace, účet úložiště a všechny související prostředky. Každý příkaz v tabulce odkazy na dokumentaci konkrétní příkaz.

| Příkaz | Poznámky |
|---|---|
| [Vytvoření skupiny az](https://docs.microsoft.com/cli/azure/group#az_group_create) | Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky. |
| [Vytvořit plán aplikační služby az](https://docs.microsoft.com/cli/azure/appservice/plan#az_appservice_plan_create) | Vytvoří plán služby App Service. Toto je jako serverové farmy pro Azure webové aplikace. |
| [Vytvoření az webapp](https://docs.microsoft.com/cli/azure/webapp#az_webapp_create) | Vytvoří webové aplikace Azure. |
| [Vytvořit účet úložiště az](https://docs.microsoft.com/cli/azure/storage/account#az_storage_account_create) | Vytvoří účet úložiště. Toto je, kde bude uložena statické prostředky. |
| [AZ úložiště účet zobrazit připojovacího řetězce](https://docs.microsoft.com/cli/azure/storage/account#az_storage_account_show_connection_string) | |
| [AZ webapp konfigurace appsettings sady](https://docs.microsoft.com/cli/azure/webapp/config/appsettings#az_webapp_config_appsettings_set) | Vytvoří nebo aktualizuje nastavení aplikace pro webové aplikace Azure. Nastavení aplikace jsou zveřejněné jako proměnné prostředí pro vaši aplikaci. |

## <a name="next-steps"></a>Další kroky

Další informace o rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).

Další ukázky skript aplikace služby rozhraní příkazového řádku najdete v [dokumentaci služby Azure App Service](../app-service-cli-samples.md).
