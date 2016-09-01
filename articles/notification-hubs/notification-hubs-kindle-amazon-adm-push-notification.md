<properties
    pageTitle="Začínáme s používáním Azure Notification Hubs pro aplikace Kindle | Microsoft Azure"
    description="V tomto kurzu zjistíte, jak používat Azure Notification Hubs k odesílání nabízených oznámení do aplikace Kindle."
    services="notification-hubs"
    documentationCenter=""
    authors="wesmc7777"
    manager="erikre"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-kindle"
    ms.devlang="Java"
    ms.topic="hero-article"
    ms.date="06/29/2016"
    ms.author="wesmc"/>

# Začínáme s použitím Notification Hubs pro aplikace Kindle

[AZURE.INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

##Přehled

V tomto kurzu zjistíte, jak používat Azure Notification Hubs k odesílání nabízených oznámení do aplikace Kindle.
Vytvoříte prázdnou aplikaci systému Kindle, která bude přijímat nabízená oznámení pomocí služby ADM (Amazon Device Messaging).

##Požadavky

V tomto kurzu budete potřebovat následující:

+ Sadu Android SDK (předpokládáme, že použijete Eclipse) získejte z <a href="http://go.microsoft.com/fwlink/?LinkId=389797">lokality Android</a>.
+ Postupujte podle kroků v <a href="https://developer.amazon.com/appsandservices/resources/development-tools/ide-tools/tech-docs/01-setting-up-your-development-environment">Nastavení vašeho vývojového prostředí</a> pro nastavení vývojového prostředí pro Kindle.

##Přidejte novou aplikaci do portálu pro vývojáře

1. Nejprve vytvořte aplikaci na [portálu pro vývojáře Amazon].

    ![][0]

2. Zkopírujte **klíč aplikace**.

    ![][1]

3. Na portálu, klikněte na název vaší aplikace a pak klikněte na kartu **Zasílání zpráv zařízení**.

    ![][2]

4. Klikněte na tlačítko **Vytvoření nového profilu zabezpečení** a pak vytvořte nový profil zabezpečení (například **profil zabezpečení TestAdm**). Potom klikněte na **Uložit**.

    ![][3]

5. Klikněte na tlačítko **Profily zabezpečení** a zobrazte profil zabezpečení, který jste právě vytvořili. Zkopírujte hodnoty **ID klienta** a **Tajný klíč klienta** pro pozdější použití.

    ![][4]

## Vytvořte klíč rozhraní API

1. Otevřete příkazový řádek s oprávněními správce.
2. Přejděte do složky sady SDK pro Android.
3. Zadejte následující příkaz:

        keytool -list -v -alias androiddebugkey -keystore ./debug.keystore

    ![][5]

4.  Pro heslo **keystore** zadejte **android**.

5.  Zkopírujte otisk prstu **MD5**.
6.  Zpět na portálu pro vývojáře na kartě **Zasílání zpráv** klikněte na tlačítko **Android/Kindle** a zadejte název balíčku pro vaši aplikaci (například **com.sample.notificationhubtest**) a hodnotu **MD5** a pak klikněte na tlačítko **Vygenerovat klíč rozhraní API**.

## Přidejte pověření do centra

Na portálu přidejte sdílený tajný klíč klienta a ID klienta na kartě **Konfigurovat** centra oznámení.

## Nastavte aplikaci

> [AZURE.NOTE] Při vytváření aplikace použijte alespoň 17 úrovní rozhraní API.

Přidejte knihovny ADM do projektu Eclipse:

1. Pro získání knihovny ADM [stáhněte sadu SDK]. Extrahujte soubor zip sady SDK.
2. V Eclipse klikněte pravým tlačítkem na projekt a pak klikněte na tlačítko **Vlastnosti**. Vyberte možnost **Cesta sestavení Java** na levé straně a pak vyberte kartu **Knihovny** v horní části. Klikněte na tlačítko **Přidat externí Jar** a vyberte soubor `\SDK\Android\DeviceMessaging\lib\amazon-device-messaging-*.jar` z adresáře, do kterého jste extrahovali Amazon SDK.
3. Stáhněte si sadu SDK NotificationHubs pro Android (odkaz).
4. Rozbalte balíček a pak přetáhněte soubor `notification-hubs-sdk.jar` do složky `libs` v Eclipse.

Upravte manifest aplikace pro podporu ADM:

1. Přidejte obor názvů Amazon do kořenového prvku manifestu:


        xmlns:amazon="http://schemas.amazon.com/apk/res/android"

2. Přidejte oprávnění jako první prvek v rámci prvku manifestu. Nahraďte **[VÁŠ NÁZEV BALÍČKU]** balíčkem, který jste použili k vytvoření aplikace.

        <permission
         android:name="[YOUR PACKAGE NAME].permission.RECEIVE_ADM_MESSAGE"
         android:protectionLevel="signature" />

        <uses-permission android:name="android.permission.INTERNET"/>

        <uses-permission android:name="[YOUR PACKAGE NAME].permission.RECEIVE_ADM_MESSAGE" />

        <!-- This permission allows your app access to receive push notifications
        from ADM. -->
        <uses-permission android:name="com.amazon.device.messaging.permission.RECEIVE" />

        <!-- ADM uses WAKE_LOCK to keep the processor from sleeping when a message is received. -->
        <uses-permission android:name="android.permission.WAKE_LOCK" />

3. Vložte následující prvek jako první podřízený prvek aplikace. Nezapomeňte nahradit **[SVŮJ NÁZEV SLUŽBY]** názvem vaší obslužné rutiny zpráv ADM, které vytvoříte v další části (včetně balíčku) a nahraďte **[NÁZEV VAŠEHO BALÍČKU]** názvem balíčku, pomocí kterého jste vytvořili vaší aplikaci.

        <amazon:enable-feature
              android:name="com.amazon.device.messaging"
                     android:required="true"/>
        <service
            android:name="[YOUR SERVICE NAME]"
            android:exported="false" />

        <receiver
            android:name="[YOUR SERVICE NAME]$Receiver" />

            <!-- This permission ensures that only ADM can send your app registration broadcasts. -->
            android:permission="com.amazon.device.messaging.permission.SEND" >

            <!-- To interact with ADM, your app must listen for the following intents. -->
            <intent-filter>
          <action android:name="com.amazon.device.messaging.intent.REGISTRATION" />
          <action android:name="com.amazon.device.messaging.intent.RECEIVE" />

          <!-- Replace the name in the category tag with your app's package name. -->
          <category android:name="[YOUR PACKAGE NAME]" />
            </intent-filter>
        </receiver>

## Vytvořte obslužnou rutinu zpráv ADM

1. Vytvořte novou třídu, která zdědí z `com.amazon.device.messaging.ADMMessageHandlerBase` a pojmenujte ji `MyADMMessageHandler`, jak je znázorněno na následujícím obrázku:

    ![][6]

2. Přidejte následující příkazy `import`:

        import android.app.NotificationManager;
        import android.app.PendingIntent;
        import android.content.Context;
        import android.content.Intent;
        import android.support.v4.app.NotificationCompat;
        import com.amazon.device.messaging.ADMMessageReceiver;
        import com.microsoft.windowsazure.messaging.NotificationHub

3. Přidejte následující kód ve třídě, kterou jste vytvořili. Nezapomeňte nahradit název centra a připojovací řetězec (naslouchání):

        public static final int NOTIFICATION_ID = 1;
        private NotificationManager mNotificationManager;
        NotificationCompat.Builder builder;
        private static NotificationHub hub;
        public static NotificationHub getNotificationHub(Context context) {
            Log.v("com.wa.hellokindlefire", "getNotificationHub");
            if (hub == null) {
                hub = new NotificationHub("[hub name]", "[listen connection string]", context);
            }
            return hub;
        }

        public MyADMMessageHandler() {
                super("MyADMMessageHandler");
            }

            public static class Receiver extends ADMMessageReceiver
            {
                public Receiver()
                {
                    super(MyADMMessageHandler.class);
                }
            }

            private void sendNotification(String msg) {
                Context ctx = getApplicationContext();

             mNotificationManager = (NotificationManager)
                    ctx.getSystemService(Context.NOTIFICATION_SERVICE);

            PendingIntent contentIntent = PendingIntent.getActivity(ctx, 0,
                new Intent(ctx, MainActivity.class), 0);

            NotificationCompat.Builder mBuilder =
                new NotificationCompat.Builder(ctx)
                .setSmallIcon(R.mipmap.ic_launcher)
                .setContentTitle("Notification Hub Demo")
                .setStyle(new NotificationCompat.BigTextStyle()
                         .bigText(msg))
                .setContentText(msg);

            mBuilder.setContentIntent(contentIntent);
            mNotificationManager.notify(NOTIFICATION_ID, mBuilder.build());
        }

4. Do metody `OnMessage()` přidejte následující kód:

        String nhMessage = intent.getExtras().getString("msg");
        sendNotification(nhMessage);

5. Do metody `OnRegistered` přidejte následující kód:

            try {
        getNotificationHub(getApplicationContext()).register(registrationId);
            } catch (Exception e) {
        Log.e("[your package name]", "Fail onRegister: " + e.getMessage(), e);
            }

6.  Do metody `OnUnregistered` přidejte následující kód:

            try {
                getNotificationHub(getApplicationContext()).unregister();
            } catch (Exception e) {
                Log.e("[your package name]", "Fail onUnregister: " + e.getMessage(), e);
            }

7. Do metody `MainActivity` přidejte následující příkaz importu:

        import com.amazon.device.messaging.ADM;

8. Na konec metody `OnCreate` přidejte následující kód:

        final ADM adm = new ADM(this);
        if (adm.getRegistrationId() == null)
        {
           adm.startRegister();
        } else {
            new AsyncTask() {
                  @Override
                  protected Object doInBackground(Object... params) {
                     try {                       MyADMMessageHandler.getNotificationHub(getApplicationContext()).register(adm.getRegistrationId());
                     } catch (Exception e) {
                         Log.e("com.wa.hellokindlefire", "Failed registration with hub", e);
                         return e;
                     }
                     return null;
                 }
               }.execute(null, null, null);
        }

## Přidejte do aplikace svůj klíč rozhraní API

1. V nástroji Eclipse vytvořte nový soubor s názvem **api_key.txt** v majetku adresáře projektu.
2. Otevřete soubor a zkopírujte klíč rozhraní API generovaný na portálu pro vývojáře Amazon.

## Spusťte aplikaci

1. Spusťte emulátor.
2. V emulátoru potáhněte prstem z horní části a klikněte na tlačítko **Nastavení** a pak klikněte na tlačítko **Můj účet** a zaregistrujte se pomocí platného účtu Amazon.
3. V nástroji Eclipse spusťte aplikaci.

> [AZURE.NOTE] Pokud dojde k potížím, zkontrolujte čas emulátoru (nebo zařízení). Časová hodnota musí být přesná. Chcete-li změnit čas emulátoru Kindle, můžete spustit následující příkaz z adresáře nástrojů platformy Android SDK:

        adb shell  date -s "yyyymmdd.hhmmss"

## Odeslat zprávu

Odeslání zprávy pomocí .NET:

        static void Main(string[] args)
        {
            NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString("[conn string]", "[hub name]");

            hub.SendAdmNativeNotificationAsync("{\"data\":{\"msg\" : \"Hello from .NET!\"}}").Wait();
        }

![][7]

<!-- URLs. -->
[portálu pro vývojáře Amazon]: https://developer.amazon.com/home.html
[stáhněte sadu SDK]: https://developer.amazon.com/public/resources/development-tools/sdk

[0]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-portal1.png
[1]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-portal2.png
[2]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-portal3.png
[3]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-portal4.png
[4]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-portal5.png
[5]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-cmd-window.png
[6]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-new-java-class.png
[7]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-notification.png



<!---HONumber=Aug16_HO4-->


