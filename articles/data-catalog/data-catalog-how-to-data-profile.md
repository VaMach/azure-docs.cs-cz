---
title: "Postup datového zdroje dat."
description: "Postupy: článek zvýraznění postup zahrnují profily úrovni tabulky a sloupce dat, při registraci zdroje dat v Azure Data Catalog a pochopit zdroje dat pomocí profilů data."
services: data-catalog
documentationcenter: 
author: spelluru
manager: NA
editor: 
tags: 
ms.assetid: 94a8274b-5c9c-4962-a4b1-2fed38a3d919
ms.service: data-catalog
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 08/03/2017
ms.author: spelluru
ms.openlocfilehash: 8f4174f0ed74706b8275c8b1f0a62753f2834fa2
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="data-profile-data-sources"></a>Zdroje dat profilů dat
## <a name="introduction"></a>Úvod
**Microsoft Azure Data Catalog** je plně spravovaná Cloudová služba, která slouží jako systém registrace a systém pro zjišťování zdrojů dat organizace. Jinými slovy **Azure Data Catalog** se točí kolem osoby zjišťovat, pochopit a používat zdroje dat a pomáhá organizacím získat větší hodnotu z jejich stávající data. Při registraci zdroje dat s **Azure Data Catalog**, jeho metadata se zkopíruje a indexované službou, ale není článek existuje ukončení.

**Dat profilace** funkce **Azure Data Catalog** prozkoumá data z podporovaných zdrojů dat v katalogu a shromažďuje statistické údaje a informace o těchto datech. Je snadné zahrnout profil datových prostředků. Když si zaregistrujete datový prostředek, zvolte **zahrnují Data profilu** v nástroj registrace zdroje dat.

## <a name="what-is-data-profiling"></a>Co je profilace dat
Data profilování prozkoumá dat ve zdroji dat, která je registrována a shromažďuje statistické údaje a informace o těchto datech. Během zjišťování zdroje dat ve statistikách vám pomohou určit, vhodnosti data k vyřešení obchodního problému.

<!-- In [How to discover data sources](data-catalog-how-to-discover.md), you learn about **Azure Data Catalog's** extensive search capabilities including searching for data assets that have a profile. See [How to include a data profile when registering a data source](#howto). -->

Následující zdroje dat podporují profilace dat:

* SQL Server (včetně databáze SQL Azure a Azure SQL Data Warehouse) tabulek a zobrazení
* Oracle tabulek a zobrazení
* Teradata tabulek a zobrazení
* Tabulek Hive

Včetně profily dat při registraci datových prostředků pomáhá uživatelům odpovězte otázky o zdrojích dat, včetně:

* Ji lze vyřešit Moje obchodního problému?
* Data v souladu s konkrétní standardy nebo vzory?
* Jaké jsou některé anomálií zdroje dat?
* Jaké jsou možné problémy integrace tato data do Moje aplikace?

> [!NOTE]
> Můžete také přidat dokumentace pro prostředek k popisu, jak může integrovat dat do aplikace. V tématu [postup dokumentování zdrojů dat](data-catalog-how-to-documentation.md).
>
>

<a name="howto"/>

## <a name="how-to-include-a-data-profile-when-registering-a-data-source"></a>Jak se zahrnuje profil dat při registraci zdroje dat
Je snadné zahrnout profil datového zdroje. Při registraci zdroje dat, v **objekty k registraci** panel Nástroj registrace zdroje dat, vyberte **zahrnují Data profilu**.

![](media/data-catalog-data-profile/data-catalog-register-profile.png)

Další informace o tom, jak registrace zdrojů dat naleznete v tématu [postup registrace zdrojů dat](data-catalog-how-to-register.md) a [Začínáme s Azure Data Catalog](data-catalog-get-started.md).

## <a name="filtering-on-data-assets-that-include-data-profiles"></a>Filtrování datových prostředků, které zahrnují data profily
Pokud chcete zjistit, datové prostředky, které zahrnují data profilu, můžete zahrnout `has:tableDataProfiles` nebo `has:columnsDataProfiles` jako jeden z vašich podmínek vyhledávání.

> [!NOTE]
> Výběr **zahrnují Data profilu** ve zdroji dat zahrnuje nástroj pro registraci tabulky a informace o profilu úrovni sloupce. Rozhraní API katalogu Data Catalog umožňuje však datové prostředky se mají být zaregistrována jen jednu sadu zahrnuty informace o profilu.
>
>

## <a name="viewing-data-profile-information"></a>Informace o profilu zobrazení dat
Jakmile zjistíte, zdroj dat vhodný k profilu, můžete zobrazit podrobnosti dat profilu. Chcete-li zobrazit data profilu, vyberte datový prostředek a zvolte **Data profilu** v okně portálu katalogu Data Catalog.

![](media/data-catalog-data-profile/data-catalog-view.png)

Data profilu v **Azure Data Catalog** zobrazuje tabulky a sloupce profil informace, včetně:

### <a name="object-data-profile"></a>Objekt dat profilu
* Počet řádků
* Velikost tabulky
* Poslední aktualizace objektu

### <a name="column-data-profile"></a>Sloupec dat profilu
* Datový typ sloupce
* Počet jedinečných hodnot
* Počet řádků s hodnotami NULL
* Minimum, maximum, průměr a standardní odchylce pro hodnoty ve sloupcích

## <a name="summary"></a>Souhrn
Data profilování poskytuje statistiky a informace o registrovaných datových prostředků můžete zjistit vhodnosti dat k řešení obchodních problémů. Zadávání poznámek a dokumentace zdroje dat, data profily můžete uživatelům lépe pochopili, vaše data.

## <a name="see-also"></a>Viz také
* [Postup registrace zdrojů dat](data-catalog-how-to-register.md)
* [Začínáme s Azure Data Catalogem](data-catalog-get-started.md)
