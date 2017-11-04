---
title: "Azure Cosmos DB: Jak dotazovat pomocí rozhraní API pro MongoDB? | Dokumentace Microsoftu"
description: "Postup dotazování pomocí rozhraní API MongoDB pro Azure Cosmos DB"
services: cosmos-db
documentationcenter: 
author: mimig1
manager: jhubbard
editor: 
tags: 
ms.assetid: 
ms.service: cosmos-db
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: 
ms.date: 05/10/2017
ms.author: mimig
ms.openlocfilehash: c783af682a78f2001930773ffcfd086efe9b5daa
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/01/2017
---
# <a name="azure-cosmos-db-how-to-query-with-api-for-mongodb"></a>Azure Cosmos DB: Jak dotazovat pomocí rozhraní API pro MongoDB?

Azure Cosmos DB [rozhraní API pro MongoDB](mongodb-introduction.md) podporuje [MongoDB prostředí dotazy](https://docs.mongodb.com/manual/tutorial/query-documents/). 

Tento článek obsahuje následující úlohy: 

> [!div class="checklist"]
> * Dotazování na data s MongoDB

## <a name="sample-document"></a>Ukázka dokumentu

Dotazy v tomto článku použít následující ukázka dokumentu.

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
## <a id="examplequery1"></a>Příklad dotazu 1 

Zadaný vzorek rodiny dokumentu výše, následující dotaz vrátí dokumenty kde pole id odpovídá `WakefieldFamily`.

**Dotaz**
    
    db.families.find({ id: “WakefieldFamily”})

**Výsledky**

    {
    "_id": "ObjectId(\"58f65e1198f3a12c7090e68c\")",
    "id": "WakefieldFamily",
    "parents": [
      {
        "familyName": "Wakefield",
        "givenName": "Robin"
      },
      {
        "familyName": "Miller",
        "givenName": "Ben"
      }
    ],
    "children": [
      {
        "familyName": "Merriam",
        "givenName": "Jesse",
        "gender": "female",
        "grade": 1,
        "pets": [
          { "givenName": "Goofy" },
          { "givenName": "Shadow" }
        ]
      },
      {
        "familyName": "Miller",
        "givenName": "Lisa",
        "gender": "female",
        "grade": 8
      }
    ],
    "address": {
      "state": "NY",
      "county": "Manhattan",
      "city": "NY"
    },
    "creationDate": 1431620462,
    "isRegistered": false
    }

## <a id="examplequery2"></a>Příklad dotazu 2 

Další dotaz vrátí všechny podřízené objekty řady. 

**Dotaz**
    
    db.familes.find( { id: “WakefieldFamily” }, { children: true } )

**Výsledky**

    {
    "_id": "ObjectId("58f65e1198f3a12c7090e68c")",
    "children": [
      {
        "familyName": "Merriam",
        "givenName": "Jesse",
        "gender": "female",
        "grade": 1,
        "pets": [
          { "givenName": "Goofy" },
          { "givenName": "Shadow" }
        ]
      },
      {
        "familyName": "Miller",
        "givenName": "Lisa",
        "gender": "female",
        "grade": 8
      }
    ]
    }


## <a id="examplequery3"></a>Příklad dotazu 3 

Další dotaz vrátí všechny rodiny, které jsou registrované. 

**Dotaz**
    
    db.families.find( { "isRegistered" : true })
**Výsledky** bude vrácen žádný dokument. 

## <a id="examplequery4"></a>Příklad dotazu 4

Další dotaz vrátí všechny rodiny, které nejsou registrované. 

**Dotaz**
    
    db.families.find( { "isRegistered" : false })
**Výsledky**

     {
    "_id": ObjectId("58f65e1198f3a12c7090e68c"),
    "id": "WakefieldFamily",
    "parents": [{
        "familyName": "Wakefield",
        "givenName": "Robin"
    }, {
        "familyName": "Miller",
        "givenName": "Ben"
    }],
    "children": [{
        "familyName": "Merriam",
        "givenName": "Jesse",
        "gender": "female",
        "grade": 1,
        "pets": [{
            "givenName": "Goofy"
        }, {
            "givenName": "Shadow"
        }]
    }, {
        "familyName": "Miller",
        "givenName": "Lisa",
        "gender": "female",
        "grade": 8
    }],
    "address": {
        "state": "NY",
        "county": "Manhattan",
        "city": "NY"
    },
    "creationDate": 1431620462,
    "isRegistered": false
}

## <a id="examplequery5"></a>Příklad dotazu 5

Další dotaz vrátí všechny řady, která nejsou registrovaná a stavu je NY. 

**Dotaz**
    
     db.families.find( { "isRegistered" : false, "address.state" : "NY" })

**Výsledky**

     {
    "_id": ObjectId("58f65e1198f3a12c7090e68c"),
    "id": "WakefieldFamily",
    "parents": [{
        "familyName": "Wakefield",
        "givenName": "Robin"
    }, {
        "familyName": "Miller",
        "givenName": "Ben"
    }],
    "children": [{
        "familyName": "Merriam",
        "givenName": "Jesse",
        "gender": "female",
        "grade": 1,
        "pets": [{
            "givenName": "Goofy"
        }, {
            "givenName": "Shadow"
        }]
    }, {
        "familyName": "Miller",
        "givenName": "Lisa",
        "gender": "female",
        "grade": 8
    }],
    "address": {
        "state": "NY",
        "county": "Manhattan",
        "city": "NY"
    },
    "creationDate": 1431620462,
    "isRegistered": false
}


## <a id="examplequery6"></a>Příklad dotazu 6

Další dotaz vrátí všechny rodiny, kde jsou podřízené objekty tříd 8.

**Dotaz**
  
     db.families.find( { children : { $elemMatch: { grade : 8 }} } )

**Výsledky**

     {
    "_id": ObjectId("58f65e1198f3a12c7090e68c"),
    "id": "WakefieldFamily",
    "parents": [{
        "familyName": "Wakefield",
        "givenName": "Robin"
    }, {
        "familyName": "Miller",
        "givenName": "Ben"
    }],
    "children": [{
        "familyName": "Merriam",
        "givenName": "Jesse",
        "gender": "female",
        "grade": 1,
        "pets": [{
            "givenName": "Goofy"
        }, {
            "givenName": "Shadow"
        }]
    }, {
        "familyName": "Miller",
        "givenName": "Lisa",
        "gender": "female",
        "grade": 8
    }],
    "address": {
        "state": "NY",
        "county": "Manhattan",
        "city": "NY"
    },
    "creationDate": 1431620462,
    "isRegistered": false
}

## <a id="examplequery7"></a>Příklad dotazu 7

Další dotaz vrátí všechny rodiny, kde je velikost, děti pole 3.

**Dotaz**
  
      db.Family.find( {children: { $size:3} } )

**Výsledky**

Žádné výsledky, bude vrácen jako nemáme k dispozici více než 2 podřízené objekty. Jenom v případě, že je parametr 2 Tento dotaz bude úspěšné a vrátit celého dokumentu.

## <a name="next-steps"></a>Další kroky

V tomto kurzu jste provést následující:

> [!div class="checklist"]
> * Zjistili, jak k dotazování pomocí MongoDB 

Nyní můžete přejít k dalším kurzu se dozvíte, jak se bude distribuovat globální data.

> [!div class="nextstepaction"]
> [Globálně distribuci dat](tutorial-global-distribution-documentdb.md)

