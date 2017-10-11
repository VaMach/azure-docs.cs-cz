#### <a name="configure-the-ios-project-in-xamarin-studio"></a>Konfigurace projektu pro iOS v Xamarin studiu
1. V Xamarin.Studio, otevřete **Info.plist**a aktualizovat **identifikátor svazku** s ID sady, který jste dříve vytvořili pomocí ID aplikace.

    ![](./media/app-service-mobile-xamarin-ios-configure-project/mobile-services-ios-push-21.png)
2. Přejděte dolů k položce **režimy pozadí**. Vyberte **povolit režimy pozadí** pole a **vzdáleného oznámení** pole.

    ![](./media/app-service-mobile-xamarin-ios-configure-project/mobile-services-ios-push-22.png)
3. Dvakrát klikněte na projekt v panelu řešení otevřete **možnosti projektu**.
4. V části **sestavení**, zvolte **iOS podepisování sady**a vyberte odpovídající identitě a zřizování profilu jste právě nastavili nahoru pro tento projekt.

   ![](./media/app-service-mobile-xamarin-ios-configure-project/mobile-services-ios-push-20.png)

   Tím se zajistí, že projektu používá nový profil pro podepisování kódu. Oficiální Xamarin zařízení zřizování dokumentaci, najdete v části [zřizování zařízení Xamarin].

#### <a name="configure-the-ios-project-in-visual-studio"></a>Konfigurace projektu pro iOS v sadě Visual Studio
1. V sadě Visual Studio, klikněte pravým tlačítkem na projekt a pak klikněte na tlačítko **vlastnosti**.
2. Na stránkách vlastností klikněte na tlačítko **iOS aplikace** kartě a aktualizovat **identifikátor** s ID, který jste vytvořili dříve.

    ![](./media/app-service-mobile-xamarin-ios-configure-project/mobile-services-ios-push-23.png)
3. V **iOS podepisování sady** , vyberte odpovídající identitě a zřizování profilu stačí nastavit pro tento projekt.

    ![](./media/app-service-mobile-xamarin-ios-configure-project/mobile-services-ios-push-24.png)

    Tím se zajistí, že projektu používá nový profil pro podepisování kódu. Oficiální Xamarin zařízení zřizování dokumentaci, najdete v části [zřizování zařízení Xamarin].
4. Klikněte dvakrát na Info.plist otevřete ho a pak povolte **RemoteNotifications** pod **režimy pozadí**.

[zřizování zařízení Xamarin]: http://developer.xamarin.com/guides/ios/getting_started/installation/device_provisioning/
