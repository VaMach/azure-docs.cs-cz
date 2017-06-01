---
title: "Vytvoření aplikace Node.js ve webové aplikaci Azure | Dokumentace Microsoftu"
description: "Během několika minut můžete nasadit svou první aplikaci Node.js Hello World ve webové aplikaci App Service."
services: app-service\web
documentationcenter: 
author: syntaxc4
manager: erikre
editor: 
ms.assetid: 582bb3c2-164b-42f5-b081-95bfcb7a502a
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 05/05/2017
ms.author: cfowler
ms.custom: mvc
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: ced6f54603120d8832ee417b02b6673f80a99613
ms.contentlocale: cs-cz
ms.lasthandoff: 05/10/2017

---
# <a name="create-a-nodejs-application-on-web-app"></a>Vytvoření aplikace Node.js Application ve webové aplikaci

Tento rychlý úvodní kurz vás provede vývojem a nasazením aplikace Node.js do Azure. Aplikaci spustíme s použitím [plánu služby Azure App Service](https://docs.microsoft.com/azure/app-service/azure-web-sites-web-hosting-plans-in-depth-overview) a vytvoříme a nakonfigurujeme v něm novou webovou aplikaci pomocí Azure CLI. Poté prostřednictvím gitu nasadíme aplikaci Node.js do Azure.

![hello-world-in-browser](media/app-service-web-get-started-nodejs-poc/hello-world-in-browser.png)

Následující postup můžete použít v případě počítačů Mac, Windows nebo Linux. K provedení všech kroků by mělo stačit přibližně 5 minut.

## <a name="prerequisites"></a>Požadavky

Před vytvořením této ukázky si stáhněte a nainstalujte následující položky:

* [Git](https://git-scm.com/)
* [ Node.js a NPM](https://nodejs.org/)
* [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="download-the-sample"></a>Stažení ukázky

Naklonujte úložiště ukázkové aplikace Hello World do místního počítače.

```bash
git clone https://github.com/Azure-Samples/nodejs-docs-hello-world
```

Přejděte do adresáře, který obsahuje ukázkový kód.

```bash
cd nodejs-docs-hello-world
```

## <a name="run-the-app-locally"></a>Místní spuštění aplikace

Spusťte aplikaci místně tak, že otevřete okno terminálu a pomocí skriptu `npm start` pro ukázku spustíte vestavěný server HTTP Node.js.

```bash
npm start
```

Otevřete webový prohlížeč a přejděte k ukázce.

```bash
http://localhost:1337
```

Na stránce se zobrazí zpráva **Hello World** od ukázkové aplikace.

![localhost-hello-world-in-browser](media/app-service-web-get-started-nodejs-poc/localhost-hello-world-in-browser.png)

V okně terminálu ukončete webový server stisknutím **Ctrl + C**.

## <a name="log-in-to-azure"></a>Přihlaste se k Azure.

Teď prostřednictvím rozhraní příkazového řádku Azure CLI 2.0 v okně terminálu vytvoříme prostředky potřebné pro hostování aplikace Node.js v Azure. Přihlaste se k předplatnému Azure pomocí příkazu [az login](/cli/azure/#login) a postupujte podle pokynů na obrazovce.

```azurecli
az login
```

<!-- ## Configure a Deployment User -->
[!INCLUDE [login-to-azure](../../includes/configure-deployment-user.md)]

## <a name="create-a-resource-group"></a>Vytvoření skupiny prostředků

Vytvořte skupinu prostředků pomocí příkazu [az group create](/cli/azure/group#create). Skupina prostředků Azure je logický kontejner, ve kterém se nasazují a spravují prostředky Azure, jako například webové aplikace, databáze a účty úložiště.

```azurecli
az group create --name myResourceGroup --location westeurope
```

## <a name="create-an-azure-app-service-plan"></a>Vytvoření plánu služby Azure App Service

Pomocí příkazu [az appservice plan create](/cli/azure/appservice/plan#create) vytvořte [plán služby App Service](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) na úrovni FREE.

<!--
 An App Service plan represents the collection of physical resources used to ..
-->
[!INCLUDE [app-service-plan](../../includes/app-service-plan.md)]

Následující příklad vytvoří plán služby App Service s názvem `quickStartPlan` a s cenovou úrovní **Free**.

```azurecli
az appservice plan create --name quickStartPlan --resource-group myResourceGroup --sku FREE
```

Po vytvoření plánu služby App Service se v rozhraní CLI Azure zobrazí podobné informace jako v následujícím příkladu:

```json
{
    "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Web/serverfarms/quickStartPlan",
    "location": "West Europe",
    "sku": {
    "capacity": 1,
    "family": "S",
    "name": "S1",
    "tier": "Standard"
    },
    "status": "Ready",
    "type": "Microsoft.Web/serverfarms"
}
```

## <a name="create-a-web-app"></a>Vytvoření webové aplikace

Po vytvoření plánu služby App Service teď vytvořte v rámci plánu služby App Service `quickStartPlan` [webovou aplikaci](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview). Tato aplikace poskytuje prostor hostitele pro nasazení kódu a také adresu URL, jejímž prostřednictvím lze nasazenou aplikaci zobrazit. Pomocí příkazu [az appservice web create](/cli/azure/appservice/web#create) vytvořte webovou aplikaci.

V následujícím příkazu nahraďte zástupný symbol `<app_name>` vlastním jedinečným názvem aplikace. `<app_name>` se používá ve výchozí lokalitě DNS pro webovou aplikaci. Pokud název `<app_name>` není jedinečný, zobrazí se popisná chybová zpráva „Web s názvem <app_name> již existuje“.

<!-- removed per https://github.com/Microsoft/azure-docs-pr/issues/11878
You can later map any custom DNS entry to the web app before you expose it to your users.
-->

```azurecli
az appservice web create --name <app_name> --resource-group myResourceGroup --plan quickStartPlan
```

Po vytvoření webové aplikace se v rámci rozhraní příkazového řádku Azure zobrazí podobné informace jako v následujícím příkladu.

```json
{
    "clientAffinityEnabled": true,
    "defaultHostName": "<app_name>.azurewebsites.net",
    "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Web/sites/<app_name>",
    "isDefaultContainer": null,
    "kind": "app",
    "location": "West Europe",
    "name": "<app_name>",
    "repositorySiteName": "<app_name>",
    "reserved": true,
    "resourceGroup": "myResourceGroup",
    "serverFarmId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Web/serverfarms/quickStartPlan",
    "state": "Running",
    "type": "Microsoft.Web/sites",
}
```

Přejděte na web a zobrazte nově vytvořenou webovou aplikaci.

```bash
http://<app_name>.azurewebsites.net
```

![app-service-web-service-created](media/app-service-web-get-started-nodejs-poc/app-service-web-service-created.png)

Nyní jsme v Azure vytvořili novou prázdnou webovou aplikaci.

## <a name="configure-local-git-deployment"></a>Konfigurace nasazení z místního Gitu

K nasazení do webové aplikace můžete použít celou řadu způsobů, včetně FTP, místního Gitu, GitHubu, Visual Studio Team Services nebo Bitbucketu.

Pomocí příkazu [az appservice web source-control config-local-git](/cli/azure/appservice/web/source-control#config-local-git) nastavte přístup k webové aplikaci přes místní git.

```azurecli
az appservice web source-control config-local-git --name <app_name> --resource-group myResourceGroup --query url --output tsv
```

Zkopírujte výstup z terminálu, budete ho potřebovat v dalším kroku.

```bash
https://<username>@<app_name>.scm.azurewebsites.net:443/<app_name>.git
```

## <a name="push-to-azure-from-git"></a>Přenos z Gitu do Azure

Přidejte vzdálené úložiště Azure do místního úložiště Gitu.

```bash
git remote add azure <paste-previous-command-output-here>
```

Nasaďte aplikaci do vzdáleného úložiště Azure. Zobrazí se výzva k zadání hesla, které jste zadali už dřív, při vytváření uživatele nasazení. Dejte pozor na to, abyste zadali heslo, které jste vytvořili v kroku [Konfigurace uživatele nasazení](#configure-a-deployment-user), ne heslo, se kterým se přihlašujete na web Azure Portal.

```bash
git push azure master
```

Během nasazení bude služba Azure App Service hlásit Gitu průběh nasazení.

```bash
Counting objects: 23, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (21/21), done.
Writing objects: 100% (23/23), 3.71 KiB | 0 bytes/s, done.
Total 23 (delta 8), reused 7 (delta 1)
remote: Updating branch 'master'.
remote: Updating submodules.
remote: Preparing deployment for commit id 'bf114df591'.
remote: Generating deployment script.
remote: Generating deployment script for node.js Web Site
remote: Generated deployment script files
remote: Running deployment command...
remote: Handling node.js deployment.
remote: Kudu sync from: '/home/site/repository' to: '/home/site/wwwroot'
remote: Copying file: '.gitignore'
remote: Copying file: 'LICENSE'
remote: Copying file: 'README.md'
remote: Copying file: 'index.js'
remote: Copying file: 'package.json'
remote: Copying file: 'process.json'
remote: Deleting file: 'hostingstart.html'
remote: Ignoring: .git
remote: Using start-up script index.js from package.json.
remote: Node.js versions available on the platform are: 4.4.7, 4.5.0, 6.2.2, 6.6.0, 6.9.1.
remote: Selected node.js version 6.9.1. Use package.json file to choose a different version.
remote: Selected npm version 3.10.8
remote: Finished successfully.
remote: Running post deployment command(s)...
remote: Deployment successful.
To https://<app_name>.scm.azurewebsites.net:443/<app_name>.git
 * [new branch]      master -> master
```

## <a name="browse-to-the-app"></a>Přechod do aplikace

V prohlížeči zadejte adresu nasazené aplikace.

```bash
http://<app_name>.azurewebsites.net
```

Tentokrát je stránka, která zobrazuje zprávu Hello World, spuštěná pomocí našeho kódu Node.js, který je spuštěný jako webová aplikace Azure App Service.

## <a name="updating-and-deploying-the-code"></a>Aktualizace a nasazení kódu

Pomocí místního textového editoru otevřete soubor `index.js`, který je součástí aplikace Node.js, a proveďte malou změnu textu v rámci volání `response.end`:

```nodejs
response.end("Hello Azure!");
```

Potvrďte změny v gitu a potom odešlete změny kódu do Azure.

```bash
git commit -am "updated output"
git push azure master
```

Po dokončení nasazení se vraťte do okna prohlížeče, které se otevřelo v kroku **Přechod do aplikace**, a stiskněte tlačítko Aktualizovat.

![hello-world-in-browser](media/app-service-web-get-started-nodejs-poc/hello-world-in-browser.png)

## <a name="manage-your-new-azure-web-app"></a>Správa vaší nové webové aplikace Azure

Přejděte na web Azure Portal a podívejte se na webovou aplikaci, kterou jste právě vytvořili.

Chcete-li to provést, přihlaste se na adrese [https://portal.azure.com](https://portal.azure.com).

V levé nabídce klikněte na **App Services** a pak klikněte na název vaší webové aplikace Azure.

![Navigace portálem k webové aplikaci Azure](./media/app-service-web-get-started-nodejs-poc/nodejs-docs-hello-world-app-service-list.png)

Dostali jste se do _okna_ vaší webové aplikace (stránka portálu, která se otvírá vodorovně).

Ve výchozím nastavení bude okno vaší webové aplikace obsahovat stránku **Přehled**. Tato stránka poskytuje přehled, jak si vaše aplikace stojí. Tady můžete také provést základní úlohy správy, jako je procházení, zastavení, spuštění, restartování a odstranění. Karty na levé straně okna zobrazují další stránky konfigurace, které můžete otevřít.

![Okno App Service na webu Azure Portal](media/app-service-web-get-started-nodejs-poc/nodejs-docs-hello-world-app-service-detail.png)

Tyto karty v okně zobrazují mnoho skvělých funkcí, které můžete do své webové aplikace přidat. Následující seznam obsahuje jen několik možností:

* Mapování vlastního názvu DNS
* Vazba vlastního certifikátu SSL
* Konfigurace průběžného nasazování
* Vertikální i horizontální navýšení kapacity
* Přidání ověřování uživatelů

**Blahopřejeme!** Nasadili jste svoji první aplikaci Node.js do služby App Service.

[!INCLUDE [cli-samples-clean-up](../../includes/cli-samples-clean-up.md)]

> [!div class="nextstepaction"]
> [Prozkoumejte ukázkové skripty rozhraní CLI služby Web Apps](app-service-cli-samples.md)

