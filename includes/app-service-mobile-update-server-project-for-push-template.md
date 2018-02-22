V této části aktualizujte kód v existující projekt back-end mobilní aplikace k odesílání nabízených oznámení pokaždé, když se při přidání nové položky. Tento proces používá technologii [šablony](../articles/notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) nabízených oznámení funkce Azure Notification Hubs, což umožňuje napříč platformami. Různých klientů jsou registrované pro nabízená oznámení pomocí šablony a jeden universal push můžete získat na všechny klientské platformy.

Vyberte jednu z následujících postupů, které odpovídá typu vašeho projektu back-end&mdash;buď [.NET back-end](#dotnet) nebo [back-end Node.js](#nodejs).

### <a name="dotnet"></a>Rozhraní .NET back-end projektu
1. V sadě Visual Studio klikněte pravým tlačítkem na projekt serveru. Potom vyberte **spravovat balíčky NuGet**. Vyhledejte `Microsoft.Azure.NotificationHubs`a potom vyberte **nainstalovat**. Tento proces nainstaluje knihovny centra oznámení pro odesílání oznámení z back-end.
2. V projektu serveru otevřete **řadiče** > **TodoItemController.cs**. Pak přidejte následující příkazy:

        using System.Collections.Generic;
        using Microsoft.Azure.NotificationHubs;
        using Microsoft.Azure.Mobile.Server.Config;
3. V **PostTodoItem** metoda, přidejte následující kód po volání **InsertAsync**:  

        // Get the settings for the server project.
        HttpConfiguration config = this.Configuration;
        MobileAppSettingsDictionary settings =
            this.Configuration.GetMobileAppSettingsProvider().GetMobileAppSettings();

        // Get the Notification Hubs credentials for the mobile app.
        string notificationHubName = settings.NotificationHubName;
        string notificationHubConnection = settings
            .Connections[MobileAppSettingsKeys.NotificationHubConnectionString].ConnectionString;

        // Create a new Notification Hub client.
        NotificationHubClient hub = NotificationHubClient
        .CreateClientFromConnectionString(notificationHubConnection, notificationHubName);

        // Send the message so that all template registrations that contain "messageParam"
        // receive the notifications. This includes APNS, GCM, WNS, and MPNS template registrations.
        Dictionary<string,string> templateParams = new Dictionary<string,string>();
        templateParams["messageParam"] = item.Text + " was added to the list.";

        try
        {
            // Send the push notification and log the results.
            var result = await hub.SendTemplateNotificationAsync(templateParams);

            // Write the success result to the logs.
            config.Services.GetTraceWriter().Info(result.State.ToString());
        }
        catch (System.Exception ex)
        {
            // Write the failure result to the logs.
            config.Services.GetTraceWriter()
                .Error(ex.Message, null, "Push.SendAsync Error");
        }

    Tento proces odešle šablony oznámení, který obsahuje položky. Text, když je vložit novou položku.
4. Znovu publikujte serverový projekt.

### <a name="nodejs"></a>Projektu back-end Node.js
1. Pokud jste zatím žádný nevytvořili, [stáhnout back-end projektu pro rychlý Start](../articles/app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#download-quickstart), nebo použijte jiný [online editor na webu Azure portal](../articles/app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#online-editor).
2. Existující kód v todoitem.js nahraďte následujícím kódem:

        var azureMobileApps = require('azure-mobile-apps'),
        promises = require('azure-mobile-apps/src/utilities/promises'),
        logger = require('azure-mobile-apps/src/logger');

        var table = azureMobileApps.table();

        table.insert(function (context) {
        // For more information about the Notification Hubs JavaScript SDK,
        // see http://aka.ms/nodejshubs.
        logger.info('Running TodoItem.insert');

        // Define the template payload.
        var payload = '{"messageParam": "' + context.item.text + '" }';  

        // Execute the insert. The insert returns the results as a promise.
        // Do the push as a post-execute action within the promise flow.
        return context.execute()
            .then(function (results) {
                // Only do the push if configured.
                if (context.push) {
                    // Send a template notification.
                    context.push.send(null, payload, function (error) {
                        if (error) {
                            logger.error('Error while sending push notification: ', error);
                        } else {
                            logger.info('Push notification sent successfully!');
                        }
                    });
                }
                // Don't forget to return the results from the context.execute().
                return results;
            })
            .catch(function (error) {
                logger.error('Error while running context.execute: ', error);
            });
        });

        module.exports = table;  

    Tento proces odešle oznámení šablona obsahující item.text při vložení nové položky.
3. Při úpravách souboru v místním počítači, znovu publikujte serverový projekt.
