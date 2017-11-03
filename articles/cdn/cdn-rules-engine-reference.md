---
title: Odkaz na modul Azure CDN pravidla | Microsoft Docs
description: "Referenční dokumentace pro Azure CDN pravidla shody stav motoru a funkce."
services: cdn
documentationcenter: 
author: Lichard
manager: akucer
editor: 
ms.assetid: 669ef140-a6dd-4b62-9b9d-3f375a14215e
ms.service: cdn
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: rli
ms.openlocfilehash: c10145661a8c575381493c9aaa901c3ef92c2e81
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="azure-cdn-rules-engine"></a>Stroj pravidel Azure CDN
Toto téma obsahuje podrobný popis funkcí a podmínky k dispozici shodu pro Azure Content Delivery Network (CDN) [stroj pravidel](cdn-rules-engine.md).

Modul HTTP pravidla slouží jako konečné právo na tom, jak konkrétní typy žádostí jsou zpracovávány CDN.

**Běžná použití**:

- Přepsání nebo definovat zásady vlastní mezipaměti.
- Zabezpečení nebo odmítnout požadavky pro citlivého obsahu.
- Přesměrování požadavků.
- Uložit data vlastního protokolu.

## <a name="terminology"></a>Terminologie
Pravidlo je definováno prostřednictvím [ **podmíněné výrazy**](cdn-rules-engine-reference-conditional-expressions.md), [ **odpovídá**](cdn-rules-engine-reference-match-conditions.md), a [ **funkce**](cdn-rules-engine-reference-features.md). Tyto prvky se zvýrazní na následujícím obrázku.

 ![Stav shody CDN](./media/cdn-rules-engine-reference/cdn-rules-engine-terminology.png)

## <a name="syntax"></a>Syntaxe

Způsobem, ve kterém budou zpracovány speciální znaky se liší podle jak podmínky shody nebo funkce zpracovává textové hodnoty. Stav shody nebo funkce mohou interpretovat text v jednom z následujících způsobů:

1. [**Literálových hodnot**](#literal-values) 
2. [**Hodnoty zástupných znaků**](#wildcard-values)
3. [**Regulární výrazy**](#regular-expressions)

### <a name="literal-values"></a>Literálových hodnot
Text, který je interpretována jako hodnota literálu bude považovat všechny speciální znaky, s výjimkou % symbol, jako součást hodnotu, která musí odpovídat. Jinými slovy, literál vyhovují podmínce nastavena na `\'*'\` bude pouze uspokojit, při které přesnou hodnotu (tj, `\'*'\`) se nachází.
 
Symbol procenta slouží k označení kódování URL (například `%20`).

### <a name="wildcard-values"></a>Hodnoty zástupných znaků
Text, který je interpretován jako zástupný znak hodnotu, bude další významy přiřadit speciální znaky. Následující tabulka popisuje, jak se interpretují následující sadu znaků.

Znak | Popis
----------|------------
\ | Abyste se vyhnuli znaky zadané v této tabulce se používá zpětné lomítko. Zpětné lomítko je třeba zadat přímo před speciální znak, který by měly být ukončeny.<br/>Například následující syntaxi řídicí sekvence hvězdičku:`\*`
% | Symbol procenta slouží k označení kódování URL (například `%20`).
* | Znak hvězdičky je zástupný znak, který reprezentuje jeden nebo více znaků.
Místo | Znak mezery označuje, že shoda podmínku může vyhovět buď zadaným hodnotám nebo vzory.
'Hodnota' | V jednoduchých uvozovkách nemá zvláštní význam. Však sadu jednoduchých uvozovek a slouží k označení, že by měl být s hodnotou zacházet jako hodnota literálu. Dá se následujícími způsoby:<br><br/>– Umožňuje shodu podmínku, kterou chcete být splněna vždy, když je zadaná hodnota odpovídá jakékoli její části hodnotu porovnání.  Například `'ma'` odpovídá některé z následujících řetězců: <br/><br/>/Business/**ma**rathon/asset.htm<br/>**Ma**p.gif<br/>/ obchodní či šablonu. **ma**p<br /><br />– Umožňuje zadat jako literál znak zvláštní znak. Můžete například určit literálu mezerou uzavřením znak mezery v rámci sady jednoduchých uvozovek (tj, `' '` nebo `'sample value'`).<br/>– Umožňuje na prázdnou hodnotu zadat. Zadejte prázdnou hodnotu zadáním sadu jednoduchých uvozovek (tj, ").<br /><br/>**Důležité:**<br/>– Pokud je zadaná hodnota neobsahuje zástupný znak, pak je automaticky považována za literálovou hodnotou. To znamená, že není nutné zadávat sadu jednoduchých uvozovek.<br/>– Pokud zpětné lomítko není řídicí jiný znak v této tabulce, pak se budou ignorovat při zadané v rámci sady jednoduchých uvozovek.<br/>-Jiný způsob, jak zadat speciální znaky, jako je literál znak pro přepnutí pomocí zpětné lomítko (tj, `\`).

### <a name="regular-expressions"></a>Regulární výrazy

Regulární výrazy definovat vzor, který bude vyhledávat v rámci textovou hodnotu. Zápis regulární výraz definuje určité významy pro celou řadu symboly. Následující tabulka uvádí, jak speciální znaky jsou považovány podmínky shody a funkce, které podporují regulární výrazy.

Speciální znak | Popis
------------------|------------
\ | Zpětné lomítko řídicí sekvence znak pomocí následujícího ho. To způsobí, že tento znak, který má být považované za literálovou hodnotou místo s ohledem na její význam regulární výraz. Například následující syntaxi řídicí sekvence hvězdičku:`\*`
% | Význam symbol procento závisí na jeho použití.<br/><br/> `%{HTTPVariable}`: Tato syntaxe identifikuje Proměnná HTTP.<br/>`%{HTTPVariable%Pattern}`: Tento syntaxi symbol procenta používá k identifikaci protokolu HTTP, proměnné a jako oddělovač.<br />`\%`: To má být použit jako hodnota literálu nebo určete kódování URL uvozovací znaky symbol procento umožňuje (například `\%20`).
* | Znak hvězdičky umožňuje předchozí znak, který má odpovídat počtu nula či více krát. 
Místo | Znak mezery je obvykle považovány za znak literálu. 
'Hodnota' | Jednoduchých uvozovek a jsou považovány za literály. Sadu jednoduchých uvozovek a nemá žádné zvláštní význam.


## <a name="next-steps"></a>Další kroky
* [Stav shody motoru pravidla](cdn-rules-engine-reference-match-conditions.md)
* [Podmíněné výrazy stroj pravidel](cdn-rules-engine-reference-conditional-expressions.md)
* [Funkce modulu pravidla](cdn-rules-engine-reference-features.md)
* [Přepsání výchozího nastavení HTTP používá stroj pravidel](cdn-rules-engine.md)
* [Přehled Azure CDN](cdn-overview.md)
