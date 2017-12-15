---
title: "Globální distribuční kurz pro Azure Cosmos DB pro rozhraní SQL API | Microsoft Docs"
description: "Zjistěte, jak nastavit globální distribuční databázi Cosmos Azure pomocí rozhraní SQL API."
services: cosmos-db
keywords: "Globální distribuční"
documentationcenter: 
author: rafats
manager: jhubbard
ms.assetid: 8b815047-2868-4b10-af1d-40a1af419a70
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 05/10/2017
ms.author: rafats
ms.custom: mvc
ms.openlocfilehash: 0cee55673c8abca29b7e389fa4fd62a48566904b
ms.sourcegitcommit: 0e4491b7fdd9ca4408d5f2d41be42a09164db775
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/14/2017
---
# <a name="how-to-setup-azure-cosmos-db-global-distribution-using-the-sql-api"></a>Jak nastavit globální distribuční databázi Cosmos Azure pomocí rozhraní SQL API

[!INCLUDE [cosmos-db-sql-api](../../includes/cosmos-db-sql-api.md)]

V tomto článku jsme ukazují, jak nastavit globální distribuční databázi Cosmos Azure a potom se připojte pomocí rozhraní API SQL pomocí portálu Azure.

Tento článek obsahuje následující úlohy: 

> [!div class="checklist"]
> * Nakonfigurujte globální distribuci pomocí portálu Azure
> * Nakonfigurujte globální distribuční pomocí [rozhraní SQL API](sql-api-introduction.md)

<a id="portal"></a>
[!INCLUDE [cosmos-db-tutorial-global-distribution-portal](../../includes/cosmos-db-tutorial-global-distribution-portal.md)]


## <a name="connecting-to-a-preferred-region-using-the-sql-api"></a>Připojování k upřednostňovaná oblast pomocí rozhraní SQL API

Aby bylo možné využít výhod [globální distribuční](distribute-data-globally.md), klientské aplikace můžete zadat seznam seřazený předvoleb oblastí se používá k provádění operací dokumentu. Tento krok můžete provést nastavením zásad pro připojení. Na základě konfigurace účtu Azure Cosmos DB, aktuální místní dostupnosti a seznamu předvoleb zadán, bude vybrána optimální koncový bod SDK SQL k provedení operace zápisu a operace čtení.

Tento seznam předvoleb je zadána při inicializaci připojení pomocí sady SDK SQL. Sady SDK přijmout volitelný parametr "PreferredLocations" tedy uspořádaný seznam oblastí Azure.

Sada SDK automaticky odesílat všechny zápisy na aktuální zápisu oblast.

Všechny operace čtení odešle první oblasti k dispozici v seznamu PreferredLocations. Pokud se požadavek nezdaří, klient se nezdaří dolů v seznamu další oblasti a tak dále.

Sady SDK se pouze pokusí číst z oblastí, zadaný v PreferredLocations. Ano například pokud databázového účtu je k dispozici v oblastech čtyři, ale klient určuje pro PreferredLocations pouze dvou read(non-write) oblastí, potom žádné čtení se zpracuje mimo oblasti pro čtení, které není zadané v PreferredLocations. Pokud oblasti čtení specifikované v PreferredLocations nejsou k dispozici, čte se zpracuje mimo oblast zápisu.

Aplikace můžete ověřit aktuální koncový bod zápisu a čtení koncový bod vybrali SDK kontrolou dvě vlastnosti WriteEndpoint a ReadEndpoint dostupné ve verzi sady SDK 1.8 a výše.

Pokud není nastavena vlastnost PreferredLocations, bude z oblasti aktuální zápisu zpracovat všechny požadavky.

## <a name="net-sdk"></a>.NET SDK
Sady SDK můžete použít beze změn kódu. V takovém případě sady SDK automaticky přesměruje obě operace čtení a zapíše do aktuální oblasti zápisu.

Ve verzi 1,8 a později sady .NET SDK ConnectionPolicy parametr pro konstruktor DocumentClient má vlastnost s názvem Microsoft.Azure.Documents.ConnectionPolicy.PreferredLocations. Tato vlastnost je typu kolekce `<string>` a musí obsahovat seznam názvů oblast. Řetězcové hodnoty jsou formátovány na sloupci Název oblasti na [oblasti Azure] [ regions] stránky, bez mezer před nebo po první a poslední znak v uvedeném pořadí.

V uvedeném pořadí jsou k dispozici v DocumentClient.WriteEndpoint a DocumentClient.ReadEndpoint aktuální zápisu a čtení koncové body.

> [!NOTE]
> Adresy URL pro koncové body by se neměla považovat jako dlohotrvající konstanty. Služba může aktualizovat tyto v libovolném bodě. Sada SDK zpracovává tuto změnu automaticky.
>
>

```csharp
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

// connect to DocDB
await docClient.OpenAsync().ConfigureAwait(false);
```

