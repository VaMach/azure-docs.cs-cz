---
title: "Správa disků virtuálních počítačů v Azure zásobníku | Microsoft Docs"
description: "Zřídit disky pro virtuální počítače pro Azure zásobníku."
services: azure-stack
documentationcenter: 
author: brenduns
manager: femila
editor: 
ms.assetid: 4e5833cf-4790-4146-82d6-737975fb06ba
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 12/14/2017
ms.author: brenduns
ms.reviewer: jiahan
ms.openlocfilehash: 0c36e2eaaf2d266842b2b7de0b0c8dc0ed1e0145
ms.sourcegitcommit: 3fca41d1c978d4b9165666bb2a9a1fe2a13aabb6
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/15/2017
---
# <a name="virtual-machine-disk-storage-for-azure-stack"></a>Disku úložiště virtuálního počítače pro Azure zásobníku

*Platí pro: Azure zásobníku integrované systémy a Azure zásobníku Development Kit*

Azure zásobníku podporuje použití [nespravované disky](https://docs.microsoft.com/azure/virtual-machines/windows/about-disks-and-vhds#unmanaged-disks) ve virtuálním počítači jako disk operačního systému (OS) i datový disk. Použít nespravované disky, můžete vytvořit [účet úložiště](https://docs.microsoft.com/azure/storage/common/storage-create-storage-account) a potom uložte disky jako objekty BLOB stránky v kontejnerech v rámci účtu úložiště. Tyto disky jsou pak označovaná jako disky virtuálních počítačů.

Pokud chcete zvýšit výkon a snížit náklady na správu systému Azure zásobníku, doporučujeme že umístit každý disk virtuálního počítače v samostatných kontejneru. Kontejner by měl obsahovat disku operačního systému nebo datový disk, ale nikoli pro obě současně. Ale neexistuje žádné omezení, která zabraňuje uvedení i do kontejneru.

Pokud přidáte jednu nebo více datových disků pro virtuální počítač, naplánujte použití dalších kontejnerů jako umístění pro uložení těchto disků. Jako datové disky disk operačního systému pro další virtuální počítače musí být také ve své vlastní oddělené kontejnery.

Když vytvoříte víc virtuálních počítačů, můžete znovu použít stejný účet úložiště pro každý nový virtuální počítač. Kontejnery, které vytvoříte, musí být jedinečné.  

Přidat disky pro virtuální počítač, použijte portál user portal nebo prostředí PowerShell.

| Metoda | Možnosti
|-|-|
|[Portál User portal](#use-the-portal-to-add-additional-disks-to-a-vm)|-Přidáte nové datové disky pro virtuální počítač, který byl dříve zajištěny. Nové disky jsou vytvořené pomocí Azure zásobníku. </br> </br>-Přidáte existující soubor VHD jako disk k virtuálnímu počítači, který byl dříve zajištěny. K tomu musíte připravit a nahrát soubor VHD do protokolů Azure. |
|[PowerShell](#use-powershell-to-add-multiple-unmanaged-disks-to-a-vm) | -Vytvořit nový virtuální počítač s diskem operačního systému a současně přidat jeden nebo více datových disků do tohoto virtuálního počítače. |


## <a name="use-the-portal-to-add-additional-disks-to-a-vm"></a>Přidejte další disky do virtuálního počítače pomocí portálu
Ve výchozím nastavení je při použití portálu k vytvoření virtuálního počítače pro většinu položek marketplace, vytvořit jenom disk s operačním systémem. Disky vytvořené pomocí Azure se označují jako spravované disky.

Po zřízení virtuálního počítače, můžete na portálu přidejte nový datový disk nebo stávající datový disk do tohoto virtuálního počítače. Každý další disk měly být umístěny v samostatných kontejneru. Disky, které přidáte do virtuálního počítače se označují jako nespravované disky.

### <a name="use-the-portal-to-attach-a-new-data-disk-to-a-vm"></a>Připojit nový datový disk k virtuálnímu počítači pomocí portálu

1.  Na portálu, klikněte na tlačítko **virtuální počítače**.    
    ![Příklad: Virtuálních počítačů řídicí panel](media/azure-stack-manage-vm-disks/vm-dashboard.png)

2.  Vyberte virtuální počítač, který má dříve zajištěny.   
    ![Příklad: Vyberte virtuální počítač v řídicím panelu](media/azure-stack-manage-vm-disks/select-a-vm.png)

3.  Virtuální počítač, klikněte na tlačítko **disky** > **připojit nový**.       
    ![Příklad: Připojte nový disk k virtuálnímu počítači](media/azure-stack-manage-vm-disks/Attach-disks.png)    

4.  V **připojit nový disk** podokně klikněte na tlačítko **umístění**. Ve výchozím umístění nastavena do kontejneru, která obsahuje disk operačního systému.      
    ![Příklad: Nastavte na místo na disku](media/azure-stack-manage-vm-disks/disk-location.png)

5.  Vyberte **účet úložiště** používat. Potom vyberte **kontejneru** místo, kam chcete umístit datový disk. Z **kontejnery** stránky, pokud chcete, můžete vytvořit nový kontejner. Pro vlastní kontejner můžete změnit umístění pro nový disk. Pokud použijete samostatné kontejner pro každý disk, je třeba distribuovat umístění datový disk, který může zlepšit výkon. Klikněte na tlačítko **vyberte** uložení výběru.     
    ![Příklad: Vyberte kontejner](media/azure-stack-manage-vm-disks/select-container.png)

6.  V **připojit nový disk** stránky, aktualizovat **název**, **typ**, **velikost**, a **použití mezipaměti u hostitele** nastavení disku. Pak klikněte na tlačítko **OK** uložíte novou konfiguraci disku pro virtuální počítač.  
    ![Příklad: Dokončení disku přílohy](media/azure-stack-manage-vm-disks/complete-disk-attach.png)  

7.  Po zásobník Azure vytvoří disk a připojí jej k virtuálnímu počítači, na nový disk, je uvedena v nastavení disku virtuálního počítače v části **datových disků**.   
    ![Příklad: Zobrazení disku](media/azure-stack-manage-vm-disks/view-data-disk.png)


### <a name="attach-an-existing-data-disk-to-a-vm"></a>Připojit stávající datový disk k virtuálnímu počítači
1.  [Připravte si soubor VHD](https://docs.microsoft.com/azure/virtual-machines/windows/classic/createupload-vhd) pro použití jako datový disk pro virtuální počítač. Tento soubor VHD nahrajte na účet úložiště, který používáte s virtuálním Počítačem, který chcete připojit soubor VHD.

  Chcete použít jiný kontejner pro uložení souboru VHD než kontejner, který obsahuje disk operačního systému.   
  ![Příklad: Nahrát soubor virtuálního pevného disku](media/azure-stack-manage-vm-disks/upload-vhd.png)

2.  Po odeslání souboru VHD, jste připraveni připojit virtuální pevný disk k virtuálnímu počítači. V nabídce na levé straně klikněte na tlačítko **virtuální počítače**.  
 ![Příklad: Vyberte virtuální počítač v řídicím panelu](media/azure-stack-manage-vm-disks/vm-dashboard.png)

3.  Ze seznamu vyberte virtuální počítač.    
  ![Příklad: Vyberte virtuální počítač v řídicím panelu](media/azure-stack-manage-vm-disks/select-a-vm.png)

4.  Na stránce pro virtuální počítač, klikněte na **disky** > **připojit existující**.   
  ![Příklad: Připojit stávající disk](media/azure-stack-manage-vm-disks/attach-disks2.png)

5.  V **připojit stávající disk** klikněte na tlačítko **souboru virtuálního pevného disku**. **Účty úložiště** otevře se stránka.    
  ![Příklad: Vyberte soubor virtuálního pevného disku](media/azure-stack-manage-vm-disks/select-vhd.png)

6.  V části **účty úložiště**, vyberte účet, který chcete použít a pak vyberte kontejner, který obsahuje soubor VHD, který jste dříve nahráli. Vyberte soubor VHD a pak klikněte na tlačítko **vyberte** uložení výběru.    
  ![Příklad: Vyberte kontejner](media/azure-stack-manage-vm-disks/select-container2.png)

7.  V části **připojit stávající disk**, vybraný soubor je uveden v části **souboru virtuálního pevného disku**. Aktualizace **použití mezipaměti u hostitele** nastavení disku a pak klikněte na **OK** uložíte novou konfiguraci disku pro virtuální počítač.    
  ![Příklad: Připojení souboru VHD](media/azure-stack-manage-vm-disks/attach-vhd.png)

8.  Po zásobník Azure vytvoří disk a připojí jej k virtuálnímu počítači, na nový disk, je uvedena v nastavení disku virtuálního počítače v části **datových disků**.   
  ![Příklad: Dokončení disk připojit](media/azure-stack-manage-vm-disks/complete-disk-attach.png)


## <a name="use-powershell-to-add-multiple-unmanaged-disks-to-a-vm"></a>Přidat k virtuálnímu počítači více nespravované disky pomocí prostředí PowerShell
Můžete použít PowerShell zřízení virtuálního počítače a přidejte nový disk data nebo připojit existující **VHD** souboru jako datový disk.

**Přidat AzureRmVMDataDisk** rutiny přidá datový disk k virtuálnímu počítači. Pokud vytvoříte virtuální počítač nebo datový disk můžete přidat do existujícího virtuálního počítače, můžete přidat datový disk. Zadejte **VhdUri** parametr distribuovat disky jiných kontejnerů.

### <a name="add-data-disks-to-a-new-virtual-machine"></a>Přidat disky dat do nového virtuálního počítače
Následující příklady použít příkazy prostředí PowerShell k vytvoření virtuálního počítače s tři datové disky, každý umístěny v různých kontejneru.

První příkaz vytvoří objekt virtuálního počítače a uloží jej v *$VirtualMachine* proměnné. Příkaz přiřadí názvem a velikostí virtuálního počítače.
  ```
  $VirtualMachine = New-AzureRmVMConfig -VMName "VirtualMachine" `
                                      -VMSize "Standard_A2"
  ```

Následující tři příkazy přiřadit cesty tří datových disků, aby *$DataDiskVhdUri01*, *$DataDiskVhdUri02*, a *$DataDiskVhdUri03* proměnné. Definujte název jinou cestu v adrese URL pro distribuci disky jiných kontejnerů.     
  ```
  $DataDiskVhdUri01 = "https://contoso.blob.local.azurestack.external/test1/data1.vhd"
  ```

  ```
  $DataDiskVhdUri02 = "https://contoso.blob.local.azurestack.external/test2/data2.vhd"
  ```

  ```
  $DataDiskVhdUri03 = "https://contoso.blob.local.azurestack.external/test3/data3.vhd"
  ```

Poslední tři příkazy přidat datových disků pro virtuální počítač uložen v *$VirtualMachine*. Každý příkaz určuje název, umístění a další vlastnosti disku. Identifikátor URI každý disk je uložený v *$DataDiskVhdUri01*, *$DataDiskVhdUri02*, a *$DataDiskVhdUri03*.
  ```
  $VirtualMachine = Add-AzureRmVMDataDisk -VM $VirtualMachine -Name 'DataDisk1' `
                  -Caching 'ReadOnly' -DiskSizeInGB 10 -Lun 0 `
                  -VhdUri $DataDiskVhdUri01 -CreateOption Empty
  ```

  ```
  $VirtualMachine = Add-AzureRmVMDataDisk -VM $VirtualMachine -Name 'DataDisk2' `
                 -Caching 'ReadOnly' -DiskSizeInGB 11 -Lun 1 `
                 -VhdUri $DataDiskVhdUri02 -CreateOption Empty
  ```

  ```
  $VirtualMachine = Add-AzureRmVMDataDisk -VM $VirtualMachine -Name 'DataDisk3' `
                  -Caching 'ReadOnly' -DiskSizeInGB 12 -Lun 2 `
                  -VhdUri $DataDiskVhdUri03 -CreateOption Empty
  ```

Pomocí následujících příkazů prostředí PowerShell přidejte disk a síťové konfigurace operačního systému na virtuální počítač a pak spusťte nový virtuální počítač.
  ```
  #set variables
  $rgName = "myResourceGroup"
  $location = "local"
  #Set OS Disk
  $osDiskUri = "https://contoso.blob.local.azurestack.external/vhds/osDisk.vhd"
  $osDiskName = "osDisk"

  $VirtualMachine = Set-AzureRmVMOSDisk -VM $VirtualMachine -Name $osDiskName -VhdUri $osDiskUri `
      -CreateOption FromImage -Windows

  # Create a subnet configuration
  $subnetName = "mySubNet"
  $singleSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix 10.0.0.0/24

  # Create a vnet configuration
  $vnetName = "myVnetName"
  $vnet = New-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $rgName -Location $location `
      -AddressPrefix 10.0.0.0/16 -Subnet $singleSubnet

  # Create a public IP
  $ipName = "myIP"
  $pip = New-AzureRmPublicIpAddress -Name $ipName -ResourceGroupName $rgName -Location $location `
      -AllocationMethod Dynamic

  # Create a network security group cnfiguration
  $nsgName = "myNsg"
  $rdpRule = New-AzureRmNetworkSecurityRuleConfig -Name myRdpRule -Description "Allow RDP" `
      -Access Allow -Protocol Tcp -Direction Inbound -Priority 110 `
      -SourceAddressPrefix Internet -SourcePortRange * `
      -DestinationAddressPrefix * -DestinationPortRange 3389
  $nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName $rgName -Location $location `
      -Name $nsgName -SecurityRules $rdpRule

  # Create a NIC configuration
  $nicName = "myNicName"
  $nic = New-AzureRmNetworkInterface -Name $nicName -ResourceGroupName $rgName `
  -Location $location -SubnetId $vnet.Subnets[0].Id -NetworkSecurityGroupId $nsg.Id -PublicIpAddressId $pip.Id

  #Create the new VM
  $VirtualMachine = Set-AzureRmVMOperatingSystem -VM $VirtualMachine -Windows -ComputerName VirtualMachine | `
      Set-AzureRmVMSourceImage -PublisherName MicrosoftWindowsServer -Offer WindowsServer `
      -Skus 2016-Datacenter -Version latest | Add-AzureRmVMNetworkInterface -Id $nic.Id
  New-AzureRmVM -ResourceGroupName $rgName -Location $location -VM $VirtualMachine
  ```



### <a name="add-data-disks-to-an-existing-virtual-machine"></a>Přidat disky data do existujícího virtuálního počítače
Následující příklady použití příkazů prostředí PowerShell přidáte tři datových disků na existující virtuální počítač.
V prvním příkazu je získán virtuální počítač s názvem virtuální počítač pomocí **Get-AzureRmVM** rutiny. Příkaz uloží virtuální počítač v *$VirtualMachine* proměnné.
  ```
  $VirtualMachine = Get-AzureRmVM -ResourceGroupName "myResourceGroup" `
                                  -Name "VirtualMachine"
  ```
Následující tři příkazy přiřadit $DataDiskVhdUri01 $DataDiskVhdUri02 a $DataDiskVhdUri03 proměnné cest tři datových disků.  Názvy jinou cestu v vhduri znamenat různé kontejnery pro umístění disku.
  ```
  $DataDiskVhdUri01 = "https://contoso.blob.local.azurestack.external/test1/data1.vhd"
  ```
  ```
  $DataDiskVhdUri02 = "https://contoso.blob.local.azurestack.external/test2/data2.vhd"
  ```
  ```
  $DataDiskVhdUri03 = "https://contoso.blob.local.azurestack.external/test3/data3.vhd"
  ```


  Následující tři příkazy přidat do virtuálního počítače uloženého v datových disků *$VirtualMachine* proměnné. Každý příkaz určuje název, umístění a další vlastnosti disku. Identifikátor URI každý disk je uložený v *$DataDiskVhdUri01*, *$DataDiskVhdUri02*, a *$DataDiskVhdUri03*.
  ```
  Add-AzureRmVMDataDisk -VM $VirtualMachine -Name "disk1" `
                        -VhdUri $DataDiskVhdUri01 -LUN 0 `
                        -Caching ReadOnly -DiskSizeinGB 10 -CreateOption Empty
  ```
  ```
  Add-AzureRmVMDataDisk -VM $VirtualMachine -Name "disk2" `
                        -VhdUri $DataDiskVhdUri02 -LUN 1 `
                        -Caching ReadOnly -DiskSizeinGB 11 -CreateOption Empty
  ```
  ```
  Add-AzureRmVMDataDisk -VM $VirtualMachine -Name "disk3" `
                        -VhdUri $DataDiskVhdUri03 -LUN 2 `
                        -Caching ReadOnly -DiskSizeinGB 12 -CreateOption Empty
  ```


  Poslední příkaz aktualizuje stav virtuálního počítače uloženého v *$VirtualMachine* v -*ResourceGroupName*.
  ```
  Update-AzureRmVM -ResourceGroupName "myResourceGroup" -VM $VirtualMachine
  ```
<!-- Pending scripts  

## Distribute the data disks of an existing VM
If you have a VM with more than one disk in the same container, the service operator of the Azure Stack deployment might ask you to redistribute the disks into individual containers.

To do so, use the scripts from the following location in GitHub. These scripts can be used to move the data disks to different containers.
-->

## <a name="next-steps"></a>Další kroky
Další informace o virtuálních počítačích Azure zásobníku najdete v tématu [důležité informace pro virtuální počítače v Azure zásobníku](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-vm-considerations).
