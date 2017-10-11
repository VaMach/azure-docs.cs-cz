1. Ve vašem **aplikace** projektu, otevřete soubor `AndroidManifest.xml`. V kódu v následujících dvou krocích, nahraďte  *`**my_app_package**`*  s názvem balíčku aplikace pro váš projekt. Toto je hodnota `package` atribut `manifest` značky.
2. Přidejte následující nová oprávnění po existující `uses-permission` element:

        <permission android:name="**my_app_package**.permission.C2D_MESSAGE"
            android:protectionLevel="signature" />
        <uses-permission android:name="**my_app_package**.permission.C2D_MESSAGE" />
        <uses-permission android:name="com.google.android.c2dm.permission.RECEIVE" />
        <uses-permission android:name="android.permission.GET_ACCOUNTS" />
        <uses-permission android:name="android.permission.WAKE_LOCK" />
3. Přidejte následující kód po `application` počáteční značce:

        <receiver android:name="com.microsoft.windowsazure.notifications.NotificationsBroadcastReceiver"
                                         android:permission="com.google.android.c2dm.permission.SEND">
            <intent-filter>
                <action android:name="com.google.android.c2dm.intent.RECEIVE" />
                <category android:name="**my_app_package**" />
            </intent-filter>
        </receiver>
4. Otevřete soubor *ToDoActivity.java*a přidejte následující příkaz importu:

        import com.microsoft.windowsazure.notifications.NotificationsManager;
5. Přidejte následující soukromé proměnné do třídy. Nahraďte  *`<PROJECT_NUMBER>`*  číslem projektu Google přiřazené vaší aplikaci v předchozím postupu.

        public static final String SENDER_ID = "<PROJECT_NUMBER>";
6. Změnit definici *MobileServiceClient* z **privátní** k **veřejné statické**, takže ho teď vypadá takto:

        public static MobileServiceClient mClient;
7. Přidání nové třídy pro zpracování oznámení. Otevřete v prohlížeči projektu klikněte **src** > **hlavní** > **java** uzly a klikněte pravým tlačítkem na název uzlu balíčku. Klikněte na tlačítko **nový**a potom klikněte na **třída jazyka Java**.
8. V **název**, typ `MyHandler`a potom klikněte na **OK**.

    ![](./media/app-service-mobile-android-configure-push/android-studio-create-class.png)

9. V souboru MyHandler nahraďte deklarace třídy se:

        public class MyHandler extends NotificationsHandler {
10. Přidejte následující příkazy pro import `MyHandler` třídy:

        import com.microsoft.windowsazure.notifications.NotificationsHandler;
        import android.app.NotificationManager;
        import android.app.PendingIntent;
        import android.content.Context;
        import android.content.Intent;
        import android.os.AsyncTask;
        import android.os.Bundle;
        import android.support.v4.app.NotificationCompat;
11. Dále přidejte tohoto člena `MyHandler` třídy:

        public static final int NOTIFICATION_ID = 1;
12. V `MyHandler` třídy, přidejte následující kód k přepsání **onRegistered** metodu, která registruje zařízení s centrem oznámení mobilní služby.

        @Override
        public void onRegistered(Context context,  final String gcmRegistrationId) {
           super.onRegistered(context, gcmRegistrationId);

           new AsyncTask<Void, Void, Void>() {

               protected Void doInBackground(Void... params) {
                   try {
                       ToDoActivity.mClient.getPush().register(gcmRegistrationId);
                       return null;
                   }
                   catch(Exception e) {
                       // handle error                
                   }
                   return null;              
               }
           }.execute();
       }
13. V `MyHandler` třídy, přidejte následující kód k přepsání **události onReceive** metoda, což způsobí, že zobrazíte při obdržení oznámení.

        @Override
        public void onReceive(Context context, Bundle bundle) {
               String msg = bundle.getString("message");

               PendingIntent contentIntent = PendingIntent.getActivity(context,
                       0, // requestCode
                       new Intent(context, ToDoActivity.class),
                       0); // flags

               Notification notification = new NotificationCompat.Builder(context)
                       .setSmallIcon(R.drawable.ic_launcher)
                       .setContentTitle("Notification Hub Demo")
                       .setStyle(new NotificationCompat.BigTextStyle().bigText(msg))
                       .setContentText(msg)
                       .setContentIntent(contentIntent)
                       .build();

               NotificationManager notificationManager = (NotificationManager)
                       context.getSystemService(Context.NOTIFICATION_SERVICE);
               notificationManager.notify(NOTIFICATION_ID, notification);
       }
14. Zpět v souboru TodoActivity.java aktualizace **onCreate** metodu *ToDoActivity* třída registrovat třídu obslužné rutiny oznámení. Nezapomeňte přidat tento kód po *MobileServiceClient* je vytvořena instance.

        NotificationsManager.handleNotifications(this, SENDER_ID, MyHandler.class);

    Aplikace je nyní aktualizovat o podporu nabízených oznámení.
