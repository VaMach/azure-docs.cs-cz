---
title: "Vytvoření webové aplikace v jazyce Java ve službě Azure App Service | Dokumentace Microsoftu"
description: "V tomto kurzu se dozvíte, jak nasadit webovou aplikaci v jazyce Java do služby Azure App Service."
services: app-service\web
documentationcenter: java
author: rmcmurray
manager: erikre
editor: 
ms.assetid: d6e73cc3-8b71-4742-a197-3edeabc6a289
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: Java
ms.topic: get-started-article
ms.date: 11/01/2016
ms.author: robmcm
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: acacfbead6cf0d68ccfeb5e818a2b04f2be9b902


---
# <a name="create-a-java-web-app-in-azure-app-service"></a>Vytvoření webové aplikace v jazyce Java ve službě Azure App Service
[!INCLUDE [tabs](../../includes/app-service-web-get-started-nav-tabs.md)]

V tomto kurzu se dozvíte, jak vytvořit [webová aplikace ve službě Azure App Service] pomocí [Azure Portal]. Portál Azure je webové rozhraní, které lze použít ke správě prostředků Azure.

> [!NOTE]
> K absolvování tohoto kurzu potřebujete účet Microsoft Azure. Pokud nemáte účet, můžete si [aktivovat výhody předplatitele sady Visual Studio] nebo se [zaregistrovat k bezplatné zkušební verzi].
> 
> Chcete-li začít se službou Azure App Service dříve, než se zaregistrujete k účtu Azure, přejděte k možnosti [Vyzkoušet službu App Service]. Zde můžete ihned vytvořit krátkodobou úvodní webovou aplikaci ve službě App Service – aniž by byla požadována platební karta a bez jakýchkoli závazků.
> 
> 

## <a name="java-application-options"></a>Možnosti aplikace v jazyce Java
Existuje několik způsobů, jimiž lze nastavit aplikaci v jazyce Java ve webové aplikaci služby App Service. 

