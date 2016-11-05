## Vytvoření virtuální sítě VNet na portálu Azure
Pokud chcete na portálu Azure Preview s využitím výše uvedeného scénáře vytvořit virtuální síť VNet, použijte následující postup.

1. V prohlížeči přejděte na http://portal.azure.com a v případě potřeby se přihlaste pomocí účtu Azure.
2. Klikněte na **NOVÝ** > **Sítě** > **Virtuální síť**, v seznamu **Vyberte model nasazení** vyberte **Resource Manager** a potom klikněte na **Vytvořit**, jak vidíte na obrázku níže.
   
    ![Vytvoření virtuální sítě VNet na portálu Azure](./media/virtual-networks-create-vnet-arm-pportal-include/vnet-create-arm-pportal-figure1.gif)
3. V okně **Vytvořit virtuální síť** nakonfigurujte nastavení sítě VNet, jak znázorňuje následující obrázek.
   
    ![Okno Vytvořit virtuální síť](./media/virtual-networks-create-vnet-arm-pportal-include/vnet-create-arm-pportal-figure2.png)
4. Klikněte na **Skupina prostředků** a vyberte skupinu prostředků, do které chcete přidat síť VNet, případně klikněte na **Vytvořit novou** a přidejte síť VNet do nové skupiny prostředků. Následující obrázek ukazuje nastavení skupiny prostředky pro novou skupinu prostředků s názvem **TestRG**. Další informace o skupinách prostředků najdete v článku [Přehled Azure Resource Manageru](../articles/resource-group-overview.md#resource-groups).
   
    ![Skupina prostředků](./media/virtual-networks-create-vnet-arm-pportal-include/vnet-create-arm-pportal-figure3.png)
5. V případě potřeby změňte nastavení **Předplatné** a **Umístění** sítě VNet. 
6. Pokud nechcete tuto síť VNet zobrazovat jako dlaždici v zobrazení **Úvodní panel**, zakažte možnost **Připnout na Úvodní panel**. 
7. Klikněte na **Vytvořit** a všimněte si dlaždice s názvem **Vytváří se virtuální síť**, znázorněné na následujícím obrázku.
   
    ![dlaždice Vytváří se virtuální síť](./media/virtual-networks-create-vnet-arm-pportal-include/vnet-create-arm-pportal-figure4.png)
8. Počkejte na vytvoření sítě VNet a potom v okně **Virtuální síť** klikněte na **Všechna nastavení** > **Podsítě** > **Přidat**, jak vidíte níže.
   
    ![Přidání podsítě na portálu Azure](./media/virtual-networks-create-vnet-arm-pportal-include/vnet-create-arm-pportal-figure5.gif)
9. Zadejte nastavení podsítě pro podsíť *BackEnd*, jak znázorňuje následující obrázek, a klikněte na **OK**. 
   
    ![Nastavení podsítě](./media/virtual-networks-create-vnet-arm-pportal-include/vnet-create-arm-pportal-figure6.png)
10. Všimněte si seznamu podsítí znázorněného na následujícím obrázku.
    
    ![Seznam podsítí v síti VNet](./media/virtual-networks-create-vnet-arm-pportal-include/vnet-create-arm-pportal-figure7.png)

<!--HONumber=Sep16_HO3-->


