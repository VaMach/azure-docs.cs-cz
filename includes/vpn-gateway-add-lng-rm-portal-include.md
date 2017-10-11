1. Na portálu v části **Všechny prostředky** klikněte na **+Přidat**. V **všechno, co** okno vyhledávacího pole, typ **brány místní sítě**, pak klikněte na vrácení seznamu prostředků. Kliknutím n **Brána místní sítě** otevřete okno, pak kliknutím na **Vytvořit** otevřete okno **Vytvořit bránu místní sítě**.
   
    ![Vytvoření brány místní sítě](./media/vpn-gateway-add-lng-rm-portal-include/lng.png)

2. V okně **Vytvoření brány místní sítě** zadejte **Název** objektu brány místní sítě. Pokud je to možné, používejte něco intuitivní, například **ClassicVNetLocal** nebo **TestVNet1Local**. Díky tomu je snazší identifikaci bránu místní sítě na portálu.
3. Zadejte platné veřejné **IP adresu** pro zařízení VPN nebo brány virtuální sítě, ke kterému se chcete připojit.<br>**Pokud tento místní síť představuje místní umístění:** zadat adresu veřejnou IP adresu, kterou chcete připojit k zařízení VPN. IP adresa nemůže být za serverem NAT a musí být dostupná pro Azure.<br>**Pokud tato místní síť představuje jiné virtuální síti:** zadejte veřejnou IP adresu, která byla přiřazena bránu virtuální sítě pro tuto virtuální síť.<br>**Pokud ještě nemáte IP adresu:** tvoří platný zástupný IP adresa a pak se vrátit a změnit toto nastavení před připojením.
4. **Adresní prostor** odkazuje na rozsahy adres sítě, kterou tato místní síť představuje. Můžete přidat více různých rozsahů adres. Ujistěte se, že rozsahy, který zde určíte nepřekrývají s rozsahy jiné sítě, ke které se připojujete.
5. V rozevíracím seznamu **Předplatné** zkontrolujte, jestli se zobrazuje správné předplatné.
6. V rozevíracím seznamu **Skupina prostředků** vyberte skupinu prostředků, kterou chcete použít. Můžete vytvořit novou skupinu prostředků, nebo vybrat skupinu prostředků, kterou jste už vytvořili.
7. Pro **umístění**, vyberte umístění, ve kterém se vytvoří tento prostředek. Můžete vybrat stejné umístění, ve kterém se nachází vaše virtuální síť, ale není to povinné.
8. Kliknutím na **Vytvořit** vytvořte bránu místní sítě.

