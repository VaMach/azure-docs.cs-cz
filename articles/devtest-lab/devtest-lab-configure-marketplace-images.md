---
title: "Konfigurace nastavení Azure Marketplace bitové kopie v Azure DevTest Labs | Microsoft Docs"
description: "Konfigurace, které Azure Marketplace obrázky lze použít při vytváření virtuálního počítače v Azure DevTest Labs"
services: devtest-lab,virtual-machines
documentationcenter: na
author: craigcaseyMSFT
manager: douge
editor: 
ms.assetid: 804c6af2-17e9-4320-af3a-f454bd398379
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/25/2016
ms.author: v-craic
ms.openlocfilehash: 5f7c9be115b27d6033429c1224fa15fd7b844c1d
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/21/2018
---
# <a name="configure-azure-marketplace-image-settings-in-azure-devtest-labs"></a>Konfigurace nastavení Azure Marketplace bitové kopie v Azure DevTest Labs
DevTest Labs podporuje vytváření virtuálních počítačů založené na imagích Azure Marketplace v závislosti na tom, jak jste nakonfigurovali Azure Marketplace Image ve vašem testovacím prostředí. Tento článek ukazuje, jak určit, které, pokud existuje, může být Azure Marketplace Image používá při vytváření virtuálních počítačů v testovacím prostředí. Tím se zajistí, že váš tým má přístup pouze k Marketplace bitové kopie, které potřebují. 

## <a name="select-which-azure-marketplace-images-are-allowed-when-creating-a-vm"></a>Vyberte, které Azure Marketplace obrázků jsou povoleny při vytváření virtuálního počítače
1. Přihlaste se k webu [Azure Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).
2. Vyberte **všechny služby**a potom vyberte **DevTest Labs** ze seznamu.
3. Ze seznamu labs vyberte požadované testovací prostředí. 
4. V okně v prostředí, vyberte **konfiguraci a zásady**.
5. Na testovacího prostředí na **konfiguraci a zásady** okno pod **základů virtuálního počítače**, vyberte **Marketplace image**.
6. Zadejte, zda chcete, aby všechny kvalifikovaný Azure Marketplace bitové kopie k dispozici pro použití jako základ nového virtuálního počítače. Pokud vyberete **Ano**, pak v Azure Marketplace bitové kopie, které splňují tato kritéria jsou povoleny v testovacím prostředí:
   
   * Obrázek vytvoří jeden virtuální počítač, **a**
   * Obrázek používá Azure Resource Manager zřizování virtuálních počítačů, **a**
   * Obrázek nevyžaduje zakoupení velmi licenčního plánu
     
    Pokud chcete povolit se žádné Image, nebo chcete určit obrázky, které lze použít, vyberte **ne**.
     
     ![Možnost, povolíte všechny Image Marketplace má být použit jako základní bitové kopie u virtuálních počítačů](./media/devtest-lab-configure-marketplace-images/allow-all-marketplace-images.png)
7. Pokud vyberete **ne** k předchozímu kroku, **bitové kopie nebo vyberte povoleno, všechny** zaškrtávací políčko je aktivní. 
   Tuto možnost společně s do vyhledávacího pole slouží k rychlému vyberte nebo zrušte všechny položky zobrazit v seznamu.
   * Vyberte Image Azure Marketplace, které chcete povolit pro vytvoření virtuálních počítačů jednotlivě zaškrtnutím políčka odpovídající každé bitové kopie.
   * Pokud chcete povolit všechny Image Azure Marketplace, který se má použít v testovacím prostředí, vyberte nic ze seznamu.
   
    ![Můžete zadat, které Azure Marketplace obrázky lze použít jako základní bitové kopie u virtuálních počítačů](./media/devtest-lab-configure-marketplace-images/select-marketplace-images.png)

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a>Další postup
Jakmile jste nakonfigurovali, jak jsou povoleny Azure Marketplace obrázky, při vytváření virtuálního počítače, dalším krokem je [přidat virtuální počítač do testovacího prostředí](devtest-lab-add-vm.md).

