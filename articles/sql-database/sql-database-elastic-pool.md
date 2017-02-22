---
title: "Co je elastický fond Azure? | Dokumentace Microsoftu"
description: "Spravujte stovky nebo tisíce databází pomocí fondu. Jednu cenu sady jednotek výkonu můžete distribuovat napříč fondem. Přidávejte a odebírejte databáze podle libosti."
keywords: "elastický fond, databáze sql"
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: b46e7fdc-2238-4b3b-a944-8ab36c5bdb8e
ms.service: sql-database
ms.custom: multiple databases
ms.devlang: NA
ms.date: 01/11/2017
ms.author: CarlRabeler
ms.workload: data-management
ms.topic: get-started-article
ms.tgt_pltfrm: NA
translationtype: Human Translation
ms.sourcegitcommit: 2681dd3792a351fecc0c72eb7fe546113a451d24
ms.openlocfilehash: 412c3f9c34e399ecdffd939b5b72e687a847b8e1


---
# <a name="what-is-an-azure-elastic-pool"></a>Co je elastický fond Azure?
Elastické fondy SQL DB poskytují jednoduché a nákladově efektivní řešení správy výkonnostních cílů pro více databází, jejichž vzory používání se značně liší a nelze je předvídat.

> [!NOTE]
> Elastické fondy jsou obecně dostupné ve všech oblastech Azure s výjimkou oblasti Západní Indie, kde jsou aktuálně ve verzi Preview.  Verze GA pro elastické fondy se v této oblasti objeví co nejdřív.
>
>

## <a name="how-it-works"></a>Jak to funguje
Běžným vzorem aplikace SaaS je model databáze s jedním tenantem: každému zákazníkovi se přidělí vlastní databáze. Každý zákazník (databáze) má nepředvídatelné požadavky na prostředky – paměť, V/V a procesor. Jak můžete s tak velkými výkyvy poptávky efektivně a zároveň i nákladově efektivně přidělovat prostředky? Tradičně máte dvě možnosti: (1) přidělovat více prostředků na základě využití ve špičce a platit navíc, nebo (2) přidělovat méně prostředků a snížit tak náklady na úkor výkonu a spokojenosti zákazníků během špiček. Elastické fondy tento problém řeší tím, že zajišťují, aby databáze dostávaly potřebné prostředky výkonu v době, kdy je potřebují. Poskytují jednoduchý mechanismus přidělování prostředků v mezích předvídatelného rozpočtu. Další informace o návrhových schématech aplikací SaaS využívajících elastické fondy najdete v tématu [Návrhová schémata pro víceklientské aplikace SaaS využívající službu Azure SQL Database](sql-database-design-patterns-multi-tenancy-saas-applications.md).

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Elastic-databases-helps-SaaS-developers-tame-explosive-growth/player]
>

Ve službě SQL Database je relativní míra schopnosti databáze zpracovávat požadavky na prostředky vyjádřena v jednotkách DTU (Database Transaction Unit) pro izolované databáze a v elastických jednotkách DTU (eDTU) pro databáze v elastickém fondu. Další informace o DTU a eDTU najdete v tématu [Úvod do služby SQL Database](sql-database-technical-overview.md).

Fond obdrží stanovený počet eDTU za stanovenou cenu. V rámci fondu disponují jednotlivé databáze flexibilní možností automatického škálování v rámci stanovených parametrů. V případě velkého zatížení může databáze spotřebovávat více eDTU, aby splnila požadavky. Databáze při nízkém zatížení spotřebovávají méně eDTU a databáze bez zatížení nespotřebovávají žádné eDTU. Zřizováním prostředků pro celý fond, a nikoli pro jednotlivé databáze, se úkoly správy zjednodušují. Kromě toho můžete předem odhadnout náklady na fond.

Další eDTU lze do existujícího fondu přidat bez jakéhokoli výpadku databáze, nicméně možná bude nutné databáze přesunout za účelem zajištění dalších výpočetních prostředků pro vyhrazení nových eDTU. Podobně platí, že pokud již přidané eDTU nejsou potřebné, lze je z existujícího fondu kdykoli odebrat.

Navíc můžete databáze do fondu přidávat nebo je z něj odebírat. Pokud databáze podle předpokladu nedostatečně využívá prostředky, odeberte ji.

