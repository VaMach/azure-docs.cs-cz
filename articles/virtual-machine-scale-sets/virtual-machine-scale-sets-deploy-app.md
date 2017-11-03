---
title: "Nasazení aplikace na sadu škálování virtuálního počítače Azure | Microsoft Docs"
description: "Zjistěte, jak nasadit aplikace pro Linux a Windows instancí virtuálního počítače ve škálovací sadě"
services: virtual-machine-scale-sets
documentationcenter: 
author: iainfoulds
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: f8892199-f2e2-4b82-988a-28ca8a7fd1eb
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/13/2017
ms.author: iainfou
ms.openlocfilehash: 7e03d5e2bbdb1b3b206fa7fa455f7dce7951f02b
ms.sourcegitcommit: 3e3a5e01a5629e017de2289a6abebbb798cec736
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/27/2017
---
# <a name="deploy-your-application-on-virtual-machine-scale-sets"></a>Nasazení aplikace na sady škálování virtuálního počítače
Ke spuštění aplikací v instancích virtuálního počítače (VM) v sadě škálování, musíte nejprve nainstalovat součásti aplikace a požadované soubory. Tento článek představuje způsoby, jak vytvořit vlastní image virtuálního počítače pro instance v škálování nastavit nebo automatické spouštění skriptů instalace v existující instance virtuálních počítačů. Také zjistíte, jak spravovat aplikace nebo aktualizace operačního systému v rámci sady škálování.


## <a name="build-a-custom-vm-image"></a>Sestavení vlastní image virtuálního počítače
Když použijete jednu z imagí platformy Azure k vytvoření instance ve škálovací sadě, žádný další software je nainstalovaná nebo nakonfigurovaná. Můžete automatizovat instalace těchto součástí, ale která přidává do čas potřebný ke zřízení instance, které chcete škálovací sady virtuálních počítačů. Pokud použijete mnoho změn konfigurace pro instance virtuálních počítačů, je Správa spojené s těmito konfigurační skripty a úlohy.

Pokud chcete zkrátit configuration management a čas pro zřízení virtuálního počítače, můžete vytvořit vlastní image virtuálního počítače, který je připraven ke spuštění aplikace v sadě škálování při zřízení instance. Celkový proces vytvoření vlastní image virtuálního počítače pro více instancí sady jsou následující:

1. K vytvoření vlastní image virtuálního počítače pro vaše více instancí sady, můžete vytvořit a přihlaste se k virtuálnímu počítači, pak nainstalujte a nakonfigurujte aplikaci. Balírna můžete použít k definování a sestavení [Linux](../virtual-machines/linux/build-image-with-packer.md) nebo [Windows](../virtual-machines/windows/build-image-with-packer.md) image virtuálního počítače. Nebo můžete ručně vytvořit a nakonfigurovat virtuální počítač:

    - Vytvoření virtuálního počítače s Linuxem pomocí [Azure CLI 2.0](../virtual-machines/linux/quick-create-cli.md), [prostředí Azure PowerShell](../virtual-machines/linux/quick-create-powershell.md), nebo [portál](../virtual-machines/linux/quick-create-portal.md).
    - Vytvoření virtuálního počítače s Windows pomocí [prostředí Azure PowerShell](../virtual-machines/windows/quick-create-powershell.md), [Azure CLI 2.0](../virtual-machines/windows/quick-create-cli.md), nebo [portál](../virtual-machines/windows/quick-create-portal.md).
    - Přihlaste se k [Linux](../virtual-machines/linux/mac-create-ssh-keys.md#use-the-ssh-key-pair) nebo [Windows](../virtual-machines/windows/connect-logon.md) virtuálních počítačů.
    - Nainstalujte a nakonfigurujte aplikace a nástroje, které jsou potřeba. Pokud potřebujete konkrétní verze knihovny nebo modul runtime, vlastní image virtuálního počítače umožňuje definovat verze a 

2. Zachycení virtuálního počítače s [rozhraní příkazového řádku Azure 2.0](../virtual-machines/linux/capture-image.md) nebo [prostředí Azure PowerShell](../virtual-machines/windows/capture-image.md). Tento krok vytvoří vlastní image virtuálního počítače, který se používá k následnému nasazení instancí v sadě škálování.

3. [Vytvoření sady škálování](virtual-machine-scale-sets-create.md) a zadejte vlastní image virtuálního počítače, který byl vytvořen v předchozích krocích.


## <a name="already-provisioned"></a>Instalace aplikace s rozšíření vlastních skriptů
Rozšíření vlastních skriptů stahuje a spouští skripty na virtuálních počítačích Azure. Toto rozšíření je užitečné pro konfiguraci nasazení post, instalace softwaru nebo jakoukoli jinou konfiguraci, nebo úlohu správy. Skripty můžete stáhnout z úložiště Azure nebo Githubu, nebo zadaná k portálu Azure při spuštění rozšíření.

Rozšíření vlastních skriptů se integruje s šablon Azure Resource Manageru a můžete také spustit pomocí rozhraní příkazového řádku Azure, PowerShell, portálu Azure nebo REST API pro virtuální počítač Azure. 

Další informace najdete v tématu [rozšíření vlastních skriptů přehled](../virtual-machines/windows/extensions-customscript.md).


### <a name="use-azure-powershell"></a>Použití Azure Powershell
PowerShell používá tabulku hash pro uložení souboru ke stažení a spuštění příkazu. V následujícím příkladu:

- Dá pokyn instance virtuálních počítačů se stáhnout skript z Githubu - *https://raw.githubusercontent.com/iainfoulds/azure-samples/master/automate-iis.ps1*
- Nastaví rozšíření pro spuštění skriptu install-`powershell -ExecutionPolicy Unrestricted -File automate-iis.ps1`
- Získá informace o škálování s [Get-AzureRmVmss](/powershell/module/azurerm.compute/get-azurermvmss)
- Platí pro instance virtuálních počítačů s příponou [AzureRmVmss aktualizace](/powershell/module/azurerm.compute/update-azurermvmss)

Rozšíření vlastních skriptů, které se použijí na *myScaleSet* instance virtuálních počítačů ve skupině prostředků s názvem *myResourceGroup*. Zadejte vlastní názvy následujícím způsobem:

```powershell
# Define the script for your Custom Script Extension to run
$customConfig = @{
    "fileUris" = (,"https://raw.githubusercontent.com/iainfoulds/azure-samples/master/automate-iis.ps1");
    "commandToExecute" = "powershell -ExecutionPolicy Unrestricted -File automate-iis.ps1"
}

# Get information about the scale set
$vmss = Get-AzureRmVmss `
                -ResourceGroupName "myResourceGroup" `
                -VMScaleSetName "myScaleSet"

# Add the Custom Script Extension to install IIS and configure basic website
$vmss = Add-AzureRmVmssExtension `
    -VirtualMachineScaleSet $vmss `
    -Name "customScript" `
    -Publisher "Microsoft.Compute" `
    -Type "CustomScriptExtension" `
    -TypeHandlerVersion 1.8 `
    -Setting $customConfig

# Update the scale set and apply the Custom Script Extension to the VM instances
Update-AzureRmVmss `
    -ResourceGroupName "myResourceGroup" `
    -Name "myScaleSet" `
    -VirtualMachineScaleSet $vmss
```

Pokud je aktualizace zásad na škálovací sadu *ruční*, aktualizovat instancím virtuálních počítačů s [aktualizace AzureRmVmssInstance](/powershell/module/azurerm.compute/update-azurermvmssinstance). Tato rutina platí konfigurace sady aktualizované škálování pro instance virtuálních počítačů a nainstaluje aplikace.


### <a name="use-azure-cli-20"></a>Použití Azure CLI 2.0
Použít rozšíření vlastních skriptů pomocí rozhraní příkazového řádku Azure, vytvořte soubor JSON, který definuje, jaké soubory k získání a příkazů pro spuštění. Tyto definice JSON lze opětovně použít napříč sadu nasazení použít konzistentní aplikace nainstaluje.

V aktuálním prostředí, vytvořte soubor s názvem *customConfig.json* a vložte následující konfigurace. Například vytvoření souboru v prostředí cloudu není na místním počítači. Můžete použít libovolný editor, které chcete. Zadejte `sensible-editor cloudConfig.json` k vytvoření tohoto souboru a zobrazit seznam dostupných editory.

```json
{
  "fileUris": ["https://raw.githubusercontent.com/iainfoulds/azure-samples/master/automate_nginx.sh"],
  "commandToExecute": "./automate_nginx.sh"
}
```

Použít konfiguraci rozšíření vlastních skriptů na instancím virtuálních počítačů ve vaší škálování s [az vmss rozšíření sady](/cli/azure/vmss/extension#set). Následující příklad se vztahuje *customConfig.json* konfigurace *myScaleSet* instance virtuálních počítačů ve skupině prostředků s názvem *myResourceGroup*. Zadejte vlastní názvy následujícím způsobem:

```azurecli
az vmss extension set \
    --publisher Microsoft.Azure.Extensions \
    --version 2.0 \
    --name CustomScript \
    --resource-group myResourceGroup \
    --vmss-name myScaleSet \
    --settings @customConfig.json
```

Pokud je aktualizace zásad na škálovací sadu *ruční*, aktualizovat instancím virtuálních počítačů s [az vmss aktualizace instance](/cli/azure/vmss#update-instances). Tato rutina platí konfigurace sady aktualizované škálování pro instance virtuálních počítačů a nainstaluje aplikace.


## <a name="install-an-app-to-a-windows-vm-with-powershell-dsc"></a>Instalace aplikace pro Windows virtuálního počítače pomocí prostředí PowerShell DSC
[Prostředí PowerShell požadovaného stavu konfigurace (DSC)](https://msdn.microsoft.com/en-us/powershell/dsc/overview) je platforma pro správu k definování konfigurace cílových počítačů. Konfigurace DSC definovat, co je potřeba nainstalovat na počítači a postup konfigurace hostitele. Modul Místní Configuration Manager (LCM) běží na každý cílový uzel, který zpracovává požadovaná akce na základě stisknutí konfigurací.

Rozšíření DSC prostředí PowerShell umožňuje přizpůsobit instance virtuálních počítačů v škálování, nastavte pomocí prostředí PowerShell. V následujícím příkladu:

- Dá pokyn instance virtuálních počítačů DSC balíček stáhnout z webu GitHub - *https://github.com/iainfoulds/azure-samples/raw/master/dsc.zip*
- Nastaví rozšíření pro spuštění skriptu install-`configure-http.ps1`
- Získá informace o škálování s [Get-AzureRmVmss](/powershell/module/azurerm.compute/get-azurermvmss)
- Platí pro instance virtuálních počítačů s příponou [AzureRmVmss aktualizace](/powershell/module/azurerm.compute/update-azurermvmss)

Rozšíření DSC se použijí na *myScaleSet* instance virtuálních počítačů ve skupině prostředků s názvem *myResourceGroup*. Zadejte vlastní názvy následujícím způsobem:

```powershell
# Define the script for your Desired Configuration to download and run
$dscConfig = @{
  "wmfVersion" = "latest";
  "configuration" = @{
    "url" = "https://github.com/iainfoulds/azure-samples/raw/master/dsc.zip";
    "script" = "configure-http.ps1";
    "function" = "WebsiteTest";
  };
}

# Get information about the scale set
$vmss = Get-AzureRmVmss `
                -ResourceGroupName "myResourceGroup" `
                -VMScaleSetName "myScaleSet"

# Add the Desired State Configuration extension to install IIS and configure basic website
$vmss = Add-AzureRmVmssExtension `
    -VirtualMachineScaleSet $vmss `
    -Publisher Microsoft.Powershell `
    -Type DSC `
    -TypeHandlerVersion 2.24 `
    -Name "DSC" `
    -Setting $dscConfig

# Update the scale set and apply the Desired State Configuration extension to the VM instances
Update-AzureRmVmss `
    -ResourceGroupName "myResourceGroup" `
    -Name "myScaleSet"  `
    -VirtualMachineScaleSet $vmss
```

Pokud je aktualizace zásad na škálovací sadu *ruční*, aktualizovat instancím virtuálních počítačů s [aktualizace AzureRmVmssInstance](/powershell/module/azurerm.compute/update-azurermvmssinstance). Tato rutina platí konfigurace sady aktualizované škálování pro instance virtuálních počítačů a nainstaluje aplikace.


## <a name="install-an-app-to-a-linux-vm-with-cloud-init"></a>Instalace aplikace na virtuální počítače Linux s inicializací cloudu
[Init cloudu](https://cloudinit.readthedocs.io/latest/) je často používaný přístup k přizpůsobení virtuálního počítače s Linuxem, jako při prvním spuštění. Init cloudu můžete použít k instalaci balíčků a zapisovat soubory nebo konfigurace zabezpečení a uživatelů. Init cloudu běží během úvodního spouštění, nejsou žádné další kroky nebo požadované agenty použít konfiguraci.

Init cloudu také funguje v různých distribucí. Například nepoužívejte **výstižný get instalace** nebo **yum nainstalovat** nainstalovat balíček. Místo toho můžete definovat seznam balíčků pro instalaci. Init cloudu automaticky používá nástroj pro správu nativní balíčku pro distro, kterou vyberete.

Další informace, včetně příklad *cloudu init.txt* souborů najdete v tématu [použít k přizpůsobení virtuálních počítačích Azure cloud init](../virtual-machines/linux/using-cloud-init.md).

Pokud chcete vytvořit sadu škálování a použít soubor init cloudu, přidejte `--custom-data` parametru [vytvořit az vmss](/cli/azure/vmss#create) příkaz a zadejte název souboru init cloudu. Následující příklad vytvoří škálování nastavení s názvem *myScaleSet* v *myResourceGroup* a nakonfiguruje instance virtuálních počítačů se soubor s názvem *cloudu init.txt*. Zadejte vlastní názvy následujícím způsobem:

```azurecli
az vmss create \
  --resource-group myResourceGroup \
  --name myScaleSet \
  --image UbuntuLTS \
  --upgrade-policy-mode automatic \
  --custom-data cloud-init.txt \
  --admin-username azureuser \
  --generate-ssh-keys
```


## <a name="install-applications-as-a-set-scales-out"></a>Instalovat aplikace jako sada horizontálně navýší kapacitu
Sady škálování umožňují zvýšit počet instancí virtuálního počítače, které spusťte aplikaci. Tato škálování proces můžete spustit ručně nebo automaticky podle metrik, jako je například využití procesoru nebo paměti.

Pokud jste použili rozšíření vlastních skriptů do sady škálování, je aplikace nainstalována do každé nové instance virtuálního počítače. Pokud byly sadou škálování je založená na vlastní image s předem nainstalovanou aplikací, každé nové instance virtuálního počítače je nasazena v použitelného stavu. 

Pokud instance škálovací sady virtuálních počítačů jsou hostitelé kontejneru, můžete pro vyžádání obsahu a spusťte kontejner Image potřeba rozšíření vlastních skriptů. Rozšíření vlastních skriptů také by se mohl zaregistrovat nové instance virtuálního počítače s nástrojem orchestrator, jako je například Azure Container Service.


## <a name="deploy-application-updates"></a>Nasazení aktualizací aplikací
Pokud aktualizujete váš kód aplikace, knihoven nebo balíčků, můžete posouvat nejnovější stav aplikace pro instance virtuálních počítačů v sadě škálování. Pokud používáte rozšíření vlastních skriptů, aktualizace do vaší aplikace a nasadit automaticky. Změňte konfiguraci vlastního skriptu, například na přejděte na instalační skript, který má název aktualizovaná verze. V předchozím příkladu rozšíření vlastních skriptů používá skript s názvem *automate_nginx.sh* následujícím způsobem:

```json
{
  "fileUris": ["https://raw.githubusercontent.com/iainfoulds/azure-samples/master/automate_nginx.sh"],
  "commandToExecute": "./automate_nginx.sh"
}
```

Pokud není, nainstalujte skriptu změny nejsou žádné aktualizace, které provedete do vaší aplikace zveřejněné rozšíření vlastních skriptů. Jeden ze způsobů je zahrnout číslem verze, která verze se zvýší s vaší aplikací. Rozšíření vlastních skriptů teď může odkazovat na *automate_nginx_v2.sh* následujícím způsobem:

```json
{
  "fileUris": ["https://raw.githubusercontent.com/iainfoulds/azure-samples/master/automate_nginx_v2.sh"],
  "commandToExecute": "./automate_nginx_v2.sh"
}
```

Rozšíření vlastních skriptů nyní spouští instance virtuálních počítačů, které chcete použít nejnovější aktualizace aplikace.


### <a name="install-applications-with-os-updates"></a>Instalovat aplikace s aktualizacemi operačního systému
Když jsou k dispozici nové verze operačního systému, můžete použít nebo vytvořit novou vlastní bitovou kopii a [nasazení upgradu OS](virtual-machine-scale-sets-upgrade-scale-set.md) na měřítko nastavit. Každá instance virtuálního počítače je upgradován na nejnovější bitovou kopii, která zadáte. Vlastní image s předinstalovaným, aplikací rozšíření vlastních skriptů nebo DSC prostředí PowerShell můžete použít tak, aby měl vaší aplikace, které jsou automaticky dostupné, jak provést upgrade. Potřebujete plánu údržby aplikace, jak provést tento postup zajistěte, aby žádná verze problémy s kompatibilitou.

Pokud používáte vlastní image virtuálního počítače s předinstalovaným aplikací, může integrovat kanál nasazení a vytvářet nové bitové kopie nasazení operačního systému upgrady v rámci škálovací sadu aktualizací aplikace. Tento přístup umožňuje kanálu, který má být vyzvednutí nejnovější sestavení aplikace, vytvořit a ověřit image virtuálního počítače a pak upgradovat instance virtuálních počítačů v sadě škálování. Pokud chcete spustit nasazení kanálu, který vytvoří a nasadí aplikaci aktualizace napříč vlastní Image virtuálních počítačů, můžete použít [Visual Studio Team Services](https://www.visualstudio.com/team-services/), [Spinnaker](https://www.spinnaker.io/), nebo [volaných](https://jenkins.io/) .


## <a name="next-steps"></a>Další kroky
Jak vytvořit a nasadit aplikace do vaší sady škálování, můžete zkontrolovat [přehled návrhu nastavení škálování](virtual-machine-scale-sets-design-overview.md). Další informace o tom, jak spravovat škálovací sadu najdete v tématu [použití Powershellu ke správě škálovací sadu](virtual-machine-scale-sets-windows-manage.md).
