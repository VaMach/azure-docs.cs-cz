## <a name="webapi-project"></a>WebAPI projektu
1. V sadě Visual Studio, otevřete **AppBackend** projekt, který jste vytvořili v **upozornění uživatelů** kurzu.
2. V Notifications.cs, nahraďte celek **oznámení** třídy následujícím kódem. Ujistěte se, že nahraďte zástupné symboly připojovací řetězec (s úplným přístupem) pro vaše Centrum oznámení a název rozbočovače. Můžete získat z těchto hodnot [portálu Azure Classic](http://manage.windowsazure.com). Tento modul představuje teď jiné zabezpečené oznámení, které se budou odesílat. Do dokončení implementace se uloží oznámení v databázi. pro jednoduchost v takovém případě jsme je uložit v paměti.
   
        public class Notification
        {
            public int Id { get; set; }
            public string Payload { get; set; }
            public bool Read { get; set; }
        }

        public class Notifications
        {
            public static Notifications Instance = new Notifications();

            private List<Notification> notifications = new List<Notification>();

            public NotificationHubClient Hub { get; set; }

            private Notifications() {
                Hub = NotificationHubClient.CreateClientFromConnectionString("{conn string with full access}",     "{hub name}");
            }

            public Notification CreateNotification(string payload)
            {
                var notification = new Notification() {
                Id = notifications.Count,
                Payload = payload,
                Read = false
                };

                notifications.Add(notification);

                return notification;
            }

            public Notification ReadNotification(int id)
            {
                return notifications.ElementAt(id);
            }
        }

1. V NotificationsController.cs, nahraďte kód uvnitř **NotificationsController** třídy definice následujícím kódem. Tato součást implementuje způsob, jak zařízení bezpečně načíst oznámení a také poskytuje způsob (pro účely tohoto kurzu) k aktivaci zabezpečené oznámení do zařízení. Všimněte si, že při odesílání oznámení do centra oznámení, jenom odešleme nezpracovaná oznámení s ID oznámení (a žádná skutečná zpráva):
   
       public NotificationsController()
       {
           Notifications.Instance.CreateNotification("This is a secure notification!");
       }
   
       // GET api/notifications/id
       public Notification Get(int id)
       {
           return Notifications.Instance.ReadNotification(id);
       }
   
       public async Task<HttpResponseMessage> Post()
       {
           var secureNotificationInTheBackend = Notifications.Instance.CreateNotification("Secure confirmation.");
           var usernameTag = "username:" + HttpContext.Current.User.Identity.Name;
   
           // windows
           var rawNotificationToBeSent = new Microsoft.Azure.NotificationHubs.WindowsNotification(secureNotificationInTheBackend.Id.ToString(),
                           new Dictionary<string, string> {
                               {"X-WNS-Type", "wns/raw"}
                           });
           await Notifications.Instance.Hub.SendNotificationAsync(rawNotificationToBeSent, usernameTag);
   
           // apns
           await Notifications.Instance.Hub.SendAppleNativeNotificationAsync("{\"aps\": {\"content-available\": 1}, \"secureId\": \"" + secureNotificationInTheBackend.Id.ToString() + "\"}", usernameTag);
   
           // gcm
           await Notifications.Instance.Hub.SendGcmNativeNotificationAsync("{\"data\": {\"secureId\": \"" + secureNotificationInTheBackend.Id.ToString() + "\"}}", usernameTag);

            return Request.CreateResponse(HttpStatusCode.OK);
        }


Všimněte si, že `Post` metoda teď neodešle oznámení s informační zprávou. Odešle nezpracovaná oznámení, že obsahuje pouze ID oznámení a ne všechny citlivého obsahu. Zkontrolujte taky, okomentujte operaci odeslání pro platformy, pro které nemáte přihlašovací údaje, které jsou nakonfigurované v centru oznámení, jak bude vést k chybám.

1. Nyní jsme bude znovu nasaďte tuto aplikaci na web Azure aby přístupná ze všech zařízení. Klikněte pravým tlačítkem na projekt **AppBackend** a vyberte **Publikovat**.
2. Vyberte web Azure jako váš cíl publikování. Přihlaste se pomocí účtu Azure a vyberte stávajícího nebo nového webu a poznamenejte si **cílová adresa URL** vlastnost **připojení** kartě. Na tuto adresu URL budeme odkazovat jako na *koncový bod back-endu* později v tomto kurzu. Klikněte na **Publikovat**.

