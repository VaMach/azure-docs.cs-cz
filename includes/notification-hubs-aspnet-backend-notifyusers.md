## <a name="create-the-webapi-project"></a>Vytvoření projektu WebAPI
Další části popisují vytvoření nové ASP.NET WebAPI back-end. Tento proces má tři hlavní funkce:

* **Ověřuje klienty**: přidejte obslužné rutiny zpráv později k ověření klientských požadavků a přidružit uživatele k žádosti.

* **Registrace pro oznámení pomocí WebAPI back-end**: přidání řadiče pro zpracování nové registrace pro klientské zařízení k přijímání oznámení. Ověřené uživatelské jméno se automaticky přidá do registrace jako [značky](https://msdn.microsoft.com/library/azure/dn530749.aspx).

* **Odesílání oznámení klientům**: také přidat řadič poskytnout způsob, jak uživatelům aktivovat zabezpečené oznámení do zařízení a klientů, které jsou přidružené k značky. 

Vytvořte nový ASP.NET WebAPI back-end následujícím způsobem: 

> [!IMPORTANT]
> Pokud používáte Visual Studio 2015 nebo starší, před zahájením tohoto kurzu, zkontrolujte, zda jste nainstalovali nejnovější verze Správce balíčků NuGet pro Visual Studio. 
>
>Pokud to chcete zkontrolovat, spusťte sadu Visual Studio. Na **nástroje** nabídce vyberte možnost **rozšíření a aktualizace**. Vyhledejte **Správce balíčků NuGet** ve vaší verzi sady Visual Studio a ujistěte se, že máte nejnovější verzi. Pokud vaše verze není na nejnovější verzi, odinstalujte ji a poté znovu nainstalujte Správce balíčků NuGet.
 
![][B4]

> [!NOTE]
> Ujistěte se, že máte nainstalovanou sadu [Azure SDK](https://azure.microsoft.com/downloads/) pro vývoj pro web.
> 
> 

1. Spusťte sadu Visual Studio nebo Visual Studio Express. 

2. Vyberte **Průzkumníka serveru**a přihlaste se k účtu Azure. Pokud chcete vytvořit prostředky webu na vašem účtu, musí být podepsané.

3. V sadě Visual Studio, vyberte **soubor** > **nový** > **projektu**, rozbalte položku **šablony**, rozbalte položku **Visual C#**a potom vyberte **webové** a **webové aplikace ASP.NET**.

4. V **název** zadejte **AppBackend**a potom vyberte **OK**. 
   
    ![Okno Nový projekt][B1]

5. V **nový projekt ASP.NET** vyberte **webového rozhraní API** zaškrtněte políčko a potom vyberte **OK**.
   
    ![Okno Nový projekt ASP.NET][B2]

6. V **konfigurace webové aplikace Azure Microsoft** okno, vyberte předplatné a pak na **plán služby App Service** seznamu, proveďte jednu z následujících:

    * Vyberte plán služby app service, kterou jste vytvořili. 
    * Vyberte **vytvořit nový plán služby app**a pak vytvořit. 
    
  Pro účely tohoto kurzu nepotřebujete databázi. Po výběru váš plán služby app service, vyberte **OK** a vytvořte tak projekt.
   
    ![Okno Konfigurace Microsoft Azure Web App][B5]

## <a name="authenticate-clients-to-the-webapi-back-end"></a>Ověření klientům WebAPI back-end
V této části vytvoříte novou třídu obslužné rutiny zpráv s názvem **AuthenticationTestHandler** pro nový back-end. Tato třída je odvozený od [DelegatingHandler](https://msdn.microsoft.com/library/system.net.http.delegatinghandler.aspx) a přidat jako obslužné rutiny zpráv tak, aby ji může zpracovat všechny požadavky tohoto zadány do back-end. 

1. V Průzkumníku řešení klikněte pravým tlačítkem myši **AppBackend** projekt, vyberte **přidat**a potom vyberte **třída**. 
 
2. Pojmenujte novou třídu **AuthenticationTestHandler.cs**a potom vyberte **přidat** ke generování třídy. Tato třída ověřuje uživatele pomocí *základní ověřování* pro jednoduchost. Aplikace můžete používat žádné schéma ověřování.

3. V souboru AuthenticationTestHandler.cs přidejte následující příkazy `using`:
   
        using System.Net.Http;
        using System.Threading;
        using System.Security.Principal;
        using System.Net;
        using System.Text;
        using System.Threading.Tasks;

4. V AuthenticationTestHandler.cs, nahraďte `AuthenticationTestHandler` třídy definice následujícím kódem: 
   
    Obslužná rutina se ověření požadavku, pokud jsou splněny následující tři podmínky:
   
   * Požadavek obsahuje *autorizace* záhlaví. 
   * Požadavek používá *základní* ověřování. 
   * Řetězce uživatelského jména a hesla jsou stejné.
     
  Jinak bude požadavek zamítnut. Toto není správný přístup k ověřování a autorizaci. Je velmi jednoduchý příklad v tomto kurzu.
     
  Pokud se ověří a autorizuje ve zprávě požadavku `AuthenticationTestHandler`, uživatel základní ověřování je připojen k aktuální požadavek na [HttpContext](https://msdn.microsoft.com/library/system.web.httpcontext.current.aspx). Informace o uživateli v HttpContext použije jiný řadič (RegisterController) později přidat [značky](https://msdn.microsoft.com/library/azure/dn530749.aspx) na žádost o registraci oznámení.
     
       public class AuthenticationTestHandler : DelegatingHandler
       {
           protected override Task<HttpResponseMessage> SendAsync(
           HttpRequestMessage request, CancellationToken cancellationToken)
           {
               var authorizationHeader = request.Headers.GetValues("Authorization").First();
     
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
     > Poznámka k zabezpečení: `AuthenticationTestHandler` třída neposkytuje true ověřování. Používá se pouze k napodobení základního ověřování a není bezpečná. Ve svých produkčních aplikacích a službách musíte implementovat mechanismus zabezpečeného ověřování.                
     > 
     > 
5. Pokud chcete zaregistrovat popisovač zpráv, přidejte následující kód na konci `Register` metoda v **App_Start/WebApiConfig.cs** třídy:
   
        config.MessageHandlers.Add(new AuthenticationTestHandler());

6. Uložte provedené změny.

## <a name="register-for-notifications-by-using-the-webapi-back-end"></a>Registrace pro oznámení pomocí WebAPI back-end
V této části přidáte nový řadič WebAPI back-endu zpracování požadavků na registraci uživatele a zařízení pro oznámení pomocí klientské knihovny pro centra oznámení. Správce přidá značku uživatele pro uživatele, který byl ověřen a připojeny k HttpContext podle `AuthenticationTestHandler`. Značka bude mít formát řetězce `"username:<actual username>"`.

1. V Průzkumníku řešení klikněte pravým tlačítkem myši **AppBackend** projektu a potom vyberte **spravovat balíčky NuGet**.

2. V levém podokně vyberte **Online** a pak na **vyhledávání** zadejte **Microsoft.Azure.NotificationHubs**.

3. V seznamu výsledků vyberte **Microsoft Azure Notification Hubs**a potom vyberte **nainstalovat**. Dokončení instalace a pak zavřete okno Správce balíčků NuGet.
   
    Tato akce přidá referenci na sadu SDK služby Azure Notification Hubs pomocí <a href="http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/">balíčku NuGet Microsoft.Azure.Notification Hubs</a>.

4. Vytvořte nový soubor – třída, která představuje připojení s centrem oznámení, která se používá k odesílání oznámení. V Průzkumníku řešení klikněte pravým tlačítkem myši **modely** složky, vyberte **přidat**a potom vyberte **třída**. Pojmenujte novou třídu **Notifications.cs**a potom vyberte **přidat** ke generování třídy. 
   
    ![Okna Přidat novou položku][B6]

5. Na začátek souboru Notifications.cs přidejte následující příkaz `using`:
   
        using Microsoft.Azure.NotificationHubs;

6. Nahraďte `Notifications` definici následujícím kódem třídy a nahraďte zástupné symboly dva připojovací řetězec (s úplným přístupem) pro vaše Centrum oznámení a název centra (k dispozici na [portál Azure](http://portal.azure.com)):
   
        public class Notifications
        {
            public static Notifications Instance = new Notifications();
   
            public NotificationHubClient Hub { get; set; }
   
            private Notifications() {
                Hub = NotificationHubClient.CreateClientFromConnectionString("<your hub's DefaultFullSharedAccessSignature>", 
                                                                             "<hub name>");
            }
        }
7. Dále vytvořte nový řadič s názvem **RegisterController**. V Průzkumníku řešení klikněte pravým tlačítkem myši **řadiče** složky, vyberte **přidat**a potom vyberte **řadič**. 

8. Vyberte **webové rozhraní API 2 řadiče - prázdný**a potom vyberte **přidat**.
   
    ![Okna Přidat vygenerované uživatelské rozhraní][B7]
   
9. V **názvu Kontroleru** zadejte **RegisterController** pojmenujte novou třídu, a potom vyberte **přidat**.

    ![Okna Přidat kontroler][B8]

10. V souboru RegisterController.cs přidejte následující příkazy `using`:
   
        using Microsoft.Azure.NotificationHubs;
        using Microsoft.Azure.NotificationHubs.Messaging;
        using AppBackend.Models;
        using System.Threading.Tasks;
        using System.Web;

11. Do definice třídy `RegisterController` přidejte následující kód. Všimněte si, že v tomto kódu přidáme značku uživatele pro uživatele, který je připojen k položka HttpContext. Uživatel byl ověřen a připojeny k HttpContext filtr zpráv, které jsme přidali, `AuthenticationTestHandler`. Můžete také přidat volitelné kontroly pro ověření, že uživatel má práva pro registraci k požadovaným značkám.
   
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
12. Uložte provedené změny.

## <a name="send-notifications-from-the-webapi-back-end"></a>Odesílání oznámení z back-end WebAPI
V této části přidáte nový řadič, který zveřejňuje způsob pro klientská zařízení k odeslání oznámení. Oznámení je založena na značky uživatelského jména, která používá knihovna správy služby centra oznámení Azure v ASP.NET WebAPI back-end.

1. Vytvořte další nový řadič **NotificationsController** stejným způsobem, který jste vytvořili **RegisterController** v předchozí části.

2. V souboru NotificationsController.cs přidejte následující příkazy `using`:
   
        using AppBackend.Models;
        using System.Threading.Tasks;
        using System.Web;

3. Přidejte následující metodu do **NotificationsController** třídy:
   
    Tento kód odešle oznámení typ, který je založen na oznámení služby platformy (PNS) `pns` parametr. Hodnota `to_tag` slouží k nastavení značky *username* (uživatelské jméno) pro zprávu. Tato značka musí odpovídat značce uživatelského jména aktivní registrace k centru událostí. Zpráva oznámení se přetáhne z textu požadavku POST a naformátuje se pro cílový systém oznámení platformy. 
   
    V závislosti na systém PNS, podporovaných zařízení použít k přijímání oznámení oznámení jsou podporovány v různých formátech. Například na zařízení se systémem Windows, můžete použít [připít oznámení s WNS](https://msdn.microsoft.com/library/windows/apps/br230849.aspx) který není podporován přímo jiné PNS. V takové instanci musí back-end vašeho formátu oznámení do podporovaných oznámení pro systém PNS zařízení, že která chcete podporovat. Pak použijte příslušné odeslat na rozhraní API [NotificationHubClient třída](https://msdn.microsoft.com/library/azure/microsoft.azure.notificationhubs.notificationhubclient_methods.aspx).
   
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

4. Chcete-li spustit aplikaci a zkontrolujte přesnost své dosavadní, vyberte **F5** klíč. Aplikace otevře webový prohlížeč a zobrazí se na domovské stránce ASP.NET. 

## <a name="publish-the-new-webapi-back-end"></a>Publikování nové WebAPI back-end
Potom můžete nasadit aplikace pro web Azure a zpřístupnit jej ze všech zařízení. 

1. Klikněte pravým tlačítkem myši **AppBackend** projektu a potom vyberte **publikovat**.

2. Vyberte **Microsoft Azure App Service** jako cíl publikování a potom vyberte **publikovat**.  
    Otevře se okno vytvořit službu App Service. Tady si můžete vytvořit všechny potřebné prostředky Azure ke spouštění webové aplikace ASP.NET v Azure.

    ![Na dlaždici služby Microsoft Azure App Service][B15]

3. V **vytvořit službu App Service** okně vyberte účtu Azure. Vyberte **změnit typ** > **webová aplikace**. Ponechte výchozí **název webové aplikace**a pak vyberte **předplatné**, **skupiny prostředků**, a **plán služby App Service**. 

4. Vyberte **Vytvořit**.

5. Poznamenejte si vlastnost **Adresa URL webu** v části **Souhrn**. Toto je adresa URL vaší *koncový bod back-end* dál v tomto kurzu. 

6. Vyberte **publikování**.

Po dokončení průvodce, publikuje webovou aplikaci ASP.NET do Azure a pak otevře aplikace ve výchozím prohlížeči.  Aplikace je zobrazitelný v Azure App Services.

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
