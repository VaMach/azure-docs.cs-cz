<properties
   pageTitle="Vytvoření partnerského vztahu virtuálních sítí s použitím rutin prostředí Powershell | Microsoft Azure"
   description="Naučte se vytvořit virtuální síť pomocí webu Azure Portal v nástroji Resurce Manager."
   services="virtual-network"
   documentationCenter=""
   authors="NarayanAnnamalai"
   manager="jefco"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/14/2016"
   ms.author="narayanannamalai; annahar"/>


# Vytvoření partnerského vztahu virtuálních sítí s použitím rutin prostředí Powershell

[AZURE.INCLUDE [virtual-networks-create-vnet-selectors-arm-include](../../includes/virtual-networks-create-vnetpeering-selectors-arm-include.md)]

[AZURE.INCLUDE [virtual-networks-create-vnet-intro](../../includes/virtual-networks-create-vnetpeering-intro-include.md)]

[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-basic-include](../../includes/virtual-networks-create-vnetpeering-scenario-basic-include.md)]

Pokud chcete vytvořit partnerský vztah virtuálních sítí pomocí prostředí PowerShell, postupujte následujícím způsobem:

1. Pokud jste prostředí Azure PowerShell nikdy nepoužívali, přejděte na téma [Instalace a konfigurace prostředí Azure PowerShell](../powershell-install-configure.md) a proveďte všechny pokyny, abyste se mohli přihlásit k Azure a vybrat své předplatné.

