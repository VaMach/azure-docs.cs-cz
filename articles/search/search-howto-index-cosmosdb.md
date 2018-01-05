---
title: "Indexování zdroje dat Cosmos DB pro službu Azure Search | Microsoft Docs"
description: "Tento článek ukazuje, jak vytvořit indexer Azure Search s DB Cosmos jako zdroj dat."
services: search
documentationcenter: 
author: chaosrealm
manager: pablocas
editor: 
ms.assetid: 
ms.service: search
ms.devlang: rest-api
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: search
ms.date: 08/10/2017
ms.author: eugenesh
robot: noindex
ms.openlocfilehash: c7c883f683c744415a1b600cea45c1882939e021
ms.sourcegitcommit: 3cdc82a5561abe564c318bd12986df63fc980a5a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/05/2018
---
# <a name="connecting-cosmos-db-with-azure-search-using-indexers"></a>Připojování Cosmos databáze s Azure Search pomocí indexerů

Pokud chcete implementovat skvělé vyhledáváním přes Cosmos DB data, můžete použít indexer Azure Search k získání dat do indexu Azure Search. V tomto článku jsme ukazují, jak integrovat Azure Cosmos DB Azure Search bez nutnosti psaní jakéhokoli kódu k údržbě indexování infrastruktury.

Chcete-li nastavit indexer Cosmos DB, musíte mít [služby Azure Search](search-create-service-portal.md)a vytvořit index, zdroj dat a nakonec indexeru. Můžete vytvořit tyto objekty pomocí [portál](search-import-data-portal.md), [.NET SDK](/dotnet/api/microsoft.azure.search), nebo [REST API](/rest/api/searchservice/) pro všechny jazyky,-rozhraní .NET. 

Pokud se přihlásíte pro tento portál [Průvodce importem dat](search-import-data-portal.md) provede vás procesem vytvoření těchto prostředků.

> [!NOTE]
> Azure Cosmos DB je nová generace DocumentDB. I když se změnil název produktu, syntaxe je stejná jako předtím. Pokračovat v zadejte `documentdb` podle pokynů v tomto článku indexer. 

> [!TIP]
> Můžete spustit **importovat data** z řídicího panelu Cosmos DB zjednodušit indexování pro tento zdroj dat průvodce. Začněte tak, že v levém navigačním panelu přejdete do **Collections** (Kolekce) > **Add Azure Search** (Přidat službu Azure Search).

<a name="Concepts"></a>
## <a name="azure-search-indexer-concepts"></a>Koncepty indexer Azure Search
Azure Search podporuje vytváření a správu datové zdroje (včetně Cosmos DB) a indexery, které provozují těchto zdrojů.

A **zdroj dat** data určená k indexu, pověření a zásady pro identifikaci změny v datech (například dokumenty modified nebo deleted uvnitř vaší kolekce). Zdroj dat je definován jako nezávislý prostředek, aby se můžete použít několik indexerů.

**Indexer** popisuje, jak tok dat ze zdroje dat do indexu vyhledávání cíl. Indexer umožňuje:

* Proveďte jednorázové kopii dat k naplnění indexu.
* Synchronizujte indexu se změnami ve zdroji dat podle plánu. Plán je součástí definice indexer.
* Vyvolání aktualizace na vyžádání do indexu podle potřeby.

<a name="CreateDataSource"></a>
## <a name="step-1-create-a-data-source"></a>Krok 1: Vytvoření zdroje dat
Chcete-li vytvořit zdroj dat, proveďte příspěvku na:

    POST https://[service name].search.windows.net/datasources?api-version=2016-09-01
    Content-Type: application/json
    api-key: [Search service admin key]

    {
        "name": "mydocdbdatasource",
        "type": "documentdb",
        "credentials": {
            "connectionString": "AccountEndpoint=https://myDocDbEndpoint.documents.azure.com;AccountKey=myDocDbAuthKey;Database=myDocDbDatabaseId"
        },
        "container": { "name": "myDocDbCollectionId", "query": null },
        "dataChangeDetectionPolicy": {
            "@odata.type": "#Microsoft.Azure.Search.HighWaterMarkChangeDetectionPolicy",
            "highWaterMarkColumnName": "_ts"
        }
    }

