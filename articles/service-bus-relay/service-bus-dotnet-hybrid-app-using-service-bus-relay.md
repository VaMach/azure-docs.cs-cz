<properties
    pageTitle="Hybridní lokální/cloudová aplikace (.NET) | Microsoft Azure"
    description="Naučte se vytvořit hybridní lokální/cloudovou aplikaci .NET s předáváním přes Azure Service Bus."
    services="service-bus"
    documentationCenter=".net"
    authors="sethmanheim"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="09/16/2016"
    ms.author="sethm"/>


# <a name=".net-on-premises/cloud-hybrid-application-using-azure-service-bus-relay"></a>Hybridní lokální/cloudová aplikace .NET s předáváním přes Azure Service Bus

## <a name="introduction"></a>Úvod

Tento článek popisuje, jak vytvořit hybridní cloudovou aplikaci pomocí Microsoft Azure a Visual Studia. Tento kurz předpokládá, že nemáte žádné předchozí zkušenosti s používáním Azure. Za méně než 30 minut budete mít aplikaci, která používá několik různých prostředků Azure a běží v cloudu.

Co se dozvíte:

-   Jak vytvořit nebo přizpůsobit existující webovou službu pro spotřebu webovým řešením.
-   Jak používat službu předávání přes Azure Service Bus ke sdílení dat mezi aplikací Azure a webovou službou hostovanou jinde.

