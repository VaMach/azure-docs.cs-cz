---
title: "Nasazení webové aplikace Umbraco na webu Azure Portal v pěti minutách | Dokumentace Microsoftu"
description: "Nasazením ukázkové aplikace v ASP.NET zjistíte, jak snadné je spustit webové aplikace ve službě App Service. Výsledky si můžete okamžitě prohlédnout."
services: app-service\web
documentationcenter: 
author: cephalin
manager: erikre
editor: 
ms.assetid: b1e6bd58-48d1-4007-9d6c-53fd6db061e3
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 02/10/2017
ms.author: cephalin
translationtype: Human Translation
ms.sourcegitcommit: 6ea03adaabc1cd9e62aa91d4237481d8330704a1
ms.openlocfilehash: 6b1dede903083d1771733330a069b6ab533d9f00
ms.lasthandoff: 04/06/2017


---
# <a name="deploy-an-umbraco-web-app-in-the-azure-portal-in-five-minutes"></a>Nasazení webové aplikace Umbraco na webu Azure Portal v pěti minutách

Tento kurz vám pomůže nasadit webovou aplikaci [Umbraco](https://our.umbraco.org/) do služby [Azure App Service](../app-service/app-service-value-prop-what-is.md) během několika minut.

![Aplikace Umbraco](./media/app-service-web-get-started-dotnet-portal/defaultpage.png)

## <a name="prerequisites"></a>Požadavky
Potřebujete účet Microsoft Azure. Pokud nemáte účet, můžete se [zaregistrovat k bezplatné zkušební verzi](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) nebo si [aktivovat výhody předplatitele sady Visual Studio](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).

> [!NOTE]
> [App Service si můžete vyzkoušet](https://azure.microsoft.com/try/app-service/) bez účtu Azure. Můžete si vytvořit úvodní aplikaci a celou hodinu si s ní hrát, bez platebních karet a bez závazků.
> 
> 

## <a name="deploy-the-aspnet-app"></a>Nasazení aplikace ASP.NET
1. Přihlaste se k webu [Azure Portal](https://portal.azure.com).

2. Otevřete [https://portal.azure.com/#create/umbracoorg.UmbracoCMS](https://portal.azure.com/#create/umbracoorg.UmbracoCMS).

    Prostřednictvím tohoto odkazu se okamžitě nakonfiguruje nová aplikace Umbraco na webu Azure Portal.

3. Do pole **Název aplikace** zadejte název webové aplikace. Pokud je zadaný název v rámci domény `azurewebsites.net` jedinečný, zobrazí se v tomto poli zelené zaškrtnutí.
   
5. Ve skupině **Skupina prostředků** klikněte na **Vytvořit novou**. Vytvoří se nová [skupina prostředků](../azure-resource-manager/resource-group-overview.md). Pojmenujte ji.

7. Klikněte na **Plán nebo umístění služby App Service** > **Vytvořit nový**. [Plán služby App Service](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) nakonfigurujte podle následujícího postupu:

    - Do pole **Plán služby App Service** zadejte požadovaný název.
    - V poli **Umístění** zvolte umístění pro hostování vašeho plánu.
    - Klikněte na **Cenová úroveň** a potom vyberte **F1 Free** nebo jinou úroveň, která vám vyhovuje, a klikněte na **Vybrat**.
    - Klikněte na tlačítko **OK**.

    Vaše konfigurace CMS Umbraco by teď měla vypadat podobně jako na následujícím snímku obrazovky:

    ![Konfigurace probíhá – první Umbraco ve službě Azure App Service](./media/app-service-web-get-started-dotnet-portal/configure-in-progress.png)

12. Klikněte na **SQL Database** > **Vytvořit novou databázi**. SQL Database nakonfigurujte podle následujícího postupu:

    - Do pole **Název** zadejte název, například **myDB**.
    - Klikněte na **Cenová úroveň** a potom vyberte **F1 Free** nebo jinou úroveň, která vám vyhovuje, a klikněte na **Vybrat**.
    - Klikněte na **Cílový server** > **Vytvořit nový server**. Databázový server nakonfigurujte podle následujícího postupu:

        - Do pole **Název serveru** zadejte název serveru. Pokud je zadaný název v rámci domény `.database.windows.net` jedinečný, zobrazí se v tomto poli zelené zaškrtnutí.
        - Do pole **Přihlášení správce serveru** zadejte požadované uživatelské jméno správce.
        - Do polí **Heslo** a **Potvrzení hesla** zadejte požadované heslo.
        - V poli Umístění vyberte stejné umístění, které používáte pro webovou aplikaci.
        - Ujistěte se, že je vybraná možnost **Povolit službám Azure přístup k serveru**.
        - Klikněte na **Vybrat**.
    
    - Klikněte na **Vybrat**.

13. Klikněte na **Nastavení webové aplikace**, zadejte uživatelské jméno a heslo databáze a klikněte na **OK**.

    Vaše konfigurace CMS Umbraco by teď měla vypadat podobně jako na následujícím snímku obrazovky:

    ![Konfigurace dokončena – první Umbraco ve službě Azure App Service](./media/app-service-web-get-started-dotnet-portal/configure-complete.png)

14. Klikněte na možnost **Vytvořit**.
    
    Azure teď na základě vaší konfigurace vytvoří aplikaci Umbraco. Mělo by se zobrazit oznámení **Nasazení začalo...**.

    ![Nasazení bylo úspěšné – první Umbraco ve službě Azure App Service](./media/app-service-web-get-started-dotnet-portal/deployment-started.png)
   
## <a name="launch-and-manage-your-umrbaco-web-app"></a>Spuštění a správa webové aplikace Umbraco

Jakmile Azure dokončí nasazení aplikace, uvidíte další oznámení.

![Nasazení bylo úspěšné – první Umbraco ve službě Azure App Service](./media/app-service-web-get-started-dotnet-portal/deployment-succeeded.png)

1. Klikněte na toto oznámení. Pokud ho zmeškáte, můžete ho kdykoli zobrazit kliknutím na zvonek upozornění (![Oznámení níže – první Umbraco ve službě Azure App Service](./media/app-service-web-get-started-dotnet-portal/notification.png)).

    Teď byste měli vidět [okno](../azure-resource-manager/resource-group-portal.md#manage-resources) pro správu vaší webové aplikace (*okno*: stránka na portálu, která se otevírá vodorovně).

3. V horní části stránky Přehled klikněte na **Procházet**.
   
    ![Procházet – první Umbraco ve službě Azure App Service](./media/app-service-web-get-started-dotnet-portal/browse.png)

    Teď vidíte **úvodní** stránku Umbraca. Nakonfigurujte instalaci Umbraca a pohrajte si s ní!

    ![Konfigurace Umbraca – první Umbraco ve službě Azure App Service](./media/app-service-web-get-started-dotnet-portal/umbraco-config.png)
    
## <a name="next-steps"></a>Další kroky
* [Nasazení webové aplikace ASP.NET do služby Azure App Service pomocí sady Visual Studio](app-service-web-get-started-dotnet.md) – informace o vytvoření nové webové aplikace Azure v sadě Visual Studio pomocí některé z obsažených šablon
* [Nasazení kódu do služby Azure App Service](web-sites-deploy.md) – informace o nasazení z FTP nebo úložišť správy zdrojového kódu
* [Přidání funkce do první webové aplikace](app-service-web-get-started-2.md) – pozvednutí aplikace Azure na další úroveň Ověřte svoje uživatele. Škálujte ji na základě poptávky. Nastavte některá upozornění týkající se výkonu. To vše pomocí několika kliknutí.

