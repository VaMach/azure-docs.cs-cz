---
title: "Vytvoření virtuálního počítače s Windows pomocí rutiny New-AzVM ve službě Azure Cloud Shell | Dokumentace Microsoftu"
description: "Rychle se naučíte, jak vytvářet virtuální počítače s Windows pomocí rutiny New-AzVM ve službě Azure Cloud Shell."
services: virtual-machines-windows
documentationcenter: virtual-machines
author: cynthn
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 09/21/2017
ms.author: cynthn
ROBOTS: NOINDEX
ms.openlocfilehash: 3be46c8c02ad136edb1936fbb39560d479b27277
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-windows-virtual-machine-with-the-new-azvm-preview-in-cloud-shell"></a>Vytvoření virtuálního počítače Windows pomocí rutiny New-AzVM (Preview) ve službě Cloud Shell 

Rutina New-AzVM (Preview) nabízí zjednodušený způsob vytvoření nového virtuálního počítače pomocí PowerShellu. Tato příručka podrobně popisuje použití PowerShellu ve službě Azure Cloud Shell s předinstalovanou rutinou New-AzVM k vytvoření nového virtuálního počítače Azure s Windows Serverem 2016. Po dokončení nasazení se připojíme k serveru pomocí protokolu RDP.  

Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) před tím, než začnete.


[!INCLUDE [cloud-shell-powershell](../../../includes/cloud-shell-powershell.md)]

## <a name="create-the-vm"></a>Vytvořte virtuální počítač.

Pomocí rutiny **New-AzVM** můžete vytvořit virtuální počítač s inteligentním výchozím nastavením, které zahrnuje použití image Windows Serveru 2016 Datacenter z Azure Marketplace. Rutinu New-AzVM můžete použít i samostatně a použijí se výchozí hodnoty pro názvy prostředků. V tomto příkladu jsme jako parametr **-Name** nastavili *myVM*. Rutina vytvoří všechny požadované prostředky a *myVM* použije jako předpona názvu prostředků. 

Ověřte, že ve službě Cloud Shell je vybraný **PowerShell**, a zadejte:

```azurepowershell-interactive
New-AzVm -Name myVM
```

Zobrazí se výzva k vytvoření uživatelského jména a hesla pro virtuální počítač, které se dále v tomto tématu použijí při připojení k tomuto virtuálnímu počítači. Heslo musí mít 12 až 123 znaků a musí splňovat tři ze čtyř bezpečnostních požadavků: jedno malé písmeno, jedno velké písmeno, jedna číslice a jeden speciální znak.

Vytvoření virtuálního počítače a přidružených prostředků trvá zhruba minutu. Po dokončení se zobrazí všechny prostředky, které se vytvořily pomocí rutiny [Find-AzureRmResource](/powershell/module/azurerm.resources/find-azurermresource).

```azurepowershell-interactive
Find-AzureRmResource `
    -ResourceGroupNameEquals myVMResourceGroup | Format-Table Name
```

## <a name="connect-to-the-vm"></a>Připojení k virtuálnímu počítači

Po dokončení nasazení vytvořte připojení ke vzdálené ploše virtuálního počítače.

Použijte příkaz [Get-AzureRmPublicIpAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress), který vrátí veřejnou IP adresu virtuálního počítače. Poznamenejte si tuto IP adresu.

```azurepowershell-interactive
Get-AzureRmPublicIpAddress `
    -ResourceGroupName myVMResourceGroup | Select IpAddress
```

Na místním počítači otevřete příkazový řádek a pomocí příkazu **mstsc** spusťte relaci vzdálené plochy s novým virtuálním počítačem. Parametr &lt;publicIPAddress&gt; nahraďte IP adresou vašeho virtuálního počítače. Po zobrazení výzvy zadejte uživatelské jméno a heslo, které jste zadali při vytváření tohoto virtuálního počítače.

```
mstsc /v:<publicIpAddress>
```

## <a name="clean-up-resources"></a>Vyčištění prostředků

Pokud už je nepotřebujete, můžete k odebrání skupiny prostředků, virtuálního počítače a všech souvisejících prostředků použít příkaz [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup).

```azurepowershell-interactive
Remove-AzureRmResourceGroup -Name myVMResourceGroup
```

## <a name="next-steps"></a>Další kroky

V tomto tématu jsme nasadili jednoduchý virtuální počítač pomocí rutiny New-AzVM a potom se k němu připojili přes RDP. Další informace o virtuálních počítačích Azure najdete v kurzu pro virtuální počítače s Windows.

> [!div class="nextstepaction"]
> [Kurzy pro virtuální počítače Azure s Windows](./tutorial-manage-vm.md)
