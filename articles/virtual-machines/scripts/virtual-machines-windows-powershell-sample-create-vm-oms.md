---
title: "Ukázka Azure skriptu prostředí PowerShell - OMS | Microsoft Docs"
description: "Ukázka Azure skriptu prostředí PowerShell - OMS"
services: virtual-machines-windows
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 12/12/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 2f9303568838113335343a420913b8dcb84cb49c
ms.sourcegitcommit: d247d29b70bdb3044bff6a78443f275c4a943b11
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/13/2017
---
# <a name="create-an-operations-management-suite-monitored-vm-with-powershell"></a>Vytvoření virtuálního počítače pomocí prostředí PowerShell monitorovat služby Operations Management Suite

Tento skript vytvoří virtuální počítač Azure, nainstaluje agenta nástroje Operations Management Suite (OMS) a zaregistruje systém s pracovní prostor služby OMS. Po spuštění skriptu se nebude zobrazovat v konzole OMS virtuálního počítače. Také je potřeba aktualizovat klíč ID a pracovního prostoru pracovní prostor OMS.

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Ukázkový skript

[!code-powershell[main](../../../powershell_scripts/virtual-machine/create-vm-monitor-oms/create-windows-vm-detailed-oms.ps1 "Create VM OMS")]

## <a name="clean-up-deployment"></a>Vyčištění nasazení 

Spusťte následující příkaz pro odebrání skupiny prostředků, virtuální počítač a všechny související prostředky.

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="script-explanation"></a>Vysvětlení skriptu

Tento skript používá následující příkazy k vytvoření nasazení. Každou položku v tabulce odkazy na dokumentaci konkrétní příkaz.

| Příkaz | Poznámky |
|---|---|
| [Nový AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) | Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky. |
| [Nový AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm) | Vytvoří virtuální počítač a připojí jej k síťové karty, virtuální sítě, podsítě a skupinu zabezpečení sítě. Tento příkaz také otevře port 80 a nastaví přihlašovací údaje pro správu. |
| [Set-AzureRmVMExtension](/powershell/module/azurerm.compute/set-azurermvmextension) | Přidání rozšíření virtuálního počítače do virtuálního počítače. V takovém případě rozšíření agenta Operations Management Suite se používá pro instalaci agenta OMS a registraci virtuálních počítačů v pracovním prostoru OMS. |
|[Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | Odebere skupinu prostředků a všechny prostředky obsažené v rámci. |

## <a name="next-steps"></a>Další kroky

Další informace o modulu Azure PowerShell najdete v tématu [dokumentace Azure PowerShell](/powershell/azure/overview).

Ukázky skriptu PowerShell další virtuální počítač nachází v [virtuálního počítače Windows Azure dokumentaci](../windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
