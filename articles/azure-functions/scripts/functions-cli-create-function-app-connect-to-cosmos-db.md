---
title: "Vytvoření funkce Azure, která se připojuje k databázi Azure Cosmos | Microsoft Docs"
description: "Azure CLI skriptu ukázkové – vytvoření funkce Azure, která se připojuje k databázi Cosmos Azure"
services: functions
documentationcenter: functions
author: rachelappel
manager: cfowler
editor: 
tags: functions
ms.assetid: 
ms.service: functions
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: 
ms.date: 04/20/2017
ms.author: rachelap
ms.custom: mvc
ms.openlocfilehash: ce48f8dd71d5b9e48dcd506df976e07d385f34c5
ms.sourcegitcommit: cf42a5fc01e19c46d24b3206c09ba3b01348966f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/29/2017
---
# <a name="create-an-azure-function-that-connects-to-an-azure-cosmos-db"></a>Vytvoření funkce Azure, která se připojuje k databázi Cosmos Azure

Tento ukázkový skript vytvoří aplikace funkce Azure, připojí se k databázi Azure Cosmos DB.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Pokud se rozhodnete nainstalovat a používat rozhraní příkazového řádku (CLI) místně, musíte mít spuštěnou verzi Azure CLI 2.0 nebo novější. Verzi zjistíte spuštěním příkazu `az --version`. Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI 2.0]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Ukázkový skript

Tato ukázka vytvoří aplikaci funkce Azure a přidá Cosmos DB koncový bod a přístupový klíč nastavení aplikace.

[!code-azurecli-interactive[main](../../../cli_scripts/azure-functions/create-function-app-connect-to-cosmos-db/create-function-app-connect-to-cosmos-db.sh "Create an Azure Function that connects to an Azure Cosmos DB")]

## <a name="clean-up-deployment"></a>Vyčištění nasazení

Po spuštění ukázka skriptu, použijte příkaz lze použít k odebrání skupiny prostředků a všechny související prostředky.

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Vysvětlení skriptu

Tento skript používá následující příkazy. Každý příkaz v tabulce odkazy na dokumentaci konkrétní příkaz.

| Příkaz | Poznámky |
|---|---|
| [AZ přihlášení](https://docs.microsoft.com/cli/azure/#login) | Přihlášení k Azure. |
| [Vytvoření skupiny az](https://docs.microsoft.com/cli/azure/group#az_group_create) | Vytvořte skupinu prostředků s umístěním |
| [Vytvořit účet úložiště az](https://docs.microsoft.com/cli/azure/storage/account) | vytvořit účet úložiště |
| [Vytvoření az functionapp](https://docs.microsoft.com/cli/azure/functionapp#az_functionapp_create) | Vytvořit novou aplikaci funkce |
| [Vytvoření az cosmosdb](https://docs.microsoft.com/cli/azure/cosmosdb#az_cosmosdb_create) | Vytvoření databáze cosmosdb |
| [Odstranění skupiny az](https://docs.microsoft.com/cli/azure/group#az_group_delete) | Vyčištění |

## <a name="next-steps"></a>Další kroky

Další informace o rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).

Další ukázky skriptu rozhraní příkazového řádku funkce Azure lze nalézt v [dokumentace Azure Functions](../functions-cli-samples.md).




