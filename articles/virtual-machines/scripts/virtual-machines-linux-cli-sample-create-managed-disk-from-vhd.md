---
title: "Azure CLI skriptu ukázkové – vytvoření spravovaného disku ze souboru VHD v účtu úložiště ve stejném předplatném | Microsoft Docs"
description: "Azure CLI skriptu ukázkové – vytvoření spravovaného disku ze souboru VHD v účtu úložiště v rámci stejného předplatného"
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
ms.openlocfilehash: ede9457f5843d0a8a04503779970a553c5ed4f96
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-managed-disk-from-a-vhd-file-in-a-storage-account-in-the-same-subscription-with-cli"></a>Vytvoření spravovaného disku ze souboru VHD v účtu úložiště ve stejném předplatném pomocí rozhraní příkazového řádku

Tento skript vytvoří se spravovaným diskem z soubor virtuálního pevného disku v účtu úložiště ve stejném předplatném. Pomocí tohoto skriptu k importu specializované (není zobecněný/Sysprep) virtuálního pevného disku na disk spravovaný operačního systému k vytvoření virtuálního počítače. Nebo použijte k importu dat virtuálního pevného disku na disk spravovaný data. 


[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Ukázkový skript

[!code-azurecli[main](../../../cli_scripts/virtual-machine/create-managed-data-disks-from-vhd/create-managed-data-disks-from-vhd.sh "Create managed disk from VHD")]


## <a name="script-explanation"></a>Vysvětlení skriptu

Tento skript používá následující příkazy k vytvoření spravovaného disku z virtuálního pevného disku. Každý příkaz v tabulce odkazy na dokumentaci konkrétní příkaz.

| Příkaz | Poznámky |
|---|---|
| [Vytvoření az disku](https://docs.microsoft.com/cli/azure/disk#az_disk_create) | Vytvoří se spravovaným diskem pomocí identifikátoru URI virtuálního pevného disku v účtu úložiště v rámci stejného předplatného |

## <a name="next-steps"></a>Další kroky

[Vytvoření virtuálního počítače připojením se spravovaným diskem jako disk operačního systému](./virtual-machines-linux-cli-sample-create-vm-from-managed-os-disks.md?toc=%2fcli%2fmodule%2ftoc.json)

Další informace o rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).

Další virtuální počítač a spravované disky ukázky skriptu rozhraní příkazového řádku najdete v [virtuální počítač Azure s Linuxem dokumentaci](../../app-service/app-service-cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
