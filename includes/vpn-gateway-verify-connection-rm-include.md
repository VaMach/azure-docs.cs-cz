### <a name="to-verify-your-connection-by-using-powershell"></a>Ověření připojení pomocí prostředí PowerShell
Pomocí rutiny `Get-AzureRmVirtualNetworkGatewayConnection` s nebo bez `-Debug` můžete ověřit, že vaše připojení bylo úspěšné. 

1. Použijte následující příklad rutiny a nakonfigurujte hodnoty tak, aby odpovídaly vašemu prostředí. Po zobrazení výzvy vyberte možnost „A“, abyste spustili „vše“. V příkladu odkazuje `-Name` na název připojení, které jste vytvořili a chcete ho testovat.
   
        Get-AzureRmVirtualNetworkGatewayConnection -Name MyGWConnection -ResourceGroupName MyRG
2. Po dokončení zpracování rutiny si prohlédněte hodnoty. Ve výše uvedeném příkladu se zobrazí stav připojení Připojeno a vy vidíte příchozí a odchozí bajty.
   
        Body:
        {
          "name": "MyGWConnection",
          "id":
        "/subscriptions/086cfaa0-0d1d-4b1c-94544-f8e3da2a0c7789/resourceGroups/MyRG/providers/Microsoft.Network/connections/MyGWConnection",
          "properties": {
            "provisioningState": "Succeeded",
            "resourceGuid": "1c484f82-23ec-47e2-8cd8-231107450446b",
            "virtualNetworkGateway1": {
              "id":
        "/subscriptions/086cfaa0-0d1d-4b1c-94544-f8e3da2a0c7789/resourceGroups/MyRG/providers/Microsoft.Network/virtualNetworkGa
        teways/vnetgw1"
            },
            "localNetworkGateway2": {
              "id":
        "/subscriptions/086cfaa0-0d1d-4b1c-94544-f8e3da2a0c7789/resourceGroups/MyRG/providers/Microsoft.Network/localNetworkGate
        ways/LocalSite"
            },
            "connectionType": "IPsec",
            "routingWeight": 10,
            "sharedKey": "abc123",
            "connectionStatus": "Connected",
            "ingressBytesTransferred": 33509044,
            "egressBytesTransferred": 4142431
          }

### <a name="to-verify-your-connection-by-using-the-azure-portal"></a>Ověření připojení pomocí webu Azure Portal
Na webu Azure Portal můžete zobrazit stav připojení otevřením připojení. To lze provést několika způsoby. Následující postup ukazuje jeden ze způsobů přechodu k připojení a jeho ověření.

1. Na webu [Azure Portal](http://portal.azure.com) klikněte na **Všechny prostředky** a přejděte ke své bráně virtuální sítě.
2. V okně vaší brány virtuální sítě klikněte na **Připojení**. Můžete zobrazit stav každého připojení.
3. Kliknutím na název připojení, které chcete ověřit, otevřete **Základní údaje**. V části Základní údaje můžete zobrazit další informace o připojení. **Stav** bude „Úspěšně dokončeno“ a „Připojeno“ po provedení úspěšného připojení.
   
    ![Ověření připojení](./media/vpn-gateway-verify-connection-rm-include/connectionsucceeded.png)

<!--HONumber=Oct16_HO3-->


