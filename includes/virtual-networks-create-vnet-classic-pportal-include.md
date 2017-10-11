## <a name="how-to-create-a-classic-vnet-in-the-azure-portal"></a>Postup vytvoření klasické virtuální sítě na portálu Azure
Pokud chcete vytvořit klasické virtuální sítě založené na výše uvedené scénáře, použijte následující postup.

1. V prohlížeči přejděte na http://portal.azure.com a v případě potřeby se přihlaste pomocí účtu Azure.
2. Klikněte na tlačítko **nový** > **sítě** > **virtuální síť**, Všimněte si, že **vybrat model nasazení** Zobrazí seznam již **Classic**a potom klikněte na **vytvořit**, jak je vidět na obrázku níže.
   
    ![Vytvoření virtuální sítě VNet na portálu Azure](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure1.gif)
3. Na **virtuální síť** okno, zadejte **název** virtuální síť a potom klikněte na **adresní prostor**. Nakonfigurujte nastavení prostoru adres pro síť VNet a jeho první podsíť a pak klikněte na tlačítko **OK**. Následující obrázek znázorňuje nastavení blok CIDR pro náš scénář.
   
    ![Okno prostoru adres](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure2.png)
4. Klikněte na tlačítko **skupiny prostředků** a vyberte skupinu prostředků přidat síť VNet, nebo kliknutím na tlačítko **vytvořit novou skupinu prostředků** přidat síť VNet do nové skupiny prostředků. Následující obrázek ukazuje nastavení skupiny prostředky pro novou skupinu prostředků s názvem **TestRG**. Další informace o skupinách prostředků najdete v článku [Přehled Azure Resource Manageru](../articles/azure-resource-manager/resource-group-overview.md#resource-groups).
   
    ![Vytvořit okno skupiny prostředků](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure3.png)
5. V případě potřeby změňte nastavení **Předplatné** a **Umístění** sítě VNet. 
6. Pokud nechcete tuto síť VNet zobrazovat jako dlaždici v zobrazení **Úvodní panel**, zakažte možnost **Připnout na Úvodní panel**. 
7. Klikněte na **Vytvořit** a všimněte si dlaždice s názvem **Vytváří se virtuální síť**, znázorněné na následujícím obrázku.
   
    ![Vytvoření sítě VNet na portálu](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure4.png)
8. Počkejte pro virtuální síť, který se má vytvořit a pokud se zobrazí na dlaždici níže, kliknutím na Přidat další podsítě.
   
    ![Vytvoření sítě VNet na portálu](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure5.png)
9. Měli byste vidět **konfigurace** pro vaši virtuální síť, jak je uvedeno níže. 
   
    ![Vytvoření sítě VNet na portálu](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure6.png)
10. Klikněte na tlačítko **podsítě** > **přidat**, zadejte **název** a zadejte **rozsahu (blok CIDR) adres** pro podsíť a potom Klikněte na tlačítko **OK**. Následující obrázek ukazuje nastavení pro náš scénář aktuální.
    
    ![Vytvoření virtuální sítě VNet na portálu Azure](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure7.gif)

