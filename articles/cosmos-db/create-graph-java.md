---
title: "Vytvoření aplikace Java databáze Azure Cosmos využívající rozhraní API Graph | Dokumentace Microsoftu"
description: "Představuje ukázku kódu Java, který můžete použít k připojení a dotazování dat grafu Azure Cosmos DB pomocí konzoly Gremlin."
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
ms.openlocfilehash: 8eac406c6ef96d7ae8dd5f4931c7d16edb723be8
ms.contentlocale: cs-cz
ms.lasthandoff: 07/17/2017

---
# <a name="azure-cosmos-db-build-a-java-application-using-the-graph-api"></a>Databáze Azure Cosmos: Vytvoření aplikace Java využívající rozhraní API Graph

Databáze Azure Cosmos je databázová služba Microsoftu s více modely použitelná v celosvětovém měřítku. Můžete snadno vytvořit a dotazovat databáze dotazů, klíčů/hodnot a grafů, které tak můžou využívat výhody použitelnosti v celosvětovém měřítku a možností horizontálního škálování v jádru databáze Azure Cosmos. 

Tento rychlý start popisuje způsob vytvoření účtu služby Azure Cosmos DB pro rozhraní Graph API (verze Preview), databáze a grafu pomocí webu Azure Portal. Potom sestavíte a spustíte aplikaci konzoly pomocí ovladače OSS [Gremlin Java](https://mvnrepository.com/artifact/org.apache.tinkerpop/gremlin-driver).  

## <a name="prerequisites"></a>Požadavky

* Než budete moct tuto ukázku spustit, je potřeba splnit následující požadavky:
   * JDK 1.7+ (pokud sadu JDK nemáte, spusťte `apt-get install default-jdk`); je třeba nastavit proměnné prostředí, jako například `JAVA_HOME`
   * Maven (pokud nemáte Maven, spusťte `apt-get install maven`)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-a-database-account"></a>Vytvoření účtu databáze

[!INCLUDE [cosmos-db-create-dbaccount-graph](../../includes/cosmos-db-create-dbaccount-graph.md)]

## <a name="add-a-graph"></a>Přidání grafu

[!INCLUDE [cosmos-db-create-graph](../../includes/cosmos-db-create-graph.md)]

## <a name="clone-the-sample-application"></a>Klonování ukázkové aplikace

Teď naklonujeme aplikaci rozhraní Graph API (verze Preview) z GitHubu, nastavíme připojovací řetězec a spustíme ji. Uvidíte, jak snadno se pracuje s daty prostřednictvím kódu programu. 

1. Otevřete okno terminálu Git, jako je třeba Git Bash, a pomocí `cd` přejděte do pracovního adresáře.  

2. Ukázkové úložiště naklonujete spuštěním následujícího příkazu. 

    ```bash
    git clone https://github.com/Azure-Samples/azure-cosmos-db-graph-java-getting-started.git
    ```

## <a name="review-the-code"></a>Kontrola kódu

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
## <a name="update-your-connection-string"></a>Aktualizace připojovacího řetězce

1. Otevřete soubor src/remote.yaml. 

3. V souboru src/remote.yaml vyplňte konfigurace *Hostitel*, *Port*, *Uživatelské jméno*, *Heslo*, *Fond připojení* a *Serializátor*:

    Nastavení|Navrhovaná hodnota|Popis
    ---|---|---
    Hostitelé|[***.graphs.azure.com]|Viz snímek obrazovky níže. Toto je hodnota Gremlin URI na stránce Přehled na webu Azure Portal v hranatých závorkách a s odebraným řetězcem „:443/“ na konci.<br><br>Tuto hodnotu můžete získat také z karty Klíče s použitím hodnoty URI, ve které odeberete „https://“, změníte „documents“ na „graphs“ a odeberete „:443/“ na konci.
    Port|443|Nastavte na hodnotu 443.
    Uživatelské jméno|*Vaše uživatelské jméno*|Prostředek ve formátu `/dbs/<db>/colls/<coll>`, kde `<db>` je název vaší databáze a `<coll>` je název vaší kolekce.
    Heslo|*Primární hlavní klíč*|Viz druhý snímek obrazovky níže. Toto je váš primární klíč, který můžete získat ze stránky Klíče na webu Azure Portal v poli Primární klíč. Pomocí tlačítka pro kopírování na levé straně pole hodnotu zkopírujte.
    Fond připojení|{enableSsl: true}|Nastavení fondu připojení pro protokol SSL.
    Serializátor|{ className: org.apache.tinkerpop.gremlin.<br>driver.ser.GraphSONMessageSerializerV1d0,<br> config: { serializeResultToString: true }}|Nastavte na tuto hodnotu a odstraňte případné konce řádků `\n` vzniklé vložením hodnoty.

    Pro hodnotu Hostitelé zkopírujte hodnotu **Gremlin URI** ze stránky **Přehled**: ![Zobrazení a zkopírování hodnoty Gremlin URI na stránce Přehled na webu Azure Portal](./media/create-graph-java/gremlin-uri.png)

    Pro hodnotu Heslo zkopírujte **Primární klíč** ze stránky **Klíče**: ![Zobrazení a zkopírování primárního klíče na stránce Klíče na webu Azure Portal](./media/create-graph-java/keys.png)

## <a name="run-the-console-app"></a>Spuštění aplikace konzoly

1. Spusťte v terminálu `mvn package`, aby se nainstalovaly požadované balíčky Java.

2. Spuštění v terminálu `mvn exec:java -D exec.mainClass=GetStarted.Program`, aby se spustila aplikace Java.

Teď se můžete vrátit do Průzkumníku dat a zobrazit dotaz nebo provést úpravy a pracovat s těmito novými daty. 

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


