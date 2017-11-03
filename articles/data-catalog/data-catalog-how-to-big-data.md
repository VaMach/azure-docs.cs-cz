---
title: "Jak pracovat se zdroji dat 'velkých objemů dat. | Microsoft Docs"
description: "Postupy: článek zvýraznění vzory pro pomocí Azure Data Catalog, velké objemy dat, datových zdrojů, včetně Azure Blob Storage, Azure Data Lake a Hadoop HDFS."
services: data-catalog
documentationcenter: 
author: steelanddata
manager: NA
editor: 
tags: 
ms.assetid: 626d1568-0780-4726-bad1-9c5000c6b31a
ms.service: data-catalog
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 08/15/2017
ms.author: maroche
ms.openlocfilehash: 001d80ce42f0e87276e59d70dffb75eb561d96cd
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-work-with-big-data-sources-in-azure-data-catalog"></a>Jak pracovat s velkými objemy dat zdroje v Azure Data Catalog
## <a name="introduction"></a>Úvod
**Microsoft Azure Data Catalog** je plně spravovaná Cloudová služba, která slouží jako systém registrace a systém pro zjišťování zdrojů dat organizace. Všechny o pomoc osoby zjišťovat, pochopit a používat zdroje dat a pomáhají organizacím získat větší hodnotu ze svých existujících zdrojů dat, včetně velkých objemů dat je.

**Azure Data Catalog** podporuje registraci Azure Blog úložiště objektů BLOB a adresáře a také Hadoop HDFS souborů a adresářů. Částečně strukturovaných povahu těchto zdrojů dat poskytuje flexibilitu. Však sám registrace je s největším hodnotu **Azure Data Catalog**, uživatelé, musíte zvážit, jak jsou uspořádány zdroje dat.

## <a name="directories-as-logical-data-sets"></a>Adresářů jako logické datové sady
Běžný vzor uspořádání zdroje velkých objemů dat je adresáře považovat za logické datové sady. Nejvyšší úrovně adresáře se používají k definování sady dat, zatímco podsložky Definujte oddíly a ukládat soubory, které obsahují samotná data.

Příkladem tohoto vzoru může být:

    \vehicle_maintenance_events
        \2013
        \2014
        \2015
            \01
                \2015-01-trailer01.csv
                \2015-01-trailer92.csv
                \2015-01-canister9635.csv
                ...
    \location_tracking_events
        \2013
        ...

V tomto příkladu vehicle_maintenance_events a location_tracking_events představují logické datové sady. Každou z těchto složek obsahuje datové soubory, které jsou uspořádané podle rok a měsíc do podsložky. Každou z těchto složek mohou obsahovat stovky nebo tisíce souborů.

V tomto vzoru registraci jednotlivých souborů s **Azure Data Catalog** pravděpodobně nemá smysl. Místo toho zaregistrujte adresáře, které představují datových sad, které smysl uživatelům práci s daty.

## <a name="reference-data-files"></a>Referenční datové soubory
Doplňkové vzor je ukládání referenční datové sady jako jednotlivé soubory. Tyto sady dat může být představit jako "malá" straně velkých objemů dat a jsou často podobná dimenze ve model analytická data. Referenční datové soubory obsahuje záznamy, které se používají k poskytování kontext pro hromadné souborů data uložená na jiném místě v úložišti velkých objemů dat.

Příkladem tohoto vzoru může být:

    \vehicles.csv
    \maintenance_facilities.csv
    \maintenance_types.csv

Když analytik nebo data vědecký pracovník pracuje s daty součástí větší struktury adresářů, data v těchto referenčních souborů slouží k poskytují podrobnější informace pro entity, které jsou uvedené jenom podle názvu nebo ID v větší datové sady.

V tomto vzoru, má smysl registraci jednotlivých referenčních dat souborů s **Azure Data Catalog**. Každý soubor představuje datové sady a každé z nich můžou opatřeny poznámkami a zjištěné jednotlivě.

## <a name="alternate-patterns"></a>Alternativní vzory
Vzory popsané v předchozí části jsou právě dva možné způsoby, které může být uspořádaný velkých objemů dat úložiště, ale každý implementace se liší. Bez ohledu na to, jak jsou strukturovaná zdroje dat, při registraci zdroje velkých objemů dat s **Azure Data Catalog**, soustředit na registraci soubory a adresáře, které představují datových sad, které jsou hodnoty s ostatními uživateli v vaší organizace. Registrace všechny soubory a adresáře může zaplnit katalogu, což těžší uživatelům najít požadované.

## <a name="summary"></a>Souhrn
Registrace zdroje dat s **Azure Data Catalog** jejich jednodušší zjišťovat a pochopit. Registrace a zadávání poznámek k velkých objemů dat souborů a adresářů, které představují logické datové sady, může pomoci uživatelům najít a použít zdroje velkých objemů dat, které potřebují.
