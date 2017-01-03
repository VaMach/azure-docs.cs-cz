---
title: "Dotazování indexu Azure Search pomocí REST API | Dokumentace Microsoftu"
description: "Sestavení vyhledávacího dotazu ve službě Azure Search a použití parametrů hledání k filtrování a řazení výsledků vyhledávání."
services: search
documentationcenter: 
manager: jhubbard
author: ashmaka
ms.assetid: 8b3ca890-2f5f-44b6-a140-6cb676fc2c9c
ms.service: search
ms.devlang: na
ms.workload: search
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.date: 12/08/2016
ms.author: ashmaka
translationtype: Human Translation
ms.sourcegitcommit: 455c4847893175c1091ae21fa22215fd1dd10c53
ms.openlocfilehash: 96e8177f57977f88c5a4a1ec0b9243b5b348f078

---

# <a name="query-your-azure-search-index-using-the-rest-api"></a>Dotazování indexu Azure Search pomocí REST API
> [!div class="op_single_selector"]
>
> * [Přehled](search-query-overview.md)
> * [Azure Portal](search-explorer.md)
> * [.NET](search-query-dotnet.md)
> * [REST](search-query-rest-api.md)
>
>

Tento článek vám ukáže postup dotazování indexu pomocí [REST API služby Azure Search](https://docs.microsoft.com/rest/api/searchservice/).

Před zahájením tohoto názorného průvodce byste již měli mít [vytvořený index Azure Search](search-what-is-an-index.md) a ten by měl být [naplněný daty](search-what-is-data-import.md).

## <a name="i-identify-your-azure-search-services-query-api-key"></a>I. Zjistěte klíč api-key správce služby Azure Search
Klíčovou komponentou každé operace vyhledávání na REST API služby Azure Search je klíč *api-key*, který byl vygenerován pro službu, kterou jste zřídili. Platný klíč vytváří na základě žádosti vztah důvěryhodnosti mezi aplikací, která žádost odeslala, a službou, která ji zpracovává.

1. Pokud chcete najít klíče api-key svojí služby, musíte se přihlásit k webu [Azure Portal](https://portal.azure.com/).
2. Přejděte do okna služby Azure Search.
3. Klikněte na ikonu klíčů.

Vaše služba bude mít *klíče správce* a *klíče dotazů*.

* Primární a sekundární *klíče správce* udělují úplná práva ke všem operacím, včetně možnosti spravovat službu, vytvářet a odstraňovat indexy, indexery a zdroje dat. Existují dva klíče, takže pokud se rozhodnete znovu vygenerovat primární klíč, můžete dál používat sekundární klíč, a naopak.
* Vaše *klíče dotazů* udělují přístup jen pro čtení k indexům a dokumentům a obvykle se distribuují klientským aplikacím, které vydávají požadavky hledání.

Pro účely dotazování indexu můžete použít jeden z klíčů dotazů. Pro dotazy lze použít i klíče správce, ale ve svých aplikacích byste měli používat klíče dotazů, což lépe odpovídá [Principu minimálního oprávnění](https://en.wikipedia.org/wiki/Principle_of_least_privilege).

## <a name="ii-formulate-your-query"></a>II. Formulování dotazu
Existují dva způsoby [vyhledávání v indexu pomocí REST API](https://docs.microsoft.com/rest/api/searchservice/Search-Documents). První způsob je vydání požadavku HTTP POST, kde parametry dotazu budou určené v objektu JSON v textu požadavku. Druhý způsob je vydání požadavku HTTP GET, kde parametry dotazu budou určené v rámci URL požadavku. Všimněte si, že metoda POST má [mírnější omezení](https://docs.microsoft.com/rest/api/searchservice/Search-Documents) velikosti parametrů dotazu, než metoda GET. Z tohoto důvodu doporučujeme používat metodu POST, pokud pro vás neplatí zvláštní podmínky, kdy by bylo pohodlnější použití metody GET.

U metody POST i GET budete muset v URL požadavku poskytnout *název služby*, *název indexu* a správnou *verzi rozhraní API* (v době publikování tohoto dokumentu je aktuální verze rozhraní API `2016-09-01`). U metody GET zadáte parametry dotazu místo *query_string* na konci URL. Formát URL vidíte níže:

    https://[service name].search.windows.net/indexes/[index name]/docs?[query string]&api-version=2016-09-01

Formát pro metodu POST je stejný, ale jako parametr řetězce dotazu obsahuje pouze api-version.

#### <a name="example-queries"></a>Ukázky dotazů
Zde naleznete několik ukázky dotazů na index s názvem „hotels“. Dotazy jsou ukázané ve formátech GET i POST.

Vyhledat výraz „budget“ v celém indexu a vrátit pouze pole `hotelName`:

```
GET https://[service name].search.windows.net/indexes/hotels/docs?search=budget&$select=hotelName&api-version=2016-09-01

POST https://[service name].search.windows.net/indexes/hotels/docs/search?api-version=2016-09-01
{
    "search": "budget",
    "select": "hotelName"
}
```

Použít na index filtr pro nalezení hotelů levnějších než 150 dolarů za noc a vrátit pole `hotelId` a `description`:

```
GET https://[service name].search.windows.net/indexes/hotels/docs?search=*&$filter=baseRate lt 150&$select=hotelId,description&api-version=2016-09-01

POST https://[service name].search.windows.net/indexes/hotels/docs/search?api-version=2016-09-01
{
    "search": "*",
    "filter": "baseRate lt 150",
    "select": "hotelId,description"
}
```

Prohledat celý index, seřadit podle určitého pole (`lastRenovationDate`) v sestupném pořadí, vzít první dva výsledky a zobrazit pouze pole `hotelName` a `lastRenovationDate`:

```
GET https://[service name].search.windows.net/indexes/hotels/docs?search=*&$top=2&$orderby=lastRenovationDate desc&$select=hotelName,lastRenovationDate&api-version=2016-09-01

POST https://[service name].search.windows.net/indexes/hotels/docs/search?api-version=2016-09-01
{
    "search": "*",
    "orderby": "lastRenovationDate desc",
    "select": "hotelName,lastRenovationDate",
    "top": 2
}
```

## <a name="iii-submit-your-http-request"></a>III. Odeslání požadavku HTTP
Nyní, když jste formulovali dotaz jako součást URL požadavku HTTP (pro metodu GET) nebo textu požadavku HTTP (pro metodu POST), můžete definovat hlavičky požadavku a odeslat dotaz.

#### <a name="request-and-request-headers"></a>Požadavek a hlavičky požadavku
Musíte definovat dvě hlavičky požadavku pro metodu GET, nebo tři hlavičky pro metodu POST.

1. Hlavičku `api-key` je nutné nastavit na klíč dotazu, který jste získali v kroku I. Všimněte si, že jako hlavičku `api-key` můžete použít i klíč správce, ale doporučujeme používat klíč dotazů, protože uděluje přístup k indexům a dokumentům výhradně pouze pro čtení.
2. Hlavička `Accept` musí být nastavená na `application/json`.
3. U metody POST by měla být hlavička `Content-Type` také nastavená na `application/json`.

Níže naleznete požadavek HTTP GET s jednoduchým dotazem, který vyhledá výraz „motel“ v indexu „hotels“ pomocí REST API služby Azure Search.

```
GET https://[service name].search.windows.net/indexes/hotels/docs?search=motel&api-version=2016-09-01
Accept: application/json
api-key: [query key]
```

Zde je stejný vzorový dotaz, tentokrát pomocí HTTP POST:

```
POST https://[service name].search.windows.net/indexes/hotels/docs/search?api-version=2016-09-01
Content-Type: application/json
Accept: application/json
api-key: [query key]

{
    "search": "motel"
}
```

Po úspěšném požadavku dotazu nastane stavový kód `200 OK` a výsledky vyhledávání se vrátí jako JSON v textu odpovědi. Zde vidíte, jak vypadají výsledky dotazů pro výše uvedený kód za předpokladu, že je index „hotels“ naplněný vzorovými daty v tématu [Import Dat do služby Azure Search pomocí REST API](search-import-data-rest-api.md) (všimněte si, že byl JSON pro přehlednost zformátován):

```JSON
{
    "value": [
        {
            "@search.score": 0.59600675,
            "hotelId": "2",
            "baseRate": 79.99,
            "description": "Cheapest hotel in town",
            "description_fr": "Hôtel le moins cher en ville",
            "hotelName": "Roach Motel",
            "category": "Budget",
            "tags":["motel", "budget"],
            "parkingIncluded": true,
            "smokingAllowed": true,
            "lastRenovationDate": "1982-04-28T00:00:00Z",
            "rating": 1,
            "location": {
                "type": "Point",
                "coordinates": [-122.131577, 49.678581],
                "crs": {
                    "type":"name",
                    "properties": {
                        "name": "EPSG:4326"
                    }
                }
            }
        }
    ]
}
```

Zjistěte více v sekci „Odpověď“ tématu [Vyhledávání dokumentů](https://docs.microsoft.com/rest/api/searchservice/Search-Documents). Další informace o stavových kódech HTTP, které se mohou vrátit v případě selhání, naleznete v tématu [Stavové kódy HTTP (Azure Search)](https://docs.microsoft.com/rest/api/searchservice/HTTP-status-codes).



<!--HONumber=Jan17_HO1-->


