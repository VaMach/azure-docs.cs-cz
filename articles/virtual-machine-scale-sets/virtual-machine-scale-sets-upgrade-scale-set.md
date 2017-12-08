---
title: "Upgrade sadu škálování virtuálního počítače Azure | Microsoft Docs"
description: "Upgrade sadu škálování virtuálního počítače Azure"
services: virtual-machine-scale-sets
documentationcenter: 
author: gbowerman
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: e229664e-ee4e-4f12-9d2e-a4f456989e5d
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/30/2017
ms.author: guybo
ms.openlocfilehash: c7093e221ff8fe69ded1cfbce4f3ddeb1a195666
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="upgrade-a-virtual-machine-scale-set"></a>Upgradovat škálovací sadu virtuálních počítačů
Tento článek popisuje, jak můžete zavádět aktualizaci operačního systému na virtuální počítač Azure měřítko nastavit bez odstávky. V tomto kontextu aktualizaci operačního systému zahrnuje změna verze nebo skladová položka operačního systému nebo změna identifikátoru URI vlastní image. Aktualizace bez výpadku prostředků aktualizace virtuálních počítačů najednou, nebo ve skupinách (například jeden doména selhání najednou) namísto všechny najednou. Díky tomu můžete spouštět všechny virtuální počítače, které nejsou probíhá upgrade.

Aby se zabránilo nejednoznačnosti, umožňuje rozlišit čtyři typy aktualizaci operačního systému, kterou chcete provést:

* Změna verze nebo SKU image platformy. Například změna Ubuntu 14.04.2-LTS verzi z 14.04.201506100 14.04.201507060, nebo změna 15.10/nejnovější Ubuntu SKU na 16.04.0-LTS/latest. Tento scénář je popsaná v tomto článku.
* Změna identifikátoru URI, který odkazuje na novou verzi vlastní image vytvořené (**vlastnosti** > **virtualMachineProfile** > **storageProfile**  >  **osDisk** > **image** > **uri**). Tento scénář je popsaná v tomto článku.
* Změna referenční bitové kopie sady škálování, která byla vytvořena pomocí Azure spravované disky.
* Opravy operačního systému z virtuálního počítače (to příklady instalace opravy zabezpečení a spuštění služby Windows Update). Tento scénář je podporují, ale nejsou zahrnuté v tomto článku.

