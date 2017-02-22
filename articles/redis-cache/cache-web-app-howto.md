---
title: "Vytvoření webové aplikace s Redis Cache | Dokumentace Microsoftu"
description: "Zjistěte, jak vytvořit webovou aplikaci s Redis Cache"
services: redis-cache
documentationcenter: 
author: steved0x
manager: douge
editor: 
ms.assetid: 454e23d7-a99b-4e6e-8dd7-156451d2da7c
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache-redis
ms.devlang: na
ms.topic: hero-article
ms.date: 01/27/2017
ms.author: sdanie
translationtype: Human Translation
ms.sourcegitcommit: 8d1b9293a0b3958d0f478b6a0b6816b8d534883d
ms.openlocfilehash: d7e98ef1205f0d88e12779a4ce9317128ae81e73


---
# <a name="how-to-create-a-web-app-with-redis-cache"></a>Vytvoření webové aplikace s Redis Cache
> [!div class="op_single_selector"]
> * [.NET](cache-dotnet-how-to-use-azure-redis-cache.md)
> * [ASP.NET](cache-web-app-howto.md)
> * [Node.js](cache-nodejs-get-started.md)
> * [Java](cache-java-get-started.md)
> * [Python](cache-python-get-started.md)
> 
> 

V tomto kurzu se dozvíte, jak vytvořit a nasadit webovou aplikaci ASP.NET do webové aplikace v Azure App Service pomocí sady Visual Studio 2015. Ukázková aplikace zobrazí seznam týmových statistik z databáze a ukáže vám různé způsoby použití Azure Redis Cache k ukládání a načítání dat z mezipaměti. Po dokončení kurzu budete mít funkční webovou aplikaci, která čte a zapisuje do databáze, je optimalizovaná pomocí Azure Redis Cache a je hostovaná v Azure.

Naučíte se:

* Jak vytvořit webovou aplikaci ASP.NET MVC 5 v sadě Visual Studio.
* Jak přistupovat k datům v databázi pomocí Entity Frameworku.
* Jak zlepšit propustnost dat a snížit zatížení databáze díky ukládání a načítání dat pomocí Azure Redis Cache.
* Jak použít seřazenou sadu Redis k načtení týmů v Top 5.
* Jak zřídit prostředky Azure pro aplikaci pomocí šablony Resource Manageru.
* Jak publikovat aplikaci do Azure pomocí sady Visual Studio.

## <a name="prerequisites"></a>Požadavky
K dokončení tohoto kurzu budete potřebovat:

