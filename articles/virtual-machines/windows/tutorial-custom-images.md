---
title: "Vytvoření vlastní Image virtuálních počítačů pomocí prostředí Azure PowerShell | Microsoft Docs"
description: "Kurz – vytvoření vlastní image virtuálního počítače pomocí prostředí Azure PowerShell."
services: virtual-machines-windows
documentationcenter: virtual-machines
author: cynthn
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 05/08/2017
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: cee283268057a407003a38f8db5af8cac151439f
ms.sourcegitcommit: 9c3150e91cc3075141dc2955a01f47040d76048a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/26/2017
---
# <a name="create-a-custom-image-of-an-azure-vm-using-powershell"></a>Vytvořit vlastní image virtuálního počítače Azure pomocí prostředí PowerShell

Vlastní Image jsou jako obrázky marketplace, ale můžete vytvořit sami. Vlastní Image slouží k zavedení konfigurace, třeba předběžného načítání aplikace, konfigurace aplikace a další konfigurace operačního systému. V tomto kurzu vytvoříte svoji vlastní image virtuálního počítače v Azure. Získáte informace o těchto tématech:

> [!div class="checklist"]
> * Nástroj Sysprep a generalize virtuální počítače
> * Vytvoření vlastní image
> * Vytvoření virtuálního počítače z vlastní image
> * Seznam všechny bitové kopie v rámci vašeho předplatného
> * Odstranit bitovou kopii

Tento kurz vyžaduje modul Azure PowerShell verze 3.6 nebo novější. Verzi zjistíte spuštěním příkazu ` Get-Module -ListAvailable AzureRM`. Pokud potřebujete upgrade, přečtěte si téma [Instalace modulu Azure PowerShell](/powershell/azure/install-azurerm-ps).

## <a name="before-you-begin"></a>Než začnete

Následující postup podrobnosti jak využít existující virtuální počítač a zapnout ho do opakovaně použitelné vlastní obrázek, který můžete použít k vytvoření nové instance virtuálního počítače.

Pokud chcete provést v příkladu v tomto kurzu, musí mít existující virtuální počítač. V případě potřeby to [ukázka skriptu](../scripts/virtual-machines-windows-powershell-sample-create-vm.md) můžete vytvořit za vás. Při absolvování kurzu, nahraďte skupinu prostředků a virtuálních počítačů názvy, kde je potřeba.

## <a name="prepare-vm"></a>Příprava virtuálního počítače

Chcete-li vytvořit bitovou kopii virtuálního počítače, je nutné připravit virtuální počítač generalizací virtuální počítač, rušení přidělení a pak označením zdrojového virtuálního počítače jako zobecněn v Azure.

### <a name="generalize-the-windows-vm-using-sysprep"></a>Generalize virtuální počítač s Windows pomocí nástroje Sysprep

