---
title: "Ukázka skriptu rozhraní příkazového řádku Azure – virtuální počítače restartovat | Microsoft Docs"
description: "Ukázka skriptu Azure CLI - restartování virtuálních počítačů, značky a ID"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: allclark
manager: douge
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 03/01/2017
ms.author: allclark
ms.custom: mvc
ms.openlocfilehash: ea114f484c774573b7d219cff9102a7308af356e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="restart-vms"></a>Restartujte virtuální počítače

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

Tento příklad ukazuje několik způsobů, jak získat některé virtuální počítače a je restartovat.

První restartování všech virtuálních počítačů ve skupině prostředků.

```bash
az vm restart --ids $(az vm list --resource-group myResourceGroup --query "[].id" -o tsv)
```

Získá druhý s příznakem virtuálních počítačů pomocí `az resouce list` a filtrů prostředky, které jsou virtuální počítače a znovu tyto virtuální počítače.

```bash
az vm restart --ids $(az resource list --tag "restart-tag" --query "[?type=='Microsoft.Compute/virtualMachines'].id" -o tsv)
```

Tato ukázka funguje v prostředí Bash. Možnosti na spouštění skriptů rozhraní příkazového řádku Azure v klientovi Windows najdete v tématu [běžící ve Windows Azure CLI](../windows/cli-options.md).


## <a name="sample-script"></a>Ukázkový skript

Ukázka má tři skripty.
První z nich zřídí virtuálních počítačů.
Takže příkaz vrátí bez čekání na jednotlivé virtuální počítače, které se má zřídit používá možnost Ne čekání.
Druhý čeká kompletní zřízení virtuálních počítačů.
Třetí skript restartování všech virtuálních počítačů, které byly zřízené, pak jenom s příznakem virtuálních počítačů.

### <a name="provision-the-vms"></a>Zřizování virtuálních počítačů

Tento skript vytvoří skupinu prostředků a vytvoří tři virtuální počítače restartovat.
Dvě z nich jsou označené.

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/restart-by-tag/provision.sh "Provision the VMs")]

### <a name="wait"></a>Wait

Tento skript kontroluje stav zřizování každých 20 sekund, dokud všechny tři virtuální počítače jsou zřízené nebo jeden z nich selže zřídit.

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/restart-by-tag/wait.sh "Wait for the VMs to be provisioned")]

### <a name="restart-the-vms"></a>Restartujte virtuální počítače

Tento skript restartuje všechny virtuální počítače ve skupině prostředků, a pak znovu právě s příznakem virtuálních počítačů.

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/restart-by-tag/restart.sh "Restart VMs by tag")]

## <a name="clean-up-deployment"></a>Vyčištění nasazení 

Po spuštění ukázka skriptu, následující příkaz lze použít k odebrání skupiny prostředků, virtuální počítače a všechny související prostředky.

```azurecli-interactive 
az group delete -n myResourceGroup --no-wait --yes
```

## <a name="script-explanation"></a>Vysvětlení skriptu

Tento skript používá následující příkazy k vytvoření skupiny prostředků, virtuální počítač, skupina dostupnosti, nástroj pro vyrovnávání zatížení a všechny související prostředky. Každý příkaz v tabulce odkazy na dokumentaci konkrétní příkaz.

| Příkaz | Poznámky |
|---|---|
| [Vytvoření skupiny az](https://docs.microsoft.com/cli/azure/group#az_group_create) | Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky. |
| [Vytvoření virtuálního počítače az](https://docs.microsoft.com/cli/azure/vm/availability-set#az_vm_availability_set_create) | Vytvoří virtuální počítače.  |
| [Seznam virtuálních počítačů az](https://docs.microsoft.com/cli/azure/vm#az_vm_list) | Použít s `--query` , aby se před restartováním, jsou zřizovat virtuální počítače a potom získat ID virtuálních počítačů se je znovu spustit. |
| [Seznam zdrojů az](https://docs.microsoft.com/cli/azure/vm#az_vm_list) | Použít s `--query` získat ID virtuálních počítačů pomocí značky. |
| [AZ restartování virtuálního počítače](https://docs.microsoft.com/cli/azure/vm#az_vm_list) | Restartování virtuálních počítačů. |
| [Odstranění skupiny az](https://docs.microsoft.com/cli/azure/vm/extension#az_vm_extension_set) | Odstraní skupinu prostředků, včetně všech vnořených prostředků. |

## <a name="next-steps"></a>Další kroky

Další informace o rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).

Ukázky skriptu rozhraní příkazového řádku další virtuální počítač nachází v [virtuální počítač Azure s Linuxem dokumentaci](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
