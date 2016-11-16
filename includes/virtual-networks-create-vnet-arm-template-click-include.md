## <a name="deploy-the-arm-template-by-using-click-to-deploy"></a>Nasazení šablony ARM pomocí metody Click to Deploy
Můžete opakovaně používat předdefinované šablony ARM nahrané v úložišti GitHub, které udržuje Microsoft a které je otevřené celé komunitě. Tyto šablony se dají nasazovat přímo z webu GitHub nebo si je můžete stáhnout a upravit podle potřeby. Pokud chcete nasadit šablonu, která vytvoří síť VNet se dvěma podsítěmi, použijte následující postup.

1. V prohlížeči přejděte na stránku [https://github.com/Azure/azure-quickstart-templates](https://github.com/Azure/azure-quickstart-templates).
2. Přejděte dolů v seznamu šablon a klikněte na šablonu **101-vnet-two-subnets**. Podívejte se do souboru **README.md** znázorněného níže.
   
    ![Soubor READEME.md na webu GitHub](./media/virtual-networks-create-vnet-arm-template-click-include/figure1.png)
3. Klikněte na **Deploy to Azure** (Nasadit do Azure). V případě potřeby zadejte své přihlašovací údaje Azure. 
4. V okně **Parametry** zadejte hodnoty, které chcete použít k vytvoření nové sítě VNet, a klikněte na **OK**. Následující obrázek znázorňuje hodnoty pro náš scénář.
   
    ![Parametry šablony ARM](./media/virtual-networks-create-vnet-arm-template-click-include/figure2.png)
5. Klikněte na **Skupina prostředků** a vyberte skupinu prostředků, do které chcete přidat síť VNet, případně klikněte na **Vytvořit novou** a přidejte síť VNet do nové skupiny prostředků. Následující obrázek ukazuje nastavení skupiny prostředky pro novou skupinu prostředků s názvem **TestRG**.
   
    ![Skupina prostředků](./media/virtual-networks-create-vnet-arm-template-click-include/figure3.png)
6. V případě potřeby změňte nastavení **Předplatné** a **Umístění** sítě VNet.
7. Pokud nechcete tuto síť VNet zobrazovat jako dlaždici v zobrazení **Úvodní panel**, zakažte možnost **Připnout na Úvodní panel**.
8. Klikněte na **Právní podmínky**, přečtěte si podmínky a vyjádřete souhlas kliknutím na tlačítko **Koupit**. 
9. Kliknutím na **Vytvořit** vytvořte síť VNet.
   
    ![Dlaždice Odesílá se nasazení na portálu Preview](./media/virtual-networks-create-vnet-arm-template-click-include/figure4.png)
10. Až se nasazení provede, klikněte na **TestVNet** > **Všechna nastavení** > **Podsítě** a podívejte se na vlastnosti podsítě, jak vidíte níže.
    
     ![Vytvoření sítě VNet na portálu Preview](./media/virtual-networks-create-vnet-arm-template-click-include/figure5.gif)



<!--HONumber=Nov16_HO2-->


