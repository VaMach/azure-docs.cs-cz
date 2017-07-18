---
title: "Vytvoření webové aplikace PHP ve službě Azure | Dokumentace Microsoftu"
description: "Během několika minut můžete nasadit svou první aplikaci PHP Hello World pomocí služby Azure App Service Web Apps."
services: app-service\web
documentationcenter: 
author: syntaxc4
manager: erikre
editor: 
ms.assetid: 6feac128-c728-4491-8b79-962da9a40788
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 05/04/2017
ms.author: cfowler
ms.custom: mvc
ms.translationtype: HT
ms.sourcegitcommit: d941879aee6042b38b7f5569cd4e31cb78b4ad33
ms.openlocfilehash: 9a53216e326e63bd4fe36c0e5d5d5e85b2098d8c
ms.contentlocale: cs-cz
ms.lasthandoff: 07/10/2017

---
# <a name="create-a-php-web-app-in-azure"></a>Vytvoření webové aplikace v PHP v Azure

[Azure Web Apps](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview) je vysoce škálovatelná služba s automatickými opravami pro hostování webů.  V tomto kurzu Rychlý start se dozvíte, jak nasadit aplikaci PHP pomocí služby Azure Web Apps. Vytvoříte webovou aplikaci pomocí rozhraní příkazového řádku [Azure CLI](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli) a pomocí Gitu nasadíte ukázkový kód PHP do webové aplikace.

Ukázková aplikace spuštěná ve službě Azure (media/app-service-web-get-started-php/hello-world-in-browser.png)

Následující postup můžete použít v případě počítačů Mac, Windows nebo Linux. Pokud máte nainstalované všechny požadované prostředky, zabere vám tento postup zhruba pět minut.

## <a name="prerequisites"></a>Požadavky

K provedení kroků v tomto kurzu Rychlý start je potřeba:

* [Nainstalovat Git](https://git-scm.com/).
* [Nainstalovat PHP](https://php.net).

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Pokud se rozhodnete nainstalovat a používat rozhraní příkazového řádku (CLI) místně, musíte mít spuštěnou verzi Azure CLI 2.0 nebo novější. Verzi zjistíte spuštěním příkazu `az --version`. Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI 2.0]( /cli/azure/install-azure-cli). 

## <a name="download-the-sample"></a>Stažení ukázky

V okně terminálu naklonujte spuštěním následujícího příkazu úložiště ukázkové aplikace do místního počítače.

```bash
git clone https://github.com/Azure-Samples/php-docs-hello-world
```

Toto okno terminálu budete používat ke spuštění všech příkazů v tomto kurzu Rychlý start.

Přejděte do adresáře, který obsahuje vzorový kód.

```bash
cd php-docs-hello-world
```

## <a name="run-the-app-locally"></a>Místní spuštění aplikace

Aplikaci spustíte místně tak, že otevřete okno terminálu a pomocí příkazu `php` spustíte integrovaný webový server PHP.

```bash
php -S localhost:8080
```

Otevřete webový prohlížeč a přejděte na ukázkovou aplikaci na adrese http://localhost:8080.

Na stránce se zobrazí zpráva **Hello World!** z ukázkové aplikace.

![Ukázková aplikace spuštěná místně](media/app-service-web-get-started-php/localhost-hello-world-in-browser.png)

V okně terminálu ukončete webový server stisknutím **Ctrl + C**.

[!INCLUDE [Log in to Azure](../../includes/login-to-azure.md)] 

[!INCLUDE [Configure deployment user](../../includes/configure-deployment-user.md)] 

[!INCLUDE [Create resource group](../../includes/app-service-web-create-resource-group.md)] 

[!INCLUDE [Create app service plan](../../includes/app-service-web-create-app-service-plan.md)] 

[!INCLUDE [Create web app](../../includes/app-service-web-create-web-app.md)] 

![Prázdná stránka webové aplikace](media/app-service-web-get-started-php/app-service-web-service-created.png)

Nyní jste v Azure vytvořili novou prázdnou webovou aplikaci.

[!INCLUDE [Configure local git](../../includes/app-service-web-configure-local-git.md)] 

[!INCLUDE [Push to Azure](../../includes/app-service-web-git-push-to-azure.md)] 

```bash
Counting objects: 2, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (2/2), done.
Writing objects: 100% (2/2), 352 bytes | 0 bytes/s, done.
Total 2 (delta 1), reused 0 (delta 0)
remote: Updating branch 'master'.
remote: Updating submodules.
remote: Preparing deployment for commit id '25f18051e9'.
remote: Generating deployment script.
remote: Running deployment command...
remote: Handling Basic Web Site deployment.
remote: Kudu sync from: '/home/site/repository' to: '/home/site/wwwroot'
remote: Copying file: '.gitignore'
remote: Copying file: 'LICENSE'
remote: Copying file: 'README.md'
remote: Copying file: 'index.php'
remote: Ignoring: .git
remote: Finished successfully.
remote: Running post deployment command(s)...
remote: Deployment successful.
To https://<app_name>.scm.azurewebsites.net/<app_name>.git
   cc39b1e..25f1805  master -> master
```

## <a name="browse-to-the-app"></a>Přechod do aplikace

V prohlížeči zadejte adresu nasazené aplikace.

```bash
http://<app_name>.azurewebsites.net
```

Vzorový kód PHP je spuštěný ve webové aplikaci služby Azure App Service.

![Ukázková aplikace spuštěná ve službě Azure](media/app-service-web-get-started-php/hello-world-in-browser.png)

**Blahopřejeme!** Nasadili jste svoji první aplikaci v PHP do služby App Service.

## <a name="update-and-redeploy-the-code"></a>Aktualizace a opětovné nasazení kódu

Pomocí místního textového editoru otevřete soubor `index.php`, který je součástí PHP aplikace, a proveďte malou změnu textu v řetězci vedle `echo`:

```php
echo "Hello Azure!";
```

Potvrďte změny v Gitu a potom odešlete změny kódu do Azure.

```bash
git commit -am "updated output"
git push azure master
```

Po dokončení nasazení se vraťte do okna prohlížeče, které se otevřelo v kroku **Přechod do aplikace**, a aktualizujte zobrazení stránky.

![Aktualizovaná ukázková aplikace spuštěná ve službě Azure](media/app-service-web-get-started-php/hello-azure-in-browser.png)

## <a name="manage-your-new-azure-web-app"></a>Správa vaší nové webové aplikace Azure

Pokud chcete spravovat webovou aplikaci, kterou jste vytvořili, přejděte na web <a href="https://portal.azure.com" target="_blank">Azure Portal</a>.

V levé nabídce klikněte na **App Services** a pak klikněte na název vaší webové aplikace Azure.

![Navigace portálem k webové aplikaci Azure](./media/app-service-web-get-started-php/php-docs-hello-world-app-service-list.png)

Zobrazí se stránka s přehledem vaší webové aplikace. Tady můžete provádět základní úlohy správy, jako je procházení, zastavení, spuštění, restartování a odstranění. 

![Okno App Service na webu Azure Portal](media/app-service-web-get-started-php/php-docs-hello-world-app-service-detail.png)

Levá nabídka obsahuje odkazy na různé stránky pro konfiguraci vaší aplikace. 

[!INCLUDE [cli-samples-clean-up](../../includes/cli-samples-clean-up.md)]

## <a name="next-steps"></a>Další kroky

> [!div class="nextstepaction"]
> [PHP s databází MySQL](app-service-web-tutorial-php-mysql.md)

