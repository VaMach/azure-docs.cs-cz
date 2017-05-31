Ještě jednou se stručně podívejme na to, co se v aplikaci děje. Otevřete soubor DocumentDBRepository.cs a zjistíte, že tyto řádky kódu vytvářejí prostředky DocumentDB. 

* Inicializuje se DocumentClient.

    ```csharp
    client = new DocumentClient(new Uri(ConfigurationManager.AppSettings["endpoint"]), ConfigurationManager.AppSettings["authKey"]);`
    ```

* Vytvoří se nová databáze.

    ```csharp
    await client.CreateDatabaseAsync(new Database { Id = DatabaseId });
    ```

* Vytvoří se nová kolekce.

    ```csharp
    await client.CreateDocumentCollectionAsync(
        UriFactory.CreateDatabaseUri(DatabaseId),
        new DocumentCollection { Id = CollectionId },
        new RequestOptions { OfferThroughput = 1000 });
    ```
