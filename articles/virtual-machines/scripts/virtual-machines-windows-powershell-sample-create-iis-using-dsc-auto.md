---
title: "Vytvoření virtuálního počítače se službou IIS pomocí DSC a New-AzVM v Azure | Microsoft Docs"
description: "Ukázkový skript prostředí PowerShell Azure – vytvoření virtuálního počítače služby IIS s DSC a New-AzVM."
services: virtual-machines-windows
documentationcenter: virtual-machines
author: cynthn
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 09/29/2017
ms.author: cynthn
ms.openlocfilehash: fb7f417912124c87ed35572a00b7da81968c8d1f
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="create-an-iis-vm-with-powershell-using-new-azvm-preview"></a>Vytvoření virtuálních počítačů služby IIS pomocí prostředí PowerShell pomocí New-AzVM (preview)

Tento skript vytvoří virtuální počítač Azure systémem Windows Server 2016 pomocí New-AzVM a pak používá k instalaci IIS rozšíření DSC virtuálního počítače Azure. Po spuštění skriptu, můžete přejít na výchozí web služby IIS na veřejnou IP adresu virtuálního počítače.

Tato ukázka používá rutinu New-AzVM, která je ve verzi preview. 

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-powershell.md](../../../includes/cloud-shell-powershell.md)]

## <a name="sample-script"></a>Ukázkový skript

```azurepowershell-interactive
# Variables for common values
$location = "westeurope"
$vmName = "myVM"

New-AzVM -Name $vmName `
    -VirtualNetworkName myVNet `
    -Location $location `
    -SecurityGroupName myNSG `
    -PublicIpAddressName myPIP


# Create an inbound network security group rule for port 80

$nsgRuleHTTP = New-AzureRmNetworkSecurityRuleConfig -Name myNetworkSecurityGroupRuleHTTP  -Protocol Tcp `
  -Direction Inbound -Priority 1000 -SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix * `
  -DestinationPortRange 80 -Access Allow

$nsg = Get-AzureRmNetworkSecurityGroup -Name myNSG -ResourceGroupName myVMResourceGroup
$nsg | Add-AzureRmNetworkSecurityRuleConfig -Name $nsgRuleHTTP -NetworkSecurityGroup myNSG 
$nsg | Set-AzureRmNetworkSecurityGroup


# Install IIS
$PublicSettings = '{"ModulesURL":"https://github.com/Azure/azure-quickstart-templates/raw/master/dsc-extension-iis-server-windows-vm/ContosoWebsite.ps1.zip", "configurationFunction": "ContosoWebsite.ps1\\ContosoWebsite", "Properties": {"MachineName": "myVM"} }'

Set-AzureRmVMExtension -ExtensionName "DSC" -ResourceGroupName myVMResourceGroup -VMName $vmName `
  -Publisher "Microsoft.Powershell" -ExtensionType "DSC" -TypeHandlerVersion 2.19 `
  -SettingString $PublicSettings -Location $location
 ```

## <a name="clean-up-deployment"></a>Vyčištění nasazení 

Spusťte následující příkaz pro odebrání skupiny prostředků, virtuální počítač a všechny související prostředky.

```azurepowershell-interactive
Remove-AzureRmResourceGroup -Name myVMResourceGroup
```

## <a name="script-explanation"></a>Vysvětlení skriptu

Tento skript používá následující příkazy k vytvoření nasazení. Každou položku v tabulce odkazy na dokumentaci konkrétní příkaz.

| Příkaz | Poznámky |
|---|---|
| [Nové AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig) | Vytvoří pravidlo konfiguraci skupiny zabezpečení sítě. Tato konfigurace slouží k vytvoření pravidla NSG, když je vytvořen NSG. |
| [Get-AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/get-azurermnetworksecuritygroup) | Získá informace o NSG. |
| [Přidat AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.network/add-azurermnetworksecurityruleconfig) | Přidá nové pravidlo ke konfiguraci. |
| [Set-AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/Set-AzureRmNetworkSecurityGroup) | Aktualizuje NSG nové pravidlo. |
| [Set-AzureRmVMExtension](/powershell/module/azurerm.compute/set-azurermvmextension) | Přidání rozšíření virtuálního počítače do virtuálního počítače. V této ukázce se používá rozšíření vlastních skriptů instalace služby IIS. |
|[Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | Odebere skupinu prostředků a všechny prostředky obsažené v rámci. |

## <a name="next-steps"></a>Další kroky

Další informace o modulu Azure PowerShell najdete v tématu [dokumentace Azure PowerShell](/powershell/azure/overview).

Ukázky skriptu PowerShell další virtuální počítač nachází v [virtuálního počítače Windows Azure dokumentaci](../windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
