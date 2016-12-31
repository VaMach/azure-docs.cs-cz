

1. Přihlaste se ke [konzole Firebase](https://firebase.google.com/console/). Vytvořte nový projekt Firebase, pokud jej ještě nemáte.
2. Po vytvoření projektu klikněte na tlačítko **Add Firebase to your Android app** (Přidat Firebase do aplikace pro Android) a postupujte podle pokynů.

    ![](./media/notification-hubs-enable-firebase-cloud-messaging/notification-hubs-add-firebase-to-android-app.png)
3. V konzole Firebase klikněte na ozubené kolečko vašeho projektu a pak klikněte na **Project Settings** (Nastavení projektu).

    ![](./media/notification-hubs-enable-firebase-cloud-messaging/notification-hubs-firebase-console-project-settings.png)
4. Klikněte na kartu **Cloud Messaging** v nastavení projektu a zkopírujte hodnotu **Server key** (Klíč serveru) a **Sender ID** (ID odesílatele). Tyto hodnoty se později použijí ke konfiguraci zásad přístupu centra oznámení a vaší obslužné rutiny oznámení v aplikaci.


<!--HONumber=Dec16_HO2-->


