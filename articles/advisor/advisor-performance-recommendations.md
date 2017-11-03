---
title: "Azure doporučení služby Advisor výkonu | Microsoft Docs"
description: "Použití Advisor k optimalizaci výkonu vašich Azure nasazení."
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
ms.openlocfilehash: 995a1f37a3fd68b39c14a95d46109c0f7814018d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="advisor-performance-recommendations"></a>Poradce při hodnocení výkonu doporučení

Azure doporučení výkonu služby Advisor napomáhají, a zvýšit rychlost a reakce důležitých podnikových aplikací. Výkon doporučení služby Advisor můžete získat **výkonu** Advisor řídicího panelu.

![Karta výkonu Advisor](./media/advisor-performance-recommendations/advisor-performance-tab.png)

## <a name="improve-database-performance-with-sql-db-advisor"></a>Zlepšení výkonu databáze službou SQL DB Advisor

Advisor vám poskytne konzistentní, konsolidované zobrazení doporučení pro všechny prostředky Azure. Integruje se službou Advisor databáze SQL, aby vám doporučení pro zlepšení výkonu databáze SQL Azure. Poradce pro funkci SQL Database vyhodnocuje analýzou historii využití výkon vaší databáze SQL Azure. Potom nabízí doporučení, která jsou nejvhodnější pro spuštění typické zatížení databáze. 

> [!NOTE]
> Doporučení získáte databáze musí obsahovat o týden využití, a v daném týdnu musí být některé konzistentní aktivity. Poradce pro databáze SQL můžete optimalizovat snadněji konzistentní dotazu v případě vzorů než pro náhodné shluky aktivity.

Další informace o službě Advisor databáze SQL najdete v tématu [Poradce pro funkci SQL Database](https://azure.microsoft.com/en-us/documentation/articles/sql-database-advisor/).

![Doporučení k databázi SQL](./media/advisor-performance-recommendations/advisor-performance-sql.png)

## <a name="improve-redis-cache-performance-and-reliability"></a>Zlepšení výkonu Redis Cache a spolehlivosti

Advisor identifikuje instance služby Redis Cache kde výkon může být nepříznivě ovlivněn velké množství paměti, zatížení serveru, šířky pásma sítě nebo velký počet připojení klientů. Osvědčené postupy Advisor také poskytuje doporučení, která umožňuje vyhnout se možným problémům. Další informace o doporučení Redis Cache najdete v tématu [Redis Cache Advisor](https://azure.microsoft.com/en-us/documentation/articles/cache-configure/#redis-cache-advisor).


## <a name="improve-app-service-performance-and-reliability"></a>Zlepšení výkonu služby App Service a spolehlivosti

Azure Advisor integruje doporučení pro zlepšení prostředí aplikační služby a zjišťování možnosti příslušné platformy. Příklady App Services doporučení:
* Zjišťování instancí, kde se pomocí aplikace moduly runtime s možnostmi zmírnění vyčerpání paměti nebo prostředků procesoru.
* Zjišťování instancí, které tyto prostředky jako webové aplikace a databáze může zvýšit výkon a nižší náklady. 

Další informace o App Services doporučení najdete v tématu [osvědčené postupy pro službu Azure App Service](https://azure.microsoft.com/en-us/documentation/articles/app-service-best-practices/).
![Doporučení služby aplikace](./media/advisor-performance-recommendations/advisor-performance-app-service.png)

## <a name="how-to-access-performance-recommendations-in-advisor"></a>Jak získat přístup k výkonu doporučení v Advisor

1. Přihlaste se k webu [Azure Portal](https://portal.azure.com).

2. V levém podokně klikněte na **další služby**.

3. V podokně nabídky služby v rámci **monitorování a správu**, klikněte na tlačítko **Azure Advisor**.  
 Se zobrazí řídicí panel služby Advisor.

4. Na řídicím panelu služby Advisor, klikněte na **výkonu** kartě.

5. Vyberte předplatné, pro který chcete dostávat doporučení a potom klikněte na **získat doporučení**.

> [!NOTE]
> Chcete-li získat přístup k doporučení služby Advisor, je nutné nejprve *zaregistrovat předplatné* službou Advisor. Předplatné je zaregistrován při *předplatné vlastníka* spustí Advisor řídicího panelu a klikne na tlačítko **získat doporučení** tlačítko. Toto je *jednorázovou operaci*. Po registraci předplatného dostanete doporučení služby Advisor jako *vlastníka*, *Přispěvatel*, nebo *čtečky* pro předplatné, skupinu prostředků nebo konkrétní prostředek.

## <a name="next-steps"></a>Další kroky

Další informace o doporučení služby Advisor najdete v tématu:

* [Úvod do služby Advisor](advisor-overview.md)
* [Začínáme se službou Advisor](advisor-get-started.md)
* [Náklady na doporučení služby Advisor](advisor-performance-recommendations.md)
* [Doporučení pro vysokou dostupnost služby Advisor](advisor-high-availability-recommendations.md)
* [Doporučení zabezpečení Advisor](advisor-security-recommendations.md)

