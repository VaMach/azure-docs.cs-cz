---
title: "Úvod do API tabulky Azure Cosmos DB | Dokumentace Microsoftu"
description: "Zjistěte, jak můžete používat Azure Cosmos DB k ukládání a dotazování velkých objemů dat s klíčovými hodnotami s nízkou latencí pomocí oblíbených rozhraní API OSS MongoDB."
services: cosmos-db
author: bhanupr
manager: jhubbard
editor: monicar
documentationcenter: 
ms.assetid: 
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/29/2017
ms.author: arramac
ms.translationtype: HT
ms.sourcegitcommit: 1c730c65194e169121e3ad1d1423963ee3ced8da
ms.openlocfilehash: 3ccc3b176df2f0a5d864554a74508292d272bd5a
ms.contentlocale: cs-cz
ms.lasthandoff: 08/30/2017

---
# <a name="introduction-to-azure-cosmos-db-table-api"></a>Úvod do Azure Cosmos DB: API tabulka

[Azure Cosmos DB](introduction.md) poskytuje rozhraní API tabulky (Preview) pro aplikace napsané pro službu Azure Table Storage, které vyžadují prémiové funkce, jako je [globální distribuce na klíč](distribute-data-globally.md), [vyhrazená propustnost](partition-data.md), latence v řádu milisekund na 99. percentilu po celém světě, záruka vysoké dostupnosti a [automatické sekundární indexování](http://www.vldb.org/pvldb/vol8/p1668-shukla.pdf). Pomocí rozhraní API tabulky je možné tyto aplikace migrovat na službu Azure Cosmos DB a využít tak prémiové funkce.

Doporučujeme začít zhlédnutím následujícího videa, ve kterém Aravind Ramachandran vysvětluje, jak začít s rozhraním API tabulky pro službu Azure Cosmos DB.

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Table-API-for-Azure-Cosmos-DB/player]
> 
> 

## <a name="premium-and-standard-table-apis"></a>Rozhraní API tabulky Premium a Standard
Pokud aktuálně používáte Azure Table Storage, získáte přechodem na „tabulky Premium“ Azure Cosmos DB následující výhody:

|  | Azure Table Storage | Azure Cosmos DB: Table Storage (Preview) |
| --- | --- | --- |
| Latence | Rychlá, bez horních omezení latence | Latence pro čtení a zápis v řádu milisekund, podložená latencí pro čtení menší než 10 ms a latencí pro zápis menší než 15 ms na 99. percentilu, při libovolném škálování, kdekoli na světě. |
| Propustnost | Model variabilní propustnosti. Tabulky mají omezení škálovatelnosti 20 000 operací za sekundu. | Vysoká škálovatelnost s [vyhrazenou rezervovanou propustností na tabulku](request-units.md), podložená smlouvami SLA Účty nemají žádné horní omezení propustnosti a podporují více než 10 milionů operací za sekundu na tabulku. |
| Globální distribuce | Jedna oblast s jednou volitelnou čitelnou sekundární oblastí čtení pro vysokou dostupnost Nemůžete zahájit převzetí služeb při selhání. | [Globální distribuce na klíč](distribute-data-globally.md) od jedné po 30 a více oblastí, podpora pro [automatické a ruční převzetí služeb při selhání](regional-failover.md) kdykoli a kdekoli v celém světě |
| Indexování | PartitionKey a RowKey používají pouze primární index. Žádné sekundární indexy | Automatické a úplné indexování u všech vlastností, žádná správa indexů |
| Dotaz | Při provádění dotazu se používá index pro primární klíč, jinak dochází k prohledávání. | Dotazy mohou ke zrychlení použít výhod automatického indexování vlastností. Modul databáze Azure Cosmos DB je schopný zajistit podporu agregací, geoprostorového indexování a řazení. |
| Konzistence | Silná v rámci primární oblasti, případně v sekundární oblasti | [Pět jasně definovaných úrovní konzistence](consistency-levels.md) pro využití dostupnosti, latence, propustnosti a konzistence na základě potřeb vašich aplikací |
| Ceny | Optimalizované úložiště  | Optimalizovaná propustnost |
| Smlouvy SLA | Dostupnost 99,9 % | 99,99% dostupnost v rámci jedné oblasti a možnost přidávat pro vyšší dostupnost další oblasti [Nejlepší komplexní smlouvy SLA v oboru](https://azure.microsoft.com/support/legal/sla/cosmos-db/) týkající se obecné dostupnosti |

## <a name="how-to-get-started"></a>Jak začít

Vytvořte si na portálu [Azure Portal](https://portal.azure.com) účet Azure Cosmos DB a začněte ho používat pomocí [Rychlého startu pro rozhraní API tabulky pomocí rozhraní .NET](create-table-dotnet.md). 

## <a name="next-steps"></a>Další kroky

Tady jsou odkazy na informace, které vám pomůžou začít:
* [Vytvoření aplikace .NET pomocí rozhraní API tabulky](create-table-dotnet.md)
* [Vývoj pomocí rozhraní API tabulky v .NET](tutorial-develop-table-dotnet.md)
* [Dotazování tabulkových dat pomocí rozhraní API tabulky](tutorial-query-table.md)
* [Nastavení globální distribuce služby Azure Cosmos DB pomocí rozhraní API tabulky](tutorial-global-distribution-table.md)
* [Sada SDK rozhraní API tabulky Azure Cosmos DB pro .NET](table-sdk-dotnet.md)

