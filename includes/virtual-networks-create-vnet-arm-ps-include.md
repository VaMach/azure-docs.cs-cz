## Vytvoření sítě VNet pomocí prostředí PowerShell
Pokud chcete vytvořit síť VNet pomocí prostředí PowerShell, použijte následující postup.

1. Pokud jste prostředí Azure PowerShell nikdy nepoužívali, přejděte na téma [Instalace a konfigurace prostředí Azure PowerShell](../articles/powershell-install-configure.md) a proveďte všechny pokyny, abyste se mohli přihlásit k Azure a vybrat své předplatné.
    
2. V případě potřeby vytvořte novou skupinu prostředků, jak vidíte níže. V našem scénáři vytvoříte skupinu prostředků s názvem *TestRG*. Další informace o skupinách prostředků najdete v tématu [Přehled Azure Resource Manageru](../articles/resource-group-overview.md).

        New-AzureRmResourceGroup -Name TestRG -Location centralus

    Očekávaný výstup:
    
        ResourceGroupName : TestRG
        Location          : centralus
        ProvisioningState : Succeeded
        Tags              :
        Permissions       :
                            Actions  NotActions
                            =======  ==========
                            *
        
        ResourceId        : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG   

3. Vytvořte novou síť VNet s názvem *TestVNet*, jak vidíte níže.

        New-AzureRmVirtualNetwork -ResourceGroupName TestRG -Name TestVNet `
            -AddressPrefix 192.168.0.0/16 -Location centralus   
        
    Očekávaný výstup:

        Name              : TestVNet
        ResourceGroupName : TestRG
        Location          : centralus
        Id                : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet
        Etag              : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
        ProvisioningState : Succeeded
        Tags              :
        AddressSpace      : {
                              "AddressPrefixes": [
                                "192.168.0.0/16"
                              ]
                            }
        DhcpOptions       : {
                              "DnsServers": null
                            }
        NetworkInterfaces : null
        Subnets           : []

4. Uložte objekt virtuální sítě do proměnné, jak vidíte níže.

        $vnet = Get-AzureRmVirtualNetwork -ResourceGroupName TestRG -Name TestVNet
    
    >[AZURE.TIP] Kroky 3 a 4 můžete spojit spuštěním příkazu **$vnet = New-AzureRmVirtualNetwork -ResourceGroupName TestRG -Name TestVNet -AddressPrefix 192.168.0.0/16 -Location centralus**.

5. Přidejte do nové proměnné sítě VNet podsíť, jak vidíte níže.

        Add-AzureRmVirtualNetworkSubnetConfig -Name FrontEnd `
            -VirtualNetwork $vnet -AddressPrefix 192.168.1.0/24
        
    Očekávaný výstup:

        Name              : TestVNet
        ResourceGroupName : TestRG
        Location          : centralus
        Id                : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet
        Etag              : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
        ProvisioningState : Succeeded
        Tags              :
        AddressSpace      : {
                              "AddressPrefixes": [
                                "192.168.0.0/16"
                              ]
                            }
        DhcpOptions       : {
                              "DnsServers": null
                            }
        NetworkInterfaces : null
        Subnets           : [
                              {
                                "Name": "FrontEnd",
                                "Etag": null,
                                "Id": null,
                                "AddressPrefix": "192.168.1.0/24",
                                "IpConfigurations": null,
                                "NetworkSecurityGroup": null,
                                "RouteTable": null,
                                "ProvisioningState": null
                              }
                            ]

6. Výše popsaný krok 5 opakujte pro každou podsíť, kterou chcete vytvořit. Následující příkaz vytvoří podsíť *BackEnd* pro náš scénář.

        Add-AzureRmVirtualNetworkSubnetConfig -Name BackEnd `
            -VirtualNetwork $vnet -AddressPrefix 192.168.2.0/24

7. I když vytvoříte podsítě, zatím existují jenom v místní proměnné sloužící k načtení sítě VNet, kterou vytvoříte ve výše popsaném kroku 4. Pokud chcete uložit změny do Azure, spusťte rutinu **Set-AzureRmVirtualNetwork**, jak vidíte níže.

        Set-AzureRmVirtualNetwork -VirtualNetwork $vnet 
        
    Očekávaný výstup:

        Name              : TestVNet
        ResourceGroupName : TestRG
        Location          : centralus
        Id                : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet
        Etag              : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
        ProvisioningState : Succeeded
        Tags              :
        AddressSpace      : {
                              "AddressPrefixes": [
                                "192.168.0.0/16"
                              ]
                            }
        DhcpOptions       : {
                              "DnsServers": []
                            }
        NetworkInterfaces : null
        Subnets           : [
                              {
                                "Name": "FrontEnd",
                                "Etag": "W/\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\"",
                                "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd",
                                "AddressPrefix": "192.168.1.0/24",
                                "IpConfigurations": [],
                                "NetworkSecurityGroup": null,
                                "RouteTable": null,
                                "ProvisioningState": "Succeeded"
                              },
                              {
                                "Name": "BackEnd",
                                "Etag": "W/\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\"",
                                "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/BackEnd",
                                "AddressPrefix": "192.168.2.0/24",
                                "IpConfigurations": [],
                                "NetworkSecurityGroup": null,
                                "RouteTable": null,
                                "ProvisioningState": "Succeeded"
                              }
                            ]



<!--HONumber=Jun16_HO2-->