> [AZURE.NOTE] Rutina prostředí PowerShell pro správu partnerských vztahů virtuálních sítí je dodávána s prostředím [Azure PowerShell 1.6.](http://www.powershellgallery.com/packages/Azure/1.6.0)

2. Čtení objektů virtuální sítě:

        $vnet1 = Get-AzureRmVirtualNetwork -ResourceGroupName vnet101 -Name vnet1
        $vnet2 = Get-AzureRmVirtualNetwork -ResourceGroupName vnet101 -Name vnet2

3. K vytvoření partnerského vztahu virtuálních sítí musíte vytvořit dvě propojení (jedno pro každý směr). Následující krok vytvoří propojení partnerského vztahu virtuálních sítí pro síť VNet1 nejprve k síti VNet2:

        Add-AzureRmVirtualNetworkPeering -name LinkToVNet2 -VirtualNetwork $vnet1 -RemoteVirtualNetworkId $vnet2.id

    Výstup ukazuje:

        Name            : LinkToVNet2
        Id: /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/vnet101/providers/Microsoft.Network/virtualNetworks/vnet1/virtualNetworkPeerings/LinkToVNet2
        Etag            : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
        ResourceGroupName   : vnet101
        VirtualNetworkName  : vnet1
        PeeringState        : Initiated
        ProvisioningState   : Succeeded
        RemoteVirtualNetwork    : {
                                            "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/vnet101/providers/Microsoft.Network/virtualNetworks/vnet2"
                                        }
        AllowVirtualNetworkAccess   : True
        AllowForwardedTraffic   : False
        AllowGatewayTransit : False
        UseRemoteGateways   : False
        RemoteGateways      : null
        RemoteVirtualNetworkAddressSpace : null

4. Tento krok vytvoří propojení partnerského vztahu virtuálních sítí pro síť VNet2 k síti VNet1:

        Add-AzureRmVirtualNetworkPeering -name LinkToVNet1 -VirtualNetwork $vnet2 -RemoteVirtualNetworkId $vnet1.id

    Výstup ukazuje:

        Name            : LinkToVNet1
        Id              : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/vnet101/providers/Microsoft.Network/virtualNetworks/vnet2/virtualNetworkPeerings/LinkToVNet1
        Etag            : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
        ResourceGroupName   : vnet101
        VirtualNetworkName  : vnet2
        PeeringState        : Connected
        ProvisioningState   : Succeeded
        RemoteVirtualNetwork    : {
                                            "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/vnet101/providers/Microsoft.Network/virtualNetworks/vnet1"
                                        }
        AllowVirtualNetworkAccess   : True
        AllowForwardedTraffic   : False
        AllowGatewayTransit : False
        UseRemoteGateways   : False
        RemoteGateways      : null
        RemoteVirtualNetworkAddressSpace : null

5. Po vytvoření propojení partnerského stavu virtuálních sítí se zobrazí následující stav propojení:

        Get-AzureRmVirtualNetworkPeering -VirtualNetworkName vnet1 -ResourceGroupName vnet101 -Name linktovnet2

    Výstup ukazuje:

        Name            : LinkToVNet2
        Id              : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/vnet101/providers/Microsoft.Network/virtualNetworks/vnet1/virtualNetworkPeerings/LinkToVNet2
        Etag            : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
        ResourceGroupName   : vnet101
        VirtualNetworkName  : vnet1
        PeeringState        : Connected
        ProvisioningState   : Succeeded
        RemoteVirtualNetwork    : {
                                             "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/vnet101/providers/Microsoft.Network/virtualNetworks/vnet2"
                                        }
        AllowVirtualNetworkAccess   : True
        AllowForwardedTraffic            : False
        AllowGatewayTransit              : False
        UseRemoteGateways                : False
        RemoteGateways                   : null
        RemoteVirtualNetworkAddressSpace : null

    Pro partnerský vztah virtuálních sítí lze konfigurovat několik vlastností:

  	|Možnost|Popis|Výchozí|
  	|:-----|:----------|:------|
  	|AllowVirtualNetworkAccess|Určuje, jestli má být adresní prostor partnerské virtuální sítě má zahrnutý jako součást značky Virtual_network.|Ano|
  	|AllowForwardedTraffic|Určuje, jestli bude přijata nebo zahozena komunikace, která nemá původ přímo z partnerské virtuální sítě.|Ne|
  	|AllowGatewayTransit|Umožňuje určit, jestli může partnerská virtuální síť používat bránu vaší virtuální sítě.|Ne|
  	|UseRemoteGateways|Určuje, jestli se má používat brána partnerské virtuální sítě. Partnerská virtuální síť musí mít nakonfigurovanou bránu a dále musí být vybraná možnost AllowGatewayTransit. Pokud vaše síť již má nakonfigurovanou bránu, tuto možnost nelze použít.|Ne|

    Každé propojení v partnerském vztahu virtuálních sítě má sadu výše uvedených vlastností. Můžete například nastavit možnost AllowVirtualNetworkAccess na hodnotu True pro propojení partnerského vztahu virtuálních sítí ze sítě VNet1 do sítě VNet2 a na hodnotu False propojení partnerského vztahu virtuálních sítí v opačném směru.

        $LinktoVNet2 = Get-AzureRmVirtualNetworkPeering -VirtualNetworkName vnet1 -ResourceGroupName vnet101 -Name LinkToVNet2
        $LinktoVNet2.AllowForwardedTraffic = $true
        Set-AzureRmVirtualNetworkPeering -VirtualNetworkPeering $LinktoVNet2

    Můžete spustit příkaz Get-AzureRmVirtualNetworkPeering a ještě hodnotu vlastnosti po změně ověřit.  Z výstupu příkazu by mělo být vidět, že se hodnota AllowForwardedTraffic po spuštění výše uvedených rutin změnila na True.

        Name            : LinkToVNet2
        Id          : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/vnet101/providers/Microsoft.Network/virtualNetworks/vnet1/virtualNetworkPeerings/LinkToVNet2
        Etag            : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
        ResourceGroupName   : vnet101
        VirtualNetworkName  : vnet1
        PeeringState        : Connected
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

    Po vytvoření partnerského vztahu v tomto scénáři byste měli být schopni iniciovat připojení ze kteréhokoli virtuálního počítače ke kterémukoli virtuálnímu počítači obou virtuálních sítí. Ve výchozím nastavení je pro možnost AllowVirtualNetworkAccess nastavena hodnota True a partnerský vztah virtuálních sítí zřídí správné seznamy řízení přístupu, které budou umožňovat komunikaci mezi virtuálními sítěmi. I poté můžete používat skupiny zabezpečení sítě (NSG) k blokování možnosti připojení mezi konkrétními podsítěmi nebo virtuálními počítači. Tímto způsobem lze využívat podrobně odstupňované řízení přístupu mezi dvěma virtuálními sítěmi.  Další informace o vytváření pravidel skupin zabezpečení sítě najdete v tomto [článku](virtual-networks-create-nsg-arm-ps.md).

[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-crosssub-include](../../includes/virtual-networks-create-vnetpeering-scenario-crosssub-include.md)]

