Pokud chcete vytvořit virtuální síť v modelu nasazení Resource Manageru pomocí webu Azure Portal, postupujte podle následujících kroků. Snímky obrazovek slouží pouze jako příklady. Nezapomeňte hodnoty nahradit vlastními. Další informace o práci s virtuálními sítěmi najdete v článku [Přehled virtuálních sítí](../articles/virtual-network/virtual-networks-overview.md).

1. V prohlížeči přejděte na [portál Azure](http://portal.azure.com) a v případě potřeby se přihlaste pomocí účtu Azure.
2. Klikněte na možnost **Nové**. Do pole **Hledat na Marketplace** zadejte text „Virtuální síť“. Ve vráceném seznamu vyhledejte položku **Virtuální síť** a kliknutím otevřete okno **Virtuální síť**.
   
    ![Okno vyhledání prostředku Virtuální síť](./media/vpn-gateway-basic-vnet-rm-portal-include/newvnetportal700.png "Locate virtual network resource blade")
3. U dolního okraje okna virtuální sítě v seznamu **Vybrat model nasazení** vyberte **Správce prostředků** a potom klikněte na **Vytvořit**.

    ![Výběr správce prostředků](./media/vpn-gateway-basic-vnet-rm-portal-include/resourcemanager250.png "Select Resource Manager")

1. V okně **Vytvoření virtuální sítě** nakonfigurujte nastavení sítě VNet. Po vyplnění polí se červený vykřičník změní na zelenou značku zaškrtnutí, pokud jsou znaky zadané do pole platné.
   
    ![Ověřování polí](./media/vpn-gateway-basic-vnet-rm-portal-include/checkmark300.png "Field validation")
2. Okno **Vytvořit virtuální síť** vypadá podobně jako v následujícím příkladu. Některé hodnoty mohou být vyplněny automaticky. V takovém případě je nahraďte vlastními.
   
    ![Okno Vytvořit virtuální síť](./media/vpn-gateway-basic-vnet-rm-portal-include/createvnet300.png "Create virtual network blade")
3. **Název**: Zadejte název vaší virtuální sítě.
4. **Adresní prostor**: Zadejte adresní prostor. Pokud chcete přidat více adresních prostorů, přidejte první adresní prostor. Další adresní prostory můžete přidat později po vytvoření virtuální sítě.
5. **Název podsítě**: Přidejte název podsítě a rozsah adres podsítě. Další podsítě můžete přidat později po vytvoření virtuální sítě.
6. **Předplatné**: Zkontrolujte, jestli je uvedeno správné předplatné. Předplatná můžete měnit prostřednictvím rozevíracího seznamu.
7. **Skupina prostředků**: Vyberte existující skupinu prostředků nebo vytvořte novou zadáním názvu nové skupiny prostředků. Pokud vytváříte novou skupinu, nazvěte skupinu prostředků v souladu s hodnotami plánované konfigurace. Další informace o skupinách prostředků najdete v článku [Přehled Azure Resource Manageru](../articles/azure-resource-manager/resource-group-overview.md#resource-groups).
8. **Umístění**: Vyberte umístění sítě VNet. Umístění určuje, kde budou uloženy prostředky nasazené v této síti VNet.
9. Pokud chcete mít k síti VNet snadný přístup na řídicím panelu, vyberte možnost **Připnout na řídicí panel** a potom klikněte na **Vytvořit**.
   
   ![Připnutí na řídicí panel](./media/vpn-gateway-basic-vnet-rm-portal-include/pintodashboard150.png "pin to dashboard")
10. Po kliknutí na **Vytvořit** se na řídicím panelu zobrazí dlaždice s informacemi o průběhu vytváření sítě VNet. Obsah dlaždice se v průběhu vytváření sítě VNet mění.
    
    ![Dlaždice Vytváření virtuální sítě](./media/vpn-gateway-basic-vnet-rm-portal-include/deploying150.png "Creating virtual network tile")



<!--HONumber=Nov16_HO2-->


