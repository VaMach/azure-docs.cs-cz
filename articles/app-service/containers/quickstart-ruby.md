---
title: "Vytvoření aplikace Ruby a nasazení do služby App Service v systému Linux | Microsoft Docs"
description: "Naučte se vytvářet aplikace pro poznámky Ruby službou App Service v systému Linux."
keywords: "služby Azure app service, linux, operačních systémů, ruby"
services: app-service
documentationcenter: 
author: SyntaxC4
manager: cfowler
editor: 
ms.assetid: 6d00c73c-13cb-446f-8926-923db4101afa
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 10/10/2017
ms.author: cfowler
ms.custom: mvc
ms.openlocfilehash: a54ef1ae40ba6ea9ad604a29c67e41228c0d5946
ms.sourcegitcommit: 62eaa376437687de4ef2e325ac3d7e195d158f9f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/22/2017
---
# <a name="create-a-ruby-app-in-app-service-on-linux"></a>Vytvoření Ruby aplikace ve službě App Service v systému Linux

[Aplikační služby v systému Linux](app-service-linux-intro.md) nabízí vysoce škálovatelnou a automatických oprav webové hostitelské služby. Tento rychlý start se dozvíte, jak vytvořit základní Ruby na které aplikace můžete poté ji nasadit do Azure jako webové aplikace v systému Linux.

![Hello world](./media/quickstart-ruby/hello-world-updated.png)

## <a name="prerequisites"></a>Požadavky

* <a href="https://www.ruby-lang.org/en/documentation/installation/#rubyinstaller" target="_blank">Nainstalujte Ruby 2.4.1 nebo vyšší</a>
* <a href="https://git-scm.com/" target="_blank">Nainstalovat Git</a>.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="download-the-sample"></a>Stažení ukázky

V okně terminálu spusťte následující příkaz, který klonovat úložiště ukázkové aplikace do místního počítače:

```bash
git clone https://github.com/Azure-Samples/ruby-docs-hello-world
```

## <a name="run-the-application-locally"></a>Místní spuštění aplikace

Spusťte server které v pořadí pro aplikace pro práci. Změnit na *hello, world* adresář a `rails server` příkaz spustí serveru.

```bash
cd hello-world\bin
rails server
```

Pomocí webového prohlížeče, přejděte do `http://localhost:3000` k testování aplikace místně.

![Hello world](./media/quickstart-ruby/hello-world.png)

## <a name="modify-app-to-display-welcome-message"></a>Upravit aplikaci k zobrazení uvítací zprávy

Upravte aplikaci proto zobrazí uvítací zprávy. Nejdřív musíte nastavit trasu změnou *~/workspace/ruby-docs-hello-world/config/routes.rb* souboru s názvem trasu `hello`.

  ```ruby
  Rails.application.routes.draw do
      #For details on the DSL available within this file, see http://guides.rubyonrails.org/routing.html
      root 'application#hello'
  end
  ```

Změňte řadič aplikace tak, aby vracel zprávy ve formátu HTML v prohlížeči. 

Otevřete *~/workspace/hello-world/app/controllers/application_controller.rb* pro úpravy. Upravit `ApplicationController` třídy vypadat jako následující ukázka kódu:

  ```ruby
  class ApplicationController > ActionController :: base
    protect_from_forgery with: :exception
    def hello
      render html: "Hello, world from Azure Web App on Linux!"
    end
  end
  ```

Aplikace je nyní nakonfigurována. Pomocí webového prohlížeče, přejděte do `http://localhost:3000` potvrďte kořenové cílová stránka.

![Hello World nakonfigurované](./media/quickstart-ruby/hello-world-configured.png)

[!INCLUDE [Try Cloud Shell](../../../includes/cloud-shell-try-it.md)]

[!INCLUDE [Configure deployment user](../../../includes/configure-deployment-user.md)]

## <a name="create-a-ruby-web-app-on-azure"></a>Vytvoření Ruby webové aplikace v Azure

Skupina prostředků musí obsahovat prostředky potřebné pro vaši webovou aplikaci. Chcete-li vytvořit skupinu prostředků, použijte [vytvořit skupinu az]() příkaz.

```azurecli-interactive
az group create --location westeurope --name myResourceGroup
```

Použití [vytvořit plán aplikační služby az](https://docs.microsoft.com/cli/azure/appservice/plan#az_appservice_plan_create) příkazu vytvoříte plán služby app service pro webové aplikace.

```azurecli-interactive
az appservice plan create --name myAppServicePlan --resource-group myResourceGroup --is-linux
```

V dalším kroku vydávat [az webapp vytvořit](https://docs.microsoft.com/cli/azure/webapp) příkazu vytvořte webovou aplikaci, která používá nově vytvořený tarifu. Všimněte si, že modul runtime je nastaven na `ruby|2.3`. Nezapomeňte nahradit `<app name>` s jedinečným názvem aplikace.

```azurecli-interactive
az webapp create --resource-group myResourceGroup --plan myAppServicePlan --name <app name> \
--runtime "ruby|2.3" --deployment-local-git
```

Výstup z tohoto příkazu zjistí informace o nově vytvořenou webovou aplikaci, jakož i adresa URL nasazení. By měla vypadat podobně jako v následujícím příkladu. V tomto kurzu, zkopírujte adresu URL pro pozdější použití.

```bash
https://<deployment user name>@<app name>.scm.azurewebsites.net/<app name>.git
```

Po vytvoření webové aplikace **přehled** stránka je k dispozici k zobrazení. Přejděte do ní. Zobrazí se následující úvodní stránka:

![Úvodní stránka](./media/quickstart-ruby/splash-page.png)


## <a name="deploy-your-application"></a>Nasazení aplikace

Spusťte následující příkazy k nasazení místní aplikace do webové stránky Azure:

```bash
git remote add azure <Git deployment URL from above>
git add -A
git commit -m "Initial deployment commit"
git push azure master
```

Potvrďte, že operace vzdálené nasazení sestav úspěch. Příkazy vytvořit výstup podobný následujícímu:

```bash
remote: Using sass-rails 5.0.6
remote: Updating files in vendor/cache
remote: Bundle gems are installed into ./vendor/bundle
remote: Updating files in vendor/cache
remote: ~site/repository
remote: Finished successfully.
remote: Running post deployment command(s)...
remote: Deployment successful.
To https://<your web app name>.scm.azurewebsites.net/<your web app name>.git
  579ccb....2ca5f31  master -> master
myuser@ubuntu1234:~workspace/<app name>$
```

Po dokončení nasazení restartování webové aplikace pro nasazení se projeví pomocí [az webapp restartování](https://docs.microsoft.com/cli/azure/webapp#az_webapp_restart) příkaz, jak je vidět tady:

```azurecli-interactive
az webapp restart --name <app name> --resource-group myResourceGroup
```

Přejít na váš web a ověřte výsledky.

```bash
http://<app name>.azurewebsites.net
```

![aktualizované webové aplikace](./media/quickstart-ruby/hello-world-updated.png)

> [!NOTE]
> Během restartování aplikace pokusu o vyhledání lokality výsledky v stavový kód HTTP `Error 503 Server unavailable`. Ho může trvat několik minut plně restartovat.
>

[!INCLUDE [Clean-up section](../../../includes/cli-script-clean-up.md)]

## <a name="next-steps"></a>Další kroky

[V systému Linux nejčastější dotazy týkající se služby Azure App Service](https://docs.microsoft.com/azure/app-service-web/app-service-linux-faq.md)
