---
title: "Azure skript prostředí PowerShell ukázkový – vytvoření spravovaného disku ze souboru VHD v účtu úložiště v předplatném stejný nebo jiný | Microsoft Docs"
description: "Azure skript prostředí PowerShell ukázkový – vytvoření spravovaného disku ze souboru VHD v účtu úložiště ve stejné nebo jiné předplatné"
services: virtual-machines-windows
documentationcenter: storage
author: ramankumarlive
manager: kavithag
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 06/05/2017
ms.author: ramankum
ms.openlocfilehash: 6d37fb1308ce0b866b42f961ada84d0869f25615
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-managed-disk-from-a-vhd-file-in-a-storage-account-in-same-or-different-subscription-with-powershell"></a>Vytvoření spravovaného disku ze souboru VHD v účtu úložiště ve stejné nebo jiné předplatné pomocí prostředí PowerShell

Tento skript vytvoří se spravovaným diskem z soubor virtuálního pevného disku v účtu úložiště ve stejné nebo jiné předplatné. Pomocí tohoto skriptu k importu specializované (není zobecněný/Sysprep) virtuálního pevného disku na disk spravovaný operačního systému k vytvoření virtuálního počítače. Navíc můžete importovat data virtuálního pevného disku na disk spravovaný data. 

Nevytvářejte více identické spravovaných disků ze souboru virtuálního pevného disku v malé množství času. Vytvoření spravované disky z soubor virtuálního pevného disku, je vytvořit snímek objektu blob souboru vhd a pak se používá k vytvoření spravovaného disky. Za minutu, která způsobuje chyby při vytváření disku z důvodu omezení lze vytvořit pouze jeden objekt blob snímku. Nechcete-li toto omezení, vytvořit [spravované snímku ze souboru virtuálního pevného disku](virtual-machines-windows-powershell-sample-create-snapshot-from-vhd.md?toc=%2fpowershell%2fmodule%2ftoc.json) a potom pomocí spravovaný spravované snímek, který chcete vytvořit několik disků v krátkém množství času. 

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-powershell.md](../../../includes/cloud-shell-powershell.md)]

Pokud si zvolíte instalaci a použití prostředí PowerShell místně, tento kurz vyžaduje, že jste prostředí Azure PowerShell verze modulu 4.0 nebo novější. Verzi zjistíte spuštěním příkazu `Get-Module -ListAvailable AzureRM`. Pokud je potřeba nainstalovat nebo upgradovat najdete v tématu [nainstalovat Azure PowerShell](/powershell/azure/install-azurerm-ps). Pokud používáte PowerShell místně, je také potřeba spustit příkaz `Login-AzureRmAccount` pro vytvoření připojení k Azure. 

## <a name="sample-script"></a>Ukázkový skript

[!code-powershell[main](../../../powershell_scripts/virtual-machine/create-managed-disks-from-vhd-in-different-subscription/create-managed-disks-from-vhd-in-different-subscription.ps1 "Create managed disk from VHD")]


## <a name="script-explanation"></a>Vysvětlení skriptu

Tento skript používá následující příkazy k vytvoření spravovaného disku z disku VHD v jiném předplatném. Každý příkaz v tabulce odkazy na dokumentaci konkrétní příkaz.

| Příkaz | Poznámky |
|---|---|
| [Nové AzureRmDiskConfig](/powershell/module/azurerm.compute/New-AzureRmDiskConfig) | Vytvoří konfigurace disku, který slouží k vytváření disků. Obsahuje typ úložiště, umístění, prostředek Id účtu úložiště, kde je uložený nadřazený virtuální pevný disk, identifikátor URI virtuálního pevného disku nadřazený virtuální pevný disk. |
| [Nové AzureRmDisk](/powershell/module/azurerm.compute/New-AzureRmDisk) | Vytvoří disk pomocí konfigurace disku, název disku a předány jako parametry název skupiny prostředků. |

## <a name="next-steps"></a>Další kroky

[Vytvoření virtuálního počítače připojením se spravovaným diskem jako disk operačního systému](./virtual-machines-windows-powershell-sample-create-vm-from-managed-os-disks.md?toc=%2fpowershell%2fmodule%2ftoc.json)

Další informace o modulu Azure PowerShell najdete v tématu [dokumentace Azure PowerShell](/powershell/azure/overview).

Ukázky skriptu PowerShell další virtuální počítač nachází v [virtuálního počítače Windows Azure dokumentaci](../windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).