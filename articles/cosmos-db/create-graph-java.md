---
title: "Vytvoření aplikace Java databáze Azure Cosmos využívající rozhraní API Graph | Dokumentace Microsoftu"
description: "Představuje ukázku kódu Java, který můžete použít k připojení a dotazování dat grafu Azure Cosmos DB pomocí konzoly Gremlin."
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
ms.date: 06/27/2017
ms.author: arramac
ms.translationtype: Human Translation
ms.sourcegitcommit: 857267f46f6a2d545fc402ebf3a12f21c62ecd21
ms.openlocfilehash: d9619bd9a012a347634282788b3a318886967a3f
ms.contentlocale: cs-cz
ms.lasthandoff: 06/28/2017


---
<a id="azure-cosmos-db-build-a-java-application-using-the-graph-api" class="xliff"></a>

# Databáze Azure Cosmos: Vytvoření aplikace Java využívající rozhraní API Graph

Databáze Azure Cosmos je databázová služba Microsoftu s více modely použitelná v celosvětovém měřítku. Můžete snadno vytvořit a dotazovat databáze dotazů, klíčů/hodnot a grafů, které tak můžou využívat výhody použitelnosti v celosvětovém měřítku a možností horizontálního škálování v jádru databáze Azure Cosmos. 

Tento rychlý start popisuje způsob vytvoření účtu služby Azure Cosmos DB pro rozhraní Graph API (verze Preview), databáze a grafu pomocí webu Azure Portal. Potom sestavíte a spustíte aplikaci konzoly pomocí ovladače OSS [Gremlin Java](https://mvnrepository.com/artifact/org.apache.tinkerpop/gremlin-driver).  

<a id="prerequisites" class="xliff"></a>

## Požadavky

* Než budete moct tuto ukázku spustit, je potřeba splnit následující požadavky:
   * JDK 1.7+ (pokud sadu JDK nemáte, spusťte `apt-get install default-jdk`); je třeba nastavit proměnné prostředí, jako například `JAVA_HOME`
   * Maven (pokud nemáte Maven, spusťte `apt-get install maven`)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

<a id="create-a-database-account" class="xliff"></a>

## Vytvoření účtu databáze

[!INCLUDE [cosmos-db-create-dbaccount-graph](../../includes/cosmos-db-create-dbaccount-graph.md)]

<a id="add-a-graph" class="xliff"></a>

## Přidání grafu

[!INCLUDE [cosmos-db-create-graph](../../includes/cosmos-db-create-graph.md)]

<a id="clone-the-sample-application" class="xliff"></a>

## Klonování ukázkové aplikace

Teď naklonujeme aplikaci rozhraní Graph API (verze Preview) z GitHubu, nastavíme připojovací řetězec a spustíme ji. Uvidíte, jak snadno se pracuje s daty prostřednictvím kódu programu. 

1. Otevřete okno terminálu Git, jako je třeba Git Bash, a pomocí `cd` přejděte do pracovního adresáře.  

2. Ukázkové úložiště naklonujete spuštěním následujícího příkazu. 

    ```bash
    git clone https://github.com/Azure-Samples/azure-cosmos-db-graph-java-getting-started.git
    ```

<a id="review-the-code" class="xliff"></a>

## Kontrola kódu

Ještě jednou se stručně podívejme na to, co se v aplikaci děje. Po otevření souboru `Program.java` uvidíte tyto řádky kódu. 

* Inicializuje se konzola Gremlin `Client` z konfigurace v nástroji `src/remote.yaml`.

    ```java
    Cluster cluster = Cluster.build(new File("src/remote.yaml")).create();
    
    Client client = cluster.connect();
    ```

* Pomocí metody `client.submit` se provede série kroků konzoly Gremlin.

    ```java
    ResultSet results = client.submit("g.V()");

    CompletableFuture<List<Result>> completableFutureResults = results.all();
    List<Result> resultList = completableFutureResults.get();

    for (Result result : resultList) {
        System.out.println(result.toString());
    }
    ```
<a id="update-your-connection-string" class="xliff"></a>

## Aktualizace připojovacího řetězce

Teď se vraťte zpátky na portál Azure Portal, kde najdete informace o připojovacím řetězci, a zkopírujte je do aplikace.

1. Na webu [Azure Portal](http://portal.azure.com/) klikněte v účtu databáze Azure Cosmos v levém navigačním panelu na možnost **Klíče** a potom klikněte na **Klíče pro čtení i zápis**. V dalším kroku zkopírujete pomocí tlačítek kopírování na pravé straně obrazovky identifikátor URI a primární klíč do souboru `Program.java`.

    ![Zobrazení a zkopírování přístupového klíče na portálu Azure Portal v okně Klíče](./media/create-graph-java/keys.png)

2. Otevřete soubor `src/remote.yaml`. 

3. Vyplňte konfigurace *Hostitel*, *Port*, *Uživatelské jméno*, *Heslo*, *Fond připojení* a *Serializátor* v souboru `src/remote.yaml`:

    Nastavení|Navrhovaná hodnota|Popis
    ---|---|---
    Hostitelé|***.graphs.azure.com|Identifikátor URI služby grafu, který můžete načíst z portálu Azure Portal
    Port|443|Nastavte na hodnotu 443
    Uživatelské jméno|*Vaše uživatelské jméno*|Prostředek formuláře `/dbs/<db>/colls/<coll>`.
    Heslo|*Primární hlavní klíč*|Váš primární hlavní klíč ke službě Azure Cosmos DB
    Fond připojení|{enableSsl: true}|Nastavení fondu připojení pro protokol SSL
    Serializátor|{ className:org.apache.tinkerpop.gremlin.<br>driver.ser.GraphSONMessageSerializerV1d0,<br> config: { serializeResultToString: true }}|Nastavte na tuto hodnotu

<a id="run-the-console-app" class="xliff"></a>

## Spuštění aplikace konzoly

1. Spusťte v terminálu `mvn package`, aby se nainstalovaly požadované balíčky Java.

2. Spuštění v terminálu `mvn exec:java -D exec.mainClass=GetStarted.Program`, aby se spustila aplikace Java.

Teď se můžete vrátit do Průzkumníku dat a zobrazit dotaz nebo provést úpravy a pracovat s těmito novými daty. 

<a id="browse-using-the-data-explorer" class="xliff"></a>

## Procházení pomocí Průzkumníku dat

Teď se můžete vrátit do Průzkumníku dat na webu Azure Portal, procházet nová data grafu a zadávat na ně dotazy.

* V Průzkumníku dat se nová databáze zobrazí v podokně Kolekce. Rozbalte položky **graphdb** a **graphcoll** a potom klikněte na **Graph** (Graf).

    V podokně Graphs (Grafy) se zobrazí data vygenerovaná ukázkovou aplikací.

<a id="review-slas-in-the-azure-portal" class="xliff"></a>

## Ověření smluv SLA na webu Azure Portal

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

<a id="clean-up-resources" class="xliff"></a>

## Vyčištění prostředků

Pokud nebudete tuto aplikace nadále používat, odstraňte na základě následujícího postupu z portálu Azure Portal všechny prostředky vytvořené podle tohoto rychlého startu: 

1. V nabídce vlevo na portálu Azure Portal klikněte na **Skupiny prostředků** a pak klikněte na název vytvořeného prostředku. 
2. Na stránce skupiny prostředků klikněte na **Odstranit**, do textového pole zadejte prostředek, který chcete odstranit, a pak klikněte na **Odstranit**.

<a id="next-steps" class="xliff"></a>

## Další kroky

V tomto rychlém startu jste se seznámili s postupem vytvoření účtu databáze Azure Cosmos, vytvoření grafu pomocí Průzkumníku dat a spuštění aplikace. Teď můžete pomocí konzoly Gremlin vytvářet složitější dotazy a implementovat účinnou logiku procházení grafů. 

> [!div class="nextstepaction"]
> [Dotazování pomocí konzoly Gremlin](tutorial-query-graph.md)


