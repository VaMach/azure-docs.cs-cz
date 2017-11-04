---
title: "Rozhraní příkazového řádku Azure ukázkový skript – vytvoření virtuálního počítače s Linuxem | Microsoft Docs"
description: "Rozhraní příkazového řádku Azure ukázkový skript – vytvoření virtuálního počítače s Linuxem"
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
ms.openlocfilehash: 350084edbf1b604e2dc3674fd06288738a87a31a
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-fully-configured-virtual-machine"></a>Vytvoření kompletně nakonfigurovaný virtuálního počítače

Tento skript vytvoří virtuální počítač Azure s Ubuntu operačního systému. Po spuštění skriptu, můžete k virtuálnímu počítači přes protokol SSH.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Ukázkový skript

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-vm-detailed/create-vm-detailed.sh "Quick Create VM")]

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
| [Vytvoření sítě vnet az](https://docs.microsoft.com/cli/azure/network/vnet#az_network_vnet_create) | Vytvoří virtuální síť Azure a podsíť. |
| [Vytvoření veřejné sítě az-ip](https://docs.microsoft.com/cli/azure/network/public-ip#az_network_public_ip_create) | Vytvoří veřejnou IP adresu se statickou IP adresu a přidružené název DNS. |
| [Vytvoření az sítě nsg](https://docs.microsoft.com/cli/azure/network/nsg#az_network_nsg_create) | Vytvoří skupinu zabezpečení sítě (NSG), což je hranice zabezpečení mezi Internetu a virtuální počítač. |
| [Vytvoření pravidla nsg az sítě](https://docs.microsoft.com/cli/azure/network/nsg/rule#az_network_nsg_rule_create) | Vytvoří pravidlo NSG chcete povolit příchozí přenosy. V této ukázce je otevřen port 22 pro SSH provoz. |
| [Vytvoření az síťových adaptérů sítě](https://docs.microsoft.com/cli/azure/network/nic#az_network_nic_create) | Vytvoří virtuální síťové karty a připojí jej k virtuální síti, podsíti a NSG. |
| [Vytvoření virtuálního počítače az](https://docs.microsoft.com/cli/azure/vm#az_vm_create) | Vytvoří virtuální počítač a připojí jej k síťové karty, virtuální sítě, podsítě a NSG. Tento příkaz také Určuje bitovou kopii virtuálního počítače, který se má použít a pověření pro správu.  |
| [Odstranění skupiny az](https://docs.microsoft.com/cli/azure/vm/extension#az_vm_extension_set) | Odstraní skupinu prostředků, včetně všech vnořených prostředků. |

## <a name="next-steps"></a>Další kroky

Další informace o rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).

Ukázky skriptu rozhraní příkazového řádku další virtuální počítač nachází v [virtuální počítač Azure s Linuxem dokumentaci](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
