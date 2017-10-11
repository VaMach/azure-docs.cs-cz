---
title: "Stáhnout šablonu pro virtuální počítač Azure | Microsoft Docs"
description: "Stáhněte si templatefor virtuálního počítače můžete k automatizaci nasazení v modelu nasazení Resource Manager"
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 51ef4f51-0942-4249-afea-4a3f87ce1ff8
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 03/22/2017
ms.author: cynthn
ms.openlocfilehash: 9e4c0c3cf0e233447369a24b1d5fe27495abd1cf
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="download-the-template-for-a-vm"></a>Stažení šablony pro virtuální počítač
Při vytváření virtuálního počítače v Azure pomocí portálu nebo prostředí PowerShell, je pro vás automaticky vytvoří šablonu Resource Manager. Tuto šablonu můžete použít k rychlé duplicitní nasazení. Šablona obsahuje informace o všechny prostředky ve skupině prostředků. Pro virtuální počítač to znamená, že šablona obsahuje všechno, která je vytvořena na podporu virtuálních počítačů v příslušné skupině prostředků, včetně síťových prostředků.

## <a name="download-the-template-using-the-portal"></a>Stažení šablony pomocí portálu
1. Přihlaste se k portálu [Azure Portal](https://portal.azure.com/).
2. Jeden nabídce centra vyberte **virtuální počítače**.
3. Ze seznamu vyberte virtuální počítač.
4. Vyberte **skriptu pro automatizaci**.
5. Vyberte **Stáhnout** a uložte soubor .zip do místního počítače.
6. Otevřete soubor .zip a rozbalte soubory do složky. Bude obsahovat soubor .zip:
   
   * Deploy.ps1
   * Deploy.SH 
   * deployer.RB
   * DeploymentHelper.cs
   * Parameters.JSON tímto kódem
   * Template.JSON

Soubor template.json je šablona.

## <a name="download-the-template-using-powershell"></a>Stažení šablony pomocí prostředí PowerShell
Můžete také stáhnout pomocí soubor .json [Export-AzureRMResourceGroup](https://msdn.microsoft.com/library/mt715427.aspx) rutiny. Můžete použít `-path` parametru zadat název a cesta k souboru .json. Tento příklad ukazuje, jak stáhnout šablonu pro skupinu prostředků s názvem **myResourceGroup** k **C:\users\public\downloads** složky v místním počítači.

```powershell
    Export-AzureRmResourceGroup -ResourceGroupName "myResourceGroup" -Path "C:\users\public\downloads"
```

## <a name="next-steps"></a>Další kroky
Další informace o nasazení prostředků pomocí šablony najdete v tématu [názorný Průvodce šablonou Resource Manageru](../../azure-resource-manager/resource-manager-template-walkthrough.md).

