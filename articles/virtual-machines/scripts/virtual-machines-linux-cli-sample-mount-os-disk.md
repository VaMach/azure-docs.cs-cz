---
title: "Ukázka skriptu Azure CLI - disk operačního systému připojit | Microsoft Docs"
description: "Ukázka skriptu Azure CLI - připojení disku operačního systému"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 02/27/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: c32ea5e6cade34a9c8dac0eab523ebcaa10ef039
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="troubleshoot-a-vms-operating-system-disk"></a>Řešení potíží s diskem operačního systému virtuálních počítačů

Tento skript připojí disk operačního systému virtuálního počítače se nezdařilo nebo problematické jako datový disk do druhého virtuálního počítače. To může být užitečné při řešení potíží s disku problémy nebo obnovení dat. 

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Ukázkový skript

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/mount-os-disk/mount-os-disk.sh "Quick Create VM")]

## <a name="script-explanation"></a>Vysvětlení skriptu

Tento skript používá následující příkazy k vytvoření skupiny prostředků, virtuální počítač a všechny související prostředky. Každý příkaz v tabulce odkazy na dokumentaci konkrétní příkaz.

| Příkaz | Poznámky |
|---|---|
| [Zobrazit az virtuálních počítačů](https://docs.microsoft.com/cli/azure/vm#az_vm_show) | Vrátí seznam virtuálních počítačů. V takovém případě se používá možnost dotazu vrátit disku operačního systému virtuálního počítače. Tato hodnota se pak přidá k názvu proměnné 'uri'. |
| [Odstranění virtuálního počítače az](https://docs.microsoft.com/cli/azure/vm#az_vm_delete) | Odstraní virtuální počítač. |
| [Vytvoření virtuálního počítače az](https://docs.microsoft.com/cli/azure/vm#az_vm_create) | Vytvoří virtuální počítač.  |
| [připojit disk az virtuálních počítačů](https://docs.microsoft.com/cli/azure/vm/disk#az_vm_disk_attach) | Disk se připojuje k virtuálnímu počítači. |
| [virtuální počítač az seznam ip adres](https://docs.microsoft.com/cli/azure/vm#az_vm_list_ip_addresses) | Vrátí IP adresy virtuálního počítače. |

## <a name="next-steps"></a>Další kroky

Další informace o rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).

Ukázky skriptu rozhraní příkazového řádku další virtuální počítač nachází v [virtuální počítač Azure s Linuxem dokumentaci](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
