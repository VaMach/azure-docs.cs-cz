
1. Navštivte [Portál Azure]. Klikněte na **Procházet vše** > **Mobilní aplikace** > prostředí back-end, které jste právě vytvořili. V nastavení mobilní aplikace klikněte na **Rychlý start** > **Cordova**. V části **Configure your client application** (Konfigurace klientské aplikace) vyberte možnost **Vytvořit novou aplikaci** a klikněte na **Stáhnout**. Tím stáhnete dokončený projekt Cordova pro aplikaci přednastavenou k připojení k vašemu prostředí back-end.

2. Rozbalte stažený soubor ZIP do adresáře na pevném disku.

3. Otevřete projekt pomocí sady **Visual Studio**.  Klikněte na **Otevřít** > **Projekt nebo řešení**.

4. Najděte soubor _název_webu_.sln a klikněte na **Otevřít**.

5. Výchozí emulátor je **Ripple – Nexus (Galaxy)**.  Klikněte na šipku rozevíracího seznamu vedle emulátoru a vyberte **Emulátor Google Android**.

6. Klikněte na **Emulátor Google Android**.  Projekt se sestaví a potom spustí.  Emulátor Google Android může zobrazit upozornění na zabezpečení sítě s požadavkem na přístup k síti.  Nakonec se zobrazí emulátor Google Android a spustí se vaše aplikace.

7. V aplikaci zadejte smysluplný text, třeba _Dokončit kurz_, a klikněte na tlačítko Přidat. Tím odešlete do prostředí back-end v Azure, které jste předtím vytvořili, požadavek POST. Prostředí back-end vloží data z požadavku do tabulky SQL TodoItem a vrátí informace o nově uložených položkách do mobilní aplikace. Mobilní aplikace zobrazí tato data v seznamu.

    ![](./media/app-service-mobile-cordova-quickstart/quickstart-startup.png)

[Portál Azure]: https://portal.azure.com/



<!--HONumber=Jun16_HO2-->


