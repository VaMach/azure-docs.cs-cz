
###Vytvoření projektu služby GCM (Google Cloud Messaging) s klíčem rozhraní API

>[AZURE.NOTE] K provedení tohoto postupu potřebujete účet Google s ověřenou e-mailovou adresou. Nový účet Google si můžete vytvořit na stránce <a href="http://go.microsoft.com/fwlink/p/?LinkId=268302" target="_blank">accounts.google.com</a>.

1. Přejděte na [Google Cloud Console](https://console.developers.google.com/project) a přihlaste se pomocí svých přihlašovacích údajů k účtu Google.

2. Klikněte na **Go to project** (Přejít na projekt) a potom klikněte na **Create Project** (Vytvořit projekt).
<!--
    ![](./media/mobile-engagement-enable-google-cloud-messaging/new-project.png)

    ![](./media/mobile-engagement-enable-google-cloud-messaging/new-project-2.png)   
-->
3. Zadejte název projektu.

4. Poznamenejte si číslo projektu, které se zobrazí pod textovým polem **Project name** (Název projektu). Později ho budete potřebovat v kurzu, který se týká zadání informací do souboru manifest v Androidu.
    ![](./media/mobile-engagement-enable-google-cloud-messaging/project-number.png)   
5. Klikněte na **Vytvořit**.

6. V levém sloupci zkontrolujte, jestli je vybraná možnost **Overview** (Přehled), a v části mobilních rozhraní API klikněte na **Google Cloud Messaging**. Na další stránce klikněte na tlačítko **Enable** (Povolit).

    ![](./media/mobile-engagement-enable-google-cloud-messaging/enable-GCM.png)
<!--
    ![](./media/mobile-engagement-enable-google-cloud-messaging/enable-gcm-2.png)
-->
7. Na další stránce klikněte na **Go to Credentials** (Přejít na přihlašovací údaje) a na následující stránce vyberte v prvním rozevíracím seznamu **Google Cloud Messaging** a v druhém **Web server** (Webový server) a potom klikněte na **What credentials do I need?** (Jaké přihlašovací údaje potřebuji?).

    ![](./media/mobile-engagement-enable-google-cloud-messaging/create-server-key.png)

8. Na stránce **Add credentials to your project** (Přidání přihlašovacích údajů k vašemu projektu) klikněte na **Create API key** (Vytvořit klíč rozhraní API).

    ![](./media/mobile-engagement-enable-google-cloud-messaging/create-server-key5.png)
<!--
    ![](./media/mobile-engagement-enable-google-cloud-messaging/create-server-key6.png)
-->
9. Poznamenejte si hodnotu položky **API KEY** (Klíč rozhraní API). Hodnotu klíče rozhraní API budete později potřebovat ke konfiguraci nastavení v části „Native Push“ (Nativní oznámení). Klikněte na **Done** (Hotovo).



<!--HONumber=Jun16_HO2-->


