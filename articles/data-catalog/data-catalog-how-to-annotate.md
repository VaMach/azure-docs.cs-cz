---
title: "Postup přidání poznámek ke zdrojům dat | Microsoft Docs"
description: "Postupy: článek zvýraznění postup opatřit poznámkami datové prostředky v Azure Data Catalog, včetně popisné názvy, značky, popisy a odborníky."
services: data-catalog
documentationcenter: 
author: steelanddata
manager: NA
editor: 
tags: 
ms.assetid: 5a7e6bb2-863c-4eca-b614-1c814920d9ed
ms.service: data-catalog
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 08/15/2017
ms.author: maroche
ms.openlocfilehash: 4518fc126c717cc79ca7950c0b1ddcd9f1d8c7d2
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-annotate-data-sources"></a>Postup přidání poznámek ke zdrojům dat
## <a name="introduction"></a>Úvod
**Microsoft Azure Data Catalog** je plně spravovaná Cloudová služba, která slouží jako systém registrace a systém pro zjišťování zdrojů dat organizace. Jinými slovy Data Catalog se točí kolem pomáhá osoby zjišťovat, pochopit a používat zdroje dat a pomáhají organizacím získat větší hodnotu ze své stávající data. Při registraci zdroje dat pomocí katalogu Data Catalog, jeho metadata je zkopírovat a indexované službou, ale není článek existuje ukončení. Data Catalog umožňuje uživatelům poskytnout vlastní popisný metadat – například značky – doplníte metadata extrahovaná ze zdroje dat a aby zdroj dat více nerozumí lidem a popisy.

## <a name="annotation-and-crowdsourcing"></a>Poznámky a crowdsourcingu
Každý má k vyjádření. A toto je dobré.
Data Catalog rozpozná, mají různé uživatele různých perspektiv v datových zdrojů organizace a že každý z těchto perspektiv mohou být cenné. Vezměte v úvahu následující scénář:

* Správce systému ví smlouvu o úrovni služeb pro servery nebo služby, které hostují zdroj dat.
* Správce databáze zná plán zálohování pro každou databázi a windows povolené zpracování ETL.
* Proces pro uživatele s požadavkem o přístup ke zdroji dat, bude znát vlastníka systému.
* Data steward neví, jak namapovat prostředky a atributy ve zdroji dat do datového modelu enterprise.
* Analytika vědět, jak se používají data v rámci podnikové procesy, které mu podporuje.

Každý z těchto perspektiv se hodí v situaci, a Data Catalog používá crowdsourcingový přístup k metadata, která umožňuje každému z nich možné zachytit a použít k zajištění úplný přehled o registrované datové zdroje. Pomocí katalogu Data Catalog portálu, můžete každý uživatel přidat a upravit své vlastní poznámky, při schopnost zobrazit poznámky od jiných uživatelů.

## <a name="different-types-of-annotations"></a>Různé typy poznámky
Data Catalog podporuje následující typy poznámky:

| Poznámky | Poznámky |
| --- | --- |
| Popisný název |Na úrovni asset data, aby datové prostředky srozumitelnější můžete zadat popisné názvy. Popisné názvy jsou nejužitečnější, pokud je základní název objektu jako nesrozumitelné, zkrácené nebo v opačném případě nemá význam pro uživatele. |
| Popis |Popisy můžete zadat na datový prostředek a atribut nebo úrovně sloupce. Popisy jsou krátké textové poznámky, které popisují uživatele perspektivy na datový prostředek nebo jeho použití. |
| Značky (značky uživatele) |Můžete zadat značky na datový prostředek a atribut nebo úrovně sloupce. Značky uživatele jsou popisky definovaný uživatelem, které slouží ke kategorizaci datových prostředků nebo atributy. |
| Značky (Glosář značky) |Můžete zadat značky na datový prostředek a atribut nebo úrovně sloupce. Glosář značky jsou definované centrálně slovníku pojmů, které slouží ke kategorizaci datových prostředků nebo atributů s použitím běžné obchodní taxonomii. Další informace naleznete v článku [Jak nastavit obchodní glosář řízeným přidáváním značek](data-catalog-how-to-business-glossary.md) |
| Odborníci |Odborníci můžete zadat na úrovni asset data. Odborníci identifikovat uživatele nebo skupiny s odbornými perspektivami na data a může sloužit jako body kontaktu pro uživatele, kteří zjišťovat registrované datové zdroje a máte otázky, které nenajdete odpovědi v existujících poznámek. |
| Požádat o přístup |Žádost o přístup k informacím můžete zadat na úrovni asset data. Tyto informace jsou pro uživatele, kteří zjistit zdroj dat, který ještě nemají oprávnění k přístupu. Uživatele můžete zadat e-mailovou adresu uživatele nebo skupiny, který uděluje přístup, adresa URL procesu nebo nástroj, který uživatelé potřebovat přístup, nebo můžete zadat samotný proces jako text. |
| Dokumentace |Dokumentace můžete zadat na úrovni asset data. Asset dokumentace je formátovaným textem informace, které může obsahovat odkazy a bitové kopie a která může poskytnout všechny informace není vyjádřené prostřednictvím popisy a značky. |

## <a name="annotating-multiple-assets"></a>Zadávání poznámek k více prostředků
Když vyberete více datových prostředků na portálu katalogu Data Catalog, uživatelé může opatřit poznámkami všechny vybrané prostředky v rámci jedné operace. Poznámky platit pro všechny vybrané prostředky, což usnadňuje vyberte a zadejte popis konzistentní a sady značek a odborníky pro prostředky související data.

> [!NOTE]
> Značky a odborníky se dá zadat i při registraci datové prostředky pomocí katalogu Data Catalog dat zdroje nástroj pro registraci.
>
>

Při výběru více tabulek nebo zobrazení, jenom na sloupce, že všechny vybrané data, která mají společné prostředky se zobrazí na portálu katalogu Data Catalog. To umožňuje uživatelům poskytnout značky a popisy pro všechny sloupce se stejným názvem pro všechny vybrané prostředky.

## <a name="annotations-and-discovery"></a>Poznámky a zjišťování
Stejně jako metadata extrahovaná ze zdroje dat během registrace se přidá do indexu vyhledávání katalogu Data Catalog, uživatelem zadané metadata jsou také indexována. To znamená, že nejen poznámky usnadňují uživatelům pochopit data, která se zjistit, poznámky také usnadňují uživatelům zjišťovat poznámkami datové prostředky pomocí vyhledávání pomocí termínů, které jsou k nim.

## <a name="summary"></a>Souhrn
Registrace zdroje dat pomocí katalogu Data Catalog umožňuje dat zjistitelný zkopírováním strukturální a popisný metadat ze zdroje dat ve službě katalogu. Po registraci zdroje dat, uživatelé mohou poskytovat poznámky snadno vyhledatelné a pochopitelné z portálu pro katalog Data Catalog.

## <a name="see-also"></a>Viz také
* [Začínáme s Azure Data Catalog](data-catalog-get-started.md) kurz podrobné informace o tom, jak přidání poznámek ke zdrojům dat.
