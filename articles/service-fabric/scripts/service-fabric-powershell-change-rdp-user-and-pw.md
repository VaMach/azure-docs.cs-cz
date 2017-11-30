---
title: "Azure skript prostředí PowerShell ukázkový – aktualizovat RDP uživatelské jméno a heslo | Microsoft Docs"
description: "Azure skript prostředí PowerShell ukázkový – aktualizovat RDP uživatelské jméno a heslo pro všechny uzly clusteru Service Fabric určitý typ uzlu."
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
ms.date: 11/17/2017
ms.author: ryanwi
ms.custom: mvc
ms.openlocfilehash: 3b97cee11c9a85cbd60a05bdbdcd010a0f0a106f
ms.sourcegitcommit: 29bac59f1d62f38740b60274cb4912816ee775ea
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/29/2017
---
# <a name="update-the-admin-username-and-password-of-the-vms-in-a-cluster"></a>Aktualizace jméno a heslo správce virtuálních počítačů v clusteru

Každý typ uzlu v clusteru Service Fabric je škálovací sadu virtuálních počítačů. Tento ukázkový skript aktualizuje uživatelské jméno správce a heslo pro virtuální počítače clusteru v konkrétním uzlu typu.  Přidejte rozšíření VMAccessAgent k sadě škálování, protože heslo správce není upravitelnými škálování nastavte vlastnost.  Uživatelské jméno a heslo změny použít na všech uzlech v sadě škálování. Podle potřeby upravte parametry.

V případě potřeby nainstalujte prostředí Azure PowerShell pomocí instrukce v nalezen [prostředí Azure PowerShell průvodce](/powershell/azure/overview). 

## <a name="sample-script"></a>Ukázkový skript

[!code-powershell[main](../../../powershell_scripts/service-fabric/change-rdp-user-and-pw/change-rdp-user-and-pw.ps1 "Updates a RDP username and password for cluster nodes")]

## <a name="script-explanation"></a>Vysvětlení skriptu

Tento skript používá následující příkazy: každý příkaz v tabulce odkazy na dokumentaci konkrétní příkaz.

| Příkaz | Poznámky |
|---|---|
| [Get-AzureRmVmss](/powershell/module/azurerm.compute/get-azurermvmss) | Získá vlastnosti typu uzlu clusteru (škálovací sadu virtuálních počítačů).   |
| [Přidat AzureRmVmssExtension](/powershell/module/azurerm.compute/add-azurermvmssextension)| Přidá rozšíření do sady škálování virtuálního počítače.|
| [Aktualizace AzureRmVmss](/powershell/module/azurerm.compute/update-azurermvmss)|Aktualizuje stav škálování virtuálních počítačů, nastavit stav místního VMSS objektu.|

## <a name="next-steps"></a>Další kroky

Další informace o modulu Azure PowerShell najdete v tématu [dokumentace Azure PowerShell](/powershell/azure/overview).

Další ukázky pro Azure Service Fabric Azure Powershell lze nalézt v [prostředí Azure PowerShell ukázky](../service-fabric-powershell-samples.md).
