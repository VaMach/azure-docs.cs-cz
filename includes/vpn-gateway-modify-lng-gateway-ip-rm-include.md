Pokud chcete upravit IP adresu brány, použijte k tomu rutinu New-AzureRmVirtualNetworkGatewayConnection. Rutina Set v současné době nepodporuje úpravu IP adresy brány.

### <a name="gwipnoconnection"></a>Úprava IP adresy brány – žádné připojení brány
Pokud chcete upravit IP adresu brány místní sítě, která ještě nemá připojení, použijte následující příklad. Můžete také zároveň upravit předpony adresy. Ujistěte se, že k přepsání aktuálního nastavení používáte existující název brány místní sítě. Pokud to neuděláte, vytvoříte novou bránu místní sítě, místo abyste přepsali tu stávající.

Použijte následující příklad a nahraďte v něm hodnoty vlastními hodnotami:

```powershell
New-AzureRmLocalNetworkGateway -Name MyLocalNetworkGWName `
-Location "West US" -AddressPrefix @('10.0.0.0/24','20.0.0.0/24','30.0.0.0/24') `
-GatewayIpAddress "5.4.3.2" -ResourceGroupName MyRGName
```

### <a name="gwipwithconnection"></a>Úprava IP adresy brány – existující připojení brány
Pokud už nějaké připojení brány existuje, musíte ho nejdřív odebrat. Po odebrání připojení můžete upravit IP adresu brány a vytvořit nové připojení. Můžete také zároveň upravit předpony adresy. Způsobí to určitý výpadek připojení VPN.

> [!IMPORTANT]
> Bránu sítě VPN neodstraňujte. Pokud to uděláte, budete muset znovu provést kroky k jejímu vytvoření. Kromě toho je nutné aktualizovat místní zařízení VPN o novou IP adresu brány VPN.
> 
> 

1. Odeberte připojení. Název vašeho připojení můžete najít pomocí rutiny Get-AzureRmVirtualNetworkGatewayConnection.

  ```powershell
  Remove-AzureRmVirtualNetworkGatewayConnection -Name MyGWConnectionName `
  -ResourceGroupName MyRGName
  ```
2. Upravte hodnotu GatewayIpAddress. Můžete také zároveň upravit předpony adresy. Ujistěte se, že k přepsání aktuálního nastavení používáte existující název brány místní sítě. Pokud to neuděláte, vytvoříte novou bránu místní sítě, místo abyste přepsali tu stávající.

  ```powershell
  New-AzureRmLocalNetworkGateway -Name MyLocalNetworkGWName `
  -Location "West US" -AddressPrefix @('10.0.0.0/24','20.0.0.0/24','30.0.0.0/24') `
  -GatewayIpAddress "104.40.81.124" -ResourceGroupName MyRGName
  ```
3. Vytvořte připojení. V tomto příkladu konfigurujeme typ připojení IPsec. Až budete připojení znovu vytvářet, použijte typ připojení stanovený pro vaši konfiguraci. Informace o dalších typech připojení najdete na stránce [Rutina prostředí PowerShell](https://msdn.microsoft.com/library/mt603611.aspx).  Pokud chcete získat název brány VirtualNetworkGateway, můžete spustit rutinu Get-AzureRmVirtualNetworkGateway.
   
    Nastavte proměnné.

  ```powershell
  $local = Get-AzureRMLocalNetworkGateway -Name MyLocalNetworkGWName -ResourceGroupName MyRGName `
  $vnetgw = Get-AzureRmVirtualNetworkGateway -Name RMGateway -ResourceGroupName MyRGName
  ```
   
    Vytvořte připojení.

  ```powershell 
  New-AzureRmVirtualNetworkGatewayConnection -Name MyGWConnectionName -ResourceGroupName MyRGName `
  -Location "West US" `
  -VirtualNetworkGateway1 $vnetgw `
  -LocalNetworkGateway2 $local `
  -ConnectionType IPsec -RoutingWeight 10 -SharedKey 'abc123'
  ```