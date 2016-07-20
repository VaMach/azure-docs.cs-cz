###Udělení přístupu službě Mobile Engagement k vašemu certifikátu push

Pokud chcete aplikaci Mobile Engagement povolit, aby vaším jménem odesílala nabízená oznámení, musíte jí udělit přístup k vašemu certifikátu. To provedete tak, že svůj certifikát nakonfigurujete a zadáte na portál Mobile Engagement. Musíte nejdřív získat certifikát .p12 podle pokynů v [dokumentaci od společnosti Apple](https://developer.apple.com/library/prerelease/ios/documentation/IDEs/Conceptual/AppDistributionGuide/AddingCapabilities/AddingCapabilities.html#//apple_ref/doc/uid/TP40012582-CH26-SW6).

1. Přejděte na portál Mobile Engagement. Zkontrolujte, jestli pracujete ve správné aplikaci, a potom klikněte na tlačítko **Engage** (Zpřístupnit), které najdete dole:

    ![](./media/mobile-engagement-ios-send-push/engage-button.png)

2. Klikněte na stránku **Nastavení** na portálu Engagement. Tam klikněte do části **Nativní oznámení**, abyste mohli nahrát svůj certifikát .p12:

    ![](./media/mobile-engagement-ios-send-push/engagement-portal.png)

3. Vyberte svůj soubor .p12, nahrajte ho a zadejte heslo:

    ![](./media/mobile-engagement-ios-send-push/native-push-settings.png)

##<a id="send"></a>Odeslání oznámení do vaší aplikace

Teď vytvoříme jednoduchou kampaň nabízených oznámení. Ta bude odesílat oznámení do vaší aplikace:

1. Na portálu Mobile Engagement přejděte na kartu **Reach**.

2. Kliknutím na **Nové oznámení** vytvořte kampaň nabízených oznámení

    ![](./media/mobile-engagement-ios-send-push/new-announcement.png)

3. Nastavte první pole své kampaně:

    ![](./media/mobile-engagement-ios-send-push/campaign-first-params.png)

    -   Zadejte **Název** kampaně. 
    -   U položky **Delivery time** (Čas doručení) vyberte **Out of app only** (Pouze mimo aplikaci): Jedná se o jednoduchý typ nabízeného oznámení Apple, který obsahuje text.
    -   Do textu oznámení nejdřív zadejte **Název**, který se zobrazí na prvním řádku nabízeného oznámení.
    -   Potom zadejte text do pole **Zpráva**, který se zobrazí na druhém řádku.

4. Přejděte dolů a v části obsahu vyberte **Pouze oznámení**

    ![](./media/mobile-engagement-ios-send-push/campaign-content.png)

5. Tím jste dokončili nastavení nejzákladnější kampaně. Teď přejděte dolů a kliknutím na tlačítko **Vytvořit** kampaň nabízených oznámení uložte. 

6. Nakonec kliknutím na **Aktivovat** odešlete nabízené oznámení. 

    ![](./media/mobile-engagement-ios-send-push/campaign-activate.png)

7. Oznámení budete moct na svém zařízení s iOS přijmout v centru oznámení, jako třeba tohle:

    ![](./media/mobile-engagement-ios-send-push/iphone-notification.png)

8. Pokud máte s tímto zařízením s iOS spárované zařízení Apple Watch, uvidíte oznámení na zařízení Apple Watch:

    ![](./media/mobile-engagement-ios-send-push/apple-watch.png)


 

 


<!--HONumber=Jun16_HO2-->


