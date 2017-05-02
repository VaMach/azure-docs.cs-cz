---
title: "Vytvoření testovacího prostředí ve službě Azure DevTest Labs | Dokumentace Microsoftu"
description: "Vytvoření testovacího prostředí ve službě Azure DevTest Labs pro virtuální počítače"
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: 8b6d3e70-6528-42a4-a2ef-449575d0f928
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 09/12/2016
ms.author: tarcher
translationtype: Human Translation
ms.sourcegitcommit: 8c4e33a63f39d22c336efd9d77def098bd4fa0df
ms.openlocfilehash: 75188a8f74b08d28b92259348626d5734ed4ce6c
ms.lasthandoff: 04/19/2017


---
# <a name="create-a-lab-in-azure-devtest-labs"></a>Vytvoření testovacího prostředí v Azure DevTest Labs
## <a name="prerequisites"></a>Požadavky
K vytvoření testovacího prostředí potřebujete:

* Předplatné Azure. Informace o možnostech nákupu Azure najdete v tématech [Jak koupit Azure](https://azure.microsoft.com/pricing/purchase-options/) nebo [Bezplatná zkušební verze na jeden měsíc](https://azure.microsoft.com/pricing/free-trial/). Abyste mohli vytvořit testovací prostředí, musíte být vlastníky předplatného.

## <a name="steps-to-create-a-lab-in-azure-devtest-labs"></a>Postup vytvoření testovacího prostředí ve službě Azure DevTest Labs
Následující kroky ukazují postup vytvoření testovacího prostředí ve službě Azure DevTest Labs pomocí webu Azure Portal. 

1. Přihlaste se k webu [Azure Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).
2. Vyberte **Další služby** a poté ze seznamu vyberte **DevTest Labs**.
3. V okně **DevTest Labs** vyberte **Přidat**.
   
    ![Přidání testovacího prostředí](./media/devtest-lab-create-lab/add-lab-button.png)
4. V okně pro **vytvoření testovacího prostředí DevTest**:
   
   1. Zadejte **Název testovacího prostředí** pro nové prostředí.
   2. Vyberte **Předplatné**, které bude přidruženo k testovacímu prostředí.
   3. Vyberte **Umístění**, do kterého se testovací prostředí uloží.
   4. Chcete-li povolit a nastavit parametry pro automatické vypínání virtuálních počítačů v testovacím prostředí, vyberte **Automatické vypnutí**. 
   5. Pokud chcete na řídicím panelu portálu vytvořit zástupce testovacího prostředí, vyberte možnost **Připnout na řídicí panel**.
   6. Pokud chcete získat šablony Azure Resource Manageru pro automatizaci konfigurace, vyberte **Možnosti automatizace**. 
   7. Vyberte **Vytvořit**.
    
    ![Okno pro vytvoření testovacího prostředí](./media/devtest-lab-create-lab/create-devtestlab-blade.png)

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a>Další kroky
Po vytvoření testovacího prostředí je zde několik kroků, které je vhodné zvážit:

* [Zabezpečení přístupu k testovacímu prostředí](devtest-lab-add-devtest-user.md)
* [Nastavení zásad testovacího prostředí](devtest-lab-set-lab-policy.md)
* [Vytvoření šablony testovacího prostředí](devtest-lab-create-template.md)
* [Vytvoření vlastních artefaktů pro virtuální počítače](devtest-lab-artifact-author.md)
* [Přidání virtuálního počítače s artefakty do testovacího prostředí](devtest-lab-add-vm-with-artifacts.md)


