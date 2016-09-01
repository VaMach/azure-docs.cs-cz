<properties
    pageTitle="Nabízená oznámení v monitorované geografické zóně s Azure Notification Hubs a Bing Spatial Data | Microsoft Azure"
    description="V tomto kurzu se dozvíte, jak pomocí Azure Notification Hubs a Bing Spatial Data doručovat nabízená oznámení na základě polohy."
    services="notification-hubs"
    documentationCenter="windows"
    keywords="nabízené oznámení,nabízená oznámení"
    authors="dend"
    manager="yuaxu"
    editor="dend"/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows-phone"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="05/31/2016"
    ms.author="dendeli"/>
    
# Nabízená oznámení v monitorované geografické zóně s Azure Notification Hubs a Bing Spatial Data
 
 > [AZURE.NOTE] K dokončení tohoto kurzu potřebujete mít aktivní účet Azure. Pokud účet nemáte, můžete si během několika minut vytvořit bezplatný zkušební účet. Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02).

V tomto kurzu se dozvíte, jak z aplikace pro Univerzální platformu Windows doručovat pomocí Azure Notification Hubs a Bing Spatial Data nabízená oznámení na základě polohy.

##Požadavky
Nejprve je nezbytné se ujistit, že splňujete všechny požadavky na software a služby:

