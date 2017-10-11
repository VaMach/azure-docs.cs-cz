---
title: "Indexer operací (rozhraní API REST služby vyhledávání systému Azure: 2015-02-28-Preview) | Microsoft Docs"
description: "Indexer operací (rozhraní API REST služby vyhledávání systému Azure: 2015-02-28-Preview)"
services: search
documentationcenter: 
author: chaosrealm
manager: pablocas
editor: 
ms.assetid: 42b5f85c-8304-4bc7-8e1e-a9c76b8ca25a
ms.service: search
ms.devlang: rest-api
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 05/01/2017
ms.author: eugenesh
ms.openlocfilehash: 356ceb98106d080d8c24dedc3547bee33750156e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="indexer-operations-azure-search-service-rest-api-2015-02-28-preview"></a>Indexer operací (rozhraní API REST služby vyhledávání systému Azure: 2015-02-28-Preview)
> [!NOTE]
> Tento článek popisuje indexery ve [2015-02-28-Preview REST API](search-api-2015-02-28-preview.md). Tato verze rozhraní API přidá verze preview indexer Azure Blob Storage s extrakce dokumentu a indexer Azure Table Storage a dalších vylepšení. Rozhraní API podporuje také všeobecně dostupná indexery (GA), včetně indexery pro databáze SQL Azure, SQL Server na virtuálních počítačích Azure a Azure Cosmos DB.
> 
> 

## <a name="overview"></a>Přehled
Vyhledávání systému Azure můžete integrovat přímo s některé běžné zdroje dat, nemusíte napsat kód pro index vaše data. Pokud chcete nastavit tuto dobu, můžete volat rozhraní API služby Azure Search k vytváření a správě **indexery** a **zdroje dat**. 

**Indexer** je na prostředek, který připojí zdroje dat s indexy vyhledávání cíl. Indexer se používá následujícími způsoby: 

* Proveďte jednorázové kopii dat k naplnění indexu.
* Synchronizujte indexu se změnami ve zdroji dat podle plánu. Plán je součástí definice indexer.
* Volání na vyžádání se aktualizovat index podle potřeby. 

