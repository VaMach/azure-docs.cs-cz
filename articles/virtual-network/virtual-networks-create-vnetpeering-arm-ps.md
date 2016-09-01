<properties
   pageTitle="Vytvoření partnerského vztahu sítě VNet s použitím rutin prostředí Powershell | Microsoft Azure"
   description="Naučte se vytvořit virtuální síť pomocí webu Azure Portal v nástroji Resurce Manager."
   services="virtual-network"
   documentationCenter=""
   authors="narayanannamalai"
   manager="jefco"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/02/2016"
   ms.author="narayanannamalai"/>

# Vytvoření partnerského vztahu sítě VNet s použitím rutin prostředí Powershell

[AZURE.INCLUDE [virtual-networks-create-vnet-selectors-arm-include](../../includes/virtual-networks-create-vnetpeering-selectors-arm-include.md)]

[AZURE.INCLUDE [virtual-networks-create-vnet-intro](../../includes/virtual-networks-create-vnetpeering-intro-include.md)]

[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-basic-include](../../includes/virtual-networks-create-vnetpeering-scenario-basic-include.md)]

Pokud chcete vytvořit partnerský vztah sítě VNet pomocí prostředí PowerShell, postupujte následujícím způsobem:

1. Pokud jste prostředí Azure PowerShell nikdy nepoužívali, přejděte na téma [Instalace a konfigurace prostředí Azure PowerShell](../powershell-install-configure.md) a proveďte všechny pokyny, abyste se mohli přihlásit k Azure a vybrat své předplatné.

        Note: PowerShell cmdlet for managing VNet peering is shipped with [Azure PowerShell 1.6.](http://www.powershellgallery.com/packages/Azure/1.6.0)

2. Čtení objektů virtuální sítě:

        $vnet1 = Get-AzureRmVirtualNetwork -ResourceGroupName vnet101 -Name vnet1
        $vnet2 = Get-AzureRmVirtualNetwork -ResourceGroupName vnet101 -Name vnet2
    
3. K vytvoření partnerského vztahu sítě VNet musíte vytvořit dvě propojení (jedno pro každý směr). Následující krok vytvoří propojení partnerského vztahu sítě VNet pro síť VNet1 nejprve k síti VNet2:

        Add-AzureRmVirtualNetworkPeering -name LinkToVNet2 -VirtualNetwork $vnet1 -RemoteVirtualNetworkId $vnet2.id 

        Output shows:

        Name            : LinkToVNet2
        Id: /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/vnet101/providers/Microsoft.Network/virtualNetworks/vnet1/virtualNetworkPeerings/LinkToVNet2
        Etag            : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
        ResourceGroupName   : vnet101
        VirtualNetworkName  : vnet1
        ProvisioningState       : Succeeded
        RemoteVirtualNetwork    : {
                                            "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/vnet101/providers/Microsoft.Network/virtualNetworks/vnet2"
                                        }
        AllowVirtualNetworkAccess   : True
        AllowForwardedTraffic   : False
        AllowGatewayTransit : False
        UseRemoteGateways   : False
        RemoteGateways      : null
        RemoteVirtualNetworkAddressSpace : null

4. Tento krok vytvoří propojení partnerského vztahu sítě VNet pro síť VNet2 k síti VNet1:

        Add-AzureRmVirtualNetworkPeering -name LinkToVNet1 -VirtualNetwork $vnet2 -RemoteVirtualNetworkId $vnet1.id 

        Output shows:

        Name            : LinkToVNet1
        Id              : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/vnet101/providers/Microsoft.Network/virtualNetworks/vnet2/virtualNetworkPeerings/LinkToVNet1
        Etag            : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
        ResourceGroupName   : vnet101
        VirtualNetworkName  : vnet2
        ProvisioningState       : Succeeded
        RemoteVirtualNetwork    : {
                                            "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/vnet101/providers/Microsoft.Network/virtualNetworks/vnet1"
                                        }
        AllowVirtualNetworkAccess   : True
        AllowForwardedTraffic   : False
        AllowGatewayTransit : False
        UseRemoteGateways   : False
        RemoteGateways      : null
        RemoteVirtualNetworkAddressSpace : null

5. Po vytvoření propojení partnerského stavu sítě VNet se zobrazí následující stav propojení:

        Get-AzureRmVirtualNetworkPeering -VirtualNetworkName vnet1 -ResourceGroupName vnet101 -Name linktovnet2

        Output shows:

        Name            : LinkToVNet2
        Id              : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/vnet101/providers/Microsoft.Network/virtualNetworks/vnet1/virtualNetworkPeerings/LinkToVNet2
        Etag            : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
        ResourceGroupName   : vnet101
        VirtualNetworkName  : vnet1
        ProvisioningState       : Succeeded
        RemoteVirtualNetwork    : {
                                             "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/vnet101/providers/Microsoft.Network/virtualNetworks/vnet2"
                                        }
        AllowVirtualNetworkAccess   : True
        AllowForwardedTraffic            : False
        AllowGatewayTransit              : False
        UseRemoteGateways                : False
        RemoteGateways                   : null
        RemoteVirtualNetworkAddressSpace : null

    Pro partnerský vztah sítě VNet lze konfigurovat několik vlastností:

  	|Možnost|Popis|Výchozí|
  	|:-----|:----------|:------|
  	|AllowVirtualNetworkAccess|Určuje, zda adresní prostor partnerské sítě VNet má být zahrnut jako součást značky Virtual_network|Ano|
  	|AllowForwardedTraffic|Určuje, zda komunikace nepocházející z partnerské sítě VNet bude přijata nebo vyřazena|Ne|
  	|AllowGatewayTransit|Umožňuje určit, zda může partnerská síť VNet používat bránu vaší sítě VNet|Ne|
  	|UseRemoteGateways|Určuje, zda se má používat brána partnerské sítě VNet. Partnerská síť VNet musí mít konfigurovanou bránu a musí být vybrána možnost AllowGatewayTransit. Tuto možnost nelze použít, pokud máte nakonfigurovanou bránu|Ne|

    Každé propojení v partnerském vztahu sítě VNet má definovánu uvedenou sadu vlastností. Můžete například nastavit možnost AllowVirtualNetworkAccess na hodnotu True pro propojení partnerského vztahu sítě VNet ze sítě VNet1 do sítě VNet2 a na hodnotu False propojení partnerského vztahu sítě VNet v opačném směru.

        $LinktoVNet2 = Get-AzureRmVirtualNetworkPeering -VirtualNetworkName vnet1 -ResourceGroupName vnet101 -Name LinkToVNet2
        $LinktoVNet2.AllowForwardedTraffic = $true
        Set-AzureRmVirtualNetworkPeering -VirtualNetworkPeering $LinktoVNet2

        You can run Get-AzureRmVirtualNetworkPeering to double check the property value after the change.  From the output, you can see AllowForwardedTraffic changes set to True after running the above cmdlets. 

        Name            : LinkToVNet2
        Id          : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/vnet101/providers/Microsoft.Network/virtualNetworks/vnet1/virtualNetworkPeerings/LinkToVNet2
        Etag            : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
        ResourceGroupName   : vnet101
        VirtualNetworkName  : vnet1
        ProvisioningState   : Succeeded
        RemoteVirtualNetwork    : {
                                            "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/vnet101/providers/Microsoft.Network/virtualNetworks/vnet2"
                                        }
        AllowVirtualNetworkAccess   : True
        AllowForwardedTraffic       : True
        AllowGatewayTransit     : False
        UseRemoteGateways       : False
        RemoteGateways      : null
        RemoteVirtualNetworkAddressSpace : null

    Po vytvoření partnerského vztahu v tomto scénáři byste měli být schopni iniciovat připojení ze kteréhokoli virtuálního počítače ke kterémukoli virtuálnímu počítači obou sítí VNet. Ve výchozím nastavení je pro možnost AllowVirtualNetworkAccess nastavena hodnota True a partnerský vztah sítě VNet zřídí správné seznamy řízení přístupu, které budou umožňovat komunikaci mezi sítěmi VNet. I poté můžete používat skupiny zabezpečení sítě (NSG) k blokování možnosti připojení mezi konkrétními podsítěmi nebo virtuálními počítači. Tímto způsobem lze využívat podrobně odstupňované řízení přístupu mezi dvěma virtuálními sítěmi.  Další informace o vytváření pravidel skupin zabezpečení sítě najdete v tomto [článku](virtual-networks-create-nsg-arm-ps.md).

[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-crosssub-include](../../includes/virtual-networks-create-vnetpeering-scenario-crosssub-include.md)]

