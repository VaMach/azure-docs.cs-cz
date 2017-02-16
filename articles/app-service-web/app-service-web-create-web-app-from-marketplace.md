---
title: "Vytvoření webové aplikace v Azure Marketplace | Dokumentace Microsoftu"
description: "Naučte se vytvořit novou webovou aplikaci WordPress v Azure Marketplace pomocí portálu Azure."
services: app-service\web
documentationcenter: 
author: rmcmurray
manager: wpickett
editor: 
ms.assetid: 972a296d-f927-470b-8534-0f2cb9eac223
ms.service: app-service-web
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 12/22/2016
ms.author: robmcm
translationtype: Human Translation
ms.sourcegitcommit: b1a633a86bd1b5997d5cbf66b16ec351f1043901
ms.openlocfilehash: a04c7129cd2e16c129f3e4b8e8e40f76ff37114d


---
# <a name="create-a-web-app-from-the-azure-marketplace"></a>Vytvoření webové aplikace v Azure Marketplace
<!-- Note: This article replaces web-sites-php-web-site-gallery.md -->

[!INCLUDE [tabs](../../includes/app-service-web-get-started-nav-tabs.md)]

Azure Marketplace nabízí širokou škálu oblíbených webových aplikací vyvinutých společností Microsoft, jinými společnostmi a iniciativami v oblasti softwaru Open Source. Jedná se například o aplikace WordPress, Umbraco CMS, Drupal atd. Tyto webové aplikace jsou postaveny na široké škále oblíbených rozhraní, jako je [PHP] v tomto příkladu aplikace WordPress, [.NET], [Node.js], [Java], [Python] a další. K vytvoření webové aplikace v Azure Marketplace nepotřebujete žádný software, kromě prohlížeče, který používáte pro [Azure Portal].

V tomto kurzu se naučíte:

* Najděte a vytvořte webovou aplikaci ve službě Azure App Service založenou na šabloně Azure Marketplace.
* Nakonfigurujte nastavení služby Azure App Service pro novou webovou aplikaci.
* Spusťte a spravujte webovou aplikaci.

Pro účely tohoto kurzu nasadíte web s blogem WordPress z Azure Marketplace. Jakmile dokončíte postup popsaný v tomto kurzu, budete mít vlastní funkční web WordPress v cloudu.

![Příklad řídicího panelu webové aplikace WordPress][WordPressDashboard1]

Web WordPress, který v tomto kurzu nasadíte, využívá jako databázi MySQL. Chcete-li jako databázi raději použít SQL Database, informace naleznete v [Project Nami], který je rovněž k dispozici prostřednictvím Azure Marketplace.

> [!NOTE]
> K absolvování tohoto kurzu potřebujete účet Microsoft Azure. Pokud nemáte účet, můžete si [aktivovat výhody předplatitele sady Visual Studio][activate] nebo se [zaregistrovat k bezplatné zkušební verzi][free trial].
> 
> Chcete-li začít se službou Azure App Service dříve, než se zaregistrujete k účtu Azure, přejděte k možnosti [Vyzkoušet službu App Service]. Odtud můžete ihned vytvořit krátkodobou úvodní webovou aplikaci ve službě App Service – aniž by byla požadována platební karta a bez jakýchkoli závazků.
> 
> 

## <a name="find-and-create-a-web-app-in-azure-app-service"></a>Najděte a vytvořte webovou aplikaci ve službě Azure App Service
1. Přihlaste se k [Azure Portal].
2. Klikněte na možnost **Nové**.
   
    ![Vytvoření nového prostředku Azure][MarketplaceStart]
3. Vyhledejte **WordPress** a klikněte na možnost **WordPress**. (Chcete-li namísto MySQL použít SQL Database, vyhledejte **Project Nami**.)
   
    ![Vyhledání aplikace WordPress v Marketplace][MarketplaceSearch]
4. Přečtěte si popis aplikace WordPress a klikněte na možnost **Vytvořit**.
   
    ![Vytvoření webové aplikace WordPress][MarketplaceCreate]

## <a name="configure-azure-app-service-settings-for-your-new-web-app"></a>Nakonfigurujte nastavení služby Azure App Service pro novou webovou aplikaci
1. Po vytvoření nové webové aplikace se zobrazí okno nastavení aplikace WordPress, pomocí něhož provedete následující kroky:
   
    ![Konfigurace nastavení webové aplikace WordPress][ConfigStart]
2. Zadejte název webové aplikace do pole **Webová aplikace**.
   
    Tento název musí být v doméně azurewebsites.net jedinečný, protože webová aplikace bude mít adresu URL *{název}*.azurewebsites.net. Není-li zadaný název jedinečný, v textovém poli se zobrazí červený vykřičník.
   
    ![Konfigurace názvu webové aplikace WordPress][ConfigAppName]
3. Máte-li více předplatných, zvolte to, které chcete použít.
   
    ![Konfigurace předplatného pro webovou aplikaci][ConfigSubscription]
