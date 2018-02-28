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
ms.openlocfilehash: a4096b11a828cf6676aa22b11c4dd4d75f3b0286
ms.sourcegitcommit: fbba5027fa76674b64294f47baef85b669de04b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/24/2018
---
# <a name="introduction-to-azure-advisor"></a>Úvod do Azure Advisor

Další informace o klíčových funkcí služby Azure Advisor a získejte odpovědi na nejčastější dotazy.

## <a name="what-is-advisor"></a>Co je Advisor?
Advisor je konzultantem přizpůsobené cloudu, která pomáhá dodržujte doporučené postupy, chcete-li optimalizovat nasazení Azure. Analyzuje konfiguraci vašich prostředků a telemetrii jejich využívání a následně doporučí řešení, která zlepší finanční úspornost, výkon, dostupnost a zabezpečení vašich prostředků Azure.

Advisor můžete:
* Získejte proaktivní, kterého lze provést akci a přizpůsobené osvědčené postupy a doporučení. 
* Zvýšit výkon, zabezpečení a vysokou dostupnost vašich prostředků, jak identifikovat příležitosti k snížit vaše celkové Azure tráví.
* Získejte doporučení s vložené navrhovaná akce.

Dostanete Advisor prostřednictvím [portál Azure](https://aka.ms/azureadvisordashboard). Přihlaste se k [portál](https://portal.azure.com), vyhledejte **Advisor** v navigační nabídce nebo vyhledejte ho **všechny služby** nabídky.

Řídicí panel Advisor zobrazuje přizpůsobené doporučení pro všechna předplatná.  Můžete použít filtry, které chcete zobrazit doporučení pro určité odběry a typy prostředků.  Doporučení jsou rozděleny do čtyř kategorií: 

* **Vysoká dostupnost**: K zajištění a zlepšování kontinuity důležitými obchodními aplikacemi. Další informace najdete v tématu [vysokou dostupnost Advisor doporučení](advisor-high-availability-recommendations.md).
* **Zabezpečení**: ke zjištění hrozby a ohrožení zabezpečení, které mohou vést k narušení zabezpečení. Další informace najdete v tématu [doporučení zabezpečení Advisor](advisor-security-recommendations.md).
* **Výkon**: aby se zvýšila rychlost aplikací. Další informace najdete v tématu [Poradce pro výkon doporučení](advisor-performance-recommendations.md).
* **Náklady na**: optimalizace a snížit celkové Azure výdajů. Další informace najdete v tématu [doporučení služby Advisor náklady](advisor-cost-recommendations.md).

  ![Typy doporučení služby Advisor](./media/advisor-overview/advisor-dashboard.png)

> [!NOTE]
> Pomocí nástroje Poradce pro Azure s předplatným, předplatné *vlastníka* musí spustit Poradce pro řídicí panel.  Tato akce registruje Poradce pro předplatné.  Od tohoto okamžiku na libovolné předplatné, *vlastníka*, *Přispěvatel*, nebo *čtečky* přístup doporučením Poradce pro předplatné. 

Můžete kliknutím na kategorii zobrazíte seznam doporučení v rámci této kategorie a vyberte doporučení Další informace o něm.  Můžete si také přečíst o akcích, které můžete využít výhod příležitost nebo vyřešte problém.

![Kategorie doporučení služby Advisor](./media/advisor-overview/advisor-ha-category-example.png) 

Vyberte doporučenou akci pro doporučení k implementaci doporučení.  Otevře se jednoduché rozhraní, které vám umožní provést doporučení nebo odkáže na dokumentaci, který vám pomůže s implementací.  Jakmile implementujete doporučení, může trvat až den pro Advisor, rozpoznat.

Pokud nemáte v úmyslu provádět okamžitou akci na doporučení, můžete odložit pro zadané časové období nebo ho zavřít.  Pokud nechcete dostávat doporučení pro konkrétní předplatné nebo skupinu prostředků, můžete nakonfigurovat Advisor pouze generovat doporučení pro zadané předplatné a skupiny prostředků.

## <a name="frequently-asked-questions"></a>Nejčastější dotazy

### <a name="how-do-i-access-advisor"></a>Přístupu Advisor
Dostanete Advisor prostřednictvím [portál Azure](https://aka.ms/azureadvisordashboard). Přihlaste se k [portál](https://portal.azure.com), vyhledejte **Advisor** v navigační nabídce nebo vyhledejte ho **všechny služby** nabídky.

Doporučení služby Advisor můžete zobrazit také prostřednictvím rozhraní prostředků virtuálního počítače. Vyberte virtuální počítač a poté přejděte k doporučení služby Advisor v nabídce. 

### <a name="what-permissions-do-i-need-to-access-advisor"></a>Uvedete, jaká oprávnění jsou nutné pro přístup k Advisor?

Pokud chcete získat doporučení Poradce pro předplatné, nejprve je nutné zaregistrovat předplatné službou Advisor. Předplatné je zaregistrován při předplatné *vlastníka* spustí Advisor řídicího panelu. Jedná se o jednorázovou operaci. Po registraci předplatného dostanete doporučení služby Advisor jako *vlastníka*, *Přispěvatel*, nebo *čtečky* předplatného.

### <a name="what-resources-does-advisor-provide-recommendations-for"></a>Jaké prostředky poskytuje Advisor doporučení pro?

Advisor poskytuje doporučení pro virtuální počítače, skupiny dostupnosti, application Gateway, aplikační služby, servery SQL Server, databáze SQL a Redis Cache.

### <a name="can-i-postpone-or-dismiss-a-recommendation"></a>Můžete odložit nebo zrušit doporučení?

Odložit nebo zrušit doporučení, klikněte **odložit** odkaz. Zadávat lze odložit období nebo vybrat možnost **nikdy** zrušíte doporučení.

## <a name="next-steps"></a>Další postup

Další informace o doporučení služby Advisor najdete v tématu:

* [Začínáme se službou Advisor](advisor-get-started.md)
* [Doporučení pro vysokou dostupnost služby Advisor](advisor-high-availability-recommendations.md)
* [Doporučení zabezpečení Advisor](advisor-security-recommendations.md)
* [Poradce při hodnocení výkonu doporučení](advisor-performance-recommendations.md)
* [Náklady na doporučení služby Advisor](advisor-cost-recommendations.md)
