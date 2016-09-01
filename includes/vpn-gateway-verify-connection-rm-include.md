### Ověření připojení pomocí prostředí PowerShell

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

### Ověření připojení pomocí webu Azure Portal

Na webu Azure Portal můžete zobrazit stav připojení otevřením připojení. To lze provést několika způsoby. Níže naleznete jednu možnost jak přejít na vaše připojení.

1. Na webu [Azure Portal](http://portal.azure.com) otevřete **Brány virtuální sítě**. Klikněte na název brány.
2. V podokně v části **Nastavení** klikněte na tlačítko **Připojení**. Můžete zobrazit stav každého připojení.
3. Pro další informace o připojení klikněte na název připojení. Na stránce Essentials týkající se připojení věnujte pozornost položce **Stav připojení**. Stav bude „Úspěšně dokončeno“ a „Připojeno“ po provedení úspěšného připojení. Tok dat můžete zkontrolovat pohledem na **Vstupující data** a **Vystupující data**.

    V příkladu níže má **Stav připojení** hodnotu „Nepřipojeno“. 

    ![Ověření připojení](./media/vpn-gateway-verify-connection-rm-include/connectionverify450.png)


<!---HONumber=Aug16_HO4-->


