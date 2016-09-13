
1. Přejděte na [Google Cloud Console](https://console.developers.google.com/project) a přihlaste se pomocí svých přihlašovacích údajů k účtu Google. 
 
2. Klikněte na **Create Project** (Vytvořit projekt), zadejte název projektu a potom klikněte na **Create** (Vytvořit). Pokud se zobrazí výzva, proveďte ověření SMS a znovu klikněte na **Create** (Vytvořit).

    ![](./media/mobile-services-enable-google-cloud-messaging/mobile-services-google-new-project.png)   

     Zadejte novou položku **Project name** (Název projektu) a klikněte na **Create project** (Vytvořit projekt).

3. Klikněte na tlačítko **Utilities and More** (Nástroje a další) a potom na **Project Information** (Informace o projektu). Poznamenejte si hodnotu **Project Number** (Číslo projektu). Tuto hodnotu budete muset nastavit jako proměnnou `SenderId` v klientské aplikaci.

    ![](./media/mobile-services-enable-google-cloud-messaging/notification-hubs-utilities-and-more.png)


4. Na řídicím panelu projektu v části **Mobile APIs** (Mobilní rozhraní API) klikněte na **Google Cloud Messaging** a potom na další stránce klikněte na **Enable API** (Povolit rozhraní API) a přijměte podmínky služby. 

    ![Povolení GCM](./media/mobile-services-enable-google-cloud-messaging/enable-GCM.png)

    ![Povolení GCM](./media/mobile-services-enable-google-cloud-messaging/enable-gcm-2.png) 

5. Na řídicím panelu projektu klikněte na **Credentials**(Přihlašovací údaje)  > **Create Credential**(Vytvořit přihlašovací údaje)  > **API Key** (Klíč rozhraní API). 

    ![](./media/mobile-services-enable-google-cloud-messaging/mobile-services-google-create-server-key.png)

6. V části **Create a new key** (Vytvořit nový klíč) klikněte na **Server key** (Klíč serveru), zadejte název klíče a potom klikněte na **Create** (Vytvořit).

7. Poznamenejte si hodnotu položky **API KEY** (Klíč rozhraní API).

    Pomocí této hodnoty klíče API potom povolíte službě Azure ověření na serveru GCM a odesílání nabízených oznámení jménem vaší aplikace.




<!--HONumber=sep16_HO1-->


