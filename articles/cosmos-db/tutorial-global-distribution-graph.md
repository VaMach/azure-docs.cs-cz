---
title: "Globální distribuční kurz pro Azure Cosmos DB pro rozhraní Graph API | Microsoft Docs"
description: "Zjistěte, jak nastavit globální distribuční databázi Cosmos Azure pomocí rozhraní Graph API."
services: cosmos-db
keywords: "globální distribuční, grafu, gremlin"
documentationcenter: 
author: luisbosquez
manager: jhubbard
editor: cgronlun
ms.assetid: 8b815047-2868-4b10-af1d-40a1af419a70
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 01/02/2018
ms.author: lbosq
ms.custom: mvc
ms.openlocfilehash: 1806bde383f04747f1f0fef46e5cf4d38de1e939
ms.sourcegitcommit: 9ea2edae5dbb4a104322135bef957ba6e9aeecde
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/03/2018
---
# <a name="how-to-setup-azure-cosmos-db-global-distribution-using-the-graph-api"></a>Jak nastavit globální distribuční databázi Cosmos Azure pomocí rozhraní Graph API

V tomto článku jsme ukazují, jak pomocí portálu Azure nastavit globální distribuční databázi Cosmos Azure a potom se připojte pomocí rozhraní Graph API.

Tento článek obsahuje následující úlohy: 

> [!div class="checklist"]
> * Nakonfigurujte globální distribuci pomocí portálu Azure
> * Nakonfigurujte globální distribuční pomocí [rozhraní Graph API](graph-introduction.md)

[!INCLUDE [cosmos-db-tutorial-global-distribution-portal](../../includes/cosmos-db-tutorial-global-distribution-portal.md)]


## <a name="connecting-to-a-preferred-region-using-the-graph-api-using-the-net-sdk"></a>Připojování k upřednostňovaná oblast pomocí rozhraní Graph API pomocí sady .NET SDK

Rozhraní Graph API je k dispozici jako rozšíření knihovny nad rozhraní SQL API.

Aby bylo možné využít výhod [globální distribuční](distribute-data-globally.md), klientské aplikace můžete zadat seznam seřazený předvoleb oblastí se používá k provádění operací dokumentu. Tento krok můžete provést nastavením zásad pro připojení. Na základě konfigurace účtu Azure Cosmos DB, aktuální místní dostupnosti a seznamu předvoleb zadán, bude vybrána optimální koncový bod SDK k provedení operace zápisu a operace čtení.

Tento seznam předvoleb je zadána při inicializaci připojení pomocí sady SDK. Sady SDK přijmout volitelný parametr "PreferredLocations" tedy uspořádaný seznam oblastí Azure.

* **Zapíše**: Sada SDK automaticky odesílá všech zápisů do aktuální oblasti zápisu.
* **Přečte**: všechny operace čtení jsou odesílány první dostupné oblasti v seznamu PreferredLocations. Pokud se požadavek nezdaří, dojde k selhání klienta dolů v seznamu k další oblasti a tak dále. Sady SDK pouze pokus o čtení z oblastí, zadaný v PreferredLocations. Ano například pokud Cosmos DB účet je k dispozici v tři oblasti, ale klient určuje pouze dva z oblasti bez zápisu pro PreferredLocations, pak žádný čtení se zpracovávají mimo oblast zápisu, i v případě převzetí služeb při selhání.

Aplikace můžete ověřit aktuální koncový bod zápisu a čtení koncový bod vybrali SDK kontrolou dvě vlastnosti WriteEndpoint a ReadEndpoint dostupné ve verzi sady SDK 1.8 a výše. Pokud není nastavena vlastnost PreferredLocations, jsou z oblasti aktuální zápisu zpracovat všechny požadavky.

### <a name="using-the-sdk"></a>Pomocí sady SDK

Například v sadě SDK .NET `ConnectionPolicy` parametr pro `DocumentClient` konstruktor má vlastnost s názvem `PreferredLocations`. Tuto vlastnost lze nastavit na seznam názvů oblast. Zobrazení názvů pro [oblasti Azure] [ regions] lze zadat jako součást `PreferredLocations`.

> [!NOTE]
> Adresy URL pro koncové body by se neměla považovat jako dlohotrvající konstanty. Služba může aktualizovat tyto v libovolném bodě. Sada SDK zpracovává tuto změnu automaticky.
>
>

```cs
// Getting endpoints from application settings or other configuration location
Uri accountEndPoint = new Uri(Properties.Settings.Default.GlobalDatabaseUri);
string accountKey = Properties.Settings.Default.GlobalDatabaseKey;

ConnectionPolicy connectionPolicy = new ConnectionPolicy();

//Setting read region selection preference
connectionPolicy.PreferredLocations.Add(LocationNames.WestUS); // first preference
connectionPolicy.PreferredLocations.Add(LocationNames.EastUS); // second preference
connectionPolicy.PreferredLocations.Add(LocationNames.NorthEurope); // third preference

// initialize connection
DocumentClient docClient = new DocumentClient(
    accountEndPoint,
    accountKey,
    connectionPolicy);

// connect to Azure Cosmos DB
await docClient.OpenAsync().ConfigureAwait(false);
```

Je to, že dokončení tohoto kurzu. Můžete naučit ke správě konzistence účtu globálně replikované načtením [úrovně konzistence v Azure Cosmos DB](consistency-levels.md). A další informace o tom, jak globální replikace databáze v Azure Cosmos DB funguje, najdete v části [distribuci dat globálně pomocí Azure Cosmos DB](distribute-data-globally.md).

## <a name="next-steps"></a>Další postup

V tomto kurzu jste provést následující:

> [!div class="checklist"]
> * Nakonfigurujte globální distribuci pomocí portálu Azure
> * Nakonfigurujte globální distribuční pomocí rozhraní API SQL

Nyní můžete přejít k dalším kurzu se dozvíte, jak vyvíjet místně pomocí emulátoru místního Azure Cosmos DB.

> [!div class="nextstepaction"]
> [Vývoj místně pomocí emulátoru](local-emulator.md)

[regions]: https://azure.microsoft.com/regions/

