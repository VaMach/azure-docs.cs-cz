1. Na levé straně portálu klikněte na **+** a do vyhledávacího pole zadejte text „Brána virtuální sítě“. Ve výsledcích hledání vyhledejte položku **Brána virtuální sítě** a klikněte na ni. V dolní části stránky **Brána virtuální sítě** klikněte na **Vytvořit** a otevřete stránku **Vytvořit bránu virtuální sítě**.
2. Na stránce **Vytvořit bránu virtuální sítě** vyplňte hodnoty pro vaši bránu virtuální sítě.

  ![Pole na stránce Vytvořit bránu virtuální sítě](./media/vpn-gateway-add-gw-rm-portal-include/gw.png "Pole na stránce Vytvořit bránu virtuální sítě")
3. Na **vytvořit bránu virtuální sítě** stránky, zadejte hodnoty pro bránu virtuální sítě.

  - **Název**: Zadejte pro bránu název. Toto není název podsítě brány. Jedná se o název objektu brány, který vytváříte.
  - **Typ brány**: Vyberte **VPN**. Brány VPN používají bránu virtuální sítě typu **VPN**. 
  - **Typ VPN**: Vyberte typ VPN určený pro vaši konfiguraci. Většina konfigurací vyžaduje trasový typ VPN.
  - **SKU**: Z rozevíracího seznamu vyberte SKU brány. SKU uvedené v rozevíracím seznamu závisí na typu VPN, který jste vybrali. Další informace o SKU brány najdete v tématu [SKU brány](../articles/vpn-gateway/vpn-gateway-about-vpn-gateway-settings.md#gwsku).
  - **Umístění:** K zobrazení umístění může být se posunout. Upravte pole **Umístění**, aby odkazovalo na umístění, kde se nachází vaše virtuální síť. Pokud umístění neukazuje na oblasti, které virtuální sítě se nachází, když vyberete virtuální síť v dalším kroku, nebude se zobrazovat v rozevíracím seznamu.
  - **Virtuální síť:** Vyberte virtuální síť, do které chcete tuto bránu přidat. Klikněte na tlačítko **virtuální síť** chcete otevřít stránku 'Vybrat virtuální síť'. Vyberte virtuální síť. Pokud svou virtuální síť nevidíte, zkontrolujte, že pole Umístění odkazuje na oblast, ve které se nachází vaše virtuální síť.
  - **Rozsah adres podsítě brány**: Toto nastavení se zobrazí pouze, pokud nebyl vytvořen dříve podsíť brány pro vaši virtuální síť. Pokud jste dříve vytvořili platnou podsíť brány, nebude se zobrazovat toto nastavení.
  - **První konfigurace protokolu IP**: stránka 'vybrat veřejnou IP adresu, vytvoří veřejnou IP adresu objekt, který získá přidružené ke službě VPN gateway. Veřejná IP adresa se dynamicky přiřazuje tento objekt při vytvoření brány VPN. Služba VPN Gateway aktuálně podporuje pouze *dynamické* přidělení veřejné IP adresy. To ale neznamená, že se IP adresa po přiřazení k vaší bráně VPN bude měnit. Veřejná IP adresa se změní pouze v případě odstranění a nového vytvoření brány. V případě změny velikosti, resetování nebo jiné operace údržby/upgradu vaší brány VPN se nezmění.

    - Nejprve, klikněte na tlačítko **konfigurace IP brány vytvořit** otevřít stránku, zvolte veřejnou IP adresu, a pak klikněte na **+ vytvořit nový** chcete otevřít stránku, vytvoření veřejné IP adresy".
    - V dalším kroku vstup **název** pro svoji veřejnou IP adresu. Nechte SKU jako **základní** Pokud je konkrétní důvod, proč změňte jej na něco jiného, pak klikněte na **OK** v dolní části této stránky a uložit provedené změny.

      ![Vytvoření veřejné IP adresy](./media/vpn-gateway-add-gw-s2s-rm-portal-include/gwip.png "Vytvořit PIP")

4. Ověřte nastavení. Můžete vybrat **připnout na řídicí panel** v dolní části stránky, pokud chcete, aby se brána zobrazovala na řídicím panelu. 
5. Kliknutím na **Vytvořit** zahajte proces vytváření brány VPN. Nastavení se ověří a zobrazí se "nasazení Brána virtuální sítě" dlaždici na řídicím panelu. Vytváření brány může trvat až 45 minut. K zobrazení stavu dokončení může být nutné obnovit stránku portálu.

Po vytvoření brány můžete zobrazením virtuální sítě na portálu zobrazit IP adresu, která jí byla přiřazena. Brána se zobrazí jako připojené zařízení. Kliknutím na připojené zařízení (bránu virtuální sítě) můžete zobrazit další informace.
