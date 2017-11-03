---
title: "Analýza vzory navigační uživatele s toky uživatele ve službě Azure Application Insights | Microsoft docs"
description: "Analýza, jak uživatelé přecházejí mezi stránkami a funkce vaší webové aplikace."
services: application-insights
documentationcenter: 
author: numberbycolors
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: multiple
ms.topic: article
ms.date: 08/02/2017
ms.author: cfreeman
ms.openlocfilehash: d17ed3dff08f00a1d6a2108608e42b29f95fbd84
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="analyze-user-navigation-patterns-with-user-flows-in-application-insights"></a>Analýza vzory navigační uživatele s toky uživatele ve službě Application Insights

![Nástroj Přehled toků uživatele aplikace](./media/app-insights-usage-flows/flows.png)

Nástroj toků uživatele vizualizuje, jak uživatelé přecházejí mezi stránkami a funkce vašeho webu. Služba je skvělá pro odpovědi na otázky jako:
* Jak opustit uživatelům a na stránce na váš web?
* Co uživatelé klikněte na stránku na webu?
* Kde jsou nejvíce místa, které uživatelé změn z vaší lokality?
* Existují míst, kde uživatelé opakovaně opakováním stejné akce?

Nástroj toků uživatel spustí z úvodní stránky zobrazení nebo událostí, který určíte. Danou tato zobrazení stránky nebo vlastní události, tok uživatele se zobrazuje zobrazení stránek a vlastních událostí, které uživatelé odeslány okamžitě později během relace, dva kroky později, a tak dále. Řádky různých tloušťky zobrazit více než jednou. Každá cesta následovalo uživatele. Speciální uzly "Relace skončila" zobrazit, kolik uživatelů odeslaných žádné zobrazení stránek nebo uživatelské události po předchozím uzlu zvýraznění, kde uživatelé pravděpodobně zbývajících vaší lokality.



> [!NOTE]
> Prostředku Application Insights musí obsahovat zobrazení stránek nebo uživatelské události použití nástroje toků uživatele. [Zjistěte, jak nastavit aplikaci shromažďovat zobrazení stránky automaticky pomocí Application Insights JavaScript SDK](app-insights-javascript.md).
> 
> 

## <a name="start-by-choosing-an-initial-page-view-or-custom-event"></a>Začněte tím, že vyberete zobrazení úvodní stránky nebo vlastní události

![Zvolte událost počáteční toky uživatele](./media/app-insights-usage-flows/flows-initial-event.png)

Abyste mohli začít odpovědi na otázky pomocí nástroje toků uživatele, vyberte zobrazení úvodní stránky nebo vlastní událost, která má sloužit jako výchozí bod pro vizualizaci:
1. Klikněte na odkaz "co uživatelé udělat po...?" Title, nebo klikněte na tlačítko Upravit. 
2. Vyberte zobrazení stránky nebo vlastní události z rozevíracího seznamu "Počáteční událost".
3. Klikněte na tlačítko "Vytvoření grafu".

Sloupec "Krok 1" vizualizaci znázorňuje, co uživatelé se nejčastěji jenom po počáteční události, seřazené horní dolů z nejčastěji na nejmenší dosažený. "Krok 2" a dalších sloupců zobrazit, co uživatelé se následně vytváření obrázku všech způsoby, jak uživatelé přešli svého webu.

Ve výchozím nastavení nástroj uživatele toků náhodně ukázky pouze posledních 24 hodin zobrazení stránky a vlastní události z vaší lokality. Můžete zvýšit časový rozsah a změnit vyrovnání výkonu a přesnost pro náhodné vzorkování v nabídce Upravit.

Pokud některé zobrazení stránek a vlastních událostí nejsou relevantní pro vás, klikněte na symbol "X" na uzlech, které chcete skrýt. Po dokončení výběru uzly, které chcete skrýt, klikněte na tlačítko "Vytvoření grafu" vizualizaci. Zobrazit všechny uzly, skrytých, klikněte na tlačítko Upravit a potom podívejte se na části "Vyloučené události".

Pokud zobrazení stránek nebo uživatelské události chybí, byste měli vidět na vizualizaci:
* Zkontrolujte v části "Vyloučené události" v nabídce Upravit.
* Použití ovládacího prvku "Úroveň podrobností" v nabídce Upravit mají být zahrnuty méně časté události vizualizaci.
* Pokud se zobrazení stránky nebo vlastní události, které očekáváte, že je odeslat zřídka uživatelé, pokuste se zvýšit časový rozsah vizualizace v nabídce Upravit.
* Ujistěte se, že stránky zobrazení nebo vlastní události, které jsou nastaveny na shromáždit pomocí Application Insights SDK ve zdrojovém kódu vašeho webu. [Další informace o shromažďování vlastních událostí.](app-insights-api-custom-events-metrics.md)

Pokud chcete zobrazit další kroky v vizualizaci, posuvníkem "Počet kroků" v nabídce Upravit.

