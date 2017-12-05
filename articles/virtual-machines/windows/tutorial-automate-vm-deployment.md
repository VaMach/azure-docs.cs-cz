---
title: "Přizpůsobení systému Windows virtuálního počítače v Azure | Microsoft Docs"
description: "Další informace o použití rozšíření vlastních skriptů a Key Vault pro přizpůsobení systému Windows virtuálních počítačů v Azure"
services: virtual-machines-windows
documentationcenter: virtual-machines
author: iainfoulds
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 08/11/2017
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: e0503cef234b01c4eefb1c1de3d88d9a812c4c39
ms.sourcegitcommit: 7136d06474dd20bb8ef6a821c8d7e31edf3a2820
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/05/2017
---
# <a name="how-to-customize-a-windows-virtual-machine-in-azure"></a>Přizpůsobení virtuálního počítače s Windows v Azure
Ke konfiguraci virtuálních počítačů (VM) rychlé a konzistentním způsobem, je obvykle potřeby nějaký způsob automatizace. Běžný postup přizpůsobení virtuální počítač s Windows je použití [vlastní skript rozšíření pro Windows](extensions-customscript.md). V tomto kurzu se naučíte:

> [!div class="checklist"]
> * Použití rozšíření vlastních skriptů instalace služby IIS
> * Vytvoření virtuálního počítače, který používá rozšíření vlastních skriptů
> * Po rozšíření platí zobrazit spuštěné webu IIS

[!INCLUDE [cloud-shell-powershell.md](../../../includes/cloud-shell-powershell.md)]

Pokud se rozhodnete nainstalovat a používat PowerShell místně, musíte použít modul Azure PowerShell verze 3.6 nebo novější. Verzi zjistíte spuštěním příkazu ` Get-Module -ListAvailable AzureRM`. Pokud potřebujete upgrade, přečtěte si téma [Instalace modulu Azure PowerShell](/powershell/azure/install-azurerm-ps). Pokud používáte PowerShell místně, je také potřeba spustit příkaz `Login-AzureRmAccount` pro vytvoření připojení k Azure. 


## <a name="custom-script-extension-overview"></a>Přehled rozšíření vlastních skriptů
Rozšíření vlastních skriptů stahuje a spouští skripty na virtuálních počítačích Azure. Toto rozšíření je užitečné pro konfiguraci nasazení post, instalace softwaru nebo jakoukoli jinou konfiguraci, nebo úlohu správy. Skripty můžete stáhnout z úložiště Azure nebo GitHub nebo zadat na portál Azure na dobu běhu rozšíření.

Rozšíření vlastních skriptů se integruje s šablon Azure Resource Manageru a můžete také spustit pomocí rozhraní příkazového řádku Azure, PowerShell, portálu Azure nebo REST API pro virtuální počítač Azure.

S Windows a virtuální počítače s Linuxem můžete použít rozšíření vlastních skriptů.


## <a name="create-virtual-machine"></a>Vytvoření virtuálního počítače
Před vytvořením virtuálního počítače, vytvořte skupinu prostředků s [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup). Následující příklad vytvoří skupinu prostředků s názvem *myResourceGroupAutomate* v *EastUS* umístění:

```azurepowershell-interactive
New-AzureRmResourceGroup -ResourceGroupName myResourceGroupAutomate -Location EastUS
```

Nastavte správce uživatelské jméno a heslo pro virtuální počítače s [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential):

```azurepowershell-interactive
$cred = Get-Credential
```

Nyní můžete vytvořit virtuální počítač s [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm). Následující příklad vytvoří virtuální sítě požadované součásti, konfigurace operačního systému a poté vytvoří virtuální počítač s názvem *Můjvp*:

```azurepowershell-interactive
# Create a subnet configuration
$subnetConfig = New-AzureRmVirtualNetworkSubnetConfig `
    -Name mySubnet `
    -AddressPrefix 192.168.1.0/24

# Create a virtual network
$vnet = New-AzureRmVirtualNetwork `
    -ResourceGroupName myResourceGroupAutomate `
    -Location EastUS `
    -Name myVnet `
    -AddressPrefix 192.168.0.0/16 `
    -Subnet $subnetConfig

# Create a public IP address and specify a DNS name
$publicIP = New-AzureRmPublicIpAddress `
    -ResourceGroupName myResourceGroupAutomate `
    -Location EastUS `
    -AllocationMethod Static `
    -IdleTimeoutInMinutes 4 `
    -Name "myPublicIP"

# Create an inbound network security group rule for port 3389
$nsgRuleRDP = New-AzureRmNetworkSecurityRuleConfig `
    -Name myNetworkSecurityGroupRuleRDP  `
    -Protocol Tcp `
    -Direction Inbound `
    -Priority 1000 `
    -SourceAddressPrefix * `
    -SourcePortRange * `
    -DestinationAddressPrefix * `
    -DestinationPortRange 3389 `
    -Access Allow

