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
ms.date: 10/12/2017
ms.author: arramac
ms.custom: mvc
ms.openlocfilehash: 2189dc7900f03a45c360fceffbcd7c1ff36f7e48
ms.sourcegitcommit: 9c3150e91cc3075141dc2955a01f47040d76048a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/26/2017
---
# <a name="azure-cosmos-db-develop-with-the-table-api-in-net"></a>Azure Cosmos DB: Vývoj s tabulkou rozhraní API v rozhraní .NET

Databáze Azure Cosmos je databázová služba Microsoftu s více modely použitelná v celosvětovém měřítku. Můžete snadno vytvořit a dotazovat databáze dotazů, klíčů/hodnot a grafů, které tak můžou využívat výhody použitelnosti v celosvětovém měřítku a možností horizontálního škálování v jádru databáze Azure Cosmos.

Tento kurz obsahuje následující úlohy: 

> [!div class="checklist"] 
> * Vytvoření účtu služby Azure Cosmos DB 
> * Povolit funkci v souboru app.config 
> * Vytvoření tabulky pomocí [tabulky API](table-introduction.md) (preview)
> * Přidání entity do tabulky 
> * Vložení dávky entit 
> * Načtení jedné entity 
> * Entity dotazu pomocí automatické sekundární indexy 
> * Nahrazení entity 
> * Odstranění entity 
> * Odstranění tabulky
 
## <a name="tables-in-azure-cosmos-db"></a>Tabulky v Azure Cosmos DB 

Poskytuje Azure Cosmos DB [tabulky API](table-introduction.md) (Náhled) pro aplikace, které potřebují úložiště dvojic klíč hodnota s návrhem bez schématu. Pro práci s Azure Cosmos DB lze použít sady SDK [Azure Table Storage](../storage/common/storage-introduction.md) a rozhraní REST API. Azure Cosmos DB můžete použít k vytvoření tabulek s požadavky na vysokou propustnost. Azure Cosmos DB podporuje tabulky s optimalizovanou propustností (neformálně označované jako „tabulky Premium“), aktuálně ve verzi Public Preview. 

Azure Table Storage můžete dále používat pro tabulky s vysokými požadavky na úložiště a nižšími nároky na propustnost.

Pokud aktuálně používáte Azure Table storage, získáte následující výhody s náhledem "premium tabulka":

