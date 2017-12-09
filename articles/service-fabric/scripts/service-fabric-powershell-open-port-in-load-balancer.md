---
title: "Azure ukázkový skript prostředí PowerShell - port otevřete aplikace nástroji pro vyrovnávání zatížení | Microsoft Docs"
description: "Azure ukázkový skript prostředí PowerShell - otevřete port nástroji pro vyrovnávání zatížení Azure pro aplikace Service Fabric."
services: service-fabric
documentationcenter: 
author: rwike77
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 
ms.service: service-fabric
ms.workload: multiple
ms.devlang: na
ms.topic: sample
ms.date: 12/08/2017
ms.author: ryanwi
ms.custom: mvc
ms.openlocfilehash: c643fc9e575a8e836a361893d78348bbd627a425
ms.sourcegitcommit: 42ee5ea09d9684ed7a71e7974ceb141d525361c9
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/09/2017
---
# <a name="open-an-application-port-in-the-azure-load-balancer"></a>Otevřete port aplikace nástroji pro vyrovnávání zatížení Azure

Aplikace běžící v Azure Service Fabric se nachází za nástrojem pro vyrovnávání zatížení Azure. Tento ukázkový skript otevře port v k nástroji pro vyrovnávání zatížení Azure tak, aby aplikace Service Fabric může komunikovat s externími klienty. Podle potřeby upravte parametry. Pokud váš cluster je ve skupině zabezpečení sítě, také [přidání skupiny pravidla zabezpečení příchozích síťových](service-fabric-powershell-add-nsg-rule.md) chcete povolit příchozí přenosy.

V případě potřeby nainstalujte modul Service Fabric prostředí PowerShell s [Service Fabric SDK](../service-fabric-get-started.md). 

## <a name="sample-script"></a>Ukázkový skript

[!code-powershell[main](../../../powershell_scripts/service-fabric/open-port-in-load-balancer/open-port-in-load-balancer.ps1 "Open a port in the load balancer")]

## <a name="script-explanation"></a>Vysvětlení skriptu

Tento skript používá následující příkazy. Každý příkaz v tabulce odkazy na dokumentaci specifické pro příkaz.

| Příkaz | Poznámky |
|---|---|
| [Get-AzureRmResource](/powershell/module/azurerm.resources/get-azurermresource) | Získá prostředek služby Azure.  |
| [Get-AzureRmLoadBalancer](/powershell/module/azurerm.network/get-azurermloadbalancer) | Získá nástroje pro vyrovnávání zatížení Azure. |
| [Přidat AzureRmLoadBalancerProbeConfig](/powershell/module/azurerm.network/add-azurermloadbalancerprobeconfig) | Přidá test konfigurace pro vyrovnávání zatížení.|
| [Get-AzureRmLoadBalancerProbeConfig](/powershell/module/azurerm.network/get-azurermloadbalancerprobeconfig) | Získá test konfigurace pro vyrovnávání zatížení. |
| [Přidat AzureRmLoadBalancerRuleConfig](/powershell/module/azurerm.network/add-azurermloadbalancerruleconfig) | Přidá pravidlo konfigurace pro vyrovnávání zatížení. |
| [Set-AzureRmLoadBalancer](/powershell/module/azurerm.network/set-azurermloadbalancer) | Nastaví cíl stavu nástroje pro vyrovnávání zatížení. |

## <a name="next-steps"></a>Další kroky

Další informace o modulu Azure PowerShell najdete v tématu [dokumentace Azure PowerShell](/powershell/azure/overview).

Další ukázky pro Azure Service Fabric Powershell najdete v [prostředí Azure PowerShell ukázky](../service-fabric-powershell-samples.md).
