---
title: "Vytvoření první webové aplikace v ASP.NET v Azure během pěti minut | Dokumentace Microsoftu"
description: "Přečtěte si, jak snadné je spouštět webové aplikace ve službě App Service, na příkladu nasazení jednoduché aplikace v ASP.NET."
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
ms.date: 05/05/2017
ms.author: cephalin
ms.translationtype: Human Translation
ms.sourcegitcommit: 2db2ba16c06f49fd851581a1088df21f5a87a911
ms.openlocfilehash: 773d616b2a5815158971d63d005fae605450ac98
ms.contentlocale: cs-cz
ms.lasthandoff: 05/08/2017


---
# <a name="create-your-first-aspnet-web-app-in-azure-in-five-minutes"></a>Vytvoření první webové aplikace v ASP.NET v Azure během pěti minut

[!INCLUDE [app-service-web-selector-get-started](../../includes/app-service-web-selector-get-started.md)] 

Tento Rychlý start vám pomůže nasadit první webovou aplikaci v ASP.NET do služby [Azure App Service](../app-service/app-service-value-prop-what-is.md) během pár minut. Až budete hotovi, budete mít jednoduchou webovou aplikaci spuštěnou v cloudu.

![Webová aplikace ASP.NET ve službě Azure App Service](./media/app-service-web-get-started-dotnet/updated-azure-web-app.png)

## <a name="prerequisites"></a>Požadavky

