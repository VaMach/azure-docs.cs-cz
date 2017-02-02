---
title: "Výkon služby SQL Database: úrovně služeb | Dokumentace Microsoftu"
description: "Porovnání úrovní služeb SQL Database."
keywords: "možnosti databáze, výkon databáze"
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: f5c5c596-cd1e-451f-92a7-b70d4916e974
ms.service: sql-database
ms.custom: overview
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 01/11/2017
ms.author: carlrab; janeng
translationtype: Human Translation
ms.sourcegitcommit: 0a00aff343bfd31c956f6cbc831e89cc1cc84b23
ms.openlocfilehash: 95ae4bd67b7d08755035e7b5559ca9648d45bdaa


---
# <a name="sql-database-options-and-performance-understand-whats-available-in-each-service-tier"></a>Možnosti a výkon služby Azure SQL Database: co je dostupné na jednotlivých úrovních služby
[Azure SQL Database](sql-database-technical-overview.md) nabízí tři úrovně služby (**Basic**, **Standard** a **Premium**) s několika úrovněmi výkonu pro zpracování různých úloh. Vyšší úrovně výkonu poskytují větší objem prostředků navržených tak, aby poskytovaly stále větší propustnost. [Úrovně služby a úrovně výkonu můžete dynamicky](sql-database-scale-up.md) měnit, a to bez výpadků. Úrovně služeb Basic, Standard a Premium mají všechny dostupnost SLA 99,99 % a nabízejí flexibilní možnosti provozní kontinuity, funkce zabezpečení a fakturaci po hodinách. 

