---
title: "Azure Cosmos DB: Sestavení webové aplikace s rozhraním API .NET a DocumentDB | Dokumentace Microsoftu"
description: "Představuje ukázku kódu .NET, který můžete použít k připojení a dotazování do rozhraní DocumentDB API služby Azure Cosmos DB."
services: cosmos-db
documentationcenter: 
author: mimig1
manager: jhubbard
editor: 
ms.assetid: 
ms.service: cosmos-db
ms.custom: quick start connect, mvc
ms.workload: 
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 05/10/2017
ms.author: mimig
ms.translationtype: Human Translation
ms.sourcegitcommit: 857267f46f6a2d545fc402ebf3a12f21c62ecd21
ms.openlocfilehash: d17f90d5ed5440dc336d1e3ae890a13077e33c4d
ms.contentlocale: cs-cz
ms.lasthandoff: 06/28/2017


---
<a id="azure-cosmos-db-build-a-documentdb-api-web-app-with-net-and-the-azure-portal" class="xliff"></a>

# Azure Cosmos DB: Sestavení webové aplikace s rozhraním API DocumentDB v prostředí .NET a na webu Azure Portal

Azure Cosmos DB je globálně distribuovaná databázová služba Microsoftu pro více modelů. Můžete snadno vytvořit a dotazovat databáze dotazů, klíčů/hodnot a grafů, které tak můžou využívat výhody použitelnosti v celosvětovém měřítku a možností horizontálního škálování v jádru Azure Cosmos DB. 

Tento rychlý start popisuje způsob vytvoření účtu služby Azure Cosmos DB, databáze dokumentů a kolekce pomocí portálu Azure Portal. Potom budete moct sestavit a nasadit webovou aplikaci seznamu úkolů založenou na [rozhraní API DocumentDB .NET](documentdb-sdk-dotnet.md), jak znázorňuje následující snímek obrazovky. 

![Aplikace seznamu úkolů s ukázkovými daty](./media/create-documentdb-dotnet/azure-comosdb-todo-app-list.png)

<a id="prerequisites" class="xliff"></a>

## Požadavky

