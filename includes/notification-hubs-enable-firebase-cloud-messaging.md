

1. Přihlaste se ke [konzole Firebase](https://firebase.google.com/console/). Vytvořte nový projekt Firebase, pokud jej ještě nemáte.
2. Po vytvoření projektu, vyberte **přidat Firebase svoji aplikaci pro Android**. Postupujte podle pokynů, které jsou k dispozici.

    ![Přidání Firebase do vaší aplikace pro Android](./media/notification-hubs-enable-firebase-cloud-messaging/notification-hubs-add-firebase-to-android-app.png)
3. V konzole Firebase vyberte ikonu pro váš projekt. Potom vyberte **nastavení projektu**.

    ![Vyberte nastavení projektu](./media/notification-hubs-enable-firebase-cloud-messaging/notification-hubs-firebase-console-project-settings.png)
4. Vyberte **Obecné** kartě v nastavení projektu. Pak stáhnout **google services.json** soubor, který obsahuje klíč rozhraní API serveru a ID klienta.
5. Vyberte **Cloud Messaging** v nastavení projektu a poté zkopírujte hodnotu **starší verze serveru klíč**. Tuto hodnotu použijete ke konfiguraci zásad přístupu centra oznámení.
