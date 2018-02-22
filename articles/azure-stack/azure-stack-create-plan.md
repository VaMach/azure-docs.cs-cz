---
title: "Vytvoření plánu v zásobníku Azure | Microsoft Docs"
description: "Jako správce cloudu vytvořte plán, který umožňuje odběratelům zřizování virtuálních počítačů."
services: azure-stack
documentationcenter: 
author: brenduns
manager: femila
editor: 
ms.assetid: 3dc92e5c-c004-49db-9a94-783f1f798b98
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 7/10/2017
ms.author: brenduns
ms.reviewer: 
ms.openlocfilehash: 5eefca3541ae9f73514f80b0f8df9e5027600f87
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/21/2018
---
# <a name="create-a-plan-in-azure-stack"></a>Vytvoření plánu ve službě Azure Stack

*Platí pro: Azure zásobníku integrované systémy a Azure zásobníku Development Kit*

[Plány](azure-stack-key-features.md) představují seskupení jedné nebo více služeb. Jako zprostředkovatele můžete vytvořit plány, které nabízejí uživatelům. Pak vaši uživatelé přihlásit k vaší nabídky pro plány a službách, které obsahují. Tento příklad ukazuje, jak vytvořit plán, který obsahuje výpočty, síť a zprostředkovatelé prostředků služby storage. Tento plán poskytuje odběratele umožňuje zřizovat virtuální počítače.

1. Přihlaste se k portálu správce Azure zásobníku (https://adminportal.local.azurestack.external). Zadejte pověření pro účet, který jste vytvořili během kroku 5 tohoto [spustit skript prostředí PowerShell](azure-stack-run-powershell-script.md) části.

2. Chcete-li vytvořit plán a nabídka, která uživatelé mohou přihlásit k odběru, klikněte na tlačítko **nový** > **klienta nabízí + plány** > **plán**.

   ![](media/azure-stack-create-plan/image01.png)
3. V **nový plán** okno vyplňte **zobrazovaný název** a **název prostředku**. Zobrazovaný název je popisný název plánu, který uživatelé uvidí. Název prostředku může vidět jenom správce. Je název, který správci používat pro práci s plánem jako prostředek Azure Resource Manager.

   ![](media/azure-stack-create-plan/image02.png)
4. Vytvořte novou **skupiny prostředků**, nebo vyberte existující jako kontejner pro plán.

   ![](media/azure-stack-create-plan/image02a.png)
5. Klikněte na tlačítko **služby**, vyberte **Microsoft.Compute**, **Microsoft.Network**, a **Microsoft.Storage**a potom klikněte na **Vyberte**.

   ![](media/azure-stack-create-plan/image03.png)
6. Klikněte na tlačítko **kvóty**, klikněte na tlačítko **Microsoft.Storage (místní)**a pak vyberte výchozí kvótu, nebo klikněte na tlačítko **vytvořit nové kvóty** přizpůsobit kvótu.

   ![](media/azure-stack-create-plan/image04.png)
7. Pokud vytváříte novou kvótu, zadejte název pro kvótu > nastavte hodnoty kvóty > klikněte na tlačítko **OK** > klikněte na název nové kvóty.

   ![](media/azure-stack-create-plan/image06.png)
8. Klikněte na tlačítko **Microsoft.Network (místní)**a pak vyberte výchozí kvótu, nebo klikněte na tlačítko **vytvořit nové kvóty** přizpůsobit kvótu.

    ![](media/azure-stack-create-plan/image07.png)
9. Pokud vytváříte novou kvótu, zadejte název pro kvótu > nastavte hodnoty kvóty > klikněte na tlačítko **OK** > klikněte na název nové kvóty.

    ![](media/azure-stack-create-plan/image08.png)
10. Klikněte na tlačítko **Microsoft.Compute (místní)**a pak vyberte výchozí kvótu, nebo klikněte na tlačítko **vytvořit nové kvóty** přizpůsobit kvótu.

    ![](media/azure-stack-create-plan/image09.png)
11. Pokud vytváříte novou kvótu, zadejte název pro kvótu > nastavte hodnoty kvóty > klikněte na tlačítko **OK** > klikněte na název nové kvóty.

    ![](media/azure-stack-create-plan/image10.png)
12. V **kvóty** okně klikněte na tlačítko **OK**a potom v **nový plán** okně klikněte na tlačítko **vytvořit** vytvořte plán.

    ![](media/azure-stack-create-plan/image11.png)
13. Chcete-li zobrazit nový plán, klikněte na tlačítko **všechny prostředky**, vyhledejte plán a klikněte na jeho název.

    ![](media/azure-stack-create-plan/image12.png)

### <a name="next-steps"></a>Další postup
[Vytvoření nabídky](azure-stack-create-offer.md)