# Create an inbound network security group rule for port 80
$nsgRuleWeb = New-AzureRmNetworkSecurityRuleConfig `
    -Name myNetworkSecurityGroupRuleWWW  `
    -Protocol Tcp `
    -Direction Inbound `
    -Priority 1001 `
    -SourceAddressPrefix * `
    -SourcePortRange * `
    -DestinationAddressPrefix * `
    -DestinationPortRange 80 `
    -Access Allow

# Create a network security group
$nsg = New-AzureRmNetworkSecurityGroup `
    -ResourceGroupName myResourceGroupAutomate `
    -Location EastUS `
    -Name myNetworkSecurityGroup `
    -SecurityRules $nsgRuleRDP,$nsgRuleWeb

# Create a virtual network card and associate with public IP address and NSG
$nic = New-AzureRmNetworkInterface `
    -Name myNic `
    -ResourceGroupName myResourceGroupAutomate `
    -Location EastUS `
    -SubnetId $vnet.Subnets[0].Id `
    -PublicIpAddressId $publicIP.Id `
    -NetworkSecurityGroupId $nsg.Id

# Create a virtual machine configuration
$vmConfig = New-AzureRmVMConfig -VMName myVM -VMSize Standard_DS2 | `
Set-AzureRmVMOperatingSystem -Windows -ComputerName myVM -Credential $cred | `
Set-AzureRmVMSourceImage -PublisherName MicrosoftWindowsServer `
    -Offer WindowsServer -Skus 2016-Datacenter -Version latest | `
Add-AzureRmVMNetworkInterface -Id $nic.Id

# Create a virtual machine using the configuration
New-AzureRmVM -ResourceGroupName myResourceGroupAutomate -Location EastUS -VM $vmConfig
```

Trvá několik minut na zdroje a virtuální počítač, který se má vytvořit.


## <a name="automate-iis-install"></a>Automatizaci instalace služby IIS
Použití [Set-AzureRmVMExtension](/powershell/module/azurerm.compute/set-azurermvmextension) k instalaci rozšíření vlastních skriptů. Spustí rozšíření `powershell Add-WindowsFeature Web-Server` nainstalovat webový server služby IIS a aktualizací *Default.htm* stránku a zobrazit název hostitele virtuálního počítače:

```azurepowershell-interactive
Set-AzureRmVMExtension -ResourceGroupName myResourceGroupAutomate `
    -ExtensionName IIS `
    -VMName myVM `
    -Publisher Microsoft.Compute `
    -ExtensionType CustomScriptExtension `
    -TypeHandlerVersion 1.4 `
    -SettingString '{"commandToExecute":"powershell Add-WindowsFeature Web-Server; powershell Add-Content -Path \"C:\\inetpub\\wwwroot\\Default.htm\" -Value $($env:computername)"}' `
    -Location EastUS
```


## <a name="test-web-site"></a>Test webu
Získat veřejnou IP adresu nástroj pro vyrovnávání zatížení s [Get-AzureRmPublicIPAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress). Následující příklad, získá IP adresu pro *myPublicIP* vytvořili dříve:

```azurepowershell-interactive
Get-AzureRmPublicIPAddress `
    -ResourceGroupName myResourceGroupAutomate `
    -Name myPublicIP | select IpAddress
```

Potom můžete zadat veřejnou IP adresu v do webového prohlížeče. Zobrazí se na webu, včetně názvu hostitele virtuálního počítače, který nástroje pro vyrovnávání zatížení distribuován provoz jako v následujícím příkladu:

![Spuštění webu IIS](./media/tutorial-automate-vm-deployment/running-iis-website.png)


## <a name="next-steps"></a>Další kroky

V tomto kurzu automatizované instalace služby IIS na virtuálním počítači. Naučili jste se tyto postupy:

> [!div class="checklist"]
> * Použití rozšíření vlastních skriptů instalace služby IIS
> * Vytvoření virtuálního počítače, který používá rozšíření vlastních skriptů
> * Po rozšíření platí zobrazit spuštěné webu IIS

Přechodu na v dalším kurzu se dozvíte, jak vytvořit vlastní Image virtuálních počítačů.

> [!div class="nextstepaction"]
> [Vytváření vlastních imagí virtuálních počítačů](./tutorial-custom-images.md)
