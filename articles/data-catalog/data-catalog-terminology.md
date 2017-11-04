---
title: "Terminologie služby Azure Data Catalog | Microsoft Docs"
description: "Tento článek obsahuje úvod do konceptů a termínů používaných v dokumentaci k Azure Data Catalog."
services: data-catalog
documentationcenter: 
author: steelanddata
manager: NA
editor: 
tags: 
ms.assetid: 6fec74d9-4a3c-4b4b-88ba-cad5ad143331
ms.service: data-catalog
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 10/01/2017
ms.author: maroche
ms.openlocfilehash: 8cb24357bb24c48dceda714a040427fcc0c0ba4d
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/03/2017
---
# <a name="azure-data-catalog-terminology"></a>Terminologie služby Azure Data Catalog
## <a name="catalog"></a>Katalogu
Azure Data Catalog je metadata cloudové úložiště v data, která se dají registrovat zdroje a data prostředků. Katalog slouží jako umístění centrální úložiště pro strukturální metadata extrahovaná ze zdroje dat a pro popisná metadata přidané uživateli.

## <a name="data-source"></a>Zdroj dat
Zdroj dat je systém nebo kontejner, který spravuje datových prostředků. Mezi příklady patří databáze serveru SQL, Oracle – databáze, databáze SQL Server Analysis Services (tabulkové nebo multidimenzionální) a servery SQL Server Reporting Services.

## <a name="data-asset"></a>Datový prostředek
Datové prostředky jsou objekty obsažené v zdroje dat, které lze registrovat pomocí katalogu. Příklady tabulek systému SQL Server a zobrazení, Oracle tabulek a zobrazení, SQL Server Analysis Services míry, dimenze a klíčové ukazatele výkonu a sestav SQL Server Reporting Services.

## <a name="data-asset-location"></a>Prostředek umístění dat
Katalog ukládá umístění zdroje dat nebo datový prostředek, který slouží k připojení ke zdroji pomocí klientské aplikace. Formát a podrobnosti o umístění lišit v závislosti na typu zdroje dat. Například tabulku systému SQL Server lze identifikovat podle jeho součástí čtyři název – název serveru, název databáze, název schématu, název objektu –, zatímco SQL Server Reporting Services sestavy lze identifikovat podle jeho adresa URL.

## <a name="structural-metadata"></a>Strukturální metadata
Strukturální metadata jsou metadata extrahovaná ze zdroje dat, který popisuje strukturu datový prostředek. To zahrnuje umístění prostředky, jeho název objektu a typu a další vlastnosti specifické pro typ. Například strukturální metadata pro tabulky a zobrazení obsahuje názvy a typy dat pro sloupce objektu.

