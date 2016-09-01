<properties
    pageTitle="Začínáme s aplikacemi API a technologií ASP.NET v App Service | Microsoft Azure"
    description="Naučte se vytvářet, nasazovat a využívat aplikace API technologie ASP.NET v Azure App Service pomocí nástroje Visual Studio 2015."
    services="app-service\api"
    documentationCenter=".net"
    authors="tdykstra"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service-api"
    ms.workload="na"
    ms.tgt_pltfrm="dotnet"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="05/27/2016"
    ms.author="rachelap"/>

# Začínáme s aplikacemi API, technologií ASP.NET a Swaggerem v Azure App Service

[AZURE.INCLUDE [selector](../../includes/app-service-api-get-started-selector.md)]

Toto je první ze série kurzů věnovaných ukázce používání funkcí Azure App Service, které jsou užitečné pro vývoj a hostování rozhraní RESTful API.  Tento kurz se zaměřuje na podporu metadat rozhraní API ve formátu Swagger.

Naučíte se:

* Jak vytvořit a nasadit [aplikace API](app-service-api-apps-why-best-platform.md) v Azure App Service pomocí nástrojů integrovaných v nástroji Visual Studio 2015
* Jak automatizovat zjišťování rozhraní API pomocí balíčku NuGet Swashbuckle a dynamicky generovat metadata rozhraní API Swaggeru
* Jak používat metadata rozhraní API Swaggeru k automatickému generování kódu klienta pro aplikace API

## Přehled ukázkové aplikace

V tomto návodu budeme pracovat s jednoduchou ukázkovou aplikací seznamu úkolů. Aplikace má front-end jednostránkové aplikace (SPA), střední vrstvu webového rozhraní API ASP.NET a datovou vrstvu webového rozhraní API ASP.NET.

![Diagram ukázkové aplikace API](./media/app-service-api-dotnet-get-started/noauthdiagram.png)

