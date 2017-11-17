---
title: "Vytvoření první funkce v systému Linux z příkazového řádku Azure (preview) | Microsoft Docs"
description: "Informace o vytvoření první funkce Azure systémem Linux výchozí image pomocí rozhraní příkazového řádku Azure."
services: functions
keywords: 
author: ggailey777
ms.author: glenga
ms.date: 11/15/2017
ms.topic: quickstart
ms.service: functions
ms.custom: mvc
ms.devlang: azure-cli
manager: cfowler
ms.openlocfilehash: d04e2000f2043e8bb11e15f6b9d7fd06ef5b9da3
ms.sourcegitcommit: 7d107bb9768b7f32ec5d93ae6ede40899cbaa894
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/16/2017
---
# <a name="create-your-first-function-running-on-linux-using-the-azure-cli-preview"></a>Vytvoření první funkce systémem Linux pomocí rozhraní příkazového řádku Azure (preview)

Azure Functions umožňuje hostovat funkcí v systému Linux v kontejneru výchozí službě Azure App Service. Tato funkce je aktuálně ve verzi preview. Můžete také [přineste si vlastní vlastní kontejner](functions-create-function-linux-custom-image.md). 

Toto téma rychlý úvodní kurz vás provede procesem jak používat Azure Functions pomocí rozhraní příkazového řádku Azure k vytvoření první aplikace funkce v systému Linux hostované na výchozí kontejner služby App Service. Samotný kód funkce je nasazený na bitovou kopii z úložiště Githubu ukázka.    

Na počítači Mac, Windows nebo Linux jsou podporovány následující kroky. 

## <a name="prerequisites"></a>Požadavky 

K dokončení tohoto rychlého startu je potřeba:

+ Aktivní předplatné Azure.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Pokud si zvolíte instalaci a použití rozhraní příkazového řádku místně, v tomto tématu vyžaduje Azure CLI verze 2.0.21 nebo novější. Spustit `az --version` najít verzi máte. Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI 2.0]( /cli/azure/install-azure-cli). 

[!INCLUDE [functions-create-resource-group](../../includes/functions-create-resource-group.md)]

[!INCLUDE [functions-create-storage-account](../../includes/functions-create-storage-account.md)]

## <a name="create-a-linux-app-service-plan"></a>Vytvoření plánu služby App Service pro Linux

Linux hostování pro funkce je momentálně podporován pouze na plán služby App Service. Hostování plánu spotřeby není dosud podporováno. Další informace o hostování najdete v tématu [hostování na Azure Functions plány porovnání](functions-scale.md). 

[!INCLUDE [app-service-plan-no-h](../../includes/app-service-web-create-app-service-plan-linux-no-h.md)]

## <a name="create-a-function-app-on-linux"></a>Vytvoření aplikace pro funkce v systému Linux

Musíte mít aplikaci funkce pro hostování provádění funkcí v systému Linux. Funkce aplikace poskytuje prostředí pro provádění kódu funkce. Umožňuje seskupit funkce jako logickou jednotku pro snadnější správu, nasazování a sdílení prostředků. Vytvoření funkce aplikace pomocí [az functionapp vytvořit](/cli/azure/functionapp#create) příkaz plán služby App Service pro Linux. 

V následujícím příkazu nahraďte název jedinečné funkce aplikace, kde uvidíte `<app_name>` zástupný symbol a účet úložiště název pro `<storage_name>`. Jako výchozí doména DNS pro příslušnou aplikaci Function App se použije `<app_name>`, a proto musí být název mezi všemi aplikacemi v Azure jedinečný. _Adresu url nasazení zdroje_ parametr je ukázka úložiště v Githubu, který obsahuje funkci "Hello, World" HTTP aktivované.

```azurecli-interactive
az functionapp create --name <app_name> --storage-account  <storage_name>  --resource-group myResourceGroup \
--plan myAppServicePlan --deployment-source-url https://github.com/Azure-Samples/functions-quickstart-linux
```
Po funkce byla aplikace vytvoří a nasadí, rozhraní příkazového řádku Azure obsahuje informace, podobně jako v následujícím příkladu:

```json
{
  "availabilityState": "Normal",
  "clientAffinityEnabled": true,
  "clientCertEnabled": false,
  "cloningInfo": null,
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

Protože `myAppServicePlan` je plán Linux bitovou kopii integrované docker se používá k vytvoření kontejneru, který spouští aplikaci funkce v systému Linux. 

>[!NOTE]  
>Ukázka úložiště aktuálně obsahuje dva soubory skriptování, [deploy.sh](https://github.com/Azure-Samples/functions-quickstart-linux/blob/master/deploy.sh) a [.deployment](https://github.com/Azure-Samples/functions-quickstart-linux/blob/master/.deployment). Soubor .deployment řídí proces nasazení použít deploy.sh jako [vlastní nasazení skriptu](https://github.com/projectkudu/kudu/wiki/Custom-Deployment-Script). V aktuální verzi preview jsou skripty potřebné k nasazení aplikace funkce na bitovou kopii systému Linux.  

[!INCLUDE [functions-test-function-code](../../includes/functions-test-function-code.md)]

[!INCLUDE [functions-cleanup-resources](../../includes/functions-cleanup-resources.md)]

[!INCLUDE [functions-quickstart-next-steps-cli](../../includes/functions-quickstart-next-steps-cli.md)]
