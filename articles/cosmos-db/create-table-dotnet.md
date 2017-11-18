---
title: "Rychlý úvod: Tabulky rozhraní API pomocí rozhraní .NET - Azure Cosmos DB | Microsoft Docs"
description: "Tento rychlý start ukazuje, jak používat rozhraní API služby Azure DB Cosmos tabulky k vytvoření aplikace pomocí portálu Azure a rozhraní .NET"
services: cosmos-db
documentationcenter: 
author: mimig1
manager: jhubbard
editor: 
ms.assetid: 66327041-4d5e-4ce6-a394-fee107c18e59
ms.service: cosmos-db
ms.custom: quickstart connect, mvc
ms.workload: 
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: quickstart
ms.date: 11/16/2017
ms.author: mimig
ms.openlocfilehash: 4e59c333e14e5e21a02c3160cf6311d1182e5a5e
ms.sourcegitcommit: 933af6219266cc685d0c9009f533ca1be03aa5e9
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/18/2017
---
# <a name="quickstart-build-a-table-api-app-with-net-and-azure-cosmos-db"></a>Rychlý úvod: Sestavení tabulku aplikace API pomocí rozhraní .NET a Azure Cosmos DB 

Tento rychlý start ukazuje způsob použití Java a Azure Cosmos DB [tabulky API](table-introduction.md) k sestavení aplikace klonováním příklad z Githubu. Tento rychlý start také ukazuje postup vytvoření účtu Azure Cosmos DB a použití Průzkumníku dat k vytvoření tabulky a entity na portálu Azure založených na webu.

Databáze Azure Cosmos je databázová služba Microsoftu s více modely použitelná v celosvětovém měřítku. Můžete snadno vytvořit a dotazovat databáze dotazů, klíčů/hodnot a grafů, které tak můžou využívat výhody použitelnosti v celosvětovém měřítku a možností horizontálního škálování v jádru databáze Azure Cosmos. 

## <a name="prerequisites"></a>Požadavky

