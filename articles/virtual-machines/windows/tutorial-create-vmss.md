---
title: "Vytvoření sady škálování virtuálního počítače pro Windows v Azure | Microsoft Docs"
description: "Vytvoření a nasazení vysoce dostupné aplikace na virtuálních počítačích Windows pomocí sady škálování virtuálního počítače"
services: virtual-machine-scale-sets
documentationcenter: 
author: iainfoulds
manager: jeconnoc
editor: 
tags: 
ms.assetid: 
ms.service: virtual-machine-scale-sets
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: 
ms.topic: article
ms.date: 12/15/2017
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: d190d046f7572c51df0c5c9e14e14a41d93e3248
ms.sourcegitcommit: 68aec76e471d677fd9a6333dc60ed098d1072cfc
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/18/2017
---
# <a name="create-a-virtual-machine-scale-set-and-deploy-a-highly-available-app-on-windows"></a>Vytvořit sadu škálování virtuálního počítače a nasazení vysoce dostupné aplikace v systému Windows
Škálovací sadu virtuálních počítačů můžete nasadit a spravovat sadu identické, automatické škálování virtuálních počítačů. Můžete škálovat počet virtuálních počítačů v sadě škálování ručně, nebo definovat pravidla pro automatické škálování podle využití prostředků, jako je například CPU, paměť vyžádání či síťový provoz. V tomto kurzu nasadíte škálování virtuálních počítačů, nastavte v Azure. Získáte informace o těchto tématech:

> [!div class="checklist"]
> * Použijte k definování stránku IIS škálování rozšíření vlastních skriptů
> * Vytvořit nástroj pro vyrovnávání zatížení pro škálovací sadu
> * Vytvoření sady škálování virtuálního počítače
> * Zvýšení nebo snížení počtu instancí v sadě škálování
> * Vytvoření pravidel škálování

Tento kurz vyžaduje prostředí Azure PowerShell verze modulu 5.1.1 nebo novější. Verzi zjistíte spuštěním příkazu ` Get-Module -ListAvailable AzureRM`. Pokud potřebujete upgrade, přečtěte si téma [Instalace modulu Azure PowerShell](/powershell/azure/install-azurerm-ps).


## <a name="scale-set-overview"></a>Přehled sady škálování
Škálovací sadu virtuálních počítačů můžete nasadit a spravovat sadu identické, automatické škálování virtuálních počítačů. Virtuální počítače ve škálovací sadě jsou rozmístěny v doménách selhání a aktualizace logiku v jednom nebo více *umístění skupiny*. Toto jsou skupiny podobně nakonfigurovaných virtuálních počítačů, podobně jako [skupiny dostupnosti](tutorial-availability-sets.md).

Podle potřeby ve škálovací sadě virtuálních počítačů vytvořeny. Můžete definovat pravidla škálování řídit, jak a kdy se přidají nebo odeberou ze sady škálování virtuálních počítačů. Tato pravidla můžete spustit na základě metriky například zatížení procesoru, využití paměti nebo síťový provoz.

Pokud používáte image platformy Azure sadách škálování podporu až 1 000 virtuálních počítačů. Pro úlohy s významné instalace nebo požadavky přizpůsobení virtuálního počítače, můžete chtít [vytvořit vlastní image virtuálního počítače](tutorial-custom-images.md). Můžete vytvořit až 300 virtuálních počítačů v škálování nastavit při použití vlastní image.


## <a name="create-an-app-to-scale"></a>Vytvoření aplikace pro škálování
Před vytvořením sady škálování, vytvořte skupinu prostředků s [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup). Následující příklad vytvoří skupinu prostředků s názvem *myResourceGroupAutomate* v *EastUS* umístění:

```powershell
New-AzureRmResourceGroup -ResourceGroupName myResourceGroupScaleSet -Location EastUS
```

V dřívějších kurzu, jste zjistili, jak [automatizovat konfiguraci](tutorial-automate-vm-deployment.md) pomocí rozšíření vlastních skriptů. Vytvoření konfigurace sady škálování a poté použít rozšíření vlastních skriptů k instalaci a konfiguraci služby IIS:

```powershell
# Create a config object
$vmssConfig = New-AzureRmVmssConfig `
    -Location EastUS `
    -SkuCapacity 2 `
    -SkuName Standard_DS2 `
    -UpgradePolicyMode Automatic

# Define the script for your Custom Script Extension to run
$publicSettings = @{
    "fileUris" = (,"https://raw.githubusercontent.com/Azure-Samples/compute-automation-configurations/master/automate-iis.ps1");
    "commandToExecute" = "powershell -ExecutionPolicy Unrestricted -File automate-iis.ps1"
}

# Use Custom Script Extension to install IIS and configure basic website
Add-AzureRmVmssExtension -VirtualMachineScaleSet $vmssConfig `
    -Name "customScript" `
    -Publisher "Microsoft.Compute" `
    -Type "CustomScriptExtension" `
    -TypeHandlerVersion 1.8 `
    -Setting $publicSettings
```

## <a name="create-scale-load-balancer"></a>Vytvořit nástroj pro vyrovnávání zatížení škálování
K nástroji pro vyrovnávání zatížení Azure je Vyrovnávání zatížení vrstvy 4 (TCP, UDP), která poskytuje vysokou dostupnost distribucí příchozí provoz mezi virtuálními počítači v pořádku. Sondu stavu nástroje pro vyrovnávání zatížení monitoruje zadaný port pro každý virtuální počítač a distribuuje jenom přenosy na provozní virtuální počítač. Další informace najdete v dalším kurzu na [jak načíst vyvážit virtuální počítače s Windows](tutorial-load-balancer.md).

Vytvořte Vyrovnávání zatížení, která má veřejnou IP adresu a distribuuje webové přenosy na portu 80:

```powershell
# Create a public IP address
$publicIP = New-AzureRmPublicIpAddress `
  -ResourceGroupName myResourceGroupScaleSet `
  -Location EastUS `
  -AllocationMethod Static `
  -Name myPublicIP

# Create a frontend and backend IP pool
$frontendIP = New-AzureRmLoadBalancerFrontendIpConfig `
  -Name myFrontEndPool `
  -PublicIpAddress $publicIP
$backendPool = New-AzureRmLoadBalancerBackendAddressPoolConfig -Name myBackEndPool

# Create the load balancer
$lb = New-AzureRmLoadBalancer `
  -ResourceGroupName myResourceGroupScaleSet `
  -Name myLoadBalancer `
  -Location EastUS `
  -FrontendIpConfiguration $frontendIP `
  -BackendAddressPool $backendPool

# Create a load balancer health probe on port 80
Add-AzureRmLoadBalancerProbeConfig -Name myHealthProbe `
  -LoadBalancer $lb `
  -Protocol tcp `
  -Port 80 `
  -IntervalInSeconds 15 `
  -ProbeCount 2

# Create a load balancer rule to distribute traffic on port 80
Add-AzureRmLoadBalancerRuleConfig `
  -Name myLoadBalancerRule `
  -LoadBalancer $lb `
  -FrontendIpConfiguration $lb.FrontendIpConfigurations[0] `
  -BackendAddressPool $lb.BackendAddressPools[0] `
  -Protocol Tcp `
  -FrontendPort 80 `
  -BackendPort 80

# Update the load balancer configuration
Set-AzureRmLoadBalancer -LoadBalancer $lb
```

## <a name="create-a-scale-set"></a>Vytvoření sady škálování
Nyní vytvoří škálování virtuálního počítače s [New-AzureRmVmss](/powershell/module/azurerm.compute/new-azurermvm). Následující příklad vytvoří škálování nastavení s názvem *myScaleSet*:

```powershell
# Reference a virtual machine image from the gallery
Set-AzureRmVmssStorageProfile $vmssConfig `
  -ImageReferencePublisher MicrosoftWindowsServer `
  -ImageReferenceOffer WindowsServer `
  -ImageReferenceSku 2016-Datacenter `
  -ImageReferenceVersion latest

# Set up information for authenticating with the virtual machine
Set-AzureRmVmssOsProfile $vmssConfig `
  -AdminUsername azureuser `
  -AdminPassword P@ssword! `
  -ComputerNamePrefix myVM

# Create the virtual network resources
$subnet = New-AzureRmVirtualNetworkSubnetConfig `
  -Name "mySubnet" `
  -AddressPrefix 10.0.0.0/24