4. Vyberte **skupinu prostředků** nebo vytvořte novou.
   
    Další informace o skupinách prostředků najdete v tématu [Přehled Azure Resource Manageru][ResourceGroups].
   
    ![Konfigurace skupiny prostředků pro webovou aplikaci][ConfigResourceGroup]
5. Vyberte **umístění/plán služby App Service** nebo vytvořte nové.
   
    Podrobnější informace o plánech služby App Service najdete v tématu [Přehled plánů služby Azure App Service][AzureAppServicePlans].
   
    ![Konfigurace plánu služby pro webovou aplikaci][ConfigServicePlan]
6. Klikněte na možnost **Databáze** a poté v okně **Nová databáze MySQL** zadejte požadované hodnoty pro konfiguraci databáze MySQL.
   
    a. Zadejte nový název nebo ponechte výchozí název.
   
    b. Položku **Typ databáze** ponechte nastavenou na možnost **Shared**.
   
    c. Zvolte totéž umístění, které jste zvolili pro webovou aplikaci.
   
    d. Zvolte cenovou úroveň. **Mercury** – bezplatná úroveň s minimálním počtem připojení a místem na disku – je vhodná pro tento kurz.
   
    e. V okně **Nová databáze MySQL** přijměte právní podmínky a klikněte na tlačítko **OK**.
   
    ![Konfigurace nastavení databáze pro webovou aplikaci][ConfigDatabase]
7. V okně **WordPress** přijměte právní podmínky a klikněte na možnost **Vytvořit**.
   
    ![Dokončete nastavení webové aplikace a klikněte na tlačítko OK][ConfigFinished]
   
    Služba Azure App Service vytvoří webovou aplikaci, což obvykle trvá méně než minutu. Průběh můžete sledovat kliknutím na ikonu zvonu v horní části stránky portálu.
   
    ![Ukazatel průběhu][ConfigProgress]

## <a name="launch-and-manage-your-wordpress-web-app"></a>Spuštění a správa webové aplikace WordPress
1. Až budete s vytvářením webové aplikace hotovi, přejděte na portálu Azure do skupiny prostředků, v níž jste aplikaci vytvořili, a zde se zobrazí webová aplikace a databáze.
   
    Dodatečný prostředek s ikonou žárovky je služba [Application Insights][ApplicationInsights], která pro vaši webovou aplikaci zajišťuje služby monitorování.
2. V okně **Skupina prostředků** klikněte na řádek webové aplikace.
   
    ![Výběr webové aplikace WordPress][WordPressSelect]
3. V okně webové aplikace klikněte na tlačítko **Procházet**.
   
    ![Navigace k webové aplikaci WordPress][WordPressBrowse]
4. Pokud budete vyzváni k výběru jazyka blogu WordPress, vyberte požadovaný jazyk a klikněte na možnost **Pokračovat**.
   
    ![Konfigurace jazyka webové aplikace WordPress][WordPressLanguage]
5. Na **úvodní** stránce WordPress zadejte informace o konfiguraci vyžadované aplikací WordPress a klikněte na možnost **Instalovat WordPress**.
   
    ![Konfigurace nastavení webové aplikace WordPress][WordPressConfigure]
6. Přihlaste se pomocí přihlašovacích údajů, které jste vytvořili na **úvodní** stránce.  
7. Otevře se stránka řídicího panelu webu, na níž se zobrazí vámi zadané informace.    
   
    ![Zobrazení řídicího panelu WordPress][WordPressDashboard2]

## <a name="next-steps"></a>Další kroky
V tomto kurzu jste se seznámili s postupem vytvoření a nasazení ukázkové webové aplikace z Azure Marketplace.

Další informace týkající se práce s webovými aplikacemi App Service Web Apps naleznete prostřednictvím odkazů v levé části stránky (máte-li široké okno prohlížeče) nebo v horní části stránky (máte-li úzké okno prohlížeče).

Další informace o vývoji webových aplikací WordPress v Azure najdete v tématu [Vývoj aplikací WordPress ve službě Azure App Service][WordPressOnAzure].

<!-- URL List -->

[PHP]: https://azure.microsoft.com/develop/php/
[.NET]: https://azure.microsoft.com/develop/net/
[Node.js]: https://azure.microsoft.com/develop/nodejs/
[Java]: https://azure.microsoft.com/develop/java/
[Python]: https://azure.microsoft.com/develop/python/
[activate]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[free trial]: https://azure.microsoft.com/pricing/free-trial/
[Vyzkoušet službu App Service]: https://azure.microsoft.com/try/app-service/
[ResourceGroups]: ../azure-resource-manager/resource-group-overview.md
[AzureAppServicePlans]: ../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md
[ApplicationInsights]: https://azure.microsoft.com/services/application-insights/
[Azure Portal]: https://portal.azure.com/
[Project Nami]: http://projectnami.org/
[WordPressOnAzure]: ./develop-wordpress-on-app-service-web-apps.md

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



<!--HONumber=Feb17_HO3-->


