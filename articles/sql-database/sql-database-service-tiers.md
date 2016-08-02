<properties
    pageTitle="Výkon a možnosti služby SQL Database: Úrovně služeb | Microsoft Azure"
    description="Porovnejte výkon a kontinuitu podnikových procesů jednotlivých úrovní služby SQL, abyste mohli vyvážit náklady a výkon v průběhu dalšího růstu."
    keywords="database options,database performance"
    services="sql-database"
    documentationCenter=""
    authors="carlrabeler"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.workload="data-management"
    ms.date="05/13/2016"
    ms.author="carlrab"/>

# Výkon a možnosti služby SQL Database: Co je k dispozici v jednotlivých úrovních služeb

[Azure SQL Database](sql-database-technical-overview.md) má více úrovní služeb pro zpracování různých zátěží. Úrovně služeb můžete změnit kdykoli, provoz vaší aplikace se tím nepřeruší. Můžete také [vytvářet izolované databáze](sql-database-get-started.md) s definovanými vlastnostmi a cenami. Nebo můžete spravovat skupinu více databází pomocí [fondu elastické databáze](sql-database-elastic-pool-create-portal.md). V obou případech jsou dostupné úrovně **Basic**, **Standard** a **Premium**. Možnosti databáze v těchto úrovních jsou podobné pro izolované databáze i elastické fondy, ale pro elastické fondy existují ještě další důležité okolnosti. Tento článek obsahuje podrobné informace o úrovních služeb pro izolované databáze i pro databáze elastické.

## Úrovně služeb a možnosti databází
Úrovně služeb Basic, Standard a Premium mají všechny dostupnost SLA 99,99 % a nabízejí předvídatelný výkon, flexibilní možnosti kontinuity podnikových procesů, funkce zabezpečení a fakturaci po hodinách. Následující tabulka obsahuje příklady úrovní služeb vhodných pro různé zátěže a aplikace.

| Úroveň služeb | Cílová zátěž |
|---|---|
| **Basic** | Nejvhodnější pro malé databáze provádějící obvykle jednu aktivní operaci najednou. Patří sem například databáze používané pro vývoj a testování nebo méně rozsáhlé a zřídka používané aplikace. |
| **Standard** | Základní možnost pro většinu cloudových aplikací, podpora více souběžných dotazů. Příkladem mohou být webové aplikace nebo aplikace pro pracovní týmy. |
| **Premium** | Navrženo pro vysoké objemy transakcí, velké počty souběžných uživatelů a pro nejvyšší úroveň kontinuity podnikových procesů. Příkladem mohou být databáze podporující kritické podnikové procesy. |

>[AZURE.NOTE] Úrovně Web a Business byly ukončeny. Přečtěte si prosím článek o [cenách SQL Database v edici Web a Business](https://azure.microsoft.com/pricing/details/sql-database/web-business/), chcete-li tyto úrovně dále používat.

## Úrovně služeb a úrovně výkonu pro izolované databáze
Pro izolované databáze jsou v každé úrovni služeb dostupné různé úrovně výkonu. Máte možnost zvolit si úroveň, která nejlépe vyhovuje vašim požadavkům na výkon. Pokud potřebujete škálování směrem nahoru nebo dolů, můžete snadno změnit úroveň služeb vaší databáze **bez přerušení dostupnosti vaší aplikace**. Podrobnosti viz [Změna úrovně služeb a úrovně výkonu databáze](sql-database-scale-up.md).

Výkonové charakteristiky, které jsou zde uvedené, platí pro databáze vytvořené ve službě [SQL Database verze 12](sql-database-v12-whats-new.md). V situacích, kdy je základní hardware Azure hostitelem více databází, bude mít vaše databáze stále zaručenou sadu prostředků a očekávaný výkon databáze nebude ovlivněn.

[AZURE.INCLUDE [SQL DB service tiers table](../../includes/sql-database-service-tiers-table.md)]

Chcete-li lépe porozumět koncepci DTU, přečtěte si část [DTU](#understanding-dtus) v tomto tématu.

>[AZURE.NOTE] Podrobné vysvětlení všech ostatních řádků v této tabulce úrovní služeb najdete v části [Možnosti a omezení úrovní služeb](sql-database-performance-guidance.md#service-tier-capabilities-and-limits).

## Úrovně služeb elastických fondů a měření výkonu pomocí eDTU
Vedle vytváření a škálování izolovaných databází máte také možnost spravovat skupiny databází v [elastických fondech](sql-database-elastic-pool.md). Všechny databáze v elastickém fondu sdílejí společnou sadu prostředků. Výkonové charakteristiky se měří v *transakčních jednotkách elastické databáze* (eDTU). Stejně jako u izolovaných databází můžete fondy vytvářet ve třech úrovních služeb: **Basic**, **Standard** a **Premium**. I pro fondy tyto tři úrovně definují omezení celkového výkonu a několik dalších vlastností.

Fondy umožňují elastickým databázím sdílet a využívat prostředky (měřené v jednotkách DTU), aniž by bylo nutné přiřazovat úroveň služeb jednotlivým databázím ve fondu. Například izolovaná databáze ve fondu, který má úroveň Standard, může přecházet od využívání 0 eDTU až k maximální hodnotě eDTU, kterou jste pro danou databázi ve fondu nastavili. To umožňuje více databázím s různou zátěží efektivně využívat prostředky eDTU, které jsou dostupné pro celý fond. Podrobnosti viz [Cenové a výkonové požadavky fondu elastické databáze](sql-database-elastic-pool-guidance.md).

V následující tabulce jsou popsané charakteristiky úrovní služeb fondů.

[AZURE.INCLUDE [SQL DB service tiers table for elastic databases](../../includes/sql-database-service-tiers-table-elastic-db-pools.md)]

Každá databáze ve fondu současně dodržuje pravidla pro jednotlivé databáze určená danou úrovní služeb. Například fond s úrovní Basic má omezení maximálního počtu relací na fond 4 800 – 28 800, ale jednotlivé databáze v tomto fondu mají omezení 300 relací (omezení pro jednu databázi v úrovni služeb Basic, jak je uvedeno v předchozí části).

## Vysvětlení jednotek DTU

[AZURE.INCLUDE [SQL DB DTU description](../../includes/sql-database-understanding-dtus.md)]

## Další kroky
- Další informace o cenách pro jednotlivé úrovně služeb najdete v tématu [SQL Database – ceny](https://azure.microsoft.com/pricing/details/sql-database/).
- Přečtěte si další podrobnosti o [fondech elastické databáze](sql-database-elastic-pool-guidance.md) a o [cenových a výkonových požadavcích fondu elastické databáze](sql-database-elastic-pool-guidance.md).
- Zjistěte, jak [monitorovat, spravovat a měnit velikost elastických fondů](sql-database-elastic-pool-manage-portal.md) a jak [sledovat výkon izolovaných databází](sql-database-single-database-monitor.md).
- Teď, když už máte přehled o úrovních služeb SQL Database, můžete si je vyzkoušet s [bezplatným účtem](https://azure.microsoft.com/pricing/free-trial/) a můžete si také [vytvořit svoji první databázi SQL](sql-database-get-started.md).



<!--HONumber=Jun16_HO2-->


