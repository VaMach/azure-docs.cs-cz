---
title: "Vytvoření první funkce z rozhraní Azure CLI| Dokumentace Microsoftu"
description: "Naučíte se postup vytvoření první funkce Azure Function pro provádění pomocí Azure CLI bez serveru."
services: functions
keywords: 
author: ggailey777
ms.author: glenga
ms.assetid: 674a01a7-fd34-4775-8b69-893182742ae0
ms.date: 11/08/2017
ms.topic: quickstart
ms.service: functions
ms.custom: mvc
ms.devlang: azure-cli
manager: cfowler
ms.openlocfilehash: 4356d00b2694224f52a9359cd4a57d3a70a34d18
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/15/2017
---
# <a name="create-your-first-function-using-the-azure-cli"></a>Vytvoření první funkce pomocí Azure CLI

Toto téma rychlý úvodní kurz vás provede procesem jak používat Azure Functions k vytvoření první funkce. Pomocí rozhraní příkazového řádku Azure k vytvoření aplikace pro funkce, která je [bez serveru](https://azure.microsoft.com/overview/serverless-computing/) infrastruktury, který je hostitelem funkce. Samotný kód funkce se nasadí z ukázkového úložiště Githubu.    

Následující kroky můžete provést v počítačích se systémem Mac, Windows nebo Linux. 

## <a name="prerequisites"></a>Požadavky 

Před spuštěním této ukázky musíte mít následující:

+ Aktivní účet [GitHub](https://github.com) 
+ Aktivní předplatné Azure.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Pokud si zvolíte instalaci a použití rozhraní příkazového řádku místně, v tomto tématu vyžaduje Azure CLI verze 2.0 nebo novější. Spustit `az --version` najít verzi máte. Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI 2.0]( /cli/azure/install-azure-cli). 


[!INCLUDE [functions-create-resource-group](../../includes/functions-create-resource-group.md)]

[!INCLUDE [functions-create-storage-account](../../includes/functions-create-storage-account.md)]

## <a name="create-a-function-app"></a>Vytvoření Function App

K hostování provádění funkcí musíte mít aplikaci Function App. Function App poskytuje prostředí pro provádění kódu funkce bez serveru. Umožňuje seskupit funkce jako logickou jednotku pro snadnější správu, nasazování a sdílení prostředků. Aplikaci Function App vytvoříte pomocí příkazu [az functionapp create](/cli/azure/functionapp#create). 

V následujícím příkazu nahraďte název jedinečné funkce aplikace, kde uvidíte `<app_name>` zástupný symbol a účet úložiště název pro `<storage_name>`. Jako výchozí doména DNS pro příslušnou aplikaci Function App se použije `<app_name>`, a proto musí být název mezi všemi aplikacemi v Azure jedinečný. _Adresu url nasazení zdroje_ parametr je ukázka úložiště v Githubu, který obsahuje funkci "Hello, World" HTTP aktivované.

```azurecli-interactive
az functionapp create --name <app_name> --storage-account  <storage_name>  --resource-group myResourceGroup \
--consumption-plan-location westeurope --deployment-source-url https://github.com/Azure-Samples/functions-quickstart
```
Nastavení _umístění spotřeba plán_ parametr znamená, funkce aplikace je hostována v hostitelských plánu spotřeby. V tomto plánu prostředky se přidají dynamicky podle požadavků vaší funkce a platíte jenom při funkce fungují. Další informace najdete v tématu [Výběr správného plánu hostování](functions-scale.md). 

Po vytvoření aplikace Function App se v Azure CLI zobrazí podobné informace jako v následujícím příkladu:

```json
{
  "availabilityState": "Normal",
  "clientAffinityEnabled": true,
  "clientCertEnabled": false,
  "containerSize": 1536,
  "dailyMemoryTimeQuota": 0,
  "defaultHostName": "quickstart.azurewebsites.net",
  "enabled": true,
  "enabledHostNames": [
    "quickstart.azurewebsites.net",
    "quickstart.scm.azurewebsites.net"
  ],
   ....
    // Remaining output has been truncated for readability.
}
```

[!INCLUDE [functions-test-function-code](../../includes/functions-test-function-code.md)]

[!INCLUDE [functions-cleanup-resources](../../includes/functions-cleanup-resources.md)]

[!INCLUDE [functions-quickstart-next-steps-cli](../../includes/functions-quickstart-next-steps-cli.md)]