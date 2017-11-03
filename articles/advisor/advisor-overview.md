---
title: "Úvod do Azure Advisor | Microsoft Docs"
description: "Chcete-li optimalizovat nasazení Azure pomocí Azure Advisor."
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
ms.openlocfilehash: 96925f251cf4984a11516a962740e19a7b9589dc
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="introduction-to-azure-advisor"></a>Úvod do Azure Advisor

Další informace o Azure Advisor a jejími klíčovými funkcemi a získejte odpovědi na nejčastější dotazy.

## <a name="what-is-advisor"></a>Co je Advisor?
Advisor je konzultantem přizpůsobené cloudu, která pomáhá dodržujte doporučené postupy, chcete-li optimalizovat nasazení Azure. Ho analyzuje konfigurace prostředků a telemetrii využití a pak doporučuje řešení, které vám pomůžou líp finanční efektivita, výkon, vysokou dostupnost a zabezpečení vašich prostředků Azure.

Advisor můžete:
* Získejte proaktivní, kterého lze provést akci a přizpůsobené osvědčené postupy a doporučení. 
* Zvýšit výkon, zabezpečení a vysokou dostupnost vašich prostředků, jak identifikovat příležitosti k snížit vaše celkové Azure tráví.
* Získejte doporučení s vložené navrhovaná akce.

Dostanete Advisor prostřednictvím [portál Azure](https://aka.ms/azureadvisordashboard). Přihlaste se k [portál](https://portal.azure.com), vyberte **Procházet**a potom přejděte k **Azure Advisor**. Řídicí panel Advisor zobrazuje přizpůsobené doporučení pro vybrané předplatné. 

Doporučení jsou rozděleny do čtyř kategorií: 

* **Vysoká dostupnost**: K zajištění a zlepšování kontinuity důležitými obchodními aplikacemi. Další informace najdete v tématu [vysokou dostupnost Advisor doporučení](advisor-high-availability-recommendations.md).

* **Zabezpečení**: ke zjištění hrozby a ohrožení zabezpečení, které mohou vést k narušení zabezpečení. Další informace najdete v tématu [doporučení zabezpečení Advisor](advisor-security-recommendations.md).

* **Výkon**: aby se zvýšila rychlost aplikací. Další informace najdete v tématu [Poradce pro výkon doporučení](advisor-performance-recommendations.md).

* **Náklady na**: optimalizace a snížit vaše celkové Azure tráví. Další informace najdete v tématu [doporučení služby Advisor náklady](advisor-cost-recommendations.md).

  ![Typy doporučení služby Advisor](./media/advisor-overview/advisor-all-tab-examples.png)

> [!NOTE]
> Chcete-li získat přístup k doporučení služby Advisor, je nutné nejprve *zaregistrovat předplatné* službou Advisor. Předplatné je zaregistrován při *předplatné vlastníka* spustí Advisor řídicího panelu a klikne na tlačítko **získat doporučení** tlačítko. Toto je *jednorázovou operaci*. Po registraci předplatného dostanete doporučení služby Advisor jako *vlastníka*, *Přispěvatel*, nebo *čtečky* pro předplatné, skupinu prostředků nebo konkrétní prostředek.

Můžete kliknout na doporučení Další informace o něm. Můžete si také přečíst o akcích, které můžete využít výhod příležitost nebo vyřešte problém. 

Advisor nabízí doporučení s vložené akce nebo odkazy na dokumentaci. Kliknutím na vložené akce vás provede "cesty s průvodcem uživatele" k implementaci. Kliknutím na odkaz dokumentace bodů dokumentaci, která popisuje, jak ručně implementovat akce. 

Advisor aktualizuje doporučení každou hodinu. Pokud nemáte v úmyslu provést okamžitou akci na doporučení, můžete zopakovat později pro zadané časové období nebo ho zavřít. 

## <a name="frequently-asked-questions"></a>Nejčastější dotazy

### <a name="how-do-i-access-advisor"></a>Přístupu Advisor
Dostanete Advisor prostřednictvím [portál Azure](https://aka.ms/azureadvisordashboard). Přihlaste se k [portál](https://portal.azure.com), vyberte **Procházet**a potom přejděte k **Azure Advisor**. Řídicí panel Advisor zobrazuje přizpůsobené doporučení pro vybrané předplatné. 

Doporučení služby Advisor můžete zobrazit také prostřednictvím okně prostředku virtuálního počítače. Vyberte virtuální počítač a poté přejděte k doporučení služby Advisor v nabídce. 

### <a name="what-permissions-do-i-need-to-access-advisor"></a>Uvedete, jaká oprávnění jsou nutné pro přístup k Advisor?

Chcete-li získat přístup k doporučení služby Advisor, je nutné nejprve *zaregistrovat předplatné* službou Advisor. Předplatné je zaregistrován při *předplatné vlastníka* spustí Advisor řídicího panelu a klikne na tlačítko **získat doporučení** tlačítko. Toto je *jednorázovou operaci*. Po registraci předplatného dostanete doporučení služby Advisor jako *vlastníka*, *Přispěvatel*, nebo *čtečky* pro předplatné, skupinu prostředků nebo konkrétní prostředek.

### <a name="how-often-are-advisor-recommendations-updated"></a>Jak často jsou doporučení služby Advisor aktualizovat?

Doporučení služby Advisor se aktualizují každou hodinu.

### <a name="what-resources-does-advisor-provide-recommendations-for"></a>Jaké prostředky poskytuje Advisor doporučení pro?

Advisor poskytuje doporučení pro virtuální počítače, skupiny dostupnosti, application Gateway, aplikační služby, servery SQL Server, databáze SQL a Redis Cache.

### <a name="can-i-snooze-or-dismiss-a-recommendation"></a>Můžete připomenout znovu nebo zrušit doporučení?

Připomenout znovu nebo zrušit doporučení, klikněte **připomenout znovu** tlačítko nebo odkaz. Můžete zadat dobu připomenutí období nebo vybrat možnost **nikdy** zrušíte doporučení.

## <a name="next-steps"></a>Další kroky

Další informace o doporučení služby Advisor najdete v tématu:

* [Začínáme se službou Advisor](advisor-get-started.md)
* [Doporučení pro vysokou dostupnost služby Advisor](advisor-high-availability-recommendations.md)
* [Doporučení zabezpečení Advisor](advisor-security-recommendations.md)
* [Poradce při hodnocení výkonu doporučení](advisor-performance-recommendations.md)
* [Náklady na doporučení služby Advisor](advisor-cost-recommendations.md)
