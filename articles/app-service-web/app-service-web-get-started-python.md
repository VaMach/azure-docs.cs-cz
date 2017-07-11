---
title: "Vytvoření webové aplikace v Pythonu v Azure | Dokumentace Microsoftu"
description: "Během několika minut můžete nasadit svou první aplikaci Hello World v Pythonu pomocí služby Azure App Service Web Apps."
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
ms.custom: mvc
ms.translationtype: Human Translation
ms.sourcegitcommit: 857267f46f6a2d545fc402ebf3a12f21c62ecd21
ms.openlocfilehash: 233db1cb74a6c81cf044953ecdf6e9de6cc50ee8
ms.contentlocale: cs-cz
ms.lasthandoff: 06/28/2017

---
<a id="create-a-python-web-app-in-azure" class="xliff"></a>

# Vytvoření webové aplikace v Pythonu v Azure

[Azure Web Apps](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview) je vysoce škálovatelná služba s automatickými opravami pro hostování webů.  Tento kurz Rychlý start vás provede vývojem a nasazením aplikace v Pythonu do Azure Web Apps. Vytvoříte webovou aplikaci pomocí rozhraní příkazového řádku [Azure CLI](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli) a pomocí Gitu nasadíte do této webové aplikace ukázkový kód v Pythonu.

![Ukázková aplikace spuštěná v Azure](media/app-service-web-get-started-python/hello-world-in-browser.png)

Následující postup můžete použít v případě počítačů Mac, Windows nebo Linux. Pokud máte nainstalované všechny požadované prostředky, zabere vám tento postup zhruba pět minut.
<a id="prerequisites" class="xliff"></a>

## Požadavky

Pro absolvování tohoto kurzu potřebujete:

1. [Nainstalovat Git](https://git-scm.com/).
1. [Nainstalovat Python](https://www.python.org/downloads/).

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Pokud se rozhodnete nainstalovat a používat rozhraní příkazového řádku (CLI) místně, musíte mít spuštěnou verzi Azure CLI 2.0 nebo novější. Verzi zjistíte spuštěním příkazu `az --version`. Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI 2.0]( /cli/azure/install-azure-cli). 

<a id="download-the-sample" class="xliff"></a>

## Stažení ukázky

V okně terminálu naklonujte spuštěním následujícího příkazu úložiště ukázkové aplikace do místního počítače.

```bash
git clone https://github.com/Azure-Samples/python-docs-hello-world
```

Toto okno terminálu budete používat ke spuštění všech příkazů v tomto kurzu Rychlý start.

Přejděte do adresáře, který obsahuje ukázkový kód.

```bash
cd Python-docs-hello-world
```

<a id="run-the-app-locally" class="xliff"></a>

## Místní spuštění aplikace

Aplikaci spustíte místně tak, že otevřete okno terminálu a pomocí příkazu `Python` spustíte integrovaný webový server Python.

```bash
python main.py
```

Otevřete webový prohlížeč a přejděte na ukázkovou aplikaci na adrese http://localhost:5000.

Na stránce se zobrazí zpráva **Hello World** od ukázkové aplikace.

![Ukázková aplikace spuštěná místně](media/app-service-web-get-started-python/localhost-hello-world-in-browser.png)

V okně terminálu ukončete webový server stisknutím **Ctrl + C**.

[!INCLUDE [Log in to Azure](../../includes/login-to-azure.md)] 

[!INCLUDE [Configure deployment user](../../includes/configure-deployment-user.md)] 

[!INCLUDE [Create resource group](../../includes/app-service-web-create-resource-group.md)] 

[!INCLUDE [Create app service plan](../../includes/app-service-web-create-app-service-plan.md)] 

[!INCLUDE [Create web app](../../includes/app-service-web-create-web-app.md)] 

![Prázdná stránka webové aplikace](media/app-service-web-get-started-python/app-service-web-service-created.png)

Nyní jste v Azure vytvořili novou prázdnou webovou aplikaci.

<a id="configure-to-use-python" class="xliff"></a>

## Konfigurace pro použití Pythonu

Pomocí příkazu [az webapp config set](/cli/azure/webapp/config#set) nakonfigurujte webovou aplikaci tak, aby používala Python verze `3.4`.

```azurecli-interactive
az webapp config set --python-version 3.4 --name <app_name> --resource-group myResourceGroup
```


Pokud nastavíte verzi Pythonu tímto způsobem, použije se výchozí kontejner poskytnutý platformou. Pokud chcete použít vlastní kontejner, v referenci k rozhraní CLI vyhledejte příkaz [az webapp config container set](/cli/azure/webapp/config/container#set).

[!INCLUDE [Configure local git](../../includes/app-service-web-configure-local-git.md)] 

[!INCLUDE [Push to Azure](../../includes/app-service-web-git-push-to-azure.md)] 

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

<a id="browse-to-the-app" class="xliff"></a>

## Přechod do aplikace

V prohlížeči zadejte adresu nasazené aplikace.

```bash
http://<app_name>.azurewebsites.net
```

Ukázkový kód Pythonu je spuštěný ve webové aplikaci služby Azure App Service.

![Ukázková aplikace spuštěná v Azure](media/app-service-web-get-started-python/hello-world-in-browser.png)

**Blahopřejeme!** Nasadili jste svoji první aplikaci v Pythonu do služby App Service.

<a id="update-and-redeploy-the-code" class="xliff"></a>

## Aktualizace a opětovné nasazení kódu

Pomocí místního textového editoru otevřete soubor `main.py` v rámci aplikace v Pythonu a proveďte malou změnu textu vedle příkazu `return`:

```python
return 'Hello, Azure!'
```

Potvrďte změny v Gitu a potom odešlete změny kódu do Azure.

```bash
git commit -am "updated output"
git push azure master
```

Po dokončení nasazení se vraťte do okna prohlížeče, které se otevřelo v kroku [Přechod do aplikace](#browse-to-the-app), a aktualizujte zobrazení stránky.

![Aktualizovaná ukázková aplikace spuštěná v Azure](media/app-service-web-get-started-python/hello-azure-in-browser.png)

<a id="manage-your-new-azure-web-app" class="xliff"></a>

## Správa vaší nové webové aplikace Azure

Pokud chcete spravovat webovou aplikaci, kterou jste vytvořili, přejděte na web <a href="https://portal.azure.com" target="_blank">Azure Portal</a>.

V levé nabídce klikněte na **App Services** a pak klikněte na název vaší webové aplikace Azure.

![Navigace portálem k webové aplikaci Azure](./media/app-service-web-get-started-nodejs-poc/nodejs-docs-hello-world-app-service-list.png)

Zobrazí se stránka s přehledem vaší webové aplikace. Tady můžete provádět základní úlohy správy, jako je procházení, zastavení, spuštění, restartování a odstranění. 

![Okno App Service na webu Azure Portal](media/app-service-web-get-started-nodejs-poc/nodejs-docs-hello-world-app-service-detail.png)

Levá nabídka obsahuje odkazy na různé stránky pro konfiguraci vaší aplikace. 

[!INCLUDE [cli-samples-clean-up](../../includes/cli-samples-clean-up.md)]

<a id="next-steps" class="xliff"></a>

## Další kroky

> [!div class="nextstepaction"]
> [Python sh PostgreSQL](app-service-web-tutorial-docker-python-postgresql-app.md)