## <a name="descriptive-metadata"></a>Popisná metadata
Popisná metadata jsou metadata, která popisuje účel nebo záměr datový prostředek. Obvykle je přidaný popisná metadata uživatelé katalogu pomocí portálu Azure Data Catalog, ale jeho lze také extrahovat ze zdroje dat během registrace. Například nástroj pro registraci Azure Data Catalog popisy extrahuje z vlastnosti Popis na SQL Server Analysis Services a SQL Server Reporting Services a z [ms_description rozšířené vlastnosti](https://technet.microsoft.com/library/ms190243.aspx) v systému SQL Databáze serveru, pokud tyto vlastnosti jsou vyplněna s hodnotami.

## <a name="request-access"></a>Požádat o přístup
Popisná metadata datový prostředek může zahrnovat informace o tom, jak požádat o přístup ke zdroji dat nebo datový prostředek. Tyto informace se zobrazí asset umístění dat a může obsahovat jednu nebo více z následujících možností:

* E-mailovou adresu uživatele nebo tým odpovědný za poskytnutí přístupu ke zdroji dat.
* Adresa URL zdokumentovaných proces, který uživatelé musí postupovat podle pokynů k získání přístupu ke zdroji dat.
* Adresa URL přístupu a identit a nástroj pro správu (jako je například Microsoft Identity Manager), můžete použít k získání přístupu ke zdroji dat.
* Volné položku, která popisuje, jak mohou uživatelé získat přístup ke zdroji dat.

## <a name="preview"></a>Preview
Náhled v Azure Data Catalog je snímek až 20 záznamů, které se dají extrahována ze zdroje dat během registrace a uložené v katalogu asset metadatům data. Ve verzi preview může pomoci lépe porozumět uživatelé, kteří je zjistili datový prostředek jeho funkci a účel. Jinými slovy zobrazuje ukázkových dat může být více než zobrazuje jenom názvy sloupců a datové typy.
Verze Preview jsou podporovány pouze pro tabulky a zobrazení a musí být explicitně vybraný uživatelem během registrace.

## <a name="data-profile"></a>Data profilu
Profil dat v Azure Data Catalog je snímek úroveň tabulky a sloupce metadata o registrovaných datový prostředek, můžete extrahovat ze zdroje dat během registrace a uložené v katalogu asset metadatům data. Může pomoci data profilu uživatele, kteří je zjistili datový prostředek lépe pochopit jeho funkci a účel. Podobně jako verze Preview, profily data musí být explicitně určen uživatele během registrace.

> [!NOTE]
> Extrahování dat profilu může být nákladná operace pro rozsáhlé tabulky a zobrazení a může výrazně prodloužit doba nutná k registraci zdroje dat.
>
>

## <a name="user-perspective"></a>Pohledu uživatele
V Azure Data Catalog může každý uživatel, poskytují popisná metadata pro prostředek registrované datové. Každý uživatel má odlišné perspektivou na data a jeho použití. Například správcem odpovědným za serveru může zadejte podrobnosti o jeho smlouvu o úrovni služeb (SLA) nebo zálohování; data steward může poskytují odkazy na dokumentaci pro firmu zpracovává data podporuje; a analytik může poskytnout popis v podmínkách, které jsou nejdůležitější jiných analytikům a může být nejhodnotnější pro tyto uživatele, kteří potřebují vyhledatelné a pochopitelné data.

Každý z těchto perspektiv jsou ze své podstaty cenné a s Azure Data Catalog každý uživatel může poskytnout informace, které má smysl, zatímco všichni uživatelé můžete použít tyto informace k pochopení dat a jeho účel.

## <a name="expert"></a>odborné
Odborník je uživatel, který byl identifikován tak, že má informovaně "odborné" perspektivy pro datový prostředek. Každý uživatel, můžete přidat sami nebo jinému uživateli jako odborník pro určitý prostředek. Není uvedena jako odborník nesou žádná zvláštní oprávnění v Azure Data Catalog; umožňuje uživatelům snadno najít tyto perspektivy, které nejvíce mohou být užitečné, pokud kontrola popisná metadata pro určitý prostředek.

## <a name="owner"></a>Vlastník
Vlastníkem je uživatel, který má další oprávnění pro správu datový prostředek v Azure Data Catalog. Uživatelé mohou převzít vlastnictví registrovaných datových prostředků a vlastníky můžete přidat další uživatele jako spoluvlastníci. Další informace najdete v části [jak spravovat datové prostředky](data-catalog-how-to-manage.md)  

> [!NOTE]
> Vlastnictví a správu jsou k dispozici pouze v Azure Data Catalog, Standard Edition.
>
>

## <a name="registration"></a>Registrace
Registrace spočívá v extrahování dat asset metadat ze zdroje dat a kopírování do služby Azure Data Catalog. Datové prostředky, které byly zaregistrovány můžete pak opatřeny poznámkami a zjistit.

## <a name="see-also"></a>Viz také
* [Co je Azure Data Catalog?](data-catalog-what-is-data-catalog.md) – Tento článek obsahuje přehled služby Azure Data Catalog, hodnotu, kterou poskytuje a scénáře, které podporuje.
* [Začínáme s Azure Data Catalog](data-catalog-get-started.md) – Tento článek obsahuje začátku do konce kurz, který ukazuje, jak používat Azure Data Catalog pro zjišťování zdroje.  
