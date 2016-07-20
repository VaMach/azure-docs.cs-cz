1. Vyhledejte bránu virtuální sítě a kliknutím na **Všechna nastavení** otevřete okno **Nastavení**.

2. V okně **Nastavení** klikněte na **Připojení** a potom kliknutím na **Přidat** v horní části okna otevřete okno **Přidat připojení**.

    ![Vytvoření připojení typu site-to-site](./media/vpn-gateway-add-site-to-site-connection-rm-portal-include/addconnection250.png)

3. V okně **Přidat připojení** zadejte **Název** připojení. 

4. U položky **Typ připojení**, vyberte **Site-to-site (IPSec)**.

5. Nastavení **Brána virtuální sítě** má pevnou hodnotu, protože právě z této brány se připojujete.

6. U položky **Brána místní sítě** klikněte na **Zvolit bránu místní sítě** a vyberte bránu místní sítě, kterou chcete použít. 

7. Hodnota položky **Sdílený klíč** musí odpovídat hodnotě, kterou používáte pro místní zařízení VPN. Pokud zařízení VPN v místní síti neposkytuje sdílený klíč, můžete si ho vymyslet a zadat ho tady a na svém místním zařízení. Důležité je, aby se tyto klíče shodovaly.

8. Hodnoty položek **Předplatné**, **Skupina prostředků** a **Umístění** jsou pevné.

9. Vytvořte připojení kliknutím na **OK**. Na obrazovce bude blikat text *Vytváří se připojení*.

10. Po vytvoření se připojení zobrazí v okně **Připojení** pro vaši bránu.

    ![Vytvoření připojení typu site-to-site](./media/vpn-gateway-add-site-to-site-connection-rm-portal-include/connectionstatus450.png)




<!--HONumber=Jun16_HO2-->


