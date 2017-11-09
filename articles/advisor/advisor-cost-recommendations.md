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
ms.openlocfilehash: 7b9c7037271fabd67c1ada80420ad72c340e46bb
ms.sourcegitcommit: ce934aca02072bdd2ec8d01dcbdca39134436359
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/08/2017
---
# <a name="advisor-cost-recommendations"></a>Náklady na doporučení služby Advisor

Pomáhá Advisor optimalizovat a snížit vaše celkové Azure tráví určením nečinnosti a nedostatečně prostředky. Můžete získat náklady doporučení z **náklady** na řídicím panelu služby Advisor.

## <a name="optimize-virtual-machine-spend-by-resizing-or-shutting-down-underutilized-instances"></a>Optimalizace tráví virtuálního počítače nebo změnou velikosti vypíná nedostatečně instancí 
I když některé scénáře aplikací může být nízkou míru využívání návrhu, můžete často ušetřit peníze pomocí správy velikost a počet virtuálních počítačů. Advisor monitoruje vaše využití virtuálního počítače po dobu 14 dnů a pak identifikuje nízké využití virtuálních počítačů. Virtuální počítače, jejichž využití procesoru je 5 % nebo méně a využití sítě je 7 MB nebo méně čtyři nebo více dní jsou považovány za nízké využití virtuálních počítačů.

Advisor zobrazí odhadované náklady pokračovat ke spuštění virtuálního počítače, takže je možné ho vypnout, nebo jeho velikost.

Pokud chcete být agresivnější zjistit nedostatečně virtuální počítače, můžete upravit průměrná pravidlo využití procesoru na základě za předplatné.

## <a name="use-a-cost-effective-solution-to-manage-performance-goals-of-multiple-sql-databases"></a>Použít nákladově efektivní řešení ke správě výkonnostní cíle více databází SQL
Advisor identifikuje instance serveru SQL, které můžete využít možnost vytvoření fondů elastické databáze. Fondy elastické databáze poskytují jednoduché a nákladově efektivní řešení pro správu výkonu cílů více databází, které mají různou vzorce používání. Další informace o Azure elastické fondy najdete v tématu [co je Azure Elastickém fondu?](https://azure.microsoft.com/en-us/documentation/articles/sql-database-elastic-pool/).

## <a name="reduce-costs-by-eliminating-unprovisioned-expressroute-circuits"></a>Snížení nákladů odstraněním není zřízený okruhy ExpressRoute
Advisor identifikuje okruhy ExpressRoute, které byly ve stavu zprostředkovatele *není zajišťováno* pro více než jeden měsíc a doporučuje odstraňování okruh, pokud nemáte v plánu poskytnutí okruhu připojení k Zprostředkovatel.

## <a name="how-to-access-cost-recommendations-in-azure-advisor"></a>Jak získat přístup k náklady na doporučení v Azure Advisor

1. Přihlaste se k [portál Azure](https://portal.azure.com)a pak otevřete [Advisor](https://aka.ms/azureadvisordashboard).

2.  Na řídicím panelu služby Advisor, klikněte na **náklady** kartě.

## <a name="next-steps"></a>Další kroky

Další informace o doporučení služby Advisor najdete v tématu:
* [Úvod do služby Advisor](advisor-overview.md)
* [Začínáme](advisor-get-started.md)
* [Poradce při hodnocení výkonu doporučení](advisor-cost-recommendations.md)
* [Doporučení pro vysokou dostupnost služby Advisor](advisor-cost-recommendations.md)
* [Doporučení zabezpečení Advisor](advisor-cost-recommendations.md)
