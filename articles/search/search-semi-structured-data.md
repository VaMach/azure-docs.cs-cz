---
title: "Vyhledávání částečně strukturovaných dat v cloudu Azure storage"
description: "Vyhledávání dat částečně strukturovaných blob pomocí Azure Search."
author: roygara
manager: timlt
ms.service: search
ms.topic: tutorial
ms.date: 10/12/2017
ms.author: v-rogara
ms.custom: mvc
ms.openlocfilehash: ea57fa35f09299f95cdfd3c11b44657d35972295
ms.sourcegitcommit: e6029b2994fa5ba82d0ac72b264879c3484e3dd0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/24/2017
---
# <a name="search-semi-structured-data-in-cloud-storage"></a>Vyhledávání částečně strukturovaných dat do cloudového úložiště

Tento kurz série dvě části zjistěte, jak k vyhledání částečně strukturovaná i nestrukturovaná data používání služby Azure search. V tomto kurzu se dozvíte, jak částečně strukturovaných dat, jako je například formát JSON, ukládají do objektů BLOB Azure. Částečně strukturovaných dat obsahuje značky nebo označení, které oddělení obsahu v rámci data. Se liší od strukturovaných dat v tom, že není oficiálně strukturovaná podle datový model, jako je například schéma relační databáze.

V této části nabídneme postupy:

> [!div class="checklist"]
> * Vytvořit a naplnit index uvnitř služby Azure Search
> * Používat službu Azure Search k vyhledávání v indexu

> [!NOTE]
> "Podpora pole JSON je funkce preview ve službě Azure Search. Není aktuálně k dispozici na portálu. Z tohoto důvodu že používáme rozhraní REST API, která poskytuje tuto funkci a nástroj klienta REST pro volání rozhraní API ve verzi preview."

## <a name="prerequisites"></a>Požadavky

Pro absolvování tohoto kurzu potřebujete:
* Dokončení [předchozí kurzu](../storage/blobs/storage-unstructured-search.md)
    * Tento kurz používá úložiště účet vyhledávací služby a vytvořili v předchozí kurzu
* Nainstalujte klienta REST a pochopit, jak vytvořit požadavek HTTP


## <a name="set-up-the-rest-client"></a>Nastavení klienta REST

