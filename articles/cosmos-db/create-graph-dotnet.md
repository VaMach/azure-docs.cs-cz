---
title: "Vytvoření aplikace .NET databáze Azure Cosmos využívající rozhraní Graph API | Dokumentace Microsoftu"
description: "Obsahuje ukázku kódu .NET, kterou můžete použít pro připojení a dotazování databáze Azure Cosmos."
services: cosmos-db
documentationcenter: 
author: dennyglee
manager: jhubbard
editor: 
ms.assetid: daacbabf-1bb5-497f-92db-079910703046
ms.service: cosmos-db
ms.custom: quick start connect, mvc
ms.workload: 
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 07/14/2017
ms.author: denlee
ms.translationtype: HT
ms.sourcegitcommit: c999eb5d6b8e191d4268f44d10fb23ab951804e7
ms.openlocfilehash: 1794341ed0d4519eef7f065d04ccf86a7e48a4a4
ms.contentlocale: cs-cz
ms.lasthandoff: 07/17/2017

---
# <a name="azure-cosmos-db-build-a-net-application-using-the-graph-api"></a>Databáze Azure Cosmos: Vytvoření aplikace .NET využívající rozhraní Graph API

Databáze Azure Cosmos je databázová služba Microsoftu s více modely použitelná v celosvětovém měřítku. Můžete snadno vytvořit a dotazovat databáze dotazů, klíčů/hodnot a grafů, které tak můžou využívat výhody použitelnosti v celosvětovém měřítku a možností horizontálního škálování v jádru databáze Azure Cosmos. 

Tento rychlý start popisuje způsob vytvoření účtu databáze Azure Cosmos, databáze a grafu (kontejneru) pomocí webu Azure Portal. Potom sestavíte a spustíte aplikaci konzoly založenou na [rozhraní Graph API](graph-sdk-dotnet.md) (verze Preview).  

## <a name="prerequisites"></a>Požadavky

Pokud ještě nemáte nainstalovanou sadu Visual Studio 2017, můžete stáhnout a použít **bezplatnou verzi** [Visual Studio 2017 Community Edition](https://www.visualstudio.com/downloads/). Nezapomeňte při instalaci sady Visual Studio povolit možnost **Azure Development**.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-a-database-account"></a>Vytvoření účtu databáze

[!INCLUDE [cosmos-db-create-dbaccount-graph](../../includes/cosmos-db-create-dbaccount-graph.md)]

## <a name="add-a-graph"></a>Přidání grafu

[!INCLUDE [cosmos-db-create-graph](../../includes/cosmos-db-create-graph.md)]

## <a name="clone-the-sample-application"></a>Klonování ukázkové aplikace

Teď naklonujeme aplikaci rozhraní Graph API z GitHubu, nastavíme připojovací řetězec a spustíme ji. Přesvědčíte se, jak snadno se pracuje s daty prostřednictvím kódu programu. 

1. Otevřete okno terminálu Git, jako je třeba Git Bash, a pomocí `cd` přejděte do pracovního adresáře.  

2. Ukázkové úložiště naklonujete spuštěním následujícího příkazu. 

    ```bash
    git clone https://github.com/Azure-Samples/azure-cosmos-db-graph-dotnet-getting-started.git
    ```

3. Potom otevřete soubor řešení v sadě Visual Studio. 

## <a name="review-the-code"></a>Kontrola kódu

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

## <a name="update-your-connection-string"></a>Aktualizace připojovacího řetězce

Teď se vraťte zpátky na portál Azure Portal, kde najdete informace o připojovacím řetězci, a zkopírujte je do aplikace.

1. Na webu Azure Portal v účtu služby Azure Cosmos DB klikněte v levém navigačním panelu na **Přehled**. V dalším kroku zkopírujete hodnotu **Gremlin URI** do souboru App.config. 

    ![Zobrazení a zkopírování přístupového klíče na webu Azure Portal v okně Klíče](./media/create-graph-dotnet/gremlin-uri.png)

    Pokud je hodnota **Gremlin URI** prázdná, můžete hodnotu vygenerovat na stránce **Klíče** na portálu s použitím hodnoty **URI**, ve které odeberete „https://“ a změníte „documents“ na „graphs“. 

2. V sadě Visual Studio 2017 otevřete soubor App.config. 

3. Z portálu zkopírujte hodnotu **Gremlin URI** a nastavte ji jako hodnotu klíče koncového bodu v souboru App.config. 

    `<add key="Endpoint" value="FILLME.graphs.azure.com:443" />`

4. Zpátky na webu Azure Portal klikněte na **Klíče** v levé navigační nabídce, zkopírujte z portálu hodnotu **PRIMÁRNÍ KLÍČ**, nastavte ji jako hodnotu klíče AuthKey v souboru App.config a uložte změny. 

    `<add key="AuthKey" value="FILLME" />`

    ![Zobrazení a zkopírování primárního klíče na webu Azure Portal na stránce Klíče](./media/create-graph-dotnet/keys.png)

Teď jste aktualizovali aplikaci a zadali do ní všechny informace potřebné ke komunikaci s Azure Cosmos DB. 

## <a name="run-the-console-app"></a>Spuštění aplikace konzoly

1. V sadě Visual Studio klikněte v **Průzkumníku řešení** pravým tlačítkem myši na projekt **GraphGetStarted** a potom klikněte na možnost **Spravovat balíčky NuGet**. 

2. Do pole **Procházet** v NuGetu zadejte *WindowsAzure.Graphs* a zaškrtněte políčko **Zahrnout předběžné verze**. 

3. Z výsledků nainstalujte knihovnu **Microsoft.Azure.Graphs**. Tím se nainstaluje balíček knihovny rozšíření grafů databáze Azure Cosmos a všechny závislosti.

4. Spusťte aplikaci stisknutím CTRL+F5.

   V okně konzoly se zobrazí vrcholy a hrany, které se přidávají do grafu. Po dokončení skriptu dvojím stisknutím klávesy ENTER zavřete okno konzoly. 

## <a name="browse-using-the-data-explorer"></a>Procházení pomocí Průzkumníku dat

Teď se můžete vrátit do Průzkumníku dat na webu Azure Portal, procházet nová data grafu a zadávat na ně dotazy.

* V Průzkumníku dat se nová databáze zobrazí v podokně Kolekce. Rozbalte položky **graphdb** a **graphcoll** a potom klikněte na **Graph** (Graf).

    V podokně Graphs (Grafy) se zobrazí data vygenerovaná ukázkovou aplikací.

## <a name="review-slas-in-the-azure-portal"></a>Ověření smluv SLA na webu Azure Portal

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a>Vyčištění prostředků

Pokud nebudete tuto aplikace nadále používat, odstraňte na základě následujícího postupu z portálu Azure Portal všechny prostředky vytvořené podle tohoto rychlého startu: 

1. V nabídce vlevo na portálu Azure Portal klikněte na **Skupiny prostředků** a pak klikněte na název vytvořeného prostředku. 
2. Na stránce skupiny prostředků klikněte na **Odstranit**, do textového pole zadejte prostředek, který chcete odstranit, a pak klikněte na **Odstranit**.

## <a name="next-steps"></a>Další kroky

V tomto rychlém startu jste se seznámili s postupem vytvoření účtu databáze Azure Cosmos, vytvoření grafu pomocí Průzkumníku dat a spuštění aplikace. Teď můžete pomocí konzoly Gremlin vytvářet složitější dotazy a implementovat účinnou logiku procházení grafů. 

> [!div class="nextstepaction"]
> [Dotazování pomocí konzoly Gremlin](tutorial-query-graph.md)