Pokud chcete pomocí prostředí PowerShell vytvořit partnerský vztah virtuálních sítí zahrnující několik předplatných, postupujte podle následujících kroků:

1. Přihlaste se k Azure s použitím účtu oprávněného uživatele A pro předplatné B a spusťte následující rutinu:

        New-AzureRmRoleAssignment -SignInName <UserB ID> -RoleDefinitionName "Network Contributor" -Scope /subscriptions/<Subscription-A-ID>/resourceGroups/<ResourceGroupName>/providers/Microsoft.Network/VirtualNetworks/VNet5

    Toto není povinné. Partnerský vztah lze navázat i v případě, že uživatelé vytvoří požadavky na partnerský vztah pro své virtuální sítě jednotlivě, pokud se jejich požadavky shodují. Když přidáte privilegovaného uživatele z druhé virtuální sítě jako uživatele místní virtuální sítě, budete mít nastavení snazší.

2. Přihlaste se k Azure s použitím účtu oprávněného uživatele B pro předplatné B a spusťte následující rutinu:

        New-AzureRmRoleAssignment -SignInName <UserA ID> -RoleDefinitionName "Network Contributor" -Scope /subscriptions/<Subscription-B-ID>/resourceGroups/<ResourceGroupName>/providers/Microsoft.Network/VirtualNetworks/VNet3

3. V rámci relace přihlášení uživatele A spusťte následující rutinu:

        $vnet3 = Get-AzureRmVirtualNetwork -ResourceGroupName hr-vnets -Name vnet3

        Add-AzureRmVirtualNetworkPeering -name LinkToVNet5 -VirtualNetwork $vnet3 -RemoteVirtualNetworkId "/subscriptions/<Subscriptoin-B-Id>/resourceGroups/<ResourceGroupName>/providers/Microsoft.Network/virtualNetworks/VNet5" -BlockVirtualNetworkAccess

4. V rámci relace přihlášení uživatele B spusťte následující rutinu:

        $vnet5 = Get-AzureRmVirtualNetwork -ResourceGroupName vendor-vnets -Name vnet5

        Add-AzureRmVirtualNetworkPeering -name LinkToVNet3 -VirtualNetwork $vnet5 -RemoteVirtualNetworkId "/subscriptions/<Subscriptoin-A-Id>/resourceGroups/<ResourceGroupName>/providers/Microsoft.Network/virtualNetworks/VNet3" -BlockVirtualNetworkAccess

5. Po vytvoření partnerského vztahu by všechny virtuální počítače v síti VNet3 měly být schopny komunikovat s kterýmkoli virtuálním počítačem v síti VNet5.

[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-transit-include](../../includes/virtual-networks-create-vnetpeering-scenario-transit-include.md)]

1. V tomto scénáři můžete spustit rutiny prostředí PowerShell a vytvořit partnerský vztah virtuálních sítí.  Je třeba nastavit vlastnost AllowForwardedTraffic na hodnotu True a propojit sítě VNET1 a HubVnet, aby byl povolen příchozí provoz z lokalit mimo adresní prostor virtuálních sítí v partnerském vztahu.

        $hubVNet = Get-AzureRmVirtualNetwork -ResourceGroupName vnet101 -Name HubVNet
        $vnet1 = Get-AzureRmVirtualNetwork -ResourceGroupName vnet101 -Name vnet1

        Add-AzureRmVirtualNetworkPeering -name LinkToHub -VirtualNetwork $vnet1 -RemoteVirtualNetworkId $HubVNet.id -AllowForwardedTraffic

        Add-AzureRmVirtualNetworkPeering -name LinkToVNet1 -VirtualNetwork $HubVNet -RemoteVirtualNetworkId $vnet1.id

