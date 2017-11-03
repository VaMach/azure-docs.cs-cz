---
title: "Ukázka skriptu Azure CLI - Export nebo zkopírování snímku jako virtuální pevný disk na účet úložiště v jiné oblasti. | Microsoft Docs"
description: "Ukázka skriptu Azure CLI - Export nebo zkopírování snímku jako virtuální pevný disk na účet úložiště ve stejné nebo jiné předplatné"
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
ms.openlocfilehash: dfb78106bc72aacee85f8412032165fdfcfc1ab3
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="exportcopy-managed-snapshots-as-vhd-to-a-storage-account-in-different-region-with-cli"></a>Export nebo zkopírování spravované snímků jako virtuální pevný disk na účet úložiště v jiné oblasti pomocí rozhraní příkazového řádku

Tento skript exporty spravované snímku na účet úložiště v jiné oblasti. Nejprve generuje identifikátor URI SAS snímku a používá je zkopírovat do účtu úložiště v jiné oblasti. Tento skript lze použijte k udržování zálohování spravované disky v jiné oblasti pro obnovení po havárii. 


[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Ukázkový skript

[!code-azurecli[main](../../../cli_scripts/virtual-machine/copy-snapshots-to-storage-account/copy-snapshots-to-storage-account.sh "Copy snapshot")]


## <a name="script-explanation"></a>Vysvětlení skriptu

Tento skript používá tyto příkazy ke generování identifikátor URI pro SAS pro spravované snímek a zkopíruje snímku na účet úložiště pomocí SAS URI. Každý příkaz v tabulce odkazy na dokumentaci konkrétní příkaz.

| Příkaz | Poznámky |
|---|---|
| [AZ snímku udělení přístupu](https://docs.microsoft.com/cli/azure/snapshot#az_snapshot_grant_access) | Vygeneruje SAS jen pro čtení, který slouží ke kopírování podkladový soubor virtuálního pevného disku na účet úložiště nebo stáhnout do místní  |
| [kopírování objektu blob úložiště az start](https://docs.microsoft.com/en-us/cli/azure/storage/blob/copy#az_storage_blob_copy_start) | Asynchronně zkopíruje objekt blob z jednoho účtu úložiště do druhého |

## <a name="next-steps"></a>Další kroky

[Vytvoření spravovaného disku z virtuálního pevného disku](virtual-machines-linux-cli-sample-create-managed-disk-from-vhd.md?toc=%2fcli%2fmodule%2ftoc.json)

[Vytvoření virtuálního počítače z spravovaného disku](./virtual-machines-linux-cli-sample-create-vm-from-managed-os-disks.md?toc=%2fcli%2fmodule%2ftoc.json)

Další informace o rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).

Další virtuální počítač a spravované disky ukázky skriptu rozhraní příkazového řádku najdete v [virtuální počítač Azure s Linuxem dokumentaci](../../app-service/app-service-cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
