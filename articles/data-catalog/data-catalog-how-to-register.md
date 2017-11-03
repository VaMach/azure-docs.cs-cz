---
title: "Registrace zdrojů dat v Azure Data Catalog | Microsoft Docs"
description: "V tomto článku se dozvíte, jak zaregistrovat zdroje dat v Azure Data Catalog, včetně pole metadat extrahovat během registrace."
services: data-catalog
documentationcenter: 
author: steelanddata
manager: NA
editor: 
tags: 
ms.assetid: bab89906-186f-4d35-9ffd-61b1d903905d
ms.service: data-catalog
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 08/15/2017
ms.author: maroche
ms.openlocfilehash: 30166823b33669dda88b41a4aee2dfc34f01466f
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="register-data-sources-in-azure-data-catalog"></a>Registrace zdrojů dat v Azure Data Catalog
## <a name="introduction"></a>Úvod
Azure Data Catalog je plně spravovaná Cloudová služba, která slouží jako systém registrace a zjišťování datových zdrojů organizace. Jinými slovy Data Catalog umožňuje uživatelům zjišťovat, pochopit a používat zdroje dat a pomáhá organizacím získat větší hodnotu ze své stávající data. Registrace zdroje dat je prvním krokem k vytváření zdroje dat zjistitelný prostřednictvím katalogu Data Catalog.

## <a name="register-data-sources"></a>Registrace zdrojů dat
Registrace je proces extrakce dat ze zdroje dat a kopírování dat do služby Data Catalog. Data zůstanou uložena tam, kde v současnosti jsou, a zůstávají pod kontrolou správců a zásad aktuálního systému.

Registrovat zdroj dat, postupujte takto:
1. Na portálu Azure Data Catalog spusťte nástroj registrace zdroje dat katalogu Data Catalog. 
2. Přihlaste se pomocí pracovního nebo školního účtu pomocí stejných přihlašovacích údajů Azure Active Directory, které používáte pro přihlášení k portálu.
3. Vyberte zdroj dat, který chcete zaregistrovat.

Další podrobné informace najdete v tématu [Začínáme s Azure Data Catalog](data-catalog-get-started.md) kurzu.

Poté, co jste registrováni zdroj dat, katalogu sleduje jeho umístění a indexy jeho metadata. Uživatele můžete vyhledávat, procházet a zjišťovat zdroje dat a pak pomocí umístění, kde k nim připojit pomocí aplikace nebo nástroje, které si sami vyberou.

## <a name="supported-data-sources"></a>Podporované zdroje dat
Seznam aktuálně podporované zdroje dat, naleznete v části [DSR katalogu dat](data-catalog-dsr.md).

## <a name="structural-metadata"></a>Strukturální metadata
Při registraci zdroje dat, nástroj pro registraci extrahuje informace o struktuře objekty, které vyberete. Tyto informace se označuje jako strukturální metadata.

Pro všechny objekty zahrnuje tato strukturální metadata umístění objektu, tak, aby uživatelé, kteří data zjišťování můžete použít tyto informace pro připojení k objektu v nástrojích klienta, které si sami vyberou. Další strukturální metadata obsahuje název objektu a typu a zadejte název atributu/sloupce a data.

## <a name="descriptive-metadata"></a>Popisná metadata
Nástroj registrace zdroje dat kromě základní strukturální metadata rozbalený ze zdroje dat, extrahuje popisných metadat. Pro SQL Server Analysis Services a SQL Server Reporting Services je tato metadata převzat ze vlastnosti popisu vystavené těchto služeb. Pro systém SQL Server, hodnoty poskytnuté pomocí ms\_popis rozšířené vlastnosti je rozbalena. Nástroj pro registraci zdroje dat pro databázi Oracle, extrahuje sloupci KOMENTÁŘE z všechny\_KARTĚ\_zobrazení KOMENTÁŘE.

Kromě popisných metadat, které je extrahována ze zdroje dat mohou uživatelé zadávat nástroj registrace zdroje dat pomocí popisných metadat. Uživatele můžete přidat značky a identifikují odborníky pro objekty, které jsou registrované. Tato popisná metadata se zkopíruje do služby Data Catalog společně s strukturální metadata.

## <a name="include-previews"></a>Zahrnout verze Preview
Ve výchozím nastavení je pouze metadata extrahována ze zdroje dat a zkopírovat do služby Data Catalog, ale které zdroj dat je často snazší když zobrazujete ukázková data, která obsahuje.

Pomocí nástroje pro registraci zdroje dat katalogu Data Catalog můžete zahrnout náhled snímek dat v každém tabulky a zobrazení, která je zaregistrovaná. Pokud zvolíte možnost zahrnutí náhledů během registrace, zahrnuje nástroj pro registraci až 20 záznamů z každé tabulky a zobrazení. Tento snímek se pak zkopíruje do katalogu spolu s metadaty strukturální a popisný.

> [!NOTE]
> Široké tabulky s velkým počtem sloupců může mít méně než 20 záznamů, které jsou zahrnuty v jejich náhled.
>
>

## <a name="include-data-profiles"></a>Zahrnují data profily
Stejně jako verze Preview včetně může poskytnout cenné kontext pro uživatele, kteří vyhledávat zdroje dat v katalogu Data Catalog, včetně dat profilu můžete usnadnit práci pochopit zjištěných datové zdroje.

Pomocí nástroje pro registraci zdroje dat katalogu Data Catalog, můžete zahrnout profil data pro jednotlivé tabulky a zobrazení, která je zaregistrovaná. Pokud se rozhodnete zahrnout profil dat během registrace, nástroj pro registraci zahrnuje souhrnné statistiky o data v každé tabulce a zobrazení, včetně:

* Počet řádků a velikost dat v objektu.
* Datum poslední aktualizace dat a schématu objekt.
* Počet záznamů hodnotu null a odlišné hodnoty pro sloupce.
* Minimum, maximum, průměr a směrodatnou odchylku hodnoty pro sloupce.

Tyto statistické údaje jsou pak zkopíruje do katalogu spolu s metadaty strukturální a popisný.

> [!NOTE]
> Text a datum sloupce nezahrnují statistiku průměr či směrodatná odchylka v jejich data profilu.
>
>

## <a name="update-registrations"></a>Aktualizovat registrace
Registrace zdroje dat je zjistitelný v katalogu Data Catalog při použití metadat a volitelné preview extrahovat během registrace. Pokud zdroj dat je třeba aktualizovat v katalogu (například pokud došlo ke změně schématu objektu, původně vyloučené tabulky by měly být zahrnuty nebo chcete aktualizovat data, která je součástí verze Preview), nástroj registrace zdroje dat můžete znovu spustit.

Opakováním registrace zdroj dat již zaregistrován provede operaci sloučení "upsert": stávající objekty jsou aktualizovány a vytvoření nových objektů. Veškerá metadata uživatele prostřednictvím portálu katalogu Data Catalog poskytuje zůstanou zachovány.

## <a name="summary"></a>Souhrn
Protože kopíruje strukturální a popisný metadat ze zdroje dat do katalogu služby, registrace zdroje dat v katalogu Data Catalog usnadňuje data vyhledat a pochopit. Po registraci zdroje dat můžete opatřit poznámkami, spravovat a zjistit pomocí portálu katalogu Data Catalog.

## <a name="next-steps"></a>Další kroky
Další informace o registraci zdrojů dat najdete v tématu [Začínáme s Azure Data Catalog](data-catalog-get-started.md) kurzu.
