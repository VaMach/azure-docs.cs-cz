---
title: Vytvoření virtuálního počítače Azure pomocí prostředí PowerShell | Microsoft Docs
description: Pomocí Azure PowerShellu a Azure Resource Manageru můžete snadno vytvořit nový virtuální počítač s Windows Serverem.
services: virtual-machines-windows
documentationcenter: ''
author: davidmu1
manager: timlt
editor: ''
tags: azure-resource-manager

ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 09/27/2016
ms.author: davidmu

---
# Vytvoření virtuálního počítače s Windows pomocí Resource Manageru a PowerShellu
Tento článek ukazuje, jak rychle vytvořit virtuální počítač Azure s Windows Serverem a prostředky, které potřebuje, pomocí [Resource Manageru](../resource-group-overview.md) a prostředí PowerShell. 

Žádný z kroků pro vytvoření virtuálního počítače v tomto článku není možné vynechat a celý postup by neměl trvat déle než 30 minut.

## Krok1: Nainstalování prostředí Azure PowerShell
Projděte si článek [Jak nainstalovat a nakonfigurovat Azure PowerShell](../powershell-install-configure.md), kde najdete informace o instalaci nejnovější verze prostředí Azure PowerShell, výběru předplatného a přihlášení k účtu.

## Krok2: Vytvoření skupiny prostředků
Nejdříve vytvořte skupinu prostředků.

1. Získejte seznamu dostupných umístění, kde je možné prostředky vytvořit.
   
        Get-AzureRmLocation | sort Location | Select Location
   
    Zobrazení by mělo být podobné následujícímu příkladu:
   
        Location
        --------
        australiaeast
        australiasoutheast
        brazilsouth
        canadacentral
        canadaeast
        centralindia
        centralus
        eastasia
        eastus
        eastus2
        japaneast
        japanwest
        northcentralus
        northeurope
        southcentralus
        southeastasia
        southindia
        westeurope
        westindia
        westus
2. Nahraďte hodnotu **$locName** umístěním ze seznamu. Vytvořte proměnnou.
   
        $locName = "centralus"
3. Nahraďte hodnotu **$rgName** názvem pro novou skupinu prostředků. Vytvořte proměnnou a skupinu prostředků.
   
        $rgName = "mygroup1"
        New-AzureRmResourceGroup -Name $rgName -Location $locName

## Krok3: Vytvoření účtu úložiště
[Účet úložiště](../storage/storage-introduction.md) je nutný k uložení virtuálního pevného disku používaného virtuálním počítačem, který vytvoříte.

1. Nahraďte hodnotu **$stName** názvem pro účet úložiště. Otestujte, že je název jedinečný.
   
        $stName = "mystorage1"
        Get-AzureRmStorageAccountNameAvailability $stName
   
    Pokud tento příkaz vrátí hodnotu **False**, je navržený název v rámci Azure jedinečný. Názvy účtů úložiště musí mít od 3 do 24 znaků a můžou obsahovat jenom číslice a malá písmena.
2. Teď spuštěním příkazu vytvořte účet úložiště.
   
        $storageAcc = New-AzureRmStorageAccount -ResourceGroupName $rgName -Name $stName -SkuName "Standard_LRS" -Kind "Storage" -Location $locName

## Krok 4: Vytvoření virtuální sítě
Všechny virtuální počítače jsou součástí [virtuální sítě](../virtual-network/virtual-networks-overview.md).

1. Nahraďte hodnotu **$subnetName** názvem podsítě. Vytvořte proměnnou a podsíť.
   
        $subnetName = "mysubnet1"
        $singleSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix 10.0.0.0/24
2. Nahraďte hodnotu **$vnetName** názvem virtuální sítě. Vytvořte proměnnou a virtuální síť s podsítí.
   
        $vnetName = "myvnet1"
        $vnet = New-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $rgName -Location $locName -AddressPrefix 10.0.0.0/16 -Subnet $singleSubnet
   
    Použijte hodnoty, které dávají smysl pro vaši aplikaci a prostředí.

## Krok 5: Vytvoření veřejné IP adresy a síťového rozhraní
Pokud chcete povolit komunikaci s virtuálním počítačem ve virtuální síti, budete potřebovat [veřejnou adresu IP](../virtual-network/virtual-network-ip-addresses-overview-arm.md) a síťové rozhraní.

