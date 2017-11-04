
V této části můžete odesílání novinek jako s příznakem šablony oznámení z konzolové aplikace .NET.

Pokud používáte funkci Microsoft Azure App Service Mobile Apps, podívejte se na [přidat nabízená oznámení pro Mobile Apps] kurzu a vyberte platformu v horní části.

Pokud chcete použít Java nebo PHP, podívejte se na [jak používat centra oznámení z Java nebo PHP]. Odesílání oznámení z jakékoli back-end pomocí [rozhraní REST centra oznámení].

Pokud jste vytvořili konzolovou aplikaci pro odesílání oznámení, když jste dokončili [Začínáme s Notification Hubs], přeskočte kroky 1 – 3.

1. Ve Visual Studiu vytvořte novou konzolovou aplikaci Visual C#:
   
      ![Odkaz konzolové aplikace][13]

2. V hlavní nabídce sady Visual Studio, vyberte **nástroje** > **Správce balíčků knihoven** > **Konzola správce balíčků** a potom v konzole okno, zadejte následující řetězec:
   
        Install-Package Microsoft.Azure.NotificationHubs
   
3. Vyberte **zadejte**.  
    Tato akce přidá referenci na sadu SDK služby Azure Notification Hubs pomocí [balíčku NuGet Microsoft.Azure.Notification Hubs].

4. Otevřete soubor Program.cs a přidejte následující `using` příkaz:
   
        using Microsoft.Azure.NotificationHubs;

5. V `Program` třídy, přidejte následující metodu nebo ho nahradit, pokud již existuje:
   
        private static async void SendTemplateNotificationAsync()
        {
            // Define the notification hub.
            NotificationHubClient hub =
                NotificationHubClient.CreateClientFromConnectionString(
                    "<connection string with full access>", "<hub name>");
   
            // Create an array of breaking news categories.
            var categories = new string[] { "World", "Politics", "Business",
                                            "Technology", "Science", "Sports"};
   
            // Send the notification as a template notification. All template registrations that contain
            // "messageParam" and the proper tags will receive the notifications.
            // This includes APNS, GCM, WNS, and MPNS template registrations.
   
            Dictionary<string, string> templateParams = new Dictionary<string, string>();
   
            foreach (var category in categories)
            {
                templateParams["messageParam"] = "Breaking " + category + " News!";
                await hub.SendTemplateNotificationAsync(templateParams, category);
            }
         }
   
    Tento kód odešle šablony oznámení pro každou z šesti značky v poli řetězců. Použití značek zajistí, že zařízení obdrží oznámení pouze pro registrované kategorie.

5. V předchozím kódu, nahraďte `<hub name>` a `<connection string with full access>` zástupné symboly pomocí názvu centra oznámení a připojovacího řetězce pro *DefaultFullSharedAccessSignature* na řídicím panelu Centra oznámení.

6. Do metody **Main** přidejte následující řádky:
   
         SendTemplateNotificationAsync();
         Console.ReadLine();

7. Vytvoření konzolové aplikace.

<!-- Images. -->
[13]: ./media/notification-hubs-back-end/notification-hub-create-console-app.png

<!-- URLs. -->
[Začínáme s Notification Hubs]: ../articles/notification-hubs/notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md
[rozhraní REST centra oznámení]: http://msdn.microsoft.com/library/windowsazure/dn223264.aspx
[přidat nabízená oznámení pro Mobile Apps]: ../articles/app-service-mobile/app-service-mobile-windows-store-dotnet-get-started-push.md
[jak používat centra oznámení z Java nebo PHP]: ../articles/notification-hubs/notification-hubs-java-push-notification-tutorial.md
[balíčku NuGet Microsoft.Azure.Notification Hubs]: http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/
