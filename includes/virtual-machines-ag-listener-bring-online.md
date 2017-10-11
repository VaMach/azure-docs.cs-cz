1. Ve Správci clusteru převzetí služeb při selhání rozbalte **role**a pak zvýraznit vaší skupiny dostupnosti.  

2. Na **prostředky** , klikněte pravým tlačítkem na název naslouchacího procesu a pak klikněte **vlastnosti**.

3. Klikněte **závislosti** kartě. Pokud nejsou uvedeny více prostředků, ověřte, že IP adresy OR, not a závislosti.  

4. Klikněte na **OK**.

5. Klikněte pravým tlačítkem na název naslouchacího procesu a pak klikněte na **přepnout do režimu Online**.

6. Po online na naslouchací proces **prostředky** , klikněte pravým tlačítkem na skupinu dostupnosti a pak klikněte **vlastnosti**.
   
    ![Konfigurace prostředek skupiny dostupnosti](./media/virtual-machines-sql-server-configure-alwayson-availability-group-listener/IC678772.gif)

7. Závislost na prostředku názvu naslouchací proces (ne IP adresy prostředky názvem) a potom klikněte na **OK**.
   
    ![Přidat závislost na název naslouchacího procesu](./media/virtual-machines-sql-server-configure-alwayson-availability-group-listener/IC678773.gif)

8. Spusťte SQL Server Management Studio a připojte se k primární replice.

9. Přejděte na **AlwaysOn vysokou dostupnost** > **skupiny dostupnosti** > **\<AvailabilityGroupName\>**   >  **Naslouchací procesy skupiny dostupnosti**.  
    Název naslouchacího procesu, který jste vytvořili ve Správci clusteru převzetí služeb při selhání by měl být zobrazen.

10. Klikněte pravým tlačítkem na název naslouchacího procesu a pak klikněte na **vlastnosti**.

11. V **Port** pole, zadejte číslo portu pro naslouchací proces skupiny dostupnosti pomocí $EndpointPort dříve používaného (v tomto kurzu 1433 se výchozí nastavení) a potom klikněte na **OK**.

