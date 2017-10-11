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
ms.openlocfilehash: 78091b515c00591a4af8d807945475b6be50188a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="route-traffic-through-a-network-virtual-appliance"></a>Směrovat provoz prostřednictvím sítě virtuálního zařízení

Tento ukázkový skript vytvoří virtuální síť s podsítí front-end a back-end. Také vytvoří virtuální počítač se ke směrování provozu mezi dvěma podsítěmi povolené předávání IP. Po spuštění skriptu můžete nasadit software pro sítě, jako je například Brána firewall aplikace, do virtuálního počítače.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]


## <a name="sample-script"></a>Ukázkový skript


[!code-azurecli-interactive[hlavní](../../../cli_scripts/virtual-network/route-traffic-through-nva/route-traffic-through-nva.sh "směrování provozu prostřednictvím sítě virtuálního zařízení")]

## <a name="clean-up-deployment"></a>Vyčištění nasazení 

Spusťte následující příkaz pro odebrání skupiny prostředků, virtuální počítač a všechny související prostředky.

```azurecli
az group delete --name MyResourceGroup --yes
```

## <a name="script-explanation"></a>Vysvětlení skriptu

Tento skript používá následující příkazy k vytvoření skupiny prostředků, virtuální sítě a skupiny zabezpečení sítě. Každý příkaz v tabulce odkazy na dokumentaci specifické pro příkaz.

| Příkaz | Poznámky |
|---|---|
| [Vytvoření skupiny az](/cli/azure/group#create) | Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky. |
| [Vytvoření sítě vnet az](/cli/azure/network/vnet#create) | Vytvoří virtuální síť Azure a front-end podsítě. |
| [Vytvoření podsítě az sítě](/cli/azure/network/vnet/subnet#create) | Vytvoří back-end a DMZ podsítě. |
| [Vytvoření veřejné sítě az-ip](/cli/azure/network/public-ip#create) | Vytvoří veřejnou IP adresu z Internetu přístup k virtuálnímu počítači. |
| [Vytvoření az síťových adaptérů sítě](/cli/azure/network/nic#create) | Vytvoří rozhraní virtuální sítě a předávání IP povolit pro ni. |
| [Vytvoření az sítě nsg](/cli/azure/network/nsg#create) | Vytvoří skupinu zabezpečení sítě (NSG). |
| [Vytvoření pravidla nsg az sítě](/cli/azure/network/nsg/rule#create) | Vytvoří pravidla NSG, které umožní příchozí porty HTTP a HTTPS k virtuálnímu počítači. |
| [aktualizace az sítě vnet podsíť](/cli/azure/network/vnet/subnet#update)| Přidruží skupiny Nsg a směrovací tabulky k podsítím. |
| [Vytvoření sítě az trasy – tabulka](/cli/azure/network/route-table#create)| Vytvoří směrovací tabulku pro všechny trasy. |
| [Vytvoření az síťovou směrovací tabulku trasu](/cli/azure/network/route-table/route#create)| Vytvoří směrování směrovat provoz mezi podsítěmi a Internetu prostřednictvím virtuálního počítače. |
| [Vytvoření virtuálního počítače az](/cli/azure/vm#create) | Vytvoří virtuální počítač a k němu připojí na síťový adaptér. Tento příkaz také Určuje bitovou kopii virtuálního počítače používat a pověření pro správu. |
| [Odstranění skupiny az](/cli/azure/group#delete) | Odstraní skupinu prostředků a všechny prostředky, které obsahuje. |

## <a name="next-steps"></a>Další kroky

Další informace o rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](/cli/azure/overview).

Další síťové ukázky skriptu rozhraní příkazového řádku najdete v [dokumentace přehled sítě Azure](../cli-samples.md)