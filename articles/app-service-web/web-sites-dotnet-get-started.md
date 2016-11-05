---
title: Nasazení aplikace ASP.NET do služby Azure App Service pomocí sady Visual Studio | Microsoft Docs
description: Zjistěte, jak nasadit webový projekt ASP.NET do nové webové aplikace ve službě Azure App Service pomocí sady Visual Studio.
services: app-service\web
documentationcenter: .net
author: tdykstra
manager: wpickett
editor: ''

ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: get-started-article
ms.date: 07/22/2016
ms.author: rachelap

---
# Nasazení webové aplikace ASP.NET do služby Azure App Service pomocí sady Visual Studio
[!INCLUDE [tabs](../../includes/app-service-web-get-started-nav-tabs.md)]

## Přehled
V tomto kurzu se dozvíte, jak nasadit webovou aplikaci ASP.NET do [webové aplikace ve službě Azure App Service](app-service-web-overview.md) pomocí sady Visual Studio 2015.

Kurz předpokládá, že jste vývojáři ASP.NET, kteří nemají žádné předchozí zkušenosti s používáním Azure. Až budete hotovi, budete mít jednoduchou funkční webovou aplikaci spuštěnou v cloudu.

Naučíte se:

* Postup vytvoření nové webové aplikace služby App Service při vytváření nového webového projektu v sadě Visual Studio.
* Postup nasazení webového projektu do webové aplikace služby App Service pomocí sady Visual Studio.

Diagram znázorňuje, co budete v kurzu dělat.

![Diagram vytvoření a nasazení v sadě Visual Studio](./media/web-sites-dotnet-get-started/Create_App.png)

