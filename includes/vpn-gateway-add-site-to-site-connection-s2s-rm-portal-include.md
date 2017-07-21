1. Přejděte do okna brány virtuální sítě a otevřete ho. K oknu se dostanete několika způsoby. V našem příkladu jsme se k bráně VNet1GW dostali tak, že jsme přešli na **TestVNet1 -> Přehled -> Připojená zařízení -> VNet1GW**.
2. V okně brány VNet1GW klikněte na **Připojení**. V horní části okna Připojení klikněte na **+Přidat** a otevřete okno **Přidat připojení**.

    ![Vytvoření připojení typu site-to-site](./media/vpn-gateway-add-site-to-site-connection-s2s-rm-portal-include/connection1.png)

3. V okně **Přidat připojení** vyplňte hodnoty a vytvořte připojení.

  - **Název**: Zadejte název připojení. V našem příkladu používáme název **VNet1toSite2**.
  - **Typ připojení**: Vyberte **Site-to-site (IPSec)**.
  - **Brána virtuální sítě**: Tato hodnota je pevně daná, protože se připojujete z této brány.
  - **Brána místní sítě**: Klikněte na **Zvolit bránu místní sítě** a vyberte bránu místní sítě, kterou chcete použít. V našem příkladu používáme **Site2**.
  - **Sdílený klíč**: Tato hodnota musí odpovídat hodnotě, kterou používáte pro místní zařízení VPN. V příkladu jsme použili „abc123“, ale můžete (a měli byste) používat něco složitějšího. Důležité je, aby hodnota, kterou zde zadáte, byla stejná jako hodnota, kterou jste zadali při konfiguraci zařízení VPN.
  - Hodnoty položek **Předplatné**, **Skupina prostředků** a **Umístění** jsou pevné.

4. Vytvořte připojení kliknutím na **OK**. Na obrazovce bude blikat text *Vytváří se připojení*.
5. Připojení si můžete zobrazit v okně **Připojení** brány virtuální sítě. Stav se změní z *Neznámý* na *Připojování* a potom na *Úspěch*.