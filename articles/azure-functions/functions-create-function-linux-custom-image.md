---
title: "Vytvoření funkce v systému Linux pomocí vlastní image (preview) | Microsoft Docs"
description: "Naučte se vytvářet Azure Functions systémem vlastní image Linux."
services: functions
keywords: 
author: ggailey777
ms.author: glenga
ms.date: 11/08/2017
ms.topic: article
ms.service: functions
ms.custom: mvc
ms.devlang: azure-cli
manager: cfowler
ms.openlocfilehash: 40fbbd0ec020320b26ed343aec8ac31d60b646dc
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/15/2017
---
# <a name="create-a-function-on-linux-using-a-custom-image-preview"></a>Vytvoření funkce v systému Linux pomocí vlastní image (preview)

Azure Functions umožňuje hostovat funkcí v systému Linux v vlastní vlastní kontejneru. Tato funkce je aktuálně ve verzi preview. Můžete také [hostitele na výchozí kontejner Azure App Service](functions-create-first-azure-function-azure-cli-linux.md).  

V tomto kurzu zjistěte, jak nasadit aplikaci funkce jako vlastní image Docker. Tento vzor je užitečné, když potřebujete Přizpůsobení předdefinované image kontejneru služby App Service. Můžete použít vlastní image při funkcí potřebovat konkrétní jazykové verze nebo vyžadovat konkrétní závislosti nebo konfigurace, která není k dispozici v rámci integrované bitové kopie.

Tento kurz vás provede procesem jak používat Azure Functions a vytvořit vlastní image odešlete do úložiště Docker Hub. Pak můžete použít tuto bitovou kopii jako zdroj nasazení pro funkce aplikace, která běží na systému Linux. Docker použijete k sestavení a push bitovou kopii. Použijete rozhraní příkazového řádku Azure k vytvoření funkce aplikace a nasazení bitové kopie z úložiště Docker Hub. 

V tomto kurzu se naučíte:

> [!div class="checklist"]
> * Vytvořte vlastní image pomocí Docker.
> * Publikujte vlastní image do registru kontejneru. 
> * Vytvoření účtu úložiště Azure. 
> * Vytvoření plánu služby App Service pro Linux. 
> * Nasaďte aplikaci funkce z úložiště Docker Hub.
> * Přidáte nastavení aplikace do aplikaci funkce. 

Na počítači Mac, Windows nebo Linux jsou podporovány následující kroky.  

## <a name="prerequisites"></a>Požadavky

Pro absolvování tohoto kurzu potřebujete:

* [Git](https://git-scm.com/downloads)
* Aktivní [předplatného Azure](https://azure.microsoft.com/pricing/free-trial/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio)
* [Docker](https://docs.docker.com/get-started/#setup)
* A [účet úložiště Docker Hub](https://docs.docker.com/docker-id/)

[!INCLUDE [Free trial note](../../includes/quickstarts-free-trial-note.md)]

## <a name="download-the-sample"></a>Stažení ukázky

V okně terminálu spusťte následující příkaz klonovat úložiště ukázkové aplikace do místního počítače a pak přejděte do adresáře, který obsahuje ukázkový kód.

```bash
git clone https://github.com/Azure-Samples/functions-linux-custom-image.git --config core.autocrlf=input
cd functions-linux-custom-image
```

## <a name="build-the-image-from-the-docker-file"></a>Sestavení bitové kopie ze souboru Docker

V tomto úložišti Git, podívejte se na _soubor Docker_. Tento soubor popisuje prostředí, které je potřeba spustit aplikaci funkce v systému Linux. 

```docker
# Base the image on the built-in Azure Functions Linux image.
FROM microsoft/azure-functions-runtime:2.0.0-jessie
ENV AzureWebJobsScriptRoot=/home/site/wwwroot

# Add files from this repo to the root site folder.
COPY . /home/site/wwwroot 
```
>[!NOTE]
> Při hostování bitovou kopii v registru kontejner privátní, měli byste přidat nastavení připojení k aplikaci funkce pomocí **ENV** proměnné v soubor Docker. Vzhledem k tomu, že v tomto kurzu nemůže zaručit, že používáte privátní registru, jsou nastavení připojení [přidat po nasazení pomocí rozhraní příkazového řádku Azure](#configure-the-function-app) jako osvědčený postup zabezpečení.   

### <a name="run-the-build-command"></a>Spusťte příkaz sestavení
Chcete-li vytvořit bitovou kopii Docker, spusťte `docker build` příkaz a zadejte název, `mydockerimage`a značky, `v1.0.0`. Nahraďte `<docker-id>` s úložiště Docker Hub účet ID.

```bash
docker build --tag <docker-id>/mydockerimage:v1.0.0 .
```

Příkaz vytvoří výstup podobný následujícímu:

```bash
Sending build context to Docker daemon  169.5kB
Step 1/3 : FROM microsoft/azure-functions-runtime:v2.0.0-jessie
v2.0.0-jessie: Pulling from microsoft/azure-functions-runtime
b178b12f7913: Pull complete
2d9ce077a781: Pull complete
4775d4ba55c8: Pull complete
Digest: sha256:073f45fc167b3b5c6642ef4b3c99064430d6b17507095...
Status: Downloaded newer image for microsoft/azure-functions-runtime:v2.0.0-jessie
 ---> 217799efa500
Step 2/3 : ENV AzureWebJobsScriptRoot /home/site/wwwroot
 ---> Running in 528fa2077d17
 ---> 7cc6323b8ae0
Removing intermediate container 528fa2077d17
Step 3/3 : COPY . /home/site/wwwroot
 ---> 5bdac9878423
Successfully built 5bdac9878423
Successfully tagged ggailey777/mydockerimage:v1.0.0
```

### <a name="test-the-image-locally"></a>Obrázek místně testu
Ověřte, že bitovou kopii integrovaný funguje tak, že spustíte bitovou kopii Docker v místní kontejneru. Problém [docker spustit](https://docs.docker.com/engine/reference/commandline/run/) příkazů a jí předat název a značka obrázku. Nezapomeňte zadat pomocí portu `-p` argument.

```bash
docker run -p 8080:80 -it <docker-ID>/mydockerimage:v1.0.0
```

Vlastní Image spuštěn v místním kontejner Docker, ověřte funkce aplikace a kontejner fungují správně procházením <adrese http://localhost: 8080>.

![Testování funkce aplikace místně.](./media/functions-create-function-linux-custom-image/run-image-local-success.png)

Po ověření aplikaci funkce v kontejneru hav můžete zastavte provádění. Teď můžete posouvat vlastní image do účtu úložiště Docker Hub.

## <a name="push-the-custom-image-to-docker-hub"></a>Vlastní image odešlete do úložiště Docker Hub

Registr je aplikace, která hostuje bitové kopie a poskytuje služby bitové kopie a kontejneru. Aby bylo možné sdílet bitové kopie, musí poslat ho přímo registru. Úložiště docker Hub je registr pro Docker bitové kopie, který umožňuje hostování vlastní úložiště, veřejné nebo soukromé. 

Předtím, než můžete posouvat bitovou kopii, musíte se přihlásit pomocí Docker Hub [docker přihlášení](https://docs.docker.com/engine/reference/commandline/login/) příkaz. Nahraďte `<docker-id>` s názvem účtu a zadejte heslo do konzoly příkazového řádku. Další možnosti hesla úložiště Docker Hub, najdete v článku [docker přihlášení příkaz dokumentaci](https://docs.docker.com/engine/reference/commandline/login/).

```bash
docker login --username <docker-id> 
```

Zpráva "přihlášení bylo úspěšné" potvrdí, že jste přihlášeni. Po přihlášení, nabízené bitovou kopii do úložiště Docker Hub pomocí [docker nabízené](https://docs.docker.com/engine/reference/commandline/push/) příkaz.

```bash
docker push <docker-id>/mydockerimage:v1.0.0 .
```

Ověřte, že nabízené úspěšné prověřením příkaz výstupu.

```bash
The push refers to a repository [docker.io/<docker-id>/mydockerimage:v1.0.0]
24d81eb139bf: Pushed
fd9e998161c9: Mounted from microsoft/azure-functions-runtime
e7796c35add2: Mounted from microsoft/azure-functions-runtime
ae9a05b85848: Mounted from microsoft/azure-functions-runtime
45c86e20670d: Mounted from microsoft/azure-functions-runtime
v1.0.0: digest: sha256:be080d80770df71234eb893fbe4d... size: 2422
```
Teď můžete použít tuto bitovou kopii jako zdroj nasazení pro novou aplikaci funkce v Azure. 

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Pokud si zvolíte instalaci a použití rozhraní příkazového řádku místně, v tomto tématu vyžaduje Azure CLI verze 2.0 nebo novější. Spustit `az --version` najít verzi máte. Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI 2.0]( /cli/azure/install-azure-cli). 

[!INCLUDE [functions-create-resource-group](../../includes/functions-create-resource-group.md)]

[!INCLUDE [functions-create-storage-account](../../includes/functions-create-storage-account.md)]

## <a name="create-a-linux-app-service-plan"></a>Vytvoření plánu služby App Service pro Linux

Linux hostování pro funkce není aktuálně podporováno v plánech spotřeby. Je nutné spustit v plánu služby App Service pro Linux. Další informace o hostování najdete v tématu [hostování na Azure Functions plány porovnání](functions-scale.md). 

[!INCLUDE [app-service-plan-no-h](../../includes/app-service-web-create-app-service-plan-linux-no-h.md)]


## <a name="create-and-deploy-the-custom-image"></a>Vytvořit a nasadit vlastní image

Funkce aplikace hostuje provádění funkcí. Vytvoření funkce aplikace z image úložiště Docker Hub pomocí [vytvořit az functionapp](/cli/azure/functionapp#create) příkaz. 

V následujícím příkazu nahraďte název jedinečné funkce aplikace, kde uvidíte `<app_name>` zástupný symbol a účet úložiště název pro `<storage_name>`. Jako výchozí doména DNS pro příslušnou aplikaci Function App se použije `<app_name>`, a proto musí být název mezi všemi aplikacemi v Azure jedinečný. Jako předtím `<docker-id>` je název účtu Docker.

```azurecli-interactive
az functionapp create --name <app_name> --storage-account  <storage_name>  --resource-group myResourceGroup \
--plan myAppServicePlan --deployment-container-image-name <docker-id>/mydockerimage:v1.0.0 
```
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

_Nasazení kontejneru image-name_ parametr označuje bitovou kopii hostované na úložiště Docker Hub sloužící k vytvoření aplikace funkce. 


## <a name="configure-the-function-app"></a>Konfigurace aplikace – funkce

Funkce musí připojovací řetězec pro připojení k výchozí účet úložiště. Při publikování vlastní bitovou kopii na účet kontejner privátní, místo toho musí nastavená tato nastavení aplikace jako proměnné prostředí v soubor Docker pomocí [ENV instrukce](https://docs.docker.com/engine/reference/builder/#env), nebo ekvivalentní. 

V takovém případě `<storage_account>` je název účtu úložiště, který jste vytvořili. Získání připojovacího řetězce s [az úložiště účet zobrazit. připojovací řetězec](/cli/azure/storage/account#show-connection-string) příkaz. Přidání těchto nastavení aplikace v aplikaci funkce pomocí [az functionapp konfigurace appsettings sadu](/cli/azure/functionapp/config/appsettings#set) příkaz.

```azurecli-interactive
storageConnectionString=$(az storage account show-connection-string \
--resource-group myResourceGroup --name <storage_account> \
--query connectionString --output tsv)

az functionapp config appsettings set --name <function_app> \
--resource-group myResourceGroup \
--settings AzureWebJobsDashboard=$storageConnectionString \
AzureWebJobsStorage=$storageConnectionString
```

Nyní můžete otestovat funkcí systémem Linux v Azure.

[!INCLUDE [functions-test-function-code](../../includes/functions-test-function-code.md)]

[!INCLUDE [functions-cleanup-resources](../../includes/functions-cleanup-resources.md)]

## <a name="next-steps"></a>Další kroky

V tomto kurzu jste se naučili:

> [!div class="checklist"]
> * Vytvořte vlastní image pomocí Docker.
> * Publikujte vlastní image do registru kontejneru. 
> * Vytvoření účtu úložiště Azure. 
> * Vytvoření plánu služby App Service pro Linux. 
> * Nasaďte aplikaci funkce z úložiště Docker Hub.
> * Přidáte nastavení aplikace do aplikaci funkce.

Další informace o vývoji Azure Functions místně pomocí nástroje Azure funkce jádra.

> [!div class="nextstepaction"] 
> [Kód a testovat místně na Azure Functions](functions-run-local.md)