Pokud ještě nemáte nainstalovanou sadu Visual Studio 2017, můžete stáhnout a použít **bezplatnou verzi** [Visual Studio 2017 Community Edition](https://www.visualstudio.com/downloads/). Nezapomeňte při instalaci sady Visual Studio povolit možnost **Azure Development**.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-a-database-account"></a>Vytvoření účtu databáze

[!INCLUDE [cosmos-db-create-dbaccount-table](../../includes/cosmos-db-create-dbaccount-table.md)]

## <a name="add-a-table"></a>Přidání tabulky

[!INCLUDE [cosmos-db-create-table](../../includes/cosmos-db-create-table.md)]

## <a name="add-sample-data"></a>Přidání ukázkových dat

Teď můžete přidávat do nové tabulky data pomocí Průzkumníku dat.

1. V Průzkumníku dat rozbalte **ukázkovou tabulku**, klikněte na **Entity** a potom klikněte na **Přidat entitu**.

   ![Vytváření nových entit v Průzkumníku dat na portálu Azure Portal](./media/create-table-dotnet/azure-cosmosdb-data-explorer-new-document.png)
2. Nyní přidejte data PartitionKey hodnota pole a pole hodnota RowKey a klikněte na tlačítko **Přidat entitu**.

   ![Nastavení klíče oddílu a klíče řádku pro novou entitu](./media/create-table-dotnet/azure-cosmosdb-data-explorer-new-entity.png)
  
    Teď můžete přidat další entity do tabulky, upravit si entity nebo zadat dotazy na data v Průzkumníku dat. V Průzkumníku dat také můžete škálovat propustnost a přidat do tabulky uložené procedury, uživatelsky definované funkce a aktivační události.

## <a name="clone-the-sample-application"></a>Klonování ukázkové aplikace

Teď naklonujeme aplikaci Table z GitHubu, nastavíme připojovací řetězec a spustíme ji. Přesvědčíte se, jak snadno se pracuje s daty prostřednictvím kódu programu. 

1. Otevřete okno terminálu git, jako je například git bash a použít `cd` příkaz Přejít do složky pro instalaci ukázkové aplikace. 

    ```bash
    cd "C:\git-samples"
    ```

2. Ukázkové úložiště naklonujete spuštěním následujícího příkazu. Tento příkaz vytvoří kopii ukázková aplikace ve vašem počítači. 

    ```bash
    git clone https://github.com/Azure-Samples/storage-table-dotnet-getting-started.git
    ```

3. Poté otevřete soubor řešení TableStorage v sadě Visual Studio. 

## <a name="update-your-connection-string"></a>Aktualizace připojovacího řetězce

Teď se vraťte zpátky na portál Azure Portal, kde najdete informace o připojovacím řetězci, a zkopírujte je do aplikace. To umožňuje aplikaci ke komunikaci s vaší hostované databází. 

1. V [portál Azure](http://portal.azure.com/), klikněte na tlačítko **připojovací řetězec**. 

    Pomocí tlačítka Kopírovat na pravé straně obrazovky zkopírujte primární PŘIPOJOVACÍ řetězec.

    ![Zobrazení a zkopírujte primární PŘIPOJOVACÍ řetězec v podokně připojovací řetězec](./media/create-table-dotnet/connection-string.png)

2. V sadě Visual Studio otevřete soubor App.config. 

3. Jako tento kurz nepoužívá emulátor úložiště, zrušte komentář u StorageConnectionString na řádek 8 a Odkomentujte StorageConnectionString na řádku 7. Řádek 7 a 8 by měl nyní vypadat takto:

    ```
    <!--key="StorageConnectionString" value="UseDevelopmentStorage=true;" />-->
    <add key="StorageConnectionString" value="DefaultEndpointsProtocol=https;AccountName=[AccountName];AccountKey=[AccountKey]" />
    ```

4. PRIMÁRNÍ PŘIPOJOVACÍ řetězec z portálu vložte do StorageConnectionString hodnotu na řádek 8. Vložte řetězec uvnitř uvozovky. Pokud váš koncový bod používá documents.azure.com, změňte na table.cosmosdb.azure.com část. 

    Řádek 8 by teď měl vypadat podobně jako:

    ```
    <add key="StorageConnectionString" value="DefaultEndpointsProtocol=https;AccountName=<account name>;AccountKey=txZACN9f...==;TableEndpoint=https://<account name>.table.cosmosdb.azure.com;" />
    ```

4. Uložte soubor App.config.

Teď jste aktualizovali aplikaci a zadali do ní všechny informace potřebné ke komunikaci s Azure Cosmos DB. 

## <a name="build-and-deploy-the-app"></a>Sestavení a nasazení aplikace

1. V sadě Visual Studio, klikněte pravým tlačítkem na **TableStorage** projektu v **Průzkumníku řešení** a pak klikněte na **spravovat balíčky NuGet**. 

2. Nuget **Procházet** zadejte *Microsoft.Azure.CosmosDB.Table*.

3. Ve výsledcích nainstalovat **Microsoft.Azure.CosmosDB.Table** knihovny. Tím se nainstaluje balíček rozhraní API služby Azure Cosmos DB tabulky a také všechny závislosti.

4. Otevřete BasicSamples.cs a přidejte zarážku 30 a řádek 52.

5. Spusťte aplikaci stisknutím CTRL+F5.

    V okně konzoly zobrazí data tabulky, který se přidává do nové tabulky databáze v Azure Cosmos DB. 
    
    Pokud dojde k chybě, informace o závislostech najdete v části [Poradce při potížích s](table-sdk-dotnet.md#troubleshooting).

    Při kliknutí na první zarážky, přejděte zpět na Průzkumníku dat na portálu Azure a rozšířit tabulka ukázku * a klikněte na tlačítko **entity**. **Entity** karty na pravé straně zobrazí nová entita, která byla přidána, poznamenejte si toto telefonní číslo uživatele je 425-555-0101.
    
6. Zavřete kartu entity v Průzkumníku dat.
    
7. Pokračujte a spusťte aplikaci na další zarážku.

    Při kliknutí na zarážce, přepněte zpět na portálu, klikněte na entity znovu a otevřete kartu entity a Všimněte si, že telefonní číslo je aktualizovaná tak, aby 425-555-0105.

8. Zpět v okně konzoly stiskněte kombinaci kláves CTRL + C pro ukončení spuštění aplikace. 

    Teď můžete přejít zpět do Průzkumníku dat a přidat nebo upravit odeberte a zadávat dotazy na data.

## <a name="review-slas-in-the-azure-portal"></a>Ověření smluv SLA na webu Azure Portal

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a>Vyčištění prostředků

[!INCLUDE [cosmosdb-delete-resource-group](../../includes/cosmos-db-delete-resource-group.md)]

## <a name="next-steps"></a>Další kroky

V tomto rychlém startu jste se seznámili s postupem vytvoření databázového účtu Azure Cosmos DB, vytvoření tabulky pomocí Průzkumníka dat a spuštění aplikace.  Teď můžete zadávat dotazy na svá data pomocí rozhraní API tabulky.  

> [!div class="nextstepaction"]
> [Importovat data tabulky do rozhraní API tabulky](table-import.md)

