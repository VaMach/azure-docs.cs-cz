
1. V zobrazení řešení (nebo **Průzkumníku řešení** v sadě Visual Studio), klikněte pravým tlačítkem myši **součásti** složku, klikněte na tlačítko **získat další komponenty...** , vyhledejte **klient Google Cloud Messaging** součástí a přidejte ji do projektu.
2. Otevřete soubor projektu ToDoActivity.cs a přidejte následující pomocí příkazu pro třídu:
   
        using Gcm.Client;
3. V **ToDoActivity** třídy, přidejte následující kód nové: 
   
        // Create a new instance field for this activity.
        static ToDoActivity instance = new ToDoActivity();
   
        // Return the current activity instance.
        public static ToDoActivity CurrentActivity
        {
            get
            {
                return instance;
            }
        }
        // Return the Mobile Services client.
        public MobileServiceClient CurrentClient
        {
            get
            {
                return client;
            }
        }
   
    Můžete se připojit k instanci mobilního klienta z procesu nabízené obslužné rutiny služby.
4. Přidejte následující kód, který **OnCreate** metoda, po **MobileServiceClient** je vytvořena:
   
       // Set the current instance of TodoActivity.
       instance = this;
   
       // Make sure the GCM client is set up correctly.
       GcmClient.CheckDevice(this);
       GcmClient.CheckManifest(this);
   
       // Register the app for push notifications.
       GcmClient.Register(this, ToDoBroadcastReceiver.senderIDs);

Vaše **ToDoActivity** je nyní připraven pro přidání nabízených oznámení.

