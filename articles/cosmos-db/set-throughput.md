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
ms.date: 06/12/2017
ms.author: mimig
ms.openlocfilehash: 69501b07369a8d5da15cf9bc7d75f07999bf0999
ms.sourcegitcommit: a5f16c1e2e0573204581c072cf7d237745ff98dc
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/11/2017
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
            <td valign="top"><p>2 500 jednotek žádosti za sekundu</p></td>
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
4. V novém okně klikněte na **Data Explorer (Preview)** v navigační nabídce.
5. V novém okně rozbalte databázi a kontejneru a pak klikněte na tlačítko **škálování & nastavení**.
6. V novém okně zadejte novou hodnotu propustnost v **propustnost** pole a pak klikněte na **Uložit**.

<a id="set-throughput-sdk"></a>

## <a name="to-set-the-throughput-by-using-the-sql-api-for-net"></a>Chcete-li nastavit propustnost pomocí rozhraní SQL API pro .NET

```C#
//Fetch the resource to be updated
Offer offer = client.CreateOfferQuery()
    .Where(r => r.ResourceLink == collection.SelfLink)    
    .AsEnumerable()
    .SingleOrDefault();

// Set the throughput to the new value, for example 12,000 request units per second
offer = new OfferV2(offer, 12000);

//Now persist these changes to the database by replacing the original resource
await client.ReplaceOfferAsync(offer);
```

## <a name="throughput-faq"></a>Propustnost – nejčastější dotazy

**Můžete nastavit Moje propustnosti na méně než 400 RU/s**

400 RU/s je k dispozici na kolekce tvořené jedním oddílem Cosmos DB (2 500 RU/s je minimální pro dělené kolekce) minimální propustnost. Žádost jednotky jsou nastaveny v intervalech 100 RU/s, ale propustnost nelze nastavit na 100 RU/s nebo jakoukoli hodnotu menší než 400 RU/s. Pokud hledáte nákladově efektivní metodu pro vývoj a testování Cosmos DB, můžete použít bezplatnou [emulátoru DB Cosmos Azure](local-emulator.md), které můžete nasadit místně bez nákladů. 

**Jak lze nastavit pomocí rozhraní API MongoDB througput?**

Nedojde k rozšíření rozhraní API MongoDB nastavit propustnost. Doporučuje se používat rozhraní SQL API, jak je znázorněno v [nastavení propustnost pomocí rozhraní SQL API pro .NET](#set-throughput-sdk).

## <a name="next-steps"></a>Další kroky

Další informace o zřizování a probíhající planetu měřítku s Cosmos DB, najdete v části [dělení a škálování s Cosmos DB](partition-data.md).
