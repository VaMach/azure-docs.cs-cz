---
title: "Přidání nabízených oznámení do vaší aplikace na platformě Xamarin.Forms | Microsoft Docs"
description: "Naučte se používat k odesílání více platformami nabízená oznámení do aplikace Xamarin.Forms služby Azure."
services: app-service\mobile
documentationcenter: xamarin
author: ysxu
manager: syntaxc4
editor: 
ms.assetid: d9b1ba9a-b3f2-4d12-affc-2ee34311538b
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-xamarin
ms.devlang: dotnet
ms.topic: article
ms.date: 10/12/2016
ms.author: yuaxu
ms.openlocfilehash: 912367636f1b26b3b07fbd5fe3fe8ed053218fd5
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="add-push-notifications-to-your-xamarinforms-app"></a>Přidání nabízených oznámení do vaší aplikace na platformě Xamarin.Forms
[!INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

## <a name="overview"></a>Přehled
V tomto kurzu přidání nabízených oznámení na všechny projekty, které je výsledkem [Xamarin.Forms úvodní](app-service-mobile-xamarin-forms-get-started.md). To znamená, že nabízených oznámení se neodesílají do všech klientů napříč platformami pokaždé, když vložení záznamu.

Pokud použijete serverový projekt stažené rychlý start, budete potřebovat balíček rozšíření nabízená oznámení. Další informace najdete v tématu [pracovat s .NET back-end serveru SDK pro Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).

## <a name="prerequisites"></a>Požadavky
Pro iOS, budete potřebovat [programu pro vývojáře Apple členství](https://developer.apple.com/programs/ios/) a fyzickém zařízení iOS. [Simulátoru iOS nabízená oznámení nepodporuje](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/iOS_Simulator_Guide/TestingontheiOSSimulator.html).

## <a name="configure-hub"></a>Konfigurace centra oznámení
[!INCLUDE [app-service-mobile-configure-notification-hub](../../includes/app-service-mobile-configure-notification-hub.md)]

## <a name="update-the-server-project-to-send-push-notifications"></a>Aktualizace serverový projekt k odesílání nabízených oznámení
[!INCLUDE [app-service-mobile-update-server-project-for-push-template](../../includes/app-service-mobile-update-server-project-for-push-template.md)]

## <a name="configure-and-run-the-android-project-optional"></a>Konfiguraci a spuštění projektu pro Android (volitelné)
Dokončení této části ke zprovoznění nabízených oznámení pro Android Xamarin.Forms projekt pro Android.

### <a name="enable-firebase-cloud-messaging-fcm"></a>Povolit Firebase Cloud Messaging (FCM)
[!INCLUDE [notification-hubs-enable-firebase-cloud-messaging](../../includes/notification-hubs-enable-firebase-cloud-messaging.md)]

### <a name="configure-the-mobile-apps-back-end-to-send-push-requests-by-using-fcm"></a>Konfigurace mobilní aplikace back-endu odesílat žádosti o nabízenou pomocí FCM
[!INCLUDE [app-service-mobile-android-configure-push](../../includes/app-service-mobile-android-configure-push.md)]

### <a name="add-push-notifications-to-the-android-project"></a>Přidání nabízených oznámení do projektu pro Android
S back-end FCM nakonfigurované můžete přidat součásti a kódy pro klienta pro registraci se FCM. Můžete také zaregistrovat pro nabízená oznámení pomocí Azure Notification Hubs prostřednictvím back-end mobilní aplikace a přijímat oznámení.

1. V **Droid** projektu, klikněte pravým tlačítkem myši **součásti** složku a klikněte na tlačítko **získat další komponenty...** . Poté vyhledejte **klient Google Cloud Messaging** součástí a přidejte ji do projektu. Tato součást podporuje nabízená oznámení pro projekt Xamarin Android.
2. Otevřete soubor projektu MainActivity.cs a v horní části souboru přidejte následující příkaz:

        using Gcm.Client;
3. Přidejte následující kód, který **OnCreate** metoda po zavolání **LoadApplication**:

        try
        {
            // Check to ensure everything's set up right
            GcmClient.CheckDevice(this);
            GcmClient.CheckManifest(this);

            // Register for push notifications
            System.Diagnostics.Debug.WriteLine("Registering...");
            GcmClient.Register(this, PushHandlerBroadcastReceiver.SENDER_IDS);
        }
        catch (Java.Net.MalformedURLException)
        {
            CreateAndShowDialog("There was an error creating the client. Verify the URL.", "Error");
        }
        catch (Exception e)
        {
            CreateAndShowDialog(e.Message, "Error");
        }
4. Přidejte nový **CreateAndShowDialog** Pomocná metoda, následujícím způsobem:

        private void CreateAndShowDialog(String message, String title)
        {
            AlertDialog.Builder builder = new AlertDialog.Builder(this);

            builder.SetMessage (message);
            builder.SetTitle (title);
            builder.Create().Show ();
        }
5. Přidejte následující kód, který **MainActivity** třídy:

        // Create a new instance field for this activity.
        static MainActivity instance = null;

        // Return the current activity instance.
        public static MainActivity CurrentActivity
        {
            get
            {
                return instance;
            }
        }

    To zpřístupňuje aktuální **MainActivity** instance, proto můžete provést na hlavního vlákna uživatelského rozhraní.
6. Inicializace `instance` proměnné na začátku **OnCreate** metoda následujícím způsobem.

        // Set the current instance of MainActivity.
        instance = this;
7. Přidat nový soubor třídy k **Droid** projektu s názvem `GcmService.cs`a zajistěte, aby následující **pomocí** příkazy nejsou v horní části souboru:

        using Android.App;
        using Android.Content;
        using Android.Media;
        using Android.Support.V4.App;
        using Android.Util;
        using Gcm.Client;
        using Microsoft.WindowsAzure.MobileServices;
        using Newtonsoft.Json.Linq;
        using System;
        using System.Collections.Generic;
        using System.Diagnostics;
        using System.Text;
8. Přidejte následující žádosti oprávnění v horní části souboru po **pomocí** příkazy a před **obor názvů** deklarace.

        [assembly: Permission(Name = "@PACKAGE_NAME@.permission.C2D_MESSAGE")]
        [assembly: UsesPermission(Name = "@PACKAGE_NAME@.permission.C2D_MESSAGE")]
        [assembly: UsesPermission(Name = "com.google.android.c2dm.permission.RECEIVE")]
        [assembly: UsesPermission(Name = "android.permission.INTERNET")]
        [assembly: UsesPermission(Name = "android.permission.WAKE_LOCK")]
        //GET_ACCOUNTS is only needed for android versions 4.0.3 and below
        [assembly: UsesPermission(Name = "android.permission.GET_ACCOUNTS")]
9. Přidejte následující definice tříd do oboru názvů.

       [BroadcastReceiver(Permission = Gcm.Client.Constants.PERMISSION_GCM_INTENTS)]
       [IntentFilter(new string[] { Gcm.Client.Constants.INTENT_FROM_GCM_MESSAGE }, Categories = new string[] { "@PACKAGE_NAME@" })]
       [IntentFilter(new string[] { Gcm.Client.Constants.INTENT_FROM_GCM_REGISTRATION_CALLBACK }, Categories = new string[] { "@PACKAGE_NAME@" })]
       [IntentFilter(new string[] { Gcm.Client.Constants.INTENT_FROM_GCM_LIBRARY_RETRY }, Categories = new string[] { "@PACKAGE_NAME@" })]
       public class PushHandlerBroadcastReceiver : GcmBroadcastReceiverBase<GcmService>
       {
           public static string[] SENDER_IDS = new string[] { "<PROJECT_NUMBER>" };
       }

   > [!NOTE]
   > Nahraďte **< PROJECT_NUMBER >** s vaše číslo projektu, který jste si předtím poznamenali.    
   >
   >
10. Nahraďte prázdné **GcmService** třída, která využívá nové všesměrového vysílání příjemce následujícím kódem:

         [Service]
         public class GcmService : GcmServiceBase
         {
             public static string RegistrationID { get; private set; }

             public GcmService()
                 : base(PushHandlerBroadcastReceiver.SENDER_IDS){}
         }
11. Přidejte následující kód, který **GcmService** třídy. Přepíše **OnRegistered** obslužné rutiny události a implementuje **zaregistrovat** metoda.

        protected override void OnRegistered(Context context, string registrationId)
        {
            Log.Verbose("PushHandlerBroadcastReceiver", "GCM Registered: " + registrationId);
            RegistrationID = registrationId;

            var push = TodoItemManager.DefaultManager.CurrentClient.GetPush();

            MainActivity.CurrentActivity.RunOnUiThread(() => Register(push, null));
        }

        public async void Register(Microsoft.WindowsAzure.MobileServices.Push push, IEnumerable<string> tags)
        {
            try
            {
                const string templateBodyGCM = "{\"data\":{\"message\":\"$(messageParam)\"}}";

                JObject templates = new JObject();
                templates["genericMessage"] = new JObject
                {
                    {"body", templateBodyGCM}
                };

                await push.RegisterAsync(RegistrationID, templates);
                Log.Info("Push Installation Id", push.InstallationId.ToString());
            }
            catch (Exception ex)
            {
                System.Diagnostics.Debug.WriteLine(ex.Message);
                Debugger.Break();
            }
        }

    Všimněte si, že tento kód používá `messageParam` parametr v registraci šablony.
12. Přidejte následující kód, který implementuje **OnMessage**:

        protected override void OnMessage(Context context, Intent intent)
        {
            Log.Info("PushHandlerBroadcastReceiver", "GCM Message Received!");

            var msg = new StringBuilder();

            if (intent != null && intent.Extras != null)
            {
                foreach (var key in intent.Extras.KeySet())
                    msg.AppendLine(key + "=" + intent.Extras.Get(key).ToString());
            }

            //Store the message
            var prefs = GetSharedPreferences(context.PackageName, FileCreationMode.Private);
            var edit = prefs.Edit();
            edit.PutString("last_msg", msg.ToString());
            edit.Commit();

            string message = intent.Extras.GetString("message");
            if (!string.IsNullOrEmpty(message))
            {
                createNotification("New todo item!", "Todo item: " + message);
                return;
            }

            string msg2 = intent.Extras.GetString("msg");
            if (!string.IsNullOrEmpty(msg2))
            {
                createNotification("New hub message!", msg2);
                return;
            }

            createNotification("Unknown message details", msg.ToString());
        }

        void createNotification(string title, string desc)
        {
            //Create notification
            var notificationManager = GetSystemService(Context.NotificationService) as NotificationManager;

            //Create an intent to show ui
            var uiIntent = new Intent(this, typeof(MainActivity));

            //Use Notification Builder
            NotificationCompat.Builder builder = new NotificationCompat.Builder(this);

            //Create the notification
            //we use the pending intent, passing our ui intent over which will get called
            //when the notification is tapped.
            var notification = builder.SetContentIntent(PendingIntent.GetActivity(this, 0, uiIntent, 0))
                    .SetSmallIcon(Android.Resource.Drawable.SymActionEmail)
                    .SetTicker(title)
                    .SetContentTitle(title)
                    .SetContentText(desc)

                    //Set the notification sound
                    .SetSound(RingtoneManager.GetDefaultUri(RingtoneType.Notification))

                    //Auto cancel will remove the notification once the user touches it
                    .SetAutoCancel(true).Build();

            //Show the notification
            notificationManager.Notify(1, notification);
        }

    To zpracovává příchozí oznámení a odesílá je do Správce oznámení, který se má zobrazit.
13. **GcmServiceBase** také vyžaduje, abyste implementovat **OnUnRegistered** a **OnError** metody obslužné rutiny, které můžete provést následujícím způsobem:

        protected override void OnUnRegistered(Context context, string registrationId)
        {
            Log.Error("PushHandlerBroadcastReceiver", "Unregistered RegisterationId : " + registrationId);
        }

        protected override void OnError(Context context, string errorId)
        {
            Log.Error("PushHandlerBroadcastReceiver", "GCM Error: " + errorId);
        }

Teď můžete je připraven nabízená oznámení v aplikaci spuštěnou na zařízení se systémem Android nebo emulátor.

### <a name="test-push-notifications-in-your-android-app"></a>Nabízená oznámení v aplikacích pro Android
První dva kroky jsou povinné, jenom v případě, že testujete na emulátor.

1. Ujistěte se, že nasazení nebo ladění na virtuální zařízení, které má rozhraní Google API nastavenou jako cíl, jak je znázorněno níže ve Správci virtuálního zařízení se systémem Android.
2. Kliknutím na Přidat účet Google do zařízení s Androidem **aplikace** > **nastavení** > **přidejte účet**. Potom postupujte podle pokynů, které chcete přidat existující účet Google do zařízení, nebo vytvořte novou.
3. V sadě Visual Studio nebo Xamarin Studio, klikněte pravým tlačítkem **Droid** projektu a klikněte na tlačítko **nastavit jako spouštěný projekt**.
4. Klikněte na tlačítko **spustit** pro sestavení projektu a spusťte aplikaci v emulátoru nebo zařízení s Androidem.
5. V aplikaci zadejte úlohu a potom klikněte na tlačítko plus (**+**) ikona.
6. Ověřte, že se při přidání položky přijato oznámení.

## <a name="configure-and-run-the-ios-project-optional"></a>Konfiguraci a spuštění projektu pro iOS (volitelné)
Tato část se týká spuštění projektu Xamarin iOS pro zařízení s iOS. Můžete ji přeskočit, pokud s takovými zařízeními nepracujete.

[!INCLUDE [Enable Apple Push Notifications](../../includes/enable-apple-push-notifications.md)]

#### <a name="configure-the-notification-hub-for-apns"></a>Konfigurace centra oznámení pro služby APN
[!INCLUDE [app-service-mobile-apns-configure-push](../../includes/app-service-mobile-apns-configure-push.md)]

Dále nakonfigurujete nastavení projektu iOS v nástroji Xamarin Studio nebo Visual Studio.

[!INCLUDE [app-service-mobile-xamarin-ios-configure-project](../../includes/app-service-mobile-xamarin-ios-configure-project.md)]

#### <a name="add-push-notifications-to-your-ios-app"></a>Přidání nabízených oznámení do vaší aplikace pro iOS
1. V **iOS** projektu, otevřete AppDelegate.cs a přidejte následující příkaz na začátek souboru kódu.

        using Newtonsoft.Json.Linq;
2. V **AppDelegate** třídy, přidejte přepsání pro **RegisteredForRemoteNotifications** události k registraci pro oznámení:

        public override void RegisteredForRemoteNotifications(UIApplication application,
            NSData deviceToken)
        {
            const string templateBodyAPNS = "{\"aps\":{\"alert\":\"$(messageParam)\"}}";

            JObject templates = new JObject();
            templates["genericMessage"] = new JObject
                {
                  {"body", templateBodyAPNS}
                };

            // Register for push with your mobile app
            Push push = TodoItemManager.DefaultManager.CurrentClient.GetPush();
            push.RegisterAsync(deviceToken, templates);
        }
3. V **AppDelegate**, také přidat následující přepsání pro **DidReceiveRemoteNotification** obslužné rutiny události:

        public override void DidReceiveRemoteNotification(UIApplication application,
            NSDictionary userInfo, Action<UIBackgroundFetchResult> completionHandler)
        {
            NSDictionary aps = userInfo.ObjectForKey(new NSString("aps")) as NSDictionary;

            string alert = string.Empty;
            if (aps.ContainsKey(new NSString("alert")))
                alert = (aps[new NSString("alert")] as NSString).ToString();

            //show alert
            if (!string.IsNullOrEmpty(alert))
            {
                UIAlertView avAlert = new UIAlertView("Notification", alert, null, "OK", null);
                avAlert.Show();
            }
        }

    Tato metoda zpracovává příchozí oznámení, když aplikace běží.
4. V **AppDelegate** třídy, přidejte následující kód, který **FinishedLaunching** metoda:

        // Register for push notifications.
        var settings = UIUserNotificationSettings.GetSettingsForTypes(
            UIUserNotificationType.Alert
            | UIUserNotificationType.Badge
            | UIUserNotificationType.Sound,
            new NSSet());

        UIApplication.SharedApplication.RegisterUserNotificationSettings(settings);
        UIApplication.SharedApplication.RegisterForRemoteNotifications();

    To umožňuje podporu pro vzdálené oznámení a registrace nabízených požadavky.

Aplikace je nyní aktualizovat o podporu nabízených oznámení.

#### <a name="test-push-notifications-in-your-ios-app"></a>Nabízená oznámení v aplikaci s iOS
1. Klikněte pravým tlačítkem na projekt pro iOS a klikněte na tlačítko **nastavit jako spouštěný projekt**.
2. Stiskněte **spustit** tlačítko nebo **F5** v sadě Visual Studio pro sestavení projektu a spusťte aplikaci v zařízení se systémem iOS. Pak klikněte na tlačítko **OK** přijímat nabízená oznámení.

   > [!NOTE]
   > Je nutné explicitně přijmout nabízená oznámení z vaší aplikace. Tento požadavek dochází pouze při prvním spuštění aplikace.
   >
   >
3. V aplikaci zadejte úlohu a potom klikněte na tlačítko plus (**+**) ikona.
4. Ověřte, zda přijetí oznámení a pak klikněte na tlačítko **OK** k zavření oznámení.

## <a name="configure-and-run-windows-projects-optional"></a>Nakonfigurujte a spusťte projekty pro Windows (volitelné)
Tato část se týká spuštění Xamarin.Forms WinApp a WinPhone81 projekty pro zařízení se systémem Windows. Tyto kroky také podporují projekty univerzální platformu Windows (UWP). Můžete ji přeskočit, pokud s takovými zařízeními nepracujete.

#### <a name="register-your-windows-app-for-push-notifications-with-windows-notification-service-wns"></a>Registrace aplikace systému Windows pro nabízená oznámení pomocí služby oznámení Windows (WNS)
[!INCLUDE [app-service-mobile-register-wns](../../includes/app-service-mobile-register-wns.md)]

#### <a name="configure-the-notification-hub-for-wns"></a>Konfigurace centra oznámení pro WNS
[!INCLUDE [app-service-mobile-configure-wns](../../includes/app-service-mobile-configure-wns.md)]

#### <a name="add-push-notifications-to-your-windows-app"></a>Přidání nabízených oznámení do aplikace pro Windows
1. V sadě Visual Studio otevřete **App.xaml.cs** v systému Windows projekt a přidejte následující příkazy.

        using Newtonsoft.Json.Linq;
        using Microsoft.WindowsAzure.MobileServices;
        using System.Threading.Tasks;
        using Windows.Networking.PushNotifications;
        using <your_TodoItemManager_portable_class_namespace>;

    Nahraďte `<your_TodoItemManager_portable_class_namespace>` s oborem názvů vašeho přenosného projektu, který obsahuje `TodoItemManager` třídy.
2. V souboru App.xaml.cs přidejte následující **InitNotificationsAsync** metoda:

        private async Task InitNotificationsAsync()
        {
            var channel = await PushNotificationChannelManager
                .CreatePushNotificationChannelForApplicationAsync();

            const string templateBodyWNS =
                "<toast><visual><binding template=\"ToastText01\"><text id=\"1\">$(messageParam)</text></binding></visual></toast>";

            JObject headers = new JObject();
            headers["X-WNS-Type"] = "wns/toast";

            JObject templates = new JObject();
            templates["genericMessage"] = new JObject
            {
                {"body", templateBodyWNS},
                {"headers", headers} // Needed for WNS.
            };

            await TodoItemManager.DefaultManager.CurrentClient.GetPush()
                .RegisterAsync(channel.Uri, templates);
        }

    Tato metoda získá kanál nabízená oznámení a zaregistruje šablonu do šablony oznámení dostávat, vaše Centrum oznámení. Šablony oznámení, která podporuje *messageParam* budou doručeny do tohoto klienta.
3. V souboru App.xaml.cs, aktualizovat **OnLaunched** definici metody obslužné rutiny událostí přidáním `async` modifikátor. Pak přidejte následující řádek kódu na konci metody:

        await InitNotificationsAsync();

    Tím se zajistí, že registrace nabízených oznámení je vytvořit nebo aktualizovat pokaždé, když je aplikace spuštěná. Je důležité k tomu zaručit, že kanál nabízené WNS je vždy aktivní.  
4. V Průzkumníku řešení pro sadu Visual Studio, otevřete **Package.appxmanifest** souboru a nastavit **informační podporující** k **Ano** pod **oznámení**.
5. Sestavte aplikaci a zkontrolujte, že jste žádné chyby. Klientská aplikace by teď zaregistrovat pro šablony oznámení z back-end mobilní aplikace. Tato část opakujte pro každý projekt Windows ve vašem řešení.

#### <a name="test-push-notifications-in-your-windows-app"></a>Nabízená oznámení v aplikaci Windows
1. V sadě Visual Studio, klikněte pravým tlačítkem na projekt Windows a klikněte na tlačítko **nastavit jako spouštěný projekt**.
2. Stiskněte tlačítko **Spustit** a sestavte projekt a spusťte aplikaci.
3. V aplikaci, zadejte název nové todoitem a pak klikněte na tlačítko plus (**+**) ikona Přidat.
4. Ověřte, že se při přidání položky přijato oznámení.

## <a name="next-steps"></a>Další kroky
Další informace o nabízených oznámení:

* [Diagnostikovat problémy nabízená oznámení](../notification-hubs/notification-hubs-push-notification-fixer.md)  
  Existují různé důvody, proč oznámení může získat vyřadit ani nekončí na zařízení. Toto téma ukazuje, jak analyzovat a zjistěte příčinu selhání nabízená oznámení.

Můžete také pokračovat na jednu z následujících kurzů:

* [Přidání ověřování do aplikace](app-service-mobile-xamarin-forms-get-started-users.md)  
  Zjistěte, jak ověřovat uživatele vaší aplikace pomocí zprostředkovatele identity.
* [Povolení offline synchronizace u aplikace](app-service-mobile-xamarin-forms-get-started-offline-data.md)  
  Zjistěte, jak pomocí back-endu Mobile Apps přidat do aplikace podporu offline režimu. S offline synchronizací, mohou uživatelé komunikovat s mobilní aplikací&mdash;zobrazení, přidávat a upravovat data&mdash;i v případě, že není žádné síťové připojení.

<!-- Images. -->

<!-- URLs. -->
[Install Xcode]: https://go.microsoft.com/fwLink/p/?LinkID=266532
[Xcode]: https://go.microsoft.com/fwLink/?LinkID=266532
[apns object]: http://go.microsoft.com/fwlink/p/?LinkId=272333