$vnet = New-AzureRmVirtualNetwork `
  -ResourceGroupName "myResourceGroupScaleSet" `
  -Name "myVnet" `
  -Location "EastUS" `
  -AddressPrefix 10.0.0.0/16 `
  -Subnet $subnet
$ipConfig = New-AzureRmVmssIpConfig `
  -Name "myIPConfig" `
  -LoadBalancerBackendAddressPoolsId $lb.BackendAddressPools[0].Id `
  -SubnetId $vnet.Subnets[0].Id

# Attach the virtual network to the config object
Add-AzureRmVmssNetworkInterfaceConfiguration `
  -VirtualMachineScaleSet $vmssConfig `
  -Name "network-config" `
  -Primary $true `
  -IPConfiguration $ipConfig

# Create the scale set with the config object (this step might take a few minutes)
New-AzureRmVmss `
  -ResourceGroupName myResourceGroupScaleSet `
  -Name myScaleSet `
  -VirtualMachineScaleSet $vmssConfig
```

Trvá několik minut vytvořit a nakonfigurovat všechny škálovací sadu prostředků a virtuálních počítačů.


## <a name="test-your-app"></a>Testování aplikace
Chcete-li zobrazit váš web služby IIS v akci, získat veřejnou IP adresu nástroj pro vyrovnávání zatížení s [Get-AzureRmPublicIPAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress). Následující příklad, získá IP adresu pro *myPublicIP* vytvořené jako součást sady škálování:

```powershell
Get-AzureRmPublicIPAddress -ResourceGroupName myResourceGroupScaleSet -Name myPublicIP | select IpAddress
```

Zadejte veřejnou IP adresu ve webovém prohlížeči. Aplikace se zobrazí, včetně názvu hostitele virtuálního počítače, který nástroje pro vyrovnávání zatížení distribuován provoz:

![Spuštěné web služby IIS](./media/tutorial-create-vmss/running-iis-site.png)

Sad v akci škálování najdete můžete můžete vynutit aktualizujte webový prohlížeč zobrazit nástroje pro vyrovnávání zatížení provoz distribuovat mezi všechny virtuální počítače používající vaši aplikaci.


## <a name="management-tasks"></a>Úlohy správy
V průběhu cyklu škálovací sady můžete spustit jeden nebo více úloh správy. Kromě toho můžete vytvořit skripty, které automatizují různé úlohy životního cyklu. Azure PowerShell poskytuje rychlý způsob, jak provést tyto úlohy. Tady jsou několik běžných úloh.

### <a name="view-vms-in-a-scale-set"></a>Zobrazení virtuální počítače ve škálovací sadě
Chcete-li zobrazit seznam virtuálních počítačů spuštěných ve škálovací sadě, použijte [Get-AzureRmVmssVM](/powershell/module/azurerm.compute/get-azurermvmssvm) následujícím způsobem:

```powershell
# Get current scale set
$scaleset = Get-AzureRmVmss `
  -ResourceGroupName myResourceGroupScaleSet `
  -VMScaleSetName myScaleSet

# Loop through the instanaces in your scale set
for ($i=1; $i -le ($scaleset.Sku.Capacity - 1); $i++) {
    Get-AzureRmVmssVM -ResourceGroupName myResourceGroupScaleSet `
      -VMScaleSetName myScaleSet `
      -InstanceId $i
}
```


### <a name="increase-or-decrease-vm-instances"></a>Zvýšení nebo snížení instance virtuálních počítačů
Pokud chcete zobrazit počet instancí, které máte aktuálně v sadě škálování, použijte [Get-AzureRmVmss](/powershell/module/azurerm.compute/get-azurermvmss) a dotazovat se na *sku.capacity*:

```powershell
Get-AzureRmVmss -ResourceGroupName myResourceGroupScaleSet `
    -VMScaleSetName myScaleSet | `
    Select -ExpandProperty Sku
```

Potom můžete ručně zvýšení nebo snížení počtu virtuálních počítačů v sad s škálování [aktualizace AzureRmVmss](/powershell/module/azurerm.compute/update-azurermvmss). Následující příklad nastaví počet virtuálních počítačů ve vaší škálování nastavena na *3*:

```powershell
# Get current scale set
$scaleset = Get-AzureRmVmss `
  -ResourceGroupName myResourceGroupScaleSet `
  -VMScaleSetName myScaleSet

# Set and update the capacity of your scale set
$scaleset.sku.capacity = 3
Update-AzureRmVmss -ResourceGroupName myResourceGroupScaleSet `
    -Name myScaleSet `
    -VirtualMachineScaleSet $scaleset
```

Pokud se používá několik minut se aktualizovat zadaný počet instancí ve vaší škálování nastavit.


### <a name="configure-autoscale-rules"></a>Konfigurace pravidel automatického škálování
Místo ručně škálování počtu instancí ve vaší škálovací sady, můžete definovat pravidla automatického škálování. Tato pravidla monitorovat instance ve škálovací sadě a reagují odpovídajícím způsobem na základě metriky a prahové hodnoty, které definujete. Následující příklad škáluje se počet instancí jedním při průměrné zatížení procesoru je větší než 60 % během období 5 minut. Pokud průměrná zatížení procesoru pak klesne pod 30 % během období 5 minut, jsou škálovat instance v jednou instancí:

```powershell
# Define your scale set information
$mySubscriptionId = (Get-AzureRmSubscription).Id
$myResourceGroup = "myResourceGroupScaleSet"
$myScaleSet = "myScaleSet"
$myLocation = "East US"

# Create a scale up rule to increase the number instances after 60% average CPU usage exceeded for a 5-minute period
$myRuleScaleUp = New-AzureRmAutoscaleRule `
  -MetricName "Percentage CPU" `
  -MetricResourceId /subscriptions/$mySubscriptionId/resourceGroups/$myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/$myScaleSet `
  -Operator GreaterThan `
  -MetricStatistic Average `
  -Threshold 60 `
  -TimeGrain 00:01:00 `
  -TimeWindow 00:05:00 `
  -ScaleActionCooldown 00:05:00 `
  -ScaleActionDirection Increase `
  -ScaleActionValue 1

# Create a scale down rule to decrease the number of instances after 30% average CPU usage over a 5-minute period
$myRuleScaleDown = New-AzureRmAutoscaleRule `
  -MetricName "Percentage CPU" `
  -MetricResourceId /subscriptions/$mySubscriptionId/resourceGroups/$myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/$myScaleSet `
  -Operator LessThan `
  -MetricStatistic Average `
  -Threshold 30 `
  -TimeGrain 00:01:00 `
  -TimeWindow 00:05:00 `
  -ScaleActionCooldown 00:05:00 `
  -ScaleActionDirection Decrease `
  -ScaleActionValue 1

# Create a scale profile with your scale up and scale down rules
$myScaleProfile = New-AzureRmAutoscaleProfile `
  -DefaultCapacity 2  `
  -MaximumCapacity 10 `
  -MinimumCapacity 2 `
  -Rules $myRuleScaleUp,$myRuleScaleDown `
  -Name "autoprofile"

# Apply the autoscale rules
Add-AzureRmAutoscaleSetting `
  -Location $myLocation `
  -Name "autosetting" `
  -ResourceGroup $myResourceGroup `
  -TargetResourceId /subscriptions/$mySubscriptionId/resourceGroups/$myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/$myScaleSet `
  -AutoscaleProfiles $myScaleProfile
```

Další návrhu informace o použití automatického škálování, najdete v části [škálování osvědčené postupy](/azure/architecture/best-practices/auto-scaling).


## <a name="next-steps"></a>Další kroky
V tomto kurzu jste vytvořili škálovací sadu virtuálních počítačů. Naučili jste se tyto postupy:

> [!div class="checklist"]
> * Použijte k definování stránku IIS škálování rozšíření vlastních skriptů
> * Vytvořit nástroj pro vyrovnávání zatížení pro škálovací sadu
> * Vytvoření sady škálování virtuálního počítače
> * Zvýšení nebo snížení počtu instancí v sadě škálování
> * Vytvoření pravidel škálování

Přechodu na v dalším kurzu se dozvíte další informace o konceptech pro virtuální počítače Vyrovnávání zatížení.

> [!div class="nextstepaction"]
> [Vyrovnávat zatížení virtuálních počítačů](tutorial-load-balancer.md)
