---
title: "Rozhraní příkazového řádku Azure Script ukázka – vytvoření virtuálního počítače s Windows Server 2016 s vyrovnáváním zatížení sítě | Microsoft Docs"
description: "Rozhraní příkazového řádku Azure Script ukázka – vytvoření virtuálního počítače s Windows Server 2016 s vyrovnáváním zatížení sítě"
services: virtual-machines-Windows
documentationcenter: virtual-machines
author: rickstercdn
manager: timlt
editor: tysonn
tags: 
ms.assetid: 
ms.service: virtual-machines-Windows
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 02/23/2017
ms.author: rclaus
ms.openlocfilehash: 9662e9a2ea128b609b76edfc4ec7b57016909761
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="load-balance-traffic-between-highly-available-virtual-machines"></a>Vyrovnávání zatížení přenosů mezi vysoce dostupné virtuální počítače

Tento ukázkový skript vytvoří vše potřebné ke spuštění několika Ubuntu virtuální počítače nakonfigurované v s vysokou dostupností a načíst vyrovnáváním konfiguraci. Po spuštění skriptu, budete mít tři virtuální počítače připojené k Azure skupiny dostupnosti a přístupné prostřednictvím Vyrovnávání zatížení Azure.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Ukázkový skript

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-vm-nlb/create-windows-vm-nlb.sh "Quick Create VM")]

## <a name="clean-up-deployment"></a>Vyčištění nasazení 

Spusťte následující příkaz pro odebrání skupiny prostředků, virtuální počítač a všechny související prostředky.

```azurecli-interactive 
az group delete --name myResourceGroup --yes
```

## <a name="script-explanation"></a>Vysvětlení skriptu

Tento skript používá následující příkazy k vytvoření skupiny prostředků, virtuální počítač, skupina dostupnosti, nástroj pro vyrovnávání zatížení a všechny související prostředky. Každý příkaz v tabulce odkazy na dokumentaci konkrétní příkaz.

| Příkaz | Poznámky |
|---|---|
| [Vytvoření skupiny az](https://docs.microsoft.com/cli/azure/group#az_group_create) | Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky. |
| [Vytvoření sítě vnet az](https://docs.microsoft.com/cli/azure/network/vnet#az_network_vnet_create) | Vytvoří virtuální síť Azure a podsíť. |
| [Vytvoření veřejné sítě az-ip](https://docs.microsoft.com/cli/azure/network/public-ip#az_network_public_ip_create) | Vytvoří veřejnou IP adresu se statickou IP adresu a přidružené název DNS. |
| [Vytvoření sítě lb az](https://docs.microsoft.com/cli/azure/network/lb#az_network_lb_create) | Vytvoří k síť Azure pro vyrovnávání zatížení (sítě NLB). |
| [Vytvoření az sítě lb testu](https://docs.microsoft.com/cli/azure/network/lb/probe#az_network_lb_probe_create) | Vytvoří kontrolu Vyrovnávání zatížení sítě. Vyrovnávání zatížení sítě testu se používá k monitorování jednotlivých virtuálních počítačů v sadě Vyrovnávání zatížení sítě. Pokud žádné virtuální počítače bude nedostupné, není provoz směruje na virtuální počítač. |
| [Vytvořit pravidlo lb az sítě](https://docs.microsoft.com/cli/azure/network/lb/rule#az_network_lb_rule_create) | Vytvoří pravidlo Vyrovnávání zatížení sítě. V této ukázce se vytvoří pravidlo pro port 80. Jako HTTP přenos dorazí na Vyrovnávání zatížení sítě, se směruje na portu 80 mezi virtuálními počítači v sadě Vyrovnávání zatížení sítě. |
| [Vytvoření az sítě lb příchozí--pravidlo nat](https://docs.microsoft.com/cli/azure/network/lb/inbound-nat-rule#az_network_lb_inbound_nat_rule_create) | Vytvoří pravidlo Vyrovnávání zatížení sítě překlad adres (NAT).  Pravidla NAT namapovat port služby NLB port na virtuálním počítači. V této ukázce se vytvoří pravidlo NAT pro provoz SSH pro každý virtuální počítač v sadě Vyrovnávání zatížení sítě.  |
| [Vytvoření az sítě nsg](https://docs.microsoft.com/cli/azure/network/nsg#az_network_nsg_create) | Vytvoří skupinu zabezpečení sítě (NSG), což je hranice zabezpečení mezi Internetu a virtuální počítač. |
| [Vytvoření pravidla nsg az sítě](https://docs.microsoft.com/cli/azure/network/nsg/rule#az_network_nsg_rule_create) | Vytvoří pravidlo NSG chcete povolit příchozí přenosy. V této ukázce je otevřen port 22 pro SSH provoz. |
| [Vytvoření az síťových adaptérů sítě](https://docs.microsoft.com/cli/azure/network/nic#az_network_nic_create) | Vytvoří virtuální síťové karty a připojí jej k virtuální síti, podsíti a NSG. |
| [Vytvoření virtuálního počítače az sady dostupnosti.](https://docs.microsoft.com/cli/azure/network/lb/rule#az_network_lb_rule_create) | Vytvoří skupinu dostupnosti. Skupiny dostupnosti zajistěte doba provozu aplikací tak, že se virtuální počítače napříč fyzické prostředky tak, že pokud dojde k selhání, není provedena celou sadu. |
| [Vytvoření virtuálního počítače az](https://docs.microsoft.com/cli/azure/vm/availability-set#az_vm_availability_set_create) | Vytvoří virtuální počítač a připojí jej k síťové karty, virtuální sítě, podsítě a NSG. Tento příkaz také určuje image virtuálního počítače jako přihlašovací údaje použité a správu.  |
| [Odstranění skupiny az](https://docs.microsoft.com/cli/azure/vm/extension#az_vm_extension_set) | Odstraní skupinu prostředků, včetně všech vnořených prostředků. |

## <a name="next-steps"></a>Další kroky

Další informace o rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).

Ukázky skriptu rozhraní příkazového řádku další virtuální počítač nachází v [virtuálního počítače Windows Azure dokumentaci](../windows/cli-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
