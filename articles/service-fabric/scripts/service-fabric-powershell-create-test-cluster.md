---
title: "Azure skript prostředí PowerShell ukázkový – vytvořit cluster Service Fabric | Microsoft Docs"
description: "Azure skript prostředí PowerShell ukázkový – vytvořit cluster Service Fabric testovací tři uzly."
services: service-fabric
documentationcenter: 
author: rwike77
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 0f9c8bc5-3789-4eb3-8deb-ae6e2200795a
ms.service: service-fabric
ms.workload: multiple
ms.devlang: na
ms.topic: sample
ms.date: 12/12/2017
ms.author: ryanwi
ms.custom: mvc
ms.openlocfilehash: 03348efa4ebdaed987df73756c6b57da0cc76fb5
ms.sourcegitcommit: d247d29b70bdb3044bff6a78443f275c4a943b11
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/13/2017
---
# <a name="create-a-three-node-test-service-fabric-cluster"></a>Vytvořit testovací tři uzly clusteru Service Fabric

Tento ukázkový skript vytvoří testovací tři uzly clusteru Service Fabric zabezpečená certifikátem X.509. Konfigurace třemi uzly clusteru je podporována pro vývojové a testovací, protože můžete provést upgrady a zůstanou platné i po selhání jednotlivých uzlů (tak dlouho, dokud není nastat současně). Provozní cluster vyžadují pět nebo více uzlů, chcete-li být odolné vůči selhání současně.  

Příkaz vytvoří certifikát podepsaný svým držitelem a odešle ji do nového trezoru klíčů, který je vytvořen ve stejné skupině prostředků jako cluster. Certifikát se také zkopíruje do místního adresáře.  Nastavením parametru *-OS* zvolte verzi Windows nebo Linuxu, která se používá na uzlech clusteru.  Podle potřeby upravte parametry.

V případě potřeby nainstalujte prostředí Azure PowerShell pomocí instrukce v nalezen [prostředí Azure PowerShell průvodce](/powershell/azure/overview) a spusťte `Login-AzureRmAccount` vytvořit připojení s Azure. 

## <a name="sample-script"></a>Ukázkový skript

[!code-powershell[main](../../../powershell_scripts/service-fabric/create-test-cluster/create-test-cluster.ps1 "Create a test Service Fabric cluster")]

## <a name="clean-up-deployment"></a>Vyčištění nasazení 

Po spuštění ukázka skriptu, následující příkaz lze použít k odebrání skupiny prostředků clusteru a všechny související prostředky.

```powershell
$groupname="mysfclustergroup"
Remove-AzureRmResourceGroup -Name $groupname -Force
```

## <a name="script-explanation"></a>Vysvětlení skriptu

Tento skript používá následující příkazy. Každý příkaz v tabulce odkazy na dokumentaci konkrétní příkaz.

| Příkaz | Poznámky |
|---|---|
| [Nové AzureRmServiceFabricCluster](/powershell/module/azurerm.servicefabric/New-AzureRmServiceFabricCluster) | Vytvoří nový cluster Service Fabric. |

## <a name="next-steps"></a>Další kroky

Další informace o modulu Azure PowerShell najdete v tématu [dokumentace Azure PowerShell](/powershell/azure/overview).

Další ukázky pro Azure Service Fabric Azure Powershell lze nalézt v [prostředí Azure PowerShell ukázky](../service-fabric-powershell-samples.md).
