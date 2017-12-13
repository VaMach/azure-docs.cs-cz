---
title: "Azure Cosmos DB: Vývoj pomocí rozhraní SQL API v rozhraní .NET | Microsoft Docs"
description: "Naučte se vyvíjet s rozhraním API Azure Cosmos DB SQL pomocí rozhraní .NET"
services: cosmos-db
documentationcenter: 
author: rafats
manager: jhubbard
editor: 
tags: 
ms.assetid: 
ms.service: cosmos-db
ms.devlang: dotnet
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: 
ms.date: 05/10/2017
ms.author: rafats
ms.custom: mvc
ms.openlocfilehash: 9209d815cadcb3abfacdc765c503851ba63863bc
ms.sourcegitcommit: aaba209b9cea87cb983e6f498e7a820616a77471
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/12/2017
---
# <a name="azure-cosmosdb-develop-with-the-sql-api-in-net"></a>Azure CosmosDB: Vývoj pomocí rozhraní SQL API v rozhraní .NET

[!INCLUDE [cosmos-db-sql-api](../../includes/cosmos-db-sql-api.md)]

Databáze Azure Cosmos je databázová služba Microsoftu s více modely použitelná v celosvětovém měřítku. Můžete snadno vytvořit a dotazovat databáze dotazů, klíčů/hodnot a grafů, které tak můžou využívat výhody použitelnosti v celosvětovém měřítku a možností horizontálního škálování v jádru databáze Azure Cosmos. 

