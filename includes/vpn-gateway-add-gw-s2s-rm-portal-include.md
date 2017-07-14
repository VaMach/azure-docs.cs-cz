1. Na levé straně stránky portálu klikněte na **+** a do vyhledávacího pole zadejte text „Brána virtuální sítě“. V části **Výsledky** vyhledejte položku **Brána virtuální sítě** a klikněte na ni.
2. V dolní části okna Brána virtuální sítě klikněte na **Vytvořit**. Tím otevřete okno **Vytvořit bránu virtuální sítě**.

    ![Vytvoření polí okna brány virtuální sítě](./media/vpn-gateway-add-gw-s2s-rm-portal-include/vnet_gw.png "Nová brána")

3. V okně **Vytvořit bránu virtuální sítě** zadejte hodnoty příslušné brány virtuální sítě.

  - **Název**: Zadejte pro bránu název. Toto není název podsítě brány. Jedná se o název objektu brány, který vytváříte.
  - **Typ brány**: Vyberte **VPN**. Brány VPN používají bránu virtuální sítě typu **VPN**. 
  - **Typ VPN**: Vyberte typ VPN určený pro vaši konfiguraci. Většina konfigurací vyžaduje trasový typ VPN.
  - **SKU**: Z rozevíracího seznamu vyberte SKU brány. SKU uvedené v rozevíracím seznamu závisí na typu VPN, který jste vybrali. Další informace o SKU brány najdete v tématu [SKU brány](../articles/vpn-gateway/vpn-gateway-about-vpn-gateway-settings.md#gwsku).
  - **Umístění:** K zobrazení umístění může být se posunout. Upravte pole **Umístění**, aby odkazovalo na umístění, kde se nachází vaše virtuální síť. Pokud umístění neukazuje na oblast, ve které je vaše virtuální síť umístěná, tato síť se nezobrazí v dalším kroku v rozevírací nabídce Zvolit virtuální síť.
  - **Virtuální síť:** Vyberte virtuální síť, do které chcete tuto bránu přidat. Klikněte na **Virtuální síť** a otevřete okno Zvolit virtuální síť. Vyberte virtuální síť. Pokud svou virtuální síť nevidíte, zkontrolujte, že pole Umístění odkazuje na oblast, ve které se nachází vaše virtuální síť.
  - **Veřejná IP adresa**: Okno Vytvoření veřejné IP adresy vytvoří objekt veřejné IP adresy. Veřejná IP adresa se dynamicky přiřadí po vytvoření brány VPN. Služba VPN Gateway aktuálně podporuje pouze *dynamické* přidělení veřejné IP adresy. To ale neznamená, že se IP adresa po přiřazení k vaší bráně VPN bude měnit. Veřejná IP adresa se změní pouze v případě odstranění a nového vytvoření brány. V případě změny velikosti, resetování nebo jiné operace údržby/upgradu vaší brány VPN se nezmění.

    - Nejprve kliknutím na **Veřejná IP adresa** otevřete okno Zvolte veřejnou IP adresu a potom kliknutím na **+Vytvořit novou** otevřete okno Vytvoření veřejné IP adresy.
    - Dále zadejte **Název** veřejné IP adresy a potom kliknutím na **OK** v dolní části okna uložte změny.

      ![Vytvoření veřejné IP adresy](./media/vpn-gateway-add-gw-s2s-rm-portal-include/pip.png "Vytvořit PIP")

4. Ověřte nastavení. Pokud chcete, aby se brána zobrazovala na řídicím panelu, můžete v dolní části okna vybrat **Připnout na řídicí panel**. 
5. Kliknutím na **Vytvořit** zahajte proces vytváření brány VPN. Nastavení bude ověřeno a na řídicím panelu se zobrazí dlaždice Nasazování brány virtuální sítě. Vytváření brány může trvat až 45 minut. K zobrazení stavu dokončení může být nutné obnovit stránku portálu.

Po vytvoření brány můžete zobrazením virtuální sítě na portálu zobrazit IP adresu, která jí byla přiřazena. Brána se zobrazí jako připojené zařízení. Kliknutím na připojené zařízení (bránu virtuální sítě) můžete zobrazit další informace.