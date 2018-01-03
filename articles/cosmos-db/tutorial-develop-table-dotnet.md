---
title: "Azure Cosmos DB: Vývoj s tabulkou rozhraní API v rozhraní .NET | Microsoft Docs"
description: "Naučte se vyvíjet s rozhraním API Azure Cosmos DB Table pomocí rozhraní .NET"
services: cosmos-db
documentationcenter: 
author: mimig1
manager: jhubbard
editor: 
ms.assetid: 4b22cb49-8ea2-483d-bc95-1172cd009498
ms.service: cosmos-db
ms.workload: 
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: tutorial
ms.date: 12/18/2017
ms.author: arramac
ms.custom: mvc
ms.openlocfilehash: 41d7e42f203170e4fa3b8e3a8c973e23808f941b
ms.sourcegitcommit: c87e036fe898318487ea8df31b13b328985ce0e1
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/19/2017
---
# <a name="azure-cosmos-db-develop-with-the-table-api-in-net"></a>Azure Cosmos DB: Vývoj s tabulkou rozhraní API v rozhraní .NET

Azure Cosmos DB je globálně distribuovaná databázová služba Microsoftu pro více modelů. Můžete snadno vytvořit a dotazovat databáze dotazů, klíčů/hodnot a grafů, které tak můžou využívat výhody použitelnosti v celosvětovém měřítku a možností horizontálního škálování v jádru databáze Azure Cosmos.

Tento kurz obsahuje následující úlohy: 

> [!div class="checklist"] 
> * Vytvoření účtu služby Azure Cosmos DB 
> * Povolit funkci v souboru app.config 
> * Vytvoření tabulky pomocí [tabulky rozhraní API](table-introduction.md)
> * Přidání entity do tabulky 
> * Vložení dávky entit 
> * Načtení jedné entity 
> * Entity dotazu pomocí automatické sekundární indexy 
> * Nahrazení entity 
> * Odstranění entity 
> * Odstranění tabulky
 
## <a name="tables-in-azure-cosmos-db"></a>Tabulky v Azure Cosmos DB 

Poskytuje Azure Cosmos DB [tabulky API](table-introduction.md) pro aplikace, které potřebují úložiště dvojic klíč hodnota s návrhem bez schématu. Rozhraní API i Azure DB Cosmos tabulky a [Azure Table storage](../storage/common/storage-introduction.md) teď podporují stejné sady SDK a rozhraní REST API. Azure Cosmos DB můžete použít k vytvoření tabulek s požadavky na vysokou propustnost.

Tento kurz je určen pro vývojáře, kteří se seznámíte s Azure Table storage SDK a chcete použít k dispozici prémiových funkcí s Azure Cosmos DB. Je založena na [Začínáme s Azure Table storage pomocí rozhraní .NET](table-storage-how-to-use-dotnet.md) a ukazuje, jak využít další možnosti jako sekundární indexy, zřízená propustnost a více domovských stránek. Tento kurz popisuje, jak pomocí portálu Azure k vytvoření účtu Azure Cosmos DB a sestavení a nasazení aplikace API tabulky. Můžeme také provede příklady .NET pro vytváření a odstraňování tabulek a vkládání, aktualizaci, odstranění a dotazování dat v tabulce. 

Pokud aktuálně používáte Azure Table storage, získáte následující výhody s rozhraním API pro Azure Cosmos DB tabulky:

