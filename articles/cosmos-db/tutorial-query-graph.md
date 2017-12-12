---
title: "Postup dotazování dat grafu v Azure Cosmos DB? | Dokumentace Microsoftu"
description: "Další informace k dotazování na data grafu v Azure Cosmos DB"
services: cosmos-db
documentationcenter: 
author: luisbosquez
manager: jhubbard
editor: 
tags: 
ms.assetid: 8bde5c80-581c-4f70-acb4-9578873c92fa
ms.service: cosmos-db
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: 
ms.date: 05/10/2017
ms.author: lbosq
ms.custom: mvc
ms.openlocfilehash: 358f2650f767057f4a220d3b3b8507de4ce14e6f
ms.sourcegitcommit: a5f16c1e2e0573204581c072cf7d237745ff98dc
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/11/2017
---
# <a name="azure-cosmos-db-how-to-query-with-the-graph-api-preview"></a>Azure Cosmos DB: Jak dotazovat pomocí rozhraní Graph API (preview)?

Azure Cosmos DB [rozhraní Graph API](graph-introduction.md) (preview) podporuje [Gremlin](https://github.com/tinkerpop/gremlin/wiki) dotazy. Tento článek obsahuje ukázkové dokumentech a dotazech, které vám pomůžou začít. A podrobné Gremlin je součástí odkaz [Gremlin podporu](gremlin-support.md) článku.

Tento článek obsahuje následující úlohy: 

> [!div class="checklist"]
> * Dotazování na data s Gremlin

## <a name="prerequisites"></a>Požadavky

Pro tyto dotazy pro práci musí mít účet Azure Cosmos DB a mít dat grafu v kontejneru. Nemáte žádné těchto? Dokončení [rychlý start 5 minut](create-graph-dotnet.md) nebo [vývojáře kurzu](tutorial-query-graph.md) k vytvoření účtu a naplnit databázi. Můžete spustit následující dotazy pomocí [knihovny Azure Cosmos DB .NET grafu](graph-sdk-dotnet.md), [Gremlin konzoly](https://tinkerpop.apache.org/docs/current/reference/#gremlin-console), nebo vaše oblíbené Gremlin ovladač.

## <a name="count-vertices-in-the-graph"></a>Počet vrcholy v grafu

Následující fragment kódu ukazuje, jak můžete zjistit, kolik vrcholy v grafu:

```
g.V().count()
```

## <a name="filters"></a>Filtry

Můžete nastavit filtry, pomocí na Gremlin `has` a `hasLabel` kroky a zkombinovat pomocí `and`, `or`, a `not` k vytvoření složitějších filtrů. Azure Cosmos DB poskytuje, bez ohledu na schéma indexu všech vlastností v rámci vrcholy a stupňů pro rychlé dotazy:

```
g.V().hasLabel('person').has('age', gt(40))
```

## <a name="projection"></a>Projekce

Můžete promítnout některé vlastnosti ve výsledcích dotazu pomocí `values` kroku:

```
g.V().hasLabel('person').values('firstName')
```

## <a name="find-related-edges-and-vertices"></a>Vyhledání souvisejících okraje a vrcholy

Zatím jste pouze viděli operátory dotazu, které fungují v některé z databází. Grafy jsou rychlé a efektivní pro operace traversal, když potřebujete přejít na související okraje a vrcholy. Umožňuje najít všechny přátelích Thomas. Provedeme to pomocí na Gremlin `outE` krok najdete všechny odesílací okrajů z Thomas pak procházení k v vrcholy z těchto hran pomocí Gremlin na `inV` krok:

```cs
g.V('thomas').outE('knows').inV().hasLabel('person')
```

Další dotaz provádí dvěma segmenty směrování k vyhledání všech Thomas. "přátelích přátel,", voláním `outE` a `inV` dvakrát. 

```cs
g.V('thomas').outE('knows').inV().hasLabel('person').outE('knows').inV().hasLabel('person')
```

Můžete vytvořit složitější dotazy a implementovat logiku traversal výkonné grafu pomocí Gremlin, včetně kombinování filtru výrazů, provádění opakování pomocí `loop` kroku a implementuje pomocí podmíněného navigace `choose` krok. Další informace o co můžete dělat s [Gremlin podporu](gremlin-support.md)!

## <a name="next-steps"></a>Další kroky

V tomto kurzu jste provést následující:

> [!div class="checklist"]
> * Zjistili, jak k dotazování pomocí grafu 

Nyní můžete přejít k dalším kurzu se dozvíte, jak se bude distribuovat globální data.

> [!div class="nextstepaction"]
> [Globálně distribuci dat](tutorial-global-distribution-documentdb.md)
