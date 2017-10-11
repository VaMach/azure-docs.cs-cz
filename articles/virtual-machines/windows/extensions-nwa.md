---
title: "Rozšíření virtuálního počítače Azure sítě sledovacích procesů agenta pro Windows | Microsoft Docs"
description: "Nasaďte agenta sledovací proces sítě v systému Windows virtuálního počítače pomocí rozšíření virtuálního počítače."
services: virtual-machines-windows
documentationcenter: 
author: dennisg
manager: amku
editor: 
tags: azure-resource-manager
ms.assetid: 27e46af7-2150-45e8-b084-ba33de8c5e3f
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 02/14/2017
ms.author: dennisg
ms.openlocfilehash: b8d6a998bc86337b286a3434f44f762cca9b7e68
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="network-watcher-agent-virtual-machine-extension-for-windows"></a>Sítě rozšíření virtuálního počítače sledovacích procesů agenta pro Windows

## <a name="overview"></a>Přehled

[Azure sledovací proces sítě](https://review.docs.microsoft.com/en-us/azure/network-watcher/) je sítě výkonu monitorování, diagnostiku a analýzy služba, která umožňuje monitorování pro sítě Azure. Rozšíření sítě sledovacích procesů agenta virtuálního počítače není pro některé funkce sledovací proces sítě na virtuálních počítačích Azure. To zahrnuje Zachytávání síťových přenosů na vyžádání a další pokročilé funkce.

Tento dokument podrobně popisuje podporované platformy a možnosti nasazení pro rozšíření sítě sledovacích procesů agenta virtuálního počítače pro systém Windows.

## <a name="prerequisites"></a>Požadavky

### <a name="operating-system"></a>Operační systém

Rozšíření sítě sledovacích procesů agenta pro Windows můžete spustit na Windows Server 2008 R2, 2012, 2012 R2 a 2016 uvolní. Všimněte si, že není v současné době podporovaný Nano Server.

### <a name="internet-connectivity"></a>Připojení k internetu

Některé funkce sítě sledovacích procesů agenta vyžaduje, aby cílový virtuální počítač připojen k Internetu. Bez možnosti navázat odchozí připojení některé funkce sítě sledovacích procesů agenta nebude fungovat správně nebo nedostupná. Další podrobnosti najdete v tématu [sledovací proces sítě dokumentaci](../../network-watcher/network-watcher-monitoring-overview.md).

## <a name="extension-schema"></a>Rozšíření schématu

Následujícím kódu JSON znázorňuje schéma pro rozšíření sítě sledovacích procesů agenta. Rozšíření ani jeden z nich vyžaduje ani podporuje nastavení uživatelem zadané v tuto chvíli a spoléhá na jeho výchozí konfigurace.

```json
{
    "type": "extensions",
    "name": "Microsoft.Azure.NetworkWatcher",
    "apiVersion": "[variables('apiVersion')]",
    "location": "[resourceGroup().location]",
    "dependsOn": [
        "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
    ],
    "properties": {
        "publisher": "Microsoft.Azure.NetworkWatcher",
        "type": "NetworkWatcherAgentWindows",
        "typeHandlerVersion": "1.4",
        "autoUpgradeMinorVersion": true
    }
}
```

### <a name="property-values"></a>Hodnoty vlastností

| Name (Název) | Hodnota nebo příklad |
| ---- | ---- |
| apiVersion | 2015-06-15 |
| Vydavatele | Microsoft.Azure.NetworkWatcher |
| type | NetworkWatcherAgentWindows |
| typeHandlerVersion | 1.4 |


## <a name="template-deployment"></a>Nasazení šablon

Rozšíření virtuálního počítače Azure se dá nasadit pomocí šablon Azure Resource Manager. Schéma JSON, které jsou popsané v předchozí části lze použít v šablonu Azure Resource Manager ke spuštění rozšíření sítě sledovacích procesů agenta při nasazení šablony Azure Resource Manager.

## <a name="powershell-deployment"></a>Nasazení prostředí PowerShell

`Set-AzureRmVMExtension` Příkaz lze použít k nasazení rozšíření sítě sledovacích procesů agenta virtuálního počítače do existujícího virtuálního počítače.

```powershell
Set-AzureRmVMExtension -ResourceGroupName "myResourceGroup1" `
                       -Location "WestUS" `
                       -VMName "myVM1" `
                       -Name "networkWatcherAgent" `
                       -Publisher "Microsoft.Azure.NetworkWatcher" `
                       -Type "NetworkWatcherAgentWindows" `
                       -TypeHandlerVersion "1.4"
```

## <a name="troubleshooting-and-support"></a>Řešení potíží a podpora

### <a name="troubleshooting"></a>Řešení potíží

Data o stavu nasazení rozšíření mohou být načteny z portálu Azure a pomocí modulu Azure PowerShell. Pokud chcete zobrazit stav nasazení rozšíření pro daný virtuální počítač, spusťte následující příkaz modulu Azure PowerShell.

```powershell
Get-AzureRmVMExtension -ResourceGroupName myResourceGroup1 -VMName myVM1 -Name networkWatcherAgent
```

Výstupu spuštění rozšíření se zaznamenává soubory, které jsou v následujícím adresáři:

```cmd
C:\WindowsAzure\Logs\Plugins\Microsoft.Azure.NetworkWatcher.NetworkWatcherAgentWindows\
```

### <a name="support"></a>Podpora

Pokud potřebujete další pomoc v libovolném bodě v tomto článku, můžete v uživatelské příručce sledovací proces sítě dokumentaci nebo se obraťte na Azure odborníky [fórech MSDN Azure a Stack Overflow](https://azure.microsoft.com/en-us/support/forums/). Alternativně můžete soubor incidentu podpory Azure. Přejděte na [podporu Azure lokality](https://azure.microsoft.com/en-us/support/options/) a vyberte Get podpory. Informace o používání Azure podporovat, najdete v tématu [podporu Microsoft Azure – nejčastější dotazy](https://azure.microsoft.com/en-us/support/faq/).