1. Nahraďte hodnotu **$ipName** názvem pro veřejnou IP adresu. Vytvořte proměnnou a veřejnou IP adresu.
   
        $ipName = "myIPaddress1"
        $pip = New-AzureRmPublicIpAddress -Name $ipName -ResourceGroupName $rgName -Location $locName -AllocationMethod Dynamic
2. Nahraďte hodnotu **$nicName** názvem pro síťové rozhraní. Vytvořte proměnnou a síťové rozhraní.
   
        $nicName = "mynic1"
        $nic = New-AzureRmNetworkInterface -Name $nicName -ResourceGroupName $rgName -Location $locName -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id

## Krok 6: Vytvoření virtuálního počítače
Teď, když máte vytvořené všechno potřebné, je čas vytvořit si virtuální počítač.

1. Spuštěním příkazu nastavte název účtu správce a heslo pro virtuální počítač.
   
        $cred = Get-Credential -Message "Type the name and password of the local administrator account."
   
    Heslo musí být 12 až 123 znaků dlouhé a musí obsahovat minimálně jedno malé písmeno, jedno velké písmeno, jednu číslici a jeden speciální znak. 
2. Nahraďte hodnotu **$vmName** názvem pro virtuální počítač. Vytvořte proměnnou a konfiguraci virtuálního počítače.
   
        $vmName = "myvm1"
        $vm = New-AzureRmVMConfig -VMName $vmName -VMSize "Standard_A1"
   
    Projděte si téma [Velikosti virtuálních počítačů v Azure](virtual-machines-windows-sizes.md), kde najdete seznam dostupných velikostí pro virtuální počítač.
3. Nahraďte hodnotu **$compName** názvem počítače pro virtuální počítač. Vytvořte proměnnou a přidejte informace o operačním systému do konfigurace.
   
        $compName = "myvm1"
        $vm = Set-AzureRmVMOperatingSystem -VM $vm -Windows -ComputerName $compName -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
4. Definujte image, která se má použít ke zřízení virtuálního počítače. 
   
        $vm = Set-AzureRmVMSourceImage -VM $vm -PublisherName MicrosoftWindowsServer -Offer WindowsServer -Skus 2012-R2-Datacenter -Version "latest"
   
    Další informace o výběru image, která se má použít, najdete v tématu [Vyhledání a výběr imagí virtuálních počítačů v Azure pomocí PowerShellu nebo CLI](virtual-machines-windows-cli-ps-findimage.md).
5. Přidejte síťové rozhraní, které jste vytvořili, do konfigurace.
   
        $vm = Add-AzureRmVMNetworkInterface -VM $vm -Id $nic.Id
6. Nahraďte hodnotu **$blobPath** cestou a názvem souboru v úložišti virtuálního pevného disku. Soubor virtuálního pevného disku je obvykle uložen v kontejneru, například **vhds/WindowsVMosDisk.vhd**. Vytvořte proměnné.
   
        $blobPath = "vhds/WindowsVMosDisk.vhd"
        $osDiskUri = $storageAcc.PrimaryEndpoints.Blob.ToString() + $blobPath
7. Nahraďte hodnotu **$diskName** názvem pro disk operačního systému. Vytvořte proměnnou a přidejte informace o disku do konfigurace.
   
        $diskName = "windowsvmosdisk"
        $vm = Set-AzureRmVMOSDisk -VM $vm -Name $diskName -VhdUri $osDiskUri -CreateOption fromImage
8. Nakonec vytvořte virtuální počítač.
   
        New-AzureRmVM -ResourceGroupName $rgName -Location $locName -VM $vm
   
    V okně PowerShellu byste měli vidět skupinu prostředků, všechny její prostředky na Portálu Azure a stav úspěšného dokončení:
   
        RequestId  IsSuccessStatusCode  StatusCode  ReasonPhrase
        ---------  -------------------  ----------  ------------
                                  True          OK  OK

## Další kroky
* Pokud byly nějaké problémy s nasazením, je dalším krokem projít si téma [Řešení potíží s nasazením skupin prostředků pomocí webu Azure Portal](../resource-manager-troubleshoot-deployments-portal.md).
* Projděte si téma [Správa virtuálních počítačů pomocí Azure Resource Manageru a prostředí PowerShell](virtual-machines-windows-ps-manage.md), kde najdete informace o tom, jak spravovat virtuální počítač, který jste vytvořili.
* Podle informací v tématu [Vytvoření virtuálního počítače s Windows pomocí šablony Resource Manageru](virtual-machines-windows-ps-template.md) můžete vytvořit virtuální počítač pomocí šablony.

<!--HONumber=Sep16_HO5-->


