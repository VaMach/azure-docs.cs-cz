---
title: "Úvod do služby Azure Data Catalog | Dokumentace Microsoftu"
description: "Tento článek obsahuje přehled služby Microsoft Azure Data Catalog, včetně jejích funkcí a potíží, které je navržena řešit. Data Catalog poskytuje možnosti, které umožní každému uživateli – od analytiků přes vědce zabývající se zpracováním dat po vývojáře – zaregistrovat, zjišťovat, pochopit a využívat zdroje dat."
services: data-catalog
documentationcenter: 
author: steelanddata
manager: NA
editor: 
tags: 
ms.assetid: cc733907-17ec-4153-9f0c-5b3754b2db19
ms.service: data-catalog
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 05/15/2017
ms.author: maroche
ms.translationtype: Human Translation
ms.sourcegitcommit: 538f282b28e5f43f43bf6ef28af20a4d8daea369
ms.openlocfilehash: fb8f43f5bb5725da30e67cdf5d7b066fe40ed003
ms.contentlocale: cs-cz
ms.lasthandoff: 04/07/2017


---
# <a name="what-is-azure-data-catalog"></a>Co je Azure Data Catalog?
Azure Data Catalog je plně spravovaná cloudová služba, která umožňuje uživatelům zjišťovat zdroje dat, které potřebují a pochopit zdroje dat, které naleznou. Zároveň pomáhá organizacím získat větší hodnotu z jejich stávajících investic. Data Catalog poskytuje možnosti, které umožní každému uživateli – od analytiků přes vědce zabývající se zpracováním dat po vývojáře – zjišťovat, pochopit a využívat zdroje dat. Data Catalog dat obsahuje crowdsourcingový model metadat a poznámek a umožňuje všem uživatelům přispět svými znalostmi k vybudování komunity a kultury dat.

## <a name="discovery-challenges-for-data-consumers"></a>Problémy zjišťování pro spotřebitele dat
Tradičně je zjišťování zdrojů podnikových dat organický proces založený na kmenových znalostech. To představuje mnoho výzev pro společnosti, které chtějí získat maximální hodnotu ze svých prostředků informací.

* Uživatelé neví, že zdroje dat existují, pokud s nimi nepřicházejí do styku jako součástí jiného procesu; k dispozici není žádné centrální umístění, kde jsou zdroje dat registrovány.
* Pokud uživatel nezná umístění zdroje dat, nemůže se k těmto datům připojit pomocí klientské aplikace; možnosti spotřeby dat vyžadují, aby uživatelé znali připojovací řetězec nebo cestu.
* Pokud uživatel nezná umístění dokumentace zdroje dat, nemůže rozumět zamýšlenému použití dat; zdroje dat a dokumentace se nacházejí na různých místech a používají se v různých prostředích.
* Jestliže má uživatel dotazy týkající se informačního prostředku, musí vyhledat odborníka nebo tým odpovědný za příslušná data a zapojit tyto odborníky offline; neexistuje žádné explicitní spojení mezi daty a zaměstnanci s odbornými perspektivami na jejich použití.
* Pokud uživatel nerozumí procesu pro vyžádání přístupu ke zdroji dat, zjištění zdroje dat a jeho dokumentace mu stále ještě neumožní přístup k datům, která požaduje.

## <a name="discovery-challenges-for-data-producers"></a>Problémy zjišťování pro producenty dat
Zatímco spotřebitelé dat se potýkají s těmito výzvami, uživatelé odpovědní za vytváření a správu informačních prostředků se potýkají s vlastními výzvami.

* Zadávání poznámek ke zdrojům dat s popisnými metadaty je často ztracené úsilí; klientské aplikace obvykle ignorují popisy uložené ve zdroji dat.
* Vytváření dokumentace pro zdroje dat je často zbytečná námaha; udržování dokumentace synchronizované se zdrojem dat je neustávající odpovědnost a uživatelé nemají důvěru v dokumentaci, neboť je často považována za zastaralou.
* Omezení přístupu ke zdroji dat a zajištění, aby spotřebitelé dat věděli, jak požádat o přístup, je neustávající výzva.