## <a name="which-databases-go-in-a-pool"></a>Jaké databáze patří do fondu?
![Databáze SQL sdílející eDTU v elastickém fondu.][1]

Skvělými kandidáty pro elastické fondy jsou typicky databáze, u kterých se období aktivity střídají s obdobími nečinnosti. V předchozím příkladu můžete vidět aktivitu izolované databáze, 4 databází a konečně aktivitu elastického fondu s 20 databázemi. Databáze, jejichž aktivita v průběhu času kolísá, jsou skvělými kandidáty pro elastické fondy, protože nejsou všechny aktivní ve stejnou dobu a mohou sdílet eDTU. Ne všechny databáze tomuto vzoru odpovídají. Databáze se stabilnějšími požadavky na prostředky se lépe hodí pro úrovně služeb Basic, Standard a Premium, kde se prostředky přidělují jednotlivě.

[Cenové a výkonové požadavky fondu elastické databáze](sql-database-elastic-pool-guidance.md).

## <a name="edtu-and-storage-limits-for-elastic-pools"></a>Omezení eDTU a úložiště pro elastické fondy

Následující tabulka popisuje vlastnosti úrovní elastických fondů Basic, Standard a Premium.

[!INCLUDE [SQL DB service tiers table for elastic pools](../../includes/sql-database-service-tiers-table-elastic-pools.md)]

Pokud se využívají všechny DTU elastického fondu, pak každá databáze ve fondu obdrží stejné množství prostředků ke zpracování dotazů.  Služba SQL Database poskytuje spravedlivé sdílení prostředků mezi databázemi tím, že zajišťuje rovnoměrné rozdělení výpočetního času. Pokud je minimální počet DTU na databázi nastaven na nenulovou hodnotu, pro každou databázi bude garantováno spravedlivé sdílení prostředků elastického fondu je společně s libovolným množstvím prostředků.

## <a name="elastic-pool-properties"></a>Vlastnosti elastických fondů

Následující tabulka popisuje omezení pro elastické fondy a databáze ve fondu.

### <a name="limits-for-elastic-pools"></a>Limity pro elastické fondy
| Vlastnost | Popis |
|:--- |:--- |
| Úroveň služeb |Basic, Standard nebo Premium. Úroveň služeb určuje rozsah limitů výkonu a úložiště, které lze nakonfigurovat, a volby kontinuity podnikových procesů. Všechny databáze ve fondu mají stejnou úroveň služeb jako fond. Úroveň služeb se označuje také jako edice. |
| Počet eDTU na fond |Maximální počet eDTU, které databáze ve fondu sdílet. Celkový počet eDTU, které databáze ve fondu využívají v jeden okamžik, nemůže tento limit překročit. |
| Maximální velikost úložiště na fond (GB) |Maximální velikost úložiště (v GB), které mohou databáze ve fondu sdílet. Celková velikost úložiště, které databáze ve fondu využívají, nemůže překročit tento limit. Tento limit se určuje podle počtu eDTU na fond. Pokud je tento limit překročen, všechny databáze budou jen pro čtení. Maximální velikost úložiště na fond odkazuje na maximální velikost datových souborů ve fondu a nezahrnuje místo, které využívají soubory protokolu. |
| Maximální počet databází na fond |Maximální počet povolených databází na fond. |
| Maximální počet souběžných pracovních procesů na fond |Maximální počet souběžných pracovních procesů (požadavků) dostupných pro všechny databáze ve fondu. |
| Maximální počet souběžných přihlášení na fond |Maximální počet souběžných přihlášení pro všechny databáze ve fondu. |
| Maximální počet souběžných relací na fond |Maximální počet relací dostupných pro všechny databáze ve fondu. |

