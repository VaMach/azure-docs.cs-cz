---
title: "Azure CLI skriptu ukázkové – vytvoření sítě pro vícevrstvé aplikace | Microsoft Docs"
description: "Azure CLI skriptu ukázkové – vytvoření virtuální sítě pro vícevrstvé aplikace."
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
ms.openlocfilehash: de65d820f2d9eea49b58185c81d815675fd76740
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-network-for-multi-tier-applications"></a>Vytvoření sítě pro vícevrstvé aplikace

Tento ukázkový skript vytvoří virtuální síť s podsítí front-end a back-end. Provoz do front-endu podsítě je omezen na protokolu HTTP a SSH, zatímco provozu do podsítě back-end je omezený na MySQL, port 3306. Po spuštění skriptu, budete mít dva virtuální počítače, jeden v jednotlivých podsítích, kterou můžete nasadit webový server a databáze MySQL software.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]


## <a name="sample-script"></a>Ukázkový skript


[!code-azurecli-interactive[main](../../../cli_scripts/virtual-network/virtual-network-multi-tier-application/virtual-network-multi-tier-application.sh  "Virtual network for multi-tier application")]

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
| [Vytvoření podsítě az sítě](/cli/azure/network/vnet/subnet#create) | Vytvoří podsíť back-end. |
| [Vytvoření veřejné sítě az-ip](/cli/azure/network/public-ip#create) | Vytvoří veřejnou IP adresu z Internetu přístup k virtuálnímu počítači. |
| [Vytvoření az síťových adaptérů sítě](/cli/azure/network/nic#create) | Vytvoří rozhraní virtuální sítě a připojí je k podsítím virtuální sítě front-end a back-end. |
| [Vytvoření az sítě nsg](/cli/azure/network/nsg#create) | Vytvoří síť skupiny zabezpečení (NSG), které jsou přidruženy k podsítím front-end a back-end. |
| [Vytvoření pravidla nsg az sítě](/cli/azure/network/nsg/rule#create) |Vytvoří pravidla NSG, které povolí nebo blokuje specifické porty ke konkrétním podsítím. |
| [Vytvoření virtuálního počítače az](/cli/azure/vm#create) | Vytvoří virtuální počítače a připojí síťovou kartu pro každý virtuální počítač. Tento příkaz také Určuje bitovou kopii virtuálního počítače používat a pověření pro správu. |
| [Odstranění skupiny az](/cli/azure/group#delete) | Odstraní skupinu prostředků a všechny prostředky, které obsahuje. |

## <a name="next-steps"></a>Další kroky

Další informace o rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](/cli/azure/overview).

Další síťové ukázky skriptu rozhraní příkazového řádku najdete v [dokumentace přehled sítě Azure](../cli-samples.md)