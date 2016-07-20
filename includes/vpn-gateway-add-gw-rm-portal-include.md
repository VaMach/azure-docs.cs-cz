1. Na portálu klikněte na **Nový** a potom na **Sítě**. V seznamu vyberte **Brána virtuální sítě**.

    ![Brána](./media/vpn-gateway-add-gw-rm-portal-include/creategw250.png)

2. V okně **Vytvořit bránu virtuální sítě** do pole **Název** zadejte název brány. Toto není název podsítě brány. Jde o název objektu brány.
 
3. Potom vyberte virtuální síť, ve které chcete bránu nasadit. Kliknutím na šipku otevřete okno **Výběr virtuální sítě** a potom klikněte na síť VNet. Aby se síť VNet zobrazila v seznamu, musí už mít platnou podsíť brány.

4. Zvolte veřejnou IP adresu. Kliknutím na šipku otevřete okno **Výběr veřejné IP adresy**. Potom kliknutím na **Vytvořit nový** otevřete okno **Vytvoření veřejné IP adresy**. Zadejte název veřejné IP adresy. Všimněte si, že IP adresa se nezadává. IP adresa se přiřadí dynamicky. Jde o název objektu IP adresy, ke kterému se adresa přiřadí. Kliknutím na tlačítko **OK** uložte změny.

5. Jako **Typ brány** vyberte typ brány určený pro vaši konfiguraci.

6. Jako **Typ VPN** vyberte typ VPN určený pro vaši konfiguraci.

7. V seznamu **Předplatné** zkontrolujte, jestli je vybrané správné předplatné.

8. Skupina prostředků v části **Skupina prostředků** je určena vybranou virtuální sítí.

9. V seznamu **Umístění** zkontrolujte, jestli je vybrané umístění, ve kterém je vaše skupina prostředků i síť VNet.

10. Pokud chcete, aby se brána zobrazovala na řídicím panelu, můžete vybrat **Připnout na řídicí panel**. Kliknutím na **Vytvořit** zahajte proces vytváření brány. Na řídicím panelu se zobrazí dlaždice nasazování brány virtuální sítě. Vytváření brány může trvat až 45 minut. V pozadí probíhá množství různých procesů. K zobrazení stavu dokončení může být nutné obnovit stránku portálu.

    
    ![Brána](./media/vpn-gateway-add-gw-rm-portal-include/deployvnetgw150.png)

11. Po vytvoření brány můžete zobrazením virtuální sítě na portálu zobrazit IP adresu, která jí byla přiřazena. Brána se zobrazí jako připojené zařízení. Kliknutím na připojené zařízení (bránu virtuální sítě) můžete zobrazit další informace.






<!--HONumber=Jun16_HO2-->


