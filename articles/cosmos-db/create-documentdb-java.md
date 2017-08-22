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
ms.custom: quick start connect, mvc
ms.workload: 
ms.tgt_pltfrm: na
ms.devlang: java
ms.topic: hero-article
ms.date: 08/02/2017
ms.author: mimig
ms.translationtype: HT
ms.sourcegitcommit: b6c65c53d96f4adb8719c27ed270e973b5a7ff23
ms.openlocfilehash: df1a25d703a7b8082bdabb4f7d593cb005d416fe
ms.contentlocale: cs-cz
ms.lasthandoff: 08/17/2017

---
# <a name="azure-cosmos-db-create-a-document-database-using-java-and-the-azure-portal"></a>Azure Cosmos DB: Vytvoření databáze dokumentů pomocí Javy a webu Azure Portal

Databáze Azure Cosmos je databázová služba Microsoftu s více modely použitelná v celosvětovém měřítku. Můžete snadno vytvořit a dotazovat databáze dotazů, klíčů/hodnot a grafů, které tak můžou využívat výhody použitelnosti v celosvětovém měřítku a možností horizontálního škálování v jádru databáze Azure Cosmos. 

V tomto rychlém startu se vytvoří databáze dokumentů pomocí nástrojů pro Azure Cosmos DB na webu Azure Portal. V tomto rychlém startu se také dozvíte, jak rychle vytvořit konzolovou aplikaci Java pomocí [rozhraní API Java DocumentDB](documentdb-sdk-java.md). Pokyny v tomto rychlém startu platí pro všechny operační systémy, které podporují Javu. Po dokončení tohoto rychlého startu budete vědět, jak vytvořit a upravit prostředky databáze dokumentů v uživatelském rozhraní nebo programově podle toho, čemu dáváte přednost.

## <a name="prerequisites"></a>Požadavky

* [Java Development Kit (JDK) 1.7+](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)
    * Na Ubuntu nainstalujte sadu JDK spuštěním příkazu `apt-get install default-jdk`.
    * Nezapomeňte nastavit proměnnou prostředí JAVA_HOME tak, aby odkazovala na složku, ve které je sada JDK nainstalovaná.