**Indexer** je užitečné, když chcete pravidelné aktualizace na index. Můžete nastavit plán vložené jako součást definice indexeru, nebo ji spustit na vyžádání pomocí [spustit Indexer](#RunIndexer). 

A **zdroj dat** Určuje, jaká data musí být indexován, přihlašovací údaje pro přístup k datům a zásady Povolit službě Azure Search efektivně identifikovat změny v datech (například upravit nebo odstranit řádky v tabulce databáze). Je definována jako prostředek nezávisle tak, aby ji můžete použít několik indexerů.

Aktuálně jsou podporovány následující zdroje dat:

* **Databáze Azure SQL** a **systému SQL Server na virtuálních počítačích Azure**. Cílové návodu, najdete v části [v tomto článku](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md). 
* **Azure Cosmos DB**. Cílové návodu, najdete v části [v tomto článku](search-howto-index-documentdb.md). 
* **Azure Blob Storage**, včetně dokumentů následující formáty: PDF, Microsoft Office (DOCX/DOC, XSLX nebo XLS, PPTX/PPT, MSG), HTML, XML, ZIP a prostý text soubory (včetně JSON). Cílové návodu, najdete v části [v tomto článku](search-howto-indexing-azure-blob-storage.md).
* **Azure Table Storage**. Cílové návodu, najdete v části [v tomto článku](search-howto-indexing-azure-tables.md).

Jsme chtěli přidání podpory pro další datové zdroje v budoucnu. Chcete-li z důvodu stanovení priorit tato rozhodnutí, zadejte svůj názor na [fóru pro zpětnou vazbu Azure Search](http://feedback.azure.com/forums/263029-azure-search).

V tématu [omezení služby](search-limits-quotas-capacity.md) pro maximální limit týkající se indexer a data prostředků zdroje.

## <a name="typical-usage-flow"></a>Tok typickému využití
Můžete vytvořit a spravovat indexery a zdroje dat prostřednictvím jednoduchých požadavků HTTP (POST, GET, PUT, DELETE) proti danou `data source` nebo `indexer` prostředků.

Nastavení automatické indexování se obvykle čtyři krocích:

1. Určete zdroj dat, který obsahuje data, která musí být indexován. Uvědomte si, že Azure Search nemusí podporovat všechny typy dat ve zdroji dat. V tématu [podporované datové typy](https://msdn.microsoft.com/library/azure/dn798938.aspx) seznamu.
2. Vytvoření indexu Azure Search, jejichž schéma je kompatibilní s datovým zdrojem.
3. Vytvoření zdroje dat Azure Search, jak je popsáno v [vytvořit zdroj dat](#CreateDataSource).
4. Vytvoření indexer Azure Search, jak je popsáno [vytvořit Indexer](#CreateIndexer).

Měli byste naplánovat na vytváření jeden indexeru pro každou kombinaci cílový index a datové zdroje. Může mít několik indexerů zápisu do stejné index a můžete znovu použít stejný zdroj dat pro několik indexerů. Indexer ale může využívat jenom jeden zdroj dat najednou a může zapisovat jenom jeden index. 

Po vytvoření indexer, můžete načíst pomocí stav jeho spuštění [získání stavu Indexer](#GetIndexerStatus) operaci. Můžete taky spustit indexer v jakékoli (místo nebo kromě spuštění pravidelně podle plánu) pomocí [spustit Indexer](#RunIndexer) operaci.

<!-- MSDN has 2 art files plus a API topic link list -->


## <a name="create-data-source"></a>Vytvoření zdroje dat
Ve službě Azure Search zdroj dat se používá s indexery, poskytuje informace o připojení pro aktualizace ad hoc nebo naplánovanou dat cílový index. Můžete vytvořit nový zdroj dat v rámci služby Azure Search pomocí požadavku HTTP POST.

    POST https://[service name].search.windows.net/datasources?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin key]

Alternativně můžete použít PUT a zadejte název zdroje dat v identifikátoru URI. Pokud zdroj dat neexistuje, bude vytvořen.

    PUT https://[service name].search.windows.net/datasources/[datasource name]?api-version=[api-version]

> [!NOTE]
> Maximální počet zdrojů dat, které jsou povoleny se liší podle cenové úrovně. Bezplatné služby umožňuje až 3 datové zdroje. Standardní služby umožňuje 50 datové zdroje. V tématu [omezení služby](search-limits-quotas-capacity.md) podrobnosti.
> 
> 

**Požadavek**

Je požadován pro všechny žádosti o služby protokol HTTPS. **Vytvořit zdroj dat** požadavek lze sestavit pomocí Metoda POST nebo PUT metody. Při použití POST, je nutné zadat název zdroje dat v těle žádosti spolu s definice zdroje dat. Pomocí PUT název je část adresy URL. Pokud zdroj dat neexistuje, vytvoří se. Pokud již existuje, je aktualizován na novou definici. 

Název zdroje dat musí být malá písmena, začínat písmenem nebo číslicí, mít žádné lomítka nebo tečky a být kratší než 128 znaků. Po spuštění název zdroje dat s písmenem nebo číslicí, může obsahovat zbytek název jakékoli písmeno, čísla a pomlčky, dokud nejsou po sobě jdoucí pomlčky. V tématu [pravidla po pojmenování](https://msdn.microsoft.com/library/azure/dn857353.aspx) podrobnosti.

`api-version` Je vyžadován. Aktuální verze je `2015-02-28`.

**Hlavičky požadavku**

Následující seznam popisuje hlavičky žádosti požadované a volitelné. 

* `Content-Type`: Vyžaduje se. Tuto možnost nastavíte na`application/json`
* `api-key`: Vyžaduje se. `api-key` Se používá k ověření požadavku na vaši službu vyhledávání. Je řetězcovou hodnotu, jedinečné pro vaši službu. **Vytvořit zdroj dat** musí zahrnovat požadavek `api-key` záhlaví nastavit klíče správce (na rozdíl od klíč dotazů). 

Budete také potřebovat názvu služby pro vytvoření adresy URL žádosti. Můžete získat název služby a `api-key` z řídicího panelu služby v [portálu Azure](https://portal.azure.com/). V tématu [vytvořte službu vyhledávání v portálu](search-create-service-portal.md) nápovědu navigace stránky.

<a name="CreateDataSourceRequestSyntax"></a>
**Syntaxe požadavku textu**

Text žádosti obsahuje definici zdroje dat, včetně typu zdroje dat, přihlašovací údaje číst data, jakož i volitelnými daty změnit zjišťování a zásady detekce odstranění dat, které se používají k identifikaci efektivně změnit ani odstranit dat ve zdroji dat při použití s pravidelně naplánované indexer. 

Syntaxe pro vytváření struktury datová část požadavku je následující. Ukázková žádost je k dispozici další na v tomto tématu.

    { 
        "name" : "Required for POST, optional for PUT. The name of the data source",
        "description" : "Optional. Anything you want, or nothing at all",
        "type" : "Required. Must be one of 'azuresql', 'documentdb', 'azureblob', or 'azuretable'",
        "credentials" : { "connectionString" : "Required. Connection string for your data source" },
        "container" : { "name" : "Required. The name of the table, collection, or blob container you wish to index" },
        "dataChangeDetectionPolicy" : { Optional. See below for details }, 
        "dataDeletionDetectionPolicy" : { Optional. See below for details }
    }

Žádost obsahuje následující vlastnosti: 

* `name`: Vyžaduje se. Název datového zdroje. Název zdroje dat musí pouze obsahovat malá písmena, číslice nebo pomlčky, nesmí začínat ani končit pomlčky a je omezený na 128 znaků.
* `description`: Volitelný popis. 
* `type`: Vyžaduje se. Musí být jeden z typů podporované datové zdroje:
  * `azuresql`-Azure SQL Database nebo SQL Server na virtuálních počítačích Azure
  * `documentdb`-DB azure Cosmos
  * `azureblob`-Azure Blob Storage
  * `azuretable`-Azure Table Storage
* `credentials`:
  * Požadované `connectionString` připojovací řetězec pro zdroj dat určuje vlastnost. Formát řetězce připojení závisí na typu zdroje dat: 
    * U Azure SQL je to obvykle připojovací řetězec systému SQL Server. Pokud používáte portál Azure k získání připojovacího řetězce, pomocí `ADO.NET connection string` možnost.
    * Pro Azure Cosmos DB, musí mít připojovací řetězec v následujícím formátu: `"AccountEndpoint=https://[your account name].documents.azure.com;AccountKey=[your account key];Database=[your database id]"`. Všechny hodnoty jsou povinné. Najdete je v [portál Azure](https://portal.azure.com/).  
    * U objektů Blob v Azure a Table Storage je to připojovací řetězec pro účet úložiště. Formát je popsán [zde](https://azure.microsoft.com/documentation/articles/storage-configure-connection-string/). Vyžaduje se protokol koncový bod HTTPS.  
* `container`, požadované: Určuje data do indexu pomocí `name` a `query` vlastnosti: 
  * `name`, požadované:
    * Azure SQL: Určuje tabulku nebo zobrazení. Můžete použít kvalifikovaný schématu názvy, například `[dbo].[mytable]`.
    * DocumentDB: Určuje kolekci. 
    * Azure Blob Storage: Určuje kontejner úložiště.
    * Azure Table Storage: Určuje název tabulky. 
  * `query`, volitelné:
    * DocumentDB: můžete zadat dotaz, který vyrovná libovolné rozložení dokumentu JSON do plochých schéma, které mohou indexu Azure Search.  
    * Azure Blob Storage: můžete zadat virtuální složky v rámci kontejneru objektů blob. Například cesta blobu `mycontainer/documents/blob.pdf`, `documents` lze použít jako virtuální složky.
    * Azure Table Storage: můžete zadat dotaz, který filtruje sadu řádků určených k importu.
    * Azure SQL: dotaz není podporovaný. Pokud potřebujete tuto funkci, prosím hlasovat pro [tento návrh](https://feedback.azure.com/forums/263029-azure-search/suggestions/9893490-support-user-provided-query-in-sql-indexer)
* Volitelné `dataChangeDetectionPolicy` a `dataDeletionDetectionPolicy` vlastnosti jsou popsány níže.

<a name="DataChangeDetectionPolicies"></a>
**Zásady detekce změn dat**

Účelem zásady detekce změn dat je efektivně identifikaci položek změněná data. Lišit v závislosti na typu zdroje dat podporované zásady. Následující oddíly popisují každou zásadu. 

***Zásady detekce změn horní meze*** 

Tuto zásadu používejte, pokud zdroj dat obsahuje sloupec nebo vlastnost, která splňuje následující kritéria:

* Vloží všechny zadejte hodnotu pro sloupec. 
* Všechny aktualizace k položce také změnit hodnotu pro sloupec. 
* Hodnota v tomto sloupci se zvyšuje s každé změně.
* Dotazy, které použít klauzuli filtru, který je podobný následujícímu `WHERE [High Water Mark Column] > [Current High Water Mark Value]` mohou být provedeny efektivně.

Například při použití Azure SQL datových zdrojů, indexované `rowversion` sloupec je ideální volbou pro použití s touto zásadou horní mez. 

Tyto zásady můžete nastavit následujícím způsobem:

    { 
        "@odata.type" : "#Microsoft.Azure.Search.HighWaterMarkChangeDetectionPolicy",
        "highWaterMarkColumnName" : "[a row version or last_updated column name]" 
    } 

Pokud používáte Azure Cosmos DB zdroje dat, je nutné použít `_ts` vlastnost poskytované Azure Cosmos DB. 

Při použití zdrojů dat objektů Blob v Azure, Azure Search automaticky používá horní meze změnit zásady detekce založené na objekt blob poslední úpravy časové razítko; nemusíte určit tato zásada sami.   

***Zásady detekce změn s integrací SQL***

Pokud vaše databáze SQL podporuje [sledování změn](https://msdn.microsoft.com/library/bb933875.aspx), doporučujeme používat SQL integrované změnit způsob sledování. Tato zásada umožňuje nejefektivnějším sledování změn a umožňuje Azure Search k identifikaci odstraněných řádků bez nutnosti mít sloupec explicitní "obnovitelného odstranění" ve schématu.

Integrované sledování změn je podporované počínaje následující verze databáze systému SQL Server: 

* SQL Server 2008 R2, pokud používáte systém SQL Server na virtuálních počítačích Azure.
* Azure SQL Database verze 12, pokud používáte Azure SQL Database.  

Když pomocí integrované sledování změn SQL zásad, nezadávejte zásadami detekce odstraňování oddělení dat – tato zásada má integrovanou podporu pro identifikaci odstranit řádky. 

Tuto zásadu lze použít pouze s tabulkami; nelze použít se zobrazeními. Budete muset povolit sledování změn pro tabulku, kterou používáte, abyste mohli používat tuto zásadu. V tématu [povolení a zakázání sledování změn](https://msdn.microsoft.com/library/bb964713.aspx) pokyny.    

Při vytváření struktury **vytvořit zdroj dat** požádat, SQL integrované zásady sledování změn, můžete nastavit následujícím způsobem:

    { 
        "@odata.type" : "#Microsoft.Azure.Search.SqlIntegratedChangeTrackingPolicy" 
    }

<a name="DataDeletionDetectionPolicies"></a>
**Zásady detekce odstranění dat**

Účelem zásady detekce odstranění dat je efektivně identifikaci položek odstraněná data. V současné době je pouze podporovaných zásad `Soft Delete` zásady, které umožňuje identifikaci odstraněné položky na základě hodnoty z `soft delete` sloupec nebo vlastnost ve zdroji dat. Tyto zásady můžete nastavit následujícím způsobem:

    { 
        "@odata.type" : "#Microsoft.Azure.Search.SoftDeleteColumnDeletionDetectionPolicy",
        "softDeleteColumnName" : "the column that specifies whether a row was deleted", 
        "softDeleteMarkerValue" : "the value that identifies a row as deleted" 
    }

> [!NOTE]
> Jsou podporovány pouze sloupce s řetězec, celé číslo nebo logické hodnoty. Hodnota použitá jako `softDeleteMarkerValue` musí být řetězec, i v případě, že odpovídající sloupec obsahuje celá čísla nebo logické hodnoty. Například pokud je hodnota, která se zobrazí ve zdroji dat 1, použít `"1"` jako `softDeleteMarkerValue`.    
> 
> 

<a name="CreateDataSourceRequestExamples"></a>
**Příklady text žádosti**

Pokud máte v úmyslu používat zdroj dat s indexer, který spouští podle plánu, tento příklad ukazuje, jak určit zásady detekce změn a odstranění: 

    { 
        "name" : "asqldatasource",
        "description" : "a description",
        "type" : "azuresql",
        "credentials" : { "connectionString" : "Server=tcp:....database.windows.net,1433;Database=...;User ID=...;Password=...;Trusted_Connection=False;Encrypt=True;Connection Timeout=30;" },
        "container" : { "name" : "sometable" },
        "dataChangeDetectionPolicy" : { "@odata.type" : "#Microsoft.Azure.Search.HighWaterMarkChangeDetectionPolicy", "highWaterMarkColumnName" : "RowVersion" }, 
        "dataDeletionDetectionPolicy" : { "@odata.type" : "#Microsoft.Azure.Search.SoftDeleteColumnDeletionDetectionPolicy", "softDeleteColumnName" : "IsDeleted", "softDeleteMarkerValue" : "true" }
    }

Pokud chcete použít zdroj dat pro jednorázové kopii dat, lze vynechat zásady:

    { 
        "name" : "asqldatasource",
        "description" : "anything you want, or nothing at all",
        "type" : "azuresql",
        "credentials" : { "connectionString" : "Server=tcp:....database.windows.net,1433;Database=...;User ID=...;Password=...;Trusted_Connection=False;Encrypt=True;Connection Timeout=30;" },
        "container" : { "name" : "sometable" }
    } 

**Odpověď**

Pro úspěšné žádosti: 201 – vytvořeno. 

<a name="UpdateDataSource"></a>

## <a name="update-data-source"></a>Aktualizovat zdroj dat
Můžete aktualizovat stávajícího zdroje dat pomocí požadavek HTTP PUT. Zadáte název zdroje dat pro aktualizaci v identifikátoru URI požadavku:

    PUT https://[service name].search.windows.net/datasources/[datasource name]?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin key]

`api-version` Je vyžadován. Aktuální verze je `2015-02-28`. [Azure verze rozhraní API služby Search](https://msdn.microsoft.com/library/azure/dn864560.aspx) má podrobnosti a další informace o alternativní verze.

`api-key` Musí být klíče správce (na rozdíl od klíč dotazů). Informace naleznete v sekci ověřování v [rozhraní API REST služby vyhledávání](https://msdn.microsoft.com/library/azure/dn798935.aspx) Další informace o klíčích. [Vytvoření služby Search na portálu](search-create-service-portal.md) vysvětluje, jak získat adresu URL služby a klíče vlastnosti používané v požadavku.

**Požadavek**

Syntaxe požadavku textu je stejná jako u [vytvořit zdroj dat požadavky](#CreateDataSourceRequestSyntax).

U stávajícího zdroje dat nelze aktualizovat některé vlastnosti. Například nelze změnit typ stávajícího zdroje dat.  

Pokud nechcete změnit připojovací řetězec pro stávajícího zdroje dat, můžete zadat literálové `<unchanged>` pro připojovací řetězec. To je užitečné v situacích, kde je potřeba aktualizovat data zdroje, ale nemáte pohodlný přístup do připojovacího řetězce, protože je to citlivým z hlediska zabezpečení dat.

**Odpověď**

Pro úspěšné žádosti: 201 – vytvořeno Pokud nový zdroj dat byl vytvořený a 204 ne obsahu Pokud stávajícího zdroje dat byla aktualizována.

<a name="ListDataSource"></a>

## <a name="list-data-sources"></a>Seznam zdrojů dat
**Zdroje dat seznamu** operace vrátí seznam zdrojů dat ve službě Azure Search. 

    GET https://[service name].search.windows.net/datasources?api-version=[api-version]
    api-key: [admin key]

`api-version` Je vyžadován. Aktuální verze je `2015-02-28`. 

`api-key` Musí být klíče správce (na rozdíl od klíč dotazů). Informace naleznete v sekci ověřování v [rozhraní API REST služby vyhledávání](https://msdn.microsoft.com/library/azure/dn798935.aspx) Další informace o klíčích. [Vytvoření služby Search na portálu](search-create-service-portal.md) vysvětluje, jak získat adresu URL služby a klíče vlastnosti používané v požadavku.

**Odpověď**

Pro úspěšné žádosti: 200 OK.

Tady je odpovědi na příkladu:

    {
      "value" : [
        {
          "name": "datasource1",
          "type": "azuresql",
          ... other data source properties
        }]
    }

Všimněte si, že můžete filtrovat odpovědi dolů pouze vlastnosti, které vás zajímají. Například pokud chcete pouze seznam názvy zdrojů dat, použijte prostředí OData `$select` dotazu možnost:

    GET /datasources?api-version=205-02-28&$select=name

V takovém případě odpověď z výše uvedeném příkladu by měly vypadat následovně: 

    {
      "value" : [ { "name": "datasource1" }, ... ]
    }

To je užitečné pro ušetří šířku pásma, pokud máte spoustu dalších zdrojů dat ve vyhledávací službě.

<a name="GetDataSource"></a>

## <a name="get-data-source"></a>Získat zdroj dat
**Získat zdroj dat** operaci získá definice zdroje dat z Azure Search.

    GET https://[service name].search.windows.net/datasources/[datasource name]?api-version=[api-version]
    api-key: [admin key]

`api-version` Je vyžadován. Aktuální verze je `2015-02-28`. 

`api-key` Musí být klíče správce (na rozdíl od klíč dotazů). Informace naleznete v sekci ověřování v [rozhraní API REST služby vyhledávání](https://msdn.microsoft.com/library/azure/dn798935.aspx) Další informace o klíčích. [Vytvoření služby Search na portálu](search-create-service-portal.md) vysvětluje, jak získat adresu URL služby a klíče vlastnosti používané v požadavku.

**Odpověď**

Stavový kód: 200 OK se vrátí pro úspěšné odpovědi.

Odpověď je podobná příklady v [požadavky příklad vytvoření zdroje dat](#CreateDataSourceRequestExamples): 

    { 
        "name" : "asqldatasource",
        "description" : "a description",
        "type" : "azuresql",
        "credentials" : { "connectionString" : "Server=tcp:....database.windows.net,1433;Database=...;User ID=...;Password=...;Trusted_Connection=False;Encrypt=True;Connection Timeout=30;" },
        "container" : { "name" : "sometable" },
        "dataChangeDetectionPolicy" : { 
            "@odata.type" : "#Microsoft.Azure.Search.HighWaterMarkChangeDetectionPolicy",
            "highWaterMarkColumnName" : "RowVersion" }, 
        "dataDeletionDetectionPolicy" : { 
            "@odata.type" : "#Microsoft.Azure.Search.SoftDeleteColumnDeletionDetectionPolicy",
            "softDeleteColumnName" : "IsDeleted", 
            "softDeleteMarkerValue" : "true" }
    }

> [!NOTE]
> Nenastavujte `Accept` hlavička požadavku na `application/json;odata.metadata=none` při volající toto rozhraní API jako tak způsobí, že `@odata.type` atribut vynechává z odpovědi a vy nebudete moci rozlišit mezi změny dat a dat odstranění zásady detekce různých typů. 
> 
> 

<a name="DeleteDataSource"></a>

## <a name="delete-data-source"></a>Odstranit zdroj dat
**Odstranit zdroj dat** operace odebere zdroj dat ze služby Azure Search.

    DELETE https://[service name].search.windows.net/datasources/[datasource name]?api-version=[api-version]
    api-key: [admin key]

> [!NOTE]
> Pokud žádné indexery referenční zdroje dat, který odstraňujete, se provede operaci odstranění. Ale tyto indexery přejde do chybového stavu při jeho příštím spuštění.  
> 
> 

`api-version` Je vyžadován. Aktuální verze je `2015-02-28`. 

`api-key` Musí být klíče správce (na rozdíl od klíč dotazů). Informace naleznete v sekci ověřování v [rozhraní API REST služby vyhledávání](https://msdn.microsoft.com/library/azure/dn798935.aspx) Další informace o klíčích. [Vytvoření služby Search na portálu](search-create-service-portal.md) vysvětluje, jak získat adresu URL služby a klíče vlastnosti používané v požadavku.

**Odpověď**

: Stav 204 ne obsahu je vrácen kód pro úspěšné odpovědi.

<a name="CreateIndexer"></a>

## <a name="create-indexer"></a>Vytvoření indexeru
Můžete vytvořit nový indexer v rámci služby Azure Search pomocí požadavku HTTP POST.

    POST https://[service name].search.windows.net/indexers?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin key]

Alternativně můžete použít PUT a zadejte název zdroje dat v identifikátoru URI. Pokud zdroj dat neexistuje, bude vytvořen.

    PUT https://[service name].search.windows.net/indexers/[indexer name]?api-version=[api-version]

> [!NOTE]
> Maximální počet indexery povoleno se liší podle cenové úrovně. Bezplatné služby umožňuje až 3 indexery. Standardní služby umožňuje 50 indexery. V tématu [omezení služby](search-limits-quotas-capacity.md) podrobnosti.
> 
> 

`api-version` Je vyžadován. Aktuální verze je `2015-02-28`. 

`api-key` Musí být klíče správce (na rozdíl od klíč dotazů). Informace naleznete v sekci ověřování v [rozhraní API REST služby vyhledávání](https://msdn.microsoft.com/library/azure/dn798935.aspx) Další informace o klíčích. [Vytvoření služby Search na portálu](search-create-service-portal.md) vysvětluje, jak získat adresu URL služby a klíče vlastnosti používané v požadavku.

<a name="CreateIndexerRequestSyntax"></a>
**Syntaxe požadavku textu**

Text žádosti obsahuje definici indexer, který určuje zdroje dat a cílový index pro indexování, jakož i volitelné indexování plán a parametry. 

Syntaxe pro vytváření struktury datová část požadavku je následující. Ukázková žádost je k dispozici další na v tomto tématu.

    { 
        "name" : "Required for POST, optional for PUT. The name of the indexer",
        "description" : "Optional. Anything you want, or null",
        "dataSourceName" : "Required. The name of an existing data source",
        "targetIndexName" : "Required. The name of an existing index",
        "schedule" : { Optional. See Indexing Schedule below. },
        "parameters" : { Optional. See Indexing Parameters below. },
        "fieldMappings" : { Optional. See Field Mappings below. },
        "disabled" : Optional boolean value indicating whether the indexer is disabled. False by default.  
    }

**Plán indexeru**

Indexer Volitelně můžete zadat plán. Pokud se nachází plánu, spustí se indexer pravidelně podle plánu. Plán má následující atributy:

* `interval`: Vyžaduje se. Doba trvání hodnotu, která určuje interval nebo období pro indexer se spustí. Nejkratší povolený interval je 5 minut; nejdelší je jeden den. Musí být naformátovaná jako hodnota "hodnoty doby podle" XSD (omezená podmnožina [ISO 8601 trvání](http://www.w3.org/TR/xmlschema11-2/#dayTimeDuration) hodnotu). Je tento vzor: `"P[nD][T[nH][nM]]"`. Příklady: `PT15M` pro každých 15 minut, `PT2H` pro každé 2 hodiny. 
* `startTime`: Vyžaduje se. Datetime UTC, pokud by se měl indexer spustit systémem. 

**Indexer parametry**

Indexer Volitelně můžete zadat několik parametrů, které ovlivňují své chování. Všechny parametry jsou volitelné.  

* `maxFailedItems`: Počet neúspěšně indexovaných spuštění indexeru považuje za selhání položek. Výchozí hodnota je 0. Vrátí informace o neúspěšné položky [získání stavu Indexer](#GetIndexerStatus) operaci. 
* `maxFailedItemsPerBatch`: Počet neúspěšně indexovaných spuštění indexeru považuje za selhání v každé dávce položek. Výchozí hodnota je 0.
* `base64EncodeKeys`: Určuje, zda dokument klíče budou kódování base-64. Vyhledávání systému Azure vynucuje omezení znaků, které můžou být v klíči dokumentu. Hodnoty v zdrojová data však může obsahovat znaky, které jsou neplatné. Pokud je potřeba tyto hodnoty jako dokument klíče indexu, může být tento příznak nastaven na hodnotu true. Výchozí hodnota je `false`.
* `batchSize`: Určuje počet položek, které čtou ze zdroje dat a indexované jako jeden batch za účelem zlepšení výkonu. Výchozí hodnota závisí na typu zdroje dat: je 1 000 pro Azure SQL a Azure Cosmos DB a 10 pro Azure Blob Storage.

**Mapování polí**

Mapování polí můžete použít k mapování název pole ve zdroji dat na jiný název pole v indexu cíl. Představte si třeba zdrojová tabulka s polem `_id`. Služba Azure Search neumožňuje pole název začíná podtržítkem, takže pole musí být přejmenován. To lze provést pomocí `fieldMappings` vlastnost indexeru následujícím způsobem: 

    "fieldMappings" : [ { "sourceFieldName" : "_id", "targetFieldName" : "id" } ] 

Můžete určit více mapování polí: 

    "fieldMappings" : [ 
        { "sourceFieldName" : "_id", "targetFieldName" : "id" },
        { "sourceFieldName" : "_timestamp", "targetFieldName" : "timestamp" },
     ]

Zdrojové a cílové názvy polí jsou velká a malá písmena.

<a name="FieldMappingFunctions"></a>
***Funkce mapování polí***

Mapování polí lze použít také k transformaci hodnoty polí zdroje pomocí *mapování funkce*.

V současné době podporuje pouze jeden tyto funkce: `jsonArrayToStringCollection`. Analyzuje pole, které obsahuje řetězec formátovaný jako pole JSON do pole Collection(Edm.String) v cílový index. Je určený pro použití se službou Azure SQL indexer na konkrétní vzhledem k tomu, že SQL nemá datový typ nativní kolekce. Můžete použít takto: 

    "fieldMappings" : [ { "sourceFieldName" : "tags", "mappingFunction" : { "name" : "jsonArrayToStringCollection" } } ] 

Například pokud zdrojové pole obsahuje řetězec `["red", "white", "blue"]`, pak cílové pole typu `Collection(Edm.String)` vyplní pomocí tří hodnot `"red"`, `"white"` a `"blue"`.

Všimněte si, že `targetFieldName` vlastnost je volitelná; Pokud je ponecháno out `sourceFieldName` hodnota se používá. 

<a name="CreateIndexerRequestExamples"></a>
**Příklady text žádosti**

Následující příklad vytvoří indexer, který kopíruje data z tabulky odkazuje `ordersds` zdroje dat pro `orders` index podle plánu, který začíná na 1 ledna 2015 UTC a spouští každou hodinu. Každé vyvolání indexeru bude úspěšné, pokud se nepodaří indexovat v každé dávce víc než 5 položek a maximálně 10 položek nepodaří indexovat celkem. 

    {
        "name" : "myindexer",
        "description" : "a cool indexer",
        "dataSourceName" : "ordersds",
        "targetIndexName" : "orders",
        "schedule" : { "interval" : "PT1H", "startTime" : "2015-01-01T00:00:00Z" },
        "parameters" : { "maxFailedItems" : 10, "maxFailedItemsPerBatch" : 5, "base64EncodeKeys": false }
    }

**Odpověď**

Pro úspěšné žádosti 201 – vytvořeno.

<a name="UpdateIndexer"></a>

## <a name="update-indexer"></a>Aktualizovat Indexer
Můžete aktualizovat existujícího indexeru pomocí požadavek HTTP PUT. Zadáte název indexeru při aktualizaci v identifikátoru URI požadavku:

    PUT https://[service name].search.windows.net/indexers/[indexer name]?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin key]

`api-version` Je vyžadován. Aktuální verze je `2015-02-28`. 

`api-key` Musí být klíče správce (na rozdíl od klíč dotazů). Informace naleznete v sekci ověřování v [rozhraní API REST služby vyhledávání](https://msdn.microsoft.com/library/azure/dn798935.aspx) Další informace o klíčích. [Vytvoření služby Search na portálu](search-create-service-portal.md) vysvětluje, jak získat adresu URL služby a klíče vlastnosti používané v požadavku.

**Požadavek**

Syntaxe požadavku textu je stejná jako u [vytvořit Indexer požadavky](#CreateIndexerRequestSyntax).

**Odpověď**

Pro úspěšné žádosti: 201 – vytvořeno Pokud byl nový indexer vytvořil a 204 ne obsahu Pokud existujícího indexeru byla aktualizována.

<a name="ListIndexers"></a>

## <a name="list-indexers"></a>Seznam indexery
**Seznamu indexery** operace vrátí seznam indexery ve službě Azure Search. 

    GET https://[service name].search.windows.net/indexers?api-version=[api-version]
    api-key: [admin key]


`api-version` Je vyžadován. Verze preview je `2015-02-28-Preview`. [Správa verzí Azure Search](https://msdn.microsoft.com/library/azure/dn864560.aspx) má podrobnosti a další informace o alternativní verze.

`api-key` Musí být klíče správce (na rozdíl od klíč dotazů). Informace naleznete v sekci ověřování v [rozhraní API REST služby vyhledávání](https://msdn.microsoft.com/library/azure/dn798935.aspx) Další informace o klíčích. [Vytvoření služby Search na portálu](search-create-service-portal.md) vysvětluje, jak získat adresu URL služby a klíče vlastnosti používané v požadavku.

**Odpověď**

Pro úspěšné žádosti: 200 OK.

Tady je odpovědi na příkladu:

    {
      "value" : [
      {
        "name" : "myindexer",
        "description" : "a cool indexer",
        "dataSourceName" : "ordersds",
        "targetIndexName" : "orders",
        ... other indexer properties
      }]
    }

Všimněte si, že můžete filtrovat odpovědi dolů pouze vlastnosti, které vás zajímají. Například pokud chcete pouze seznam názvů indexer, použijte prostředí OData `$select` dotazu možnost:

    GET /indexers?api-version=2014-10-20-Preview&$select=name

V takovém případě odpověď z výše uvedeném příkladu by měly vypadat následovně: 

    {
      "value" : [ { "name": "myindexer" } ]
    }

To je užitečné pro ušetří šířku pásma, pokud máte spoustu indexery ve službě vyhledávání.

<a name="GetIndexer"></a>

## <a name="get-indexer"></a>Získat indexeru
**Získat Indexer** operaci získá definici indexer z Azure Search.

    GET https://[service name].search.windows.net/indexers/[indexer name]?api-version=[api-version]
    api-key: [admin key]

`api-version` Je vyžadován. Verze preview je `2015-02-28-Preview`. 

`api-key` Musí být klíče správce (na rozdíl od klíč dotazů). Informace naleznete v sekci ověřování v [rozhraní API REST služby vyhledávání](https://msdn.microsoft.com/library/azure/dn798935.aspx) Další informace o klíčích. [Vytvoření služby Search na portálu](search-create-service-portal.md) vysvětluje, jak získat adresu URL služby a klíče vlastnosti používané v požadavku.

**Odpověď**

Stavový kód: 200 OK se vrátí pro úspěšné odpovědi.

Odpověď je podobná příklady v [požadavky příklad vytvoření Indexer](#CreateIndexerRequestExamples): 

    {
        "name" : "myindexer",
        "description" : "a cool indexer",
        "dataSourceName" : "ordersds",
        "targetIndexName" : "orders",
        "schedule" : { "interval" : "PT1H", "startTime" : "2015-01-01T00:00:00Z" },
        "parameters" : { "maxFailedItems" : 10, "maxFailedItemsPerBatch" : 5, "base64EncodeKeys": false }
    }


<a name="DeleteIndexer"></a>

## <a name="delete-indexer"></a>Odstranit Indexer
**Odstranit Indexer** operace odebere indexer ze služby Azure Search.

    DELETE https://[service name].search.windows.net/indexers/[indexer name]?api-version=[api-version]
    api-key: [admin key]

Při odstranění indexer spuštěních indexer probíhá v daném čase spustí dokončen, ale žádné další spuštěních bude naplánována s. Pokusí použít indexer neexistující způsobí stavový kód protokolu HTTP 404 nebyl nalezen. 

`api-version` Je vyžadován. Verze preview je `2015-02-28-Preview`. 

`api-key` Musí být klíče správce (na rozdíl od klíč dotazů). Informace naleznete v sekci ověřování v [rozhraní API REST služby vyhledávání](https://msdn.microsoft.com/library/azure/dn798935.aspx) Další informace o klíčích. [Vytvoření služby Search na portálu](search-create-service-portal.md) vysvětluje, jak získat adresu URL služby a klíče vlastnosti používané v požadavku.

**Odpověď**

: Stav 204 ne obsahu je vrácen kód pro úspěšné odpovědi.

<a name="RunIndexer"></a>

## <a name="run-indexer"></a>Spustit Indexer
Kromě spuštění pravidelně podle plánu, může být volána indexer na vyžádání prostřednictvím **spustit Indexer** operace: 

    POST https://[service name].search.windows.net/indexers/[indexer name]/run?api-version=[api-version]
    api-key: [admin key]

`api-version` Je vyžadován. Verze preview je `2015-02-28-Preview`. 

`api-key` Musí být klíče správce (na rozdíl od klíč dotazů). Informace naleznete v sekci ověřování v [rozhraní API REST služby vyhledávání](https://msdn.microsoft.com/library/azure/dn798935.aspx) Další informace o klíčích. [Vytvoření služby Search na portálu](search-create-service-portal.md) vysvětluje, jak získat adresu URL služby a klíče vlastnosti používané v požadavku.

**Odpověď**

Stavový kód: vrácen 202 platných pro úspěšné odpovědi.

<a name="GetIndexerStatus"></a>

## <a name="get-indexer-status"></a>Získat stav indexeru
**Získání stavu Indexer** operace načte aktuální stav a provádění historii indexer: 

    GET https://[service name].search.windows.net/indexers/[indexer name]/status?api-version=[api-version]
    api-key: [admin key]


`api-version` Je vyžadován. Verze preview je `2015-02-28-Preview`. 

`api-key` Musí být klíče správce (na rozdíl od klíč dotazů). Informace naleznete v sekci ověřování v [rozhraní API REST služby vyhledávání](https://msdn.microsoft.com/library/azure/dn798935.aspx) Další informace o klíčích. [Vytvoření služby Search na portálu](search-create-service-portal.md) vysvětluje, jak získat adresu URL služby a klíče vlastnosti používané v požadavku.

**Odpověď**

Stavový kód: 200 OK pro úspěšné odpovědi.

Text odpovědi obsahuje informace o celkový stav indexer, poslední vyvolání indexeru, jakož i historii poslední indexer volání (pokud existuje). 

Ukázkový text odpovědi vypadá takto: 

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

**Stav indexeru**

Indexer stav může být jedna z následujících hodnot:

* `running`Označuje, že indexeru běží normálně. Všimněte si, že některé spuštění indexeru se pravděpodobně stále nedaří, takže je vhodné zkontrolovat `lastResult` také vlastnost. 
* `error`Označuje, že indexeru došlo k chybě, která nemůže být vyřešen bez lidského zásahu. Například pověření ke zdroji dat platnost vypršela, nebo došlo ke změně schématu zdroje dat nebo cílový index v ukončování způsobem. 

**Výsledek spuštění indexeru**

Výsledek spuštění indexeru obsahuje informace o provádění jedné indexer. Nejnovější výsledek je prezentované jako `lastResult` vlastnost indexer stavu. Další poslední výsledky, pokud je k dispozici, se vrátí jako `executionHistory` vlastnost indexer stavu. 

Výsledek spuštění indexeru obsahuje následující vlastnosti: 

* `status`: stav spuštění. V tématu [stav spuštění indexeru](#IndexerExecutionStatus) níže podrobnosti. 
* `errorMessage`: chybovou zprávu pro selhání spuštění. 
* `startTime`: čas v UTC při spuštění tohoto spuštění.
* `endTime`: čas v UTC při spuštění tohoto skončila. Tato hodnota není nastavená, pokud provádění stále probíhá.
* `errors`: pole chyb na úrovni položek, pokud existuje. Každá položka obsahuje klíč dokumentu (`key` vlastnosti) a chybovou zprávou (`errorMessage` vlastnost). 
* `itemsProcessed`: počet datové položky (například řádky tabulky), kteří se pokusili indexeru indexu při spuštění tohoto zdroje. 
* `itemsFailed`: počet položek, které došlo k chybě během spuštění tohoto.  
* `initialTrackingState`: vždy `null` pro první spuštění indexeru, nebo pokud data změnit zásady sledování není povoleno ve zdroji dat použít. Pokud je tato zásada je povoleno, v dalších spuštěních tato hodnota určuje první hodnota zpracovává spuštění tohoto sledování (nejnižší) změn. 
* `finalTrackingState`: vždy `null` Pokud data změnit zásady sledování není povolena ve zdroji dat použít. Označuje, jinak hodnota sledování hodnota úspěšně zpracoval spuštění tohoto nejnovější změn (nejvyšší). 

<a name="IndexerExecutionStatus"></a>
**Stav spuštění indexeru**

Stav spuštění indexeru zachycuje stav spuštění jedné indexer. Může mít následující hodnoty:

* `success`Určuje, zda byla úspěšně dokončena spuštění indexeru.
* `inProgress`Označuje, že spuštění indexeru je v průběhu. 
* `transientFailure`Označuje, že se nezdařilo spuštění indexeru. V tématu `errorMessage` vlastnost podrobnosti. Selhání může nebo nemusí vyžadovat lidského zásahu opravit – například opravě nekompatibility schématu mezi zdroji dat a cílový index vyžaduje akce uživatele, když s prodlevou zdroj dočasná data, která nemá. Indexer volání bude pokračovat podle plánu, pokud je k dispozici. 
* `persistentFailure`Označuje, že indexeru selhala způsobem, který vyžaduje lidského zásahu. Naplánované indexer spuštěních zastaví. Po vyřešení problému, restartujte naplánované spuštění pomocí resetovat Indexer rozhraní API. 
* `reset`Označuje, že byl obnoven indexeru voláním resetovat Indexer API (viz níže). 

<a name="ResetIndexer"></a>

## <a name="reset-indexer"></a>Resetovat Indexer
**Resetovat Indexer** operace obnoví stav přidružený indexeru sledování změn. To umožňuje spouštět od začátku přeindexování (například pokud došlo ke změně vašeho schématu zdroje dat) nebo chcete-li změnit zásady detekce změn dat pro zdroj dat přidružený indexeru.   

    POST https://[service name].search.windows.net/indexers/[indexer name]/reset?api-version=[api-version]
    api-key: [admin key]

`api-version` Je vyžadován. Verze preview je `2015-02-28-Preview`. 

`api-key` Musí být klíče správce (na rozdíl od klíč dotazů). Informace naleznete v sekci ověřování v [rozhraní API REST služby vyhledávání](https://msdn.microsoft.com/library/azure/dn798935.aspx) Další informace o klíčích. [Vytvoření služby Search na portálu](search-create-service-portal.md) vysvětluje, jak získat adresu URL služby a klíče vlastnosti používané v požadavku.

**Odpověď**

Stavový kód: 204 žádný obsah pro úspěšné odpovědi.

## <a name="mapping-between-sql-data-types-and-azure-search-data-types"></a>Mapování mezi SQL datové typy a typy dat vyhledávání systému Azure
<table style="font-size:12">
<tr>
<td>Datový typ SQL.</td>    
<td>Cílový index povolené typy polí</td>
<td>Poznámky</td>
</tr>
<tr>
<td>Bit</td>
<td>Edm.Boolean Edm.String</td>
<td></td>
</tr>
<tr>
<td>int, smallint, tinyint</td>
<td>Edm.String Edm.Int32, Edm.Int64,</td>
<td></td>
</tr>
<tr>
<td>bigint</td>
<td>Edm.Int64 Edm.String</td>
<td></td>
</tr>
<tr>
<td>skutečné, float</td>
<td>Edm.Double Edm.String</td>
<td></td>
</tr>
<tr>
<td>Smallmoney peníze<br>Decimal<br>číselné
</td>
<td>Edm.String</td>
<td>Vyhledávání systému Azure nepodporuje převod decimal typy do Edm.Double, protože by to ztratit přesnost
</td>
</tr>
<tr>
<td>Char, nchar, varchar, nvarchar</td>
<td>Edm.String<br/>Collection(Edm.String)</td>
<td>V tématu [funkce mapování polí](#FieldMappingFunctions) v tomto dokumentu podrobnosti o tom, jak transformace sloupec řetězce do Collection(Edm.String)</td>
</tr>
<tr>
<td>smalldatetime, datetime, datetime2, date, datetimeoffset</td>
<td>Edm.DateTimeOffset Edm.String</td>
<td></td>
</tr>
<tr>
<td>uniqueidentifer</td>
<td>Edm.String</td>
<td></td>
</tr>
<tr>
<td>Geography</td>
<td>Edm.GeographyPoint</td>
<td>Jsou podporovány pouze geography instance typu bodu s SRID 4326 (což je výchozí hodnota)</td>
</tr>
<tr>
<td>ROWVERSION</td>
<td>Není k dispozici</td>
<td>Verze řádku sloupce nelze uložit do indexu vyhledávání, ale mohou být použity pro sledování změn</td>
</tr>
<tr>
<td>čas, časový interval<br>binary, varbinary, image,<br>XML, geometry, typy CLR</td>
<td>Není k dispozici</td>
<td>Nepodporuje se</td>
</tr>
</table>

## <a name="mapping-between-json-data-types-and-azure-search-data-types"></a>Mapování mezi JSON datové typy a typy dat vyhledávání systému Azure
<table style="font-size:12">
<tr>
<td>JSON datového typu</td>    
<td>Cílový index povolené typy polí</td>
<td>Poznámky</td>
</tr>
<tr>
<td>BOOL</td>
<td>Edm.Boolean Edm.String</td>
<td></td>
</tr>
<tr>
<td>Integrální čísla</td>
<td>Edm.String Edm.Int32, Edm.Int64,</td>
<td></td>
</tr>
<tr>
<td>Čísla s plovoucí desetinnou čárkou</td>
<td>Edm.Double Edm.String</td>
<td></td>
</tr>
<tr>
<td>Řetězec</td>
<td>Edm.String</td>
<td></td>
</tr>
<tr>
<td>pole primitivní typy, například ["a", "b", "c"]</td>
<td>Collection(Edm.String)</td>
<td></td>
</tr>
<tr>
<td>Řetězce, které vypadají podobně jako kalendářní data</td>
<td>Edm.DateTimeOffset Edm.String</td>
<td></td>
</tr>
<tr>
<td>GeoJSON bodu objekty</td>
<td>Edm.GeographyPoint</td>
<td>GeoJSON body jsou objekty JSON v následujícím formátu: {"typ": "Místo", "coordinates": [dlouhý a lat]} </td>
</tr>
<tr>
<td>Jiné objekty JSON</td>
<td>Není k dispozici</td>
<td>Nepodporuje; Vyhledávání systému Azure aktuálně podporuje jenom primitivní typy a kolekcí řetězců</td>
</tr>
</table>
