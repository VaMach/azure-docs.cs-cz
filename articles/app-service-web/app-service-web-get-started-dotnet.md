---
title: "Vytvoření webové aplikace ASP.NET ve službě Azure | Dokumentace Microsoftu"
description: "Nasazením ukázkové webové aplikace ASP.NET se naučíte, jak spouštět webové aplikace ve službě Azure App Service."
services: app-service\web
documentationcenter: 
author: cephalin
manager: wpickett
editor: 
ms.assetid: b1e6bd58-48d1-4007-9d6c-53fd6db061e3
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 06/14/2017
ms.author: cephalin
ms.custom: mvc
ms.translationtype: Human Translation
ms.sourcegitcommit: 7948c99b7b60d77a927743c7869d74147634ddbf
ms.openlocfilehash: 2b447bcc5930550af3996cb40925ab59d203dc7c
ms.contentlocale: cs-cz
ms.lasthandoff: 06/20/2017

---
# Vytvoření webové aplikace ASP.NET v Azure
<a id="create-an-aspnet-web-app-in-azure" class="xliff"></a>

[Azure Web Apps](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview) je vysoce škálovatelná služba s automatickými opravami pro hostování webů.  V tomto kurzu Rychlý start se dozvíte, jak nasadit svoji první webovou aplikaci ASP.NET pomocí služby Azure Web Apps. Po dokončení kurzu budete mít skupinu prostředků, která se bude skládat z plánu služby App Service a webové aplikace Azure s nasazenou webovou aplikací.

![Webová aplikace ASP.NET ve službě Azure App Service](./media/app-service-web-get-started-dotnet/updated-azure-web-app.png)

## Požadavky
<a id="prerequisites" class="xliff"></a>

K provedení kroků v tomto kurzu je potřeba:

* Nainstalovat [Visual Studio 2017](https://www.visualstudio.com/en-us/visual-studio-homepage-vs.aspx) s následujícími sadami funkcí:
    - **Vývoj pro ASP.NET a web**
    - **Azure – vývoj**

    ![Vývoj pro ASP.NET a Azure – vývoj (v části Web a cloud)](media/app-service-web-tutorial-dotnet-sqldatabase/workloads.png)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## Vytvoření webové aplikace ASP.NET
<a id="create-an-aspnet-web-app" class="xliff"></a>

Ve Visual Studiu vytvořte projekt tak, že vyberete **Soubor > Nový > Projekt**. 

V dialogovém okně **Nový projekt** vyberte **Visual C# > Web > Webová aplikace ASP.NET (.NET Framework)**.

Aplikaci pojmenujte _myFirstAzureWebApp_ a pak vyberte **OK**.
   
![Dialogové okno Nový projekt](./media/app-service-web-get-started-dotnet/new-project.png)

Do Azure můžete nasadit jakýkoli typ webové aplikace ASP.NET. V tomto kurzu Rychlý start vyberte šablonu **MVC** a ujistěte se, že u ověřování je nastavena možnost **Bez ověření**.
      
Vyberte **OK**.

![Dialogové okno Nový projekt ASP.NET](./media/app-service-web-get-started-dotnet/select-mvc-template.png)

V nabídce vyberte **Ladit > Spustit bez ladění** a spusťte tak webovou aplikaci místně.

![Místní spuštění aplikace](./media/app-service-web-get-started-dotnet/local-web-app.png)

## Publikování aplikací do Azure
<a id="publish-to-azure" class="xliff"></a>

V **Průzkumníku řešení** klikněte pravým tlačítkem na projekt **myFirstAzureWebApp** a vyberte možnost **Publikovat**.

![Publikování z Průzkumníka řešení](./media/app-service-web-get-started-dotnet/solution-explorer-publish.png)

Zkontrolujte, že je vybrána možnost **Microsoft Azure App Service** a vyberte **Publikovat**.

![Publikování ze stránky přehledu projektu](./media/app-service-web-get-started-dotnet/publish-to-app-service.png)

Tím se otevře dialogové okno **Vytvořit plán Aplikační služby**, které vám pomůže vytvořit všechny prostředky služby Azure potřebné ke spuštění webové aplikace ASP.NET ve službě Azure.

## Přihlášení k Azure
<a id="sign-in-to-azure" class="xliff"></a>

V dialogovém okně **Vytvořit plán Aplikační služby** vyberte **Přidat účet** a přihlaste se ke svému předplatnému Azure. Pokud jste již přihlášení, ujistěte se, že v účtu je vaše předplatné Azure. Můžete vybrat účet, se kterým jste přihlášení, a přidat správný účet.

> [!NOTE]
> Pokud už jste přihlášení, nevybírejte zatím možnost **Vytvořit**.
>
>
   
![Přihlášení k Azure](./media/app-service-web-get-started-dotnet/sign-in-azure.png)

Jakmile budete přihlášení, můžete v tomto dialogovém okně vytvořit všechny potřebné prostředky pro webovou aplikaci Azure.

## Vytvoření skupiny prostředků
<a id="create-a-resource-group" class="xliff"></a>

[!INCLUDE [resource group intro text](../../includes/resource-group.md)]

Vedle pole **Skupina prostředků** vyberte **Nová**.

Skupinu prostředků pojmenujte **myResourceGroup** a vyberte **OK**.

## Vytvoření plánu služby App Service
<a id="create-an-app-service-plan" class="xliff"></a>

[!INCLUDE [app-service-plan](../../includes/app-service-plan.md)]

Vedle pole **Plán služby App Service** vyberte **Nový**. 

V dialogovém okně **Konfigurovat plán Aplikační služby** použijte nastavení podle tabulky následující po snímku obrazovky.

![Vytvoření plánu služby App Service](./media/app-service-web-get-started-dotnet/configure-app-service-plan.png)

| Nastavení | Navrhovaná hodnota | Popis |
|-|-|-|
|Plán služby App Service| myAppServicePlan | Název plánu služby App Service. |
| Umístění | Západní Evropa | Datacentrum, které je hostitelem webové aplikace. |
| Velikost | Free | [Cenová úroveň](https://azure.microsoft.com/pricing/details/app-service/) určuje funkce hostování. |

Vyberte **OK**.

## Vytvoření a publikování webové aplikace
<a id="create-and-publish-the-web-app" class="xliff"></a>

Do pole **Název webové aplikace** zadejte jedinečný název aplikace (platné znaky jsou `a-z`, `0-9` a `-`). Adresa URL webové aplikace je `http://<app_name>.azurewebsites.net`, kde `<app_name>` je název vaší webové aplikace. 

Můžete přijmout automaticky vygenerovaný název, který je jedinečný.

Výběrem možnosti **Vytvořit** spustíte vytváření prostředků Azure.

![Nastavení názvu webové aplikace](./media/app-service-web-get-started-dotnet/web-app-name.png)

Průvodce po dokončení publikuje webovou aplikaci ASP.NET do služby Azure a pak aplikaci spustí ve výchozím prohlížeči.

![Publikovaná webová aplikace ASP.NET v Azure](./media/app-service-web-get-started-dotnet/published-azure-web-app.png)

Adresa URL používá dříve zadaný název webové aplikace ve tvaru `http://<app_name>.azurewebsites.net`. 

Blahopřejeme, vaše webová aplikace ASP.NET je veřejně spuštěná ve službě Azure App Service.

## Aktualizace a opětovné nasazení aplikace
<a id="update-the-app-and-redeploy" class="xliff"></a>

Z **Průzkumníku řešení** otevřete _Views\Home\Index.cshtml_.

Najděte HTML značku `<div class="jumbotron">` poblíž začátku a nahraďte celý element následujícím kódem:

```HTML
<div class="jumbotron">
    <h1>ASP.NET in Azure!</h1>
    <p class="lead">This is a simple app that we’ve built that demonstrates how to deploy a .NET app to Azure App Service.</p>
</div>
```

Opětovné nasazení do služby Azure provedete tak, že v **Průzkumníku řešení** kliknete pravým tlačítkem na projekt **myFirstAzureWebApp** a vyberete **Publikovat**.

Na stránce publikování vyberte **Publikovat**.

Po dokončení publikování spustí Visual Studio prohlížeč na adrese URL webové aplikace.

![Aktualizovaná webová aplikace ASP.NET v Azure](./media/app-service-web-get-started-dotnet/updated-azure-web-app.png)

## Správa webové aplikace Azure
<a id="manage-the-azure-web-app" class="xliff"></a>

Pokud chcete webovou aplikaci spravovat, přejděte na web <a href="https://portal.azure.com" target="_blank">Azure Portal</a>.

V levé nabídce klikněte na **App Services** a potom vyberte název své webové aplikace Azure.

![Navigace portálem k webové aplikaci Azure](./media/app-service-web-get-started-dotnet/access-portal.png)

Zobrazí se stránka s přehledem vaší webové aplikace. Tady můžete provádět základní úlohy správy, jako je procházení, zastavení, spuštění, restartování a odstranění. 

![Okno App Service na webu Azure Portal](./media/app-service-web-get-started-dotnet/web-app-blade.png)

Levá nabídka obsahuje odkazy na různé stránky pro konfiguraci vaší aplikace. 

[!INCLUDE [Clean-up section](../../includes/clean-up-section-portal.md)]

## Další kroky
<a id="next-steps" class="xliff"></a>

> [!div class="nextstepaction"]
> [ASP.NET s databází SQL Database](app-service-web-tutorial-dotnet-sqldatabase.md)

