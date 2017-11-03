---
title: "Součástí odkaz pro Návrhář zobrazení v OMS Log Analytics | Microsoft Docs"
description: "Návrhář zobrazení v analýzy protokolů umožňuje vytvářet vlastní zobrazení v konzole OMS, které obsahují různé vizualizace dat v úložišti OMS. Tento článek obsahuje odkaz nastavení pro jednotlivé části vizualizace k dispozici pro použití ve vaší vlastní zobrazení."
services: log-analytics
documentationcenter: 
author: bwren
manager: jwhit
editor: 
ms.assetid: 5718d620-b96e-4d33-8616-e127ee9379c4
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/17/2017
ms.author: bwren
ms.openlocfilehash: 40a6101576708936404447576d704a49666143fe
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="log-analytics-view-designer-visualization-part-reference"></a>Odkazy na protokol Návrhář zobrazení Analytics vizualizace část
Návrhář zobrazení v analýzy protokolů umožňuje vytvářet vlastní zobrazení v konzole OMS, které obsahují různé vizualizace dat z úložiště OMS. Tento článek obsahuje odkaz nastavení pro jednotlivé části vizualizace k dispozici pro použití ve vaší vlastní zobrazení.

Další články, které jsou k dispozici pro Návrhář zobrazení jsou:

* [Zobrazit návrháře](log-analytics-view-designer.md) -přehled Návrhář zobrazení a postupy pro vytváření a úpravy vlastních zobrazení.
* [Odkaz na dlaždici](log-analytics-view-designer-tiles.md) – odkaz na nastavení pro každou z dlaždice, které jsou k dispozici pro použití ve vaší vlastní zobrazení.

