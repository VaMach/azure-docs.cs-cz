---
title: "O bitových kopií pro virtuální počítače s Windows | Microsoft Docs"
description: "Další informace o použití bitové kopie s virtuální počítače s Windows v Azure."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 66ff3fab-8e7f-4dff-b8da-ab1c9c9c9af8
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 03/20/2017
ms.author: cynthn
ms.openlocfilehash: d421cee0becabdf81d865036d0c98b12b077152b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="about-images-for-windows-virtual-machines"></a>O bitových kopií pro virtuální počítače s Windows
> [!IMPORTANT]
> Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Resource Manager a klasický](../../../resource-manager-deployment-model.md). Tento článek se zabývá pomocí modelu nasazení Classic. Microsoft doporučuje, aby byl ve většině nových nasazení použit model Resource Manager. Informace o hledání a používání imagí v modelu Resource Manager najdete v tématu [zde](../../virtual-machines-windows-cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

[!INCLUDE [virtual-machines-common-classic-about-images](../../../../includes/virtual-machines-common-classic-about-images.md)]

## <a name="working-with-images"></a>Práce s obrázky

Modul Azure PowerShell a portálu Azure můžete použít ke správě bitových kopií, která je k dispozici k předplatnému Azure. Modul Azure PowerShell poskytuje další parametry příkazu, takže přesně určit pouze požadované anebo najdete v článku. Portál Azure poskytuje grafickým uživatelským rozhraním pro řadu každodenních úloh pro správu.

Zde jsou některé příklady, které používají modulu Azure PowerShell.

* **Získat všechny image**:`Get-AzureVMImage`vrátí seznam hodnot všechny bitové kopie k dispozici v aktuálním předplatném: vaše Image a ty poskytovaný Azure nebo partnerů. V rozevíracím seznamu může být velký. Další příklady ukazují, jak získat seznam kratší.
* **Získat image rodiny**:`Get-AzureVMImage | select ImageFamily` získá seznam image rodiny zobrazením řetězce **ImageFamily** vlastnost.
* **Získat všechny bitové kopie v konkrétní rodině**:`Get-AzureVMImage | Where-Object {$_.ImageFamily -eq $family}`
* **Najít Image virtuálních počítačů**: `Get-AzureVMImage | where {(gm –InputObject $_ -Name DataDiskConfigurations) -ne $null} | Select -Property Label, ImageName` Tato rutina funguje tak, že filtrování DataDiskConfiguration vlastnost, která se vztahuje pouze na Image virtuálních počítačů. Tento příklad také vyfiltruje výstup jenom na název popisku a bitové kopie.
* **Uložte bitovou kopii zobecněný**:`Save-AzureVMImage –ServiceName "myServiceName" –Name "MyVMtoCapture" –OSState "Generalized" –ImageName "MyVmImage" –ImageLabel "This is my generalized image"`
* **Uložte bitovou kopii specializované**:`Save-AzureVMImage –ServiceName "mySvc2" –Name "MyVMToCapture2" –ImageName "myFirstVMImageSP" –OSState "Specialized" -Verbose`

  > [!TIP]
  > Parametr OSState je potřeba k vytvoření image virtuálního počítače, která obsahuje disk operačního systému a datové disky připojené. Pokud nepoužijete parametr, rutina vytvoří bitovou kopii operačního systému. Hodnota parametru udává, zda obrázek zobecněn nebo specializuje, na základě na tom, jestli disk operačního systému je připravena pro opakované použití.

* **Odstranit bitovou kopii**:`Remove-AzureVMImage –ImageName "MyOldVmImage"`

## <a name="next-steps"></a>Další kroky
Můžete také [vytvoření počítače s Windows pomocí portálu Azure](tutorial.md).
