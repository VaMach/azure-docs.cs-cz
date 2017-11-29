---
title: "Azure ukázkový skript prostředí PowerShell - změnit rozsah portů protokolu RDP. | Dokumentace Microsoftu"
description: "Ukázkový skript prostředí PowerShell Azure - změní rozsah portů RDP nasazené clusteru."
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
ms.date: 11/28/2017
ms.author: ryanwi
ms.custom: mvc
ms.openlocfilehash: ca24c1afb9a29db7c5ff36d23a10da84ca2f45c3
ms.sourcegitcommit: cf42a5fc01e19c46d24b3206c09ba3b01348966f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/29/2017
---
# <a name="update-the-rdp-port-range-values"></a>Aktualizujte hodnoty rozsahu portu protokolu RDP

Tento ukázkový skript změní hodnoty rozsahu portu RDP na uzlu clusteru virtuálních počítačů po nasazení clusteru.  Prostředí Azure PowerShell slouží tak, aby základní virtuálních počítačů není cyklus.  Získá skript `Microsoft.Network/loadBalancers` prostředků ve skupině prostředků clusteru a aktualizací `inboundNatPools.frontendPortRangeStart` a `inboundNatPools.frontendPortRangeEnd` hodnoty. Podle potřeby upravte parametry.

V případě potřeby nainstalujte prostředí Azure PowerShell pomocí instrukce v nalezen [prostředí Azure PowerShell průvodce](/powershell/azure/overview). 

## <a name="sample-script"></a>Ukázkový skript

[!code-powershell[main](../../../powershell_scripts/service-fabric/change-rdp-port-range/change-rdp-port-range.ps1 "Update the RDP port range values")]

## <a name="script-explanation"></a>Vysvětlení skriptu

Tento skript používá následující příkazy. Každý příkaz v tabulce odkazy na dokumentaci konkrétní příkaz.

| Příkaz | Poznámky |
|---|---|
| [Get-AzureRmResource](/powershell/module/azurerm.resources/get-azurermresource) | Získá `Microsoft.Network/loadBalancers` prostředků. |
|[Set-AzureRmResource](/powershell/module/azurerm.resources/set-azurermresource)|Aktualizace `Microsoft.Network/loadBalancers` prostředků.|

## <a name="next-steps"></a>Další kroky

Další informace o modulu Azure PowerShell najdete v tématu [dokumentace Azure PowerShell](/powershell/azure/overview).

Další ukázky pro Azure Service Fabric Azure Powershell lze nalézt v [prostředí Azure PowerShell ukázky](../service-fabric-powershell-samples.md).