* [Účet Azure](#azure-account)
* [Visual Studio 2015 se sadou Azure SDK pro .NET](#visual-studio-2015-with-the-azure-sdk-for-net)

### <a name="azure-account"></a>Účet Azure
K dokončení tohoto kurzu potřebujete mít účet Azure. Můžete:

* [Otevřít bezplatný účet Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=redis_cache_hero). Získáte kredity, které můžete použít k vyzkoušení placených služeb Azure. I po vyčerpání kreditů si můžete účet ponechat a používat bezplatné funkce a služby Azure.
* [Aktivovat výhody pro předplatitele sady Visual Studio](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=redis_cache_hero). Díky předplatnému MSDN každý měsíc získáváte kredity, které můžete použít pro placené služby Azure.

### <a name="visual-studio-2015-with-the-azure-sdk-for-net"></a>Sadu Visual Studio 2015 se sadou Azure SDK pro .NET
Tento kurz je napsán pro sadu Visual Studio 2015 se [sadou Azure SDK pro .NET](../dotnet-sdk.md)ve verzi 2.8.2 nebo novější. [Stáhněte si nejnovější verzi sady Azure SDK pro sadu Visual Studio 2015](http://go.microsoft.com/fwlink/?linkid=518003). Sada Visual Studio se automaticky nainstaluje se sadou SDK, pokud ji ještě nemáte.

Máte-li sadu Visual Studio 2013, můžete si [stáhnout nejnovější verzi Azure SDK pro sadu Visual Studio 2013](http://go.microsoft.com/fwlink/?LinkID=324322). Některé obrazovky se mohou lišit od ilustrací v tomto kurzu.

> [!NOTE]
> V závislosti na počtu závislostí sady SDK, které už na počítači máte, může instalace sady SDK trvat delší dobu – od několika minut až po půl hodiny nebo i déle.
> 
> 

## <a name="create-the-visual-studio-project"></a>Vytvoření projektu sady Visual Studio
1. Otevřete sadu Visual Studio a klikněte na **Soubor**, **Nový**, **Projekt**.
2. Rozbalte uzel **Visual C#** v seznamu **Šablony**, vyberte **Cloud**, a klikněte na **Webová aplikace .NET**. Ujistěte se, že je vybrán **.NET Framework 4.5.2** nebo vyšší.  Do textového pole **Název** zadejte **ContosoTeamStates** a klikněte na **OK**.
   
    ![Vytvoření projektu][cache-create-project]
3. Jako typ projektu vyberte **MVC**. Zrušte zaškrtnutí políčka **Hostovat v cloudu**. [Prostředky Azure zřídíte](#provision-the-azure-resources) a [aplikaci do Azure publikujete](#publish-the-application-to-azure) v dalších krocích tohoto kurzu. Příklad zřízení webové aplikace App Service ze sady Visual Studio se zaškrtnutým políčkem **Hostovat v cloudu** najdete v tématu [Začínáme s Web Apps v Azure App Service pomocí ASP.NET a sady Visual Studio](../app-service-web/web-sites-dotnet-get-started.md).
   
    ![Výběr šablony projektu][cache-select-template]
4. Projekt vytvoříte kliknutím na **OK**.

## <a name="create-the-aspnet-mvc-application"></a>Vytvoření aplikace ASP.NET MVC
V této části kurzu vytvoříte základní aplikaci, která načítá a zobrazuje týmové statistiky z databáze.

* [Přidání modelu](#add-the-model)
* [Přidání kontroleru](#add-the-controller)
* [Konfigurace zobrazení](#configure-the-views)

### <a name="add-the-model"></a>Přidání modelu
1. V **Průzkumníku řešení** klikněte pravým tlačítkem na **Modely** a vyberte **Přidat**, **Třídu**. 
   
    ![Přidání modelu][cache-model-add-class]
2. Jako název třídy zadejte `Team` a klikněte na **Přidat**.
   
    ![Přidání třídy modelu][cache-model-add-class-dialog]
3. Nahraďte příkazy `using` v horní části souboru `Team.cs` za následující příkazy `using`.

    ```c#
    using System;
    using System.Collections.Generic;
    using System.Data.Entity;
    using System.Data.Entity.SqlServer;
    ```


1. Nahraďte definici třídy `Team` za následující fragment kódu, který obsahuje kromě aktualizované definice třídy `Team` i některé další pomocné třídy Entity Frameworku. Další informace o přístupu Code First k Entity Frameworku, který používáme v tomto kurzu, najdete v tématu [Code First pro novou databázi](https://msdn.microsoft.com/data/jj193542).

    ```c#
    public class Team
    {
        public int ID { get; set; }
        public string Name { get; set; }
        public int Wins { get; set; }
        public int Losses { get; set; }
        public int Ties { get; set; }
    
        static public void PlayGames(IEnumerable<Team> teams)
        {
            // Simple random generation of statistics.
            Random r = new Random();
    
            foreach (var t in teams)
            {
                t.Wins = r.Next(33);
                t.Losses = r.Next(33);
                t.Ties = r.Next(0, 5);
            }
        }
    }
    
    public class TeamContext : DbContext
    {
        public TeamContext()
            : base("TeamContext")
        {
        }
    
        public DbSet<Team> Teams { get; set; }
    }
    
    public class TeamInitializer : CreateDatabaseIfNotExists<TeamContext>
    {
        protected override void Seed(TeamContext context)
        {
            var teams = new List<Team>
            {
                new Team{Name="Adventure Works Cycles"},
                new Team{Name="Alpine Ski House"},
                new Team{Name="Blue Yonder Airlines"},
                new Team{Name="Coho Vineyard"},
                new Team{Name="Contoso, Ltd."},
                new Team{Name="Fabrikam, Inc."},
                new Team{Name="Lucerne Publishing"},
                new Team{Name="Northwind Traders"},
                new Team{Name="Consolidated Messenger"},
                new Team{Name="Fourth Coffee"},
                new Team{Name="Graphic Design Institute"},
                new Team{Name="Nod Publishers"}
            };
    
            Team.PlayGames(teams);
    
            teams.ForEach(t => context.Teams.Add(t));
            context.SaveChanges();
        }
    }
    
    public class TeamConfiguration : DbConfiguration
    {
        public TeamConfiguration()
        {
            SetExecutionStrategy("System.Data.SqlClient", () => new SqlAzureExecutionStrategy());
        }
    }
    ```


1. V **Průzkumníku řešení** dvakrát klikněte na soubor **web.config** a otevřete jej.
   
    ![Soubor web.config][cache-web-config]
2. Přidejte následující připojovací řetězec do oddílu `connectionStrings`. Název připojovacího řetězce se musí shodovat s názvem třídy kontextu databáze v Entity Frameworku, který je `TeamContext`.

    ```xml   
    <add name="TeamContext" connectionString="Data Source=(LocalDB)\v11.0;AttachDbFilename=|DataDirectory|\Teams.mdf;Integrated Security=True" providerName="System.Data.SqlClient" />
    ```

    Po přidání tohoto řádku kódu by měl oddíl `connectionStrings` vypadat podobně jako v následujícím příkladu.

    ```xml
    <connectionStrings>
        <add name="DefaultConnection" connectionString="Data Source=(LocalDb)\MSSQLLocalDB;AttachDbFilename=|DataDirectory|\aspnet-ContosoTeamStats-20160216120918.mdf;Initial Catalog=aspnet-ContosoTeamStats-20160216120918;Integrated Security=True"
            providerName="System.Data.SqlClient" />
        <add name="TeamContext" connectionString="Data Source=(LocalDB)\v11.0;AttachDbFilename=|DataDirectory|\Teams.mdf;Integrated Security=True"     providerName="System.Data.SqlClient" />
    </connectionStrings>
    ```

### <a name="add-the-controller"></a>Přidání kontroleru
1. Projekt sestavíte stisknutím klávesy **F6**. 
2. V **Průzkumníku řešení** klikněte pravým tlačítkem na složku **Kontrolery** a vyberte **Přidat**, **Kontroler**.
   
    ![Přidání kontroleru][cache-add-controller]
3. Vyberte **Kontroler MVC 5 se zobrazeními, s použitím Entity Frameworku**, a klikněte na **Přidat**. Pokud po kliknutí na **Přidat** dojde k chybě, ujistěte se, že jste nejdříve sestavili projekt.
   
    ![Přidání třídy kontroleru][cache-add-controller-class]
4. Vyberte **Team (ContosoTeamStats.Models)** z rozevíracího seznamu **Třída modelu**. Vyberte **TeamContext (ContosoTeamStats.Models)** z rozevíracího seznamu **Třída kontextu dat**. Zadejte `TeamsController` do textového pole **Název kontroleru** (pokud není vyplněné automaticky). Kliknutím na **Přidat** vytvoříte třídu kontroleru a přidáte výchozí zobrazení.
   
    ![Konfigurace kontroleru][cache-configure-controller]
5. V **Průzkumníku řešení** rozbalte **Global.asax** a dvojím kliknutím otevřete soubor **Global.asax.cs**.
   
    ![Soubor Global.asax.cs][cache-global-asax]
6. Přidejte následující dva příkazy `using` do horní části souboru pod ostatní příkazy `using`.

    ```c#
    using System.Data.Entity;
    using ContosoTeamStats.Models;
    ```


1. Přidejte následující řádek kódu na konec metody `Application_Start`.

    ```c#
    Database.SetInitializer<TeamContext>(new TeamInitializer());
    ```


1. V **Průzkumníku řešení** rozbalte `App_Start` a dvakrát klikněte na soubor `RouteConfig.cs`.
   
    ![Soubor RouteConfig.cs][cache-RouteConfig-cs]
2. Nahraďte `controller = "Home"` v následujícím kódu v metodě `RegisterRoutes` za `controller = "Teams"`, jak je znázorněno v následujícím příkladu.

    ```c#
    routes.MapRoute(
        name: "Default",
        url: "{controller}/{action}/{id}",
        defaults: new { controller = "Teams", action = "Index", id = UrlParameter.Optional }
    );
```


### <a name="configure-the-views"></a>Konfigurace zobrazení
1. V **Průzkumníku řešení** rozbalte složku **Zobrazení**, poté složku **Sdílené**, a dvakrát klikněte na soubor **_Layout.cshtml**. 
   
    ![Soubor _Layout.cshtml][cache-layout-cshtml]
2. Změňte obsah elementu `title` a nahraďte `My ASP.NET Application` za `Contoso Team Stats`, jak je znázorněno v následujícím příkladu.

    ```html
    <title>@ViewBag.Title - Contoso Team Stats</title>
    ```


1. V oddílu `body` aktualizujte první příkaz `Html.ActionLink` a nahraďte hodnoty `Application name` za `Contoso Team Stats` a `Home` za `Teams`.
   
   * Před: `@Html.ActionLink("Application name", "Index", "Home", new { area = "" }, new { @class = "navbar-brand" })`
   * Po: `@Html.ActionLink("Contoso Team Stats", "Index", "Teams", new { area = "" }, new { @class = "navbar-brand" })`
     
     ![Změny kódu][cache-layout-cshtml-code]
2. Stisknutím kombinace kláves **Ctrl+F5** sestavíte a spustíte aplikaci. Tato verze aplikace načítá výsledky přímo z databáze. Všimněte si akcí **Vytvořit nový**, **Upravit**, **Podrobnosti** a **Odstranit**, které do aplikace automaticky přidalo vygenerované uživatelské rozhraní **Kontroler MVC 5 se zobrazeními, s použitím Entity Frameworku**. V další části kurzu přidáte mezipaměť Redis Cache pro optimalizování přístupu k datům a poskytnutí dodatečných funkcí aplikaci.

![Startovní aplikace][cache-starter-application]

## <a name="configure-the-application-to-use-redis-cache"></a>Konfigurace aplikace pro používání Redis Cache
V této části kurzu nakonfigurujete ukázkovou aplikaci pro ukládání a načítání týmových statistik Contoso z instance služby Azure Redis Cache pomocí klienta mezipaměti [StackExchange.Redis](https://github.com/StackExchange/StackExchange.Redis).

* [Konfigurace aplikace pro používání StackExchange.Redis](#configure-the-application-to-use-stackexchangeredis)
* [Aktualizace třídy TeamsController pro vracení výsledků z mezipaměti nebo z databáze](#update-the-teamscontroller-class-to-return-results-from-the-cache-or-the-database)
* [Aktualizace metod Create, Edit a Delete pro práci s mezipamětí](#update-the-create-edit-and-delete-methods-to-work-with-the-cache)
* [Aktualizace zobrazení Teams Index pro práci s mezipamětí](#update-the-teams-index-view-to-work-with-the-cache)

### <a name="configure-the-application-to-use-stackexchangeredis"></a>Konfigurace aplikace pro používání StackExchange.Redis
1. Chcete-li konfigurovat klientskou aplikaci v sadě Visual Studio pomocí balíčku StackExchange.Redis NuGet, klikněte pravým tlačítkem na projekt v **Průzkumníku řešení** a vyberte**Správa balíčků NuGet**. 
   
    ![Správa balíčků NuGet][redis-cache-manage-nuget-menu]
2. Do textového pole pro vyhledávání zadejte **StackExchange.Redis**, vyberte z výsledků požadovanou verzi, a klikněte na **Instalovat**.
   
    ![Balíček StackExchange.Redis NuGet][redis-cache-stack-exchange-nuget]
   
    Balíček NuGet se stáhne a přidá klientským aplikacím požadované odkazy na sestavení pro přístup do Azure Redis Cache pomocí klienta mezipaměti StackExchange.Redis. Pokud upřednostňujete použití verze klientské knihovny **StackExchange.Redis** se silným názvem, vyberte **StackExchange.Redis.StrongName**, jinak vyberte **StackExchange.Redis**.
3. V **Průzkumníku řešení** rozbalte složku **Kontrolery** a dvojím kliknutím otevřete soubor **TeamsController.cs**.
   
    ![Kontroler Teams][cache-teamscontroller]
4. Přidejte následující dva příkazy `using` do souboru **TeamsController.cs**.

    ```c#   
    using System.Configuration;
    using StackExchange.Redis;
    ```

5. Přidejte následující dvě vlastnosti do třídy `TeamsController`.

    ```c#   
    // Redis Connection string info
    private static Lazy<ConnectionMultiplexer> lazyConnection = new Lazy<ConnectionMultiplexer>(() =>
    {
        string cacheConnection = ConfigurationManager.AppSettings["CacheConnection"].ToString();
        return ConnectionMultiplexer.Connect(cacheConnection);
    });
    
    public static ConnectionMultiplexer Connection
    {
        get
        {
            return lazyConnection.Value;
        }
    }
    ```

6. Na svém počítači vytvořte soubor s názvem `WebAppPlusCacheAppSecrets.config` a umístěte jej do složky, která nebude registrována zároveň se zdrojovým kódem ukázkové aplikace v případě, že se ji rozhodnete někam registrovat. V tomto příkladu je soubor `AppSettingsSecrets.config` umístěn v `C:\AppSecrets\WebAppPlusCacheAppSecrets.config`.
   
    Upravte soubor `WebAppPlusCacheAppSecrets.config` a přidejte následující obsah. Pokud aplikaci spouštíte místně, tyto údaje se použijí pro připojení k vaší instanci služby Azure Redis Cache. Později v tomto kurzu zřídíte instanci služby Azure Redis Cache a aktualizujete název mezipaměti a heslo. Pokud neplánujete spouštět ukázkovou aplikaci místně, můžete přeskočit vytvoření tohoto souboru a další kroky, které se tohoto souboru týkají. Při nasazení do Azure aplikace získá informace o připojení k mezipaměti z nastavení webové aplikace, a ne z tohoto souboru. Vzhledem k tomu, že se soubor `WebAppPlusCacheAppSecrets.config` nenasazuje do Azure společně s aplikací, nebudete ho potřebovat, pokud plánujete spouštět aplikaci pouze místně.

    ```xml
    <appSettings>
      <add key="CacheConnection" value="MyCache.redis.cache.windows.net,abortConnect=false,ssl=true,password=..."/>
    </appSettings>
    ```


1. V **Průzkumníku řešení** dvakrát klikněte na soubor **web.config** a otevřete jej.
   
    ![Soubor web.config][cache-web-config]
2. Přidejte následující atribut `file` do elementu `appSettings`. Pokud jste použili jiný název souboru nebo umístění, nahraďte těmito hodnotami hodnoty v příkladu.
   
   * Před: `<appSettings>`
   * Po: ` <appSettings file="C:\AppSecrets\WebAppPlusCacheAppSecrets.config">`
     
   Modul runtime ASP.NET sloučí obsah externího souboru se značkami v elementu `<appSettings>`. Pokud zadaný soubor nelze nalézt, modul runtime atribut souboru ignoruje. Vaše tajné kódy (připojovací řetězce k mezipaměti) nejsou součástí zdrojového kódu aplikace. Při nasazení vaší webové aplikace do Azure se soubor `WebAppPlusCacheAppSecrests.config` nenasadí (přesně to chcete). Existuje několik způsobů určení těchto tajných kódů v Azure – v tomto kurzu jsou pro vás automaticky nakonfigurované během [zřízení prostředků Azure](#provision-the-azure-resources) v dalším kroku kurzu. Další informace o práci s tajnými kódy v Azure najdete v tématu [Osvědčené postupy pro nasazování hesel a dalších citlivých dat do ASP.NET a do Azure App Service](http://www.asp.net/identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure).

### <a name="update-the-teamscontroller-class-to-return-results-from-the-cache-or-the-database"></a>Aktualizace třídy TeamsController pro vracení výsledků z mezipaměti nebo z databáze
V této ukázce lze týmové statistiky získat z databáze nebo z mezipaměti. Týmové statistiky jsou v mezipaměti uložené pomocí datových typů Redis jako serializovaný seznam `List<Team>`, a také jako seřazená sada. Při načítání položek ze seřazené sady můžete načíst část položek, všechny položky nebo provézt dotaz na určité položky. V této ukázce provedete na seřazenou sadu dotaz pro získání Top 5 týmů podle počtu vítězství.

> [!NOTE]
> Pro používání Azure Redis Cache není nutné v mezipaměti ukládat týmové statistiky ve více formátech. V tomto kurzu používáme více formátů, abychom předvedli různé způsoby a datové typy, které můžete použít pro ukládání dat do mezipaměti.
> 
> 

1. Přidejte následující příkazy `using` do horní části souboru `TeamsController.cs` k ostatním příkazům `using`.

    ```c#   
    using System.Diagnostics;
    using Newtonsoft.Json;
    ```

2. Nahraďte stávající implementaci metody `public ActionResult Index()` za následující implementaci.

    ```c#
    // GET: Teams
    public ActionResult Index(string actionType, string resultType)
    {
        List<Team> teams = null;

        switch(actionType)
        {
            case "playGames": // Play a new season of games.
                PlayGames();
                break;

            case "clearCache": // Clear the results from the cache.
                ClearCachedTeams();
                break;

            case "rebuildDB": // Rebuild the database with sample data.
                RebuildDB();
                break;
        }

        // Measure the time it takes to retrieve the results.
        Stopwatch sw = Stopwatch.StartNew();

        switch(resultType)
        {
            case "teamsSortedSet": // Retrieve teams from sorted set.
                teams = GetFromSortedSet();
                break;

            case "teamsSortedSetTop5": // Retrieve the top 5 teams from the sorted set.
                teams = GetFromSortedSetTop5();
                break;

            case "teamsList": // Retrieve teams from the cached List<Team>.
                teams = GetFromList();
                break;

            case "fromDB": // Retrieve results from the database.
            default:
                teams = GetFromDB();
                break;
        }

        sw.Stop();
        double ms = sw.ElapsedTicks / (Stopwatch.Frequency / (1000.0));

        // Add the elapsed time of the operation to the ViewBag.msg.
        ViewBag.msg += " MS: " + ms.ToString();

        return View(teams);
    }
    ```


1. Přidejte následující tři metody do třídy `TeamsController` pro implementaci typů akcí `playGames`, `clearCache` a `rebuildDB` z výrazu switch, který jste přidali v předchozím fragmentu kódu.
   
    Metoda `PlayGames` aktualizuje týmové statistiky tak, že simuluje sezónu her, výsledky uloží do databáze, a vymaže již zastaralá data z mezipaměti.

    ```c#
    void PlayGames()
    {
        ViewBag.msg += "Updating team statistics. ";
        // Play a "season" of games.
        var teams = from t in db.Teams
                    select t;

        Team.PlayGames(teams);

        db.SaveChanges();

        // Clear any cached results
        ClearCachedTeams();
    }
    ```

    Metoda `RebuildDB` znovu inicializuje databázi s výchozí sadou týmů, vygeneruje pro ně statistiky, a vymaže již zastaralá data z mezipaměti.

    ```c#
    void RebuildDB()
    {
        ViewBag.msg += "Rebuilding DB. ";
        // Delete and re-initialize the database with sample data.
        db.Database.Delete();
        db.Database.Initialize(true);

        // Clear any cached results
        ClearCachedTeams();
    }
    ```

    Metoda `ClearCachedTeams` odebere z mezipaměti všechny uložené týmové statistiky.

    ```c#
    void ClearCachedTeams()
    {
        IDatabase cache = Connection.GetDatabase();
        cache.KeyDelete("teamsList");
        cache.KeyDelete("teamsSortedSet");
        ViewBag.msg += "Team data removed from cache. ";
    } 
    ```


1. Přidejte následující čtyři metody do třídy `TeamsController` pro implementaci různých způsobů načítání týmových statistik z mezipaměti a z databáze. Každá z těchto metod vrací seznam `List<Team>`, který se následně zobrazí v zobrazení.
   
    Metoda `GetFromDB` načítá týmové statistiky z databáze.
   
    ```c#
    List<Team> GetFromDB()
    {
        ViewBag.msg += "Results read from DB. ";
        var results = from t in db.Teams
            orderby t.Wins descending
            select t; 

        return results.ToList<Team>();
    }
    ```

    Metoda `GetFromList` načítá týmové statistiky z mezipaměti jako serializovaný seznam `List<Team>`. Dojde-li k neúspěšnému přístupu do mezipaměti, týmové statistiky se načtou z databáze a uloží se do mezipaměti pro další použití. V této ukázce používáme pro serializaci objektů .NET do a z mezipaměti serializaci JSON.NET. Další informace najdete v tématu [Práce s objekty .NET v Azure Redis Cache](cache-dotnet-how-to-use-azure-redis-cache.md#work-with-net-objects-in-the-cache).

    ```c#
    List<Team> GetFromList()
    {
        List<Team> teams = null;

        IDatabase cache = Connection.GetDatabase();
        string serializedTeams = cache.StringGet("teamsList");
        if (!String.IsNullOrEmpty(serializedTeams))
        {
            teams = JsonConvert.DeserializeObject<List<Team>>(serializedTeams);

            ViewBag.msg += "List read from cache. ";
        }
        else
        {
            ViewBag.msg += "Teams list cache miss. ";
            // Get from database and store in cache
            teams = GetFromDB();

            ViewBag.msg += "Storing results to cache. ";
            cache.StringSet("teamsList", JsonConvert.SerializeObject(teams));
        }
        return teams;
    }
    ```

    Metoda `GetFromSortedSet` načítá týmové statistiky ze seřazené sady v mezipaměti. Dojde-li k neúspěšnému přístupu do mezipaměti, týmové statistiky se načtou z databáze a uloží se do mezipaměti jako seřazená sada.

    ```c#
    List<Team> GetFromSortedSet()
    {
        List<Team> teams = null;
        IDatabase cache = Connection.GetDatabase();
        // If the key teamsSortedSet is not present, this method returns a 0 length collection.
        var teamsSortedSet = cache.SortedSetRangeByRankWithScores("teamsSortedSet", order: Order.Descending);
        if (teamsSortedSet.Count() > 0)
        {
            ViewBag.msg += "Reading sorted set from cache. ";
            teams = new List<Team>();
            foreach (var t in teamsSortedSet)
            {
                Team tt = JsonConvert.DeserializeObject<Team>(t.Element);
                teams.Add(tt);
            }
        }
        else
        {
            ViewBag.msg += "Teams sorted set cache miss. ";

            // Read from DB
            teams = GetFromDB();

            ViewBag.msg += "Storing results to cache. ";
            foreach (var t in teams)
            {
                Console.WriteLine("Adding to sorted set: {0} - {1}", t.Name, t.Wins);
                cache.SortedSetAdd("teamsSortedSet", JsonConvert.SerializeObject(t), t.Wins);
            }
        }
        return teams;
    }
    ```

    Metoda `GetFromSortedSetTop5` načítá Top 5 týmů ze seřazené sady v mezipaměti. Začne tím, že zjistí přítomnost klíče `teamsSortedSet` v mezipaměti. Pokud tento klíč neexistuje, zavolá se metoda `GetFromSortedSet`, která načte týmové statistiky a uloží je do mezipaměti. Poté se provede dotaz na seřazenou sadu k získání Top 5 týmů, které jsou následně vráceny.

    ```c#
    List<Team> GetFromSortedSetTop5()
    {
        List<Team> teams = null;
        IDatabase cache = Connection.GetDatabase();

        // If the key teamsSortedSet is not present, this method returns a 0 length collection.
        var teamsSortedSet = cache.SortedSetRangeByRankWithScores("teamsSortedSet", stop: 4, order: Order.Descending);
        if(teamsSortedSet.Count() == 0)
        {
            // Load the entire sorted set into the cache.
            GetFromSortedSet();

            // Retrieve the top 5 teams.
            teamsSortedSet = cache.SortedSetRangeByRankWithScores("teamsSortedSet", stop: 4, order: Order.Descending);
        }

        ViewBag.msg += "Retrieving top 5 teams from cache. ";
        // Get the top 5 teams from the sorted set
        teams = new List<Team>();
        foreach (var team in teamsSortedSet)
        {
            teams.Add(JsonConvert.DeserializeObject<Team>(team.Element));
        }
        return teams;
    }
    ```

### <a name="update-the-create-edit-and-delete-methods-to-work-with-the-cache"></a>Aktualizace metod Create, Edit a Delete pro práci s mezipamětí
Kód generování uživatelského rozhraní vygenerovaný jako součást této ukázky obsahuje metody pro přidávání, upravování a odstraňování týmů. Pokaždé když dojde k přidání, úpravě nebo odstranění týmu se data v mezipaměti stanou zastaralými. V této části upravíte tyto tři metody pro vymazávání týmů uložených v mezipaměti, aby nedocházelo k tomu, že mezipaměť nebude synchronizována s databází.

1. Přejděte k metodě `Create(Team team)` ve třídě `TeamsController`. Přidejte volání metody `ClearCachedTeams`, jak je znázorněno v následujícím příkladu.

    ```c#
    // POST: Teams/Create
    // To protect from overposting attacks, please enable the specific properties you want to bind to, for 
    // more details see http://go.microsoft.com/fwlink/?LinkId=317598.
    [HttpPost]
    [ValidateAntiForgeryToken]
    public ActionResult Create([Bind(Include = "ID,Name,Wins,Losses,Ties")] Team team)
    {
        if (ModelState.IsValid)
        {
            db.Teams.Add(team);
            db.SaveChanges();
            // When a team is added, the cache is out of date.
            // Clear the cached teams.
            ClearCachedTeams();
            return RedirectToAction("Index");
        }

        return View(team);
    }
    ```


1. Přejděte k metodě `Edit(Team team)` ve třídě `TeamsController`. Přidejte volání metody `ClearCachedTeams`, jak je znázorněno v následujícím příkladu.

    ```c#
    // POST: Teams/Edit/5
    // To protect from overposting attacks, please enable the specific properties you want to bind to, for 
    // more details see http://go.microsoft.com/fwlink/?LinkId=317598.
    [HttpPost]
    [ValidateAntiForgeryToken]
    public ActionResult Edit([Bind(Include = "ID,Name,Wins,Losses,Ties")] Team team)
    {
        if (ModelState.IsValid)
        {
            db.Entry(team).State = EntityState.Modified;
            db.SaveChanges();
            // When a team is edited, the cache is out of date.
            // Clear the cached teams.
            ClearCachedTeams();
            return RedirectToAction("Index");
        }
        return View(team);
    }
    ```


1. Přejděte k metodě `DeleteConfirmed(int id)` ve třídě `TeamsController`. Přidejte volání metody `ClearCachedTeams`, jak je znázorněno v následujícím příkladu.

    ```c#
    // POST: Teams/Delete/5
    [HttpPost, ActionName("Delete")]
    [ValidateAntiForgeryToken]
    public ActionResult DeleteConfirmed(int id)
    {
        Team team = db.Teams.Find(id);
        db.Teams.Remove(team);
        db.SaveChanges();
        // When a team is deleted, the cache is out of date.
        // Clear the cached teams.
        ClearCachedTeams();
        return RedirectToAction("Index");
    }
    ```


### <a name="update-the-teams-index-view-to-work-with-the-cache"></a>Aktualizace zobrazení Teams Index pro práci s mezipamětí
1. V **Průzkumníku řešení** rozbalte složku **Zobrazení**, poté složku **Týmy**, a dvakrát klikněte na soubor **Index.cshtml**.
   
    ![Soubor Index.cshtml][cache-views-teams-index-cshtml]
2. V horní části souboru vyhledejte následující element odstavce.
   
    ![Tabulka akcí][cache-teams-index-table]
   
    Toto je odkaz pro vytvoření nového týmu. Nahraďte element odstavce za následující tabulku. Tato tabulka obsahuje odkazy pro vytvoření nového týmu, hraní nové sezóny her, vymazání mezipaměti, načtení týmů z mezipaměti v různých formátech, načtení týmů z databáze a nové sestavení databáze s novými vzorovými daty.

    ```html
    <table class="table">
        <tr>
            <td>
                @Html.ActionLink("Create New", "Create")
            </td>
            <td>
                @Html.ActionLink("Play Season", "Index", new { actionType = "playGames" })
            </td>
            <td>
                @Html.ActionLink("Clear Cache", "Index", new { actionType = "clearCache" })
            </td>
            <td>
                @Html.ActionLink("List from Cache", "Index", new { resultType = "teamsList" })
            </td>
            <td>
                @Html.ActionLink("Sorted Set from Cache", "Index", new { resultType = "teamsSortedSet" })
            </td>
            <td>
                @Html.ActionLink("Top 5 Teams from Cache", "Index", new { resultType = "teamsSortedSetTop5" })
            </td>
            <td>
                @Html.ActionLink("Load from DB", "Index", new { resultType = "fromDB" })
            </td>
            <td>
                @Html.ActionLink("Rebuild DB", "Index", new { actionType = "rebuildDB" })
            </td>
        </tr>    
    </table>
    ```


1. Přejděte do dolní části souboru **Index.cshtml** a přidejte následující element `tr` tak, aby byl posledním řádkem poslední tabulky v souboru.
   
    ```html
    <tr><td colspan="5">@ViewBag.Msg</td></tr>
    ```
   
    Tento řádek zobrazuje hodnotu `ViewBag.Msg` obsahující zprávu o stavu aktuální operace, která se spustí po kliknutí na jeden z odkazů na akce z předchozího kroku.   
   
    ![Zpráva o stavu][cache-status-message]
2. Projekt sestavíte stisknutím klávesy **F6**.

## <a name="provision-the-azure-resources"></a>Zřízení prostředků Azure
Abyste mohli hostovat svoji aplikaci v Azure, musíte nejdříve zřídit služby Azure, které vaše aplikace vyžaduje. Ukázková aplikace v tomto kurzu používá následující služby Azure.

* Azure Redis Cache
* Webová aplikace App Service
* Databáze SQL

Chcete-li tyto služby nasadit do nové nebo stávající skupiny prostředků, klikněte na následující tlačítko **Nasadit do Azure**.

[![Nasadit do Azure][deploybutton]](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-web-app-redis-cache-sql-database%2Fazuredeploy.json)

Toto tlačítko **Nasadit do Azure** používá šablonu [Vytvoření webové aplikace s Redis Cache a databází SQL](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-redis-cache-sql-database) [Rychlý start v Azure](https://github.com/Azure/azure-quickstart-templates) pro zřízení těchto služeb, nastavení připojovacího řetězce pro databázi SQL a nastavení aplikace pro připojovací řetězec Azure Redis Cache.

> [!NOTE]
> Pokud ještě nemáte účet Azure, můžete si během několika minut [zdarma vytvořit účet Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=redis_cache_hero).
> 
> 

Kliknutím na tlačítko **Nasadit do Azure** se dostanete na web Azure Portal a zahájí se proces vytváření prostředků popsaných v šabloně.

![Nasazení do Azure][cache-deploy-to-azure-step-1]

1. V oddílu **Základy** vyberte předplatné Azure, které se má použít, a vyberte stávající skupinu prostředků nebo vytvořte novou, a zadejte umístění skupiny prostředků.
2. V oddílu **Nastavení** zadejte název účtu správce (**ADMINISTRATORLOGIN** – nepoužívejte **admin**), heslo správce (**ADMINISTRATORLOGINPASSWORD**) a název databáze (**DATABASENAME**). Další parametry jsou nakonfigurovány pro plán hostování služby App Service úrovně Free a další nízkonákladové možnosti pro služby SQL Database a Azure Redis Cache, které nejsou součástí úrovně Free.

    ![Nasazení do Azure][cache-deploy-to-azure-step-2]

3. Po dokončení konfigurace požadovaných nastavení přejděte na konec stránky, přečtěte si podmínky a ujednání a zaškrtněte políčko **Souhlasím s podmínkami a ujednáními uvedenými nahoře**.
4. Pokud chcete začít zřizovat prostředky, klikněte na **Nákup**.

Chcete-li zobrazit průběh nasazování, klikněte na ikonu oznámení a na **Nasazení začalo**.

![Nasazení začalo][cache-deployment-started]

Stav nasazování můžete zobrazit v podokně **Microsoft.Template**.

![Nasazení do Azure][cache-deploy-to-azure-step-3]

Jakmile je zřizování dokončeno, můžete publikovat svoji aplikaci do Azure ze sady Visual Studio.

> [!NOTE]
> Všechny případné chyby během procesu zřizování jsou zobrazeny v podokně **Microsoft.Template**. Mezi běžné chyby patří příliš mnoho SQL Serverů nebo příliš mnoho plánů hostování služby App Service úrovně Free na předplatné. Vyřešte všechny chyby a restartujte proces kliknutím na **Znovu nasadit** v podokně **Microsoft.Template** nebo na tlačítko **Nasadit do Azure** v tomto kurzu.
> 
> 

## <a name="publish-the-application-to-azure"></a>Publikování aplikace do Azure
V tomto kroku kurzu publikujete aplikaci do Azure a spustíte ji v cloudu.

1. V sadě Visual Studio klikněte pravým tlačítkem na projekt **ContosoTeamStats** a vyberte **Publikovat**.
   
    ![Publikování][cache-publish-app]
2. Klikněte na **Microsoft Azure App Service**.
   
    ![Publikování][cache-publish-to-app-service]
3. Vyberte předplatné použité při vytváření prostředků Azure, rozbalte skupinu prostředků obsahující prostředky, vyberte požadovanou webovou aplikaci, a klikněte na **OK**. Pokud jste použili tlačítko **Nasadit do Azure**, bude název vaší webové aplikace začínat na **webSite** a pokračovat nějakými dalšími znaky.
   
    ![Výběr webové aplikace][cache-select-web-app]
4. Ověřte své nastavení kliknutím na **Ověřit připojení**, a poté klikněte na **Publikovat**.
   
    ![Publikování][cache-publish]
   
    Po chvíli se proces publikování dokončí a spustí se prohlížeč s běžící ukázkovou aplikací. Obdržíte-li během ověřování nebo publikování chybu DNS a došlo-li právě k dokončení procesu zřizování prostředků Azure pro aplikaci, chvíli počkejte a zkuste to znovu.
   
    ![Mezipaměť byla přidána][cache-added-to-application]

Následující tabulka popisuje každý odkaz na akci v ukázkové aplikaci.

| Akce | Popis |
| --- | --- |
| Vytvořit nový |Vytvoření nového týmu. |
| Odehrát sezónu |Odehrání sezóny her, aktualizace týmových statistik a vymazání zastaralých dat z mezipaměti. |
| Vymazat mezipaměť |Vymazání týmových statistik z mezipaměti. |
| Seznam z mezipaměti |Načtení týmových statistik z mezipaměti. Dojde-li k neúspěšnému přístupu do mezipaměti, statistiky se načtou z databáze a uloží se do mezipaměti pro další použití. |
| Seřazená sada z mezipaměti |Načtení týmových statistik z mezipaměti pomocí seřazené sady. Dojde-li k neúspěšnému přístupu do mezipaměti, statistiky se načtou z databáze a uloží se do mezipaměti jako seřazená sada. |
| Top 5 týmů z mezipaměti |Načtení Top 5 týmů z mezipaměti pomocí seřazené sady. Dojde-li k neúspěšnému přístupu do mezipaměti, statistiky se načtou z databáze a uloží se do mezipaměti jako seřazená sada. |
| Načíst z databáze |Načtení týmových statistik z databáze. |
| Znovu sestavit databázi |Opětovné vytvoření databáze se vzorovými týmovými daty. |
| Upravit / Podrobnosti / Odstranit |Upravení týmu, zobrazení podrobností o týmu, odstranění týmu. |

Klikněte na některé akce a experimentujte s načítáním dat z různých zdrojů. Všimněte si rozdílů v době trvání různých způsobů načítání dat z databáze a mezipaměti.

## <a name="delete-the-resources-when-you-are-finished-with-the-application"></a>Odstranění prostředků po dokončení aplikace
Po dokončení ukázkové aplikace můžete odstranit použité prostředky Azure a uspořit tak náklady a prostředky. Pokud použijete tlačítko **Nasadit do Azure** v oddílu [Zřízení prostředků Azure](#provision-the-azure-resources) a všechny vaše prostředky jsou ve stejné skupině prostředků, můžete je odstranit všechny najednou tak, že odstraníte skupinu prostředků.

1. Přihlaste se na web [Azure Portal ](https://portal.azure.com) a klikněte na **Skupiny prostředků**.
2. Zadejte název vaší skupiny prostředků do textového pole **Filtrování položek...**.
3. Klikněte na **...** napravo od vaší skupiny prostředků.
4. Klikněte na **Odstranit**.
   
    ![Odstranění][cache-delete-resource-group]
5. Zadejte název vaší skupiny prostředků a klikněte na **Odstranit**.
   
    ![Potvrzení odstranění][cache-delete-confirm]

Za chvíli je skupina prostředků včetně všech obsažených prostředků odstraněna.

> [!IMPORTANT]
> Upozorňujeme, že odstranění skupiny prostředků je nevratné, a skupina prostředků včetně všech v ní obsažených prostředků bude trvale odstraněna. Ujistěte se, že nechtěně neodstraníte nesprávnou skupinu prostředků nebo prostředky. Pokud jste vytvořili prostředky pro hostování této ukázky ve stávající skupině prostředků, můžete odstranit jednotlivé prostředky z jejich odpovídajících podoken.
> 
> 

## <a name="run-the-sample-application-on-your-local-machine"></a>Spuštění ukázkové aplikace na místním počítači
Ke spuštění aplikace na místním počítači potřebujete mít instanci služby Azure Redis Cache, ve které budete ukládat data do mezipaměti. 

* Pokud jste svoji aplikaci publikovali do Azure podle postupu v předchozím oddílu, můžete použít instanci služby Azure Redis Cache zřízenou během tohoto kroku.
* Máte-li jinou stávající instanci služby Azure Redis Cache, můžete ji použít ke spuštění ukázky místně.
* Potřebujete-li vytvořit instanci služby Azure Redis Cache, postupujte podle pokynů v oddílu [Vytvoření mezipaměti](cache-dotnet-how-to-use-azure-redis-cache.md#create-a-cache).

Po vybrání nebo vytvoření mezipaměti, kterou chcete použít, přejděte do mezipaměti na webu Azure Portal a získejte [název hostitele](cache-configure.md#properties) a [přístupové klíče](cache-configure.md#access-keys) ke svojí mezipaměti. Pokyny najdete v oddílu [Konfigurace nastavení mezipaměti Redis](cache-configure.md#configure-redis-cache-settings).

1. Pomocí oblíbeného editoru otevřete soubor `WebAppPlusCacheAppSecrets.config`, který jste vytvořili během kroku [Konfigurace aplikace pro použití Redis Cache](#configure-the-application-to-use-redis-cache) v tomto kurzu.
2. Upravte atribut `value`, nahraďte `MyCache.redis.cache.windows.net` za [název hostitele](cache-configure.md#properties) vaší mezipaměti a jako heslo zadejte [primární, nebo sekundární klíč](cache-configure.md#access-keys) vaší mezipaměti.

    ```xml
    <appSettings>
      <add key="CacheConnection" value="MyCache.redis.cache.windows.net,abortConnect=false,ssl=true,password=..."/>
    </appSettings>
    ```


1. Aplikaci spustíte stisknutím kombinace kláves **Ctrl+F5**.

> [!NOTE]
> Vzhledem k tomu, že aplikace včetně databáze je spuštěna místně, a Redis Cache je hostovaná v Azure, se může zdát, že je mezipaměť méně výkonná než databáze.  Pro zajištění nejlepšího výkonu by klientská aplikace a instance služby Azure Redis Cache měly být ve stejném umístění. 
> 
> 

## <a name="next-steps"></a>Další kroky
* Další informace o [Začátcích s ASP.NET MVC 5](http://www.asp.net/mvc/overview/getting-started/introduction/getting-started) najdete na stránce [ASP.NET](http://asp.net/).
* Další příklady vytvoření webové aplikace ASP.NET ve službě App Service najdete v článku [Vytvoření a nasazení webové aplikace ve službě Azure App Service](https://github.com/Microsoft/HealthClinic.biz/wiki/Create-and-deploy-an-ASP.NET-web-app-in-Azure-App-Service) z [demoverze](https://blogs.msdn.microsoft.com/visualstudio/2015/12/08/connectdemos-2015-healthclinic-biz/) [HealthClinic.biz](https://github.com/Microsoft/HealthClinic.biz) představené na konferenci 2015 Connect.
  * Další rychlé starty z demoverze HealthClinic.biz najdete v článku [Rychlé starty nástrojů pro vývojáře Azure](https://github.com/Microsoft/HealthClinic.biz/wiki/Azure-Developer-Tools-Quickstarts).
* Zjistěte více o přístupu [Code First pro novou databázi](https://msdn.microsoft.com/data/jj193542) k Entity Frameworku, který používáme v tomto kurzu.
* Zjistěte více o [webových aplikacích v Azure App Service](../app-service-web/app-service-web-overview.md).
* Zjistěte, jak [monitorovat](cache-how-to-monitor.md) vaši mezipaměť na webu Azure Portal.
* Prozkoumejte prémiové funkce Azure Redis Cache
  
  * [Konfigurace trvalosti pro Azure Redis Cache ve verzi Premium](cache-how-to-premium-persistence.md)
  * [Konfigurace clusteringu pro Azure Redis Cache ve verzi Premium](cache-how-to-premium-clustering.md)
  * [Konfigurace podpory služby Virtual Network pro Azure Redis Cache ve verzi Premium](cache-how-to-premium-vnet.md)
  * Další podrobnosti o velikosti, propustnosti a šířce pásma u prémiových mezipamětí najdete v tématu [Azure Redis Cache – Nejčastější dotazy](cache-faq.md#what-redis-cache-offering-and-size-should-i-use).

<!-- IMAGES -->
[cache-starter-application]: ./media/cache-web-app-howto/cache-starter-application.png
[cache-added-to-application]: ./media/cache-web-app-howto/cache-added-to-application.png
[cache-create-project]: ./media/cache-web-app-howto/cache-create-project.png
[cache-select-template]: ./media/cache-web-app-howto/cache-select-template.png
[cache-model-add-class]: ./media/cache-web-app-howto/cache-model-add-class.png
[cache-model-add-class-dialog]: ./media/cache-web-app-howto/cache-model-add-class-dialog.png
[cache-add-controller]: ./media/cache-web-app-howto/cache-add-controller.png
[cache-add-controller-class]: ./media/cache-web-app-howto/cache-add-controller-class.png
[cache-configure-controller]: ./media/cache-web-app-howto/cache-configure-controller.png
[cache-global-asax]: ./media/cache-web-app-howto/cache-global-asax.png
[cache-RouteConfig-cs]: ./media/cache-web-app-howto/cache-RouteConfig-cs.png
[cache-layout-cshtml]: ./media/cache-web-app-howto/cache-layout-cshtml.png
[cache-layout-cshtml-code]: ./media/cache-web-app-howto/cache-layout-cshtml-code.png
[redis-cache-manage-nuget-menu]: ./media/cache-web-app-howto/redis-cache-manage-nuget-menu.png
[redis-cache-stack-exchange-nuget]: ./media/cache-web-app-howto/redis-cache-stack-exchange-nuget.png
[cache-teamscontroller]: ./media/cache-web-app-howto/cache-teamscontroller.png
[cache-web-config]: ./media/cache-web-app-howto/cache-web-config.png
[cache-views-teams-index-cshtml]: ./media/cache-web-app-howto/cache-views-teams-index-cshtml.png
[cache-teams-index-table]: ./media/cache-web-app-howto/cache-teams-index-table.png
[cache-status-message]: ./media/cache-web-app-howto/cache-status-message.png
[deploybutton]: ./media/cache-web-app-howto/deploybutton.png
[cache-deploy-to-azure-step-1]: ./media/cache-web-app-howto/cache-deploy-to-azure-step-1.png
[cache-deploy-to-azure-step-2]: ./media/cache-web-app-howto/cache-deploy-to-azure-step-2.png
[cache-deploy-to-azure-step-3]: ./media/cache-web-app-howto/cache-deploy-to-azure-step-3.png
[cache-deployment-started]: ./media/cache-web-app-howto/cache-deployment-started.png
[cache-publish-app]: ./media/cache-web-app-howto/cache-publish-app.png
[cache-publish-to-app-service]: ./media/cache-web-app-howto/cache-publish-to-app-service.png
[cache-select-web-app]: ./media/cache-web-app-howto/cache-select-web-app.png
[cache-publish]: ./media/cache-web-app-howto/cache-publish.png
[cache-delete-resource-group]: ./media/cache-web-app-howto/cache-delete-resource-group.png
[cache-delete-confirm]: ./media/cache-web-app-howto/cache-delete-confirm.png




<!--HONumber=Jan17_HO4-->