Tento kurz ukazuje, jak používat Visual Studio 2017 k sestavení a nasazení webové aplikace ASP.NET do Azure. Pokud ještě nemáte nainstalovanou sadu Visual Studio 2017, můžete stáhnout a použít **bezplatnou verzi** [Visual Studio 2017 Community Edition](https://www.visualstudio.com/downloads/). Nezapomeňte při instalaci sady Visual Studio povolit možnost **Azure Development**.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-an-aspnet-web-app"></a>Vytvoření webové aplikace v ASP.NET

V sadě Visual Studio vytvořte projekt pomocí `Ctrl`+`Shift`+`N`.

V dialogovém okně **Nový projekt** klikněte na položku **Visual C# > Web > ASP.NET Web Application (.NET Framework)**.

Pojmenujte aplikaci **myFirstAzureWebApp** a klikněte na **OK**.
   
![Dialogové okno Nový projekt](./media/app-service-web-get-started-dotnet/new-project.png)

Do Azure můžete nasadit jakýkoli typ webové aplikace ASP.NET. V tomto kurzu vyberte šablonu **MVC** a nastavte ověřování na **Bez ověřování**.
      
Klikněte na tlačítko **OK**.

![Dialogové okno Nový projekt ASP.NET](./media/app-service-web-get-started-dotnet/select-mvc-template.png)

## <a name="publish-to-azure"></a>Publikování aplikací do Azure

V **Průzkumníku řešení** klikněte pravým tlačítkem na projekt **myFirstAzureWebApp** a vyberte možnost **Publikovat**.

![Publikování z Průzkumníka řešení](./media/app-service-web-get-started-dotnet/solution-explorer-publish.png)

Zkontrolujte, že je vybraná možnost **Microsoft Azure App Service** a klikněte na **Publikovat**.

![Publikování ze stránky přehledu projektu](./media/app-service-web-get-started-dotnet/publish-to-app-service.png)

Tím se otevře dialogové okno **Vytvoření služby App Service** které vám pomůže vytvořit všechny prostředky Azure potřebné ke spuštění vaší webové aplikace ASP.NET v Azure.

## <a name="sign-in-to-azure"></a>Přihlášení k Azure

V dialogovém okně **Vytvoření služby App Service** klikněte na **Přidat účet** a přihlaste se ke svému předplatnému Azure. Pokud jste již přihlášení k účtu Microsoft, ujistěte se, že odpovídá vašemu předplatnému Azure. Pokud jste přihlášeni k účtu Microsoft, který nemá přiřazené předplatné Azure, kliknutím na něj přidejte správný účet.
   
![Přihlášení k Azure](./media/app-service-web-get-started-dotnet/sign-in-azure.png)

Až se přihlásíte, můžete v tomto okně vytvořit všechny prostředky, které potřebujete pro vaši webovou aplikaci Azure.

## <a name="create-a-resource-group"></a>Vytvoření skupiny prostředků

Nejdříve potřebujete _skupinu prostředků_. 

> [!NOTE] 
> Skupina prostředků je logický kontejner, ve kterém se nasazují a spravují prostředky Azure, jako například webové aplikace, databáze a účty úložiště.
>
>

Vedle pole **Skupina prostředků** klikněte na tlačítko **Nová**.

Pojmenujte novou skupinu prostředků **myResourceGroup** a klikněte na **OK**.

## <a name="create-an-app-service-plan"></a>Vytvoření plánu služby App Service

Vaše nová webová aplikace Azure také potřebuje _plán služby App Service_. 

> [!NOTE]
> Plán služby App Service představuje kolekci fyzických prostředků použitých k hostování vašich aplikací. Všechny aplikace přiřazené k plánu služby App Service sdílí službou definované prostředky a tak umožňují snížení nákladů při hostování více aplikací. 
>
> Plány služby App Service definují:
>
> - Oblast (Severní Evropa, Východní USA, Jihovýchodní Asie)
> - Velikost instance (Malá, Střední, Velká)
> - Počet škálování (jedna, dvě nebo tři instance atd.) 
> - SKU (Free, Shared, Basic, Standard, Premium)
>
>

Vedle pole **Plán služby App Service** klikněte na **Nový**. 

V dialogovém okně **Konfigurace plánu služby App Service** nastavte nový plán takto:

- **App Service Plan:** Zadejte **myAppServicePlan**. 
- **Umístění:** Zvolte **Západní Evropa** nebo jakoukoli jinou oblast, která vám vyhovuje.
- **Velikost:** Zvolte **Free** nebo kteroukoli jinou [cenovou úroveň](https://azure.microsoft.com/pricing/details/app-service/).

Klikněte na **OK**.

![Vytvoření plánu služby App Service](./media/app-service-web-get-started-dotnet/configure-app-service-plan.png)

## <a name="create-and-publish-the-web-app"></a>Vytvoření a publikování webové aplikace

Teď už zbývá jen vaši webovou aplikaci pojmenovat. Do pole **Název webové aplikace** zadejte jedinečný název. Tento název se použije jako výchozí DNS název pro webovou aplikaci (`<app_name>.azurewebsites.net`), proto musí být mezi všemi aplikacemi v Azure jedinečný. Později můžete na webovou aplikaci namapovat jakékoli další záznamy DNS, než ji zpřístupníte uživatelům.

Můžete také přijmout automaticky vygenerovaný název, který už je jedinečný.

Kliknutím na **Vytvořit** zahájíte vytváření prostředků Azure.

![Nastavení názvu webové aplikace](./media/app-service-web-get-started-dotnet/web-app-name.png)

Až průvodce dokončí vytváření prostředků Azure, automaticky poprvé publikuje vaši webovou aplikaci ASP.NET ve službě Azure a spustí publikovanou webovou aplikaci Azure ve vašem výchozím prohlížeči.

![Publikovaná webová aplikace ASP.NET v Azure](./media/app-service-web-get-started-dotnet/published-azure-web-app.png)

Adresa URL používá dříve zadaný název webové aplikace ve tvaru `http://<app_name>.azurewebsites.net`. 

Blahopřejeme, vaše první webová aplikace v ASP.NET běží živě v Azure App Service.

## <a name="update-the-app-and-redeploy"></a>Aktualizace a opětovné nasazení aplikace

Aktualizace a opětovné nasazení aplikace v Azure jsou velmi snadné. Pojďme provést malou úpravu domovské stránky.

Z **Průzkumníku řešení** otevřete **Views\Home\Index.cshtml**.

Najděte HTML značku `<div class="jumbotron">` poblíž začátku a celou ji nahraďte následujícím kódem:

```HTML
<div class="jumbotron">
    <h1>ASP.NET in Azure!</h1>
    <p class="lead">This is a simple app that we’ve built that demonstrates how to deploy a .NET app to Azure App Service.</p>
</div>
```

Opětovné nasazení do Azure provedete tak, že v **Průzkumníku řešení** kliknete pravým tlačítkem na projekt **myFirstAzureWebApp** a vyberete **Publikovat**.

Na stránce publikování klikněte na **Publikovat**.

Až Visual Studio práci dokončí, otevře aktualizovanou webovou aplikaci Azure ve vašem prohlížeči.

![Aktualizovaná webová aplikace ASP.NET v Azure](./media/app-service-web-get-started-dotnet/updated-azure-web-app.png)

## <a name="manage-your-new-azure-web-app"></a>Správa vaší nové webové aplikace Azure

Přejděte na web Azure Portal a podívejte se na webovou aplikaci, kterou jste právě vytvořili. 

Chcete-li to provést, přihlaste se na adrese [https://portal.azure.com](https://portal.azure.com).

V levé nabídce klikněte na **App Services** a pak klikněte na název vaší webové aplikace Azure.

![Navigace portálem k webové aplikaci Azure](./media/app-service-web-get-started-dotnet/access-portal.png)

Dostali jste se do _okna_ vaší webové aplikace (stránka portálu, která se otvírá vodorovně). 

Ve výchozím nastavení bude okno vaší webové aplikace obsahovat stránku **Přehled**. Tato stránka poskytuje přehled, jak si vaše aplikace stojí. Tady můžete také provést základní úlohy správy, jako je procházení, zastavení, spuštění, restartování a odstranění.  

![Okno App Service na webu Azure Portal](./media/app-service-web-get-started-dotnet/web-app-blade.png)

Karty na levé straně okna zobrazují další stránky konfigurace, které můžete otevřít. Následující seznam obsahuje jen několik možností:

- Mapování vlastního názvu DNS
- Vazba vlastního certifikátu SSL
- Konfigurace průběžného nasazování
- Vertikální i horizontální navýšení kapacity
- Přidání ověřování uživatelů

## <a name="clean-up-resources"></a>Vyčištění prostředků

Pokud chcete svoji webovou aplikaci Azure odstranit, klikněte na volbu **Odstranit** na stránce **Přehled**. Existuje ale lepší způsob, jak odstranit vše, co jste v tomto kurzu vytvořili. Na stránce **Přehled** vaší webové aplikace klikněte na skupinu prostředků, otevře se její okno. 

![Skupina prostředků v okně App Service](./media/app-service-web-get-started-dotnet/access-resource-group.png)

V okně skupiny prostředků uvidíte jak plán služby App Service, tak i aplikaci služby App Service, kterou pro vás sada Visual Studio vytvořila. 

V horní části okna klikněte na **Odstranit**. 

<!--![Delete resource group in Azure portal](./media/app-service-web-get-started-dotnet/delete-resource-group.png)-->

V okně potvrzení zadejte pro kontrolu název skupiny prostředků **myResourceGroup** do textového pole a klikněte na **Odstranit**.

## <a name="next-steps"></a>Další kroky

> [!div class="nextstepaction"]
> [Zjistěte, jak v Azure vytvořit aplikaci ASP.NET se službou SQL Database](app-service-web-tutorial-dotnet-sqldatabase.md)

> [!div class="nextstepaction"]
> [Prozkoumejte ukázkové skripty PowerShellu pro Web Apps](app-service-powershell-samples.md)

