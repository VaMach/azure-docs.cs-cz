

Pokud chcete zaregistrovat aplikaci pro nabízená oznámení prostřednictvím služby Apple Push Notification Service (APNs), je třeba pro projekt vytvořit nový certifikát push, ID aplikace a zřizovací profil na portálu pro vývojáře Apple Developer. ID aplikace bude obsahovat nastavení konfigurace, které umožní vaší aplikaci odesílat a přijímat nabízená oznámení. Toto nastavení bude obsahovat certifikát nabízených oznámení potřebný k ověření ve službě Apple Push Notification Service (APNs) při odesílání a přijímání nabízených oznámení. Další informace o těchto konceptech najdete v oficiální dokumentaci ke službě [Apple Push Notification Service](http://go.microsoft.com/fwlink/p/?LinkId=272584).

#### <a name="generate-the-certificate-signing-request-file-for-the-push-certificate"></a>Generování souboru s žádostí o podepsání certifikátu pro certifikát push
Tyto kroky vás provedou vytvořením žádosti o podepsání certifikátu. Ta bude sloužit k vygenerování certifikátu push pro použití s APNs.

1. V Macu spusťte nástroj Keychain Access. Můžete ho otevřít na Launchpadu ve složce **Nástroje** nebo **Jiné**.
2. Klikněte na **Keychain Access**, rozbalte **Průvodce certifikací**, klikněte na **Vyžádat certifikát od certifikační autority...**.
   
      ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-request-cert-from-ca.png)
3. Vyberte svoji **uživatelskou e-mailovou adresu** a **běžný název**, zkontrolujte, jestli je vybraná možnost **Uloženo na disk** a potom klikněte na **Pokračovat**. Pole **E-mailová adresa CA** není povinné, takže ho můžete nechat prázdné.
   
      ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-csr-info.png)
4. Do pole **Uložit jako** zadejte název souboru CSR, pomocí možnosti **Kam** vyberte umístění a potom klikněte na **Uložit**.
   
      ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-save-csr.png)
   
      Tím soubor CSR uložíte do vybraného umístění. Výchozím umístěním je plocha. Zapamatujte si umístění tohoto souboru.

#### <a name="register-your-app-for-push-notifications"></a>Registrace aplikace pro nabízená oznámení
Vytvořte pro svoji aplikaci nové explicitní ID aplikace u Applu a nakonfigurujte ji pro nabízená oznámení.  

1. Přejděte na stránky [iOS Provisioning Portal](http://go.microsoft.com/fwlink/p/?LinkId=272456) na webu Apple Developer Center, přihlaste se pomocí Apple ID, klikněte na **Identifiers** (Identifikátory), potom na **App IDs** (ID aplikací) a nakonec klikněte na znak **+** a zaregistrujte novou aplikaci.
   
       ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-ios-appids.png)
2. Aktualizujte následující tři pole týkající se nové aplikace a potom klikněte na **Continue** (Pokračovat):
   
   * **Name** (Název): V části **App ID Description** (Popis ID aplikace) zadejte do pole **Name** (Název) popisný název aplikace.
   * **Bundle Identifier** (Identifikátor svazku): V části **Explicit App ID** (Explicitní ID aplikace) zadejte **identifikátor svazku** ve formě `<Organization Identifier>.<Product Name>`, jak je uvedeno v [Průvodci distribucí aplikace](https://developer.apple.com/library/mac/documentation/IDEs/Conceptual/AppDistributionGuide/ConfiguringYourApp/ConfiguringYourApp.html#//apple_ref/doc/uid/TP40012582-CH28-SW8). Musí se shodovat s tím, který používáte v projektu XCode, Xamarin nebo Cordova pro svou aplikaci.
   * **Push Notifications** (Nabízená oznámení): V části **App Services** (Služby aplikací) zaškrtněte možnost **Push Notifications** (Nabízená oznámení).
     
     ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-new-appid-info.png)
