---
title: "Účty úložiště v Azure zásobníku | Microsoft Docs"
description: "Zjistěte, jak vytvořit účet úložiště Azure zásobníku."
services: azure-stack
documentationcenter: 
author: vhorne
manager: byronr
editor: 
ms.assetid: e1152110-b756-4c1a-9fa2-73fe3ab0ad8e
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 3/1/2017
ms.author: victorh
ms.openlocfilehash: 41c9ee37c43d4ad41c51ea2ed023d3b47d460dd1
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="storage-accounts-in-azure-stack"></a>Účty úložiště v Azure Stack
Účty úložiště zahrnují služby Blob a Table a unikátní obor názvů pro datové objekty úložiště. Ve výchozím nastavení jsou data ve vašem účtu dostupná pouze pro vás, vlastníka účtu úložiště.

1. V počítači, POC zásobník Azure, přihlaste se k `https://adminportal.local.azurestack.external` jako [správce](azure-stack-connect-azure-stack.md)a potom klikněte na **nový** > **Data + úložiště**  >  **Účet úložiště**.

   ![](media/azure-stack-provision-storage-account/image01.png)
2. V **vytvořit účet úložiště** okno, zadejte název účtu úložiště. Vytvořte novou **skupiny prostředků**, nebo vyberte existující šablonu a potom klikněte na tlačítko **vytvořit** k vytvoření účtu úložiště.

   ![](media/azure-stack-provision-storage-account/image02.png)
3. Chcete-li zobrazit váš nový účet úložiště, klikněte na tlačítko **všechny prostředky**, vyhledejte účet úložiště a klikněte na jeho název.

    ![](media/azure-stack-provision-storage-account/image03.png)

### <a name="next-steps"></a>Další kroky
[Použití šablon Azure Resource Manageru](user/azure-stack-arm-templates.md)

[Další informace o účtech Azure storage](../storage/common/storage-create-storage-account.md)

[Stáhnout příručku ověření Azure konzistentní úložiště Azure zásobníku](http://aka.ms/azurestacktp1doc)
