1. Kliknutím na příslušnou ikonu na panelu nástrojů v Android Studiu nebo kliknutím na možnosti **Tools**(Nástroje)  -> **Android** -> **SDK Manager** v nabídce otevřete Android SDK Manager. Vyhledejte cílovou verzi sady SDK pro Android, kterou používá váš projekt, otevřete ji kliknutím na **Show Package Details** (Zobrazit podrobnosti balíčku) a vyberte možnost **Google APIs** (Rozhraní API Google), pokud ještě není nainstalovaná.
2. Klikněte na kartu **SDK Tools** (Nástroje SDK). Pokud jste ještě nenainstalovali služby Google Play, klikněte na položku **Google Play Services**, jak vidíte níže. Potom spusťte instalaci kliknutím na **Apply** (Použít). 
   
    Poznamenejte si cestu k sadě SDK, abyste ji mohli použít později. 
   
       ![](./media/notification-hubs-android-studio-add-google-play-services/notification-hubs-android-studio-sdk-manager.png)
3. Otevřete soubor **build.gradle** v adresáři aplikace.
   
    ![](./media/notification-hubs-android-studio-add-google-play-services/notification-hubs-android-studio-add-google-play-dependency.png)
4. Do části *dependencies* přidejte tento řádek: 
   
           compile 'com.google.android.gms:play-services-gcm:9.2.0'
5. Klikněte na ikonu **Sync Project with Gradle Files** (Synchronizovat projekt se soubory Gradle) na panelu nástrojů.
6. Otevřete soubor **AndroidManifest.xml** a ke značce *application* přidejte následující značku.
   
        <meta-data android:name="com.google.android.gms.version"
            android:value="@integer/google_play_services_version" />



<!--HONumber=Nov16_HO2-->


