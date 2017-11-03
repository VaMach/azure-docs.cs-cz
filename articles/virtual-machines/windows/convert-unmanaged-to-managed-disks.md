---
title: "Převod virtuálního počítače s Windows nespravované disky na spravovaných disků - disků spravované Azure | Microsoft Docs"
description: "Jak převést virtuální počítač s Windows z nespravovaných disků na discích spravovaných pomocí prostředí PowerShell v modelu nasazení Resource Manager"
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: cynthn
ms.openlocfilehash: 445117371fde91d0a0fcb96f06e42e2033692789
ms.sourcegitcommit: 4ed3fe11c138eeed19aef0315a4f470f447eac0c
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/23/2017
---
# <a name="convert-a-windows-virtual-machine-from-unmanaged-disks-to-managed-disks"></a>Převod virtuálního počítače s Windows nespravovaných disky na spravované disky

Pokud máte existující Windows virtuální počítače (VM) používající nespravované disky, můžete převést virtuální počítače použít spravované disky prostřednictvím [Azure spravované disky](managed-disks-overview.md) služby. Tento proces převede disk operačního systému a všechny připojené datových disků.

Tento článek ukazuje, jak převést virtuální počítače pomocí prostředí Azure PowerShell. Pokud je potřeba nainstalovat nebo upgradovat najdete v tématu [instalace a konfigurace prostředí Azure PowerShell](/powershell/azure/install-azurerm-ps).

## <a name="before-you-begin"></a>Než začnete


* Zkontrolujte [plánování migrace na spravované disky](on-prem-to-azure.md#plan-for-the-migration-to-managed-disks).

[!INCLUDE [virtual-machines-common-convert-disks-considerations](../../../includes/virtual-machines-common-convert-disks-considerations.md)]




## <a name="convert-single-instance-vms"></a>Převést virtuální počítače jednou instancí
Tato část popisuje jak převést virtuální počítače Azure jednou instancí z nespravovaných disků na spravované disky. (Pokud jsou vaše virtuální počítače v nastavení dostupnosti, najdete v další části.) 

1. Zrušit přidělení virtuálního počítače pomocí [Stop-AzureRmVM](/powershell/module/azurerm.compute/stop-azurermvm) rutiny. Následující příklad zruší přidělení virtuálního počítače s názvem `myVM` ve skupině prostředků s názvem `myResourceGroup`: 

  ```azurepowershell-interactive
  $rgName = "myResourceGroup"
  $vmName = "myVM"
  Stop-AzureRmVM -ResourceGroupName $rgName -Name $vmName -Force
  ```

2. Převeďte virtuální počítač na disky spravované pomocí [ConvertTo-AzureRmVMManagedDisk](/powershell/module/azurerm.compute/convertto-azurermvmmanageddisk) rutiny. Následující proces převede předchozí virtuálních počítačů, včetně disku operačního systému a všechny datové disky:

  ```azurepowershell-interactive
  ConvertTo-AzureRmVMManagedDisk -ResourceGroupName $rgName -VMName $vmName
  ```

3. Spusťte virtuální počítač po převodu na spravované disky pomocí [Start-AzureRmVM](/powershell/module/azurerm.compute/start-azurermvm). Následující příklad restartuje předchozí virtuální počítač:

  ```azurepowershell-interactive
  Start-AzureRmVM -ResourceGroupName $rgName -Name $vmName
  ```


## <a name="convert-vms-in-an-availability-set"></a>Převést virtuální počítače v nastavení dostupnosti

Pokud virtuální počítače, které chcete převést na spravované disky jsou v nastavení dostupnosti, je nutné nejprve převést skupinu dostupnosti do skupiny spravované dostupnosti.

1. Převést skupinu dostupnosti pomocí [aktualizace AzureRmAvailabilitySet](/powershell/module/azurerm.compute/update-azurermavailabilityset) rutiny. Následující příklad aktualizuje skupinu dostupnosti s názvem `myAvailabilitySet` ve skupině prostředků s názvem `myResourceGroup`:

  ```azurepowershell-interactive
  $rgName = 'myResourceGroup'
  $avSetName = 'myAvailabilitySet'

  $avSet = Get-AzureRmAvailabilitySet -ResourceGroupName $rgName -Name $avSetName
  Update-AzureRmAvailabilitySet -AvailabilitySet $avSet -Sku Aligned 
  ```

  Pokud oblast, kde vaše dostupnosti nastavit nachází má pouze 2 domén spravované selhání, ale počet domén selhání nespravované 3, tento příkaz zobrazí chybu podobná "zadaný počet domén selhání 3 musí spadat do rozsahu 1 až 2." Opravte případné chyby, aktualizujte doméně selhání 2 a update `Sku` k `Aligned` následujícím způsobem:

  ```azurepowershell-interactive
  $avSet.PlatformFaultDomainCount = 2
  Update-AzureRmAvailabilitySet -AvailabilitySet $avSet -Sku Aligned
  ```

2. Deallocate a převést virtuální počítače v sadě dostupnosti. Následující skript zruší přidělení každý virtuální počítač pomocí [Stop-AzureRmVM](/powershell/module/azurerm.compute/stop-azurermvm) rutiny, převede ji na základě [ConvertTo-AzureRmVMManagedDisk](/powershell/module/azurerm.compute/convertto-azurermvmmanageddisk)a restartuje s použitím [Start-AzureRmVM](/powershell/module/azurerm.compute/start-azurermvm):

  ```azurepowershell-interactive
  $avSet = Get-AzureRmAvailabilitySet -ResourceGroupName $rgName -Name $avSetName

  foreach($vmInfo in $avSet.VirtualMachinesReferences)
  {
     $vm = Get-AzureRmVM -ResourceGroupName $rgName | Where-Object {$_.Id -eq $vmInfo.id}
     Stop-AzureRmVM -ResourceGroupName $rgName -Name $vm.Name -Force
     ConvertTo-AzureRmVMManagedDisk -ResourceGroupName $rgName -VMName $vm.Name
     Start-AzureRmVM -ResourceGroupName $rgName -Name $vm.Name
  }
  ```


## <a name="troubleshooting"></a>Řešení potíží

Pokud dojde k chybě při převodu, nebo pokud je virtuální počítač ve stavu selhání kvůli problémům v předchozí převod, spusťte `ConvertTo-AzureRmVMManagedDisk` rutinu znovu. Jednoduché opakování obvykle odblokuje situaci.


## <a name="next-steps"></a>Další kroky

[Převést standardní spravovaných disků na premium](convert-disk-storage.md)

Využít jen pro čtení kopie virtuálního počítače pomocí [snímky](snapshot-copy-managed-disk.md).

