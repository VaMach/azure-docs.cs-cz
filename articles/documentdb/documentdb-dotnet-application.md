---
title: "Kurz k ASP.NET MVC pro DocumentDB: Vývoj webových aplikací | Microsoft Docs"
description: "Kurz k ASP.NET MVC, v rámci kterého se vytvoří webová aplikace MVC pomocí DocumentDB. Budete ukládat JSON a přístupová data z aplikace seznamu úkolů hostované na Webech Azure – podrobný kurz ASP.NET MVC."
keywords: "kurz asp.net mvc, vývoj webových aplikací, aplikace mvc web, kurz asp net mvc krok za krokem"
services: documentdb
documentationcenter: .net
author: syamkmsft
manager: jhubbard
editor: cgronlun
ms.assetid: 52532d89-a40e-4fdf-9b38-aadb3a4cccbc
ms.service: documentdb
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 12/25/2016
ms.author: syamk
translationtype: Human Translation
ms.sourcegitcommit: 72b2d9142479f9ba0380c5bd2dd82734e370dee7
ms.openlocfilehash: 44307f258ea05635addf85bf9c59cd78b2ac0f1e
ms.lasthandoff: 03/08/2017


---
# <a name="_Toc395809351"></a>Kurz k ASP.NET MVC: Vývoj webových aplikací s DocumentDB| Microsoft Azure
> [!div class="op_single_selector"]
> * [.NET](documentdb-dotnet-application.md)
> * [.NET pro MongoDB](documentdb-mongodb-application.md)
> * [Node.js](documentdb-nodejs-application.md)
> * [Java](documentdb-java-application.md)
> * [Python](documentdb-python-application.md)
> 
> 

Pro větší názornost, jak lze pomocí Azure DocumentDB efektivně ukládat dokumenty JSON a zadávat na ně dotazy, obsahuje tento článek úplný podrobný návod, jak pomocí Azure DocumentDB vytvořit aplikaci pro seznam úkolů. Úkoly se budou ukládat jako dokumenty JSON do Azure DocumentDB.

![Snímek obrazovky webové aplikace MVC pro seznam úkolů vytvořené v tomto kurzu – podrobný kurz ASP.NET MVC](./media/documentdb-dotnet-application/asp-net-mvc-tutorial-image1.png)

