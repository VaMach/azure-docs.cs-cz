---
title: "Vytvořit nabídku v zásobníku Azure | Microsoft Docs"
description: "Jako správce cloudu zjistěte, jak vytvořit nabídku pro vaše uživatele v Azure zásobníku."
services: azure-stack
documentationcenter: 
author: ErikjeMS
manager: byronr
editor: 
ms.assetid: 96b080a4-a9a5-407c-ba54-111de2413d59
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/10/2017
ms.author: erikje
ms.openlocfilehash: 269a6106f657536ba74be366f842b2f9cd86c5dc
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="create-an-offer-in-azure-stack"></a>Vytvoření nabídky ve službě Azure Stack

*Platí pro: Azure zásobníku integrované systémy a Azure zásobníku Development Kit*

[Nabízí](azure-stack-key-features.md) jsou skupiny jeden nebo více plánů, která představují zprostředkovatelů pro uživatele se koupit nebo přihlášení k odběru. Tento dokument ukazuje, jak vytvořit nabídku, která zahrnuje [plán, který jste vytvořili](azure-stack-create-plan.md) v posledním kroku. Tato nabídka poskytuje odběratele umožňuje zřizovat virtuální počítače.

1. Přihlaste se k portálu správce Azure zásobníku (https://adminportal.local.azurestack.external) > klikněte na tlačítko **nový** > **klienta nabízí + plány**  >   **Nabízejí**.

   ![](media/azure-stack-create-offer/image01.png)
2. V **nové nabízejí** okno vyplňte **zobrazovaný název** a **název prostředku**a poté vyberte nový nebo existující **skupiny prostředků**. Zobrazovaný název je nabídka popisný název a obsahuje jenom informace o nabídka, která uživatelům zobrazí při přihlášení k odběru. Proto je nutné používat intuitivní název, který pomáhá uživateli pochopit, co se dodává s nabídkou. Název prostředku může vidět jenom správce. Je to název, který správci používají pro práci s nabídkou jako s prostředkem Azure Resource Manageru.

   ![](media/azure-stack-create-offer/image01a.png)
3. Klikněte na **Základní plány** a v okně **Plán** vyberte plány, které chcete do nabídky zahrnout, a potom klikněte na **Vybrat**. Kliknutím na **Vytvořit** vytvořte svou nabídku.

   ![](media/azure-stack-create-offer/image02.png)
4. Klikněte na tlačítko **všechny prostředky**, vyhledejte novou nabídku, klikněte na nabídku nový, klikněte na **změny stavu**a potom klikněte na **veřejné**.

   ![](media/azure-stack-create-offer/image03.png)

Nabízí musí být provedeny veřejné uživatelům získat úplné zobrazení při přihlášení k odběru. Nabízí může být:

* **Veřejné**: viditelné pro uživatele.
* **Privátní**: viditelné na správce cloudu. Užitečné při navrhování plán nebo nabídky, nebo pokud chce správce cloudu schválení každé předplatné.
* **Vyřazeno z provozu**: Uzavřené pro nové odběratele. Můžete použít Správce cloudu vyřazení zabránit budoucím odběry, ale ponechat nezměněný aktuální odběratele.

Změny nabídku nejsou okamžitě viditelné pro uživatele. Chcete-li zobrazit změny, bude pravděpodobně nutné odhlášení nebo přihlášení zobrazíte nové předplatné v "pro výběr předplatného" při vytváření skupiny prostředků nebo prostředků.

> [!NOTE]
>Můžete také vytvořit výchozí nabízí, plány a kvóty pomocí prostředí PowerShell, jak je popsáno v [Správce služby Azure zásobníku readme](https://github.com/Azure/AzureStack-Tools/tree/master/ServiceAdmin).
>


### <a name="next-steps"></a>Další kroky
[Přihlášení k odběru na nabídku a pak zřízení virtuálního počítače](azure-stack-subscribe-plan-provision-vm.md)
