---
title: "Vytvoření webové aplikace .NET Core a nasazení do služby App Service v systému Linux | Microsoft Docs"
description: "Nasazení první aplikace .NET Core Hello, World do služby App Service v systému Linux v minutách."
keywords: "služby Azure app service, webové aplikace, dotnet, core, linux, operačních systémů"
services: app-service
documentationCenter: 
author: cephalin
manager: syntaxc4
editor: 
ms.assetid: c02959e6-7220-496a-a417-9b2147638e2e
ms.service: app-service
ms.workload: web
ms.tgt_pltfrm: linux
ms.devlang: na
ms.topic: quickstart
ms.date: 08/30/2017
ms.author: cfowler
ms.custom: mvc
ms.openlocfilehash: c01b55e9548f9b2e2c7aca161afda42c2c248379
ms.sourcegitcommit: 62eaa376437687de4ef2e325ac3d7e195d158f9f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/22/2017
---
# <a name="create-a-net-core-web-app-in-app-service-on-linux"></a>Vytvoření webové aplikace .NET Core ve službě App Service v systému Linux

[Aplikační služby v systému Linux](app-service-linux-intro.md) nabízí vysoce škálovatelnou a automatických oprav webové hostitelské služby pomocí operační systém Linux. Tento rychlý start ukazuje, jak vytvořit [.NET Core](https://docs.microsoft.com/aspnet/core/) aplikace v App Service v systému Linux. Vytvořit webovou aplikaci pomocí [rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli), a pomocí Git nasaďte kód .NET Core do webové aplikace.

![Ukázková aplikace spuštěná ve službě Azure](media/quickstart-dotnetcore/dotnet-browse-azure.png)

Následující postup můžete použít v případě počítačů Mac, Windows nebo Linux.

## <a name="prerequisites"></a>Požadavky

K provedení kroků v tomto kurzu Rychlý start je potřeba:

* <a href="https://git-scm.com/" target="_blank">Nainstalovat Git</a>.
* <a href="https://www.microsoft.com/net/download/core" target="_blank">Nainstalujte základní rozhraní .NET SDK</a>

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="create-the-app-locally"></a>Vytvoření aplikace místně

V okně terminálu na váš počítač, vytvořte adresář s názvem `hellodotnetcore` a změně aktuálního adresáře na ni.

```bash
md hellodotnetcore
cd hellodotnetcore
```

Vytvořte novou webovou aplikaci .NET Core.

```bash
dotnet new web
```

## <a name="run-the-app-locally"></a>Místní spuštění aplikace

Obnovení balíčků NuGet a spusťte aplikaci.

```bash
dotnet restore
dotnet run
```

Otevřete webový prohlížeč a přejděte na aplikaci ve `http://localhost:5000`.

Na stránce se zobrazí zpráva **Hello World** z ukázkové aplikace.

![Testování pomocí prohlížeče](media/quickstart-dotnetcore/dotnet-browse-local.png)

V okně terminálu ukončete webový server stisknutím **Ctrl + C**. Inicializace úložiště Git pro projekt .NET Core.

```bash
git init
git add .
git commit -m "first commit"
```

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

[!INCLUDE [Configure deployment user](../../../includes/configure-deployment-user.md)]

[!INCLUDE [Create resource group](../../../includes/app-service-web-create-resource-group.md)]

[!INCLUDE [Create app service plan](../../../includes/app-service-web-create-app-service-plan-linux.md)]

## <a name="create-a-web-app"></a>Vytvoření webové aplikace

[!INCLUDE [Create web app](../../../includes/app-service-web-create-web-app-dotnetcore-no-h.md)]

Vyhledejte nově vytvořenou webovou aplikaci. Nahraďte  _&lt;název aplikace >_ s názvem vaší webové aplikace.

```bash
http://<app name>.azurewebsites.net
```

![Prázdná stránka webové aplikace](media/quickstart-dotnetcore/dotnet-browse-created.png)

[!INCLUDE [Push to Azure](../../../includes/app-service-web-git-push-to-azure.md)] 

```bash
Counting objects: 22, done.
Delta compression using up to 8 threads.
Compressing objects: 100% (18/18), done.
Writing objects: 100% (22/22), 51.21 KiB | 3.94 MiB/s, done.
Total 22 (delta 1), reused 0 (delta 0)
remote: Updating branch 'master'.
remote: Updating submodules.
remote: Preparing deployment for commit id '741f16d1db'.
remote: Generating deployment script.
remote: Project file path: ./hellodotnetcore.csproj
remote: Generated deployment script files
remote: Running deployment command...
remote: Handling ASP.NET Core Web Application deployment.
remote: ...............................................................................................
remote:   Restoring packages for /home/site/repository/hellodotnetcore.csproj...
remote: ....................................
remote:   Installing System.Xml.XPath 4.0.1.
remote:   Installing System.Diagnostics.Tracing 4.1.0.
remote:   Installing System.Threading.Tasks.Extensions 4.0.0.
remote:   Installing System.Reflection.Emit.ILGeneration 4.0.1.
remote:   ...
remote: Finished successfully.
remote: Running post deployment command(s)...
remote: Deployment successful.
To https://cephalin-dotnetcore.scm.azurewebsites.net/cephalin-dotnetcore.git
 * [new branch]      master -> master
```

## <a name="browse-to-the-app"></a>Přechod do aplikace

V prohlížeči zadejte adresu nasazené aplikace.

```bash
http://<app_name>.azurewebsites.net
```

Ukázkový kód Node.js běží ve webové aplikaci s integrovanou bitové kopie.

![Ukázková aplikace spuštěná ve službě Azure](media/quickstart-dotnetcore/dotnet-browse-azure.png)

**Blahopřejeme!** První aplikaci Node.js jste nasadili do služby App Service v systému Linux.

## <a name="update-and-redeploy-the-code"></a>Aktualizace a opětovné nasazení kódu

V místním adresáři, otevřete _Startup.cs_ souboru. Změňte text ve volání metody, které malá `context.Response.WriteAsync`:

```csharp
await context.Response.WriteAsync("Hello Azure!");
```

Potvrďte změny v Gitu a potom odešlete změny kódu do Azure.

```bash
git commit -am "updated output"
git push azure master
```

Po dokončení nasazení se vraťte do okna prohlížeče, které se otevřelo v kroku **Přechod do aplikace**, a stiskněte tlačítko Aktualizovat.

![Aktualizovaná ukázková aplikace spuštěná ve službě Azure](media/quickstart-dotnetcore/dotnet-browse-azure-updated.png)

## <a name="manage-your-new-azure-web-app"></a>Správa vaší nové webové aplikace Azure

Pokud chcete spravovat webovou aplikaci, kterou jste vytvořili, přejděte na web <a href="https://portal.azure.com" target="_blank">Azure Portal</a>.

V levé nabídce klikněte na **App Services** a pak klikněte na název vaší webové aplikace Azure.

![Navigace portálem k webové aplikaci Azure](./media/quickstart-dotnetcore/portal-app-service-list.png)

Zobrazí se stránka s přehledem vaší webové aplikace. Tady můžete provádět základní úlohy správy, jako je procházení, zastavení, spuštění, restartování a odstranění. 

![Stránka služby App Service na webu Azure Portal](media/quickstart-dotnetcore/portal-app-overview.png)

Levá nabídka obsahuje odkazy na různé stránky pro konfiguraci vaší aplikace. 

[!INCLUDE [cli-samples-clean-up](../../../includes/cli-samples-clean-up.md)]

## <a name="next-steps"></a>Další kroky

> [!div class="nextstepaction"]
> [Vytvoření webové aplikace .NET Core a databáze SQL ve službě Azure App Service v systému Linux](tutorial-dotnetcore-sqldb-app.md)