Nástroj Sysprep odstraní všechny vaše osobní informace o účtu, mimo jiné a připraví počítač, který se má použít jako obrázek. Podrobnosti o nástroji Sysprep najdete v tématu [postup použití nástroje Sysprep: Úvod](http://technet.microsoft.com/library/bb457073.aspx).


1. Připojte k virtuálnímu počítači.
2. Otevřete okno příkazového řádku jako správce. Změňte adresář na *%windir%\system32\sysprep*a poté spusťte *sysprep.exe*.
3. V **nástroj pro přípravu systému** dialogové okno, vyberte *prostředí Out-of-Box zadejte systému (při prvním zapnutí)*a ujistěte se, že *generalizace* je zaškrtnuté políčko.
4. V **možnosti vypnutí**, vyberte *vypnutí* a pak klikněte na **OK**.
5. Po dokončení nástroj Sysprep vypne virtuální počítač. **Virtuální počítač nerestartuje**.

### <a name="deallocate-and-mark-the-vm-as-generalized"></a>Deallocate a označit virtuální počítač jako zobecněn

Virtuální počítač na vytvoření bitové kopie, je třeba navrácena a označené jako zobecněn v Azure.

Virtuální počítač pomocí navrátil [Stop-AzureRmVM](/powershell/module/azurerm.compute/stop-azurermvm).

```powershell
Stop-AzureRmVM -ResourceGroupName myResourceGroup -Name myVM -Force
```

Nastavte stav virtuálního počítače na `-Generalized` pomocí [Set-AzureRmVm](/powershell/module/azurerm.compute/set-azurermvm). 
   
```powershell
Set-AzureRmVM -ResourceGroupName myResourceGroup -Name myVM -Generalized
```


## <a name="create-the-image"></a>Vytvoření bitové kopie

Nyní můžete vytvořit image virtuálního počítače pomocí [New-AzureRmImageConfig](/powershell/module/azurerm.compute/new-azurermimageconfig) a [New-AzureRmImage](/powershell/module/azurerm.compute/new-azurermimage). Následující příklad vytvoří bitovou kopii s názvem *myImage* z virtuálního počítače s názvem *Můjvp*.

Získáte virtuální počítač. 

```powershell
$vm = Get-AzureRmVM -Name myVM -ResourceGroupName myResourceGroup
```

Vytvořte konfiguraci bitové kopie.

```powershell
$image = New-AzureRmImageConfig -Location EastUS -SourceVirtualMachineId $vm.ID 
```

Vytvoření bitové kopie.

```powershell
New-AzureRmImage -Image $image -ImageName myImage -ResourceGroupName myResourceGroup
``` 

 
## <a name="create-vms-from-the-image"></a>Vytvořit virtuální počítače z image

Teď, když máte image, můžete vytvořit jeden nebo více nové virtuální počítače z image. Vytvoření virtuálního počítače z vlastní image je velmi podobný k vytvoření virtuálního počítače pomocí image pořízenou prostřednictvím Marketplace. Pokud používáte image pořízenou prostřednictvím Marketplace, je nutné zadat informace o bitové kopie, zprostředkovatele bitové kopie, nabídky, SKU a verzi. Vlastní Image stačí zadat Identifikátor prostředku vlastní image. 

V následující skript, vytvoříme proměnnou *$image* k ukládání informací o používání vlastní image [Get-AzureRmImage](/powershell/module/azurerm.compute/get-azurermimage) a pak nám pomocí [Set-AzureRmVMSourceImage](/powershell/module/azurerm.compute/set-azurermvmsourceimage) a zadejte ID pomocí *$image* proměnné jsme právě vytvořili. 

Tento skript vytvoří virtuální počítač s názvem *myVMfromImage* z našich vlastní image ve novou skupinu prostředků s názvem *myResourceGroupFromImage* v *západní USA* umístění.


```powershell
$cred = Get-Credential -Message "Enter a username and password for the virtual machine."

New-AzureRmResourceGroup -Name myResourceGroupFromImage -Location EastUS

$subnetConfig = New-AzureRmVirtualNetworkSubnetConfig `
    -Name mySubnet `
    -AddressPrefix 192.168.1.0/24

$vnet = New-AzureRmVirtualNetwork `
    -ResourceGroupName myResourceGroupFromImage `
    -Location EastUS `
    -Name MYvNET `
    -AddressPrefix 192.168.0.0/16 `
    -Subnet $subnetConfig

$pip = New-AzureRmPublicIpAddress `
    -ResourceGroupName myResourceGroupFromImage `
    -Location EastUS `
    -Name "mypublicdns$(Get-Random)" `
    -AllocationMethod Static `
    -IdleTimeoutInMinutes 4

  $nsgRuleRDP = New-AzureRmNetworkSecurityRuleConfig `
    -Name myNetworkSecurityGroupRuleRDP `
    -Protocol Tcp `
    -Direction Inbound `
    -Priority 1000 `
    -SourceAddressPrefix * `
    -SourcePortRange * `
    -DestinationAddressPrefix * `
    -DestinationPortRange 3389 `
    -Access Allow

  $nsg = New-AzureRmNetworkSecurityGroup `
    -ResourceGroupName myResourceGroupFromImage `
    -Location EastUS `
    -Name myNetworkSecurityGroup `
    -SecurityRules $nsgRuleRDP

$nic = New-AzureRmNetworkInterface `
    -Name myNic `
    -ResourceGroupName myResourceGroupFromImage `
    -Location EastUS `
    -SubnetId $vnet.Subnets[0].Id `
    -PublicIpAddressId $pip.Id `
    -NetworkSecurityGroupId $nsg.Id

$vmConfig = New-AzureRmVMConfig `
    -VMName myVMfromImage `
    -VMSize Standard_D1 | Set-AzureRmVMOperatingSystem -Windows `
        -ComputerName myComputer `
        -Credential $cred 

# Here is where we create a variable to store information about the image 
$image = Get-AzureRmImage `
    -ImageName myImage `
    -ResourceGroupName myResourceGroup

# Here is where we specify that we want to create the VM from and image and provide the image ID
$vmConfig = Set-AzureRmVMSourceImage -VM $vmConfig -Id $image.Id

$vmConfig = Add-AzureRmVMNetworkInterface -VM $vmConfig -Id $nic.Id

New-AzureRmVM `
    -ResourceGroupName myResourceGroupFromImage `
    -Location EastUS `
    -VM $vmConfig
```

## <a name="image-management"></a>Správy bitových kopií 

Zde jsou některé příklady běžné úlohy správy bitové kopie a postup jejich dokončení pomocí prostředí PowerShell.

Zobrazí seznam všech Image podle názvu.

```powershell
$images = Find-AzureRMResource -ResourceType Microsoft.Compute/images 
$images.name
```

Odstraňte bitovou kopii. Tento příklad odstraní image s názvem *myOldImage* z *myResourceGroup*.

```powershell
Remove-AzureRmImage `
    -ImageName myOldImage `
    -ResourceGroupName myResourceGroup
```

## <a name="next-steps"></a>Další kroky

V tomto kurzu jste vytvořili vlastní image virtuálního počítače. Naučili jste se tyto postupy:

> [!div class="checklist"]
> * Nástroj Sysprep a generalize virtuální počítače
> * Vytvoření vlastní image
> * Vytvoření virtuálního počítače z vlastní image
> * Seznam všechny bitové kopie v rámci vašeho předplatného
> * Odstranit bitovou kopii

Přechodu na další informace o tom, jak vysoce dostupné virtuální počítače v dalším kurzu.

> [!div class="nextstepaction"]
> [Vytvoření vysoce dostupných virtuálních počítačů](tutorial-availability-sets.md)



