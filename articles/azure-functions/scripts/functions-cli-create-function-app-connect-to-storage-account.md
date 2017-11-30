---
title: "Vytvoření funkce Azure, která se připojuje k Azure Storage | Microsoft Docs"
description: "Azure CLI skriptu ukázkové – vytvoření funkce Azure, která se připojuje k Azure Storage"
services: functions
documentationcenter: functions
author: ggailey777
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
ms.author: glenga
ms.custom: mvc
ms.openlocfilehash: 363a3fd1c80538495658720274840b921baa8675
ms.sourcegitcommit: 29bac59f1d62f38740b60274cb4912816ee775ea
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/29/2017
---
# <a name="integrate-function-app-into-azure-storage-account"></a>Integrace funkce aplikace do účtu úložiště Azure

Tento ukázkový skript vytvoří aplikaci funkce a účet úložiště.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Pokud používáte rozhraní příkazového řádku místně, ujistěte se, že používáte Azure CLI verze 2.0 nebo novější. Verzi zjistíte spuštěním příkazu `az --version`. Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI 2.0](/cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Ukázkový skript

Tato ukázka vytvoří aplikaci funkce Azure a přidá připojovací řetězec úložiště do nastavení aplikace.

[!code-azurecli-interactive[main](../../../cli_scripts/azure-functions/create-function-app-connect-to-storage/create-function-app-connect-to-storage-account.sh "Integrate Function App into Azure Storage Account")]


## <a name="clean-up-deployment"></a>Vyčištění nasazení

Po spuštění ukázka skriptu, spusťte následující příkaz k odebrání skupiny prostředků a všechny související prostředky:

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Vysvětlení skriptu

Tento skript používá následující příkazy. Každý příkaz v tabulce odkazy na dokumentaci konkrétní příkaz.

| Příkaz | Poznámky |
|---|---|
| [AZ přihlášení](https://docs.microsoft.com/cli/azure/#login) | Přihlaste se k Azure. |
| [Vytvoření skupiny az](https://docs.microsoft.com/cli/azure/group#az_group_create) | Vytvořte skupinu prostředků s umístěním |
| [Vytvořit účet úložiště az](https://docs.microsoft.com/cli/azure/storage/account) | vytvořit účet úložiště |
| [Vytvoření az functionapp](https://docs.microsoft.com/cli/azure/functionapp#az_functionapp_create) | Vytvořit novou aplikaci funkce |
| [Odstranění skupiny az](https://docs.microsoft.com/cli/azure/group#az_group_delete) | Vyčištění |

## <a name="next-steps"></a>Další kroky

Další informace o rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).

Další ukázky skriptu rozhraní příkazového řádku funkce Azure lze nalézt v [dokumentace Azure Functions](../functions-cli-samples.md).