## <a name="after-visiting-a-page-or-feature-where-do-users-go-and-what-do-they-click"></a>Po přečtení informací na stránky nebo funkce, které uživatelé navštěvují a co kliknou?

![Použití uživatele toků zjistit, kde uživatelé kliknou na](./media/app-insights-usage-flows/flows-one-step.png)

Pokud je vaše počáteční událost zobrazení stránky, první sloupec ("krok 1) tato vizualizace je rychlý způsob, jak porozumět, co uživatelé se okamžitě po přečtení informací na stránce. Zkuste otevřít váš web v okně vedle vizualizace toků uživatele. Porovnejte vaše očekávání jak uživatelé komunikovat s stránky seznam událostí ve sloupci "Krok 1". Element uživatelského rozhraní na stránce, která vypadá zanedbatelný si s týmem často, může být mezi nejčastěji používané na stránce. Může být skvělý výchozí bod pro návrh vylepšení na váš web.

Pokud je vaše počáteční událost vlastní události, první sloupec zobrazuje, co uživatelé se jenom po provedení této akce. Stejně jako u zobrazení stránky, zvažte, jestli odpovídá zjištěnou chování uživatelů cílů a očekávání vašeho týmu. Pokud je vaše vybrané počáteční událost "Přidání položek do nákupního košíku", například vypadat a zjistěte, zda v vizualizaci krátce po tomto datu se zobrazí "Přejděte na najdete v článku věnovaném" a "Dokončení nákupu". Pokud chování uživatelů se výrazně liší od vašim požadavkům, použijte vizualizaci pochopit, jak jsou uživatelé získávání "zachytí" aplikace aktuální návrh vašeho webu.

## <a name="where-are-the-places-that-users-churn-most-from-your-site"></a>Kde jsou nejvíce místa, které uživatelé změn z vaší lokality?

Sledovat "Relace skončila" uzlů, které zobrazují vysokou up ve sloupci v vizualizace, zejména časná v toku. To znamená mnoho uživatelů pravděpodobně churned z vaší lokality po následující výše uvedená cesta stránek a interakce uživatelského rozhraní. Někdy se očekává – po dokončení nákupu na stránku elektronické obchodování například - ale obvykle změn je znaménkem návrhu problémy, nízký výkon nebo jiných problémů s váš web, který je možné zlepšit.

Mít na paměti, že "relace skončila" uzly jsou pouze podle telemetrická data shromažďovaná společností tento prostředek Application Insights. Pokud Application Insights neobdrží telemetrie pro určité uživatelské interakce, uživatelé může stále mít zpracoval s webem tyto způsoby po nástroj uživatele toků uvádí relace ukončena.

## <a name="are-there-places-where-users-repeat-the-same-action-over-and-over"></a>Existují míst, kde uživatelé opakovaně opakováním stejné akce?

Podívejte se na zobrazení stránky nebo vlastní událost, která se opakuje mnoha uživateli v dalších krocích vizualizaci. To obvykle znamená, že uživatelé fungují opakovaných akcí na váš web. Pokud zjistíte opakování, vezměte v úvahu Změna návrhu vaší lokality nebo přidání nových funkcí ke snížení opakování. Například přidání hromadné úpravy funkce, pokud zjistíte, uživatelé provádění opakovaných akcí na každý řádek table element.

## <a name="common-questions"></a>Nejčastější dotazy

### <a name="why-do-steps-appear-disconnected"></a>Proč kroky zobrazí odpojené?

![Uživatel toků se odpojené kroky](./media/app-insights-usage-flows/flows-disconnected.png)

Odpojení kroků (sloupce) v vizualizace toků uživatele znamená, že žádná z cest provedenou uživatelů mezi kroky byly dostatečně častý na to, která se má zobrazit. V vizualizaci zobrazit méně častá událostí, takže kroky se zobrazí připojené, nastavte jezdec "Úroveň podrobností" v nabídce Upravit.

### <a name="does-the-initial-event-represent-the-first-time-the-event-appears-in-a-session-or-any-time-it-appears-in-a-session"></a>Podporuje představují počáteční událost při prvním události se zobrazí v relaci, nebo kdykoli, který se zobrazuje v relaci?

Počáteční událost na vizualizaci pouze představuje první čas, kdy uživatel odeslaných tímto zobrazení stránky nebo vlastní události během relace. Pokud uživatelé můžete odeslat počáteční událost více než jednou. v relaci, pak sloupci "Krok 1" se zobrazí pouze chování uživatele po *první* instanci počáteční události, ne všechny instance.

## <a name="next-steps"></a>Další kroky

* [Přehled využití](app-insights-usage-overview.md)
* [Uživatelů, relací a události](app-insights-usage-segmentation.md)
* [Uchování](app-insights-usage-retention.md)
* [Přidání vlastních událostí do vaší aplikace](app-insights-api-custom-events-metrics.md)