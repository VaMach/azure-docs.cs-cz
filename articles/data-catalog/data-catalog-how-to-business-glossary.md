---
title: "Nastavit obchodní Glosář pro řídí označování v Azure Data Catalog | Microsoft Docs"
description: "Článek s postupy pro definování a používání běžných termínů obchodní do značky zvýraznění obchodní Glosář v Azure Data Catalog registrovaných datových prostředků."
services: data-catalog
documentationcenter: 
author: steelanddata
manager: NA
editor: 
tags: 
ms.assetid: b3d63dbe-1ae7-499f-bc46-42124e950cd6
ms.service: data-catalog
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 10/15/2017
ms.author: maroche
ms.openlocfilehash: bcd2ba4f92937cf1e134b2a59d97c7bea7802145
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/03/2017
---
# <a name="set-up-the-business-glossary-for-governed-tagging"></a>Nastavit obchodní Glosář pro řídí označování
## <a name="introduction"></a>Úvod
Azure Data Catalog umožňuje zdroj dat zjišťování, takže můžete snadno vyhledat a pochopit zdroje dat, které budete muset provádět analýzy a rozhodnutí. Tyto možnosti zajistěte největších dopad při můžete najít a pochopit nejširší řadu dostupných zdrojů dat.

Je označování jeden katalogu Data Catalog funkce, která podporuje větší přehled o datové prostředky. Pomocí označování, můžete přidružit klíčová slova prostředek nebo sloupec, který naopak je snazší zjistit asset prostřednictvím vyhledávání a procházení. Označování také pomáhá další snadno pochopit kontextu a záměr prostředku.

Ale označování může v některých případech způsobit problémy své vlastní. Mezi příklady problémy, které můžou vést k označování patří:

* Použití zkratky na některých prostředků a rozšířená text na ostatní. Tato nekonzistence omezuje zjišťování prostředků, i když byl záměr k označování prostředků se stejnou značkou.
* Potenciální rozdíly v význam, a to v závislosti na kontextu. Například značku názvem *výnosy* na Zákazník může znamenat datové sady, obrat podle zákazníka, ale stejnou značku pro čtvrtletní prodeje datovou sadu může znamenat čtvrtletně výnosů společnosti.  

Abyste řeší tyto a další podobné problémy katalogu Data Catalog zahrnuje obchodní Glosář.

Pomocí katalogu Data Catalog obchodní glosář, můžete organizaci dokumentů klíče obchodní podmínky a vytvoření běžných termínů obchodní jejich definice. Tato zásady správného řízení umožňuje konzistence dat využití celé organizace. Po termín, který je definován v obchodní glosář, jej lze přiřadit k datovému prostředku v katalogu. Tento přístup *řídí označování*, se o stejný postup jako označování.

## <a name="glossary-availability-and-privileges"></a>Glosář dostupnosti a oprávnění
Obchodní Glosář je k dispozici pouze v Azure Data Catalog, Standard Edition. Bezplatná edice Data Catalog nezahrnuje Glosář a nenabízí možnosti pro označování upraveny.

Dostanete obchodní Glosář prostřednictvím **Glosář** možnost v navigační nabídce portálu katalogu Data Catalog.  

![Přístup k obchodní Glosář](./media/data-catalog-how-to-business-glossary/01-portal-menu.png)

Data katalogu a členové rolí správce Glosář můžete vytvořit, upravit a odstranit slovníku pojmů v obchodní Glosář. Všichni uživatelé katalogu Data Catalog můžete zobrazit definice období a značky prostředky pomocí slovníku pojmů.

![Přidání nové období Glosář](./media/data-catalog-how-to-business-glossary/02-new-term.png)

## <a name="creating-glossary-terms"></a>Vytváření slovníku pojmů
Data katalogu správců a správců Glosář kliknutím můžete vytvořit slovníku pojmů **nové období** tlačítko. Každému termínu glosář obsahuje následující pole:

* Obchodní definice pro termín
* Popis, který zachycuje zamýšlené použití nebo obchodní pravidla pro prostředek nebo sloupce
* Seznam účastníkům, kteří znají nejvíce o termín
* Nadřazený termín, která definuje hierarchie, ve kterém jsou uspořádána termín

## <a name="glossary-term-hierarchies"></a>Glosář termínů hierarchie
Pomocí katalogu Data Catalog obchodní glosář, organizace může popsat jeho slovník business jako hierarchie podmínek a může vytvořit klasifikace podmínky, která lépe představuje jeho obchodní taxonomii.

Termín, který musí být jedinečný na dané úrovni hierarchie. Duplicitní názvy nejsou povoleny. Neexistuje žádné omezení počtu úrovní v hierarchii, ale hierarchie je často srozumitelnější když existují tři úrovně nebo méně.

Použití hierarchií v obchodní Glosář je volitelné. Ponechat prázdné pole termín nadřazený pro slovníku pojmů vytvoří dvojrozměrném (jiných než hierarchických) seznam termínů v glosáři.  

## <a name="tagging-assets-with-glossary-terms"></a>Označování prostředky pomocí slovníku pojmů
Po definování slovníku pojmů v rámci katalogu prostředí označování prostředků je optimalizovaná pro vyhledávání glosáři jako uživatel zadá značku. Portál pro Data Catalog zobrazí seznam odpovídající Glosář termínů lze vybírat. Pokud si uživatel vybere ze seznamu Glosář termín, termín se přidá do prostředku jako tag (také nazývané značku glosář). Uživatele můžete také rozhodnout vytvořit novou značku zadáním termín, který není v glosáři (také nazývané značku uživatele).

![Datový prostředek označené značku jednoho uživatele a dvě značky Glosář](./media/data-catalog-how-to-business-glossary/03-tagged-asset.png)

> [!NOTE]
> Značky uživatele jsou pouze typ značky, které jsou podporovány v bezplatná edice Data Catalog.
>
>

### <a name="hover-behavior-on-tags"></a>Chování při přechodu na značky
Na portálu katalogu Data Catalog jsou dva typy značek vizuálně odlišné a existuje jiný hover chování. Po přesunutí ukazatele myši značku uživatele, zobrazí se text značky a uživatele nebo uživatelů, kteří přidání značka. Po přesunutí ukazatele myši značku glosář, uvidíte také definici Glosář termínů a odkazu k otevření obchodní glosář, chcete-li zobrazit úplnou definice podmínek.

### <a name="search-filters-for-tags"></a>Filtry hledání pro značky
Glosář značky a značky uživatele jsou obě vyhledávat a můžete je použít jako filtry v hledání.

## <a name="summary"></a>Souhrn
Pomocí obchodní Glosář v Azure Data Catalog a upraveny označování, které umožňuje můžete identifikovat, spravovat a zjistili datové prostředky konzistentním způsobem. Obchodní Glosář můžete zvýšit úroveň learning slovník business členové organizace. Glosáři také podporuje zaznamenávání smysluplný metadata, která zjednodušuje asset zjišťování a pochopení.

## <a name="next-steps"></a>Další kroky
* [Dokumentace k REST API pro operace obchodní Glosář](https://msdn.microsoft.com/library/mt708855.aspx)