Chcete-li s použitím prostředí PowerShell vytvořit partnerský vztah sítě VNet zahrnující několik předplatných, postupujte podle následujících kroků:

1. Přihlaste se k Azure s použitím účtu oprávněného uživatele A pro předplatné B a spusťte následující rutinu:

        New-AzureRmRoleAssignment -SignInName <UserB ID> -RoleDefinitionName "Network Contributor" -Scope /subscriptions/<Subscription-A-ID>/resourceGroups/<ResourceGroupName>/providers/Microsoft.Network/VirtualNetwork/VNet5

        This is not a requirement, peering can be established even if users individually raise peering requests for thier respective Vnets as long as the requests match. Adding a privileged user of the other VNet as a user in the local VNet makes it easier to do the setup. 

2. Přihlaste se k Azure s použitím účtu oprávněného uživatele B pro předplatné B a spusťte následující rutinu:

        New-AzureRmRoleAssignment -SignInName <UserA ID> -RoleDefinitionName "Network Contributor" -Scope /subscriptions/<Subscription-B-ID>/resourceGroups/<ResourceGroupName>/providers/Microsoft.Network/VirtualNetwork/VNet3

3. V rámci relace přihlášení uživatele A spusťte následující rutinu:

        $vnet3 = Get-AzureRmVirtualNetwork -ResourceGroupName hr-vnets -Name vnet3

        Add-AzureRmVirtualNetworkPeering -name LinkToVNet5 -VirtualNetwork $vnet3 -RemoteVirtualNetworkId "/subscriptions/<Subscriptoin-B-Id>/resourceGroups/<ResourceGroupName>/providers/Microsoft.Network/virtualNetworks/VNet5" -BlockVirtualNetworkAccess

