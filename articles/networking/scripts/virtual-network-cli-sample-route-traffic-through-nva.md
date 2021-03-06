---
title: "Ukázka skriptu Azure CLI - směrovat provoz prostřednictvím zařízení virtuální sítě | Microsoft Docs"
description: "Azure CLI ukázka skriptu - směrovat provoz prostřednictvím brány firewall sítě virtuálního zařízení."
services: virtual-network
documentationcenter: virtual-network
author: jimdial
manager: timlt
editor: tysonn
tags: 
ms.assetid: 
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: 
ms.workload: infrastructure
ms.date: 07/07/2017
ms.author: jdial
ms.openlocfilehash: af6e9d55038acba194f3cc631ff1b4e2e371852c
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/01/2018
---
# <a name="route-traffic-through-a-network-virtual-appliance"></a>Směrovat provoz prostřednictvím sítě virtuálního zařízení

Tento ukázkový skript vytvoří virtuální síť s podsítí front-end a back-end. Také vytvoří virtuální počítač se ke směrování provozu mezi dvěma podsítěmi povolené předávání IP. Po spuštění skriptu můžete nasadit software pro sítě, jako je například Brána firewall aplikace, do virtuálního počítače.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]


## <a name="sample-script"></a>Ukázkový skript


[!code-azurecli-interactive[main](../../../cli_scripts/virtual-network/route-traffic-through-nva/route-traffic-through-nva.sh "Route traffic through a network virtual appliance")]

## <a name="clean-up-deployment"></a>Vyčištění nasazení 

Spusťte následující příkaz pro odebrání skupiny prostředků, virtuální počítač a všechny související prostředky.

```azurecli
az group delete --name MyResourceGroup --yes
```

## <a name="script-explanation"></a>Vysvětlení skriptu

Tento skript používá následující příkazy k vytvoření skupiny prostředků, virtuální sítě a skupiny zabezpečení sítě. Každý příkaz v tabulce odkazy na dokumentaci specifické pro příkaz.

| Příkaz | Poznámky |
|---|---|
| [Vytvoření skupiny az](/cli/azure/group#az_group_create) | Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky. |
| [Vytvoření sítě vnet az](/cli/azure/network/vnet#az_network_vnet_create) | Vytvoří virtuální síť Azure a front-end podsítě. |
| [Vytvoření podsítě az sítě](/cli/azure/network/vnet/subnet#az_network_vnet_subnet_create) | Vytvoří back-end a DMZ podsítě. |
| [Vytvoření veřejné sítě az-ip](/cli/azure/network/public-ip#az_network_public_ip_create) | Vytvoří veřejnou IP adresu z Internetu přístup k virtuálnímu počítači. |
| [Vytvoření az síťových adaptérů sítě](/cli/azure/network/nic#az_network_nic_create) | Vytvoří rozhraní virtuální sítě a předávání IP povolit pro ni. |
| [Vytvoření az sítě nsg](/cli/azure/network/nsg#az_network_nsg_create) | Vytvoří skupinu zabezpečení sítě (NSG). |
| [Vytvoření pravidla nsg az sítě](/cli/azure/network/nsg/rule#az_network_nsg_rule_create) | Vytvoří pravidla NSG, které umožní příchozí porty HTTP a HTTPS k virtuálnímu počítači. |
| [aktualizace az sítě vnet podsíť](/cli/azure/network/vnet/subnet#az_network_vnet_subnet_update)| Přidruží skupiny Nsg a směrovací tabulky k podsítím. |
| [Vytvoření sítě az trasy – tabulka](/cli/azure/network/route-table#az_network_route_table_create)| Vytvoří směrovací tabulku pro všechny trasy. |
| [Vytvoření az síťovou směrovací tabulku trasu](/cli/azure/network/route-table/route#az_network_route_table_route_create)| Vytvoří směrování směrovat provoz mezi podsítěmi a Internetu prostřednictvím virtuálního počítače. |
| [Vytvoření virtuálního počítače az](/cli/azure/vm#az_vm_create) | Vytvoří virtuální počítač a k němu připojí na síťový adaptér. Tento příkaz také Určuje bitovou kopii virtuálního počítače používat a pověření pro správu. |
| [Odstranění skupiny az](/cli/azure/group#az_group_delete) | Odstraní skupinu prostředků a všechny prostředky, které obsahuje. |

## <a name="next-steps"></a>Další postup

Další informace o rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](/cli/azure/overview).

Další síťové ukázky skriptu rozhraní příkazového řádku najdete v [dokumentace přehled sítě Azure](../cli-samples.md)