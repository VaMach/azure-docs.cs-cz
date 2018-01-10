---
title: "Vytvoří funkci, v Azure, který je nasazen z Githubu | Microsoft Docs"
description: "Vytvoření funkce aplikace a nasazení kódu funkce z úložiště Githubu pomocí Azure Functions."
services: functions
ms.service: functions
keywords: 
ms.devlang: azurecli
author: syntaxc4
ms.author: cfowler
ms.date: 01/09/2018
ms.topic: sample
ms.custom: mvc
ms.openlocfilehash: c4224bc7973cd1e3ca36799db9f23a124fcba807
ms.sourcegitcommit: 6fb44d6fbce161b26328f863479ef09c5303090f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/10/2018
---
# <a name="create-a-function-in-azure-that-is-deployed-from-github"></a>Vytvoří funkci, v Azure, který je nasazen z Githubu

Tento ukázkový skript vytvoří funkce aplikace pomocí [plánu spotřeby](../functions-scale.md#consumption-plan) s jejími související prostředky a průběžně nasadí funkce kódu z úložiště Githubu. V této ukázce potřebujete:

* Úložiště GitHub funkce kód, který máte oprávnění pro správu.
* A [Personal Access Token (Jan)](https://help.github.com/articles/creating-an-access-token-for-command-line-use) pro váš účet GitHub.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Pokud místo používáte Azure CLI místně, musíte nainstalovat a použít verze 2.0 nebo novější. Chcete-li určit verzi rozhraní příkazového řádku Azure, spusťte `az --version`. Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI 2.0]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Ukázkový skript

Tato ukázka funkce Azure aplikace vytvoří a nasadí kód funkce z Githubu.

[!code-azurecli-interactive[main](../../../cli_scripts/azure-functions/deploy-function-app-with-function-github-continuous/deploy-function-app-with-function-github-continuous.sh?highlight=3-4 "Azure Service")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Vysvětlení skriptu

Každý příkaz v tabulce odkazy na dokumentaci konkrétní příkaz. Tento skript používá následující příkazy:

| Příkaz | Poznámky |
|---|---|
| [Vytvoření skupiny az](https://docs.microsoft.com/cli/azure/group#az_group_create) | Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky. |
| [Vytvořit účet úložiště az](https://docs.microsoft.com/cli/azure/appservice/plan#az_appservice_plan_create) | Vytvoří plán služby App Service. |
| [Vytvoření az functionapp](https://docs.microsoft.com/cli/azure/appservice/web#az_appservice_web_delete) |
| [Konfigurace správy zdrojového kódu webové služby App Service az](https://docs.microsoft.com/cli/azure/appservice/web/source-control#az_appservice_web_source_control_config) | Přidruží aplikaci funkce Git nebo Mercurial úložiště. |

## <a name="next-steps"></a>Další postup

Další informace o rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).

Další ukázky skriptu rozhraní příkazového řádku funkce Azure lze nalézt v [dokumentace Azure Functions](../functions-cli-samples.md).