* [Visual Studio 2015 Update 1](https://www.visualstudio.com/en-us/downloads/download-visual-studio-vs.aspx) nebo novější (dostačující bude i [Community Edition](https://go.microsoft.com/fwlink/?LinkId=691978&clcid=0x409)) 
* Nejnovější verze [Azure SDK](https://azure.microsoft.com/downloads/) 
* [Účet na webu Dev Center pro Mapy Bing](https://www.bingmapsportal.com/) (Je možné si jej vytvořit zdarma a přidružit si ho k účtu Microsoft.) 

##Začínáme

Začněme vytvořením projektu. V nástroji Visual Studio vytvořte nový projekt typu **Prázdná aplikace (univerzální pro Windows)**.

![](./media/notification-hubs-geofence/notification-hubs-create-blank-app.png)

Jakmile se vytváření projektu dokončí, měli byste mít základ samotné aplikace. Nyní nastavme vše pro monitorovanou geografickou zónu. Jelikož pro tento účel využijeme služby Bing, je k dispozici veřejný koncový bod REST API, který nám umožní dotazovat se na konkrétní oblasti lokality:

    http://spatial.virtualearth.net/REST/v1/data/
    
K jeho zprovoznění je nutné zadat následující parametry:

* **ID zdroje dat** a **Název zdroje dat** – v rozhraní API Map Bing zdroje dat obsahují různá kategorizovaná metadata, například lokality a pracovní doby provozu. Můžete si o nich zde přečíst více. 
* **Název entity** – entita, kterou chcete použít jako referenční bod pro oznámení. 
* **Klíč rozhraní API Map Bing** – klíč, který jste dříve získali při vytváření účtu Dev Center pro Bing.
 
Podrobně nyní popíšeme nastavení jednotlivých prvků uvedených výše.

##Nastavení zdroje dat

Nastavení je možné provést na webu Dev Center pro Mapy Bing. Jednoduše v horním navigačním panelu klikněte na **Zdroje dat** a vyberte **Spravovat zdroje dat**.

![](./media/notification-hubs-geofence/bing-maps-manage-data.png)

Pokud jste s rozhraními API Map Bing ještě nepracovali, nejspíše nebudou k dispozici žádné zdroje dat, tudíž stačí jeden vytvořit kliknutím na Nahrát data do zdroje dat. Nezapomeňte vyplnit všechna povinná pole:

![](./media/notification-hubs-geofence/bing-maps-create-data.png)

Možná se ptáte: co je to datový soubor a co byste měli nahrávat? Pro účely tohoto testu můžeme jednoduše použít vyznačenou oblast kolem nábřeží v San Franciscu, kterou definujeme jako soubor .pipe:

    Bing Spatial Data Services, 1.0, TestBoundaries
    EntityID(Edm.String,primaryKey)|Name(Edm.String)|Longitude(Edm.Double)|Latitude(Edm.Double)|Boundary(Edm.Geography)
    1|SanFranciscoPier|||POLYGON ((-122.389825 37.776598,-122.389438 37.773087,-122.381885 37.771849,-122.382186 37.777022,-122.389825 37.776598))
    
Kód výše představuje tuto entitu:

![](./media/notification-hubs-geofence/bing-maps-geofence.png)

Jednoduše zkopírujte a vložte řetězec výše do nového souboru, který pak uložíte jako **NotificationHubsGeofence.pipe** a nahrajete na Dev Center pro Bing.

>[AZURE.NOTE]Může se zobrazit výzva k zadání nového klíče jako **hlavního klíče**, který se bude lišit od **klíče dotazu**. Nový klíč jednoduše vytvořte přes řídicí panel a aktualizujte stránku pro nahrání zdroje dat.

Jakmile nahrajete datový soubor, bude nezbytné publikovat zdroj dat. 

Přejděte na **Správa zdrojů dat** postupem uvedeným výše, najděte v seznamu zdroj dat a ve sloupci **Akce** klikněte na **Publikovat**. Po chvíli byste měli zdroj dat vidět na kartě **Publikované zdroje dat**:

![](./media/notification-hubs-geofence/bing-maps-published-data.png)

Pokud kliknete na **Upravit**, ihned uvidíte, které lokality jsme do něj zahrnuli:

![](./media/notification-hubs-geofence/bing-maps-data-details.png)

V tuto chvíli portál nezobrazuje hranice monitorované geografické zóny, kterou jsme vytvořili. Potřebujeme pouze potvrzení, že zadaná lokalita je ve správném okolí.

Nyní máte všechny požadavky na zdroj dat. Pokud chcete získat podrobnosti o adrese URL žádosti o volání rozhraní API, klikněte na webu Dev Center pro Mapy Bing na **Zdroje dat** a vyberte **Informace o zdroji dat**.

![](./media/notification-hubs-geofence/bing-maps-data-info.png)

**URL dotazu** je to, co zde hledáme. Toto je koncový bod, oproti kterému můžeme spouštět dotazy, abychom zjistili, jestli se zařízení aktuálně nachází uvnitř lokality nebo ne. K provedení této kontroly potřebujeme jen spustit volání GET s následujícími parametry vůči adrese URL dotazu:

    ?spatialFilter=intersects(%27POINT%20LONGITUDE%20LATITUDE)%27)&$format=json&key=QUERY_KEY

Tímto způsobem určujete cílový bod, který získáme ze zařízení, a služba Mapy Bing automaticky provede výpočet a určí, jestli se bod nachází uvnitř monitorové geografické zóny. Jakmile požadavek spustíte přes prohlížeč (nebo adresu cURL), obdržíte standardní odpověď JSON:

![](./media/notification-hubs-geofence/bing-maps-json.png)

Tato odpověď se odesílá jen v případě, že se bod nachází v určené oblasti. Pokud se tam nenachází, obdržíte prázdný kontejner **výsledku**:

![](./media/notification-hubs-geofence/bing-maps-nores.png)

##Nastavení aplikace pro UPW

Nyní když máme připraven zdroj dat, můžeme začít pracovat na aplikaci pro UPW, kterou jsme si připravili dříve.

Nejprve musíme pro naši aplikaci povolit zjišťování polohy. To proveďte kliknutím dvakrát na soubor `Package.appxmanifest` v **Průzkumníkovi řešení**.

![](./media/notification-hubs-geofence/vs-package-manifest.png)

Na kartě vlastností balíčku, která se právě otevřela, klikněte na **Schopnosti** a ujistěte se, že je vybrána možnost **Poloha**:

![](./media/notification-hubs-geofence/vs-package-location.png)

Protože schopnost zjišťovat polohu je teď deklarována, vytvořte v řešení novou složku s názvem `Core` a přidejte do ní nový soubor pojmenovaný `LocationHelper.cs`:

![](./media/notification-hubs-geofence/vs-location-helper.png)

Třída `LocationHelper` je v tuto chvíli vcelku jednoduchá – umožňuje nám pouze získat polohu uživatele přes systémové rozhraní API.

    using System;
    using System.Threading.Tasks;
    using Windows.Devices.Geolocation;

    namespace NotificationHubs.Geofence.Core
    {
        public class LocationHelper
        {
            private static readonly uint AppDesiredAccuracyInMeters = 10;

            public async static Task<Geoposition> GetCurrentLocation()
            {
                var accessStatus = await Geolocator.RequestAccessAsync();
                switch (accessStatus)
                {
                    case GeolocationAccessStatus.Allowed:
                        {
                            Geolocator geolocator = new Geolocator { DesiredAccuracyInMeters = AppDesiredAccuracyInMeters };

                            return await geolocator.GetGeopositionAsync();
                        }
                    default:
                        {
                            return null;
                        }
                }
            }

        }
    }

Další informace o získávání polohy uživatel v aplikacích pro UPW získáte v oficiálním [dokumentu MSDN](https://msdn.microsoft.com/library/windows/apps/mt219698.aspx).

Pokud chcete zkontrolovat, že získávání polohy skutečně funguje, otevřete kód hlavní stránky (`MainPage.xaml.cs`). Vytvořte novou obslužnou rutinu události pro událost `Loaded` v konstruktoru `MainPage`:

    public MainPage()
    {
        this.InitializeComponent();
        this.Loaded += MainPage_Loaded;
    }

Implementace obslužné rutiny události bude následující:

    private async void MainPage_Loaded(object sender, RoutedEventArgs e)
    {
        var location = await LocationHelper.GetCurrentLocation();

        if (location != null)
        {
            Debug.WriteLine(string.Concat(location.Coordinate.Longitude,
                " ", location.Coordinate.Latitude));
        }
    }

Povšimněte si, že jsme obslužnou rutinu deklarovali jako asynchronní, protože `GetCurrentLocation` může používat await, a vyžaduje tudíž spouštění v asynchronním kontextu. Navíc vzhledem k tomu, že za určitých okolností můžeme získat nulovou polohu (např. když je vypnuto zjišťování polohy nebo aplikaci byl zamítnut přístup k poloze), potřebujeme zajistit správné zpracování kontrolou hodnoty null.

Spusťte aplikaci. Nezapomeňte povolit přístup k poloze:

![](./media/notification-hubs-geofence/notification-hubs-location-access.png)

Jakmile se aplikace spustí, měli byste v okně **Výstup** vidět souřadnice:

![](./media/notification-hubs-geofence/notification-hubs-location-output.png)

Nyní víte, že získávání polohy funguje. Můžete bez obav odstranit testovací obslužnou rutinu události Loaded, protože ji už nebudeme používat.

Dalším krokem je zachycení změň v poloze. Vraťme se tedy ke třídě `LocationHelper` a přidejme obslužnou rutinu události pro `PositionChanged`:

    geolocator.PositionChanged += Geolocator_PositionChanged;

Implementace zobrazí souřadnice polohy v okně **Výstup**:

    private static async void Geolocator_PositionChanged(Geolocator sender, PositionChangedEventArgs args)
    {
        await CoreApplication.MainView.CoreWindow.Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
        {
            Debug.WriteLine(string.Concat(args.Position.Coordinate.Longitude, " ", args.Position.Coordinate.Latitude));
        });
    }

##Nastavení back-endu

Stáhněte si [ukázku back-endu .NET z GitHubu](https://github.com/Azure/azure-notificationhubs-samples/tree/master/dotnet/NotifyUsers). Až se stahování dokončí, otevřete složku `NotifyUsers` a následně soubor `NotifyUsers.sln`.

Nastavte projekt `AppBackend` jako **Spouštěný projekt** a spusťte jej.

![](./media/notification-hubs-geofence/vs-startup-project.png)

Projekt je již nakonfigurován tak, aby cílovým zařízením odesílal nabízená oznámení, proto budeme potřebovat udělat jen dvě věci – použít správný připojovací řetězec centra oznámení a přidat identifikaci hranice, aby se oznámení odesílalo jenom v případě, že se uživatel nachází uvnitř monitorové geografické zóny.

Pro konfiguraci připojovacího řetězce otevřete ve složce `Models` soubor `Notifications.cs`. Funkce `NotificationHubClient.CreateClientFromConnectionString` by měla obsahovat informace o centru oznámení, které můžete získat na [Portálu Azure](https://portal.azure.com) (podívejte se do okna **Zásady přístupu** v **Nastavení**). Uložte aktualizovaný konfigurační soubor.

Nyní potřebujeme vytvořit model pro výsledek rozhraní API Map Bing. Nejjednodušší způsob, jak toho docílit, je kliknout pravým tlačítkem na složku `Models` a vybrat **Přidat** > **Třída**. Pojmenujte ji `GeofenceBoundary.cs`. Poté zkopírujte JSON z odpovědi rozhraní API, kterou jsme probírali v prvním oddílu. V nástroji Visual Studio pak použijte **Upravit** > **Vložit jinak** > **Vložit formát JSON jako třídy**. 

Tímto způsobem je zajištěno, že se objekt deserializuje podle očekávání. Výsledná sada tříd by měla vypadat přibližně takto:

    namespace AppBackend.Models
    {
        public class Rootobject
        {
            public D d { get; set; }
        }

        public class D
        {
            public string __copyright { get; set; }
            public Result[] results { get; set; }
        }

        public class Result
        {
            public __Metadata __metadata { get; set; }
            public string EntityID { get; set; }
            public string Name { get; set; }
            public float Longitude { get; set; }
            public float Latitude { get; set; }
            public string Boundary { get; set; }
            public string Confidence { get; set; }
            public string Locality { get; set; }
            public string AddressLine { get; set; }
            public string AdminDistrict { get; set; }
            public string CountryRegion { get; set; }
            public string PostalCode { get; set; }
        }

        public class __Metadata
        {
            public string uri { get; set; }
        }
    }

Dále otevřete `Controllers` > `NotificationsController.cs`. Potřebujeme upravit volání Post tak, aby používalo cílovou zeměpisnou délku a šířku. K tomu jednoduše přidejte dva řetězce do signatury funkce – `latitude` a `longitude`.

    public async Task<HttpResponseMessage> Post(string pns, [FromBody]string message, string to_tag, string latitude, string longitude)

Vytvořte v projektu novou třídu s názvem `ApiHelper.cs`. Použijeme ji k připojení k Bingu pro kontrolu průsečíků hranic. Následujícím způsobem implementujte funkci `IsPointWithinBounds`:

    public class ApiHelper
    {
        public static readonly string ApiEndpoint = "{YOUR_QUERY_ENDPOINT}?spatialFilter=intersects(%27POINT%20({0}%20{1})%27)&$format=json&key={2}";
        public static readonly string ApiKey = "{YOUR_API_KEY}";

        public static bool IsPointWithinBounds(string longitude,string latitude)
        {
            var json = new WebClient().DownloadString(string.Format(ApiEndpoint, longitude, latitude, ApiKey));
            var result = JsonConvert.DeserializeObject<Rootobject>(json);
            if (result.d.results != null && result.d.results.Count() > 0)
            {
                return true;
            }
            else
            {
                return false;
            }
        }
    }

>[AZURE.NOTE] Je nezbytné nahradit koncový bod rozhraní API adresou URL dotazu, kterou jste získali dříve z webu Dev Center pro Bing (totéž platí pro klíč rozhraní API). 

Pokud dotaz vrací výsledky, znamená to, že se zadaný bod nachází uvnitř monitorované geografické zóny, proto vrátíme `true`. Pokud žádné výsledky nejsou, Bing nám oznamuje, že se bod nachází mimo rámec vyhledávání, proto vrátíme `false`.

Zpět v `NotificationsController.cs` vytvořte kontrolu přímo před příkazem switch.

    if (ApiHelper.IsPointWithinBounds(longitude, latitude))
    {
        switch (pns.ToLower())
        {
            case "wns":
                //// Windows 8.1 / Windows Phone 8.1
                var toast = @"<toast><visual><binding template=""ToastText01""><text id=""1"">" +
                            "From " + user + ": " + message + "</text></binding></visual></toast>";
                outcome = await Notifications.Instance.Hub.SendWindowsNativeNotificationAsync(toast, userTag);

                // Windows 10 specific Action Center support
                toast = @"<toast><visual><binding template=""ToastGeneric""><text id=""1"">" +
                            "From " + user + ": " + message + "</text></binding></visual></toast>";
                outcome = await Notifications.Instance.Hub.SendWindowsNativeNotificationAsync(toast, userTag);

                break;
        }
    }

Takto se budou oznámení odesílat jenom v případě, že se bod nachází v dané oblasti.

##Testování nabízených oznámení v aplikaci pro UPW

Po návratu do aplikace pro UPW bychom nyní měli být schopni oznámení otestovat. Ve třídě `LocationHelper` vytvořte novou funkci – `SendLocationToBackend`:

    public static async Task SendLocationToBackend(string pns, string userTag, string message, string latitude, string longitude)
    {
        var POST_URL = "http://localhost:8741/api/notifications?pns=" +
            pns + "&to_tag=" + userTag + "&latitude=" + latitude + "&longitude=" + longitude;

        using (var httpClient = new HttpClient())
        {
            try
            {
                await httpClient.PostAsync(POST_URL, new StringContent("\"" + message + "\"",
                    System.Text.Encoding.UTF8, "application/json"));
            }
            catch (Exception ex)
            {
                Debug.WriteLine(ex.Message);
            }
        }
    }

>[AZURE.NOTE] Zaměňte `POST_URL` za umístění nasazené webové aplikace, kterou jsme vytvořili v předchozím oddílu. V tuto chvíli je možné ji spouštět lokálně, ale během nasazování veřejné verze bude nutné ji hostovat pomocí externího poskytovatele.

Nyní se ujistíme, že jsme aplikaci pro UPW zaregistrovali k nabízeným oznámením. V nástroji Visual Studio klikněte na **Projekt** > **Store** > **Propojit aplikaci se Storem**.

![](./media/notification-hubs-geofence/vs-associate-with-store.png)

Jakmile se přihlásíte ke svému účtu vývojáře, ujistěte se, že jste vybrali existující aplikaci, nebo vyberte novou a přidružte k ní balíček. 

Přejděte na Dev Center a otevřete aplikaci, kterou jste právě vytvořili. Klikněte na **Služby** > **Nabízená oznámení** > **Web služeb Live Service**.

![](./media/notification-hubs-geofence/ms-live-services.png)

Na webu si poznamenejte **Tajný klíč aplikace** a **SID balíčku**. Obojí budete potřebovat na Portálu Azure – otevřete své centrum oznámení, klikněte na **Nastavení** > **Notification Services** > **Služba nabízených oznámení Windows (WNS)** a do povinných polí zadejte příslušné informace.

![](./media/notification-hubs-geofence/notification-hubs-wns.png)

Klikněte na **Uložit**.

V **Průzkumníkovi řešení** klikněte pravým tlačítkem na **Odkazy** a vyberte **Spravovat balíčky NuGet**. Budeme muset přidat odkaz na **spravovanou knihovnu Microsoft Azure Service Bus** – jednoduše vyhledejte balíček `WindowsAzure.Messaging.Managed` a přidejte jej do projektu.

![](./media/notification-hubs-geofence/vs-nuget.png)

Pro účely testování můžeme znovu vytvořit obslužnou rutinu události `MainPage_Loaded` a přidat do ní tento fragment kódu:

    var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();

    var hub = new NotificationHub("HUB_NAME", "HUB_LISTEN_CONNECTION_STRING");
    var result = await hub.RegisterNativeAsync(channel.Uri);

    // Displays the registration ID so you know it was successful
    if (result.RegistrationId != null)
    {
        Debug.WriteLine("Reg successful.");
    }

Kód uvedený výše zaregistruje aplikaci do centra oznámení. Jste připraveni! 

V `LocationHelper` uvnitř obslužné rutiny `Geolocator_PositionChanged` můžete přidat testovací kód, který bod nuceně umístí do monitorové geografické zóny.

    await LocationHelper.SendLocationToBackend("wns", "TEST_USER", "TEST", "37.7746", "-122.3858");

Jelikož nepředáváme skutečné souřadnice (které by v tuto chvíli nemusely odpovídat místu uvnitř oblasti) a používáme předem definované testovací hodnoty, uvidíme, že se při aktualizaci zobrazí oznámení:

![](./media/notification-hubs-geofence/notification-hubs-test-notification.png)

##Co dále?

Pokud si chcete být jisti, že je řešení připravené na reálný provoz, může být zapotřebí podniknout ještě několik dalších kroků.

Nejprve je nezbytné zajistit, že monitorovaná geografická zóna je dynamická. To bude vyžadovat další práci s rozhraním API služby Bing, aby bylo možné nahrávat nové hranice do existujícího zdroje dat. Další podrobnosti k tomuto tématu najdete v [dokumentaci rozhraní API pro Bing Spatial Data Services](https://msdn.microsoft.com/library/ff701734.aspx).

Abyste zajistili doručování správným účastníkům, můžete požadovat cílení pomocí [tagování](notification-hubs-tags-segment-push-message.md).

Řešení uvedené výše popisuje scénář, při kterém můžete mít širokou škálu cílových platforem, proto jsme neomezovali monitorování geografické zóny schopnostmi specifickými pro daný systém. Je ale nutné dodat, že Univerzální platforma Windows nabízí integrované možnosti pro [detekci monitorované geografické zóny](https://msdn.microsoft.com/windows/uwp/maps-and-location/set-up-a-geofence).

Další podrobnosti týkající se schopností Notification Hubs najdete na [portálu dokumentace](https://azure.microsoft.com/documentation/services/notification-hubs/).



<!---HONumber=Aug16_HO4-->