Můžete vytvořit izolované databáze s vyhrazeným prostředkem s vybranou [úrovní výkonu](sql-database-service-tiers.md#single-database-service-tiers-and-performance-levels). Můžete ale také spravovat několik databází v [elastickém fondu](sql-database-service-tiers.md#elastic-pool-service-tiers-and-performance-in-edtus), ve kterém se prostředky sdílejí napříč databázemi. Prostředky dostupné pro izolované databáze se uvádějí v jednotkách DTU (Database Transaction Unit); prostředky dostupné pro elastické fondy se uvádějí v elastických jednotkách DTU (eDTU). Další informace o jednotkách DTU a eDTU najdete v tématu [Co je DTU](sql-database-what-is-a-dtu.md). 

V obou případech jsou dostupné úrovně služby **Basic**, **Standard** a **Premium**. 

## <a name="choosing-a-service-tier"></a>Výběr úrovně služeb
Následující tabulka obsahuje příklady úrovní služeb vhodných pro různé zátěže a aplikace.

| Úroveň služeb | Cílová zátěž |
| :--- | --- |
| **Basic** | Nejvhodnější pro malé databáze provádějící obvykle jednu aktivní operaci najednou. Patří sem například databáze používané pro vývoj a testování nebo méně rozsáhlé a zřídka používané aplikace. |
| **Standard** |Základní možnost pro cloudové aplikace s nízkými až středními požadavky na výkon V/V. Podporuje více současných dotazů. Příkladem mohou být webové aplikace nebo aplikace pro pracovní týmy. |
| **Premium** | Tato varianta je určená pro vysoké objemy transakcí s vysokými požadavky na výkon V/V. Podporuje více souběžných uživatelů. Příkladem mohou být databáze podporující kritické podnikové procesy. |

Nejdřív se rozhodněte, jestli chcete provozovat izolovanou databázi nebo jestli chcete seskupit databáze, které sdílejí prostředky. Přečtěte si, co je [potřeba vzít v úvahu u elastických fondů](sql-database-elastic-pool-guidance.md). Při rozhodování o úrovni služeb začněte tak, že určíte minimální databázové funkce, které budete potřebovat:

* Maximální velikost pro jednotlivé databáze (max. 2 GB pro Basic, max. 250 GB pro Standard a max. 500 GB až 1 TB pro Premium u špičkových úrovní výkonu)
* Maximální celková velikost úložiště v případě fondu elastické databáze (117 GB pro Basic, 1200 GB pro Standard a 750 GB pro Premium)
* Maximální počet databází na fond (400 pro Basic, 400 pro Standard a 50 pro Premium)
* Období uchovávání záloh databáze (7 dní pro Basic, 35 dní pro Standard a Premium)

Jakmile určíte minimální úroveň služeb, jste připraveni k určení úrovně výkonu databáze (počet DTU). Dobrým výchozím bodem jsou v řadě případů standardní úrovně výkonu S2 a S3. U databází s vysokými požadavky na V/V nebo procesor jsou tím správným výchozím bodem úrovně výkonu Premium. Úroveň Premium nabízí větší kapacitu procesoru a v porovnání s nejvyšší úrovní výkonu Standard začíná na 10násobku V/V operací.

Po počátečním výběru úrovně výkonu můžete později dynamicky horizontálně navyšovat nebo snižovat kapacitu [jednotlivých databází](sql-database-scale-up.md) nebo [elastického fondu](sql-database-elastic-pool-manage-portal.md#change-performance-settings-of-a-pool) na základě aktuálních zkušeností. U migračních scénářů můžete k odhadu počtu potřebných DTU také použít [Kalkulačku DTU](http://dtucalculator.azurewebsites.net/). 

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Azure-SQL-Database-dynamically-scale-up-or-scale-down/player]
>

## <a name="single-database-service-tiers-and-performance-levels"></a>Úrovně služeb a úrovně výkonu pro izolované databáze
Pro izolované databáze jsou v každé úrovni služeb dostupné různé úrovně výkonu. Máte možnost zvolit si úroveň, která nejlépe vyhovuje vašim požadavkům na výkon. Pokud potřebujete škálování směrem nahoru nebo dolů, můžete snadno změnit úroveň služeb vaší databáze. Podrobnosti viz [Změna úrovně služeb a úrovně výkonu databáze](sql-database-scale-up.md).

Bez ohledu na počet hostovaných databází, bude mít vaše databáze zaručenou sadu prostředků a očekávaný výkon databáze nebude ovlivněn.

[!INCLUDE [SQL DB service tiers table](../../includes/sql-database-service-tiers-table.md)]

> [!NOTE]
> Podrobné vysvětlení všech ostatních řádků v této tabulce úrovní služeb najdete v části [Možnosti a omezení úrovní služeb](sql-database-performance-guidance.md#service-tier-capabilities-and-limits).
> 

## <a name="elastic-pool-service-tiers-and-performance-in-edtus"></a>Úrovně služeb elastických fondů a měření výkonu pomocí eDTU

Fondy umožňují databázím sdílet a využívat prostředky (měřené v jednotkách eDTU), aniž by bylo nutné všem databázím ve fondu přiřazovat konkrétní úroveň výkonu. Například izolovaná databáze ve fondu, který má úroveň Standard, může přecházet od využívání 0 eDTU až k maximální hodnotě eDTU, kterou jste pro danou databázi ve fondu nastavili. Fondy umožňují více databázím s různou zátěží efektivně využívat prostředky eDTU, které jsou dostupné pro celý fond. Podrobnosti viz [Cenové a výkonové požadavky fondu elastické databáze](sql-database-elastic-pool-guidance.md).

V následující tabulce jsou popsané charakteristiky úrovní služeb fondů.

[!INCLUDE [SQL DB service tiers table for elastic pools](../../includes/sql-database-service-tiers-table-elastic-pools.md)]

Každá databáze ve fondu současně dodržuje pravidla pro izolované databáze určená danou úrovní. Například fond s úrovní Basic má omezení maximálního počtu relací na fond 4800 – 28800, ale jednotlivé databáze ve fondu Basic mají omezení 300 relací.


## <a name="next-steps"></a>Další kroky

* Přečtěte si další podrobnosti o [elastických fondech](sql-database-elastic-pool-guidance.md) a o [cenových a výkonových požadavcích elastických fondů](sql-database-elastic-pool-guidance.md).
* Zjistěte, jak [monitorovat, spravovat a měnit velikost elastických fondů](sql-database-elastic-pool-manage-portal.md) a jak [sledovat výkon izolovaných databází](sql-database-single-database-monitor.md).
* Teď, když už máte přehled o úrovních služeb SQL Database, můžete si je vyzkoušet s [bezplatným účtem](https://azure.microsoft.com/pricing/free-trial/) a můžete si také [vytvořit svoji první databázi SQL](sql-database-get-started.md).




<!--HONumber=Jan17_HO2-->


