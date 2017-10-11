---
title: "Přidat vymahatelných virtuální počítač do testovacího prostředí v Azure DevTest Labs | Microsoft Docs"
description: "Informace o postupu přidání vymahatelných virtuálního počítače do testovacího prostředí v Azure DevTest Labs"
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: f671e66e-9630-4e30-a131-a6bad9ed9c11
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/17/2017
ms.author: tarcher
ms.openlocfilehash: 98950d72e90b0e178bae2fffa7644fd824a25eea
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="add-a-claimable-vm-to-a-lab-in-azure-devtest-labs"></a>Přidat vymahatelných virtuální počítač do testovacího prostředí v Azure DevTest Labs
Přidat vymahatelných virtuálního počítače do testovacího prostředí podobným způsobem, jak můžete [přidat standardní virtuální počítač](devtest-lab-add-vm.md) – z *základní* to znamená buď [vlastní image](devtest-lab-create-template.md), [vzorec](devtest-lab-manage-formulas.md), nebo [Marketplace image](devtest-lab-configure-marketplace-images.md). Tento kurz vás provede procesem přidání vymahatelných virtuálního počítače do testovacího prostředí v DevTest Labs pomocí portálu Azure a zobrazuje proces, že uživatel přejde do deklarací identity virtuálního počítače.

> [!NOTE]
> Pokud nasadíte virtuální počítače testovacího prostředí prostřednictvím [šablon Azure Resource Manageru](devtest-lab-create-environment-from-arm.md), můžete vytvořit virtuální počítače vymahatelných nastavením **allowClaim** vlastnost na hodnotu true v části Vlastnosti.
>
>

## <a name="steps-to-add-a-claimable-vm-to-a-lab-in-azure-devtest-labs"></a>Postup pro přidání do testovacího prostředí v Azure DevTest Labs vymahatelných virtuálního počítače
1. Přihlaste se k webu [Azure Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).
1. Vyberte **více služeb**a potom vyberte **DevTest Labs** ze seznamu.
1. Ze seznamu labs vyberte testovací prostředí, ve kterém chcete vytvořit vymahatelných virtuálního počítače.  
1. V tomto prostředí **přehled** vyberte **+ přidat**.  

    ![Virtuální počítač tlačítko Přidat](./media/devtest-lab-add-vm/devtestlab-home-blade-add-vm.png)

1. Na **zvolte na základní** okně vyberte základ pro virtuální počítač.
1. Na **virtuálního počítače** okno, zadejte název pro nový virtuální počítač **název virtuálního počítače** textového pole.

    ![Okno prostředí virtuálních počítačů](./media/devtest-lab-add-vm/devtestlab-lab-vm-blade.png)

1. Zadejte **uživatelské jméno** , jsou udělena oprávnění správce na virtuálním počítači.  
1. Pokud chcete použít heslo uložené v vaše [tajný úložiště](https://azure.microsoft.com/updates/azure-devtest-labs-keep-your-secrets-safe-and-easy-to-use-with-the-new-personal-secret-store), vyberte **použít uložené tajný klíč**a zadejte hodnotu klíče, která odpovídá váš tajný klíč (heslo). Jinak, zadejte heslo do textového pole s názvem bez přípony **zadejte hodnotu**.
1. **Typ disku virtuálního počítače** určuje disku typů úložiště, které u virtuálních počítačů v testovacím prostředí.
1. Vyberte **velikost virtuálního počítače** a vyberte jednu z předdefinovaných položky, které zadejte jader procesoru, velikosti paměti RAM a velikost pevného disku pro vytvoření virtuálního počítače.
1. Vyberte **artefakty** a ze seznamu artefaktů, vyberte a nakonfigurujte artefaktů, které chcete přidat do základní bitové kopie. Pokud nepracovali DevTest Labs nebo konfigurace artefaktů, vyhledejte [přidat existující artefaktů pro virtuální počítač](devtest-lab-add-vm.md#add-an-existing-artifact-to-a-vm) části a vraťte se sem po dokončení.
1. Vyberte **upřesňující nastavení** nakonfigurovat možnosti sítě a vypršení platnosti možnosti Virtuálního počítače. V části **deklarace identity možnosti**, zvolte **Ano** k zajištění vymahatelných počítače.

  ![Zvolte, aby vymahatelných virtuálního počítače.](./media/devtest-lab-add-vm/devtestlab-claim-VM-option.png)

1. Pokud chcete zobrazit nebo zkopírování šablony Azure Resource Manager, přečtěte si [šablony uložit Azure Resource Manageru](devtest-lab-add-vm.md#save-azure-resource-manager-template) části a sem vraťte po dokončení.
1. Vyberte **vytvořit** přidat zadaný virtuální počítač do testovacího prostředí.
1. V okně prostředí zobrazí stav vytváření Virtuálního počítače – nejprve jako **vytváření**, pak jako **systémem** po spuštění virtuálního počítače.


## <a name="using-a-claimable-vm"></a>Pomocí vymahatelných virtuálního počítače

Uživatel může deklarace identity žádné virtuální počítače ze seznamu "Vymahatelných virtuální počítače" jedním z těchto kroků:

* Ze seznamu "Vymahatelných virtuální počítače" v dolní části okna Přehled v prostředí, klikněte pravým tlačítkem na jednu z virtuálních počítačů v seznamu a vyberte **deklarace identity počítače**.

 ![Požádat o konkrétní vymahatelných virtuální počítač.](./media/devtest-lab-add-vm/devtestlab-claim-VM.png)


* V horní části **přehled** okně zvolte **všechny deklarace**. Náhodné virtuální počítač přidělenou ze seznamu vymahatelných virtuálních počítačů.

 ![Požádat o žádné vymahatelných virtuální počítače.](./media/devtest-lab-add-vm/devtestlab-claim-any.png)


Poté, co uživatel deklarací virtuálního počítače, je do jejich seznam "Moje virtuální počítače" Přesunout nahoru a již není vymahatelných jiným uživatelem.

## <a name="next-steps"></a>Další kroky
* Po vytvoření virtuálního počítače, můžete připojit k virtuálnímu počítači tak, že vyberete **Connect** v okně Virtuálního počítače.
* Prozkoumejte [Galerie šablon DevTest Labs Azure Resource Manager rychlý start](https://github.com/Azure/azure-devtestlab/tree/master/ARMTemplates)
