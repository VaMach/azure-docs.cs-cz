---
title: "Aplikace Azure Statistika využití dopad | Microsoft docs"
description: "Analyzovat způsob různé vlastnosti potenciálně ovlivnit převod sazby za částí aplikace."
services: application-insights
documentationcenter: 
author: mrbullwinkle
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: multiple
ms.topic: article
ms.date: 01/25/2018
ms.author: mbullwin ; daviste
ms.openlocfilehash: d76db02647ce878343f60fc84cf063c5b7833438
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/09/2018
---
# <a name="impact-analysis-with-application-insights"></a>Analýza dopadu s Application Insights

Dopad analyzuje, jak časů načítání a dalších vlastností ovlivnit převod sazby pro různé části vaší aplikace. Přesněji uvést, zjistí jak **všechny dimenze** z **stránky zobrazení**, **vlastní události**, nebo **požadavku** ovlivňuje využití různé **stránky zobrazení** nebo **vlastní události**. 

![Nástroj dopad](./media/app-insights-usage-impact/0001-impact.png)

## <a name="still-not-sure-what-impact-does"></a>Ještě nejste si jistí dopad jaké?

Jedním ze způsobů zamyslet nad dopad je jako nástroj ultimate pro vyrovnání argumenty s někým ve vašem týmu o tom, jak ovlivňuje pomalost v některých aspektů vaší lokality zda zaujmout uživatele. Když uživatelé může tolerovat určité množství pomalost, dopad poskytuje přehled o jak nejlepší vyvážit optimalizace a výkonu chcete maximalizovat převod uživatele.

Ale Analýza výkonu je pouze podmnožinu dopad na možnosti. Vzhledem k tomu, že dopad podporuje vlastní události a dimenzí, zodpovězení otázek jako jak volba prohlížeče uživatele korelovat s různé sazby převod jsou právě několika kliknutí.

![Snímek obrazovky převodu pomocí prohlížeče](./media/app-insights-usage-impact/0004-browsers.png)

> [!NOTE]
> Prostředku Application Insights musí obsahovat zobrazení stránek nebo uživatelské události použití nástroje dopad. [Zjistěte, jak nastavit aplikaci shromažďovat zobrazení stránky automaticky pomocí Application Insights JavaScript SDK](app-insights-javascript.md). Také mějte na paměti, že od analýze korelace, záleží velikost vzorku.
>
>

## <a name="is-page-load-time-impacting-how-many-people-convert-on-my-page"></a>Je čas načítání stránky, které mají vliv kolik lidí převést na stránce?

Zahájíte odpovědi na otázky pomocí nástroje dopad zvolte zobrazení úvodní stránky, vlastní události nebo požadavku.

![Nástroj dopad](./media/app-insights-usage-impact/0002-dropdown.png)

1. Vyberte zobrazení stránky z **pro zobrazení stránky** rozevíracího seznamu.
2. Ponechte **analyzovat jak jeho** rozevírací na výchozí výběr **doba trvání** (v tomto kontextu **doba trvání** je alias **čas načítání stránky**.)
3. Pro **má dopad na používání** rozevíracího seznamu, vyberte vlastní události. Tato událost by měl odpovídat prvku uživatelského rozhraní na zobrazení stránky, které jste vybrali v kroku 1.

![Snímek obrazovky výsledky](./media/app-insights-usage-impact/0003-results.png)

U této instance jako **stránky produktu** čas načítání zvyšuje rychlost převodu do **nákupu produktu kliknutí na** ocitne mimo provoz. Na základě rozšíření výše, dobu trvání optimální stránky zatížení 3.5 sekund může použít k dosažení potenciální míra konverze 55 %. Další vylepšení výkonu snížit čas načítání níže 3.5 sekund není aktuálně korelovat s převod další výhody.

## <a name="what-if-im-tracking-page-views-or-load-times-in-custom-ways"></a>Co když I mě sledování zobrazení stránek nebo zatížení časy vlastní způsoby?

Dopad podporuje standardní a vlastní vlastnosti a měření. Použijte všechno. Místo doby trvání použijte na primární a sekundární události získat podrobnější filtry.

## <a name="do-users-from-different-countries-or-regions-convert-at-different-rates"></a>Různými rychlostmi převést uživatele z různých zemí nebo oblastí?

1. Vyberte zobrazení stránky z **pro zobrazení stránky** rozevíracího seznamu.
2. Vyberte "Země nebo oblast," v **analyzovat jak jeho** rozevíracího seznamu
3. Pro **má dopad na používání** rozevíracího seznamu, vyberte vlastní událost, která odpovídá prvku uživatelského rozhraní na zobrazení stránky jste zvolili v kroku 1.

V takovém případě výsledky už začlenit do modelu průběžné osy x stejným způsobem jako v prvním příkladu. Místo toho se zobrazí podobná segmentovaným trychtýřového grafu vizualizace. Řadit podle **využití** zobrazíte variantu převod na vlastní událost na základě země.


## <a name="how-does-the-impact-tool-calculate-these-conversion-rates"></a>Jak nástroj dopad vypočítat tyto rychlosti převod?

Pod pokličkou, využívá nástroj dopad [Pearsonův korelace koeficient] (https://en.wikipedia.org/wiki/Pearson_correlation_coefficient). Výsledky se vypočítávají v rozsahu -1 a 1 hodnotou -1 představující nulové korelace a 1 představující kladné korelace.

Základní rozpis toho, jak funguje analýza dopadu vypadá takto:

Umožní _A_ = hlavní zobrazení nebo vlastní události/požadavek na stránku, vyberte v první rozevíracího seznamu. (**Pro zobrazení stránky**).

Umožní _B_ = událostí sekundární stránky zobrazení nebo vlastní, vyberete (**má dopad na používání**).

Dopad vypadá v ukázce všechny relace od uživatelů v vybraný časový rozsah. Pro každou relaci, vypadá pro každý výskyt _A_.

Relace jsou poté rozdělen do dvou různých druhů z _subsessions_ na základě jedné z dvě podmínky:

- Převedený subsession se skládá z relace s _B_ událostí a zahrnuje všechny _A_ události, které nastaly před _B_.
- Nepřevedeném subsession dojde při všech _A_je dojít bez terminál _B_.

Nakonec výpočtu dopad se liší v závislosti na tom, jestli jsme se analýza metrika nebo dimenze. Pro všechny metriky _A_je v podrelaci zprůměrovány. Zatímco pro dimensions hodnota jednotlivých _A_ přispívá _1 nebo N_ na hodnotu přiřazenou _B_ kde _N_ je počet _A_je v podrelací.

## <a name="next-steps"></a>Další postup

- Pokud chcete povolit použití možnosti, zahájit odesílání [vlastních událostí](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#trackevent) nebo [stránky zobrazení](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#page-views).
- Pokud jste již odeslat vlastní události nebo zobrazení stránky, prozkoumejte využití nástroje se dozvíte, jak uživatelé používat služby.
    - [Trychtýře](usage-funnels.md)
    - [Uchování](app-insights-usage-retention.md)
    - [Toky uživatele](app-insights-usage-flows.md)
    - [Workbooks](app-insights-usage-workbooks.md)
    - [Přidat uživatelský kontext](app-insights-usage-send-user-context.md)