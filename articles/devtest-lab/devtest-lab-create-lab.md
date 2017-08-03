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
ms.date: 05/30/2017
ms.author: tarcher
ms.translationtype: HT
ms.sourcegitcommit: fff84ee45818e4699df380e1536f71b2a4003c71
ms.openlocfilehash: 265a968f902f53c7561c8c7e937f8eacfdb37167
ms.contentlocale: cs-cz
ms.lasthandoff: 08/01/2017

---
# <a name="create-a-lab-in-azure-devtest-labs"></a>Vytvoření testovacího prostředí v Azure DevTest Labs
Testovací prostředí ve službě Azure DevTest Labs je infrastruktura, která zahrnuje skupinu prostředků, třeba službu Virtual Machines, která vám umožní lépe spravovat tyto prostředky zadáním omezení a kvót. Tento článek vás provede procesem vytvoření testovacího prostředí pomocí webu Azure Portal.

## <a name="prerequisites"></a>Požadavky
K vytvoření testovacího prostředí potřebujete:

* Předplatné Azure. Informace o možnostech nákupu Azure najdete v tématech [Jak koupit Azure](https://azure.microsoft.com/pricing/purchase-options/) nebo [Bezplatná zkušební verze na jeden měsíc](https://azure.microsoft.com/pricing/free-trial/). Abyste mohli vytvořit testovací prostředí, musíte být vlastníky předplatného.

## <a name="steps-to-create-a-lab-in-azure-devtest-labs"></a>Postup vytvoření testovacího prostředí ve službě Azure DevTest Labs
Následující kroky ukazují postup vytvoření testovacího prostředí ve službě Azure DevTest Labs pomocí webu Azure Portal. 

1. Přihlaste se k webu [Azure Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).
1. V hlavní nabídce na levé straně vyberte **Další služby** (v dolní části seznamu).

    ![Možnost nabídky Další služby](./media/devtest-lab-create-lab/more-services-menu-option.png)

1. V seznamu dostupných služeb vyberte **DevTest Labs**.
1. V okně **DevTest Labs** vyberte **Přidat**.
   
    ![Přidání testovacího prostředí](./media/devtest-lab-create-lab/add-lab-button.png)

1. V okně pro **vytvoření testovacího prostředí DevTest**:
   
    1. Zadejte **Název testovacího prostředí** pro nové prostředí.
    2. Vyberte **Předplatné**, které bude přidruženo k testovacímu prostředí.
    3. Vyberte **Umístění**, do kterého se testovací prostředí uloží.
    4. Chcete-li povolit a nastavit parametry pro automatické vypínání virtuálních počítačů v testovacím prostředí, vyberte **Automatické vypnutí**. Funkce automatického vypnutí je především funkce na úsporu nákladů, pomocí které můžete určit, kdy se má virtuální počítač automaticky vypnout. Po vytvoření testovacího prostředí můžete nastavení automatického vypnutí změnit podle kroků uvedených v článku [Správa všech zásad pro testovací prostředí v Azure DevTest Labs](./devtest-lab-set-lab-policy.md#set-auto-shutdown).
    5. Pokud chcete na řídicím panelu portálu vytvořit zástupce testovacího prostředí, vyberte možnost **Připnout na řídicí panel**.
    6. Pokud chcete získat šablony Azure Resource Manageru pro automatizaci konfigurace, vyberte **Možnosti automatizace**. 
    7. Vyberte **Vytvořit**. Po výběru možnosti **Vytvořit** se zobrazí okno **DevTest Labs**. Stav procesu vytvoření testovacího prostředí můžete monitorovat v oblasti **Oznámení**. Po dokončení aktualizujte stránku, abyste zobrazili nově vytvořené testovací prostředí v seznamu testovacích prostředí.  
    
    ![Okno pro vytvoření testovacího prostředí](./media/devtest-lab-create-lab/create-devtestlab-blade.png)

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a>Další kroky
Po vytvoření testovacího prostředí je zde několik kroků, které je vhodné zvážit:

* [Zabezpečení přístupu k testovacímu prostředí](devtest-lab-add-devtest-user.md)
* [Nastavení zásad testovacího prostředí](devtest-lab-set-lab-policy.md)
* [Vytvoření šablony testovacího prostředí](devtest-lab-create-template.md)
* [Vytvoření vlastních artefaktů pro virtuální počítače](devtest-lab-artifact-author.md)
* [Přidání virtuálního počítače s artefakty do testovacího prostředí](https://azure.microsoft.com/resources/videos/how-to-create-vms-with-artifacts-in-a-devtest-lab/)


