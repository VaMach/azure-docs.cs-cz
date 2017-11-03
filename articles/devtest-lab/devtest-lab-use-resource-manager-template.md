---
title: "Zobrazení a použití šablony virtuálního počítače Azure Resource Manageru | Microsoft Docs"
description: "Zjistěte, jak vytvořit ostatních virtuálních počítačů pomocí šablony Azure Resource Manageru z virtuálního počítače"
services: devtest-lab,virtual-machines,visual-studio-online
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: a759d9ce-655c-4ac6-8834-cb29dd7d30dd
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: tarcher
ms.openlocfilehash: 0807ab367b91be5acd261f2b58ca2112b2c9e380
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="use-a-virtual-machines-azure-resource-manager-template"></a>Použít šablonu virtuálního počítače Azure Resource Manager

Když vytváříte virtuální počítač (VM) v DevTest Labs prostřednictvím [portál Azure](http://go.microsoft.com/fwlink/p/?LinkID=525040), můžete zobrazit šablonu Azure Resource Manager před uložením virtuálního počítače. Šablony pak slouží jako základ pro vytvoření testovacího prostředí více virtuálních počítačů se stejným nastavením.

Tento článek popisuje postup zobrazení šablony Resource Manageru při vytvoření virtuálního počítače a jak se dá nasadit později k automatizovanému vytvoření virtuálního počítače stejný.

## <a name="multi-vm-vs-single-vm-resource-manager-templates"></a>Více virtuálních počítačů vs. šablony Resource Manageru single-VM
Existují dva způsoby, jak vytvořit virtuální počítače v DevTest Labs pomocí šablony Resource Manageru: zřizování prostředků Microsoft.DevTestLab/labs/virtualmachines nebo zřídit Microsoft.Commpute/virtualmachines prostředků. Každý se používá v různých scénářích a vyžadují různé oprávnění.

- Šablony Resource Manageru, které používají typ prostředku Microsoft.DevTestLab/labs/virtualmachines (podle údajů v vlastnost "prostředku" v šabloně) můžou zřizovat virtuální počítače jednotlivých testovacího prostředí. Každý virtuální počítač se pak objeví jako jedna položka v seznamu DevTest Labs virtuální počítače:

   ![Seznam virtuálních počítačů jako jednotlivé položky v seznamu DevTest Labs virtuální počítače](./media/devtest-lab-use-arm-template/devtestlab-lab-vm-single-item.png)

   Tento typ šablony Resource Manageru můžete zřídit pomocí příkazu prostředí Azure PowerShell **New-AzureRmResourceGroupDeployment** nebo pomocí příkazu příkazového řádku Azure CLI **vytvořit nasazení skupiny az**. Vyžaduje oprávnění správce, aby uživatelé, kteří jsou přiřazeny k roli uživatele DevTest Labs nelze provést nasazení. 

- Šablony Resource Manageru, které používají typ prostředku Microsoft.Compute/virtualmachines můžete zřídit několik virtuálních počítačů jako jeden prostředí v DevTest Labs seznamu virtuálních počítačů:

   ![Seznam virtuálních počítačů jako jednotlivé položky v seznamu DevTest Labs virtuální počítače](./media/devtest-lab-use-arm-template/devtestlab-lab-vm-single-environment.png)

   Virtuální počítače ve stejném prostředí lze spravovat společně a sdílet stejný životní cyklus. Uživatelé, kteří jsou přiřazeny k roli uživatele DevTest Labs můžete vytvořit prostředí pomocí těchto šablon, dokud správce nakonfiguroval testovací prostředí tímto způsobem.

Zbývající část tohoto článku popisuje šablony Resource Manageru, které používají Mirosoft.DevTestLab/labs/virtualmachines. Ty se používají správci testovacího prostředí automatizovat vytvoření testovacího prostředí virtuálních počítačů (například vymahatelných virtuální počítače) nebo zlaté image generace (například obrázek factory).

[Osvědčené postupy pro vytváření šablon Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-template-best-practices) nabízí mnoho pokyny a doporučení k vytváření šablon Azure Resource Manageru, která jsou spolehlivé a snadno se používá.

## <a name="view-and-save-a-virtual-machines-resource-manager-template"></a>Zobrazení a uložení šablony Resource Manageru virtuálního počítače
1. Postupujte podle kroků v [vytvoření vaší první virtuální počítač v testovacím prostředí](devtest-lab-create-first-vm.md) zahajte proces vytváření virtuálního počítače.
1. Zadejte požadované informace pro virtuální počítač a přidejte všechny artefakty, které chcete pro tento virtuální počítač.
1. V dolní části okna nastavení konfigurace, zvolte **šablony ARM zobrazení**.

   ![Tlačítko šablony ARM zobrazení](./media/devtest-lab-use-arm-template/devtestlab-lab-view-rm-template.png)
1. Zkopírujte a uložte šablony Resource Manageru pro pozdější použití vytvořit jiný virtuální počítač.

   ![Šablona Resource Manageru uložit pro pozdější použití](./media/devtest-lab-use-arm-template/devtestlab-lab-copy-rm-template.png)

Po uložení šablony Resource Manageru, musíte aktualizovat sekci parametrů šablony, než budete moci použít. Můžete vytvořit parameter.json, který upravuje jenom parametry, mimo skutečné šablony Resource Manageru. 

![Přizpůsobení paramaters pomocí souboru JSON](./media/devtest-lab-use-arm-template/devtestlab-lab-custom-params.png)

## <a name="deploy-a-resource-manager-template-to-create-a-vm"></a>Nasazení šablony Resource Manageru k vytvoření virtuálního počítače
Po uložení šablony Resource Manageru a přizpůsobené vašim potřebám, můžete k automatizovanému vytvoření virtuálního počítače. [Nasazení prostředků pomocí šablony Resource Manageru a prostředí Azure PowerShell](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-deploy) popisuje, jak pomocí prostředí Azure PowerShell s Resource Manager šablony nasazení vašich prostředků Azure. [Nasazení prostředků pomocí šablony Resource Manageru a rozhraní příkazového řádku Azure](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-deploy-cli) popisuje, jak nasazení vašich prostředků Azure pomocí rozhraní příkazového řádku Azure pomocí šablony Resource Manageru.

> [!NOTE]
> Pouze uživatel s oprávněními vlastníka testovacího prostředí můžete vytvořit virtuální počítače ze šablony Resource Manageru pomocí prostředí Azure PowerShell. Pokud chcete automatizovat vytvoření virtuálního počítače pomocí šablony Resource Manageru a máte oprávnění uživatele, můžete použít [ **az testovacího prostředí virtuálního počítače vytvořit** příkaz v rozhraní příkazového řádku](https://docs.microsoft.com/cli/azure/lab/vm#az_lab_vm_create).

### <a name="next-steps"></a>Další kroky
* Zjistěte, jak [vytvoření prostředí více virtuálních počítačů pomocí šablony Resource Manageru](devtest-lab-create-environment-from-arm.md).
* Prozkoumejte další šablony Resource Manageru rychlým zahájením pro automatizaci DevTest Labs z [veřejného úložiště DevTest Labs GitHub](https://github.com/Azure/azure-quickstart-templates).
