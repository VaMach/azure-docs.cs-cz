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
ms.date: 05/10/2017
ms.author: mimig
ms.openlocfilehash: e9cb4b5b886cec46c0483287460c720855867f38
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-setup-azure-cosmos-db-global-distribution-using-the-table-api"></a>Jak nastavit globální distribuční databázi Cosmos Azure pomocí rozhraní API pro tabulky

V tomto článku jsme ukazují, jak pomocí portálu Azure do instalačního programu globální distribuční databázi Cosmos Azure a potom se připojte pomocí rozhraní API pro tabulku (preview).

Tento článek obsahuje následující úlohy: 

> [!div class="checklist"]
> * Nakonfigurujte globální distribuci pomocí portálu Azure
> * Nakonfigurovat globální distribuční [tabulky rozhraní API](table-introduction.md)

[!INCLUDE [cosmos-db-tutorial-global-distribution-portal](../../includes/cosmos-db-tutorial-global-distribution-portal.md)]


## <a name="connecting-to-a-preferred-region-using-the-table-api"></a>Připojování k upřednostňovaná oblast pomocí rozhraní API pro tabulky

Aby bylo možné využít výhod [globální distribuční](distribute-data-globally.md), klientské aplikace můžete zadat seznam seřazený předvoleb oblastí se používá k provádění operací dokumentu. To můžete provést nastavením `TablePreferredLocations` hodnota konfigurace v souboru config aplikace sada SDK úložiště Azure ve verzi Preview. Na základě konfigurace účtu Azure Cosmos DB, aktuální místní dostupnosti a seznamu předvoleb zadán, bude vybrána optimální koncový bod pomocí sady SDK úložiště Azure a provedení operace zápisu operace čtení.

`TablePreferredLocations` By měla obsahovat čárkami oddělený seznam upřednostňovaných (více funkci) umístění pro čtení. Každá instance klienta můžete určit podmnožinu těchto oblastí v upřednostňovaném pořadí pro čtení s nízkou latencí. Oblasti musí mít název pomocí jejich [zobrazované názvy](https://msdn.microsoft.com/library/azure/gg441293.aspx), například `West US`.

Všechny operace čtení odešle první dostupné oblasti v `TablePreferredLocations` seznamu. Pokud se požadavek nezdaří, klient se nezdaří dolů v seznamu další oblasti a tak dále.

Sada SDK bude pouze pokus o čtení z oblastí, zadaný v `TablePreferredLocations`. Ano, například pokud databázový účet je k dispozici v tři oblasti, ale klient určuje pouze dva oblastí bez zápisu pro `TablePreferredLocations`, pak se zpracuje žádné čtení z oblasti zápisu i v případě převzetí služeb při selhání.

Sada SDK automaticky odesílat všechny zápisy na aktuální zápisu oblast.

Pokud `TablePreferredLocations` není nastavena vlastnost, z aktuální oblasti zápisu se zpracuje všechny požadavky.

```xml
    <appSettings>
      <add key="TablePreferredLocations" value="East US, West US, North Europe"/>           
    </appSettings>
```

Je to, že dokončení tohoto kurzu. Můžete naučit ke správě konzistence účtu globálně replikované načtením [úrovně konzistence v Azure Cosmos DB](consistency-levels.md). A další informace o tom, jak globální replikace databáze v Azure Cosmos DB funguje, najdete v části [distribuci dat globálně pomocí Azure Cosmos DB](distribute-data-globally.md).

## <a name="next-steps"></a>Další kroky

V tomto kurzu jste provést následující:

> [!div class="checklist"]
> * Nakonfigurujte globální distribuci pomocí portálu Azure
> * Nakonfigurujte globální distribuční pomocí rozhraní API DocumentDB

Nyní můžete přejít k dalším kurzu se dozvíte, jak vyvíjet místně pomocí emulátoru místního Azure Cosmos DB.

> [!div class="nextstepaction"]
> [Vývoj místně pomocí emulátoru](local-emulator.md)