---
title: "Vytvoření databáze dokumentů Azure Cosmos DB pomocí Javy | Dokumentace Microsoftu"
description: "Představuje ukázku kódu Java, který můžete použít k připojení a dotazování do rozhraní API DocumentDB služby Azure Cosmos DB"
services: cosmos-db
documentationcenter: 
author: mimig1
manager: jhubbard
editor: 
ms.assetid: 89ea62bb-c620-46d5-baa0-eefd9888557c
ms.service: cosmos-db
ms.custom: quick start connect, mvc, devcenter
ms.workload: 
ms.tgt_pltfrm: na
ms.devlang: java
ms.topic: quickstart
ms.date: 10/30/2017
ms.author: mimig
ms.openlocfilehash: 5a793abdc24387ae2b758d29b9dfb25f134097d3
ms.sourcegitcommit: e5355615d11d69fc8d3101ca97067b3ebb3a45ef
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/31/2017
---
# <a name="azure-cosmos-db-create-a-document-database-using-java-and-the-azure-portal"></a>Azure Cosmos DB: Vytvoření databáze dokumentů pomocí Javy a webu Azure Portal

Databáze Azure Cosmos je databázová služba Microsoftu s více modely použitelná v celosvětovém měřítku. Pomocí Azure Cosmos DB, můžete rychle vytvořit a dotaz spravovaného dokumentu, tabulku a graf databáze.

V tomto rychlém startu se vytvoří databáze dokumentů pomocí nástrojů pro Azure Cosmos DB na webu Azure Portal. V tomto rychlém startu se také dozvíte, jak rychle vytvořit konzolovou aplikaci Java pomocí [rozhraní API Java DocumentDB](documentdb-sdk-java.md). Pokyny v tomto rychlém startu platí pro všechny operační systémy, které podporují Javu. Po dokončení tento rychlý start budete zkušenosti s vytvářením a úpravy dokumentu databáze prostředků v uživatelského rozhraní nebo prostřednictvím kódu programu, podle toho, co je vaši volbu.

## <a name="prerequisites"></a>Požadavky

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)] 
[!INCLUDE [cosmos-db-emulator-docdb-api](../../includes/cosmos-db-emulator-docdb-api.md)]

Navíc platí: 

* [Java Development Kit (JDK) 1.7+](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)
    * Na Ubuntu nainstalujte sadu JDK spuštěním příkazu `apt-get install default-jdk`.
    * Nezapomeňte nastavit proměnnou prostředí JAVA_HOME tak, aby odkazovala na složku, ve které je sada JDK nainstalovaná.
