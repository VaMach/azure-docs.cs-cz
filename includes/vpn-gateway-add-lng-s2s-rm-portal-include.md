1. Na portálu v části **Všechny prostředky** klikněte na **+Přidat**. Do vyhledávacího pole okna **Všechno** zadejte **Brána místní sítě** a pak kliknutím vyhledejte. Zobrazí se seznam. Kliknutím n **Brána místní sítě** otevřete okno, pak kliknutím na **Vytvořit** otevřete okno **Vytvořit bránu místní sítě**.

      ![Vytvoření brány místní sítě](./media/vpn-gateway-add-lng-s2s-rm-portal-include/createlng.png)
2. V okně **Vytvoření brány místní sítě** zadejte **Název** objektu brány místní sítě.
3. Zadejte platnou veřejnou **IP adresu** pro zařízení VPN nebo bránu virtuální sítě, ke které se chcete připojit.<br>Jedná se o veřejnou IP adresu zařízení VPN, ke kterému se chcete připojit. IP adresa nemůže být za serverem NAT a musí být dostupná pro Azure. *Použijte vlastní hodnoty, ne hodnoty zobrazené na snímku obrazovky.*
4. **Adresní prostor** odkazuje na rozsahy adres sítě, kterou tato místní síť představuje. Můžete přidat více různých rozsahů adres. Zkontrolujte, že se zadané rozsahy nepřekrývají s rozsahy jiných sítí, ke kterým se budete chtít připojit. Azure bude směrovat zadaný rozsah adres na místní IP adresu zařízení VPN. *Použijte zde vlastní hodnoty, ne hodnoty zobrazené na snímku obrazovky.*
5. V rozevíracím seznamu **Předplatné** zkontrolujte, jestli se zobrazuje správné předplatné.
6. V rozevíracím seznamu **Skupina prostředků** vyberte skupinu prostředků, kterou chcete použít. Můžete vytvořit novou skupinu prostředků, nebo vybrat skupinu prostředků, kterou jste už vytvořili.
7. V případě **Umístění** vyberte umístění, ve kterém bude tento objekt vytvořen. Můžete vybrat stejné umístění, ve kterém se nachází vaše virtuální síť, ale není to povinné.
8. Kliknutím na **Vytvořit** vytvořte bránu místní sítě.