---
title: "Vytvoření webové aplikace WordPress ve službě Azure App Service | Dokumentace Microsoftu"
description: "Naučte se vytvořit novou webovou aplikaci Azure pro blog WordPress pomocí webu Azure Portal."
services: app-service\web
documentationcenter: php
author: rmcmurray
manager: erikre
editor: 
ms.assetid: 193ae094-0d7c-4749-a09b-ff4b1240149e
ms.service: app-service-web
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: PHP
ms.topic: hero-article
ms.date: 12/22/2016
ms.author: robmcm
translationtype: Human Translation
ms.sourcegitcommit: b1a633a86bd1b5997d5cbf66b16ec351f1043901
ms.openlocfilehash: 83ef925a8268873d09c4a1611a1f2341acd92ba7


---
# <a name="create-a-wordpress-web-app-in-azure-app-service"></a>Vytvoření webové aplikace WordPress ve službě Azure App Service
[!INCLUDE [tabs](../../includes/app-service-web-get-started-nav-tabs.md)]

V tomto kurzu se dozvíte, jak nasadit web blogu WordPress z Azure Marketplace.

Po dokončení kurzu budete mít vlastní funkční web blogu WordPress spuštěný v cloudu.

![Web WordPress](./media/web-sites-php-web-site-gallery/wpdashboard.png)

Co se dozvíte:

* Postup nalezení šablony aplikace v Azure Marketplace.
* Postup vytvoření webové aplikace založené na šabloně ve službě Azure App Service.
* Postup konfigurace nastavení služby Azure App Service pro novou webovou aplikaci a databázi.

