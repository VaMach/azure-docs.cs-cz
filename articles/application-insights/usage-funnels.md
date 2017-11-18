---
title: "Nálevky Azure Application Insights"
description: "Zjistěte, jak nálevky můžete zjistit, jak jsou zákazníci interakci s vaší aplikací."
services: application-insights
documentationcenter: 
author: mrbullwinkle
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 07/17/2017
ms.author: mbullwin
ms.openlocfilehash: 0396c59d9d95ab71f0af04029d87afbb6e47dc35
ms.sourcegitcommit: 933af6219266cc685d0c9009f533ca1be03aa5e9
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/18/2017
---
# <a name="discover-how-customers-are-using-your-application-with-the-application-insights-funnels"></a>Zjistit, jak zákazníci používají aplikace s nálevky statistiky aplikace

Principy zkušeností zákazníků je nesmírně důležité k vaší firmě. Pokud vaše aplikace zahrnuje několik fází, musíte vědět, pokud se většina zákazníků pokročíte prostřednictvím celý proces, nebo pokud jsou jejich ukončení procesu v určitém okamžiku. Průběh prostřednictvím sérii kroků ve webové aplikaci se označuje jako "trychtýřového grafu". Můžete nálevky Statistika aplikaci a získáte přehled o vašich uživatelů a monitorování podrobné převod sazby. 

## <a name="create-your-funnel"></a>Vytvoření vaší trychtýřového grafu
Než vytvoříte vaší trychtýřového grafu, musíte rozhodnout na otázku, kterou chcete odpovědět. Například můžete chtít vědět, kolik uživatelů prohlížíte domovské stránce zobrazení profil zákazníka a vytvoření lístku. V tomto příkladu vlastníci společnosti Fabrikam Fiber zajímat, do jaké procento zákazníci, kteří úspěšně vytvořit lístek zákazníka.

Tady jsou kroky, které budou chtít vytvořit jejich trychtýřového grafu.

1. Klikněte na tlačítko Nový na nástroj nálevky.
1. Vyberte časové rozmezí "Posledních 90 dnů" z **časový rozsah** rozevíracího seznamu. Vyberte buď "Nálevky" nebo "Sdílené nálevky"
1. Vyberte **Index** událost z **kroku 1** rozevíracího seznamu. 
1. Vyberte **zákazníka** událost z **kroku 2** rozevíracího seznamu.
1. Vyberte **vytvořit** událost z **krok 3** rozevíracího seznamu.
1. Přidat název do trychtýřového grafu a klikněte na tlačítko **Uložit**.

Následující obrázek ukazuje, že data nálevky Nástroj generuje. Zde Fabrikam vlastníky najdete v během posledních 90 dnů, 54.3 % zákazníků, kteří navštívili domovské stránce vytvořený lístek zákazníka. Můžete také zobrazit, 2.7 tisíc zákazníků přišel do indexu z domovské stránky, může to znamenat problém s aktualizací zobrazení.


![Nástroj nálevky s daty](./media/app-insights-understand-usage-patterns/funnel1.png)

### <a name="funnel-features"></a>Trychtýřový funkce
1. Pokud vaše aplikace je vzorků, zobrazí se banner vzorkování. Klepnutím na informační zprávě otevřete podokno kontextu instruující vypnutí vzorkování. 
2. Můžete exportovat vaší trychtýřového grafu do [Power BI](app-insights-export-power-bi.md).
3. Klikněte na krok a získejte detailnější přehledy na pravé straně. 
4. Historické převod ukazuje převod za posledních 90 dnů. 
5. Vaši uživatelé lepší pochopení přechodem na nástroje Uživatelé z nálevky. Každý krok získáte filtry uživatelé vám kurátorované. 

## <a name="next-steps"></a>Další kroky
  * [Přehled využití](app-insights-usage-overview.md)
  * [Uživatelů, relací a události](app-insights-usage-segmentation.md)
  * [Uchování](app-insights-usage-retention.md)
  * [Workbooks](app-insights-usage-workbooks.md)
  * [Přidat uživatelský kontext](app-insights-usage-send-user-context.md)
  * [Export do Power BI](app-insights-export-power-bi.md)

