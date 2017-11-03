---
title: "Ukázka skriptu Azure Powershellu - směrovat provoz prostřednictvím zařízení virtuální sítě | Microsoft Docs"
description: "Azure PowerShell ukázka skriptu - směrovat provoz prostřednictvím brány firewall sítě virtuálního zařízení."
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
ms.openlocfilehash: 883d28dac72a66c2186d222f72b04d68e532cead
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="route-traffic-through-a-network-virtual-appliance"></a>Směrovat provoz prostřednictvím sítě virtuálního zařízení

Tento ukázkový skript vytvoří virtuální síť s podsítí front-end a back-end. Také vytvoří virtuální počítač se ke směrování provozu mezi dvěma podsítěmi povolené předávání IP. Po spuštění skriptu můžete nasadit software pro sítě, jako je například Brána firewall aplikace, do virtuálního počítače.

V případě potřeby nainstalujte prostředí Azure PowerShell pomocí instrukce v nalezen [prostředí Azure PowerShell průvodce](https://docs.microsoft.com/powershell/azureps-cmdlets-docs/)a poté spusťte `Login-AzureRmAccount` vytvořit připojení s Azure.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Ukázkový skript


[!code-powershell[main](../../../powershell_scripts/virtual-network/route-traffic-through-nva/route-traffic-through-nva.ps1 "Route traffic through a network virtual appliance")]

## <a name="clean-up-deployment"></a>Vyčištění nasazení 

Spusťte následující příkaz pro odebrání skupiny prostředků, virtuální počítač a všechny související prostředky.

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```
## <a name="script-explanation"></a>Vysvětlení skriptu

Tento skript používá následující příkazy k vytvoření skupiny prostředků, virtuální sítě a skupiny zabezpečení sítě. Každý příkaz v tabulce odkazy na dokumentaci specifické pro příkaz.

| Příkaz | Poznámky |
|---|---|
| [Nový AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup)  | Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky. |
| [Nový AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork) | Vytvoří virtuální síť Azure a front-end podsítě. |
| [Nové AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig) | Vytvoří back-end a DMZ podsítě. |
| [Nové AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress) | Vytvoří veřejnou IP adresu z Internetu přístup k virtuálnímu počítači. |
| [Nové AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface) | Vytvoří rozhraní virtuální sítě a předávání IP povolit pro ni. |
| [Nové AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup) | Vytvoří skupinu zabezpečení sítě (NSG). |
| [Nové AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig) | Vytvoří pravidla NSG, které umožní příchozí porty HTTP a HTTPS k virtuálnímu počítači. |
| [Set-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/set-azurermvirtualnetworksubnetconfig)| Přidruží skupiny Nsg a směrovací tabulky k podsítím. |
| [Nové AzureRmRouteTable](/powershell/module/azurerm.network/new-azurermroutetable)| Vytvoří směrovací tabulku pro všechny trasy. |
| [Nové AzureRMRouteConfig](/powershell/module/azurerm.network/new-azurermrouteconfig)| Vytvoří směrování směrovat provoz mezi podsítěmi a Internetu prostřednictvím virtuálního počítače. |
| [Nový AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm) | Vytvoří virtuální počítač a k němu připojí na síťový adaptér. Tento příkaz také Určuje bitovou kopii virtuálního počítače používat a pověření pro správu. |
| [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup)  | Odstraní skupinu prostředků a všechny prostředky, které obsahuje. |

## <a name="next-steps"></a>Další kroky

Další informace o prostředí Azure PowerShell najdete v tématu [dokumentace Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview).

Další ukázky sítě skript prostředí PowerShell najdete v [přehled sítě Azure dokumentaci](../powershell-samples.md?toc=%2fazure%2fnetworking%2ftoc.json).