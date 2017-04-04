1. Na levé straně stránky portálu klikněte na **+** a do vyhledávacího pole zadejte text „Brána virtuální sítě“. V části **Výsledky** vyhledejte položku **Brána virtuální sítě** a klikněte na ni. V dolní části okna **Brána virtuální sítě** klikněte na **Vytvořit**. Tím otevřete okno **Vytvořit bránu virtuální sítě**.
2. V okně **Vytvořit bránu virtuální sítě** zadejte hodnoty pro příslušnou bránu virtuální sítě.

    ![Vytvoření polí okna brány virtuální sítě](./media/vpn-gateway-add-gw-s2s-rm-portal-include/newgw.png "Nová brána")
3. **Název**: Zadejte pro bránu název. Toto není název podsítě brány. Jedná se o název objektu brány, který vytváříte.
4. **Typ brány**: Vyberte **VPN**. Brány VPN používají bránu virtuální sítě typu **VPN**. 
5. **Typ VPN**: Vyberte typ VPN určený pro vaši konfiguraci. Většina konfigurací vyžaduje trasový typ VPN.
6. **SKU**: Z rozevíracího seznamu vyberte SKU brány. SKU uvedené v rozevíracím seznamu závisí na typu VPN, který jste vybrali.
7. **Umístění:** K zobrazení umístění může být se posunout. Upravte pole **Umístění**, aby odkazovalo na umístění, kde se nachází vaše virtuální síť. Pokud umístění neukazuje na oblast, ve které je vaše virtuální síť umístěná, tato síť se nezobrazí v dalším kroku v rozevírací nabídce Zvolit virtuální síť.
8. **Virtuální síť:** Vyberte virtuální síť, do které chcete tuto bránu přidat. Klikněte na **Virtuální síť** a otevře se okno **Zvolit virtuální síť**. Vyberte virtuální síť. Pokud svou virtuální síť nevidíte, zkontrolujte, že pole **Umístění** odkazuje na oblast, ve které se nachází vaše virtuální síť.
9. **Veřejná IP adresa:** Prostřednictvím tohoto okna můžete vytvořit objekt veřejné IP adresy, ke kterému bude dynamicky přiřazena veřejná IP adresa. Kliknutím na **Veřejná IP adresa** otevřete okno **Zvolte veřejnou IP adresu**. Kliknutím na **+Vytvořit novou** otevřete okno **Vytvořit veřejnou IP adresu**. Zadejte název veřejné IP adresy. Kliknutím na tlačítko **OK** uložte změny v tomto okně.

    ![Vytvoření veřejné IP adresy](./media/vpn-gateway-add-gw-s2s-rm-portal-include/createpip.png "Vytvořit PIP")
10. **Předplatné**: Zkontrolujte, jestli je vybráno správné předplatné.
11. **Skupina prostředků**: Toto nastavení je určeno vámi vybranou virtuální sítí. 
12. Po zadání předchozích nastavení už hodnotu **Umístění** neupravujte.
13. Ověřte nastavení. Pokud chcete, aby se brána zobrazovala na řídicím panelu, můžete v dolní části okna vybrat **Připnout na řídicí panel**.
14. Kliknutím na **Vytvořit** zahajte proces vytváření brány. Nastavení bude ověřeno a na řídicím panelu se zobrazí dlaždice Nasazování brány virtuální sítě. Vytváření brány může trvat až 45 minut. K zobrazení stavu dokončení může být nutné obnovit stránku portálu.
    
    ![Vytvoření brány](./media/vpn-gateway-add-gw-s2s-rm-portal-include/creategw.png "Vytvořit bránu")
15. Po vytvoření brány můžete zobrazením virtuální sítě na portálu zobrazit IP adresu, která jí byla přiřazena. Brána se zobrazí jako připojené zařízení. Kliknutím na připojené zařízení (bránu virtuální sítě) můžete zobrazit další informace.