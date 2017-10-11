V tomto kroku vytvoříte ručně naslouchacího procesu skupiny dostupnosti ve Správci clusteru převzetí služeb při selhání a SQL Server Management Studio.

1. Otevřete Správce clusteru převzetí služeb při selhání z uzlu, který je hostitelem primární repliky.

2. Vyberte **sítě** uzel a Poznámka: název sítě s clustery. Tento název se používá v $ClusterNetworkName proměnné ve skriptu prostředí PowerShell.

3. Rozbalte název clusteru a pak klikněte na tlačítko **role**.

4. V **role** podokně klikněte pravým tlačítkem na název skupiny dostupnosti a potom vyberte **přidat prostředek** > **klientský přístupový bod**.
   
    ![Přidat klientský přístupový bod pro skupinu dostupnosti](./media/virtual-machines-sql-server-configure-alwayson-availability-group-listener/IC678769.gif)

5. V **název** pole, vytvořte název pro tento nový naslouchací proces, klikněte na **Další** dvakrát a potom klikněte na **Dokončit**.  
    Nelze zobrazit naslouchací proces nebo prostředek do online režimu v tomto okamžiku.

6. Klikněte **prostředky** kartě a potom rozbalte klientský přístupový bod, kterou jste právě vytvořili. 
    Zobrazí se prostředek IP adresy pro každou síť clusteru v clusteru. Pokud je to řešení Azure, se zobrazí pouze jeden prostředek IP adresy.

7. Proveďte jednu z následujících akcí:
   
   * Ke konfiguraci hybridního řešení:
     
        a. Klikněte pravým tlačítkem na prostředek IP adresy, která odpovídá místní podsíť a potom vyberte **vlastnosti**. Poznamenejte si název IP adresy a síťového názvu.
   
        b. Vyberte **statickou IP adresu**, přiřadit nepoužívané IP adresu a pak klikněte na tlačítko **OK**.
 
   * Konfigurace řešení služby Azure:

        a. Klikněte pravým tlačítkem na IP adresu prostředek, který odpovídá podsíti služby Azure a pak vyberte **vlastnosti**.
       
       > [!NOTE]
       > Pokud naslouchací proces později selže do režimu online z důvodu konfliktních IP adresa vybraná protokolem DHCP, můžete nakonfigurovat platnou statickou IP adresu v tomto okně Vlastnosti.
       > 
       > 

       b. Ve stejném **IP adresu** vlastnosti – okno, změny **název IP adresy**.  
        Tento název se používá v proměnné $IPResourceName skriptu prostředí PowerShell. Pokud vaše řešení zahrnuje více virtuálních sítí Azure, můžete tento krok opakujte pro každý prostředek IP.