Pokud ještě nemáte nainstalovanou sadu Visual Studio 2017, můžete stáhnout a použít **bezplatnou verzi** [Visual Studio 2017 Community Edition](https://www.visualstudio.com/downloads/). Nezapomeňte při instalaci sady Visual Studio povolit možnost **Azure Development**.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

<a id="create-account"></a>
<a id="create-a-database-account" class="xliff"></a>

## Vytvoření účtu databáze

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

<a id="create-collection"></a>
<a id="add-a-collection" class="xliff"></a>

## Přidání kolekce

[!INCLUDE [cosmos-db-create-collection](../../includes/cosmos-db-create-collection.md)]

<a id="add-sample-data"></a>
<a id="add-sample-data" class="xliff"></a>

## Přidání ukázkových dat

Teď můžete do nové kolekce přidávat data pomocí Průzkumníka dat.

1. V Průzkumníku dat se nová databáze zobrazí v podokně Kolekce. Rozbalte databázi **Tasks**, rozbalte kolekci **Items**, klikněte na **Dokumenty** a potom klikněte na **Nové dokumenty**. 

   ![Vytváření nových dokumentů v Průzkumníku dat na portálu Azure Portal](./media/create-documentdb-dotnet/azure-cosmosdb-data-explorer-new-document.png)
  
2. Teď přidejte do kolekce několik dokumentů s následující strukturou, kde jako ID v jednotlivých dokumentech vložíte jedinečné hodnoty a podle potřeby změníte další vlastnosti. Nové dokumenty můžou mít jakoukoli strukturu, protože Azure Cosmos DB neuplatňuje pro data žádné schéma.

     ```json
     {
         "id": "1",
         "category": "personal",
         "name": "groceries",
         "description": "Pick up apples and strawberries.",
         "isComplete": false
     }
     ```

3. Po přidání formátu json na kartu **Dokumenty** klikněte na **Uložit**.

    ![Zkopírujte data json a v Průzkumníku dat na webu Azure Portal klikněte na Uložit.](./media/create-documentdb-dotnet/azure-cosmosdb-data-explorer-save-document.png)

     Teď můžete k načítání dat používat dotazy v Průzkumníku dat. Ve výchozím nastavení používá Průzkumník dat pro načtení všech dokumentů v kolekci příkaz `SELECT * FROM c`. Ten můžete změnit na příkaz `SELECT * FROM c ORDER BY c.name ASC`, který vrátí všechny dokumenty ve vzestupném abecedním pořadí podle vlastnosti name. 
 
     Průzkumník dat můžete použít také pro vytváření uložených procedur, funkcí UDF a triggerů pro provádění obchodní logiky a také propustnosti škálování na straně serveru. Průzkumník dat zpřístupní všechna integrovaná programová data v rozhraních API, ale zajistí jednoduchý přístup k vašim datům na portálu Azure Portal.

<a id="clone-the-sample-application" class="xliff"></a>

## Klonování ukázkové aplikace

Teď naklonujeme aplikaci API DocumentDB z GitHubu, nastavíme připojovací řetězec a spustíme ji. Přesvědčíte se, jak snadno se pracuje s daty prostřednictvím kódu programu. 

1. Otevřete okno terminálu Git, jako je třeba Git Bash, a pomocí `CD` přejděte do pracovního adresáře.  

2. Ukázkové úložiště naklonujete spuštěním následujícího příkazu. 

    ```bash
    git clone https://github.com/Azure-Samples/documentdb-dotnet-todo-app.git
    ```

3. Potom otevřete soubor řešení todo v sadě Visual Studio. 

<a id="review-the-code" class="xliff"></a>

## Kontrola kódu

Ještě jednou se stručně podívejme na to, co se v aplikaci děje. Otevřete soubor DocumentDBRepository.cs a zjistíte, že tyto řádky kódu vytvářejí prostředky Azure Cosmos DB. 

* Na řádku 73 se inicializuje DocumentClient.

    ```csharp
    client = new DocumentClient(new Uri(ConfigurationManager.AppSettings["endpoint"]), ConfigurationManager.AppSettings["authKey"]);`
    ```

* Na řádku 88 se vytvoří nová databáze.

    ```csharp
    await client.CreateDatabaseAsync(new Database { Id = DatabaseId });
    ```

* Na řádku 107 se vytvoří nová kolekce.

    ```csharp
    await client.CreateDocumentCollectionAsync(
        UriFactory.CreateDatabaseUri(DatabaseId),
        new DocumentCollection { Id = CollectionId },
        new RequestOptions { OfferThroughput = 1000 });
    ```

<a id="update-your-connection-string" class="xliff"></a>

## Aktualizace připojovacího řetězce

Teď se vraťte zpátky na portál Azure Portal, kde najdete informace o připojovacím řetězci, a zkopírujte je do aplikace.

1. Na portálu [Azure Portal](http://portal.azure.com/) klikněte v účtu Azure Cosmos DB v levém navigačním panelu na **Klíče** a potom klikněte na **Klíče pro čtení i zápis**. V dalším kroku zkopírujte pomocí tlačítek kopírování na pravé straně obrazovky identifikátor URI a primární klíč do souboru web.config.

    ![Zobrazení a zkopírování přístupového klíče na portálu Azure Portal v okně Klíče](./media/create-documentdb-dotnet/keys.png)

2. V sadě Visual Studio 2017 otevřete soubor web.config. 

3. Z portálu zkopírujte hodnotu identifikátoru URI (pomocí tlačítka kopírování) a nastavte ji jako hodnotu klíče koncového bodu v souboru web.config. 

    `<add key="endpoint" value="FILLME" />`

4. Pak z portálu zkopírujte hodnotu PRIMÁRNÍHO KLÍČE a nastavte ji jako hodnotu authKey v souboru web.config. Teď jste aktualizovali aplikaci a zadali do ní všechny informace potřebné ke komunikaci s Azure Cosmos DB. 

    `<add key="authKey" value="FILLME" />`
    
<a id="run-the-web-app" class="xliff"></a>

## Spuštění webové aplikace
1. V sadě Visual Studio klikněte v **Průzkumníku řešení** pravým tlačítkem myši na projekt a potom klikněte na **Spravovat balíčky NuGet**. 

2. Do pole **Procházet** v NuGetu zadejte *DocumentDB*.

3. Z výsledků nainstalujte knihovnu **Microsoft.Azure.DocumentDB**. Tím se nainstaluje balíček Microsoft.Azure.DocumentDB a zároveň všechny závislosti.

4. Spusťte aplikaci stisknutím CTRL+F5. Aplikace se zobrazí v prohlížeči. 

5. V prohlížeči klikněte na **Vytvořit nový** a vytvořte v aplikaci seznamu úkolů několik nových úloh.

   ![Aplikace seznamu úkolů s ukázkovými daty](./media/create-documentdb-dotnet/azure-comosdb-todo-app-list.png)

Teď se můžete vrátit do Průzkumníku dat a zobrazit dotaz nebo provést úpravy a pracovat s těmito novými daty. 

<a id="review-slas-in-the-azure-portal" class="xliff"></a>

## Ověření podmínek SLA na portálu Azure Portal

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

<a id="clean-up-resources" class="xliff"></a>

## Vyčištění prostředků

Pokud nebudete tuto aplikace nadále používat, odstraňte na základě následujícího postupu z portálu Azure Portal všechny prostředky vytvořené podle tohoto rychlého startu:

1. V nabídce vlevo na portálu Azure Portal klikněte na **Skupiny prostředků** a pak klikněte na název vytvořeného prostředku. 
2. Na stránce skupiny prostředků klikněte na **Odstranit**, do textového pole zadejte prostředek, který chcete odstranit, a pak klikněte na **Odstranit**.

<a id="next-steps" class="xliff"></a>

## Další kroky

V tomto rychlém startu jste se seznámili s postupem vytvoření účtu Azure Cosmos DB, vytvoření kolekce pomocí Průzkumníka dat a spuštění webové aplikace. Teď můžete do účtu Cosmos DB importovat další data. 

> [!div class="nextstepaction"]
> [Importování dat do služby Azure Cosmos DB](import-data.md)



