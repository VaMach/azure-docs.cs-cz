---
title: "Spravovat virtuální počítače ve Škálovací sadě virtuálních počítačů | Microsoft Docs"
description: "Správa virtuálních počítačů v škálování virtuálních počítačů, nastavit pomocí prostředí Azure PowerShell."
services: virtual-machine-scale-sets
documentationcenter: 
author: Thraka
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: d35fa77a-de96-4ccd-a332-eb181d1f4273
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/27/2016
ms.author: adegeo
ms.openlocfilehash: d09a020b903e5f43afe03b86c675bcc1eb536cbc
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="manage-virtual-machines-in-a-virtual-machine-scale-set"></a>Spravovat virtuální počítače ve škálovací sadě virtuálních počítačů
Úkoly v tomto článku použijte ke správě virtuálních počítačů sady škálování virtuálního počítače.

Většinu úloh, které zahrnují spravuje virtuální počítač ve škálovací sadě vyžadovat, že znáte ID instance na počítač, který chcete spravovat. Můžete použít [Průzkumníka prostředků Azure](https://resources.azure.com) k vyhledání ID instance virtuálního počítače ve škálovací sadě. Pomocí Průzkumníka prostředků také ověřte stav úlohy, které dokončíte.

Projděte si článek [Jak nainstalovat a nakonfigurovat Azure PowerShell](/powershell/azure/overview), kde najdete informace o instalaci nejnovější verze prostředí Azure PowerShell, výběru předplatného a přihlášení k účtu.

## <a name="display-information-about-a-scale-set"></a>Zobrazit informace o sadě škálování
Můžete získat obecné informace o sadě škálování, která je také označována jako zobrazení instance. Nebo můžete získat podrobnější informace, například informace o prostředcích v sadě škálování.

Nahraďte hodnoty v uvozovkách název nebo vaší skupině prostředků a škálování nastavení a potom spusťte příkaz:

    Get-AzureRmVmss -ResourceGroupName "resource group name" -VMScaleSetName "scale set name"

Vrátí přibližně takto:

    Id                                          : /subscriptions/{sub-id}/resourceGroups/myrg1/providers/Microsoft.Compute/virtualMachineScaleSets/myvmss1
    Name                                        : myvmss1
    Type                                        : Microsoft.Compute/virtualMachineScaleSets
    Location                                    : centralus
    Sku                                         :
      Name                                      : Standard_A0
      Tier                                      : Standard
      Capacity                                  : 3
    UpgradePolicy                               :
      Mode                                      : Manual
    VirtualMachineProfile                       :
      OsProfile                                 :
        ComputerNamePrefix                      : vmss1
        AdminUsername                           : admin1
        WindowsConfiguration                    :
          ProvisionVMAgent                      : True
          EnableAutomaticUpdates                : True
    StorageProfile                              :
      ImageReference                            :
        Publisher                               : MicrosoftWindowsServer
        Offer                                   : WindowsServer
        Sku                                     : 2012-R2-Datacenter
        Version                                 : latest
      OsDisk                                    :
        Name                                    : vmssosdisk
        Caching                                 : ReadOnly
        CreateOption                            : FromImage
        VhdContainers[0]                        : https://astore.blob.core.windows.net/vmss
        VhdContainers[1]                        : https://gstore.blob.core.windows.net/vmss
        VhdContainers[2]                        : https://mstore.blob.core.windows.net/vmss
        VhdContainers[3]                        : https://sstore.blob.core.windows.net/vmss
        VhdContainers[4]                        : https://ystore.blob.core.windows.net/vmss
    NetworkProfile                              :
      NetworkInterfaceConfigurations[0]         :
        Name                                    : mync1
        Primary                                 : True
        IpConfigurations[0]                     :
          Name                                  : ip1
          Subnet                                :
            Id                                  : /subscriptions/{sub-id}/resourceGroups/myrg1/providers/Microsoft.Network/virtualNetworks/myvn1/subnets/mysn1
          LoadBalancerBackendAddressPools[0]    :
            Id                                  : /subscriptions/{sub-id}/resourceGroups/myrg1/providers/Microsoft.Network/loadBalancers/mylb1/backendAddressPools/bepool1
        LoadBalancerInboundNatPools[0]          :
            Id                                  : /subscriptions/{sub-id}/resourceGroups/myrg1/providers/Microsoft.Network/loadBalancers/mylb1/inboundNatPools/natpool1
    ExtensionProfile                            :
      Extensions[0]                             :
        Name                                    : Microsoft.Insights.VMDiagnosticsSettings
        Publisher                               : Microsoft.Azure.Diagnostics
        Type                                    : IaaSDiagnostics
        TypeHandlerVersion                      : 1.5
        AutoUpgradeMinorVersion                 : True
        Settings                                : {"xmlCfg":"...","storageAccount":"astore"}
    ProvisioningState                           : Succeeded

Nahraďte název vaší skupiny a škálování sady prostředků hodnoty v uvozovkách. Nahraďte  *#*  s identifikátorem instance virtuálního počítače, který chcete získat informace a potom ho spusťte:

    Get-AzureRmVmssVM -ResourceGroupName "resource group name" -VMScaleSetName "scale set name" -InstanceId #

Vrátí něco podobného jako v tomto příkladu:

    Id                            : /subscriptions/{sub-id}/resourceGroups/myrg1/providers/Microsoft.Compute/
                                    virtualMachineScaleSets/myvmss1/virtualMachines/0
    Name                          : myvmss1_0
    Type                          : Microsoft.Compute/virtualMachineScaleSets/virtualMachines
    Location                      : centralus
    InstanceId                    : 0
    Sku                           :
      Name                        : Standard_A0
      Tier                        : Standard
    LatestModelApplied            : True
    StorageProfile                :
      ImageReference              :
        Publisher                 : MicrosoftWindowsServer
        Offer                     : WindowsServer
        Sku                       : 2012-R2-Datacenter
        Version                   : 4.0.20160617
      OsDisk                      :
        OsType                    : Windows
        Name                      : vmssosdisk-os-0-e11cad52959b4b76a8d9f26c5190c4f8
        Vhd                       :
          Uri                     : https://astore.blob.core.windows.net/vmss/vmssosdisk-os-0-e11cad52959b4b76a8d9f26c5190c4f8.vhd
        Caching                   : ReadOnly
        CreateOption              : FromImage
    OsProfile                     :
      ComputerName                : myvmss1-0
      AdminUsername               : admin1
      WindowsConfiguration        :
        ProvisionVMAgent          : True
        EnableAutomaticUpdates    : True
    NetworkProfile                :
      NetworkInterfaces[0]        :
        Id                        : /subscriptions/{sub-id}/resourceGroups/myrg1/providers/Microsoft.Compute/virtualMachineScaleSets/
                                    myvmss1/virtualMachines/0/networkInterfaces/mync1
    ProvisioningState             : Succeeded
    Resources[0]                  :
      Id                          : /subscriptions/{sub-id}/resourceGroups/myrg1/providers/Microsoft.Compute/virtualMachines/
                                    myvmss1_0/extensions/Microsoft.Insights.VMDiagnosticsSettings
      Name                        : Microsoft.Insights.VMDiagnosticsSettings
      Type                        : Microsoft.Compute/virtualMachines/extensions
      Location                    : centralus
      Publisher                   : Microsoft.Azure.Diagnostics
      VirtualMachineExtensionType : IaaSDiagnostics
      TypeHandlerVersion          : 1.5
      AutoUpgradeMinorVersion     : True
      Settings                    : {"xmlCfg":"...","storageAccount":"astore"}
      ProvisioningState           : Succeeded

## <a name="start-a-virtual-machine-in-a-scale-set"></a>Spuštění virtuálního počítače ve škálovací sadě
Nahraďte název vaší skupiny a škálování sady prostředků hodnoty v uvozovkách. Nahraďte  *#*  s identifikátorem virtuálního počítače, který chcete spustit a potom ho spusťte:

    Start-AzureRmVmss -ResourceGroupName "resource group name" -VMScaleSetName "scale set name" -InstanceId #

V Průzkumníku prostředků, vidíme, že stav instance je **systémem**:

    "statuses": [
      {
        "code": "ProvisioningState/succeeded",
        "level": "Info",
        "displayStatus": "Provisioning succeeded",
        "time": "2016-03-15T02:10:08.0730839+00:00"
      },
      {
        "code": "PowerState/running",
        "level": "Info",
        "displayStatus": "VM running"
      }
    ]

Všechny virtuální počítače můžete spustit v sad není pomocí parametru - InstanceId škálování.

## <a name="stop-a-virtual-machine-in-a-scale-set"></a>Zastavit virtuální počítač ve škálovací sadě
Nahraďte název vaší skupiny a škálování sady prostředků hodnoty v uvozovkách. Nahraďte  *#*  s identifikátorem virtuálního počítače, který chcete zastavit a pak ho spusťte:

    Stop-AzureRmVmss -ResourceGroupName "resource group name" -VMScaleSetName "scale set name" -InstanceId #

V Průzkumníku prostředků, vidíme, že stav instance je **navrácena**:

    "statuses": [
      {
        "code": "ProvisioningState/succeeded",
        "level": "Info",
        "displayStatus": "Provisioning succeeded",
        "time": "2016-03-15T01:25:17.8792929+00:00"
      },
      {
        "code": "PowerState/deallocated",
        "level": "Info",
        "displayStatus": "VM deallocated"
      }
    ]

Pokud chcete zastavit virtuální počítač a není zrušit přidělení, použijte parametr - StayProvisioned. Není pomocí parametru - InstanceId můžete zastavit všechny virtuální počítače v sadě.

## <a name="restart-a-virtual-machine-in-a-scale-set"></a>Restartování virtuálního počítače ve škálovací sadě
Nahraďte název vaší skupiny prostředků a byly sadou škálování hodnoty v uvozovkách. Nahraďte  *#*  identifikátorem virtuální počítač, který chcete restartovat a potom ho spusťte:

    Restart-AzureRmVmss -ResourceGroupName "resource group name" -VMScaleSetName "scale set name" -InstanceId #

Není pomocí parametru - InstanceId se dá restartovat všechny virtuální počítače v sadě.

## <a name="remove-a-virtual-machine-from-a-scale-set"></a>Odebrání ze sady škálování virtuálního počítače
Nahraďte název vaší skupiny prostředků a byly sadou škálování hodnoty v uvozovkách. Nahraďte  *#*  identifikátorem virtuální počítač, který chcete odebrat a potom ho spusťte:  

    Remove-AzureRmVmss -ResourceGroupName "resource group name" –VMScaleSetName "scale set name" -InstanceId #

Škálovací sadu virtuálních počítačů najednou můžete odebrat není pomocí parametru - ID instance.

## <a name="change-the-capacity-of-a-scale-set"></a>Změna kapacity škálovací sadě
Můžete přidat nebo odebrat virtuální počítače tak, že změníte kapacita sady. Získáte škálovací sadu, který chcete změnit, nastavte kapacitu na co se má být a aktualizujte měřítka nastavit nové kapacity. V těchto příkazech nahraďte hodnoty v uvozovkách název vaší skupiny prostředků a byly sadou škálování.

    $vmss = Get-AzureRmVmss -ResourceGroupName "resource group name" -VMScaleSetName "scale set name"
    $vmss.sku.capacity = 5
    Update-AzureRmVmss -ResourceGroupName "resource group name" -Name "scale set name" -VirtualMachineScaleSet $vmss 

Pokud odebíráte ze sady škálování virtuálních počítačů, virtuálních počítačů s nejvyšší ID nejdřív odstranit.

