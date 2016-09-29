<properties
   pageTitle="Vytvoření partnerského vztahu virtuálních sítí pomocí šablon Resource Manageru | Microsoft Azure"
   description="Naučte se vytvořit partnerský vztah virtuálních sítí pomocí šablon v Resource Manageru."
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
   ms.date="09/14/2016"
   ms.author="narayanannamalai;annahar"/>


# Vytvoření partnerského vztahu virtuálních sítí pomocí šablon Resource Manageru

[AZURE.INCLUDE [virtual-networks-create-vnet-selectors-arm-include](../../includes/virtual-networks-create-vnetpeering-selectors-arm-include.md)]

[AZURE.INCLUDE [virtual-networks-create-vnet-intro](../../includes/virtual-networks-create-vnetpeering-intro-include.md)]

[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-basic-include](../../includes/virtual-networks-create-vnetpeering-scenario-basic-include.md)]

Pokud chcete vytvořit partnerský vztah virtuálních sítí pomocí šablon Resource Manageru, postupujte následovně:

1. Pokud jste prostředí Azure PowerShell nikdy nepoužívali, přejděte na téma [Instalace a konfigurace prostředí Azure PowerShell](../powershell-install-configure.md) a proveďte všechny pokyny, abyste se mohli přihlásit k Azure a vybrat své předplatné.

    > [AZURE.NOTE] Rutina prostředí PowerShell pro správu partnerských vztahů virtuálních sítí je dodávána s prostředím [Azure PowerShell 1.6.](http://www.powershellgallery.com/packages/Azure/1.6.0)

2. V následujícím textu je uvedena definice propojení partnerského vztahu virtuálních sítí ze sítě VNet1 do sítě VNet2 na základě výše uvedeného scénáře. Zkopírujte následující obsah a uložte ho do souboru s názvem VNetPeeringVNet1.json.

        {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
        },
        "variables": {
        },
        "resources": [
            {
            "apiVersion": "2016-06-01",
            "type": "Microsoft.Network/virtualNetworks/virtualNetworkPeerings",
            "name": "VNet1/LinkToVNet2",
            "location": "[resourceGroup().location]",
            "properties": {
            "allowVirtualNetworkAccess": true,
            "allowForwardedTraffic": false,
            "allowGatewayTransit": false,
            "useRemoteGateways": false,
                "remoteVirtualNetwork": {
                "id": "[resourceId('Microsoft.Network/virtualNetworks', 'vnet2')]"       
        }
            }
            }
        ]
        }

3. V následující sekci je uvedena definice propojení partnerského vztahu virtuálních sítí ze sítě VNet2 do sítě VNet1 na základě výše uvedeného scénáře.  Zkopírujte následující obsah a uložte ho do souboru s názvem VNetPeeringVNet2.json.

        {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
        },
        "variables": {
        },
        "resources": [
            {
            "apiVersion": "2016-06-01",
            "type": "Microsoft.Network/virtualNetworks/virtualNetworkPeerings",
            "name": "VNet2/LinkToVNet1",
            "location": "[resourceGroup().location]",
            "properties": {
            "allowVirtualNetworkAccess": true,
            "allowForwardedTraffic": false,
            "allowGatewayTransit": false,
            "useRemoteGateways": false,
                "remoteVirtualNetwork": {
                "id": "[resourceId('Microsoft.Network/virtualNetworks', 'vnet1')]"       
                }
            }
            }
        ]
        }

    Jak vidíte v šabloně výše, existuje několik konfigurovatelných vlastností pro partnerský vztah virtuálních sítí:

  	|Možnost|Popis|Výchozí|
  	|:-----|:----------|:------|
  	|AllowVirtualNetworkAccess|Určuje, jestli adresní prostor partnerské virtuální sítě má nebo nemá být zahrnut jako součást značky virtual_network.|Ano|
  	|AllowForwardedTraffic|Určuje, jestli bude přijata nebo zahozena komunikace, která nemá původ přímo z partnerské virtuální sítě.|Ne|
  	|AllowGatewayTransit|Umožňuje určit, jestli může partnerská virtuální síť používat bránu vaší virtuální sítě.|Ne|
  	|UseRemoteGateways|Určuje, jestli se má používat brána partnerské virtuální sítě. Partnerská virtuální síť musí mít nakonfigurovanou bránu a dále musí být vybraná možnost AllowGatewayTransit. Pokud vaše síť již má nakonfigurovanou bránu, tuto možnost nelze použít.|Ne|

    Každé propojení v partnerském vztahu virtuálních sítě má sadu výše uvedených vlastností. Můžete například vlastnost AllowVirtualNetworkAccess nastavit na hodnotu True pro propojení partnerského vztahu virtuálních sítí ze sítě VNet1 do sítě VNet2 a na hodnotu False pro propojení v opačném směru.


