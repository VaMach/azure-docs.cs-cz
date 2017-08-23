1. Na levé straně portálu klikněte na **+** a do vyhledávacího pole zadejte text „Brána virtuální sítě“. Ve výsledcích hledání vyhledejte položku **Brána virtuální sítě** a klikněte na ni. V dolní části stránky **Brána virtuální sítě** klikněte na **Vytvořit** a otevřete stránku **Vytvořit bránu virtuální sítě**.
2. Na stránce **Vytvořit bránu virtuální sítě** vyplňte hodnoty pro vaši bránu virtuální sítě.

  ![Pole na stránce Vytvořit bránu virtuální sítě](./media/vpn-gateway-add-gw-p2s-rm-portal-include/p2sgw.png "Pole na stránce Vytvořit bránu virtuální sítě")
3. **Název**: Zadejte pro bránu název. Název brány není stejný jako název podsítě brány. Jedná se o název objektu brány, který vytváříte.
4. **Typ brány**: Vyberte **VPN**. Brány VPN používají bránu virtuální sítě typu **VPN**.
5. **Typ VPN**: Vyberte typ VPN určený pro vaši konfiguraci. Většina konfigurací vyžaduje trasový typ VPN.
6. **SKU**: Z rozevíracího seznamu vyberte SKU brány. SKU uvedené v rozevíracím seznamu závisí na typu VPN, který jste vybrali.
7. **Umístění**: Upravte pole **Umístění** tak, aby odkazovalo na umístění, ve kterém se nachází vaše virtuální síť. Pokud umístění neukazuje na oblast, ve které je vaše virtuální síť umístěná, tato síť se nezobrazí v rozevírací nabídce Zvolit virtuální síť.
8. Zvolte virtuální síť, do které chcete bránu přidat. Kliknutím na **Virtuální síť** otevřete okno **Zvolit virtuální síť**. Vyberte virtuální síť. Pokud svou virtuální síť nevidíte, zkontrolujte, že pole **Umístění** odkazuje na oblast, ve které se nachází vaše virtuální síť.
9. **Veřejná IP adresa:** Vytvořte objekt veřejné IP adresy, ke kterému se bude dynamicky přiřazovat veřejná IP adresa. Kliknutím na **Veřejná IP adresa** otevřete stránku **Zvolit veřejnou IP adresu**. Kliknutím na **+Vytvořit novou** otevřete stránku **Vytvořit veřejnou IP adresu**. Zadejte název veřejné IP adresy. Klikněte na tlačítko **OK** a uložte změny. IP adresa se dynamicky přiřadí po vytvoření brány VPN. Služba VPN Gateway aktuálně podporuje pouze *dynamické* přidělení veřejné IP adresy. To ale neznamená, že se IP adresa po přiřazení k vaší bráně VPN bude měnit. Veřejná IP adresa se změní pouze v případě odstranění a nového vytvoření brány. V případě změny velikosti, resetování nebo jiné operace údržby/upgradu vaší brány VPN se nezmění.
10. **Předplatné**: Zkontrolujte, jestli je vybráno správné předplatné.
11. **Skupina prostředků**: Toto nastavení je určeno vámi vybranou virtuální sítí.
12. Po zadání předchozích nastavení už hodnotu **Umístění** neupravujte.
13. Ověřte nastavení. Pokud chcete, aby se brána zobrazovala na řídicím panelu, můžete v dolní části stránky vybrat **Připnout na řídicí panel**.
14. Kliknutím na **Vytvořit** zahajte proces vytváření brány. Nastavení se ověří a provede se nasazení brány. Vytváření brány může trvat až 45 minut.

Po vytvoření brány můžete zobrazit IP adresu, která k ní byla přiřazena, zobrazením virtuální sítě. Brána se zobrazí jako připojené zařízení. Kliknutím na připojené zařízení (bránu virtuální sítě) můžete zobrazit další informace.