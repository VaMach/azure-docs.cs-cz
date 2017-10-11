---
title: "Dlaždice odkaz pro Návrhář zobrazení v OMS Log Analytics | Microsoft Docs"
description: "Návrhář zobrazení v analýzy protokolů umožňuje vytvářet vlastní zobrazení v konzole OMS, které obsahují různé vizualizace dat v úložišti OMS. Tento článek obsahuje odkaz nastavení pro každou z dlaždice, které jsou k dispozici pro použití ve vaší vlastní zobrazení."
services: log-analytics
documentationcenter: 
author: bwren
manager: jwhit
editor: 
ms.assetid: 41787c8f-6c13-4520-b0d3-5d3d84fcf142
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/17/2017
ms.author: bwren
ms.openlocfilehash: 2bce5c63b4c6edd3753f1b234cc4f493dcf53dea
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="log-analytics-view-designer-tile-reference"></a>Odkazy na dlaždici protokol Návrhář zobrazení analýzy
Návrhář zobrazení v analýzy protokolů umožňuje vytvářet vlastní zobrazení v konzole OMS, které obsahují různé vizualizace dat v úložišti OMS. Tento článek obsahuje odkaz nastavení pro každou z dlaždice, které jsou k dispozici pro použití ve vaší vlastní zobrazení.

Další články, které jsou k dispozici pro Návrhář zobrazení jsou:

* [Zobrazit návrháře](log-analytics-view-designer.md) -přehled Návrhář zobrazení a postupy pro vytváření a úpravy vlastních zobrazení.
* [Odkaz na část vizualizace](log-analytics-view-designer-parts.md) – odkaz na nastavení pro každou z dlaždice, které jsou k dispozici pro použití ve vaší vlastní zobrazení.