4. Pokud chcete nasadit soubor šablony, můžete spustit rutinu New-AzureRmResourceGroupDeployment, s jejíž pomocí lze vytvořit nebo aktualizovat nasazení. Další informace o použití šablon Resource Manageru najdete v tomto [článku](../resource-group-template-deploy.md).

        New-AzureRmResourceGroupDeployment -ResourceGroupName <resource group name> -TemplateFile <template file path> -DeploymentDebugLogLevel all

    > [AZURE.NOTE] Nahraďte odpovídajícím způsobem název skupiny prostředků a souboru šablony.

    Níže je příklad založený na výše uvedeném scénáři:

        New-AzureRmResourceGroupDeployment -ResourceGroupName VNet101 -TemplateFile .\VNetPeeringVNet1.json -DeploymentDebugLogLevel all

    Výstup ukazuje:

        DeploymentName      : VNetPeeringVNet1
        ResourceGroupName   : VNet101
        ProvisioningState       : Succeeded
        Timestamp           : 7/26/2016 9:05:03 AM
        Mode            : Incremental
        TemplateLink        :
        Parameters          :
        Outputs         :
        DeploymentDebugLogLevel : RequestContent, ResponseContent

        New-AzureRmResourceGroupDeployment -ResourceGroupName VNet101 -TemplateFile .\VNetPeeringVNet2.json -DeploymentDebugLogLevel all

    Výstup ukazuje:

        DeploymentName      : VNetPeeringVNet2
        ResourceGroupName   : VNet101
        ProvisioningState       : Succeeded
        Timestamp           : 7/26/2016 9:07:22 AM
        Mode            : Incremental
        TemplateLink        :
        Parameters          :
        Outputs         :
        DeploymentDebugLogLevel : RequestContent, ResponseContent

5. Po dokončení nasazení můžete spuštěním následující rutiny zobrazit stav partnerského vztahu:

        Get-AzureRmVirtualNetworkPeering -VirtualNetworkName VNet1 -ResourceGroupName VNet101 -Name linktoVNet2

    Výstup ukazuje:

        Name            : LinkToVNet2
        Id              : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/VNet101/providers/Microsoft.Network/virtualNetworks/VNet1/virtualNetworkPeerings/LinkToVNet2
        Etag            : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
        ResourceGroupName   : VNet101
        VirtualNetworkName  : VNet1
        ProvisioningState       : Succeeded
        RemoteVirtualNetwork    : {
                                            "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/VNet101/providers/Microsoft.Network/virtualNetworks/VNet2"
                                        }
        AllowVirtualNetworkAccess   : True
        AllowForwardedTraffic            : False
        AllowGatewayTransit              : False
        UseRemoteGateways                : False
        RemoteGateways                   : null
        RemoteVirtualNetworkAddressSpace : null

    Po navázání partnerského vztahu v tomto scénáři by mělo být možné iniciovat připojení z libovolného virtuálního počítače do libovolného virtuálního počítače v obou virtuálních sítích. Ve výchozím nastavení má vlastnost AllowVirtualNetworkAccess hodnotu True a partnerský vztah virtuálních sítí zřídí odpovídající seznamy ACL, které umožní komunikaci mezi virtuálními sítěmi. Stále můžete pomocí pravidel skupiny zabezpečení sítě (NSG) blokovat připojení mezi konkrétními podsítěmi nebo virtuálními počítači, a tak podrobně řídit přístupy mezi dvěma dotčenými virtuálními sítěmi.  Další informace o vytváření pravidel NSG najdete v tomto [článku](virtual-networks-create-nsg-arm-ps.md).

[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-crosssub-include](../../includes/virtual-networks-create-vnetpeering-scenario-crosssub-include.md)]

Pokud chcete vytvořit partnerský vztah virtuálních sítí mezi různými předplatnými, postupujte následujícím způsobem:

1. Přihlaste se ke službě Azure s účtem uživatele A, který má v předplatném A práva privilegovaného uživatele, a spusťte následující rutinu:

        New-AzureRmRoleAssignment -SignInName <UserB ID> -RoleDefinitionName "Network Contributor" -Scope /subscriptions/<Subscription-A-ID>/resourceGroups/<ResourceGroupName>/providers/Microsoft.Network/VirtualNetwork/VNet5

    Toto není povinné. Partnerský vztah lze navázat i v případě, že uživatelé vytvoří požadavky na partnerský vztah pro své virtuální sítě jednotlivě, pokud se jejich požadavky shodují. Když přidáte privilegované uživatele z druhé virtuální sítě jako uživatele místní virtuální sítě , budete mít nastavení snazší.

