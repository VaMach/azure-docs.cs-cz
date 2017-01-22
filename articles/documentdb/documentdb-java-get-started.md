---
title: 'Kurz k NoSQL: Sada Azure DocumentDB Java SDK | Dokumentace Microsoftu'
description: "Kurz k NoSQL, v rámci kterého se vytváří online databáze a konzolová aplikace jazyka Java pomocí sady DocumentDB Java SDK. Azure DocumentDB je databáze NoSQL pro JSON."
keywords: "kurz nosql, online databáze, konzolová aplikace jazyka java"
services: documentdb
documentationcenter: Java
author: arramac
manager: jhubbard
editor: monicar
ms.assetid: 75a9efa1-7edd-4fed-9882-c0177274cbb2
ms.service: documentdb
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: java
ms.topic: hero-article
ms.date: 01/05/2017
ms.author: arramac
translationtype: Human Translation
ms.sourcegitcommit: ddd676df429c20d1c07cfe64abc9ab69ef11bd8c
ms.openlocfilehash: 845858c3df6456293a2552f55ffb35254024931b


---
# <a name="nosql-tutorial-build-a-documentdb-java-console-application"></a>Kurz k NoSQL: Vytvoření konzolové aplikace DocumentDB v jazyce Java
> [!div class="op_single_selector"]
> * [.NET](documentdb-get-started.md)
> * [.NET Core](documentdb-dotnetcore-get-started.md)
> * [Java](documentdb-java-get-started.md)
> * [Node.js](documentdb-nodejs-get-started.md)
> * [C++](documentdb-cpp-get-started.md)
>  
> 

Vítejte v kurzu k NoSQL pro sadu Azure DocumentDB Java SDK! Až projdete tímto kurzem, budete mít konzolovou aplikaci, která vytváří prostředky DocumentDB a dotazuje se na ně.

Kurz zahrnuje:

* Vytvoření a připojení k účtu DocumentDB
* Konfigurace řešení v nástroji Visual Studio
* Vytvoření online databáze
* Vytvoření kolekce
* Vytvoření dokumentů JSON
* Dotazování na kolekci
* Vytvoření dokumentů JSON
* Dotazování na kolekci
* Nahrazení dokumentu
* Odstranění dokumentu
* Odstranění databáze

Můžeme začít!

## <a name="prerequisites"></a>Požadavky
Ujistěte se, že máte následující:

* Aktivní účet Azure. Pokud žádný nemáte, můžete si zaregistrovat [bezplatný účet](https://azure.microsoft.com/free/). Alternativně můžete pro tento kurz použít [emulátor Azure DocumentDB](documentdb-nosql-local-emulator.md).
* [Git](https://git-scm.com/downloads)
* [Java Development Kit (JDK) 7+](http://www.oracle.com/technetwork/java/javase/downloads/index.html)
* [Maven](http://maven.apache.org/download.cgi).

## <a name="step-1-create-a-documentdb-account"></a>Krok 1: Vytvoření účtu DocumentDB
Vytvořme účet DocumentDB. Pokud už máte účet, který chcete použít, můžete přeskočit k části [Klonování projektu z GitHubu](#GitClone). Pokud používáte emulátor DocumentDB, nastavte emulátor pomocí postupu v tématu [Emulátor Azure DocumentDB](documentdb-nosql-local-emulator.md) a přeskočte k části [Klonování projektu z GitHubu](#GitClone).

[!INCLUDE [documentdb-create-dbaccount](../../includes/documentdb-create-dbaccount.md)]

## <a name="a-idgitcloneastep-2-clone-the-github-project"></a><a id="GitClone"></a>Krok 2: Klonování projektu z GitHubu
Můžete začít naklonováním úložiště GitHub pro projekt [Začínáme s DocumentDB a jazykem Java](https://github.com/Azure-Samples/documentdb-java-getting-started). Například spusťte z místního adresáře následující příkaz, který načte ukázkový projekt pro místní použití.

    git clone git@github.com:Azure-Samples/documentdb-java-getting-started.git

    cd documentdb-java-getting-started

Adresář obsahuje soubor `pom.xml` pro projekt a složku `src` obsahující zdrojový kód Java, včetně souboru `Program.java`, který ukazuje, jak s Azure DocumentDB provádět jednoduché operace, jako je vytváření dokumentů a dotazování dat v kolekci. Soubor `pom.xml` obsahuje závislost na sadu [DocumentDB Java SDK v nástroji Maven](https://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb).

    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-documentdb</artifactId>
        <version>LATEST</version>
    </dependency>

## <a name="a-idconnectastep-3-connect-to-a-documentdb-account"></a><a id="Connect"></a>Krok 3: Připojení k účtu DocumentDB
Dále přejděte zpět na [Azure Portal](https://portal.azure.com) a získejte koncový bod a primární hlavní klíč. Koncový bod a primární klíč pro DocumentDB jsou potřeba k tomu, aby aplikace věděla, kam se připojit, a aby DocumentDB důvěřovala připojení aplikace.

Na webu Azure Portal přejděte na účet DocumentDB a klikněte na **Klíče**. Zkopírujte identifikátor URI z portálu a vložte ho do `<your endpoint URI>` v souboru Program.java. Poté zkopírujte PRIMÁRNÍ KLÍČ z portálu a vložte ho do `<your key>`.

    this.client = new DocumentClient(
        "<your endpoint URI>",
        "<your key>"
        , new ConnectionPolicy(),
        ConsistencyLevel.Session);

![Snímek obrazovky webu Azure Portal, který se v kurzu NoSQL používá k vytvoření konzolové aplikace v jazyce Java. Zobrazuje účet DocumentDB se zvýrazněným aktivním centrem, zvýrazněným tlačítkem Klíče v okně účtu DocumentDB a zvýrazněnými hodnotami URI, PRIMÁRNÍ KLÍČ a SEKUNDÁRNÍ KLÍČ v okně Klíče.][keys]

## <a name="step-4-create-a-database"></a>Krok 4: Vytvoření databáze
[Databázi](documentdb-resources.md#databases) DocumentDB je možné vytvořit pomocí metody [createDatabase](http://azure.github.io/azure-documentdb-java/com/microsoft/azure/documentdb/DocumentClient.html#createDatabase-com.microsoft.azure.documentdb.Database-com.microsoft.azure.documentdb.RequestOptions-) třídy **DocumentClient**. Databáze je logický kontejner úložiště dokumentů JSON rozděleného mezi kolekcemi.

    Database database = new Database();
    database.setId("familydb");
    this.client.createDatabase(database, null);

## <a name="a-idcreatecollastep-5-create-a-collection"></a><a id="CreateColl"></a>Krok 5: Vytvoření kolekce
> [!WARNING]
> **createCollection** vytvoří novou kolekci s vyhrazenou propustností, za kterou se hradí poplatky. Další podrobnosti najdete na [stránce s cenami](https://azure.microsoft.com/pricing/details/documentdb/).
> 
> 

[Kolekci](documentdb-resources.md#collections) je možné vytvořit pomocí metody [createCollection](http://azure.github.io/azure-documentdb-java/com/microsoft/azure/documentdb/DocumentClient.html#createCollection-java.lang.String-com.microsoft.azure.documentdb.DocumentCollection-com.microsoft.azure.documentdb.RequestOptions-) třídy **DocumentClient**. Kolekce je kontejner dokumentů JSON a přidružené logiky javascriptové aplikace.


    DocumentCollection collectionInfo = new DocumentCollection();
    collectionInfo.setId("familycoll");

    // DocumentDB collections can be reserved with throughput specified in request units/second. 
    // Here we create a collection with 400 RU/s.
    RequestOptions requestOptions = new RequestOptions();
    requestOptions.setOfferThroughput(400);

    this.client.createCollection("/dbs/familydb", collectionInfo, requestOptions);

## <a name="a-idcreatedocastep-6-create-json-documents"></a><a id="CreateDoc"></a>Krok 6: Vytvoření dokumentů JSON
[Dokument](documentdb-resources.md#documents) je možné vytvořit pomocí metody [createDocument](http://azure.github.io/azure-documentdb-java/com/microsoft/azure/documentdb/DocumentClient.html#createDocument-java.lang.String-java.lang.Object-com.microsoft.azure.documentdb.RequestOptions-boolean-) třídy **DocumentClient**. Dokumenty představují uživatelem definovaný (libovolný) obsah JSON. Nyní můžete vložit jeden nebo více dokumentů. Pokud již máte data, která chcete uložit do databáze, můžete použít [nástroj pro migraci dat](documentdb-import-data.md) služby DocumentDB a importovat tato data do databáze.

    // Insert your Java objects as documents 
    Family andersenFamily = new Family();
    andersenFamily.setId("Andersen.1");
    andersenFamily.setLastName("Andersen")

    // More initialization skipped for brevity. You can have nested references
    andersenFamily.setParents(new Parent[] { parent1, parent2 });
    andersenFamily.setDistrict("WA5");
    Address address = new Address();
    address.setCity("Seattle");
    address.setCounty("King");
    address.setState("WA");

    andersenFamily.setAddress(address);
    andersenFamily.setRegistered(true);

    this.client.createDocument("/dbs/familydb/colls/familycoll", family, new RequestOptions(), true);

![Diagram ilustrující hierarchický vztah mezi účtem, online databází, kolekcí a dokumenty používanými v kurzu NoSQL k vytvoření konzolové aplikace v jazyce Java](./media/documentdb-get-started/nosql-tutorial-account-database.png)

## <a name="a-idqueryastep-7-query-documentdb-resources"></a><a id="Query"></a>Krok 7: Dotazování prostředků DocumentDB
DocumentDB podporuje bohaté [dotazy](documentdb-sql-query.md) na dokumenty JSON uložené v každé z kolekcí.  Následující vzorový kód ukazuje dotazování na dokumenty v DocumentDB pomocí syntaxe SQL a metody [queryDocuments](http://azure.github.io/azure-documentdb-java/com/microsoft/azure/documentdb/DocumentClient.html#queryDocuments-java.lang.String-com.microsoft.azure.documentdb.SqlQuerySpec-com.microsoft.azure.documentdb.FeedOptions-).

    FeedResponse<Document> queryResults = this.client.queryDocuments(
        "/dbs/familydb/colls/familycoll",
        "SELECT * FROM Family WHERE Family.lastName = 'Andersen'", 
        null);

    System.out.println("Running SQL query...");
    for (Document family : queryResults.getQueryIterable()) {
        System.out.println(String.format("\tRead %s", family));
    }

## <a name="a-idreplacedocumentastep-8-replace-json-document"></a><a id="ReplaceDocument"></a>Krok 8: Nahrazení dokumentu JSON
DocumentDB podporuje aktualizaci dokumentů JSON pomocí metody [replaceDocument](http://azure.github.io/azure-documentdb-java/com/microsoft/azure/documentdb/DocumentClient.html#replaceDocument-com.microsoft.azure.documentdb.Document-com.microsoft.azure.documentdb.RequestOptions-).

    // Update a property
    andersenFamily.Children[0].Grade = 6;

    this.client.replaceDocument(
        "/dbs/familydb/colls/familycoll/docs/Andersen.1", 
        andersenFamily,
        null);

## <a name="a-iddeletedocumentastep-9-delete-json-document"></a><a id="DeleteDocument"></a>Krok 9: Odstranění dokumentu JSON
Podobně DocumentDB podporuje odstraňování dokumentů JSON pomocí metody [deleteDocument](http://azure.github.io/azure-documentdb-java/com/microsoft/azure/documentdb/DocumentClient.html#deleteDocument-java.lang.String-com.microsoft.azure.documentdb.RequestOptions-).  

    this.client.delete("/dbs/familydb/colls/familycoll/docs/Andersen.1", null);

## <a name="a-iddeletedatabaseastep-10-delete-the-database"></a><a id="DeleteDatabase"></a>Krok 10: Odstranění databáze
Odstraněním vytvořené databáze dojde k odebrání databáze a všech jejích podřízených prostředků (kolekcí, dokumentů atd.).

    this.client.deleteDatabase("/dbs/familydb", null);

## <a name="a-idrunastep-11-run-your-java-console-application-all-together"></a><a id="Run"></a>Krok 11: Spuštění celé konzolové aplikace jazyka Java!
Pokud chcete spustit aplikaci z konzoly, nejprve ji zkompilujte pomocí nástroje Maven:
    
    mvn package

Spuštěním příkazu `mvn package` se z nástroje Maven stáhne nejnovější verze knihovny DocumentDB a vytvoří se soubor `GetStarted-0.0.1-SNAPSHOT.jar`. Potom spusťte aplikaci následujícím příkazem:

    mvn exec:java -D exec.mainClass=GetStarted.Program

Blahopřejeme! Dokončili jste tento kurz NoSQL a máte funkční konzolovou aplikaci jazyka Java!

## <a name="next-steps"></a>Další kroky
* Chcete kurz vývoje webové aplikace v jazyce Java? Viz [Vytvoření webové aplikace pomocí jazyka Java a DocumentDB](documentdb-java-application.md).
* Naučte se [monitorovat účet DocumentDB](documentdb-monitor-accounts.md).
* Spouštějte dotazy proti ukázkovým datovým sadám v [Query Playground](https://www.documentdb.com/sql/demo).
* Přečtěte si více o tomto programovacím modelu v části Vyvíjejte na [stránce dokumentace DocumentDB](https://azure.microsoft.com/documentation/services/documentdb/).

[documentdb-create-account]: documentdb-create-account.md
[keys]: media/documentdb-get-started/nosql-tutorial-keys.png



<!--HONumber=Jan17_HO1-->


