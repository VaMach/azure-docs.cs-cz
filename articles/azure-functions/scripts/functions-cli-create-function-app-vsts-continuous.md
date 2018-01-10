---
title: "Vytvoří funkci, v Azure, který je nasazen z Visual Studio Team Services | Microsoft Docs"
description: "Vytvoření funkce aplikace a nasazení funkce kód z Visual Studio Team Services"
services: functions
keywords: 
author: syntaxc4
ms.author: cfowler
ms.date: 01/09/2018
ms.topic: sample
ms.service: functions
ms.custom: mvc
ms.openlocfilehash: bf9428f23e851bae3485ec3d724dfb9ccd2af4c1
ms.sourcegitcommit: 6fb44d6fbce161b26328f863479ef09c5303090f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/10/2018
---
# <a name="create-a-function-in-azure-that-is-deployed-from-visual-studio-team-services"></a>Vytvoří funkci, v Azure, který je nasazen z Visual Studio Team Services

Toto téma ukazuje, jak používat Azure Functions k vytvoření [bez serveru](https://azure.microsoft.com/overview/serverless-computing/) funkce aplikace pomocí [plánu spotřeby](../functions-scale.md#consumption-plan). Funkce aplikace, která je kontejner pro funkce, nepřetržitě nasazení z Visual Studio Team Services (VSTS) úložiště. K dokončení tohoto tématu, musíte mít:

* Služby VSTS úložiště, který obsahuje projektu funkce aplikace a ke které máte oprávnění pro správu.
* A [osobní přístupový token (Jan)](https://docs.microsoft.com/vsts/accounts/use-personal-access-tokens-to-authenticate) pro přístup k vaší služby VSTS úložiště.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Pokud místo používáte Azure CLI místně, musíte nainstalovat a použít verze 2.0 nebo novější. Chcete-li určit verzi rozhraní příkazového řádku Azure, spusťte `az --version`. Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI 2.0]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Ukázkový skript

Tato ukázka funkce Azure aplikace vytvoří a nasadí funkce kód z Visual Studio Team Services.

[!code-azurecli-interactive[main](../../../cli_scripts/azure-functions/deploy-function-app-with-function-vsts/deploy-function-app-with-function-vsts.sh?highlight=3-4 "Azure Service")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Vysvětlení skriptu

Tento skript používá následující příkazy k vytvoření skupiny prostředků, účet úložiště, aplikaci funkce a všechny související prostředky. Každý příkaz v tabulce odkazy na dokumentaci konkrétní příkaz.

| Příkaz | Poznámky |
|---|---|
| [Vytvoření skupiny az](https://docs.microsoft.com/cli/azure/group#az_group_create) | Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky. |
| [Vytvořit účet úložiště az](https://docs.microsoft.com/cli/azure/appservice/plan#az_appservice_plan_create) | Vytvoří plán služby App Service. |
| [Vytvoření az functionapp](https://docs.microsoft.com/cli/azure/appservice/web#az_appservice_web_delete) |
| [Konfigurace správy zdrojového kódu webové služby App Service az](https://docs.microsoft.com/cli/azure/appservice/web/source-control#az_appservice_web_source_control_config) | Přidruží aplikaci funkce Git nebo Mercurial úložiště. |

## <a name="next-steps"></a>Další postup

Další informace o rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).

Další ukázky skriptu rozhraní příkazového řádku funkce Azure lze nalézt v [dokumentace Azure Functions](../functions-cli-samples.md).
