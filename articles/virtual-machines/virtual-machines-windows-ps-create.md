---
title: "Vytvoření virtuálního počítače Azure pomocí PowerShellu | Dokumentace Microsoftu"
description: "Pomocí Azure PowerShellu a Azure Resource Manageru můžete snadno vytvořit nový virtuální počítač s Windows Serverem."
services: virtual-machines-windows
documentationcenter: 
author: davidmu1
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 14fe9ca9-e228-4d3b-a5d8-3101e9478f6e
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 10/21/2016
ms.author: davidmu
translationtype: Human Translation
ms.sourcegitcommit: 5919c477502767a32c535ace4ae4e9dffae4f44b
ms.openlocfilehash: 87f62d99ae8671fb3732806d8cd8bd7d9aa101e1


---
# <a name="create-a-windows-vm-using-resource-manager-and-powershell"></a>Vytvoření virtuálního počítače s Windows pomocí Resource Manageru a PowerShellu
Tento článek ukazuje, jak rychle vytvořit virtuální počítač Azure s Windows Serverem a prostředky, které potřebuje, pomocí [Resource Manageru](../azure-resource-manager/resource-group-overview.md) a prostředí PowerShell. 

Žádný z kroků pro vytvoření virtuálního počítače v tomto článku není možné vynechat a celý postup by neměl trvat déle než 30 minut. Příklad hodnot parametrů v příkazech nahraďte názvy, které dávají smysl pro vaše prostředí.

## <a name="step-1-install-azure-powershell"></a>Krok1: Nainstalování prostředí Azure PowerShell
Projděte si článek [Jak nainstalovat a nakonfigurovat Azure PowerShell](../powershell-install-configure.md), kde najdete informace o instalaci nejnovější verze prostředí Azure PowerShell, výběru předplatného a přihlášení k účtu.

## <a name="step-2-create-a-resource-group"></a>Krok2: Vytvoření skupiny prostředků
Všechny prostředky musí být obsažené ve skupině prostředků, takže si nejprve vytvořte tuto skupinu.  

1. Získejte seznamu dostupných umístění, kde je možné prostředky vytvořit.
   
    ```powershell
    Get-AzureRmLocation | sort Location | Select Location
    ```
2. Nastavte umístění pro prostředky. Tento příkaz nastaví umístění na **centralus**.
   
    ```powershell
    $location = "centralus"
    ```
3. Vytvořte skupinu prostředků. Tento příkaz vytvoří skupinu prostředků nazvanou **myResourceGroup** v umístění, které jste nastavili.
   
    ```powershell
    $myResourceGroup = "myResourceGroup"
    New-AzureRmResourceGroup -Name $myResourceGroup -Location $location
    ```

## <a name="step-3-create-a-storage-account"></a>Krok3: Vytvoření účtu úložiště
[Účet úložiště](../storage/storage-introduction.md) je nutný k uložení virtuálního pevného disku používaného virtuálním počítačem, který vytvoříte. Názvy účtů úložiště musí mít od 3 do 24 znaků a můžou obsahovat jenom číslice a malá písmena.

1. Otestujte, jestli je název účtu úložiště jedinečný. Tento příkaz testuje název **myStorageAccount**.
   
    ```powershell
    $myStorageAccountName = "mystorageaccount"
    Get-AzureRmStorageAccountNameAvailability $myStorageAccountName
    ```
   
    Pokud tento příkaz vrátí hodnotu **False**, je navržený název v rámci Azure jedinečný. 
2. Teď vytvořte účet úložiště.
   
    ```powershell    
    $myStorageAccount = New-AzureRmStorageAccount -ResourceGroupName $myResourceGroup `
        -Name $myStorageAccountName -SkuName "Standard_LRS" -Kind "Storage" -Location $location
    ```

## <a name="step-4-create-a-virtual-network"></a>Krok 4: Vytvoření virtuální sítě
Všechny virtuální počítače jsou součástí [virtuální sítě](../virtual-network/virtual-networks-overview.md).

1. Vytvořte podsíť pro virtuální síť. Tento příkaz vytvoří podsíť s názvem **mySubnet** s předponou adresy 10.0.0.0/24.
   
    ```powershell
    $mySubnet = New-AzureRmVirtualNetworkSubnetConfig -Name "mySubnet" -AddressPrefix 10.0.0.0/24
    ```
2. Teď vytvořte virtuální síť. Tento příkaz vytvoří virtuální síť nazvanou **myVnet** pomocí podsítě, kterou jste vytvořili, a předpony adresy **10.0.0.0/16**.
   
    ```powershell
    $myVnet = New-AzureRmVirtualNetwork -Name "myVnet" -ResourceGroupName $myResourceGroup `
        -Location $location -AddressPrefix 10.0.0.0/16 -Subnet $mySubnet
    ```

## <a name="step-5-create-a-public-ip-address-and-network-interface"></a>Krok 5: Vytvoření veřejné IP adresy a síťového rozhraní
Pokud chcete povolit komunikaci s virtuálním počítačem ve virtuální síti, budete potřebovat [veřejnou adresu IP](../virtual-network/virtual-network-ip-addresses-overview-arm.md) a síťové rozhraní.

