---
title: "Nasazení aplikace WordPress na webu Azure Portal v pěti minutách | Dokumentace Microsoftu"
description: "Nasazením aplikace WordPress zjistíte, jak snadné je spustit webové aplikace ve službě App Service. Výsledky si můžete okamžitě prohlédnout."
services: app-service\web
documentationcenter: 
author: cephalin
manager: erikre
editor: 
ms.assetid: 6feac128-c728-4491-8b79-962da9a40788
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 02/13/2017
ms.author: cephalin
translationtype: Human Translation
ms.sourcegitcommit: 0921b01bc930f633f39aba07b7899ad60bd6a234
ms.openlocfilehash: 7ef5802866bf96859d3f44cdb58cbb412b08a700
ms.lasthandoff: 03/01/2017


---
# <a name="deploy-a-wordpress-app-in-the-azure-portal-in-five-minutes"></a>Nasazení aplikace WordPress na webu Azure Portal v pěti minutách

V tomto kurzu se dozvíte, jak nasadit první aplikaci [WordPress](https://wordpress.org/) do služby [Azure App Service](../app-service/app-service-value-prop-what-is.md) během několika minut.

![Web WordPress](./media/app-service-web-get-started-php-portal/wpdashboard.png)

## <a name="prerequisites"></a>Požadavky
Potřebujete účet Microsoft Azure. Pokud nemáte účet, můžete se [zaregistrovat k bezplatné zkušební verzi](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) nebo si [aktivovat výhody předplatitele sady Visual Studio](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).

> [!NOTE]
> [App Service si můžete vyzkoušet](https://azure.microsoft.com/try/app-service/) bez účtu Azure. Můžete si vytvořit úvodní aplikaci a celou hodinu si s ní hrát, bez platebních karet a bez závazků.
> 
> 

## <a name="deploy-the-wordpress-app"></a>Nasazení aplikace WordPress
1. Přihlaste se k webu [Azure Portal](https://portal.azure.com).

2. Otevřete [https://portal.azure.com/#create/WordPress.WordPress](https://portal.azure.com/#create/WordPress.WordPress).

    Prostřednictvím tohoto odkazu se okamžitě nakonfiguruje nová aplikace WordPress na webu Azure Portal.

3. Do pole **Název aplikace** zadejte název webové aplikace. Pokud je zadaný název v rámci domény `azurewebsites.net` jedinečný, zobrazí se v tomto poli zelené zaškrtnutí.
   
5. Ve skupině **Skupina prostředků** klikněte na **Vytvořit novou**. Vytvoří se nová [skupina prostředků](../azure-resource-manager/resource-group-overview.md). Pojmenujte ji.

6. V části **Poskytovatel databáze** vyberte **CleaDB**.

7. Klikněte na **Plán nebo umístění služby App Service** > **Vytvořit nový**. [Plán služby App Service](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) nakonfigurujte podle následujícího postupu:

    - Do pole **Plán služby App Service** zadejte požadovaný název.
    - V poli **Umístění** zvolte umístění pro hostování vašeho plánu.
    - Klikněte na **Cenová úroveň** a potom vyberte **F1 Free** nebo jinou úroveň, která vám vyhovuje, a klikněte na **Vybrat**.
    - Klikněte na tlačítko **OK**.

8. Klikněte na **Databáze** > **Vytvořit novou**. SQL Database nakonfigurujte podle následujícího postupu:

    - Do pole **Název databáze** zadejte požadovaný název. 
    - V poli **Umístění** zvolte stejné umístění jako pro plán služby App Service.
    - Klikněte na **Cenová úroveň** a potom vyberte **Mercury** nebo jinou úroveň, která vám vyhovuje, a klikněte na **Vybrat**.
    - Klikněte na **Právní podmínky** a klikněte na **Koupit**.
    - Klikněte na tlačítko **OK**.

9. Klikněte na možnost **Vytvořit**.

    Azure teď na základě vaší konfigurace vytvoří aplikaci WordPress. Mělo by se zobrazit oznámení **Nasazení začalo...**.

    ![Nasazení začalo – první WordPress ve službě Azure App Service](./media/app-service-web-get-started-php-portal/deployment-started.png)
   
## <a name="launch-and-manage-your-wordpress-web-app"></a>Spuštění a správa webové aplikace WordPress

Jakmile Azure dokončí nasazení aplikace, uvidíte další oznámení.

![Nasazení bylo úspěšné – první WordPress ve službě Azure App Service](./media/app-service-web-get-started-php-portal/deployment-succeeded.png)

1. Klikněte na toto oznámení. Pokud ho zmeškáte, můžete ho kdykoli zobrazit kliknutím na zvonek upozornění (![Oznámení níže – první WordPress ve službě Azure App Service](./media/app-service-web-get-started-dotnet-portal/notification.png)).

    Teď byste měli vidět [okno](../azure-resource-manager/resource-group-portal.md#manage-resources) pro správu vaší webové aplikace (*okno*: stránka na portálu, která se otevírá vodorovně).

3. V horní části stránky Přehled klikněte na **Procházet**.
   
    ![Procházet – první WordPress ve službě Azure App Service](./media/app-service-web-get-started-php-portal/browse.png)

    Teď vidíte **úvodní** stránku WordPressu. Nakonfigurujte instalaci WordPressu a pohrajte si s ní!

    ![Konfigurace WordPressu – první WordPress ve službě Azure App Service](./media/app-service-web-get-started-php-portal/wordpress-config.png)
    
## <a name="next-steps"></a>Další kroky
* [Vytvořte, nakonfigurujte a nasaďte webovou aplikaci Laravel do Azure](app-service-web-php-get-started.md). Naučte se základní dovednosti pro spuštění libovolné webové aplikace v PHP v prostředí Azure:

    * Vytvoření a konfigurace aplikací v Azure z prostředí PowerShell/Bash
    * Nastavení verze PHP
    * Použití spouštěcího souboru, který není v kořenovém adresáři aplikace
    * Povolení automatizace nástroje Composer
    * Přístup k proměnným pro konkrétní prostředí
    * Odstraňování běžných chyb

* [Nasazení kódu do služby Azure App Service](web-sites-deploy.md) – informace o nasazení z FTP nebo úložišť správy zdrojového kódu
* [Přidání funkce do první webové aplikace](app-service-web-get-started-2.md) – pozvednutí aplikace Azure na další úroveň Ověřte svoje uživatele. Škálujte ji na základě poptávky. Nastavte některá upozornění týkající se výkonu. To vše pomocí několika kliknutí.

