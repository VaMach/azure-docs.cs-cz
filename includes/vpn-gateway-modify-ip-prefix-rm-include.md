### Přidání nebo odebrání předpon, pokud jste ještě nevytvořili připojení brány sítě VPN

- **Pokud chcete přidat** další předpony adresy k bráně místní sítě, kterou jste vytvořili, ale ještě nemáte připojení brány sítě VPN, použijte následující příklad.

        $local = Get-AzureRmLocalNetworkGateway -Name LocalSite -ResourceGroupName testrg
        Set-AzureRmLocalNetworkGateway -LocalNetworkGateway $local -AddressPrefix @('10.0.0.0/24','20.0.0.0/24','30.0.0.0/24')


- **Pokud chcete odebrat** předponu adresy z brány místní sítě, která nemá připojení k síti VPN, použijte níže uvedený příklad. Vynechte předpony, které už nepotřebujete. V tomto příkladu už nepotřebujeme předponu 20.0.0.0/24 (z předchozího příkladu), takže bránu místní sítě aktualizujeme a tuto předponu vyloučíme.

        $local = Get-AzureRmLocalNetworkGateway -Name LocalSite -ResourceGroupName testrg
        Set-AzureRmLocalNetworkGateway -LocalNetworkGateway $local -AddressPrefix @('10.0.0.0/24','30.0.0.0/24')

### Přidání nebo odebrání předpon, pokud jste už vytvořili připojení brány sítě VPN

Pokud jste vytvořili připojení k síti VPN a chcete přidat nebo odebrat předpony IP adres obsažené v bráně místní sítě, musíte v uvedeném pořadí provést následující kroky. Způsobí to určitý výpadek připojení k síti VPN.

>[AZURE.IMPORTANT] Bránu sítě VPN neodstraňujte. Pokud to uděláte, budete muset znovu provést postup pro její vytvoření a znovu nakonfigurovat místní směrovač s využitím nového nastavení.
 
1. Odeberte připojení protokolu IPsec. 
2. Upravte předpony brány místní sítě. 
3. Vytvořte nové připojení protokolu IPsec. 

Jako vodítko můžete použít následující ukázku.

    $gateway1 = Get-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg
    $local = Get-AzureRmLocalNetworkGateway -Name LocalSite -ResourceGroupName testrg

    Remove-AzureRmVirtualNetworkGatewayConnection -Name localtovon -ResourceGroupName testrg

    $local = Get-AzureRmLocalNetworkGateway -Name LocalSite -ResourceGroupName testrg
    Set-AzureRmLocalNetworkGateway -LocalNetworkGateway $local -AddressPrefix @('10.0.0.0/24','20.0.0.0/24','30.0.0.0/24')
    
    New-AzureRmVirtualNetworkGatewayConnection -Name localtovon -ResourceGroupName testrg -Location 'West US' -VirtualNetworkGateway1 $gateway1 -LocalNetworkGateway2 $local -ConnectionType IPsec -RoutingWeight 10 -SharedKey 'abc123'



<!--HONumber=Jun16_HO2-->