2. Přihlaste se ke službě Azure s účtem uživatele A, který má v předplatném A práva privilegovaného uživatele, a spusťte následující rutinu:

        New-AzureRmRoleAssignment -SignInName <UserA ID> -RoleDefinitionName "Network Contributor" -Scope /subscriptions/<Subscription-B-ID>/resourceGroups/<ResourceGroupName>/providers/Microsoft.Network/VirtualNetwork/VNet3

3. V relaci přihlášení uživatele A spusťte tuto rutinu:

        New-AzureRmResourceGroupDeployment -ResourceGroupName VNet101 -TemplateFile .\VNetPeeringVNet3.json -DeploymentDebugLogLevel all

    Soubor JSON bude definován takhle:  

        {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
        },
        "variables": {
        },
        "resources": [
            {
            "apiVersion": "2016-06-01",
            "type": "Microsoft.Network/virtualNetworks/virtualNetworkPeerings",
            "name": "VNet3/LinkToVNet5",
            "location": "[resourceGroup().location]",
            "properties": {
            "allowVirtualNetworkAccess": true,
            "allowForwardedTraffic": false,
            "allowGatewayTransit": false,
            "useRemoteGateways": false,
                "remoteVirtualNetwork": {
                "id": "/subscriptions/<Subscription-B-ID>/resourceGroups/<resource group name>/providers/Microsoft.Network/virtualNetworks/VNet5"
                }
            }
            }
        ]
        }

4. V relaci přihlášení uživatele B spusťte následující rutinu:

        New-AzureRmResourceGroupDeployment -ResourceGroupName VNet101 -TemplateFile .\VNetPeeringVNet5.json -DeploymentDebugLogLevel all

    Soubor JSON bude definován takhle:

        {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
        },
        "variables": {
        },
        "resources": [
            {
            "apiVersion": "2016-06-01",
            "type": "Microsoft.Network/virtualNetworks/virtualNetworkPeerings",
            "name": "VNet5/LinkToVNet3",
            "location": "[resourceGroup().location]",
            "properties": {
            "allowVirtualNetworkAccess": true,
            "allowForwardedTraffic": false,
            "allowGatewayTransit": false,
            "useRemoteGateways": false,
                "remoteVirtualNetwork": {
                "id": "/subscriptions/Subscription-A-ID /resourceGroups/<resource group name>/providers/Microsoft.Network/virtualNetworks/VNet3"
                }
            }
            }
        ]
        }

    Po navázání partnerského vztahu v tomto scénáři by mělo být možné navázat spojení z libovolného virtuálního počítače do libovolného virtuálního počítače v obou virtuálních sítích s různým předplatným.

[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-transit-include](../../includes/virtual-networks-create-vnetpeering-scenario-transit-include.md)]

1. V tomto scénáři můžete navázat partnerský vztah virtuálních sítí nasazením následující ukázkové šablony.  Bude nutné nastavit vlastnost AllowForwardedTraffic na hodnotu True, což umožňuje síťovému virtuálnímu zařízení v partnerské virtuální síti odesílat a přijímat přenosy dat.

    Tady vidíte šablonu pro vytvoření partnerského vztahu virtuálních sítí ze sítě HubVNet do sítě VNet1. Všimněte si, že vlastnost AllowForwardedTraffic je nastavená na hodnotu false.

        {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
        },
        "variables": {
        },
        "resources": [
            {
            "apiVersion": "2016-06-01",
            "type": "Microsoft.Network/virtualNetworks/virtualNetworkPeerings",
            "name": "HubVNet/LinkToVNet1",
            "location": "[resourceGroup().location]",
            "properties": {
            "allowVirtualNetworkAccess": true,
            "allowForwardedTraffic": false,
            "allowGatewayTransit": false,
            "useRemoteGateways": false,
                "remoteVirtualNetwork": {
                "id": "[resourceId('Microsoft.Network/virtualNetworks', 'vnet1')]"       
                }
            }
            }
            }
        ]
        }

