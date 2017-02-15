---
title: "Začínáme s Azure Table Storage pomocí rozhraní .NET | Dokumentace Microsoftu"
description: "Ukládejte si strukturovaná data v cloudu pomocí Azure Table Storage, úložiště dat typu NoSQL."
services: storage
documentationcenter: .net
author: mmacy
manager: timlt
editor: tysonn
ms.assetid: fe46d883-7bed-49dd-980e-5c71df36adb3
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 12/08/2016
ms.author: marsma
translationtype: Human Translation
ms.sourcegitcommit: 931503f56b32ce9d1b11283dff7224d7e2f015ae
ms.openlocfilehash: 98307e924927655c8a7de0e8fc6a7c0c2b57af00


---
# <a name="get-started-with-azure-table-storage-using-net"></a>Začínáme s úložištěm Azure Table pomocí rozhraní .NET
[!INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]

[!INCLUDE [storage-check-out-samples-dotnet](../../includes/storage-check-out-samples-dotnet.md)]

## <a name="overview"></a>Přehled
Azure Table Storage je služba, která ukládá strukturovaná data typu NoSQL v cloudu. Table Storage je úložiště klíčů/atributů s návrhem bez schématu. Vzhledem k tomu, že je Table Storage bez schématu, je snadné data přizpůsobovat měnícím se potřebám vaší aplikace. Přístup k datům je rychlý a nákladově efektivní pro všechny typy aplikací. Využívání úložiště Table Storage obvykle znamená výrazně nižší náklady než tradiční SQL pro podobné objemy dat.

Úložiště Table Storage můžete používat k ukládání flexibilních datových sad, například uživatelských dat pro webové aplikace, adresářů, informací o zařízení a dalších typů metadat, které vaše služba vyžaduje. V tabulce můžete uložit libovolný počet entit a účet úložiště může obsahovat libovolný počet tabulek, až do limitu kapacity účtu úložiště.

### <a name="about-this-tutorial"></a>O tomto kurzu
V tomto kurzu si ukážeme, jak napsat kód .NET pro některé běžné scénáře použití služby Azure Table Storage, včetně vytváření a odstraňování tabulek a vkládání, aktualizace a odstraňování dat tabulky a dotazování se na ně.

**Požadavky:**

