Pokud chcete upravit IP adresu brány, použijte rutinu `New-AzureRmVirtualNetworkGatewayConnection`. Pokud se bude název brány místní sítě přesně shodovat s existujícím názvem, nastavení se přepíše. Rutina „Set“ v tuto chvíli nepodporuje úpravu IP adresy brány.

### <a name="gwipnoconnection"></a>Úprava IP adresy brány – žádné připojení brány

Pokud chcete aktualizovat IP adresu brány místní sítě, která ještě nemá připojení, použijte následující příklad. Můžete taky zároveň aktualizovat předpony adresy. Nastavení, které zadáte, přepíše stávající nastavení. Ujistěte se, že používáte stávající název brány místní sítě. Pokud to neuděláte, vytvoříte novou bránu místní sítě, místo abyste přepsali tu stávající.

Použijte následující příklad a nahraďte v něm hodnoty vlastními hodnotami.

    New-AzureRmLocalNetworkGateway -Name MyLocalNetworkGWName `
    -Location "West US" -AddressPrefix @('10.0.0.0/24','20.0.0.0/24','30.0.0.0/24') `
    -GatewayIpAddress "5.4.3.2" -ResourceGroupName MyRGName


### <a name="gwipwithconnection"></a>Úprava IP adresy brány – existující připojení brány

Pokud už nějaké připojení brány existuje, musíte ho nejdřív odebrat. Potom můžete upravit IP adresu brány a vytvořit nové připojení. Způsobí to určitý výpadek připojení k síti VPN.


>[AZURE.IMPORTANT] Bránu sítě VPN neodstraňujte. Pokud to uděláte, budete muset znovu provést postup pro její vytvoření a znovu nakonfigurovat místní směrovač na IP adresu, která se přiřadí nově vytvořené bráně.
 

1. Odeberte připojení. Název připojení můžete zjistit pomocí rutiny `Get-AzureRmVirtualNetworkGatewayConnection`.

        Remove-AzureRmVirtualNetworkGatewayConnection -Name MyGWConnectionName `
        -ResourceGroupName MyRGName

2. Upravte hodnotu GatewayIpAddress. V tuto chvíli taky můžete upravit předpony adresy, pokud je to potřeba. Upozorňujeme, že při tom dojde k odebrání stávajícího nastavení brány místní sítě. Při úpravě použijte stávající název brány místní sítě, aby došlo k přepsání nastavení. Pokud to neuděláte, vytvoříte novou bránu místní sítě, místo abyste upravili tu stávající.

        New-AzureRmLocalNetworkGateway -Name MyLocalNetworkGWName `
        -Location "West US" -AddressPrefix @('10.0.0.0/24','20.0.0.0/24','30.0.0.0/24') `
        -GatewayIpAddress "104.40.81.124" -ResourceGroupName MyRGName

3. Vytvořte připojení. V tomto příkladu konfigurujeme typ připojení IPsec. Až budete připojení znovu vytvářet, použijte typ připojení stanovený pro vaši konfiguraci. Informace o dalších typech připojení najdete na stránce [Rutina prostředí PowerShell](https://msdn.microsoft.com/library/mt603611.aspx).  Pokud chcete získat název brány VirtualNetworkGateway, můžete spustit rutinu `Get-AzureRmVirtualNetworkGateway`.

    Nastavte proměnné:

        $local = Get-AzureRMLocalNetworkGateway -Name MyLocalNetworkGWName -ResourceGroupName MyRGName `
        $vnetgw = Get-AzureRmVirtualNetworkGateway -Name RMGateway -ResourceGroupName MyRGName

    Vytvořte připojení:
    
        New-AzureRmVirtualNetworkGatewayConnection -Name MyGWConnectionName -ResourceGroupName MyRGName `
        -Location "West US" `
        -VirtualNetworkGateway1 $vnetgw `
        -LocalNetworkGateway2 $local `
        -ConnectionType IPsec -RoutingWeight 10 -SharedKey 'abc123'




<!---HONumber=Aug16_HO4-->