Sady škálování virtuálního počítače, které jsou nasazeny jako součást [Azure Service Fabric](https://azure.microsoft.com/services/service-fabric/) clusteru nejsou popsané v tomto poli. V tématu [opravy operačního systému Windows v clusteru Service Fabric](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-patch-orchestration-application) Další informace o použití dílčích oprav Service Fabric.

Změna verze nebo SKU operačního systému z image platformy nebo identifikátor URI vlastní image základní pořadí vypadá takto:

1. Získáte modelu sady škálování virtuálního počítače.
2. Změna verze SKU, odkaz na obrázek nebo hodnota identifikátoru URI v modelu.
3. Aktualizace modelu.
4. Proveďte *manualUpgrade* volání na virtuálních počítačích v sadě škálování. Tento krok platí jen pokud *upgradePolicy* je nastaven na **ruční** v sadě vaší škálování. Pokud je nastaven na hodnotu **automatické**, všechny virtuální počítače jsou upgradovány najednou, což způsobuje výpadek.

Tyto informace v paměti a podíváme, jak může aktualizovat verzi škálování, nastavte v prostředí PowerShell a pomocí rozhraní REST API. Tyto příklady zahrnují v případě image platformy, ale tento článek poskytuje dostatek informací, můžete přizpůsobit tento proces na vlastní image.

## <a name="powershell"></a>PowerShell
Tento příklad aktualizace škálování virtuálních počítačů Windows nastavte (vytváření 4.0.20160229 na novou verzi. Po aktualizaci modelu se dělá instance aktualizace jeden virtuální počítač najednou.

```powershell
$rgname = "myrg"
$vmssname = "myvmss"
$newversion = "4.0.20160229"
$instanceid = "1"

# get the VMSS model
$vmss = Get-AzureRmVmss -ResourceGroupName $rgname -VMScaleSetName $vmssname

# set the new version in the model data
$vmss.virtualMachineProfile.storageProfile.imageReference.version = $newversion

# update the virtual machine scale set model
Update-AzureRmVmss -ResourceGroupName $rgname -Name $vmssname -VirtualMachineScaleSet $vmss

# now start updating instances
Update-AzureRmVmssInstance -ResourceGroupName $rgname -VMScaleSetName $vmssname -InstanceId $instanceId
```

Při aktualizaci identifikátor URI pro vlastní image místo změna image verze platformy, nahraďte "nastavena na novou verzi" řádku příkaz, který se bude aktualizovat zdrojové bitové kopie identifikátor URI. Například pokud byly sadou škálování byl vytvořen bez použití Azure spravované disky, aktualizace bude vypadat takto:

```powershell
# set the new version in the model data
$vmss.virtualMachineProfile.storageProfile.osDisk.image.uri= $newURI
```

Pokud vlastní image na základě škálovací sadu byl vytvořen pomocí spravované disky Azure, pak by aktualizovat odkaz na obrázek. Například:

```powershell
# set the new version in the model data
$vmss.virtualMachineProfile.storageProfile.imageReference.id = $newImageReference
```

## <a name="the-rest-api"></a>Rozhraní REST API
Tady je několik příkladů Python, které zavádět aktualizaci verze operačního systému pomocí rozhraní REST API Azure. Jak používat jednoduchá [azurerm](https://pypi.python.org/pypi/azurerm) knihovna funkcí rozhraní REST API Azure obálku udělat GET na rozsahu nastavit modelu, za nímž následuje PUT s aktualizovaném modelu. Také zobrazit v zobrazení instance virtuálního počítače můžete identifikovat virtuální počítače podle aktualizaci domény.

### <a name="vmssupgrade"></a>Vmssupgrade
 [Vmssupgrade](https://github.com/gbowerman/vmsstools) je skript jazyka Python, který se používá k zavedení upgrade operačního systému spuštěné škálování virtuálního počítače nastavit jednu aktualizační doménu najednou.

![Skript Vmssupgrade pro výběr virtuálních počítačů nebo v doméně aktualizace](./media/virtual-machine-scale-sets-upgrade-scale-set/vmssupgrade-screenshot.png)

Tento skript umožňuje vybrat konkrétní virtuální počítače a aktualizovat nebo zadejte doménu aktualizace. Podporuje změna image verze platformy nebo změna identifikátoru URI vlastní image.

### <a name="vmsseditor"></a>Vmsseditor
[Vmsseditor](https://github.com/gbowerman/vmssdashboard) je obecný editor pro sady škálování virtuálního počítače, které ukazuje virtuální počítač stav jako heatmap, kde jeden řádek představuje jednu aktualizační doménu. Kromě jiných věcí můžete aktualizovat model pro sadu škálování s novou verzi, SKU nebo vlastní image URI a pak vyberte domén selhání upgradu. Pokud tak učiníte, všechny virtuální počítače v této doméně aktualizace upgradují na nový model. Alternativně můžete provést postupného upgradu podle velikost dávky podle svého výběru.  

Následující snímek obrazovky ukazuje model škálování pro Ubuntu 14.04-2LTS verze 14.04.201507060 nastavit. Mnoho dalších možností byly přidány k tomuto nástroji od doby pořízení tomto snímku obrazovky.

![Model Vmsseditor měřítka, nastavte pro Ubuntu 14.04-2LTS](./media/virtual-machine-scale-sets-upgrade-scale-set/vmssEditor1.png)

Po kliknutí na tlačítko **Upgrade** a potom **získat podrobnosti o**, virtuální počítače v UD 0 spustí aktualizovat.

![Probíhá aktualizace zobrazení Vmsseditor](./media/virtual-machine-scale-sets-upgrade-scale-set/vmssEditor2.png)

