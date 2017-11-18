---
title: "Rychlý úvod: Tabulky rozhraní API pomocí Node.js – Azure Cosmos DB | Microsoft Docs"
description: "Tento rychlý start ukazuje, jak vytvořit aplikaci pomocí portálu Azure a Node.js pomocí rozhraní API služby Azure DB Cosmos tabulky"
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
ms.devlang: node
ms.topic: quickstart
ms.date: 11/16/2017
ms.author: arramac
ms.openlocfilehash: 1dcc2178b3c7017338e0097773fbf0d04c8b6a20
ms.sourcegitcommit: a036a565bca3e47187eefcaf3cc54e3b5af5b369
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/17/2017
---
# <a name="quickstart-build-a-table-api-app-with-nodejs-and-azure-cosmos-db"></a>Rychlý úvod: Sestavení tabulku aplikace API pomocí Node.js a Azure Cosmos DB

Tento rychlý start ukazuje způsob použití Node.js a Azure Cosmos DB [tabulky API](table-introduction.md) k sestavení aplikace klonováním příklad z Githubu. Tento rychlý start také ukazuje postup vytvoření účtu Azure Cosmos DB a použití Průzkumníku dat k vytvoření tabulky a entity na portálu Azure založených na webu.

Databáze Azure Cosmos je databázová služba Microsoftu s více modely použitelná v celosvětovém měřítku. Můžete rychle vytvořit a dotazovat dokumentu, klíč/hodnota, celou sloupce a graf databází, které těžit z globální distribuci a možnosti vodorovné škálování základem Azure Cosmos DB. 

## <a name="prerequisites"></a>Požadavky

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]
[!INCLUDE [cosmos-db-emulator-docdb-api](../../includes/cosmos-db-emulator-docdb-api.md)]

Navíc platí:

* [Node.js](https://nodejs.org/en/) verze 0.10.29 nebo vyšší
* [Git](http://git-scm.com/)

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
    git clone https://github.com/Azure-Samples/storage-table-node-getting-started.git
    ```

## <a name="update-your-connection-string"></a>Aktualizace připojovacího řetězce

Teď se vraťte zpátky na portál Azure Portal, kde najdete informace o připojovacím řetězci, a zkopírujte je do aplikace. To umožňuje aplikaci ke komunikaci s vaší hostované databází. 

1. V [portál Azure](http://portal.azure.com/), klikněte na tlačítko **připojovací řetězec**. 

    ![Zobrazení a zkopírujte řetězec informace o požadované připojení z v podokně připojovací řetězec](./media/create-table-nodejs/connection-string.png)

2. Zkopírujte primární PŘIPOJOVACÍ řetězec pomocí tlačítko Kopírovat na pravé straně.

3. Otevřete soubor app.config a vložte hodnotu do připojovacího řetězce na řádku tři. Pokud koncový bod část připojovací řetězec používá documents.azure.com, opravte část table.cosmosdb.azure.com místo toho chcete použít.

3. Uložte soubor app.config.

Teď jste aktualizovali aplikaci a zadali do ní všechny informace potřebné ke komunikaci s Azure Cosmos DB. 

## <a name="run-the-app"></a>Spuštění aplikace

1. V okně terminálu git `cd` ke složce úložiště tabulky java-getting-started.

    ```
    cd "C:\git-samples\storage-table-node-getting-started"
    ```

2. Spusťte následující příkaz k instalaci [azure], [uzlu uuid], [nconf] a [asynchronní] moduly místně i tak, aby pro ně v souboru package.json uložte položku

   ```
   npm install azure-storage node-uuid async nconf --save
   ```

2. Okno terminálu, spusťte následující příkazy ke spuštění v gitu spustit aplikaci uzlu.

    ```
    node ./tableSample.js 
    ```

    V okně konzoly zobrazí data tabulky, který se přidává do nové tabulky databáze v Azure Cosmos DB.

    Teď se můžete vrátit do Průzkumníku dat a zobrazit dotaz nebo provést úpravy a pracovat s těmito novými daty. 

## <a name="review-slas-in-the-azure-portal"></a>Ověření podmínek SLA na portálu Azure Portal

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a>Vyčištění prostředků

[!INCLUDE [cosmosdb-delete-resource-group](../../includes/cosmos-db-delete-resource-group.md)]

## <a name="next-steps"></a>Další kroky

V tomto rychlém startu jste se seznámili s postupem vytvoření databázového účtu Azure Cosmos DB, vytvoření tabulky pomocí Průzkumníka dat a spuštění aplikace.  Teď můžete zadávat dotazy na svá data pomocí rozhraní API tabulky.  

> [!div class="nextstepaction"]
> [Importovat data tabulky do rozhraní API tabulky](table-import.md)
