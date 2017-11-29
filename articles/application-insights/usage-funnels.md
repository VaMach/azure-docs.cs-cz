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
ms.openlocfilehash: bbb25af888f34737f6a61cf43890ff248c4cc4de
ms.sourcegitcommit: cf42a5fc01e19c46d24b3206c09ba3b01348966f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/29/2017
---
# <a name="discover-how-customers-are-using-your-application-with-application-insights-funnels"></a>Zjistit, jak zákazníci používají aplikace s nálevky Application Insights

Porozumění zkušeností zákazníků, je nesmírně důležité k vaší firmě. Pokud vaše aplikace zahrnuje několik fází, budete muset vědět, pokud se většina zákazníků pokročíte prostřednictvím celý proces, nebo pokud jsou jejich ukončení procesu v určitém okamžiku. Průběh prostřednictvím sérii kroků ve webové aplikaci se označuje jako *trychtýřového grafu*. Můžete použít Azure Application Insights nálevky a získáte přehled o uživatelům a monitorovat podrobné převod sazby. 

## <a name="create-your-funnel"></a>Vytvoření vaší trychtýřového grafu
Před vytvořením vaší trychtýřového grafu rozhodnete na otázku, kterou chcete odpovědět. Například můžete chtít vědět, kolik uživatelů prohlížíte domovské stránce zobrazení profil zákazníka a vytvoření lístku. V tomto příkladu vlastníci společnosti Fabrikam Fiber zajímat, do jaké procento zákazníci, kteří úspěšně vytvořit lístek zákazníka.

Tady jsou kroky, které budou chtít vytvořit jejich trychtýřového grafu.

1. V nástroji Application Insights nálevky vyberte **nový**.
1. Z **časový rozsah** rozevírací nabídky vyberte **posledních 90 dní**. Vyberte buď **Moje nálevky** nebo **sdílené nálevky**.
1. Z **kroku 1** rozevíracího seznamu vyberte **Index**. 
1. Z **kroku 2** seznamu, vyberte **zákazníka**.
1. Z **krok 3** seznamu, vyberte **vytvořit**.
1. Přidat název do trychtýřového grafu a vyberte **Uložit**.

Následující snímek obrazovky ukazuje, že příklad druhu dat nálevky Nástroj generuje. Vlastníci společnosti Fabrikam uvidíte, že během posledních 90 dnů, 54.3 procent svým zákazníkům, kteří navštívili na domovskou stránku vytvořit lístek zákazníka. Můžete také zjistit, že 2,700 zákazníků pocházejí z domovské stránky do indexu. To může znamenat problém s aktualizací zobrazení.


![Snímek obrazovky nálevky nástroj s daty](./media/app-insights-understand-usage-patterns/funnel1.png)

### <a name="funnels-features"></a>Funkce nálevky
Na předchozím snímku obrazovky zahrnuje pět zvýrazněné oblasti. Tyto jsou funkce nálevky. Následující seznam popisuje více o jednotlivých odpovídající oblast na snímku obrazovky:
1. Pokud vaše aplikace je vzorků, zobrazí se banner vzorkování. Výběr Banner informující o, otevře se podokno kontextu, která vysvětluje, jak vypnout vzorkování. 
2. Můžete exportovat vaší trychtýřového grafu do [Power BI](app-insights-export-power-bi.md).
3. Vyberte krok zobrazíte další podrobnosti na pravé straně. 
4. Historické převod graf zobrazuje převod sazby za posledních 90 dnů. 
5. Pochopte lepší uživatelům přístup k nástroj Uživatelé. Pomocí filtrů v jednotlivých kroků. 

## <a name="next-steps"></a>Další kroky
  * [Přehled využití](app-insights-usage-overview.md)
  * [Uživatelů, relací a události](app-insights-usage-segmentation.md)
  * [Uchování](app-insights-usage-retention.md)
  * [Workbooks](app-insights-usage-workbooks.md)
  * [Přidat uživatelský kontext](app-insights-usage-send-user-context.md)
  * [Export do Power BI](app-insights-export-power-bi.md)

