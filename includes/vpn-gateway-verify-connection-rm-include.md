Připojení VPn můžete ověřit na portálu Azure tak, že přejdete na **Brány virtuální sítě** **>** ***klikněte na název brány*** **>** **Nastavení** **>** **Připojení**. Když vyberete název připojení, můžete zobrazit další informace o tomto připojení. V následujícím příkladu není připojení připojené a nepřenášejí se přes ně žádná data.


![Ověření připojení](./media/vpn-gateway-verify-connection-rm-include/connectionverify450.png)


### Ověření připojení pomocí prostředí PowerShell

Úspěšné vytvoření připojení můžete ověřit také pomocí příkazu `Get-AzureRmVirtualNetworkGatewayConnection –Debug`. Můžete použít následující příklad rutiny a nakonfigurovat hodnoty tak, aby odpovídaly vašemu prostředí. Po zobrazení výzvy vyberte „A“, abyste spustili vše.

    Get-AzureRmVirtualNetworkGatewayConnection -Name localtovon -ResourceGroupName testrg -Debug

 Po dokončení rutiny si prohlédněte hodnoty. Ve výše uvedeném příkladu se zobrazí stav připojení *Připojeno* a vy vidíte příchozí a odchozí bajty.

    Body:
    {
      "name": "localtovon",
      "id":
    "/subscriptions/086cfaa0-0d1d-4b1c-9455-f8e3da2a0c7789/resourceGroups/testrg/providers/Microsoft.Network/connections/loca
    ltovon",
      "properties": {
        "provisioningState": "Succeeded",
        "resourceGuid": "1c484f82-23ec-47e2-8cd8-231107450446b",
        "virtualNetworkGateway1": {
          "id":
    "/subscriptions/086cfaa0-0d1d-4b1c-9455-f8e3da2a0c7789/resourceGroups/testrg/providers/Microsoft.Network/virtualNetworkGa
    teways/vnetgw1"
        },
        "localNetworkGateway2": {
          "id":
    "/subscriptions/086cfaa0-0d1d-4b1c-9455-f8e3da2a0c7789/resourceGroups/testrg/providers/Microsoft.Network/localNetworkGate
    ways/LocalSite"
        },
        "connectionType": "IPsec",
        "routingWeight": 10,
        "sharedKey": "abc123",
        "connectionStatus": "Connected",
        "ingressBytesTransferred": 33509044,
        "egressBytesTransferred": 4142431
      }


<!--HONumber=Jun16_HO2-->


