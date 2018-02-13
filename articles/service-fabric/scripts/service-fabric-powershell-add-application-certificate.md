---
title: "Azure skript prostředí PowerShell ukázkový – přidání certifikátu aplikací do clusteru s podporou | Microsoft Docs"
description: "Azure skript prostředí PowerShell ukázkový – přidání certifikát aplikací do clusteru Service Fabric."
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
ms.date: 01/18/2018
ms.author: ryanwi
ms.custom: mvc
ms.openlocfilehash: c9cf6485c2621f839b93da162e5f4d82a8d287a4
ms.sourcegitcommit: 2a70752d0987585d480f374c3e2dba0cd5097880
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/19/2018
---
# <a name="add-an-application-certificate-to-a-service-fabric-cluster"></a>Přidat certifikát aplikací do clusteru Service Fabric

Tento ukázkový skript vytvoří certifikát podepsaný svým držitelem v zadané Azure key vault a nainstaluje na všech uzlech tohoto clusteru Service Fabric. Certifikát se také stáhne do místní složky. Název stažený certifikát je stejný jako název certifikátu v trezoru klíčů. Podle potřeby upravte parametry.

V případě potřeby nainstalujte prostředí Azure PowerShell pomocí instrukce v nalezen [prostředí Azure PowerShell průvodce](/powershell/azure/overview) a spusťte `Login-AzureRmAccount` vytvořit připojení s Azure. 

## <a name="sample-script"></a>Ukázkový skript

[!code-powershell[main](../../../powershell_scripts/service-fabric/add-application-certificate/add-new-application-certificate.ps1 "Add an application certificate to a cluster")]

## <a name="script-explanation"></a>Vysvětlení skriptu

Tento skript používá následující příkazy: každý příkaz v tabulce odkazy na dokumentaci konkrétní příkaz.

| Příkaz | Poznámky |
|---|---|
| [Add-AzureRmServiceFabricApplicationCertificate](/powershell/module/azurerm.servicefabric/Add-AzureRmServiceFabricApplicationCertificate) | Přidáte nový certifikát aplikace do sady škálování virtuálního počítače, který vytváří cluster.  |

## <a name="next-steps"></a>Další kroky

Další informace o modulu Azure PowerShell najdete v tématu [dokumentace Azure PowerShell](/powershell/azure/overview).

Další ukázky pro Azure Service Fabric Azure Powershell lze nalézt v [prostředí Azure PowerShell ukázky](../service-fabric-powershell-samples.md).
