1. Na [portálu](http://portal.azure.com) přejděte na virtuální síť Resource Manageru, pro kterou chcete vytvořit bránu virtuální sítě.
2. Na stránce vaší virtuální sítě v části **Nastavení** kliknutím na **Podsítě** rozbalte stránku **Podsítě**.
3. Na stránce **Podsítě** kliknutím na **+Podsíť brány** otevřete stránku **Přidat podsíť**. 

  ![Přidání podsítě brány](./media/vpn-gateway-add-gwsubnet-p2s-rm-portal-include/addgwsubnet.png "Přidání podsítě brány")
4. **Název** podsítě se automaticky vyplní hodnotou GatewaySubnet. To je požadovaná hodnota, aby služba Azure podsíť rozpoznala jako podsíť brány. Upravte automaticky vyplněné hodnoty v poli **Adresní prostor** tak, aby odpovídaly požadavkům vaší konfigurace, a pak kliknutím na **OK** v dolní části stránky vytvořte podsíť.

  ![Přidání podsítě](./media/vpn-gateway-add-gwsubnet-p2s-rm-portal-include/p2sgwsub.png "Přidání podsítě")