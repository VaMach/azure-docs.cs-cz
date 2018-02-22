---
title: "Nahrání souboru virtuálního pevného disku do Azure DevTest Labs pomocí prostředí PowerShell | Microsoft Docs"
description: "Nahrání souboru virtuálního pevného disku do testovacího prostředí na účet úložiště pomocí prostředí PowerShell"
services: devtest-lab,virtual-machines
documentationcenter: na
author: craigcaseyMSFT
manager: douge
editor: 
ms.assetid: 
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/10/2017
ms.author: v-craic
ms.openlocfilehash: c940f5646bc7845db7b774d7ada40b8105534d69
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/21/2018
---
# <a name="upload-vhd-file-to-labs-storage-account-using-powershell"></a>Nahrání souboru virtuálního pevného disku do testovacího prostředí na účet úložiště pomocí prostředí PowerShell

[!INCLUDE [devtest-lab-upload-vhd-selector](../../includes/devtest-lab-upload-vhd-selector.md)]

V Azure DevTest Labs soubory virtuálního pevného disku lze vytvořit vlastní Image, které se používají ke zřízení virtuálních počítačů. Následující kroky vás provedou pomocí prostředí PowerShell pro nahrání souboru virtuálního pevného disku k účtu úložiště testovacího prostředí. Jakmile jste odeslali souboru virtuálního pevného disku [další kroky části](#next-steps) jsou uvedeny některé články, které ukazují, jak vytvořit vlastní image z nahrávaný soubor virtuálního pevného disku. Další informace o disky a virtuální pevné disky v Azure najdete v tématu [o disky a virtuální pevné disky pro virtuální počítače](../virtual-machines/linux/about-disks-and-vhds.md)

## <a name="step-by-step-instructions"></a>Podrobné pokyny

Následující kroky vás provedou nahrání souboru virtuálního pevného disku do Azure DevTest Labs pomocí prostředí PowerShell. 

1. Přihlaste se k webu [Azure Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).

1. Vyberte **všechny služby**a potom vyberte **DevTest Labs** ze seznamu.

1. Ze seznamu labs vyberte požadované testovací prostředí.  

1. V okně v prostředí, vyberte **konfigurace**. 

1. V testovacím prostředí **konfigurace** vyberte **vlastní Image (VHD)**.

1. Na **vlastní image** okně vyberte **+ přidat**. 

1. Na **vlastní image** vyberte **virtuálního pevného disku**.

1. Na **virtuálního pevného disku** vyberte **nahrát virtuální pevný disk pomocí prostředí PowerShell**.

    ![Nahrání virtuálního pevného disku pomocí prostředí PowerShell](./media/devtest-lab-upload-vhd-using-powershell/upload-image-using-psh.png)

1. Na **Odeslat bitovou kopii pomocí prostředí PowerShell** okně zkopírovat do textového editoru generovaného skriptu prostředí PowerShell.

1. Změnit **LocalFilePath** parametr **přidat AzureVhd** rutiny tak, aby odkazovalo na umístění souboru virtuálního pevného disku, který chcete odeslat.

1. V řádku prostředí PowerShell, spusťte **přidat AzureVhd** rutiny (s upravenou **LocalFilePath** parametr).

> [!WARNING] 
> 
> Proces odesílání soubor virtuálního pevného disku může být náročná v závislosti na velikosti souboru virtuálního pevného disku a rychlost připojení.

## <a name="next-steps"></a>Další postup

- [Vytvoření vlastní image v Azure DevTest Labs ze souboru virtuálního pevného disku pomocí portálu Azure](devtest-lab-create-template.md)
- [Vytvoření vlastní image v Azure DevTest Labs ze souboru virtuálního pevného disku pomocí prostředí PowerShell](devtest-lab-create-custom-image-from-vhd-using-powershell.md)
