---
title: "Vytvoření první funkce z rozhraní Azure CLI| Dokumentace Microsoftu"
description: "Naučíte se postup vytvoření první funkce Azure Function pro provádění pomocí Azure CLI bez serveru."
services: functions
keywords: 
author: ggailey777
ms.author: glenga
ms.assetid: 674a01a7-fd34-4775-8b69-893182742ae0
ms.date: 08/22/2017
ms.topic: quickstart
ms.service: functions
ms.custom: mvc
ms.devlang: azure-cli
manager: cfowler
ms.openlocfilehash: ab35963dc9d10134799270e6ab3e6593be0e601a
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="create-your-first-function-using-the-azure-cli"></a>Vytvoření první funkce pomocí Azure CLI

Tento rychlý start vám ukáže, jak používat Azure Functions k vytvoření první funkce. Pomocí Azure CLI vytvoříte aplikaci Function App, což je infrastruktura bez serveru, která je hostitelem funkce. Samotný kód funkce se nasadí z ukázkového úložiště Githubu.    

Následující kroky můžete provést v počítačích se systémem Mac, Windows nebo Linux. 

## <a name="prerequisites"></a>Požadavky 

Před spuštěním této ukázky musíte mít následující:

+ Aktivní účet [GitHub](https://github.com) 
+ Aktivní předplatné Azure.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Pokud si zvolíte instalaci a použití rozhraní příkazového řádku místně, v tomto tématu vyžaduje Azure CLI verze 2.0 nebo novější. Spustit `az --version` najít verzi máte. Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI 2.0]( /cli/azure/install-azure-cli). 


## <a name="create-a-resource-group"></a>Vytvoření skupiny prostředků

