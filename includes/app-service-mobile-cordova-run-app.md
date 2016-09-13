
1. Navštivte [Azure Portal]. Klikněte na **Procházet vše** > **Mobilní aplikace** > prostředí back-end, které jste právě vytvořili. V nastavení mobilní aplikace klikněte na **Rychlý start** > **Cordova**. V části **Configure your client application** (Konfigurace klientské aplikace) vyberte možnost **Vytvořit novou aplikaci** a klikněte na **Stáhnout**. Tím stáhnete dokončený projekt Cordova pro aplikaci přednastavenou k připojení k vašemu prostředí back-end.

2. Rozbalte stažený soubor ZIP do adresáře na pevném disku, přejděte do souboru řešení (.sln) a otevřete ho pomocí aplikace Visual Studio.

5. V aplikaci Visual Studio zvolte platformu řešení (Android, iOS nebo Windows) z rozevíracího seznamu vedle spouštěcí šipky a pak vyberte konkrétní nasazení zařízení nebo emulátor kliknutím na rozevírací seznam na zelené šipce. Všimněte si, že můžete použít výchozí platformu Android a emulátor Ripple. Pokročilejší kurzy vyžadují výběr podporovaného zařízení nebo emulátoru. 

6. Stisknutím klávesy F5 nebo kliknutím na zelenou šipku sestavte a spusťte aplikaci Cordova. Pokud se v emulátoru zobrazí dialogové okno zabezpečení požadující přístup k síti, potvrďte ho.   

7. Po spuštění aplikace v zařízení nebo emulátoru zadejte smysluplný text do pole **Zadejte nový text**, například _Dokončení kurzu_ a pak klikněte na tlačítko **Přidat**.  
Tím odešlete do prostředí back-end v Azure, které jste předtím vytvořili, požadavek POST. Prostředí backend vloží data z požadavku do tabulky TodoItem v databázi SQL Database a vrátí informace o nově uložených položkách do mobilní aplikace. Mobilní aplikace zobrazí tato data v seznamu.

    ![](./media/app-service-mobile-cordova-quickstart/quickstart-startup.png)
    
8. Opakujte předchozí tři kroky pro každou platformu zařízení, kterou plánujete podporovat.

[Azure Portal]: https://portal.azure.com/



<!--HONumber=sep16_HO1-->