* [Stáhněte](http://maven.apache.org/download.cgi) a [nainstalujte](http://maven.apache.org/install.html) binární archiv [Maven](http://maven.apache.org/).
    * Na Ubuntu můžete Maven nainstalovat spuštěním příkazu `apt-get install maven`.
* [Git](https://www.git-scm.com/)
    * Na Ubuntu můžete Git nainstalovat spuštěním příkazu `sudo apt-get install git`.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-a-database-account"></a>Vytvoření účtu databáze

Než budete moct vytvořit databázi dokumentů, je potřeba pomocí služby Azure Cosmos DB vytvořit účet databáze SQL (DocumentDB).

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

## <a name="add-a-collection"></a>Přidání kolekce

[!INCLUDE [cosmos-db-create-collection](../../includes/cosmos-db-create-collection.md)]

<a id="add-sample-data"></a>
## <a name="add-sample-data"></a>Přidání ukázkových dat

Teď můžete do nové kolekce přidávat data pomocí Průzkumníka dat.

1. V Průzkumníku dat se nová databáze zobrazí v podokně Kolekce. Rozbalte databázi **Tasks**, rozbalte kolekci **Items**, klikněte na **Dokumenty** a potom klikněte na **Nové dokumenty**. 

   ![Vytváření nových dokumentů v Průzkumníku dat na portálu Azure Portal](./media/create-documentdb-dotnet/azure-cosmosdb-data-explorer-new-document.png)
  
2. Teď do kolekce přidejte dokument s následující strukturou.

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

4.  Vytvořte a uložte ještě jeden dokument, ve kterém vložíte jedinečnou hodnotu pro vlastnost `id` a změníte ostatní vlastnosti podle svých potřeb. Nové dokumenty můžou mít jakoukoli strukturu, protože Azure Cosmos DB neuplatňuje pro data žádné schéma.

     Teď můžete svá data načíst pomocí dotazů v Průzkumníku dat kliknutím na tlačítka **Upravit filtr** a **Použít filtr**. Ve výchozím nastavení používá Průzkumník dat pro načtení všech dokumentů v kolekci příkaz `SELECT * FROM c`. Ten však můžete změnit na jiný [příkaz jazyka SQL](documentdb-sql-query.md), například `SELECT * FROM c ORDER BY c._ts DESC`, který vrátí všechny dokumenty v sestupném pořadí podle časového razítka. 
 
     Průzkumník dat můžete použít také pro vytváření uložených procedur, funkcí UDF a triggerů pro provádění obchodní logiky a také propustnosti škálování na straně serveru. Průzkumník dat zpřístupní všechna integrovaná programová data v rozhraních API, ale zajistí jednoduchý přístup k vašim datům na portálu Azure Portal.

## <a name="clone-the-sample-application"></a>Klonování ukázkové aplikace

Teď přejděme k práci s kódem. Naklonujeme aplikaci s rozhraním API DocumentDB z GitHubu, nastavíme připojovací řetězec a spustíme ji. Uvidíte, jak snadno se pracuje s daty prostřednictvím kódu programu. 

1. Otevřete okno terminálu Git, jako je třeba Git Bash, a pomocí `CD` přejděte do pracovního adresáře.  

2. Ukázkové úložiště naklonujete spuštěním následujícího příkazu. 

    ```bash
    git clone https://github.com/Azure-Samples/azure-cosmos-db-documentdb-java-getting-started.git
    ```

## <a name="review-the-code"></a>Kontrola kódu

Ještě jednou se stručně podívejme na to, co se v aplikaci děje. Otevřete soubor `Program.java` ze složky \src\GetStarted a vyhledejte tyto řádky kódu, které vytvářejí prostředky služby Azure Cosmos DB. 

* Inicializuje se `DocumentClient`.

    ```java
    this.client = new DocumentClient("https://FILLME.documents.azure.com",
            "FILLME", 
            new ConnectionPolicy(),
            ConsistencyLevel.Session);
    ```

* Vytvoří se nová databáze.

    ```java
    Database database = new Database();
    database.setId(databaseName);
    
    this.client.createDatabase(database, null);
    ```

* Vytvoří se nová kolekce.

    ```java
    DocumentCollection collectionInfo = new DocumentCollection();
    collectionInfo.setId(collectionName);

    ...

    this.client.createCollection(databaseLink, collectionInfo, requestOptions);
    ```

* Vytvoří se některé dokumenty.

    ```java
    // Any Java object within your code can be serialized into JSON and written to Azure Cosmos DB
    Family andersenFamily = new Family();
    andersenFamily.setId("Andersen.1");
    andersenFamily.setLastName("Andersen");
    // More properties

    String collectionLink = String.format("/dbs/%s/colls/%s", databaseName, collectionName);
    this.client.createDocument(collectionLink, family, new RequestOptions(), true);
    ```

* Provede se dotaz SQL přes JSON.

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

Teď se vraťte zpátky na portál Azure Portal, kde najdete informace o připojovacím řetězci, a zkopírujte je do aplikace. Tím aplikaci umožníte komunikovat s hostovanou databází.

1. Na webu [Azure Portal](http://portal.azure.com/) klikněte v účtu databáze Azure Cosmos v levém navigačním panelu na možnost **Klíče** a potom klikněte na **Klíče pro čtení i zápis**. V dalším kroku zkopírujete pomocí tlačítek kopírování na pravé straně obrazovky identifikátor URI a PRIMÁRNÍ KLÍČ do souboru `Program.java`.

    ![Zobrazení a zkopírování přístupového klíče na webu Azure Portal v okně Klíče](./media/create-documentdb-dotnet/keys.png)

2. V otevřeném souboru `Program.java` zkopírujte z portálu hodnotu identifikátoru URI (pomocí tlačítka kopírování) a nastavte ji jako hodnotu koncového bodu konstruktoru DocumentClient v souboru `Program.java`. 

    `"https://FILLME.documents.azure.com"`

4. Potom z portálu zkopírujte hodnotu PRIMÁRNÍHO KLÍČE a nahraďte jí parametr FILLME. Tím z ní uděláte druhý parametr v konstruktoru DocumentClient. Teď jste aktualizovali aplikaci a zadali do ní všechny informace potřebné ke komunikaci s Azure Cosmos DB. 
    
## <a name="run-the-app"></a>Spuštění aplikace

1. V okně terminálu Git přejděte příkazem `cd` do složky azure-cosmos-db-documentdb-java-getting-started.

2. V okně terminálu Git zadejte `mvn package`, aby se nainstalovaly požadované balíčky Java.

3. V okně terminálu Git spuštěním příkazu `mvn exec:java -D exec.mainClass=GetStarted.Program` spusťte svoji aplikaci Java.

    V okně terminálu se zobrazí oznámení o vytvoření databáze FamilyDB a výzva ke stisknutí klávesy a pokračování. Stisknutím libovolné klávesy vytvořte databázi, potom přejděte do Průzkumníku dat a uvidíte, že teď obsahuje databázi FamilyDB. Pokračujte a stiskem kláves vytvořte kolekci a dokumenty a potom proveďte dotaz. Po dokončení projektu se prostředky odstraní z vašeho účtu. 

    ![Zobrazení a zkopírování přístupového klíče na webu Azure Portal v okně Klíče](./media/create-documentdb-java/console-output.png)


## <a name="review-slas-in-the-azure-portal"></a>Ověření smluv SLA na webu Azure Portal

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a>Vyčištění prostředků

Pokud nebudete tuto aplikace nadále používat, odstraňte na základě následujícího postupu z portálu Azure Portal všechny prostředky vytvořené podle tohoto rychlého startu:

1. V nabídce vlevo na portálu Azure Portal klikněte na **Skupiny prostředků** a pak klikněte na název vytvořeného prostředku. 
2. Na stránce skupiny prostředků klikněte na **Odstranit**, do textového pole zadejte prostředek, který chcete odstranit, a pak klikněte na **Odstranit**.

## <a name="next-steps"></a>Další kroky

V tomto rychlém startu jste se seznámili s postupem vytvoření účtu Azure Cosmos DB, databáze dokumentů a kolekce pomocí Průzkumníku dat a spuštění aplikace, která to samé udělá programově. Teď můžete do účtu databáze Cosmos importovat další data. 

> [!div class="nextstepaction"]
> [Importování dat do služby Azure Cosmos DB](import-data.md)



