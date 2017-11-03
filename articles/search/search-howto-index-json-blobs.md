---
title: "Indexování objekty BLOB JSON s indexer objektu blob Azure Search"
description: "Indexování objekty BLOB JSON s indexer objektu blob Azure Search"
services: search
documentationcenter: 
author: chaosrealm
manager: pablocas
editor: 
ms.assetid: 57e32e51-9286-46da-9d59-31884650ba99
ms.service: search
ms.devlang: rest-api
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 09/07/2017
ms.author: eugenesh
ms.openlocfilehash: bf4d3a517e1308a142d21cffff64f3c6e104eb62
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="indexing-json-blobs-with-azure-search-blob-indexer"></a>Indexování objekty BLOB JSON s indexer objektu blob Azure Search
Tento článek ukazuje, jak nakonfigurovat indexer Azure Search objektů blob k extrahování obsahu strukturovaných z objektů BLOB, které obsahují JSON.

## <a name="scenarios"></a>Scénáře
Ve výchozím nastavení [indexer objektu blob Azure Search](search-howto-indexing-azure-blob-storage.md) analyzuje objekty BLOB JSON jako jeden blok textu. Často budete chtít zachovat strukturu dokumentů JSON. Například uděleno dokumentu JSON

    {
        "article" : {
             "text" : "A hopefully useful article explaining how to parse JSON blobs",
            "datePublished" : "2016-04-13"
            "tags" : [ "search", "storage", "howto" ]    
        }
    }

můžete k analýze do dokumentu sady Azure Search pomocí pole "značky", "datePublished" a "text".

Případně, pokud obsahovat objektů blob **pole objektů JSON**, můžete každý element pole se samostatný dokument Azure Search. Například zadaný objekt blob se tento text JSON:  

    [
        { "id" : "1", "text" : "example 1" },
        { "id" : "2", "text" : "example 2" },
        { "id" : "3", "text" : "example 3" }
    ]

jej můžete naplnit index Azure Search s tři samostatné dokumenty, každý s pole "id" a "text".

> [!IMPORTANT]
> Pole JSON analýza funkce je aktuálně ve verzi preview. Je k dispozici pouze v rozhraní REST API pomocí verze **2016-09-01-Preview**. Mějte na paměti, verzi preview rozhraní API jsou určené pro testování a vyhodnocení a neměl by se používat v produkčním prostředí.
>
>

## <a name="setting-up-json-indexing"></a>Nastavení indexování JSON
Indexování objekty BLOB JSON je podobná extrakce běžný dokument. Nejprve vytvořte zdroj dat přesně stejně jako za normálních okolností: 

    POST https://[service name].search.windows.net/datasources?api-version=2016-09-01
    Content-Type: application/json
    api-key: [admin key]

    {
        "name" : "my-blob-datasource",
        "type" : "azureblob",
        "credentials" : { "connectionString" : "DefaultEndpointsProtocol=https;AccountName=<account name>;AccountKey=<account key>;" },
        "container" : { "name" : "my-container", "query" : "optional, my-folder" }
    }   

Poté vytvořte index vyhledávání cíl, pokud jste ještě nemáte. 

Nakonec vytvořte indexer a nastavte `parsingMode` parametru `json` (do indexu každý objekt blob jako jedním dokumentem) nebo `jsonArray` (Pokud objektů BLOB obsahovat pole JSON a je třeba každý element pole jsou považovány za samostatný dokument):

    POST https://[service name].search.windows.net/indexers?api-version=2016-09-01
    Content-Type: application/json
    api-key: [admin key]

    {
      "name" : "my-json-indexer",
      "dataSourceName" : "my-blob-datasource",
      "targetIndexName" : "my-target-index",
      "schedule" : { "interval" : "PT2H" },
      "parameters" : { "configuration" : { "parsingMode" : "json" } }
    }

V případě potřeby použijte **pole mapování** a vyberte vlastnosti Zdrojový dokument JSON používaných k naplnění indexu vyhledávání cíl, jak je znázorněno v následujícím oddílu.

