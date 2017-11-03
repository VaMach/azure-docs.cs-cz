---
title: "Vytvoření aplikace .NET služby Azure Cosmos DB pomocí rozhraní API tabulky | Dokumentace Microsoftu"
description: "Začínáme s rozhraním API tabulek Azure Cosmos DB pomocí technologie .NET"
services: cosmos-db
documentationcenter: 
author: arramac
manager: jhubbard
editor: 
ms.assetid: 66327041-4d5e-4ce6-a394-fee107c18e59
ms.service: cosmos-db
ms.custom: quick start connect, mvc
ms.workload: 
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: quickstart
ms.date: 06/22/2017
ms.author: arramac
ms.openlocfilehash: 0ce99a4754d7ec6f35bda63af6fc0166cf7e0eb4
ms.sourcegitcommit: b979d446ccbe0224109f71b3948d6235eb04a967
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/25/2017
---
# <a name="azure-cosmos-db-build-a-net-application-using-the-table-api"></a>Azure Cosmos DB: Vytvoření aplikace .NET pomocí rozhraní API tabulky

Azure Cosmos DB je globálně distribuovaná databázová služba Microsoftu pro více modelů. Můžete snadno vytvořit a dotazovat databáze dotazů, klíčů/hodnot a grafů, které tak můžou využívat výhody použitelnosti v celosvětovém měřítku a možností horizontálního škálování v jádru Azure Cosmos DB. 

