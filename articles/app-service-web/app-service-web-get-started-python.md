---
title: "Vytvoření první webové aplikace v Pythonu v Azure během pěti minut | Dokumentace Microsoftu"
description: "Během několika minut můžete nasadit svou první aplikaci v Pythonu Hello World ve webové aplikaci App Service."
services: app-service\web
documentationcenter: 
author: syntaxc4
manager: erikre
editor: 
ms.assetid: 928ee2e5-6143-4c0c-8546-366f5a3d80ce
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 03/17/2017
ms.author: cfowler
translationtype: Human Translation
ms.sourcegitcommit: 1cc1ee946d8eb2214fd05701b495bbce6d471a49
ms.openlocfilehash: 9bd8db6c765f8f702a6e4ea5b17507269d3310d1
ms.lasthandoff: 04/25/2017


---
# <a name="create-a-python-application-on-web-app"></a>Vytvoření aplikace v Pythonu na základě webové aplikace

Tento rychlý úvodní kurz vás provede vývojem a nasazením aplikace v Pythonu do Azure. Aplikaci spustíme s použitím služby Azure App Service a zde vytvoříme a nakonfigurujeme novou webovou aplikaci pomocí Azure CLI. Poté prostřednictvím gitu nasadíme aplikaci v Pythonu do Azure.

![hello-world-in-browser](media/app-service-web-get-started-python/hello-world-in-browser.png)

Podle následujících kroků můžete postupovat v případě počítačů Mac, Windows nebo Linux. K provedení všech kroků by mělo stačit přibližně 5 minut.

## <a name="before-you-begin"></a>Než začnete

Před spuštěním této ukázky místně nainstalujte následující požadované položky:

1. [Stáhněte a nainstalujte git](https://git-scm.com/).
1. [Stáhněte a nainstalujte Python](https://www.python.org/downloads/).
1. Stáhněte a nainstalujte [rozhraní příkazového řádku Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli).

## <a name="download-the-sample"></a>Stažení ukázky

Naklonujte úložiště ukázkové aplikace Hello World do místního počítače.

```bash
git clone https://github.com/Azure-Samples/python-docs-hello-world
```

> [!TIP]
> Alternativně můžete [stáhnout ukázku](https://github.com/Azure-Samples/Python-docs-hello-world/archive/master.zip) jako soubor ZIP a rozbalit ho.

Přejděte do adresáře, který obsahuje ukázkový kód.

```bash
cd Python-docs-hello-world
```

## <a name="run-the-app-locally"></a>Místní spuštění aplikace

Spusťte aplikaci místně tak, že otevřete okno terminálu a pomocí příkazu `Python` pro ukázku spustíte vestavěný webový server Python.

```bash
python main.py
```

Otevřete webový prohlížeč a přejděte k ukázce.

```bash
http://localhost:5000
```

Na stránce se zobrazí zpráva **Hello World** od ukázkové aplikace.

![localhost-hello-world-in-browser](media/app-service-web-get-started-python/localhost-hello-world-in-browser.png)

V okně terminálu ukončete webový server stisknutím **Ctrl + C**.

## <a name="log-in-to-azure"></a>Přihlaste se k Azure.

Teď prostřednictvím rozhraní příkazového řádku Azure CLI 2.0 v okně terminálu vytvoříme prostředky potřebné pro hostování aplikace v Pythonu v Azure. Přihlaste se k předplatnému Azure pomocí příkazu [az login](/cli/azure/#login) a postupujte podle pokynů na obrazovce.

```azurecli
az login
```

## <a name="configure-a-deployment-user"></a>Konfigurace uživatele nasazení

Pro FTP a místní Git je třeba mít na serveru nakonfigurovaného uživatele nasazení, aby bylo možné nasazení ověřit. Vytvoření uživatele nasazení představuje jednorázovou konfiguraci. Uživatelské jméno a heslo si poznamenejte, protože je použijete v následujícím kroku.

> [!NOTE]
> Uživatel nasazení je vyžadován pro nasazení pomocí FTP a místního Gitu do webové aplikace.
> Položky `username` a `password` jsou na úrovni účtu, a tudíž se liší od přihlašovacích údajů předplatného Azure. **Vytvoření těchto přihlašovacích údajů je vyžadováno pouze jednou.**
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

Následující příklad vytvoří v pracovních procesech systému Linux plán služby App Service s názvem `quickStartPlan` s použitím cenové úrovně **FREE**.

```azurecli
az appservice plan create --name quickStartPlan --resource-group myResourceGroup --sku FREE
```

Po vytvoření plánu služby App Service se v rozhraní příkazového řádku Azure zobrazí podobné informace jako v následujícím příkladu.

```json
{
"appServicePlanName": "quickStartPlan",
"geoRegion": "North Europe",
"id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Web/serverfarms/quickStartPlan",
"kind": "app",
"location": "North Europe",
"maximumNumberOfWorkers": 1,
"name": "quickStartPlan",
"provisioningState": "Succeeded",
"resourceGroup": "myResourceGroup",
"sku": {
  "capacity": 0,
  "family": "F",
  "name": "F1",
  "size": "F1",
  "tier": "Free"
},
"status": "Ready",
"type": "Microsoft.Web/serverfarms",
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
  "enabled": true,
  "enabledHostNames": [
    "<app_name>.azurewebsites.net",
    "<app_name>.scm.azurewebsites.net"
  ],
  "hostNames": [
    "<app_name>.azurewebsites.net"
  ],
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Web/sites/<app_name>",
  "kind": "app",
  "location": "North Europe",
  "outboundIpAddresses": "13.69.190.80,13.69.191.239,13.69.186.193,13.69.187.34",
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

![app-service-web-service-created](media/app-service-web-get-started-python/app-service-web-service-created.png)

Nyní jsme v Azure vytvořili novou prázdnou webovou aplikaci. Teď webovou aplikaci nakonfigurujeme tak, aby používala Python, a nasadíme do ní naši aplikaci.

## <a name="configure-to-use-python"></a>Konfigurace pro použití Pythonu

Pomocí příkazu [az appservice web config update](/cli/azure/app-service/web/config#update) nakonfigurujte webovou aplikaci tak, aby používala Python verze `3.4`.

> [!TIP]
> Tento způsob nastavení verze Pythonu používá výchozí kontejner poskytovaný platformou. Pokud chcete použít vlastní kontejner, vyhledejte referenční informace pro příkaz příkazového řádku [az appservice web config container update](https://docs.microsoft.com/cli/azure/appservice/web/config/container#update).

```azurecli
az appservice web config update --python-version 3.4 --name <app-name> --resource-group myResourceGroup
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
Counting objects: 18, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (16/16), done.
Writing objects: 100% (18/18), 4.31 KiB | 0 bytes/s, done.
Total 18 (delta 4), reused 0 (delta 0)
remote: Updating branch 'master'.
remote: Updating submodules.
remote: Preparing deployment for commit id '44e74fe7dd'.
remote: Generating deployment script.
remote: Generating deployment script for python Web Site
remote: Generated deployment script files
remote: Running deployment command...
remote: Handling python deployment.
remote: KuduSync.NET from: 'D:\home\site\repository' to: 'D:\home\site\wwwroot'
remote: Deleting file: 'hostingstart.html'
remote: Copying file: '.gitignore'
remote: Copying file: 'LICENSE'
remote: Copying file: 'main.py'
remote: Copying file: 'README.md'
remote: Copying file: 'requirements.txt'
remote: Copying file: 'virtualenv_proxy.py'
remote: Copying file: 'web.2.7.config'
remote: Copying file: 'web.3.4.config'
remote: Detected requirements.txt.  You can skip Python specific steps with a .skipPythonDeployment file.
remote: Detecting Python runtime from site configuration
remote: Detected python-3.4
remote: Creating python-3.4 virtual environment.
remote: .................................
remote: Pip install requirements.
remote: Successfully installed Flask click itsdangerous Jinja2 Werkzeug MarkupSafe
remote: Cleaning up...
remote: .
remote: Overwriting web.config with web.3.4.config
remote:         1 file(s) copied.
remote: Finished successfully.
remote: Running post deployment command(s)...
remote: Deployment successful.
To https://<app_name>.scm.azurewebsites.net/<app_name>.git
 * [new branch]      master -> master
```

## <a name="browse-to-the-app"></a>Přechod do aplikace

V prohlížeči zadejte adresu nasazené aplikace.

```bash
http://<app_name>.azurewebsites.net
```

Tentokrát je stránka, která zobrazuje zprávu Hello World, spuštěná pomocí našeho kódu v Pythonu, který je spuštěný jako webová aplikace Azure App Service.

![]()

## <a name="updating-and-deploying-the-code"></a>Aktualizace a nasazení kódu

Pomocí místního textového editoru otevřete soubor `main.py` v rámci aplikace v Pythonu a proveďte malou změnu textu v řetězci vedle příkazu `return`:

```python
return 'Hello, Azure!'
```

Potvrďte změny v gitu a potom odešlete změny kódu do Azure.

```bash
git commit -am "updated output"
git push azure master
```

Po dokončení nasazení se přepněte zpět do okna prohlížeče, které se otevřelo v kroku Přechod do aplikace, a stiskněte tlačítko Aktualizovat.

![hello-world-in-browser](media/app-service-web-get-started-python/hello-world-in-browser.png)

## <a name="manage-your-new-azure-web-app"></a>Správa vaší nové webové aplikace Azure

Přejděte na web Azure Portal a podívejte se na webovou aplikaci, kterou jste právě vytvořili.

Chcete-li to provést, přihlaste se na adrese [https://portal.azure.com](https://portal.azure.com).

V levé nabídce klikněte na **App Services** a pak klikněte na název vaší webové aplikace Azure.

![Navigace portálem k webové aplikaci Azure](./media/app-service-web-get-started-python/Python-docs-hello-world-app-service-list.png)

Dostali jste se do _okna_ vaší webové aplikace (stránka portálu, která se otvírá vodorovně).

Ve výchozím nastavení bude okno vaší webové aplikace obsahovat stránku **Přehled**. Tato stránka poskytuje přehled, jak si vaše aplikace stojí. Tady můžete také provést základní úlohy správy, jako je procházení, zastavení, spuštění, restartování a odstranění. Karty na levé straně okna zobrazují další stránky konfigurace, které můžete otevřít.

![Okno App Service na webu Azure Portal](media/app-service-web-get-started-python/Python-docs-hello-world-app-service-detail.png)

Tyto karty v okně zobrazují mnoho skvělých funkcí, které můžete do své webové aplikace přidat. Následující seznam obsahuje jen několik možností:

* Mapování vlastního názvu DNS
* Vazba vlastního certifikátu SSL
* Konfigurace průběžného nasazování
* Vertikální i horizontální navýšení kapacity
* Přidání ověřování uživatelů

**Blahopřejeme!** Nasadili jste svoji první aplikaci v Pythonu do služby App Service.

[!INCLUDE [cli-samples-clean-up](../../includes/cli-samples-clean-up.md)]

## <a name="next-steps"></a>Další kroky

Prozkoumejte předem vytvořené [skripty rozhraní příkazového řádku pro Web Apps](app-service-cli-samples.md).
