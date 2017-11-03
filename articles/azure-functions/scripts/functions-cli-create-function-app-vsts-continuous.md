---
title: "Vytvoření funkce aplikace a nasazení funkce kód z Visual Studio Team Services | Microsoft Docs"
description: "Vytvoření funkce aplikace a nasazení funkce kód z Visual Studio Team Services"
services: functions
keywords: 
author: syntaxc4
ms.author: cfowler
ms.date: 04/28/2017
ms.topic: sample
ms.service: functions
ms.custom: mvc
ms.openlocfilehash: 5851b5219b6e25a5a2b005fc3d3c3b44d98ed746
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="create-an-app-service"></a>Vytvoření služby aplikace

V tomto scénáři se dozvíte, jak vytvořit funkce aplikace pomocí [plánu spotřeby](../functions-scale.md#consumption-plan) s jejími související prostředky a průběžně nasadí funkce kódu z úložiště Visual Studio Team Services (VSTS). V této ukázce budete potřebovat:

* Služby VSTS úložiště s kódem funkce, které mají oprávnění pro správu.
* A [Personal Access Token (Jan)](https://help.github.com/articles/creating-an-access-token-for-command-line-use) pro váš účet GitHub.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Pokud se rozhodnete nainstalovat a používat rozhraní příkazového řádku (CLI) místně, musíte mít spuštěnou verzi Azure CLI 2.0 nebo novější. Verzi zjistíte spuštěním příkazu `az --version`. Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI 2.0]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Ukázkový skript

Tato ukázka funkce Azure aplikace vytvoří a nasadí funkce kód z Visual Studio Team Services.

[!code-azurecli-interactive[main](../../../cli_scripts/azure-functions/deploy-function-app-with-function-vsts/deploy-function-app-with-function-vsts.sh?highlight=3-4 "Azure Service")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Vysvětlení skriptu

Tento skript používá následující příkazy k vytvoření skupiny prostředků, webové aplikace, documentdb a všechny související prostředky. Každý příkaz v tabulce odkazy na dokumentaci konkrétní příkaz.

| Příkaz | Poznámky |
|---|---|
| [Vytvoření skupiny az](https://docs.microsoft.com/cli/azure/group#az_group_create) | Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky. |
| [Vytvořit účet úložiště az](https://docs.microsoft.com/cli/azure/appservice/plan#az_appservice_plan_create) | Vytvoří plán služby App Service. |
| [Vytvoření az functionapp](https://docs.microsoft.com/cli/azure/appservice/web#az_appservice_web_delete) |
| [Konfigurace správy zdrojového kódu webové služby App Service az](https://docs.microsoft.com/cli/azure/appservice/web/source-control#az_appservice_web_source_control_config) | Přidruží aplikaci funkce Git nebo Mercurial úložiště. |

## <a name="next-steps"></a>Další kroky

Další informace o rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).

Další ukázky skriptu rozhraní příkazového řádku funkce Azure lze nalézt v [dokumentace Azure Functions](../functions-cli-samples.md).
