---
title: "Vytvoření aplikace .NET služby Azure Cosmos DB pomocí rozhraní API tabulky | Dokumentace Microsoftu"
description: "Začínáme s rozhraním API tabulek Azure Cosmos DB pomocí technologie .NET"
services: cosmosdb
documentationcenter: 
author: arramac
manager: jhubbard
editor: 
ms.assetid: 66327041-4d5e-4ce6-a394-fee107c18e59
ms.service: cosmosdb
ms.custom: quick start connect
ms.workload: 
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 05/10/2017
ms.author: arramac
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: 73b9448153ec520d77afd1bdb65b9694e7bf7b9e
ms.contentlocale: cs-cz
ms.lasthandoff: 05/10/2017


---
# <a name="azure-cosmos-db-build-a-net-application-using-the-table-api"></a>Azure Cosmos DB: Vytvoření aplikace .NET pomocí rozhraní API tabulky

Azure Cosmos DB je globálně distribuovaná databázová služba Microsoftu pro více modelů. Můžete snadno vytvořit a dotazovat databáze dotazů, klíčů/hodnot a grafů, které tak můžou využívat výhody použitelnosti v celosvětovém měřítku a možností horizontálního škálování v jádru Azure Cosmos DB. 

Tento rychlý start ukazuje, jak vytvořit databázový účet Azure Cosmos DB a tabulku pod tímto účtem pomocí portálu Azure Portal. Potom stačí napsat kód k vložení, aktualizaci a odstranění entit a spustit potřebné dotazy. Z NuGetu si můžete si stáhnout [Azure Storage Preview SDK](https://aka.ms/premiumtablenuget), který má stejné podpisy tříd a metod jako veřejná sada [Azure Storage SDK](https://www.nuget.org/packages/WindowsAzure.Storage), ale má také možnost připojení k účtům Azure Cosmos DB pomocí [rozhraní API tabulky](table-introduction.md) (Preview). 

## <a name="prerequisites"></a>Požadavky

Pokud ještě nemáte nainstalovanou sadu Visual Studio 2017, můžete stáhnout a použít **bezplatnou verzi** [Visual Studio 2017 Community Edition](https://www.visualstudio.com/downloads/). Nezapomeňte při instalaci sady Visual Studio povolit možnost **Azure Development**.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-a-database-account"></a>Vytvoření účtu databáze

[!INCLUDE [cosmosdb-create-dbaccount-table](../../includes/cosmosdb-create-dbaccount-table.md)]

## <a name="add-a-table"></a>Přidání tabulky

[!INCLUDE [cosmosdb-create-table](../../includes/cosmosdb-create-table.md)]

## <a name="add-sample-data"></a>Přidání ukázkových dat

Teď můžete přidávat do nové tabulky data pomocí Průzkumníku dat.

1. V Průzkumníku dat rozbalte **ukázkovou databázi**, **ukázkovou tabulku**, klikněte na tlačítko **Entity**a potom klikněte na **Přidat entitu**.
2. Teď přidejte data do hodnoty pole PartitionKey a do hodnoty pole RowKey a klikněte na **Přidat entitu**.

   ![Vytváření nových dokumentů v Průzkumníku dat na portálu Azure Portal](./media/create-table-dotnet/azure-cosmosdb-data-explorer-new-document.png)
  
    Teď můžete přidat další entity do tabulky, upravit si entity nebo zadat dotazy na data v Průzkumníku dat. V Průzkumníku dat také můžete škálovat propustnost a přidat do tabulky uložené procedury, uživatelsky definované funkce a aktivační události.

## <a name="clone-the-sample-application"></a>Klonování ukázkové aplikace

Teď naklonujeme aplikaci API DocumentDB z GitHubu, nastavíme připojovací řetězec a spustíme ji. Přesvědčíte se, jak snadno se pracuje s daty prostřednictvím kódu programu. 

1. Otevřete okno terminálu Git, jako je třeba Git Bash, a pomocí `cd` přejděte do pracovního adresáře.  

2. Ukázkové úložiště naklonujete spuštěním následujícího příkazu. 

    ```bash
    git clone https://github.com/Azure-Samples/azure-cosmos-db-table-dotnet-getting-started.git
    ```

3. Potom otevřete soubor řešení v sadě Visual Studio. 

## <a name="review-the-code"></a>Kontrola kódu

Ještě jednou se stručně podívejme na to, co se v aplikaci děje. Otevřete soubor DocumentDBRepository.cs a zjistíte, že tyto řádky kódu vytvářejí prostředky Azure Cosmos DB. 

* Inicializuje se DocumentClient.

    ```csharp
    client = new DocumentClient(new Uri(ConfigurationManager.AppSettings["endpoint"]), ConfigurationManager.AppSettings["authKey"]);`
    ```

* Vytvoří se nová databáze.

    ```csharp
    await client.CreateDatabaseAsync(new Database { Id = DatabaseId });
    ```

* Vytvoří se nový kontejner grafu.

    ```csharp
    await client.CreateDocumentCollectionAsync(
        UriFactory.CreateDatabaseUri(DatabaseId),
        new DocumentCollection { Id = CollectionId },
        new RequestOptions { OfferThroughput = 1000 });
    ```

## <a name="update-your-connection-string"></a>Aktualizace připojovacího řetězce

Teď se vraťte zpátky na portál Azure Portal, kde najdete informace o připojovacím řetězci, a zkopírujte je do aplikace.

1. Na portálu [Azure Portal](http://portal.azure.com/) klikněte v účtu Azure Cosmos DB v levém navigačním panelu na **Klíče** a potom klikněte na **Klíče pro čtení i zápis**. V dalším kroku zkopírujete pomocí tlačítek kopírování na pravé straně obrazovky identifikátor URI a primární klíč do souboru app.config.

    ![Zobrazení a zkopírování přístupového klíče na portálu Azure Portal v okně Klíče](./media/create-documentdb-dotnet-core/keys.png)

2. V sadě Visual Studio otevřete soubor app.config. 

3. Název účtu Azure Cosmos DB zkopírujte z portálu a nastavte ho jako hodnotu AccountName v hodnotě řetězce PremiumStorageConnection v souboru app.config. Na snímku obrazovky výše je název účtu cosmos-db-quickstart. Název účtu se zobrazí v horní části portálu.

    `<add key="PremiumStorageConnectionString" 
        value="DefaultEndpointsProtocol=https;AccountName=MYSTORAGEACCOUNT;AccountKey=AUTHKEY;TableEndpoint=https://COSMODB.documents.azure.com" />`

4. Pak z portálu zkopírujte hodnotu primárního klíče a nastavte ji jako hodnotu AccountKey v řetězci PremiumStorageConnectionString. 

    `AccountKey=AUTHKEY`

5. Nakonec zkopírujte hodnotu identifikátoru URI ze stránky Klíče na portálu (pomocí tlačítka Kopírovat) a nastavte ji jako hodnotu TableEndpoint v řetězci PremiumStorageConnectionString.

    `TableEndpoint=https://COSMODB.documents.azure.com`

    Řetězec StandardStorageConnectionString můžete nechat beze změny.

Teď jste aktualizovali aplikaci a zadali do ní všechny informace potřebné ke komunikaci s Azure Cosmos DB. 

## <a name="run-the-web-app"></a>Spuštění webové aplikace

1. V sadě Visual Studio klikněte v **Průzkumníku řešení** pravým tlačítkem myši na projekt a potom klikněte na **Spravovat balíčky NuGet**. 

2. Do pole **Procházet** v NuGetu zadejte *WindowsAzure.Storage* a zaškrtněte políčko **Zahrnout předběžné verze**. 

3. Z výsledků nainstalujte knihovnu **WindowsAzure.Storage**. Tím se nainstaluje náhled balíčku s rozhraním API tabulek Azure Cosmos DB a také všechny závislosti.

4. Spusťte aplikaci stisknutím CTRL+F5.

    V okně konzoly se zobrazí data, která se přidávají do tabulky. Po dokončení skriptu zavřete okno konzoly. 

Teď se můžete vrátit do Průzkumníku dat a zobrazit dotaz nebo provést úpravy a pracovat s těmito novými daty. 

## <a name="review-slas-in-the-azure-portal"></a>Ověření podmínek SLA na portálu Azure Portal

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmosdb-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a>Vyčištění prostředků

Pokud nebudete tuto aplikace nadále používat, odstraňte na základě následujícího postupu z portálu Azure Portal všechny prostředky vytvořené podle tohoto rychlého startu: 

1. V nabídce vlevo na portálu Azure Portal klikněte na **Skupiny prostředků** a pak klikněte na název vytvořeného prostředku. 
2. Na stránce skupiny prostředků klikněte na **Odstranit**, do textového pole zadejte prostředek, který chcete odstranit, a pak klikněte na **Odstranit**.

## <a name="next-steps"></a>Další kroky

V tomto rychlém startu jste se seznámili s postupem vytvoření databázového účtu Azure Cosmos DB, vytvoření tabulky pomocí Průzkumníka dat a spuštění aplikace.  Teď můžete zadávat dotazy na svá data pomocí rozhraní API tabulky.  

> [!div class="nextstepaction"]
> [Dotazy pomocí rozhraní API tabulky](tutorial-query-table.md)


