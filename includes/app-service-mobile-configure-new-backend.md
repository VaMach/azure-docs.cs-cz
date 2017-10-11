
1. Klikněte **App Services** tlačítko vyberte vaše mobilní aplikace back-endu, vyberte **rychlý Start**a pak vybrat klientskou platformu (iOS, Android, Xamarin, Cordova).

    ![Portál Azure s mobilní aplikace rychlý start zvýrazněná][quickstart]

2. Pokud není nakonfigurováno připojení k databázi, vytvořte ji následujícím způsobem:

    ![Portál Azure s mobilní aplikací připojení k databázi][connect]

    a. Vytvořte novou databázi SQL a serveru.

    ![Portál Azure s Mobile Apps vytvářet nové databáze a serveru][server]

    b. Počkejte na úspěšné vytvoření datového připojení.

    ![Azure portálu oznámení o úspěšném vytvoření datové připojení][notification]

    c. Vytvoření datového připojení musí proběhnout úspěšně.

    ![Azure portálu oznámení "Již máte data připojení"][already-connection]

3. V části **2. Vytvoření rozhraní API tabulky** vyberte jako **Jazyk back-endu** možnost Node.js. 
 
4. Přijměte potvrzení a pak vyberte **vytvořit tabulku TodoItem**.  
    Tato akce vytvoří novou tabulku položky úkolů ve vaší databázi. 

    >[!IMPORTANT]
    > Probíhá přepínání existující back-end na Node.js přepíše veškerý obsah. Místo toho vytvoření rozhraní .NET back-end, naleznete v tématu [pracovat s .NET back-end server SDK pro Mobile Apps][instructions].

<!-- Images. -->
[quickstart]: ./media/app-service-mobile-configure-new-backend/quickstart.png
[connect]: ./media/app-service-mobile-configure-new-backend/connect-to-bd.png
[notification]: ./media/app-service-mobile-configure-new-backend/notification-data-connection-create.png
[server]: ./media/app-service-mobile-configure-new-backend/create-new-server.png
[already-connection]: ./media/app-service-mobile-configure-new-backend/already-connection.png

<!-- URLs -->
[instructions]: ../articles/app-service-mobile/app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#create-app