1. Vytvořte aplikaci a nakonfigurujte **Nastavení aplikace**.
   
    Služba App Service poskytuje několik verzí Tomcat a Jetty s výchozí konfigurací. Pokud aplikace, kterou budete hostovat, bude pracovat s některou z integrovaných verzí, tato metoda nastavení webového kontejneru je nejjednodušší a současně je ideální v případě, že chcete pouze nahrát soubor war do webového kontejneru. U této metody vytvoříte aplikaci na webu Azure Portal a potom přejdete do okna **Nastavení aplikace** své aplikace a zvolíte verzi jazyka Java spolu s požadovaným webovým kontejnerem Java. Při použití této metody jsou jazyk Java i webový kontejner spuštěny z Program Files. Ostatní metody umisťují webový kontejner a potenciálně také prostředí Java Virtual Machine do místa na disku. Při použití tohoto modelu nemáte přístup k úpravě souborů v této části systému souborů. To znamená, že nemůžete provádět akce, jako je například konfigurace souboru *server.xml* či umisťování souborů knihovny do složky */lib*. Další informace naleznete dále v tomto kurzu v části [Vytvoření a konfigurace webové aplikace v jazyce Java](#appsettings) .
2. Použijte šablonu z Azure Marketplace.
   
    Azure Marketplace obsahuje šablony, které automaticky vytvářejí a konfigurují webové aplikace v jazyce Java s webovými kontejnery Tomcat nebo Jetty. Webové kontejnery vytvářené těmito šablonami lze konfigurovat. Další informace naleznete v tomto kurzu v části [Použití šablony Java z Azure Marketplace](#marketplace).
3. Vytvořte aplikaci a ručně zkopírujte a upravte konfigurační soubory. 
   
    Je možné, že chcete hostovat vlastní aplikaci v jazyce Java, která není nasazena do žádného z webových kontejnerů poskytovaných službou App Service. Příklad:
   
   * Aplikace v jazyce Java vyžaduje verzi Tomcat nebo Jetty, která není službou App Service přímo podporována nebo není poskytnuta v galerii.
   * Aplikace v jazyce Java přijímá požadavky protokolu HTTP a není nasazena jako soubor WAR do již existujícího webového kontejneru.
   * Chcete sami od začátku nakonfigurovat webový kontejner. 
   * Chcete použít verzi jazyka Java, která není ve službě App Service podporována, a chcete ji nahrát sami.
     
     Pro takovéto případy můžete vytvořit aplikaci pomocí portálu Azure a poté ručně poskytnout příslušné soubory modulu runtime. V takovém případě se soubory započítají vůči kvótám prostoru úložiště pro plán služby App Service. Další informace naleznete v tématu [Nahrání vlastní webové aplikace v jazyce Java do Azure].

## <a name="a-nameportala-create-and-configure-a-java-web-app"></a><a name="portal"></a>Vytvoření a konfigurace webové aplikace v jazyce Java
V této části se dozvíte, jak vytvořit webovou aplikaci a nakonfigurovat ji pro jazyk Java pomocí okna **Nastavení aplikace** portálu.

1. Přihlaste se k webu [Azure Portal].
2. Klikněte na položku **Nové > Web + mobilní zařízení > Webová aplikace**.
   
    ![Nová webová aplikace][newwebapp]
3. Zadejte název webové aplikace do pole **Webová aplikace**.
   
    Tento název musí být v doméně azurewebsites.net jedinečný, protože webová aplikace bude mít adresu URL {název}.azurewebsites.net. Není-li zadaný název jedinečný, v textovém poli se zobrazí červený vykřičník.
4. Vyberte **skupinu prostředků** nebo vytvořte novou.
   
    Podrobnější informace o skupinách prostředků naleznete v tématu [Použití webu Azure Portal ke správě prostředků Azure].
5. Vyberte **umístění/plán služby App Service** nebo vytvořte nové.
   
    Podrobnější informace o plánech služby App Service naleznete v tématu [Přehled plánů služby Azure App Service].
6. Klikněte na možnost **Vytvořit**.
   
    ![Vytvoření webové aplikace][newwebapp2]
7. Po vytvoření webové aplikace klikněte na možnost **Webové aplikace > {vaše webová aplikace}**.
   
    ![Výběr webové aplikace][selectwebapp]
8. V okně **Webová aplikace** klikněte na možnost **Nastavení**.
9. Klikněte na možnost **Nastavení aplikace**.
10. Vyberte požadovanou **verzi Javy**. 
11. Vyberte požadovanou **podverzi Javy**. Pokud vyberete položku **Nejnovější**, aplikace bude používat nejnovější podverzi, která je pro danou hlavní verzi Javy k dispozici ve službě App Service. Položka **Nejnovější** je jedinečná pro aplikace v jazyce Java vytvořené v **Nastavení aplikace**. Vytvoříte-li aplikaci v jazyce Java z galerie, bude nutné spravovat vlastní změny kontejneru a prostředí Java Virtual Machine. 
12. Zvolte požadovaný **Webový kontejner**. Pokud vyberete název kontejneru, který začíná slovem**Nejnovější**, vaše aplikace bude udržována na nejnovější verzi hlavní verze tohoto webového kontejneru dostupné ve službě App Service. 
    
    ![Verze webového kontejneru][versions]
13. Klikněte na možnost **Uložit**.
    
    Webová aplikace se během chvíle stane aplikací založenou na jazyce Java a nakonfigurovanou pro použití vámi vybraného webové kontejneru.
14. Klikněte na možnost **Webové aplikace > {vaše nová webová aplikace}**.
15. Klikněte na adresu **URL** a přejděte na nový web.
    
    Webová stránka potvrdí, že jste vytvořili webovou aplikaci založenou na jazyce Java.

## <a name="a-namemarketplacea-use-a-java-template-from-the-azure-marketplace"></a><a name="marketplace"></a>Použití šablony v jazyce Java z Azure Marketplace
V této části se dozvíte, jak vytvořit webovou aplikaci v jazyce Java pomocí Azure Marketplace. Tentýž obecný postup lze také použít k vytvoření mobilní aplikace nebo aplikace API založené na jazyce Java. 

1. Přihlaste se k webu [Azure Portal].
2. Klikněte na možnost **Nové > Marketplace**.
   
    ![Nový Marketplace][newmarketplace]
3. Klikněte na možnost **Web + mobilní zařízení**.
   
    Pravděpodobně bude nutné posunutím vlevo zobrazit okno **Marketplace**, kde můžete vybrat položku **Web + mobilní zařízení**.
4. Do textového vyhledávacího pole zadejte název aplikačního serveru Java, například **Apache Tomcat** nebo **Jetty**, a stiskněte klávesu Enter.
5. Ve výsledcích hledání klikněte na aplikační server Java.
   
    ![Web Mobilní zařízení Jetty][webmobilejetty]
6. V prvním okně **Apache Tomcat** nebo **Jetty** klikněte na možnost **Vytvořit**.
   
    ![Okno portálu Jetty][jettyblade]
7. V dalším okně **Apache Tomcat** nebo **Jetty** zadejte název webové aplikace do pole **Webová aplikace**.
   
    Tento název musí být v doméně azurewebsites.net jedinečný, protože webová aplikace bude mít adresu URL {název}.azurewebsites.net. Není-li zadaný název jedinečný, v textovém poli se zobrazí červený vykřičník.
8. Vyberte **skupinu prostředků** nebo vytvořte novou.
   
    Podrobnější informace o skupinách prostředků naleznete v tématu [Použití webu Azure Portal ke správě prostředků Azure].
9. Vyberte **umístění/plán služby App Service** nebo vytvořte nové.
   
    Podrobnější informace o plánech služby App Service naleznete v tématu [Přehled plánů služby Azure App Service].
10. Klikněte na možnost **Vytvořit**.
    
    ![Portál Jetty Vytvořit][jettyportalcreate2]
    
    Zanedlouho, obvykle během méně než minuty, Azure dokončí vytváření nové webové aplikace.
11. Klikněte na možnost **Webové aplikace > {vaše nová webová aplikace}**.
12. Klikněte na adresu **URL** a přejděte na nový web.
    
    ![Jetty URL][jettyurl]
    
    Server Tomcat je dodáván s výchozí sadou stránek; pokud jste zvolili Tomcat, zobrazí se stránka podobná následujícímu příkladu.
    
    ![Webová aplikace využívající Apache Tomcat][tomcat]
    
    Pokud jste zvolili Jetty, zobrazí se stránka podobná následujícímu příkladu. Jetty nemá výchozí sadu stránek, proto je zde znovu použito totéž JSP jako pro prázdný web v jazyce Java.
    
    ![Webová aplikace využívající Jetty][jetty]

Nyní, když jste vytvořili webovou aplikaci s kontejnerem aplikace, pokračujte částí [Další kroky](#next-steps), která vás seznámí s postupem nahrání aplikace do webové aplikace.

## <a name="next-steps"></a>Další kroky
V tomto okamžiku je ve vaší webové aplikaci ve službě Azure App Service spuštěn aplikační server Java. Chcete-li do webové aplikace nasadit vlastní kód, informace naleznete v tématu [Přidání aplikace nebo webové stránky do webové aplikace v jazyce Java].

Další informace týkající se vývoje aplikací v jazyce Java v Azure naleznete v tématu [Středisko pro vývojáře Java].

<!-- URL List -->

[Přidání aplikace nebo webové stránky do webové aplikace v jazyce Java]: ./web-sites-java-add-app.md
[Přehled plánů služby Azure App Service]: ../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md
[Azure Portal]: https://portal.azure.com/
[aktivovat výhody předplatitele sady Visual Studio]: http://go.microsoft.com/fwlink/?LinkId=623901
[zaregistrovat k bezplatné zkušební verzi]: http://go.microsoft.com/fwlink/?LinkId=623901
[Vyzkoušet službu App Service]: http://go.microsoft.com/fwlink/?LinkId=523751
[webová aplikace ve službě Azure App Service]: http://go.microsoft.com/fwlink/?LinkId=529714
[Středisko pro vývojáře Java]: /develop/java/
[Použití webu Azure Portal ke správě prostředků Azure]: ../azure-portal/resource-group-portal.md
[Nahrání vlastní webové aplikace v jazyce Java do Azure]: ./web-sites-java-custom-upload.md

<!-- IMG List -->

[newwebapp]: ./media/web-sites-java-get-started/newwebapp.png
[newwebapp2]: ./media/web-sites-java-get-started/newwebapp2.png
[selectwebapp]: ./media/web-sites-java-get-started/selectwebapp.png
[versions]: ./media/web-sites-java-get-started/versions.png
[newmarketplace]: ./media/web-sites-java-get-started/newmarketplace.png
[webmobilejetty]: ./media/web-sites-java-get-started/webmobilejetty.png
[jettyblade]: ./media/web-sites-java-get-started/jettyblade.png
[jettyportalcreate2]: ./media/web-sites-java-get-started/jettyportalcreate2.png
[jettyurl]: ./media/web-sites-java-get-started/jettyurl.png
[tomcat]: ./media/web-sites-java-get-started/tomcat.png
[jetty]: ./media/web-sites-java-get-started/jetty.png



<!--HONumber=Nov16_HO2-->


