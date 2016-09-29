1. Na portálu přejděte na **Nová** > **Sítě** > **Brána virtuální sítě**. Tím otevřete okno **Vytvoření brány virtuální sítě**.

    ![brána](./media/vpn-gateway-add-gw-rm-portal-include/creategw250.png)

2. V okně **Vytvořit bránu virtuální sítě** zadejte název brány do pole **Název**. Toto není název podsítě brány. Jedná se o název objektu brány, který vytvoříte.

3. Upravte pole **Umístění**, aby odkazovalo na umístění, kde se nachází vaše virtuální síť. Pokud to neprovedete, seznam virtuálních sítí nezobrazí vaši virtuální síť.
 
4. Potom vyberte virtuální síť, do které chcete tuto bránu přidat. Klikněte na **Virtuální síť** a otevře se okno **Zvolit virtuální síť**. Vyberte virtuální síť. Aby se virtuální síť zobrazila v seznamu, musí už mít platnou podsíť brány.

5. Zvolte veřejnou IP adresu. Kliknutím na **Veřejná IP adresa** otevřete okno **Zvolte veřejnou IP adresu**. Kliknutím na **+Vytvořit novou** otevřete okno **Vytvořit veřejnou IP adresu**. Zadejte název veřejné IP adresy. Tím vytvoříte objekt veřejné IP adresy, ke kterému bude dynamicky přiřazena veřejná IP adresa. <br>Klikněte na tlačítko **OK** a uložte změny.

5. Jako **Typ brány** vyberte typ brány určený pro vaši konfiguraci.

6. Jako **Typ VPN** vyberte typ VPN určený pro vaši konfiguraci.

7. V seznamu **Předplatné** zkontrolujte, jestli je vybrané správné předplatné.

8. **Skupina prostředků** je určená vámi vybranou virtuální sítí. 

9. Po zadání výše uvedených nastavení už neupravujte **Umístění**. 

10. V tomto okamžiku bude okno vypadat podobně jako obrázek v kroku 1. Zkontrolujte, jestli nastavení odpovídají nastavením pro vaši vlastní konfiguraci. Pokud chcete, aby se brána zobrazovala na řídicím panelu, můžete v dolní části okna vybrat **Připnout na řídicí panel**.

11. Kliknutím na **Vytvořit** zahajte proces vytváření brány. Nastavení bude ověřeno a na řídicím panelu se zobrazí dlaždice Nasazování brány virtuální sítě. Vytváření brány může trvat až 45 minut. K zobrazení stavu dokončení může být nutné obnovit stránku portálu.

    ![brána](./media/vpn-gateway-add-gw-rm-portal-include/deployvnetgw150.png)

11. Po vytvoření brány můžete zobrazením virtuální sítě na portálu zobrazit IP adresu, která jí byla přiřazena. Brána se zobrazí jako připojené zařízení. Kliknutím na připojené zařízení (bránu virtuální sítě) můžete zobrazit další informace.





<!--HONumber=Sep16_HO3-->


