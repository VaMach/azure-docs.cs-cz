---
title: "Vytvoření aplikace Node.js ve webové aplikaci | Dokumenty Microsoft"
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
ms.date: 03/28/2017
ms.author: cfowler
translationtype: Human Translation
ms.sourcegitcommit: b0c27ca561567ff002bbb864846b7a3ea95d7fa3
ms.openlocfilehash: c32cb52e4bb7bacde20e21820f277b4e86877e74
ms.lasthandoff: 04/25/2017

---
# <a name="create-a-nodejs-application-on-web-app"></a>Vytvoření aplikace Node.js Application ve webové aplikaci

Tento rychlý úvodní kurz vás provede vývojem a nasazením aplikace Node.js do Azure. Aplikaci spustíme s použitím Azure App Service v prostředí Linuxu a zde vytvoříme a nakonfigurujeme novou webovou aplikaci s použitím rozhraní příkazového řádku Azure. Poté prostřednictvím gitu nasadíme aplikaci Node.js do Azure.

![hello-world-in-browser](media/app-service-web-get-started-nodejs-poc/hello-world-in-browser.png)

Podle následujících kroků můžete postupovat v případě počítačů Mac, Windows nebo Linux. K provedení všech kroků by mělo stačit přibližně 5 minut.

## <a name="before-you-begin"></a>Než začnete

Před spuštěním této ukázky místně nainstalujte následující požadované položky:

1. [Stáhněte a nainstalujte git](https://git-scm.com/).
1. [Stáhněte a nainstalujte Node.js a NPM](https://nodejs.org/)
1. Stáhněte a nainstalujte [rozhraní příkazového řádku Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli).

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="download-the-sample"></a>Stažení ukázky

Naklonujte úložiště ukázkové aplikace Hello World do místního počítače.

```bash
git clone https://github.com/Azure-Samples/nodejs-docs-hello-world
```

> [!TIP]
> Alternativně můžete [stáhnout ukázku](https://github.com/Azure-Samples/nodejs-docs-hello-world/archive/master.zip) jako soubor ZIP a rozbalit ho.

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

## <a name="configure-a-deployment-user"></a>Konfigurace uživatele nasazení

Pro FTP a místní Git je třeba mít na serveru nakonfigurovaného uživatele nasazení, aby bylo možné nasazení ověřit. Vytvoření uživatele nasazení představuje jednorázovou konfiguraci. Uživatelské jméno a heslo si poznamenejte, protože je použijete v následujícím kroku.

> [!NOTE]
> Uživatel nasazení je vyžadován pro nasazení pomocí FTP a místního Gitu do webové aplikace.
> Položky `username` a `password` jsou na úrovni účtu, a tudíž se liší od přihlašovacích údajů předplatného Azure. Vytvoření těchto přihlašovacích údajů je vyžadováno pouze jednou.
>

Pomocí příkazu [az appservice web deployment user set](/cli/azure/appservice/web/deployment/user#set) vytvořte své přihlašovací údaje na úrovni účtu.

```azurecli
az appservice web deployment user set --user-name <username> --password <password>
```

## <a name="create-a-resource-group"></a>Vytvoření skupiny prostředků

Vytvořte skupinu prostředků pomocí příkazu [az group create](/cli/azure/group#create). Skupina prostředků Azure je logický kontejner, ve kterém se nasazují a spravují prostředky Azure, jako například webové aplikace, databáze a účty úložiště.

```azurecli
az group create --name myResourceGroup --location westeurope
```

## <a name="create-an-azure-app-service"></a>Vytvoření služby Azure App Service

Pomocí příkazu [az appservice plan create](/cli/azure/appservice/plan#create) vytvoříte plán služby App Service pro systém Linux.

> [!NOTE]
> Plán služby App Service představuje kolekci fyzických prostředků použitých k hostování vašich aplikací. Všechny aplikace přiřazené k plánu služby App Service sdílí službou definované prostředky, a tím umožňují snížení nákladů při hostování více aplikací.
>
> Plány služby App Service definují:
> * Oblast (Severní Evropa, Východní USA, Jihovýchodní Asie)
> * Velikost instance (Malá, Střední, Velká)
> * Počet škálování (jedna, dvě nebo tři instance atd.)
> * SKU (Free, Shared, Basic, Standard, Premium)
>

Následující příklad vytvoří v pracovních procesech systému Linux plán služby App Service s názvem `quickStartPlan` s použitím cenové úrovně **Standard**.

```azurecli
az appservice plan create --name quickStartPlan --resource-group myResourceGroup --sku S1 --is-linux
```

Po vytvoření plánu služby App Service se v rozhraní příkazového řádku Azure zobrazí podobné informace jako v následujícím příkladu.

```json
{
    "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Web/serverfarms/quickStartPlan",
    "kind": "linux",
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

Po vytvoření plánu služby App Service teď vytvořte v rámci plánu služby App Service `quickStartPlan` webovou aplikaci. Tato aplikace poskytuje prostor hostitele pro nasazení kódu a také adresu URL, jejímž prostřednictvím lze nasazenou aplikaci zobrazit. Pomocí příkazu [az appservice web create](/cli/azure/appservice/web#create) vytvořte webovou aplikaci.

V následujícím příkazu nahraďte zástupný symbol `<app_name>` vlastním jedinečným názvem aplikace. `<app_name>` se použije jako výchozí název DNS pro příslušnou webovou aplikaci, proto musí být mezi všemi aplikacemi v Azure jedinečný. Později můžete na webovou aplikaci namapovat libovolné vlastní záznamy DNS, než ji zpřístupníte uživatelům.

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

Nyní jsme v Azure vytvořili novou prázdnou webovou aplikaci. Teď webovou aplikaci nakonfigurujeme tak, aby používala Node.js, a aplikaci tam nasadíme.

## <a name="configure-to-use-nodejs"></a>Konfigurace použití Node.js

Pomocí příkazu [az appservice web config update](/cli/azure/app-service/web/config#update) nakonfigurujte webovou aplikaci tak, aby používala Node.js verze `6.9.3`.

> [!TIP]
> Nastavení verze node.js tímto způsobem používá výchozí kontejner poskytovaný platformou. Pokud chcete použít vlastní kontejner, prostudujte si referenci k rozhraní příkazového řádku pro příkaz [az appservice web config container update](/cli/azure/appservice/web/config/container#update).

```azurecli
az appservice web config update --linux-fx-version "NODE|6.9.3" --startup-file process.json --name <app_name> --resource-group myResourceGroup
```

## <a name="configure-local-git-deployment"></a>Konfigurace nasazení místního gitu

K nasazení do webové aplikace můžete použít celou řadu způsobů, včetně FTP, místního Gitu, GitHubu, služby Visual Studio Team Services nebo Bitbucketu.

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

Nasaďte aplikaci do vzdáleného úložiště Azure. Zobrazí se výzva k zadání hesla, které jste zadali dříve v rámci vytváření nasazení uživatele.

```azurecli
git push azure master
```

Během nasazení bude služba Azure App Service komunikovat průběh nasazování s Gitem.

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

Po dokončení nasazení se přepněte zpět do okna prohlížeče, které se otevřelo v kroku Přechod do aplikace, a stiskněte tlačítko Aktualizovat.

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

## <a name="next-steps"></a>Další kroky

Prozkoumejte předem vytvořené [skripty rozhraní příkazového řádku pro Web Apps](app-service-cli-samples.md).