* [Stáhněte](http://maven.apache.org/download.cgi) a [nainstalujte](http://maven.apache.org/install.html) binární archiv [Maven](http://maven.apache.org/).
    * Na Ubuntu můžete Maven nainstalovat spuštěním příkazu `apt-get install maven`.
* [Git](https://www.git-scm.com/)
    * Na Ubuntu můžete Git nainstalovat spuštěním příkazu `sudo apt-get install git`.

## <a name="create-a-database-account"></a>Vytvoření účtu databáze

Než budete moct vytvořit databázi dokumentů, je potřeba pomocí služby Azure Cosmos DB vytvořit účet databáze SQL (DocumentDB).

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

## <a name="add-a-collection"></a>Přidání kolekce

[!INCLUDE [cosmos-db-create-collection](../../includes/cosmos-db-create-collection.md)]

<a id="add-sample-data"></a>
## <a name="add-sample-data"></a>Přidání ukázkových dat

Teď můžete do nové kolekce přidávat data pomocí Průzkumníka dat.

1. Rozbalte **položky** kolekce, klikněte na tlačítko **dokumenty** > **nový dokument**.

   ![Vytváření nových dokumentů v Průzkumníku dat na portálu Azure Portal](./media/create-documentdb-java/azure-cosmosdb-data-explorer-new-document.png)
  
2. Nyní přidejte dokumentu do kolekce s následující strukturou a klikněte na tlačítko **Uložit**.

     ```json
     {
         "id": "1",
         "category": "personal",
         "name": "groceries",
         "description": "Pick up apples and strawberries.",
         "isComplete": false
     }
     ```

    ![Zkopírujte data json a v Průzkumníku dat na webu Azure Portal klikněte na Uložit.](./media/create-documentdb-java/azure-cosmosdb-data-explorer-save-document.png)

3.  Vytvořte a uložte jeden další dokument, kde můžete změnit `id` 2 a změnit ostatní vlastnosti, jak můžete vidět přizpůsobit. Nové dokumenty můžou mít jakoukoli strukturu, protože Azure Cosmos DB neuplatňuje pro data žádné schéma.

## <a name="query-your-data"></a>Dotazování na data

Teď můžete použít dotazy v Průzkumníku dat k načtení a filtrovat data.

1. Že ve výchozím nastavení, dotazu nastavena na `SELECT * FROM c`. Tento výchozí dotaz načte a zobrazí všechny dokumenty v kolekci. 

    ![Výchozí dotaz v Průzkumníku dat je "vybrat * z c.](./media/create-documentdb-java/azure-cosmosdb-data-explorer-query.png)

2. Změnit dotaz kliknutím **upravit filtr** tlačítko přidání `ORDER BY c._ts DESC` predikátem pole dotazu a následným kliknutím na položku **použít filtr**.

    ![Změňte výchozí dotaz přidáním ORDER BY c._ts DESC a kliknutím na tlačítko použít filtr](./media/create-documentdb-java/azure-cosmosdb-data-explorer-edit-query.png)

Toto nastavení upravit dotaz seznamy dokumentů v sestupném pořadí podle jejich časové razítko, takže teď druhého dokumentu je uvedená jako první. Pokud jste obeznámeni se syntaxí SQL, můžete zadat libovolný z podporovaném [dotazy SQL](documentdb-sql-query.md) v tomto poli. 

Tím končí naše práci v Průzkumníku dat. Před můžeme přesunout k práce s kódem, vezměte na vědomí, že můžete taky Průzkumníku dat k vytvoření uložené procedury, funkce UDF a aktivačních událostí, provádět serverovou obchodní logiku a také škálování propustnost. Průzkumník dat zpřístupní všechna integrovaná programová data v rozhraních API, ale zajistí jednoduchý přístup k vašim datům na portálu Azure Portal.

## <a name="clone-the-sample-application"></a>Klonování ukázkové aplikace

Teď přejděme k práci s kódem. Naklonujeme aplikaci s rozhraním API DocumentDB z GitHubu, nastavíme připojovací řetězec a spustíme ji. Přesvědčíte se, jak snadno se pracuje s daty prostřednictvím kódu programu. 

1. Otevřete okno terminálu git, jako je například git bash a použít `cd` příkaz Přejít do složky pro instalaci ukázkové aplikace. 

    ```bash
    cd "C:\git-samples"
    ```

2. Ukázkové úložiště naklonujete spuštěním následujícího příkazu. Tento příkaz vytvoří kopii ukázková aplikace ve vašem počítači.

    ```bash
    git clone https://github.com/Azure-Samples/azure-cosmos-db-documentdb-java-getting-started.git
    ```

## <a name="review-the-code"></a>Kontrola kódu

Tento krok je volitelný. Pokud vás zajímá učení vytváření databázových prostředků v kódu, můžete zkontrolovat následující fragmenty kódu. Fragmenty kódu jsou převzaty z `Program.java` nainstalován ve složce C:\git-samples\azure-cosmos-db-documentdb-java-getting-started\src\GetStarted souboru. Jinak, můžete přeskočit na [aktualizovat připojovací řetězec](#update-your-connection-string). 

* `DocumentClient`Inicializace. [DocumentClient](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb._document_client) poskytuje logické zastoupení straně klienta pro službu Azure Cosmos DB databáze. Tohoto klienta se používá ke konfiguraci a spuštění požadavků na službu.

    ```java
    this.client = new DocumentClient("https://FILLME.documents.azure.com",
            "FILLME", 
            new ConnectionPolicy(),
            ConsistencyLevel.Session);
    ```

* [Databáze](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb._database) vytvoření.

    ```java
    Database database = new Database();
    database.setId(databaseName);
    
    this.client.createDatabase(database, null);
    ```

* [DocumentCollection](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb._document_collection) vytvoření.

    ```java
    DocumentCollection collectionInfo = new DocumentCollection();
    collectionInfo.setId(collectionName);

    ...

    this.client.createCollection(databaseLink, collectionInfo, requestOptions);
    ```

* Vytváření dokumentů pomocí [createDocument](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb._document_client.createdocument) metoda.

    ```java
    // Any Java object within your code can be serialized into JSON and written to Azure Cosmos DB
    Family andersenFamily = new Family();
    andersenFamily.setId("Andersen.1");
    andersenFamily.setLastName("Andersen");
    // More properties

    String collectionLink = String.format("/dbs/%s/colls/%s", databaseName, collectionName);
    this.client.createDocument(collectionLink, family, new RequestOptions(), true);
    ```

* Dotazy SQL za JSON se provádí pomocí [dokumenty dotazu](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb._document_client.querydocuments) metoda.

    ```java
    FeedOptions queryOptions = new FeedOptions();
    queryOptions.setPageSize(-1);
    queryOptions.setEnableCrossPartitionQuery(true);

    String collectionLink = String.format("/dbs/%s/colls/%s", databaseName, collectionName);
    FeedResponse<Document> queryResults = this.client.queryDocuments(
        collectionLink,
        "SELECT * FROM Family WHERE Family.lastName = 'Andersen'", queryOptions);

    System.out.println("Running SQL query...");
    for (Document family : queryResults.getQueryIterable()) {
        System.out.println(String.format("\tRead %s", family));
    }
    ```    

## <a name="update-your-connection-string"></a>Aktualizace připojovacího řetězce

Teď se vraťte zpátky na portál Azure Portal, kde najdete informace o připojovacím řetězci, a zkopírujte je do aplikace. To umožňuje aplikaci ke komunikaci s vaší hostované databází.

1. V [portál Azure](http://portal.azure.com/), klikněte na tlačítko **klíče**. 

    Pomocí tlačítek kopírovat na pravé straně obrazovky zkopírujte nejvyšší hodnotu identifikátoru URI.

    ![Zobrazení a zkopírování přístupového klíče v Azure stránky portálu, klíče](./media/create-documentdb-java/keys.png)

2. Otevřete `Program.java` soubor ze složky C:\git-samples\azure-cosmos-db-documentdb-java-getting-started\src\GetStarted. 

3. Vložit hodnotu URI z portálu přes `https://FILLME.documents.azure.com` na řádku 45.

4. Přejděte zpět na portál a zkopírujte hodnotu PRIMÁRNÍHO klíče, jak je znázorněno na snímku obrazovky. Vložit hodnotu primární klíč z portálu přes `FILLME` na řádku 46.

    Metoda getStartedDemo by teď měl vypadat podobně jako tento: 
    
    ```java
    private void getStartedDemo() throws DocumentClientException, IOException {
        this.client = new DocumentClient("https://youraccountname.documents.azure.com:443/",
                "your-primary-key...RJhQrqQ5QQ==", 
                new ConnectionPolicy(),
                ConsistencyLevel.Session);
    ```

5. Uložte soubor Program.java.

## <a name="run-the-app"></a>Spuštění aplikace

1. V okně terminálu Git přejděte příkazem `cd` do složky azure-cosmos-db-documentdb-java-getting-started.

    ```git
    cd "C:\git-samples\azure-cosmos-db-documentdb-java-getting-started"
    ```

2. V okně terminálu Git zadejte `mvn package`, aby se nainstalovaly požadované balíčky Java.

3. V okně terminálu git spustit `mvn exec:java -D exec.mainClass=GetStarted.Program` -li spustit aplikaci Java.

    Okno terminálu zobrazí oznámení, že FamilyDB databáze byla vytvořena. Stisknutím klávesy k vytvoření této kolekce, pak přejděte do Průzkumníku dat a uvidíte, že teď obsahuje FamilyDB databáze.
    
    Pokračujte stisknutím kláves vytvářet dokumenty a potom spustit dotaz.
    
    Na konci tohoto programu se odstraní všechny prostředky v této aplikaci z vašeho účtu tak, aby vám zbytečně nenabíhaly žádné poplatky. 

    ![Výstup konzoly](./media/create-documentdb-java/console-output.png)


## <a name="review-slas-in-the-azure-portal"></a>Ověření smluv SLA na webu Azure Portal

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a>Vyčištění prostředků

[!INCLUDE [cosmosdb-delete-resource-group](../../includes/cosmos-db-delete-resource-group.md)]

## <a name="next-steps"></a>Další kroky

V tomto rychlém startu jste se seznámili s postupem vytvoření účtu Azure Cosmos DB, databáze dokumentů a kolekce pomocí Průzkumníku dat a spuštění aplikace, která to samé udělá programově. Nyní můžete importovat další data do Azure Cosmos DB kolekce. 

> [!div class="nextstepaction"]
> [Importování dat do služby Azure Cosmos DB](import-data.md)


