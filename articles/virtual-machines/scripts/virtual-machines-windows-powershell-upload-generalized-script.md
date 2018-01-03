---
title: "Nahrání zobecněný virtuální pevný disk do Azure ukázka skriptu prostředí PowerShell | Microsoft Docs"
description: "Ukázkový skript prostředí PowerShell nahrajte zobecněný virtuální pevný disk do Azure a vytvoření nového virtuálního počítače pomocí modelu nasazení resource manager a spravované disky."
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
ms.date: 01/02/2017
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: 9534ce2a32ac57a441535cfa26f2981b804182d1
ms.sourcegitcommit: 9ea2edae5dbb4a104322135bef957ba6e9aeecde
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/03/2018
---
# <a name="sample-script-to-upload-a-vhd-to-azure-and-create-a-new-vm"></a>Ukázkový skript pro nahrání virtuálního pevného disku do Azure a vytvoření nového virtuálního počítače

Tento skript trvá soubor VHD místní z zobecněný virtuální počítač a odesílá do Azure, vytvoří bitovou kopii disku spravované a použije k vytvoření nového virtuálního počítače.

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Ukázkový skript

```powershell
# Provide values for the variables
$resourceGroup = 'myResourceGroup'
$location = 'EastUS'
$storageaccount = 'mystorageaccount'
$storageType = 'Standard_LRS'
$containername = 'mycontainer'
$localPath = 'C:\Users\Public\Documents\Hyper-V\VHDs\generalized.vhd'
$vmName = 'myVM'
$imageName = 'myImage'
$vhdName = 'myUploadedVhd.vhd'
$diskSizeGB = '128'
$subnetName = 'mySubnet'
$vnetName = 'myVnet'
$ipName = 'myPip'
$nicName = 'myNic'
$nsgName = 'myNsg'
$ruleName = 'myRdpRule'
$vmName = 'myVM'
$computerName = 'myComputerName'
$vmSize = 'Standard_DS1_v2'

# Get the username and password to be used for the administrators account on the VM. 
# This is used when connecting to the VM using RDP.

$cred = Get-Credential

# Upload the VHD
New-AzureRmResourceGroup -Name $resourceGroup -Location $location
New-AzureRmStorageAccount -ResourceGroupName $resourceGroup -Name $storageAccount -Location $location `
    -SkuName $storageType -Kind "Storage"
$urlOfUploadedImageVhd = ('https://' + $storageaccount + '.blob.core.windows.net/' + $containername + '/' + $vhdName)
Add-AzureRmVhd -ResourceGroupName $resourceGroup -Destination $urlOfUploadedImageVhd `
    -LocalFilePath $localPath

# Note: Uploading the VHD may take awhile!

# Create a managed image from the uploaded VHD 
$imageConfig = New-AzureRmImageConfig -Location $location
$imageConfig = Set-AzureRmImageOsDisk -Image $imageConfig -OsType Windows -OsState Generalized `
    -BlobUri $urlOfUploadedImageVhd
$image = New-AzureRmImage -ImageName $imageName -ResourceGroupName $resourceGroup -Image $imageConfig
 
# Create the networking resources
$singleSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix 10.0.0.0/24
$vnet = New-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $resourceGroup -Location $location `
    -AddressPrefix 10.0.0.0/16 -Subnet $singleSubnet
$pip = New-AzureRmPublicIpAddress -Name $ipName -ResourceGroupName $resourceGroup -Location $location `
    -AllocationMethod Dynamic
$nic = New-AzureRmNetworkInterface -Name $nicName -ResourceGroupName $resourceGroup -Location $location `
    -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id
$rdpRule = New-AzureRmNetworkSecurityRuleConfig -Name $ruleName -Description 'Allow RDP' -Access Allow `
    -Protocol Tcp -Direction Inbound -Priority 110 -SourceAddressPrefix Internet -SourcePortRange * `
    -DestinationAddressPrefix * -DestinationPortRange 3389
$nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName $resourceGroup -Location $location `
    -Name $nsgName -SecurityRules $rdpRule
$vnet = Get-AzureRmVirtualNetwork -ResourceGroupName $resourceGroup -Name $vnetName

# Start building the VM configuration
$vm = New-AzureRmVMConfig -VMName $vmName -VMSize $vmSize

# Set the VM image as source image for the new VM
$vm = Set-AzureRmVMSourceImage -VM $vm -Id $image.Id

# Finish the VM configuration and add the NIC.
$vm = Set-AzureRmVMOSDisk -VM $vm  -DiskSizeInGB $diskSizeGB -CreateOption FromImage -Caching ReadWrite
$vm = Set-AzureRmVMOperatingSystem -VM $vm -Windows -ComputerName $computerName -Credential $cred `
    -ProvisionVMAgent -EnableAutoUpdate
$vm = Add-AzureRmVMNetworkInterface -VM $vm -Id $nic.Id

# Create the VM
New-AzureRmVM -VM $vm -ResourceGroupName $resourceGroup -Location $location

# Verify that the VM was created
$vmList = Get-AzureRmVM -ResourceGroupName $resourceGroup
$vmList.Name


```