Tady je snímek obrazovky front-endu [AngularJS](https://angularjs.org/).

![Seznam úkolů ukázkové aplikace API](./media/app-service-api-dotnet-get-started/todospa.png)

Řešení nástroje Visual Studio se skládá ze tří projektů:

![](./media/app-service-api-dotnet-get-started/projectsinse.png)

* **ToDoListAngular** – front-end: jednostránková aplikace AngularJS, která volá střední vrstvu. 

* **ToDoListAPI** – střední vrstva: projekt webového rozhraní API ASP.NET, který volá datovou vrstvu k provádění operací CRUD s položkami seznamu úkolů.

* **ToDoListDataAPI** – datová vrstva: projekt webového rozhraní API ASP.NET, který provádí operace CRUD s položkami seznamu úkolů. 

Třívrstvá architektura je jednou z mnoha architektur, které můžete pomocí aplikací API implementovat, a tady je použita pouze pro účely ukázky. Kód v jednotlivých vrstvách je co nejjednodušší, aby mohl sloužit jako ukázka funkcí aplikací API: například datová vrstva používá místo databáze paměť serveru jako svůj mechanismus trvalosti.

Po dokončení tohoto kurzu budete mít v cloudu v App Service API Apps nastavené a spuštěné dva projekty webového rozhraní API.

Další kurz v této sérii nasadí front-end jednostránkové aplikace do cloudu.

## Požadavky

* Webové rozhraní API ASP.NET – tento kurz předpokládá základní znalosti práce s [webovým rozhraním API 2](http://www.asp.net/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api) ASP.NET v nástroji Visual Studio.

* Účet Azure – můžete si [zdarma otevřít účet Azure](/pricing/free-trial/?WT.mc_id=A261C142F) nebo [aktivovat výhody pro předplatitele nástroje Visual Studio](/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).

    Pokud chcete začít se službou Azure App Service, ještě než si zaregistrujete účet Azure, přejděte k [možnosti vyzkoušet si tuto službu](http://go.microsoft.com/fwlink/?LinkId=523751). Tam si můžete v App Service hned vytvořit krátkodobou úvodní aplikaci – nepožaduje se žádná platební karta ani to s sebou nenese žádné závazky.

* Visual Studio 2015 se [sadou Azure SDK pro .NET](http://go.microsoft.com/fwlink/?linkid=518003) – sada SDK nainstaluje nástroj Visual Studio 2015 automaticky, pokud ho ještě nemáte.

    >[AZURE.NOTE] V závislosti na tom, kolik závislostí sady SDK už na počítači máte, může instalace sady SDK trvat dlouhou dobu: několik minut, půl hodiny nebo i déle.

## Stažení ukázkové aplikace 

1. Stáhněte si úložiště [Azure-Samples/app-service-api-dotnet-to-do-list](https://github.com/Azure-Samples/app-service-api-dotnet-todo-list).

    Můžete kliknout na tlačítko **Stáhnout ZIP** nebo klonovat úložiště na místním počítači. 

2. Otevřete řešení seznamu úkolů v nástroji Visual Studio 2015 nebo 2013.

2. Sestavte řešení, aby se obnovily balíčky NuGet.

    Pokud si chcete aplikaci prohlédnout v provozu ještě před nasazením, můžete ji spustit místně. Jenom zkontrolujte, že všechny tři projekty jsou spouštěné projekty. Je potřeba použít Internet Explorer nebo Microsoft Edge, protože tyto prohlížeče umožňují volání JavaScriptu na adresy URL `http://localhost` mezi zdroji. 

## Používání uživatelského rozhraní a metadat rozhraní API Swaggeru

Podpora metadat rozhraní API [Swaggeru](http://swagger.io/) 2.0 je integrována v Azure App Service. Každá aplikace API může určit koncový bod adresy URL, který vrací metadata pro rozhraní API ve formátu dat JSON pro Swagger. Metadata vrácená z tohoto koncového bodu je možné použít ke generování kódu klienta. 

Projekt webového rozhraní API ASP.NET může dynamicky generovat metadata Swagger pomocí balíčku NuGet [Swashbuckle](https://www.nuget.org/packages/Swashbuckle). Balíček NuGet Swashbuckle již je v projektech ToDoListDataAPI a ToDoListAPI, které jste si stáhli, nainstalována.

V této části kurzu si prohlédnete generovaná metadata Swagger 2.0 a vyzkoušíte si testovací uživatelské rozhraní založené na metadatech Swagger.

2. Nastavte projekt ToDoListDataAPI (**ne** projekt ToDoListAPI) jako spouštěný projekt. 
 
4. Stisknutím klávesy F5 nebo kliknutím na **Ladění > Spustit ladění** spusťte projekt v režimu ladění.

    Otevře se prohlížeč a zobrazí se chybová stránka HTTP 403.

12. Na konec adresy v adresním řádku doplňte `swagger/docs/v1` a stiskněte Enter. (Adresa URL je `http://localhost:45914/swagger/docs/v1`.)

    Toto je výchozí adresa URL, kterou používá Swashbuckle k vracení metadat JSON pro Swagger 2.0 pro rozhraní API. 

    Pokud používáte Internet Explorer, zobrazí se v prohlížeči výzva ke stažení souboru *v1.json*.

    ![Stažení metadat JSON do IE](./media/app-service-api-dotnet-get-started/iev1json.png)

    Pokud používáte Chrome, Firefox nebo Microsoft Edge, zobrazí se data JSON v okně prohlížeče. Různé prohlížeče zpracovávají data JSON různě a okno prohlížeče může vypadat jinak než v tomto příkladu.

    ![Metadata JSON v Chromu](./media/app-service-api-dotnet-get-started/chromev1json.png)

    Následující příklad ukazuje první část metadat Swagger pro rozhraní API s definicí pro metodu Get. Tato metadata řídí uživatelské rozhraní Swagger, které budete používat v následujících krocích, a v další části kurzu slouží k automatickému generování kódu klienta.

        {
          "swagger": "2.0",
          "info": {
            "version": "v1",
            "title": "ToDoListDataAPI"
          },
          "host": "localhost:45914",
          "schemes": [ "http" ],
          "paths": {
            "/api/ToDoList": {
              "get": {
                "tags": [ "ToDoList" ],
                "operationId": "ToDoList_GetByOwner",
                "consumes": [ ],
                "produces": [ "application/json", "text/json", "application/xml", "text/xml" ],
                "parameters": [
                  {
                    "name": "owner",
                    "in": "query",
                    "required": true,
                    "type": "string"
                  }
                ],
                "responses": {
                  "200": {
                    "description": "OK",
                    "schema": {
                      "type": "array",
                      "items": { "$ref": "#/definitions/ToDoItem" }
                    }
                  }
                },
                "deprecated": false
              },

1. Zavřete prohlížeč a zastavte ladění v nástroji Visual Studio.

3. V **Průzkumníku řešení** otevřete v projektu ToDoListDataAPI soubor *App_Start\SwaggerConfig.cs*, přejděte dolů na následující kód a odkomentujte ho.

        /*
            })
        .EnableSwaggerUi(c =>
            {
        */

    Soubor *SwaggerConfig.cs* se v projektu vytvoří při instalaci balíčku Swashbuckle. Pomocí tohoto souboru můžete Swashbuckle nakonfigurovat několika způsoby.

    Kód, který jste odkomentovali, aktivuje uživatelské rozhraní Swagger, které budete používat v dalších krocích. Při vytváření projektu webového rozhraní API pomocí šablony projektu aplikace API je tento kód ve výchozím nastavení okomentován. Jedná se o bezpečnostní opatření.

5. Spusťte projekt znovu.

3. Na konec adresy v adresním řádku doplňte `swagger` a stiskněte Enter. (Adresa URL je `http://localhost:45914/swagger`.)

4. Když se otevře stránka uživatelského rozhraní Swagger, zobrazte kliknutím na **ToDoList** (Seznam úkolů) dostupné metody.

    ![Dostupné metody uživatelského rozhraní Swagger](./media/app-service-api-dotnet-get-started/methods.png)

5. Klikněte na první tlačítko **Get** v seznamu.

6. V části **Parameters** (Parametry) zadejte jako hodnotu parametru `owner` hvězdičku a potom klikněte na **Try it out** (Vyzkoušet).

    Pokud v dalších kurzech přidáte ověřování, střední vrstva poskytne datové vrstvě skutečné ID uživatele. Dokud ale aplikace běží bez povolení ověřování, mají všechny úlohy místo ID vlastníka hvězdičku.

    ![Vyzkoušení uživatelského rozhraní Swagger](./media/app-service-api-dotnet-get-started/gettryitout1.png)

    Uživatelské rozhraní Swagger zavolá metodu Get seznamu úkolů a zobrazí kód odpovědi a výsledky JSON.

    ![Vyzkoušení uživatelského rozhraní Swagger – výsledky](./media/app-service-api-dotnet-get-started/gettryitout.png)

6. Klikněte na **Post** a potom klikněte na pole v části **Model Schema** (Schéma modelu).

    Kliknutím na schéma modelu předvyplníte vstupní pole, do kterého můžete zadat hodnotu parametru pro metodu Post. (Pokud používáte Internet Explorer a tento postup nefunguje, použijte jiný prohlížeč, nebo zadejte hodnotu parametru v dalším kroku ručně.)  

    ![Vyzkoušení uživatelského rozhraní Swagger – metoda Post](./media/app-service-api-dotnet-get-started/post.png)

7. Změňte kód JSON ve vstupním poli parametru `todo` tak, aby vypadal jako následující příklad, nebo nahraďte váš vlastní text popisu:

        {
          "ID": 2,
          "Description": "buy the dog a toy",
          "Owner": "*"
        }

10. Klikněte na **Try it out** (Vyzkoušet).

    Rozhraní API seznamu úkolů vrátí kód odpovědi HTTP 204, který označuje úspěch.

11. Klikněte na první tlačítko **Get** a potom v této části stránky klikněte na tlačítko **Try it out** (Vyzkoušet).

    Odpověď metody Get teď obsahuje novou položku seznamu úkolů. 

12. Nepovinné: Vyzkoušejte také metody Put, Delete a Get by ID.

14. Zavřete prohlížeč a zastavte ladění v nástroji Visual Studio.

Swashbuckle funguje s libovolným projektem webového rozhraní API ASP.NET. Pokud chcete přidat generování metadat Swagger do existujícího projektu, stačí nainstalovat balíček Swashbuckle. 

**Poznámka:** Metadata Swagger mají pro každou operaci s rozhraním API jedinečný identifikátor. Ve výchozím nastavení může Swashbuckle generovat duplicitní ID operací Swagger pro metody kontroleru webového rozhraní API. K tomu dojde, pokud kontroler přetížil metody HTTP, například `Get()` nebo `Get(id)`. Informace o tom, jak vyřešit přetížení, najdete v tématu [Přizpůsobení definic rozhraní API generovaných ve Swashbuckle](app-service-api-dotnet-swashbuckle-customize.md). Pokud vytvoříte projekt webového rozhraní API v nástroji Visual Studio pomocí šablony aplikace API Azure, přidá se kód, který generuje jedinečné identifikátory operací, automaticky do souboru *SwaggerConfig.cs*.  

## <a id="createapiapp"></a> Vytvoření aplikace API v Azure a nasazení kódu v ní

V této části použijte nástroje Azure, které jsou integrovány v průvodci **Publikovat web** nástroje Visual Studio, k vytvoření nové aplikace API v Azure. Potom nasaďte projekt ToDoListDataAPI do nové aplikace API a spuštěním uživatelského rozhraní Swagger zavolejte rozhraní API.

1. V **Průzkumníku řešení** klikněte pravým tlačítkem na projekt ToDoListDataAPI a potom klikněte na **Publikovat**.

    ![Kliknutí na Publikovat v nástroji Visual Studio](./media/app-service-api-dotnet-get-started/pubinmenu.png)

3.  V kroku **Profil** průvodce **Publikovat web** klikněte na **Microsoft Azure App Service**.

    ![Kliknutí na Azure App Service v průvodci Publikovat web](./media/app-service-api-dotnet-get-started/selectappservice.png)

4. Přihlaste se k účtu Azure, pokud ještě nejste přihlášeni, nebo aktualizujte přihlašovací údaje, jestliže vypršela jejich platnost.

4. V dialogovém okně App Service zvolte **předplatné** Azure, které chcete použít, a klikněte na **Nová**.

    ![Kliknutí na Nová v dialogovém okně App Service](./media/app-service-api-dotnet-get-started/clicknew.png)

    Zobrazí se karta **Hosting** dialogového okna **Vytvořit službu App Service**.

    Vzhledem k tomu, že nasazujete projekt webového rozhraní API, ve kterém je nainstalován Swashbuckle, předpokládá Visual Studio, že chcete vytvořit aplikaci API. To vyplývá z názvu **Název aplikace API** a ze skutečnosti, že rozevírací seznam **Změnit typ** je nastaven na **Aplikace API**.

    ![Typ aplikace v dialogovém okně App Service](./media/app-service-api-dotnet-get-started/apptype.png)

4. Zadejte **název aplikace API**, který bude v doméně *azurewebsites.net* jedinečný. Můžete přijmout výchozí název, který Visual Studio navrhne.

    Pokud zadáte název, který již použil někdo jiný, zobrazí se vpravo červený vykřičník.

    Adresa URL aplikace API bude `{API app name}.azurewebsites.net`.

6. V rozevíracím seznamu **Skupina prostředků** klikněte na **Nová** a zadejte ToDoListGroup nebo jiný název, který preferujete. 

    Skupina prostředků je kolekce prostředků Azure, jako jsou aplikace API, databáze, virtuální počítače apod. V rámci tohoto kurzu bude nejlepší vytvořit novou skupinu prostředků, protože pak bude možné v jednom kroku odstranit všechny prostředky Azure, které při kurzu vytvoříte.

    V tomto poli můžete vybrat existující [skupinu prostředků](../resource-group-overview.md). Můžete taky vytvořit novou tak, že zadáte název, který zatím nemá žádná existující skupina prostředků ve vašem předplatném.

4. Klikněte na tlačítko **Nová** vedle rozevíracího seznamu **Plán služby App Service**.

    Hodnoty **názvu aplikace API**, **předplatného** a **skupiny prostředků** na tomto snímku obrazovky jsou vzorové – vaše hodnoty se budou lišit.

    ![Dialogové okno Vytvořit službu App Service](./media/app-service-api-dotnet-get-started/createas.png)

    V následujícím postupu vytvoříte plán služby App Service pro novou skupinu prostředků. Plán služby App Service určuje výpočetní prostředky, na kterých běží vaše aplikace API. Pokud se například rozhodnete pro úroveň Free, vaše aplikace API poběží na sdílených virtuálních počítačích, zatímco u některých placených úrovní poběží na vyhrazených virtuálních počítačích. Informace o plánech služby App Service naleznete v části [Přehled plánů služby App Service](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).

5. V dialogovém okně **Nakonfigurovat plán služby App Service** zadejte ToDoListPlan nebo jiný název, který preferujete.

5. V rozevíracím seznamu **Umístění** vyberte umístění, které je k vám nejblíže.

    Toto nastavení určuje, ve kterém datacentru Azure vaše aplikace poběží. Zvolením umístění, které od vás není daleko, minimalizujete [latenci](http://www.bing.com/search?q=web%20latency%20introduction&qs=n&form=QBRE&pq=web%20latency%20introduction&sc=1-24&sp=-1&sk=&cvid=eefff99dfc864d25a75a83740f1e0090).

5. V rozevíracím seznamu **Velikost** klikněte na **Free**.

    V rámci tohoto kurzu zajistí cenová úroveň Free dostatečný výkon.

6. V dialogovém okně **Nakonfigurovat plán služby App Service** klikněte na **OK**.

    ![Kliknutí na OK v dialogovém okně Nakonfigurovat plán služby App Service](./media/app-service-api-dotnet-get-started/configasp.png)

7. V dialogovém okně **Vytvořit službu App Service** klikněte na **Vytvořit**.

    ![Kliknutí na Vytvořit v dialogovém okně Vytvořit službu App Service](./media/app-service-api-dotnet-get-started/clickcreate.png)

    Visual Studio vytvoří aplikaci API a profil publikování, který bude obsahovat všechna požadovaná nastavení pro aplikaci API. Potom otevře průvodce **Publikovat web**, který použijete k nasazení projektu.

    Průvodce **Publikovat web** se otevře na kartě **Připojení** (viz následující obrázek). 

    Nastavení **Server** a **Název lokality** na kartě **Připojení** ukazují na vaši aplikaci API. **Uživatelské jméno** a **Heslo** jsou přihlašovací údaje nasazení, která pro vás vytvoří Azure. Po nasazení otevře Visual Studio v prohlížeči **cílovou adresu URL** (to je jediný účel **cílové adresy URL**).  

8. Klikněte na **Další**. 

    ![Kliknutí na Další na kartě Připojení průvodce Publikovat web](./media/app-service-api-dotnet-get-started/connnext.png)

    Další karta je karta **Nastavení** (viz následující obrázek). Tady můžete na kartě konfigurace buildu změnit nasazení ladicího buildu na [vzdálené ladění](../app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md#remotedebug). Na této kartě je také několik **možností publikování souboru**:

    * Odebrat další soubory v cíli
    * Předkompilovat během publikování
    * Vyjmout soubory ze složky App_Data

    V tomto kurzu není potřeba vybírat žádnou z nich. Podrobné vysvětlení, co tyto možnosti představují, najdete v části [Postupy: Nasazení webového projektu pomocí publikování jedním kliknutím v nástroji Visual Studio](https://msdn.microsoft.com/library/dd465337.aspx).

14. Klikněte na **Další**.

    ![Kliknutí na Další na kartě Nastavení průvodce Publikovat web](./media/app-service-api-dotnet-get-started/settingsnext.png)

    Další karta je karta **Náhled** (viz následující obrázek), která umožňuje zjistit, které soubory budou zkopírovány z projektu do aplikace API. Pokud projekt nasazujete do aplikace API, do které jste už dříve nasadili jiný projekt, pak se jenom zkopírují změněné soubory. Pokud chcete zobrazit seznam kopírovaného obsahu, můžete kliknout na tlačítko **Spustit náhled**.

15. Klikněte na **Publikovat**.

    ![Kliknutí na Publikovat na kartě Náhled v průvodci Publikovat web](./media/app-service-api-dotnet-get-started/clickpublish.png)

    Visual Studio nasadí projekt ToDoListDataAPI do nové aplikace API. Okno **Výstup** zaprotokoluje úspěšné nasazení a v okně prohlížeče se zobrazí stránka na adrese URL aplikace API s informací o úspěšném vytvoření.

    ![Okno výstupu – úspěšné nasazení](./media/app-service-api-dotnet-get-started/deploymentoutput.png)

    ![Stránka úspěšného vytvoření nové aplikace API](./media/app-service-api-dotnet-get-started/appcreated.png)

11. Na konec adresy URL v adresním řádku prohlížeče doplňte „swagger“ a stiskněte Enter. (Adresa URL je `http://{apiappname}.azurewebsites.net/swagger`.)

    Prohlížeč zobrazí stejné uživatelské rozhraní Swagger, které jste viděli již dříve, teď ale běží v cloudu. Vyzkoušejte metodu Get a zjistíte, že jste zpět u 2 výchozích položek seznamu úkolů. Změny, které jste provedli dříve, se uložily v paměti na místním počítači.

12. Otevřete [Portál Azure](https://portal.azure.com/).

    Portál Azure je webové rozhraní pro správu prostředků Azure, jako jsou například aplikace API.
 
14. Klikněte na **Procházet > App Services**.

    ![Procházení služeb App Services](./media/app-service-api-dotnet-get-started/browseas.png)

15. V okně **App Services** vyhledejte a vyberte novou aplikaci API. (Oddíly, které se otevřou vpravo na Portálu Azure, označujeme jako *okna *.)

    ![Okno App Services](./media/app-service-api-dotnet-get-started/choosenewapiappinportal.png)

    Otevřou se dvě okna. V jednom okně je přehled aplikace API a ve druhém dlouhý seznam nastavení, která můžete zobrazit a měnit.

16. V okně **Nastavení** najděte část **API** a klikněte na **Definice rozhraní API**. 

    ![Definice rozhraní API v okně Nastavení](./media/app-service-api-dotnet-get-started/apidefinsettings.png)

    Okno **Definice rozhraní API** umožňuje zadat adresu URL, která vrací metadata Swagger 2.0 ve formátu JSON. Když Visual Studio vytvoří aplikaci API, nastaví adresu URL definice rozhraní API na výchozí hodnotu pro metadata generovaná ve Swashbuckle, kterou jste viděli dříve, což je základní adresa URL aplikace API plus `/swagger/docs/v1`. 

    ![Adresa URL definice rozhraní API](./media/app-service-api-dotnet-get-started/apidefurl.png)

    Když vyberete aplikaci API, pro kterou chcete vygenerovat kód klienta, načte Visual Studio metadata z této adresy URL. 

## <a id="codegen"></a> Generování kódu klienta pro datovou vrstvu

Jednou z výhod integrace Swaggeru do aplikací API Azure je automatické generování kódu. Vygenerované třídy klienta usnadňují psaní kódu, který volá aplikaci API.

Projekt ToDoListAPI už má kód klienta vygenerovaný, ale v následujících krocích ho odstraníme a znovu vygenerujeme, abyste si mohli postup generování kódu prohlédnout.

1. V **Průzkumníku řešení** nástroje Visual Studio odstraňte v projektu ToDoListAPI složku *ToDoListDataAPI*. **Upozornění: Odstraňte jenom tuto složku, ne projekt ToDoListDataAPI.**

    ![Odstranění vygenerovaného kódu klienta](./media/app-service-api-dotnet-get-started/deletecodegen.png)

    Tato složka byla vytvořena procesem generování kódu, kterým se chystáte projít.

2. Klikněte pravým tlačítkem na projekt ToDoListAPI a potom klikněte na **Přidat > Klient REST API**.

    ![Přidání klienta REST API v nástroji Visual Studio](./media/app-service-api-dotnet-get-started/codegenmenu.png)

3. V dialogovém okně **Přidat klienta REST API** klikněte na **Adresa URL Swaggeru** a potom na **Vybrat prostředek Azure**.

    ![Výběr prostředku Azure](./media/app-service-api-dotnet-get-started/codegenbrowse.png)

8. V dialogovém okně **App Service** rozbalte skupinu prostředků, kterou používáte pro tento kurz, vyberte svou aplikaci API a klikněte na **OK**.

    ![Výběr aplikace API pro generování kódu](./media/app-service-api-dotnet-get-started/codegenselect.png)

    Všimněte si, že při návratu do dialogového okna **Přidat klienta REST API** se do textového pole doplnila hodnota adresy URL definice rozhraní API, kterou jste předtím viděli na portálu. 

    ![Adresa URL definice rozhraní API](./media/app-service-api-dotnet-get-started/codegenurlplugged.png)

    >[AZURE.TIP] Alternativní způsob získání metadat pro generování kódu: místo použití dialogového okna procházení můžete adresu URL zadat přímo. Nebo pokud chcete kód klienta generovat před nasazením do Azure, můžete spustit projekt webového rozhraní API místně, přejít na adresu URL, která poskytuje soubor dat JSON pro Swagger, uložit soubor a potom použít možnost **Vybrat existující soubor metadat Swagger**.

9. V dialogovém okně **Přidat klienta REST API** klikněte na **OK**.

    Visual Studio vytvoří složku pojmenovanou podle aplikace API a vygeneruje třídy klienta.

    ![Soubory kódu pro generovaného klienta](./media/app-service-api-dotnet-get-started/codegenfiles.png)

5. V projektu ToDoListAPI otevřete soubor *Controllers\ToDoListController.cs*, abyste zobrazili kód, který pomocí generovaného klienta volá rozhraní API. 

    Následující fragment kódu ukazuje vytvoření instance objektu klienta a volání metody Get.

        private static ToDoListDataAPI NewDataAPIClient()
        {
            var client = new ToDoListDataAPI(new Uri(ConfigurationManager.AppSettings["toDoListDataAPIURL"]));
            return client;
        }
        
        public async Task<IEnumerable<ToDoItem>> Get()
        {
            using (var client = NewDataAPIClient())
            {
                var results = await client.ToDoList.GetByOwnerAsync(owner);
                return results.Select(m => new ToDoItem
                {
                    Description = m.Description,
                    ID = (int)m.ID,
                    Owner = m.Owner
                });
            }
        }

    Parametr konstruktoru získá adresu URL koncového bodu z nastavení aplikace `toDoListDataAPIURL`. V souboru Web.config je tato hodnota nastavena na místní adresu URL služby IIS Express projektu rozhraní API, abyste mohli aplikaci spouštět místně. Pokud vynecháte parametr konstruktoru, bude výchozím koncovým bodem adresa URL, ze které jste kód vygenerovali.

6. Třída klienta se vygeneruje s jiným názvem na základě názvu vaší aplikace API; změňte kód v souboru *Controllers\ToDoListController.cs* tak, aby název typu odpovídal tomu, co bylo vygenerováno v projektu. Pokud jste například aplikaci API nazvali ToDoListDataAPI071316, pak byste tento kód:

        private static ToDoListDataAPI NewDataAPIClient()
        {
            var client = new ToDoListDataAPI(new Uri(ConfigurationManager.AppSettings["toDoListDataAPIURL"]));

měli změnit na:

        private static ToDoListDataAPI071316 NewDataAPIClient()
        {
            var client = new ToDoListDataAPI071316(new Uri(ConfigurationManager.AppSettings["toDoListDataAPIURL"]));


## Vytvoření aplikace API k hostování střední vrstvy

V předchozích krocích jste [vytvořili aplikaci API datové vrstvy a nasadili do ní kód](#createapiapp).  Teď provedete stejný postup pro aplikaci API střední vrstvy. 

1. V **Průzkumníku řešení** klikněte pravým tlačítkem na projekt ToDoListAPI střední vrstvy (ne na datovou vrstvu ToDoListDataAPI) a potom klikněte na **Publikovat**.

    ![Kliknutí na Publikovat v nástroji Visual Studio](./media/app-service-api-dotnet-get-started/pubinmenu2.png)

3.  Na kartě **Profil** průvodce **Publikovat web** klikněte na **Microsoft Azure App Service**.

5. V dialogovém okně **App Service** klikněte na **Nová**.

3. Na kartě **Hosting** v dialogovém okně **Vytvořit službu App Service** přijměte výchozí **název aplikace API** nebo zadejte název, který bude v doméně *azurewebsites.net* jedinečný. 

5. Zvolte **předplatné** Azure, které jste dosud používali.

6. V rozevíracím seznamu **Skupina prostředků** zvolte stejnou skupinu prostředků, kterou jste vytvořili dříve.

4. V rozevíracím seznamu **Plán služby App Service** vyberte stejný plán, který jste vytvořili dříve. Tato hodnota se nastaví jako výchozí. 

7. Klikněte na možnost **Vytvořit**.

    Visual Studio vytvoří aplikaci API, vytvoří pro ni profil publikování a zobrazí krok **Připojení** průvodce **Publikovat web**.

3.  V kroku **Připojení** průvodce **Publikovat web** klikněte na **Publikovat**.

    Visual Studio nasadí projekt ToDoListAPI do nové aplikace API a otevře v prohlížeči adresu URL této aplikace API. Zobrazí se stránka úspěšného vytvoření.

## Konfigurace střední vrstvy pro volání datové vrstvy

Pokud jste právě volali aplikaci API střední vrstvy, pokusí se tato aplikace zavolat datovou vrstvu pomocí adresy URL místního hostitele, která je stále v souboru Web.config. V této části zadejte adresu URL aplikace API datové vrstvy do nastavení prostředí v aplikaci API střední vrstvy. Když kód v aplikaci API střední vrstvy načte nastavení adresy URL datové vrstvy, přepíše nastavení prostředí údaje v souboru Web.config. 
 
1. Přejděte na [Portál Azure](https://portal.azure.com/) a otevřete okno **Aplikace API** pro aplikaci, kterou jste vytvořili k hostování projektu TodoListAPI (střední vrstva).

2. V okně **Nastavení** aplikaci API klikněte na **Nastavení aplikace**.
 
4. V okně **Nastavení aplikace** této aplikace API přejděte dolů do části **Nastavení aplikace** a přidejte následující klíč a hodnotu:

  	| **Klíč** | toDoListDataAPIURL |
  	|---|---|
  	| **Hodnota** | https://{název vaší aplikace API datové vrstvy}.azurewebsites.net |
  	| **Příklad** | https://todolistdataapi.azurewebsites.net |

4. Klikněte na **Uložit**.

    ![Kliknutí na Uložit v Nastavení aplikace](./media/app-service-api-dotnet-get-started/asinportal.png)

    Při spuštění kódu v Azure tato hodnota přepíše adresu URL místního hostitele, která je v souboru Web.config. 

## Test

11. V okně prohlížeče přejděte na adresu URL nové aplikace API střední vrstvy, kterou jste právě vytvořili pro projekt ToDoListAPI. Můžete se tam dostat kliknutím na adresu URL v hlavním okně aplikace API na portálu.

13. Na konec adresy URL v adresním řádku prohlížeče doplňte „swagger“ a stiskněte Enter. (Adresa URL je `http://{apiappname}.azurewebsites.net/swagger`.)

    Prohlížeč zobrazí stejné uživatelské rozhraní Swagger, které jste předtím viděli pro projekt ToDoListDataAPI, s tím rozdílem, že pole `owner` není pro operaci Get povinné, protože aplikace API střední vrstvy tuto hodnotu odesílá do aplikace API datové vrstvy za vás. (Během postupu v kurzech ověřování odešle střední vrstva skutečné ID uživatele pro parametr `owner`; zatím je toto ID pevně kódováno pomocí hvězdičky.)

12. Vyzkoušejte metodu Get a další metody, a ověřte tak, že aplikace API střední vrstvy úspěšně volá aplikaci API datové vrstvy.

    ![Metoda Get uživatelského rozhraní Swagger](./media/app-service-api-dotnet-get-started/midtierget.png)

## Řešení potíží

V případě, že při procházení tohoto kurzu narazíte na problém, můžete ho zkusit vyřešit pomocí následujících kroků:

* Zkontrolujte, že používáte nejnovější verzi [sady Azure SDK pro .NET](http://go.microsoft.com/fwlink/?linkid=518003).

* Dva názvy projektů jsou podobné (ToDoListAPI, ToDoListDataAPI). Pokud vaše prostředí při práci s projektem vypadá jinak než v těchto pokynech, zkontrolujte, že máte otevřený správný projekt.

* Pokud jste v podnikové síti a pokoušíte se provést nasazení do služby Azure App Service přes bránu firewall, zkontrolujte, že jsou pro nasazení webu otevřené porty 443 a 8172. Pokud tyto porty nedají otevřít, můžete použít jiné metody nasazení.  Viz [Nasazení vaší aplikace do Azure App Service](../app-service-web/web-sites-deploy.md).

* Chyby „Názvy tras musí být jedinečné“ – k těmto chybám může dojít, pokud do aplikace API neúmyslně nasadíte nesprávný projekt a později nasadíte správný. Pokud chcete vyřešit tento problém, nasaďte znovu správný projekt do aplikace API a na kartě **Nastavení** v průvodci **Publikovat web** vyberte **Odebrat další soubory v cíli**.

Jakmile vaše aplikace API technologie ASP.NET poběží v Azure App Service, můžete zjistit další informace o funkcích nástroje Visual Studio, které usnadňují řešení potíží. Informace o protokolování, vzdáleném ladění apod. najdete v tématu [Řešení potíží s aplikacemi Azure App Service v nástroji Visual Studio](../app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md).

## Další kroky

Už víte, jak nasazovat existující projekty webového rozhraní API do aplikací API, jak generovat kód klienta pro aplikace API a jak využívat aplikace API z klientů .NET. V dalším kurzu této série se dozvíte, jak [pomocí CORS využívat aplikace API z klientů JavaScript](app-service-api-cors-consume-javascript.md).
 
Další informace o generování kódu klienta najdete v úložišti [Azure/AutoRest](https://github.com/azure/autorest) na webu GitHub.com. Nápovědu v případě potíží s používáním generovaného klienta získáte otevřením [problému v úložišti AutoRest](https://github.com/azure/autorest/issues).

Pokud chcete vytvořit nový projekt aplikace API od začátku, použijte šablonu **aplikace API v Azure**.

![Šablona aplikace API v nástroji Visual Studio](./media/app-service-api-dotnet-get-started/apiapptemplate.png)

Šablona projektu **aplikace API Azure** je ekvivalentem možnosti vybrání **prázdné** šablony ASP.NET 4.5.2, zaškrtnutí políčka pro přidání podpory webového rozhraní API a nainstalování balíčku NuGet Swashbuckle. Kromě toho přidá šablona určitý kód konfigurace Swashbuckle, který bude bránit vytvoření duplicitních ID operací Swagger. Po vytvoření projektu aplikace API můžete tento projekt nasadit do aplikace API stejným způsobem, jaký už znáte z tohoto kurzu.



<!--HONumber=Aug16_HO4-->


