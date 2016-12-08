---
title: "Vícevrstvá aplikace .NET | Dokumentace Microsoftu"
description: "Kurz .NET, který vám pomůže vytvořit vícevrstvou aplikaci v Azure, která používá fronty Service Bus ke komunikaci mezi vrstvami."
services: service-bus-messaging
documentationcenter: .net
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 1b8608ca-aa5a-4700-b400-54d65b02615c
ms.service: service-bus-messaging
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: get-started-article
ms.date: 09/01/2016
ms.author: sethm
translationtype: Human Translation
ms.sourcegitcommit: 9ace119de3676bcda45d524961ebea27ab093415
ms.openlocfilehash: c90454109c2fcfe69d512b84d411e4fd4e810f65


---
# <a name="net-multi-tier-application-using-azure-service-bus-queues"></a>Vícevrstvá aplikace .NET, která používá fronty Azure Service Bus
## <a name="introduction"></a>Úvod
Vývoj pro Microsoft Azure je snadný při použití Visual Studia a bezplatné sady Azure SDK pro .NET. Tento kurz vás provede jednotlivými kroky při vytváření aplikace, která používá několik prostředků Azure běžících ve vašem lokálním prostředí. Tyto kroky předpokládají, že nemáte žádné předchozí zkušenosti s používáním Azure.

Naučíte se:

* Jak ve svém počítači vytvořit prostředí pro vývoj pro Azure stažením a instalací jednoho balíčku.
* Jak používat Visual Studio k vývoji pro Azure.
* Jak vytvořit vícevrstvou aplikaci v Azure pomocí webových rolí a rolí pracovního procesu.
* Jak komunikovat mezi vrstvami pomocí front Service Bus.

[!INCLUDE [create-account-note](../../includes/create-account-note.md)]

