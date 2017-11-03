---
title: "Ukázka skriptu Azure CLI – vytvoření webové aplikace a nasazení kódu do pracovního prostředí | Microsoft Docs"
description: "Ukázka skriptu Azure CLI – vytvoření webové aplikace a nasazení kódu do pracovního prostředí"
services: app-service\web
documentationcenter: 
author: cephalin
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: 2b995dcd-e471-4355-9fda-00babcdb156e
ms.service: app-service-web
ms.workload: web
ms.devlang: azurecli
ms.tgt_pltfrm: na
ms.topic: sample
ms.date: 06/19/2017
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: 449e5729a15e619c43e5f4a0643915c2d3114d17
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-web-app-and-deploy-code-to-a-staging-environment"></a>Vytvoření webové aplikace a nasazení kódu do pracovního prostředí

Tento ukázkový skript vytvoří webovou aplikaci ve službě App Service se další nasazovací slot názvem "přípravy" a pak nasadí ukázkovou aplikaci na "pracovní" slot.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Pokud se rozhodnete nainstalovat a používat rozhraní příkazového řádku (CLI) místně, musíte mít spuštěnou verzi Azure CLI 2.0 nebo novější. Verzi zjistíte spuštěním příkazu `az --version`. Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI 2.0]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Ukázkový skript

[!code-azurecli-interactive[main](../../../cli_scripts/app-service/deploy-deployment-slot/deploy-deployment-slot.sh "Create a web app and deploy code to a staging environment")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Vysvětlení skriptu

Tento skript používá následující příkazy. Každý příkaz v tabulce odkazy na dokumentaci konkrétní příkaz.

| Příkaz | Poznámky |
|---|---|
| [Vytvoření skupiny az](https://docs.microsoft.com/cli/azure/group#az_group_create) | Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky. |
| [Vytvořit plán aplikační služby az](https://docs.microsoft.com/cli/azure/appservice/plan#az_appservice_plan_create) | Vytvoří plán služby App Service. |
| [Vytvoření az webapp](https://docs.microsoft.com/cli/azure/webapp#az_webapp_create) | Vytvoří webové aplikace Azure. |
| [Vytvoření az webapp nasazovací slot.](https://docs.microsoft.com/cli/azure/webapp/deployment/slot#az_webapp_deployment_slot_create) | Vytvořte nasazovací slot. |
| [AZ webapp nasazení zdroj konfigurace](https://docs.microsoft.com/cli/azure/webapp/deployment/source#az_webapp_deployment_source_config) | Přidruží úložiště Git nebo Mercurial webové aplikace Azure. |
| [Procházet az webapp](https://docs.microsoft.com/cli/azure/webapp#az_webapp_browse) | Webové aplikace Azure, otevřete v prohlížeči. |
| [Prohození slotů nasazení az webapp](https://docs.microsoft.com/cli/azure/webapp/deployment/slot#az_webapp_deployment_slot_swap) | Prohodit slot zadané nasazení do produkčního prostředí. |

## <a name="next-steps"></a>Další kroky

Další informace o rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).

Další ukázky skript aplikace služby rozhraní příkazového řádku najdete v [dokumentaci služby Azure App Service](../app-service-cli-samples.md).