* [Microsoft Visual Studio](https://www.visualstudio.com/en-us/visual-studio-homepage-vs.aspx)
* [Klientská knihovna Azure Storage pro .NET](https://www.nuget.org/packages/WindowsAzure.Storage/)
* [Azure Configuration Manager for .NET](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager/)
* [Účet úložiště Azure](storage-create-storage-account.md#create-a-storage-account)

[!INCLUDE [storage-dotnet-client-library-version-include](../../includes/storage-dotnet-client-library-version-include.md)]

### <a name="more-samples"></a>Další ukázky
Další příklady použití Table Storage najdete v článku [Začínáme s Azure Table Storage v rozhraní .NET](https://azure.microsoft.com/documentation/samples/storage-table-dotnet-getting-started/). Můžete si stáhnout a spustit ukázkovou aplikaci nebo si prohlédnout kód na GitHubu.

[!INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

[!INCLUDE [storage-development-environment-include](../../includes/storage-development-environment-include.md)]

### <a name="add-namespace-declarations"></a>Přidání deklarací oboru názvů
Na začátek souboru `program.cs` přidejte následující příkazy **using**:

```csharp
using Microsoft.Azure; // Namespace for CloudConfigurationManager
using Microsoft.WindowsAzure.Storage; // Namespace for CloudStorageAccount
using Microsoft.WindowsAzure.Storage.Table; // Namespace for Table storage types
```

### <a name="parse-the-connection-string"></a>Analýza připojovacího řetězce
[!INCLUDE [storage-cloud-configuration-manager-include](../../includes/storage-cloud-configuration-manager-include.md)]

### <a name="create-the-table-service-client"></a>Vytvoření klienta služby Table
Třída **CloudTableClient** vám umožňuje načíst tabulky a entity, které jsou uložené ve službě Table Storage. Tady je jeden ze způsobů, jak vytvořit klienta služby:

```csharp
// Create the table client.
CloudTableClient tableClient = storageAccount.CreateCloudTableClient();
```

Teď můžete napsat kód, který bude číst data z Table Storage a bude je tam také zapisovat.

## <a name="create-a-table"></a>Vytvoření tabulky
Tento příklad ukazuje, jak vytvořit tabulku, pokud ještě neexistuje:

```csharp
// Retrieve the storage account from the connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create the table client.
CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

// Retrieve a reference to the table.
CloudTable table = tableClient.GetTableReference("people");

// Create the table if it doesn't exist.
table.CreateIfNotExists();
```

## <a name="add-an-entity-to-a-table"></a>Přidání entity do tabulky
Entity se mapují na objekty C\# pomocí vlastní třídy odvozené z **TableEntity**. Když budete chtít do tabulky přidat entitu, vytvořte třídu, která definuje vlastnosti vaší entity. Následující kód definuje třídu entity, která používá jméno zákazníka jako klíč řádku a jeho příjmení jako klíč oddílu. Společně pak klíč oddílu a řádku entity jednoznačně identifikují entitu v tabulce. Na entity se stejným klíčem oddílu je možné se (v porovnání s těmi, které mají různé klíče oddílů) rychleji dotazovat, ale používání různých klíčů oddílů umožňuje větší škálovatelnost paralelních operací. Jakákoli vlastnost, která by měla být uložena ve službě Table, musí být veřejná vlastnost podporovaného typu, která zpřístupňuje jak nastavení, tak načítání hodnot.
Typ entity navíc *musí* zveřejňovat konstruktor bez parametrů.

```csharp
public class CustomerEntity : TableEntity
{
    public CustomerEntity(string lastName, string firstName)
    {
        this.PartitionKey = lastName;
        this.RowKey = firstName;
    }

    public CustomerEntity() { }

    public string Email { get; set; }

    public string PhoneNumber { get; set; }
}
```

Operace s tabulkou zahrnující entity se provádí prostřednictvím objektu **CloudTable**, který jste vytvořili dříve v části Vytvoření tabulky. Operace, která se má provést, je reprezentovaná objektem **TableOperation**.  Následující příklad kódu ukazuje vytvoření objektu **CloudTable** a následně objektu **CustomerEntity**.  V rámci přípravy na operaci je vytvořen objekt **TableOperation** pro vložení entity zákazníka do tabulky.  Nakonec se operace provede voláním metody **CloudTable.Execute**.

```csharp
// Retrieve the storage account from the connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create the table client.
CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

// Create the CloudTable object that represents the "people" table.
CloudTable table = tableClient.GetTableReference("people");

// Create a new customer entity.
CustomerEntity customer1 = new CustomerEntity("Harp", "Walter");
customer1.Email = "Walter@contoso.com";
customer1.PhoneNumber = "425-555-0101";

// Create the TableOperation object that inserts the customer entity.
TableOperation insertOperation = TableOperation.Insert(customer1);

// Execute the insert operation.
table.Execute(insertOperation);
```

## <a name="insert-a-batch-of-entities"></a>Vložení dávky entit
V rámci jedné operace zápisu můžete do tabulky vložit dávku entit. Několik dalších poznámek ohledně dávkových operací:

* V rámci jedné dávkové operace můžete provádět aktualizace, odstranění a vložení.
* Jedna dávková operace může obsahovat až 100 entit.
* Všechny entity v jedné dávkové operaci musí mít stejný klíč oddílu.
* Dotaz je sice také možné provést jako dávkovou operaci, musí to být ale jediná operace v dávce.

<!-- -->
Následující příklad kódu vytvoří dva objekty entity a každý z nich přidá do **TableBatchOperation** pomocí metody **Insert**. Pak se volá metoda **CloudTable.Execute** k provedení operace.

```csharp
// Retrieve the storage account from the connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create the table client.
CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

// Create the CloudTable object that represents the "people" table.
CloudTable table = tableClient.GetTableReference("people");

// Create the batch operation.
TableBatchOperation batchOperation = new TableBatchOperation();

// Create a customer entity and add it to the table.
CustomerEntity customer1 = new CustomerEntity("Smith", "Jeff");
customer1.Email = "Jeff@contoso.com";
customer1.PhoneNumber = "425-555-0104";

// Create another customer entity and add it to the table.
CustomerEntity customer2 = new CustomerEntity("Smith", "Ben");
customer2.Email = "Ben@contoso.com";
customer2.PhoneNumber = "425-555-0102";

// Add both customer entities to the batch insert operation.
batchOperation.Insert(customer1);
batchOperation.Insert(customer2);

// Execute the batch operation.
table.ExecuteBatch(batchOperation);
```

## <a name="retrieve-all-entities-in-a-partition"></a>Načtení všech entit v oddílu
Pokud chcete zadat dotaz na tabulku pro všechny entity v oddílu, použijte objekt **TableQuery**.
Následující příklad kódu určuje filtr pro entity, kde Smith je klíč oddílu. Tento příklad zobrazí pole každé entity z výsledků dotazu z konzoly.

```csharp
// Retrieve the storage account from the connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create the table client.
CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

// Create the CloudTable object that represents the "people" table.
CloudTable table = tableClient.GetTableReference("people");

// Construct the query operation for all customer entities where PartitionKey="Smith".
TableQuery<CustomerEntity> query = new TableQuery<CustomerEntity>().Where(TableQuery.GenerateFilterCondition("PartitionKey", QueryComparisons.Equal, "Smith"));

// Print the fields for each customer.
foreach (CustomerEntity entity in table.ExecuteQuery(query))
{
    Console.WriteLine("{0}, {1}\t{2}\t{3}", entity.PartitionKey, entity.RowKey,
        entity.Email, entity.PhoneNumber);
}
```

## <a name="retrieve-a-range-of-entities-in-a-partition"></a>Načtení rozsahu entit v oddílu
Pokud nechcete, aby se zadával dotaz na všechny entity v oddílu, můžete zadat rozsah nakombinováním filtru klíče oddílu s filtrem klíče řádku. Následující příklad kódu používá dva filtry k získání všech entit v oddílu Smith, kde klíč řádku (jméno) začíná písmenem abecedy před písmenem E, a potom zobrazí výsledky dotazu.

```csharp
// Retrieve the storage account from the connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create the table client.
CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

// Create the CloudTable object that represents the "people" table.
CloudTable table = tableClient.GetTableReference("people");

// Create the table query.
TableQuery<CustomerEntity> rangeQuery = new TableQuery<CustomerEntity>().Where(
    TableQuery.CombineFilters(
        TableQuery.GenerateFilterCondition("PartitionKey", QueryComparisons.Equal, "Smith"),
        TableOperators.And,
        TableQuery.GenerateFilterCondition("RowKey", QueryComparisons.LessThan, "E")));

// Loop through the results, displaying information about the entity.
foreach (CustomerEntity entity in table.ExecuteQuery(rangeQuery))
{
    Console.WriteLine("{0}, {1}\t{2}\t{3}", entity.PartitionKey, entity.RowKey,
        entity.Email, entity.PhoneNumber);
}
```

## <a name="retrieve-a-single-entity"></a>Načtení jedné entity
Můžete napsat dotaz pro načtení jedné konkrétní entity. Následující kód používá objekt **TableOperation** k určení zákazníka Ben Smith.
Tato metoda vrátí místo kolekce pouze jednu entitu a vrácenou hodnotou při volání metody **TableResult.Result** je objekt **CustomerEntity**.
Určení jak klíčů oddílu, tak klíčů řádků v dotazu představuje nejrychlejší způsob, jak načíst jednu entitu ze služby Table.

```csharp
// Retrieve the storage account from the connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create the table client.
CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

// Create the CloudTable object that represents the "people" table.
CloudTable table = tableClient.GetTableReference("people");

// Create a retrieve operation that takes a customer entity.
TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

// Execute the retrieve operation.
TableResult retrievedResult = table.Execute(retrieveOperation);

// Print the phone number of the result.
if (retrievedResult.Result != null)
{
    Console.WriteLine(((CustomerEntity)retrievedResult.Result).PhoneNumber);
}
else
{
    Console.WriteLine("The phone number could not be retrieved.");
}
```

## <a name="replace-an-entity"></a>Nahrazení entity
Pokud chcete entitu aktualizovat, načtěte ji ze služby Table, upravte objekt entity a potom uložte změny zpět do služby Table. Následující kód změní telefonní číslo stávajícího zákazníka. Namísto volání metody **Insert** tento kód používá metodu **Replace**. To způsobí, že entita se na serveru plně nahradí, pokud se entita na serveru od načtení nezměnila, protože v takovém případě se operace nezdaří.  Toto selhání zabrání vaší aplikaci v nechtěném přepsání změny provedené mezi načtením a aktualizací provedenou jinou součástí vaší aplikace.  Toto selhání by se mělo správně zpracovat tak, že entitu znovu načtete, provedete požadované změny (pokud je stále ještě chcete provést) a pak provedete další operaci nahrazení (**Replace**).  V další části si ukážeme, jak toto chování potlačit.

```csharp
// Retrieve the storage account from the connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create the table client.
CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

// Create the CloudTable object that represents the "people" table.
CloudTable table = tableClient.GetTableReference("people");

// Create a retrieve operation that takes a customer entity.
TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

// Execute the operation.
TableResult retrievedResult = table.Execute(retrieveOperation);

// Assign the result to a CustomerEntity object.
CustomerEntity updateEntity = (CustomerEntity)retrievedResult.Result;

if (updateEntity != null)
{
    // Change the phone number.
    updateEntity.PhoneNumber = "425-555-0105";

    // Create the Replace TableOperation.
    TableOperation updateOperation = TableOperation.Replace(updateEntity);

    // Execute the operation.
    table.Execute(updateOperation);

    Console.WriteLine("Entity updated.");
}
else
{
    Console.WriteLine("Entity could not be retrieved.");
}
```

## <a name="insert-or-replace-an-entity"></a>Vložení nebo nahrazení entity
Operace **Replace** se nezdaří, pokud byla entita od načtení ze serveru změněna.  Kromě toho musíte entitu nejdřív načíst ze serveru, aby operace **Replace** proběhla úspěšně.
V některých případech ale nevíte, jestli entita existuje na serveru a jestli jsou hodnoty, které jsou v ní aktuálně uložené, relevantní. Vaše aktualizace by je měla všechny přepsat.  K tomu použijete operaci **InsertOrReplace**.  Tato operace vloží entitu, pokud neexistuje, nebo ji nahradí, pokud existuje, a to bez ohledu na to, kdy byla provedena poslední aktualizace.  V následujícím příkladu kódu je entita zákazník Ben Smith načtena, ale pak je uložena zpět na server prostřednictvím operace **InsertOrReplace**.  Jakékoli aktualizace provedené v entitě mezi operacemi načtení a aktualizace budou přepsány.

```csharp
// Retrieve the storage account from the connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create the table client.
CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

// Create the CloudTable object that represents the "people" table.
CloudTable table = tableClient.GetTableReference("people");

// Create a retrieve operation that takes a customer entity.
TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

// Execute the operation.
TableResult retrievedResult = table.Execute(retrieveOperation);

// Assign the result to a CustomerEntity object.
CustomerEntity updateEntity = (CustomerEntity)retrievedResult.Result;

if (updateEntity != null)
{
    // Change the phone number.
    updateEntity.PhoneNumber = "425-555-1234";

    // Create the InsertOrReplace TableOperation.
    TableOperation insertOrReplaceOperation = TableOperation.InsertOrReplace(updateEntity);

    // Execute the operation.
    table.Execute(insertOrReplaceOperation);

    Console.WriteLine("Entity was updated.");
}
else
{
    Console.WriteLine("Entity could not be retrieved.");
}
```

## <a name="query-a-subset-of-entity-properties"></a>Dotaz na podmnožinu vlastností entity
Dotaz na tabulku může místo všech vlastností entity načíst jenom několik z nich. Tato technika, které se říká projekce, snižuje šířku pásma a může zlepšit výkon dotazů, zejména u velkých entit. Dotaz v následujícím kódu vrátí pouze e-mailové adresy entit v tabulce. To se provádí pomocí dotazu **DynamicTableEntity** a také **EntityResolver**. Další informace o projekcích najdete v [Blogovém příspěvku představení funkcí Upsert a projekce dotazu][Introducing Upsert and Query Projection blog post]. Poznámka: Projekci nepodporuje emulátor místního úložiště, takže tento kód bude možné spustit pouze v případě, že používáte účet služby Table.

```csharp
// Retrieve the storage account from the connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create the table client.
CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

// Create the CloudTable that represents the "people" table.
CloudTable table = tableClient.GetTableReference("people");

// Define the query, and select only the Email property.
TableQuery<DynamicTableEntity> projectionQuery = new TableQuery<DynamicTableEntity>().Select(new string[] { "Email" });

// Define an entity resolver to work with the entity after retrieval.
EntityResolver<string> resolver = (pk, rk, ts, props, etag) => props.ContainsKey("Email") ? props["Email"].StringValue : null;

foreach (string projectedEmail in table.ExecuteQuery(projectionQuery, resolver, null, null))
{
    Console.WriteLine(projectedEmail);
}
```

## <a name="delete-an-entity"></a>Odstranění entity
Entitu můžete po jejím načtení snadno odstranit, a to pomocí stejného vzoru zobrazovaného pro aktualizaci entity.  Následující kód načte a odstraní entitu zákazníka.

```csharp
// Retrieve the storage account from the connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create the table client.
CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

// Create the CloudTable that represents the "people" table.
CloudTable table = tableClient.GetTableReference("people");

// Create a retrieve operation that expects a customer entity.
TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

// Execute the operation.
TableResult retrievedResult = table.Execute(retrieveOperation);

// Assign the result to a CustomerEntity.
CustomerEntity deleteEntity = (CustomerEntity)retrievedResult.Result;

// Create the Delete TableOperation.
if (deleteEntity != null)
{
    TableOperation deleteOperation = TableOperation.Delete(deleteEntity);

    // Execute the operation.
    table.Execute(deleteOperation);

    Console.WriteLine("Entity deleted.");
}
else
{
    Console.WriteLine("Could not retrieve the entity.");
}
```

## <a name="delete-a-table"></a>Odstranění tabulky
Následující příklad kódu nakonec odstraní tabulku z účtu úložiště. Tabulku, která byla odstraněna, nebude možné po odstranění nějakou dobu znovu vytvořit.

```csharp
// Retrieve the storage account from the connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create the table client.
CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

// Create the CloudTable that represents the "people" table.
CloudTable table = tableClient.GetTableReference("people");

// Delete the table it if exists.
table.DeleteIfExists();
```

## <a name="retrieve-entities-in-pages-asynchronously"></a>Asynchronní načítání entit na stránkách
Pokud načítáte velký počet entit a chcete entity zpracovávat/zobrazovat tak, jak jsou načítány, a nečekat, až se všechny vrátí, můžete entity načíst pomocí segmentovaného dotazu. Tento příklad ukazuje, jak vracet výsledky na stránkách pomocí vzoru Async-Await, aby čekání na vrácení velké sady výsledků neblokovalo provádění. Další podrobnosti o použití vzoru Async-Await v rozhraní .NET najdete v tématu [Asynchronní programování s Async a Await (C# a Visual Basic)](https://msdn.microsoft.com/library/hh191443.aspx).

```csharp
// Initialize a default TableQuery to retrieve all the entities in the table.
TableQuery<CustomerEntity> tableQuery = new TableQuery<CustomerEntity>();

// Initialize the continuation token to null to start from the beginning of the table.
TableContinuationToken continuationToken = null;

do
{
    // Retrieve a segment (up to 1,000 entities).
    TableQuerySegment<CustomerEntity> tableQueryResult =
        await table.ExecuteQuerySegmentedAsync(tableQuery, continuationToken);

    // Assign the new continuation token to tell the service where to
    // continue on the next iteration (or null if it has reached the end).
    continuationToken = tableQueryResult.ContinuationToken;

    // Print the number of rows retrieved.
    Console.WriteLine("Rows retrieved {0}", tableQueryResult.Results.Count);

// Loop until a null continuation token is received, indicating the end of the table.
} while(continuationToken != null);
```

## <a name="next-steps"></a>Další kroky
Teď, když jste se naučili základy používání služby Table Storage, podívejte se na následujících odkazech na další informace o složitějších úlohách úložiště:

* Další příklady Table Storage najdete v článku [Začínáme s Azure Table Storage v rozhraní .NET](https://azure.microsoft.com/documentation/samples/storage-table-dotnet-getting-started/).
* Projděte si referenční dokumentaci ke službě Table, kde najdete úplné podrobnosti o dostupných rozhraních API:
  * [Klientská knihovna Storage pro .NET – referenční informace](http://go.microsoft.com/fwlink/?LinkID=390731&clcid=0x409)
  * [REST API – referenční informace](http://msdn.microsoft.com/library/azure/dd179355)
* Zjistěte, jak můžete zjednodušit kód, který vytváříte, aby fungoval s Azure Storage, pomocí sady [Azure WebJobs SDK](../app-service-web/websites-dotnet-webjobs-sdk-get-started.md).
* Projděte si další průvodce funkcemi, kde najdete další informace o dalších možnostech pro ukládání dat v Azure.
  * [Začínáme s Azure Blob Storage pomocí rozhraní .NET](storage-dotnet-how-to-use-blobs.md) pro ukládání nestrukturovaných dat
  * [Připojení k SQL Database s použitím rozhraní .NET (C#)](../sql-database/sql-database-develop-dotnet-simple.md) pro uložení relačních dat

[Download and install the Azure SDK for .NET]: /develop/net/
[Creating an Azure Project in Visual Studio]: http://msdn.microsoft.com/library/azure/ee405487.aspx

[Blob5]: ./media/storage-dotnet-how-to-use-table-storage/blob5.png
[Blob6]: ./media/storage-dotnet-how-to-use-table-storage/blob6.png
[Blob7]: ./media/storage-dotnet-how-to-use-table-storage/blob7.png
[Blob8]: ./media/storage-dotnet-how-to-use-table-storage/blob8.png
[Blob9]: ./media/storage-dotnet-how-to-use-table-storage/blob9.png

[Introducing Upsert and Query Projection blog post]: http://blogs.msdn.com/b/windowsazurestorage/archive/2011/09/15/windows-azure-tables-introducing-upsert-and-query-projection.aspx
[.NET Client Library reference]: http://go.microsoft.com/fwlink/?LinkID=390731&clcid=0x409
[Azure Storage Team blog]: http://blogs.msdn.com/b/windowsazurestorage/
[Configure Azure Storage connection strings]: http://msdn.microsoft.com/library/azure/ee758697.aspx
[OData]: http://nuget.org/packages/Microsoft.Data.OData/5.0.2
[Edm]: http://nuget.org/packages/Microsoft.Data.Edm/5.0.2
[Spatial]: http://nuget.org/packages/System.Spatial/5.0.2
[How to: Programmatically access Table storage]: #tablestorage



<!--HONumber=Dec16_HO2-->


