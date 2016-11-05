---
title: 'Výkon a možnosti služby SQL Database: Úrovně služeb | Microsoft Docs'
description: Porovnejte výkon a kontinuitu podnikových procesů jednotlivých úrovní služby SQL, abyste mohli vyvážit náklady a výkon v průběhu dalšího růstu.
keywords: možnosti databáze, výkon databáze
services: sql-database
documentationcenter: ''
author: CarlRabeler
manager: jhubbard
editor: CarlRabeler

ms.service: sql-database
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 08/10/2016
ms.author: carlrab

---
# Výkon a možnosti služby SQL Database: Co je k dispozici v jednotlivých úrovních služeb
[Azure SQL Database](sql-database-technical-overview.md) nabízí tři úrovně služby s více úrovněmi výkonu pro zpracování různých úloh. Každá úroveň výkonu poskytuje rostoucí sadu prostředků navržených k poskytnutí stále větší propustnosti. Každou databázi můžete spravovat v její vlastní [úrovni služby](sql-database-service-tiers.md#standalone-database-service-tiers-and-performance-levels) s vlastní úrovní výkonu. Můžete také spravovat více databází v [elastickém fondu](sql-database-service-tiers.md#elastic-pool-service-tiers-and-performance-in-edtus) se sdílenou sadou prostředků. Prostředky dostupné pro samostatné databáze jsou vyjádřeny v jednotkách DTU (Database Transaction Unit); prostředky dostupné pro elastické fondy jsou vyjádřeny v elastických jednotkách DTU (eDTU). Další informace o jednotkách DTU a eDTU najdete v tématu [Co je DTU](sql-database-what-is-a-dtu.md). 

V obou případech jsou dostupné úrovně služby **Basic**, **Standard** a **Premium**. Možnosti databáze v těchto úrovních jsou podobné pro samostatné databáze i elastické fondy, ale pro elastické fondy existují ještě další důležité okolnosti. Tento článek obsahuje podrobné informace o úrovních služeb pro samostatné databáze i pro elastické fondy.

## Úrovně služeb a možnosti databází
Úrovně služeb Basic, Standard a Premium mají všechny dostupnost SLA 99,99 % a nabízejí předvídatelný výkon, flexibilní možnosti kontinuity podnikových procesů, funkce zabezpečení a fakturaci po hodinách. Následující tabulka obsahuje příklady úrovní služeb vhodných pro různé zátěže a aplikace.

| Úroveň služeb | Cílová zátěž |
| --- | --- |
| **Basic** |Nejvhodnější pro malé databáze provádějící obvykle jednu aktivní operaci najednou. Patří sem například databáze používané pro vývoj a testování nebo méně rozsáhlé a zřídka používané aplikace. |
| **Standard** |Základní možnost pro většinu cloudových aplikací, podpora více souběžných dotazů. Příkladem mohou být webové aplikace nebo aplikace pro pracovní týmy. |
| **Premium** |Navrženo pro vysoké objemy transakcí, mnoho souběžných uživatelů a pro nejvyšší úroveň kontinuity podnikových procesů. Příkladem mohou být databáze podporující kritické podnikové procesy. |

> [!NOTE]
> Úrovně Web a Business byly ukončeny. Přečtěte si článek o [cenách SQL Database v edici Web a Business](https://azure.microsoft.com/pricing/details/sql-database/web-business/), chcete-li tyto úrovně dále používat.
> 
> 

## Úrovně služeb a úrovně výkonu pro samostatné databáze
Pro samostatné databáze jsou v každé úrovni služeb dostupné různé úrovně výkonu. Máte možnost zvolit si úroveň, která nejlépe vyhovuje vašim požadavkům na výkon. Pokud potřebujete škálování směrem nahoru nebo dolů, můžete snadno změnit úroveň služeb vaší databáze. Podrobnosti viz [Změna úrovně služeb a úrovně výkonu databáze](sql-database-scale-up.md).

Výkonové charakteristiky, které jsou zde uvedené, platí pro databáze vytvořené ve službě [SQL Database verze 12](sql-database-v12-whats-new.md). Bez ohledu na počet hostovaných databází, bude mít vaše databáze zaručenou sadu prostředků a očekávaný výkon databáze nebude ovlivněn.

[!INCLUDE [SQL DB service tiers table](../../includes/sql-database-service-tiers-table.md)]

> [!NOTE]
> Podrobné vysvětlení všech ostatních řádků v této tabulce úrovní služeb najdete v části [Možnosti a omezení úrovní služeb](sql-database-performance-guidance.md#service-tier-capabilities-and-limits).
> 
> 

## Úrovně služeb elastických fondů a měření výkonu pomocí eDTU
Vedle vytváření a škálování samostatných databází máte také možnost spravovat skupiny databází v [elastických fondech](sql-database-elastic-pool.md). Všechny databáze v elastickém fondu sdílejí společnou sadu prostředků. Výkonové charakteristiky se měří v *transakčních jednotkách elastické databáze* (eDTU). Stejně jako u samostatných databází můžete fondy vytvářet ve třech úrovních služeb: **Basic**, **Standard** a **Premium**. I pro fondy tyto tři úrovně definují omezení celkového výkonu a několik dalších vlastností.

Fondy umožňují databázím sdílet a využívat prostředky (měřené v jednotkách DTU), aniž by bylo nutné přiřazovat konkrétní úroveň výkonu všem databázím ve fondu. Například samostatná databáze ve fondu, který má úroveň Standard, může přecházet od využívání 0 eDTU až k maximální hodnotě eDTU, kterou jste pro danou databázi ve fondu nastavili. Fondy umožňují více databázím s různou zátěží efektivně využívat prostředky eDTU, které jsou dostupné pro celý fond. Podrobnosti viz [Cenové a výkonové požadavky fondu elastické databáze](sql-database-elastic-pool-guidance.md).

V následující tabulce jsou popsané charakteristiky úrovní služeb fondů.

[!INCLUDE [SQL DB service tiers table for elastic pools](../../includes/sql-database-service-tiers-table-elastic-db-pools.md)]

Každá databáze ve fondu současně dodržuje pravidla pro samostatné databáze určená danou úrovní služeb. Například fond s úrovní Basic má omezení maximálního počtu relací na fond 4800 – 28800, ale jednotlivé databáze ve fondu Basic mají omezení 300 relací.

## Výběr úrovně služeb
Při rozhodování o úrovni služby začněte určením, zda má být databáze samostatná nebo součástí elastického fondu. 

### Výběr úrovně služeb pro samostatnou databázi
Při rozhodování o úrovni služeb pro samostatnou databázi začněte určením databázové funkce, kterou je třeba zvolit pro vaši verzi databáze SQL:

* Velikost databáze (maximálně 2 GB pro Basic, maximálně 250 GB pro Standard a maximálně 500 GB až 1 TB pro Premium – v závislosti na úrovni výkonu)
* Období uchovávání záloh databáze (7 dní pro Basic, 35 dní pro Standard a 35 dní pro Premium)

Po určení edice databáze SQL jste připraveni k určení úrovně výkonu pro databázi (počet DTU). Můžete provést odhad a následně [dynamicky škálovat nahoru nebo dolů](sql-database-scale-up.md) na základě skutečných zkušeností. Můžete také použít [Kalkulačku DTU](http://dtucalculator.azurewebsites.net/) a odhadnout počet potřebných DTU. 

### Výběr úrovně služeb pro fond elastické databáze.
Při rozhodování o úrovni služeb pro fond elastické databáze začněte určením vlastností databáze, pro které musíte zvolit úroveň služby pro váš fond.

* Velikost databáze (2 GB pro Basic, 250 GB pro Standard a 500 GB pro Premium)
* Období uchovávání záloh databáze (7 dní pro Basic, 35 dní pro Standard a 35 dní pro Premium)
* Počet databází na fond (400 pro Basic, 400 pro Standard a 50 pro Premium)
* Maximální úložiště na fond (117 GB pro Basic, 1200 pro Standard a 750 pro Premium)

Po určení úrovně služeb pro váš fond jste připraveni k určení úrovně výkonu pro fond (eDTU). Můžete provést odhad a následně [dynamicky škálovat nahoru nebo dolů](sql-database-elastic-pool-manage-portal.md#change-performance-settings-of-a-pool) na základě skutečných zkušeností. Jako pomoc při určení horního limitu fondu můžete k odhadu počtu DTU potřebných pro každou databázi ve fondu použít [Kalkulačku DTU](http://dtucalculator.azurewebsites.net/).

## Další kroky
* Další informace o cenách pro jednotlivé úrovně služeb najdete v tématu [SQL Database – ceny](https://azure.microsoft.com/pricing/details/sql-database/).
* Přečtěte si další podrobnosti o [elastických fondech](sql-database-elastic-pool-guidance.md) a o [cenových a výkonových požadavcích elastických fondů](sql-database-elastic-pool-guidance.md).
* Zjistěte, jak [monitorovat, spravovat a měnit velikost elastických fondů](sql-database-elastic-pool-manage-portal.md) a jak [sledovat výkon samostatných databází](sql-database-single-database-monitor.md).
* Teď, když už máte přehled o úrovních služeb SQL Database, můžete si je vyzkoušet s [bezplatným účtem](https://azure.microsoft.com/pricing/free-trial/) a můžete si také [vytvořit svoji první databázi SQL](sql-database-get-started.md).

## Další zdroje
* [Vzory návrhu pro víceklientské aplikace SaaS využívající Azure SQL Database](sql-database-design-patterns-multi-tenancy-saas-applications.md)
* [Videokurz Microsoft Virtual Academy o možnostech elastické databáze v Azure SQL Database](https://mva.microsoft.com/en-US/training-courses/elastic-database-capabilities-with-azure-sql-db-16554)

<!--HONumber=Oct16_HO3-->


