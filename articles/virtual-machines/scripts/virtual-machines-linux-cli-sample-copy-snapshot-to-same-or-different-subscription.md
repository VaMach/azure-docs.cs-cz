---
title: "Ukázka skriptu Azure CLI - kopírování (přesunout) snímek spravovaných disků na stejný nebo jiný odběr pomocí rozhraní příkazového řádku | Microsoft Docs"
description: "Ukázka skriptu Azure CLI - kopírování (přesunout) snímek spravovaných disků na stejný nebo jiný odběr pomocí rozhraní příkazového řádku"
services: virtual-machines-linux
documentationcenter: storage
author: ramankumarlive
manager: kavithag
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/19/2017
ms.author: ramankum
ms.custom: mvc
ms.openlocfilehash: 7c301a314ee946bb9199650bb7f674b8dce7c141
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="copy-snapshot-of-a-managed-disk-to-same-or-different-subscription-with-cli"></a>Kopírování snímku spravovaných disků na stejný nebo jiný odběr pomocí rozhraní příkazového řádku

Tento skript zkopíruje snímek spravovaných disků na stejný nebo jiný odběr. Pomocí tohoto skriptu snímek přesunout do jiného předplatného ve stejné oblasti jako nadřazené snímku.


[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Ukázkový skript

[!code-azurecli[main](../../../cli_scripts/virtual-machine/copy-snapshot-to-same-or-different-subscription/copy-snapshot-to-same-or-different-subscription.sh "Copy snapshot")]


## <a name="script-explanation"></a>Vysvětlení skriptu

Tento skript používá následující příkazy pro vytvoření snímku v cílové předplatné pomocí Id zdrojové snímku. Každý příkaz v tabulce odkazy na dokumentaci konkrétní příkaz.

| Příkaz | Poznámky |
|---|---|
| [Zobrazit az snímku](https://docs.microsoft.com/cli/azure/snapshot#az_snapshot_show) | Získá všechny vlastnosti snímku pomocí názvu a vlastnosti skupiny prostředků snímku. Vlastnost ID se používá ke kopírování snímku do jiného předplatného.  |
| [Vytvoření snímku az](https://docs.microsoft.com/cli/azure/snapshot#az_snapshot_create) | Zkopíruje snímek vytvořením snímku v jiné předplatné pomocí Id a název nadřazené snímku.  |

## <a name="next-steps"></a>Další kroky

[Vytvoření virtuálního počítače ze snímku](./virtual-machines-linux-cli-sample-create-vm-from-snapshot.md?toc=%2fpowershell%2fmodule%2ftoc.json)

Další informace o rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).

Další virtuální počítač a spravované disky ukázky skriptu rozhraní příkazového řádku najdete v [virtuální počítač Azure s Linuxem dokumentaci](../../app-service/app-service-cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
