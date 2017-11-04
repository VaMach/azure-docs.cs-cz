---
title: "Stažení položky marketplace z Azure | Microsoft Docs"
description: "Položky marketplace lze stáhnout z Azure do nasazení Moje zásobník Azure."
services: azure-stack
documentationcenter: 
author: ErikjeMS
manager: byronr
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/03/2017
ms.author: erikje
ms.openlocfilehash: 4d7c335a3c68cc9bb8cb0c823883716a3dd6620a
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="download-marketplace-items-from-azure-to-azure-stack"></a>Stažení položky marketplace z Azure do Azure zásobníku

*Platí pro: Azure zásobníku integrované systémy a Azure zásobníku Development Kit*

Jak se rozhodnete, který obsah, který chcete zahrnout do vaší zásobník Azure marketplace, měli byste zvážit obsah dostupný v Azure Marketplace. Můžete stáhnout ze seznamu spravovaných položek Azure marketplace, které byly předem otestované ke spuštění v Azure zásobníku. Často přidání nových položek do tohoto seznamu, tak zkontrolujte, že kontrola zpět nový obsah.

Chcete-li stáhnout položky marketplace, je nutné nejprve [zaregistrovat zásobník Azure s Azure](azure-stack-register.md). 

## <a name="download"></a>Ke stažení
1. Přihlaste se k portálu správce Azure zásobníku (https://portal.local.azurestack.external).
2. Některé položky marketplace. mohou být značně velké.  Zkontrolujte a ujistěte se, zda máte dostatek místa v systému klepnutím na tlačítko **zprostředkovatelé prostředků** > **úložiště**.

    ![](media/azure-stack-download-azure-marketplace-item/image01.png)

3. Klikněte na tlačítko **další služby** > **Marketplace správu**.

    ![](media/azure-stack-download-azure-marketplace-item/image02.png)

4. Klikněte na tlačítko **přidat z Azure** zobrazíte seznam položek, které jsou k dispozici ke stažení. Kliknutím na každou položku v seznamu k zobrazení jeho popis a velikost souboru ke stažení.

    ![](media/azure-stack-download-azure-marketplace-item/image03.png)

5. Vyberte položku v seznamu a pak klikněte na **Stáhnout**. Tím se spustí stahování image virtuálního počítače pro položku, kterou jste vybrali. Stahování se liší.

    ![](media/azure-stack-download-azure-marketplace-item/image04.png)

6. Po dokončení stahování, můžete nasadit novou položku marketplace jako operátor zásobník Azure nebo uživatele. Klikněte na tlačítko **+ nový**, hledání mezi kategorie pro novou položku marketplace. a potom vyberte položku.
7. Klikněte na tlačítko **vytvořit** otevřete vytváření prostředí pro nově stažených položek. Podrobné pokyny k nasazení vaší položky.

## <a name="next-steps"></a>Další kroky

[Vytvoření a publikování položky Marketplace](azure-stack-create-and-publish-marketplace-item.md)
