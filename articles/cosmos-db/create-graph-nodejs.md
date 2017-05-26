---
title: "Vytvoření aplikace Node.js databáze Azure Cosmos využívající rozhraní Graph API | Dokumentace Microsoftu"
description: "Obsahuje ukázku kódu Node.js, kterou můžete použít pro připojení a dotazování databáze Azure Cosmos."
services: cosmosdb
documentationcenter: 
author: mimig1
manager: jhubbard
editor: 
ms.assetid: daacbabf-1bb5-497f-92db-079910703046
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
ms.openlocfilehash: 45adf2accd3d9f43bc1d73b9ff93cc34d4d7c90a
ms.contentlocale: cs-cz
ms.lasthandoff: 05/10/2017


---
# <a name="azure-cosmos-db-build-a-nodejs-application-using-the-graph-api"></a>Databáze Azure Cosmos: Vytvoření aplikace Node.js využívající rozhraní Graph API

Databáze Azure Cosmos je databázová služba Microsoftu s více modely použitelná v celosvětovém měřítku. Můžete snadno vytvořit a dotazovat databáze dotazů, klíčů/hodnot a grafů, které tak můžou využívat výhody použitelnosti v celosvětovém měřítku a možností horizontálního škálování v jádru databáze Azure Cosmos. 

Tento rychlý start popisuje způsob vytvoření účtu služby Azure Cosmos DB pro rozhraní Graph API (verze Preview), databáze a grafu pomocí webu Azure Portal. Potom sestavíte a spustíte aplikaci konzoly pomocí ovladače OSS [Gremlin Node.js](https://aka.ms/gremlin-node).  

## <a name="prerequisites"></a>Požadavky

* Než budete moct tuto ukázku spustit, je potřeba splnit následující požadavky:
    * [Node.js](https://nodejs.org/en/) verze 0.10.29 nebo vyšší
    * [Git](http://git-scm.com/)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-a-database-account"></a>Vytvoření účtu databáze

[!INCLUDE [cosmosdb-create-dbaccount-graph](../../includes/cosmosdb-create-dbaccount-graph.md)]

## <a name="add-a-graph"></a>Přidání grafu

[!INCLUDE [cosmosdb-create-graph](../../includes/cosmosdb-create-graph.md)]

## <a name="clone-the-sample-application"></a>Klonování ukázkové aplikace

Teď naklonujeme aplikaci rozhraní Graph API z GitHubu, nastavíme připojovací řetězec a spustíme ji. Přesvědčíte se, jak snadno se pracuje s daty prostřednictvím kódu programu. 

1. Otevřete okno terminálu Git, jako je třeba Git Bash, a pomocí `cd` přejděte do pracovního adresáře.  

2. Ukázkové úložiště naklonujete spuštěním následujícího příkazu. 

    ```bash
    git clone https://github.com/Azure-Samples/azure-cosmos-db-graph-nodejs-getting-started.git
    ```

3. Potom otevřete soubor řešení v sadě Visual Studio. 

## <a name="review-the-code"></a>Kontrola kódu

Ještě jednou se stručně podívejme na to, co se v aplikaci děje. Po otevření souboru `app.js` uvidíte tyto řádky kódu.

* Vytvoří se klient Gremlin.

    ```nodejs
    const client = Gremlin.createClient(
        443, 
        "https://<fillme>.graphs.azure.com", 
        { 
            "session": false, 
            "ssl": true, 
            "user": "/dbs/<db>/colls/<coll>",
            "password": "<authKey>"
        });
    ```

* Pomocí metody `client.execute` se provede série kroků konzoly Gremlin.

    ```nodejs
    console.log('Running Count'); 
    client.execute("g.V().count()", { }, (err, results) => {
        if (err) return console.error(err);
        console.log(JSON.stringify(results));
        console.log();
    });
    ```

## <a name="update-your-connection-string"></a>Aktualizace připojovacího řetězce

Teď se vraťte zpátky na web Azure Portal, kde najdete informace o připojovacím řetězci, a zkopírujte je do aplikace.

1. Na webu [Azure Portal](http://portal.azure.com/) klikněte v účtu databáze Azure Cosmos v levém navigačním panelu na možnost **Klíče** a potom klikněte na **Klíče pro čtení i zápis**. V dalším kroku zkopírujete pomocí tlačítek kopírování na pravé straně obrazovky identifikátor URI a primární klíč do souboru `app.js`.

    ![Zobrazení a zkopírování přístupového klíče na webu Azure Portal v okně Klíče](./media/create-documentdb-dotnet/keys.png)

2. V souboru `app.js` vyplňte konfigurace *endpoint*, *db*, *coll* a *authKey*:

    ```nodejs
    const client = Gremlin.createClient(
        443, 
        config.endpoint, 
        { 
            "session": false, 
            "ssl": true, 
            "user": `/dbs/${config.database}/colls/${config.collection}`,
            "password": config.primaryKey
        });
    ```

## <a name="run-the-console-app"></a>Spuštění aplikace konzoly

1. Spusťte v terminálu `npm install`, aby se nainstalovaly požadované moduly NPM.

2. Nahraďte obsah složky `node_modules\gremlin` zdrojovým kódem z [rozvětvení Gremlin databáze Cosmos](https://github.com/CosmosDB/gremlin-javascript) s podporou SSL a SASL, která je pro databázi Azure Cosmos nezbytná, ale ovladač ji v současné době neposkytuje (dočasně, dokud se v ovladači nepřijmou změny).

2. Spuštění v terminálu `node app.js`, aby se spustila aplikace uzlu.

Teď se můžete vrátit do Průzkumníku dat a zobrazit dotaz nebo provést úpravy a pracovat s těmito novými daty. 

## <a name="browse-using-the-data-explorer"></a>Procházení pomocí Průzkumníku dat

Teď se můžete vrátit do Průzkumníku dat na webu Azure Portal, procházet nová data grafu a zadávat na ně dotazy.

* V Průzkumníku dat se nová databáze zobrazí v podokně Kolekce. Rozbalte položky **graphdb** a **graphcoll** a potom klikněte na **Graph** (Graf).

    V podokně Graphs (Grafy) se zobrazí data vygenerovaná ukázkovou aplikací.

## <a name="review-slas-in-the-azure-portal"></a>Ověření smluv SLA na webu Azure Portal

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmosdb-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a>Vyčištění prostředků

Pokud nebudete tuto aplikaci dál používat, odstraňte z webu Azure Portal všechny prostředky vytvořené v tomto rychlém startu pomocí následujícího postupu: 

1. V levé nabídce na webu Azure Portal klikněte na **Skupiny prostředků** a potom klikněte na název vytvořeného prostředku. 
2. Na stránce skupiny prostředků klikněte na **Odstranit**, do textového pole zadejte název prostředku, který chcete odstranit, a potom klikněte na **Odstranit**.

## <a name="next-steps"></a>Další kroky

V tomto rychlém startu jste se seznámili s postupem vytvoření účtu databáze Azure Cosmos, vytvoření grafu pomocí Průzkumníku dat a spuštění aplikace. Teď můžete pomocí konzoly Gremlin vytvářet složitější dotazy a implementovat účinnou logiku procházení grafů. 

> [!div class="nextstepaction"]
> [Dotazování pomocí konzoly Gremlin](tutorial-query-graph.md)