2. Po vytvoření partnerského vztahu můžete postupovat podle tohoto [článku](virtual-network-create-udr-arm-ps.md) a definovat uživatelem definovanou trasu (UDR) tak, aby provoz sítě VNet1 přesměrovávala prostřednictvím virtuální zařízení a využívaly se jeho funkce. Když zadáváte adresu dalšího směrování v rámci trasy, nastavte ji jako IP adresu virtuálního zařízení v partnerské virtuální síti HubVNet. Zde je příklad:

        $route = New-AzureRmRouteConfig -Name TestNVA -AddressPrefix 10.3.0.0/16 -NextHopType VirtualAppliance -NextHopIpAddress 192.0.1.5

        $routeTable = New-AzureRmRouteTable -ResourceGroupName VNet101 -Location brazilsouth -Name TestRT -Route $route

        $vnet1 = Get-AzureRmVirtualNetwork -ResourceGroupName VNet101 -Name VNet1

        Set-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet1 -Name subnet-1 -AddressPrefix 10.1.1.0/24 -RouteTable $routeTable

        Set-AzureRmVirtualNetwork -VirtualNetwork $vnet1

[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-asmtoarm-include](../../includes/virtual-networks-create-vnetpeering-scenario-asmtoarm-include.md)]

Partnerský vztah mezi klasickou virtuální sítí a virtuální sítí Azure Resource Manageru vytvoříte v PowerShellu podle následujícího postupu:

1. Takhle načtěte objekt virtuální sítě **VNET1** (virtuální síť Azure Resource Manageru):      $vnet1 = Get-AzureRmVirtualNetwork -ResourceGroupName vnet101 -Name vnet1

2. K vytvoření partnerského vztahu v tomto scénáři je potřeba jediné propojení, a to propojení ze sítě **VNET1** do **VNET2**. V tomto kroku je nutné, abyste znali ID prostředku klasické virtuální sítě. Formát ID skupiny prostředků vypadá takhle: /subscriptions/SubscriptionID/resourceGroups/ResourceGroupName/providers/Microsoft.ClassicNetwork/virtualNetworks/VirtualNetworkName

    SubscriptionID, ResourceGroupName a VirtualNetworkName nahraďte příslušnými názvy.

    To můžete udělat takhle:

        Add-AzureRmVirtualNetworkPeering -name LinkToVNet2 -VirtualNetwork $vnet1 -RemoteVirtualNetworkId /subscriptions/xxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxx/resourceGroups/MyResourceGroup/providers/Microsoft.ClassicNetwork/virtualNetworks/VNET2

3. Jakmile se partnerské propojení vytvoří, můžete jeho stav zobrazit jako na výstupu níže:

        Name                             : LinkToVNet2
        Id                               : /subscriptions/xxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxx/resourceGroups/MyResourceGroup/providers/Microsoft.Network/virtualNetworks/VNET1/virtualNetworkPeerings/LinkToVNet2
        Etag                             : W/"acecbd0f-766c-46be-aa7e-d03e41c46b16"
        ResourceGroupName                : MyResourceGroup
        VirtualNetworkName               : VNET1
        PeeringState                     : Connected
        ProvisioningState                : Succeeded
        RemoteVirtualNetwork             : {
                                         "Id": "/subscriptions/xxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxx/resourceGroups/MyResourceGroup/providers/Microsoft.ClassicNetwork/virtualNetworks/VNET2"
                                       }
        AllowVirtualNetworkAccess        : True
        AllowForwardedTraffic            : False
        AllowGatewayTransit              : False
        UseRemoteGateways                : False
        RemoteGateways                   : null
        RemoteVirtualNetworkAddressSpace : null

## Odebrání partnerského vztahu virtuální sítě

1.  Pokud chcete odebrat partnerský vztah virtuální sítě, je třeba spustit následující rutinu:

        Remove-AzureRmVirtualNetworkPeering  

        remove both links, as shown below:

        Remove-AzureRmVirtualNetworkPeering -ResourceGroupName vnet101 -VirtualNetworkName vnet1 -Name linktovnet2
        Remove-AzureRmVirtualNetworkPeering -ResourceGroupName vnet101 -VirtualNetworkName vnet1 -Name linktovnet2

2. Po odebrání jednoho propojení v rámci partnerského vztahu virtuální sítě se stav partnerského propojení změní na stav odpojení. V tomto stavu nelze propojení vytvořit znovu, dokud se stav partnerského propojení nezmění na stav Iniciováno. Před opětným vytvářením partnerského vztahu virtuální sítě doporučujeme odebrat obě propojení.



<!--HONumber=Oct16_HO1-->


