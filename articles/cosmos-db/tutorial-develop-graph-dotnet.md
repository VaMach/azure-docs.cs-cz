---
title: "Azure Cosmos DB: Vývoj pomocí Graph API v rozhraní .NET | Microsoft Docs"
description: "Naučte se vyvíjet s rozhraním API Azure Cosmos DB SQL pomocí rozhraní .NET"
services: cosmos-db
documentationcenter: 
author: luisbosquez
manager: jhubbard
editor: 
ms.assetid: cc8df0be-672b-493e-95a4-26dd52632261
ms.service: cosmos-db
ms.workload: 
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: tutorial
ms.date: 05/10/2017
ms.author: lbosq
ms.custom: mvc
ms.openlocfilehash: ae60cb03d9777b607a6df77155d7f50faaab1de1
ms.sourcegitcommit: 0e4491b7fdd9ca4408d5f2d41be42a09164db775
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/14/2017
---
# <a name="azure-cosmos-db-develop-with-the-graph-api-in-net"></a>Azure Cosmos DB: Vývoj pomocí Graph API v rozhraní .NET
Azure Cosmos DB je globálně distribuované databáze více modelu služby společnosti Microsoft. Můžete snadno vytvořit a dotazovat databáze dotazů, klíčů/hodnot a grafů, které tak můžou využívat výhody použitelnosti v celosvětovém měřítku a možností horizontálního škálování v jádru databáze Azure Cosmos. 

Tento kurz ukazuje, jak vytvořit účet Azure Cosmos DB pomocí portálu Azure a vytvoření grafu databáze a kontejneru. Aplikace se pak vytvoří jednoduchý sociálních sítí s čtyři lidmi pomocí [rozhraní Graph API](graph-sdk-dotnet.md) (preview), pak prochází a dotazy pomocí Gremlin grafu.

Tento kurz obsahuje následující úlohy:

> [!div class="checklist"]
> * Vytvoření účtu služby Azure Cosmos DB 
> * Vytvoření grafu databáze a kontejneru
> * Serializovat vrcholy a okrajů pro objekty .NET
> * Přidejte vrcholy a okraje
> * Dotaz pomocí Gremlin grafu

## <a name="graphs-in-azure-cosmos-db"></a>Grafy v Azure Cosmos DB
Můžete vytvářet, aktualizovat a dotaz grafy pomocí Azure Cosmos DB [Microsoft.Azure.Graphs](graph-sdk-dotnet.md) knihovny. Knihovna Microsoft.Azure.Graph poskytuje jeden rozšiřující metodu `CreateGremlinQuery<T>` na `DocumentClient` třída na provedení dotazů Gremlin.

Gremlin je funkční programovací jazyk, který podporuje zápis operace (DML) a operace dotazů a traversal. Mezi příklady v tomto článku získat vaše Začínáme s Gremlin nabídneme. V tématu [Gremlin dotazy](gremlin-support.md) podrobný návod Gremlin možnosti dostupné v Azure Cosmos DB. 

## <a name="prerequisites"></a>Požadavky
Ujistěte se prosím, že máte následující:

* Aktivní účet Azure. Pokud žádný nemáte, můžete si zaregistrovat [bezplatný účet](https://azure.microsoft.com/free/). 
    * Alternativně můžete použít [místní emulátoru](local-emulator.md) pro účely tohoto kurzu.
* Sadu [Visual Studio](http://www.visualstudio.com/).

## <a name="create-database-account"></a>Vytvoření databázového účtu

Začněme vytvořením účtu Azure Cosmos DB na portálu Azure.  

> [!TIP]
> * Již máte účet Azure Cosmos DB? Pokud ano, přeskočit na [nastavit řešení sady Visual Studio](#SetupVS)
> * Pokud používáte emulátor DB Cosmos Azure, postupujte podle kroků v [emulátoru DB Cosmos Azure](local-emulator.md) nastavit emulátoru a přeskočit na [nastavení řešení v nástroji Visual Studio](#SetupVS). 
>
> 

[!INCLUDE [cosmos-db-create-dbaccount-graph](../../includes/cosmos-db-create-dbaccount-graph.md)]

## <a id="SetupVS"></a>Nastavení řešení v sadě Visual Studio
1. Otevřete na svém počítači sadu **Visual Studio**.
2. V nabídce **Soubor** vyberte **Nový** a zvolte **Projekt**.
3. V **nový projekt** dialogovém okně, vyberte **šablony** / **Visual C#** / **konzolovou aplikaci (rozhraní .NET Framework)** , pojmenujte svůj projekt a potom klikněte na **OK**.
4. V **Průzkumníku řešení** klikněte pravým tlačítkem na novou konzolovou aplikaci v rámci řešení sady Visual Studio a pak klikněte na **Spravovat balíčky NuGet**.
5. V **NuGet** , klikněte na **Procházet**a typ **Microsoft.Azure.Graphs** do vyhledávacího pole a kontroly **zahrnují předprodejní verze**.
6. Ve výsledcích hledání **Microsoft.Azure.Graphs** a klikněte na tlačítko **nainstalovat**.
   
   Pokud se vám zobrazí zpráva týkající se kontroly změn řešení, klikněte na **OK**. Pokud se vám zobrazí zpráva týkající se přijetí licence, klikněte na **Souhlasím**.
   
    `Microsoft.Azure.Graphs` Knihovna poskytuje jeden rozšiřující metodu `CreateGremlinQuery<T>` pro provádění operací Gremlin. Gremlin je funkční programovací jazyk, který podporuje zápis operace (DML) a operace dotazů a traversal. Mezi příklady v tomto článku získat vaše Začínáme s Gremlin nabídneme. [Dotazy gremlin](gremlin-support.md) obsahuje podrobný návod Gremlin funkcí v Azure Cosmos DB.

## <a id="add-references"></a>Připojení aplikace

Přidejte tyto dvě konstanty a *klienta* proměnné ve vaší aplikaci. 

```csharp
string endpoint = ConfigurationManager.AppSettings["Endpoint"]; 
string authKey = ConfigurationManager.AppSettings["AuthKey"]; 
``` 
V dalším kroku head zpět do [portál Azure](https://portal.azure.com) získat adresu URL koncového bodu a primární klíč. Adresa URL koncového bodu a primární klíč jsou potřeba k tomu, aby aplikace věděla, kam se má připojit, a aby služba Azure Cosmos DB důvěřovala připojení aplikace. 

Na portálu Azure přejděte ke svému účtu Azure Cosmos DB, klikněte na **klíče**a potom klikněte na **klíče pro čtení a zápis**. 

Zkopírujte URI z portálu a vložte ji přes `Endpoint` ve výše uvedené vlastnosti koncového bodu. Poté zkopírujte primární klíč z portálu a vložte ji do `AuthKey` vlastnost výše. 

![Snímek obrazovky portálu Azure používá v kurzu k vytvoření aplikace v jazyce C#. Ukazuje Azure DB Cosmos účtů tlačítkem klíče v Azure Cosmos DB navigaci a hodnotami URI a primární klíč v okně klíče](./media/tutorial-develop-graph-dotnet/keys.png) 
 
## <a id="instantiate"></a>Vytvoření instance DocumentClient 
Dál vytvořte novou instanci třídy **DocumentClient**.  

```csharp 
DocumentClient client = new DocumentClient(new Uri(endpoint), authKey); 
``` 

## <a id="create-database"></a>Vytvoření databáze 

Teď vytvořte Azure DB Cosmos [databáze](sql-api-resources.md#databases) pomocí [CreateDatabaseAsync](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.createdatabaseasync.aspx) metoda nebo [CreateDatabaseIfNotExistsAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdatabaseifnotexistsasync.aspx) metodu  **DocumentClient** třídy z [SQL .NET SDK](sql-api-sdk-dotnet.md).  

```csharp 
Database database = await client.CreateDatabaseIfNotExistsAsync(new Database { Id = "graphdb" }); 
``` 
 
## <a name="create-a-graph"></a>Vytvoření grafu. 

Dále vytvořte kontejner grafu pomocí použití [CreateDocumentCollectionAsync](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.createdocumentcollectionasync.aspx) metoda nebo [CreateDocumentCollectionIfNotExistsAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdocumentcollectionifnotexistsasync.aspx) metodu **DocumentClient** třídy. Kolekce je kontejner entit grafu. 

```csharp 
DocumentCollection graph = await client.CreateDocumentCollectionIfNotExistsAsync( 
    UriFactory.CreateDatabaseUri("graphdb"), 
    new DocumentCollection { Id = "graphcollz" }, 
    new RequestOptions { OfferThroughput = 1000 }); 
``` 

## <a id="serializing"></a>Serializovat vrcholy a okrajů pro objekty .NET
Používá Azure Cosmos DB [GraphSON přenosový formát](gremlin-support.md), která definuje schéma JSON pro vrcholy, okraje a vlastností. .NET SDK služby Azure Cosmos DB zahrnuje JSON.NET jako závislost, a to umožňuje nám k serializaci nebo deserializaci GraphSON do objektů .NET, které jsme můžete pracovat v kódu.

Jako příklad umožňuje pracovat s jednoduché sociálních sítí čtyři osoby. Podíváme na tom, jak vytvořit `Person` vrcholy, přidejte `Knows` vztahy mezi nimi, potom dotaz a procházení graf tak, aby najde "friend friend" relace. 

`Microsoft.Azure.Graphs.Elements` Obor názvů obsahuje `Vertex`, `Edge`, `Property` a `VertexProperty` třídy pro deserializaci GraphSON odpovědí dobře definované objekty .NET.

## <a name="run-gremlin-using-creategremlinquery"></a>Spustit Gremlin pomocí CreateGremlinQuery
Gremlin, například SQL, podporuje čtení, zápisu a operace dotazů. Například následující fragment kódu ukazuje, jak vytvořit vrcholy, okraje, provádět některé ukázkové dotazy s pomocí `CreateGremlinQuery<T>`a asynchronně iteraci v rámci těchto výsledků pomocí `ExecuteNextAsync` a ' HasMoreResults.

```cs
Dictionary<string, string> gremlinQueries = new Dictionary<string, string>
{
    { "Cleanup",        "g.V().drop()" },
    { "AddVertex 1",    "g.addV('person').property('id', 'thomas').property('firstName', 'Thomas').property('age', 44)" },
    { "AddVertex 2",    "g.addV('person').property('id', 'mary').property('firstName', 'Mary').property('lastName', 'Andersen').property('age', 39)" },
    { "AddVertex 3",    "g.addV('person').property('id', 'ben').property('firstName', 'Ben').property('lastName', 'Miller')" },
    { "AddVertex 4",    "g.addV('person').property('id', 'robin').property('firstName', 'Robin').property('lastName', 'Wakefield')" },
    { "AddEdge 1",      "g.V('thomas').addE('knows').to(g.V('mary'))" },
    { "AddEdge 2",      "g.V('thomas').addE('knows').to(g.V('ben'))" },
    { "AddEdge 3",      "g.V('ben').addE('knows').to(g.V('robin'))" },
    { "UpdateVertex",   "g.V('thomas').property('age', 44)" },
    { "CountVertices",  "g.V().count()" },
    { "Filter Range",   "g.V().hasLabel('person').has('age', gt(40))" },
    { "Project",        "g.V().hasLabel('person').values('firstName')" },
    { "Sort",           "g.V().hasLabel('person').order().by('firstName', decr)" },
    { "Traverse",       "g.V('thomas').outE('knows').inV().hasLabel('person')" },
    { "Traverse 2x",    "g.V('thomas').outE('knows').inV().hasLabel('person').outE('knows').inV().hasLabel('person')" },
    { "Loop",           "g.V('thomas').repeat(out()).until(has('id', 'robin')).path()" },
    { "DropEdge",       "g.V('thomas').outE('knows').where(inV().has('id', 'mary')).drop()" },
    { "CountEdges",     "g.E().count()" },
    { "DropVertex",     "g.V('thomas').drop()" },
};

foreach (KeyValuePair<string, string> gremlinQuery in gremlinQueries)
{
    Console.WriteLine($"Running {gremlinQuery.Key}: {gremlinQuery.Value}");

    // The CreateGremlinQuery method extensions allow you to execute Gremlin queries and iterate
    // results asychronously
    IDocumentQuery<dynamic> query = client.CreateGremlinQuery<dynamic>(graph, gremlinQuery.Value);
    while (query.HasMoreResults)
    {
        foreach (dynamic result in await query.ExecuteNextAsync())
        {
            Console.WriteLine($"\t {JsonConvert.SerializeObject(result)}");
        }
    }

    Console.WriteLine();
}
```

## <a name="add-vertices-and-edges"></a>Přidejte vrcholy a okraje

Podívejme se na Gremlin příkazy uvedené v předchozí části podrobněji. První jsme některé vrcholy pomocí na Gremlin `addV` metoda. Například následující fragment vytváří vrchol "Thomas rodinu" typu "Osoba", s vlastnostmi pro křestní jméno, příjmení a stáří.

```cs
// Create a vertex
IDocumentQuery<Vertex> createVertexQuery = client.CreateGremlinQuery<Vertex>(
    graphCollection, 
    "g.addV('person').property('firstName', 'Thomas')");

while (createVertexQuery.HasMoreResults)
{
    Vertex thomas = (await create.ExecuteNextAsync<Vertex>()).First();
}
```

Poté vytvoříme některé okraje mezi tyto vrcholy pomocí na Gremlin `addE` metoda. 

```cs
// Add a "knows" edge
IDocumentQuery<Edge> createEdgeQuery = client.CreateGremlinQuery<Edge>(
    graphCollection, 
    "g.V('thomas').addE('knows').to(g.V('mary'))");

while (create.HasMoreResults)
{
    Edge thomasKnowsMaryEdge = (await create.ExecuteNextAsync<Edge>()).First();
}
```

Aktualizujeme existující vrchol pomocí `properties` krok v Gremlin. Jsme přeskočit volání spuštění dotazu prostřednictvím `HasMoreResults` a `ExecuteNextAsync` pro zbytek příklady.

```cs
// Update a vertex
client.CreateGremlinQuery<Vertex>(
    graphCollection, 
    "g.V('thomas').property('age', 45)");
```

Můžete vložit okraje a vrcholy pomocí Gremlin na `drop` krok. Zde je fragment kódu, který ukazuje, jak odstranit vrchol a okraj. Pamatujte, že vyřazení vrchol kaskádové odstranění přidružené okrajů.

```cs
// Drop an edge
client.CreateGremlinQuery(graphCollection, "g.E('thomasKnowsRobin').drop()");

// Drop a vertex
client.CreateGremlinQuery(graphCollection, "g.V('robin').drop()");
```

## <a name="query-the-graph"></a>Dotaz grafu

Můžete provádět dotazy a také pomocí Gremlin traversals. Například následující fragment kódu ukazuje, jak můžete zjistit, kolik vrcholy v grafu:

```cs
// Run a query to count vertices
IDocumentQuery<int> countQuery = client.CreateGremlinQuery<int>(graphCollection, "g.V().count()");
```
Můžete nastavit filtry, pomocí na Gremlin `has` a `hasLabel` kroky a zkombinovat pomocí `and`, `or`, a `not` k vytvoření složitějších filtrů:

```cs
// Run a query with filter
IDocumentQuery<Vertex> personsByAge = client.CreateGremlinQuery<Vertex>(
  graphCollection, 
  "g.V().hasLabel('person').has('age', gt(40))");
```

Můžete promítnout některé vlastnosti ve výsledcích dotazu pomocí `values` kroku:

```cs
// Run a query with projection
IDocumentQuery<string> firstNames = client.CreateGremlinQuery<string>(
  graphCollection, 
  $"g.V().hasLabel('person').values('firstName')");
```

Zatím jste pouze viděli operátory dotazu, které fungují v některé z databází. Grafy jsou rychlé a efektivní pro operace traversal, když potřebujete přejít na související okraje a vrcholy. Umožňuje najít všechny přátelích Thomas. Provedeme to pomocí na Gremlin `outE` krok najdete všechny odesílací okrajů z Thomas pak procházení k v vrcholy z těchto hran pomocí Gremlin na `inV` krok:

```cs
// Run a traversal (find friends of Thomas)
IDocumentQuery<Vertex> friendsOfThomas = client.CreateGremlinQuery<Vertex>(
  graphCollection,
  "g.V('thomas').outE('knows').inV().hasLabel('person')");
```

Další dotaz provádí dvěma segmenty směrování k vyhledání všech Thomas. "přátelích přátel,", voláním `outE` a `inV` dvakrát. 

```cs
// Run a traversal (find friends of friends of Thomas)
IDocumentQuery<Vertex> friendsOfFriendsOfThomas = client.CreateGremlinQuery<Vertex>(
  graphCollection,
  "g.V('thomas').outE('knows').inV().hasLabel('person').outE('knows').inV().hasLabel('person')");
```

Můžete vytvořit složitější dotazy a implementovat logiku traversal výkonné grafu pomocí Gremlin, včetně kombinování filtru výrazů, provádění opakování pomocí `loop` kroku a implementuje pomocí podmíněného navigace `choose` krok. Další informace o co můžete dělat s [Gremlin podporu](gremlin-support.md)!

Je to, v tomto kurzu pro Azure Cosmos DB je dokončena! 

## <a name="clean-up-resources"></a>Vyčištění prostředků

Pokud nebudete tuto aplikaci nadále používat, pomocí následujícího postupu odstraňte všechny prostředky vytvořené tímto rychlým startem na webu Azure Portal.  

1. V nabídce vlevo na portálu Azure Portal klikněte na **Skupiny prostředků** a pak klikněte na název vytvořeného prostředku. 
2. Na stránce skupiny prostředků klikněte na **Odstranit**, do textového pole zadejte prostředek, který chcete odstranit, a pak klikněte na **Odstranit**.

## <a name="next-steps"></a>Další kroky

V tomto kurzu jste provést následující:

> [!div class="checklist"]
> * Vytvoření účtu Azure Cosmos DB 
> * Vytvoření grafu databáze a kontejneru
> * Serializovaná vrcholy a okrajů pro objekty .NET
> * Přidání vrcholy a okraje
> * Dotaz pomocí Gremlin grafu

Teď můžete pomocí konzoly Gremlin vytvářet složitější dotazy a implementovat účinnou logiku procházení grafů. 

> [!div class="nextstepaction"]
> [Dotazování pomocí konzoly Gremlin](tutorial-query-graph.md)
