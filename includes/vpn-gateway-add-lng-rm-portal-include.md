1. Na portálu klikněte na **Nový** > **Sítě** > **Brána místní sítě**.

    ![Vytvoření brány místní sítě](./media/vpn-gateway-add-lng-rm-portal-include/addlng250.png)

2. V okně **Vytvoření brány místní sítě** zadejte **Název** objektu brány místní sítě.
 
3. Zadejte platnou veřejnou **IP adresu** pro zařízení VPN nebo bránu virtuální sítě, ke které se chcete připojit.<br>Pokud tato místní síť představuje místní umístění, toto je veřejná IP adresa zařízení VPN, ke kterému se chcete připojit. IP adresa nemůže být za serverem NAT a musí být dostupná pro Azure.<br>Pokud tato místní síť představuje jinou virtuální síť, zadejte veřejnou IP adresu, která byla přiřazená k bráně virtuální sítě pro takovou virtuální síť.<br>

4. **Adresní prostor** odkazuje na rozsahy adres sítě, kterou tato místní síť představuje. Můžete přidat více různých rozsahů adres. Zkontrolujte, že se zadané rozsahy nepřekrývají s rozsahy jiných sítí, ke kterým se budete chtít připojit.
 
5. V rozevíracím seznamu **Předplatné** zkontrolujte, jestli se zobrazuje správné předplatné.

6. V rozevíracím seznamu **Skupina prostředků** vyberte skupinu prostředků, kterou chcete použít. Můžete vytvořit novou skupinu prostředků, nebo vybrat skupinu prostředků, kterou jste už vytvořili.

7. V případě **Umístění** vyberte umístění, ve kterém bude tento objekt vytvořen. Můžete vybrat stejné umístění, ve kterém se nachází vaše virtuální síť, ale není to povinné.

8. Kliknutím na **Vytvořit** vytvořte bránu místní sítě.


<!--HONumber=Sep16_HO3-->


