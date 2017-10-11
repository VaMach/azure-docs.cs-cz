---
title: "Regulární výrazy v OMS Log Analytics protokolu hledání | Microsoft Docs"
description: "RegEx – klíčové slovo v hledání protokolu analýzy protokolů filtru můžete výsledky podle regulární výraz.  Tento článek obsahuje syntaxi pro tyto výrazy s několik příkladů."
services: log-analytics
documentationcenter: 
author: bwren
manager: carmonm
editor: 
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/08/2017
ms.author: bwren
ms.openlocfilehash: 9746170f157ed5065adc953a31687ff18bd73708
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="using-regular-expressions-to-filter-log-searches-in-log-analytics"></a>Používání regulárních výrazů k filtrování protokolu vyhledá v analýzy protokolů

[Přihlaste se hledání](log-analytics-log-searches.md) umožňují extrahovat informace z úložiště analýzy protokolů.  [Výrazech filtrů](log-analytics-search-reference.md#filter-expressions) vám umožní filtrovat výsledky hledání podle konkrétních kritérií.  **RegEx** – klíčové slovo můžete zadat regulární výraz pro filtr.  

Tento článek poskytuje podrobné informace o syntaxi regulárního výrazu používaný analýzy protokolů.

> [!NOTE]
> Regulární výraz můžete použít pouze s prohledávatelné pole.  Další informace o prohledatelná pole najdete v tématu **typy polí** v [najít data pomocí protokolu hledání v analýzy protokolů](log-analytics-log-searches.md#use-additional-filters).


## <a name="regex-keyword"></a>RegEx – klíčové slovo

Použijte následující syntaxi používat **RegEx** – klíčové slovo v hledání protokolů.  V dalších částech tohoto článku můžete použít syntaxi regulárního výrazu sám sebe.

    field:Regex("Regular Expression")
    field=Regex("Regular Expression")

Chcete-li například použít regulární výraz, který vrátí výstrahy záznamů s typem *upozornění* nebo *chyba*, byste použili následující hledání protokolů.

    Type=Alert AlertSeverity=RegEx("Warning|Error")

## <a name="partial-matches"></a>Částečné shody
Všimněte si, že regulární výraz musí odpovídat celý text vlastnosti.  Termínem shodují jen částečně nevrátí žádné záznamy.  Například pokud jste se pokoušeli vrátí záznamy z počítače s názvem srv01.contoso.com, následující hledání protokolů by **není** vrátit žádné záznamy.

    Computer=RegEx("srv..")

Je to proto, že pouze první část název odpovídá regulárnímu výrazu.  Následující dva protokolu hledání by vrátit záznamů z tohoto počítače, protože budou odpovídat celý název.

    Computer=RegEx("srv..@")
    Computer=RegEx("srv...contoso.com")

## <a name="characters"></a>Znaky
Zadejte jiné znaky.

| Znak | Popis | Příklad | Odpovídá vzorku |
|:--|:--|:--|:--|
| A | Jeden výskyt znaku. | Computer=Regex("Srv01.contoso.com") | Srv01.contoso.com |
| . | Jednoho libovolného znaku. | Computer=Regex("SRV...contoso.com") | Srv01.contoso.com<br>SRV02.contoso.com<br>srv03.contoso.com |
| na? | Nula nebo jeden výskyt znaku. | Počítač = RegEx ("srv01?. contoso.com") | srv0.contoso.com<br>Srv01.contoso.com |
| * | Nula nebo více výskytů znaku. | Computer=Regex("Srv01*.contoso.com") | srv0.contoso.com<br>Srv01.contoso.com<br>srv011.contoso.com<br>srv0111.contoso.com |
| + | Jeden nebo více výskytů znak. | Computer=Regex("Srv01+.contoso.com") | Srv01.contoso.com<br>srv011.contoso.com<br>srv0111.contoso.com |
| [*abc*] | Nahradit libovolný znak v závorkách | Computer=Regex("srv0[123].contoso.com") | Srv01.contoso.com<br>SRV02.contoso.com<br>srv03.contoso.com |
| [**-*z*] | Odpovídat jednoho znaku v rozsahu.  Může zahrnovat více oblastí. | Computer=Regex("srv0[1-3].contoso.com") | Srv01.contoso.com<br>SRV02.contoso.com<br>srv03.contoso.com |
| [^*abc*] | Žádný ze znaků v hranatých závorkách | Computer=Regex("srv0[^123].contoso.com") | srv05.contoso.com<br>SRV06.contoso.com<br>srv07.contoso.com |
| [^**-*z*] | Žádný ze znaků v rozsahu. | Computer=Regex("srv0[^1-3].contoso.com") | srv05.contoso.com<br>SRV06.contoso.com<br>srv07.contoso.com |
| [*n*-*m*] | Odpovídat rozsahu číselné znaky. | Computer=Regex("SRV[01-03].contoso.com") | Srv01.contoso.com<br>SRV02.contoso.com<br>srv03.contoso.com |
| @ | Libovolný řetězec znaků. | Počítač = RegEx ("srv@.contoso.com") | Srv01.contoso.com<br>SRV02.contoso.com<br>srv03.contoso.com |


## <a name="multiple-occurences-of-character"></a>Více výskytů znaku
Zadejte vícenásobné výskyty konkrétní znaků.

| Znak | Popis | Příklad | Odpovídá vzorku |
|:--|:--|:--|:--|
| {n} |  *n*výskyty znak. | Computer=Regex("BW-Win-sc01{3}.bwren.Lab") | BW-win-sc0111.bwren.lab |
| {n} |  *n*nebo více výskytů znak. | Computer=Regex("BW-Win-sc01{3,}.bwren.Lab") | BW-win-sc0111.bwren.lab<br>BW-win-sc01111.bwren.lab<br>BW-win-sc011111.bwren.lab<br>BW-win-sc0111111.bwren.lab |
| {n, m} |  *n*k *m* výskyty znak. | Computer=Regex("BW-Win-sc01{3,5}.bwren.Lab") | BW-win-sc0111.bwren.lab<br>BW-win-sc01111.bwren.lab<br>BW-win-sc011111.bwren.lab |


## <a name="logical-expressions"></a>Logické výrazy
Vyberte z více hodnot.

| Znak | Popis | Příklad | Odpovídá vzorku |
|:--|:--|:--|:--|
| &#124; | Logické OR.  Vrátí výsledek, pokud odpovídají na buď výraz. | Typ = výstrahy AlertSeverity = RegEx ("upozornění &#124; Chyba") | Upozornění<br>Chyba |
| & | Logickým operátorem a.  Vrátí výsledek, pokud odpovídají na oba výrazy | EventData = regex ("(zabezpečení.\* &. \*úspěch. \*)") | Auditování úspěšných zabezpečení |


## <a name="literals"></a>Literály
Speciální znaky převeďte literál znaků.  To zahrnuje znaky, které poskytují funkce regulární výrazy, například?-\*^\[\]{}\(\)+\|. &.

| Znak | Popis | Příklad | Odpovídá vzorku |
|:--|:--|:--|:--|
| \\ | Převede zvláštní znak literál. | Status_CF =\\[Chyba\\] @<br>Status_CF = chyby\\-@ | [Chyba] Soubor nebyl nalezen.<br>Chyba – soubor nebyl nalezen. |


## <a name="next-steps"></a>Další kroky

* Seznamte se s [protokolu hledání](log-analytics-log-searches.md) můžete zobrazit a analyzovat data v úložišti analýzy protokolů.
