V této části aktualizujte kód v existující projekt back-end mobilní aplikace k odesílání nabízených oznámení pokaždé, když se při přidání nové položky. To používá technologii [šablony](../articles/notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) funkce Azure Notification Hubs, povolení nabízených oznámení napříč platformami. Různých klientů jsou registrované pro nabízená oznámení pomocí šablony a jeden universal push můžete získat na všechny klientské platformy.

Vyberte jednu z následujících postupů, které odpovídá typu vašeho projektu back-end&mdash;buď [.NET back-end](#dotnet) nebo [back-end Node.js](#nodejs).

### <a name="dotnet"></a>Rozhraní .NET back-end projektu
1. V sadě Visual Studio, klikněte pravým tlačítkem na serverový projekt a klikněte na tlačítko **spravovat balíčky NuGet**. Vyhledejte `Microsoft.Azure.NotificationHubs`a potom klikněte na **nainstalovat**. To nainstaluje knihovny centra oznámení pro odesílání oznámení z back-end vašeho.
2. V projektu serveru otevřete **řadiče** > **TodoItemController.cs**a přidejte následující příkazy:

        using System.Collections.Generic;
        using Microsoft.Azure.NotificationHubs;
        using Microsoft.Azure.Mobile.Server.Config;
3. V **PostTodoItem** metoda, přidejte následující kód po volání **InsertAsync**:  

        // Get the settings for the server project.
        HttpConfiguration config = this.Configuration;
        MobileAppSettingsDictionary settings =
            this.Configuration.GetMobileAppSettingsProvider().GetMobileAppSettings();

        // Get the Notification Hubs credentials for the Mobile App.
        string notificationHubName = settings.NotificationHubName;
        string notificationHubConnection = settings
            .Connections[MobileAppSettingsKeys.NotificationHubConnectionString].ConnectionString;

        // Create a new Notification Hub client.
        NotificationHubClient hub = NotificationHubClient
        .CreateClientFromConnectionString(notificationHubConnection, notificationHubName);

        // Sending the message so that all template registrations that contain "messageParam"
        // will receive the notifications. This includes APNS, GCM, WNS, and MPNS template registrations.
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

    Tím se odešle šablony oznámení, který obsahuje položky. Text, když je vložit novou položku.
4. Znovu publikujte serverový projekt.

### <a name="nodejs"></a>Projektu back-end Node.js
1. Pokud jste zatím žádný nevytvořili, [stáhnout back-end projektu pro rychlý Start](../articles/app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#download-quickstart), nebo použijte jiný [online editor na webu Azure portal](../articles/app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#online-editor).
2. Nahraďte stávající kód v todoitem.js s následujícími službami:

        var azureMobileApps = require('azure-mobile-apps'),
        promises = require('azure-mobile-apps/src/utilities/promises'),
        logger = require('azure-mobile-apps/src/logger');

        var table = azureMobileApps.table();

        table.insert(function (context) {
        // For more information about the Notification Hubs JavaScript SDK,
        // see http://aka.ms/nodejshubs
        logger.info('Running TodoItem.insert');

        // Define the template payload.
        var payload = '{"messageParam": "' + context.item.text + '" }';  

        // Execute the insert.  The insert returns the results as a Promise,
        // Do the push as a post-execute action within the promise flow.
        return context.execute()
            .then(function (results) {
                // Only do the push if configured
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
                // Don't forget to return the results from the context.execute()
                return results;
            })
            .catch(function (error) {
                logger.error('Error while running context.execute: ', error);
            });
        });

        module.exports = table;  

    Tím se odešle oznámení šablona obsahující item.text při vložení nové položky.
3. Při úpravách souboru v místním počítači, znovu publikujte serverový projekt.