<!-- 
[!code-powershell[main](../../../powershell_scripts/virtual-machine/create-vm-iis/create-windows-vm-iis.ps1 "Create VM IIS")] -->

## <a name="clean-up-deployment"></a>Vyčištění nasazení 

Spusťte následující příkaz pro odebrání skupiny prostředků, virtuální počítač a všechny související prostředky.

```powershell
Remove-AzureRmResourceGroup -Name $resourceGroup
```

## <a name="script-explanation"></a>Vysvětlení skriptu

Tento skript používá následující příkazy k vytvoření nasazení. Každou položku v tabulce odkazy na dokumentaci konkrétní příkaz.

| Příkaz                                                                                                             | Poznámky                                                                                                                                                                                |
|---------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Nový AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup)                           | Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.                                                                                                                          |
| [Nové AzureRmStorageAccount](/powershell/module/azurerm.resources/new-azurermstorageaccount)                         | Vytvoří účet úložiště.                                                                                                                                                           |
| [Přidat AzureRmVhd](/powershell/module/azurerm.resources/add-azurermvhd)                                               | Ukládání virtuální pevný disk z virtuálního počítače místní do objektu blob v účtu úložiště cloudu v Azure.                                                                       |
| [Nové AzureRmImageConfig](/powershell/module/azurerm.resources/new-azurermimageconfig)                               | Vytvoří objekt konfigurovat bitové kopie.                                                                                                                                                 |
| [Set-AzureRmImageOsDisk](/powershell/module/azurerm.resources/set-azurermimageosdisk)                               | Nastaví vlastnosti disku operačního systému na objekt bitové kopie.                                                                                                                        |
| [Nové AzureRmImage](/powershell/module/azurerm.resources/new-azurermimage)                                           | Vytvoří novou bitovou kopii.                                                                                                                                                                 |
| [Nové AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.resources/new-azurermvirtualnetworksubnetconfig) | Vytvoří konfiguraci podsítě. Tato konfigurace se používá s procesem vytvoření virtuální sítě.                                                                                |
| [Nový AzureRmVirtualNetwork](/powershell/module/azurerm.resources/new-azurermvirtualnetwork)                         | Vytvoří virtuální síť.                                                                                                                                                           |
| [Nové AzureRmPublicIpAddress](/powershell/module/azurerm.resources/new-azurermpublicipaddress)                       | Vytvoří veřejnou IP adresu.                                                                                                                                                         |
| [Nové AzureRmNetworkInterface](/powershell/module/azurerm.resources/new-azurermnetworkinterface)                     | Vytvoří rozhraní sítě.                                                                                                                                                         |
| [Nové AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.resources/new-azurermnetworksecurityruleconfig)   | Vytvoří pravidlo konfiguraci skupiny zabezpečení sítě. Tato konfigurace slouží k vytvoření pravidla NSG, když je vytvořen NSG.                                                       |
| [Nové AzureRmNetworkSecurityGroup](/powershell/module/azurerm.resources/new-azurermnetworksecuritygroup)             | Vytvoří skupinu zabezpečení sítě.                                                                                                                                                    |
| [Get-AzureRmVirtualNetwork](/powershell/module/azurerm.resources/get-azurermvirtualnetwork)                         | Získá virtuální síť ve skupině prostředků.                                                                                                                                          |
| [Nové AzureRmVMConfig](/powershell/module/azurerm.resources/new-azurermvmconfig)                                     | Vytvoří konfigurace virtuálního počítače. Tato konfigurace zahrnuje informace, jako je název virtuálního počítače, operační systém a pověření pro správu. Konfigurace se používá při vytváření virtuálních počítačů. |
| [Set-AzureRmVMSourceImage](/powershell/module/azurerm.resources/set-azurermvmsourceimage)                           | Určuje obrázek, který pro virtuální počítač.                                                                                                                                            |
| [Set-AzureRmVMOSDisk](/powershell/module/azurerm.resources/set-azurermvmosdisk)                                     | Nastaví vlastnosti disku operačního systému na virtuálním počítači.                                                                                                                      |
| [Set-AzureRmVMOperatingSystem](/powershell/module/azurerm.resources/set-azurermvmoperatingsystem)                   | Nastaví vlastnosti disku operačního systému na virtuálním počítači.                                                                                                                      |
| [Přidat AzureRmVMNetworkInterface](/powershell/module/azurerm.resources/add-azurermvmnetworkinterface)                 | Přidá rozhraní sítě k virtuálnímu počítači.                                                                                                                                       |
| [Nový AzureRmVM](/powershell/module/azurerm.resources/new-azurermvm)                                                 | Vytvoření virtuálního počítače.                                                                                                                                                            |
| [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup)                     | Odebere skupinu prostředků a všechny prostředky obsažené v rámci.                                                                                                                         |

## <a name="next-steps"></a>Další postup

Další informace o modulu Azure PowerShell najdete v tématu [dokumentace Azure PowerShell](/powershell/azure/overview).

Ukázky skriptu PowerShell další virtuální počítač nachází v [virtuálního počítače Windows Azure dokumentaci](../windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