[AZURE.INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="how-the-service-bus-relay-helps-with-hybrid-solutions"></a>Jak předávání přes Service Bus pomáhá s hybridními řešeními

Podniková řešení se obvykle skládají z kombinace vlastního kódu napsaného pro řešení nových a jedinečných podnikových řešení a stávajících funkcí poskytovaných řešeními a systémy, které již existují.

Architekti řešení začínají používat cloud, protože jim to umožňuje snadněji zvládat nároky na škálování a snížit provozní náklady. Přitom zjišťují, že existující prostředky služeb, které by chtěli využívat jako stavební prvky pro svá řešení, jsou za firemním firewallem a cloudové řešení k nim nemá snadný přístup. Spousta interních služeb není postavená nebo hostovaná tak, aby se dala snadno vystavit na rozhraní firemní sítě.

Předávání přes Service Bus je navržené pro situace, kdy je potřeba vzít existující webové služby WCF (Windows Communication Foundation) a bezpečně je zpřístupnit pro řešení, která jsou mimo firemní zónu, a to bez nutnosti provádět nežádoucí změny infrastruktury podnikové sítě. Takové služby předávání přes Service Bus se stále hostují uvnitř existujícího prostředí, ale delegují čekání na příchozí spojení a požadavky na Service Bus hostovaný v cloudu. Service Bus taky takové služby chrání před neoprávněným přístupem pomocí ověření [Sdíleným přístupovým podpisem](../service-bus-messaging/service-bus-sas-overview.md) (SAS).

## <a name="solution-scenario"></a>Scénář řešení

V tomto kurzu vytvoříte webovou stránku ASP.NET, která vám umožní zobrazit seznam produktů na stránce inventáře produktů.

![][0]

Kurz předpokládá, že máte produktové informace dostupné v existujícím lokálním systému, a používá předávání přes Service Bus k získání přístupu do takového systému. To se simuluje jako webová služba, která spustí jednoduchou konzolovou aplikaci a využívá sadu produktů uloženou v paměti. Taky si vyzkoušíte spuštění této konzolové aplikace na svém vlastním počítači a nasazení webové role do Azure. Uvidíte tak, jak webová role běžící v datovém centru Azure skutečně zavolá do vašeho počítače, i když se váš počítač skoro určitě nachází za nejméně jedním firewallem a vrstvou překládání adres (NAT).

Toto je snímek obrazovky úvodní stránky hotové webové aplikace.

![][1]

## <a name="set-up-the-development-environment"></a>Nastavení vývojového prostředí

Než začnete s vývojem aplikací pro Azure, připravte si nástroje a vývojové prostředí.

1.  Nainstalujte sadu Azure SDK pro .NET ze stránky [Stažení nástrojů a SDK][].

2.  Klikněte na **Instalovat sadu SDK** pro verzi Visual Studia, kterou používáte. Kroky v tomto kurzu ukazují postup ve Visual Studiu 2015.

4.  Když se zobrazí dialog pro spuštění nebo uložení instalačního programu, klikněte na **Spustit**.

5.  V **Instalačním programu webové platformy** klikněte na **Instalovat** a pokračujte v instalaci.

6.  Po dokončení instalace budete mít všechno, co je potřeba k vývoji aplikace. Sada SDK obsahuje nástroje, které vám umožní snadno vyvíjet aplikace pro Azure ve Visual Studiu. Pokud nemáte Visual Studio nainstalované, SDK taky nainstaluje bezplatnou verzi Visual Studio Express.

## <a name="create-a-namespace"></a>Vytvoření oboru názvů

Pokud chcete začít používat funkce Service Bus v Azure, musíte nejdřív vytvořit obor názvů služby. Obor názvů poskytuje kontejner oboru pro adresování prostředků služby Service Bus v rámci vaší aplikace.

[AZURE.INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="create-an-on-premises-server"></a>Vytvoření lokálního serveru

Nejdřív vytvoříte lokální (testovací) systém katalogu produktů. Bude vcelku jednoduchý a nahradí skutečný lokální systém katalogu produktů, i s kompletní rovinou služeb, kterou chceme integrovat.

Tento projekt je konzolová aplikace z Visual Studia a pomocí [balíčku NuGet pro Azure Service Bus](https://www.nuget.org/packages/WindowsAzure.ServiceBus/) zahrnuje konfiguračních nastavení a knihovny Service Bus.

### <a name="create-the-project"></a>Vytvoření projektu

1.  Spusťte Visual Studio s právy správce. Visual Studio spustíte jako správce tak, že na ikonu programu **Visual Studio** kliknete pravým tlačítkem a vyberete možnost **Spustit jako správce**.

2.  Ve Visual Studiu v nabídce **Soubor** klikněte na **Nový** a pak na **Projekt**.

3.  V **Nainstalovaných šablonách** v části **Visual C#** klikněte na **Konzolová aplikace**. Do pole **Název** zadejte název **ProductsServer**:

    ![][11]

4.  Kliknutím na tlačítko **OK** vytvořte projekt **ProductsServer**.

7.  Pokud jste už nainstalovali správce balíčků NuGet pro Visual Studio, přejděte na další krok. Pokud ne, přejděte na [NuGet][] a klikněte na [Nainstalovat NuGet](http://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c). Podle pokynů nainstalujte správce balíčků NuGet, a pak znovu spusťte Visual Studio.

7.  V Průzkumníku řešení klikněte pravým tlačítkem na projekt **ProductsServer**, pak klikněte na **Správa balíčků NuGet**.

8.  Klikněte na kartu **Procházet** a potom najděte `Microsoft Azure Service Bus`. Klikněte na **Instalovat** a přijměte podmínky použití.

    ![][13]

    Poznámka: Teď jsou vytvořené reference na sestavení klienta.

9.  Přidejte novou třídu pro váš produktový kontrakt. V Průzkumníku řešení klikněte pravým tlačítkem na projekt **ProductsServer**, pak klikněte na **Přidat** a pak na **Třída**.

10. Do pole **Název** zadejte název **ProductsContract.cs**. Pak klikněte na **Přidat**.

11. V **ProductsContract.cs** místo definice oboru názvů zadejte následující kód, který definuje kontrakt služby.

    ```
    namespace ProductsServer
    {
        using System.Collections.Generic;
        using System.Runtime.Serialization;
        using System.ServiceModel;
    
        // Define the data contract for the service
        [DataContract]
        // Declare the serializable properties.
        public class ProductData
        {
            [DataMember]
            public string Id { get; set; }
            [DataMember]
            public string Name { get; set; }
            [DataMember]
            public string Quantity { get; set; }
        }
    
        // Define the service contract.
        [ServiceContract]
        interface IProducts
        {
            [OperationContract]
            IList<ProductData> GetProducts();
    
        }
    
        interface IProductsChannel : IProducts, IClientChannel
        {
        }
    }
    ```

12. V Program.cs místo definice oboru názvů zadejte následující kód, který přidá službu profilu a hosta pro ni.

    ```
    namespace ProductsServer
    {
        using System;
        using System.Linq;
        using System.Collections.Generic;
        using System.ServiceModel;
    
        // Implement the IProducts interface.
        class ProductsService : IProducts
        {
    
            // Populate array of products for display on website
            ProductData[] products =
                new []
                    {
                        new ProductData{ Id = "1", Name = "Rock",
                                         Quantity = "1"},
                        new ProductData{ Id = "2", Name = "Paper",
                                         Quantity = "3"},
                        new ProductData{ Id = "3", Name = "Scissors",
                                         Quantity = "5"},
                        new ProductData{ Id = "4", Name = "Well",
                                         Quantity = "2500"},
                    };
    
            // Display a message in the service console application
            // when the list of products is retrieved.
            public IList<ProductData> GetProducts()
            {
                Console.WriteLine("GetProducts called.");
                return products;
            }
    
        }
    
        class Program
        {
            // Define the Main() function in the service application.
            static void Main(string[] args)
            {
                var sh = new ServiceHost(typeof(ProductsService));
                sh.Open();
    
                Console.WriteLine("Press ENTER to close");
                Console.ReadLine();
    
                sh.Close();
            }
        }
    }
    ```

13. V Průzkumníku řešení poklikejte na soubor **App.config**.cs a otevře se v editoru Visual Studio. Na konci elementu **&lt;system.ServiceModel&gt;** (ale stále ještě v &lt;system.ServiceModel&gt;) přidejte následující kód XML. Nezapomeňte místo *yourServiceNamespace* zadat název vašeho oboru názvů a místo *yourKey* váš SAS klíč, který jste předtím získali z portálu:

    ```
    <system.serviceModel>
    ...
      <services>
         <service name="ProductsServer.ProductsService">
           <endpoint address="sb://yourServiceNamespace.servicebus.windows.net/products" binding="netTcpRelayBinding" contract="ProductsServer.IProducts" behaviorConfiguration="products"/>
         </service>
      </services>
      <behaviors>
         <endpointBehaviors>
           <behavior name="products">
             <transportClientEndpointBehavior>
                <tokenProvider>
                   <sharedAccessSignature keyName="RootManageSharedAccessKey" key="yourKey" />
                </tokenProvider>
             </transportClientEndpointBehavior>
           </behavior>
         </endpointBehaviors>
      </behaviors>
    </system.serviceModel>
    ```
14. Pořád ještě v souboru App.config v elementu **&lt;appSettings&gt;** místo hodnoty připojovacího řetězce zadejte připojovací řetězec, který jste předtím získali z portálu. 

    ```
    <appSettings>
    <!-- Service Bus specific app settings for messaging connections -->
    <add key="Microsoft.ServiceBus.ConnectionString"
           value="Endpoint=sb://yourNamespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=yourKey"/>
    </appSettings>
    ```

14. Stiskněte **Ctrl+Shift+B** nebo v nabídce **Sestavení** klikněte na **Sestavit řešení** a tím sestavte aplikaci a potvrďte přesnost své dosavadní práce.

## <a name="create-an-asp.net-application"></a>Vytvoření aplikace ASP.NET

V této části sestavíte jednoduchou aplikaci ASP.NET, která zobrazí data načtená z vaší produktové služby.

### <a name="create-the-project"></a>Vytvoření projektu

1.  Zkontrolkujte, že je Visual Studio spuštěné s právy správce.

2.  Ve Visual Studiu v nabídce **Soubor** klikněte na **Nový** a pak na **Projekt**.

3.  V **Nainstalovaných šablonách** v části **Visual C#** klikněte na **Webová aplikace ASP.NET**. Jako název projektu zadejte **ProductsPortal**. Pak klikněte na **OK**.

    ![][15]

4.  V seznamu **Vyberte šablonu** klikněte na **MVC**. 

6.  Označte zatržítko **Hostitel v cloudu**.

    ![][16]

5. Klikněte na tlačítko **Změna ověřování**. V dialogovém okně **Změna ověřování** klikněte na možnost **Bez ověřování** a poté klikněte na tlačítko **OK**. V tomto kurzu nasazujete aplikaci, která nepotřebuje přihlášení uživatele.

    ![][18]

6.  Ujistěte se, zda je v části **Microsoft Azure** v dialogovém okně **Nový projekt ASP.NET** zaškrtnuta položka **Hostovat v cloudu** a zda je v rozevíracím seznamu vybrána položka **App Service**.

    ![][19]

7. Klikněte na tlačítko **OK**. 

8. Teď musíte nakonfigurovat prostředky Azure pro novou webovou aplikaci. Postupujte podle pokynů v části [Konfigurace prostředků Azure pro novou webovou aplikaci](../app-service-web/web-sites-dotnet-get-started.md#configure-azure-resources-for-a-new-web-app). Potom se vraťte do tohoto kurzu a pokračujte dalším krokem.

5.  V Průzkumníkovi řešení klikněte pravým tlačítkem na **Modely**, pak levým na **Přidat** a pak na **Třída**. Do pole **Název** zadejte název **Product.cs**: Pak klikněte na **Přidat**.

    ![][17]

### <a name="modify-the-web-application"></a>Úprava webové aplikace

1.  V souboru Product.cs ve Visual Studiu nahraďte existující definici oboru názvů následujícím kódem.

    ```
    // Declare properties for the products inventory.
    namespace ProductsWeb.Models
    {
        public class Product
        {
            public string Id { get; set; }
            public string Name { get; set; }
            public string Quantity { get; set; }
        }
    }
    ```

2.  V Průzkumníku řešení rozbalte složku **Kontrolery**, pak poklikejte na soubor **HomeController.cs** a ten se otevře ve Visual Studiu.

3. V souboru **HomeController.cs** nahraďte existující definici oboru názvů následujícím kódem.

    ```
    namespace ProductsWeb.Controllers
    {
        using System.Collections.Generic;
        using System.Web.Mvc;
        using Models;
    
        public class HomeController : Controller
        {
            // Return a view of the products inventory.
            public ActionResult Index(string Identifier, string ProductName)
            {
                var products = new List<Product>
                    {new Product {Id = Identifier, Name = ProductName}};
                return View(products);
            }
         }
    }
    ```

3.  V Průzkumníku řešení rozbalte složku Views\Shared, pak poklikejte na soubor **_Layout.cshtml** a ten se otevře v editoru Visual Studia.

5.  Všechny výskyty **My ASP.NET Application** změňte na **LITWARE's Products**.

6. Odstraňte odkazy **Home**, **About** a **Contact**. V následujícím příkladu odstraňte zvýrazněný kód.

    ![][41]

7.  V Průzkumníku řešení rozbalte složku Views\Home, pak poklikejte na soubor **Index.cshtml** a ten se otevře v editoru Visual Studia.
    Celý obsah souboru nahraďte následujícím kódem.

    ```
    @model IEnumerable<ProductsWeb.Models.Product>
    
    @{
            ViewBag.Title = "Index";
    }
    
    <h2>Prod Inventory</h2>
    
    <table>
            <tr>
                <th>
                    @Html.DisplayNameFor(model => model.Name)
                </th>
                  <th></th>
                <th>
                    @Html.DisplayNameFor(model => model.Quantity)
                </th>
            </tr>
    
    @foreach (var item in Model) {
            <tr>
                <td>
                    @Html.DisplayFor(modelItem => item.Name)
                </td>
                <td>
                    @Html.DisplayFor(modelItem => item.Quantity)
                </td>
            </tr>
    }
    
    </table>
    ```

9.  Pokud chcete zkontrolovat přesnost svojí dosavadní práce, můžete stisknout **Ctrl+Shift+B** a tím projekt sestavit.


### <a name="run-the-app-locally"></a>Místní spuštění aplikace

Spusťte aplikace pro kontrolu, že funguje.

1.  Zkontrolujte, že aktivní projekt je **ProductsPortal**. V Průzkumníku řešení klikněte pravým tlačítkem na název projektu a klikněte na možnost **Nastavit jako spouštěný projekt**.
2.  Ve Visual Studiu stiskněte F5.
3.  Měla by se objevit vaše aplikace, jak běží v prohlížeči.

    ![][21]

## <a name="put-the-pieces-together"></a>Složení částí do jednoho celku

Dalším krokem je spojit lokální produktový server s aplikací ASP.NET.

1.  Pokud v aplikaci Visual Studio není otevřený projekt **ProductsPortal**, který jste vytvořili v části [Vytvoření aplikace ASP.NET](#create-an-aspnet-application), znovu ho otevřete.

2.  Podobně jako v části Vytvoření lokálního serveru přidejte do referencí projektu balíček NuGet. V Průzkumníku řešení klikněte pravým tlačítkem na projekt **ProductsPortal**, pak klikněte na **Správa balíčků NuGet**.

3.  Vyhledejte „Service Bus“ a vyberte položku **Microsoft Azure Service Bus**. Potom zavřete dialogové okno, tím dokončíte instalaci.

4.  V Průzkumníku řešení klikněte pravým tlačítkem na projekt **ProductsPortal**, pak klikněte na **Přidat** a pak na **Existující položka**.

5.  Přejděte na soubor **ProductsContract.cs** v konzolovém projektu **ProductsServer**. Kliknutím zvýrazněte ProductsContract.cs. Klikněte na šipku dolů vedle tlačítka **Přidat**, pak klikněte na **Přidat jako odkaz**.

    ![][24]

6.  Teď ve Visual Studiu otevřete soubor **HomeController.cs** a nahraďte definici oboru názvů následujícím kódem. Nezapomeňte místo *yourServiceNamespace* zadat název vašeho oboru názvů služby a místo *yourKey* váš SAS klíč. To klientovi umožní zavolat lokální službu a vrátit výsledek volání.

    ```
    namespace ProductsWeb.Controllers
    {
        using System.Linq;
        using System.ServiceModel;
        using System.Web.Mvc;
        using Microsoft.ServiceBus;
        using Models;
        using ProductsServer;
    
        public class HomeController : Controller
        {
            // Declare the channel factory.
            static ChannelFactory<IProductsChannel> channelFactory;
    
            static HomeController()
            {
                // Create shared access signature token credentials for authentication.
                channelFactory = new ChannelFactory<IProductsChannel>(new NetTcpRelayBinding(),
                    "sb://yourServiceNamespace.servicebus.windows.net/products");
                channelFactory.Endpoint.Behaviors.Add(new TransportClientEndpointBehavior {
                    TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider(
                        "RootManageSharedAccessKey", "yourKey") });
            }
    
            public ActionResult Index()
            {
                using (IProductsChannel channel = channelFactory.CreateChannel())
                {
                    // Return a view of the products inventory.
                    return this.View(from prod in channel.GetProducts()
                                     select
                                         new Product { Id = prod.Id, Name = prod.Name,
                                             Quantity = prod.Quantity });
                }
            }
        }
    }
    ```

7.  V Průzkumníku řešení klikněte pravým tlačítkem na řešení **ProductsPortal** (klikněte skutečně na řešení, ne na projekt). Klikněte na **Přidat** a pak klikněte na **Existující projekt**.

8.  Přejděte do projektu **ProductsServer**, pak poklikejte na řešení **ProductsServer.csproj** a tím ho přidejte.

9.  Aby se data mohla zobrazit na **ProductsPortal**, musí běžet **ProductsServer**. V Průzkumníku řešení klikněte pravým tlačítkem na řešení **ProductsPortal** a pak na **Vlastnosti**. Zobrazí se dialogové okno **Stránky vlastností**.

10. Na levé straně klikněte na **Spouštěný projekt**. Na pravé straně klikněte na **Více projektů po spuštění**. Zkontrolujte, že se objeví **ProductsServer** a **ProductsPortal** v tomto pořadí, a že je pro obojí nastavená akce **Start**.

      ![][25]

11. Pořád ještě v dialogovém okně **Vlastnosti** na levé straně klikněte na **Závislosti projektu**.

12. V seznamu **Projekty** klikněte na **ProductsServer**. Zkontrolujte, že **ProductsPortal** **není** vybraný.

14. V seznamu **Projekty** klikněte na **ProductsPortal**. Zkontrolujte, že je **ProductsServer** vybraný. 

    ![][26]

15. Kliknutím na **OK** zavřete se dialogové okno **Stránky vlastností**.

## <a name="run-the-project-locally"></a>Spusťte projekt lokálně.

Aplikaci můžete lokálně spustit a otestovat ve Visual Studiu stisknutím klávesy **F5**. Nejdřív by se měl spustit lokální server (**ProductsServer**), potom by se v okně prohlížeče měla spustit aplikace **ProductsPortal**. Tentokrát uvidíte, že inventář produktů zobrazí seznam dat načtených z lokálního systému služby.

![][10]

Na stránce **ProductsPortal** stiskněte **Obnovit**. Pokaždé, když obnovíte stránku, měla by aplikace serveru při volání `GetProducts()` z **ProductsServer** zobrazit zprávu.

Před dalším krokem zavřete obě aplikace.

## <a name="deploy-the-productsportal-project-to-an-azure-web-app"></a>Nasazení projektu ProductsPortal do webové aplikace Azure

Dalším krokem je převod frontendu **ProductsPortal** do webové aplikace Azure. Nejdřív nasaďte projekt **ProductsPortal**, potom proveďte všechny kroky v části [Nasazení webového projektu do webové aplikace Azure](../app-service-web/web-sites-dotnet-get-started.md#deploy-the-web-project-to-the-azure-web-app). Po dokončení nasazení se vraťte do tohoto kurzu a pokračujte dalším krokem.

> [AZURE.NOTE] V okně prohlížeče se při automatickém spuštění webového projektu **ProductsPortal** po nasazení může objevit chybová zpráva. To je v pořádku a děje se to, protože aplikace **ProductsServer** ještě neběží.

Zkopírujte adresu URL nasazené aplikace protože ji budete potřebovat v dalším kroku. Tuto adresu URL můžete taky získat z okna Aktivita služby Azure App Service ve Visual Studiu:

![][9] 

### <a name="set-productsportal-as-web-app"></a>Nastavení ProductsPortal jako webové aplikace

Než spustíte aplikaci v cloudu, musíte zkontrolovat, že se **ProductsPortal** spustí z Visual Studia jak webová aplikace.

1. Ve Visual Studiu klikněte pravým tlačítkem na projekt **ProjectsPortal** a potom klikněte na **Vlastnosti**.

3. V levém sloupci klikněte na **Web**.

5. V části **Spustit akci** klikněte na tlačítko **Otevřít adresu URL** a do textového pole zadejte URL webové aplikace nasazené předtím, například `http://productsportal1234567890.azurewebsites.net/`.

    ![][27]

6. Ve Visual Studiu zvolte v nabídce **Soubor** možnost **Uložit vše**.

7. Ve Visual Studiu zvolte v nabídce Sestavení možnost **Znovu sestavit řešení**.

## <a name="run-the-application"></a>Spuštění aplikace

2.  Stisknutím klávesy F5 aplikaci sestavíte a spustíte. Nejdřív by se měl spustit lokální server (konzolová aplikace **ProductsServer**), potom by se v okně prohlížeče měla spustit aplikace **ProductsPortal**, jak je vidět na tomto snímku obrazovky. Znovu si všimněte, že inventář produktů zobrazí seznam dat načtených z lokálního systému služby a tato data zobrazí ve webové aplikaci. Zkontrolujte adresu URL a ujistěte se, že **ProductsPortal** běží v cloudu jako webová aplikace Azure. 

    ![][1]

    > [AZURE.IMPORTANT] Konzolová aplikace **ProductsServer** musí běžet a musí být schopná odesílat data do aplikace **ProductsPortal**. pokud prohlížeč zobrazí chybu, počkejte několik dalších sekund, než se načte **ProductsServer** a zobrazí se následující zpráva. Potom v prohlížeči stiskněte **Obnovit**.

    ![][37]

3. Zpátky v prohlížeči na stránce **ProductsPortal** stiskněte **Obnovit**. Pokaždé, když obnovíte stránku, měla by aplikace serveru při volání `GetProducts()` z **ProductsServer** zobrazit zprávu.

    ![][38]

## <a name="next-steps"></a>Další kroky  

Pokud se o službě Service Bus chcete dozvědět víc, pročtěte si následující zdroje:  

* [Azure Service Bus][sbwacom]  
* [Jak používat fronty služby Service Bus][sbwacomqhowto]  


  [0]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hybrid.png
  [1]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/App2.png
  [Stažení nástrojů a sady SDK]: http://go.microsoft.com/fwlink/?LinkId=271920
  [NuGet]: http://nuget.org
  
  [11]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-con-1.png
  [13]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/getting-started-multi-tier-13.png
  [15]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-2.png
  [16]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-4.png
  [17]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-7.png
  [18]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-5.png
  [19]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-6.png
  [9]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-9.png
  [10]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/App3.png

  [21]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/App1.png
  [24]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-12.png
  [25]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-13.png
  [26]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-14.png
  [27]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-8.png
  
  [36]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/App2.png
  [37]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-service1.png
  [38]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-service2.png
  [41]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/getting-started-multi-tier-40.png
  [43]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/getting-started-hybrid-43.png


  [sbwacom]: /documentation/services/service-bus/  
  [sbwacomqhowto]: ../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md




<!--HONumber=Oct16_HO3-->


