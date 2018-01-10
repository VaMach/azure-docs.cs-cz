

1. Přihlaste se ke [konzole Firebase](https://firebase.google.com/console/). Vytvořte nový projekt Firebase, pokud jej ještě nemáte.
2. Po vytvoření projektu klikněte na tlačítko **Add Firebase to your Android app** (Přidat Firebase do aplikace pro Android) a postupujte podle pokynů.

    ![](./media/notification-hubs-enable-firebase-cloud-messaging/notification-hubs-add-firebase-to-android-app.png)
3. V konzole Firebase klikněte na ozubené kolečko vašeho projektu a pak klikněte na **Project Settings** (Nastavení projektu).

    ![](./media/notification-hubs-enable-firebase-cloud-messaging/notification-hubs-firebase-console-project-settings.png)
4. Klikněte **Obecné** v nastavení projektu a pak stáhnout **google services.json** soubor, který obsahuje klíč rozhraní API serveru a ID klienta.
5. Klikněte **Cloud Messaging** v nastavení projektu a zkopírujte hodnotu **klíč starší verze serveru**. Tato hodnota se použije ke konfiguraci zásad přístupu centra oznámení.