1. Vytvořte veřejnou IP adresu. Tento příkaz vytvoří veřejnou IP adresu nazvanou **myPublicIp** s **dynamickou** metodou přidělování.
   
    ```powershell
    $myPublicIp = New-AzureRmPublicIpAddress -Name "myPublicIp" -ResourceGroupName $myResourceGroup `
        -Location $location -AllocationMethod Dynamic
    ```
2. Vytvořte síťové rozhraní. Tento příkaz vytvoří síťové rozhraní s názvem **myNIC**.
   
    ```powershell
    $myNIC = New-AzureRmNetworkInterface -Name "myNIC" -ResourceGroupName $myResourceGroup `
        -Location $location -SubnetId $myVnet.Subnets[0].Id -PublicIpAddressId $myPublicIp.Id
    ```

## <a name="step-6-create-a-virtual-machine"></a>Krok 6: Vytvoření virtuálního počítače
Teď, když máte vytvořené všechno potřebné, je čas vytvořit si virtuální počítač.

1. Spuštěním tohoto příkazu nastavte název účtu správce a heslo pro virtuální počítač.

    ```powershell
    $cred = Get-Credential -Message "Type the name and password of the local administrator account."
    ```
   
    Heslo musí být 12 až 123 znaků dlouhé a musí obsahovat minimálně jedno malé písmeno, jedno velké písmeno, jednu číslici a jeden speciální znak. 
2. Vytvořte objekt konfigurace pro virtuální počítač. Tento příkaz vytvoří objekt konfigurace s názvem **myVmConfig**, který definuje název a velikost virtuálního počítače.
   
    ```powershell
    $myVm = New-AzureRmVMConfig -VMName "myVM" -VMSize "Standard_DS1_v2"
    ```
   
    Projděte si téma [Velikosti virtuálních počítačů v Azure](virtual-machines-windows-sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json), kde najdete seznam dostupných velikostí pro virtuální počítač.
3. Nakonfigurujte nastavení operačního systému pro virtuální počítač. Tento příkaz nastaví název počítače, typ operačního systému a přihlašovací údaje účtu pro virtuální počítač.
   
    ```powershell
    $myVM = Set-AzureRmVMOperatingSystem -VM $myVM -Windows -ComputerName "myVM" -Credential $cred `
        -ProvisionVMAgent -EnableAutoUpdate
    ```
4. Definujte image, která se má použít ke zřízení virtuálního počítače. Tento příkaz definuje image Windows Serveru, která se má používat pro virtuální počítač. 
   
    ```powershell
    $myVM = Set-AzureRmVMSourceImage -VM $myVM -PublisherName "MicrosoftWindowsServer" `
        -Offer "WindowsServer" -Skus "2012-R2-Datacenter" -Version "latest"
    ```
   
    Další informace o výběru image, která se má použít, najdete v tématu [Vyhledání a výběr imagí virtuálních počítačů v Azure pomocí PowerShellu nebo CLI](virtual-machines-windows-cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
5. Přidejte síťové rozhraní, které jste vytvořili, do konfigurace.
   
    ```powershell
    $myVM = Add-AzureRmVMNetworkInterface -VM $myVM -Id $myNIC.Id
    ```
6. Definujte název a umístění pevného disku virtuálního počítače. Soubor virtuálního pevného disku je uložený v kontejneru. Tento příkaz vytvoří disk v kontejneru nazvaném **vhds/WindowsVMosDisk.vhd** v účtu úložiště, který jste vytvořili.
   
    ```powershell
    $blobPath = "vhds/myOsDisk1.vhd"
    $osDiskUri = $myStorageAccount.PrimaryEndpoints.Blob.ToString() + $blobPath
    ```
7. Přidejte informace o disku operačního systému do konfigurace virtuálního počítače. Nahraďte hodnotu **$diskName** názvem pro disk operačního systému. Vytvořte proměnnou a přidejte informace o disku do konfigurace.
   
    ```powershell
    $vm = Set-AzureRmVMOSDisk -VM $myVM -Name "myOsDisk1" -VhdUri $osDiskUri -CreateOption fromImage
    ```
8. Nakonec vytvořte virtuální počítač.
   
    ```powershell
    New-AzureRmVM -ResourceGroupName $myResourceGroup -Location $location -VM $myVM
    ```

## <a name="next-steps"></a>Další kroky
* Pokud byly nějaké problémy s nasazením, je dalším krokem projít si téma [Řešení potíží s nasazením skupin prostředků pomocí webu Azure Portal](../resource-manager-troubleshoot-deployments-portal.md).
* Projděte si téma [Správa virtuálních počítačů pomocí Azure Resource Manageru a prostředí PowerShell](virtual-machines-windows-ps-manage.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json), kde najdete informace o tom, jak spravovat virtuální počítač, který jste vytvořili.
* Podle informací v tématu [Vytvoření virtuálního počítače s Windows pomocí šablony Resource Manageru](virtual-machines-windows-ps-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) můžete vytvořit virtuální počítač pomocí šablony.




<!--HONumber=Nov16_HO2-->