K dokončení tohoto kurzu potřebujete klienta REST. Pro účely tohoto kurzu používáme [Postman](https://www.getpostman.com/). Nebojte se, že pomocí jiného klienta REST, pokud jste již celý některá.

Po instalaci Postman, spusťte ji.

Pokud je to poprvé volání REST do Azure, zde je stručný úvod důležité součásti v tomto kurzu: metodu požadavku pro každé volání v tomto kurzu je "POST". Hlavička klíče jsou "Content-type" a "api-key." Hodnoty hlavičky klíče jsou "application/json" a "klíč správce" (zástupný symbol pro vyhledávání primární klíč je klíč správce) v uvedeném pořadí. Text je třeba umístit skutečný obsah volání. V závislosti na klienta, kterou používáte může být několik odchylek na tom, jak vytvořit dotaz, ale ty jsou základní informace.

  ![Částečně strukturovaných vyhledávání](media/search-semi-structured-data/postmanoverview.png)

Pro volání REST, zahrnuté v tomto kurzu se vyžaduje vaše vyhledávání klíč api-key. Můžete najít váš klíč api-key v části **klíče** uvnitř vaši službu vyhledávání. Tento klíč rozhraní api musí být v hlavičce každé volání rozhraní API (nahraďte "klíč správce" v předchozí snímek obrazovky s ním) v tomto kurzu přesměruje vám umožní provádět. Zachovat klíč, protože potřebujete pro každé volání.

  ![Částečně strukturovaných vyhledávání](media/search-semi-structured-data/keys.png)

## <a name="download-the-sample-data"></a>Stáhněte si ukázková data

Ukázka datové sady připravený pro vás. **Stáhněte si [klinické zkušební verze json.zip](https://github.com/Azure-Samples/storage-blob-integration-with-cdn-search-hdi/raw/master/clinical-trials-json.zip)**  a rozbalte ho do vlastní složky.

Obsažené v ukázce jsou například soubory JSON, které byly původně textové soubory získané z [clinicaltrials.gov](https://clinicaltrials.gov/ct2/results). Jsme jejich převodu do formátu JSON pro usnadnění vaší práce.

## <a name="log-in-to-azure"></a>Přihlaste se k Azure.

Přihlaste se k portálu [Azure Portal](http://portal.azure.com).

## <a name="upload-the-sample-data"></a>Nahrát ukázková data

Na portálu Azure přejděte zpět na účtu úložiště vytvořeném v [předchozí kurzu](../storage/blobs/storage-unstructured-search.md). Otevřete **data** kontejner a klikněte na **nahrát**.

Klikněte na tlačítko **Upřesnit**, zadejte "klinické – zkušební verze json" a potom odeslat všechny stažené soubory JSON.

  ![Částečně strukturovaných vyhledávání](media/search-semi-structured-data/clinicalupload.png)

Po dokončení nahrávání souborů by se zobrazit v vlastní podsložky uvnitř kontejneru data.

## <a name="connect-your-search-service-to-your-container"></a>Připojit k vaší kontejneru služby search

Používáme Postman provádět tři volání rozhraní API pro vaši službu vyhledávání k vytvoření zdroje dat a index, indexer. Zdroj dat zahrnuje ukazatel na účtu úložiště a vaše data JSON. Při načítání dat, díky služby search připojení.

Musí obsahovat řetězec dotazu **rozhraní api-version = 2016-09-01-Preview** a každé volání by měla vrátit **201 – vytvořeno**. Všeobecně dostupná verze rozhraní api ještě nemá možnost pro zpracování formátu json jako jsonArray, aktuálně nemá pouze preview api-version.

Spusťte následující tři volání rozhraní API z vašeho klienta REST.

### <a name="create-a-datasource"></a>Vytvoření zdroje dat

Zdroj dat určuje, jaká data do indexu.

Koncový bod tohoto volání je `https://[service name].search.windows.net/datasources?api-version=2016-09-01-Preview`. Nahraďte `[service name]` s názvem služby search.

Pro toto volání potřebujete název účtu úložiště a klíč účtu úložiště. Klíč účtu úložiště najdete na portálu Azure v účtu úložiště **přístupové klíče**. Umístění je znázorněno na následujícím obrázku:

  ![Částečně strukturovaných vyhledávání](media/search-semi-structured-data/storagekeys.png)

Nezapomeňte nahradit `[storage account name]` a `[storage account key]` v těle volání před provedením volání.

```json
{
    "name" : "clinical-trials-json",
    "type" : "azureblob",
    "credentials" : { "connectionString" : "DefaultEndpointsProtocol=https;AccountName=[storage account name];AccountKey=[storage account key];" },
    "container" : { "name" : "data", "query" : "clinical-trials-json" }
}
```

Odpověď by měla vypadat podobně jako:

```json
{
    "@odata.context": "https://exampleurl.search.windows.net/$metadata#datasources/$entity",
    "@odata.etag": "\"0x8D505FBC3856C9E\"",
    "name": "clinical-trials-json",
    "description": null,
    "type": "azureblob",
    "subtype": null,
    "credentials": {
        "connectionString": "DefaultEndpointsProtocol=https;AccountName=[mystorageaccounthere];AccountKey=[[myaccountkeyhere]]];"
    },
    "container": {
        "name": "data",
        "query": "clinical-trials-json"
    },
    "dataChangeDetectionPolicy": null,
    "dataDeletionDetectionPolicy": null
}
```

### <a name="create-an-index"></a>Vytvoření indexu
    
Druhé volání rozhraní API vytváří index. Index určuje všechny parametry a jejich atributy.

Adresa URL pro toto volání je `https://[service name].search.windows.net/indexes?api-version=2016-09-01-Preview`. Nahraďte `[service name]` s názvem služby search.

Nejprve nahraďte adresu URL. Zkopírujte a vložte následující kód do vaší textu a spusťte dotaz.

```json
{
  "name": "clinical-trials-json-index",  
  "fields": [
  {"name": "FileName", "type": "Edm.String", "searchable": false, "retrievable": true, "facetable": false, "filterable": false, "sortable": true},
  {"name": "Description", "type": "Edm.String", "searchable": true, "retrievable": false, "facetable": false, "filterable": false, "sortable": false},
  {"name": "MinimumAge", "type": "Edm.Int32", "searchable": false, "retrievable": true, "facetable": true, "filterable": true, "sortable": true},
  {"name": "Title", "type": "Edm.String", "searchable": true, "retrievable": true, "facetable": false, "filterable": true, "sortable": true},
  {"name": "URL", "type": "Edm.String", "searchable": false, "retrievable": false, "facetable": false, "filterable": false, "sortable": false},
  {"name": "MyURL", "type": "Edm.String", "searchable": false, "retrievable": true, "facetable": false, "filterable": false, "sortable": false},
  {"name": "Gender", "type": "Edm.String", "searchable": false, "retrievable": true, "facetable": true, "filterable": true, "sortable": false},
  {"name": "MaximumAge", "type": "Edm.Int32", "searchable": false, "retrievable": true, "facetable": true, "filterable": true, "sortable": true},
  {"name": "Summary", "type": "Edm.String", "searchable": true, "retrievable": true, "facetable": false, "filterable": false, "sortable": false},
  {"name": "NCTID", "type": "Edm.String", "key": true, "searchable": true, "retrievable": true, "facetable": false, "filterable": true, "sortable": true},
  {"name": "Phase", "type": "Edm.String", "searchable": false, "retrievable": true, "facetable": true, "filterable": true, "sortable": false},
  {"name": "Date", "type": "Edm.String", "searchable": false, "retrievable": true, "facetable": false, "filterable": false, "sortable": true},
  {"name": "OverallStatus", "type": "Edm.String", "searchable": false, "retrievable": true, "facetable": true, "filterable": true, "sortable": false},
  {"name": "OrgStudyId", "type": "Edm.String", "searchable": true, "retrievable": true, "facetable": false, "filterable": true, "sortable": false},
  {"name": "HealthyVolunteers", "type": "Edm.String", "searchable": false, "retrievable": true, "facetable": true, "filterable": true, "sortable": false},
  {"name": "Keywords", "type": "Collection(Edm.String)", "searchable": true, "retrievable": true, "facetable": true, "filterable": false, "sortable": false},
  {"name": "metadata_storage_last_modified", "type":"Edm.DateTimeOffset", "searchable": false, "retrievable": true, "filterable": true, "sortable": false},
  {"name": "metadata_storage_size", "type":"Edm.String", "searchable": false, "retrievable": true, "filterable": true, "sortable": false},
  {"name": "metadata_content_type", "type":"Edm.String", "searchable": true, "retrievable": true, "filterable": true, "sortable": false}
  ],
  "suggesters": [
  {
    "name": "sg",
    "searchMode": "analyzingInfixMatching",
    "sourceFields": ["Title"]
  }
  ]
}
```

Odpověď by měla vypadat podobně jako:

```json
{
    "@odata.context": "https://exampleurl.search.windows.net/$metadata#indexes/$entity",
    "@odata.etag": "\"0x8D505FC00EDD5FA\"",
    "name": "clinical-trials-json-index",
    "fields": [
        {
            "name": "FileName",
            "type": "Edm.String",
            "searchable": false,
            "filterable": false,
            "retrievable": true,
            "sortable": true,
            "facetable": false,
            "key": false,
            "indexAnalyzer": null,
            "searchAnalyzer": null,
            "analyzer": null,
            "synonymMaps": []
        },
        {
            "name": "Description",
            "type": "Edm.String",
            "searchable": true,
            "filterable": false,
            "retrievable": false,
            "sortable": false,
            "facetable": false,
            "key": false,
            "indexAnalyzer": null,
            "searchAnalyzer": null,
            "analyzer": null,
            "synonymMaps": []
        },
        ...
          "scoringProfiles": [],
    "defaultScoringProfile": null,
    "corsOptions": null,
    "suggesters": [],
    "analyzers": [],
    "tokenizers": [],
    "tokenFilters": [],
    "charFilters": []
}
```

### <a name="create-an-indexer"></a>Vytvořením indexeru.

Indexer zdroji dat se připojuje k cílový index vyhledávání a volitelně poskytuje plán, který chcete automatizovat aktualizace dat.

Adresa URL pro toto volání je `https://[service name].search.windows.net/indexers?api-version=2016-09-01-Preview`. Nahraďte `[service name]` s názvem služby search.

Nejprve nahraďte adresu URL. Zkopírujte a vložte následující kód do vaší textu a spusťte dotaz.

```json
{
  "name" : "clinical-trials-json-indexer",
  "dataSourceName" : "clinical-trials-json",
  "targetIndexName" : "clinical-trials-json-index",
  "parameters" : { "configuration" : { "parsingMode" : "jsonArray" } }
}
```

Odpověď by měla vypadat podobně jako:

```json
{
    "@odata.context": "https://exampleurl.search.windows.net/$metadata#indexers/$entity",
    "@odata.etag": "\"0x8D505FDE143D164\"",
    "name": "clinical-trials-json-indexer",
    "description": null,
    "dataSourceName": "clinical-trials-json",
    "targetIndexName": "clinical-trials-json-index",
    "schedule": null,
    "parameters": {
        "batchSize": null,
        "maxFailedItems": null,
        "maxFailedItemsPerBatch": null,
        "base64EncodeKeys": null,
        "configuration": {
            "parsingMode": "jsonArray"
        }
    },
    "fieldMappings": [],
    "enrichers": [],
    "disabled": null
}
```

## <a name="search-your-json-files"></a>Hledání souborů JSON

Teď, když vaši službu vyhledávání se připojil k vaší kontejner dat, můžete začít hledání souborů.

Otevřete portál Azure a vraťte se na vaši službu vyhledávání. Stejně jako jste to udělali v předchozím kurzu.

  ![Nestrukturovaných vyhledávání](media/search-semi-structured-data/indexespane.png)

### <a name="user-defined-metadata-search"></a>Metadata definovaná uživatelem vyhledávání

Jak před, data mohou být dotazována v mnoha různými způsoby: fulltextové vyhledávání, vlastnosti systému nebo metadata definovaná uživatelem. Vlastnosti systému i metadata definovaná uživatelem může vyhledávat pouze s `$select` parametr, pokud byly označeny jako **získat** během vytváření cílový index. Parametry v indexu nemohou být změněny po jejich vytvoření. Však lze přidat další parametry.

Příklad základního dotazu je `$select=Gender,metadata_storage_size`, což omezuje návratu do tyto dva parametry.

  ![Částečně strukturovaných vyhledávání](media/search-semi-structured-data/lastquery.png)

Jedná se například komplexnější dotaz `$filter=MinimumAge ge 30 and MaximumAge lt 75`, která vrací pouze výsledky, kde parametry MinimumAge je větší než nebo rovno 30 a MaximumAge je menší než 75.

  ![Částečně strukturovaných vyhledávání](media/search-semi-structured-data/metadatashort.png)

Pokud chcete experimentovat a vyzkoušet další dotazy, klidně Uděláte to tak. Vědět, které můžete použít logické operátory (a, nebo ne) a relační operátory (eq, ne, gt, lt, ge, le). Porovnání řetězců rozlišují velká a malá písmena.

`$filter` Parametr pracuje pouze s metadat, které byly označeny filtrovatelných při vytváření indexu.

## <a name="next-steps"></a>Další kroky

V tomto kurzu jste se naučili o vyhledávání částečně strukturovaných dat pomocí Azure search, jako například:

> [!div class="checklist"]
> * Vytvoření služby Azure Search pomocí REST API
> * Používat službu Azure Search k vyhledání vašeho kontejneru

Další informace o vyhledávání na tento odkaz.

> [!div class="nextstepaction"]
> [Indexování dokumentů v Azure Blob Storage](search-howto-indexing-azure-blob-storage.md)