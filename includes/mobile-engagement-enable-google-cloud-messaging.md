
###Vytvoření projektu služby GCM (Google Cloud Messaging) s klíčem rozhraní API

>[AZURE.NOTE] K provedení tohoto postupu potřebujete účet Google s ověřenou e-mailovou adresou. Nový účet Google si můžete vytvořit na stránce <a href="http://go.microsoft.com/fwlink/p/?LinkId=268302" target="_blank">accounts.google.com</a>.

1. Přejděte na [Google Cloud Console](https://console.developers.google.com/project) a přihlaste se pomocí svých přihlašovacích údajů k účtu Google.

2. Přejděte na **Všechny projekty** a potom klikněte na **Vytvořit projekt**.

3. Zadejte **Název projektu** a klikněte na tlačítko **Vytvořit**

4. Po vytvoření projektu si nezapomeňte poznamenat **Číslo projektu**, které bude představovat dlouhou číselnou hodnotu. Toto číslo naleznete v části **Oddíl IAM a Admin** v **Nastavení** projektu a bude požadováno později. 
 
    ![](./media/mobile-engagement-enable-google-cloud-messaging/project-number.png)

5. Nyní vytvoříme klíč pro platformu Google Cloud Messaging, která bude použita naší platformou pro odesílání oznámení do zařízení se systémem Android. Přejděte do části **Správce rozhraní API** a klikněte na **Google Cloud Messaging** pod položkou **Mobilní rozhraní API**. 

    ![](./media/mobile-engagement-enable-google-cloud-messaging/gcm.png)

6. Na další stránce klikněte na tlačítko **Povolit**. Řídicí panel zobrazí výzvu k vytvoření přihlašovacích údajů. Takže klikněte na tlačítko **Přejít na přihlašovací údaje**. 

    ![](./media/mobile-engagement-enable-google-cloud-messaging/enable-GCM.png)

6. Vyberte v prvním rozevíracím seznamu **Google Cloud Messaging** a v druhém **Webový server** a potom klikněte na **Jaké přihlašovací údaje potřebuji?**

    ![](./media/mobile-engagement-enable-google-cloud-messaging/create-server-key.png)

7. Na stránce **Add credentials to your project** (Přidání přihlašovacích údajů k vašemu projektu) klikněte na **Create API key** (Vytvořit klíč rozhraní API).

    ![](./media/mobile-engagement-enable-google-cloud-messaging/create-server-key5.png)

8. Poznamenejte si hodnotu položky **API KEY** (Klíč rozhraní API). Hodnotu klíče rozhraní API budete později potřebovat ke konfiguraci nastavení v části „Native Push“ (Nativní oznámení). Klikněte na **Done** (Hotovo).



<!--HONumber=Aug16_HO4-->


