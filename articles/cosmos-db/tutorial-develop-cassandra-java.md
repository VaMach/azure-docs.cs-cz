---
title: "Azure Cosmos DB: Vývoj pomocí rozhraní API v jazyce Java Cassandra | Microsoft Docs"
description: "Naučte se vyvíjet s rozhraním API Azure Cosmos DB Cassandra používá Java"
services: cosmos-db
documentationcenter: 
author: mimig1
manager: jhubbard
editor: 
tags: 
ms.assetid: 6732d883-835c-481f-98e1-287893530948
ms.service: cosmos-db
ms.devlang: dotnet
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: 
ms.date: 11/15/2017
ms.author: mimig
ms.custom: mvc
ms.openlocfilehash: 53987e5863d9fc11b4fa377295d198293819269c
ms.sourcegitcommit: 0e4491b7fdd9ca4408d5f2d41be42a09164db775
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/14/2017
---
# <a name="azure-cosmosdb-develop-with-the-cassandra-api-in-java"></a>Azure CosmosDB: Vývoj pomocí Cassandra rozhraní API v jazyce Java

Azure Cosmos DB je globálně distribuované databáze více modelu služby společnosti Microsoft. Můžete snadno vytvořit a dotazovat databáze dotazů, klíčů/hodnot a grafů, které tak můžou využívat výhody použitelnosti v celosvětovém měřítku a možností horizontálního škálování v jádru databáze Azure Cosmos. 

