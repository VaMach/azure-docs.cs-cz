---
title: "Azure skript prostředí PowerShell ukázkový – vytvoření sítě pro vícevrstvé aplikace | Microsoft Docs"
description: "Azure skript prostředí PowerShell ukázkový – vytvoření virtuální sítě pro vícevrstvé aplikace."
services: virtual-network
documentationcenter: virtual-network
author: georgewallace
manager: timlt
editor: tysonn
tags: 
ms.assetid: 
ms.service: virtual-network
ms.devlang: powershell
ms.topic: article
ms.tgt_pltfrm: 
ms.workload: infrastructure
ms.date: 05/16/2017
ms.author: gwallace
ms.openlocfilehash: ab49e78ef17b093d2bbe4e3276a1ece3a4247f91
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-network-for-multi-tier-applications"></a>Vytvoření sítě pro vícevrstvé aplikace

Tento ukázkový skript vytvoří virtuální síť s podsítí front-end a back-end. Provoz do front-endu podsítě je omezen na protokolu HTTP a SSH, zatímco provozu do podsítě back-end je omezený na MySQL, port 3306. Po spuštění skriptu, budete mít dva virtuální počítače, jeden v jednotlivých podsítích, kterou můžete nasadit webový server a databáze MySQL software.

V případě potřeby nainstalujte prostředí Azure PowerShell pomocí instrukce v nalezen [prostředí Azure PowerShell průvodce](https://docs.microsoft.com/powershell/azureps-cmdlets-docs/)a poté spusťte `Login-AzureRmAccount` vytvořit připojení s Azure.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Ukázkový skript


[!code-powershell[hlavní](../../../powershell_scripts/virtual-network/virtual-network-multi-tier-application/virtual-network-multi-tier-application.ps1  "virtuální sítě pro vícevrstvé aplikace")]

## <a name="clean-up-deployment"></a>Vyčištění nasazení 

Spusťte následující příkaz pro odebrání skupiny prostředků, virtuální počítač a všechny související prostředky.

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="script-explanation"></a>Vysvětlení skriptu

Tento skript používá následující příkazy k vytvoření skupiny prostředků, virtuální sítě a skupiny zabezpečení sítě. Každý příkaz v tabulce odkazy na dokumentaci specifické pro příkaz.

| Příkaz | Poznámky |
|---|---|
| [Nový AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) | Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky. |
| [Nový AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork) | Vytvoří virtuální síť Azure a front-end podsítě. |
| [Nové AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig) | Vytvoří podsíť back-end. |
| [Nové AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress) | Vytvoří veřejnou IP adresu z Internetu přístup k virtuálnímu počítači. |
| [Nové AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface) | Vytvoří rozhraní virtuální sítě a připojí je k podsítím virtuální sítě front-end a back-end. |
| [Nové AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup) | Vytvoří síť skupiny zabezpečení (NSG), které jsou přidruženy k podsítím front-end a back-end. |
| [Nové AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig) |Vytvoří pravidla NSG, které povolí nebo blokuje specifické porty ke konkrétním podsítím. |
| [Nový AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm) | Vytvoří virtuální počítače a připojí síťovou kartu pro každý virtuální počítač. Tento příkaz také Určuje bitovou kopii virtuálního počítače používat a pověření pro správu. |
| [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | Odstraní skupinu prostředků a všechny prostředky, které obsahuje. |

## <a name="next-steps"></a>Další kroky

Další informace o prostředí Azure PowerShell najdete v tématu [dokumentace Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview).

Další ukázky sítě skript prostředí PowerShell najdete v [přehled sítě Azure dokumentaci](../powershell-samples.md?toc=%2fazure%2fnetworking%2ftoc.json).