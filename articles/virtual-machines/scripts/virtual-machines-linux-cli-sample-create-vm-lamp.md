---
title: "Rozhraní příkazového řádku Azure ukázkový skript – nasazení zásobníku svítilna ve Škálovací sadě Vyrovnávání zatížení sítě Machin virtuální | Microsoft Docs"
description: "Použití rozšíření vlastních skriptů k nasazení svítilna zásobníku v zatížení = sad v Azure škálování vyrovnáváním virtuálního počítače."
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
ms.date: 04/05/2017
ms.author: allclark
ms.custom: mvc
ms.openlocfilehash: 23170923d7c05c9b7230cf331725250b2a3c0f09
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="deploy-the-lamp-stack-in-a-load-balanced-virtual-machine-scale-set"></a>Nasazení svítilna zásobníku v sadě škálování Vyrovnávání zatížení sítě virtuálních počítačů

Tento příklad vytvoří škálovací sadu virtuálních počítačů a platí rozšíření, které spouští vlastní skript k nasazení zásobníku svítilna na každém virtuálním počítači v sadě škálování.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

## <a name="sample-script"></a>Ukázkový skript

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-scaleset-php-ansible/build-stack.sh "Create virtual machine scale set with LAMP stack")]

## <a name="connect"></a>Připojení

Pomocí tohoto kódu Pokud chcete zjistit, jak se připojit k virtuální počítače a vaše škálování nastavit.

[!code-azurecli[main](../../../cli_scripts/virtual-machine/create-scaleset-php-ansible/how-to-access.sh "Access the virtual machine scale set")]

## <a name="clean-up-deployment"></a>Vyčištění nasazení 

Spusťte následující příkaz pro odebrání skupiny prostředků, sadě škálování a virtuální počítače a všechny související prostředky.

```azurecli-interactive 
az group delete -n myResourceGroup
```

## <a name="script-explanation"></a>Vysvětlení skriptu

Tento skript používá následující příkazy k vytvoření skupiny prostředků, virtuální počítač, skupina dostupnosti, nástroj pro vyrovnávání zatížení a všechny související prostředky. Každý příkaz v tabulce odkazy na dokumentaci konkrétní příkaz.

| Příkaz | Poznámky |
|---|---|
| [Vytvoření skupiny az](https://docs.microsoft.com/cli/azure/group#az_group_create) | Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky. |
| [Vytvoření az vmss](https://docs.microsoft.com/cli/azure/vmss#az_vmss_create) | Vytvoří škálovací sadu virtuálních počítačů |
| [Vytvořit pravidlo lb az sítě](https://docs.microsoft.com/cli/azure/network/lb/rule#az_network_lb_rule_create) | Přidat koncový bod Vyrovnávání zatížení sítě |
| [sada rozšíření vmss az](https://docs.microsoft.com/cli/azure/vmss/extension#az_vmss_extension_set) | Vytváření rozšíření, který spouští vlastních skriptů při nasazení virtuálního počítače |
| [AZ vmss aktualizace instance](https://docs.microsoft.com/cli/azure/vmss#az_vmss_update_instances) | Spusťte vlastní skript v rámci instancí virtuálních počítačů, které byly nasazené, než bylo použito rozšíření pro sadu škálování. |
| [AZ vmss škálování](https://docs.microsoft.com/cli/azure/vmss#az_vmss_scale) | Škálování měřítka nastavený přidáním více instancí virtuálního počítače. Vlastní skript běží v nich při jejich nasazení. |
| [seznam veřejné ip az sítě](https://docs.microsoft.com/cli/azure/network/public-ip#az_network_public_ip_list) | Získáte IP adresy virtuálních počítačů vytvořené vzorku. |
| [Zobrazit lb az sítě](https://docs.microsoft.com/cli/azure/network/lb#az_network_lb_show) | Získejte front-endu a back-end porty používané pro vyrovnávání zatížení. |

## <a name="next-steps"></a>Další kroky

Další informace o rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).

Ukázky skriptu rozhraní příkazového řádku další virtuální počítač nachází v [virtuální počítač Azure s Linuxem dokumentaci](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
