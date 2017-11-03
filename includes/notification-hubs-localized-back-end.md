



Při odesílání oznámení šablony, která potřebujete poskytovat sadu vlastností v našem případě zašleme sada vlastností obsahující lokalizované verzi aktuální novinky, například:

    {
        "News_English": "World News in English!",
        "News_French": "World News in French!",
        "News_Mandarin": "World News in Mandarin!"
    }


V této části ukazuje, jak k odeslání oznámení pomocí konzolové aplikace

Vzhledem k tomu, že back-end může všesměrové vysílání pro žádné z podporovaných zařízení, všesměrově kód zahrnuté na zařízeních s Windows Store a iOS.

### <a name="to-send-notifications-using-a-c-console-app"></a>K odeslání oznámení pomocí konzolové aplikace jazyka C#
Změnit `SendTemplateNotificationAsync` metoda v konzolové aplikace, které jste dříve vytvořili následujícím kódem. Všimněte si, jak v tomto případě není třeba odesílání několika oznámení pro různá národní prostředí a platformy.

        private static async void SendTemplateNotificationAsync()
        {
            // Define the notification hub.
            NotificationHubClient hub = 
                NotificationHubClient.CreateClientFromConnectionString(
                    "<connection string with full access>", "<hub name>");

            // Sending the notification as a template notification. All template registrations that contain 
            // "messageParam" or "News_<local selected>" and the proper tags will receive the notifications. 
            // This includes APNS, GCM, WNS, and MPNS template registrations.
            Dictionary<string, string> templateParams = new Dictionary<string, string>();

            // Create an array of breaking news categories.
            var categories = new string[] { "World", "Politics", "Business", "Technology", "Science", "Sports"};
            var locales = new string[] { "English", "French", "Mandarin" };

            foreach (var category in categories)
            {
                templateParams["messageParam"] = "Breaking " + category + " News!";

                // Sending localized News for each tag too...
                foreach( var locale in locales)
                {
                    string key = "News_" + locale;

                    // Your real localized news content would go here.
                    templateParams[key] = "Breaking " + category + " News in " + locale + "!";
                }

                await hub.SendTemplateNotificationAsync(templateParams, category);
            }
        }


Všimněte si, že toto jednoduché volání dodá lokalizované část příspěvků na **všechny** zařízení, bez ohledu na platformě, jak vaše Centrum oznámení sestavení a doručí správné nativní datové části pro všechna zařízení odběru na konkrétní značka.

### <a name="sending-the-notification-with-mobile-services"></a>Odesílání oznámení pomocí mobilní služby
V Plánovač mobilních služeb můžete použít následující skript:

    var azure = require('azure');
    var notificationHubService = azure.createNotificationHubService('<hub name>', '<connection string with full access>');
    var notification = {
            "News_English": "World News in English!",
            "News_French": "World News in French!",
            "News_Mandarin", "World News in Mandarin!"
    }
    notificationHubService.send('World', notification, function(error) {
        if (!error) {
            console.warn("Notification successful");
        }
    });


