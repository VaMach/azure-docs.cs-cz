## <a name="create-the-webapi-project"></a>Vytvoření projektu WebAPI
V následujících částech se vytvoří nový back-end ASP.NET WebAPI, který bude sloužit ke třem hlavním účelům:

1. **Ověřování klientů:** Později se přidá popisovač zprávy, který bude ověřovat požadavky klientů a přiřazovat k požadavkům uživatele.
2. **Registrace klientských oznámení:** Později přidáte kontroler, který bude zpracovávat nové registrace klientských zařízení k přijímání oznámení. Ověřené uživatelské jméno se automaticky přidá do registrace jako [značka](https://msdn.microsoft.com/library/azure/dn530749.aspx).
3. **Odesílání oznámení klientům:** Později přidáte také kontroler, který uživateli umožní aktivovat zabezpečené nabízení do zařízení a klientů přidružených ke značce. 

Následující kroky ukazují, jak vytvořit nový back-end ASP.NET WebAPI: 

> [!IMPORTANT]
> Pokud používáte sadu Visual Studio 2015 nebo starší, před zahájením tohoto kurzu se ujistěte, že máte nainstalovanou nejnovější verzi správce balíčků NuGet. Pokud to chcete zkontrolovat, spusťte sadu Visual Studio. V nabídce **Nástroje** klikněte na **Rozšíření a aktualizace**. Vyhledejte **Správce balíčků NuGet** pro vaši verzi sady Visual Studio a ujistěte se, že máte nejnovější verzi. Pokud ne, odinstalujte a znovu nainstalujte správce balíčků NuGet.
> 
> ![][B4]
> 
> [!NOTE]
> Ujistěte se, že máte nainstalovanou sadu [Azure SDK](https://azure.microsoft.com/downloads/) pro vývoj pro web.
> 
> 

1. Spusťte sadu Visual Studio nebo Visual Studio Express. Klikněte na **Průzkumník serveru** a přihlaste se ke svému účtu Azure. Sada Visual Studio bude potřebovat vaše přihlášení, aby ve vašem účtu mohla vytvořit prostředky webu.
2. V sadě Visual Studio klikněte na **Soubor**, potom na **Nový**, **Projekt**, rozbalte **Šablony**, **Visual C#**, klikněte na **Web** a **Webová aplikace ASP.NET**, zadejte název **AppBackend** a klikněte na **OK**. 
   
    ![][B1]
3. V dialogovém okně **Nový projekt ASP.NET** klikněte na **Web API** a potom na **OK**.
   
    ![][B2]
4. V dialogovém okně **Konfigurace webové aplikace Microsoft Azure** zvolte předplatné a **Plán služby App Service**, který už jste vytvořili. Můžete také zvolit možnost **Vytvořit nový plán služby App Service** a vytvořit nový plán v dialogovém okně. Pro účely tohoto kurzu nepotřebujete databázi. Jakmile vyberete plán služby App Service, kliknutím na **OK** vytvořte projekt.
   
    ![][B5]

## <a name="authenticating-clients-to-the-webapi-backend"></a>Ověřování klientů v back-endu WebAPI
V této části pro nový back-end vytvoříte novou třídu popisovače zprávy **AuthenticationTestHandler**. Tato třída je odvozená od třídy [DelegatingHandler](https://msdn.microsoft.com/library/system.net.http.delegatinghandler.aspx) a přidaná jako popisovač zprávy, aby mohla zpracovávat všechny požadavky přicházející na back-end. 

1. V Průzkumníku řešení klikněte pravým tlačítkem na projekt **AppBackend**, klikněte na **Přidat** a potom klikněte na **Třída**. Pojmenujte novou třídu **AuthenticationTestHandler.cs** a kliknutím na **Přidat** ji vygenerujte. Tato třída bude pro zjednodušení sloužit k ověřování uživatelů pomocí *základního ověřování*. Nezapomeňte, že vaše aplikace může používat jakékoli schéma ověřování.
2. V souboru AuthenticationTestHandler.cs přidejte následující příkazy `using`:
   
        using System.Net.Http;
        using System.Threading;
        using System.Security.Principal;
        using System.Net;
        using System.Text;
        using System.Threading.Tasks;

3. V souboru AuthenticationTestHandler.cs nahraďte definici třídy `AuthenticationTestHandler` následujícím kódem. 
   
    Tato obslužná rutina ověří požadavek při splnění všech těchto tří podmínek:
   
   * Požadavek obsahuje *autorizační* hlavičku. 
   * Požadavek používá *základní* ověřování. 
   * Řetězce uživatelského jména a hesla jsou stejné.
     
     Jinak bude požadavek zamítnut. Toto není správný přístup k ověřování a autorizaci. Je to jenom velmi jednoduchý příklad pro účely tohoto kurzu.
     
     Pokud třída `AuthenticationTestHandler` ověří a autorizuje zprávu požadavku, pak se uživatel základního ověřování připojí k aktuálnímu požadavku v objektu [HttpContext](https://msdn.microsoft.com/library/system.web.httpcontext.current.aspx). Informace o uživateli v objektu HttpContext později použije jiný kontroler (RegisterController) pro přidání [značky](https://msdn.microsoft.com/library/azure/dn530749.aspx) do požadavku na registraci oznámení.
     
       public class AuthenticationTestHandler : DelegatingHandler   {       protected override Task<HttpResponseMessage> SendAsync(       HttpRequestMessage request, CancellationToken cancellationToken)       {           var authorizationHeader = request.Headers.GetValues("Authorization").First();
     
               if (authorizationHeader != null && authorizationHeader
                   .StartsWith("Basic ", StringComparison.InvariantCultureIgnoreCase))
               {
                   string authorizationUserAndPwdBase64 =
                       authorizationHeader.Substring("Basic ".Length);
                   string authorizationUserAndPwd = Encoding.Default
                       .GetString(Convert.FromBase64String(authorizationUserAndPwdBase64));
                   string user = authorizationUserAndPwd.Split(':')[0];
                   string password = authorizationUserAndPwd.Split(':')[1];
     
                   if (verifyUserAndPwd(user, password))
                   {
                       // Attach the new principal object to the current HttpContext object
                       HttpContext.Current.User =
                           new GenericPrincipal(new GenericIdentity(user), new string[0]);
                       System.Threading.Thread.CurrentPrincipal =
                           System.Web.HttpContext.Current.User;
                   }
                   else return Unauthorized();
               }
               else return Unauthorized();
     
               return base.SendAsync(request, cancellationToken);
           }
     
           private bool verifyUserAndPwd(string user, string password)
           {
               // This is not a real authentication scheme.
               return user == password;
           }
     
           private Task<HttpResponseMessage> Unauthorized()
           {
               var response = new HttpResponseMessage(HttpStatusCode.Forbidden);
               var tsc = new TaskCompletionSource<HttpResponseMessage>();
               tsc.SetResult(response);
               return tsc.Task;
           }
       }
     
     > [!NOTE]
     > **Poznámka k zabezpečení:** Třída `AuthenticationTestHandler` nezajišťuje skutečné ověřování. Používá se pouze k napodobení základního ověřování a není bezpečná. Ve svých produkčních aplikacích a službách musíte implementovat mechanismus zabezpečeného ověřování.                
     > 
     > 
4. Přidejte následující kód pro registraci popisovače zprávy na konec metody `Register` ve třídě **App_Start/WebApiConfig.cs**:
   
        config.MessageHandlers.Add(new AuthenticationTestHandler());
5. Uložte provedené změny.

## <a name="registering-for-notifications-using-the-webapi-backend"></a>Registrace k oznámením pomocí back-endu WebAPI
V této části přidáme do back-endu WebAPI nový kontroler, který bude zpracovávat požadavky na registraci uživatele a zařízení k oznámením pomocí klientské knihovny pro centra oznámení. Kontroler přidá značku uživatele pro uživatele, který byl ověřen a připojen k objektu HttpContext třídou `AuthenticationTestHandler`. Značka bude mít formát řetězce `"username:<actual username>"`.

1. V Průzkumníku řešení klikněte pravým tlačítkem na projekt **AppBackend** a potom klikněte na **Správa balíčků NuGet**.
2. Na levé straně klikněte na **Online** a ve **vyhledávacím** poli vyhledejte **Microsoft.Azure.NotificationHubs**.
3. V seznamů výsledků klikněte na **Microsoft Azure Notification Hubs** a potom klikněte na **Nainstalovat**. Dokončete instalaci a potom zavřete okno správce balíčků NuGet.
   
    Ten přidá odkaz na sadu SDK centra oznámení Azure pomocí <a href="http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/">balíčku Microsoft.Azure.Notification Hubs NuGet</a>.
4. Nyní vytvoříme nový soubor třídy, která představuje propojení s centrem událostí sloužícím k odesílání oznámení. V Průzkumníku řešení klikněte pravým tlačítkem na složku **Modely**, klikněte na **Přidat** a potom klikněte na **Třída**. Pojmenujte novou třídu **Notifications.cs** a potom ji kliknutím na **Přidat** vygenerujte. 
   
    ![][B6]
5. Na začátek souboru Notifications.cs přidejte následující příkaz `using`:
   
        using Microsoft.Azure.NotificationHubs;
6. Nahraďte definici třídy `Notifications` následujícím kódem a nezapomeňte přitom nahradit dva zástupné symboly připojovacím řetězcem (pro úplný přístup) vašeho centra událostí a názvem centra (najdete ho na [portálu Azure Classic](http://manage.windowsazure.com)):
   
        public class Notifications
        {
            public static Notifications Instance = new Notifications();
   
            public NotificationHubClient Hub { get; set; }
   
            private Notifications() {
                Hub = NotificationHubClient.CreateClientFromConnectionString("<your hub's DefaultFullSharedAccessSignature>", 
                                                                             "<hub name>");
            }
        }
7. Dále vytvoříme nový kontroler **RegisterController**. V Průzkumníku řešení klikněte pravým tlačítkem na složku **Kontrolery**, klikněte na **Přidat** a potom na **Kontroler**. Klikněte na položku **Kontroler rozhraní Web API 2 – Prázdný** a potom klikněte na **Přidat**. Pojmenujte novou třídu **RegisterController** a znovu klikněte na **Přidat**. Vygeneruje se kontroler.
   
    ![][B7]
   
    ![][B8]
8. V souboru RegisterController.cs přidejte následující příkazy `using`:
   
        using Microsoft.Azure.NotificationHubs;
        using Microsoft.Azure.NotificationHubs.Messaging;
        using AppBackend.Models;
        using System.Threading.Tasks;
        using System.Web;
9. Do definice třídy `RegisterController` přidejte následující kód. Všimněte si, že v tomto kódu přidáváme značku uživatele pro uživatele, který je připojený k objektu HttpContext. Tento uživatel byl ověřen a připojen k objektu HttpContext filtrem zpráv `AuthenticationTestHandler`, který jsme vytvořili. Můžete také přidat volitelné kontroly pro ověření, že uživatel má práva pro registraci k požadovaným značkám.
   
        private NotificationHubClient hub;
   
        public RegisterController()
        {
            hub = Notifications.Instance.Hub;
        }
   
        public class DeviceRegistration
        {
            public string Platform { get; set; }
            public string Handle { get; set; }
            public string[] Tags { get; set; }
        }
   
        // POST api/register
        // This creates a registration id
        public async Task<string> Post(string handle = null)
        {
            string newRegistrationId = null;
   
            // make sure there are no existing registrations for this push handle (used for iOS and Android)
            if (handle != null)
            {
                var registrations = await hub.GetRegistrationsByChannelAsync(handle, 100);
   
                foreach (RegistrationDescription registration in registrations)
                {
                    if (newRegistrationId == null)
                    {
                        newRegistrationId = registration.RegistrationId;
                    }
                    else
                    {
                        await hub.DeleteRegistrationAsync(registration);
                    }
                }
            }
   
            if (newRegistrationId == null) 
                newRegistrationId = await hub.CreateRegistrationIdAsync();
   
            return newRegistrationId;
        }
   
        // PUT api/register/5
        // This creates or updates a registration (with provided channelURI) at the specified id
        public async Task<HttpResponseMessage> Put(string id, DeviceRegistration deviceUpdate)
        {
            RegistrationDescription registration = null;
            switch (deviceUpdate.Platform)
            {
                case "mpns":
                    registration = new MpnsRegistrationDescription(deviceUpdate.Handle);
                    break;
                case "wns":
                    registration = new WindowsRegistrationDescription(deviceUpdate.Handle);
                    break;
                case "apns":
                    registration = new AppleRegistrationDescription(deviceUpdate.Handle);
                    break;
                case "gcm":
                    registration = new GcmRegistrationDescription(deviceUpdate.Handle);
                    break;
                default:
                    throw new HttpResponseException(HttpStatusCode.BadRequest);
            }
   
            registration.RegistrationId = id;
            var username = HttpContext.Current.User.Identity.Name;
   
            // add check if user is allowed to add these tags
            registration.Tags = new HashSet<string>(deviceUpdate.Tags);
            registration.Tags.Add("username:" + username);
   
            try
            {
                await hub.CreateOrUpdateRegistrationAsync(registration);
            }
            catch (MessagingException e)
            {
                ReturnGoneIfHubResponseIsGone(e);
            }
   
            return Request.CreateResponse(HttpStatusCode.OK);
        }
   
        // DELETE api/register/5
        public async Task<HttpResponseMessage> Delete(string id)
        {
            await hub.DeleteRegistrationAsync(id);
            return Request.CreateResponse(HttpStatusCode.OK);
        }
   
        private static void ReturnGoneIfHubResponseIsGone(MessagingException e)
        {
            var webex = e.InnerException as WebException;
            if (webex.Status == WebExceptionStatus.ProtocolError)
            {
                var response = (HttpWebResponse)webex.Response;
                if (response.StatusCode == HttpStatusCode.Gone)
                    throw new HttpRequestException(HttpStatusCode.Gone.ToString());
            }
        }
10. Uložte provedené změny.

## <a name="sending-notifications-from-the-webapi-backend"></a>Odesílání oznámení z back-endu WebAPI
V této části přidáte nový kontroler, který klientským zařízením zpřístupní možnost odesílat oznámení na základě značky uživatelského jména pomocí knihovny pro správu služby Azure Notification Hubs v back-endu ASP.NET WebAPI.

1. Vytvořte další nový kontroler **NotificationsController**. Vytvořte ho stejným způsobem jako kontroler **RegisterController** v předchozí části.
2. V souboru NotificationsController.cs přidejte následující příkazy `using`:
   
        using AppBackend.Models;
        using System.Threading.Tasks;
        using System.Web;
3. Do třídy **NotificationsController** přidejte následující metodu.
   
    Tento kód odesílá typ oznámení na základě parametru `pns` systému oznámení platformy. Hodnota `to_tag` slouží k nastavení značky *username* (uživatelské jméno) pro zprávu. Tato značka musí odpovídat značce uživatelského jména aktivní registrace k centru událostí. Zpráva oznámení se přetáhne z textu požadavku POST a naformátuje se pro cílový systém oznámení platformy. 
   
    V závislosti na systému oznámení platformy, který vaše zařízení používá k přijímání oznámení, jsou podporována různá oznámení v různých formátech. Například na zařízeních s Windows byste mohli použít [informační zprávu pomocí Služby nabízených oznámení Windows](https://msdn.microsoft.com/library/windows/apps/br230849.aspx), kterou ostatní systémy oznámení platformy přímo nepodporují. Takže by váš back-end musel oznámení formátovat na podporované oznámení pro systémy oznámení platformy zařízení, která chcete podporovat. Ve [třídě NotificationHubClient](https://msdn.microsoft.com/library/azure/microsoft.azure.notificationhubs.notificationhubclient_methods.aspx) tedy použijte vhodné rozhraní API pro odesílání.
   
        public async Task<HttpResponseMessage> Post(string pns, [FromBody]string message, string to_tag)
        {
            var user = HttpContext.Current.User.Identity.Name;
            string[] userTag = new string[2];
            userTag[0] = "username:" + to_tag;
            userTag[1] = "from:" + user;
   
            Microsoft.Azure.NotificationHubs.NotificationOutcome outcome = null;
            HttpStatusCode ret = HttpStatusCode.InternalServerError;
   
            switch (pns.ToLower())
            {
                case "wns":
                    // Windows 8.1 / Windows Phone 8.1
                    var toast = @"<toast><visual><binding template=""ToastText01""><text id=""1"">" + 
                                "From " + user + ": " + message + "</text></binding></visual></toast>";
                    outcome = await Notifications.Instance.Hub.SendWindowsNativeNotificationAsync(toast, userTag);
                    break;
                case "apns":
                    // iOS
                    var alert = "{\"aps\":{\"alert\":\"" + "From " + user + ": " + message + "\"}}";
                    outcome = await Notifications.Instance.Hub.SendAppleNativeNotificationAsync(alert, userTag);
                    break;
                case "gcm":
                    // Android
                    var notif = "{ \"data\" : {\"message\":\"" + "From " + user + ": " + message + "\"}}";
                    outcome = await Notifications.Instance.Hub.SendGcmNativeNotificationAsync(notif, userTag);
                    break;
            }
   
            if (outcome != null)
            {
                if (!((outcome.State == Microsoft.Azure.NotificationHubs.NotificationOutcomeState.Abandoned) ||
                    (outcome.State == Microsoft.Azure.NotificationHubs.NotificationOutcomeState.Unknown)))
                {
                    ret = HttpStatusCode.OK;
                }
            }
   
            return Request.CreateResponse(ret);
        }
4. Stisknutím klávesy **F5** aplikaci spusťte a ověřte, že jste zatím postupovali správně. Aplikace by měla otevřít webový prohlížeč a zobrazit domovskou stránku ASP.NET. 

## <a name="publish-the-new-webapi-backend"></a>Publikování nového back-endu WebAPI
1. Nyní tuto aplikaci nasadíme na web Azure, aby byla přístupná ze všech zařízení. Klikněte pravým tlačítkem na projekt **AppBackend** a vyberte **Publikovat**.
2. Jako cíl publikování vyberte **Microsoft Azure App Service** a klikněte na **Publikovat**. Tím se otevře dialogové okno Vytvoření služby App Service, které vám pomůže vytvořit všechny prostředky Azure potřebné ke spuštění vaší webové aplikace ASP.NET v Azure.

    ![][B15]
3. V dialogovém okně **Vytvoření služby App Service** vyberte váš účet Azure. Klikněte na **Změnit typ** a vyberte **Webová aplikace**. Ponechte vyplněný **Název webové aplikace** a vyberte **Předplatné**, **Skupinu prostředků** a **Plán služby App Service**.  Klikněte na možnost **Vytvořit**.

4. Poznamenejte si vlastnost **Adresa URL webu** v části **Souhrn**. Na tuto adresu URL budeme odkazovat jako na *koncový bod back-endu* později v tomto kurzu. Klikněte na **Publikovat**.

5. Průvodce po dokončení publikuje webovou aplikaci ASP.NET do služby Azure a pak aplikaci spustí ve výchozím prohlížeči.  Vaši aplikaci bude možné zobrazit ve službě Azure App Service.

Adresa URL používá dříve zadaný název webové aplikace ve formátu http://<název_aplikace>.azurewebsites.net.

[B1]: ./media/notification-hubs-aspnet-backend-notifyusers/notification-hubs-secure-push1.png
[B2]: ./media/notification-hubs-aspnet-backend-notifyusers/notification-hubs-secure-push2.png
[B3]: ./media/notification-hubs-aspnet-backend-notifyusers/notification-hubs-secure-push3.png
[B4]: ./media/notification-hubs-aspnet-backend-notifyusers/notification-hubs-secure-push4.png
[B5]: ./media/notification-hubs-aspnet-backend-notifyusers/notification-hubs-secure-push5.png
[B6]: ./media/notification-hubs-aspnet-backend-notifyusers/notification-hubs-secure-push6.png
[B7]: ./media/notification-hubs-aspnet-backend-notifyusers/notification-hubs-secure-push7.png
[B8]: ./media/notification-hubs-aspnet-backend-notifyusers/notification-hubs-secure-push8.png
[B14]: ./media/notification-hubs-aspnet-backend-notifyusers/notification-hubs-secure-push14.png
[B15]: ./media/notification-hubs-aspnet-backend-notifyusers/publish-to-app-service.png
[B16]: ./media/notification-hubs-aspnet-backend-notifyusers/notification-hubs-notify-users16.PNG
[B18]: ./media/notification-hubs-aspnet-backend-notifyusers/notification-hubs-notify-users18.PNG
