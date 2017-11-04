---
title: "Analýza uživatelů, relací a událostí ve službě Azure Application Insights | Microsoft docs"
description: "Demografické údaje analýzy uživatelů vaší webové aplikace."
services: application-insights
documentationcenter: 
author: botatoes
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: multiple
ms.topic: article
ms.date: 05/03/2017
ms.author: mbullwin
ms.openlocfilehash: df707f2c54ce6d4fd48490085b182ad16d7cfaed
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/01/2017
---
# <a name="users-sessions-and-events-analysis-in-application-insights"></a>Analýza uživatelů, relací a událostí ve službě Application Insights

Zjistěte, kdy lidí používá vaši webovou aplikaci, jaké stránky, budou se nejvíce zajímá, kde se nachází vaši uživatelé, jaké prohlížeče a operační systémy používají. Analyzovat obchodní a využití telemetrie pomocí [Azure Application Insights](app-insights-overview.md).

## <a name="get-started"></a>Začínáme

Pokud nevidíte ještě data uživatelů, relací nebo okna události na portálu služby Application Insights [zjistěte, jak začít pracovat s nástroji využití](app-insights-usage-overview.md).

## <a name="the-users-sessions-and-events-segmentation-tool"></a>Nástroj segmentaci uživatelů, relací a události

Tři z okna využití použít nástroj stejné k nařezání a rozčlenění telemetrie z vaší webové aplikace z tři perspektivy. Filtrování a rozdělení dat, můžete odkrýt přehledy o relativní použití různých stránek a funkcí.

* **Nástroj Uživatelé**: kolik lidí používá vaši aplikaci a jejich funkce.  Uživatelé, se počítají pomocí anonymní ID uložené v prohlížeči soubory cookie. Jednoho uživatele, kteří používají různé prohlížeče nebo počítače se budou počítat jako více než jeden uživatel.
* **Nástroj relací**: kolik relací aktivity uživatelů, zahrnuli určité stránky a funkce vaší aplikace. Relace se počítá po půl hodiny nečinnosti uživatele nebo po průběžné 24h použití.
* **Nástroj pro události**: jak často se používají určité stránky a funkce vaší aplikace. Zobrazení stránky se počítá při prohlížeč načte stránky z vaší aplikace, pokud máte [instrumentovány ho](app-insights-javascript.md). 

    Vlastní události představuje jeden výskyt něco stane v aplikaci, často zásah uživatele takto: klikněte na tlačítko nebo na dokončení některých úloh. Vložení kódu v aplikaci, kterou chcete [generovat vlastní události](app-insights-api-custom-events-metrics.md#trackevent).

![Použití nástroje](./media/app-insights-usage-segmentation/users.png)

## <a name="querying-for-certain-users"></a>Dotaz na určité uživatele 

Prozkoumejte různé skupiny uživatelů úpravou možností dotazu v horní části nástroj Uživatelé: 

* Kdo používá: Zvolte vlastní události a stránky zobrazení. 
* Během: Vyberte časový rozsah. 
* Podle: Volba způsobu bucket dat, buď v časovém intervalu, nebo jinou vlastnost třeba prohlížeče nebo města. 
* Rozdělit: Zvolte vlastnosti a které segmentu nebo rozdělení data. 
* Přidání filtrů: Omezte dotaz na určité uživatele, relace nebo události na základě jejich vlastností, jako je například prohlížeč nebo města. 
 
## <a name="saving-and-sharing-reports"></a>Ukládání a sdílení sestavy 
Uživatelé sestavy, privátní vám v části Moje sestavy nebo sdílené můžete uložit se všemi ostatními s přístupem k tomuto prostředku Application Insights v části sdílené sestavy.  
 
Při ukládání sestavy nebo úpravou jeho vlastnosti, vyberte "Aktuální relativní časové rozmezí" pro uložení sestavy budou průběžně aktualizovat data, návratem některé pevné množství času.  
 
Zvolte "Aktuální absolutní časové rozmezí" Uložit sestavu s pevnou sadu dat. Mějte na paměti, že data ve službě Application Insights je uložena pouze po dobu 90 dnů, takže pokud uplynulo víc než 90 dní od sestavy s rozsahem absolutním čase byla uložena, sestavy se zobrazí prázdné. 
 
## <a name="example-instances"></a>Příklad instancí

V části instancí příklad zobrazí informace o několik jednotlivých uživatelů, relací nebo událostí, které jsou porovnávány pomocí aktuálního dotazu. Vzhledem k tomu a zkoumat chování jednotlivce, kromě agregace, může poskytovat přehled o tom, jak lidé ve skutečnosti používají vaši aplikaci. 
 
## <a name="insights"></a>Insights 

Na bočním panelu Statistika ukazuje velkých clusterech uživatelů, které sdílejí společné vlastnosti. Tyto clustery odkrýt překvapivé trendy v tom, jak uživatelé používají vaši aplikaci. Pokud například 40 % všech využití aplikace pochází z uživatelé, kteří používají jeden funkce.  


## <a name="next-steps"></a>Další kroky
- Pokud chcete povolit použití možnosti, zahájit odesílání [vlastních událostí](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-api-custom-events-metrics#trackevent) nebo [stránky zobrazení](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#page-views).
- Pokud jste již odeslat vlastní události nebo zobrazení stránky, prozkoumejte využití nástroje se dozvíte, jak uživatelé používat služby.
    - [Trychtýře](usage-funnels.md)
    - [Uchování](app-insights-usage-retention.md)
    - [Toky uživatele](app-insights-usage-flows.md)
    - [Workbooks](app-insights-usage-workbooks.md)
    - [Přidat uživatelský kontext](app-insights-usage-send-user-context.md)

