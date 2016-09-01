<properties
    pageTitle="Vytvoření webové aplikace v Azure Marketplace | Microsoft Azure"
    description="Naučte se vytvořit novou webovou aplikaci WordPress v Azure Marketplace pomocí portálu Azure."
    services="app-service\web"
    documentationCenter=""
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="07/11/2016"
    ms.author="robmcm"/>

<!-- Note: This article replaces web-sites-php-web-site-gallery.md -->

# Vytvoření webové aplikace v Azure Marketplace

[AZURE.INCLUDE [tabs](../../includes/app-service-web-get-started-nav-tabs.md)]

Azure Marketplace nabízí širokou škálu oblíbených webových aplikací vyvinutých společností Microsoft, jinými společnostmi a iniciativami v oblasti softwaru Open Source. Jedná se například o aplikace WordPress, Umbraco CMS, Drupal atd. Tyto webové aplikace jsou postaveny na široké škále oblíbených rozhraní, jako je [PHP] v tomto příkladu aplikace WordPress, [.NET], [Node.js], [Java], [Python] a další. K vytvoření webové aplikace v Azure Marketplace nepotřebujete žádný software, kromě prohlížeče, který používáte pro [Azure Portal].

V tomto kurzu se naučíte:

* Najděte a vytvořte webovou aplikaci ve službě Azure App Service založenou na šabloně Azure Marketplace.
* Nakonfigurujte nastavení služby Azure App Service pro novou webovou aplikaci.
* Spusťte a spravujte webovou aplikaci.

Pro účely tohoto kurzu nasadíte web s blogem WordPress z Azure Marketplace. Jakmile dokončíte postup popsaný v tomto kurzu, budete mít vlastní funkční web WordPress v cloudu.

![Příklad řídicího panelu webové aplikace WordPress][WordPressDashboard1]

Web WordPress, který v tomto kurzu nasadíte, využívá jako databázi MySQL. Chcete-li jako databázi raději použít SQL Database, informace naleznete v [Project Nami], který je rovněž k dispozici prostřednictvím Azure Marketplace.

> [AZURE.NOTE]
> K absolvování tohoto kurzu potřebujete účet Microsoft Azure. Pokud nemáte účet, můžete si [aktivovat výhody předplatitele Visual Studio][aktivovat] nebo se [zaregistrovat k bezplatné zkušební verzi][bezplatná zkušební verze].
>
> Chcete-li začít se službou Azure App Service dříve, než se zaregistrujete k účtu Azure, přejděte k možnosti [Vyzkoušet službu App Service]. Odtud můžete ihned vytvořit krátkodobou úvodní webovou aplikaci ve službě App Service – aniž by byla požadována platební karta a bez jakýchkoli závazků.

## Najděte a vytvořte webovou aplikaci ve službě Azure App Service

1. Přihlaste se k [Azure Portal].

1. Klikněte na možnost **Nové**.
    
    ![Vytvoření nového prostředku Azure][MarketplaceStart]
    
1. Vyhledejte **WordPress** a klikněte na možnost **WordPress**. (Chcete-li namísto MySQL použít SQL Database, vyhledejte **Project Nami**.)

    ![Vyhledání aplikace WordPress v Marketplace][MarketplaceSearch]
    
1. Přečtěte si popis aplikace WordPress a klikněte na možnost **Vytvořit**.

    ![Vytvoření webové aplikace WordPress][MarketplaceCreate]

## Nakonfigurujte nastavení služby Azure App Service pro novou webovou aplikaci

1. Po vytvoření nové webové aplikace se zobrazí okno nastavení aplikace WordPress, pomocí něhož provedete následující kroky:

    ![Konfigurace nastavení webové aplikace WordPress][ConfigStart]

1. Zadejte název webové aplikace do pole **Webová aplikace**.

    Tento název musí být v doméně azurewebsites.net jedinečný, protože webová aplikace bude mít adresu URL *{název}*.azurewebsites.net. Není-li zadaný název jedinečný, v textovém poli se zobrazí červený vykřičník.

    ![Konfigurace názvu webové aplikace WordPress][ConfigAppName]

1. Máte-li více předplatných, zvolte to, které chcete použít. 

    ![Konfigurace předplatného pro webovou aplikaci][ConfigSubscription]

1. Vyberte **skupinu prostředků** nebo vytvořte novou.

    Další informace o skupinách prostředků najdete v tématu [Přehled Azure Resource Manageru][SkupinyProstředků].

    ![Konfigurace skupiny prostředků pro webovou aplikaci][ConfigResourceGroup]

1. Vyberte **umístění/plán služby App Service** nebo vytvořte nové.

    Podrobnější informace o plánech služby App Service naleznete v tématu [Přehled plánů služby Azure App Service][AzureAppServicePlans]. 

    ![Konfigurace plánu služby pro webovou aplikaci][ConfigServicePlan]

1. Klikněte na možnost **Databáze** a poté v okně **Nová databáze MySQL** zadejte požadované hodnoty pro konfiguraci databáze MySQL.

    a. Zadejte nový název nebo ponechte výchozí název.

    b. Položku **Typ databáze** ponechte nastavenou na možnost **Shared**.

    c. Zvolte totéž umístění, které jste zvolili pro webovou aplikaci.

    d. Zvolte cenovou úroveň. **Mercury** – bezplatná úroveň s minimálním počtem připojení a místem na disku – je vhodná pro tento kurz.

    e. V okně **Nová databáze MySQL** přijměte právní podmínky a klikněte na tlačítko **OK**. 

    ![Konfigurace nastavení databáze pro webovou aplikaci][ConfigDatabase]