Tento rychlý start ukazuje, jak vytvořit databázový účet Azure Cosmos DB a tabulku pod tímto účtem pomocí portálu Azure Portal. Potom napíšete kód pro vložení, aktualizování a odstranění entit a pomocí balíčku od NuGetu [Windows Azure Storage Premium Table](https://aka.ms/premiumtablenuget) (Preview) spustíte některé dotazy. Tato knihovna má nejen stejné podpisy tříd a metod jako veřejná sada [Windows Azure Storage SDK](https://www.nuget.org/packages/WindowsAzure.Storage), ale také možnost připojení k účtům Azure Cosmos DB pomocí [rozhraní API tabulky](table-introduction.md) (Preview). 

## <a name="prerequisites"></a>Požadavky

Pokud ještě nemáte nainstalovanou sadu Visual Studio 2017, můžete stáhnout a použít **bezplatnou verzi** [Visual Studio 2017 Community Edition](https://www.visualstudio.com/downloads/). Nezapomeňte při instalaci sady Visual Studio povolit možnost **Azure Development**.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-a-database-account"></a>Vytvoření účtu databáze

[!INCLUDE [cosmos-db-create-dbaccount-table](../../includes/cosmos-db-create-dbaccount-table.md)]

## <a name="add-a-table"></a>Přidání tabulky

[!INCLUDE [cosmos-db-create-table](../../includes/cosmos-db-create-table.md)]

## <a name="add-sample-data"></a>Přidání ukázkových dat

Teď můžete přidávat do nové tabulky data pomocí Průzkumníku dat (Preview).

1. V Průzkumníku dat rozbalte **ukázkovou tabulku**, klikněte na **Entity** a potom klikněte na **Přidat entitu**.

   ![Vytváření nových entit v Průzkumníku dat na portálu Azure Portal](./media/create-table-dotnet/azure-cosmosdb-data-explorer-new-document.png)
2. Teď přidejte data do hodnoty pole PartitionKey a do hodnoty pole RowKey a klikněte na **Přidat entitu**.

   ![Nastavení klíče oddílu a klíče řádku pro novou entitu](./media/create-table-dotnet/azure-cosmosdb-data-explorer-new-entity.png)
  
    Teď můžete přidat další entity do tabulky, upravit si entity nebo zadat dotazy na data v Průzkumníku dat. V Průzkumníku dat také můžete škálovat propustnost a přidat do tabulky uložené procedury, uživatelsky definované funkce a aktivační události.

## <a name="clone-the-sample-application"></a>Klonování ukázkové aplikace

Teď naklonujeme aplikaci Table z GitHubu, nastavíme připojovací řetězec a spustíme ji. Přesvědčíte se, jak snadno se pracuje s daty prostřednictvím kódu programu. 

1. Otevřete okno terminálu Git, jako je třeba Git Bash, a pomocí `cd` přejděte do pracovního adresáře.  

2. Ukázkové úložiště naklonujete spuštěním následujícího příkazu. 

    ```bash
    git clone https://github.com/Azure-Samples/azure-cosmos-db-table-dotnet-getting-started.git
    ```

3. Potom otevřete soubor řešení v sadě Visual Studio. 

## <a name="review-the-code"></a>Kontrola kódu

Ještě jednou se stručně podívejme na to, co se v aplikaci děje. Otevřete soubor Program.cs a zjistíte, že tyto řádky kódu vytvářejí prostředky databáze Azure Cosmos. 

* Inicializuje se CloudTableClient.

    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(connectionString); 
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();
    ```

* Vytvoří se nová tabulka, pokud ještě neexistuje.

    ```csharp
    CloudTable table = tableClient.GetTableReference("people");
    table.CreateIfNotExists();
    ```

* Sérii kroků, které jsou spouštěny na tabulky pomocí `TableOperation` třídy.

   ```csharp
   TableOperation insertOperation = TableOperation.Insert(item);
   table.Execute(insertOperation);
   ```
   
   ```csharp
   TableOperation retrieveOperation = TableOperation.Retrieve<T>(items[i].PartitionKey, items[i].RowKey);
   table.Execute(retrieveOperation);
   ```
   
   ```csharp
   TableOperation deleteOperation = TableOperation.Delete(items[i]);
   table.Execute(deleteOperation);
   ```


## <a name="update-your-connection-string"></a>Aktualizace připojovacího řetězce

Nyní aktualizujeme informace připojovacího řetězce, aby vaše aplikace mohla komunikovat s databází Azure Cosmos. 

1. V sadě Visual Studio otevřete soubor app.config. 

2. Na portálu [Azure portal](http://portal.azure.com/) v levé navigační nabídce databáze Azure Cosmos klikněte na **Připojovací řetězec**. Pak v novém podokně klikněte u připojovacího řetězce na tlačítko kopírovat. 

    ![Zobrazení a kopírování koncového bodu a klíče účtu v podokně Připojovací řetězec](./media/create-table-dotnet/keys.png)

3. Tuto hodnotu vložte do souboru app.config jako hodnotu řetězce PremiumStorageConnectionString. 

    `<add key="PremiumStorageConnectionString" 
        value="DefaultEndpointsProtocol=https;AccountName=MYSTORAGEACCOUNT;AccountKey=AUTHKEY;TableEndpoint=https://COSMOSDB.documents.azure.com" />`    

    Řetězec StandardStorageConnectionString můžete nechat beze změny.

Teď jste aktualizovali aplikaci a zadali do ní všechny informace potřebné ke komunikaci s Azure Cosmos DB. 

## <a name="run-the-console-app"></a>Spuštění aplikace konzoly

1. V sadě Visual Studio klikněte v **Průzkumníku řešení** pravým tlačítkem myši na projekt **PremiumTableGetStarted** a potom klikněte na možnost **Spravovat balíčky NuGet**. 

2. Do pole **Procházet** v NuGetu zadejte *WindowsAzure.Storage-PremiumTable*.

3. Zaškrtněte políčko **Zahrnout předběžné verze**. 

4. Z výsledků nainstalujte knihovnu **WindowsAzure.Storage-PremiumTable**. Tím se nainstaluje náhled balíčku s rozhraním API tabulek Azure Cosmos DB a také všechny závislosti. Všimněte si, že toto je jiný balíček NuGet než balíček Windows Azure Storage používaný úložištěm tabulek v Azure. 

5. Spusťte aplikaci stisknutím CTRL+F5.

    Okno konzoly zobrazí přidávání, načítání, nahrazování a odstraňování dat z tabulky a zadávání dotazů na ně. Po dokončení skriptu stisknutím jakékoli klávesy zavřete okno konzoly. 
    
    ![Výstup konzoly rychlého startu](./media/create-table-dotnet/azure-cosmosdb-table-quickstart-console-output.png)

6. Pokud chcete v Průzkumníku dat zobrazit nové entity, okomentujte v souboru program.cs řádky 188–208, aby se neodstranily, a spusťte ukázku znovu. 

    Nyní můžete přejít zpět do Průzkumníku dat, kliknout na **Aktualizovat**, rozbalit tabulku **lidé** a kliknout na **Entity**, poté můžete s těmito novými daty pracovat. 

    ![Nové entity v Průzkumníku dat](./media/create-table-dotnet/azure-cosmosdb-table-quickstart-data-explorer.png)

## <a name="review-slas-in-the-azure-portal"></a>Ověření smluv SLA na webu Azure Portal

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a>Vyčištění prostředků

Pokud nebudete tuto aplikace nadále používat, odstraňte na základě následujícího postupu z portálu Azure Portal všechny prostředky vytvořené podle tohoto rychlého startu: 

1. V nabídce vlevo na portálu Azure Portal klikněte na **Skupiny prostředků** a pak klikněte na název vytvořeného prostředku. 
2. Na stránce skupiny prostředků klikněte na **Odstranit**, do textového pole zadejte prostředek, který chcete odstranit, a pak klikněte na **Odstranit**.

## <a name="next-steps"></a>Další kroky

V tomto rychlém startu jste se seznámili s postupem vytvoření databázového účtu Azure Cosmos DB, vytvoření tabulky pomocí Průzkumníka dat a spuštění aplikace.  Teď můžete zadávat dotazy na svá data pomocí rozhraní API tabulky.  

> [!div class="nextstepaction"]
> [Dotazy pomocí rozhraní API tabulky](tutorial-query-table.md)