3. Na obrazovce Confirm your App ID (Potvrzení ID aplikace) zkontrolujte nastavení a až tak učiníte, klikněte na **Submit** (Odeslat).
4. Po odeslání nového ID aplikace se zobrazí obrazovka **Registration complete** (Registrace je dokončena). Klikněte na **Done** (Hotovo).
5. V centru pro vývojáře v části App IDs (ID aplikací) najděte právě vytvořené ID aplikace a klikněte na jeho řádek. Kliknutím na řádek s ID aplikace zobrazíte podrobnosti o aplikaci. V dolní části klikněte na tlačítko **Edit** (Upravit).
6. Přejděte do dolní části obrazovky a klikněte v části **Development Push SSL Certificate** (Vývojový certifikát pro nabízená oznámení SSL) na tlačítko **Create Certificate...** (Vytvořit certifikát...).
   
       ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-appid-create-cert.png)
   
       This will display the "Add iOS Certificate" assistant.
   
   > [!NOTE]
   > Tento kurz používá vývojový certifikát. Stejný postup se používá při registraci produkčního certifikátu. Dejte pozor, abyste při odesílání oznámení používali stejný typ certifikátu.
   > 
   > 
7. Klikněte na **Choose File** (Vybrat soubor) a přejděte do umístění, kam jste uložili žádost o podepsání certifikátu pro váš certifikát push. Pak klikněte na **Generate** (Vygenerovat).
   
      ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-appid-cert-choose-csr.png)
8. Až portál vytvoří certifikát, klikněte na tlačítko **Download** (Stáhnout).
   
      ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-appid-download-cert.png)
   
       This downloads the signing certificate and saves it to your computer in your Downloads folder.
   
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-cert-downloaded.png)
   
   > [!NOTE]
   > Ve výchozím nastavení má stažený soubor vývojářského certifikátu název **aps_development.cer**.
   > 
   > 
9. Poklikejte na stažený nabízený certifikát **aps_development.cer**. Tím nový certifikát nainstalujete do Klíčenky, jak je znázorněno níže:
   
       ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-cert-in-keychain.png)
   
   > [!NOTE]
   > Název ve vašem certifikátu se může lišit, ale bude mu předcházet text **Apple Development iOS Push Services:**.
   > 
   > 
10. V nástroji Keychain Access v kategorii **Certificates** (Certifikáty) klikněte se stisknutou klávesou Control na nový certifikát push, který jste právě vytvořili. Klikněte na **Exportovat**, zadejte název souboru, vyberte formát **.p12** a potom klikněte na **Uložit**.
    
    Zapamatujte si název souboru a umístění exportovaného certifikátu push .p12. Po nahrání na portál Azure Classic bude sloužit k povolení ověřování v APNs.

#### <a name="create-a-provisioning-profile-for-the-app"></a>Vytvoření zřizovacího profilu pro aplikaci
1. Na stránkách <a href="http://go.microsoft.com/fwlink/p/?LinkId=272456" target="_blank">iOS Provisioning Portal</a> vyberte **Provisioning Profiles** (Zřizovací profily), potom **All** (Všechny) a nakonec kliknutím na tlačítko **+** vytvořte nový profil. Tím spustíte průvodce **Add iOS Provisiong Profile** (Přidání zřizovacího profilu iOS).
   
       ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-new-provisioning-profile.png)
2. V části **Development** (Vývoj) vyberte jako typ zřizovacího profilu **iOS App Development** (Vývoj aplikací pro iOS) a klikněte na **Continue** (Pokračovat).
3. Potom v rozevíracím seznamu **App ID** (ID aplikace) vyberte právě vytvořené ID aplikace a klikněte na **Continue** (Pokračovat).
   
       ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-select-appid-for-provisioning.png)
4. Na obrazovce **Select certificates** (Výběr certifikátů) vyberte svůj vývojářský certifikát, kterým podepisujete kód, a klikněte na **Continue** (Pokračovat). Jedná se o podpisový certifikát, ne právě vytvořený certifikát push.
   
       ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-provisioning-select-cert.png)
5. Potom vyberte **zařízení**, která chcete použít pro testování, a klikněte na **Continue** (Pokračovat).
   
       ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-provisioning-select-devices.png)
6. Nakonec do pole **Název profilu** zadejte název profilu a klikněte na **Generovat**.
   
       ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-provisioning-name-profile.png)



<!--HONumber=Feb17_HO1-->