## <a name="nodejs-javascript-and-python-sdks"></a>NodeJS, JavaScript a Python SDK
Sady SDK můžete použít beze změn kódu. V takovém případě sada SDK bude automaticky přímé čtení i zápisy na aktuální zápisu oblast.

Ve verzi 1,8 a novější každý SDK ConnectionPolicy parametr pro konstruktor DocumentClient novou vlastnost s názvem DocumentClient.ConnectionPolicy.PreferredLocations. To je parametr pole řetězců, která přebírá seznam názvů oblast. Názvy jsou formátovány za ve sloupci Název oblasti [oblasti Azure] [ regions] stránky. Také můžete použít předdefinované konstanty v objektu pohodlí AzureDocuments.Regions

V uvedeném pořadí jsou k dispozici v DocumentClient.getWriteEndpoint a DocumentClient.getReadEndpoint aktuální zápisu a čtení koncové body.

> [!NOTE]
> Adresy URL pro koncové body by se neměla považovat jako dlohotrvající konstanty. Služba může aktualizovat tyto v libovolném bodě. Tato změna bude zpracována sady SDK automaticky.
>
>

Níže je příklad kódu pro NodeJS/Javascript. Python a Java bude postupují stejným způsobem.

```java
// Creating a ConnectionPolicy object
var connectionPolicy = new DocumentBase.ConnectionPolicy();

// Setting read region selection preference, in the following order -
// 1 - West US
// 2 - East US
// 3 - North Europe
connectionPolicy.PreferredLocations = ['West US', 'East US', 'North Europe'];

// initialize the connection
var client = new DocumentDBClient(host, { masterKey: masterKey }, connectionPolicy);
```

## <a name="rest"></a>REST
Jakmile se databázový účet má k dispozici v několika oblastech, klienti mohou odesílat dotazy jeho dostupnost provedením požadavek GET na následující identifikátor URI.

    https://{databaseaccount}.documents.azure.com/

Služba, vrátí se seznam oblastí a jejich odpovídající Azure Cosmos DB koncový bod identifikátory URI pro repliky. Aktuální oblasti zápisu budou uvedené v odpovědi. Klienta můžete pak vybrat příslušný koncový bod pro všechny další požadavky REST API následujícím způsobem.

Příklad odpovědi

    {
        "_dbs": "//dbs/",
        "media": "//media/",
        "writableLocations": [
            {
                "Name": "West US",
                "DatabaseAccountEndpoint": "https://globaldbexample-westus.documents.azure.com:443/"
            }
        ],
        "readableLocations": [
            {
                "Name": "East US",
                "DatabaseAccountEndpoint": "https://globaldbexample-eastus.documents.azure.com:443/"
            }
        ],
        "MaxMediaStorageUsageInMB": 2048,
        "MediaStorageUsageInMB": 0,
        "ConsistencyPolicy": {
            "defaultConsistencyLevel": "Session",
            "maxStalenessPrefix": 100,
            "maxIntervalInSeconds": 5
        },
        "addresses": "//addresses/",
        "id": "globaldbexample",
        "_rid": "globaldbexample.documents.azure.com",
        "_self": "",
        "_ts": 0,
        "_etag": null
    }


* Požadavky PUT, POST a DELETE musí přejít do uvedeného zápisu identifikátoru URI
* Získá všechny a ostatních jen pro čtení požadavků (například dotazy) může přejděte na libovolný koncový bod podle svého výběru

Zápis, požadavky na jen pro čtení oblasti selže s kódem chyby protokolu HTTP 403 "(zakázáno).

Pokud oblasti zápisu změn po počáteční zjišťování fáze klienta, následné zápisy do oblasti předchozí zápis selže s kódem chyby protokolu HTTP 403 "(zakázáno). Klient pak měli získat seznam oblastí znovu k získání oblasti aktualizované zápisu.

Je to, že dokončení tohoto kurzu. Můžete naučit ke správě konzistence účtu globálně replikované načtením [úrovně konzistence v Azure Cosmos DB](consistency-levels.md). A další informace o tom, jak globální replikace databáze v Azure Cosmos DB funguje, najdete v části [distribuci dat globálně pomocí Azure Cosmos DB](distribute-data-globally.md).

## <a name="next-steps"></a>Další kroky

V tomto kurzu jste provést následující:

> [!div class="checklist"]
> * Nakonfigurujte globální distribuci pomocí portálu Azure
> * Nakonfigurujte globální distribuční pomocí rozhraní API SQL

Nyní můžete přejít k dalším kurzu se dozvíte, jak vyvíjet místně pomocí emulátoru místního Azure Cosmos DB.

> [!div class="nextstepaction"]
> [Vývoj místně pomocí emulátoru](local-emulator.md)

[regions]: https://azure.microsoft.com/regions/

