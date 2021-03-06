---
title: "Globální distribuční kurz pro Azure Cosmos DB pro tabulku API | Microsoft Docs"
description: "Zjistěte, jak nastavit globální distribuční databázi Cosmos Azure pomocí rozhraní API tabulky."
services: cosmos-db
keywords: "globální distribuční tabulky"
documentationcenter: 
author: mimig1
manager: jhubbard
editor: cgronlun
ms.assetid: 8b815047-2868-4b10-af1d-40a1af419a70
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 12/13/2017
ms.author: mimig
ms.custom: mvc
ms.openlocfilehash: 40c0bfe913e1396194de00cf6fa1d1ff823b1d0e
ms.sourcegitcommit: c87e036fe898318487ea8df31b13b328985ce0e1
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/19/2017
---
# <a name="how-to-setup-azure-cosmos-db-global-distribution-using-the-table-api"></a>Jak nastavit globální distribuční databázi Cosmos Azure pomocí rozhraní API pro tabulky

Tento článek obsahuje následující úlohy: 

> [!div class="checklist"]
> * Nakonfigurujte globální distribuci pomocí portálu Azure
> * Nakonfigurovat globální distribuční [tabulky rozhraní API](table-introduction.md)

[!INCLUDE [cosmos-db-tutorial-global-distribution-portal](../../includes/cosmos-db-tutorial-global-distribution-portal.md)]


## <a name="connecting-to-a-preferred-region-using-the-table-api"></a>Připojování k upřednostňovaná oblast pomocí rozhraní API pro tabulky

Aby bylo možné využít výhod [globální distribuční](distribute-data-globally.md), klientské aplikace můžete zadat seznam seřazený předvoleb oblastí se používá k provádění operací dokumentu. To můžete provést nastavením [TableConnectionPolicy.PreferredLocations](https://docs.microsoft.com/dotnet/api/microsoft.azure.cosmosdb.table.tableconnectionpolicy.preferredlocations?view=azure-dotnet#Microsoft_Azure_CosmosDB_Table_TableConnectionPolicy_PreferredLocations) vlastnost. Sadu SDK Azure Cosmos DB tabulky API vybere nejlepší koncového bodu pro komunikaci se na základě konfigurace účtu, aktuální místní dostupnosti a seznamu zadaný předvoleb.

PreferredLocations by měla obsahovat čárkami oddělený seznam upřednostňovaných (více funkci) umístění pro čtení. Každá instance klienta můžete určit podmnožinu těchto oblastí v upřednostňovaném pořadí pro čtení s nízkou latencí. Oblasti musí mít název pomocí jejich [zobrazované názvy](https://msdn.microsoft.com/library/azure/gg441293.aspx), například `West US`.

Všechny operace čtení jsou odesílány první dostupné oblasti v seznamu PreferredLocations. Pokud se požadavek nezdaří, klient se nezdaří dolů v seznamu další oblasti a tak dále.

Sada SDK se pokusí přečíst z regionů zadaný v PreferredLocations. Ano například pokud databázový účet je k dispozici v tři oblasti, ale klient pouze určuje pro PreferredLocations dvou oblastí bez zápisu, pak žádný čtení se zpracuje mimo oblast zápisu, i v případě převzetí služeb při selhání.

Sada SDK automaticky odesílá všech zápisů do aktuální oblasti zápisu.

Pokud není nastavena vlastnost PreferredLocations, bude z oblasti aktuální zápisu zpracovat všechny požadavky.

Je to, že dokončení tohoto kurzu. Můžete naučit ke správě konzistence účtu globálně replikované načtením [úrovně konzistence v Azure Cosmos DB](consistency-levels.md). A další informace o tom, jak globální replikace databáze v Azure Cosmos DB funguje, najdete v části [distribuci dat globálně pomocí Azure Cosmos DB](distribute-data-globally.md).

## <a name="next-steps"></a>Další kroky

V tomto kurzu jste provést následující:

> [!div class="checklist"]
> * Nakonfigurujte globální distribuci pomocí portálu Azure
> * Nakonfigurujte globální distribuci pomocí rozhraní API služby Azure Cosmos DB tabulky