>[!NOTE]
> Pokud pracovní prostor byl upgradován na verzi [nové protokolu Analytics query language](log-analytics-log-search-upgrade.md), pak dotazů ve všech zobrazeních, musí být napsaná [nové dotazovací jazyk](https://go.microsoft.com/fwlink/?linkid=856078).  Všechna zobrazení, které byly vytvořeny před byl upgradován pracovním prostoru bude automtically převést.

Následující tabulka popisuje různé typy dlaždice, které jsou k dispozici v Návrháři zobrazení.  Následující oddíly popisují každý typ dlaždice v podrobností a jejich vlastnosti.

| Typ zobrazení | Popis |
|:--- |:--- |
| [Seznam dotazů](#list-of-queries-part) |Zobrazí seznam protokolů vyhledávací dotazy.  Uživatel kliknutím na každý dotaz zobrazíte jeho výsledky. |
| [Počet & seznamu](#number-amp-list-part) |Hlavička obsahuje jeden číslo zobrazuje počet záznamů z protokolu vyhledávací dotaz.  Seznam zobrazí top deset výsledky z dotazu s grafické vyjádření relativní hodnota je číselný sloupec nebo jeho změn v průběhu času. |
| [Dvě čísla & seznamu](#two-numbers-amp-list-part) |Hlavička obsahuje dvě čísla zobrazující počet záznamy ze samostatné protokolu vyhledávací dotazy.  Seznam zobrazí top deset výsledky z dotazu s grafické vyjádření relativní hodnota je číselný sloupec nebo jeho změn v průběhu času. |
| [Prstenec & seznamu](#donut-amp-list-part) |Hlavička zobrazí jedno číslo souhrnu ze hodnotu sloupce v protokolu dotazu.  Prstenec graficky zobrazí výsledky nejvyšší tři záznamy. |
| [Dva časové osy & seznamu](#two-timelines-amp-list-part) |Záhlaví zobrazí výsledky dva dotazy protokolu časem jako sloupcové grafy s popisek zobrazení souhrnu ze hodnotu sloupce v dotazu protokolu jedno číslo.  Seznam zobrazí top deset výsledky z dotazu s grafické vyjádření relativní hodnota je číselný sloupec nebo jeho změn v průběhu času. |
| [Informace o](#information-part) |Záhlaví obsahuje statický text a nepovinný odkaz.  Zobrazí se seznam jedné nebo více položek s statický text a název. |
| [Spojnicový graf, popisku & seznamu](#line-chart-callout-amp-list-part) |Hlavička zobrazuje spojnicový graf s více řad z protokolu dotazu přes čas a popisku s souhrnnou hodnotu.  Seznam zobrazí top deset výsledky z dotazu s grafické vyjádření relativní hodnota je číselný sloupec nebo jeho změn v průběhu času. |
| [Seznam & Spojnicový graf](#line-chart-amp-list-part) |Hlavička zobrazí spojnicový graf s více řad z protokolu dotazu v čase.  Seznam zobrazí top deset výsledky z dotazu s grafické vyjádření relativní hodnota je číselný sloupec nebo jeho změn v průběhu času. |
| [Zásobník části grafy řádku](#stack-of-line-charts-part) |Zobrazuje tři samostatné spojnicových grafů s více řad z protokolu dotazu v čase. |

## <a name="list-of-queries-part"></a>Seznam dotazů část
Zobrazí seznam protokolů vyhledávací dotazy.  Uživatel kliknutím na každý dotaz zobrazíte jeho výsledky.  Zobrazení bude obsahovat jeden dotaz ve výchozím nastavení, a můžete kliknout na **+ dotazu** přidat další dotazy.

![Seznam zobrazení dotazů](media/log-analytics-view-designer/view-list-queries.png)

| Nastavení | Popis |
|:--- |:--- |
| **Obecné** | |
| Název |Text, který se zobrazí v horní části zobrazení. |
| Nové skupiny |Vyberte, chcete-li vytvořit novou skupinu v zobrazení začínající na aktuální zobrazení. |
| Předvybrané filtry |Čárkami oddělený seznam vlastností, které chcete zahrnout do filtru v levém podokně, když uživatel vybere dotazu. |
| V režimu |Počáteční zobrazení zobrazí, když je vybraný dotaz.  Uživatele můžete vybrat všechny dostupné zobrazení po otevření dotazu. |
| **Dotazy** | |
| Vyhledávací dotaz. |Dotaz spustit. |
| Popisný název |Popisný název tohoto dotazu zobrazit uživateli. |

## <a name="number--list-part"></a>Počet & seznam součástí
Hlavička obsahuje jeden číslo zobrazuje počet záznamů z protokolu vyhledávací dotaz.  Seznam zobrazí top deset výsledky z dotazu s grafické vyjádření relativní hodnota je číselný sloupec nebo jeho změn v průběhu času.

![Seznam zobrazení dotazů](media/log-analytics-view-designer/view-number-list.png)

| Nastavení | Popis |
|:--- |:--- |
| **Obecné** | |
| Název skupiny |Text, který se zobrazí v horní části zobrazení. |
| Nové skupiny |Vyberte, chcete-li vytvořit novou skupinu v zobrazení začínající na aktuální zobrazení. |
| Ikona |Soubor bitové kopie, který se zobrazí vedle výsledek v hlavičce. |
| Použití ikony |Vyberte ikonu zobrazení. |
| **Název** | |
| Legendy |Text, který se zobrazí v horní části záhlaví. |
| Dotaz |Dotaz pro hlavičku.  Zobrazí se počet záznamů vrácených dotazem. |
| **Seznam** | |
| Dotaz |Dotaz pro seznamu.  Zobrazí se první dva vlastnosti prvních deset záznamů ve výsledcích.  První vlastností by měl být textovou hodnotu a druhou vlastností číselná hodnota.  Řádky se vytvářejí automaticky podle relativní hodnota číselné sloupce.<br><br>Příkaz řazení v dotazu seřadit záznamy v seznamu.  Uživatel může kliknout na najdete všechny spuštěním dotazu a vrátíte se všechny záznamy. |
| Skrýt grafu |Vyberte, chcete-li zakázat graf tak, aby právo číselné sloupce. |
| Povolit minigrafů |Vyberte, chcete-li zobrazit minigraf místo vodorovném řádku.  V tématu [společná nastavení](#sparklines) podrobnosti. |
| Barva |Barva řádky nebo minigrafů. |
| Název & hodnota oddělovače |Pokud chcete analyzovat vlastnost text do více hodnot jeden znak oddělovač.  V tématu [společná nastavení](#name-value-separator) podrobnosti. |
| Navigace dotazu |Dotaz spustit, když uživatel vybere položku v seznamu.  V tématu [společná nastavení](#navigation-query) podrobnosti. |
| **Seznam** |**> Názvy sloupce** |
| Name (Název) |Text, který se zobrazí v horní části první sloupec v seznamu. |
| Hodnota |Text, který se zobrazí v horní části druhý sloupec v seznamu. |
| **Seznam** |**> Prahové hodnoty** |
| Povolit prahové hodnoty |Vyberte, chcete-li povolit prahové hodnoty.  V tématu [společná nastavení](#thresholds) podrobnosti. |

## <a name="two-numbers--list-part"></a>Dvou čísel & část Seznam
Hlavička obsahuje dvě čísla zobrazující počet záznamy ze samostatné protokolu vyhledávací dotazy.  Seznam zobrazí top deset výsledky z dotazu s grafické vyjádření relativní hodnota je číselný sloupec nebo jeho změn v průběhu času.

![Dvou čísel a zobrazení seznamu](media/log-analytics-view-designer/view-two-numbers-list.png)

| Nastavení | Popis |
|:--- |:--- |
| **Obecné** | |
| Název skupiny |Text, který se zobrazí v horní části zobrazení. |
| Nové skupiny |Vyberte, chcete-li vytvořit novou skupinu v zobrazení začínající na aktuální zobrazení. |
| Ikona |Soubor bitové kopie, který se zobrazí vedle výsledek v hlavičce. |
| Použití ikony |Vyberte ikonu zobrazení. |
| **Název** | |
| Legendy |Text, který se zobrazí v horní části záhlaví. |
| Dotaz |Dotaz pro hlavičku.  Zobrazí se počet záznamů vrácených dotazem. |
| **Seznam** | |
| Dotaz |Dotaz pro seznamu.  Zobrazí se první dva vlastnosti prvních deset záznamů ve výsledcích.  První vlastností by měl být textovou hodnotu a druhou vlastností číselná hodnota.  Řádky se vytvářejí automaticky podle relativní hodnota číselné sloupce.<br><br>Příkaz řazení v dotazu seřadit záznamy v seznamu.  Uživatel může kliknout na najdete všechny spuštěním dotazu a vrátíte se všechny záznamy. |
| Skrýt grafu |Vyberte, chcete-li zakázat graf tak, aby právo číselné sloupce. |
| Povolit minigrafů |Vyberte, chcete-li zobrazit minigraf místo vodorovném řádku.  V tématu [společná nastavení](#sparklines) podrobnosti. |
| Barva |Barva řádky nebo minigrafů. |
| Operace |Operace k provedení pro minigraf.  V tématu [společná nastavení](#sparklines) podrobnosti. |
| Název & hodnota oddělovače |Pokud chcete analyzovat vlastnost text do více hodnot jeden znak oddělovač.  V tématu [společná nastavení](#name-value-separator) podrobnosti. |
| Navigace dotazu |Dotaz spustit, když uživatel vybere položku v seznamu.  V tématu [společná nastavení](#navigation-query) podrobnosti. |
| **Seznam** |**> Názvy sloupce** |
| Name (Název) |Text, který se zobrazí v horní části první sloupec v seznamu. |
| Hodnota |Text, který se zobrazí v horní části druhý sloupec v seznamu. |
| **Seznam** |**> Prahové hodnoty** |
| Povolit prahové hodnoty |Vyberte, chcete-li povolit prahové hodnoty.  V tématu [společná nastavení](#thresholds) podrobnosti. |

## <a name="donut--list-part"></a>Část prstenec & seznamu
Hlavička zobrazí jedno číslo souhrnu ze hodnotu sloupce v protokolu dotazu.  Prstenec graficky zobrazí výsledky nejvyšší tři záznamy.

![Zobrazení prstenec & seznamu](media/log-analytics-view-designer/view-donut-list.png)

| Nastavení | Popis |
|:--- |:--- |
| **Obecné** | |
| Název skupiny |Text, který se zobrazí v horní části na dlaždici. |
| Nové skupiny |Vyberte, chcete-li vytvořit novou skupinu v zobrazení začínající na aktuální zobrazení. |
| Ikona |Soubor bitové kopie, který se zobrazí vedle výsledek v hlavičce. |
| Použití ikony |Vyberte ikonu zobrazení. |
| **Záhlaví** | |
| Název |Text, který se zobrazí v horní části záhlaví. |
| Subtitle |Text, který se zobrazí pod názvem v horní části záhlaví. |
| **Prstenec** | |
| Dotaz |Dotaz, aby běžel prstenec.  První vlastností by měl být textovou hodnotu a druhou vlastností číselná hodnota. |
| **Prstenec** |**> Center** |
| Text |Text, který se zobrazí v části hodnotu uvnitř prstenec. |
| Operace |Operace k plnění vlastnost value pro shrnutí jednu hodnotu.<br><br>-Součet: Přidejte hodnoty všech záznamů.<br>-Procento: Procento záznamy vrácené hodnoty v **způsobit hodnoty použít v operaci center** celkový počet záznamů v dotazu. |
| Výsledek hodnoty použít v operaci center |Volitelně klikněte na symbol plus přidat jednu nebo více hodnot.  Výsledky dotazu bude omezeno na záznamy s hodnotami vlastností, které zadáte.  Pokud budou přidávána žádné hodnoty, jsou zahrnuty všechny záznamy v dotazu. |
| **Další možnosti** |**> Barvy** |
| Barva 1<br>Barva 2<br>Barva 3 |Vyberte barvu pro každou z hodnoty zobrazené na prstenec. |
| **Další možnosti** |**> Mapování Upřesnit barev** |
| Hodnota pole |Zadejte název pole, které chcete zobrazit jako barvu, pokud je součástí prstenec. |
| Barva |Vyberte barvu pro pole jedinečný. |
| **Seznam** | |
| Dotaz |Dotaz pro seznamu.  Zobrazí se počet záznamů vrácených dotazem. |
| Skrýt grafu |Vyberte, chcete-li zakázat graf tak, aby právo číselné sloupce. |
| Povolit minigrafů |Vyberte, chcete-li zobrazit minigraf místo vodorovném řádku.  V tématu [společná nastavení](#sparklines) podrobnosti. |
| Barva |Barva řádky nebo minigrafů. |
| Operace |Operace k provedení pro minigraf.  V tématu [společná nastavení](#sparklines) podrobnosti. |
| Název & hodnota oddělovače |Pokud chcete analyzovat vlastnost text do více hodnot jeden znak oddělovač.  V tématu [společná nastavení](#name-value-separator) podrobnosti. |
| Navigace dotazu |Dotaz spustit, když uživatel vybere položku v seznamu.  V tématu [společná nastavení](#navigation-query) podrobnosti. |
| **Seznam** |**> Názvy sloupce** |
| Name (Název) |Text, který se zobrazí v horní části první sloupec v seznamu. |
| Hodnota |Text, který se zobrazí v horní části druhý sloupec v seznamu. |
| **Seznam** |**> Prahové hodnoty** |
| Povolit prahové hodnoty |Vyberte, chcete-li povolit prahové hodnoty.  V tématu [společná nastavení](#thresholds) podrobnosti. |

## <a name="two-timelines--list-part"></a>Dva časové osy & seznam součástí
Záhlaví zobrazí výsledky dva dotazy protokolu časem jako sloupcové grafy s popisek zobrazení souhrnu ze hodnotu sloupce v dotazu protokolu jedno číslo.  Seznam zobrazí top deset výsledky z dotazu s grafické vyjádření relativní hodnota je číselný sloupec nebo jeho změn v průběhu času.

![Dva časové osy & seznamu zobrazení](media/log-analytics-view-designer/view-two-timelines-list.png)

| Nastavení | Popis |
|:--- |:--- |
| **Obecné** | |
| Název skupiny |Text, který se zobrazí v horní části na dlaždici. |
| Nové skupiny |Vyberte, chcete-li vytvořit novou skupinu v zobrazení začínající na aktuální zobrazení. |
| Ikona |Soubor bitové kopie, který se zobrazí vedle výsledek v hlavičce. |
| Použití ikony |Vyberte ikonu zobrazení. |
| **Nejprve grafu<br>druhé grafu** | |
| Legendy |Text, který se zobrazí v části popisku řady, první. |
| Barva |Barva má být použita pro sloupce v řadě. |
| Dotaz |Dotaz pro první řady.  Počet záznamů v každém časovém intervalu budou odpovídat sloupcům grafu. |
| Operace |Operace k plnění vlastnost value pro shrnutí pro jednu hodnotu pro popisek.<br><br>-Součet: Součet hodnoty ze všech záznamů.<br>-Průměr: Průměr hodnoty ze všech záznamů.<br>-Posledního vzorku: Hodnota od posledního intervalu zahrnuté v grafu.<br>-První ukázka: Hodnota z první interval zahrnuté v grafu.<br>-Count: Počet všech záznamů vrácených dotazem. |
| **Seznam** | |
| Dotaz |Dotaz pro seznamu.  Zobrazí se počet záznamů vrácených dotazem. |
| Skrýt grafu |Vyberte, chcete-li zakázat graf tak, aby právo číselné sloupce. |
| Povolit minigrafů |Vyberte, chcete-li zobrazit minigraf místo vodorovném řádku.  V tématu [společná nastavení](#sparklines) podrobnosti. |
| Barva |Barva řádky nebo minigrafů. |
| Operace |Operace k provedení pro minigraf.  V tématu [společná nastavení](#sparklines) podrobnosti. |
| Navigace dotazu |Dotaz spustit, když uživatel vybere položku v seznamu.  V tématu [společná nastavení](#navigation-query) podrobnosti. |
| **Seznam** |**> Názvy sloupce** |
| Name (Název) |Text, který se zobrazí v horní části první sloupec v seznamu. |
| Hodnota |Text, který se zobrazí v horní části druhý sloupec v seznamu. |
| **Seznam** |**> Prahové hodnoty** |
| Povolit prahové hodnoty |Vyberte, chcete-li povolit prahové hodnoty.  V tématu [společná nastavení](#thresholds) podrobnosti. |

## <a name="information-part"></a>Část informace
Záhlaví obsahuje statický text a nepovinný odkaz.  Zobrazí se seznam jedné nebo více položek s statický text a název.

![Informace o zobrazení](media/log-analytics-view-designer/view-information.png)

| Nastavení | Popis |
|:--- |:--- |
| **Obecné** | |
| Název skupiny |Text, který se zobrazí v horní části na dlaždici. |
| Nové skupiny |Vyberte, chcete-li vytvořit novou skupinu v zobrazení začínající na aktuální zobrazení. |
| Barva |Barva pozadí pro hlavičku. |
| **Záhlaví** | |
| Image |Soubor bitové kopie k zobrazení v hlavičce. |
| Štítek |Text, který se zobrazí v záhlaví. |
| **Záhlaví** |**> Odkaz** |
| Štítek |Text odkazu. |
| URL |Adresa URL pro odkaz. |
| **Informace položky** | |
| Název |Text, který se zobrazí název každé položky. |
| Obsah |Text k zobrazení pro každou položku. |

## <a name="line-chart-callout--list-part"></a>Spojnicový graf, popisku & část Seznam
Hlavička zobrazuje spojnicový graf s více řad z protokolu dotazu přes čas a popisku s souhrnnou hodnotu.  Seznam zobrazí top deset výsledky z dotazu s grafické vyjádření relativní hodnota je číselný sloupec nebo jeho změn v průběhu času.

![Spojnicový graf, popisků a zobrazení seznamu](media/log-analytics-view-designer/view-line-chart-callout-list.png)

| Nastavení | Popis |
|:--- |:--- |
| **Obecné** | |
| Název skupiny |Text, který se zobrazí v horní části na dlaždici. |
| Nové skupiny |Vyberte, chcete-li vytvořit novou skupinu v zobrazení začínající na aktuální zobrazení. |
| Ikona |Soubor bitové kopie, který se zobrazí vedle výsledek v hlavičce. |
| Použití ikony |Vyberte ikonu zobrazení. |
| **Záhlaví** | |
| Název |Text, který se zobrazí v horní části záhlaví. |
| Subtitle |Text, který se zobrazí pod názvem v horní části záhlaví. |
| **Spojnicový graf** | |
| Dotaz |Dotaz pro spojnicový graf.  První vlastností by měl být textovou hodnotu a druhou vlastností číselná hodnota.  Toto je obvykle dotaz, který používá **měr** – klíčové slovo pro shrnutí výsledků.  Pokud dotaz používá **interval** – klíčové slovo pak osy x grafu budou používat tento časový interval.  Pokud dotaz neobsahuje **interval** – klíčové slovo a každou hodinu intervaly se používají pro osu x. |
| **Spojnicový graf** |**> Popisku** |
| Název popisku |Text k zobrazení větší než hodnota popisku. |
| Název řady |Hodnota vlastnosti řady, které chcete použít pro hodnotu popisku.  Pokud je k dispozici žádné řady, použijí se všechny záznamy z dotazu. |
| Operace |Operace k plnění vlastnost value pro shrnutí pro jednu hodnotu pro popisek.<br><br>-Průměr: Průměr hodnoty ze všech záznamů.<br>-Count počet všech záznamů vrácených dotazem.<br>-Posledního vzorku: Hodnota od posledního intervalu zahrnuté v grafu.<br>-Max: Maximální hodnota z intervalů zahrnuté v grafu.<br>-Min: Minimální hodnota z intervalů zahrnuté v grafu.<br>-Součet: Součet hodnoty ze všech záznamů. |
| **Spojnicový graf** |**> Osy Y** |
| Použít logaritmickou stupnici |Vyberte, chcete-li použít hodnota na logaritmické stupnici pro osu y. |
| Jednotky |Zadejte jednotky pro hodnoty vrácených dotazem.  Tyto informace slouží k zobrazení popisků v grafu označující typy hodnot a volitelně pro převod hodnoty.  Typ jednotky určuje kategorii jednotky a definuje aktuální Unit – typ hodnoty, které jsou k dispozici.  Pokud vyberete hodnotu v pak číselné hodnoty jsou převést z typu aktuální jednotku na převést na typ převeďte na. |
| Vlastní popisek |Text k zobrazení pro osy Y vedle popisek pro daný typ jednotky.  Pokud není zadaný žádný štítek, se zobrazí pouze typ jednotky. |
| **Seznam** | |
| Dotaz |Dotaz pro seznamu.  Zobrazí se počet záznamů vrácených dotazem. |
| Skrýt grafu |Vyberte, chcete-li zakázat graf tak, aby právo číselné sloupce. |
| Povolit minigrafů |Vyberte, chcete-li zobrazit minigraf místo vodorovném řádku.  V tématu [společná nastavení](#sparklines) podrobnosti. |
| Barva |Barva řádky nebo minigrafů. |
| Operace |Operace k provedení pro minigraf.  V tématu [společná nastavení](#sparklines) podrobnosti. |
| Název & hodnota oddělovače |Pokud chcete analyzovat vlastnost text do více hodnot jeden znak oddělovač.  V tématu [společná nastavení](#name-value-separator) podrobnosti. |
| Navigace dotazu |Dotaz spustit, když uživatel vybere položku v seznamu.  V tématu [společná nastavení](#navigation-query) podrobnosti. |
| **Seznam** |**> Názvy sloupce** |
| Name (Název) |Text, který se zobrazí v horní části první sloupec v seznamu. |
| Hodnota |Text, který se zobrazí v horní části druhý sloupec v seznamu. |
| **Seznam** |**> Prahové hodnoty** |
| Povolit prahové hodnoty |Vyberte, chcete-li povolit prahové hodnoty.  V tématu [společná nastavení](#thresholds) podrobnosti. |

## <a name="line-chart--list-part"></a>Část řádku seznamu & grafu
Hlavička zobrazí spojnicový graf s více řad z protokolu dotazu v čase.  Seznam zobrazí top deset výsledky z dotazu s grafické vyjádření relativní hodnota je číselný sloupec nebo jeho změn v průběhu času.

![Řádky – zobrazení seznamu & grafu](media/log-analytics-view-designer/view-line-chart-callout-list.png)

| Nastavení | Popis |
|:--- |:--- |
| **Obecné** | |
| Název skupiny |Text, který se zobrazí v horní části na dlaždici. |
| Nové skupiny |Vyberte, chcete-li vytvořit novou skupinu v zobrazení začínající na aktuální zobrazení. |
| Ikona |Soubor bitové kopie, který se zobrazí vedle výsledek v hlavičce. |
| Použití ikony |Vyberte ikonu zobrazení. |
| **Záhlaví** | |
| Název |Text, který se zobrazí v horní části záhlaví. |
| Subtitle |Text, který se zobrazí pod názvem v horní části záhlaví. |
| **Spojnicový graf** | |
| Dotaz |Dotaz pro spojnicový graf.  První vlastností by měl být textovou hodnotu a druhou vlastností číselná hodnota.  Toto je obvykle dotaz, který používá **měr** – klíčové slovo pro shrnutí výsledků.  Pokud dotaz používá **interval** – klíčové slovo pak osy x grafu budou používat tento časový interval.  Pokud dotaz neobsahuje **interval** – klíčové slovo a každou hodinu intervaly se používají pro osu x. |
| **Spojnicový graf** |**> Osy Y** |
| Použít logaritmickou stupnici |Vyberte, chcete-li použít hodnota na logaritmické stupnici pro osu y. |
| Jednotky |Zadejte jednotky pro hodnoty vrácených dotazem.  Tyto informace slouží k zobrazení popisků v grafu označující typy hodnot a volitelně pro převod hodnoty.  Typ jednotky určuje kategorii jednotky a definuje aktuální Unit – typ hodnoty, které jsou k dispozici.  Pokud vyberete hodnotu v pak číselné hodnoty jsou převést z typu aktuální jednotku na převést na typ převeďte na. |
| Vlastní popisek |Text k zobrazení pro osy Y vedle popisek pro daný typ jednotky.  Pokud není zadaný žádný štítek, se zobrazí pouze typ jednotky. |
| **Seznam** | |
| Dotaz |Dotaz pro seznamu.  Zobrazí se počet záznamů vrácených dotazem. |
| Skrýt grafu |Vyberte, chcete-li zakázat graf tak, aby právo číselné sloupce. |
| Povolit minigrafů |Vyberte, chcete-li zobrazit minigraf místo vodorovném řádku.  V tématu [společná nastavení](#sparklines) podrobnosti. |
| Barva |Barva řádky nebo minigrafů. |
| Operace |Operace k provedení pro minigraf.  V tématu [společná nastavení](#sparklines) podrobnosti. |
| Název & hodnota oddělovače |Pokud chcete analyzovat vlastnost text do více hodnot jeden znak oddělovač.  V tématu [společná nastavení](#name-value-separator) podrobnosti. |
| Navigace dotazu |Dotaz spustit, když uživatel vybere položku v seznamu.  V tématu [společná nastavení](#navigation-query) podrobnosti. |
| **Seznam** |**> Názvy sloupce** |
| Name (Název) |Text, který se zobrazí v horní části první sloupec v seznamu. |
| Hodnota |Text, který se zobrazí v horní části druhý sloupec v seznamu. |
| **Seznam** |**> Prahové hodnoty** |
| Povolit prahové hodnoty |Vyberte, chcete-li povolit prahové hodnoty.  V tématu [společná nastavení](#thresholds) podrobnosti. |

## <a name="stack-of-line-charts-part"></a>Zásobník části grafy řádku
Zobrazuje tři samostatné spojnicových grafů s více řad z protokolu dotazu v čase.

![Zásobník spojnicových grafů](media/log-analytics-view-designer/view-stack-line-charts.png)

| Nastavení | Popis |
|:--- |:--- |
| **Obecné** | |
| Název skupiny |Text, který se zobrazí v horní části na dlaždici. |
| Nové skupiny |Vyberte, chcete-li vytvořit novou skupinu v zobrazení začínající na aktuální zobrazení. |
| Ikona |Soubor bitové kopie, který se zobrazí vedle výsledek v hlavičce. |
| **Grafu 1<br>grafu 2<br>grafu 3** |**> Záhlaví** |
| Název |Text, který se zobrazí v horní části grafu. |
| Subtitle |Text, který se zobrazí pod názvem v horní části grafu. |
| **Grafu 1<br>grafu 2<br>grafu 3** |**Spojnicový graf** |
| Dotaz |Dotaz pro spojnicový graf.  První vlastností by měl být textovou hodnotu a druhou vlastností číselná hodnota.  Toto je obvykle dotaz, který používá **měr** – klíčové slovo pro shrnutí výsledků.  Pokud dotaz používá **interval** – klíčové slovo pak osy x grafu budou používat tento časový interval.  Pokud dotaz neobsahuje **interval** – klíčové slovo a každou hodinu intervaly se používají pro osu x. |
| **Graf** |**> Osy Y** |
| Použít logaritmickou stupnici |Vyberte, chcete-li použít hodnota na logaritmické stupnici pro osu y. |
| Jednotky |Zadejte jednotky pro hodnoty vrácených dotazem.  Tyto informace slouží k zobrazení popisků v grafu označující typy hodnot a volitelně pro převod hodnoty.  Typ jednotky určuje kategorii jednotky a definuje aktuální Unit – typ hodnoty, které jsou k dispozici.  Pokud vyberete hodnotu v pak číselné hodnoty jsou převést z typu aktuální jednotku na převést na typ převeďte na. |
| Vlastní popisek |Text k zobrazení pro osy Y vedle popisek pro daný typ jednotky.  Pokud není zadaný žádný štítek, se zobrazí pouze typ jednotky. |

## <a name="common-settings"></a>Obecná nastavení
Následující části popisují nastavení, které jsou společné pro několik částí vizualizace.

### <a name="name-value-separator">Název & hodnota oddělovače</a>
Pokud chcete analyzovat vlastnost text z dotazu seznamu do více hodnot jeden znak oddělovač.  Pokud zadáte oddělovač, můžete zadat názvy pro každé pole oddělené oddělovačem stejné do pole název.

Představte si třeba vlastnost s názvem *umístění* , jako například zahrnuty hodnoty *Redmond vytváření 41* a *Bellevue Building12*.  Můžete zadat – pro název a hodnotu oddělovače a *města vytváření* pro název.  To by rozloží hodnoty jednotlivých do dvou vlastností názvem *města* a *vytváření*.

### <a name="navigation-query">Navigace dotazu</a>
Dotaz spustit, když uživatel vybere položku v seznamu.  Použití *{vybranou položku}* zahrnout syntaxe pro položku, kterou uživatel vybral.

Například, pokud dotaz obsahuje sloupec s názvem *počítače* a dotaz navigace je *{vybranou položku}*, dotazu jako *počítač = "Tento počítač"* by se spustí, když uživatel Vybraný počítač.  Pokud se dotaz navigační *typ = událostí {vybranou položku}* potom dotaz *typ = událostí počítač = "Tento počítač"* by spustit.

### <a name="sparklines">Minigrafů</a>
Minigraf je malý spojnicový graf, který znázorňuje hodnotu položky seznamu v čase.  Pro vizualizaci části seznamu můžete vybrat, jestli zobrazíte vodorovných ukazující relativní hodnota je číselný sloupec nebo minigraf indikující její hodnota v čase.

Následující tabulka popisuje nastavení minigrafů.

| Nastavení | Popis |
|:--- |:--- |
| Povolit minigrafů |Vyberte, chcete-li zobrazit minigraf místo vodorovném řádku. |
| Operace |Pokud jsou povolené minigrafů, jedná se operaci provést na každou vlastnost v seznamu k výpočtu hodnot pro minigraf.<br><br>-Posledního vzorku: Poslední hodnotu pro sérii během časového intervalu.<br>-Max: Maximální hodnota pro řadu během časového intervalu.<br>-Min: Minimální hodnota pro řadu během časového intervalu.<br>-Součet: Součet hodnot pro řadu během časového intervalu.<br>-Shrnutí: Používá stejný příkaz měr jako dotaz v hlavičce. |

### <a name="thresholds">Prahové hodnoty</a>
Prahové hodnoty umožňují zobrazit Barevná ikona vedle jednotlivých položek v seznamu budete rychlý vizuální indikátor položek, které překročí určitou hodnotu nebo spadá do určitého rozsahu.  Pokud překročí hodnotu chyby, například může zobrazit zelená ikona pro položky s přijatelnou hodnotu, žlutý, pokud je hodnota v rozsahu, která určuje, upozornění a red.

Když povolíte prahové hodnoty pro část, je nutné zadat jeden nebo více prahové hodnoty.  Pokud je hodnota položky větší než prahová hodnota a nižší než další prahová hodnota, se používá tuto barvu.  Pokud položka je větší než pak nejvyšší prahovou hodnotu, tato barva nastavena.   

Každá sada prahová hodnota má jeden prahové hodnoty s hodnotou **výchozí**.  Toto je barvu nastavit, pokud se překročí žádné jiné hodnoty.  Můžete přidat nebo odebrat prahové hodnoty kliknutím  **+**  nebo **x** tlačítko.

Následující tabulka popisuje nastavení prahů.

| Nastavení | Popis |
|:--- |:--- |
| Povolit prahové hodnoty |Vyberte, chcete-li zobrazit ikonu Barva nalevo od každého hodnotu, která určuje jeho stav relativně k zadané prahové hodnoty. |
| Name (Název) |Název pro identifikaci prahovou hodnotu. |
| Prahová hodnota |Hodnota pro mezní hodnotu.  Barva stavu pro každou položku seznamu je nastavena na hodnotu barvu nejvyšší prahová hodnota překročena hodnotou položky.  Neexistuje jeden výchozí prahová hodnota, která je barvu, pokud se překročí žádné prahové hodnoty. |
| Barva |Barva pro prahovou hodnotu. |

## <a name="next-steps"></a>Další kroky
* Další informace o [protokolu hledání](log-analytics-log-searches.md) pro podporu dotazů v části vizualizace.