Vytváření a správa dokumentace pro zdroj dat je složité a časově náročné. O to větší je výzva ohledně snadného zpřístupnění této dokumentace pro každého uživatele, který používá příslušný zdroj dat.

Když se tyto výzvy zkombinují , představují významnou překážkou pro společnosti, které chtějí podněcovat a podporovat používání a pochopení podnikových dat.

## <a name="azure-data-catalog-can-help"></a>Azure Data Catalog může pomoci
Katalog Data Catalog je určen k řešení těchto problémů a umožňuje podnikům získat větší hodnotu ze stávajících prostředků. Data Catalog pomáhá tím, že činí zdroje dat snadno zjistitelné a srozumitelné pro uživatele, kteří potřebují data, která spravují.

Data Catalog poskytuje službu na principu cloudu, do níž lze zaregistrovat zdroj dat. Data zůstávají uložena ve stávajícím umístění, ale do katalogu Data Catalog je přidána kopie metadat spolu s odkazem na umístění zdroje dat. Tato metadata jsou také indexována, aby byl každý zdroj dat snadno zjistitelný prostřednictvím vyhledávání a aby byla srozumitelná uživatelům, kteří je zjistili.

Po zaregistrování zdroje dat mohou být jeho metadata rozšířena uživatelem, který provedl registraci, nebo jinými uživateli v podniku. Každý uživatel může opatřit poznámkami zdroj dat tím, že přidá popisy, značky nebo další metadata, například dokumentaci a procesy pro žádosti o přístup ke zdroji dat. Tato popisná metadata doplňují strukturální metadata (například názvy sloupců a typy dat) zaregistrované ze zdroje dat.

Primárním účelem registrace zdrojů dat je zjišťování a porozumění zdrojům, a jejich používání. Pokud podnikoví uživatelé potřebují pro svoji činnost data (což by mohly být aplikace business intelligence, vývoj aplikací, vědecké zpracování dat nebo jiný úkol, ve kterém jsou vyžadována správná data), mohou používat zkušenosti se zjišťováním v katalogu Data Catalog k rychlému vyhledání dat, která odpovídají jejich potřebám, pochopení dat k vyhodnocení jejich vhodnosti pro daný účel a spotřebě těchto dat tím, že zdroj dat otevřou v upřednostňovaném nástroji. Současně umožňuje Data Catalog uživatelům přispívat do katalogu označováním, dokumentováním a zadáváním poznámek ke zdrojům dat, které jsou již zaregistrovány, a registrováním nových zdrojů dat, které lze poté zjistit, pochopit a spotřebovat komunitou uživatelů katalogu.

![Možnosti katalogu Data Catalog](./media/data-catalog-what-is-data-catalog/data-catalog-capabilities.png)

## <a name="get-started-with-data-catalog"></a>Začínáme s katalogem Data Catalog
Chcete-li začít s katalogem Data Catalog ještě dnes, navštivte stránky [www.azuredatacatalog.com](https://www.azuredatacatalog.com).

Příručka Začínáme je k dispozici [zde](data-catalog-get-started.md).

## <a name="learn-more-about-data-catalog"></a>Další informace o katalogu Data Catalog
Další informace o možnostech katalogu Data Catalog naleznete v tématu:

* [Postup registrace zdrojů dat](data-catalog-how-to-register.md)
* [Zjišťování zdrojů dat](data-catalog-how-to-discover.md)
* [Postup přidání poznámek ke zdrojům dat](data-catalog-how-to-annotate.md)
* [Postup dokumentování zdrojů dat](data-catalog-how-to-documentation.md)
* [Jak se připojit ke zdrojům dat](data-catalog-how-to-connect.md)
* [Jak pracovat s velkým objemem dat](data-catalog-how-to-big-data.md)
* [Jak spravovat datové prostředky](data-catalog-how-to-manage.md)
* [Jak nastavit obchodní glosář](data-catalog-how-to-business-glossary.md)
* [Nejčastější dotazy](data-catalog-frequently-asked-questions.md)

