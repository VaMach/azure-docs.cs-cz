
1. Klikněte na **App Services** > vyberte váš back-end mobilní aplikace > klikněte na **Rychlý start** > vaše klientská platforma (iOS, Android, Xamarin, Cordova).

![Azure Portal se zvýrazněnou možností Mobile Apps Quickstart][quickstart]

2. Pokud není nakonfigurováno připojení k databázi, musíte vytvořit datové připojení.

![Azure Portal s připojením k databázi pro Mobile Apps][connect]

  * Vytvořte novou službu SQL Database a server.

  ![Azure Portal s vytvořením nové databáze a serveru pro Mobile Apps][server]

  * Počkejte na úspěšné vytvoření datového připojení.

  ![Azure Portal s oznámením o vytvoření datového připojení pro Mobile Apps][notification]

  * Vytvoření datového připojení musí proběhnout úspěšně.

  ![Azure Portal s oznámením o vytvoření datového připojení pro Mobile Apps][already-connection]

3. V části **2. Vytvoření rozhraní API tabulky** vyberte jako **Jazyk back-endu** možnost Node.js. Přijměte potvrzení a klikněte na **Create TodoItem table** (Vytvořit tabulku TodoItem). Tím se ve vaší databázi vytvoří nová tabulka *TodoItem*. Pamatujte si, že přepnutím stávajícího prostředí back-end na Node.js přepíšete všechen obsah! Pokud místo toho chcete vytvořit back-end .NET, [postupujte podle těchto pokynů][instructions].

<!-- Images. -->
[quickstart]: ./media/app-service-mobile-configure-new-backend/quickstart.png
[connect]: ./media/app-service-mobile-configure-new-backend/connect-to-bd.png
[notification]: ./media/app-service-mobile-configure-new-backend/notification-data-connection-create.png
[server]: ./media/app-service-mobile-configure-new-backend/create-new-server.png
[already-connection]: ./media/app-service-mobile-configure-new-backend/already-connection.png

<!-- URLs -->
[instructions]: ../articles/app-service-mobile/app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#create-app
