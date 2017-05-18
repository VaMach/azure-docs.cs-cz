---
title: "Úvod do API tabulky Azure Cosmos DB | Dokumentace Microsoftu"
description: "Zjistěte, jak můžete používat Azure Cosmos DB k ukládání a dotazování velkých objemů dat s klíčovými hodnotami s nízkou latencí pomocí oblíbených rozhraní API OSS MongoDB."
services: cosmosdb
author: bhanupr
manager: jhubbard
editor: monicar
documentationcenter: 
ms.assetid: 
ms.service: cosmosdb
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/10/2017
ms.author: arramac
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: 8226ecfb5fa8c70b162ae776679302db6cfdcdbc
ms.contentlocale: cs-cz
ms.lasthandoff: 05/10/2017


---
# <a name="introduction-to-azure-cosmos-db-table-api"></a>Úvod do Azure Cosmos DB: API tabulka

[Azure Cosmos DB](introduction.md) je databázová služba Microsoftu s více modely pro klíčové aplikace použitelná v celosvětovém měřítku. Azure Cosmos DB poskytuje [globální distribuci na klíč](../documentdb/documentdb-distribute-data-globally.md), [elastické škálování propustnosti a úložiště](partition-data.md) po celém světě, latence v řádu milisekund na 99. percentilu, [pět jasně definovaných úrovní konzistence](../documentdb/documentdb-consistency-levels.md) a zaručenou vysokou dostupnost. To vše je podloženo [nejlepšími smlouvami SLA v oboru](https://azure.microsoft.com/support/legal/sla/documentdb/v1_1/). Azure Cosmos DB [automaticky indexuje data](http://www.vldb.org/pvldb/vol8/p1668-shukla.pdf), aniž by vyžadovala zapojení správy schémat a indexů. Zahrnuje více modelů a podporuje modely dokumentů, klíčových hodnot, grafů a sloupcových dat. 

![Rozhraní Azure Table Storage API a Azure Cosmos DB](./media/table-introduction/premium-tables.png) 

Azure Cosmos DB poskytuje rozhraní API tabulky pro aplikace, které potřebují úložiště klíčových hodnot s flexibilním schématem. Pro práci s Azure Cosmos DB lze použít sady SDK [Azure Table Storage](../storage/storage-introduction.md) a rozhraní REST API. Azure Cosmos DB můžete použít k vytvoření tabulek s požadavky na vysokou propustnost. Azure Cosmos DB podporuje tabulky s optimalizovanou propustností (neformálně označované jako „tabulky Premium“), aktuálně ve verzi Public Preview. 

Azure Table Storage můžete dále používat pro tabulky s vysokými požadavky na úložiště a nižšími nároky na propustnost. Azure Cosmos DB představí podporu tabulek s optimalizovaným úložištěm v některé z budoucích aktualizací a stávající a nové účty Azure Table Storage se upgradují na Azure Cosmos DB.

## <a name="premium-and-standard-table-apis"></a>Rozhraní API tabulky Premium a Standard
Pokud aktuálně používáte Azure Table Storage, získáte přechodem na „tabulky Premium“ Azure Cosmos DB následující výhody:

|  | Azure Table Storage | Azure Cosmos DB: Table Storage (Preview) |
| --- | --- | --- |
| Latence | Rychlá, bez horních omezení latence | Latence pro čtení a zápis v řádu milisekund, podložená latencí pro čtení menší než 10 ms a latencí pro zápis menší než 15 ms na 99. percentilu, při libovolném škálování, kdekoli na světě |
| Propustnost | Vysoká škálovatelnost, žádný vyhrazený model propustnosti Tabulky mají omezení škálovatelnosti 20 000 operací za sekundu. | Vysoká škálovatelnost s [vyhrazenou rezervovanou propustností na tabulku](../documentdb/documentdb-request-units.md), podložená smlouvami SLA Účty nemají žádné horní omezení propustnosti a podporují více než 10 milionů operací za sekundu na tabulku. |
| Globální distribuce | Jedna oblast s jednou volitelnou čitelnou sekundární oblastí čtení pro vysokou dostupnost Nemůžete zahájit převzetí služeb při selhání. | [Globální distribuce na klíč](../documentdb/documentdb-distribute-data-globally.md) od jedné po 30 a více oblastí, podpora pro [automatické a ruční převzetí služeb při selhání](../documentdb/documentdb-regional-failovers.md) kdykoli a kdekoli v celém světě |
| Indexování | PartitionKey a RowKey používají pouze primární index. Žádné sekundární indexy | Automatické a úplné indexování u všech vlastností, žádná správa indexů |
| Dotaz | Při provádění dotazu se používá index pro primární klíč, jinak dochází k prohledávání. | Dotazy mohou ke zrychlení použít výhod automatického indexování vlastností. Modul databáze Azure Cosmos DB je schopný zajistit podporu agregací, geoprostorového indexování a řazení. |
| Konzistence | Silná v rámci primární oblasti, případně v sekundární oblasti | [pět jasně definovaných úrovní konzistence](../documentdb/documentdb-consistency-levels.md) pro využití dostupnosti, latence, propustnosti a konzistence na základě potřeb vašich aplikací |
| Ceny | Optimalizované úložiště  | Optimalizovaná propustnost |
| Smlouvy SLA | Dostupnost 99,9 % | 99,99% dostupnost v rámci jedné oblasti a možnost přidávat pro vyšší dostupnost další oblasti [Nejlepší komplexní smlouvy SLA v oboru](https://azure.microsoft.com/support/legal/sla/documentdb/v1_1/) týkající se obecné dostupnosti |

## <a name="how-to-get-started"></a>Jak začít

Vytvořte si na portálu [Azure Portal](https://portal.azure.com) účet Azure Cosmos DB a začněte ho používat pomocí [Rychlého startu pro rozhraní API tabulky pomocí rozhraní .NET](create-table-dotnet.md). 

## <a name="next-steps"></a>Další kroky

Tady jsou odkazy na informace, které vám pomůžou začít:
* Začněte s [rozhraním API tabulek Azure Cosmos DB](create-table-dotnet.md) pomocí stávající .NET Table SDK.
* Přečtěte si o [globální distribuci s Azure Cosmos DB](../documentdb/documentdb-distribute-data-globally.md).
* Přečtěte si o [zajištěné propustnosti v Azure Cosmos DB](../documentdb/documentdb-request-units.md).
