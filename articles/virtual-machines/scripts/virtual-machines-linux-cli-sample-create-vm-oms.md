---
title: "Azure CLI skriptu ukázkové – vytvoření virtuálního počítače s Linuxem pomocí OMS monitorování | Microsoft Docs"
description: "Azure CLI skriptu ukázkové – vytvoření virtuálního počítače s Linuxem pomocí monitorování OMS"
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
ms.openlocfilehash: cc0cf9a990615ad96f532072b5628a5c807e5e83
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="monitor-a-vm-with-operations-management-suite"></a>Monitorování virtuálních počítačů pomocí služby Operations Management Suite

Tento skript vytvoří virtuální počítač Azure, nainstaluje agenta nástroje Operations Management Suite (OMS) a zaregistruje systém s pracovní prostor služby OMS. Po spuštění skriptu se nebude zobrazovat v konzole OMS virtuálního počítače.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Ukázkový skript

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-vm-monitor-oms/create-vm-monitor-oms.sh "Quick Create VM")]

## <a name="clean-up-deployment"></a>Vyčištění nasazení 

Spusťte následující příkaz pro odebrání skupiny prostředků, virtuální počítač a všechny související prostředky.

```azurecli-interactive 
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Vysvětlení skriptu

Tento skript používá následující příkazy k vytvoření skupiny prostředků, virtuální počítač a všechny související prostředky. Každý příkaz v tabulce odkazy na dokumentaci konkrétní příkaz.

| Příkaz | Poznámky |
|---|---|
| [Vytvoření skupiny az](https://docs.microsoft.com/cli/azure/group#az_group_create) | Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky. |
| [Vytvoření virtuálního počítače az](https://docs.microsoft.com/cli/azure/vm#az_vm_create) | Vytvoří virtuální počítač a připojí jej k síťové karty, virtuální sítě, podsítě a NSG. Tento příkaz také Určuje bitovou kopii virtuálního počítače, který se má použít a pověření pro správu.  |
| [sada rozšíření virtuálního počítače Azure](https://docs.microsoft.com/cli/azure/vm/extension#az_vm_extension_set) | Virtuální počítač spouští rozšíření virtuálního počítače. V takovém případě rozšíření agenta Operations Management Suite se používá pro instalaci agenta OMS a registraci virtuálních počítačů v pracovním prostoru OMS. |
| [Odstranění skupiny az](https://docs.microsoft.com/cli/azure/vm/extension#az_vm_extension_set) | Odstraní skupinu prostředků, včetně všech vnořených prostředků. |

## <a name="next-steps"></a>Další kroky

Další informace o rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).

Ukázky skriptu rozhraní příkazového řádku další virtuální počítač nachází v [virtuální počítač Azure s Linuxem dokumentaci](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