Azure Marketplace nabízí širokou škálu oblíbených webových aplikací vyvinutých společností Microsoft, jinými společnostmi a iniciativami v oblasti softwaru Open Source. Webové aplikace jsou postaveny na široké škále oblíbených rozhraní, jako je [PHP](/develop/nodejs/) v tomto příkladu aplikace WordPress, [.NET](/develop/net/), [Node.js](/develop/nodejs/), [Java](/develop/java/), [Python](/develop/python/) a další. K vytvoření webové aplikace v Azure Marketplace nepotřebujete žádný software, kromě prohlížeče, který používáte pro [web Azure Portal](https://portal.azure.com/). 

Web WordPress, který nasazujete v tomto kurzu, využívá jako databázi MySQL. Chcete-li jako databázi raději použít SQL Database, informace naleznete v [Projektu Nami](http://projectnami.org/). **Projekt Nami** je rovněž k dispozici prostřednictvím Azure Marketplace.

> [!NOTE]
> K absolvování tohoto kurzu potřebujete účet Microsoft Azure. Pokud nemáte účet, můžete si [aktivovat výhody předplatitele sady Visual Studio](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F) nebo se [zaregistrovat k bezplatné zkušební verzi](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).
> 
> Chcete-li začít se službou Azure App Service dříve, než se zaregistrujete k účtu Azure, přejděte k možnosti [Vyzkoušet službu App Service](https://azure.microsoft.com/try/app-service/). Zde můžete ihned vytvořit krátkodobou úvodní webovou aplikaci ve službě App Service – aniž by byla požadována platební karta a bez jakýchkoli závazků.
> 
> 

## <a name="select-wordpress-and-configure-for-azure-app-service"></a>Výběr aplikace WordPress a konfigurace pro službu Azure App Service
1. Přihlaste se k [webu Azure Portal](https://portal.azure.com/).
2. Klikněte na možnost **Nové**.
   
    ![Vytvořit nové][5]
3. Vyhledejte **WordPress** a klikněte na možnost **WordPress**. Chcete-li namísto MySQL použít SQL Database, vyhledejte **Projekt Nami**.
   
    ![WordPress ze seznamu][7]
4. Přečtěte si popis aplikace WordPress a klikněte na možnost **Vytvořit**.
   
    ![Vytvořit](./media/web-sites-php-web-site-gallery/create.png)
5. Zadejte název webové aplikace do pole **Webová aplikace**.
   
    Tento název musí být v doméně azurewebsites.net jedinečný, protože webová aplikace bude mít adresu URL {název}.azurewebsites.net. Není-li zadaný název jedinečný, v textovém poli se zobrazí červený vykřičník.
6. Máte-li více předplatných, zvolte to, které chcete použít. 
7. Vyberte **skupinu prostředků** nebo vytvořte novou.
   
    Další informace o skupinách prostředků najdete v tématu [Přehled Azure Resource Manageru](../azure-resource-manager/resource-group-overview.md).
8. Vyberte **umístění/plán služby App Service** nebo vytvořte nové.
   
    Podrobnější informace o plánech služby App Service naleznete v tématu [Přehled plánů služby Azure App Service](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).    
9. Klikněte na možnost **Databáze** a poté v okně **Nová databáze MySQL** zadejte požadované hodnoty pro konfiguraci databáze MySQL.
   
    a. Zadejte nový název nebo ponechte výchozí název.
   
    b. Položku **Typ databáze** ponechte nastavenou na možnost **Shared**.
   
    c. Zvolte totéž umístění, které jste zvolili pro webovou aplikaci.
   
    d. Zvolte cenovou úroveň. Mercury (bezplatná úroveň s minimálním počtem povolených připojení a místem na disku) je vhodná pro tento kurz.
10. V okně **Nová databáze MySQL** klikněte na tlačítko **OK**. 
11. V okně **WordPress** přijměte právní podmínky a klikněte na možnost **Vytvořit**. 
    
     ![Konfigurace webové aplikace](./media/web-sites-php-web-site-gallery/configure.png)
    
     Služba Azure App Service vytvoří webovou aplikaci, což obvykle trvá méně než minutu. Průběh můžete sledovat kliknutím na ikonu zvonu v horní části stránky portálu.
    
     ![Ukazatel průběhu](./media/web-sites-php-web-site-gallery/progress.png)

## <a name="launch-and-manage-your-wordpress-web-app"></a>Spuštění a správa webové aplikace WordPress
1. Až budete s vytvářením webové aplikace hotovi, přejděte na webu Azure Portal do skupiny prostředků, v níž jste aplikaci vytvořili, a zde se zobrazí webová aplikace a databáze.
   
    Dodatečný prostředek s ikonou žárovky je služba [Application Insights](/services/application-insights/), která pro vaši webovou aplikaci zajišťuje služby monitorování.
2. V okně **Skupina prostředků** klikněte na řádek webové aplikace.
   
    ![Konfigurace webové aplikace](./media/web-sites-php-web-site-gallery/resourcegroup.png)
3. V okně webové aplikace klikněte na tlačítko **Procházet**.
   
    ![adresa URL webu][browse]
4. Na **úvodní** stránce WordPress zadejte informace o konfiguraci vyžadované aplikací WordPress a klikněte na možnost **Instalovat WordPress**.
   
    ![Konfigurace WordPress](./media/web-sites-php-web-site-gallery/wpconfigure.png)
5. Přihlaste se pomocí přihlašovacích údajů, které jste vytvořili na **úvodní** stránce.  
6. Otevře se stránka řídicího panelu webu.    
   
    ![Web WordPress](./media/web-sites-php-web-site-gallery/wpdashboard.png)

## <a name="next-steps"></a>Další kroky
Seznámili jste se s postupem vytvoření a nasazení webové aplikace PHP z galerie. Další informace o použití PHP v Azure naleznete ve [Středisku pro vývojáře PHP](/develop/php/).

Další informace týkající se práce s webovými aplikacemi App Service Web Apps naleznete prostřednictvím odkazů v levé části stránky (máte-li široké okno prohlížeče) nebo v horní části stránky (máte-li úzké okno prohlížeče). 

## <a name="whats-changed"></a>Co se změnilo
* Průvodce změnou z webů na službu App Service naleznete v tématu [Služba Azure App Service a její vliv na stávající služby Azure](http://go.microsoft.com/fwlink/?LinkId=529714).

[5]: ./media/web-sites-php-web-site-gallery/startmarketplace.png
[7]: ./media/web-sites-php-web-site-gallery/search-web-app.png
[browse]: ./media/web-sites-php-web-site-gallery/browse-web.png



<!--HONumber=Jan17_HO3-->