1. V okně **WordPress** přijměte právní podmínky a klikněte na možnost **Vytvořit**. 

    ![Dokončete nastavení webové aplikace a klikněte na tlačítko OK][ConfigFinished]

    Služba Azure App Service vytvoří webovou aplikaci, což obvykle trvá méně než minutu. Průběh můžete sledovat kliknutím na ikonu zvonu v horní části stránky portálu.

    ![Ukazatel průběhu][ConfigProgress]

## Spuštění a správa webové aplikace WordPress
    
1. Až budete s vytvářením webové aplikace hotovi, přejděte na portálu Azure do skupiny prostředků, v níž jste aplikaci vytvořili, a zde se zobrazí webová aplikace a databáze.

    Dodatečný prostředek s ikonou žárovky je služba [Application Insights][ApplicationInsights], která pro vaši webovou aplikaci zajišťuje služby monitorování.

1. V okně **Skupina prostředků** klikněte na řádek webové aplikace.

    ![Výběr webové aplikace WordPress][WordPressSelect]

1. V okně webové aplikace klikněte na tlačítko **Procházet**.

    ![Navigace k webové aplikaci WordPress][WordPressBrowse]

1. Pokud budete vyzváni k výběru jazyka blogu WordPress, vyberte požadovaný jazyk a klikněte na možnost **Pokračovat**.

    ![Konfigurace jazyka webové aplikace WordPress][WordPressLanguage]

1. Na **úvodní** stránce WordPress zadejte informace o konfiguraci vyžadované aplikací WordPress a klikněte na možnost **Instalovat WordPress**.

    ![Konfigurace nastavení webové aplikace WordPress][WordPressConfigure]

1. Přihlaste se pomocí přihlašovacích údajů, které jste vytvořili na **úvodní** stránce.  

1. Otevře se stránka řídicího panelu webu, na níž se zobrazí vámi zadané informace.    

    ![Zobrazení řídicího panelu WordPress][WordPressDashboard2]

## Další kroky

V tomto kurzu jste se seznámili s postupem vytvoření a nasazení ukázkové webové aplikace z Azure Marketplace.

Další informace týkající se práce s webovými aplikacemi App Service Web Apps naleznete prostřednictvím odkazů v levé části stránky (máte-li široké okno prohlížeče) nebo v horní části stránky (máte-li úzké okno prohlížeče).

Další informace o vývoji webových aplikací WordPress v Azure naleznete v části [Vývoj aplikací WordPress ve službě Azure App Service][WordPressVAzure]. 

<!-- URL List -->

[PHP]: https://azure.microsoft.com/develop/php/
[.NET]: https://azure.microsoft.com/develop/net/
[Node.js]: https://azure.microsoft.com/develop/nodejs/
[Java]: https://azure.microsoft.com/develop/java/
[Python]: https://azure.microsoft.com/develop/python/
[aktivovat]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[bezplatná zkušební verze]: https://azure.microsoft.com/pricing/free-trial/
[Vyzkoušet službu App Service]: http://go.microsoft.com/fwlink/?LinkId=523751
[SkupinyProstředků]: ../resource-group-overview.md
[AzureAppServicePlans]: ../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md
[ApplicationInsights]: https://azure.microsoft.com/services/application-insights/
[Azure Portal]: https://portal.azure.com/
[Project Nami]: http://projectnami.org/
[WordPressVAzure]: ./develop-wordpress-on-app-service-web-apps.md

<!-- IMG List -->

[MarketplaceStart]: ./media/app-service-web-create-web-app-from-marketplace/marketplacestart.png
[MarketplaceSearch]: ./media/app-service-web-create-web-app-from-marketplace/marketplacesearch.png
[MarketplaceCreate]: ./media/app-service-web-create-web-app-from-marketplace/marketplacecreate.png
[ConfigStart]: ./media/app-service-web-create-web-app-from-marketplace/configstart.png
[ConfigAppName]: ./media/app-service-web-create-web-app-from-marketplace/configappname.png
[ConfigSubscription]: ./media/app-service-web-create-web-app-from-marketplace/configsubscription.png
[ConfigResourceGroup]: ./media/app-service-web-create-web-app-from-marketplace/configresourcegroup.png
[ConfigServicePlan]: ./media/app-service-web-create-web-app-from-marketplace/configserviceplan.png
[ConfigDatabase]: ./media/app-service-web-create-web-app-from-marketplace/configdatabase.png
[ConfigFinished]: ./media/app-service-web-create-web-app-from-marketplace/configfinished.png
[ConfigProgress]: ./media/app-service-web-create-web-app-from-marketplace/configprogress.png
[WordPressSelect]: ./media/app-service-web-create-web-app-from-marketplace/wpselect.png
[WordPressBrowse]: ./media/app-service-web-create-web-app-from-marketplace/wpbrowse.png
[WordPressLanguage]: ./media/app-service-web-create-web-app-from-marketplace/wplanguage.png
[WordPressDashboard1]: ./media/app-service-web-create-web-app-from-marketplace/wpdashboard1.png
[WordPressDashboard2]: ./media/app-service-web-create-web-app-from-marketplace/wpdashboard2.png
[WordPressConfigure]: ./media/app-service-web-create-web-app-from-marketplace/wpconfigure.png



<!---HONumber=Aug16_HO4-->