Text žádosti obsahuje definice zdroje dat, která by měla obsahovat následující pole:

* **název**: Vyberte libovolný název představují databáze Cosmos DB.
* **typ**: musí být `documentdb`.
* **přihlašovací údaje**:
  
  * **connectionString**: vyžaduje. Zadejte informace o připojení k databázi Azure Cosmos DB v následujícím formátu:`AccountEndpoint=<Cosmos DB endpoint url>;AccountKey=<Cosmos DB auth key>;Database=<Cosmos DB database id>`
* **kontejner**:
  
  * **název**: vyžaduje. Zadejte id kolekce Cosmos DB pro indexování.
  * **dotaz**: volitelné. Můžete zadat dotaz na narovnání libovolný dokument JSON na ploché schéma, které mohou indexu Azure Search.
* **dataChangeDetectionPolicy**: doporučené. V tématu [indexování dokumentů změnit](#DataChangeDetectionPolicy) části.
* **dataDeletionDetectionPolicy**: volitelné. V tématu [indexování dokumentů odstranit](#DataDeletionDetectionPolicy) části.

### <a name="using-queries-to-shape-indexed-data"></a>Pomocí dotazů na obrazec indexované dat
Můžete zadat dotaz Cosmos DB zploštění vnořené vlastnosti nebo pole, vlastnosti projektu JSON a filtrování dat pro indexování. 

Příklad dokumentu:

    {
        "userId": 10001,
        "contact": {
            "firstName": "andy",
            "lastName": "hoh"
        },
        "company": "microsoft",
        "tags": ["azure", "documentdb", "search"]
    }

Dotaz filtru:

    SELECT * FROM c WHERE c.company = "microsoft" and c._ts >= @HighWaterMark ORDER BY c._ts

Vyrovnání dotazu:

    SELECT c.id, c.userId, c.contact.firstName, c.contact.lastName, c.company, c._ts FROM c WHERE c._ts >= @HighWaterMark ORDER BY c._ts
    
    
Projekce dotazu:

    SELECT VALUE { "id":c.id, "Name":c.contact.firstName, "Company":c.company, "_ts":c._ts } FROM c WHERE c._ts >= @HighWaterMark ORDER BY c._ts


Pole sloučení dotaz:

    SELECT c.id, c.userId, tag, c._ts FROM c JOIN tag IN c.tags WHERE c._ts >= @HighWaterMark ORDER BY c._ts

<a name="CreateIndex"></a>
## <a name="step-2-create-an-index"></a>Krok 2: Vytvoření indexu
Pokud již nemáte, vytvořte cílový index Azure Search. Můžete vytvořit index pomocí [portál Azure uživatelského rozhraní](search-create-index-portal.md), [vytvořit Index REST API](/rest/api/searchservice/create-index) nebo [Index – třída](/dotnet/api/microsoft.azure.search.models.index).

Následující příklad vytvoří index s polem id a popis:

    POST https://[service name].search.windows.net/indexes?api-version=2016-09-01
    Content-Type: application/json
    api-key: [Search service admin key]

    {
       "name": "mysearchindex",
       "fields": [{
         "name": "id",
         "type": "Edm.String",
         "key": true,
         "searchable": false
       }, {
         "name": "description",
         "type": "Edm.String",
         "filterable": false,
         "sortable": false,
         "facetable": false,
         "suggestions": true
       }]
     }

Zajistěte, aby schéma cílový index kompatibilní s schéma dokumentů JSON zdroj nebo výstup projekce vašeho vlastního dotazu.

> [!NOTE]
> Pro dělené kolekce je výchozí dokument klíč Cosmos DB `_rid` vlastností, která se získá přejmenován na `rid` ve službě Azure Search. Navíc Cosmos DB na `_rid` hodnoty obsahovat znaky, které jsou v Azure Search klíče neplatné. Z tohoto důvodu `_rid` hodnoty jsou kódováním Base64.
> 
> 

### <a name="mapping-between-json-data-types-and-azure-search-data-types"></a>Mapování mezi JSON datové typy a typy dat vyhledávání systému Azure
| JSON DATOVÉHO TYPU | TYPY POLÍ KOMPATIBILNÍ CÍLOVÝ INDEX |
| --- | --- |
| BOOL |Edm.Boolean Edm.String |
| Čísla, která vypadat podobně jako celá čísla |Edm.String Edm.Int32, Edm.Int64, |
| Čísla této vypadají plovoucí body |Edm.Double Edm.String |
| Řetězec |Edm.String |
| Pole jednoduchých typů, například ["a", "b", "c"] |Collection(Edm.String) |
| Řetězce, které vypadají podobně jako kalendářní data |Edm.DateTimeOffset Edm.String |
| GeoJSON objekty, například {"typ": "Místo", "coordinates": [dlouhý a lat]} |Edm.GeographyPoint |
| Jiné objekty JSON |neuvedeno |

<a name="CreateIndexer"></a>
## <a name="step-3-create-an-indexer"></a>Krok 3: Vytvořením indexeru.

Jakmile byly vytvořeny index a zdroj dat, jste připraveni vytvořit indexeru:

    POST https://[service name].search.windows.net/indexers?api-version=2016-09-01
    Content-Type: application/json
    api-key: [admin key]

    {
      "name" : "mydocdbindexer",
      "dataSourceName" : "mydocdbdatasource",
      "targetIndexName" : "mysearchindex",
      "schedule" : { "interval" : "PT2H" }
    }

Indexer se spustí každé dvě hodiny (interval plánování je nastavena na "PT2H"). Pokud chcete spustit indexer každých 30 minut, nastavte interval, který "PT30M". Nejkratší podporovaný interval je 5 minut. Plán je volitelné - li tento parametr vynechán, indexer se spustí pouze po jeho vytvoření. Indexer na vyžádání je však můžete spustit kdykoli.   

Další informace o rozhraní API Indexer vytvořit, podívejte se na [vytvořit Indexer](https://docs.microsoft.com/rest/api/searchservice/create-indexer).

<a id="RunIndexer"></a>
### <a name="running-indexer-on-demand"></a>Spuštění indexeru na vyžádání
Kromě spuštění pravidelně podle plánu, lze indexer také vyvolat na vyžádání:

    POST https://[service name].search.windows.net/indexers/[indexer name]/run?api-version=2016-09-01
    api-key: [Search service admin key]

> [!NOTE]
> Při spuštění rozhraní API vrátí úspěšně, bylo naplánováno vyvolání indexeru, ale vlastní zpracování probíhá asynchronně. 

Můžete sledovat stav indexer v portálu nebo pomocí získat Indexer stav rozhraní API, které jsme popisují další. 

<a name="GetIndexerStatus"></a>
### <a name="getting-indexer-status"></a>Získání stavu indexeru
Můžete načíst historii stavu a spuštění indexeru:

    GET https://[service name].search.windows.net/indexers/[indexer name]/status?api-version=2016-09-01
    api-key: [Search service admin key]

Odpověď obsahuje celkový stav indexer, indexer poslední (nebo probíhající) volání a historii poslední indexer volání.

    {
        "status":"running",
        "lastResult": {
            "status":"success",
            "errorMessage":null,
            "startTime":"2014-11-26T03:37:18.853Z",
            "endTime":"2014-11-26T03:37:19.012Z",
            "errors":[],
            "itemsProcessed":11,
            "itemsFailed":0,
            "initialTrackingState":null,
            "finalTrackingState":null
         },
        "executionHistory":[ {
            "status":"success",
             "errorMessage":null,
            "startTime":"2014-11-26T03:37:18.853Z",
            "endTime":"2014-11-26T03:37:19.012Z",
            "errors":[],
            "itemsProcessed":11,
            "itemsFailed":0,
            "initialTrackingState":null,
            "finalTrackingState":null
        }]
    }

Historie provádění obsahuje až 50 poslední dokončené spuštěních, které jsou seřazeny v opačném chronologickém pořadí (takže nejnovější provádění bude první v odpovědi).

<a name="DataChangeDetectionPolicy"></a>
## <a name="indexing-changed-documents"></a>Indexování změněné dokumenty
Účelem zásady detekce změn dat je efektivně identifikaci položek změněná data. V současné době je pouze podporovaných zásad `High Water Mark` pomocí zásad `_ts` vlastnost (časové razítko) poskytované Cosmos databáze, které se určuje takto:

    {
        "@odata.type" : "#Microsoft.Azure.Search.HighWaterMarkChangeDetectionPolicy",
        "highWaterMarkColumnName" : "_ts"
    }

Pomocí této zásady je vhodné zajistit indexer dobrý výkon. 

Pokud používáte vlastní dotaz, ujistěte se, že `_ts` vlastnost se promítá v dotazu.

<a name="IncrementalProgress"></a>
### <a name="incremental-progress-and-custom-queries"></a>Přírůstkové průběh a vlastní dotazy
Přírůstkové průběh během indexování zajistí, že pokud indexer provádění přeruší přechodných chyb nebo provádění časový limit, indexeru můžete vyzvedávat kde bylo přerušeno při příštím spuštění, aniž by museli znovu indexu celou kolekci od začátku. To je zvlášť důležité při indexování rozsáhlých kolekcí. 

Povolit přírůstkové průběh při použití vlastního dotazu, ujistěte se, že váš dotaz řadí výsledky podle `_ts` sloupce. To umožňuje pravidelné kontroly směřující která Azure Search používá k zajištění přírůstkové průběh v případě selhání.   

V některých případech, i pokud dotaz obsahuje `ORDER BY [collection alias]._ts` klauzuli vyhledávání systému Azure nemusí odvození že dotaz je seřazené podle `_ts`. Se dá zjistit Azure Search, výsledky jsou seřazené podle pomocí `assumeOrderByHighWaterMarkColumn` vlastnosti konfigurace. Pokud chcete zadat Tento pomocný, vytvořit nebo aktualizovat indexer následujícím způsobem: 

    {
     ... other indexer definition properties
     "parameters" : {
            "configuration" : { "assumeOrderByHighWaterMarkColumn" : true } }
    } 

<a name="DataDeletionDetectionPolicy"></a>
## <a name="indexing-deleted-documents"></a>Indexování odstranit dokumenty
Při odstranění řádků z kolekce, obvykle chcete odstranit tyto řádky z také indexu vyhledávání. Účelem zásady detekce odstranění dat je efektivně identifikaci položek odstraněná data. V současné době je pouze podporovaných zásad `Soft Delete` zásad (odstranění označeno příznak nějaká), která je určena následujícím způsobem:

    {
        "@odata.type" : "#Microsoft.Azure.Search.SoftDeleteColumnDeletionDetectionPolicy",
        "softDeleteColumnName" : "the property that specifies whether a document was deleted",
        "softDeleteMarkerValue" : "the value that identifies a document as deleted"
    }

Pokud používáte vlastní dotaz, ujistěte se, že vlastnost odkazuje `softDeleteColumnName` se promítá v dotazu.

Následující příklad vytvoří zdroj dat s zásadu obnovitelného odstranění:

    POST https://[Search service name].search.windows.net/datasources?api-version=2016-09-01
    Content-Type: application/json
    api-key: [Search service admin key]

    {
        "name": "mydocdbdatasource",
        "type": "documentdb",
        "credentials": {
            "connectionString": "AccountEndpoint=https://myDocDbEndpoint.documents.azure.com;AccountKey=myDocDbAuthKey;Database=myDocDbDatabaseId"
        },
        "container": { "name": "myDocDbCollectionId" },
        "dataChangeDetectionPolicy": {
            "@odata.type": "#Microsoft.Azure.Search.HighWaterMarkChangeDetectionPolicy",
            "highWaterMarkColumnName": "_ts"
        },
        "dataDeletionDetectionPolicy": {
            "@odata.type": "#Microsoft.Azure.Search.SoftDeleteColumnDeletionDetectionPolicy",
            "softDeleteColumnName": "isDeleted",
            "softDeleteMarkerValue": "true"
        }
    }

## <a name="NextSteps"></a>Další kroky
Blahopřejeme! Jste se naučili integrace Azure Cosmos DB služby Azure Search pomocí indexeru pro Cosmos DB.

* Informace o další informace o databázi Cosmos Azure najdete v tématu [stránku služby Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/).
* Další informace o další informace o službě Azure Search najdete v tématu [stránku služby vyhledávání](https://azure.microsoft.com/services/search/).