- Klíč [globální distribuční](distribute-data-globally.md) s více domovských stránek a [automatickou a ruční převzetí služeb při selhání](regional-failover.md)
- Podpora pro automatické indexování pro všechny vlastnosti (dále jen "sekundární indexy") a rychlé dotazy schématu vznikl 
- Podpora pro [nezávislé škálování úložiště a propustnost](partition-data.md), napříč libovolný počet oblastí
- Podpora pro [vyhrazené propustnost za tabulky](request-units.md) , je možné rozšířit stovek na miliony požadavků za sekundu
- Podpora pro [pět přizpůsobitelné úrovně konzistence](consistency-levels.md) pro kompromisy dostupnost, latence a konzistence závisí na vaší aplikaci musí
- 99,99 % dostupnost v rámci jedné oblasti a možnost přidávat další oblasti pro vyšší dostupnosti, a [komplexní SLA špičkový](https://azure.microsoft.com/support/legal/sla/cosmos-db/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) na obecné dostupnosti
- Práce s existující úložiště Azure .NET SDK a beze změn kódu do vaší aplikace

Ve verzi Preview podporuje Azure Cosmos DB rozhraní API tabulky pomocí sady .NET SDK. Si můžete stáhnout [SDK Preview úložiště Azure](https://aka.ms/premiumtablenuget) z NuGet, který má stejné třídy a metody podpisy jako [sada SDK úložiště Azure](https://www.nuget.org/packages/WindowsAzure.Storage), ale také může připojit k Azure Cosmos DB účty pomocí rozhraní API tabulky.

Další informace o složité úlohy Azure Table storage, najdete v části:

* [Úvod do Azure Cosmos DB: tabulky rozhraní API](table-introduction.md)
* V tabulce referenční dokumentaci ke službě kompletní informace o dostupných rozhraních API [Klientská knihovna pro úložiště pro .NET – referenční informace](http://go.microsoft.com/fwlink/?LinkID=390731&clcid=0x409)

### <a name="about-this-tutorial"></a>O tomto kurzu
V tomto kurzu pro vývojáře, kteří se seznámíte s Azure Table storage SDK a chcete použít k dispozici prémiové funkce používá Azure Cosmos DB. Je založena na [Začínáme s Azure Table storage pomocí rozhraní .NET](table-storage-how-to-use-dotnet.md) a ukazuje, jak využít další možnosti jako sekundární indexy, zřízená propustnost a více domovských stránek. Nabídneme použití portálu Azure k vytvoření účtu Azure Cosmos DB sestavení a nasazení aplikace tabulky. Můžeme také provede příklady .NET pro vytváření a odstraňování tabulek a vkládání, aktualizaci, odstranění a dotazování dat v tabulce. 

Pokud ještě nemáte nainstalované Visual Studio 2017, můžete stáhnout a použít **volné** [Visual Studio 2017 Community Edition](https://www.visualstudio.com/downloads/). Nezapomeňte při instalaci sady Visual Studio povolit možnost **Azure Development**.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-a-database-account"></a>Vytvoření účtu databáze

Začněme vytvořením účtu Azure Cosmos DB na portálu Azure.  

> [!TIP]
> * Již máte účet Azure Cosmos DB? Pokud ano, přeskočit na [nastavit řešení sady Visual Studio](#SetupVS).
> * Měli jste účet Azure DocumentDB? Pokud ano, váš účet je teď účet Azure Cosmos DB a můžete přeskočit na [nastavit řešení sady Visual Studio](#SetupVS).  
> * Pokud používáte emulátor DB Cosmos Azure, postupujte podle kroků v [emulátoru DB Cosmos Azure](local-emulator.md) nastavit emulátoru a přeskočit na [nastavení řešení v nástroji Visual Studio](#SetupVS).
<!---Loc Comment: Please, check link [Set up your Visual Studio solution] since it's not redirecting to any location.---> 
>
>

[!INCLUDE [cosmosdb-create-dbaccount-table](../../includes/cosmos-db-create-dbaccount-table.md)] 

## <a name="clone-the-sample-application"></a>Klonování ukázkové aplikace

Teď naklonujeme aplikaci Table z GitHubu, nastavíme připojovací řetězec a spustíme ji.

1. Otevřete okno terminálu Git, jako je třeba Git Bash, a pomocí `cd` přejděte do pracovního adresáře.  

2. Ukázkové úložiště naklonujete spuštěním následujícího příkazu. 

    ```bash
    git clone https://github.com/Azure-Samples/azure-cosmos-db-table-dotnet-getting-started
    ```

3. Potom otevřete soubor řešení v sadě Visual Studio.

## <a name="update-your-connection-string"></a>Aktualizace připojovacího řetězce

Teď se vraťte zpátky na portál Azure Portal, kde najdete informace o připojovacím řetězci, a zkopírujte je do aplikace.

1. Na webu [Azure Portal](http://portal.azure.com/) klikněte v účtu databáze Azure Cosmos v levém navigačním panelu na možnost **Klíče** a potom klikněte na **Klíče pro čtení i zápis**. Pomocí tlačítek kopírovat na pravé straně obrazovky budete zkopírujte připojovací řetězec do souboru app.config v dalším kroku.

2. V sadě Visual Studio otevřete soubor app.config. 

3. Zkopírujte URI hodnota z portálu (pomocí tlačítko Kopírovat) a nastavit jej jako hodnota klíč účtu v souboru app.config. Použijte název účtu, který je vytvořený pro název účtu v souboru app.config.
  
```
<add key="StorageConnectionString" value="DefaultEndpointsProtocol=https;AccountName=account-name;AccountKey=account-key;TableEndpoint=https://account-name.documents.azure.com" />
```

> [!NOTE]
> Chcete-li používat tuto aplikaci s standardní Azure Table Storage, je potřeba změnit připojovací řetězec v `app.config file`. Název účtu použijte jako název tabulky účtu a klíč jako Azure úložiště primární klíč. <br>
>`<add key="StorageConnectionString" value="DefaultEndpointsProtocol=https;AccountName=account-name;AccountKey=account-key;EndpointSuffix=core.windows.net" />`
> 
>

## <a name="build-and-deploy-the-app"></a>Sestavení a nasazení aplikace
1. V sadě Visual Studio klikněte v **Průzkumníku řešení** pravým tlačítkem myši na projekt a potom klikněte na **Spravovat balíčky NuGet**. 

2. Do pole **Procházet** v NuGetu zadejte ***WindowsAzure.Storage-PremiumTable***. Zkontrolujte **zahrnují předprodejní verze**.

3. Ve výsledcích nainstalovat **WindowsAzure.Storage PremiumTable** a zvolte buildu preview `0.0.1-preview`. Tato akce nainstaluje balíček úložiště Azure Table a všechny závislé součásti.

4. Spusťte aplikaci stisknutím CTRL+F5. 

Teď můžete přejít zpět do Průzkumníku dat a zobrazit dotaz, upravit a práci s daty této tabulky. 

> [!NOTE]
> Pro použití této aplikace pomocí emulátoru DB Cosmos Azure, stejně musíte změnit připojovací řetězec v `app.config file`. Použijte menší než hodnota pro emulátor. <br>
>`<add key="StorageConnectionString" value=DefaultEndpointsProtocol=https;AccountName=localhost;AccountKey=<insertkey>==;TableEndpoint=https://localhost -->`
> 
>

## <a name="azure-cosmos-db-capabilities"></a>Možnosti Azure Cosmos DB
Azure Cosmos DB podporuje několik možností, které nejsou k dispozici ve službě Azure Table storage rozhraní API. Nové funkce se dá povolit buď následující `appSettings` hodnoty konfigurace. Zavedeme nejsou žádné nové podpisy nebo přetížení do verze Preview SDK úložiště Azure. To umožňuje připojit standard a premium tabulky a pracovat s jinými službami Azure Storage jako objekty BLOB a fronty. 


| Klíč | Popis |
| --- | --- |
| TableConnectionMode  | Azure Cosmos DB podporuje dva režimy připojení. V `Gateway` režim, jsou vždy vytvářeny požadavky k bráně Azure Cosmos DB, který předává do oddílů odpovídající data. V `Direct` režim připojení, klient načte mapování tabulek do oddílů a jsou vytvářeny požadavky přímo na oddíly data. Doporučujeme, abyste `Direct`, výchozí hodnota.  |
| TableConnectionProtocol | Azure Cosmos DB podporuje dva protokoly připojení - `Https` a `Tcp`. `Tcp`je výchozí a doporučuje, protože je jednodušší. |
| TablePreferredLocations | Čárkami oddělený seznam upřednostňovaných (více funkci) umístění pro čtení. Každý účet Azure Cosmos DB lze přidružit 1 – 30 + oblasti. Každá instance klienta můžete určit podmnožinu těchto oblastí v upřednostňovaném pořadí pro čtení s nízkou latencí. Oblasti musí mít název pomocí jejich [zobrazované názvy](https://msdn.microsoft.com/library/azure/gg441293.aspx), například `West US`. Viz také [více domovských stránek rozhraní API](tutorial-global-distribution-table.md).
| TableConsistencyLevel | Můžete se kompromisy mezi latence, konzistence a dostupnosti výběrem mezi pěti dobře definované úrovně konzistence: `Strong`, `Session`, `Bounded-Staleness`, `ConsistentPrefix`, a `Eventual`. Výchozí hodnota je `Session`. Volba úrovně konzistence díky významně zvýšit výkon rozdíl ve více oblastech nastavení. V tématu [úrovně konzistence](consistency-levels.md) podrobnosti. |
| TableThroughput | Vyhrazenou propustností pro tabulku vyjádřené v jednotek žádosti (RU) za sekundu. Jedné tabulky může podporovat 100s miliony RU/s. V tématu [požadované jednotky](request-units.md). Výchozí hodnota je`400` |
| TableIndexingPolicy | Konzistentní a automatické sekundární indexování všech sloupců v rámci tabulky | Řetězce formátu JSON, které odpovídají specifikace zásady indexování. V tématu [zásady indexování](indexing-policies.md) zobrazíte, jak můžete změnit zásady indexování pro zahrnutí a vyloučení určité sloupce. | Automatické indexování všech vlastností (hodnota hash pro řetězce) a rozsah čísel |
| TableQueryMaxItemCount | Nakonfigurujte maximální počet položek vrátí na dotaz tabulky v jednom dobu odezvy. Výchozí hodnota je `-1`, které umožní Azure DB Cosmos dynamicky určí hodnotu za běhu. |
| TableQueryEnableScan | Pokud dotaz nelze použít pro libovolný filtr index, spusťte ji přesto prostřednictvím kontrolu. Výchozí hodnota je `false`.|
| TableQueryMaxDegreeOfParallelism | Stupně paralelního zpracování pro spuštění dotazu mezi oddílu. `0`sériový s žádné předem načítání, `1` je sériové s předem načítání a vyšší hodnoty zvýšit rychlost paralelního zpracování. Výchozí hodnota je `-1`, které umožní Azure DB Cosmos dynamicky určí hodnotu za běhu. |

Chcete-li změnit výchozí hodnotu, otevřete `app.config` soubor v Průzkumníku řešení v sadě Visual Studio. Přidejte obsah níže uvedeného prvku `<appSettings>`. Nahraďte `account-name` s názvem účtu úložiště a `account-key` nahraďte svým klíčem účtu přístup. 

```xml
<configuration>
    <startup> 
        <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.5.2" />
    </startup>
    <appSettings>
      <!-- Client options -->
      <add key="StorageConnectionString" value="DefaultEndpointsProtocol=https;AccountName=account-name;AccountKey=account-key; TableEndpoint=https://account-name.documents.azure.com" />
      <add key="TableConnectionMode" value="Direct"/>
      <add key="TableConnectionProtocol" value="Tcp"/>
      <add key="TablePreferredLocations" value="East US, West US, North Europe"/>
      <add key="TableConsistencyLevel" value="Eventual"/>

      <!--Table creation options -->
      <add key="TableThroughput" value="700"/>
      <add key="TableIndexingPolicy" value="{""indexingMode"": ""Consistent""}"/>

      <!-- Table query options -->
      <add key="TableQueryMaxItemCount" value="-1"/>
      <add key="TableQueryEnableScan" value="false"/>
      <add key="TableQueryMaxDegreeOfParallelism" value="-1"/>
      <add key="TableQueryContinuationTokenLimitInKb" value="16"/>
            
    </appSettings>
</configuration>
```

Ještě jednou se stručně podívejme na to, co se v aplikaci děje. Otevřete `Program.cs` souboru a najít, že tyto řádky kódu vytvořit tabulku prostředky. 

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

table.CreateIfNotExists();
```

Je důležité rozdíly v vytváření tabulek. Azure Cosmos DB si vyhrazuje propustnost, na rozdíl od modelu na základě spotřeby úložiště Azure pro transakce. Rezervace model má dvě klíčové výhody:

* Vaše propustnost je vyhrazené nebo vyhrazené, tak můžete nikdy získat omezeny Pokud je vaše četnost požadavků nebo zřízené propustnosti pod ní.
* Model rezervace je další [nákladově efektivní pro úlohy náročné propustnost](key-value-store-cost.md)

Můžete nakonfigurovat výchozí propustnosti tím, že nakonfigurujete nastavení `TableThroughput` z hlediska RU (jednotek žádosti) za sekundu. 

Čtení entity 1 KB normalizována jako 1 RU a další operace jsou normalizovány na pevnou hodnotu RU na základě jejich spotřeby procesoru, paměti a procesorů. Další informace o [požadované jednotky v Azure Cosmos DB](request-units.md).

> [!NOTE]
> Zatímco úložiště Table SDK aktuálně nepodporuje změny propustnosti, můžete změnit propustnost okamžitě později pomocí portálu Azure nebo rozhraní příkazového řádku Azure.

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

Rozhraní API tabulky pro Azure Cosmos DB je ve verzi preview. Při obecné dostupnosti záruky latence p99 jsou zajišťované SLA jako jiná rozhraní API Azure Cosmos DB. 

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
Azure Table storage podporuje dávkové operace rozhraní API, která umožňuje zkombinovat aktualizace, odstranění a vložení v jedné dávkové operace. Azure Cosmos DB nemá některá omezení na rozhraní API pro dávkové jako Azure Table storage. Například můžete provádět vícenásobné čtení v dávce, můžete provést několik zápisy na stejnou entitu v dávce a neexistuje žádné omezení na 100 operace na jednu dávku. 

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

Ve verzi preview Azure Cosmos DB podporuje stejné funkce jako Azure Table storage dotazu pro rozhraní API tabulky. Azure Cosmos DB také podporuje řazení, agregace, geoprostorové dotazu, hierarchie a širokou škálu integrované funkce. Další funkce bude k dispozici v rozhraní API tabulky v aktualizaci budoucí služby. V tématu [Azure Cosmos DB dotazu](documentdb-sql-query.md) přehled těchto funkcí. 

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

Pokud nebudete tuto aplikaci nadále používat, pomocí následujícího postupu odstraňte všechny prostředky vytvořené tímto rychlým startem na webu Azure Portal.   

1. V nabídce vlevo na portálu Azure Portal klikněte na **Skupiny prostředků** a pak klikněte na název vytvořeného prostředku.  
2. Na stránce skupiny prostředků klikněte na **Odstranit**, do textového pole zadejte prostředek, který chcete odstranit, a pak klikněte na **Odstranit**. 

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
