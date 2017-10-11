---
title: "Řešení potíží se přes rozšíření virtuálního počítače s Windows | Microsoft Docs"
description: "Další informace o řešení potíží se přes rozšíření virtuálního počítače Windows Azure"
services: virtual-machines-windows
documentationcenter: 
author: kundanap
manager: timlt
editor: 
tags: top-support-issue,azure-resource-manager
ms.assetid: 878ab9b6-c3e6-40be-82d4-d77fecd5030f
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 03/29/2016
ms.author: kundanap
ms.openlocfilehash: 4dba196e1b838f2092b30972fb070514bd2a4b25
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="troubleshooting-azure-windows-vm-extension-failures"></a>Řešení potíží se přes rozšíření virtuálního počítače Windows Azure
[!INCLUDE [virtual-machines-common-extensions-troubleshoot](../../../includes/virtual-machines-common-extensions-troubleshoot.md)]

## <a name="viewing-extension-status"></a>Zobrazení stavu rozšíření
Šablony Azure Resource Manager lze spustit z prostředí Azure PowerShell. Jakmile se spustí šablony, lze zobrazit stav rozšíření z Průzkumníka prostředků Azure nebo nástroje příkazového řádku.

Zde naleznete příklad:

Azure PowerShell:

      Get-AzureRmVM -ResourceGroupName $RGName -Name $vmName -Status

Zde je ukázkový výstup:

      Extensions:  {
      "ExtensionType": "Microsoft.Compute.CustomScriptExtension",
      "Name": "myCustomScriptExtension",
      "SubStatuses": [
        {
          "Code": "ComponentStatus/StdOut/succeeded",
          "DisplayStatus": "Provisioning succeeded",
          "Level": "Info",
          "Message": "    Directory: C:\\temp\\n\\n\\nMode                LastWriteTime     Length Name
              \\n----                -------------     ------ ----                              \\n-a---          9/1/2015   2:03 AM         11
              test.txt                          \\n\\n",
                      "Time": null
          },
        {
          "Code": "ComponentStatus/StdErr/succeeded",
          "DisplayStatus": "Provisioning succeeded",
          "Level": "Info",
          "Message": "",
          "Time": null
        }
    }
  ]

## <a name="troubleshooting-extension-failures"></a>Řešení potíží s chyby rozšíření
### <a name="re-running-the-extension-on-the-vm"></a>Opětovné spuštění rozšíření ve virtuálním počítači
Pokud se spouštění skriptů na virtuálním počítači pomocí rozšíření vlastních skriptů, může někdy spustit došlo k chybě, kde byl virtuální počítač úspěšně vytvořen, ale došlo k selhání skriptu. Doporučený způsob obnovení z této chyby je v rámci těchto podmínek, odeberte rozšíření a znovu spustit šablonu.
Poznámka: V budoucnosti, tato funkce by vylepšit odebrat potřeba odinstalovat rozšíření.

#### <a name="remove-the-extension-from-azure-powershell"></a>Odeberte rozšíření z prostředí Azure PowerShell
    Remove-AzureRmVMExtension -ResourceGroupName $RGName -VMName $vmName -Name "myCustomScriptExtension"

Po odebrání rozšíření šablony lze spouštět skripty ve virtuálním počítači znovu spustit.

