---
title: "Tom, jak zjišťovat zdroje dat v Azure Data Catalog | Microsoft Docs"
description: "V tomto článku se dozvíte, jak ke zjištění registrovaných datových prostředků s Azure Data Catalog, včetně vyhledávání a filtrování a pomocí přístupů zvýraznění funkcí z portálu Azure Data Catalog."
services: data-catalog
documentationcenter: 
author: steelanddata
manager: NA
editor: 
tags: 
ms.assetid: f72ae3a3-6573-4710-89a7-f13555e1968c
ms.service: data-catalog
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 08/15/2017
ms.author: maroche
ms.openlocfilehash: 9ff67dcb5ecb00440f73f979fd8d2b79a570c674
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="how-to-discover-data-sources-in-azure-data-catalog"></a>Tom, jak zjišťovat zdroje dat v Azure Data Catalog
## <a name="introduction"></a>Úvod
Azure Data Catalog je plně spravovaná Cloudová služba, která slouží jako systém registrace a zjišťování datových zdrojů organizace. Jinými slovy Data Catalog umožňuje uživatelům zjišťovat, pochopit a používat zdroje dat a pomáhá organizacím získat větší hodnotu ze své stávající data. Po registraci zdroje dat pomocí katalogu Data Catalog jeho metadata je indexovaný služba, takže můžete snadno vyhledat ke zjištění dat, které potřebujete.

## <a name="searching-and-filtering"></a>Vyhledávání a filtrování
Funkce zjišťování v katalogu Data Catalog používá dva primární mechanismy: vyhledávání a filtrování.

Vyhledávání je koncipováno tak, aby bylo jak intuitivní, tak výkonné. Ve výchozím nastavení se vyhledává shoda hledaných výrazů s libovolnou vlastností v katalogu včetně poznámek přidaných uživatelem.

Filtrování je koncipováno jako doplněk k vyhledávání. Můžete vybrat konkrétní charakteristiky jako odborníky, typ zdroje dat, typ objektu a značky. Můžete zobrazit jenom odpovídající datových prostředků a omezit výsledky vyhledávání na odpovídající prostředky.

Pomocí kombinace vyhledávání a filtrování, můžete rychle procházet zdroje dat, které byly zaregistrovány pomocí katalogu Data Catalog zjišťování zdrojů dat, které potřebujete.

## <a name="search-syntax"></a>Syntaxe služby Search
I když výchozí textové vyhledávání je jednoduché a intuitivní, můžete taky syntaxe vyhledávání katalogu Data Catalog pro větší kontrolu nad výsledky hledání. Hledání v katalogu dat podporuje následující techniky:

| Technika | Použití | Příklad |
| --- | --- | --- |
| Základní hledání |Základní vyhledávání, který používá jeden nebo více výrazů vyhledávání. Výsledky jsou všechny prostředky, které odpovídají všechny vlastnosti s jednou nebo více výrazů uvedených. |`sales data` |
| Zkoumání vlastnosti |Vrátí pouze zdroje dat kde hledaný termín shoduje se zadanou vlastnost. |`name:finance` |
| Logické operátory |Rozšíří nebo zúží vyhledávání pomocí logických operací. |`finance NOT corporate` |
| Seskupování pomocí závorek |Závorky lze použijte k seskupení části dotazu k dosažení logické izolace, zejména ve spojení s logickými operátory. |`name:finance AND (tags:Q1 OR tags:Q2)` |
| Operátory porovnání |Porovnávání jiné než rovnost použijte pro vlastnosti, které mají číselné a číselné datové typy. |`modifiedTime > "11/05/2014"` |

Další informace o hledání v katalogu dat najdete v tématu [Azure Data Catalog](https://msdn.microsoft.com/library/azure/mt267594.aspx) článku.

## <a name="hit-highlighting"></a>Zvýrazňování položek
Při zobrazení výsledků vyhledávání, všechny zobrazené vlastnosti, které odpovídají zadané hledaným výrazům (například data asset název, popis a značky) se zvýrazní, aby bylo snazší identifikovat, proč daný datový prostředek vrátil zadaný hledaný.

> [!NOTE]
> Chcete-li vypnout přístupů zvýraznění, použijte **zvýrazněte** přepnout na portálu katalogu Data Catalog.
>
>

Při zobrazení výsledků vyhledávání, nemusí být vždy zřejmé proč datový prostředek je součástí, dokonce i s přístupů zvýraznění povolena. Protože vyhledávají se všechny vlastnosti ve výchozím nastavení, může být z důvodu shoda na úrovni sloupce vlastnost vrácena datový prostředek. A protože více uživatelů může opatřit poznámkami registrované datové prostředky s vlastní značky a popisy, ne všechny metadata mohou být zobrazeny v seznamu výsledků hledání.

Ve výchozím zobrazení vedle sebe, obsahuje každou dlaždici zobrazí ve výsledcích hledání **zobrazení hledaný termín shoduje** ikonu, takže můžete rychle zobrazit počet shod a jejich umístění a přejít k nim, pokud chcete.

 ![Zvýrazňování položek a hledání odpovídá na portálu Azure Data Catalog](./media/data-catalog-how-to-discover/search-matches.png)

## <a name="summary"></a>Souhrn
Protože registraci zdroje dat pomocí katalogu Data Catalog kopie strukturální a popisný metadat ze zdroje dat do katalogu služby, zdroj dat vyhledatelné a pochopitelné jednodušší. Poté, co jste registrováni zdroj dat, můžete zjistit pomocí filtrování a vyhledávání z portálu pro katalog Data Catalog.

## <a name="next-steps"></a>Další kroky
* Podrobné informace o tom, jak zjistit zdroje dat najdete v tématu [Začínáme s Azure Data Catalog](data-catalog-get-started.md).
