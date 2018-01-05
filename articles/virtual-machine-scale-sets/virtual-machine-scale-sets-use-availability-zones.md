---
title: "Vytvořit sadu Azure škálování používající dostupnost zóny (Preview) | Microsoft Docs"
description: "Informace o vytvoření sady škálování virtuálního počítače Azure, které používají dostupnost zóny pro vyšší redundance proti výpadkům"
services: virtual-machine-scale-sets
documentationcenter: 
author: iainfoulds
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machine-scale-sets
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm
ms.devlang: na
ms.topic: article
ms.date: 01/03/2018
ms.author: iainfou
ms.openlocfilehash: 310fdc68d3eb662906053d2bc0c45e6cfa18d4da
ms.sourcegitcommit: df4ddc55b42b593f165d56531f591fdb1e689686
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/04/2018
---
# <a name="create-a-virtual-machine-scale-set-that-uses-availability-zones-preview"></a>Vytvoření škálovací sadu virtuálních počítačů, který používá dostupnost zóny (Preview)
K ochraně vaší sady škálování virtuálního počítače datacenter úrovni selhání, můžete vytvořit škálování nastavit v zóně dostupnosti. Oblasti Azure, které podporují dostupnost zóny mít minimálně tři samostatné zóny, každou s vlastní nezávisle spotřeby zdroje, sítě a chlazení. Další informace najdete v tématu [přehled dostupnosti zón](../availability-zones/az-overview.md).

K používání zón dostupnosti, musí být škálovací sadu vytvořené v [podporované oblasti Azure](../availability-zones/az-overview.md#regions-that-support-availability-zones). Můžete vytvořit sada škálování, které používá dostupnost zóny s jedním z následujících metod:

- [portál Azure Portal](#use-the-azure-portal)
- [Azure CLI 2.0](#use-the-azure-cli-20)
- [Azure PowerShell](#use-azure-powershell)
- [Šablony Azure Resource Manageru](#use-azure-resource-manager-templates)


## <a name="use-the-azure-portal"></a>Použití webu Azure Portal
Proces vytvoření sada škálování, které používá dostupnosti zóny je stejný jako podrobné v [Začínáme článku](virtual-machine-scale-sets-create-portal.md). Když vyberete podporovanou oblast Azure, můžete vytvořit škálování nastavit v jedné ze zón k dispozici, jak je znázorněno v následujícím příkladu:

![Vytvoření sad v jedné zóně dostupnosti škálování](media/virtual-machine-scale-sets-use-availability-zones/create-portal-single-az.png)

Sada škálování a podpůrné prostředky, například pro vyrovnávání zatížení Azure a veřejné IP adresy, jsou vytvořené v jedné oblasti, který určíte.


## <a name="use-the-azure-cli-20"></a>Použití Azure CLI 2.0
Proces vytvoření sada škálování, které používá dostupnosti zóny je stejný jako podrobné v [Začínáme článku](virtual-machine-scale-sets-create-cli.md). K používání zón dostupnosti, je nutné vytvořit váš škálování nastavit v podporovanou oblast Azure. Přidat `--zones` parametru [vytvořit az vmss](/cli/azure/vmss#az_vmss_create) příkaz a zadejte zóně, ve které chcete použít (například zóny *1*, *2*, nebo *3*). Následující příklad vytvoří škálování nastavení s názvem *myScaleSet* v zóně *1*:

```azurecli
az vmss create \
    --resource-group myResourceGroup \
    --name myScaleSet \
    --image UbuntuLTS \
    --upgrade-policy-mode automatic \
    --admin-username azureuser \
    --generate-ssh-keys \
    --zones 1
```

Jak dlouho trvá několik minut vytvořit a nakonfigurovat všechna měřítka nastavit zdroje a virtuální počítače v zóně, který určíte.


## <a name="use-azure-powershell"></a>Použití Azure Powershell
Proces vytvoření sada škálování, které používá dostupnosti zóny je stejný jako podrobné v [Začínáme článku](virtual-machine-scale-sets-create-powershell.md). K používání zón dostupnosti, je nutné vytvořit váš škálování nastavit v podporovanou oblast Azure. Přidat `-Zone` parametru [New-AzureRmVmssConfig](/powershell/module/azurerm.compute/new-azurermvmssconfig) příkaz a zadejte zóně, ve které chcete použít (například zóny *1*, *2*, nebo *3*). Následující příklad vytvoří konfigurace sady škálování s názvem *vmssConfig* v *východní USA 2* zóny *1*:

```powershell
$vmssConfig = New-AzureRmVmssConfig `
    -Location "East US 2" `
    -SkuCapacity 2 `
    -SkuName "Standard_DS2" `
    -UpgradePolicyMode Automatic `
    -Zone "1"
```

Při vytváření skutečné škálovací sadu, postupujte podle dalších kroků popsaných [Začínáme článku](virtual-machine-scale-sets-create-powershell.md).


## <a name="use-azure-resource-manager-templates"></a>Použití šablon Azure Resource Manageru
Proces vytvoření sada škálování, které používá dostupnosti zóny je stejný jako v článku na získávání Začínáme pro podrobné [Linux](virtual-machine-scale-sets-create-template-linux.md) nebo [Windows](virtual-machine-scale-sets-create-template-windows.md). K používání zón dostupnosti, je nutné vytvořit váš škálování nastavit v podporovanou oblast Azure. Přidat `zones` vlastnost, která má *Microsoft.Compute/virtualMachineScaleSets* prostředků zadejte v šabloně a zóně, ve které chcete použít (například zóny *1*, *2*, nebo *3*). Následující příklad vytvoří Linux škálování nastavení s názvem *myScaleSet* v *východní USA 2* zóny *1*:

```json
{
  "type": "Microsoft.Compute/virtualMachineScaleSets",
  "name": "myScaleSet",
  "location": "East US 2",
  "apiVersion": "2017-12-01",
  "zones": ["1"],
  "sku": {
    "name": "Standard_A1",
    "capacity": "2"
  },
  "properties": {
    "upgradePolicy": {
      "mode": "Automatic"
    },
    "virtualMachineProfile": {
      "storageProfile": {
        "osDisk": {
          "caching": "ReadWrite",
          "createOption": "FromImage"
        },
        "imageReference":  {
          "publisher": "Canonical",
          "offer": "UbuntuServer",
          "sku": "16.04-LTS",
          "version": "latest"
        }
      },
      "osProfile": {
        "computerNamePrefix": "myvmss",
        "adminUsername": "azureuser",
        "adminPassword": "P@ssw0rd!"
      }
    }
  }
}
```

Pro vytvoření sady skutečné škálování, postupujte podle dalších kroků popsaných v článku na získávání Začínáme pro [Linux](virtual-machine-scale-sets-create-template-linux.md) nebo [Windows](virtual-machine-scale-sets-create-template-windows.md)


## <a name="next-steps"></a>Další postup
Teď, když jste vytvořili škálování nastavit v zóně dostupnosti, můžete další postup [nasazení aplikace na virtuálním počítači škálování sady](virtual-machine-scale-sets-deploy-app.md) nebo [škálování pomocí sady škálování virtuálního počítače](virtual-machine-scale-sets-autoscale-overview.md).