### <a name="limits-for-pooled-databases"></a>Omezení pro databáze ve fondu
| Vlastnost | Popis |
|:--- |:--- |
| Maximální počet eDTU na databázi |Maximální počet eDTU, které může využívat libovolná databáze ve fondu za předpokladu, že jsou dostupné v závislosti na využití ostatními databázemi ve fondu.  Maximální počet eDTU na databázi není garancí prostředků pro databázi.  Toto nastavení je globální a platí pro všechny databáze ve fondu. Nastavte maximální počet eDTU na databázi dostatečně vysoký, aby databáze zvládly využití ve špičkách. Očekává se určitý stupeň předimenzování, protože fond obecně předpokládá studené a horké vzory používání, při nichž nenastávají špičky u všech databází ve stejnou chvíli. Předpokládejme například, že využití ve špičce na databázi je 20 eDTU a špička v jednu chvíli nastává pouze u 20 % ze 100 databází ve fondu.  Pokud je maximální počet eDTU na databázi nastaven na 20 eDTU, je rozumné fond 5krát předimenzovat a nastavit počet eDTU na fond na 400. |
| Minimální počet eDTU na databázi |Minimální počet eDTU garantovaných pro každou databázi ve fondu.  Toto nastavení je globální a platí pro všechny databáze ve fondu. Minimální počet eDTU na databázi může být nastaven na 0, což je i výchozí hodnota. Tato vlastnost je nastavena na libovolnou hodnotu mezi 0 a průměrným využitím eDTU na databázi. Násobek počtu databází ve fondu a minimálním počtem eDTU na databázi nemůže překročit počet eDTU na fond.  Pokud je například ve fondu 20 databází a minimální počet eDTU na databázi je nastaven na 10 eDTU, pak musí být počet eDTU na fond alespoň 200 eDTU. |
| Maximální velikost úložiště na databázi (GB) |Maximální velikost úložiště pro databázi ve fondu. Databáze ve fondu sdílí úložiště fondu, proto je velikost úložiště databáze omezena na menší z hodnot zbývajícího úložiště fondu a maximální velikosti úložiště na databázi. Maximální velikost úložiště na databázi odkazuje na maximální velikost datových souborů a nezahrnuje místo, které využívají soubory protokolu. |

## <a name="elastic-jobs"></a>Elastické úlohy
U fondu jsou úlohy správy zjednodušené díky spouštění skriptů v **[elastických úlohách](sql-database-elastic-jobs-overview.md)**. Elastická úloha eliminuje většinu únavných úkolů spojených s velkým počtem databází. Pro začátek si přečtěte téma [Začínáme s elastickými úlohami](sql-database-elastic-jobs-getting-started.md).

Další informace o ostatních databázových nástrojích pro práci s více databázemi najdete v tématu [Horizontální navýšení kapacity se službou Azure SQL Database](sql-database-elastic-scale-introduction.md).

## <a name="business-continuity-features-for-databases-in-a-pool"></a>Funkce kontinuity podnikových procesů pro databáze ve fondu
Databáze ve fondu obecně podporují stejné [funkce provozní kontinuity](sql-database-business-continuity.md), jaké jsou dostupné pro izolované databáze.

### <a name="point-in-time-restore"></a>Obnovení k určitému bodu v čase
Obnovení k určitému bodu v čase využívá automatické zálohování databází k obnovení databáze ve fondu do stavu v konkrétním bodu v čase. Viz [Obnovení k určitému bodu v čase](sql-database-recovery-using-backups.md#point-in-time-restore).

### <a name="geo-restore"></a>Geografické obnovení
Geografické obnovení poskytuje výchozí možnost zotavení v případě nedostupnosti databáze z důvodu incidentu v oblasti, ve které je databáze hostovaná. Viz [Obnovení služby Azure SQL Database a převzetí služeb při selhání sekundární lokalitou](sql-database-disaster-recovery.md).

### <a name="active-geo-replication"></a>Aktivní geografická replikace
Pro aplikace s mnohem vyššími požadavky na zotavení, které geografické obnovení nemůže nabídnout, můžete nakonfigurovat aktivní geografickou replikaci pomocí webu [Azure Portal](sql-database-geo-replication-portal.md), prostředí [PowerShell](sql-database-geo-replication-powershell.md) nebo jazyka [Transact-SQL](sql-database-geo-replication-transact-sql.md).

## <a name="additional-resources"></a>Další zdroje
* [Videokurz Microsoft Virtual Academy o možnostech elastické databáze ve službě Azure SQL Database](https://mva.microsoft.com/en-US/training-courses/elastic-database-capabilities-with-azure-sql-db-16554)

<!--Image references-->
[1]: ./media/sql-database-elastic-pool/databases.png



<!--HONumber=Jan17_HO2-->


