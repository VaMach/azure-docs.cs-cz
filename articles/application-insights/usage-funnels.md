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
ms.openlocfilehash: d7af89409cb908f98f86288a0d673ab287e3aaaa
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/01/2017
---
# <a name="discover-how-customers-are-using-your-application-with-the-application-insights-funnels"></a>Zjistit, jak zákazníci používají aplikace s nálevky statistiky aplikace

Principy zkušeností zákazníků je nesmírně důležité k vaší firmě. Pokud vaše aplikace zahrnuje několik fází, musíte vědět, pokud se většina zákazníků pokročíte prostřednictvím celý proces, nebo pokud jsou jejich ukončení procesu v určitém okamžiku. Průběh prostřednictvím sérii kroků ve webové aplikaci se označuje jako "trychtýřového grafu". Můžete nálevky Statistika aplikaci a získáte přehled o vašich uživatelů a monitorování podrobné převod sazby. 

## <a name="get-started-with-the-funnels-blade"></a>Začínáme s nálevky okna
Nejjednodušší způsob, jak získat informace o nálevky je provedeme příklad. Následující ilustrace ukazují vlastníci kroky elektronické obchodování bude trvat, než se dozvíte, jak zákazníkům komunikovat s touto webovou aplikací.  

### <a name="create-your-funnel"></a>Vytvoření vaší trychtýřového grafu
Než vytvoříte vaší trychtýřového grafu, musíte rozhodnout na otázku, kterou chcete odpovědět. Například můžete chtít vědět, jak mnoho zákazníků zobrazení vaší domovské stránce kliknutím na oznámení o inzerovaném programu. V tomto příkladu vlastníci společnosti Fabrikam Fiber zajímat, do jaké procento zákazníci, kteří nákup po přidání položky do jejich nákupní košík během poslední měsíc.

Tady jsou kroky, které budou chtít vytvořit jejich trychtýřového grafu.

1. Klikněte na tlačítko Nový v okně nálevky.
1. Vyberte časové rozmezí "Poslední měsíc" z **časový rozsah** rozevíracího seznamu. 
1. Vyberte **stránky produktu** událost z **kroku 1** rozevíracího seznamu. 
1. Vyberte **přidat nákupního košíku** událost z **kroku 2** rozevíracího seznamu.
1. Vyberte **kliknutím na nákup** událost z **krok 3** rozevíracího seznamu.
1. Přidat název do trychtýřového grafu a klikněte na tlačítko **Uložit**.

Následující obrázek ukazuje, že data v okně nálevky generuje. Tady Fabrikam můžete zobrazit vlastníky během posledního týdne dokončit 22.7 % zákazníků, kteří přidat položku do jejich nákupní košík nákupu. Můžete také zobrazit, 1 % zákazníkům před hostujících stránky produktu a 20 % zákazníkům odhlášení po dokončení jejich nákup kliknutí na oznámení o inzerovaném programu.


![Okno nálevky s daty](./media/app-insights-understand-usage-patterns/funnel1.png)

## <a name="next-steps"></a>Další kroky
  * [Přehled využití](app-insights-usage-overview.md)
  * [Uživatelů, relací a události](app-insights-usage-segmentation.md)
  * [Uchování](app-insights-usage-retention.md)
  * [Workbooks](app-insights-usage-workbooks.md)
  * [Přidat uživatelský kontext](app-insights-usage-send-user-context.md)
