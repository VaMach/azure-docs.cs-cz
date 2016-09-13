<properties
    pageTitle="Použití aplikace Fiddler k vyhodnocování a testování rozhraní REST API služby Azure Search | Microsoft Azure | Hostovaná cloudová vyhledávací služba"
    description="Použijte aplikaci Fiddler k ověření dostupnosti služby Azure Search bezkódovou metodou a vyzkoušení rozhraní REST API."
    services="search"
    documentationCenter=""
    authors="HeidiSteen"
    manager="mblythe"
    editor=""/>

<tags
    ms.service="search"
    ms.devlang="rest-api"
    ms.workload="search"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.date="08/08/2016"
    ms.author="heidist"/>

# Použití aplikace Fiddler k vyhodnocování a testování rozhraní REST API služby Azure Search
> [AZURE.SELECTOR]
- [Přehled](search-query-overview.md)
- [Průzkumník služby Search](search-explorer.md)
- [Fiddler](search-fiddler.md)
- [.NET](search-query-dotnet.md)
- [REST](search-query-rest-api.md)

Tento článek vysvětluje, jak použít aplikaci Fiddler, která je dostupná [zdarma ke stažení na webu Telerik](http://www.telerik.com/fiddler), k vydávání žádostí HTTP a zobrazování odpovědí pomocí rozhraní REST API služby Azure Search, bez nutnosti psaní jakéhokoli kódu. Azure Search je plně spravovaná, hostovaná cloudová vyhledávací služba v Microsoft Azure, která je snadno programovatelná prostřednictvím rozhraní .NET a REST API. Rozhraní REST API služby Azure Search jsou popsaná na webu [MSDN](https://msdn.microsoft.com/library/azure/dn798935.aspx).

V následujících krocích vytvoříte index, nahrajete dokumenty, budete dotazovat index a potom budete dotazovat systém na informace o službě.

K provedení těchto kroků budete potřebovat službu Azure Search a `api-key`. Pokyny, jak začít, najdete v tématu [Vytvoření služby Azure Search na portálu](search-create-service-portal.md).

## Vytvoření indexu

1. Spusťte aplikaci Fiddler. V nabídce **Soubor** vypněte **Zachycování provozu**, a tím skryjte irelevantní aktivitu protokolu HTTP, která s aktuální úlohou nesouvisí.

3. Na kartě **autora** zformulujete podobnou žádost jako na následujícím snímku obrazovky.

    ![][1]

2. Vyberte **PUT**.

3. Zadejte adresu URL, která určuje adresu URL služby, atributy žádosti a verzi rozhraní API. Je nutné pamatovat na několik věcí:
   + Jako předponu použijte HTTPS.
   + Atribut žádosti je „/indexes/hotels“. Toto dává službě Search pokyn k vytvoření indexu s názvem „hotely“.
   + Verze rozhraní API se zadává malými písmeny jako „?api-version=2015-02-28“. Verze rozhraní API jsou důležité, protože služba Azure Search pravidelně nasazuje aktualizace. Ve výjimečných případech může aktualizace služby zavést zásadní změnu rozhraní API. Z tohoto důvodu služba Azure Search v každé žádosti vyžaduje verzi rozhraní API, abyste měli plně pod kontrolou, která verze se použije.

    Úplná adresa URL by měla vypadat podobně jako v následujícím příkladu.

            https://my-app.search.windows.net/indexes/hotels?api-version=2015-02-28

4.  Zadejte hlavičku žádosti, přičemž hodnoty host a api-key nahraďte hodnotami platnými pro vaši službu.

            User-Agent: Fiddler
            host: my-app.search.windows.net
            content-type: application/json
            api-key: 1111222233334444

5.  Do textu žádosti vložte pole, která tvoří definici indexu.
            
             {
            "name": "hotels",  
            "fields": [
              {"name": "hotelId", "type": "Edm.String", "key":true, "searchable": false},
              {"name": "baseRate", "type": "Edm.Double"},
              {"name": "description", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false},
              {"name": "hotelName", "type": "Edm.String"},
              {"name": "category", "type": "Edm.String"},
              {"name": "tags", "type": "Collection(Edm.String)"},
              {"name": "parkingIncluded", "type": "Edm.Boolean"},
              {"name": "smokingAllowed", "type": "Edm.Boolean"},
              {"name": "lastRenovationDate", "type": "Edm.DateTimeOffset"},
              {"name": "rating", "type": "Edm.Int32"},
              {"name": "location", "type": "Edm.GeographyPoint"}
             ]
            }

6.  Klikněte na tlačítko **Spustit**.

Během pár sekund by se v seznamu relace měla zobrazit odpověď HTTP 201, což znamená, že se index úspěšně vytvořil.

Pokud se zobrazí kód HTTP 504, ověřte, jestli je v adrese URL určený protokol HTTPS. Pokud se zobrazí kód HTTP 400 nebo 404, zkontrolujte, jestli v textu žádosti nejsou chyby způsobené kopírováním a vkládáním. Kód HTTP 403 obvykle znamená potíže s klíčem rozhraní API (buď je klíč neplatný, nebo je při určení klíče rozhraní API použitá nesprávná syntaxe).

## Nahrání dokumentů

Žádost o odeslání dokumentů bude na kartě **autora** vypadat podobně, jako vidíte níže. Text žádosti obsahuje data hledání pro 4 hotely.

   ![][2]

1. Vyberte **POST**.

2.  Zadejte adresu URL, která začíná HTTPS a dál obsahuje adresu URL služby a „/indexes/<'indexname'>/docs/index?api-version=2015-02-28“. Úplná adresa URL by měla vypadat podobně jako v následujícím příkladu.

            https://my-app.search.windows.net/indexes/hotels/docs/index?api-version=2015-02-28

3.  Hlavička žádosti by měla být stejná jako předtím. Nezapomeňte, že jste hodnoty host a api-key nahradili hodnotami, které jsou platné pro vaši službu.

            User-Agent: Fiddler
            host: my-app.search.windows.net
            content-type: application/json
            api-key: 1111222233334444

4.  Text žádosti obsahuje čtyři dokumenty, které se mají přidat do indexu hotels.

            {
            "value": [
            {
                "@search.action": "upload",
                "hotelId": "1",
                "baseRate": 199.0,
                "description": "Best hotel in town",
                "hotelName": "Fancy Stay",
                "category": "Luxury",
                "tags": ["pool", "view", "wifi", "concierge"],
                "parkingIncluded": false,
                "smokingAllowed": false,
                "lastRenovationDate": "2010-06-27T00:00:00Z",
                "rating": 5,
                "location": { "type": "Point", "coordinates": [-122.131577, 47.678581] }
              },
              {
                "@search.action": "upload",
                "hotelId": "2",
                "baseRate": 79.99,
                "description": "Cheapest hotel in town",
                "hotelName": "Roach Motel",
                "category": "Budget",
                "tags": ["motel", "budget"],
                "parkingIncluded": true,
                "smokingAllowed": true,
                "lastRenovationDate": "1982-04-28T00:00:00Z",
                "rating": 1,
                "location": { "type": "Point", "coordinates": [-122.131577, 49.678581] }
              },
              {
                "@search.action": "upload",
                "hotelId": "3",
                "baseRate": 279.99,
                "description": "Surprisingly expensive",
                "hotelName": "Dew Drop Inn",
                "category": "Bed and Breakfast",
                "tags": ["charming", "quaint"],
                "parkingIncluded": true,
                "smokingAllowed": false,
                "lastRenovationDate": null,
                "rating": 4,
                "location": { "type": "Point", "coordinates": [-122.33207, 47.60621] }
              },
              {
                "@search.action": "upload",
                "hotelId": "4",
                "baseRate": 220.00,
                "description": "This could be the one",
                "hotelName": "A Hotel for Everyone",
                "category": "Basic hotel",
                "tags": ["pool", "wifi"],
                "parkingIncluded": true,
                "smokingAllowed": false,
                "lastRenovationDate": null,
                "rating": 4,
                "location": { "type": "Point", "coordinates": [-122.12151, 47.67399] }
              }
             ]
            }

8.  Klikněte na tlačítko **Spustit**.

Během pár sekund by se v seznamu relace měla zobrazit odpověď HTTP 200. To znamená, že se dokumenty úspěšně vytvořily. Pokud se zobrazí kód 207, nejméně jeden dokument nešel nahrát. Pokud se zobrazí kód 404, hlavička nebo text žádosti obsahuje syntaktickou chybu.

## Dotazování indexu

Teď, když jsou index a dokumenty nahrané, můžete na ně vydávat dotazy.  Na kartě **autora** bude příkaz **GET**, který dotazuje službu, vypadat podobně jako na následujícím snímku obrazovky.

   ![][3]

1.  Vyberte **GET**.

2.  Zadejte adresu URL, která začíná HTTPS a dál obsahuje adresu URL služby, „/indexes/<'indexname'>/docs?“ a nakonec parametry dotazu. Jako příklad použijte následující adresu URL, kde ukázkový název hostitele nahradíte názvem, který je platný pro vaši službu.
    
            https://my-app.search.windows.net/indexes/hotels/docs?search=motel&facet=category&facet=rating,values:1|2|3|4|5&api-version=2015-02-28

    Tento dotaz hledá výraz „motel“ a načte kategorie faset pro hodnocení.

3.  Hlavička žádosti by měla být stejná jako předtím. Nezapomeňte, že jste hodnoty host a api-key nahradili hodnotami, které jsou platné pro vaši službu.

            User-Agent: Fiddler
            host: my-app.search.windows.net
            content-type: application/json
            api-key: 1111222233334444

Měli byste dostat kód odpovědi 200 a odpověď by měla vypadat podobně jako na následujícím snímku obrazovky.

   ![][4]

Následující příklad dotazu je převzatý z tématu [Operace prohledání indexu (rozhraní API služby Azure Search)](http://msdn.microsoft.com/library/dn798927.aspx) na webu MSDN. Mnoho příkladů dotazů v tomto tématu obsahuje mezery, které nejsou v aplikaci Fiddler povolené. Před vložením nahraďte v řetězci dotazu každou mezeru znakem +, abyste mohli dotaz zkusit v aplikaci Fiddler.

**Před nahrazením mezer:**

        GET /indexes/hotels/docs?search=*&$orderby=lastRenovationDate desc&api-version=2015-02-28

**Po nahrazení mezer znakem +:**

        GET /indexes/hotels/docs?search=*&$orderby=lastRenovationDate+desc&api-version=2015-02-28

## Dotaz na systém

Můžete taky dotázat systém a získat počet dokumentů a spotřebu úložiště. Žádost bude na kartě **autora** vypadat podobně, jako vidíte níže, a odpověď vrátí počet dokumentů a využité místo.

 ![][5]

1.  Vyberte **GET**.

2.  Zadejte adresu URL, která obsahuje adresu URL služby a potom „/indexes/hotels/stats?api-version=2015-02-28“:

            https://my-app.search.windows.net/indexes/hotels/stats?api-version=2015-02-28

3.  Zadejte hlavičku žádosti, přičemž hodnoty host a api-key nahraďte hodnotami platnými pro vaši službu.

            User-Agent: Fiddler
            host: my-app.search.windows.net
            content-type: application/json
            api-key: 1111222233334444

4.  Text žádosti nechte prázdný.

5.  Klikněte na tlačítko **Spustit**. V seznamu relace by se měl zobrazit stavový kód HTTP 200. Vyberte položku publikovanou k vašemu příkazu.

6.  Klikněte na kartu **Kontroly**, klikněte na kartu **Hlavičky** a vyberte formát JSON. Měli byste vidět počet dokumentů a velikost úložiště (v KB).

## Další kroky

Informace o bezkódovém přístupu k správě a používání služby Azure Search najdete v tématu [Správa služby Search v Azure](search-manage.md).


<!--Image References-->
[1]: ./media/search-fiddler/AzureSearch_Fiddler1_PutIndex.png
[2]: ./media/search-fiddler/AzureSearch_Fiddler2_PostDocs.png
[3]: ./media/search-fiddler/AzureSearch_Fiddler3_Query.png
[4]: ./media/search-fiddler/AzureSearch_Fiddler4_QueryResults.png
[5]: ./media/search-fiddler/AzureSearch_Fiddler5_QueryStats.png



<!--HONumber=sep16_HO1-->


