---
title: "Vyberte bitové kopie virtuálního počítače s Windows v Azure | Microsoft Docs"
description: "Další informace o použití prostředí Azure PowerSHell k určení vydavatele, nabídky, SKU a verze pro Image Marketplace virtuálních počítačů."
services: virtual-machines-windows
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 188b8974-fabd-4cd3-b7dc-559cbb86b98a
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 07/12/2017
ms.author: danlep
ms.openlocfilehash: c9b35ff5f3fbd33639805b5a4f105df32562a691
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-find-windows-vm-images-in-the-azure-marketplace-with-azure-powershell"></a>Postup nalezení bitové kopie virtuálního počítače s Windows v Azure Marketplace s prostředím Azure PowerShell

Toto téma popisuje, jak pomocí prostředí Azure PowerShell k vyhledání Image virtuálních počítačů v Azure Marketplace. Tyto informace slouží k určení image pořízenou prostřednictvím Marketplace při vytvoření virtuálního počítače s Windows.

Ujistěte se, zda je nainstalován a nakonfigurován nejnovější [modul Azure PowerShell](/powershell/azure/install-azurerm-ps).



## <a name="table-of-commonly-used-windows-images"></a>Tabulka běžně používané bitových kopií systému Windows
| Název vydavatele | Nabídka | Skladová jednotka (SKU) |
|:--- |:--- |:--- |:--- |
| MicrosoftWindowsServer |WindowsServer |2016 Datacenter |
| MicrosoftWindowsServer |WindowsServer |2016 Datacenter Server Core |
| MicrosoftWindowsServer |WindowsServer |2016 datového centra s kontejnery |
| MicrosoftWindowsServer |WindowsServer |2016. Nano Server |
| MicrosoftWindowsServer |WindowsServer |2012-R2-Datacenter |
| MicrosoftWindowsServer |WindowsServer |AKTUALIZACE SP1 2008 R2 |
| MicrosoftDynamicsNAV |DynamicsNAV |2017 |
| MicrosoftSharePoint |MicrosoftSharePointServer |2016 |
| MicrosoftSQLServer |SQL2016 WS2016 |Enterprise |
| MicrosoftSQLServer |SQL2014SP2 WS2012R2 |Enterprise |
| MicrosoftWindowsServerHPCPack |WindowsServerHPCPack |2012R2 |
| MicrosoftWindowsServerEssentials |WindowsServerEssentials |WindowsServerEssentials |

## <a name="find-specific-images"></a>Vyhledání konkrétních imagí


Při vytváření nového virtuálního počítače pomocí Azure Resource Manageru je v některých případech nutné zadat image kombinací následujících vlastností image:

* Vydavatel
* Nabídka
* Skladová jednotka (SKU)

Například použít tyto hodnoty [Set-AzureRMVMSourceImage](/powershell/module/azurerm.compute/set-azurermvmsourceimage) rutiny prostředí PowerShell nebo pomocí šablony skupiny prostředků, ve kterém musíte zadat typ virtuálního počítače, který se má vytvořit.

Pokud potřebujete zjistit tyto hodnoty, můžete spustit [Get-AzureRMVMImagePublisher](/powershell/module/azurerm.compute/get-azurermvmimagepublisher), [Get-AzureRMVMImageOffer](/powershell/module/azurerm.compute/get-azurermvmimageoffer), a [Get-AzureRMVMImageSku](/powershell/module/azurerm.compute/get-azurermvmimagesku) rutiny přejděte bitové kopie. Můžete určit tyto hodnoty:

1. Vypsat vydavatele imagí.
2. Pro daného vydavatele vypsat jeho nabídky.
3. Pro danou nabídku vypsat její skladovou jednotku (SKU).

Nejprve vypište vydavatele pomocí následujících příkazů:

```azurepowershell-interactive
$locName="<Azure location, such as West US>"
Get-AzureRMVMImagePublisher -Location $locName | Select PublisherName
```

Vyplňte název zvoleného vydavatele a spusťte následující příkazy:

```azurepowershell-interactive
$pubName="<publisher>"
Get-AzureRMVMImageOffer -Location $locName -Publisher $pubName | Select Offer
```

Vyplňte název zvolené nabídky a spusťte následující příkazy:

```azurepowershell-interactive
$offerName="<offer>"
Get-AzureRMVMImageSku -Location $locName -Publisher $pubName -Offer $offerName | Select Skus
```

Z výstupu `Get-AzureRMVMImageSku` příkaz, abyste měli všechny informace, musíte zadat bitovou kopii pro nový virtuální počítač.

Následuje úplný příklad:

```azurepowershell-interactive
$locName="West US"
Get-AzureRMVMImagePublisher -Location $locName | Select PublisherName

```

Výstup:

```
PublisherName
-------------
a10networks
aiscaler-cache-control-ddos-and-url-rewriting-
alertlogic
AlertLogic.Extension
Barracuda.Azure.ConnectivityAgent
barracudanetworks
basho
boxless
bssw
Canonical
...
```

Pro vydavatele „MicrosoftWindowsServer“:

```azurepowershell-interactive
$pubName="MicrosoftWindowsServer"
Get-AzureRMVMImageOffer -Location $locName -Publisher $pubName | Select Offer
```

Výstup:

```
Offer
-----
Windows-HUB
WindowsServer
WindowsServer-HUB
```

Pro nabídku „WindowsServer“:

```azurepowershell-interactive
$offerName="WindowsServer"
Get-AzureRMVMImageSku -Location $locName -Publisher $pubName -Offer $offerName | Select Skus
```

Výstup:

```
Skus
----
2008-R2-SP1
2008-R2-SP1-smalldisk
2012-Datacenter
2012-Datacenter-smalldisk
2012-R2-Datacenter
2012-R2-Datacenter-smalldisk
2016-Datacenter
2016-Datacenter-Server-Core
2016-Datacenter-Server-Core-smalldisk
2016-Datacenter-smalldisk
2016-Datacenter-with-Containers
2016-Nano-Server
```

Z tohoto seznamu si zkopírujte název zvolené skladové jednotky (SKU) a máte veškeré informace pro rutinu PowerShellu `Set-AzureRMVMSourceImage` nebo pro šablonu skupiny prostředků.

## <a name="next-steps"></a>Další kroky
Nyní si můžete vybrat přesně tu image, kterou chcete použít. Pokud chcete rychle vytvořit virtuální počítač pomocí informace o obrázku, který jste právě najít, přečtěte si téma [vytvoření virtuálního počítače s Windows pomocí prostředí PowerShell](quick-create-powershell.md).