V tomto kurzu sestavíte a spustíte vícevrstvou aplikaci v cloudové službě Azure. Front-endem bude webová role ASP.NET MVC a back-endem bude role pracovního procesu, která používá frontu Service Bus. Můžete vytvořit stejnou vícevrstvou aplikaci s front-endem jako webový projekt, který je nasazený do webové stránky Azure namísto cloudové služby. Pokyny k tomu, co je potřeba udělat jinak na front-endu webové stránky Azure, najdete v části [Další kroky](#nextsteps). Taky můžete vyzkoušet kurz [Hybridní lokální/cloudová aplikace .NET](../service-bus-relay/service-bus-dotnet-hybrid-app-using-service-bus-relay.md).

Na následujícím snímku obrazovky je vidět hotová aplikace.

![][0]

## <a name="scenario-overview-inter-role-communication"></a>Přehled scénáře: komunikace mezi rolemi
Abyste mohli odeslat objednávku ke zpracování, musí komponenta uživatelského prostředí front-endu, která běží ve webové roli, pracovat s logikou střední úrovně běžící v roli pracovního procesu. Tento příklad používá zprostředkované zasílání zpráv pro komunikaci mezi vrstvami.

Pomocí zprostředkovaného zasílání zpráv mezi webem a prostředními úrovněmi odděluje obě části. Na rozdíl od přímého přenosu zpráv (tzn. TCP nebo HTTP) se webová úroveň nemusí k prostřední úrovni připojit přímo, namísto toho odesílá pracovní jednotky jako zprávy do služby Service Bus, která je spolehlivě uchová, dokud nebude prostřední vrstva připravená je spotřebovat a zpracovat.

Service Bus nabízí dvě entity, které podporují zprostředkované zasílání zpráv: fronty a témata. V případě front se každá zpráva odeslaná do fronty spotřebuje jedním příjemcem. Témata podporují chování typu publikovat/odebírat, ve kterém je každá publikovaná zpráva zpřístupněná odběru registrovanému pro dané téma. Každý odběr logicky uchovává svoji vlastní frontu zpráv. Odběry se taky dají konfigurovat pomocí pravidel filtrů, které omezují skupinu zpráv předávaných do fronty odběru na takové, které odpovídají filtru. Následující příklad používá fronty Service Bus.

![][1]

Tento komunikační mechanizmus má několik výhod oproti přímému přenosu zpráv.

* **Časové oddělení**. S asynchronním vzorcem zasílání zpráv nemusí být producenti a spotřebitelé online ve stejnou dobu. Service Bus spolehlivě uchová zprávy, dokud spotřebitel nebude připravený je přijmout. Díky tomu se součásti distribuované aplikace můžou odpojit, například při údržbě nebo při selhání jedné ze součástí, a přitom to nebude mít vliv na systém jako celek. Navíc stačí, aby spotřebitelská aplikace byla online i jen v určitou dobu během dne.
* **Vyrovnávání zátěže**. V mnoha aplikacích se zátěž na systém může postupně měnit, zatímco doba nutná ke zpracování pracovní jednotky je obvykle stálá. Propojovací producenti a spotřebitelé zpráv s frontou – to znamená, že spotřebitelskou aplikaci (pracovní proces) stačí zřídit jen na obvyklou zátěž, ne na zátěž ve špičce. S měnící se příchozí zátěží se mění hloubka fronty. To znamená přímou úsporu nákladů ve smyslu infrastruktury nutné pro zvládání zatížení aplikace.
* **Vyrovnávání zatížení**. Když se zátěž zvyšuje, můžou se přidat další pracovní procesy, které budou číst zprávy z fronty. Každou zprávu zpracovává jen jeden pracovní proces. Toto vyrovnávání zátěže podle požadavků umožňuje optimální využívání pracovních počítačů i v případě, že se pracovní počítače liší z hlediska výkonu, protože zprávy žádaný a zpracovávají svou vlastním maximální rychlostí. Tomuto chování se často říká *konkurence mezi spotřebiteli*.
  
  ![][2]

V následující části se probírá kód, který tuto architekturu implementuje.

## <a name="set-up-the-development-environment"></a>Nastavení vývojového prostředí
Než začnete s vývojem aplikací pro Azure, připravte si nástroje a vývojové prostředí.

1. Nainstalujte si Azure SDK pro .NET ze stránky [Stažení nástrojů a SDK][Stažení nástrojů a SDK].
2. Klikněte na **Instalovat sadu SDK** pro verzi Visual Studia, kterou používáte. Kroky v tomto kurzu ukazují postup ve Visual Studiu 2015.
3. Když se zobrazí dialog pro spuštění nebo uložení instalačního programu, klikněte na **Spustit**.
4. V **Instalačním programu webové platformy** klikněte na **Instalovat** a pokračujte v instalaci.
5. Po dokončení instalace budete mít všechno, co je potřeba k vývoji aplikace. Sada SDK obsahuje nástroje, které vám umožní snadno vyvíjet aplikace pro Azure ve Visual Studiu. Pokud nemáte Visual Studio nainstalované, SDK taky nainstaluje bezplatnou verzi Visual Studio Express.

## <a name="create-a-namespace"></a>Vytvoření oboru názvů
Dál je potřeba vytvořit obor názvů služby a získat klíč sdíleného přístupového podpisu (SAS). Obor názvů aplikaci poskytuje hranice pro každou aplikaci vystavenou přes službu Service Bus. Systém vygeneruje klíč SAS při vytvoření oboru názvů. Kombinace oboru názvů a klíče SAS poskytuje pověření, kterým služba Service Bus ověří přístup k aplikaci.

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="create-a-web-role"></a>Vytvoření webové role
V této části vytvoříte front-end své aplikace. Nejdřív vytvoříte stránky, které vaše aplikace zobrazí.
Potom přidáte kód, který odesílá položky do fronty Service Bus a zobrazí informace o stavu fronty.

### <a name="create-the-project"></a>Vytvoření projektu
1. Spusťte Visual Studio s právy správce. Visual Studio spustíte jako správce tak, že na ikonu programu **Visual Studio** kliknete pravým tlačítkem a vyberete možnost **Spustit jako správce**. Emulátor výpočtů v Azure, který se bude probírat později v tomto článku, potřebuje, aby bylo Visual Studio spuštěné s právy správce.
   
   Ve Visual Studiu v nabídce **Soubor** klikněte na **Nový** a pak na **Projekt**.
2. V **Nainstalovaných šablonách** v části **Visual C#** klikněte na **Cloud** a pak na **Cloudová služba Azure**. Jako název projektu zadejte **MultiTierApp**. Pak klikněte na **OK**.
   
   ![][9]
3. Z rolí **.NET Framework 4.5** poklikáním vyberte **Webovou roli ASP.NET**.
   
   ![][10]
4. Najeďte kurzorem na **WebRole1** v části **řešení Cloudové služby Azure**, klikněte na ikonu tužky a přejmenujte webovou roli na **FrontendWebRole**. Pak klikněte na **OK**. (Ujistěte se, že „Frontend“ zadáte s malým „e“ tzn. nikoli „FrontEnd“.)
   
   ![][11]
5. V dialogovém okně **Nový projekt ASP.NET** v seznamu **Vyberte šablonu** klikněte na **MVC**.
   
   ![][12]
6. Pořád ještě v dialogovém okně **Nový projekt ASP.NET** klikněte na tlačítko **Změna ověřování**. V dialogovém okně **Změna ověřování** klikněte na možnost **Bez ověřování** a poté klikněte na tlačítko **OK**. V tomto kurzu nasazujete aplikaci, která nepotřebuje přihlášení uživatele.
   
    ![][16]
7. Pořád ještě v dialogovém okně **Nový projekt ASP.NET** klikněte na tlačítko **OK** a vytvořte tak projekt.
8. V **Průzkumníku řešení** v projektu **FrontendWebRole** klikněte pravým tlačítkem na** Reference**, a pak klikněte na **Správa balíčků NuGet**.
9. Klikněte na kartu **Procházet** a potom najděte `Microsoft Azure Service Bus`. Klikněte na **Instalovat** a přijměte podmínky použití.
   
   ![][13]
   
   Poznámka: Teď jsou vytvořené reference na sestavení klienta a přidané některé nové soubory s kódem.
10. V **Průzkumníku řešení** klikněte pravým tlačítkem na **Modely**, pak klikněte na **Přidat** a pak na **Třída**. Do pole **Název** zadejte název **OnlineOrder.cs**. Pak klikněte na **Přidat**.

### <a name="write-the-code-for-your-web-role"></a>Napsání kódu pro vaši webovou roli
V této části vytvoříte stránky, které vaše aplikace zobrazí.

1. V souboru OnlineOrder.cs ve Visual Studiu nahraďte existující definici oboru názvů následujícím kódem.
   
   ```
   namespace FrontendWebRole.Models
   {
       public class OnlineOrder
       {
           public string Customer { get; set; }
           public string Product { get; set; }
       }
   }
   ```
2. V **Průzkumníku řešení** poklikejte na **Controllers\HomeController.cs**. Na začátek souboru přidejte následující příkazy **using**, které přidají obory názvů pro model, který jste právě vytvořili, a pro Service Bus.
   
   ```
   using FrontendWebRole.Models;
   using Microsoft.ServiceBus.Messaging;
   using Microsoft.ServiceBus;
   ```
3. v souboru HomeController.cs ve Visual Studiu taky místo existující definice oboru názvů zadejte následující kód. Tento kód obsahuje metody pro zpracování odesílání položek do fronty.
   
   ```
   namespace FrontendWebRole.Controllers
   {
       public class HomeController : Controller
       {
           public ActionResult Index()
           {
               // Simply redirect to Submit, since Submit will serve as the
               // front page of this application.
               return RedirectToAction("Submit");
           }
   
           public ActionResult About()
           {
               return View();
           }
   
           // GET: /Home/Submit.
           // Controller method for a view you will create for the submission
           // form.
           public ActionResult Submit()
           {
               // Will put code for displaying queue message count here.
   
               return View();
           }
   
           // POST: /Home/Submit.
           // Controller method for handling submissions from the submission
           // form.
           [HttpPost]
           // Attribute to help prevent cross-site scripting attacks and
           // cross-site request forgery.  
           [ValidateAntiForgeryToken]
           public ActionResult Submit(OnlineOrder order)
           {
               if (ModelState.IsValid)
               {
                   // Will put code for submitting to queue here.
   
                   return RedirectToAction("Submit");
               }
               else
               {
                   return View(order);
               }
           }
       }
   }
   ```
4. V nabídce **Sestavení** klikněte na **Sestavit řešení** a vyzkoušejte přesnost své dosavadní práce.
5. Teď vytvořte zobrazení pro metodu `Submit()`, kterou jste vytvořili předtím. Klikněte pravým tlačítkem do metody `Submit()` (přetížení `Submit()`, které nepřijímá žádné parametry), a pak vyberte **Přidat zobrazení**.
   
   ![][14]
6. Objeví se dialogové okno pro vytvoření zobrazení. V seznamu **Šablona** vyberte **Vytvořit**. V seznamu **Třída modelu** klikněte na třídu **OnlineOrder**.
   
   ![][15]
7. Klikněte na tlačítko **Přidat**.
8. Teď změňte zobrazený název vaší aplikace. V **Průzkumníku řešení** poklikejte na soubor **Views\Shared\\_Layout.cshtml** a otevře se v editoru Visual Studio.
9. Všechny výskyty **My ASP.NET Application** změňte na **LITWARE'S Products**.
10. Odstraňte odkazy **Home**, **About** a **Contact**. Odstraňte zvýrazněný uzel:
    
    ![][28]
11. Nakonec upravte stránku pro odesílání tak, aby obsahovala nějaké informace o frontě. V **Průzkumníku řešení** poklikejte na soubor **Views\Home\Submit.cshtml** a otevře se v editoru Visual Studio. Po `<h2>Submit</h2>` přidejte následující řádek. Hodnota `ViewBag.MessageCount` je zatím prázdná. Zaplníte ji později.
    
    ```
    <p>Current number of orders in queue waiting to be processed: @ViewBag.MessageCount</p>
    ```
12. Teď je implementované vaše uživatelské prostředí. Stisknutím klávesy **F5** můžete spustit svoji aplikaci a zkontrolovat, že vypadá tak, jak má.
    
    ![][17]

### <a name="write-the-code-for-submitting-items-to-a-service-bus-queue"></a>Napsání nového kódu pro odesílání položek do fronty Service Bus
Teď přidejte kód pro odesílání položek do fronty. Nejdřív vytvořte třídu, která obsahuje informace o připojení k vaší frontě Service Bus. Potom inicializujte připojení ze souboru Global.aspx.cs. Nakonec aktualizujte kód pro odesílání, který jste vytvořili předtím v souboru HomeController.cs tak, aby položky odesílal do fronty Service Bus.

1. V **Průzkumníku řešení** klikněte pravým tlačítkem na **FrontendWebRole** (pravým tlačítkem klikněte na projekt, ne na roli). Klikněte na **Přidat** a potom na **Třída**.
2. Zadejte název třídy **QueueConnector.cs**. Klikněte na **Přidat** a třída se vytvoří.
3. Teď přidejte kód, který bude obsahovat informace o připojení a inicializovat připojení k frontě Service Bus. Celý obsah souboru QueueConnector.cs nahraďte následujícím kódem a zadejte hodnoty pro `your Service Bus namespace` (název vašeho oboru názvů) a `yourKey` – to je **primární klíč**, který jste předtím získali z webu Azure Portal.
   
   ```
   using System;
   using System.Collections.Generic;
   using System.Linq;
   using System.Web;
   using Microsoft.ServiceBus.Messaging;
   using Microsoft.ServiceBus;
   
   namespace FrontendWebRole
   {
       public static class QueueConnector
       {
           // Thread-safe. Recommended that you cache rather than recreating it
           // on every request.
           public static QueueClient OrdersQueueClient;
   
           // Obtain these values from the portal.
           public const string Namespace = "your Service Bus namespace";
   
           // The name of your queue.
           public const string QueueName = "OrdersQueue";
   
           public static NamespaceManager CreateNamespaceManager()
           {
               // Create the namespace manager which gives you access to
               // management operations.
               var uri = ServiceBusEnvironment.CreateServiceUri(
                   "sb", Namespace, String.Empty);
               var tP = TokenProvider.CreateSharedAccessSignatureTokenProvider(
                   "RootManageSharedAccessKey", "yourKey");
               return new NamespaceManager(uri, tP);
           }
   
           public static void Initialize()
           {
               // Using Http to be friendly with outbound firewalls.
               ServiceBusEnvironment.SystemConnectivity.Mode =
                   ConnectivityMode.Http;
   
               // Create the namespace manager which gives you access to
               // management operations.
               var namespaceManager = CreateNamespaceManager();
   
               // Create the queue if it does not exist already.
               if (!namespaceManager.QueueExists(QueueName))
               {
                   namespaceManager.CreateQueue(QueueName);
               }
   
               // Get a client to the queue.
               var messagingFactory = MessagingFactory.Create(
                   namespaceManager.Address,
                   namespaceManager.Settings.TokenProvider);
               OrdersQueueClient = messagingFactory.CreateQueueClient(
                   "OrdersQueue");
           }
       }
   }
   ```
4. Teď musíte zajistit, aby se vaše metoda **Initialize** volala. V **Průzkumníku řešení** poklikejte na **Global.asax\Global.asax.cs**.
5. Přidejte následující řádek na konec metody **Application_Start**.
   
   ```
   FrontendWebRole.QueueConnector.Initialize();
   ```
6. Nakonec aktualizujte webový kód, který jste vytvořili předtím, aby se položky odesílaly do fronty. V **Průzkumníku řešení** poklikejte na **Controllers\HomeController.cs**.
7. Aktualizujte metodu `Submit()` (přetížení, které nepřijímá žádné parametry) následujícím způsobem, aby se získal počet zpráv pro frontu.
   
   ```
   public ActionResult Submit()
   {
       // Get a NamespaceManager which allows you to perform management and
       // diagnostic operations on your Service Bus queues.
       var namespaceManager = QueueConnector.CreateNamespaceManager();
   
       // Get the queue, and obtain the message count.
       var queue = namespaceManager.GetQueue(QueueConnector.QueueName);
       ViewBag.MessageCount = queue.MessageCount;
   
       return View();
   }
   ```
8. Aktualizujte metodu `Submit(OnlineOrder order)` (přetížení, které přijímá jeden parametr) následujícím způsobem, aby se do fronty odesílaly informace o objednávce.
   
   ```
   public ActionResult Submit(OnlineOrder order)
   {
       if (ModelState.IsValid)
       {
           // Create a message from the order.
           var message = new BrokeredMessage(order);
   
           // Submit the order.
           QueueConnector.OrdersQueueClient.Send(message);
           return RedirectToAction("Submit");
       }
       else
       {
           return View(order);
       }
   }
   ```
9. Teď můžete aplikaci znovu spustit. Pokaždé, když odešlete odbejdnávku, počet zpráv se zvýší.
   
   ![][18]

## <a name="create-the-worker-role"></a>Vytvoření role pracovního procesu
Teď vytvoříte roli pracovního procesu, která zpracuje odesílání objednávek. Tento příklad používá šablonu Visual Studia **Role pracovního procesu s frontou Service Bus**. Potřebné pověření jste už získali z portálu.

1. Zkontrolujte, že máte Visual Studio připojené ke svému účtu Azure.
2. Ve Visual Studiu v **Průzkumníku řešení** klikněte pravým tlačítkem na složku **Roles** v projektu **MultiTierApp**.
3. Klikněte na **Přidat**, a pak klikněte na **Nový projekt role pracovního procesu**. Zobrazí se dialogové okno **Přidat nový projekt role**.
   
   ![][26]
4. V dialogovém okně **Přidat nový projekt role** klikněte na **Role pracovního procesu s frontou Service Bus**.
   
   ![][23]
5. Do pole **Název** zadejte název projektu **OrderProcessingRole**. Pak klikněte na **Přidat**.
6. Připojovací řetězec, který jste získali v kroku 9 v části „Vytvoření oboru názvů Service Bus“ zkopírujte do schránky.
7. V **Průzkumníku řešení** klikněte pravým tlačítkem na roli **OrderProcessingRole**, které jste vytvořili v kroku 5 (ujistěte se, že kliknete pravým tlačítkem na **OrderProcessingRole** v seznamu **Role**, ne na třídu). Potom klikněte na **Vlastnosti**.
8. Na kartě **Nastavení** v dialogovém okně **Vlastnosti** klikněte do pole **Hodnota** pro **Microsoft.ServiceBus.ConnectionString** a vložte hodnotu koncového bodu, kterou jste zkopírovali v kroku 6.
   
   ![][25]
9. Vytvořte třídu **OnlineOrder**, která bude zastupovat objednávky při jejich zpracování z fronty. Můžete znovu použít třídu, kterou jste už vytvořili. V **Průzkumníku řešení** klikněte pravým tlačítkem na třídu **OrderProcessingRole** (pravým tlačítkem klikněte ikonu třídy, ne na roli). Klikněte na **Přidat**, pak klikněte na **Existující položka**.
10. Přejděte do podsložky pro **FrontendWebRole\Models** a poklikejte na **OnlineOrder.cs**, tím ho přidáte do projektu.
11. Ve **WorkerRole.cs** změňte hodnotu proměnné **QueueName** z `"ProcessingQueue"` na `"OrdersQueue"`, jak je vidět v následujícím kódu.
    
    ```
    // The name of your queue.
    const string QueueName = "OrdersQueue";
    ```
12. Na začátek souboru WorkerRole.cs přidejte následující příkaz using.
    
    ```
    using FrontendWebRole.Models;
    ```
13. Ve funkci `Run()` ve volání `OnMessage()` místo obsahu klauzule `try` vložte následující kód.
    
    ```
    Trace.WriteLine("Processing", receivedMessage.SequenceNumber.ToString());
    // View the message as an OnlineOrder.
    OnlineOrder order = receivedMessage.GetBody<OnlineOrder>();
    Trace.WriteLine(order.Customer + ": " + order.Product, "ProcessingMessage");
    receivedMessage.Complete();
    ```
14. Dokončili jste aplikaci. Celou aplikaci můžete vyzkoušet tak, že v Průzkumníku řešení kliknete pravým tlačítkem na projekt MultiTierApp, vyberete **Nastavit jako spouštěný projekt**, a pak stisknete F5. Všimněte si, že počet zpráv se nezvyšuje, protože role pracovního procesu zpracovává položky z fronty a označuje je jako hotové. Výstup své role pracovního procesu můžete sledovat v uživatelském prostředí Emulátoru výpočtů v Azure. To můžete udělat tak, že v oznamovací oblasti hlavního panelu kliknete pravým tlačítkem na ikonu emulátoru a vyberte **Zobrazit uživatelské prostředí emulátoru služby Compute**.
    
    ![][19]
    
    ![][20]

## <a name="next-steps"></a>Další kroky
Pokud se o službě Service Bus chcete dozvědět víc, pročtěte si následující zdroje:  

* [Azure Service Bus][sbmsdn]  
* [Stránka služeb Service Bus][sbwacom]  
* [Jak používat fronty služby Service Bus][sbwacomqhowto]  

Další informace o víceúrovňových scénářích najdete v:  

* [Vícevrstvá aplikace .NET cloudových služeb Azure, která používá tabulky, fronty a objekty blob][mutitierstorage]  

[0]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-01.png
[1]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-100.png
[2]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-101.png
[Stažení nástrojů a SDK]: http://go.microsoft.com/fwlink/?LinkId=271920


[GetSetting]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.cloudconfigurationmanager.getsetting.aspx
[Microsoft.WindowsAzure.Configuration.CloudConfigurationManager]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.cloudconfigurationmanager.aspx
[NamespaceMananger]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx

[QueueClient]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.aspx

[TopicClient]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicclient.aspx

[EventHubClient]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventhubclient.aspx

[9]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-10.png
[10]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-11.png
[11]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-02.png
[12]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-12.png
[13]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-13.png
[14]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-33.png
[15]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-34.png
[16]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-14.png
[17]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-36.png
[18]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-37.png

[19]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-38.png
[20]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-39.png
[23]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/SBWorkerRole1.png
[25]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/SBWorkerRoleProperties.png
[26]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/SBNewWorkerRole.png
[28]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-40.png

[sbmsdn]: http://msdn.microsoft.com/library/azure/ee732537.aspx  
[sbwacom]: /documentation/services/service-bus/  
[sbwacomqhowto]: service-bus-dotnet-get-started-with-queues.md  
[mutitierstorage]: https://code.msdn.microsoft.com/Windows-Azure-Multi-Tier-eadceb36



<!--HONumber=Nov16_HO3-->


