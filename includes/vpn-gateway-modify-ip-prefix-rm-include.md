### <a name="noconnection"></a>Postup přidání nebo odebrání předpon – žádné připojení brány

- **Pokud chcete přidat** další předpony adresy k bráně místní sítě, kterou jste vytvořili, ale ještě nemáte připojení brány, použijte následující příklad. Nezapomeňte změnit hodnoty na své vlastní.

        $local = Get-AzureRmLocalNetworkGateway -Name MyLocalNetworkGWName -ResourceGroupName MyRGName `
        Set-AzureRmLocalNetworkGateway -LocalNetworkGateway $local `
        -AddressPrefix @('10.0.0.0/24','20.0.0.0/24','30.0.0.0/24')

- **Pokud chcete odebrat** předponu adresy z brány místní sítě, která nemá připojení k síti VPN, použijte níže uvedený příklad. Vynechte předpony, které už nepotřebujete. V tomto příkladu už nepotřebujeme předponu 20.0.0.0/24 (z předchozího příkladu), takže bránu místní sítě aktualizujeme a tuto předponu vyloučíme.

        $local = Get-AzureRmLocalNetworkGateway -Name MyLocalNetworkGWName -ResourceGroupName MyRGName `
        Set-AzureRmLocalNetworkGateway -LocalNetworkGateway $local `
        -AddressPrefix @('10.0.0.0/24','30.0.0.0/24')

### <a name="withconnection"></a>Postup přidání nebo odebrání předpon – existující připojení brány

Pokud jste vytvořili připojení k bráně a chcete přidat nebo odebrat předpony IP adres obsažené v bráně místní sítě, musíte v uvedeném pořadí provést následující kroky. Způsobí to určitý výpadek připojení k síti VPN. Při aktualizaci předpon nejprve odeberete připojení, upravíte předpony a pak vytvořte nové připojení. V následujících příkladech nezapomeňte změnit hodnoty na své vlastní.

>[AZURE.IMPORTANT] Bránu sítě VPN neodstraňujte. Pokud to uděláte, budete muset znovu provést postup pro její vytvoření a znovu nakonfigurovat místní směrovač s využitím nového nastavení.
 
1. Odeberte připojení.

        Remove-AzureRmVirtualNetworkGatewayConnection -Name MyGWConnectionName -ResourceGroupName MyRGName

2. Upravte předpony IP adresy pro bránu místní sítě.

    Nastavte proměnnou pro LocalNetworkGateway.

        $local = Get-AzureRmLocalNetworkGateway -Name MyLocalNetworkGWName -ResourceGroupName MyRGName

    Upravte předpony.

        Set-AzureRmLocalNetworkGateway -LocalNetworkGateway $local `
        -AddressPrefix @('10.0.0.0/24','20.0.0.0/24','30.0.0.0/24')

4. Vytvořte připojení. V tomto příkladu konfigurujeme typ připojení IPsec. Až budete připojení znovu vytvářet, použijte typ připojení stanovený pro vaši konfiguraci. Informace o dalších typech připojení najdete na stránce [Rutina prostředí PowerShell](https://msdn.microsoft.com/library/mt603611.aspx).

    Nastavte proměnnou pro VirtualNetworkGateway.

        $gateway1 = Get-AzureRmVirtualNetworkGateway -Name RMGateway  -ResourceGroupName MyRGName

    Vytvořte připojení. Všimněte si, že v tomto příkladu se používá proměnná $local, kterou jste nastavili v předchozím kroku.


        New-AzureRmVirtualNetworkGatewayConnection -Name MyGWConnectionName `
        -ResourceGroupName MyRGName -Location 'West US' `
        -VirtualNetworkGateway1 $gateway1 -LocalNetworkGateway2 $local `
        -ConnectionType IPsec `
        -RoutingWeight 10 -SharedKey 'abc123'


<!--HONumber=Sep16_HO3-->


