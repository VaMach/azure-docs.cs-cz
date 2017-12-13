---
title: "Postup dotazování pomocí SQL v Azure Cosmos DB? | Dokumentace Microsoftu"
description: "Postup dotazování pomocí SQL v Azure Cosmos DB"
services: cosmos-db
documentationcenter: 
author: rafats
manager: jhubbard
editor: 
tags: 
ms.assetid: 
ms.service: cosmos-db
ms.custom: tutorial-develop, mvc
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: 
ms.date: 05/10/2017
ms.author: rafats
ms.openlocfilehash: ae2d7c4ecdeeb88714cdaa0558950ea52fab99d3
ms.sourcegitcommit: aaba209b9cea87cb983e6f498e7a820616a77471
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/12/2017
---
# <a name="azure-cosmos-db-how-to-query-using-sql"></a>Azure Cosmos DB: Jak dotazovat pomocí SQL?

[!INCLUDE [cosmos-db-sql-api](../../includes/cosmos-db-sql-api.md)]

Azure Cosmos DB [rozhraní SQL API](documentdb-introduction.md) podporuje dotazování dokumentů pomocí SQL. Tento článek obsahuje ukázkové dokumentu a dva ukázkové dotazy SQL a výsledky.

Tento článek obsahuje následující úlohy: 

> [!div class="checklist"]
> * Dotazování na data pomocí SQL

## <a name="sample-document"></a>Ukázka dokumentu

Dotazy SQL v tomto článku použít následující ukázka dokumentu.

```json
{
  "id": "WakefieldFamily",
  "parents": [
      { "familyName": "Wakefield", "givenName": "Robin" },
      { "familyName": "Miller", "givenName": "Ben" }
  ],
  "children": [
      {
        "familyName": "Merriam", 
        "givenName": "Jesse", 
        "gender": "female", "grade": 1,
        "pets": [
            { "givenName": "Goofy" },
            { "givenName": "Shadow" }
        ]
      },
      { 
        "familyName": "Miller", 
         "givenName": "Lisa", 
         "gender": "female", 
         "grade": 8 }
  ],
  "address": { "state": "NY", "county": "Manhattan", "city": "NY" },
  "creationDate": 1431620462,
  "isRegistered": false
}
```
## <a name="where-can-i-run-sql-queries"></a>Kde je můžete spouštět dotazy SQL?

Můžete spouštět dotazy pomocí Průzkumníku dat na portálu Azure pomocí [REST API a sadám SDK,](documentdb-sdk-dotnet.md)a to i v [Query playground](https://www.documentdb.com/sql/demo), která se spouští dotazy na existující sady ukázková data.

Další informace o dotazech SQL najdete v tématu:
* [Dotaz SQL a syntaxe SQL](documentdb-sql-query.md)

## <a name="prerequisites"></a>Požadavky

Tento kurz předpokládá, že máte účet Azure Cosmos databáze a kolekce. Nemáte žádné těchto? Dokončení [rychlý start 5 minut](create-mongodb-nodejs.md) nebo [vývojáře kurzu](tutorial-develop-mongodb.md) vytvoření účtu a kolekce.

## <a name="example-query-1"></a>Příklad dotazu 1

Zadaný vzorek rodiny dokumentu výše, následující dotaz SQL vrátí dokumenty kde pole id odpovídá `WakefieldFamily`. Vzhledem k tomu, že je `SELECT *` příkaz výstup tohoto dotazu je kompletní dokumentu JSON:

**Dotaz**

    SELECT * 
    FROM Families f 
    WHERE f.id = "WakefieldFamily"

**Výsledky**

```json
{
  "id": "WakefieldFamily",
  "parents": [
      { "familyName": "Wakefield", "givenName": "Robin" },
      { "familyName": "Miller", "givenName": "Ben" }
  ],
  "children": [
      {
        "familyName": "Merriam", 
        "givenName": "Jesse", 
        "gender": "female", "grade": 1,
        "pets": [
            { "givenName": "Goofy" },
            { "givenName": "Shadow" }
        ]
      },
      { 
        "familyName": "Miller", 
         "givenName": "Lisa", 
         "gender": "female", 
         "grade": 8 }
  ],
  "address": { "state": "NY", "county": "Manhattan", "city": "NY" },
  "creationDate": 1431620462,
  "isRegistered": false
}
```

## <a name="example-query-2"></a>Příklad dotazu 2

Další dotaz vrátí všechny názvy daným podřízených prvků v dané rodině, jehož id odpovídá `WakefieldFamily` seřazené podle jejich úrovni.

**Dotaz**

    SELECT c.givenName 
    FROM Families f 
    JOIN c IN f.children 
    WHERE f.id = 'WakefieldFamily'
    ORDER BY f.children.grade ASC

**Výsledky**

    [
      { "givenName": "Jesse" }, 
      { "givenName": "Lisa"}
    ]


## <a name="next-steps"></a>Další kroky

V tomto kurzu jste provést následující:

> [!div class="checklist"]
> * Zjistili, jak k dotazování pomocí SQL  

Nyní můžete přejít k dalším kurzu se dozvíte, jak se bude distribuovat globální data.

> [!div class="nextstepaction"]
> [Globálně distribuci dat](tutorial-global-distribution-documentdb.md)

