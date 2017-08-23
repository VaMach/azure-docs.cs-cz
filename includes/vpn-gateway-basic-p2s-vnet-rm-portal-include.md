Pokud chcete vytvořit virtuální síť v modelu nasazení Resource Manageru pomocí webu Azure Portal, postupujte podle následujících kroků. Snímky obrazovek slouží pouze jako příklady. Nezapomeňte hodnoty nahradit vlastními. Další informace o práci s virtuálními sítěmi najdete v článku [Přehled virtuálních sítí](../articles/virtual-network/virtual-networks-overview.md).

1. V prohlížeči přejděte na portál [Azure Portal](http://portal.azure.com) a v případě potřeby se přihlaste pomocí účtu Azure.
2. Klikněte na **+**. Do pole **Hledat na Marketplace** zadejte text „Virtuální síť“. Ve vráceném seznamu vyhledejte položku **Virtuální síť** a kliknutím otevřete stránku **Virtuální síť**.

  ![Stránka pro vyhledání prostředku virtuální sítě](./media/vpn-gateway-basic-p2s-vnet-rm-portal-include/newvnetportal700.png "Stránka pro vyhledání prostředku virtuální sítě")
3. U dolního okraje stránky Virtuální síť v seznamu **Vybrat model nasazení** vyberte **Resource Manager** a potom klikněte na **Vytvořit**.

  ![Výběr Resource Manageru](./media/vpn-gateway-basic-p2s-vnet-rm-portal-include/resourcemanager250.png "Výběr Resource Manageru")
4. Na stránce **Vytvořit virtuální síť** nakonfigurujte nastavení virtuální sítě. Po vyplnění polí se červený vykřičník změní na zelenou značku zaškrtnutí, pokud jsou znaky zadané do pole platné. Některé hodnoty mohou být vyplněny automaticky. V takovém případě je nahraďte vlastními. Stránka **Vytvořit virtuální síť** vypadá podobně jako v následujícím příkladu:

  ![Kontrola polí](./media/vpn-gateway-basic-p2s-vnet-rm-portal-include/createp2sgvnet.png "Kontrola polí")
5. **Název**: Zadejte název vaší virtuální sítě.
6. **Adresní prostor**: Zadejte adresní prostor. Pokud chcete přidat více adresních prostorů, přidejte první adresní prostor. Další adresní prostory můžete přidat později po vytvoření virtuální sítě.
7. **Název podsítě**: Přidejte název podsítě a rozsah adres podsítě. Další podsítě můžete přidat později po vytvoření virtuální sítě.
8. **Předplatné**: Zkontrolujte, jestli je uvedeno správné předplatné. Předplatná můžete měnit prostřednictvím rozevíracího seznamu.
9. **Skupina prostředků**: Vyberte existující skupinu prostředků nebo vytvořte novou zadáním názvu nové skupiny prostředků. Pokud vytváříte novou skupinu, nazvěte skupinu prostředků v souladu s hodnotami plánované konfigurace. Další informace o skupinách prostředků najdete v článku [Přehled Azure Resource Manageru](../articles/azure-resource-manager/resource-group-overview.md#resource-groups).
10. **Umístění**: Vyberte umístění sítě VNet. Umístění určuje, kde budou uloženy prostředky nasazené v této síti VNet.
11. Pokud chcete mít k síti VNet snadný přístup na řídicím panelu, vyberte možnost **Připnout na řídicí panel** a potom klikněte na **Vytvořit**.

 ![Připnout na řídicí panel](./media/vpn-gateway-basic-p2s-vnet-rm-portal-include/pintodashboard150.png "Připnout na řídicí panel")
12. Po kliknutí na **Vytvořit** se na řídicím panelu zobrazí dlaždice s informacemi o průběhu vytváření sítě VNet. Obsah dlaždice se v průběhu vytváření sítě VNet mění.

  ![Dlaždice Vytváří se virtuální síť](./media/vpn-gateway-basic-p2s-vnet-rm-portal-include/deploying150.png "Dlaždice Vytváří se virtuální síť")