Tento kurz ukazuje, jak vytvořit účet Azure Cosmos DB pomocí portálu Azure a pak vytvořte Cassandra Table(sql-api-partition-data.md#partition-keys) pomocí [Cassandra API](cassandra-introduction.md). Definováním primární klíč při vytvoření tabulky, vaše aplikace připravena k pak moci bez obtíží škálujte podle rozšiřujícího se vaše data. 

Tento kurz obsahuje následující úlohy pomocí rozhraní API Cassandra:

> [!div class="checklist"]
> * Vytvoření účtu služby Azure Cosmos DB
> * Vytvoření keyspace a tabulky s primárním klíčem
> * Vložení dat
> * Dotazování dat
> * Zkontrolujte SLA

## <a name="prerequisites"></a>Požadavky

Přístup k rozhraní API služby Azure Cosmos DB Cassandra programu preview. Pokud jste ještě nenainstalovali pro přístup k ještě [nyní](https://aka.ms/cosmosdb-cassandra-signup).

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]Alternativně můžete [zkuste Azure Cosmos DB zdarma](https://azure.microsoft.com/try/cosmosdb/) bez předplatného Azure, bez poplatků a závazky.

Navíc platí: 

* [Java Development Kit (JDK) 1.7+](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)
    * Na Ubuntu nainstalujte sadu JDK spuštěním příkazu `apt-get install default-jdk`.
    * Nezapomeňte nastavit proměnnou prostředí JAVA_HOME tak, aby odkazovala na složku, ve které je sada JDK nainstalovaná.
* [Stáhněte](http://maven.apache.org/download.cgi) a [nainstalujte](http://maven.apache.org/install.html) binární archiv [Maven](http://maven.apache.org/).
    * Na Ubuntu můžete Maven nainstalovat spuštěním příkazu `apt-get install maven`.
* [Git](https://www.git-scm.com/)
    * Na Ubuntu můžete Git nainstalovat spuštěním příkazu `sudo apt-get install git`.

## <a name="create-a-database-account"></a>Vytvoření účtu databáze

Než bude možné vytvořit databázi dokumentů, musíte vytvořit účet Cassandra s Azure Cosmos DB.

[!INCLUDE [cosmos-db-create-dbaccount-cassandra](../../includes/cosmos-db-create-dbaccount-cassandra.md)]

## <a name="clone-the-sample-application"></a>Klonování ukázkové aplikace

Teď přejděme k práci s kódem. Pojďme klonovat Cassandra aplikace z Githubu, nastavení připojovacího řetězce a potom ho spusťte. Přesvědčíte se, jak snadno se pracuje s daty prostřednictvím kódu programu. 

1. Otevřete okno terminálu git, jako je například git bash a použít `cd` příkaz Přejít do složky pro instalaci ukázkové aplikace. 

    ```bash
    cd "C:\git-samples"
    ```

2. Ukázkové úložiště naklonujete spuštěním následujícího příkazu. Tento příkaz vytvoří kopii ukázková aplikace ve vašem počítači.

    ```bash
    git clone https://github.com/Azure-Samples/azure-cosmos-db-cassandra-java-getting-started.git
    ```

## <a name="review-the-code"></a>Kontrola kódu

Tento krok je volitelný. Pokud vás zajímá učení vytváření databázových prostředků v kódu, můžete zkontrolovat následující fragmenty kódu. Jinak, můžete přeskočit na [aktualizovat připojovací řetězec](#update-your-connection-string). Tyto fragmenty kódu jsou převzaty z src/main/java/com/azure/cosmosdb/cassandra/util/CassandraUtils.java.  

* Jsou nastaveny Cassandra hostitelů, port, uživatelské jméno, heslo a možností protokolu SSL. Informace o připojovacím řetězci pochází z stránce připojovací řetězec na portálu Azure.

   ```java
   cluster = Cluster.builder().addContactPoint(cassandraHost).withPort(cassandraPort).withCredentials(cassandraUsername, cassandraPassword).withSSL(sslOptions).build();
   ```

* `cluster` Připojí k rozhraní API služby Azure DB Cosmos Cassandra a vrátí relaci pro přístup.

    ```java
    return cluster.connect();
    ```

Následující fragmenty kódu jsou z src/main/java/com/azure/cosmosdb/cassandra/repository/UserRepository.java souboru.

* Vytvořte nový keyspace.

    ```java
    public void createKeyspace() {
        final String query = "CREATE KEYSPACE IF NOT EXISTS uprofile WITH replication = {'class': 'SimpleStrategy', 'replication_factor': '3' } ";
        session.execute(query);
        LOGGER.info("Created keyspace 'uprofile'");
    }
    ```

* Vytvořte novou tabulku.

   ```java
   public void createTable() {
        final String query = "CREATE TABLE IF NOT EXISTS uprofile.user (user_id int PRIMARY KEY, user_name text, user_bcity text)";
        session.execute(query);
        LOGGER.info("Created table 'user'");
   }
   ```

* Vložte entity uživatele pomocí objektu připravený příkaz.

    ```java
    public PreparedStatement prepareInsertStatement() {
        final String insertStatement = "INSERT INTO  uprofile.user (user_id, user_name , user_bcity) VALUES (?,?,?)";
        return session.prepare(insertStatement);
    }

    public void insertUser(PreparedStatement statement, int id, String name, String city) {
        BoundStatement boundStatement = new BoundStatement(statement);
        session.execute(boundStatement.bind(id, name, city));
    }
    ```

* Dotaz pro získání všech informací o uživateli.

    ```java
   public void selectAllUsers() {
        final String query = "SELECT * FROM uprofile.user";
        List<Row> rows = session.execute(query).all();

        for (Row row : rows) {
            LOGGER.info("Obtained row: {} | {} | {} ", row.getInt("user_id"), row.getString("user_name"), row.getString("user_bcity"));
        }
    }
    ```

* Dotaz se získat informace o jenom jednoho konkrétního uživatele.

    ```java
    public void selectUser(int id) {
        final String query = "SELECT * FROM uprofile.user where user_id = 3";
        Row row = session.execute(query).one();

        LOGGER.info("Obtained row: {} | {} | {} ", row.getInt("user_id"), row.getString("user_name"), row.getString("user_bcity"));
    }
    ```

## <a name="update-your-connection-string"></a>Aktualizace připojovacího řetězce

Teď se vraťte zpátky na portál Azure Portal, kde najdete informace o připojovacím řetězci, a zkopírujte je do aplikace. To umožňuje aplikaci ke komunikaci s vaší hostované databází.

1. V [portál Azure](http://portal.azure.com/), klikněte na tlačítko **připojovací řetězec**. 

    ![Zobrazení a zkopírování uživatelského jména z Azure stránky portálu, připojovací řetězec](./media/tutorial-develop-cassandra-java/keys.png)

2. Použití ![Tlačítko Kopírovat](./media/tutorial-develop-cassandra-java/copy.png) tlačítko na pravé straně obrazovky zkopírujte hodnotu bod kontaktu.

3. Otevřete `config.properties` soubor ze složky C:\git-samples\azure-cosmosdb-cassandra-java-getting-started\java-examples\src\main\resources. 

3. Vložit hodnotu bodu obraťte se na portálu přes `<Cassandra endpoint host>` na řádek 2.

    Řádek 2 config.properties by teď měl vypadat podobně jako 

    `cassandra_host=cosmos-db-quickstarts.documents.azure.com`

3. Přejděte zpět na portál a zkopírujte hodnotu uživatelského jména. Za hodnotu uživatelského jména z portálu přes `<cassandra endpoint username>` na řádku 4.

    Řádek 4 config.properties by teď měl vypadat podobně jako 

    `cassandra_username=cosmos-db-quickstart`

4. Přejděte zpět na portál a zkopírujte hodnotu heslo. Vložte hodnotu HESLA z portálu přes `<cassandra endpoint password>` na řádku 5.

    Řádku 5 config.properties by teď měl vypadat podobně jako 

    `cassandra_password=2Ggkr662ifxz2Mg...==`

5. Na řádku 6, pokud chcete používat konkrétní certifikát SSL, potom můžete nahradit `<SSL key store file location>` s umístěním certifikátu protokolu SSL. Pokud není zadána hodnota, použije se JDK certifikát nainstalovaný na < JAVA_HOME >/prostředí jre/lib/zabezpečení nebo cacerts. 

6. Pokud jste změnili řádek 6 používat konkrétní certifikát SSL, aktualizujte řádek 7 používat heslo pro tento certifikát. 

7. Uložte soubor config.properties.

## <a name="run-the-app"></a>Spuštění aplikace

1. V okně terminálu git `cd` azure-cosmosdb-cassandra-java-getting-started\java-examples složky.

    ```git
    cd "C:\git-samples\azure-cosmosdb-cassandra-java-getting-started\java-examples"
    ```

2. V okně terminálu git pomocí následujícího příkazu vygenerujte soubor cosmosdb-cassandra-examples.jar.

    ```git
    mvn clean install
    ```

3. V okně terminálu git spusťte následující příkaz a spusťte aplikaci Java.

    ```git
    java -cp target/cosmosdb-cassandra-examples.jar com.azure.cosmosdb.cassandra.examples.UserProfile
    ```

    Okno terminálu zobrazí oznámení, že jsou vytvořeny keyspace a tabulka. Potom vybere a vrátí všechny uživatele v tabulce a zobrazí výstup a potom vybere řádek podle id a zobrazí hodnotu.  
    
    Teď se můžete vrátit do Průzkumníku dat a zobrazit dotaz nebo provést úpravy a pracovat s těmito novými daty. 

## <a name="review-slas-in-the-azure-portal"></a>Ověření podmínek SLA na portálu Azure Portal

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a>Vyčištění prostředků

[!INCLUDE [cosmosdb-delete-resource-group](../../includes/cosmos-db-delete-resource-group.md)]

## <a name="next-steps"></a>Další kroky

V tento rychlý start jste zjistili, jak provést následující akce:

> [!div class="checklist"]
> * Vytvoření účtu služby Azure Cosmos DB
> * Vytvoření keyspace a tabulky s primárním klíčem
> * Vložení dat
> * Dotazování dat
> * Najdete SLA

Nyní můžete importovat další data do Azure Cosmos DB kolekce. 

> [!div class="nextstepaction"]
> [Importovat Cassandra data do Azure Cosmos DB](cassandra-import-data.md)
