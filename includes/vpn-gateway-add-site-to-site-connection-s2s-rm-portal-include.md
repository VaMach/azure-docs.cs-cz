1. Vyhledejte bránu virtuální sítě.
2. Klikněte na **Připojení**. V horní části okna Připojení klikněte na **+Přidat** a otevřete okno **Přidat připojení**.
   
    ![Vytvoření připojení typu site-to-site](./media/vpn-gateway-add-site-to-site-connection-s2s-rm-portal-include/connection.png)
3. V okně **Přidat připojení** zadejte **Název** připojení. 
4. U položky **Typ připojení**, vyberte **Site-to-site (IPSec)**.
5. Nastavení **Brána virtuální sítě** má pevnou hodnotu, protože právě z této brány se připojujete.
6. U položky **Brána místní sítě** klikněte na **Zvolit bránu místní sítě** a vyberte bránu místní sítě, kterou chcete použít. 
7. Hodnota položky **Sdílený klíč** musí odpovídat hodnotě, kterou používáte pro místní zařízení VPN. V příkladu jsme použili „abc123“, ale můžete (a měli byste) používat něco složitějšího. Důležité je, aby hodnota, kterou zde zadáte, byla stejná jako hodnota, kterou jste zadali při konfiguraci zařízení VPN.
8. Hodnoty položek **Předplatné**, **Skupina prostředků** a **Umístění** jsou pevné.
9. Vytvořte připojení kliknutím na **OK**. Na obrazovce bude blikat text *Vytváří se připojení*.
10. Po vytvoření se připojení zobrazí v okně **Připojení** pro vaši bránu virtuální sítě.
    
    ![Vytvoření připojení typu site-to-site](./media/vpn-gateway-add-site-to-site-connection-s2s-rm-portal-include/connectionstatus450.png)

