---
title: "Vytvoření aplikace .NET databáze Azure Cosmos využívající rozhraní Graph API | Dokumentace Microsoftu"
description: "Obsahuje ukázku kódu .NET, kterou můžete použít pro připojení a dotazování databáze Azure Cosmos."
services: cosmos-db
documentationcenter: 
author: mimig1
manager: jhubbard
editor: 
ms.assetid: daacbabf-1bb5-497f-92db-079910703046
ms.service: cosmos-db
ms.custom: quick start connect, mvc
ms.workload: 
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 05/21/2017
ms.author: arramac
ms.translationtype: Human Translation
ms.sourcegitcommit: 80be19618bd02895d953f80e5236d1a69d0811af
ms.openlocfilehash: 3491aa53a55d988876710c0ac19383e642dda27b
ms.contentlocale: cs-cz
ms.lasthandoff: 06/07/2017


---
# Databáze Azure Cosmos: Vytvoření aplikace .NET využívající rozhraní Graph API
<a id="azure-cosmos-db-build-a-net-application-using-the-graph-api" class="xliff"></a>

Databáze Azure Cosmos je databázová služba Microsoftu s více modely použitelná v celosvětovém měřítku. Můžete snadno vytvořit a dotazovat databáze dotazů, klíčů/hodnot a grafů, které tak můžou využívat výhody použitelnosti v celosvětovém měřítku a možností horizontálního škálování v jádru databáze Azure Cosmos. 

Tento rychlý start popisuje způsob vytvoření účtu databáze Azure Cosmos, databáze a grafu (kontejneru) pomocí webu Azure Portal. Potom sestavíte a spustíte aplikaci konzoly založenou na [rozhraní Graph API](graph-sdk-dotnet.md) (verze Preview).  

## Požadavky
<a id="prerequisites" class="xliff"></a>