Vytvořte skupinu prostředků pomocí příkazu [az group create](/cli/azure/group#create). Skupina prostředků Azure je logický kontejner, ve kterém se nasazují a spravují prostředky Azure, jako například aplikace Function App, databáze a účty úložiště.

Následující příklad vytvoří skupinu prostředků s názvem `myResourceGroup`.  
Pokud nepoužíváte cloudové prostředí, přihlaste se pomocí první `az login`.

```azurecli-interactive
az group create --name myResourceGroup --location westeurope
```


## <a name="create-an-azure-storage-account"></a>Vytvoření účtu služby Azure Storage

Aplikace Functions používá účet Azure Storage k zachování stavu a dalších informací o vašich funkcích. Ve skupině prostředků, kterou jste vytvořili vytvořte účet úložiště pomocí příkazu [az storage account create](/cli/azure/storage/account#create).

V následujícím příkazu nahraďte název účtu globálně jedinečný úložiště, kde uvidíte `<storage_name>` zástupný symbol. Názvy účtů úložiště musí mít od 3 do 24 znaků a můžou obsahovat jenom číslice a malá písmena.

```azurecli-interactive
az storage account create --name <storage_name> --location westeurope --resource-group myResourceGroup --sku Standard_LRS
```

Po vytvoření účtu úložiště se v rozhraní Azure CLI zobrazí podobné informace jako v následujícím příkladu:

```json
{
  "creationTime": "2017-04-15T17:14:39.320307+00:00",
  "id": "/subscriptions/bbbef702-e769-477b-9f16-bc4d3aa97387/resourceGroups/myresourcegroup/...",
  "kind": "Storage",
  "location": "westeurope",
  "name": "myfunctionappstorage",
  "primaryEndpoints": {
    "blob": "https://myfunctionappstorage.blob.core.windows.net/",
    "file": "https://myfunctionappstorage.file.core.windows.net/",
    "queue": "https://myfunctionappstorage.queue.core.windows.net/",
    "table": "https://myfunctionappstorage.table.core.windows.net/"
  },
     ....
    // Remaining output has been truncated for readability.
}
```

## <a name="create-a-function-app"></a>Vytvoření Function App

K hostování provádění funkcí musíte mít aplikaci Function App. Function App poskytuje prostředí pro provádění kódu funkce bez serveru. Umožňuje seskupit funkce jako logickou jednotku pro snadnější správu, nasazování a sdílení prostředků. Aplikaci Function App vytvoříte pomocí příkazu [az functionapp create](/cli/azure/functionapp#create). 

V následujícím příkazu nahraďte název jedinečné funkce aplikace, kde uvidíte `<app_name>` zástupný symbol a účet úložiště název pro `<storage_name>`. Jako výchozí doména DNS pro příslušnou aplikaci Function App se použije `<app_name>`, a proto musí být název mezi všemi aplikacemi v Azure jedinečný. 

```azurecli-interactive
az functionapp create --name <app_name> --storage-account  <storage_name>  --resource-group myResourceGroup \
--consumption-plan-location westeurope
```
Ve výchozím nastavení se aplikace Function App vytvoří s plánem hostování Consumption, což znamená, že se prostředky přidávají dynamicky podle požadavků vašich funkcí a platíte, jenom když funkce běží. Další informace najdete v tématu [Výběr správného plánu hostování](functions-scale.md). 

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

Teď, když máte aplikaci Function App, můžete nasadit samotný kód funkce z ukázkového úložiště GitHub.

## <a name="deploy-your-function-code"></a>Nasazení kódu funkce  

Existuje několik způsobů vytvoření kódu funkce v nové aplikaci Function App. V tomto tématu se připojíte k ukázkovému úložišti v GitHubu. Tak jako předtím nahraďte v následujícím kódu zástupný symbol `<app_name>` názvem aplikace Function App, který jste vytvořili. 

```azurecli-interactive
az functionapp deployment source config --name <app_name> --resource-group myResourceGroup --branch master \
--repo-url https://github.com/Azure-Samples/functions-quickstart \
--manual-integration 
```
Po nastavení zdroj nasazení, rozhraní příkazového řádku Azure uvádí informace podobně jako v následujícím příkladu (hodnoty null Odebrat čitelnější):

```json
{
  "branch": "master",
  "deploymentRollbackEnabled": false,
  "id": "/subscriptions/bbbef702-e769-477b-9f16-bc4d3aa97387/resourceGroups/myResourceGroup/...",
  "isManualIntegration": true,
  "isMercurial": false,
  "location": "West Europe",
  "name": "quickstart",
  "repoUrl": "https://github.com/Azure-Samples/functions-quickstart",
  "resourceGroup": "myResourceGroup",
  "type": "Microsoft.Web/sites/sourcecontrols"
}
```

## <a name="test-the-function"></a>Testování funkce

Na počítačích se systémem Mac nebo Linux otestujete nasazenou funkci pomocí cURL nebo v systému Windows pomocí skriptu Bash. Proveďte následující příkaz cURL, který nahradí zástupný symbol `<app_name>` názvem vaší aplikace Function App. Připojte řetězec dotazu `&name=<yourname>` k adrese URL.

```bash
curl http://<app_name>.azurewebsites.net/api/HttpTriggerJS1?name=<yourname>
```  

![Odpověď funkce se zobrazí v prohlížeči.](./media/functions-create-first-azure-function-azure-cli/functions-azure-cli-function-test-curl.png)  

Pokud nemáte cURL k dispozici v příkazovém řádku, zadejte stejnou adresu URL do pole adresy ve webovém prohlížeči. Znovu nahraďte zástupný symbol `<app_name>` názvem aplikace Function App, připojte řetězec dotazu `&name=<yourname>` k adrese URL a proveďte požadavek. 

    http://<app_name>.azurewebsites.net/api/HttpTriggerJS1?name=<yourname>
   
![Odpověď funkce se zobrazí v prohlížeči.](./media/functions-create-first-azure-function-azure-cli/functions-azure-cli-function-test-browser.png)  

## <a name="clean-up-resources"></a>Vyčištění prostředků

Další rychlé starty v této kolekci jsou postavené na tomto rychlém startu. Pokud chcete pokračovat v práci s dalšími rychlými starty nebo kurzy, nevyčišťujte prostředky vytvořené v rámci tohoto rychlého startu. Pokud pokračovat nechcete, pomocí následujícího příkazu odstraňte všechny prostředky vytvořené tímto rychlým startem:

```azurecli-interactive
az group delete --name myResourceGroup
```
Po zobrazení výzvy zadejte `y`.

## <a name="next-steps"></a>Další kroky

[!INCLUDE [Next steps note](../../includes/functions-quickstart-next-steps.md)]
