---
title: "Zřízení propustnost pro Azure Cosmos DB | Microsoft Docs"
description: "Zjistěte, jak nastavit zřízená propustnost pro containsers, kolekce, grafy a tabulky Azure Cosmos DB."
services: cosmos-db
author: mimig1
manager: jhubbard
editor: 
documentationcenter: 
ms.assetid: f98def7f-f012-4592-be03-f6fa185e1b1e
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/02/2018
ms.author: mimig
ms.openlocfilehash: 8797910651c54baa3529b015d4195cf2a5c06ece
ms.sourcegitcommit: 9890483687a2b28860ec179f5fd0a292cdf11d22
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
# <a name="set-throughput-for-azure-cosmos-db-containers"></a>Nastavit propustnost pro Azure Cosmos DB kontejnery

Propustnost lze nastavit pro Azure Cosmos DB kontejnerů na portálu Azure nebo pomocí sady SDK klienta. 

Následující tabulka uvádí k dispozici pro kontejnery propustnost:

<table border="0" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td valign="top"><p></p></td>
            <td valign="top"><p><strong>Jeden oddíl kontejneru</strong></p></td>
            <td valign="top"><p><strong>Oddílů kontejneru</strong></p></td>
        </tr>
        <tr>
            <td valign="top"><p>Minimální propustnost</p></td>
            <td valign="top"><p>400 jednotek žádosti za sekundu</p></td>
            <td valign="top"><p>1 000 jednotek žádosti za sekundu</p></td>
        </tr>
        <tr>
            <td valign="top"><p>Maximální propustnost</p></td>
            <td valign="top"><p>10 000 jednotek žádosti za sekundu</p></td>
            <td valign="top"><p>Unlimited</p></td>
        </tr>
    </tbody>
</table>

## <a name="to-set-the-throughput-by-using-the-azure-portal"></a>Chcete-li nastavit propustnost pomocí portálu Azure

1. V novém okně, otevřete [portál Azure](https://portal.azure.com).
2. Na levém panelu klikněte na tlačítko **Azure Cosmos DB**, nebo klikněte na tlačítko **více služeb** v dolní části, posuňte se k **databáze**a potom klikněte na **Azure Cosmos DB**.
3. Vyberte svůj účet Cosmos DB.
4. V novém okně klikněte na **Průzkumníku dat** v navigační nabídce.
5. V novém okně rozbalte databázi a kontejneru a pak klikněte na tlačítko **škálování & nastavení**.
6. V novém okně zadejte novou hodnotu propustnost v **propustnost** pole a pak klikněte na **Uložit**.

<a id="set-throughput-sdk"></a>

## <a name="to-set-the-throughput-by-using-the-sql-api-for-net"></a>Chcete-li nastavit propustnost pomocí rozhraní SQL API pro .NET

```csharp
// Fetch the offer of the collection whose throughput needs to be updated
Offer offer = client.CreateOfferQuery()
    .Where(r => r.ResourceLink == collection.SelfLink)    
    .AsEnumerable()
    .SingleOrDefault();

// Set the throughput to the new value, for example 12,000 request units per second
offer = new OfferV2(offer, 12000);

// Now persist these changes to the collection by replacing the original offer resource
await client.ReplaceOfferAsync(offer);
```

<a id="set-throughput-java"></a>

## <a name="to-set-the-throughput-by-using-the-sql-api-for-java"></a>Chcete-li nastavit propustnost pomocí rozhraní API SQL pro jazyk Java

Tento fragment kódu je převzat ze souboru OfferCrudSamples.java v [azure-documentdb-java](https://github.com/Azure/azure-documentdb-java/blob/master/documentdb-examples/src/test/java/com/microsoft/azure/documentdb/examples/OfferCrudSamples.java) úložišti. 

```Java
// find offer associated with this collection
Iterator < Offer > it = client.queryOffers(
    String.format("SELECT * FROM r where r.offerResourceId = '%s'", collectionResourceId), null).getQueryIterator();
assertThat(it.hasNext(), equalTo(true));

Offer offer = it.next();
assertThat(offer.getString("offerResourceId"), equalTo(collectionResourceId));
assertThat(offer.getContent().getInt("offerThroughput"), equalTo(throughput));

// update the offer
int newThroughput = 10300;
offer.getContent().put("offerThroughput", newThroughput);
client.replaceOffer(offer);
```

## <a name="throughput-faq"></a>Propustnost – nejčastější dotazy

**Můžete nastavit Moje propustnosti na méně než 400 RU/s**

400 RU/s je k dispozici minimální propustnost kontejnerům Cosmos DB tvořené jedním oddílem (je 1000 RU/s minimum pro kontejnery oddílů). Žádost jednotky jsou nastaveny v intervalech 100 RU/s, ale propustnost nelze nastavit na 100 RU/s nebo jakoukoli hodnotu menší než 400 RU/s. Pokud hledáte nákladově efektivní metodu pro vývoj a testování Cosmos DB, můžete použít bezplatnou [emulátoru DB Cosmos Azure](local-emulator.md), které můžete nasadit místně bez nákladů. 

**Jak lze nastavit pomocí rozhraní API MongoDB througput?**

Nedojde k rozšíření rozhraní API MongoDB nastavit propustnost. Doporučuje se používat rozhraní SQL API, jak je znázorněno v [nastavení propustnost pomocí rozhraní SQL API pro .NET](#set-throughput-sdk).

## <a name="next-steps"></a>Další postup

Další informace o zřizování a probíhající planetu měřítku s Cosmos DB, najdete v části [dělení a škálování s Cosmos DB](partition-data.md).