Tento návod popisuje, jak pomocí služby DocumentDB, kterou poskytuje Azure, ukládat data a přistupovat k nim z webové aplikace ASP.NET MVC, která je hostována v Azure. Pokud hledáte kurz, který se zaměřuje jenom na DocumentDB, nikoli komponenty ASP.NET MVC, přečtěte si téma o [vytvoření konzolové aplikace DocumentDB v jazyce C#](documentdb-get-started.md).

> [!TIP]
> V tomto kurzu se předpokládá, že již máte zkušenosti s používáním ASP.NET MVC a Webů Azure. Pokud jsou pro vás technologie ASP.NET nebo [požadované nástroje](#_Toc395637760) nové, doporučujeme stáhnout úplný ukázkový projekt z [GitHubu][GitHub] a postupovat podle pokynů v této ukázce. Až jej budete mít sestavený, můžete se k tomuto článku vrátit, abyste kódu lépe porozuměli v kontextu projektu.
> 
> 

## <a name="_Toc395637760"></a>Předpoklady pro tento databázový kurz
Než budete postupovat podle pokynů tohoto článku, měli byste se ujistit, že máte následující:

* Aktivní účet Azure. Pokud účet nemáte, můžete si během několika minut vytvořit bezplatný zkušební účet. Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/). 

    NEBO

    Místní instalaci [emulátoru Azure DocumentDB](documentdb-nosql-local-emulator.md).
* [Visual Studio 2015](http://www.visualstudio.com/) nebo Visual Studio 2013 Update 4 nebo vyšší. Pokud používáte sadu Visual Studio 2013, budete muset nainstalovat [balíček nuget Microsoft.Net.Compilers](https://www.nuget.org/packages/Microsoft.Net.Compilers/) a přidat podporu pro C# 6.0. 
* Azure SDK pro rozhraní .NET verze 2.5.1 nebo vyšší, k dispozici prostřednictvím [instalace webové platformy Microsoft][Microsoft Web Platform Installer].

Všechny snímky obrazovky v tomto článku byly pořízeny pomocí nástroje Visual Studio 2013 s aktualizací Update 4 a sady Azure SDK pro .NET verze 2.5.1. Pokud konfigurace vašeho serveru využívá jiné verze, je možné, že se vaše obrazovky a možnosti budou mírně lišit, ale pokud splníte předpoklady uvedené výše, řešení by mělo fungovat.

## <a name="_Toc395637761"></a>Krok 1: Vytvoření databázového účtu DocumentDB
Začněme vytvořením účtu DocumentDB. Pokud již účet máte nebo pokud používáte pro účely tohoto kurzu emulátor DocumentDB, můžete přeskočit na [Vytvoření nové aplikace ASP.NET MVC](#_Toc395637762).

[!INCLUDE [documentdb-create-dbaccount](../../includes/documentdb-create-dbaccount.md)]

[!INCLUDE [documentdb-keys](../../includes/documentdb-keys.md)]

<br/>
Nyní vám ukážeme, jak od základů vytvořit novou aplikaci ASP.NET MVC. 

## <a name="_Toc395637762"></a>Krok 2: Vytvoření nové aplikace ASP.NET MVC
Nyní když máte účet, můžeme vytvořit nový projekt ASP.NET.

1. V nástroji Visual Studio najeďte myší v nabídce **Soubor** na **Nový** a klikněte na **Projekt**.
   
       The **New Project** dialog box appears.
2. V podokně **Typy projektů** rozbalte **Šablony**, **Visual C#**, **Web** a vyberte **Webová aplikace ASP.NET**.
   
      ![Snímek obrazovky dialogového okna Nový projekt se zvýrazněným typem projektu Webová aplikace ASP.NET](./media/documentdb-dotnet-application/asp-net-mvc-tutorial-image10.png)
3. Do pole **Název** zadejte název projektu. Tento kurz používá název todo. Pokud se rozhodnete použít něco jiného, pak kdykoli v tomto kurzu bude zmíněn obor názvů todo, je potřeba upravit poskytnuté ukázky kódu tak, aby používaly vámi zvolený název aplikace. 
4. Po kliknutí na **Procházet** přejděte na složku, ve které chcete vytvořit projekt, a klikněte na **OK**.
   
      Zobrazí se dialogové okno **Nový projekt ASP.NET**.
   
      ![Snímek obrazovky dialogového okna Nový projekt ASP.NET se zvýrazněnou šablonou aplikace MVC a zaškrtnutým políčkem Hostovat v cloudu](./media/documentdb-dotnet-application/asp-net-mvc-tutorial-image11.png)
5. V podokně šablon vyberte **MVC**.
6. Pokud se chystáte hostovat aplikaci v Azure, vyberte v pravé dolní části možnost **Hostovat v cloudu**, aby se hostitelem aplikace stala služba Azure. Rozhodli jsme se pro hostování v cloudu a spuštění aplikace hostované na webu Azure. Výběrem této možnosti se pro vás předem zřídí web Azure, aby bylo mnohem snazší nasadit konečnou funkční aplikaci. Pokud aplikaci chcete hostovat v jiné službě nebo v tuto chvíli nechcete konfigurovat Azure, stačí zrušit zaškrtnutí políčka **Hostovat v cloudu**.
7. Klikněte na **OK** a nechte Visual Studio odvést svou práci s generováním prázdné šablony ASP.NET MVC. 

    Pokud se zobrazí chyba „Během zpracování vaší žádosti došlo k chybě“, přečtěte si oddíl [Řešení potíží](#troubleshooting).

8. Pokud se rozhodnete aplikaci hostovat v cloudu, zobrazí se nejméně jedna další obrazovka, která vás bude žádat o přihlášení k účtu Azure a zadání několika hodnot pro nový web. Zadejte všechny dodatečné hodnoty a pokračujte. 
   
      Nevybrali jsme tady Databázový server, protože v tomto projektu nepoužíváme server služby Azure SQL Database. Později na Portálu Azure vytvoříme nový účet Azure DocumentDB.
   
    Další informace o volbě **plánu služby App Service** a **skupiny prostředků** najdete v tématu [Podrobný přehled plánů služby Azure App Service](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).
   
      ![Snímek obrazovky dialogového okna Konfigurace webu Microsoft Azure](./media/documentdb-dotnet-application/image11_1.png)
9. Až nástroj Visual Studio dokončí vytváření standardní aplikace MVC, budete mít k dispozici prázdnou aplikaci ASP.NET, kterou můžete spouštět místně.
   
    Lokální spouštění projektu přeskočíme, protože všichni jsme již určitě viděli aplikaci ASP.NET Hello World. Přejděme rovnou na přidání DocumentDB do projektu a sestavení aplikace.

## <a name="_Toc395637767"></a>Krok 3: Přidání DocumentDB do projektu webové aplikace MVC
Teď již máme za sebou většinu potřebných příprav technologie ASP.NET MVC, které potřebujeme pro toto řešení. Přejděme tedy ke skutečnému účelu tohoto kurzu, a tím je přidání Azure DocumentDB k webové aplikaci MVC.

1. Sada DocumentDB .NET SDK se připravuje a distribuuje jako balíček NuGet. Balíček NuGet získáte v nástroji Visual Studio tak, že v něm použijete správce balíčků NuGet. Klikněte pravým tlačítkem na projekt v **Průzkumníkovi řešení** a pak levým na **Spravovat balíčky NuGet**.
   
      ![Snímek obrazovky možností nabídky po kliknutí pravým tlačítkem na projekt webové aplikace v Průzkumníkovi řešení se zvýrazněnou možností Spravovat balíčky NuGet](./media/documentdb-dotnet-application/image21.png)
   
    Zobrazí se dialogové okno **Správa balíčků NuGet**.
2. Do pole **Procházet** NuGet zadejte ***Azure DocumentDB***.
   
    Z výsledků nainstalujte balíček **Microsoft Azure DocumentDB Client Library**. Tím se stáhne a nainstaluje balíček DocumentDB včetně všech jeho závislostí, například Newtonsoft.Json. Dokončete instalaci tak, že v okně **Náhled** kliknete na **OK** a v okně **Souhlas s podmínkami licence** na **Přijímám**.
   
      ![Snímek obrazovky okna Správa balíčků NuGet se zvýrazněnou položkou Microsoft Azure DocumentDB Client Library](./media/documentdb-dotnet-application/nuget.png)
   
      K instalaci balíčku můžete případně použít i Konzolu Správce balíčků. Pokud ji chcete využít, v nabídce **Nástroje** klikněte na **Správce balíčků NuGet** a pak na **Konzola Správce balíčků**. Do příkazového řádku zadejte následující příkaz.
   
        Install-Package Microsoft.Azure.DocumentDB
3. Až se balíček nainstaluje, řešení Visual Studio by se mělo podobat tomu na následujícím obrázku se dvěma přidanými referencemi, Microsoft.Azure.Documents.Client a Newtonsoft.Json.
   
      ![Snímek obrazovky se dvěma přidanými referencemi na datový projekt JSON v Průzkumníkovi řešení](./media/documentdb-dotnet-application/image22.png)

## <a name="_Toc395637763"></a>Krok 4: Nastavení aplikace ASP.NET MVC
Nyní k této aplikaci MVC přidejme modely, zobrazení a kontrolery:

* [Přidání modelu](#_Toc395637764)
* [Přidání kontroleru](#_Toc395637765)
* [Přidání zobrazení](#_Toc395637766)

### <a name="_Toc395637764"></a>Přidání datového modelu JSON
Začněme vytvořením velkého **M** v MVC, tedy modelem. 

1. V **Průzkumníkovi řešení** klikněte pravým tlačítkem na složku **Modely**, pak levým na **Přidat** a nakonec také levým na **Třída**.
   
      Zobrazí se dialogové okno **Přidat novou položku**.
2. Pojmenujte svou novou třídu **Item.cs** a klikněte na **Přidat**. 
3. Do tohoto nového souboru **Item.cs** přidejte za poslední *příkaz using* následující příkaz.
   
        using Newtonsoft.Json;
4. Nyní nahraďte tento kód 
   
        public class Item
        {
        }
   
    následujícím kódem.
   
        public class Item
        {
            [JsonProperty(PropertyName = "id")]
            public string Id { get; set; }
   
            [JsonProperty(PropertyName = "name")]
            public string Name { get; set; }
   
            [JsonProperty(PropertyName = "description")]
            public string Description { get; set; }
   
            [JsonProperty(PropertyName = "isComplete")]
            public bool Completed { get; set; }
        }
   
    Všechna data v DocumentDB se přenesou a uloží jako dokumenty JSON. Pokud chcete řídit způsob, jakým se objekty serializují a deserializují technologií JSON.NET, můžete použít atribut **JsonProperty**, jak je předvedeno v třídě **Item**, kterou jsme právě vytvořili. Není nezbytně **nutné** to provést, ale chceme se ujistit, že naše vlastnosti dodržují konvence pojmenování ve formátu JSON camelCase. 
   
    Nejenže můžete určovat formát názvu vlastnosti, když se ukládá do formátu JSON, ale můžete zcela přejmenovat vlastnosti .NET, jako jsme to udělali s vlastností **Popis**. 

### <a name="_Toc395637765"></a>Přidání kontroleru
Tím je vyřešeno **M**, teď vytvořme **C** z MVC, tedy třídu kontroleru (angl. controller).

1. V **Průzkumníkovi řešení** klikněte pravým tlačítkem na složku **Kontrolery**, pak levým na **Přidat** a nakonec také levým na **Kontroler**.
   
    Zobrazí se dialogové okno **Přidat vygenerované uživatelské rozhraní**.
2. Vyberte **Kontroler MVC 5 – prázdný** a klikněte na **Přidat**.
   
    ![Snímek obrazovky dialogového okna Přidat vygenerované uživatelské rozhraní se zvýrazněnou možností Kontroler MVC 5 – prázdný](./media/documentdb-dotnet-application/image14.png)
3. Pojmenujte nový kontroler, **ItemController**.
   
    ![Snímek obrazovky dialogového okna Přidat kontroler](./media/documentdb-dotnet-application/image15.png)
   
    Až se soubor vytvoří, řešení v nástroji Visual Studio by se mělo podobat tomu na následujícím obrázku s novým souborem ItemController.cs v **Průzkumníkovi řešení**. Zobrazuje se i nový soubor Item.cs vytvořený dříve.
   
    ![Snímek obrazovky řešení Visual Studio – Průzkumník řešení se zvýrazněnými novými soubory ItemController.cs a Item.cs](./media/documentdb-dotnet-application/image16.png)
   
    Soubor ItemController.cs můžete zavřít – vrátíme se k němu později. 

### <a name="_Toc395637766"></a>Přidání zobrazení
Nyní pojďme vytvořit **V** z MVC, tedy zobrazení (angl. view):

* [Přidání zobrazení Index položky](#AddItemIndexView)
* [Přidání zobrazení Nová položka](#AddNewIndexView)
* [Přidání zobrazení Upravit položku](#_Toc395888515)

#### <a name="AddItemIndexView"></a>Přidání zobrazení Index položky
1. V **Průzkumníkovi řešení** rozbalte složku **Zobrazení**, klikněte pravým tlačítkem na prázdnou složku **Položka**, kterou pro vás Visual Studio vytvořilo již dříve (když jste přidali **ItemController**), klikněte na **Přidat** a nakonec klikněte na **Zobrazení**.
   
    ![Snímek obrazovky Průzkumníka řešení, na kterém se zobrazuje složka Položka vytvořená nástrojem Visual Studio se zvýrazněnými příkazy Přidat zobrazení](./media/documentdb-dotnet-application/image17.png)
2. V dialogovém okně **Přidat zobrazení** proveďte následující akce:
   
   * Do pole **Název zobrazení** zadejte ***Index***.
   * V poli **Šablona** vyberte ***Seznam***.
   * V poli **Třída modelu** vyberte ***Položka (todo.Models)***.
   * Pole **Třída kontextu dat** nechejte prázdné. 
   * Do pole stránky rozložení zadejte ***~/Views/Shared/_Layout.cshtml***.
     
     ![Snímek obrazovky ukazující dialogové okno Přidat zobrazení](./media/documentdb-dotnet-application/image18.png)
3. Jakmile jsou všechny tyto hodnoty nastaveny, klikněte na **Přidat** a nechejte Visual Studio vytvořit novou šablonu zobrazení. Až se to dokončí, otevře se soubor cshtml, který se vytvoří. Tento soubor můžeme v nástroji Visual Studio zavřít, protože se k němu vrátíme později.

#### <a name="AddNewIndexView"></a>Přidání zobrazení Nová položka
Podobným způsobem, jakým jsme vytvořili zobrazení **Index položky**, vytvoříme nyní nové zobrazení pro vytváření nových **položek**.

1. V **Průzkumníkovi řešení** klikněte pravým tlačítkem na složku **Položka**, pak levým na **Přidat** a nakonec také levým na **Zobrazení**.
2. V dialogovém okně **Přidat zobrazení** proveďte následující akce:
   
   * Do pole **Název zobrazení** zadejte ***Create***.
   * V poli **Šablona** vyberte ***Create***.
   * V poli **Třída modelu** vyberte ***Položka (todo.Models)***.
   * Pole **Třída kontextu dat** nechejte prázdné.
   * Do pole stránky rozložení zadejte ***~/Views/Shared/_Layout.cshtml***.
   * Klikněte na **Přidat**.

#### <a name="_Toc395888515"></a>Přidání zobrazení Upravit položku
A nakonec stejným způsobem jako předtím přidejte jedno poslední zobrazení pro úpravu **položky**.

1. V **Průzkumníkovi řešení** klikněte pravým tlačítkem na složku **Položka**, pak levým na **Přidat** a nakonec také levým na **Zobrazení**.
2. V dialogovém okně **Přidat zobrazení** proveďte následující akce:
   
   * Do pole **Název zobrazení** zadejte ***Edit***.
   * V poli **Šablona** vyberte ***Edit***.
   * V poli **Třída modelu** vyberte ***Položka (todo.Models)***.
   * Pole **Třída kontextu dat** nechejte prázdné. 
   * Do pole stránky rozložení zadejte ***~/Views/Shared/_Layout.cshtml***.
   * Klikněte na **Přidat**.

Až bude vše hotovo, zavřete všechny dokumenty cshtml v nástroji Visual Studio, protože se k těmto zobrazením vrátíme později.

## <a name="_Toc395637769"></a>Krok 5: Připojení DocumentDB
Nyní, když jsou vyřešeny všechny standardní náležitosti MVC, se můžeme zaměřit na přidávání kódu pro DocumentDB. 

V této části přidáme kód pro zpracování následujícího:

* [Výpis neúplných položek](#_Toc395637770)
* [Přidávání položek](#_Toc395637771)
* [Úprava položek](#_Toc395637772)

### <a name="_Toc395637770"></a>Výpis neúplných položek ve webové aplikaci MVC
První věc, kterou je zde potřeba udělat, je přidat třídu, která bude obsahovat všechnu logiku pro připojení a používání DocumentDB. Pro účely tohoto kurzu zapouzdříme všechnu tuto logiku do třídy úložiště nazvané DocumentDBRepository. 

1. V **Průzkumníkovi řešení** klikněte pravým tlačítkem na složku projekt, pak levým na **Přidat** a nakonec také levým na **Třída**. Pojmenujte novou třídu **DocumentDBRepository** a klikněte na **Přidat**.
2. Do nově vytvořené třídy **DocumentDBRepository** přidejte následující *příkazy using* nad deklaraci *namespace*.
   
        using Microsoft.Azure.Documents; 
        using Microsoft.Azure.Documents.Client; 
        using Microsoft.Azure.Documents.Linq; 
        using System.Configuration;
        using System.Linq.Expressions;
        using System.Threading.Tasks;
   
    Nyní nahraďte tento kód 
   
        public class DocumentDBRepository
        {
        }
   
    následujícím kódem.
   
        public static class DocumentDBRepository<T> where T : class
        {
            private static readonly string DatabaseId = ConfigurationManager.AppSettings["database"];
            private static readonly string CollectionId = ConfigurationManager.AppSettings["collection"];
            private static DocumentClient client;
   
            public static void Initialize()
            {
                client = new DocumentClient(new Uri(ConfigurationManager.AppSettings["endpoint"]), ConfigurationManager.AppSettings["authKey"]);
                CreateDatabaseIfNotExistsAsync().Wait();
                CreateCollectionIfNotExistsAsync().Wait();
            }
   
            private static async Task CreateDatabaseIfNotExistsAsync()
            {
                try
                {
                    await client.ReadDatabaseAsync(UriFactory.CreateDatabaseUri(DatabaseId));
                }
                catch (DocumentClientException e)
                {
                    if (e.StatusCode == System.Net.HttpStatusCode.NotFound)
                    {
                        await client.CreateDatabaseAsync(new Database { Id = DatabaseId });
                    }
                    else
                    {
                        throw;
                    }
                }
            }
   
            private static async Task CreateCollectionIfNotExistsAsync()
            {
                try
                {
                    await client.ReadDocumentCollectionAsync(UriFactory.CreateDocumentCollectionUri(DatabaseId, CollectionId));
                }
                catch (DocumentClientException e)
                {
                    if (e.StatusCode == System.Net.HttpStatusCode.NotFound)
                    {
                        await client.CreateDocumentCollectionAsync(
                            UriFactory.CreateDatabaseUri(DatabaseId),
                            new DocumentCollection { Id = CollectionId },
                            new RequestOptions { OfferThroughput = 1000 });
                    }
                    else
                    {
                        throw;
                    }
                }
            }
        }
   
   > [!TIP]
   > Při vytváření nové DocumentCollection můžete zadat volitelný parametr RequestOptions typu OfferType, který vám umožní určit úroveň výkonu nové kolekce. Pokud tento parametr nebude zadán, využije se výchozí typ nabídky. Další informace o typech nabídek DocumentDB si prosím přečtěte v tématu [Úrovně výkonu DocumentDB](documentdb-performance-levels.md).
   > 
   > 
3. Čteme několik hodnot z konfigurace, proto si otevřete soubor **Web.config** své aplikace a do části `<AppSettings>` přidejte následující řádky.
   
        <add key="endpoint" value="enter the URI from the Keys blade of the Azure Portal"/>
        <add key="authKey" value="enter the PRIMARY KEY, or the SECONDARY KEY, from the Keys blade of the Azure  Portal"/>
        <add key="database" value="ToDoList"/>
        <add key="collection" value="Items"/>
4. Nyní pomocí okna Klíče na Portálu Azure aktualizujte hodnoty pro *endpoint* a *authKey*. Jako hodnotu pro nastavení endpoint použijte **URI** z okna Klíče, pro nastavení authKey pak jako hodnotu využijte **PRIMÁRNÍ KLÍČ** nebo **SEKUNDÁRNÍ KLÍČ** z téhož okna.

    Tím je vyřešeno připojení úložiště DocumentDB. Nyní pojďme přidat logiku aplikace.

1. Tím prvním, co chceme mít možnost v aplikaci seznamu úkolů dělat, je zobrazit neúplné položky.  Zkopírujte a vložte následující fragment kódu kamkoli do třídy **DocumentDBRepository**.
   
        public static async Task<IEnumerable<T>> GetItemsAsync(Expression<Func<T, bool>> predicate)
        {
            IDocumentQuery<T> query = client.CreateDocumentQuery<T>(
                UriFactory.CreateDocumentCollectionUri(DatabaseId, CollectionId))
                .Where(predicate)
                .AsDocumentQuery();
   
            List<T> results = new List<T>();
            while (query.HasMoreResults)
            {
                results.AddRange(await query.ExecuteNextAsync<T>());
            }
   
            return results;
        }
2. Otevřete **ItemController**, který jsme přidali dříve, a nad deklaraci oboru názvů přidejte následující *příkazy using*.
   
        using System.Net;
        using System.Threading.Tasks;
        using todo.Models;
   
    Pokud se váš projekt nejmenuje todo, bude nutné aktualizovat příkaz todo.Models tak, aby odpovídal názvu projektu.
   
    Nyní nahraďte tento kód
   
        //GET: Item
        public ActionResult Index()
        {
            return View();
        }
   
    následujícím kódem.
   
        [ActionName("Index")]
        public async Task<ActionResult> IndexAsync()
        {
            var items = await DocumentDBRepository<Item>.GetItemsAsync(d => !d.Completed);
            return View(items);
        }
3. Otevřete **Global.asax.cs** a přidejte následující řádek do metody **Application_Start** 
   
        DocumentDBRepository<todo.Models.Item>.Initialize();

V tuto chvíli by mělo být možné sestavit vaše řešení bez jakýchkoli chyb.

Pokud jste nyní aplikaci spustili, přešli jste na **HomeController** a zobrazení **Index** tohoto kontroleru. To je výchozí chování projektu šablony MVC, který jsme zvolili na začátku, ale takto to mít nechceme! Změňte směrování v této aplikaci MVC, abychom dané chování upravili.

Otevřete ***App\_Start\RouteConfig.cs***, vyhledejte řádek, který začíná na defaults: a změňte jej tak, že bude vypadat přibližně takto.

        defaults: new { controller = "Item", action = "Index", id = UrlParameter.Optional }

Tímto bude aplikaci ASP.NET MVC oznámeno, že pokud jste v adrese URL nezadali hodnotu pro řízení chování směrování, má se jako kontroler místo **Home** použít **Item** a jako zobrazení **Index**.

Když nyní aplikaci spustíte, zavolá váš **ItemController**, který zavolá třídu úložiště a použije metodu GetItems, pomocí které vrátí všechny neúplné položky do zobrazení **View**\\**Item**\\**Index**. 

Pokud teď projekt sestavíte a spustíte, mělo by se zobrazit něco přibližně takového.    

![Snímek obrazovky webové aplikace vytvořené v tomto databázovém kurzu](./media/documentdb-dotnet-application/image23.png)

### <a name="_Toc395637771"></a>Přidávání položek
Přidejme do databáze nějaké položky, abychom tam neměli jen prázdnou mřížku.

Přidejme nějaký kód do DocumentDBRepository a ItemController, aby byl záznam v DocumentDB zachován jako trvalý.

1. Do třídy **DocumentDBRepository** přidejte následující metodu.
   
       public static async Task<Document> CreateItemAsync(T item)
       {
           return await client.CreateDocumentAsync(UriFactory.CreateDocumentCollectionUri(DatabaseId, CollectionId), item);
       }
   
   Tato metoda jednoduše přijme předaný objekt a zachová jej jako trvalý v DocumentDB.
2. Otevřete soubor ItemController.cs a přidejte do třídy následující fragment kódu. Takto ASP.NET MVC ví, co dělat s akcí **Create**. V tomto případě má pouze vykreslit přidružené zobrazení Create.cshtml vytvořené dříve.
   
        [ActionName("Create")]
        public async Task<ActionResult> CreateAsync()
        {
            return View();
        }
   
    Nyní potřebujeme v tomto kontroleru další kód, který přijme odeslané informace ze zobrazení **Create**.
3. Přidejte do třídy ItemController.cs další blok kódu, který technologii ASP.NET MVC řekne, co dělat s operací POST formuláře pro tento kontroler.
   
        [HttpPost]
        [ActionName("Create")]
        [ValidateAntiForgeryToken]
        public async Task<ActionResult> CreateAsync([Bind(Include = "Id,Name,Description,Completed")] Item item)
        {
            if (ModelState.IsValid)
            {
                await DocumentDBRepository<Item>.CreateItemAsync(item);
                return RedirectToAction("Index");
            }
   
            return View(item);
        }
   
    Teto kód zavolá do DocumentDBRepository a použije metodu CreateItemAsync, aby se nová položka úkolu zachovala v databázi. 
   
    **Poznámka k zabezpečení:** Atribut **ValidateAntiForgeryToken** zde slouží k tomu, aby pomohl zabezpečit tuto aplikace před útokem CSRF. Není třeba provádět žádnou další akci, jen přidat tento atribut – vaše zobrazení musí s tímto tokenem proti padělání pracovat také. Další informace k tomuto tématu a příklady, jak toto správně implementovat, najdete v článku o [zabránění útoku CSRF][Preventing Cross-Site Request Forgery]. Zdrojový kód dostupný na [GitHubu][GitHub] má toto plně implementováno.
   
    **Poznámka k zabezpečení:** U parametru metody používáme i atribut **Bind**, abychom zvýšili zabezpečení před útoky typu OVERPOST. Další informace najdete v tématu [Základní operace CRUD v ASP.NET MVC][Basic CRUD Operations in ASP.NET MVC].

Tímto je kód potřebný k přidávání nových položek do databáze hotový.

### <a name="_Toc395637772"></a>Úprava položek
Ještě musíme implementovat jednu poslední funkci, a to přidat schopnost upravit **položky** v databázi a označit je jako dokončené. Zobrazení pro úpravy již bylo do projektu přidáno, takže nyní potřebujeme jen opět přidat kód do kontroleru a třídy **DocumentDBRepository**.

1. Do třídy **DocumentDBRepository** přidejte následující kód.
   
        public static async Task<Document> UpdateItemAsync(string id, T item)
        {
            return await client.ReplaceDocumentAsync(UriFactory.CreateDocumentUri(DatabaseId, CollectionId, id), item);
        }
   
        public static async Task<T> GetItemAsync(string id)
        {
            try
            {
                Document document = await client.ReadDocumentAsync(UriFactory.CreateDocumentUri(DatabaseId, CollectionId, id));
                return (T)(dynamic)document;
            }
            catch (DocumentClientException e)
            {
                if (e.StatusCode == HttpStatusCode.NotFound)
                {
                    return null;
                }
                else
                {
                    throw;
                }
            }
        }
   
    První z těchto metod, **GetItem**, načítá položku z DocumentDB, která se předá zpět do **ItemControlleru** a pak do zobrazení **Edit**.
   
    Druhá metoda, kterou jsme právě přidali, nahrazuje **dokument** v DocumentDB verzí **dokumentu** předanou z **ItemControlleru**.
2. Do třídy **ItemController** přidejte následující kód.
   
        [HttpPost]
        [ActionName("Edit")]
        [ValidateAntiForgeryToken]
        public async Task<ActionResult> EditAsync([Bind(Include = "Id,Name,Description,Completed")] Item item)
        {
            if (ModelState.IsValid)
            {
                await DocumentDBRepository<Item>.UpdateItemAsync(item.Id, item);
                return RedirectToAction("Index");
            }
   
            return View(item);
        }
   
        [ActionName("Edit")]
        public async Task<ActionResult> EditAsync(string id)
        {
            if (id == null)
            {
                return new HttpStatusCodeResult(HttpStatusCode.BadRequest);
            }
   
            Item item = await DocumentDBRepository<Item>.GetItemAsync(id);
            if (item == null)
            {
                return HttpNotFound();
            }
   
            return View(item);
        }
   
    První metoda zpracovává operaci HTTP GET, ke které dochází, když uživatel klikne na odkaz **Upravit** v zobrazení **Index**. Tato metoda načítá [**dokument**](http://msdn.microsoft.com/library/azure/microsoft.azure.documents.document.aspx) z DocumentDB a předává jej do zobrazení **Edit**.
   
    Zobrazení **Edit** pak provede operaci HTTP POST do **IndexControlleru**. 
   
    Druhá metoda, kterou jsme přidali, zpracovává předávání aktualizovaného objektu do DocumentDB, aby zůstal v databázi zachován jako trvalý.

To je vše, nic dalšího ke spuštění aplikace, vypsání neúplných **položek**, přidávání nových **položek** a úpravě **položek** nepotřebujeme.

## <a name="_Toc395637773"></a>Krok 6: Místní spuštění aplikace
Aplikaci otestujete na svém místním počítači tak, že provedete následující akce:

1. Stiskněte v nástroji Visual Studio klávesu F5 – aplikace se sestaví v režimu ladění. Po sestavení aplikace by se měl spustit prohlížeč se stránkou s prázdnou mřížkou, kterou jsme viděli dříve:
   
    ![Snímek obrazovky webové aplikace vytvořené v tomto databázovém kurzu](./media/documentdb-dotnet-application/image24.png)
   
    Pokud používáte sadu Visual Studio 2013 a zobrazí se chybová zpráva „Nelze vyčkat v těle klauzule problému.“ je třeba nainstalovat [balíček nuget Microsoft.Net.Compilers](https://www.nuget.org/packages/Microsoft.Net.Compilers/). Můžete také porovnat svůj kód proti vzorovému projektu na [GitHubu][GitHub]. 
2. Klikněte na odkaz **Vytvořit nový** a do polí **Název** a **Popis** zadejte hodnoty. Zaškrtávací políčko **Dokončeno** ponechte prázdné, jinak bude nová **položka** přidána ve stavu dokončení a nezobrazí se v úvodním seznamu.
   
    ![Snímek obrazovky zobrazení pro vytváření](./media/documentdb-dotnet-application/image25.png)
3. Klikněte na **Vytvořit**. Budete přesměrováni zpět na zobrazení **Index** a v seznamu se zobrazí **položka**.
   
    ![Snímek obrazovky zobrazení pro index](./media/documentdb-dotnet-application/image26.png)
   
    Do seznamu úkolů můžete dle libosti přidat několik dalších **položek**.
4. Klikněte na **Upravit** vedle **položky** v seznamu. Přejdete na zobrazení **Upravit**, kde můžete aktualizovat jakoukoli vlastnost objektu, včetně příznaku **Dokončeno**. Pokud označíte příznak **Dokončeno** a kliknete na **Uložit**, **položka** se odstraní ze seznamu neúplných úkolů.
   
    ![Snímek obrazovky zobrazení Index se zaškrtnutým políčkem Dokončeno](./media/documentdb-dotnet-application/image27.png)
5. Až budete s testováním aplikace hotovi, stiskněte Ctrl+F5, aby se ukončilo ladění aplikace. Jste připraveni aplikaci nasadit!

## <a name="_Toc395637774"></a>Krok 7: Nasazení aplikace na Azure Websites
Nyní, když je aplikace dokončena a správně funguje s DocumentDB, nasadíme tuto webovou aplikaci na Weby Azure. Pokud jste při vytváření prázdného projektu ASP.NET MVC vybrali možnost **Hostovat v cloudu**, Visual Studio tento krok velmi usnadní a většinu práce odvede za vás. 

1. K publikování této aplikace stačí kliknout pravým tlačítkem na projekt v **Průzkumníkovi řešení** a kliknout na **Publikovat**.
   
    ![Snímek obrazovky s možností Publikovat v Průzkumníkovi řešení](./media/documentdb-dotnet-application/image28.png)
2. Vše již by mělo být nakonfigurováno podle vašich přihlašovacích údajů. Dokonce je již v Azure na zobrazené **cílové adrese URL** vytvořen web – stačí jen kliknout na **Publikovat**.
   
    ![Snímek obrazovky dialogového okna Publikovat web v nástroji Visual Studio – podrobný kurz k ASP.NET MVC](./media/documentdb-dotnet-application/image29.png)

Za několik sekund Visual Studio dokončí publikování webové aplikace a spustí prohlížeč, kde se můžete podívat, jak vaše práce běží v Azure!

## <a name="Troubleshooting"></a>Řešení potíží

Pokud se při pokusu o nasazení webové aplikace zobrazí chyba „Během zpracování vaší žádosti došlo k chybě“, postupujte následovně: 

1. Zavřete chybovou zprávu a pak znovu vyberte **Microsoft Azure Web Apps**. 
2. Přihlaste se a vyberte **Nový** pro vytvoření nové webové aplikace. 
3. Na obrazovce **Vytvořit webovou aplikaci v systému Microsoft Azure** proveďte následující: 
    
    - Název webové aplikace: todo-net-app
    - Plán služby App Service: Vytvořte nový s názvem todo-net-app
    - Skupina prostředků: Vytvořte novou s názvem todo-net-app
    - Oblast: Vyberte oblast co nejblíže uživatelům vaší aplikace.
    - Databázový server: Klikněte na Bez databáze a pak klikněte na **Vytvořit**. 

4. Na obrazovce * todo-net-app klikněte na **Ověřit připojení**. Po ověření připojení klikněte na **Publikovat**. 
    
    Aplikace se pak zobrazí v prohlížeči.


## <a name="_Toc395637775"></a>Další kroky
Blahopřejeme! Právě jste vytvořili svou první webovou aplikaci ASP.NET MVC, která používá Azure DocumentDB, a publikovali jste ji na Weby Azure. Zdrojový kód hotové aplikace včetně podrobností a odstraněných funkcí, které nebyly součástí tohoto kurzu, si můžete stáhnout nebo naklonovat z [GitHubu][GitHub]. Pokud byste tedy chtěli tyto funkce zahrnout do své aplikace, můžete si kód stáhnout a přidat.

Pokud chcete rozšířit funkce aplikace, prohlédněte si rozhraní API dostupná v [knihovně .NET DocumentDB](https://msdn.microsoft.com/library/azure/dn948556.aspx) a nebojte se přispět do knihovny .NET DocumentDB na [GitHubu][GitHub]. 

[\*]: https://microsoft.sharepoint.com/teams/DocDB/Shared%20Documents/Documentation/Docs.LatestVersions/PicExportError
[Visual Studio Express]: http://www.visualstudio.com/products/visual-studio-express-vs.aspx
[Microsoft Web Platform Installer]: http://www.microsoft.com/web/downloads/platform.aspx
[Preventing Cross-Site Request Forgery]: http://go.microsoft.com/fwlink/?LinkID=517254
[Basic CRUD Operations in ASP.NET MVC]: http://go.microsoft.com/fwlink/?LinkId=317598
[GitHub]: https://github.com/Azure-Samples/documentdb-net-todo-app

