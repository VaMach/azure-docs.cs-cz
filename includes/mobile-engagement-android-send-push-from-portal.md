### Udělení přístupu aplikaci Mobile Engagement k vašemu klíči rozhraní API GCM
Pokud chcete aplikaci Mobile Engagement povolit, aby vaším jménem odesílala nabízená oznámení, musíte jí udělit přístup k vašemu klíči rozhraní API. To provedete tak, že svůj klíč nakonfigurujete a zadáte na portál Mobile Engagement.

1. Na portálu Azure Classic zkontrolujte, jestli pracujete v aplikaci určené pro tento projekt, a potom klikněte na tlačítko **Engage** (Zpřístupnit), které najdete dole:
   
    ![](./media/mobile-engagement-android-send-push/engage-button.png)
2. Potom klikněte na **Settings**(Nastavení)  -> **Native Push** (Nativní oznámení) a zadejte klíč GCM:
   
    ![](./media/mobile-engagement-android-send-push/engagement-portal.png)
3. Klikněte na ikonu **Upravit** před možností **Klíč rozhraní API** v části **GCM Settings** (Nastavení GCM), jak vidíte níže:
   
    ![](./media/mobile-engagement-android-send-push/native-push-settings.png)
4. V místní nabídce vložte klíč serveru GCM, který jste předtím získali, a klikněte na **Ok**.
   
    ![](./media/mobile-engagement-android-send-push/api-key.png)

## <a id="send"></a>Odeslání oznámení do vaší aplikace
Teď vytvoříme jednoduchou kampaň nabízených oznámení. Ta bude odesílat oznámení do vaší aplikace.

1. Na portálu Mobile Engagement přejděte na kartu **REACH**.
2. Kliknutím na **Nové oznámení** vytvořte kampaň nabízených oznámení.
   
    ![](./media/mobile-engagement-android-send-push/new-announcement.png)
3. Nastavte první pole kampaně pomocí následujících kroků:
   
    ![](./media/mobile-engagement-android-send-push/campaign-first-params.png)
   
    a. Zadejte název kampaně.
   
    b. U položky **Typ doručení** vyberte *Systémové oznámení -> Jednoduché*: Toto typ jednoduchého nabízeného oznámení pro Android, které obsahuje název a řádek menšího textu.
   
    c. U položky **Delivery time** (Čas doručení) vyberte *Any time* (Kdykoli), aby mohla aplikace přijmout oznámení bez ohledu na to, jestli je spuštěná, nebo ne.
   
    d. Do textu oznámení zadejte **Název**, který se v nabízeném oznámení zobrazí tučně.
   
    e. Do pole **Zpráva** zadejte zprávu.
4. Přejděte dolů a v části **Obsah** vyberte **Pouze oznámení**.
   
    ![](./media/mobile-engagement-android-send-push/campaign-content.png)
5. Tím jste dokončili nastavení nejzákladnější možné kampaně. Nyní znovu přejděte dolů a kliknutím na tlačítko **Vytvořit** kampaň uložte.
6. Poslední krok: Kliknutím na **Aktivovat** aktivujte svoji kampaň, která bude zasílat nabízená oznámení.
   
    ![](./media/mobile-engagement-android-send-push/campaign-activate.png)

<!--HONumber=Sep16_HO3-->