Pokud ještě nemáte nainstalovanou sadu Visual Studio 2017, můžete stáhnout a použít **bezplatnou verzi** [Visual Studio 2017 Community Edition](https://www.visualstudio.com/downloads/). Nezapomeňte při instalaci sady Visual Studio povolit možnost **Azure Development**.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## Vytvoření účtu databáze
<a id="create-a-database-account" class="xliff"></a>

[!INCLUDE [cosmos-db-create-dbaccount-graph](../../includes/cosmos-db-create-dbaccount-graph.md)]

## Přidání grafu
<a id="add-a-graph" class="xliff"></a>

[!INCLUDE [cosmos-db-create-graph](../../includes/cosmos-db-create-graph.md)]

## Klonování ukázkové aplikace
<a id="clone-the-sample-application" class="xliff"></a>

Teď naklonujeme aplikaci rozhraní Graph API z GitHubu, nastavíme připojovací řetězec a spustíme ji. Přesvědčíte se, jak snadno se pracuje s daty prostřednictvím kódu programu. 

1. Otevřete okno terminálu Git, jako je třeba Git Bash, a pomocí `cd` přejděte do pracovního adresáře.  

2. Ukázkové úložiště naklonujete spuštěním následujícího příkazu. 

    ```bash
    git clone https://github.com/Azure-Samples/azure-cosmos-db-graph-dotnet-getting-started.git
    ```

3. Potom otevřete soubor řešení v sadě Visual Studio. 

## Kontrola kódu
<a id="review-the-code" class="xliff"></a>

Ještě jednou se stručně podívejme na to, co se v aplikaci děje. Otevřete soubor Program.cs a zjistíte, že tyto řádky kódu vytvářejí prostředky databáze Azure Cosmos. 

* Inicializuje se DocumentClient. Ve verzi Preview jsme do klienta Azure Cosmos DB přidali rozhraní API s rozšířením grafu. Pracujeme na samostatném klientovi pro grafy, který bude oddělený od klienta a prostředků Azure Cosmos DB.

    ```csharp
    using (DocumentClient client = new DocumentClient(
        new Uri(endpoint),
        authKey,
        new ConnectionPolicy { ConnectionMode = ConnectionMode.Direct, ConnectionProtocol = Protocol.Tcp }))
    ```

* Vytvoří se nová databáze.

    ```csharp
    Database database = await client.CreateDatabaseIfNotExistsAsync(new Database { Id = "graphdb" });
    ```

* Vytvoří se nový graf.

    ```csharp
    DocumentCollection graph = await client.CreateDocumentCollectionIfNotExistsAsync(
        UriFactory.CreateDatabaseUri("graphdb"),
        new DocumentCollection { Id = "graph" },
        new RequestOptions { OfferThroughput = 1000 });
    ```
* Pomocí metody `CreateGremlinQuery` se provede série kroků konzoly Gremlin.

    ```csharp
    // The CreateGremlinQuery method extensions allow you to execute Gremlin queries and iterate
    // results asychronously
    IDocumentQuery<dynamic> query = client.CreateGremlinQuery<dynamic>(graph, "g.V().count()");
    while (query.HasMoreResults)
    {
        foreach (dynamic result in await query.ExecuteNextAsync())
        {
            Console.WriteLine($"\t {JsonConvert.SerializeObject(result)}");
        }
    }

    ```

## Aktualizace připojovacího řetězce
<a id="update-your-connection-string" class="xliff"></a>

Teď se vraťte zpátky na portál Azure Portal, kde najdete informace o připojovacím řetězci, a zkopírujte je do aplikace.

1. Na webu [Azure Portal](http://portal.azure.com/) klikněte v účtu databáze Azure Cosmos v levém navigačním panelu na možnost **Klíče** a potom klikněte na **Klíče pro čtení i zápis**. V dalším kroku zkopírujete pomocí tlačítek kopírování na pravé straně obrazovky identifikátor URI a primární klíč do souboru `App.config`.

    ![Zobrazení a zkopírování přístupového klíče na webu Azure Portal v okně Klíče](./media/create-graph-dotnet/keys.png)

2. V sadě Visual Studio 2017 otevřete soubor `App.config`. 

3. Z portálu zkopírujte hodnotu identifikátoru URI (pomocí tlačítka kopírování) a nastavte ji jako hodnotu klíče koncového bodu v souboru `App.config`. 

    `<add key="Endpoint" value="FILLME.documents.azure.com:443" />`

4. Potom z portálu zkopírujte hodnotu PRIMÁRNÍHO KLÍČE a nastavte ji jako hodnotu authKey v souboru `App.config`. 

    `<add key="AuthKey" value="FILLME" />`

Teď jste aktualizovali aplikaci a zadali do ní všechny informace potřebné ke komunikaci s databází Azure Cosmos. 

## Spuštění aplikace konzoly
<a id="run-the-console-app" class="xliff"></a>

1. V sadě Visual Studio klikněte v **Průzkumníku řešení** pravým tlačítkem myši na projekt **GraphGetStarted** a potom klikněte na možnost **Spravovat balíčky NuGet**. 

2. Do pole **Procházet** v NuGetu zadejte *WindowsAzure.Graphs* a zaškrtněte políčko **Zahrnout předběžné verze**. 

3. Z výsledků nainstalujte knihovnu **Microsoft.Azure.Graphs**. Tím se nainstaluje balíček knihovny rozšíření grafů databáze Azure Cosmos a všechny závislosti.

4. Spusťte aplikaci stisknutím CTRL+F5.

   V okně konzoly se zobrazí vrcholy a hrany, které se přidávají do grafu. Po dokončení skriptu dvojím stisknutím klávesy ENTER zavřete okno konzoly. 

## Procházení pomocí Průzkumníku dat
<a id="browse-using-the-data-explorer" class="xliff"></a>

Teď se můžete vrátit do Průzkumníku dat na webu Azure Portal, procházet nová data grafu a zadávat na ně dotazy.

* V Průzkumníku dat se nová databáze zobrazí v podokně Kolekce. Rozbalte položky **graphdb** a **graphcoll** a potom klikněte na **Graph** (Graf).

    V podokně Graphs (Grafy) se zobrazí data vygenerovaná ukázkovou aplikací.

## Ověření smluv SLA na webu Azure Portal
<a id="review-slas-in-the-azure-portal" class="xliff"></a>

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## Vyčištění prostředků
<a id="clean-up-resources" class="xliff"></a>

Pokud nebudete tuto aplikace nadále používat, odstraňte na základě následujícího postupu z portálu Azure Portal všechny prostředky vytvořené podle tohoto rychlého startu: 

1. V nabídce vlevo na portálu Azure Portal klikněte na **Skupiny prostředků** a pak klikněte na název vytvořeného prostředku. 
2. Na stránce skupiny prostředků klikněte na **Odstranit**, do textového pole zadejte prostředek, který chcete odstranit, a pak klikněte na **Odstranit**.

## Další kroky
<a id="next-steps" class="xliff"></a>

V tomto rychlém startu jste se seznámili s postupem vytvoření účtu databáze Azure Cosmos, vytvoření grafu pomocí Průzkumníku dat a spuštění aplikace. Teď můžete pomocí konzoly Gremlin vytvářet složitější dotazy a implementovat účinnou logiku procházení grafů. 

> [!div class="nextstepaction"]
> [Dotazování pomocí konzoly Gremlin](tutorial-query-graph.md)