4. V rámci relace přihlášení uživatele B spusťte následující rutinu:

        $vnet5 = Get-AzureRmVirtualNetwork -ResourceGroupName vendor-vnets -Name vnet5

        Add-AzureRmVirtualNetworkPeering -name LinkToVNet3 -VirtualNetwork $vnet5 -RemoteVirtualNetworkId "/subscriptions/<Subscriptoin-A-Id>/resourceGroups/<ResourceGroupName>/providers/Microsoft.Network/virtualNetworks/VNet3" -BlockVirtualNetworkAccess

5. Po vytvoření partnerského vztahu by všechny virtuální počítače v síti VNet3 měly být schopny komunikovat s kterýmkoli virtuálním počítačem v síti VNet5.

[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-transit-include](../../includes/virtual-networks-create-vnetpeering-scenario-transit-include.md)]

1. V tomto scénáři můžete spustit rutiny prostředí PowerShell a vytvořit partnerský vztah sítě VNet.  Je třeba nastavit vlastnost AllowForwardedTraffic na hodnotu True a propojit sítě VNET1 a HubVnet, aby byl povolen příchozí provoz z lokalit mimo adresní prostor sítě VNet v partnerském vztahu. 

        $hubVNet = Get-AzureRmVirtualNetwork -ResourceGroupName vnet101 -Name HubVNet
        $vnet1 = Get-AzureRmVirtualNetwork -ResourceGroupName vnet101 -Name vnet1

        Add-AzureRmVirtualNetworkPeering -name LinkToHub -VirtualNetwork $vnet1 -RemoteVirtualNetworkId $HubVNet.id -AllowForwardedTraffic

        Add-AzureRmVirtualNetworkPeering -name LinkToVNet1 -VirtualNetwork $HubVNet -RemoteVirtualNetworkId $vnet1.id

2. Po vytvoření partnerského vztahu můžete postupovat podle tohoto [článku](virtual-network-create-udr-arm-ps.md) a definovat uživatelem definovanou trasu (UDR) tak, aby provoz sítě VNet1 přesměrovávala prostřednictvím virtuální zařízení a využívaly se jeho funkce. Když zadáváte adresu dalšího směrování v rámci trasy, nastavte ji jako IP adresu virtuálního zařízení v partnerské síti VNet HubVNet. Zde je příklad:

        $route = New-AzureRmRouteConfig -Name TestNVA -AddressPrefix 10.3.0.0/16 -NextHopType VirtualAppliance -NextHopIpAddress 192.0.1.5

        $routeTable = New-AzureRmRouteTable -ResourceGroupName VNet101 -Location brazilsouth -Name TestRT -Route $route

        $vnet1 = Get-AzureRmVirtualNetwork -ResourceGroupName VNet101 -Name VNet1

        Set-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet1 -Name subnet-1 -AddressPrefix 10.1.1.0/24 -RouteTable $routeTable

        Set-AzureRmVirtualNetwork -VirtualNetwork $vnet1

## Odebrání partnerského vztahu sítě VNet

1.  Chcete-li odebrat partnerský vztah sítě VNet, je třeba spustit následující rutinu: 

        Remove-AzureRmVirtualNetworkPeering  
    
        remove both links, as shown below:

        Remove-AzureRmVirtualNetworkPeering -ResourceGroupName vnet101 -VirtualNetworkName vnet1 -Name linktovnet2 
        Remove-AzureRmVirtualNetworkPeering -ResourceGroupName vnet101 -VirtualNetworkName vnet1 -Name linktovnet2 

2. Po odebrání jednoho propojení v rámci partnerského vztahu sítě VNET se stav partnerského propojení změní na stav odpojení. V tomto stavu nelze propojení vytvořit znovu, dokud se stav partnerského propojení nezmění na stav Iniciováno. Před opětným vytvářením partnerského vztahu sítě VNet doporučujeme odebrat obě propojení. 



<!---HONumber=Aug16_HO4-->