2. Tady vidíte šablonu pro vytvoření partnerského vztahu virtuálních sítí ze sítě VNet1 do sítě HubVNet. Všimněte si, že vlastnost AllowForwardedTraffic je nastavená na hodnotu true.

        {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
        },
        "variables": {
        },
        "resources": [
            {
            "apiVersion": "2016-06-01",
            "type": "Microsoft.Network/virtualNetworks/virtualNetworkPeerings",
            "name": "VNet1/LinkToHubVNet",
            "location": "[resourceGroup().location]",
            "properties": {
            "allowVirtualNetworkAccess": true,
            "allowForwardedTraffic": true,
            "allowGatewayTransit": false,
            "useRemoteGateways": false,
                "remoteVirtualNetwork": {
                "id": "[resourceId('Microsoft.Network/virtualNetworks', 'HubVnet')]"       
                }
            }
            }
        ]
        }


3. Po navázání partnerského vztahu si můžete projít tento [článek](virtual-network-create-udr-arm-ps.md) a definovat Uživatelem definované trasy (UDR) pro přesměrování přenosu dat sítě VNet1 prostřednictvím virtuálního zařízení, abyste využili jeho možností. Při zadávání adresy pro další směrování v trase můžete jako adresu nastavit IP adresu virtuálního zařízení v partnerské virtuální síti HubVNet.

[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-asmtoarm-include](../../includes/virtual-networks-create-vnetpeering-scenario-asmtoarm-include.md)]

Partnerský vztah mezi virtuálními sítěmi z různých modelů nasazení vytvoříte následujícím způsobem:
1. Níže uvedený text představuje definici propojení partnerského vztahu virtuálních sítí ze sítě VNET1 do sítě VNET2 v tomto scénáři. K navázání partnerského vztahu klasické virtuální sítě a virtuální sítě Azure Resource Manageru je potřeba jediné propojení.

    Nezapomeňte doplnit své ID předplatného s ohledem na umístění virtuální sítě nebo sítě VNET2 a hodnotu MyResouceGroup změňte na vhodný název skupiny prostředků.

    {  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",  "contentVersion": "1.0.0.0",  "parameters": {  },  "variables": {  },  "resources": [      {      "apiVersion": "2016-06-01",      "type": "Microsoft.Network/virtualNetworks/virtualNetworkPeerings",      "name": "VNET1/LinkToVNET2",      "location": "[resourceGroup().location]",      "properties": {      "allowVirtualNetworkAccess": true,      "allowForwardedTraffic": false,      "allowGatewayTransit": false,      "useRemoteGateways": false,          "remoteVirtualNetwork": {          "id": "[resourceId('Microsoft.ClassicNetwork/virtualNetworks', 'VNET2')]"  }      }      }  ]  }

2. Soubor šablony nasadíte spuštěním následující rutiny, která vytváří nebo aktualizuje nasazení.

        New-AzureRmResourceGroupDeployment -ResourceGroupName MyResourceGroup -TemplateFile .\VnetPeering.json -DeploymentDebugLogLevel all

        Output shows:

        DeploymentName          : VnetPeering
        ResourceGroupName       : MyResourceGroup
        ProvisioningState       : Succeeded
        Timestamp               : XX/XX/YYYY 5:42:33 PM
        Mode                    : Incremental
        TemplateLink            :
        Parameters              :
        Outputs                 :
        DeploymentDebugLogLevel : RequestContent, ResponseContent

3. Po úspěšném nasazení můžete pomocí následující rutiny zobrazit stav partnerského vztahu:

        Get-AzureRmVirtualNetworkPeering -VirtualNetworkName VNET1 -ResourceGroupName MyResourceGroup -Name LinkToVNET2

        Output shows:

        Name                             : LinkToVNET2
        Id                               : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/MyResource
                                   Group/providers/Microsoft.Network/virtualNetworks/VNET1/virtualNetworkPeering
                                   s/LinkToVNET2
        Etag                             : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
        ResourceGroupName                : MyResourceGroup
        VirtualNetworkName               : VNET1
        PeeringState                     : Connected
        ProvisioningState                : Succeeded
        RemoteVirtualNetwork             : {
                                     "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/M
                                   yResourceGroup/providers/Microsoft.ClassicNetwork/virtualNetworks/VNET2"
                                   }
        AllowVirtualNetworkAccess        : True
        AllowForwardedTraffic            : False
        AllowGatewayTransit              : False
        UseRemoteGateways                : False
        RemoteGateways                   : null
        RemoteVirtualNetworkAddressSpace : null

Jakmile se naváže partnerský vztah mezi klasickou virtuální sítí a virtuální sítí Resource Manageru, měli byste mít možnost iniciovat propojení z libovolného virtuálního počítače v síti VNET1 do libovolného virtuálního počítače v síti VNET2 a naopak.



<!--HONumber=Sep16_HO3-->


