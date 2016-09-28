Pokud chcete vytvořit síť VNet pomocí portálu Azure, použijte následující postup. Snímky obrazovek slouží pouze jako příklady. Hodnoty na obrázcích nahraďte vlastními hodnotami. Další informace o práci s virtuálními sítěmi najdete v článku [Přehled virtuálních sítí](../articles/virtual-network/virtual-networks-overview.md).

1. V prohlížeči přejděte na [portál Azure](http://portal.azure.com) a v případě potřeby se přihlaste pomocí účtu Azure.

2. Klikněte na **Nový** **>** **Sítě** **>** **Virtuální síť**.

    ![Okno sítí VNet](./media/vpn-gateway-basic-vnet-rm-portal-include/newvnetportal650.png)

3. U dolního okraje okna virtuální sítě v seznamu **Vybrat model nasazení** vyberte **Správce prostředků** a potom klikněte na **Vytvořit**.


    ![Výběr správce prostředků](./media/vpn-gateway-basic-vnet-rm-portal-include/resourcemanager250.png)

4. V okně **Vytvoření virtuální sítě** nakonfigurujte nastavení sítě VNet. V tomto okně přidáte první adresní prostor a jeden rozsah adres podsítě. Po dokončení vytváření sítě VNet můžete přejít zpět a přidat další podsítě a adresní prostory. Jedná se o aktuální omezení portálu. Vždy se můžete vrátit zpět a aktualizovat tyto hodnoty úpravou vlastností sítě VNet na portálu nebo pomocí PowerShellu. Použité hodnoty budou záviset na konfiguraci, kterou chcete vytvořit. Vždy používejte hodnoty odpovídající hodnotám plánované konfigurace. 

    ![Okno Vytvoření virtuální sítě](./media/vpn-gateway-basic-vnet-rm-portal-include/createavnet250.png)

5. V rozevíracím seznamu **Předplatné** zkontrolujte, jestli je vybrané správné předplatné. Pomocí rozevíracího seznamu můžete vybrané předplatné změnit.

6. Klikněte na **Skupina prostředků** a vyberte existující skupinu nebo zadáním názvu nové skupiny prostředků vytvořte novou skupinu prostředků. Pokud vytváříte novou skupinu, nazvěte skupinu prostředků v souladu s hodnotami plánované konfigurace. Další informace o skupinách prostředků najdete v článku [Přehled Azure Resource Manageru](resource-group-overview.md#resource-groups).

7. Potom vyberte nastavení **Umístění** sítě VNet. Upozornění: Umístění určí, kde budou uložené prostředky nasazené v této síti VNet. Toto nastavení nemůžete později změnit, aniž by byly prostředky znovu nasazeny.

8. Pokud chcete mít k síti VNet snadný přístup na řídicím panelu, vyberte možnost **Připnout na řídicí panel** a potom klikněte na **Vytvořit**.
    
    ![Připnutí na řídicí panel](./media/vpn-gateway-basic-vnet-rm-portal-include/pintodashboard150.png)


9. Po kliknutí na Vytvořit se na řídicím panelu zobrazí dlaždice s informacemi o průběhu vytváření sítě VNet. Obsah dlaždice se bude v průběhu vytváření sítě VNet měnit.

    ![Dlaždice Vytváření virtuální sítě](./media/vpn-gateway-basic-vnet-rm-portal-include/deploying150.png)

<!--HONumber=Sep16_HO3-->


