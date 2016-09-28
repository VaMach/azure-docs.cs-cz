<properties
    pageTitle="Vytvoření webové aplikace WordPress ve službě Azure App Service | Microsoft Azure"
    description="Naučte se vytvořit novou webovou aplikaci Azure pro blog WordPress pomocí webu Azure Portal."
    services="app-service\web"
    documentationCenter="php"
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="PHP"
    ms.topic="hero-article"
    ms.date="08/11/2016"
    ms.author="robmcm"/>


# Vytvoření webové aplikace WordPress ve službě Azure App Service

[AZURE.INCLUDE [tabs](../../includes/app-service-web-get-started-nav-tabs.md)]

V tomto kurzu se dozvíte, jak nasadit web blogu WordPress z Azure Marketplace.

Po dokončení kurzu budete mít vlastní funkční web blogu WordPress spuštěný v cloudu.

![Web WordPress](./media/web-sites-php-web-site-gallery/wpdashboard.png)

Co se dozvíte:

* Postup nalezení šablony aplikace v Azure Marketplace.
* Postup vytvoření webové aplikace založené na šabloně ve službě Azure App Service.
* Postup konfigurace nastavení služby Azure App Service pro novou webovou aplikaci a databázi.

Azure Marketplace nabízí širokou škálu oblíbených webových aplikací vyvinutých společností Microsoft, jinými společnostmi a iniciativami v oblasti softwaru Open Source. Webové aplikace jsou postaveny na široké škále oblíbených rozhraní, jako je [PHP](/develop/nodejs/) v tomto příkladu aplikace WordPress, [.NET](/develop/net/), [Node.js](/develop/nodejs/), [Java](/develop/java/), [Python](/develop/python/) a další. K vytvoření webové aplikace v Azure Marketplace nepotřebujete žádný software, kromě prohlížeče, který používáte pro [web Azure Portal](https://portal.azure.com/). 

Web WordPress, který nasazujete v tomto kurzu, využívá jako databázi MySQL. Chcete-li jako databázi raději použít SQL Database, informace naleznete v [Projektu Nami](http://projectnami.org/). **Projekt Nami** je rovněž k dispozici prostřednictvím Azure Marketplace.

> [AZURE.NOTE]
> K absolvování tohoto kurzu potřebujete účet Microsoft Azure. Pokud nemáte účet, můžete si [aktivovat výhody předplatitele sady Visual Studio](/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F) nebo se [zaregistrovat k bezplatné zkušební verzi](/en-us/pricing/free-trial/?WT.mc_id=A261C142F).
>
> Chcete-li začít se službou Azure App Service dříve, než se zaregistrujete k účtu Azure, přejděte k možnosti [Vyzkoušet službu App Service](http://go.microsoft.com/fwlink/?LinkId=523751). Zde můžete ihned vytvořit krátkodobou úvodní webovou aplikaci ve službě App Service – aniž by byla požadována platební karta a bez jakýchkoli závazků.

## Výběr aplikace WordPress a konfigurace pro službu Azure App Service

1. Přihlaste se k [webu Azure Portal](https://portal.azure.com/).

2. Klikněte na možnost **Nové**.
    
    ![Vytvořit nové][5]
    
3. Vyhledejte **WordPress** a klikněte na možnost **WordPress**. Chcete-li namísto MySQL použít SQL Database, vyhledejte **Projekt Nami**.

    ![WordPress ze seznamu][7]
    
5. Přečtěte si popis aplikace WordPress a klikněte na možnost **Vytvořit**.

    ![Vytvořit](./media/web-sites-php-web-site-gallery/create.png)

4. Zadejte název webové aplikace do pole **Webová aplikace**.

    Tento název musí být v doméně azurewebsites.net jedinečný, protože webová aplikace bude mít adresu URL {název}.azurewebsites.net. Není-li zadaný název jedinečný, v textovém poli se zobrazí červený vykřičník.

8. Máte-li více předplatných, zvolte to, které chcete použít. 

5. Vyberte **skupinu prostředků** nebo vytvořte novou.

    Další informace o skupinách prostředků najdete v tématu [Přehled Azure Resource Manageru](../resource-group-overview.md).

5. Vyberte **umístění/plán služby App Service** nebo vytvořte nové.

    Podrobnější informace o plánech služby App Service naleznete v tématu [Přehled plánů služby Azure App Service](../azure-web-sites-web-hosting-plans-in-depth-overview.md). 

7. Klikněte na možnost **Databáze** a poté v okně **Nová databáze MySQL** zadejte požadované hodnoty pro konfiguraci databáze MySQL.

    a. Zadejte nový název nebo ponechte výchozí název.

    b. Položku **Typ databáze** ponechte nastavenou na možnost **Shared**.

    c. Zvolte totéž umístění, které jste zvolili pro webovou aplikaci.

    d. Zvolte cenovou úroveň. Mercury (bezplatná úroveň s minimálním počtem povolených připojení a místem na disku) je vhodná pro tento kurz.

8. V okně **Nová databáze MySQL** klikněte na tlačítko **OK**. 

8. V okně **WordPress** přijměte právní podmínky a klikněte na možnost **Vytvořit**. 

    ![Konfigurace webové aplikace](./media/web-sites-php-web-site-gallery/configure.png)

    Služba Azure App Service vytvoří webovou aplikaci, což obvykle trvá méně než minutu. Průběh můžete sledovat kliknutím na ikonu zvonu v horní části stránky portálu.

    ![Ukazatel průběhu](./media/web-sites-php-web-site-gallery/progress.png)

## Spuštění a správa webové aplikace WordPress
    
7. Až budete s vytvářením webové aplikace hotovi, přejděte na webu Azure Portal do skupiny prostředků, v níž jste aplikaci vytvořili, a zde se zobrazí webová aplikace a databáze.

    Dodatečný prostředek s ikonou žárovky je služba [Application Insights](/services/application-insights/), která pro vaši webovou aplikaci zajišťuje služby monitorování.

1. V okně **Skupina prostředků** klikněte na řádek webové aplikace.

    ![Konfigurace webové aplikace](./media/web-sites-php-web-site-gallery/resourcegroup.png)

2. V okně webové aplikace klikněte na tlačítko **Procházet**.

    ![adresa URL webu][browse]

3. Na **úvodní** stránce WordPress zadejte informace o konfiguraci vyžadované aplikací WordPress a klikněte na možnost **Instalovat WordPress**.

    ![Konfigurace WordPress](./media/web-sites-php-web-site-gallery/wpconfigure.png)

4. Přihlaste se pomocí přihlašovacích údajů, které jste vytvořili na **úvodní** stránce.  

5. Otevře se stránka řídicího panelu webu.    

    ![Web WordPress](./media/web-sites-php-web-site-gallery/wpdashboard.png)

## Další kroky

Seznámili jste se s postupem vytvoření a nasazení webové aplikace PHP z galerie. Další informace o použití PHP v Azure naleznete ve [Středisku pro vývojáře PHP](/develop/php/).

Další informace týkající se práce s webovými aplikacemi App Service Web Apps naleznete prostřednictvím odkazů v levé části stránky (máte-li široké okno prohlížeče) nebo v horní části stránky (máte-li úzké okno prohlížeče). 

## Co se změnilo
* Průvodce změnou z webů na službu App Service naleznete v tématu [Služba Azure App Service a její vliv na stávající služby Azure](http://go.microsoft.com/fwlink/?LinkId=529714).

[5]: ./media/web-sites-php-web-site-gallery/startmarketplace.png
[7]: ./media/web-sites-php-web-site-gallery/search-web-app.png
[procházet]: ./media/web-sites-php-web-site-gallery/browse-web.png



<!--HONumber=Sep16_HO3-->


