---
title: "Upravit sadu škálování virtuálního počítače Azure | Microsoft Docs"
description: "Upravit sadu škálování virtuálního počítače Azure"
services: virtual-machine-scale-sets
documentationcenter: 
author: gatneil
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: e229664e-ee4e-4f12-9d2e-a4f456989e5d
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/14/2018
ms.author: negat
ms.openlocfilehash: cdd1015f63e80b7ec51565c18f3440ce1828fb03
ms.sourcegitcommit: 782d5955e1bec50a17d9366a8e2bf583559dca9e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/02/2018
---
# <a name="modify-a-virtual-machine-scale-set"></a>Upravit škálovací sadu virtuálních počítačů
Tento článek popisuje, jak upravit existující sady škálování. Jedná se o tom, jak změnit konfiguraci měřítka nastavit, jak změnit konfiguraci aplikací běžících na rozsahu sady, Správa dostupnosti a další.

## <a name="fundamental-concepts"></a>základní koncepty

### <a name="the-scale-set-model"></a>Model sada škálování

Sada škálování má "škálování sadu model" shromažďuje *požadované* stav měřítka nastavený jako celek. K dotazování modelů pro sadu škálování, můžete použít:

Rozhraní API REST: `GET https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachineScaleSets/{vmScaleSetName}?api-version={apiVersion}` (Další informace najdete v tématu [dokumentace k REST API](https://docs.microsoft.com/rest/api/compute/virtualmachinescalesets/get))

Prostředí PowerShell: `Get-AzureRmVmss -ResourceGroupName {resourceGroupName} -VMScaleSetName {vmScaleSetName}` (Další informace najdete v tématu [dokumentace k prostředí Powershell](https://docs.microsoft.com/powershell/module/azurerm.compute/get-azurermvmss))

Rozhraní příkazového řádku: `az vmss show -g {resourceGroupName} -n {vmSaleSetName}` (Další informace najdete v tématu [dokumentaci k rozhraní příkazového řádku](https://docs.microsoft.com/cli/azure/vmss?view=azure-cli-latest#az_vmss_show))

Můžete také použít [resources.azure.com](https://resources.azure.com) nebo [sady Azure SDK](https://azure.microsoft.com/downloads/) k dotazování modelů pro sadu škálování.

Přesný prezentace výstupu závisí na možnosti, které poskytnete příkazu, ale tady je ukázka výstupu z rozhraní příkazového řádku:

```
$ az vmss show -g {resourceGroupName} -n {vmScaleSetName}
{
  "location": "westus",
  "overprovision": true,
  "plan": null,
  "singlePlacementGroup": true,
  "sku": {
    "additionalProperties": {},
    "capacity": 1,
    "name": "Standard_D2_v2",
    "tier": "Standard"
  },
  .
  .
  .
}
```

Jak můžete vidět, tyto vlastnosti se vztahují ke stupnici nastavit jako celek.



### <a name="the-scale-set-instance-view"></a>Zobrazení instance škálovací sady

Nastavit také škálování má "škálování sadu instanci zobrazit" shromažďuje aktuální *runtime* stav měřítka nastavený jako celek. K zobrazení instance škálovací sady dotazu, můžete použít:

Rozhraní API REST: `GET https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachineScaleSets/{vmScaleSetName}/instanceView?api-version={apiVersion}` (Další informace najdete v tématu [dokumentace k REST API](https://docs.microsoft.com/rest/api/compute/virtualmachinescalesets/getinstanceview))

Prostředí PowerShell: `Get-AzureRmVmss -ResourceGroupName {resourceGroupName} -VMScaleSetName {vmScaleSetName} -InstanceView` (Další informace najdete v tématu [dokumentace k prostředí Powershell](https://docs.microsoft.com/powershell/module/azurerm.compute/get-azurermvmss))

Rozhraní příkazového řádku: `az vmss get-instance-view -g {resourceGroupName} -n {vmSaleSetName}` (Další informace najdete v tématu [dokumentaci k rozhraní příkazového řádku](https://docs.microsoft.com/cli/azure/vmss?view=azure-cli-latest#az_vmss_get_instance_view))

Můžete také použít [resources.azure.com](https://resources.azure.com) nebo [sady Azure SDK](https://azure.microsoft.com/downloads/) k dotazu na zobrazení instance škálovací sady.

Přesný prezentace výstupu závisí na možnosti, které poskytnete příkazu, ale tady je ukázkový výstup z rozhraní příkazového řádku:

```
$ az vmss get-instance-view -g {resourceGroupName} -n {virtualMachineScaleSetName}
{
  "statuses": [
    {
      "additionalProperties": {},
      "code": "ProvisioningState/succeeded",
      "displayStatus": "Provisioning succeeded",
      "level": "Info",
      "message": null,
      "time": "{time}"
    }
  ],
  "virtualMachine": {
    "additionalProperties": {},
    "statusesSummary": [
      {
        "additionalProperties": {},
        "code": "ProvisioningState/succeeded",
        "count": 1
      }
    ]
  }
  .
  .
  .
}
```

Jak vidíte, zadejte tyto vlastnosti sady souhrn aktuální stav modulu runtime virtuálních počítačů v měřítka, včetně obsahuje stav rozšíření použít do sady škálování (vynechaný jako stručný výtah).



### <a name="the-scale-set-vm-model-view"></a>Škálovací sady virtuálních počítačů modelu zobrazení

Podobně jako jak sadu škálování má modelu zobrazení, má každý virtuální počítač ve škálovací sadě vlastní zobrazení modelu. K dotazu na zobrazení model pro sadu škálování, můžete použít:

Rozhraní API REST: `GET https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachineScaleSets/{vmScaleSetName}/virtualmachines/{instanceId}?api-version={apiVersion}` (Další informace najdete v tématu [dokumentace k REST API](https://docs.microsoft.com/rest/api/compute/virtualmachinescalesetvms/get))

Prostředí PowerShell: `Get-AzureRmVmssVm -ResourceGroupName {resourceGroupName} -VMScaleSetName {vmScaleSetName} -InstanceId {instanceId}` (Další informace najdete v tématu [dokumentace k prostředí Powershell](https://docs.microsoft.com/powershell/module/azurerm.compute/get-azurermvmssvm))

Rozhraní příkazového řádku: `az vmss show -g {resourceGroupName} -n {vmSaleSetName} --instance-id {instanceId}` (Další informace najdete v tématu [dokumentaci k rozhraní příkazového řádku](https://docs.microsoft.com/cli/azure/vmss?view=azure-cli-latest#az_vmss_show))

Můžete také použít [resources.azure.com](https://resources.azure.com) nebo [sady Azure SDK](https://azure.microsoft.com/downloads/) k dotazování modelu pro virtuální počítač ve škálovací sadě.

Přesný prezentace výstupu závisí na možnosti, které poskytnete příkazu, ale tady je ukázka výstupu z rozhraní příkazového řádku:

```
$ az vmss show -g {resourceGroupName} -n {vmScaleSetName}
{
  "location": "westus",
  "name": "{name}",
  "sku": {
    "name": "Standard_D2_v2",
    "tier": "Standard"
  },
  .
  .
  .
}
```

Jak vidíte, popisují tyto vlastnosti konfigurace virtuálního počítače, není konfigurace měřítka nastavit jako celek. Například model sada škálování má `overprovision` jako vlastnost, zatímco modelu pro virtuální počítač ve škálovací sadě neexistuje. Tento rozdíl je, protože předimenzování je vlastnost sad jako celé, ne jednotlivé virtuální počítače ve škálovací sadě škálování (Další informace o předimenzování najdete v tématu [této dokumentace](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-design-overview#overprovisioning)).



### <a name="the-scale-set-vm-instance-view"></a>Měřítko nastavit zobrazení instance virtuálního počítače

Podobně jako jak sadu škálování má zobrazení instance, má každý virtuální počítač ve škálovací sadě vlastní zobrazení instance. K zobrazení instance škálovací sady dotazu, můžete použít:

Rozhraní API REST: `GET https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachineScaleSets/{vmScaleSetName}/virtualmachines/{instanceId}/instanceView?api-version={apiVersion}` (Další informace najdete v tématu [dokumentace k REST API](https://docs.microsoft.com/rest/api/compute/virtualmachinescalesetvms/getinstanceview))

Prostředí PowerShell: `Get-AzureRmVmssVm -ResourceGroupName {resourceGroupName} -VMScaleSetName {vmScaleSetName} -InstanceId {instanceId} -InstanceView` (Další informace najdete v tématu [dokumentace k prostředí Powershell](https://docs.microsoft.com/powershell/module/azurerm.compute/get-azurermvmssvm))

Rozhraní příkazového řádku: `az vmss get-instance-view -g {resourceGroupName} -n {vmSaleSetName} --instance-id {instanceId}` (Další informace najdete v tématu [dokumentaci k rozhraní příkazového řádku](https://docs.microsoft.com/cli/azure/vmss?view=azure-cli-latest#az_vmss_get_instance_view))

Můžete také použít [resources.azure.com](https://resources.azure.com) nebo [sady Azure SDK](https://azure.microsoft.com/downloads/) k dotazu na zobrazení instance virtuálního počítače ve škálovací sadě.

Přesný prezentace výstupu závisí na možnosti, které poskytnete příkazu, ale tady je ukázka výstupu z rozhraní příkazového řádku:

```
$ az vmss get-instance-view -g {resourceGroupName} -n {vmScaleSetName} --instance-id {instanceId}
{
  "additionalProperties": {
    "osName": "ubuntu",
    "osVersion": "16.04"
  },
  "disks": [
    {
      "name": "{name}",
      "statuses": [
        {
          "additionalProperties": {},
          "code": "ProvisioningState/succeeded",
          "displayStatus": "Provisioning succeeded",
          "time": "{time}"
        }
      ]
    }
  ],
  "statuses": [
    {
      "additionalProperties": {},
      "code": "ProvisioningState/succeeded",
      "displayStatus": "Provisioning succeeded",
      "time": "{time}"
    },
    {
      "additionalProperties": {},
      "code": "PowerState/running",
      "displayStatus": "VM running"
    }
  ],
  "vmAgent": {
    "statuses": [
      {
        "additionalProperties": {},
        "code": "ProvisioningState/succeeded",
        "displayStatus": "Ready",
        "level": "Info",
        "message": "Guest Agent is running",
        "time": "{time}"
      }
    ],
    "vmAgentVersion": "{version}"
  },
  .
  .
  .
}
```

Jak vidíte, tyto vlastnosti popisují aktuální stav modulu runtime virtuálního počítače, včetně všech rozšíření použít do sady škálování (vynechaný jako stručný výtah).




## <a name="how-to-update-global-scale-set-properties"></a>Postup aktualizace globálním měřítku nastavit vlastnosti

Chcete-li aktualizovat nastavte vlastnost globálním měřítku, je nutné aktualizovat vlastnost v modelu sady škálování. Tato aktualizace prostřednictvím můžete provést:

Rozhraní API REST: `PUT https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachineScaleSets/{vmScaleSetName}?api-version={apiVersion}` (Další informace najdete v tématu [dokumentace k REST API](https://docs.microsoft.com/rest/api/compute/virtualmachinescalesets/createorupdate))

Šablony Resource Manageru: Můžete taky nasadit šablony Resource Manageru pomocí vlastnosti z rozhraní API REST a aktualizovat nastavení vlastností globálním měřítku.

Prostředí PowerShell: `Update-AzureRmVmss -ResourceGroupName {resourceGroupName} -VMScaleSetName {vmScaleSetName} -VirtualMachineScaleSet {scaleSetConfigPowershellObject}` (Další informace najdete v tématu [dokumentace k prostředí Powershell](https://docs.microsoft.com/powershell/module/azurerm.compute/update-azurermvmss))

CLI. Jestliže chcete upravit: `az vmss update --set {propertyPath}={value}`. Přidat objekt do seznamu vlastností v sadě škálování: `az vmss update --add {propertyPath} {JSONObjectToAdd}`. Chcete-li odebrat objekt ze seznamu vlastností v sadě škálování: `az vmss update --remove {propertyPath} {indexToRemove}`. (Další informace najdete v tématu [dokumentaci k rozhraní příkazového řádku](https://docs.microsoft.com/cli/azure/vmss?view=azure-cli-latest#az_vmss_update)). Případně pokud jste předtím nasadili pomocí sad škálování `az vmss create` příkazů, můžete spustit `az vmss create` příkaz znovu a aktualizovat sadu škálování. K tomu je potřeba zajistit, aby všechny vlastnosti v `az vmss create` příkaz identická jako předtím, s výjimkou vlastnosti, které chcete upravit.



Můžete také použít [resources.azure.com](https://resources.azure.com) nebo [sady Azure SDK](https://azure.microsoft.com/downloads/) aktualizovat měřítka nastavit modelu.

Jakmile dojde k aktualizaci modelu sady škálování, nová konfigurace platí pro všechny nové virtuální počítače vytvořené v sadě škálování. Ale modely pro stávající virtuální počítače ve škálovací sadě musí stále uvést do režimu aktuální nejnovější celkové škálování sadu modelu. Ve model pro každý virtuální počítač je vlastnost typu boolean s názvem `latestModelApplied` určující, zda virtuální počítač je aktuální nejnovější celkové škálování sadu modelu (`true` znamená virtuálního počítače je aktuální pomocí nejnovější modelu).




## <a name="how-to-bring-vms-up-to-date-with-the-latest-scale-set-model"></a>Zajištění aktuálnosti s modelem nejnovější sady škálování virtuálních počítačů

Sady škálování mít "upgradu zásady" určující, jak nabíhají aktuální s modelem nejnovější sady škálování virtuálních počítačů. Jsou tři režimy pro zásady upgradu:

- Automatické: V tomto režimu je škálovací sadu žádné záruky o pořadí se snížila virtuálních počítačů. Sada škálování může vypnout všechny virtuální počítače ve stejnou dobu. 
- Vrácení: V tomto režimu byly sadou škálování zavede aktualizace v dávkách, volitelné pozastavení doba mezi dávky.
- Ruční: V tomto režimu při aktualizaci modelu sady škálování, nedojde k žádné akci do existujících virtuálních počítačů. Pokud chcete aktualizovat existující virtuální počítače, je potřeba udělat "ruční upgrade" každý existující virtuální počítač. Můžete provést ruční upgrade prostřednictvím:

Rozhraní API REST: `POST https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachineScaleSets/{vmScaleSetName}/manualupgrade?api-version={apiVersion}` (Další informace najdete v tématu [dokumentace k REST API](https://docs.microsoft.com/rest/api/compute/virtualmachinescalesets/updateinstances))

Prostředí PowerShell: `Update-AzureRmVmssInstance -ResourceGroupName {resourceGroupName} -VMScaleSetName {vmScaleSetName} -InstanceId {instanceId}` (Další informace najdete v tématu [dokumentace k prostředí Powershell](https://docs.microsoft.com/powershell/module/azurerm.compute/update-azurermvmssinstance))

Rozhraní příkazového řádku: `az vmss update-instances -g {resourceGroupName} -n {vmScaleSetName} --instance-ids {instanceIds}` (Další informace najdete v tématu [dokumentaci k rozhraní příkazového řádku](https://docs.microsoft.com/cli/azure/vmss?view=azure-cli-latest#az_vmss_update_instances)).

Můžete také [sady Azure SDK](https://azure.microsoft.com/downloads/) provedete ruční upgrade na virtuální počítač ve škálovací sadě.

>[!NOTE]
> Clusterů Service Fabric lze použít pouze automatickém režimu, ale aktualizace se proto liší. Další informace o aktualizacích služby infrastruktury, najdete v části [v Service Fabric dokumentaci](https://docs.microsoft.com/azure/service-fabric/service-fabric-application-upgrade).

>[!NOTE]
> Neexistuje jeden typ změny globálním měřítku umožňuje nastavit vlastnosti, která neodpovídá pravidlům pro zásady upgradu. Toto jsou změny ke stupnici nastavit profil operačního systému (například uživatelské jméno správce a heslo). Tyto vlastnosti lze pouze změněné ve verzi rozhraní API 2017-12-01 nebo novější. Tyto změny se uplatní jenom na virtuální počítače vytvořené po změnu v hodnotě stupnice nastavit modelu. Chcete-li převést stávající virtuální počítače aktuální, musíte udělat "obnovení z Image" každý existujícího virtuálního počítače. Toto obnovení z Image prostřednictvím můžete provést:

Rozhraní API REST: `POST https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachineScaleSets/{vmScaleSetName}/reimage?api-version={apiVersion}` (Další informace najdete v tématu [dokumentace k REST API](https://docs.microsoft.com/rest/api/compute/virtualmachinescalesets/reimage))

Prostředí PowerShell: `Set-AzureRmVmssVM -ResourceGroupName {resourceGroupName} -VMScaleSetName {vmScaleSetName} -InstanceId {instanceId} -Reimage` (Další informace najdete v tématu [dokumentace k prostředí Powershell](https://docs.microsoft.com/powershell/module/azurerm.compute/set-azurermvmssvm))

Rozhraní příkazového řádku: `az vmss reimage -g {resourceGroupName} -n {vmScaleSetName} --instance-id {instanceId}` (Další informace najdete v tématu [dokumentaci k rozhraní příkazového řádku](https://docs.microsoft.com/cli/azure/vmss?view=azure-cli-latest#az_vmss_reimage)).

Můžete také [sady Azure SDK](https://azure.microsoft.com/downloads/) Image virtuálního počítače ve škálovací sadě.




## <a name="properties-with-restrictions-on-modification"></a>Vlastnosti omezení pro úpravy

### <a name="create-time-properties"></a>Čas vytvoření vlastnosti

Některé vlastnosti lze nastavit pouze při vytváření původně měřítka sady. Tyto vlastnosti:

- Zóny
- Vydavatel referenční bitové kopie
- Nabídka referenční bitové kopie

### <a name="properties-that-can-only-be-changed-based-on-the-current-value"></a>Vlastnosti, které lze změnit pouze na základě aktuální hodnoty

S výjimkami v závislosti na aktuální hodnota může změnit některé vlastnosti. Tyto vlastnosti:

- singlePlacementGroup: Pokud singlePlacementGroup hodnotu true, může být změněna na hodnotu false. Nicméně, pokud je nastavena hodnota false, singlePlacementGroup ho **nemusí** změnit na hodnotu true.
- podsítě: podsíť sady škálování může být změněno, pokud jsou původní podsíť a nové podsítě ve stejné virtuální síti.

### <a name="properties-that-require-deallocation-to-change"></a>Vlastnosti, které vyžadují navrácení změnit

Některé vlastnosti může být změněn na určité hodnoty pouze, pokud jsou virtuální počítače ve škálovací sadě navrácena. Tyto vlastnosti:

- Název SKU: Pokud SKU nového virtuálního počítače není podporován na hardwaru měřítka sada je v současné době, budete muset zrušit přidělení virtuální počítače ve škálovací nastavit před změnou názvu sku. Další informace o změně velikosti virtuálních počítačů najdete v tématu [tento příspěvek blogu Azure](https://azure.microsoft.com/blog/resize-virtual-machines/).


## <a name="vm-specific-updates"></a>Aktualizace specifické pro virtuální počítač

Některé změny může použít pro konkrétní virtuální počítače místo nastavení vlastností globálním měřítku. V současné době pouze aktualizace specifické pro virtuální počítač, která je podporována je připojování nebo odpojování datových disků do nebo z virtuálních počítačů v sadě škálování. Tato funkce je ve verzi preview. Další informace najdete v tématu [náhled dokumentaci](https://github.com/Azure/vm-scale-sets/tree/master/preview/disk).

## <a name="scenarios-application-updates-os-updates-etc"></a>Scénáře: Aktualizace aplikace, aktualizace operačního systému atd.

### <a name="application-updates"></a>Aktualizace aplikace

Pokud je aplikace nasazená na měřítko nastavit prostřednictvím rozšíření, aktualizuje se konfigurace rozšíření způsobí, že aplikace aktualizovat v souladu s zásad upgradu. Například pokud máte novou verzi skript běžet v rozšíření vlastních skriptů, může aktualizovat vlastnost fileUris tak, aby odkazoval na nový skript. V některých případech ale můžete vynutit aktualizaci, i když konfigurace rozšíření je beze změny (například můžete aktualizovat skript beze změny identifikátor URI skriptu). V těchto případech můžete upravit forceUpdateTag chcete vynutit aktualizaci. Platformy Azure nebude interpretovat tuto vlastnost tak jeho změna nemá vliv na způsob spuštění rozšíření. Úprava ho jednoduše vynutí rozšíření znovu spustit. Další informace o forceUpdateTag najdete v tématu [dokumentace k REST API pro rozšíření](https://docs.microsoft.com/rest/api/compute/virtualmachineextensions/createorupdate).

Také je běžné pro aplikace pro nasazení pomocí vlastní image. Tento scénář je popsaná v následující části "Aktualizacím operačního systému"

### <a name="os-updates"></a>Aktualizace operačního systému

Pokud používáte Image platformy, můžete aktualizovat image změnou elementu imageReference (Další informace v [dokumentace k REST API](https://docs.microsoft.com/en-us/rest/api/compute/virtualmachinescalesets/createorupdate)).

>[!NOTE]
> S Image platformy je běžné zadejte "nejnovější" pro verzi referenční bitové kopie. To znamená, že během škálování sadu vytvořit, škálování a obnovení z Image, virtuální počítače jsou vytvořeny s na nejnovější dostupnou verzi. Ale ho **nemá** znamená, že bitovou kopii operačního systému bude automaticky aktualizován časem jako jsou vydávány nové verze bitové kopie. Jedná se o samostatnou funkci, momentálně ve verzi preview. Další informace najdete v tématu [automatické upgrady operačního systému dokumentaci](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-automatic-upgrade).

Pokud používáte vlastní Image, můžete aktualizovat image aktualizace ID elementu imageReference (Další informace v [dokumentace k REST API](https://docs.microsoft.com/en-us/rest/api/compute/virtualmachinescalesets/createorupdate)).

## <a name="examples"></a>Příklady

### <a name="updating-the-os-image-for-your-scale-set"></a>Aktualizace bitové kopie operačního systému pro škálovací sadu

Řekněme, že máte škálování nastavit spuštěna stará verze architektury Ubuntu LTS 16.04 a chcete ji aktualizovat na novější verzi 16.04 LTS Ubuntu (například verze 16.04.201801090). Vlastnost verze referenční bitové kopie není v seznamu, tak můžete přímo upravit tyto vlastnosti s těmito příkazy:

Prostředí PowerShell: `Update-AzureRmVmss -ResourceGroupName {resourceGroupName} -VMScaleSetName {vmScaleSetName} -ImageReferenceVersion 16.04.201801090`

CLI: `az vmss update -g {resourceGroupName} -n {vmScaleSetName} --set virtualMachineProfile.storageProfile.imageReference.version=16.04.201801090`


### <a name="updating-the-load-balancer-for-your-scale-set"></a>Aktualizace nástroje pro vyrovnávání zatížení pro škálovací sadu

Řekněme, že máte škálování nastavit s nástrojem pro vyrovnávání zatížení Azure a budete chtít nahradit nástroje pro vyrovnávání zatížení Azure pomocí služby Azure Application Gateway. Vlastnosti zatížení vyrovnávání a aplikace brány pro sadu škálování jsou v seznamu, abyste je mohli používat příkazy pro odebrání a přidávání seznamu elementů místo přímém upravování vlastnosti:

Prostředí PowerShell: 
```
# get the current model of the scale set and store it in a local powershell object named $vmss
> $vmss=Get-AzureRmVmss -ResourceGroupName {resourceGroupName} -Name {vmScaleSetName}

# create a local powershell object for the new desired IP configuration, which includes the referencerence to the application gateway
> $ipconf = New-AzureRmVmssIPConfig myNic -ApplicationGatewayBackendAddressPoolsId /subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/applicationGateways/{applicationGatewayName}/backendAddressPools/{applicationGatewayBackendAddressPoolName} -SubnetId $vmss.VirtualMachineProfile.NetworkProfile.NetworkInterfaceConfigurations[0].IpConfigurations[0].Subnet.Id –Name $vmss.VirtualMachineProfile.NetworkProfile.NetworkInterfaceConfigurations[0].IpConfigurations[0].Name

# replace the existing IP configuration in the local powershell object (which contains the references to the current Azure Load Balancer) with the new IP configuration
> $vmss.VirtualMachineProfile.NetworkProfile.NetworkInterfaceConfigurations[0].IpConfigurations[0] = $ipconf

# Update the model of the scale set with the new configuration in the local powershell object
> Update-AzureRmVmss -ResourceGroupName {resourceGroupName} -Name {vmScaleSetName} -virtualMachineScaleSet $vmss

```

CLI:
```
az vmss update -g {resourceGroupName} -n {vmScaleSetName} --remove virtualMachineProfile.networkProfile.networkInterfaceConfigurations[0].ipConfigurations[0].loadBalancerBackendAddressPools 0 # remove the load balancer backend pool from the scale set model
az vmss update -g {resourceGroupName} -n {vmScaleSetName} --remove virtualMachineProfile.networkProfile.networkInterfaceConfigurations[0].ipConfigurations[0].loadBalancerInboundNatPools 0 # remove the load balancer backend pool from the scale set model; only necessary if you have NAT pools configured on the scale set
az vmss update -g {resourceGroupName} -n {vmScaleSetName} --add virtualMachineProfile.networkProfile.networkInterfaceConfigurations[0].ipConfigurations[0].ApplicationGatewayBackendAddressPools '{"id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/applicationGateways/{applicationGatewayName}/backendAddressPools/{applicationGatewayBackendPoolName}"}' # add the application gateway backend pool to the scale set model
```

>[!NOTE]
> Těchto příkazů se předpokládá, že je pouze jeden vyrovnávání IP konfigurace a zatížení na škálovací sadu. Pokud existuje více možná budete muset použít index seznamů než 0.