Tento kurz ukazuje, jak vytvořit účet Azure Cosmos DB pomocí portálu Azure a pak vytvořte databázi dokumentů a kolekce s [klíč oddílu](documentdb-partition-data.md#partition-keys) pomocí [SQL .NET API](documentdb-introduction.md). Definováním klíč oddílu, když vytvoříte kolekci, je připraven aplikace pak moci bez obtíží škálujte podle rozšiřujícího se vaše data. 

Tento kurz se zaměřuje na tyto úlohy pomocí [SQL .NET API](documentdb-sdk-dotnet.md):

> [!div class="checklist"]
> * Vytvoření účtu služby Azure Cosmos DB
> * Vytvoření databáze a kolekce s klíčem oddílu
> * Vytvoření dokumentů JSON
> * Aktualizace dokumentu
> * Dotaz na dělené kolekce
> * Spuštění uložené procedury
> * Odstranění dokumentu
> * Odstranění databáze

## <a name="prerequisites"></a>Požadavky
Ujistěte se prosím, že máte následující:

* Aktivní účet Azure. Pokud žádný nemáte, můžete si zaregistrovat [bezplatný účet](https://azure.microsoft.com/free/). 

  [!INCLUDE [cosmos-db-emulator-docdb-api](../../includes/cosmos-db-emulator-docdb-api.md)]

* Pokud ještě nemáte nainstalovanou sadu Visual Studio 2017, můžete stáhnout a použít **bezplatnou verzi** [Visual Studio 2017 Community Edition](https://www.visualstudio.com/downloads/). Nezapomeňte při instalaci sady Visual Studio povolit možnost **Azure Development**.

## <a name="create-an-azure-cosmos-db-account"></a>Vytvoření účtu služby Azure Cosmos DB

Začněme vytvořením účtu Azure Cosmos DB na portálu Azure.

> [!TIP]
> * Již máte účet Azure Cosmos DB? Pokud ano, přeskočit na [nastavit řešení sady Visual Studio](#SetupVS)
> * Pokud používáte emulátor DB Cosmos Azure, postupujte podle kroků v [emulátoru DB Cosmos Azure](local-emulator.md) nastavit emulátoru a přeskočit na [nastavení řešení v nástroji Visual Studio](#SetupVS). 
>
>

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

## <a id="SetupVS"></a>Nastavení řešení v sadě Visual Studio
1. Otevřete na svém počítači sadu **Visual Studio**.
2. V nabídce **Soubor** vyberte **Nový** a zvolte **Projekt**.
3. V **nový projekt** dialogovém okně, vyberte **šablony** / **Visual C#** / **konzolovou aplikaci (rozhraní .NET Framework)** , pojmenujte svůj projekt a potom klikněte na **OK**.
   ![Snímek obrazovky okna Nový projekt](./media/tutorial-develop-documentdb-dotnet/nosql-tutorial-new-project-2.png)

4. V **Průzkumníku řešení** klikněte pravým tlačítkem na novou konzolovou aplikaci v rámci řešení sady Visual Studio a pak klikněte na **Spravovat balíčky NuGet**.
    
    ![Snímek obrazovky místní nabídky projektu](./media/tutorial-develop-documentdb-dotnet/nosql-tutorial-manage-nuget-pacakges.png)
5. V **NuGet** , klikněte na **Procházet**a typ **documentdb** do vyhledávacího pole.
<!---stopped here--->
6. Najděte ve výsledcích **Microsoft.Azure.DocumentDB** a klikněte na **Nainstalovat**.
   Je třeba ID balíčku klientské knihovny Azure Cosmos DB [Microsoft.Azure.DocumentDB](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB).
   ![Snímek obrazovky nabídky NuGet pro vyhledání Azure Cosmos DB Client SDK](./media/tutorial-develop-documentdb-dotnet/nosql-tutorial-manage-nuget-pacakges-2.png)

    Pokud se vám zobrazí zpráva týkající se kontroly změn řešení, klikněte na **OK**. Pokud se vám zobrazí zpráva týkající se přijetí licence, klikněte na **Souhlasím**.

## <a id="Connect"></a>Přidejte odkazy na projekt
Příklady zbývajících kroků v tomto kurzu poskytují fragmenty kódu rozhraní API pro SQL, který je nutná k vytváření a aktualizují Azure Cosmos DB prostředky ve vašem projektu.

Nejprve přidejte tyto odkazy na aplikace.
<!---These aren't added by default when you install the pkg?--->

```csharp
using System.Net;
using Microsoft.Azure.Documents;
using Microsoft.Azure.Documents.Client;
using Newtonsoft.Json;
```

## <a id="add-references"></a>Připojení aplikace

V dalším kroku přidejte tyto dvě konstanty a *klienta* proměnné ve vaší aplikaci.

```csharp
private const string EndpointUrl = "<your endpoint URL>";
private const string PrimaryKey = "<your primary key>";
private DocumentClient client;
```

Potom head zpátky [portál Azure](https://portal.azure.com) získat adresu URL koncového bodu a primární klíč. Adresa URL koncového bodu a primární klíč jsou potřeba k tomu, aby aplikace věděla, kam se má připojit, a aby služba Azure Cosmos DB důvěřovala připojení aplikace.

Na portálu Azure přejděte ke svému účtu Azure Cosmos DB, klikněte na **klíče**a potom klikněte na **klíče pro čtení a zápis**.

Zkopírujte URI z portálu a vložte ji přes `<your endpoint URL>` v souboru program.cs. Poté zkopírujte primární klíč z portálu a vložte ji přes `<your primary key>`. Nezapomeňte odebrat `<` a `>` z hodnoty.

![Snímek obrazovky portálu Azure používá v kurzu NoSQL k vytvoření konzolové aplikace jazyka C#. Zobrazuje účet Azure Cosmos DB s klíče v okně účtu Azure Cosmos DB a hodnotami URI a primární klíč v okně klíče](./media/tutorial-develop-documentdb-dotnet/nosql-tutorial-keys.png)

## <a id="instantiate"></a>Vytvoření instance DocumentClient

Teď vytvořte novou instanci třídy **DocumentClient**.

```csharp
DocumentClient client = new DocumentClient(new Uri(EndpointUrl), PrimaryKey);
```

## <a id="create-database"></a>Vytvoření databáze

Dále vytvořte Azure DB Cosmos [databáze](documentdb-resources.md#databases) pomocí [CreateDatabaseAsync](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.createdatabaseasync.aspx) metoda nebo [CreateDatabaseIfNotExistsAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdatabaseifnotexistsasync.aspx) metodu  **DocumentClient** třídy z [SQL .NET SDK](documentdb-sdk-dotnet.md). Databáze je logický kontejner úložiště dokumentů JSON rozděleného mezi kolekcemi.

```csharp
await client.CreateDatabaseAsync(new Database { Id = "db" });
```
## <a name="decide-on-a-partition-key"></a>Vyberte klíč oddílu 

Kolekce jsou kontejnery pro ukládání dokumentů. Jsou logické prostředky a můžete [span jeden nebo více fyzických oddílů](partition-data.md). A [klíč oddílu](documentdb-partition-data.md) je vlastnost (nebo cestu) v rámci vaší dokumenty, které slouží k distribuci dat mezi servery nebo oddíly. Všechny dokumenty se stejným klíčem oddílu ukládají do stejného oddílu. 

Rozhodnutí o důležité, aby předtím, než vytvoříte kolekci, je určení klíč oddílu. Klíče oddílů jsou vlastnosti (nebo cestu) ve vaší dokumenty, které můžete používat k distribuci dat mezi několika servery nebo oddíly Azure Cosmos DB. Cosmos DB hashuje hodnotu klíče oddílu a hash výsledek používá k určení oddílu pro uložení dokumentů. Všechny dokumenty se stejným klíčem oddílu ukládají do stejného oddílu a klíče oddílů nelze změnit po vytvoření kolekce. 

V tomto kurzu vytvoříme nastavit klíč oddílu na `/deviceId` tak, aby všechna data pro jedno zařízení je uložen v jeden oddíl. Chcete vybrat klíč oddílu, který má velký počet hodnot, z nichž každý se používají v o stejnou frekvenci zajistit, že Cosmos DB můžete vyrovnávat zatížení, podle vašich dat roste a dosáhnout úplnou propustnost kolekce. 

Další informace o oddílech najdete v tématu [postup oddílu a škálování v Azure Cosmos DB?](partition-data.md) 

## <a id="CreateColl"></a>Vytvoření kolekce 

Teď, když jsme si vědomi naše klíč oddílu `/deviceId`, umožňuje vytvořit [kolekce](documentdb-resources.md#collections) pomocí [CreateDocumentCollectionAsync](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.createdocumentcollectionasync.aspx) metoda nebo [CreateDocumentCollectionIfNotExistsAsync ](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdocumentcollectionifnotexistsasync.aspx) metodu **DocumentClient** třídy. Kolekce je kontejner dokumentů JSON a všechny přidružené logiky Javascriptové aplikace. 

> [!WARNING]
> Vytvoření kolekce hradí, jako jsou rezervování propustnost pro aplikace komunikovat s Azure Cosmos DB. Další podrobnosti, navštivte naše [stránce s cenami](https://azure.microsoft.com/pricing/details/cosmos-db/)
> 
> 

```csharp
// Collection for device telemetry. Here the JSON property deviceId is used  
// as the partition key to spread across partitions. Configured for 2500 RU/s  
// throughput and an indexing policy that supports sorting against any  
// number or string property. .
DocumentCollection myCollection = new DocumentCollection();
myCollection.Id = "coll";
myCollection.PartitionKey.Paths.Add("/deviceId");

await client.CreateDocumentCollectionAsync(
    UriFactory.CreateDatabaseUri("db"),
    myCollection,
    new RequestOptions { OfferThroughput = 2500 });
```

Tato metoda umožňuje volání Azure Cosmos DB a služby zřizuje počet oddílů podle propustnosti, požadované rozhraní REST API. Propustnost kolekce můžete změnit, protože výkon musí momentální pomocí sady SDK nebo [portál Azure](set-throughput.md).

## <a id="CreateDoc"></a>Vytvoření dokumentů JSON
Umožňuje vložit některé dokumenty JSON do Azure Cosmos DB. [Dokument](documentdb-resources.md#documents) je možné vytvořit pomocí metody [CreateDocumentAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdocumentasync.aspx) třídy **DocumentClient**. Dokumenty představují uživatelem definovaný (libovolný) obsah JSON. Tato ukázka třída obsahuje zařízení, čtení a volání CreateDocumentAsync vložit nového zařízení čtení do kolekce.

```csharp
public class DeviceReading
{
    [JsonProperty("id")]
    public string Id;

    [JsonProperty("deviceId")]
    public string DeviceId;

    [JsonConverter(typeof(IsoDateTimeConverter))]
    [JsonProperty("readingTime")]
    public DateTime ReadingTime;

    [JsonProperty("metricType")]
    public string MetricType;

    [JsonProperty("unit")]
    public string Unit;

    [JsonProperty("metricValue")]
    public double MetricValue;
  }

// Create a document. Here the partition key is extracted 
// as "XMS-0001" based on the collection definition
await client.CreateDocumentAsync(
    UriFactory.CreateDocumentCollectionUri("db", "coll"),
    new DeviceReading
    {
        Id = "XMS-001-FE24C",
        DeviceId = "XMS-0001",
        MetricType = "Temperature",
        MetricValue = 105.00,
        Unit = "Fahrenheit",
        ReadingTime = DateTime.UtcNow
    });
```
## <a name="read-data"></a>Čtení dat

Umožňuje čtení dokumentu svým klíč oddílu a Id pomocí metody ReadDocumentAsync. Všimněte si, že čtení obsahují hodnotu PartitionKey (odpovídající `x-ms-documentdb-partitionkey` hlavička požadavku v rozhraní REST API).

```csharp
// Read document. Needs the partition key and the Id to be specified
Document result = await client.ReadDocumentAsync(
  UriFactory.CreateDocumentUri("db", "coll", "XMS-001-FE24C"), 
  new RequestOptions { PartitionKey = new PartitionKey("XMS-0001") });

DeviceReading reading = (DeviceReading)(dynamic)result;
```

## <a name="update-data"></a>Aktualizace dat

Teď umožňuje aktualizovat některé data pomocí metody ReplaceDocumentAsync.

```csharp
// Update the document. Partition key is not required, again extracted from the document
reading.MetricValue = 104;
reading.ReadingTime = DateTime.UtcNow;

await client.ReplaceDocumentAsync(
  UriFactory.CreateDocumentUri("db", "coll", "XMS-001-FE24C"), 
  reading);
```

## <a name="delete-data"></a>Odstranění dat

Teď umožňuje odstranit dokument klíčem oddílu a id pomocí metody DeleteDocumentAsync.

```csharp
// Delete a document. The partition key is required.
await client.DeleteDocumentAsync(
  UriFactory.CreateDocumentUri("db", "coll", "XMS-001-FE24C"), 
  new RequestOptions { PartitionKey = new PartitionKey("XMS-0001") });
```
## <a name="query-partitioned-collections"></a>Dotaz na dělené kolekce

Pro dotazování dat v kolekcích oddílů Azure Cosmos DB automaticky směruje dotaz na oddíly odpovídající hodnoty klíče oddílu zadaných ve filtru (pokud existují). Například tento dotaz se směruje na právě oddílu klíč oddílu "XMS-0001".

```csharp
// Query using partition key
IQueryable<DeviceReading> query = client.CreateDocumentQuery<DeviceReading>(
    UriFactory.CreateDocumentCollectionUri("db", "coll"))
    .Where(m => m.MetricType == "Temperature" && m.DeviceId == "XMS-0001");
```
    
Následující dotaz na klíč oddílu (DeviceId) nemá filtr a je fanned pro všechny oddíly, kde je u indexu oddílu spustit. Všimněte si, že budete muset určit EnableCrossPartitionQuery (`x-ms-documentdb-query-enablecrosspartition` v rozhraní REST API) tak, aby měl sady SDK při spuštění dotazu napříč oddíly.

```csharp
// Query across partition keys
IQueryable<DeviceReading> crossPartitionQuery = client.CreateDocumentQuery<DeviceReading>(
    UriFactory.CreateDocumentCollectionUri("db", "coll"), 
    new FeedOptions { EnableCrossPartitionQuery = true })
    .Where(m => m.MetricType == "Temperature" && m.MetricValue > 100);
```

## <a name="parallel-query-execution"></a>Provádění paralelního dotazu
Azure Cosmos DB SQL SDK 1.9.0 a vyšší možnosti provádění paralelního dotazu podpory, které umožňují provádět dotazy s nízkou latencí pro dělené kolekce, i v případě, že potřebují k touch velký počet oddílů. Například následující dotaz je nakonfigurována pro spuštění paralelně napříč oddíly.

```csharp
// Cross-partition Order By queries
IQueryable<DeviceReading> crossPartitionQuery = client.CreateDocumentQuery<DeviceReading>(
    UriFactory.CreateDocumentCollectionUri("db", "coll"), 
    new FeedOptions { EnableCrossPartitionQuery = true, MaxDegreeOfParallelism = 10, MaxBufferedItemCount = 100})
    .Where(m => m.MetricType == "Temperature" && m.MetricValue > 100)
    .OrderBy(m => m.MetricValue);
```
    
Provádění paralelního dotazu můžete spravovat pomocí ladění následující parametry:

* Nastavením `MaxDegreeOfParallelism`, můžete řídit stupně paralelního zpracování tedy maximální počet souběžných síťové připojení k kolekce oddíly. Pokud nastavíte na hodnotu -1, stupně paralelního zpracování spravuje sady SDK. Pokud `MaxDegreeOfParallelism` není zadaný, nebo je nastavený na 0, což je výchozí hodnota, bude jedno síťové připojení k oddílům kolekce.
* Nastavením `MaxBufferedItemCount`, můžete kompromisy využití paměti dotazu latence a na straně klienta. Pokud tento parametr vynecháte nebo tuto možnost nastavíte na hodnotu -1, počet položek do vyrovnávací paměti při provádění paralelního dotazu, které spravuje sady SDK.

Zadaný stav stejné kolekce, paralelní dotaz vrátí výsledky ve stejném pořadí jako sériové provádění. Při provádění dotazu mezi oddílu, který zahrnuje řazení (ORDER BY a/nebo horní), sadu SDK SQL vydá dotaz paralelně napříč oddíly a sloučí částečně seřazená výsledky na straně klienta k vytvoření globální seřazené výsledky.

## <a name="execute-stored-procedures"></a>Provedení uložené procedury
Nakonec můžete provést jednotlivé transakce na dokumenty se stejným ID zařízení, například pokud jste údržbě agregace nebo nejnovější stav zařízení do jednoho dokumentu přidáním následující kód do projektu.

```csharp
await client.ExecuteStoredProcedureAsync<DeviceReading>(
    UriFactory.CreateStoredProcedureUri("db", "coll", "SetLatestStateAcrossReadings"),
    new RequestOptions { PartitionKey = new PartitionKey("XMS-001") }, 
    "XMS-001-FE24C");
```

A je to! ty jsou hlavními součástmi Azure Cosmos DB aplikaci, která používá klíč oddílu efektivně škálovat distribuci dat mezi oddílů.  

## <a name="clean-up-resources"></a>Vyčištění prostředků

Pokud nechcete pokračovat v používání této aplikace, odstraňte všechny prostředky, které jsou vytvořené v tomto kurzu na portálu Azure pomocí následujících kroků:

1. Z nabídky na levé straně na portálu Azure, klikněte na tlačítko **skupiny prostředků** a pak klikněte na jedinečný název prostředku, který jste vytvořili. 
2. Na stránce skupiny prostředků klikněte na **Odstranit**, do textového pole zadejte prostředek, který chcete odstranit, a pak klikněte na **Odstranit**.

## <a name="next-steps"></a>Další kroky

V tomto kurzu jste provést následující: 

> [!div class="checklist"]
> * Vytvoření účtu Azure Cosmos DB
> * Vytvořit databázi a kolekci s klíčem oddílu
> * Vytvoření dokumentů JSON
> * Aktualizovat dokumentu
> * Dotaz dělené kolekce
> * Byla spuštěna uložené procedury
> * Odstranit dokumentu
> * Odstranit databázi

Teď můžete pokračovat v dalším kurzu a další data importovat do účtu Cosmos DB. 

> [!div class="nextstepaction"]
> [Importování dat do služby Azure Cosmos DB](import-data.md)
