---
title: "Odpojit datový disk z virtuálního počítače Windows - Azure | Microsoft Docs"
description: "Naučte se odpojit datový disk z virtuálního počítače v Azure pomocí modelu nasazení Resource Manager."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 13180343-ac49-4a3a-85d8-0ead95e2028c
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 03/21/2017
ms.author: cynthn
ms.openlocfilehash: bbbd31313db44d32a829e9e4c6c9b5fd9c0e533e
ms.sourcegitcommit: d41d9049625a7c9fc186ef721b8df4feeb28215f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/02/2017
---
# <a name="how-to-detach-a-data-disk-from-a-windows-virtual-machine"></a>Jak se odpojit datový disk od virtuálního počítače s Windows
Když už nepotřebujete datový disk připojený k virtuálnímu počítači, můžete ho jednoduše odpojit. Odebere disk z virtuálního počítače, ale neodstraní z úložiště.

> [!WARNING]
> Pokud se odpojit disk není automaticky odstraněn. Pokud jste přihlášení k odběru služby storage úrovně Premium, můžete nadále toho vám být účtovány poplatky za úložiště pro disk. Další informace najdete v části [ceny a fakturace při použití služby Premium Storage](premium-storage.md#pricing-and-billing).
>
>

Pokud znovu chcete použít stávající data na disku, můžete ho znovu připojit ke stejnému nebo jinému virtuálnímu počítači.

## <a name="detach-a-data-disk-using-the-portal"></a>Odpojení datového disku pomocí portálu
1. V centru portálu vyberte **virtuální počítače**.
2. Vyberte virtuální počítač, který má datový disk, kterou chcete odpojit a klikněte na tlačítko **Zastavit** se zrušit přidělení virtuálního počítače.
3. V okně virtuálního počítače vyberte **disky**.
4. V horní části **disky** vyberte **upravit**.
5. V **disky** okno na pravé straně datový disk, který chcete odpojit, klikněte na tlačítko ![obrázek tlačítka odpojení](./media/detach-disk/detach.png) odpojit tlačítko.
5. Po odebrání disku nahoře v okně klikněte na tlačítko Uložit.
6. V okně virtuálního počítače klikněte na **přehled** a klikněte **spustit** tlačítka v horní části okna restartujte virtuální počítač.



Disk zůstává v úložišti, ale už není připojený k virtuálnímu počítači.

## <a name="detach-a-data-disk-using-powershell"></a>Odpojit datový disk pomocí prostředí PowerShell
V tomto příkladu v prvním příkazu je získán virtuální počítač s názvem **MyVM07** v **RG11** pomocí rutiny Get-AzureRmVM skupinu prostředků. Příkaz uloží virtuální počítač v **$VirtualMachine** proměnné.

Druhý příkaz odebere datový disk s názvem DataDisk3 z virtuálního počítače.

Poslední příkaz aktualizuje stav virtuálního počítače dokončete proces odebrání datový disk.

```azurepowershell-interactive
$VirtualMachine = Get-AzureRmVM -ResourceGroupName "RG11" -Name "MyVM07"
Remove-AzureRmVMDataDisk -VM $VirtualMachine -Name "DataDisk3"
Update-AzureRmVM -ResourceGroupName "RG11" -Name "MyVM07" -VM $VirtualMachine
```

Další informace najdete v tématu [odebrat AzureRmVMDataDisk](/powershell/module/azurerm.compute/remove-azurermvmdatadisk).

## <a name="next-steps"></a>Další kroky
Pokud chcete použít datový disk, jste právě [připojte ji k jiným virtuálním Počítačem](attach-managed-disk-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

