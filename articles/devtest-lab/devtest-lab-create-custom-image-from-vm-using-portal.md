---
title: "Z virtuálního počítače vytvořit vlastní image Azure DevTest Labs | Microsoft Docs"
description: "Naučte se vytvořit vlastní image v Azure DevTest Labs ze zřízeného virtuálního počítače pomocí portálu Azure"
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: 
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/10/2017
ms.author: tarcher
ms.openlocfilehash: 9d2dcf7164985508d691e8a0c123efaf3b8aa19a
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-custom-image-from-a-vm"></a>Vytvořit vlastní image z virtuálního počítače

[!INCLUDE [devtest-lab-custom-image-definition](../../includes/devtest-lab-custom-image-definition.md)]

## <a name="step-by-step-instructions"></a>Podrobné pokyny

Můžete vytvořit vlastní image ze zřízeného virtuálního počítače a pak použít tento vlastní bitovou kopii k Vytvořte identické virtuální počítače. Následující kroky ukazují, jak vytvořit vlastní image z virtuálního počítače:

1. Přihlaste se k webu [Azure Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).

1. Vyberte **Další služby** a poté ze seznamu vyberte **DevTest Labs**.

1. Ze seznamu labs vyberte požadované testovací prostředí.  

1. V okně v prostředí, vyberte **Můj virtuální počítače**.
 
1. Na **Můj virtuální počítače** okně vyberte virtuální počítač, ze kterého chcete vytvořit vlastní image.

1. V okně Virtuálního počítače, vyberte **vytvořit vlastní image (VHD)**.

    ![Vytvoření vlastní image položky nabídky](./media/devtest-lab-create-template/create-custom-image.png)

1. Na **vytvořit image** okno, zadejte název a popis pro vlastní bitovou kopii. Tyto informace se zobrazí v seznamu základů při vytváření virtuálního počítače.

    ![Vytvořit vlastní image okno](./media/devtest-lab-create-template/create-custom-image-blade.png)

1. Vyberte, zda byl spuštěn nástroj sysprep na virtuálním počítači. Pokud na virtuální počítač nebyl spuštěn nástroje sysprep, určete, jestli má nástroj sysprep, které jsou spuštěny při vytvoření virtuálního počítače z této vlastní image.

1. Vyberte **OK** po dokončení vytvoření vlastní image.

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="related-blog-posts"></a>Příspěvky blogu související

- [Vlastní Image nebo vzorce?](https://blogs.msdn.microsoft.com/devtestlab/2016/04/06/custom-images-or-formulas/)
- [Kopírování vlastních bitových kopií mezi Azure DevTest Labs](http://www.visualstudiogeeks.com/blog/DevOps/How-To-Move-CustomImages-VHD-Between-AzureDevTestLabs#copying-custom-images-between-azure-devtest-labs)

##<a name="next-steps"></a>Další kroky

- [Přidejte virtuální počítač do testovacího prostředí](./devtest-lab-add-vm-with-artifacts.md)