V závěru kurzu vám část s názvem [Řešení problémů](#troubleshooting) poskytne nápady co dělat, pokud něco nebude fungovat, a část [Další kroky](#next-steps) nabídne odkazy na další kurzy, které se používáním služby Azure App Service zabývají podrobněji.

Toto je úvodní kurz, a proto popisuje nasazení webového projektu, který je jednoduchý a nepoužívá databázi ani neprovádí ověřování či autorizaci. Odkazy na pokročilejší témata týkající se nasazení naleznete v části [Postup nasazení webové aplikace Azure](web-sites-deploy.md).

Kromě času potřebného k instalaci sady Azure SDK pro rozhraní .NET bude absolvování tohoto kurzu trvat asi 10 až 15 minut.

## Požadavky
* Kurz předpokládá, že jste již dříve pracovali s ASP.NET MVC a sadou Visual Studio. Pokud potřebujete úvodní informace, prostudujte téma [Začínáme s ASP.NET MVC 5](http://www.asp.net/mvc/overview/getting-started/introduction/getting-started).
* Potřebujete mít účet Azure. Můžete si [zdarma otevřít účet Azure](/pricing/free-trial/?WT.mc_id=A261C142F) nebo [aktivovat výhody pro předplatitele sady Visual Studio](/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F). 
  
    Pokud chcete začít se službou Azure App Service, ještě než si zaregistrujete účet Azure, přejděte k [možnosti vyzkoušet si tuto službu](http://go.microsoft.com/fwlink/?LinkId=523751). Zde můžete vytvořit krátkodobou úvodní aplikaci ve službě App Service – aniž by byla požadována platební karta a bez jakýchkoli závazků.

## <a name="setupdevenv"></a>Nastavení vývojového prostředí
Kurz je napsán pro sadu Visual Studio 2015 se sadou [Azure SDK pro .NET](../dotnet-sdk.md) 2.9 nebo novější. 

* [Stáhněte si nejnovější sadu Azure SDK pro sadu Visual Studio 2015](http://go.microsoft.com/fwlink/?linkid=518003). Sada SDK nainstaluje sadu Visual Studio 2015, pokud ji ještě nemáte.
  
  > [!NOTE]
  > Instalace sady SDK může trvat delší dobu, od několika minut až více než půl hodiny, a to podle počtu závislostí sady SDK, které jsou již v počítači.
  > 
  > 

Pokud máte sadu Visual Studio 2013 a chcete ji používat, můžete si [stáhnout nejnovější sadu Azure SDK pro sadu Visual Studio 2013](http://go.microsoft.com/fwlink/?LinkID=324322). Některé obrazovky se mohou lišit od ilustrací.

## Konfigurace nového webového projektu
Dalším krokem je vytvoření webového projektu v sadě Visual Studio a webové aplikace ve službě Azure App Service. V této části kurzu nakonfigurujete nový webový projekt. 

1. Otevřete sadu Visual Studio 2015.
2. Klikněte na položku **Soubor > Nový > Projekt**.
3. V dialogovém okně **Nový projekt** klikněte na položku **Visual C# > Web > ASP.NET Web Application**.
4. Ujistěte se, zda je jako cílové rozhraní vybrána položka **.NET Framework 4.5.2**.
5. Služba [Azure Application Insights](../application-insights/app-insights-overview.md) monitoruje webovou aplikaci z hlediska dostupnosti, výkonu a využití. Při prvním vytváření webového projektu po instalaci sady Visual Studio je zaškrtávací políčko **Přidat do projektu službu Application Insights** ve výchozím nastavení zaškrtnuto. Pokud nechcete vyzkoušet službu Application Insights, ale toto políčko je zaškrtnuto, zrušte jeho zaškrtnutí.
6. Pojmenujte aplikaci **MyExample** a klikněte na tlačítko **OK**.
   
    ![Dialogové okno Nový projekt](./media/web-sites-dotnet-get-started/GS13newprojdb.png)
7. V dialogovém okně **Nový projekt ASP.NET** vyberte šablonu **MVC** a klikněte na položku **Změna ověřování**.
   
    Pro účely tohoto kurzu provedete nasazení webového projektu ASP.NET MVC. Chcete-li se seznámit s postupem nasazení projektu ASP.NET Web API, informace naleznete v části [Další kroky](#next-steps). 
   
    ![Dialogové okno Nový projekt ASP.NET](./media/web-sites-dotnet-get-started/GS13changeauth.png)
8. V dialogovém okně **Změna ověřování** klikněte na možnost **Bez ověřování** a poté klikněte na tlačítko **OK**.
   
    ![Bez ověřování](./media/web-sites-dotnet-get-started/GS13noauth.png)
   
    V tomto úvodním kurzu nasazujete jednoduchou aplikaci, která neprovádí přihlášení uživatele.
9. Ujistěte se, zda je v části **Microsoft Azure** v dialogovém okně **Nový projekt ASP.NET** zaškrtnuta položka **Hostovat v cloudu** a zda je v rozevíracím seznamu vybrána položka **App Service**.
   
    ![Dialogové okno Nový projekt ASP.NET](./media/web-sites-dotnet-get-started/GS13newaspnetprojdb.png)
   
    Tato nastavení dávají sadě Visual Studio pokyn, aby pro webový projekt vytvořila webovou aplikaci Azure.
10. Klikněte na tlačítko **OK**.

## Konfigurace prostředků Azure pro novou webovou aplikaci
Nyní sadě Visual Studio řeknete, které prostředky Azure má vytvořit.

1. V dialogovém okně **Vytvoření služby App Service** klikněte na tlačítko **Přidat účet** a přihlaste se k Azure pomocí ID a hesla účtu, který používáte ke správě předplatného Azure.
   
    ![Přihlášení k Azure](./media/web-sites-dotnet-get-started/configuresitesettings.png)
   
    Pokud jste se na tomtéž počítači již dříve přihlásili, tlačítko **Přidat účet** se nemusí zobrazit. V takovém případě můžete tento krok přeskočit, nebo je možné, že bude nutné znovu zadat přihlašovací údaje.
2. Zadejte **Název webové aplikace**, který je v doméně *azurewebsites.net* jedinečný. Můžete ji například pojmenovat MyExample s číslem vpravo (např. MyExample810), a tím zajistit její jedinečnost. Pokud je pro vás vytvořen výchozí název webu, tento název je jedinečný a můžete jej použít.
   
    Pokud zadaný název již použil někdo jiný, vpravo se namísto zelené značky zaškrtnutí zobrazí červený vykřičník a je nutné zadat jiný název.
   
    Adresa URL vaší aplikace obsahuje tento název plus *.azurewebsites.net*. Příklad: pokud je název `MyExample810`, adresa URL je `myexample810.azurewebsites.net`.
   
    S webovou aplikací Azure lze rovněž použít vlastní doménu. Další informace naleznete v tématu [Konfigurace vlastního názvu domény ve službě Azure App Service](web-sites-custom-domain-name.md).
3. Klikněte na tlačítko **Nový** vedle pole **Skupina prostředků** a zadejte název „MyExample“, případně jiný preferovaný název. 
   
    ![Dialogové okno Vytvoření služby App Service](./media/web-sites-dotnet-get-started/rgcreate.png)
   
    Skupina prostředků je kolekce prostředků Azure, jako jsou webové aplikace, databáze či virtuální počítače. V rámci kurzu je obecně nejvhodnější vytvořit novou skupinu prostředků, protože pak bude možné jedním krokem snadno odstranit veškeré prostředky Azure, které během kurzu vytvoříte. Další informace naleznete v tématu [Přehled Azure Resource Manager](../resource-group-overview.md).
4. Klikněte na tlačítko **Nový** vedle rozevíracího seznamu **Plán služby App Service**.
   
    ![Dialogové okno Vytvoření služby App Service](./media/web-sites-dotnet-get-started/createasplan.png)
   
    Otevře se dialogové okno **Konfigurace plánu služby App Service**.
   
    ![Dialogové okno Konfigurace plánu služby App Service](./media/web-sites-dotnet-get-started/configasp.png)
   
    V následujících krocích nakonfigurujete plán služby App Service pro novou skupinu prostředků. Plán služby App Service určuje výpočetní prostředky, v nichž je webová aplikace spuštěna. Pokud například vyberete úroveň Free, aplikace API bude spuštěna ve sdílených virtuálních počítačích, zatímco u některých placených úrovní bude spuštěna ve vyhrazených virtuálních počítačích. Další informace naleznete v tématu [Přehled plánů služby App Service](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).
5. V dialogové okně **Konfigurace plánu služby App Service** zadejte název „MyExamplePlan“, případně jiný preferovaný název.
6. V rozevíracím seznamu **Umístění** vyberte umístění, které je vám nejblíže.
   
    Toto nastavení určuje, ve kterém datacentru Azure vaše aplikace poběží. V rámci tohoto kurzu můžete vybrat libovolnou oblast – rozdíl bude nepatrný. Avšak u produkční aplikace bude ve vašem zájmu, aby server byl co nejblíže ke klientům, kteří k němu přistupují, aby se minimalizovala [latence](http://www.bing.com/search?q=web%20latency%20introduction&qs=n&form=QBRE&pq=web%20latency%20introduction&sc=1-24&sp=-1&sk=&cvid=eefff99dfc864d25a75a83740f1e0090).
7. V rozevíracím seznamu **Velikost** klikněte na položku **Free**.
   
    V rámci tohoto kurzu zajistí cenová úroveň Free dostatečný výkon.
8. V dialogovém okně **Konfigurace plánu služby App Service** klikněte na tlačítko **OK**.
9. V dialogovém okně **Vytvoření služby App Service** klikněte na možnost **Vytvořit**.

## Vytvoření projektu a webové aplikace sadou Visual Studio
Zanedlouho, obvykle během méně než minuty, sada Visual Studio vytvoří webový projekt a webovou aplikaci.  

Okno **Průzkumník řešení** zobrazuje soubory a složky v novém projektu.

![Průzkumník řešení](./media/web-sites-dotnet-get-started/solutionexplorer.png)

Okno **Aktivita služby Azure App Service** ukazuje, že byla vytvořena webová aplikace.

![Webová aplikace vytvořená v okně Aktivita služby Azure App Service](./media/web-sites-dotnet-get-started/GS13sitecreated1.png)

Okno **Průzkumník cloudu** umožňuje zobrazit a spravovat prostředky Azure, včetně nové webové aplikace, kterou jste právě vytvořili.

![Webová aplikace vytvořená v průzkumníku cloudu](./media/web-sites-dotnet-get-started/siteinse.png)

## Nasazení webového projektu do webové aplikace Azure
V této části nasadíte webový projekt do webové aplikace.

1. V **Průzkumníku řešení** klikněte pravým tlačítkem na požadovaný projekt a zvolte možnost **Publikovat**.
   
    ![Volba možnosti Publikovat v nabídce sady Visual Studio](./media/web-sites-dotnet-get-started/choosepublish.png)
   
    Během několika sekund se zobrazí průvodce **Publikování webu**. Průvodce se otevře s *profilem publikování*, který obsahuje nastavení pro nasazení webového projektu do nové webové aplikace.
   
    Profil publikování obsahuje uživatelské jméno a heslo pro nasazení.  Tyto přihlašovací údaje byly vygenerovány pro vás a není nutné je zadat. Heslo je zašifrováno ve skrytém souboru konkrétního uživatele ve složce `Properties\PublishProfiles`.
2. Na kartě **Připojení** průvodce **Publikování webu** klikněte na tlačítko **Další**.
   
    ![Kliknutí na tlačítko Další na kartě Připojení v průvodci Publikování webu](./media/web-sites-dotnet-get-started/GS13ValidateConnection.png)
   
    Následuje karta **Nastavení**. Zde můžete změnit konfiguraci sestavení a nasadit sestavení pro ladění za účelem [vzdáleného ladění](web-sites-dotnet-troubleshoot-visual-studio.md#remotedebug). Na této kartě je také několik [možností publikování souboru](https://msdn.microsoft.com/library/dd465337.aspx#Anchor_2).
3. Na kartě **Nastavení** klikněte na tlačítko **Další**.
   
   ![Karta Nastavení průvodce Publikování webu](./media/web-sites-dotnet-get-started/GS13SettingsTab.png)
   
   Následuje karta **Náhled**. Zde můžete zjistit, které soubory budou zkopírovány z projektu do aplikace API. Pokud projekt nasazujete do aplikace API, do které jste již dříve nasadili jiný projekt, zkopírují se pouze změněné soubory. Chcete-li zobrazit seznam kopírovaných položek, můžete kliknout na tlačítko **Spustit náhled**.
4. Na kartě **Náhled** klikněte na tlačítko **Publikovat**.
   
   ![Karta Náhled průvodce Publikování webu](./media/web-sites-dotnet-get-started/GS13previewoutput.png)
   
   Když kliknete na tlačítko **Publikovat**, sada Visual Studio spustí proces kopírování souborů na server Azure. Tento proces může trvat jednu či dvě minuty.
   
   Okna **Výstup** a **Aktivita služby Azure App Service** ukazují, které akce nasazení byly provedeny, a hlásí úspěšné dokončení nasazení.
   
   ![Okno Výstup sady Visual Studio s hlášením úspěšného nasazení](./media/web-sites-dotnet-get-started/PublishOutput.png)
   
   Po úspěšném nasazení se automaticky otevře výchozí prohlížeč s adresou URL nasazené webové aplikace, přičemž vytvořená aplikace je nyní spuštěna v cloudu. Adresa URL v adresním řádku prohlížeče ukazuje, že webová aplikace je načtena z internetu.
   
   ![Webová aplikace spuštěná v Azure](./media/web-sites-dotnet-get-started/GS13deployedsite.png)
   
   > [!TIP]
   > Můžete povolit panel nástrojů **Publikování webu jedním kliknutím** pro rychlé nasazení. Klikněte na položku **Zobrazení > Panely nástrojů** a vyberte možnost **Publikování webu jedním kliknutím**. Pomocí tohoto panelu nástrojů můžete vybrat profil, publikovat kliknutím na tlačítko, případně kliknutím na tlačítko spustit průvodce **Publikování webu**.
   > ![Panel nástrojů Publikování webu jedním kliknutím](./media/web-sites-dotnet-get-started/weboneclickpublish.png)
   > 
   > 

## Řešení potíží
Pokud během tohoto kurzu narazíte na problém, ujistěte se, zda používáte nejnovější verzi sady Azure SDK pro .NET. To lze nejsnáze provést tak, že si [stáhnete sadu Azure SDK pro Visual Studio 2015](http://go.microsoft.com/fwlink/?linkid=518003). Máte-li nainstalovánu aktuální verzi, instalační program webové platformy zobrazí informaci, že instalace není zapotřebí.

Pokud jste v podnikové síti a pokoušíte se provést nasazení do služby Azure App Service přes bránu firewall, ujistěte se, zda jsou pro Nasazení webu otevřené porty 443 a 8172. Nelze-li tyto porty otevřít, v následující části Další kroky naleznete jiné možnosti nasazení.

Jakmile webová aplikace ASP.NET poběží ve službě Azure App Service, pravděpodobně se budete chtít blíže seznámit s funkcemi sady Visual Studio, které usnadňují řešení potíží. Informace o protokolování, vzdáleném ladění apod. naleznete v tématu [Řešení potíží s webovými aplikacemi Azure v sadě Visual Studio](web-sites-dotnet-troubleshoot-visual-studio.md).

## Další kroky
V tomto kurzu jste se seznámili s postupem vytvoření jednoduché webové aplikace a jejího nasazení do webové aplikace Azure. Toto jsou některá související témata a zdroje, které vám umožní blíže se seznámit se službou Azure App Service:

* Monitorování a správa webové aplikace v [portálu Azure](https://portal.azure.com/) 
  
    Další informace naleznete v tématu [přehled portálu Azure](/services/management-portal/) a [Konfigurace webových aplikací ve službě Azure App Service](web-sites-configure.md).
* Nasazení stávajícího webového projektu do nové webové aplikace pomocí sady Visual Studio
  
    V **Průzkumníku řešení** klikněte pravým tlačítkem na požadovaný projekt a poté klikněte na možnost **Publikovat**. Jako cíl publikování zvolte možnost **Microsoft Azure App Service** a klikněte na možnost **Nová**. Dialogová okna jsou poté shodná s těmi, která jste viděli v tomto kurzu.
* Nasazení webového projektu ze správy zdrojů
  
    Informace týkající se [automatizace nasazení](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery) ze [systému správy zdrojů](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control) naleznete v tématu [Začínáme s webovými aplikacemi ve službě Azure App Service](app-service-web-get-started.md) a [Postup nasazení webové aplikace Azure](web-sites-deploy.md).
* Nasazení webového rozhraní API ASP.NET do aplikace API ve službě Azure App Service
  
    Seznámili jste se s postupem vytvoření instance služby Azure App Service, která je určena především k hostování webu. Služba App Service také nabízí funkce pro hostování webových rozhraní API, například podporu CORS a podporu metadat rozhraní API pro generování kódu klienta. Ačkoli lze ve webové aplikaci použít funkce rozhraní API, pokud chcete především hostovat rozhraní API v instanci služby App Service, vhodnější volbou bude **aplikace API**. Další informace naleznete v tématu [Začínáme se službou API Apps a technologií ASP.NET ve službě Azure App Service](../app-service-api/app-service-api-dotnet-get-started.md). 
* Přidání vlastního názvu domény a SSL
  
    Informace týkající se použití protokolu SSL a vlastní domény (příklad: www.contoso.com namísto contoso.azurewebsites.net) naleznete v následujících zdrojích:
  
  * [Konfigurace vlastní domény ve službě Azure App Service](web-sites-custom-domain-name.md)
  * [Povolení protokolu HTTPS pro web Azure](web-sites-configure-ssl-certificate.md)
* Odstranění skupiny prostředků obsahující webovou aplikaci a veškeré související prostředky Azure, se kterými již nechcete pracovat
  
    Informace týkající se práce se skupinami prostředků na portálu Azure naleznete v tématu [Nasazení prostředků pomocí šablon Resource Manageru a portálu Azure](../resource-group-template-deploy-portal.md).   
* Další příklady vytvoření webové aplikace ASP.NET ve službě App Service najdete v článku [Vytvoření a nasazení webové aplikace ve službě Azure App Service](https://github.com/Microsoft/HealthClinic.biz/wiki/Create-and-deploy-an-ASP.NET-web-app-in-Azure-App-Service) a [Vytvoření a nasazení mobilní aplikace ve službě Azure App Service](https://github.com/Microsoft/HealthClinic.biz/wiki/Create-and-deploy-a-mobile-app-in-Azure-App-Service) z [demoverze](https://blogs.msdn.microsoft.com/visualstudio/2015/12/08/connectdemos-2015-healthclinic-biz/) [HealthClinic.biz](https://github.com/Microsoft/HealthClinic.biz) představené na konferenci 2015 Connect. Další rychlé starty z demoverze HealthClinic.biz najdete v článku [Rychlé starty nástrojů pro vývojáře Azure](https://github.com/Microsoft/HealthClinic.biz/wiki/Azure-Developer-Tools-Quickstarts).

<!---HONumber=Aug16_HO4-->


