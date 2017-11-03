---
title: "Začínáme s Azure Advisor | Microsoft Docs"
description: "Začínáme s Azure Advisor."
services: advisor
documentationcenter: NA
author: manbeenkohli
manager: carmonm
editor: 
ms.assetid: 
ms.service: advisor
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 02/10/2017
ms.author: makohli
ms.openlocfilehash: a662841bebda460d4225e080f16705b3f16fdc46
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="get-started-with-azure-advisor"></a>Začínáme se službou Azure Advisor

Zjistěte, jak přístup Advisor prostřednictvím portálu Azure, získat doporučení, implementujte doporučení, vyhledejte doporučení a doporučení aktualizace.

## <a name="get-advisor-recommendations"></a>Doporučení Advisoru

1. Přihlaste se k webu [Azure Portal](https://portal.azure.com).

2. V levém podokně klikněte na **další služby**.

3. V podokně nabídky služby v rámci **monitorování a správu**, klikněte na tlačítko **Azure Advisor**.  
 Se zobrazí řídicí panel služby Advisor.

   ![Přístup k Azure Advisor pomocí portálu Azure](./media/advisor-overview/advisor-azure-portal-menu.png) 

4. Na řídicím panelu Advisor vyberte předplatné, pro který chcete dostávat doporučení.  
Řídicí panel Advisor zobrazuje přizpůsobené doporučení pro vybrané předplatné. 

5. Chcete-li získat doporučení pro určité kategorie, klikněte na jednu z karty: **vysokou dostupnost**, **zabezpečení**, **výkonu**, nebo **náklady**.
 
> [!NOTE]
> Chcete-li získat přístup k doporučení služby Advisor, je nutné nejprve *zaregistrovat předplatné* službou Advisor. Předplatné je zaregistrován při *předplatné vlastníka* spustí Advisor řídicího panelu a klikne na tlačítko **získat doporučení** tlačítko. Toto je *jednorázovou operaci*. Po registraci předplatného dostanete doporučení služby Advisor jako *vlastníka*, *Přispěvatel*, nebo *čtečky* pro předplatné, skupinu prostředků nebo konkrétní prostředek.

  ![Řídicí panel Azure Advisor](./media/advisor-overview/advisor-all-tab.png)

## <a name="get-advisor-recommendation-details-and-implement-a-solution"></a>Získat podrobnosti o doporučení služby Advisor a implementovat řešení

**Doporučení** okno v Advisor nabízí další informace o doporučení. 

1. Přihlaste se k [portál Azure](https://portal.azure.com)a pak spusťte [Azure Advisor](https://aka.ms/azureadvisordashboard).

2. Na **doporučení služby Advisor** řídicí panel, klikněte na tlačítko **získat doporučení**.

3. V seznamu doporučení klikněte na doporučení, které chcete zkontrolovat podrobně.  
**Doporučení** zobrazí se okno.

4. Na **doporučení** okno, zkontrolujte informace o akcích, můžete provést vyřešit potenciální problém nebo využít možnost náklady na ukládání. 
  
  ![V okně doporučení služby Advisor](./media/advisor-overview/advisor-recommendation-action-example.png)

## <a name="search-for-advisor-recommendations"></a>Vyhledejte doporučení služby Advisor

Můžete vyhledat doporučení pro konkrétní skupinu předplatné nebo prostředek. Doporučení můžete taky vyhledat podle stavu.

1. Přihlaste se k [portál Azure](https://portal.azure.com)a pak spusťte [Azure Advisor](https://aka.ms/azureadvisordashboard).

2. Vyhledejte doporučení pomocí filtrování pro stav doporučení, skupiny prostředků a předplatná (**Active** nebo **Snoozed**).

3. Chcete-li zobrazit seznam doporučení Advisor, které jsou založeny na kritéria vyhledávání filtru, klikněte na tlačítko **získat doporučení**.

  ![Kritéria vyhledávací filtr služby Advisor](./media/advisor-get-started/advisor-search.png)

## <a name="snooze-or-dismiss-advisor-recommendations"></a>Připomenout znovu nebo zrušit doporučení služby Advisor

1. Přihlaste se k [portál Azure](https://portal.azure.com)a pak spusťte [Azure Advisor](https://aka.ms/azureadvisordashboard).

2. Klikněte na tlačítko **získat doporučení**a potom v seznamu doporučení, klikněte na doporučení.

3. Na **doporučení** okně klikněte na tlačítko **připomenout znovu**.  

   ![Příklad akce doporučení služby Advisor](./media/advisor-get-started/advisor-snooze.png)

4. Zadejte připomenutí časové období, nebo vyberte **nikdy** zrušíte doporučení.


## <a name="next-steps"></a>Další kroky

Další informace o službě Advisor najdete v tématu:
* [Úvod do Azure Advisor](advisor-overview.md)
* [Doporučení pro vysokou dostupnost služby Advisor](advisor-high-availability-recommendations.md)
* [Doporučení zabezpečení Advisor](advisor-security-recommendations.md)
-  [Poradce při hodnocení výkonu doporučení](advisor-performance-recommendations.md)
* [Náklady na doporučení služby Advisor](advisor-performance-recommendations.md)
