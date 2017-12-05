---
title: "Globální distribuční kurz pro Azure Cosmos DB pro MongoDB API | Microsoft Docs"
description: "Zjistěte, jak nastavit globální distribuční databázi Cosmos Azure pomocí rozhraní API pro MongoDB."
services: cosmos-db
keywords: "globální distribuční, MongoDB"
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
ms.custom: mvc
ms.openlocfilehash: a934643be02abeadecf9e8384e29bd3e0e1a36f5
ms.sourcegitcommit: 7136d06474dd20bb8ef6a821c8d7e31edf3a2820
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/05/2017
---
# <a name="how-to-setup-azure-cosmos-db-global-distribution-using-the-mongodb-api"></a>Jak nastavit globální distribuční databázi Cosmos Azure pomocí rozhraní API pro MongoDB

V tomto článku jsme ukazují, jak pomocí portálu Azure nastavit globální distribuční databázi Cosmos Azure a potom se připojte pomocí rozhraní API pro MongoDB.

Tento článek obsahuje následující úlohy: 

> [!div class="checklist"]
> * Nakonfigurujte globální distribuci pomocí portálu Azure
> * Nakonfigurujte globální distribuční pomocí [MongoDB rozhraní API](mongodb-introduction.md)

[!INCLUDE [cosmos-db-tutorial-global-distribution-portal](../../includes/cosmos-db-tutorial-global-distribution-portal.md)]

## <a name="verifying-your-regional-setup-using-the-mongodb-api"></a>Ověření vašeho regionální nastavení pomocí rozhraní API pro MongoDB
Nejjednodušší způsob dvojitou Kontrola globální konfigurace v rámci rozhraní API pro MongoDB je ke spuštění *isMaster()* příkazu z prostředí Mongo.

Z vašeho prostředí Mongo:

   ```
      db.isMaster()
   ```
   
Příklad výsledků:

   ```JSON
      {
         "_t": "IsMasterResponse",
         "ok": 1,
         "ismaster": true,
         "maxMessageSizeBytes": 4194304,
         "maxWriteBatchSize": 1000,
         "minWireVersion": 0,
         "maxWireVersion": 2,
         "tags": {
            "region": "South India"
         },
         "hosts": [
            "vishi-api-for-mongodb-southcentralus.documents.azure.com:10255",
            "vishi-api-for-mongodb-westeurope.documents.azure.com:10255",
            "vishi-api-for-mongodb-southindia.documents.azure.com:10255"
         ],
         "setName": "globaldb",
         "setVersion": 1,
         "primary": "vishi-api-for-mongodb-southindia.documents.azure.com:10255",
         "me": "vishi-api-for-mongodb-southindia.documents.azure.com:10255"
      }
   ```

## <a name="connecting-to-a-preferred-region-using-the-mongodb-api"></a>Připojování k upřednostňovaná oblast pomocí rozhraní API pro MongoDB

Rozhraní API MongoDB umožňuje určit vaší kolekce čtení předvoleb globálně distribuované databáze. Pro obě nízká latence čtení a globální vysokou dostupnost, doporučujeme, aby nastavení předvoleb čtení vaší kolekce *nejbližší*. Přečtěte si předvolbu *nejbližší* nakonfigurovaný tak, aby číst z nejbližší oblast.

```csharp
var collection = database.GetCollection<BsonDocument>(collectionName);
collection = collection.WithReadPreference(new ReadPreference(ReadPreferenceMode.Nearest));
```

Pro aplikace s primární pro čtení a zápis oblast a sekundární oblasti pro obnovení po havárii (DR) scénáře, doporučujeme, aby nastavení předvoleb čtení vaší kolekce *sekundární upřednostňovaný*. Přečtěte si předvolbu *sekundární upřednostňovaný* nakonfigurovaný tak, aby ke čtení z oblasti sekundární, pokud primární oblasti není k dispozici.

```csharp
var collection = database.GetCollection<BsonDocument>(collectionName);
collection = collection.WithReadPreference(new ReadPreference(ReadPreferenceMode.SecondaryPreferred));
```

Nakonec pokud chcete ručně zadejte vaše čtení oblasti. Oblast značky lze nastavit v rámci vaši volbu pro čtení.

```csharp
var collection = database.GetCollection<BsonDocument>(collectionName);
var tag = new Tag("region", "Southeast Asia");
collection = collection.WithReadPreference(new ReadPreference(ReadPreferenceMode.Secondary, new[] { new TagSet(new[] { tag }) }));
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
