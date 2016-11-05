> [!IMPORTANT]
> Pokud chcete přijímat nabízená oznámení ze služby Mobile Engagement, je potřeba povolit ve vaší aplikaci funkci `Silent Remote Notifications`. Musíte přidat hodnotu vzdáleného oznámení do pole UIBackgroundModes v souboru Info.plist.
> 
> 

1. Otevřete v projektu soubor `info.plist`.
2. Klikněte pravým tlačítkem myši na horní položku v seznamu (`Information Property List`) a přidejte nový řádek.
   
    ![](./media/mobile-engagement-ios-silent-push/xcode-plist-add-silent-push1.png)
3. Na novém řádku zadejte `Required background modes`
   
    ![](./media/mobile-engagement-ios-silent-push/xcode-plist-add-silent-push2.png)
4. Rozbalte řádek kliknutím na šipku vlevo.
5. Přidejte k položce Item 0 tuto hodnotu: `App downloads content in response to push notifications`
   
    ![](./media/mobile-engagement-ios-silent-push/xcode-plist-add-silent-push3.png)
6. Po provedení změny by měl kód XML souboru info.plist obsahovat následující klíč a hodnotu:
   
        <key>UIBackgroundModes</key>
        <array>
        <string>remote-notification</string>
        </array>
7. Pokud používáte **Xcode 7+** a **iOS 9+**:
   
   * Povolte možnost **Push Notifications** (Nabízená oznámení) v části Targets (Cíle) > Your Target Name (Název vašeho cíle) > Capabilities (Schopnosti).

<!--HONumber=Jun16_HO2-->


