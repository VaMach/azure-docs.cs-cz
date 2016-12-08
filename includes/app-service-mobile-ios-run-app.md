
1. Na počítači Mac navštivte [Azure Portal]. Klikněte na **Procházet vše** > **Mobilní aplikace** > prostředí back-end, které jste právě vytvořili. V nastavení mobilní aplikace klikněte na **Rychlý start** > **iOS (Objective-C)**. Pokud dáváte přednost jazyku Swift, klikněte místo toho na **Rychlý start** > **iOS (Swift)**. V části **Download and run your iOS project** (Stažení a spuštění projektu iOS) klikněte na **Stáhnout**. Tím stáhnete dokončený projekt Xcode pro aplikaci přednastavenou k připojení k vašemu prostředí back-end. Otevřete projekt pomocí Xcode.
2. Stisknutím tlačítka **Spustit** sestavíte projekt a spustíte aplikaci v simulátoru iOS.
3. V aplikaci zadejte smysluplný text, například *Dokončit kurz* a pak klikněte na ikonu plus (**+**). Tím odešlete do prostředí back-end v Azure, které jste předtím vytvořili, požadavek POST. Prostředí back-end vloží data z požadavku do tabulky SQL TodoItem a vrátí informace o nově uložených položkách do mobilní aplikace. Mobilní aplikace zobrazí tato data v seznamu. 

   ![Aplikace Rychlý start pro iOS](./media/app-service-mobile-ios-quickstart/mobile-quickstart-startup-ios.png)

[Azure Portal]: https://portal.azure.com/


<!--HONumber=Nov16_HO3-->