> [!IMPORTANT]
> Při použití `json` nebo `jsonArray` analýzy režimu Azure Search předpokládá, že všechny objekty BLOB ve zdroji dat obsahovat JSON. Pokud potřebujete podporovat směs JSON a jiný JSON objekty BLOB ve stejném datovém zdroji, dejte nám vědět na [našeho webu UserVoice](https://feedback.azure.com/forums/263029-azure-search).
>
>

## <a name="using-field-mappings-to-build-search-documents"></a>Použití mapování polí k sestavení vyhledávání dokumentů
V současné době Azure Search nemůže indexovat libovolné dokumenty JSON přímo, protože podporuje jenom primitivní datové typy, řetězce pole a GeoJSON body. Můžete však použít **pole mapování** a vyberte části dokumentu JSON "navýšení" je do nejvyšší úrovně pole hledání dokumentu. Další informace o základní informace o mapování pole najdete v tématu [mapování polí indexer Azure Search přemostění rozdíly mezi zdroje dat a indexy vyhledávání](search-indexer-field-mappings.md).

Vracející se zpět do dokumentu JSON náš příklad:

    {
        "article" : {
             "text" : "A hopefully useful article explaining how to parse JSON blobs",
            "datePublished" : "2016-04-13"
            "tags" : [ "search", "storage", "howto" ]    
        }
    }

Řekněme, že máte index vyhledávání s následující pole: `text` typu `Edm.String`, `date` typu `Edm.DateTimeOffset`, a `tags` typu `Collection(Edm.String)`. Chcete-li mapování vaše struktury JSON na požadovaný tvar, použijte následující mapování polí:

    "fieldMappings" : [
        { "sourceFieldName" : "/article/text", "targetFieldName" : "text" },
        { "sourceFieldName" : "/article/datePublished", "targetFieldName" : "date" },
        { "sourceFieldName" : "/article/tags", "targetFieldName" : "tags" }
      ]

Názvy polí zdroje v mapování jsou určeny pomocí [JSON ukazatel](http://tools.ietf.org/html/rfc6901) zápis. Začínat lomítkem odkazovat na kořen dokumentu JSON a potom vyberte požadované vlastnosti (na libovolné úroveň vnoření) pomocí dopředného cesty oddělené lomítko.

Můžete také odkazovat na pole jednotlivé elementy pomocí index počítaný od nuly. Například vyberte první prvek pole "značky" z výše uvedeném příkladu, použijte mapování polí takto:

    { "sourceFieldName" : "/article/tags/0", "targetFieldName" : "firstTag" }

> [!NOTE]
> Pokud název pole zdroje v cestě mapování pole odkazuje na vlastnost, která neexistuje ve formátu JSON, že mapování přeskočen bez chyby. To se provádí tak, aby podporujeme dokumentů s zvolte jiné schéma (což je běžný případ použití). Protože není k dispozici žádné ověření, budete muset Ujistěte se, aby se zabránilo překlepům specifikací mapování pole.
>
>

Pokud vaše dokumenty JSON obsahovat pouze jednoduché vlastnosti nejvyšší úrovně, možná nebudete potřebovat mapování polí vůbec. Například pokud vaše struktury JSON vypadá to, nejvyšší úrovně vlastnosti "text", "datePublished" a "značky" přímo namapuje na odpovídající pole v indexu vyhledávání:

    {
       "text" : "A hopefully useful article explaining how to parse JSON blobs",
       "datePublished" : "2016-04-13"
       "tags" : [ "search", "storage", "howto" ]    
     }

Tady je kompletní indexer datové části s mapování polí:

    POST https://[service name].search.windows.net/indexers?api-version=2016-09-01
    Content-Type: application/json
    api-key: [admin key]

    {
      "name" : "my-json-indexer",
      "dataSourceName" : "my-blob-datasource",
      "targetIndexName" : "my-target-index",
      "schedule" : { "interval" : "PT2H" },
      "parameters" : { "configuration" : { "parsingMode" : "json" } },
      "fieldMappings" : [
        { "sourceFieldName" : "/article/text", "targetFieldName" : "text" },
        { "sourceFieldName" : "/article/datePublished", "targetFieldName" : "date" },
        { "sourceFieldName" : "/article/tags", "targetFieldName" : "tags" }
        ]
    }

## <a name="indexing-nested-json-arrays"></a>Indexování vnořená pole JSON
Co dělat, když chcete indexu pole objektů JSON, ale tohoto pole je vnořený někde v tomto dokumentu? Můžete si vybrat vlastností, které obsahuje pole pomocí `documentRoot` vlastnosti konfigurace. Pokud například objektů BLOB vypadat takto:

    {
        "level1" : {
            "level2" : [
                { "id" : "1", "text" : "Use the documentRoot property" },
                { "id" : "2", "text" : "to pluck the array you want to index" },
                { "id" : "3", "text" : "even if it's nested inside the document" }  
            ]
        }
    }

Pomocí této konfigurace můžete indexu pole obsažené ve `level2` vlastnost:

    {
        "name" : "my-json-array-indexer",
        ... other indexer properties
        "parameters" : { "configuration" : { "parsingMode" : "jsonArray", "documentRoot" : "/level1/level2" } }
    }

## <a name="help-us-make-azure-search-better"></a>Pomozte nám vylepšit Azure Search
Pokud máte žádosti o funkce nebo vylepšení nápady, oslovení nám na našem [UserVoice lokality](https://feedback.azure.com/forums/263029-azure-search/).
