<properties
    pageTitle="Začínáme s centrem oznámení pro aplikace Xamarin Android | Microsoft Azure"
    description="V tomto kurzu zjistíte, jak používat Azure Notification Hubs k odesílání nabízených oznámení do aplikace Xamarin Android."
    authors="wesmc7777"
    manager="erikre"
    editor=""
    services="notification-hubs"
    documentationCenter="xamarin"/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin-android"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="06/29/2016"
    ms.author="wesmc"/>


# Začínáme s centry oznámení se sadou Xamarin pro Android

[AZURE.INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

##Přehled

V tomto kurzu zjistíte, jak používat Azure Notification Hubs k odesílání nabízených oznámení do aplikace systému Xamarin.Android.
Vytvoříte prázdnou aplikaci systému Xamarin.Android, která bude přijímat nabízená oznámení pomocí služby GCM (Google Cloud Messaging). Jakmile budete hotovi, budete moci používat vaše centra oznámení k vysílání nabízených oznámení pro všechna zařízení používající vaši aplikaci. Dokončený kód je k dispozici ve vzorku [Aplikace NotificationHubs][GitHub].

Tento kurz představuje scénář jednoduchého vysílání přes centra oznámení.


## Než začnete

[AZURE.INCLUDE [notification-hubs-hero-slug](../../includes/notification-hubs-hero-slug.md)]

Dokončený kód v tomto kurzu lze najít v části Github [zde](https://github.com/Azure/azure-notificationhubs-samples/tree/master/dotnet/Xamarin/GetStartedXamarinAndroid).



##Požadavky

V tomto kurzu budete potřebovat následující:

+ Visual Studio s Xamarinem v systému Windows nebo Xamarin Studio v systému Mac OS X. Úplné pokyny k instalaci najdete v tématu [Instalační program a instalace Visual Studia a Xamarinu](https://msdn.microsoft.com/library/mt613162.aspx).
+ Aktivní účet Google
+ [Komponenta zasílání zpráv Azure]
+ [Komponenta klienta zasílání zpráv cloudu Google]

Dokončení tohoto kurzu je předpokladem pro všechny ostatní kurzy Notification Hubs pro Xamarin.Android Apps.

> [AZURE.IMPORTANT] K dokončení tohoto kurzu potřebujete mít aktivní účet Azure. Pokud účet nemáte, můžete si během několika minut vytvořit bezplatný zkušební účet. Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A9C9624B5&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fpartner-xamarin-notification-hubs-android-get-started%2F).

##Povolení služby GCM (Google Cloud Messaging)

[AZURE.INCLUDE [mobile-services-enable-Google-cloud-messaging](../../includes/mobile-services-enable-google-cloud-messaging.md)]

##Konfigurace centra oznámení

[AZURE.INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]


<ol start="7">
<li><p>Klikněte na kartu <b>Konfigurovat</b>, zadejte hodnotu <b>Klíč rozhraní API</b> získanou v předchozí části a pak klikněte na tlačítko <b>Uložit</b>.</p>
</li>
</ol>
&emsp;&emsp;![](./media/notification-hubs-android-get-started/notification-hub-configure-android.png)


Vaše centrum oznámení je nyní nakonfigurováno pro práci se službou GCM. Zároveň máte připojovací řetězce, pomocí kterých můžete svou aplikaci zaregistrovat pro příjem oznámení a odesílání nabízených oznámení.

##Připojte aplikaci k centru oznámení

###Vytvoření nového projektu

1. V Xamarin Studiu klikněte na tlačítko **Nové řešení**, klikněte na tlačítko **Aplikace pro Android** a klikněte na tlačítko **Další**.

    ![](./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-create-xamarin-android-project1.png)

2. Zadejte **Název aplikace** a **Identifikátor**. Klikněte na tlačítko **Cílové plaformy**, které chcete podporovat, a pak klikněte na tlačítko **Další** a **Vytvořit**.

    ![](./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-create-xamarin-android-project2.png)


    Dojde k vytvoření nového projektu pro Android.

2. Otevřete vlastnosti projektu kliknutím pravým tlačítkem myši na nový projekt v zobrazení Řešení a zvolte **Možnosti**. Vyberte položku **Aplikace pro Android** v části **Sestavení**.

    Zajistěte, aby první písmeno **Názvu balíčku** bylo malé písmeno.

    > [AZURE.IMPORTANT] První písmeno názvu balíčku musí být malé. Jinak se zobrazí chyby manifestu aplikace při registraci vašeho **BroadcastReceiver** a **IntentFilter** pro nabízená oznámení níže.

    ![](./media/partner-xamarin-notification-hubs-android-get-started/notification-hub--xamarin-android-app-options.png)


3. Volitelně můžete nastavit **Minimální verzi systému Android** na jinou úroveň rozhraní API.

4. Volitelně můžete nastavit **Cílovou verzi systému Android** na jinou verzi rozhraní API, kterou chcete zacílit (musí se jednat o úroveň rozhraní API 8 nebo vyšší).

Klikněte na tlačítko **OK** a zavřete dialogové okno Možnosti projektu.


###Přidejte do projektu požadované součásti

Klient služby GCM (Google Cloud Messaging) dostupný na úložišti součástí Xamarin zjednodušuje proces podpory nabízených oznámení v Xamarin.Android.

1. Klikněte pravým tlačítkem na složku Komponenty v aplikaci Xamarin.Android a vyberte možnost **Získat více komponent**.

2. Vyhledejte komponentu **Zasílání zpráv Azure** a přidejte ji do projektu.

3. Vyhledejte komponentu **Klient služby GCM (Google Cloud Messaging)** a přidejte ji do projektu.


###Nastavte centra oznámení v projektu

1. Shromážděte následující informace pro aplikaci Android a centrum oznámení:

    - **GoogleProjectNumber**: tuto hodnotu čísla projektu získáte z přehledu své aplikace na portálu pro vývojáře Google. Poznámku o této hodnotě jste vytvořili dříve při vytvoření aplikace na portálu.
    - **Připojovací řetězec naslouchání**: na řídicím panelu na [portál Azure Classic] klikněte na tlačítko **Zobrazit připojovací řetězce**. Zkopírujte připojovací řetězec *DefaultListenSharedAccessSignature* pro tuto hodnotu.
    - **Název centra**: Toto je název centra z [portál Azure Classic]. Například *mynotificationhub2*.

    Vytvořte třídu **Constants.cs** pro váš projekt Xamarin a definujte následující hodnoty konstant ve třídě. Nahraďte zástupné symboly vašimi hodnotami.

        public static class Constants
        {
            public const string SenderID = "<GoogleProjectNumber>"; // Google API Project Number
            public const string ListenConnectionString = "<Listen connection string>";
            public const string NotificationHubName = "<hub name>";
        }

2. Přidejte následující hodnoty pomocí příkazů do souboru **MainActivity.cs**:

        using Android.Util;
        using Gcm.Client;

3. Přidejte proměnnou instance do třídy `MainActivity`, která se použije k zobrazení dialogového okna výstrah při spuštění aplikace:

        public static MainActivity instance;


3. Vytvořte následující metodu v třídě **MainActivity**:

        private void RegisterWithGCM()
        {
            // Check to ensure everything's set up right
            GcmClient.CheckDevice(this);
            GcmClient.CheckManifest(this);

            // Register for push notifications
            Log.Info("MainActivity", "Registering...");
            GcmClient.Register(this, Constants.SenderID);
        }

4. V metodě `OnCreate` souboru **MainActivity.cs** inicializujte proměnnou `instance` a přidejte volání do `RegisterWithGCM`:

        protected override void OnCreate (Bundle bundle)
        {
            instance = this;

            base.OnCreate (bundle);

            // Set your view from the "main" layout resource
            SetContentView (Resource.Layout.Main);

            // Get your button from the layout resource,
            // and attach an event to it
            Button button = FindViewById<Button> (Resource.Id.myButton);

            RegisterWithGCM();
        }


4. Vytvořte novou třídu **MyBroadcastReceiver**.

    > [AZURE.NOTE] Projdeme s vámi vytvoření třídy **BroadcastReceiver** od začátku níže. Rychlá alternativa k ručnímu vytvoření souboru **MyBroadcastReceiver.cs** představuje odkaz na soubor **GcmService.cs** nacházející se v projektu vzorku Xamarin.Android, který je součástí [vzorků NotificationHubs][GitHub]. Duplikování souboru **GcmService.cs** a změna názvů tříd může být skvělým místem, kde lze také začít.

5. Přidejte následující možnosti pomocí příkazů do souboru **MyBroadcastReceiver.cs** (odkazující na komponentu a sestavení, které jste přidali dříve):

        using System.Collections.Generic;
        using System.Text;
        using Android.App;
        using Android.Content;
        using Android.Util;
        using Gcm.Client;
        using WindowsAzure.Messaging;

5. V souboru **MyBroadcastReceiver.cs** přidejte následující žádosti oprávnění mezi příkazy **pomocí** a prohlášením **obor názvů**:

        [assembly: Permission(Name = "@PACKAGE_NAME@.permission.C2D_MESSAGE")]
        [assembly: UsesPermission(Name = "@PACKAGE_NAME@.permission.C2D_MESSAGE")]
        [assembly: UsesPermission(Name = "com.google.android.c2dm.permission.RECEIVE")]

        //GET_ACCOUNTS is needed only for Android versions 4.0.3 and below
        [assembly: UsesPermission(Name = "android.permission.GET_ACCOUNTS")]
        [assembly: UsesPermission(Name = "android.permission.INTERNET")]
        [assembly: UsesPermission(Name = "android.permission.WAKE_LOCK")]

6. V souboru **MyBroadcastReceiver.cs** změňte třídu **MyBroadcastReceiver** tak, aby odpovídala následující:

        [BroadcastReceiver(Permission=Gcm.Client.Constants.PERMISSION_GCM_INTENTS)]
        [IntentFilter(new string[] { Gcm.Client.Constants.INTENT_FROM_GCM_MESSAGE },
            Categories = new string[] { "@PACKAGE_NAME@" })]
        [IntentFilter(new string[] { Gcm.Client.Constants.INTENT_FROM_GCM_REGISTRATION_CALLBACK },
            Categories = new string[] { "@PACKAGE_NAME@" })]
        [IntentFilter(new string[] { Gcm.Client.Constants.INTENT_FROM_GCM_LIBRARY_RETRY },
            Categories = new string[] { "@PACKAGE_NAME@" })]
        public class MyBroadcastReceiver : GcmBroadcastReceiverBase<PushHandlerService>
        {
            public static string[] SENDER_IDS = new string[] { Constants.SenderID };

            public const string TAG = "MyBroadcastReceiver-GCM";
        }

7. Přidejte jinou třídu do souboru **MyBroadcastReceiver.cs** s názvem **PushHandlerService**, která je odvozena z **GcmServiceBase**. Nezapomeňte použít atribut **Služby** na třídu:

        [Service] // Must use the service tag
        public class PushHandlerService : GcmServiceBase
        {
            public static string RegistrationID { get; private set; }
            private NotificationHub Hub { get; set; }

            public PushHandlerService() : base(Constants.SenderID)
            {
                Log.Info(MyBroadcastReceiver.TAG, "PushHandlerService() constructor");
            }
        }


8. **GcmServiceBase** implementuje metody **OnRegistered()**, **OnUnRegistered()**, **OnMessage()**, **OnRecoverableError()** a **OnError()**. Naše třída implementace **PushHandlerService** musí přepsat tyto metody a tyto metody se aktivují v odezvě na interakci s centrem oznámení.


9. Potlačte metodu **OnRegistered()** v **PushHandlerService** pomocí následujícího kódu:

        protected override void OnRegistered(Context context, string registrationId)
        {
            Log.Verbose(MyBroadcastReceiver.TAG, "GCM Registered: " + registrationId);
            RegistrationID = registrationId;

            createNotification("PushHandlerService-GCM Registered...",
                                "The device has been Registered!");

            Hub = new NotificationHub(Constants.NotificationHubName, Constants.ListenConnectionString,
                                        context);
            try
            {
                Hub.UnregisterAll(registrationId);
            }
            catch (Exception ex)
            {
                Log.Error(MyBroadcastReceiver.TAG, ex.Message);
            }

            //var tags = new List<string>() { "falcons" }; // create tags if you want
            var tags = new List<string>() {};

            try
            {
                var hubRegistration = Hub.Register(registrationId, tags.ToArray());
            }
            catch (Exception ex)
            {
                Log.Error(MyBroadcastReceiver.TAG, ex.Message);
            }
        }

    > [AZURE.NOTE] V kódu **OnRegistered()** výše si všimněte schopnosti určit značky pro registraci do konkrétních kanálů zasílání zpráv.

10. Potlačte metodu **OnMessage()** v **PushHandlerService** pomocí následujícího kódu:

        protected override void OnMessage(Context context, Intent intent)
        {
            Log.Info(MyBroadcastReceiver.TAG, "GCM Message Received!");

            var msg = new StringBuilder();

            if (intent != null && intent.Extras != null)
            {
                foreach (var key in intent.Extras.KeySet())
                    msg.AppendLine(key + "=" + intent.Extras.Get(key).ToString());
            }

            string messageText = intent.Extras.GetString("message");
            if (!string.IsNullOrEmpty (messageText))
            {
                createNotification ("New hub message!", messageText);
            }
            else
            {
                createNotification ("Unknown message details", msg.ToString ());
            }
        }

11. Přidejte následující metody **createNotification** a **dialogNotify** do **PushHandlerService** pro upozornění uživatelů při obdržení oznámení.

    >[AZURE.NOTE] Návrh oznámení v systému Android verze 5.0 a novější představuje významné odchýlení od předchozích verzí. Pokud toto vyzkoušíte v systému Android 5.0 nebo novějším, aplikaci bude nutné spouštět pro příjem oznámení. Další informace naleznete v tématu [Oznámení systému Android](http://go.microsoft.com/fwlink/?LinkId=615880).

        void createNotification(string title, string desc)
        {
            //Create notification
            var notificationManager = GetSystemService(Context.NotificationService) as NotificationManager;

            //Create an intent to show UI
            var uiIntent = new Intent(this, typeof(MainActivity));

            //Create the notification
            var notification = new Notification(Android.Resource.Drawable.SymActionEmail, title);

            //Auto-cancel will remove the notification once the user touches it
            notification.Flags = NotificationFlags.AutoCancel;

            //Set the notification info
            //we use the pending intent, passing our ui intent over, which will get called
            //when the notification is tapped.
            notification.SetLatestEventInfo(this, title, desc, PendingIntent.GetActivity(this, 0, uiIntent, 0));

            //Show the notification
            notificationManager.Notify(1, notification);
            dialogNotify (title, desc);
        }

        protected void dialogNotify(String title, String message)
        {

            MainActivity.instance.RunOnUiThread(() => {
                AlertDialog.Builder dlg = new AlertDialog.Builder(MainActivity.instance);
                AlertDialog alert = dlg.Create();
                alert.SetTitle(title);
                alert.SetButton("Ok", delegate {
                    alert.Dismiss();
                });
                alert.SetMessage(message);
                alert.Show();
            });
        }


12. Potlačte abstraktní členy **OnUnRegistered()**, **OnRecoverableError()** a **OnError()** tak, aby se váš kód zkompiloval:

        protected override void OnUnRegistered(Context context, string registrationId)
        {
            Log.Verbose(MyBroadcastReceiver.TAG, "GCM Unregistered: " + registrationId);

            createNotification("GCM Unregistered...", "The device has been unregistered!");
        }

        protected override bool OnRecoverableError(Context context, string errorId)
        {
            Log.Warn(MyBroadcastReceiver.TAG, "Recoverable Error: " + errorId);

            return base.OnRecoverableError (context, errorId);
        }

        protected override void OnError(Context context, string errorId)
        {
            Log.Error(MyBroadcastReceiver.TAG, "GCM Error: " + errorId);
        }


##Spusťte aplikaci v emulátoru

Pokud spustíte tuto aplikaci v emulátoru, ujistěte se, že používáte virtuální zařízení Android (AVD) podporující rozhraní API Google.

> [AZURE.IMPORTANT] Aby bylo možné přijímat nabízená oznámení, musíte vytvořit účet Google na virtuálním zařízení se systémem Android. (V emulátoru přejděte na **Nastavení** a klikněte na tlačítko **Přidat účet**.) Ujistěte se také, že je emulátor připojen k internetu.

>[AZURE.NOTE] Návrh oznámení v systému Android verze 5.0 a novější představuje významné odchýlení od předchozích verzí. Další informace naleznete v tématu [Oznámení systému Android](http://go.microsoft.com/fwlink/?LinkId=615880).


1. Z části **Nástroje** klikněte na tlačítko **Otevřít správce emulátoru Android**, vyberte zařízení a pak klikněte na tlačítko **Upravit**.

    ![][18]

2. Vyberte **rozhraní API Google** v části **Cíl** a pak klikněte na tlačítko **OK**.

    ![][19]

3. Na horním panelu nástrojů klikněte na tlačítko **Spustit** a pak vyberte svou aplikaci. Spustí se emulátor a pak se spustí aplikace.

  Aplikace načte *registrationId* z GCM a zaregistruje se pomocí centra oznámení.

##Odesílání oznámení z backendu


Příjem oznámení ve vaší aplikaci můžete otestovat zasláním oznámení na [portál Azure Classic] prostřednictvím karty ladění v centru oznámení, jak je znázorněno na obrazovce níže.

![][30]


Nabízená oznámení se většinou posílají ve službě backend, jako je služba Mobile Services, nebo v technologii ASP.NET pomocí kompatibilní knihovny. Můžete také použít rozhraní API REST k přímému zasílání oznámení, pokud pro vaše prostředí backend není dostupná žádná knihovna.

Tady je seznam některých dalších kurzů, které se týkají zasílání oznámení:

- ASP.NET: Viz [Použití centra oznámení pro nabízená oznámení uživatelům].
- Sada Azure Notification Hubs Java SDK: Informace o odesílání oznámení z Javy najdete v článku [Jak používat Notification Hubs z Javy](notification-hubs-java-push-notification-tutorial.md). Tato metoda prošla pro potřeby vývoje pro Android testováním v Eclipse.
- PHP: Viz [Jak používat Notification Hubs z PHP](notification-hubs-php-push-notification-tutorial.md).


V dalším pododdílu kurzu odešlete oznámení pomocí konzolové aplikace .NET a pomocí mobilních služeb prostřednictvím skriptu uzlu.

####(Volitelné) Odesílání oznámení pomocí aplikace .NET

V této části si ukážeme odesílání oznámení pomocí konzolové aplikace .NET.

1. Vytvořte novou konzolovou aplikaci Visual C#:

    ![][20]

2. Ve Visual Studiu klikněte na položku **Nástroje**, klikněte na **Správce balíčků NuGet** a pak klikněte na **Konzola Správce balíčků**.

    Tím se zobrazí Konzola Správce balíčků ve Visual Studiu.

3. V okně konzoly Správce balíčků nastavte **Výchozí projekt** na nový projekt konzolové aplikace a pak v okně konzoly spusťte následující příkaz:

        Install-Package Microsoft.Azure.NotificationHubs

    Ten přidá odkaz na sadu SDK centra oznámení Azure pomocí <a href="http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/">balíčku Microsoft.Azure.Notification Hubs NuGet</a>.

    ![](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-package-manager.png)

4. Otevřete soubor Program.cs a přidejte následující příkaz `using`:

        using Microsoft.Azure.NotificationHubs;

5. Do třídy `Program` přidejte následující metodu: Aktualizujte zástupný text pomocí připojovacího řetězce *DefaultFullSharedAccessSignature* a názvu centra z [portál Azure Classic].

        private static async void SendNotificationAsync()
        {
            NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString("<connection string with full access>", "<hub name>");
            await hub.SendGcmNativeNotificationAsync("{ \"data\" : {\"message\":\"Hello from Azure!\"}}");
        }

6. Přidejte následující řádky do vaší **Hlavní** metody:

         SendNotificationAsync();
         Console.ReadLine();

7. Stiskněte klávesu F5 a spusťte aplikaci. Měli byste obdržet oznámení v aplikaci.

    ![][21]

####(Volitelné) Odesílání oznámení pomocí mobilní služby

1. Sledujte [Začínáme používat Mobile Services]

1. Přihlaste se do [portál Azure Classic] a vyberte mobilní služby.

2. Vyberte kartu **Plánovač** nahoře.

    ![][22]

3. Vytvořte novou naplánovanou úlohu, vložte název a vyberte **Na vyžádání**.

    ![][23]

4. Po vytvoření úlohy klikněte na název úlohy. Klikněte na kartu **Skript** v horním panelu.

5. Vložte následující skript dovnitř funkce plánovače. Ujistěte se, zda jste nahradili zástupné symboly pomocí názvu centra oznámení a připojovacího řetězce pro *DefaultFullSharedAccessSignature* získaného dříve. Klikněte na **Uložit**.

        var azure = require('azure');
        var notificationHubService = azure.createNotificationHubService('<hub name>', '<connection string>');
        notificationHubService.gcm.send(null,'{"data":{"message" : "Hello from Mobile Services!"}}',
          function (error)
          {
            if (!error) {
               console.warn("Notification successful");
            }
            else
            {
              console.warn("Notification failed" + error);
            }
          }
        );

6. Klikněte na tlačítko **Spustit jednou** na dolním panelu. Měli byste obdržet oznámení informační zprávy.

##Další kroky

V tomto jednoduchém příkladu jste vysílali oznámení pro všechna zařízení Android. Chcete-li se zaměřit na konkrétní uživatele, využijte kurz [Použití centra oznámení pro nabízená oznámení uživatelům]. Pokud chcete segmentovat uživatele podle zájmových skupin, můžete si přečíst kurz [Používání centra oznámení k odesílání novinek]. Další informace o tom, jak používat centra oznámení, naleznete v tématu [Průvodce centry oznámení] a v tématu [Centra oznámení s postupy pro Android].

<!-- Anchors. -->
[Povolení služby GCM (Google Cloud Messaging)]: #register
[Konfigurace centra oznámení]: #configure-hub
[Připojování aplikace k centru oznámení]: #connecting-app
[Spuštění aplikace v emulátoru]: #run-app
[Odesílání oznámení z backendu]: #send
[Další kroky]:#next-steps

<!-- Images. -->

[11]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-configure-android.png

[13]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-create-xamarin-android-app1.png
[15]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-create-xamarin-android-app3.png

[18]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-create-android-app7.png
[19]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-create-android-app8.png

[20]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-create-console-app.png
[21]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-android-toast.png
[22]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-scheduler1.png
[23]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-scheduler2.png

[30]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hubs-debug-hub-gcm.png


<!-- URLs. -->
[Odeslat stránku aplikace]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[Moje aplikace]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK pro Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253
[Začínáme používat Mobile Services]: /develop/mobile/tutorials/get-started-xamarin-android/#create-new-service
[JavaScript a HTML]: /develop/mobile/tutorials/get-started-with-push-js

[portál Azure Classic]: https://manage.windowsazure.com/
[objekt služby WNS]: http://go.microsoft.com/fwlink/p/?LinkId=260591
[Průvodce centry oznámení]: http://msdn.microsoft.com/library/jj927170.aspx
[Centra oznámení s postupy pro Android]: http://msdn.microsoft.com/library/dn282661.aspx

[Použití centra oznámení pro nabízená oznámení uživatelům]: /manage/services/notification-hubs/notify-users-aspnet
[Používání centra oznámení k odesílání novinek]: /manage/services/notification-hubs/breaking-news-dotnet
[Stránka komponenty GCMClient]: http://components.xamarin.com/view/GCMClient
[Stránka Xamarin.NotificationHub Githubu]: https://github.com/SaschaDittmann/Xamarin.NotificationHub
[GitHub]: http://go.microsoft.com/fwlink/p/?LinkId=331329
[Komponenta klienta zasílání zpráv cloudu Google]: http://components.xamarin.com/view/GCMClient/
[Komponenta zasílání zpráv Azure]: http://components.xamarin.com/view/azure-messaging



<!--HONumber=Sep16_HO3-->


