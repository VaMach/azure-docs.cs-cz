---
title: "Vytvoření vlastní image Azure DevTest Labs ze souboru virtuálního pevného disku | Microsoft Docs"
description: "Naučte se vytvořit vlastní image v Azure DevTest Labs ze souboru virtuálního pevného disku pomocí portálu Azure"
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: b795bc61-7c28-40e6-82fc-96d629ee0568
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/10/2017
ms.author: tarcher
ms.openlocfilehash: a421327ab8794315005327833b873dc5ebd57e9e
ms.sourcegitcommit: cfd1ea99922329b3d5fab26b71ca2882df33f6c2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/30/2017
---
# <a name="create-a-custom-image-from-a-vhd-file"></a>Vytvořit vlastní image ze souboru virtuálního pevného disku

[!INCLUDE [devtest-lab-create-custom-image-from-vhd-selector](../../includes/devtest-lab-create-custom-image-from-vhd-selector.md)]

[!INCLUDE [devtest-lab-custom-image-definition](../../includes/devtest-lab-custom-image-definition.md)]

[!INCLUDE [devtest-lab-upload-vhd-options](../../includes/devtest-lab-upload-vhd-options.md)]

## <a name="step-by-step-instructions"></a>Podrobné pokyny

Následující postup vás provede procesem vytvoření vlastní image ze souboru virtuálního pevného disku pomocí portálu Azure:

1. Přihlaste se k webu [Azure Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).

1. Vyberte **Další služby** a poté ze seznamu vyberte **DevTest Labs**.

1. Ze seznamu labs vyberte požadované testovací prostředí.  

1. V okně v prostředí, vyberte **konfigurace**. 

1. V testovacím prostředí **konfigurace** vyberte **vlastní Image (VHD)**.

1. Na **vlastní image** vyberte **+ přidat**.

    ![Přidat vlastní obrázek](./media/devtest-lab-create-template/add-custom-image.png)

1. Zadejte název pro vlastní image. Tento název se zobrazí v seznamu základní Image při vytváření virtuálního počítače.

1. Zadejte popis vlastní image. Tento popis se zobrazí v seznamu základní Image při vytváření virtuálního počítače.

1. Vyberte **virtuálního pevného disku**.

1. Z **virtuálního pevného disku** okně vyberte požadovaný soubor VHD.

1. Vyberte **OK** zavřete **virtuálního pevného disku** okno.

1. Vyberte **konfigurace operačního systému**.

1. Na **konfigurace operačního systému** , vyberte buď **Windows** nebo **Linux**.

1. Pokud **Windows** je vybrána, zadejte prostřednictvím políčka zda *Sysprep* byl spuštěn v počítači. 

1. Vyberte **OK** zavřete **konfigurace operačního systému** okno.

1. Vyberte **OK** vytvořit vlastní image.

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="related-blog-posts"></a>Příspěvky blogu související

- [Vlastní Image nebo vzorce?](https://blogs.msdn.microsoft.com/devtestlab/2016/04/06/custom-images-or-formulas/)
- [Kopírování vlastních bitových kopií mezi Azure DevTest Labs](http://www.visualstudiogeeks.com/blog/DevOps/How-To-Move-CustomImages-VHD-Between-AzureDevTestLabs#copying-custom-images-between-azure-devtest-labs)

##<a name="next-steps"></a>Další kroky

- [Přidejte virtuální počítač do testovacího prostředí](./devtest-lab-add-vm.md)
