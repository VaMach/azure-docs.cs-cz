---
title: "Vytvořit testovací virtuální počítač v Azure zásobníku | Microsoft Docs"
description: "Zjistěte, jak zřídit testovací virtuální počítač v Azure zásobníku jako operátor cloudu."
services: azure-stack
documentationcenter: 
author: brenduns
manager: femila
editor: 
ms.assetid: c86646e1-a12e-493f-b396-f17bfacd60c2
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 9/25/2017
ms.author: brenduns
ms.reviewer: 
ms.openlocfilehash: 41096f68e5e7d9e31098d1e8919101418abe4c03
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/21/2018
---
# <a name="create-a-test-virtual-machine-in-azure-stack"></a>Vytvoření testovacího virtuálního počítače v Azure zásobníku

*Platí pro: Azure zásobníku Development Kit*

Jako operátor zásobník Azure, můžete vytvořit testovací virtuální počítač k ověření vaší [zásobník Azure](azure-stack-poc.md) nasazení sady pro vývojáře.

> [!NOTE]
> Než zřídíte virtuální počítače, musíte [přidat bitovou kopii zkušební verze Windows serveru 2016 do zásobníku Azure marketplace](azure-stack-add-default-image.md).
> 
> 

## <a name="create-a-virtual-machine"></a>Vytvoření virtuálního počítače
1. Na hostiteli Azure zásobníku Development Kit [přihlášení](azure-stack-connect-azure-stack.md) k portálu správce (`https://adminportal.local.azurestack.external`) a potom klikněte na **nový** > **výpočetní**  >  **Zkušební verze systému Windows Server 2016 Datacenter** > **vytvořit**.  
2. V **Základy** okno, zadejte **název**, **uživatelské jméno**, a **heslo**. Vyberte **předplatné**. Vytvoření **skupiny prostředků**, nebo vyberte existující šablonu a pak klikněte na tlačítko **OK**.  
3. V **zvolte velikost** okně klikněte na tlačítko **A1 standardní**a potom klikněte na **vyberte**.  
4. V **nastavení** , přijměte výchozí hodnoty a klikněte na tlačítko **OK**
5. V okně **Shrnutí** klikněte na **OK** a vytvořte virtuální počítač.  
6. Chcete-li zobrazit nový virtuální počítač, klikněte na tlačítko **všechny prostředky**, vyhledejte virtuální počítač a klikněte na jeho název.
    ![](media/azure-stack-provision-vm/image06.png)


## <a name="next-steps"></a>Další postup
[Používání portálů správce a uživatele v Azure zásobníku](azure-stack-manage-portals.md)
