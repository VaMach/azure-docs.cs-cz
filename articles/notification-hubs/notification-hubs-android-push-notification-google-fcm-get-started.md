---
title: "Odesílání nabízených oznámení do systému Android a pomocí Azure Notification Hubs a služby Firebase Cloud Messaging | Dokumentace Microsoftu"
description: "V tomto kurzu zjistíte, jak používat Azure Notification Hubs a službu služby Firebase Cloud Messaging k odesílání nabízených oznámení do zařízení Android."
services: notification-hubs
documentationcenter: android
keywords: "nabízená oznámení, nabízené oznámení, nabízené oznámení android, fcm, firebase cloud messaging"
author: ysxu
manager: erikre
editor: 
ms.assetid: 02298560-da61-4bbb-b07c-e79bd520e420
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: java
ms.topic: hero-article
ms.date: 07/14/2016
ms.author: yuaxu
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 45a3fa5c7190e039fd637c78a41eeb3f6ede9bc7


---
# <a name="sending-push-notifications-to-android-with-azure-notification-hubs"></a>Odesílání nabízených oznámení do systému Android pomocí Azure Notification Hubs
[!INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

## <a name="overview"></a>Přehled
> [!IMPORTANT]
> Toto téma popisuje nabízená oznámení ve službě Google Firebase Cloud Messaging (FCM). Pokud stále používáte službu Google Cloud Messaging (GCM), přečtěte si článek [Odesílání nabízených oznámení do systému Android a pomocí Azure Notification Hubs a služby GCM](notification-hubs-android-push-notification-google-gcm-get-started.md).
> 
> 

V tomto kurzu zjistíte, jak používat Azure Notification Hubs a službu Firebase Cloud Messaging k odesílání nabízených oznámení do aplikace systému Android.
Vytvoříte prázdnou aplikaci systému Android, která bude přijímat nabízená oznámení pomocí služby Firebase Cloud Messaging (FCM).

[!INCLUDE [notification-hubs-hero-slug](../../includes/notification-hubs-hero-slug.md)]

Hotový kód z tohoto kurzu si můžete stáhnout z Githubu [tady](https://github.com/Azure/azure-notificationhubs-samples/tree/master/Android/GetStartedFirebase).

## <a name="prerequisites"></a>Požadavky
> [!IMPORTANT]
> K dokončení tohoto kurzu potřebujete mít aktivní účet Azure. Pokud účet nemáte, můžete si během několika minut vytvořit bezplatný zkušební účet. Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A643EE910&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-android-get-started).
> 
> 

* Kromě aktivního účtu Azure uvedeného výše budete v tomto kurzu potřebovat nejnovější verzi [Android Studia](http://go.microsoft.com/fwlink/?LinkId=389797).
* Android 2.3 nebo novější pro službu Firebase Cloud Messaging.
* Služba Firebase Cloud Messaging vyžaduje úložiště Google verze 27 nebo novější.
* Služby Google Play 9.0.2 nebo novější pro službu Firebase Cloud Messaging.
* Dokončení tohoto kurzu je předpokladem pro všechny ostatní kurzy Notification Hubs pro Android Apps.

## <a name="create-a-new-android-studio-project"></a>Vytvoření nového projektu v Android Studiu
1. V nástroji Android Studio spusťte nový projekt Android Studio.
   
       ![Android Studio - new project](./media/notification-hubs-android-push-notification-google-fcm-get-started/notification-hubs-android-studio-new-project.png)
2. Zvolte faktor formuláře **Telefon i tablet** a hodnotu **Minimální SDK**, které chcete podporovat. Pak klikněte na tlačítko **Další**.
   
       ![Android Studio - project creation workflow](./media/notification-hubs-android-push-notification-google-fcm-get-started/notification-hubs-android-studio-choose-form-factor.png)
3. Zvolte možnost **Prázdná aktivita** pro hlavní aktivitu, klikněte na tlačítko **Další** a pak klikněte na tlačítko **Dokončit**.

## <a name="create-a-project-that-supports-firebase-cloud-messaging"></a>Vytvoření projektu, který podporuje službu Firebase Cloud Messaging
[!INCLUDE [notification-hubs-enable-firebase-cloud-messaging](../../includes/notification-hubs-enable-firebase-cloud-messaging.md)]

## <a name="configure-a-new-notification-hub"></a>Konfigurace nového centra oznámení
[!INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]

&emsp;&emsp;6. V centru oznámení v okně **Nastavení** vyberte **Služby oznámení** a potom **Google (GCM)**. Zadejte klíč serveru FCM, který jste si zkopírovali z [konzoly Firebase](https://firebase.google.com/console/) a klikněte na **Uložit**.

&emsp;&emsp;![Azure Notification Hubs – Google (GCM)](./media/notification-hubs-android-push-notification-google-fcm-get-started/notification-hubs-gcm-api.png)

Vaše centrum oznámení je teď nakonfigurováno pro práci se službou Firebase Cloud Messaging. Zároveň máte připojovací řetězce, pomocí kterých můžete svojí aplikaci zaregistrovat pro příjem a odesílání nabízených oznámení.

## <a name="a-idconnectingappaconnect-your-app-to-the-notification-hub"></a><a id="connecting-app"></a>Připojte aplikaci k centru oznámení
### <a name="add-google-play-services-to-the-project"></a>Přidejte do projektu služby Google Play
[!INCLUDE [Add Play Services](../../includes/notification-hubs-android-studio-add-google-play-services.md)]

### <a name="adding-azure-notification-hubs-libraries"></a>Přidání knihoven Azure Notification Hubs
1. Do souboru `Build.Gradle` pro **aplikaci** přidejte následující řádky v části **závislosti**.
   
        compile 'com.microsoft.azure:notification-hubs-android-sdk:0.4@aar'
        compile 'com.microsoft.azure:azure-notifications-handler:1.0.1@aar'
2. Přidejte následující úložiště za část **závislosti**.
   
        repositories {
            maven {
                url "http://dl.bintray.com/microsoftazuremobile/SDK"
            }
        }

### <a name="updating-the-androidmanifestxml"></a>Probíhá aktualizace souboru AndroidManifest.xml.
1. Abychom zajistili podporu FCM, musíme implementovat službu naslouchání ID instance do našeho kódu, který se používá k [získání registračních tokenů](https://firebase.google.com/docs/cloud-messaging/android/client#sample-register) pomocí [rozhraní FirebaseInstanceId API společnosti Google](https://firebase.google.com/docs/reference/android/com/google/firebase/iid/FirebaseInstanceId). V tomto kurzu pojmenujeme třídu `MyInstanceIDService`. 
   
    Přidejte následující definice služby do souboru AndroidManifest.xml uvnitř značky `<application>`. 
   
        <service android:name=".MyInstanceIDService">
            <intent-filter>
                <action android:name="com.google.firebase.INSTANCE_ID_EVENT"/>
            </intent-filter>
        </service>
2. Po obdržení registračního tokenu FCM z rozhraní FirebaseInstanceId API ho použijeme k [registraci do centra oznámení Azure](notification-hubs-push-notification-registration-management.md). Tuto registrace podpoříme na pozadí pomocí `IntentService` s názvem `RegistrationIntentService`. Tato služba bude také odpovědná za aktualizaci registračního tokenu FCM.
   
    Přidejte následující definice služby do souboru AndroidManifest.xml uvnitř značky `<application>`. 
   
        <service
            android:name=".RegistrationIntentService"
            android:exported="false">
        </service>
3. Také definujeme příjemce pro příjem oznámení. Přidejte následující definice příjemce do souboru AndroidManifest.xml uvnitř značky `<application>`. Nahraďte zástupný symbol `<your package>` pomocí skutečného názvu balíčku zobrazeného v horní části souboru `AndroidManifest.xml`.
   
        <receiver android:name="com.microsoft.windowsazure.notifications.NotificationsBroadcastReceiver"
            android:permission="com.google.android.c2dm.permission.SEND">
            <intent-filter>
                <action android:name="com.google.android.c2dm.intent.RECEIVE" />
                <category android:name="<your package name>" />
            </intent-filter>
        </receiver>
4. Přidejte následující nezbytná oprávnění související s FCM pod značkou `</application>`. Nezapomeňte nahradit `<your package>` názvem balíčku, který je zobrazen v horní části souboru `AndroidManifest.xml`.
   
    Další informace o těchto oprávnění najdete v článku [Nastavení klientské aplikace GCM pro Android](https://developers.google.com/cloud-messaging/android/client#manifest) a [Migrace klientské aplikace GCM pro Android do služby Firebase Cloud Messaging](https://developers.google.com/cloud-messaging/android/android-migrate-fcm#remove_the_permissions_required_by_gcm).
   
        <uses-permission android:name="android.permission.INTERNET"/>
        <uses-permission android:name="android.permission.GET_ACCOUNTS"/>
        <uses-permission android:name="com.google.android.c2dm.permission.RECEIVE" />

### <a name="adding-code"></a>Přidání kódu
1. V zobrazení projektu rozbalte **app** > **src** > **main** > **java**. Klikněte pravým tlačítkem na váš balíček ve složce **java**, klikněte na tlačítko **Nový** a pak klikněte na tlačítko **třída jazyka Java**. Přidejte novou třídu s názvem `NotificationSettings`. 
   
    ![Android Studio – nová třída Java](./media/notification-hubs-android-push-notification-google-fcm-get-started/notification-hub-android-new-class.png)
   
    Nezapomeňte aktualizovat tyto tři zástupné symboly v následujícím kódu pro třídu `NotificationSettings`:
   
   * **SenderId**: ID odesílatele, které jste získali v [konzole Firebase](https://firebase.google.com/console/)v nastavení projektu na kartě **Zprávy v cloudu**.
   * **HubListenConnectionString**: připojovací řetězec **DefaultListenAccessSignature** pro rozbočovač. Tento připojovací řetězec můžete zkopírovat kliknutím na položku **Zásady přístupu** v okně **Nastavení** rozbočovače na [Azure Portal].
   * **HubName**: použije název centra oznámení, který se zobrazí v centru okna na webu [Azure Portal].
     
     Kód `NotificationSettings`:
     
       public class NotificationSettings {
     
           public static String SenderId = "<Your project number>";
           public static String HubName = "<Your HubName>";
           public static String HubListenConnectionString = "<Enter your DefaultListenSharedAccessSignature connection string>";
       }
2. Pomocí kroků výše přidejte další novou třídu s názvem `MyInstanceIDService`. Toto bude naše implementace služby procesu naslouchání Instance ID.
   
    Kód pro tuto třídu bude volat naši službu `IntentService`, aby na pozadí [obnovila token FCM](https://developers.google.com/instance-id/guides/android-implementation#refresh_tokens).
   
        import android.content.Intent;
        import android.util.Log;
        import com.google.firebase.iid.FirebaseInstanceIdService;

        public class MyInstanceIDService extends FirebaseInstanceIdService {

            private static final String TAG = "MyInstanceIDService";

            @Override
            public void onTokenRefresh() {

                Log.d(TAG, "Refreshing GCM Registration Token");

                Intent intent = new Intent(this, RegistrationIntentService.class);
                startService(intent);
            }
        };


1. Přidejte další novou třídu do projektu s názvem, `RegistrationIntentService`. Toto bude implementace pro službu `IntentService`, která zpracuje [obnovení tokenu FCM](https://developers.google.com/instance-id/guides/android-implementation#refresh_tokens) a [registraci v centru oznámení](notification-hubs-push-notification-registration-management.md).
   
    Pro tuto třídu použijte následující kód.
   
        import android.app.IntentService;
        import android.content.Intent;
        import android.content.SharedPreferences;
        import android.preference.PreferenceManager;
        import android.util.Log;        
        import com.google.firebase.iid.FirebaseInstanceId;
        import com.microsoft.windowsazure.messaging.NotificationHub;
   
        public class RegistrationIntentService extends IntentService {
   
            private static final String TAG = "RegIntentService";
   
            private NotificationHub hub;
   
            public RegistrationIntentService() {
                super(TAG);
            }
   
            @Override
            protected void onHandleIntent(Intent intent) {
   
                SharedPreferences sharedPreferences = PreferenceManager.getDefaultSharedPreferences(this);
                String resultString = null;
                String regID = null;
                String storedToken = null;
   
                try {
                    String FCM_token = FirebaseInstanceId.getInstance().getToken();
                    Log.d(TAG, "FCM Registration Token: " + FCM_token);
   
                    // Storing the registration id that indicates whether the generated token has been
                    // sent to your server. If it is not stored, send the token to your server,
                    // otherwise your server should have already received the token.
                    if (((regID=sharedPreferences.getString("registrationID", null)) == null)){
   
                        NotificationHub hub = new NotificationHub(NotificationSettings.HubName,
                                NotificationSettings.HubListenConnectionString, this);
                        Log.d(TAG, "Attempting a new registration with NH using FCM token : " + FCM_token);
                        regID = hub.register(FCM_token).getRegistrationId();
   
                        // If you want to use tags...
                        // Refer to : https://azure.microsoft.com/en-us/documentation/articles/notification-hubs-routing-tag-expressions/
                        // regID = hub.register(token, "tag1,tag2").getRegistrationId();
   
                        resultString = "New NH Registration Successfully - RegId : " + regID;
                        Log.d(TAG, resultString);
   
                        sharedPreferences.edit().putString("registrationID", regID ).apply();
                        sharedPreferences.edit().putString("FCMtoken", FCM_token ).apply();
                    }
   
                    // Check if the token may have been compromised and needs refreshing.
                    else if ((storedToken=sharedPreferences.getString("FCMtoken", "")) != FCM_token) {
   
                        NotificationHub hub = new NotificationHub(NotificationSettings.HubName,
                                NotificationSettings.HubListenConnectionString, this);
                        Log.d(TAG, "NH Registration refreshing with token : " + FCM_token);
                        regID = hub.register(FCM_token).getRegistrationId();
   
                        // If you want to use tags...
                        // Refer to : https://azure.microsoft.com/en-us/documentation/articles/notification-hubs-routing-tag-expressions/
                        // regID = hub.register(token, "tag1,tag2").getRegistrationId();
   
                        resultString = "New NH Registration Successfully - RegId : " + regID;
                        Log.d(TAG, resultString);
   
                        sharedPreferences.edit().putString("registrationID", regID ).apply();
                        sharedPreferences.edit().putString("FCMtoken", FCM_token ).apply();
                    }
   
                    else {
                        resultString = "Previously Registered Successfully - RegId : " + regID;
                    }
                } catch (Exception e) {
                    Log.e(TAG, resultString="Failed to complete registration", e);
                    // If an exception happens while fetching the new token or updating our registration data
                    // on a third-party server, this ensures that we'll attempt the update at a later time.
                }
   
                // Notify UI that registration has completed.
                if (MainActivity.isVisible) {
                    MainActivity.mainActivity.ToastNotify(resultString);
                }
            }
        }
2. Do vaší třídy `MainActivity` přidejte následující prohlášení `import` nad deklaraci třídy.
   
        import com.google.android.gms.common.ConnectionResult;
        import com.google.android.gms.common.GoogleApiAvailability;
        import com.microsoft.windowsazure.notifications.NotificationsManager;
        import android.content.Intent;
        import android.util.Log;
        import android.widget.TextView;
        import android.widget.Toast;
3. Přidejte následující soukromé členy v horní části třídy. Tyto [budeme používat ke kontrole dostupnosti služby Google Play dle doporučení Google](https://developers.google.com/android/guides/setup#ensure_devices_have_the_google_play_services_apk).
   
        public static MainActivity mainActivity;
        public static Boolean isVisible = false;    
        private static final String TAG = "MainActivity";
        private static final int PLAY_SERVICES_RESOLUTION_REQUEST = 9000;
4. Ve své třídě `MainActivity` přidejte následující metodu pro dostupnost služeb Google Play. 
   
        /**
         * Check the device to make sure it has the Google Play Services APK. If
         * it doesn't, display a dialog that allows users to download the APK from
         * the Google Play Store or enable it in the device's system settings.
         */
        private boolean checkPlayServices() {
            GoogleApiAvailability apiAvailability = GoogleApiAvailability.getInstance();
            int resultCode = apiAvailability.isGooglePlayServicesAvailable(this);
            if (resultCode != ConnectionResult.SUCCESS) {
                if (apiAvailability.isUserResolvableError(resultCode)) {
                    apiAvailability.getErrorDialog(this, resultCode, PLAY_SERVICES_RESOLUTION_REQUEST)
                            .show();
                } else {
                    Log.i(TAG, "This device is not supported by Google Play Services.");
                    ToastNotify("This device is not supported by Google Play Services.");
                    finish();
                }
                return false;
            }
            return true;
        }
5. Ve třídě `MainActivity` přidejte následující kód, který zkontrolujte služby Google Play před voláním služby `IntentService`, aby získal registrační token FCM a zaregistroval se v centru oznámení.
   
        public void registerWithNotificationHubs()
        {
            if (checkPlayServices()) {
                // Start IntentService to register this application with FCM.
                Intent intent = new Intent(this, RegistrationIntentService.class);
                startService(intent);
            }
        }
6. Do metody `OnCreate` třídy `MainActivity` přidejte následující kód pro spuštění procesu registrace při vytvoření aktivity.
   
        @Override
        protected void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.activity_main);
   
            mainActivity = this;
            NotificationsManager.handleNotifications(this, NotificationSettings.SenderId, MyHandler.class);
            registerWithNotificationHubs();
        }
7. Přidejte  tyto další metody do `MainActivity` pro ověření stavu aplikace a stavu sestavy ve vaší aplikaci.
   
        @Override
        protected void onStart() {
            super.onStart();
            isVisible = true;
        }
   
        @Override
        protected void onPause() {
            super.onPause();
            isVisible = false;
        }
   
        @Override
        protected void onResume() {
            super.onResume();
            isVisible = true;
        }
   
        @Override
        protected void onStop() {
            super.onStop();
            isVisible = false;
        }
   
        public void ToastNotify(final String notificationMessage) {
            runOnUiThread(new Runnable() {
                @Override
                public void run() {
                    Toast.makeText(MainActivity.this, notificationMessage, Toast.LENGTH_LONG).show();
                    TextView helloText = (TextView) findViewById(R.id.text_hello);
                    helloText.setText(notificationMessage);
                }
            });
        }
8. Metoda `ToastNotify` používá ovládání *„Hello World“* `TextView` k trvalému hlášení stavu a oznámení v aplikaci. Do rozložení activity_main.xml přidejte následující id pro ovládací prvek.
   
       android:id="@+id/text_hello"
9. Vedle přidáme podtřídu pro našeho příjemce, kterého jsme definovali v souboru AndroidManifest.xml. Přidejte další novou třídu do projektu s názvem `MyHandler`.
10. Nad `MyHandler.java` přidejte následující příkazy pro import:
    
        import android.app.NotificationManager;
        import android.app.PendingIntent;
        import android.content.Context;
        import android.content.Intent;
        import android.media.RingtoneManager;
        import android.net.Uri;
        import android.os.Bundle;
        import android.support.v4.app.NotificationCompat;
        import com.microsoft.windowsazure.notifications.NotificationsHandler;
11. Přidejte následující kód pro třídu `MyHandler` a vytvořte tak podtřídu `com.microsoft.windowsazure.notifications.NotificationsHandler`.
    
    Tento kód přepíše metodu `OnReceive`, aby obslužná rutina nahlásila oznámení, která byla přijata. Obslužná rutina také odesílá nabízená oznámení správci oznámení Android pomocí metody `sendNotification()`. Metoda `sendNotification()` musí být spouštěna, když není aplikace spuštěna a není přijato oznámení.
    
        public class MyHandler extends NotificationsHandler {
            public static final int NOTIFICATION_ID = 1;
            private NotificationManager mNotificationManager;
            NotificationCompat.Builder builder;
            Context ctx;
    
            @Override
            public void onReceive(Context context, Bundle bundle) {
                ctx = context;
                String nhMessage = bundle.getString("message");
                sendNotification(nhMessage);
                if (MainActivity.isVisible) {
                    MainActivity.mainActivity.ToastNotify(nhMessage);
                }
            }
    
            private void sendNotification(String msg) {
    
                Intent intent = new Intent(ctx, MainActivity.class);
                intent.addFlags(Intent.FLAG_ACTIVITY_CLEAR_TOP);
    
                mNotificationManager = (NotificationManager)
                        ctx.getSystemService(Context.NOTIFICATION_SERVICE);
    
                PendingIntent contentIntent = PendingIntent.getActivity(ctx, 0,
                        intent, PendingIntent.FLAG_ONE_SHOT);
    
                Uri defaultSoundUri = RingtoneManager.getDefaultUri(RingtoneManager.TYPE_NOTIFICATION);
                NotificationCompat.Builder mBuilder =
                        new NotificationCompat.Builder(ctx)
                                .setSmallIcon(R.mipmap.ic_launcher)
                                .setContentTitle("Notification Hub Demo")
                                .setStyle(new NotificationCompat.BigTextStyle()
                                        .bigText(msg))
                                .setSound(defaultSoundUri)
                                .setContentText(msg);
    
                mBuilder.setContentIntent(contentIntent);
                mNotificationManager.notify(NOTIFICATION_ID, mBuilder.build());
            }
        }
12. V Android Studio na řádku nabídek klikněte na tlačítko **Sestavit** > **Znovu sestavit projekt** a ujistěte se, zda se ve vašem kódu nenachází žádné chyby.
13. Spusťte aplikaci na svém zařízení a ověřte, jestli se úspěšně zaregistrovala v centru oznámení. 
    
    > [!NOTE]
    > Registrace může být při počátečním spuštění neúspěšná, dokud nezavoláte metodu `onTokenRefresh()` ze služby instance ID. Obnovení by mělo inicializovat úspěšnou registraci v centru oznámení.
    > 
    > 

## <a name="sending-push-notifications"></a>Odeslání nabízených oznámení
Můžete otestovat přijímání nabízených oznámení ve vaší aplikaci jejich odesláním prostřednictvím [Azure Portal] – hledejte část **Poradce při potížích** v okně centra, jak je uvedeno níže.

![Azure Notification Hubs – testovací odeslání](./media/notification-hubs-android-push-notification-google-fcm-get-started/notification-hubs-test-send.png)

[!INCLUDE [notification-hubs-sending-notifications-from-the-portal](../../includes/notification-hubs-sending-notifications-from-the-portal.md)]

## <a name="optional-send-push-notifications-directly-from-the-app"></a>(Volitelné) Zasílání nabízených oznámení přímo z aplikace
> [!IMPORTANT]
> Tento příklad odesílání oznámení z klientské aplikace poskytujeme jenom pro výukové účely. Vzhledem k tomu, že v klientské aplikaci bude nutná přítomnost `DefaultFullSharedAccessSignature`, bude centrum oznámení vystaveno riziku, kdy může některý uživatel získat přístup k odesílání neoprávněných oznámení vašim klientům.
> 
> 

Za normálních okolností byste odesílali oznámení pomocí serveru backend. V některých případech můžete chtít možnost zasílání nabízených oznámení přímo z klientské aplikace. Tato část vysvětluje postup odesílání oznámení z klienta pomocí [API služby REST centra oznámení Azure](https://msdn.microsoft.com/library/azure/dn223264.aspx).

1. V zobrazení projektu Android Studio rozbalte možnost **App** > **src** > **main** > **res** > **layout**. Otevřete soubor rozložení `activity_main.xml` a klikněte na kartu **Text** pro aktualizaci textového obsahu souboru. Aktualizujte ho pomocí kódu níže, který přidává nové `Button` a `EditText` ovládací prvky pro zasílání zpráv s nabízeným oznámením centra oznámení. Přidejte tento kód v dolní části, těsně před `</RelativeLayout>`.
   
        <Button
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="@string/send_button"
        android:id="@+id/sendbutton"
        android:layout_centerVertical="true"
        android:layout_centerHorizontal="true"
        android:onClick="sendNotificationButtonOnClick" />
   
        <EditText
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:id="@+id/editTextNotificationMessage"
        android:layout_above="@+id/sendbutton"
        android:layout_centerHorizontal="true"
        android:layout_marginBottom="42dp"
        android:hint="@string/notification_message_hint" />
2. V zobrazení projektu Android Studio rozbalte **App** > **src** > **main** > **res** > **values**. Otevřete soubor `strings.xml` a přidejte řetězcové hodnoty, které jsou odkazovány pomocí nového `Button` a `EditText` ovládacími prvky. Přidejte tyto položky ve spodní části souboru, těsně před `</resources>`.
   
        <string name="send_button">Send Notification</string>
        <string name="notification_message_hint">Enter notification message text</string>
3. Do souboru `NotificationSetting.java` přidejte následující nastavení na třídu `NotificationSettings`.
   
    Aktualizujte `HubFullAccess` pomocí připojovacího řetězce **DefaultFullSharedAccessSignature** pro centrum. Tento připojovací řetězec lze kopírovat z [Azure Portal] kliknutím na položku **zásady přístupu** v okně **Nastavení** pro vaše centrum oznámení.
   
        public static String HubFullAccess = "<Enter Your DefaultFullSharedAccessSignature Connection string>";
4. Do souboru `MainActivity.java` přidejte následující prohlášení `import` nad třídu `MainActivity`.
   
        import java.io.BufferedOutputStream;
        import java.io.BufferedReader;
        import java.io.InputStreamReader;
        import java.io.OutputStream;
        import java.net.HttpURLConnection;
        import java.net.URL;
        import java.net.URLEncoder;
        import javax.crypto.Mac;
        import javax.crypto.spec.SecretKeySpec;
        import android.util.Base64;
        import android.view.View;
        import android.widget.EditText;
5. Do souboru `MainActivity.java` přidejte následující členy v horní části třídy `MainActivity`.    
   
        private String HubEndpoint = null;
        private String HubSasKeyName = null;
        private String HubSasKeyValue = null;
6. Je třeba vytvořit token SaS (Software Access Signature) k ověření požadavku POST k odesílání zpráv do vašeho centra oznámení. To se provede analýzou klíčových dat z připojovacího řetězce a pak vytvořením SaS tokenu, jak je uvedeno v referenci rozhraní REST API [Běžné koncepty](http://msdn.microsoft.com/library/azure/dn495627.aspx). Následující kód představuje příklad implementace.
   
    Do `MainActivity.java` přidejte následující metodu do třídy `MainActivity` k analýze připojovacího řetězce.
   
        /**
         * Example code from http://msdn.microsoft.com/library/azure/dn495627.aspx
         * to parse the connection string so a SaS authentication token can be
         * constructed.
         *
         * @param connectionString This must be the DefaultFullSharedAccess connection
         *                         string for this example.
         */
        private void ParseConnectionString(String connectionString)
        {
            String[] parts = connectionString.split(";");
            if (parts.length != 3)
                throw new RuntimeException("Error parsing connection string: "
                        + connectionString);
   
            for (int i = 0; i < parts.length; i++) {
                if (parts[i].startsWith("Endpoint")) {
                    this.HubEndpoint = "https" + parts[i].substring(11);
                } else if (parts[i].startsWith("SharedAccessKeyName")) {
                    this.HubSasKeyName = parts[i].substring(20);
                } else if (parts[i].startsWith("SharedAccessKey")) {
                    this.HubSasKeyValue = parts[i].substring(16);
                }
            }
        }
7. Do `MainActivity.java` přidejte následující metodu do třídy `MainActivity` k vytvoření ověřovacího tokenu SaS.
   
        /**
         * Example code from http://msdn.microsoft.com/library/azure/dn495627.aspx to
         * construct a SaS token from the access key to authenticate a request.
         *
         * @param uri The unencoded resource URI string for this operation. The resource
         *            URI is the full URI of the Service Bus resource to which access is
         *            claimed. For example,
         *            "http://<namespace>.servicebus.windows.net/<hubName>"
         */
        private String generateSasToken(String uri) {
   
            String targetUri;
            String token = null;
            try {
                targetUri = URLEncoder
                        .encode(uri.toString().toLowerCase(), "UTF-8")
                        .toLowerCase();
   
                long expiresOnDate = System.currentTimeMillis();
                int expiresInMins = 60; // 1 hour
                expiresOnDate += expiresInMins * 60 * 1000;
                long expires = expiresOnDate / 1000;
                String toSign = targetUri + "\n" + expires;
   
                // Get an hmac_sha1 key from the raw key bytes
                byte[] keyBytes = HubSasKeyValue.getBytes("UTF-8");
                SecretKeySpec signingKey = new SecretKeySpec(keyBytes, "HmacSHA256");
   
                // Get an hmac_sha1 Mac instance and initialize with the signing key
                Mac mac = Mac.getInstance("HmacSHA256");
                mac.init(signingKey);
   
                // Compute the hmac on input data bytes
                byte[] rawHmac = mac.doFinal(toSign.getBytes("UTF-8"));
   
                // Using android.util.Base64 for Android Studio instead of
                // Apache commons codec
                String signature = URLEncoder.encode(
                        Base64.encodeToString(rawHmac, Base64.NO_WRAP).toString(), "UTF-8");
   
                // Construct authorization string
                token = "SharedAccessSignature sr=" + targetUri + "&sig="
                        + signature + "&se=" + expires + "&skn=" + HubSasKeyName;
            } catch (Exception e) {
                if (isVisible) {
                    ToastNotify("Exception Generating SaS : " + e.getMessage().toString());
                }
            }
   
            return token;
        }
8. Do `MainActivity.java` přidejte následující metodu do třídy `MainActivity` pro zajištění kliknutí na tlačítko **Odeslat oznámení** a odešlete zprávu nabízeného oznámení do centra pomocí předdefinovaného REST API.
   
        /**
         * Send Notification button click handler. This method parses the
         * DefaultFullSharedAccess connection string and generates a SaS token. The
         * token is added to the Authorization header on the POST request to the
         * notification hub. The text in the editTextNotificationMessage control
         * is added as the JSON body for the request to add a GCM message to the hub.
         *
         * @param v
         */
        public void sendNotificationButtonOnClick(View v) {
            EditText notificationText = (EditText) findViewById(R.id.editTextNotificationMessage);
            final String json = "{\"data\":{\"message\":\"" + notificationText.getText().toString() + "\"}}";
   
            new Thread()
            {
                public void run()
                {
                    try
                    {
                        // Based on reference documentation...
                        // http://msdn.microsoft.com/library/azure/dn223273.aspx
                        ParseConnectionString(NotificationSettings.HubFullAccess);
                        URL url = new URL(HubEndpoint + NotificationSettings.HubName +
                                "/messages/?api-version=2015-01");
   
                        HttpURLConnection urlConnection = (HttpURLConnection)url.openConnection();
   
                        try {
                            // POST request
                            urlConnection.setDoOutput(true);
   
                            // Authenticate the POST request with the SaS token
                            urlConnection.setRequestProperty("Authorization", 
                                generateSasToken(url.toString()));
   
                            // Notification format should be GCM
                            urlConnection.setRequestProperty("ServiceBusNotification-Format", "gcm");
   
                            // Include any tags
                            // Example below targets 3 specific tags
                            // Refer to : https://azure.microsoft.com/en-us/documentation/articles/notification-hubs-routing-tag-expressions/
                            // urlConnection.setRequestProperty("ServiceBusNotification-Tags", 
                            //        "tag1 || tag2 || tag3");
   
                            // Send notification message
                            urlConnection.setFixedLengthStreamingMode(json.length());
                            OutputStream bodyStream = new BufferedOutputStream(urlConnection.getOutputStream());
                            bodyStream.write(json.getBytes());
                            bodyStream.close();
   
                            // Get reponse
                            urlConnection.connect();
                            int responseCode = urlConnection.getResponseCode();
                            if ((responseCode != 200) && (responseCode != 201)) {
                                BufferedReader br = new BufferedReader(new InputStreamReader((urlConnection.getErrorStream())));
                                String line;
                                StringBuilder builder = new StringBuilder("Send Notification returned " +
                                        responseCode + " : ")  ;
                                while ((line = br.readLine()) != null) {
                                    builder.append(line);
                                }
   
                                ToastNotify(builder.toString());
                            }
                        } finally {
                            urlConnection.disconnect();
                        }
                    }
                    catch(Exception e)
                    {
                        if (isVisible) {
                            ToastNotify("Exception Sending Notification : " + e.getMessage().toString());
                        }
                    }
                }
            }.start();
        }

## <a name="testing-your-app"></a>Testování vaší aplikace
#### <a name="push-notifications-in-the-emulator"></a>Nabízená oznámení v emulátoru
Pokud chcete testovat nabízená oznámení uvnitř emulátoru, ověřte, zda bitová kopie emulátoru podporuje úroveň rozhraní Google API, kterou jste zvolili pro vaši aplikaci. Pokud bitová kopie nepodporuje nativní rozhraní Google API, zobrazí se výjimka **SLUŽBA\_NENÍ\_K DISPOZICI**.

Kromě výše uvedeného zajistěte, že jste přidali účet Google do svého spuštěného emulátoru pod položkou **Nastavení** > **účtů**. V opačném případě mohou vaše pokusy o registraci s GCM mít za následek výjimku **OVĚŘOVÁNÍ\_SE NEZDAŘILO**.

#### <a name="running-the-application"></a>Spouštění aplikace.
1. Spusťte aplikaci a všimněte si, že je ID registrace hlášené pro úspěšnou registraci.
   
       ![Testing on Android - Channel registration](./media/notification-hubs-android-push-notification-google-fcm-get-started/notification-hubs-android-studio-registered.png)
2. Zadejte zprávu oznámení k odeslání do všech zařízení Android, která byla zaregistrovaná v centru.
   
       ![Testing on Android - sending a message](./media/notification-hubs-android-push-notification-google-fcm-get-started/notification-hubs-android-studio-set-message.png)
3. Stiskněte tlačítko **Odeslat oznámení**. Všechna zařízení, které mají spuštěné aplikace, zobrazí instance `AlertDialog` se zprávou nabízených oznámení. Zařízení, která nemají spuštěnou aplikaci, ale byla dříve registrována pro nabízená oznámení, obdrží oznámení ve správci oznámení Android. Ta lze zobrazit potažením dolů z levého horního rohu.
   
       ![Testing on Android - notifications](./media/notification-hubs-android-push-notification-google-fcm-get-started/notification-hubs-android-studio-received-message.png)

## <a name="next-steps"></a>Další kroky
Jako další krok doporučujeme tutoriál [Používání center oznámení pro nabízená oznámení uživatelům]. Zobrazí se postup odesílání oznámení z ASP.NET back-end pomocí značek pro cílové konkrétní uživatele.

Pokud chcete segmentovat uživatele podle zájmových skupin, podívejte se na tutoriál [Používání centra oznámení k odesílání novinek].

Další obecné informace o centrech oznámení naleznete v tématu naše [Průvodce centry oznámení].

<!-- Images. -->



<!-- URLs. -->
[Začínáme s nabízenými oznámeními v mobilních službách]: ../mobile-services-javascript-backend-android-get-started-push.md  
[Mobile Services Android SDK]: https://go.microsoft.com/fwLink/?LinkID=280126&clcid=0x409
[Odkazování na projekt knihovny]: http://go.microsoft.com/fwlink/?LinkId=389800
[Portál Azure Classic]: https://manage.windowsazure.com/
[Průvodce centry oznámení]: notification-hubs-push-notification-overview.md
[Používání center oznámení pro nabízená oznámení uživatelům]: notification-hubs-aspnet-backend-gcm-android-push-to-user-google-notification.md
[Používání centra oznámení k odesílání novinek]: notification-hubs-aspnet-backend-android-xplat-segmented-gcm-push-notification.md
[Azure Portal]: https://portal.azure.com



<!--HONumber=Nov16_HO2-->