>[!NOTE]
> Pokud pracovní prostor byl upgradován na verzi [nové protokolu Analytics query language](log-analytics-log-search-upgrade.md), pak dotazů ve všech zobrazeních, musí být napsaná [nové dotazovací jazyk](https://go.microsoft.com/fwlink/?linkid=856078).  Všechna zobrazení, které byly vytvořeny před byl upgradován pracovním prostoru bude automtically převést.

Následující tabulka uvádí různé typy dlaždice, které jsou k dispozici v Návrháři zobrazení.  Následující oddíly popisují každý typ dlaždice v podrobností a jejich vlastnosti.

| Dlaždice | Popis |
|:--- |:--- |
| [Číslo](#number-tile) |Jedno číslo zobrazuje počet záznamů z dotazu. |
| [Dvou čísel.](#two-numbers-tile) |Zobrazuje počet záznamů z dva různé dotazy dvou čísel jeden. |
| [Prstenec](#donut-tile) |Prstenec grafu na základě dotazu s souhrnnou hodnotu v centru. |
| [Spojnicový graf & popisku](#line-chart-amp-callout-tile) |Spojnicový graf na základě dotazu a popisku s souhrnnou hodnotu. |
| [Spojnicový graf](#line-chart-tile) |Spojnicový graf založené na dotazu. |
| [Dva časové osy](#two-timelines-tile) |Sloupcový graf s dvou řad, každou v závislosti na samostatné dotazu. |

## <a name="number-tile"></a>Číslo dlaždice
**Číslo** dlaždice zobrazí jedno číslo zobrazuje počet záznamů z protokolu dotazu a štítek.

![Číslo dlaždice](media/log-analytics-view-designer/tile-number.png)

| Nastavení | Popis |
|:--- |:--- |
| Name (Název) |Text, který se zobrazí v horní části na dlaždici. |
| Popis |Text, který se zobrazí pod názvem dlaždice. |
| **Dlaždice** | |
| Legendy |Text, který se zobrazí v části hodnota. |
| Dotaz |Dotaz spustit.  Zobrazí se počet záznamů vrácených dotazem. |
| **Upřesnit** |**> Ověření toku dat** |
| Povoleno |Vyberte, pokud ověření toku dat by měla být povolená pro dlaždici.  To poskytuje alternativní zprávu, pokud data nejsou k dispozici pro dlaždici.  To se obvykle používá k poskytování zprávu dočasné období, když je nainstalován zobrazení a dodává data k dispozici. |
| Dotaz |Dotaz spustit, aby se kontrola, zda je k dispozici pro zobrazení dat.  Pokud dotaz vrátí žádné výsledky, se zobrazí zpráva namísto hodnoty z hlavní dotazu. |
| Zpráva |Zpráva zobrazí, pokud datový tok ověření dotazu nevrátí žádná data.  Pokud zadáte žádná zpráva *provádění Assessment* se zobrazí. |


## <a name="two-numbers-tile"></a>Dlaždice dvou čísel.
**Dva číslo** dlaždice zobrazí zobrazující počet záznamy ze dvou různých protokolových dotazy a popisek pro každý dvou čísel.

![Dlaždice dvou čísel.](media/log-analytics-view-designer/tile-two-numbers.png)

| Nastavení | Popis |
|:--- |:--- |
| Name (Název) |Text, který se zobrazí v horní části na dlaždici. |
| Popis |Text, který se zobrazí pod názvem dlaždice. |
| **První dlaždice** | |
| Legendy |Text, který se zobrazí v části hodnota. |
| Dotaz |Dotaz spustit.  Zobrazí se počet záznamů vrácených dotazem. |
| **Druhý dlaždice** | |
| Legendy |Text, který se zobrazí v části hodnota. |
| Dotaz |Dotaz spustit.  Zobrazí se počet záznamů vrácených dotazem. |
| **Upřesnit** |**> Ověření toku dat** |
| Povoleno |Vyberte, pokud ověření toku dat by měla být povolená pro dlaždici.  To poskytuje alternativní zprávu, pokud data nejsou k dispozici pro dlaždici.  To se obvykle používá k poskytování zprávu dočasné období, když je nainstalován zobrazení a dodává data k dispozici. |
| Dotaz |Dotaz spustit, aby se kontrola, zda je k dispozici pro zobrazení dat.  Pokud dotaz vrátí žádné výsledky, se zobrazí zpráva namísto hodnoty z hlavní dotazu. |
| Zpráva |Zpráva zobrazí, pokud datový tok ověření dotazu nevrátí žádná data.  Pokud zadáte žádná zpráva *provádění Assessment* se zobrazí. |


## <a name="donut-tile"></a>Dlaždice prstenec
**Prstenec** dlaždice zobrazí jedno číslo souhrnu ze hodnotu sloupce v protokolu dotazu.  Prstenec graficky zobrazí výsledky nejvyšší tři záznamy.

![Dlaždice prstenec](media/log-analytics-view-designer/tile-donut.png)

| Nastavení | Popis |
|:--- |:--- |
| Name (Název) |Text, který se zobrazí v horní části na dlaždici. |
| Popis |Text, který se zobrazí pod názvem dlaždice. |
| **Prstenec** | |
| Dotaz |Dotaz, aby běžel prstenec.  První vlastností by měl být textovou hodnotu a druhou vlastností číselná hodnota.  Toto je obvykle dotaz, který používá **měr** – klíčové slovo pro shrnutí výsledků. |
| **Prstenec** |**> Center** |
| Text |Text, který se zobrazí v části hodnotu uvnitř prstenec. |
| Operace |Operace k plnění vlastnost value pro shrnutí jednu hodnotu.<br><br>-Součet: Přidejte hodnoty všech záznamů se hodnota vlastnosti.<br>-Procento: Procento sečtené hodnoty ze záznamů se hodnota vlastnosti ve srovnání s sečtené hodnoty všechny záznamy. |
| Výsledek hodnoty použít v operaci center |Volitelně klikněte na symbol plus přidat jednu nebo více hodnot.  Výsledky dotazu bude omezeno na záznamy s hodnotami vlastností, které zadáte.  Pokud žádné hodnoty se přidají, než jsou zahrnuty všechny záznamy v dotazu. |
| **Prstenec** |**> Další možnosti** |
| Barvy |Barva má být zobrazen pro každé vlastnosti tři hlavní.  Pokud chcete zadat alternativní barvy pro konkrétní hodnoty vlastnosti, použijte rozšířené mapování barev. |
| Mapování pokročilé barev |Zobrazí barvu pro konkrétní hodnoty vlastnosti.  Pokud je hodnota, kterou zadáte v horní tři, se zobrazí barvu alternativní místo standardní barvu.  Pokud vlastnost není v horní tři, se nezobrazí barvu. |
| **Upřesnit** |**> Ověření toku dat** |
| Povoleno |Vyberte, pokud ověření toku dat by měla být povolená pro dlaždici.  To poskytuje alternativní zprávu, pokud data nejsou k dispozici pro dlaždici.  To se obvykle používá k poskytování zprávu dočasné období, když je nainstalován zobrazení a dodává data k dispozici. |
| Dotaz |Dotaz spustit, aby se kontrola, zda je k dispozici pro zobrazení dat.  Pokud dotaz vrátí žádné výsledky, se zobrazí zpráva namísto hodnoty z hlavní dotazu. |
| Zpráva |Zpráva zobrazí, pokud datový tok ověření dotazu nevrátí žádná data.  Pokud zadáte žádná zpráva *provádění Assessment* se zobrazí. |


## <a name="line-chart-tile"></a>Dlaždice grafu řádku
**Spojnicový graf** dlaždice zobrazí spojnicový graf s více řad z protokolu dotazu v čase.  

![Spojnicový graf & popisku dlaždice](media/log-analytics-view-designer/tile-line-chart.png)

| Nastavení | Popis |
|:--- |:--- |
| Name (Název) |Text, který se zobrazí v horní části na dlaždici. |
| Popis |Text, který se zobrazí pod názvem dlaždice. |
| **Spojnicový graf** | |
| Dotaz |Dotaz pro spojnicový graf.  První vlastností by měl být textovou hodnotu a druhou vlastností číselná hodnota.  Toto je obvykle dotaz, který používá **měr** – klíčové slovo pro shrnutí výsledků.  Pokud dotaz používá **interval** – klíčové slovo pak osy x grafu budou používat tento časový interval.  Pokud dotaz neobsahuje **interval** – klíčové slovo a každou hodinu intervaly se používají pro osu x. |
| **Spojnicový graf** |**> Osy Y** |
| Použít logaritmickou stupnici |Vyberte, chcete-li použít hodnota na logaritmické stupnici pro osu y. |
| Jednotky |Zadejte jednotky pro hodnoty vrácených dotazem.  Tyto informace slouží k zobrazení popisků v grafu označující typy hodnot a volitelně pro převod hodnoty.  **Typ jednotky** Určuje kategorii jednotky a definuje **aktuální typ jednotky** hodnoty, které jsou k dispozici.  Pokud vyberete hodnotu v **převést na** pak číselné hodnoty jsou převést z **aktuální jednotku** typ, který má **převést na** typu. |
| Vlastní popisek |Text k zobrazení pro osy Y vedle popisek pro daný typ jednotky.  Pokud není zadaný žádný štítek, se zobrazí pouze typ jednotky. |
| **Upřesnit** |**> Ověření toku dat** |
| Povoleno |Vyberte, pokud ověření toku dat by měla být povolená pro dlaždici.  To poskytuje alternativní zprávu, pokud data nejsou k dispozici pro dlaždici.  To se obvykle používá k poskytování zprávu dočasné období, když je nainstalován zobrazení a dodává data k dispozici. |
| Dotaz |Dotaz spustit, aby se kontrola, zda je k dispozici pro zobrazení dat.  Pokud dotaz vrátí žádné výsledky, se zobrazí zpráva namísto hodnoty z hlavní dotazu. |
| Zpráva |Zpráva zobrazí, pokud datový tok ověření dotazu nevrátí žádná data.  Pokud zadáte žádná zpráva *provádění Assessment* se zobrazí. |


## <a name="line-chart--callout-tile"></a>Dlaždice grafu & popisků čáry
**Čáry popisku & grafu** dlaždice zobrazuje spojnicový graf s více řad z protokolu dotazu přes čas a popisku s souhrnnou hodnotu.  

![Spojnicový graf & popisku dlaždice](media/log-analytics-view-designer/tile-line-chart-callout.png)

| Nastavení | Popis |
|:--- |:--- |
| Name (Název) |Text, který se zobrazí v horní části na dlaždici. |
| Popis |Text, který se zobrazí pod názvem dlaždice. |
| **Spojnicový graf** | |
| Dotaz |Dotaz pro spojnicový graf.  První vlastností by měl být textovou hodnotu a druhou vlastností číselná hodnota.  Toto je obvykle dotaz, který používá **měr** – klíčové slovo pro shrnutí výsledků.  Pokud dotaz používá **interval** – klíčové slovo pak osy x grafu budou používat tento časový interval.  Pokud dotaz neobsahuje **interval** – klíčové slovo a každou hodinu intervaly se používají pro osu x. |
| **Spojnicový graf** |**> Popisku** |
| Popisku |Text názvu zobrazíte větší než hodnota popisku. |
| Název řady |Hodnota vlastnosti řady, které chcete použít pro hodnotu popisku.  Pokud je k dispozici žádné řady, použijí se všechny záznamy z dotazu. |
| Operace |Operace k plnění vlastnost value pro shrnutí pro jednu hodnotu pro popisek.<br>-Průměr: Průměr hodnoty ze všech záznamů.<br><br>-Count: Počet všech záznamů vrácených dotazem.<br>-Posledního vzorku: Hodnota od posledního intervalu zahrnuté v grafu.<br>-Max: Maximální hodnota z intervalů zahrnuté v grafu.<br>-Min: Minimální hodnota z intervalů zahrnuté v grafu.<br>-Součet: Součet hodnoty ze všech záznamů. |
| **Spojnicový graf** |**> Osy Y** |
| Použít logaritmickou stupnici |Vyberte, chcete-li použít hodnota na logaritmické stupnici pro osu y. |
| Jednotky |Zadejte jednotky pro hodnoty vrácených dotazem.  Tyto informace slouží k zobrazení popisků v grafu označující typy hodnot a volitelně pro převod hodnoty.  **Typ jednotky** Určuje kategorii jednotky a definuje **aktuální typ jednotky** hodnoty, které jsou k dispozici.  Pokud vyberete hodnotu v **převést na** pak číselné hodnoty jsou převést z **aktuální jednotku** typ, který má **převést na** typu. |
| Vlastní popisek |Text k zobrazení pro osy Y vedle popisek pro daný typ jednotky.  Pokud není zadaný žádný štítek, se zobrazí pouze typ jednotky. |
| **Upřesnit** |**> Ověření toku dat** |
| Povoleno |Vyberte, pokud ověření toku dat by měla být povolená pro dlaždici.  To poskytuje alternativní zprávu, pokud data nejsou k dispozici pro dlaždici.  To se obvykle používá k poskytování zprávu dočasné období, když je nainstalován zobrazení a dodává data k dispozici. |
| Dotaz |Dotaz spustit, aby se kontrola, zda je k dispozici pro zobrazení dat.  Pokud dotaz vrátí žádné výsledky, se zobrazí zpráva namísto hodnoty z hlavní dotazu. |
| Zpráva |Zpráva zobrazí, pokud datový tok ověření dotazu nevrátí žádná data.  Pokud zadáte žádná zpráva *provádění Assessment* se zobrazí. |


## <a name="two-timelines-tile"></a>Dlaždice obě časové osy
**Obě časové osy** dlaždice zobrazuje výsledky dva dotazy protokolu časem jako sloupcových grafů.  U každé série se zobrazí popisek.  

![Dlaždice obě časové osy](media/log-analytics-view-designer/tile-two-timelines.png)

| Nastavení | Popis |
|:--- |:--- |
| Name (Název) |Text, který se zobrazí v horní části na dlaždici. |
| Popis |Text, který se zobrazí pod názvem dlaždice. |
| První graf | |
| Legendy |Text, který se zobrazí v části popisku řady, první. |
| Barva |Barva má být použita pro sloupce v první řadě. |
| Graf dotazu |Dotaz pro první řady.  Počet záznamů v každém časovém intervalu budou odpovídat sloupcům grafu. |
| Operace |Operace k plnění vlastnost value pro shrnutí pro jednu hodnotu pro popisek.<br><br>-Průměr: Průměr hodnoty ze všech záznamů.<br>-Count: Počet všech záznamů vrácených dotazem.<br>-Posledního vzorku: Hodnota od posledního intervalu zahrnuté v grafu.<br>-Max: Maximální hodnota z intervalů zahrnuté v grafu. |
| **Druhý graf** | |
| Legendy |Text, který se zobrazí v části popisku řady, druhý. |
| Barva |Barva má být použita pro sloupce v druhé řady. |
| Graf dotazu |Dotaz pro druhý řady.  Počet záznamů v každém časovém intervalu budou odpovídat sloupcům grafu. |
| Operace |Operace k plnění vlastnost value pro shrnutí pro jednu hodnotu pro popisek.<br><br>-Průměr: Průměr hodnoty ze všech záznamů.<br>-Count: Počet všech záznamů vrácených dotazem.<br>-Posledního vzorku: Hodnota od posledního intervalu zahrnuté v grafu.<br>-Max: Maximální hodnota z intervalů zahrnuté v grafu. |
| **Upřesnit** |**> Ověření toku dat** |
| Povoleno |Vyberte, pokud ověření toku dat by měla být povolená pro dlaždici.  To poskytuje alternativní zprávu, pokud data nejsou k dispozici pro dlaždici.  To se obvykle používá k poskytování zprávu dočasné období, když je nainstalován zobrazení a dodává data k dispozici. |
| Dotaz |Dotaz spustit, aby se kontrola, zda je k dispozici pro zobrazení dat.  Pokud dotaz vrátí žádné výsledky, se zobrazí zpráva namísto hodnoty z hlavní dotazu. |
| Zpráva |Zpráva zobrazí, pokud datový tok ověření dotazu nevrátí žádná data.  Pokud zadáte žádná zpráva *provádění Assessment* se zobrazí. |


## <a name="next-steps"></a>Další kroky
* Další informace o [protokolu hledání](log-analytics-log-searches.md) pro podporu dotazů v dlaždice.
* Přidat [vizualizace částí](log-analytics-view-designer-parts.md) do vlastních zobrazení.
