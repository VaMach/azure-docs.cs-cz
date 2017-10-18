---
title: "Úvod do rozhraní API tabulky Azure Cosmos DB | Dokumentace Microsoftu"
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
ms.openlocfilehash: c184b02f46ac2ad66c6ce607967a687c82d31477
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="introduction-to-azure-cosmos-db-table-api"></a>Úvod do Azure Cosmos DB: API tabulka

[Azure Cosmos DB](introduction.md) poskytuje rozhraní API tabulky (Preview) pro aplikace napsané pro službu Azure Table Storage, které vyžadují prémiové funkce, jako například:

* [Globální distribuce na klíč](distribute-data-globally.md).
* [Vyhrazená propustnost](partition-data.md) po celém světě.
* Latence v řádu milisekund na 99. percentilu.
* Záruka vysoké dostupnosti.
* [Automatické sekundární indexování](http://www.vldb.org/pvldb/vol8/p1668-shukla.pdf).

Pomocí rozhraní API tabulky je možné tyto aplikace beze změn kódu migrovat na službu Azure Cosmos DB a využít tak prémiové funkce. Rozhraní API tabulky je k dispozici pro .NET a Python.

Doporučujeme vám zhlédnout následující video, ve kterém Aravind Ramachandran vysvětluje, jak začít s rozhraním API tabulky pro službu Azure Cosmos DB:

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Table-API-for-Azure-Cosmos-DB/player]
> 
> 

## <a name="premium-and-standard-table-apis"></a>Rozhraní API tabulky Premium a Standard
Pokud aktuálně používáte službu Table Storage, získáte přechodem na „tabulky Premium“ Azure Cosmos DB následující výhody:

| | Azure Table Storage | Azure Cosmos DB: Table Storage (Preview) |
| --- | --- | --- |
| Latence | Rychlá, bez horních omezení latence. | Latence pro čtení a zápis v řádu milisekund, podložená latencí pro čtení menší než 10 ms a latencí pro zápis menší než 15 ms na 99. percentilu, při libovolném škálování, kdekoli na světě. |
| Propustnost | Model variabilní propustnosti. Tabulky mají omezení škálovatelnosti 20 000 operací za sekundu. | Vysoká škálovatelnost s [vyhrazenou rezervovanou propustností na tabulku](request-units.md), podložená smlouvami SLA. Účty nemají žádné horní omezení propustnosti a podporují více než 10 milionů operací za sekundu na tabulku. |
| Globální distribuce | Jedna oblast s jednou volitelnou čitelnou sekundární oblastí čtení pro vysokou dostupnost. Nemůžete zahájit převzetí služeb při selhání. | [Globální distribuce na klíč](distribute-data-globally.md) od jedné po 30 a více oblastí. Podpora [automatického a ručního převzetí služeb při selhání](regional-failover.md) kdykoli a kdekoli na světě. |
| Indexování | PartitionKey a RowKey používají pouze primární index. Žádné sekundární indexy. | Automatické a úplné indexování u všech vlastností, žádná správa indexů. |
| Dotaz | Při provádění dotazu se používá index pro primární klíč, jinak dochází k prohledávání. | Dotazy mohou ke zrychlení použít výhod automatického indexování vlastností. Databázový stroje Azure Cosmos DB je schopný zajistit podporu agregací, geoprostorového indexování a řazení. |
| Konzistence | Silná v rámci primární oblasti. Nahodilá v rámci sekundární oblasti. | [Pět jasně definovaných úrovní konzistence](consistency-levels.md) pro využití dostupnosti, latence, propustnosti a konzistence na základě potřeb vašich aplikací. |
| Ceny | Optimalizované úložiště. | Optimalizovaná propustnost. |
| Smlouvy SLA | 99,99 % dostupnost. | 99,99% dostupnost v rámci jedné oblasti a možnost přidávat další oblasti pro vyšší dostupnost. [Nejlepší komplexní smlouvy SLA v oboru](https://azure.microsoft.com/support/legal/sla/cosmos-db/) týkající se obecné dostupnosti. |

## <a name="get-started"></a>Začínáme

Vytvořte si účet služby Azure Cosmos DB na webu [Azure Portal](https://portal.azure.com). Pak začněte pomocí našeho [Rychlého startu pro rozhraní API tabulky pomocí rozhraní .NET](create-table-dotnet.md). 

## <a name="next-steps"></a>Další kroky

Tady jsou odkazy na informace, které vám pomůžou začít:
* [Vytvoření aplikace .NET pomocí rozhraní API tabulky](create-table-dotnet.md)
* [Vývoj pomocí rozhraní API tabulky v .NET](tutorial-develop-table-dotnet.md)
* [Dotazování tabulkových dat pomocí rozhraní API tabulky](tutorial-query-table.md)
* [Informace o nastavení globální distribuce služby Azure Cosmos DB pomocí rozhraní API tabulky](tutorial-global-distribution-table.md)
* [Rozhraní .NET API tabulky Azure Cosmos DB](table-sdk-dotnet.md)
* [Sada SDK tabulky Azure Cosmos DB pro Python](table-sdk-python.md)

