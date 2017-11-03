---
title: "Azure doporučení služby Advisor náklady | Microsoft Docs"
description: "Pomocí Azure Advisor optimalizovat náklady na vašich Azure nasazení."
services: advisor
documentationcenter: NA
author: KumudD
manager: carmonm
editor: 
ms.assetid: 
ms.service: advisor
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/16/2016
ms.author: kumud
ms.openlocfilehash: 51320d93689da3e37c0946d8877b27a11793d9c7
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="advisor-cost-recommendations"></a>Náklady na doporučení služby Advisor

Pomáhá Advisor optimalizovat a snížit vaše celkové Azure tráví určením nečinnosti a nedostatečně prostředky. Můžete získat náklady doporučení z **náklady** na řídicím panelu služby Advisor.

![Karta náklady na Advisor](./media/advisor-cost-recommendations/advisor-cost-tab2.png)

## <a name="optimize-virtual-machine-spend-by-resizing-underutilized-instances"></a>Optimalizovat virtuální počítač tráví změnou velikosti nedostatečně instancí 
I když některé scénáře aplikací může být nízkou míru využívání návrhu, můžete často ušetřit peníze pomocí správy velikost a počet virtuálních počítačů. Advisor monitoruje vaše využití virtuálního počítače po dobu 14 dnů a pak identifikuje nízké využití virtuálních počítačů. Virtuální počítače, jejichž využití procesoru je 5 % nebo méně a využití sítě je 7 MB nebo méně čtyři nebo více dní jsou považovány za nízké využití virtuálních počítačů.

Advisor zobrazí odhadované náklady pokračovat ke spuštění virtuálního počítače, takže je možné ho vypnout, nebo jeho velikost.  

![Advisor náklady doporučení pro změnu velikosti virtuálních počítačů](./media/advisor-cost-recommendations/advisor-cost-resizevms.png)

## <a name="use-a-cost-effective-solution-to-manage-performance-goals-of-multiple-sql-databases"></a>Použít nákladově efektivní řešení ke správě výkonnostní cíle více databází SQL
Advisor identifikuje instance serveru SQL, které můžete využít možnost vytvoření fondů elastické databáze. Fondy elastické databáze poskytují jednoduché a nákladově efektivní řešení pro správu výkonu cílů více databází, které mají různou vzorce používání. Další informace o Azure elastické fondy najdete v tématu [co je Azure Elastickém fondu?](https://azure.microsoft.com/en-us/documentation/articles/sql-database-elastic-pool/).

![Advisor náklady doporučení pro fondy elastické databáze](./media/advisor-cost-recommendations/advisor-cost-elasticdbpools.png)

## <a name="how-to-access-cost-recommendations-in-azure-advisor"></a>Jak získat přístup k náklady na doporučení v Azure Advisor

1. Přihlaste se k webu [Azure Portal](https://portal.azure.com).

2. V levém podokně klikněte na **další služby**.

3. V podokně nabídky služby v rámci **monitorování a správu**, klikněte na tlačítko **Azure Advisor**.  
 Se zobrazí řídicí panel služby Advisor.

4. Na řídicím panelu služby Advisor, klikněte na **náklady** kartě.

5. Vyberte předplatné, pro který chcete dostávat doporučení a potom klikněte na **získat doporučení**.

> [!NOTE]
> Chcete-li získat přístup k doporučení služby Advisor, je nutné nejprve *zaregistrovat předplatné* službou Advisor. Předplatné je zaregistrován při *předplatné vlastníka* spustí Advisor řídicího panelu a klikne na tlačítko **získat doporučení** tlačítko. Toto je *jednorázovou operaci*. Po registraci předplatného dostanete doporučení služby Advisor jako *vlastníka*, *Přispěvatel*, nebo *čtečky* pro předplatné, skupinu prostředků nebo konkrétní prostředek.

## <a name="next-steps"></a>Další kroky

Další informace o doporučení služby Advisor najdete v tématu:
* [Úvod do služby Advisor](advisor-overview.md)
* [Začínáme](advisor-get-started.md)
* [Poradce při hodnocení výkonu doporučení](advisor-cost-recommendations.md)
* [Doporučení pro vysokou dostupnost služby Advisor](advisor-cost-recommendations.md)
* [Doporučení zabezpečení Advisor](advisor-cost-recommendations.md)