- Klíč [globální distribuční](distribute-data-globally.md) s více domovských stránek a [automatickou a ruční převzetí služeb při selhání](regional-failover.md)
- Podpora pro automatické indexování pro všechny vlastnosti (dále jen "sekundární indexy") a rychlé dotazy schématu vznikl 
- Podpora pro [nezávislé škálování úložiště a propustnost](partition-data.md), napříč libovolný počet oblastí
- Podpora pro [vyhrazené propustnost za tabulky](request-units.md) , je možné rozšířit stovek na miliony požadavků za sekundu
- Podpora pro [pět přizpůsobitelné úrovně konzistence](consistency-levels.md) pro kompromisy dostupnost, latence a konzistence závisí na vaší aplikaci musí
- 99,99 % dostupnost v rámci jedné oblasti a možnost přidávat další oblasti pro vyšší dostupnosti, a [komplexní SLA špičkový](https://azure.microsoft.com/support/legal/sla/cosmos-db/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) na obecné dostupnosti
- Práce s existující úložiště Azure .NET SDK a beze změn kódu do vaší aplikace

Tento kurz se zaměřuje Cosmos DB tabulky rozhraní API služby Azure pomocí sady .NET SDK. Si můžete stáhnout [SDK Preview úložiště Azure](https://aka.ms/tableapinuget) z NuGet.

Další informace o složité úlohy Azure Table storage, najdete v části:

* [Úvod do Azure Cosmos DB tabulky API](table-introduction.md)
* V tabulce referenční dokumentaci ke službě kompletní informace o dostupných rozhraních API [Azure Cosmos DB tabulky rozhraní API .NET SDK](https://docs.microsoft.com/dotnet/api/overview/azure/cosmosdb/client?view=azure-dotnet)

### <a name="about-this-tutorial"></a>O tomto kurzu
V tomto kurzu pro vývojáře, kteří se seznámíte s Azure Table storage SDK a chcete použít k dispozici prémiové funkce používá Azure Cosmos DB. Je založena na [Začínáme s Azure Table storage pomocí rozhraní .NET](table-storage-how-to-use-dotnet.md) a ukazuje, jak využít další možnosti jako sekundární indexy, zřízená propustnost a více domovských stránek. Nabídneme použití portálu Azure k vytvoření účtu Azure Cosmos DB sestavení a nasazení aplikace tabulky. Můžeme také provede příklady .NET pro vytváření a odstraňování tabulek a vkládání, aktualizaci, odstranění a dotazování dat v tabulce. 

Pokud ještě nemáte nainstalovanou sadu Visual Studio 2017, můžete stáhnout a použít **bezplatnou verzi** [Visual Studio 2017 Community Edition](https://www.visualstudio.com/downloads/). Nezapomeňte při instalaci sady Visual Studio povolit možnost **Azure Development**.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-a-database-account"></a>Vytvoření účtu databáze

Začněme vytvořením účtu Azure Cosmos DB na portálu Azure.  
 
> [!IMPORTANT]  
> Abyste mohli pracovat s obecně dostupnými sadami Table API SDK, musíte si vytvořit nový účet Table API. Obecně dostupné sady SDK nepodporují účty Table API vytvořené během období Preview. 
>

[!INCLUDE [cosmosdb-create-dbaccount-table](../../includes/cosmos-db-create-dbaccount-table.md)] 

## <a name="clone-the-sample-application"></a>Klonování ukázkové aplikace

Teď naklonujeme aplikaci Table z GitHubu, nastavíme připojovací řetězec a spustíme ji. Přesvědčíte se, jak snadno se pracuje s daty prostřednictvím kódu programu. 

1. Otevřete okno terminálu Git, třeba Git Bash, a pomocí příkazu `cd` přejděte do složky, do které chcete nainstalovat ukázkovou aplikaci. 

    ```bash
    cd "C:\git-samples"
    ```

2. Ukázkové úložiště naklonujete spuštěním následujícího příkazu. Tento příkaz vytvoří na vašem počítači kopii ukázkové aplikace. 

    ```bash
    git clone https://github.com/Azure-Samples/storage-table-dotnet-getting-started.git
    ```

3. Potom otevřete soubor řešení v sadě Visual Studio. 

## <a name="update-your-connection-string"></a>Aktualizace připojovacího řetězce

Teď se vraťte zpátky na portál Azure Portal, kde najdete informace o připojovacím řetězci, a zkopírujte je do aplikace. Tím aplikaci umožníte komunikovat s hostovanou databází. 

1. Na webu [Azure Portal](http://portal.azure.com/) klikněte na **Připojovací řetězec**. 

    Pomocí tlačítka pro kopírování na pravé straně obrazovky zkopírujte PRIMÁRNÍ PŘIPOJOVACÍ ŘETĚZEC.

    ![Zobrazení a zkopírování hodnoty PŘIPOJOVACÍ ŘETĚZEC v podokně Připojovací řetězec](./media/create-table-dotnet/connection-string.png)

2. V sadě Visual Studio otevřete soubor app.config. 

3. Vzhledem k tomu, že v tomto kurzu se nepoužívá emulátor úložiště, zrušte komentář u StorageConnectionString na řádku 8 a přidejte komentář u StorageConnectionString na řádku 7. Řádky 7 a 8 by teď měly vypadat takto:

    ```
    <!--key="StorageConnectionString" value="UseDevelopmentStorage=true;" />-->
    <add key="StorageConnectionString" value="DefaultEndpointsProtocol=https;AccountName=[AccountName];AccountKey=[AccountKey]" />
    ```

4. Vložte PRIMÁRNÍ PŘIPOJOVACÍ ŘETĚZEC z portálu do hodnoty StorageConnectionString na řádku 8. Řetězec vložte do uvozovek.
   
    > [!IMPORTANT]
    > Pokud váš koncový bod používá adresu documents.azure.com, znamená to, že máte účet Preview, a pokud chcete pracovat s obecně dostupnou sadou Table API SDK, musíte si vytvořit [nový účet Table API](#create-a-database-account). 
    >

    Řádek 8 by teď měl vypadat nějak takto:

    ```
    <add key="StorageConnectionString" value="DefaultEndpointsProtocol=https;AccountName=<account name>;AccountKey=txZACN9f...==;TableEndpoint=https://<account name>.table.cosmosdb.azure.com;" />
    ```

5. Uložte soubor app.config.

Teď jste aktualizovali aplikaci a zadali do ní všechny informace potřebné ke komunikaci s Azure Cosmos DB. 

## <a name="azure-cosmos-db-capabilities"></a>Možnosti Azure Cosmos DB
Azure Cosmos DB podporuje několik možností, které nejsou k dispozici ve službě Azure Table storage rozhraní API. 

Některé dílčí funkce přistupuje prostřednictvím nového přetížení CreateCloudTableClient, umožňující jeden k určení úrovně zásady a konzistence připojení.

| Nastavení připojení tabulky | Popis |
| --- | --- |
| Režim připojení  | Azure Cosmos DB podporuje dva režimy připojení. V `Gateway` režim, jsou vždy vytvářeny požadavky k bráně Azure Cosmos DB, který předává do oddílů odpovídající data. V `Direct` režim připojení, klient načte mapování tabulek do oddílů a jsou vytvářeny požadavky přímo na oddíly data. Doporučujeme, abyste `Direct`, výchozí hodnota.  |
| Připojení protokolu | Azure Cosmos DB podporuje dva protokoly připojení - `Https` a `Tcp`. `Tcp`je výchozí a doporučuje, protože je jednodušší. |
| Upřednostňované umístění | Čárkami oddělený seznam upřednostňovaných (více funkci) umístění pro čtení. Každý účet Azure Cosmos DB lze přidružit 1 – 30 + oblasti. Každá instance klienta můžete určit podmnožinu těchto oblastí v upřednostňovaném pořadí pro čtení s nízkou latencí. Oblasti musí mít název pomocí jejich [zobrazované názvy](https://msdn.microsoft.com/library/azure/gg441293.aspx), například `West US`. Viz také [více domovských stránek rozhraní API](tutorial-global-distribution-table.md). |
| Úrovně konzistence | Můžete se kompromisy mezi latence, konzistence a dostupnosti výběrem mezi pěti dobře definované úrovně konzistence: `Strong`, `Session`, `Bounded-Staleness`, `ConsistentPrefix`, a `Eventual`. Výchozí hodnota je `Session`. Volba úrovně konzistence díky významně zvýšit výkon rozdíl ve více oblastech nastavení. V tématu [úrovně konzistence](consistency-levels.md) podrobnosti. |

Další funkce se dá povolit buď následující `appSettings` hodnoty konfigurace.

| Klíč | Popis |
| --- | --- |
| TableQueryMaxItemCount | Nakonfigurujte maximální počet položek vrátí na dotaz tabulky v jednom dobu odezvy. Výchozí hodnota je `-1`, které umožní Azure DB Cosmos dynamicky určí hodnotu za běhu. |
| TableQueryEnableScan | Pokud dotaz nelze použít pro libovolný filtr index, spusťte ji přesto prostřednictvím kontrolu. Výchozí hodnota je `false`.|
| TableQueryMaxDegreeOfParallelism | Stupně paralelního zpracování pro spuštění dotazu mezi oddílu. `0`sériový s žádné předem načítání, `1` je sériové s předem načítání a vyšší hodnoty zvýšit rychlost paralelního zpracování. Výchozí hodnota je `-1`, které umožní Azure DB Cosmos dynamicky určí hodnotu za běhu. |

Chcete-li změnit výchozí hodnotu, otevřete `app.config` soubor v Průzkumníku řešení v sadě Visual Studio. Přidejte obsah níže uvedeného prvku `<appSettings>`. Nahraďte `account-name` s názvem účtu úložiště a `account-key` nahraďte svým klíčem účtu přístup. 

```xml
<configuration>
    ...
    <appSettings>
      <!-- Client options -->
      <add key="CosmosDBStorageConnectionString" 
        value="DefaultEndpointsProtocol=https;AccountName=MYSTORAGEACCOUNT;AccountKey=AUTHKEY;TableEndpoint=https://account-name.table.cosmosdb.azure.com" />
      <add key="StorageConnectionString" value="DefaultEndpointsProtocol=https;AccountName=account-name;AccountKey=account-key; TableEndpoint=https://account-name.documents.azure.com" />

      <!-- Table query options -->
      <add key="TableQueryMaxItemCount" value="-1"/>
      <add key="TableQueryEnableScan" value="false"/>
      <add key="TableQueryMaxDegreeOfParallelism" value="-1"/>
      <add key="TableQueryContinuationTokenLimitInKb" value="16"/>
            
    </appSettings>
</configuration>
```

Ještě jednou se stručně podívejme na to, co se v aplikaci děje. Otevřete `Program.cs` souboru a zjistí, že tyto řádky kódu vytvořit tabulku prostředky. 

## <a name="create-the-table-client"></a>Vytvoření tabulky klienta
Inicializaci `CloudTableClient` pro připojení k účtu tabulky.

```csharp
CloudTableClient tableClient = storageAccount.CreateCloudTableClient();
```
Tento klient je inicializována pomocí `TableConnectionMode`, `TableConnectionProtocol`, `TableConsistencyLevel`, a `TablePreferredLocations` konfigurační hodnoty, pokud zadaný v nastavení aplikace.

## <a name="create-a-table"></a>Vytvoření tabulky
Pak vytvořte tabulku pomocí `CloudTable`. Tabulky v databázi Cosmos Azure můžete nezávisle škálovat z hlediska úložiště a propustnost a dělení se automaticky zpracovává službou. Azure Cosmos DB podporuje pevné velikosti a neomezená tabulky. V tématu [vytváření oddílů v Azure Cosmos DB](partition-data.md) podrobnosti. 

```csharp
CloudTable table = tableClient.GetTableReference("people");
400
table.CreateIfNotExists(throughput: 800);
```

Je důležité rozdíly v vytváření tabulek. Azure Cosmos DB si vyhrazuje propustnost, na rozdíl od modelu na základě spotřeby úložiště Azure pro transakce. Vaše propustnost je vyhrazené/vyhrazená, takže jste nikdy získat omezeny Pokud je vaše četnost požadavků nebo pod zřízené propustnosti.

Můžete nakonfigurovat výchozí propustnost zahrnutím jako parametr CreateIfNotExists.

Čtení entity 1 KB normalizována jako 1 RU a další operace jsou normalizovány na pevnou hodnotu RU na základě jejich spotřeby procesoru, paměti a procesorů. Další informace o [požadované jednotky v Azure Cosmos DB](request-units.md) a speciálně pro [klíče ukládá hodnotu](key-value-store-cost.md).

Potom provede jednoduché pro čtení a zápisu operace pomocí Azure Table storage SDK. Tento kurz představuje předvídatelný nízkou milisekundu jednociferné latenci a rychlé dotazy, které poskytuje Azure Cosmos DB.

## <a name="add-an-entity-to-a-table"></a>Přidání entity do tabulky
Entity v Azure Table storage vycházet z `TableEntity` třídy a musí mít `PartitionKey` a `RowKey` vlastnosti. Zde je ukázka definice pro entitu zákazníka.

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

Následující fragment kódu ukazuje, jak vloží entitu s Azure storage SDK. Azure Cosmos DB je určená pro zaručit s nízkou latencí v jakémkoli měřítku po celém světě.

Dokončení zápisu < 15 ms v p99 a ms ~ 6 v p50 pro aplikace spuštěné ve stejné oblasti jako účet Azure Cosmos DB. A tato doba trvání účty pro fakt, že zápisy se až po jejich synchronně replikovány, spolehlivě potvrzené, a veškerý obsah je indexovaný potvrzené zpět do klienta.


```csharp
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
Azure Table storage podporuje dávkové operace rozhraní API, která umožňuje zkombinovat aktualizace, odstranění a vložení v rámci jedné dávkové operace.

```csharp
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
## <a name="retrieve-a-single-entity"></a>Načtení jedné entity
Načte (získá) v Azure DB Cosmos dokončení < 10 ms v p99 a ~ 1 ms v p50 ve stejné oblasti Azure. Můžete přidat jako v mnoha oblastech ke svému účtu pro čtení s nízkou latencí a nasadit aplikace do čtení ze své místní oblast ("s více adresami") nastavením `TablePreferredLocations`. 

Můžete načíst jednu entitu pomocí následující fragment kódu:

```csharp
// Create a retrieve operation that takes a customer entity.
TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

// Execute the retrieve operation.
TableResult retrievedResult = table.Execute(retrieveOperation);
```
> [!TIP]
> Další informace o více funkci rozhraní API v [vývoj s několika oblastí](tutorial-global-distribution-table.md)
>

## <a name="query-entities-using-automatic-secondary-indexes"></a>Entity dotazu pomocí automatické sekundární indexy
Tabulky lze dotazovat pomocí `TableQuery` třídy. Azure Cosmos DB má optimalizované zápisu databázový stroj, který automaticky indexuje všechny sloupce v tabulce. Indexování v Azure Cosmos DB nerozlišuje schématu. Proto i v případě, že vaše schéma je odlišné mezi řádky nebo pokud schéma vyvíjí v průběhu času, je automaticky indexován. Vzhledem k tomu, že Azure Cosmos DB podporuje automatické sekundární indexy, dotazy pro vlastnost, můžete použít index a efektivně obsluhovat.

```csharp
CloudTable table = tableClient.GetTableReference("people");

// Filter against a property that's not partition key or row key
TableQuery<CustomerEntity> emailQuery = new TableQuery<CustomerEntity>().Where(
    TableQuery.GenerateFilterCondition("Email", QueryComparisons.Equal, "Ben@contoso.com"));

foreach (CustomerEntity entity in table.ExecuteQuery(emailQuery))
{
    Console.WriteLine("{0}, {1}\t{2}\t{3}", entity.PartitionKey, entity.RowKey,
        entity.Email, entity.PhoneNumber);
}
```

Azure Cosmos DB podporuje stejné funkce jako Azure Table storage dotazu pro rozhraní API tabulky. Azure Cosmos DB také podporuje řazení, agregace, geoprostorové dotazu, hierarchie a širokou škálu integrované funkce. Další funkce bude k dispozici v rozhraní API tabulky v aktualizaci budoucí služby. V tématu [Azure Cosmos DB dotazu](sql-api-sql-query.md) přehled těchto funkcí. 

## <a name="replace-an-entity"></a>Nahrazení entity
Pokud chcete entitu aktualizovat, načtěte ji ze služby Table, upravte objekt entity a potom uložte změny zpět do služby Table. Následující kód změní telefonní číslo stávajícího zákazníka. 

```csharp
TableOperation updateOperation = TableOperation.Replace(updateEntity);
table.Execute(updateOperation);
```
Podobně můžete provádět `InsertOrMerge` nebo `Merge` operace.  

## <a name="delete-an-entity"></a>Odstranění entity
Entitu můžete po jejím načtení snadno odstranit, a to pomocí stejného vzoru zobrazovaného pro aktualizaci entity. Následující kód načte a odstraní entitu zákazníka.

```csharp
TableOperation deleteOperation = TableOperation.Delete(deleteEntity);
table.Execute(deleteOperation);
```

## <a name="delete-a-table"></a>Odstranění tabulky
Následující příklad kódu nakonec odstraní tabulku z účtu úložiště. Můžete odstranit a znovu vytvořte tabulku okamžitě s Azure Cosmos DB.

```csharp
CloudTable table = tableClient.GetTableReference("people");
table.DeleteIfExists();
```

## <a name="clean-up-resources"></a>Vyčištění prostředků

[!INCLUDE [cosmosdb-delete-resource-group](../../includes/cosmos-db-delete-resource-group.md)]

## <a name="next-steps"></a>Další kroky

V tomto kurzu jsme popsané postupy a začněte využívat Azure Cosmos DB s rozhraním API pro tabulky a jste provést následující: 

> [!div class="checklist"] 
> * Vytvoření účtu Azure Cosmos DB 
> * Povolená funkce v souboru app.config 
> * Vytvoření tabulky 
> * Přidání entity do tabulky 
> * Vložit dávku entit 
> * Načíst jednu entitu 
> * Dotazovaný entity pomocí automatické sekundární indexy 
> * Nahradí entitu 
> * Odstranit entity 
> * Odstranit tabulku  

Teď můžete pokračovat v dalším kurzu a další informace o dotazování dat v tabulce. 

> [!div class="nextstepaction"]
> [Dotaz s tabulkou rozhraní API](tutorial-query-table.md)
