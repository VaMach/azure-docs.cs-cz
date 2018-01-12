---
title: "Vytvoření vlastní image Azure DevTest Labs ze souboru virtuálního pevného disku | Microsoft Docs"
description: "Naučte se vytvořit vlastní image v Azure DevTest Labs ze souboru virtuálního pevného disku pomocí portálu Azure"
services: devtest-lab,virtual-machines
documentationcenter: na
author: craigcaseyMSFT
manager: douge
editor: 
ms.assetid: b795bc61-7c28-40e6-82fc-96d629ee0568
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/10/2018
ms.author: v-craic
ms.openlocfilehash: d1f1b9948fb591484c107818a01e141932effbba
ms.sourcegitcommit: 562a537ed9b96c9116c504738414e5d8c0fd53b1
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/12/2018
---
# <a name="create-a-custom-image-from-a-vhd-file"></a>Vytvořit vlastní image ze souboru virtuálního pevného disku

[!INCLUDE [devtest-lab-create-custom-image-from-vhd-selector](../../includes/devtest-lab-create-custom-image-from-vhd-selector.md)]

[!INCLUDE [devtest-lab-custom-image-definition](../../includes/devtest-lab-custom-image-definition.md)]

[!INCLUDE [devtest-lab-upload-vhd-options](../../includes/devtest-lab-upload-vhd-options.md)]

## <a name="step-by-step-instructions"></a>Podrobné pokyny

Následující postup vás provede procesem vytvoření vlastní image ze souboru virtuálního pevného disku pomocí portálu Azure:

1. Přihlaste se k webu [Azure Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).

1. Vyberte **všechny služby**a potom vyberte **DevTest Labs** ze seznamu.

1. Ze seznamu labs vyberte požadované testovací prostředí.  

1. V hlavním podokně v prostředí, vyberte **konfiguraci a zásady**. 

1. Na **konfiguraci a zásady** podokně, vyberte **vlastní image**.

1. Na **vlastní image** podokně, vyberte **+ přidat**.

    ![Přidat vlastní obrázek](./media/devtest-lab-create-template/add-custom-image.png)

1. Zadejte název pro vlastní image. Tento název se zobrazí v seznamu základní Image při vytváření virtuálního počítače.

1. Zadejte popis vlastní image. Tento popis se zobrazí v seznamu základní Image při vytváření virtuálního počítače.

1. Pro **typ operačního systému**, vyberte buď **Windows** nebo **Linux**.

    - Pokud vyberete **Windows**, zadejte prostřednictvím políčka zda *sysprep* byl spuštěn v počítači. 
    - Pokud vyberete **Linux**, zadejte prostřednictvím políčka zda *deprovision* byl spuštěn v počítači. 

1. Vyberte **virtuálního pevného disku** z rozevírací nabídky. Toto je virtuální pevný disk, který se použije k vytvoření nové vlastní image. V případě potřeby vyberte k **nahrát virtuální pevný disk pomocí prostředí PowerShell**.

1. Pokud společnost Microsoft není publikována image použitá k vytvoření vlastní image, můžete zadat také název plánu, nabídka plán a plán vydavatele.

1. Vyberte **OK** vytvořit vlastní image.

Po několika minutách vlastní image se vytvoří a je uložený v účtu úložiště v prostředí. Pokud chce uživatel laboratoře k vytvoření nového virtuálního počítače, je k dispozici v seznamu obrázků základní bitovou kopii.

![Vlastní image, které jsou k dispozici v seznamu základní bitové kopie](./media/devtest-lab-create-template/custom-image-available-as-base.png)


[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="related-blog-posts"></a>Příspěvky blogu související

- [Vlastní Image nebo vzorce?](https://blogs.msdn.microsoft.com/devtestlab/2016/04/06/custom-images-or-formulas/)
- [Kopírování vlastních bitových kopií mezi Azure DevTest Labs](http://www.visualstudiogeeks.com/blog/DevOps/How-To-Move-CustomImages-VHD-Between-AzureDevTestLabs#copying-custom-images-between-azure-devtest-labs)

## <a name="next-steps"></a>Další postup

- [Přidejte virtuální počítač do testovacího prostředí](./devtest-lab-add-vm.md)
