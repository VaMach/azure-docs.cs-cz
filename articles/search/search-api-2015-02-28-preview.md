---
title: "Služba Azure Search REST API verze 2015-02-28-verze Preview služby | Microsoft Docs"
description: "Azure Search služby REST API verze 2015-02-28-Preview zahrnuje povolenými experimentálními funkcemi, jako je například analyzátory přirozeného jazyka a moreLikeThis hledání."
services: search
documentationcenter: na
author: brjohnstmsft
manager: pablocas
editor: 
ms.assetid: 3dba3bf8-9c83-42f6-82bc-04727bd11037
ms.service: search
ms.devlang: rest-api
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: search
ms.date: 05/01/2017
ms.author: brjohnst
ms.openlocfilehash: e6ad5c964bfa8421be2706cb4015980e01a271b7
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="azure-search-service-rest-api-version-2015-02-28-preview"></a>Rozhraní API REST služby vyhledávání systému Azure: Verze 2015-02-28-Preview
Tento článek je referenční dokumentaci k nástroji pro `api-version=2015-02-28-Preview`. Tato verze preview rozšiřuje všeobecně dostupná verzí [rozhraní api-version = 2015-02-28](https://msdn.microsoft.com/library/dn798935.aspx), tím, že poskytuje následující povolenými experimentálními funkcemi:

* `moreLikeThis`parametr v dotazu [vyhledávání dokumentů](#SearchDocs) rozhraní API. Najde jiné dokumenty, které jsou relevantní pro konkrétní jiného dokumentu.

Několik dalších částí `2015-02-28-Preview` REST API popsané samostatně. Mezi ně patří:

* [Vyhodnocování profily](search-api-scoring-profiles-2015-02-28-preview.md)
* [Indexery](search-api-indexers-2015-02-28-preview.md)

Služba Azure Search je k dispozici v několika verzích. Naleznete [verze služby vyhledávání](http://msdn.microsoft.com/library/azure/dn864560.aspx) podrobnosti.

## <a name="apis-in-this-document"></a>Rozhraní API v tomto dokumentu
Rozhraní API služby Azure Search podporuje dva syntaxe adresy URL pro operace rozhraní API: jednoduchý a OData (viz [podporu pro OData (API služby Azure Search)](http://msdn.microsoft.com/library/azure/dn798932.aspx) podrobnosti). V následujícím seznamu jsou jednoduché syntaxe.

[Vytvoření indexu](#CreateIndex)

    POST /indexes?api-version=2015-02-28-Preview

[Aktualizovat Index](#UpdateIndex)

    PUT /indexes/[index name]?api-version=2015-02-28-Preview

[Získat Index](#GetIndex)

    GET /indexes/[index name]?api-version=2015-02-28-Preview

[Výpis indexy](#ListIndexes)

    GET /indexes?api-version=2015-02-28-Preview

[Získat statistiku indexu](#GetIndexStats)

    GET /indexes/[index name]/stats?api-version=2015-02-28-Preview

[Analyzátor testu](#TestAnalyzer)

    POST /indexes/[index name]/analyze?api-version=2015-02-28-Preview

[Odstranění indexu](#DeleteIndex)

    DELETE /indexes/[index name]?api-version=2015-02-28-Preview

[Přidat, odstranit a aktualizovat Data v indexu](#AddOrUpdateDocuments)

    POST /indexes/[index name]/docs/index?api-version=2015-02-28-Preview

[Vyhledávání dokumentů](#SearchDocs)

    GET /indexes/[index name]/docs?[query parameters]
    POST /indexes/[index name]/docs/search?api-version=2015-02-28-Preview

[Vyhledávání dokumentů](#LookupAPI)

     GET /indexes/[index name]/docs/[key]?[query parameters]

[Počet dokumentů](#CountDocs)

    GET /indexes/[index name]/docs/$count?api-version=2015-02-28-Preview

[Návrhy](#Suggestions)

    GET /indexes/[index name]/docs/suggest?[query parameters]
    POST /indexes/[index name]/docs/suggest?api-version=2015-02-28-Preview

- - -
<a name="IndexOps"></a>

## <a name="index-operations"></a>Operace indexu
Můžete vytvořit a spravovat indexy ve službě Azure Search pomocí jednoduchých požadavků HTTP (POST, GET, PUT, DELETE) s daným indexem prostředků. Pokud chcete vytvořit index, nejprve POST dokument JSON, který popisuje schéma indexu. Schéma definuje pole index, jejich datové typy a jejich použití (například v fulltextové vyhledávání, řazení a filtrů používání omezujících vlastností). Definuje také vyhodnocování profily, trochu a další atributy, které umožňuje nakonfigurovat chování indexu.

Následující příklad uvádí ukázku schématu, používá pro vyhledávání na hotelů informace s pole popisu definované ve dvou jazycích. Všimněte si, jak atributy řídit, jak se používá pole. Například `hotelId` slouží jako klíč dokumentu (`"key": true`) a je vyloučen z fulltextové vyhledávání (`"searchable": false`).

    {
    "name": "hotels",  
    "fields": [
      {"name": "hotelId", "type": "Edm.String", "key": true, "searchable": false},
      {"name": "baseRate", "type": "Edm.Double"},
      {"name": "description", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false},
      {"name": "description_fr", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false, "analyzer": "fr.lucene"},
      {"name": "hotelName", "type": "Edm.String"},
      {"name": "category", "type": "Edm.String"},
      {"name": "tags", "type": "Collection(Edm.String)"},
      {"name": "parkingIncluded", "type": "Edm.Boolean"},
      {"name": "smokingAllowed", "type": "Edm.Boolean"},
      {"name": "lastRenovationDate", "type": "Edm.DateTimeOffset"},
      {"name": "rating", "type": "Edm.Int32"},
      {"name": "location", "type": "Edm.GeographyPoint"}
     ],
     "suggesters": [
      {
       "name": "sg",
       "searchMode": "analyzingInfixMatching",
       "sourceFields": ["hotelName"]
      }
     ]
    }

Po vytvoření indexu budete odesílat dokumenty, které naplňte index. V tématu [přidat nebo aktualizovat dokumenty](#AddOrUpdateDocuments) pro tento další krok.

Video Úvod do indexu Azure Search, najdete v článku [Channel 9 cloudu zahrnují díl na Azure Search](http://go.microsoft.com/fwlink/p/?LinkId=511509).

<a name="CreateIndex"></a>

## <a name="create-index"></a>Vytvoření indexu
Index je hlavním prostředkem, uspořádání a vyhledávání dokumentů ve službě Azure Search je podobná jak tabulku organizuje záznamů v databázi. Každý index obsahuje kolekci dokumentů, že všechny odpovídat na schéma indexu (názvy polí, datové typy a vlastnosti), ale indexy určují také další konstrukce (trochu, vyhodnocování profily a možnosti CORS), které definují jiného chování vyhledávání.

Můžete vytvořit nový index v rámci služby Azure Search pomocí požadavek HTTP POST nebo PUT. Text žádosti je schématu JSON, která určuje informace index a konfigurace.

    POST https://[service name].search.windows.net/indexes?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin key]

Alternativně můžete použít PUT a zadejte název indexu v identifikátoru URI. Pokud index neexistuje, bude vytvořen.

    PUT https://[search service url]/indexes/[index name]?api-version=[api-version]

Vytvoření indexu určuje strukturu dokumenty uložené a použít v operace hledání. Naplňování indexu je samostatný operace. Pro tento krok, můžete použít [indexer](https://msdn.microsoft.com/library/azure/mt183328.aspx) (k dispozici pro podporované zdroje dat) nebo [přidání, aktualizace nebo odstranění dokumentů](https://msdn.microsoft.com/library/azure/dn798930.aspx) operaci. Převedený index se vygeneruje, když jsou odeslány dokumenty.

**Poznámka:**: maximální počet indexů povoleno se liší podle cenové úrovně. Bezplatné služby umožňuje až 3 indexy. Standardní služby umožňuje 50 indexy jednu službu vyhledávání. V tématu [limity a omezení](http://msdn.microsoft.com/library/azure/dn798934.aspx) podrobnosti.

**Požadavek**

Je požadován pro všechny žádosti o služby protokol HTTPS. **Create Index** požadavek lze sestavit pomocí Metoda POST nebo PUT metody. Pokud používáte POST, je nutné zadat název indexu v těle žádosti spolu s definice schématu indexu. Pomocí PUT název indexu je součástí adresy URL. Pokud index neexistuje, vytvoří se. Pokud již existuje, je aktualizován na novou definici.

Název indexu musí být malá písmena, začínat písmenem nebo číslicí, mít žádné lomítka nebo tečky a být kratší než 128 znaků. Po spuštění název indexu s písmenem nebo číslicí, může obsahovat zbytek název jakékoli písmeno, čísla a pomlčky, dokud nejsou po sobě jdoucí pomlčky.

`api-version` Je vyžadován. V tématu [verze služby vyhledávání](http://msdn.microsoft.com/library/azure/dn864560.aspx) seznam dostupných verzí.

**Hlavičky požadavku**

Následující seznam popisuje hlavičky žádosti požadované a volitelné.

* `Content-Type`: Vyžaduje se. Tuto možnost nastavíte na`application/json`
* `api-key`: Vyžaduje se. `api-key` Se používá ke
* ověřit požadavek na vaši službu vyhledávání. Je řetězcovou hodnotu, jedinečné pro vaši službu. **Create Index** musí zahrnovat požadavek `api-key` záhlaví nastavit klíče správce (na rozdíl od klíč dotazů).

Budete také potřebovat názvu služby pro vytvoření adresy URL žádosti. Můžete získat název služby a `api-key` z řídicího panelu služby na portálu Azure. V tématu [vytvoření služby Azure Search na portálu](search-create-service-portal.md) nápovědu navigace stránky.

<a name="RequestData"></a>
**Syntaxe požadavku textu**

Text žádosti obsahuje definici schématu, který obsahuje seznam datových polí v rámci dokumenty, kteří budou dodáni do tohoto indexu, datové typy, atributy, jakož i volitelný seznam vyhodnocování profily, které se používají ke stanovení skóre odpovídající dokumenty v době dotazů .

Všimněte si, že pro požadavek POST musíte zadat název indexu v textu požadavku.

Může existovat pouze jedno pole s klíčem v indexu. Musí být řetězcové pole. Toto pole představuje jedinečný identifikátor každého dokumentu uloženého v indexu.

Hlavní části indexu zahrnují následující:

* `name`
* `fields`které budou dodáni do tohoto indexu, včetně názvu, datový typ a vlastnosti, které definují povolené akce na tomto poli.
* `suggesters`použít pro automatické dokončování nebo našeptávání dotazů.
* `scoringProfiles`používá pro určení rozsahu skóre vlastní vyhledávání. V tématu [přidejte vyhodnocování profily](https://msdn.microsoft.com/library/azure/dn798928.aspx) podrobnosti.
* `analyzers`, `charFilters`, `tokenizers`, `tokenFilters` používá k definování, jak jsou dokumenty či dotazy rozdělen do indexovanou/prohledávatelné tokeny. V tématu [Analysis ve službě Azure Search](https://aka.ms//azsanalysis) podrobnosti.
* `defaultScoringProfile`umožňuje přepsat výchozí chování vyhodnocování.
* `corsOptions`tak, aby dotazy cross-origin oproti indexu.

Syntaxe pro vytváření struktury datová část požadavku je následující. Ukázková žádost je k dispozici další na v tomto tématu.

    {
      "name": (optional on PUT; required on POST) "name_of_index",
      "fields": [
        {
          "name": "name_of_field",
          "type": "Edm.String | Collection(Edm.String) | Edm.Int32 | Edm.Int64 | Edm.Double | Edm.Boolean | Edm.DateTimeOffset | Edm.GeographyPoint",
          "searchable": true (default where applicable) | false (only Edm.String and Collection(Edm.String) fields can be searchable),
          "filterable": true (default) | false,
          "sortable": true (default where applicable) | false (Collection(Edm.String) fields cannot be sortable),
          "facetable": true (default where applicable) | false (Edm.GeographyPoint fields cannot be facetable),
          "key": true | false (default, only Edm.String fields can be keys),
          "retrievable": true (default) | false,              
          "analyzer": "name of the analyzer used for search and indexing", (only if 'searchAnalyzer' and 'indexAnalyzer' are not set)
          "searchAnalyzer": "name of the search analyzer", (only if 'indexAnalyzer' is set and 'analyzer' is not set)
          "indexAnalyzer": "name of the indexing analyzer" (only if 'searchAnalyzer' is set and 'analyzer' is not set)
        }
      ],
      "suggesters": [
        {
          "name": "name of suggester",
          "searchMode": "analyzingInfixMatching" (other modes may be added in the future),
          "sourceFields": ["field1", "field2", ...]
        }
      ],
      "scoringProfiles": [
        {
          "name": "name of scoring profile",
          "text": (optional, only applies to searchable fields) {
            "weights": {
              "searchable_field_name": relative_weight_value (positive numbers),
              ...
            }
          },
          "functions": (optional) [
            {
              "type": "magnitude | freshness | distance | tag",
              "boost": # (positive number used as multiplier for raw score != 1),
              "fieldName": "...",
              "interpolation": "constant | linear (default) | quadratic | logarithmic",
              "magnitude": {
                "boostingRangeStart": #,
                "boostingRangeEnd": #,
                "constantBoostBeyondRange": true | false (default)
              },
              "freshness": {
                "boostingDuration": "..." (value representing timespan leading to now over which boosting occurs)
              },
              "distance": {
                "referencePointParameter": "...", (parameter to be passed in queries to use as reference location, see "scoringParameter" for syntax details)
                "boostingDistance": # (the distance in kilometers from the reference location where the boosting range ends)
              },
              "tag": {
                "tagsParameter": "..." (parameter to be passed in queries to specify list of tags to compare against target field, see "scoringParameter" for syntax details)
              }
            }
          ],
          "functionAggregation": (optional, applies only when functions are specified)
            "sum (default) | average | minimum | maximum | firstMatching"
        }
      ],
      "analyzers":(optional)[ ... ],
      "charFilters":(optional)[ ... ],
      "tokenizers":(optional)[ ... ],
      "tokenFilters":(optional)[ ... ],
      "defaultScoringProfile": (optional) "...",
      "corsOptions": (optional) {
        "allowedOrigins": ["*"] | ["origin_1", "origin_2", ...],
        "maxAgeInSeconds": (optional) max_age_in_seconds (non-negative integer)
      }
    }

**Atributy indexu**

Následující atributy můžete nastavit při vytváření indexu. Podrobnosti o vyhodnocování a vyhodnocování profilů najdete v tématu [přidat vyhodnocování profily](https://msdn.microsoft.com/library/azure/dn798928.aspx).

`name`-Nastaví název pole.

`type`-Nastaví datový typ pole.

`searchable`-Označí pole jako fulltextově možnost vyhledávání. To znamená, že ho určitým analysis například dělení slov během indexování. Pokud nastavíte `searchable` pole na hodnotu jako "slunečného dne", interně jej bude možné rozdělit na jednotlivé tokeny "slunečného" a "den". To umožňuje fulltextové vyhledávání pro tyto podmínky. Pole typu `Edm.String` nebo `Collection(Edm.String)` jsou `searchable` ve výchozím nastavení. Pole dalších typů nelze `searchable`.

* **Poznámka:**: `searchable` pole využívat místo navíc v indexu, protože Azure Search budou ukládat další tokenizovaná verzi hodnotu pole pro fulltextové vyhledávání. Pokud chcete ušetřit místo v indexu a nepotřebujete pole mají být zahrnuty do hledání, nastavte `searchable` k `false`.

`filterable`– Umožňuje na pole odkazovat ve `$filter` dotazy. `filterable`se liší od `searchable` v tom, jak jsou zpracovávány řetězce. Pole typu `Edm.String` nebo `Collection(Edm.String)` , které jsou `filterable` nejsou prováděny dělení slov, tak porovnání jsou pro jenom přesné shody. Například pokud nastavíte toto pole `f` "slunečného den" `$filter=f eq 'sunny'` se najít žádné shody, ale `$filter=f eq 'sunny day'` bude. Všechna pole jsou `filterable` ve výchozím nastavení.

`sortable`-Ve výchozím nastavení systém řadí výsledky podle skóre, ale v mnoha prostředí budou uživatelé chtít seřadit podle pole v dokumentech. Pole typu `Collection(Edm.String)` nemůže být `sortable`. Všechna ostatní pole jsou `sortable` ve výchozím nastavení.

`facetable`-Obvykle používaných v prezentace výsledky hledání, který obsahuje počet přístupů podle kategorie (pro příklad, vyhledejte digitální fotoaparáty a přístupů viz značku, megapixely, ceny, atd.). Tuto možnost nelze použít s pole typu `Edm.GeographyPoint`. Všechna ostatní pole jsou `facetable` ve výchozím nastavení.

* **Poznámka:**: pole typu `Edm.String` , které jsou `filterable`, `sortable`, nebo `facetable` může být maximálně 32 KB délku. Je to proto, že tato pole jsou považovány za jeden hledaný termín a maximální délka termín, který se ve službě Azure Search je 32KB. Pokud potřebujete ukládat další text než tento v poli jednoho řetězce, je potřeba explicitně nastavit `filterable`, `sortable`, a `facetable` k `false` v definici indexu.
* **Poznámka:**: Pokud má pole žádná z výše uvedených atributů nastavena na hodnotu `true` (`searchable`, `filterable`, `sortable`, nebo`facetable`) pole je efektivně vyloučen z obráceným index. Tato možnost je užitečná pro pole, které nejsou používány v dotazech, ale je potřeba mít ve výsledcích hledání. Kromě těchto polí z indexu zvyšuje výkon.

`key`-Označí pole jako obsahující jedinečné identifikátory pro dokumenty v indexu. Právě jedno pole musí být zvolena jako `key` pole a musí být typu `Edm.String`. Klíčová pole lze použít k vyhledání dokumentů přímo prostřednictvím [rozhraní API pro vyhledávání](#LookupAPI).

`retrievable`-Nastaví, zda může být pole vrácené ve výsledku hledání.  To je užitečné, když chcete pole (například okraje) použít jako filtr, řazení a vyhodnocování mechanismus, ale nechcete, aby pole, které chcete být viditelné pro koncového uživatele. Tento atribut musí být `true` pro `key` pole.

`analyzer`-Nastaví název analyzátor použití pole na čas při vyhledávání a indexování čas. Pro sadu povolených hodnot najdete v části [analyzátorů](https://msdn.microsoft.com/library/mt605304.aspx). Tuto možnost lze použít pouze s `searchable` pole a nelze nastavit společně s buď `searchAnalyzer` nebo `indexAnalyzer`.  Jakmile analyzátor jste vybrali, nelze změnit pro pole.

`searchAnalyzer`-Nastaví název analyzátor používá během hledání pro pole. Pro sadu povolených hodnot najdete v části [analyzátorů](https://msdn.microsoft.com/library/mt605304.aspx). Tuto možnost lze použít pouze s `searchable` pole. Je nutné ji nastavit společně s `indexAnalyzer` a nelze ji nastavit společně s `analyzer` možnost. Tento analyzátor mohou být aktualizovány na stávající pole.

`indexAnalyzer`-Nastaví název analyzátor používá během indexování pro pole. Pro sadu povolených hodnot najdete v části [analyzátorů](https://msdn.microsoft.com/library/mt605304.aspx). Tuto možnost lze použít pouze s `searchable` pole. Je nutné ji nastavit společně s `searchAnalyzer` a nelze ji nastavit společně s `analyzer` možnost. Jakmile analyzátor jste vybrali, nelze změnit pro pole.

`suggesters`-Nastaví režim hledání a pole, které jsou zdroj obsahu pro návrhy. V tématu [trochu](#Suggesters) podrobnosti.

`scoringProfiles`-Definuje vlastní vyhodnocování chování, které umožňují ovlivnit položky zobrazovat na vyšších pozicích ve výsledcích hledání. Vyhodnocování profily jsou tvořeny váhu pole a funkce. V tématu [přidat vyhodnocování profily](https://msdn.microsoft.com/library/azure/dn798928.aspx) Další informace o atributech použít v profil vyhodnocování.

<!-- This is a standalone topic in MSDN -->
<a name="LanguageSupport"></a>
**Jazyková podpora**

Prohledatelná pole podstoupit analysis která nejvíc často zahrnuje dělení slov, normalizaci text a filtrování podmínky. Ve výchozím nastavení jsou prohledatelná pole ve službě Azure Search analyzovaný se [Apache Lucene standardní analyzátor](http://lucene.apache.org/core/4_9_0/analyzers-common/index.html) která dělí text do elementů následující["Segmentace Unicode Text"](http://unicode.org/reports/tr29/) pravidla. Kromě toho standardní analyzátor převede všechny znaky na jejich formulář malá písmena. Indexované dokumenty a hledaných termínů projít analýza během indexování a zpracování dotazu.

Služba Azure Search podporuje různé jazyky. Každý jazyk vyžaduje analyzer nestandardní text, který účty pro charakteristiky daného jazyka. Služba Azure Search nabízí dva typy analyzátorů:

* 35 analyzátorů zajišťoval Lucene.
* 50 analyzátorů zajišťoval proprietární přirozeného jazyka Microsoft zpracování technologie používané v kanceláři a Bing.

Někteří vývojáři preferovat známější, jednoduchý a open-source řešení Lucene. Lucene analyzátorů je rychlejší, ale Microsoft analyzátorů mít rozšířené možnosti, například Lematizace, word decompounding (v jazycích jako němčina, dánština, holandština, švédština, norština, estonština, dokončit, maďarština, slovenština) a entity rozpoznávání (adresy URL. e-mailů, kalendářních dat, čísel). Pokud je to možné byste měli spustit porovnání společnosti Microsoft a Lucene analyzátorů rozhodnout, které z nich je lepší přizpůsobit.

***Jejich porovnání***

Analyzátor Lucene pro angličtinu rozšiřuje standardní analyzátor. Se odebere z slova Přivlastňovací pád (koncové společnosti), platí rozklad dle [silné rozklad algoritmus](http://tartarus.org/~martin/PorterStemmer/)a také odebere Angličtina [zastavit slova](http://en.wikipedia.org/wiki/Stop_words).

Porovnání provede Microsoft analyzer Lematizace místo rozklad. Znamená to, se může zpracovat tvary a nestandardní word forms mnohem lepší co má za následek více relevantní výsledky hledání (sledovat modul 7 [Azure Search MVA prezentace](http://www.microsoftvirtualacademy.com/training-courses/adding-microsoft-azure-search-to-your-websites-and-apps) podrobnosti).

Indexování s Microsoft analyzátorů je v průměru dvakrát až třikrát pomalejší než jejich ekvivalenty Lucene, v závislosti na jazyk. Pro dotazy Průměrná velikost nesmí být výrazně ovlivněn výkon vyhledávání.

***Konfigurace***

Pro každé pole v definici indexu můžete nastavit `analyzer` vlastnost na název analyzátor, který určuje, které jazyk a dodavatele. Stejné Analyzátor se použijí, když indexování a hledání toto pole.
Například můžete mít samostatné pole pro angličtinu, francouzštinu a španělština hotelů popisy, které existují souběžného ve stejném indexu. Použití [parametr dotazu 'searchFields'](#SearchQueryParameters) k určení, které pro specifický jazyk pole pro vyhledávání proti v dotazech. Příklady dotazů, které zahrnují můžete zkontrolovat `analyzer` vlastnost [vyhledávání dokumentů](#SearchDocs). 

***Analyzátor seznamu***

Níže je seznam podporovaných jazyků společně s názvy analyzátor Lucene a společnosti Microsoft.

<table style="font-size:12">
    <tr>
        <th>Jazyk</th>
        <th>Název analyzátor Microsoft</th>
        <th>Název analyzátor Lucene</th>
    </tr>
    <tr>
        <td>arabština</td>
        <td>ar.microsoft</td>
        <td>ar.lucene</td>        
    </tr>
    <tr>
        <td>Arménština</td>
        <td></td>
        <td>hy.lucene</td>
      </tr>
    <tr>
        <td>Bengálském</td>
        <td>Bn.microsoft</td>
        <td></td>
    </tr>
      <tr>
        <td>Baskičtina</td>
        <td></td>
        <td>EU.lucene</td>
    </tr>
      <tr>
         <td>Bulharština</td>
        <td>BG.microsoft</td>
        <td>BG.lucene</td>
      </tr>
      <tr>
        <td>Katalánština</td>
        <td>CA.microsoft</td>
        <td>CA.lucene</td>          
      </tr>
    <tr>
        <td>Zjednodušená čínština</td>
        <td>zh-Hans.microsoft</td>
        <td>zh-Hans.lucene</td>        
    </tr>
    <tr>
        <td>Tradiční čínština</td>
        <td>zh-Hant.microsoft</td>
        <td>zh-Hant.lucene</td>        
    <tr>
    <tr>
        <td>Chorvatština</td>
        <td>hr.microsoft</td>
        <td/></td>
    </tr>
    <tr>
        <td>čeština</td>
        <td>cs.microsoft</td>
        <td>cs.lucene</td>        
    </tr>    
    <tr>
        <td>dánština</td>
        <td>da.microsoft</td>
        <td>da.lucene</td>        
    </tr>    
    <tr>
        <td>holandština</td>
        <td>NL.microsoft</td>
        <td>NL.lucene</td>    
    </tr>    
    <tr>
        <td>Angličtina</td>        
        <td>en.microsoft</td>
        <td>en.lucene</td>        
    </tr>
    <tr>
        <td>Estonština</td>
        <td>et.microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>finština</td>
        <td>Fi.microsoft</td>
        <td>Fi.lucene</td>        
    </tr>    
    <tr>
        <td>francouzština</td>
        <td>FR.microsoft</td>
        <td>FR.lucene</td>        
    </tr>
    <tr>
        <td>Galicijština</td>
        <td></td>
        <td>GL.lucene</td>        
      </tr>
    <tr>
        <td>němčina</td>
        <td>de.microsoft</td>
        <td>de.lucene</td>        
    </tr>
    <tr>
        <td>řečtina</td>
        <td>El.microsoft</td>
        <td>El.lucene</td>        
    </tr>
    <tr>
        <td>Gudžarátština</td>
        <td>gu.microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Hebrejština</td>
        <td>he.microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Hindština</td>
        <td>Hi.microsoft</td>
        <td>Hi.lucene</td>        
    </tr>
    <tr>
        <td>maďarština</td>        
        <td>HU.microsoft</td>
        <td>HU.lucene</td>
    </tr>
    <tr>
        <td>Islandština</td>
        <td>is.microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Indonéština (Bahasa)</td>
        <td>ID.microsoft</td>
        <td>ID.lucene</td>        
    </tr>
    <tr>
        <td>Irština</td>
        <td></td>
          <td>GA.lucene</td>
    </tr>
    <tr>
        <td>italština</td>
        <td>IT.microsoft</td>
        <td>IT.lucene</td>        
    </tr>
    <tr>
        <td>japonština</td>
        <td>ja.microsoft</td>
        <td>ja.lucene</td>

    </tr>
    <tr>
        <td>Kannadština</td>
        <td>Ka.microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>korejština</td>
        <td>Ko.microsoft</td>
        <td>Ko.lucene</td>
    </tr>
    <tr>
        <td>Lotyština</td>        
        <td>LV.microsoft</td>
        <td>LV.lucene</td>    
    </tr>
    <tr>
        <td>Litevština</td>
        <td>lt.microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Malajalámština</td>
        <td>ml.microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Malajština (Latina)</td>
        <td>MS.microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Maráthština</td>
        <td>MR.microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>norština</td>
        <td>NB.microsoft</td>
        <td>No.lucene</td>        
    </tr>
      <tr>
        <td>Perština</td>
        <td></td>
        <td>fa.lucene</td>        
      </tr>
    <tr>
        <td>polština</td>
        <td>PL.microsoft</td>
        <td>PL.lucene</td>        
    </tr>
    <tr>
        <td>Portugalština (Brazílie)</td>
        <td>PT-Br.microsoft</td>
        <td>PT-Br.lucene</td>        
    </tr>
    <tr>
        <td>Portugalština (Portugalsko)</td>
        <td>PT-Pt.microsoft</td>        
        <td>PT-Pt.lucene</td>
    </tr>
    <tr>
        <td>Paňdžábština</td>
        <td>Pa.microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>rumunština</td>
        <td>ro.microsoft</td>
        <td>ro.lucene</td>
    </tr>
    <tr>
        <td>ruština</td>
        <td>RU.microsoft</td>
        <td>RU.lucene</td>    
    </tr>
    <tr>
        <td>Srbština (cyrilice)</td>
        <td>SR-cyrillic.microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Srbština (Latina)</td>
        <td>SR-latin.microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Slovenština</td>
        <td>Sk.microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Slovinština</td>
        <td>SL.microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>španělština</td>
        <td>ES.microsoft</td>
        <td>ES.lucene</td>
    </tr>
    <tr>
        <td>švédština</td>
        <td>Sv.microsoft</td>
        <td>Sv.lucene</td>
    </tr>

    <tr>
        <td>Tamilština</td>
        <td>ta.microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Telugština</td>
        <td>te.microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Thajština</td>
        <td>TH.microsoft</td>
        <td>TH.lucene</td>
    </tr>
    <tr>
        <td>turečtina</td>
        <td>TR.microsoft</td>
        <td>TR.lucene</td>        
    </tr>
    <tr>
        <td>Ukrajinština</td>
        <td>UK.microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Urdština</td>
        <td>Your.microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Vietnamština</td>
        <td>VI.microsoft</td>
        <td></td>
    </tr>
    <td colspan="3">Kromě toho poskytuje Azure Search bez ohledu na jazyk analyzátor konfigurace</td>
    <tr>
        <td>Skládání standardní ASCII</td>
        <td>standardasciifolding.lucene</td>
        <td>
        <ul>
            <li>Segmentace text Unicode (standardní Tokenizátor)</li>
            <li>Skládání filtru ASCII – převede znaky znakové sady Unicode, které nepatří do sady nejprve 127 znaků ASCII do jejich ekvivalenty ASCII. To je užitečné pro odebrání znaky s diakritikou.</li>
        </ul>
        </td>
    </tr>
</table>

Všechny analyzátory s názvy opatřen poznámkou <i>lucene</i> se používá technologii [analyzátory jazyka Apache Lucene](http://lucene.apache.org/core/4_9_0/analyzers-common/overview-summary.html). Další informace o ASCII skládání filtru najdete [zde](http://lucene.apache.org/core/4_9_0/analyzers-common/org/apache/lucene/analysis/miscellaneous/ASCIIFoldingFilter.html).

**Moduly pro návrhy**

A `suggester` definuje pole v indexu, která se používají pro podporu automatického dokončování v hledání. Obvykle jsou odesílány částečné vyhledávání řetězce [návrhy API](#Suggestions) při uživatele je zadáním vyhledávací dotaz a rozhraní API vrací sadu navrhované frází. Modulu pro návrhy, kterou definujete v indexu určuje pole, která slouží k vytváření našeptávání hledaným výrazům. V tématu [trochu](#Suggesters) podrobnosti o konfiguraci.

**Profily skórování**

A `scoringProfile` definuje vlastní vyhodnocování chování, které umožňují ovlivnit položky zobrazovat na vyšších pozicích ve výsledcích hledání. Vyhodnocování profily jsou tvořeny váhu pole a funkce. Jejich použití, zadejte profil podle názvu na řetězec dotazu.

Výchozí profil vyhodnocování funguje na pozadí k výpočtu skóre vyhledávání pro každou položku v sadě výsledků. Můžete použít interní, nepojmenované profil vyhodnocování. Alternativně nastavte `defaultScoringProfile` chcete použít vlastní profil jako výchozí, vyvolá vždy, když není zadaný vlastního profilu v řetězci dotazu.

V tématu [vyhodnocování profily přidat do indexu vyhledávání (služby REST API Azure Search)](search-api-scoring-profiles-2015-02-28-preview.md) podrobnosti.

**Možnosti CORS**

Javascript na straně klienta nelze volat všechny rozhraní API ve výchozím nastavení, protože prohlížeč zabrání všech žádostí napříč zdroji. Povolení CORS (sdílení prostředků různého původu) nastavením `corsOptions` atribut tak, aby dotazy nepůvodního zdroje do indexu. Všimněte si, že pouze dotaz rozhraní API pro podporu CORS z bezpečnostních důvodů. Tyto možnosti můžete nastavit pro CORS:

* `allowedOrigins`(povinné): Toto je seznam původů, které bude udělen přístup k indexu. To znamená, že žádný kód Javascript zpracování těchto původem se bude moct dotazování indexu (za předpokladu, že poskytuje správný klíč rozhraní API). Každý původ je obvykle ve formátu `protocol://fully-qualified-domain-name:port` i když port je často vynechán. V tématu [v tomto článku](http://go.microsoft.com/fwlink/?LinkId=330822) další podrobnosti.
  * Pokud chcete povolit přístup k všechny původy, zahrnují `*` jako jedna položka v `allowedOrigins` pole. Všimněte si, že **to není doporučeno, postup pro produkční vyhledávací služby.** Však může být užitečné pro vývoj nebo účely ladění.
* `maxAgeInSeconds`(volitelné): prohlížeče tato hodnota slouží k určení doby (v sekundách) k předběžných odpovědí CORS mezipaměti. Toto musí být nezáporné celé číslo. Čím větší je tato hodnota, tím lepší bude výkon, ale tím déle bude taky trvat se projeví změny zásad CORS. Pokud není nastavena, použije se výchozí hodnota 5 minut.

<a name="CreateUpdateIndexExample"></a>
**Příklad text žádosti**

    {
      "name": "hotels",  
      "fields": [
        {"name": "hotelId", "type": "Edm.String", "key": true, "searchable": false},
        {"name": "baseRate", "type": "Edm.Double"},
        {"name": "description", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false},
        {"name": "description_fr", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false, "analyzer": "fr.lucene"},
        {"name": "hotelName", "type": "Edm.String"},
        {"name": "category", "type": "Edm.String"},
        {"name": "tags", "type": "Collection(Edm.String)"},
        {"name": "parkingIncluded", "type": "Edm.Boolean"},
        {"name": "smokingAllowed", "type": "Edm.Boolean"},
        {"name": "lastRenovationDate", "type": "Edm.DateTimeOffset"},
        {"name": "rating", "type": "Edm.Int32"},
        {"name": "location", "type": "Edm.GeographyPoint"}
      ],
      "suggesters": [
        {
          "name": "sg",
          "searchMode": "analyzingInfixMatching",
          "sourceFields": ["hotelName"]
        }
      ]
    }

**Odpověď**

Pro úspěšné žádosti: "201 – vytvořeno".

Ve výchozím nastavení bude obsahovat text odpovědi JSON pro definici indexu, která byla vytvořena. Pokud `Prefer` hlavička požadavku je nastaven na `return=minimal`, text odpovědi bude prázdný, a bude kód stavu úspěch "204 žádný obsah" místo "201 – vytvořeno". To platí bez ohledu na to, jestli PUT nebo POST byla použita k vytvoření indexu.

**Poznámky**

V současné době je omezená podpora aktualizace schématu indexu. Všechny aktualizace schémat, které by vyžadovaly přeindexování například změny typu polí nejsou aktuálně podporovány. Ale existující pole nemůžete změnit ani odstranit, můžete přidat nová pole do stávajícího indexu kdykoli. Při přidání nové pole, všechny stávající dokumenty v indexu bude mít automaticky pro toto pole hodnotu null. Žádné další úložiště budou využívat, dokud jsou přidána nových dokumentů do indexu.

<a name="Suggesters"></a>

## <a name="suggesters"></a>Moduly pro návrhy
Návrhy funkce ve službě Azure Search je funkce našeptávání nebo automatické dokončování dotazů, které poskytuje seznam potenciální hledaný text v reakci na částečné řetězce vstupy do vyhledávacího pole. Pravděpodobně jste zaznamenali dotazů při používání komerční vyhledávací stroje: zadáním ".NET" v Bing vytvoří seznam podmínek pro ".NET 4.5", "rozhraní .NET Framework 3,5", a tak dále. Pokud používáte službu vyhledávání REST API, implementace návrhy ve vlastní aplikaci Azure Search vyžaduje následující:

* Povolit návrhy přidáním **modulu pro návrhy** konstrukce v indexu, poskytnutí názvu, režim hledání a seznam polí, pro kterou našeptávání je volána. Například pokud zadáte "mesto" jako zdrojové pole zadáte řetězec částečné vyhledávání "Sea" bude mít za následek "Seattle", "Pobřežního" a "Seatac" (všechny tři, jsou názvy skutečné města) jako dotazů nabízen uživateli.
* Vyvolání návrhy voláním [API návrhy](#Suggestions) v kódu aplikace. Částečné řetězce jsou obvykle při uživatele je zadáním vyhledávací dotaz a toto rozhraní API vrací sadu navrhované frází odešle do služby.

Tento článek vysvětluje, jak nakonfigurovat **modulu pro návrhy**. Také byste měli revidovat [návrhy API](#Suggestions) podrobnosti o tom, jak se používá modulu pro návrhy.

**Využití**

`Suggesters`jsou vytvořené v indexu a pracovní nejlépe, když se použije pro návrh konkrétní dokumenty místo přijít podmínky či fráze. Pole nejlepší candidate jsou produktů, názvů a jiné poměrně krátké věty, které můžete identifikovat položku. Méně účinné jsou opakovaných pole, kategorie a značky, nebo velmi dlouhé pole jako pole popisy nebo komentáře.

Jako součást definice indexu, můžete přidat jednoho modulu pro návrhy na `suggesters` kolekce. Vlastnosti, které definují modulu pro návrhy zahrnují následující:

* `name`: Název modulu pro návrhy. Při volání metody použijete název modulu pro návrhy `suggest` rozhraní API.
* `searchMode`: Strategie použitá k vyhledání kandidátských frází. Jediný momentálně podporovaný režim je `analyzingInfixMatching`, který provádí flexibilní porovnávání frází na začátku nebo uprostřed vět.
* `sourceFields`: Seznam minimálně jedno pole, které jsou zdroj obsahu pro návrhy. Pouze pole typu `Edm.String` a `Collection(Edm.String)` může být zdrojů pro návrhy. Lze použít pouze pole, které nemají vlastní analyzátor jazyka nastavit.

**Příklad modulu pro návrhy**

Modulu pro návrhy je součástí indexu. V může existovat pouze jedna modulu pro návrhy `suggesters` kolekce v aktuální verzi, spolu s kolekci polí a `scoringProfiles`.

        {
          "name": "hotels",
          "fields": [
             . . .
           ],
          "suggesters": [
            {
            "name": "sg",
            "searchMode": "analyzingInfixMatching",
            "sourceFields: ["hotelName", "category"]
            }
          ],
          "scoringProfiles": [
             . . .
          ]
        }

> [!NOTE]
> Pokud jste použili verze verzi public preview služby Azure Search, `suggesters` nahrazuje starší logická vlastnost (`"suggestions": false`), předpona návrhy podporována pouze pro krátké řetězce (3-25 znaků). Čím jsou nahrazené, `suggesters`, podporuje infix odpovídající, který vyhledá odpovídající podmínky na začátku nebo uprostřed pole obsahu, lepší toleranci chyb v řetězci pro vyhledávání. Od verze všeobecně dostupná, to je nyní pouze implementace návrhy rozhraní API. Starší `suggestions` vlastnost, která byla zavedena v `api-version=2014-07-31-Preview` dál v této verzi fungovat, ale není v provozu `2015-02-28` nebo novější verze Azure Search.
> 
> 

<a name="UpdateIndex"></a>

## <a name="update-index"></a>Aktualizovat Index
Můžete aktualizovat existující index v rámci služby Azure Search pomocí požadavek HTTP PUT. Aktualizace můžou zahrnovat přidávání nové pole do stávajícího schématu, úprava možnosti CORS a úprava profily vyhodnocování. V tématu [přidat vyhodnocování profily](https://msdn.microsoft.com/library/azure/dn798928.aspx) Další informace. Zadáte název indexu při aktualizaci v identifikátoru URI požadavku:

    PUT https://[search service url]/indexes/[index name]?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin key]

**Důležité:** podpora pro aktualizace schématu indexu je omezena na operace, které nevyžadují nové sestavení indexu vyhledávání. Všechny aktualizace schémat, které by vyžadovaly přeindexování, například změny typu polí nejsou aktuálně podporovány. Kdykoli můžete přidat nová pole, ale existující pole nemůžete změnit ani odstranit. Totéž platí i pro `suggesters`. Nová pole, mohou být přidány do modulu pro návrhy v době jsou přidány pole, ale pole nelze odebrat z `suggesters` a existující pole nelze přidat do `suggesters`.

Při přidávání nové pole do indexu, všechny stávající dokumenty v indexu bude mít automaticky pro toto pole hodnotu null. Žádné další úložiště budou využívat, dokud jsou přidána nových dokumentů do indexu.

**Požadavek**

Je požadován pro všechny žádosti o služby protokol HTTPS. **Aktualizace indexu** požadavku je vytvořený pomocí HTTP PUT. Pomocí PUT název indexu je součástí adresy URL. Pokud index neexistuje, vytvoří se. Pokud už existuje index, se aktualizuje na novou definici.

Název indexu musí být malá písmena, začínat písmenem nebo číslicí, mít žádné lomítka nebo tečky a být kratší než 128 znaků. Po spuštění název indexu s písmenem nebo číslicí, může obsahovat zbytek název jakékoli písmeno, čísla a pomlčky, dokud nejsou po sobě jdoucí pomlčky.

`api-version=[string]`(povinné). Verze preview je `api-version=2015-02-28-Preview`. V tématu [verze služby vyhledávání](http://msdn.microsoft.com/library/azure/dn864560.aspx) podrobnosti a alternativní verze.

**Hlavičky požadavku**

Následující seznam popisuje hlavičky žádosti požadované a volitelné.

* `Content-Type`: Vyžaduje se. Tuto možnost nastavíte na`application/json`
* `api-key`: Vyžaduje se. `api-key` Se používá k ověření požadavku na vaši službu vyhledávání. Je řetězcovou hodnotu, jedinečné pro vaši službu. **Aktualizace indexu** musí zahrnovat požadavek `api-key` záhlaví nastavit klíče správce (na rozdíl od klíč dotazů).

Budete také potřebovat názvu služby pro vytvoření adresy URL žádosti. Můžete získat název služby a `api-key` z řídicího panelu služby na portálu Azure. V tématu [vytvoření služby Azure Search na portálu](search-create-service-portal.md) nápovědu navigace stránky.

**Syntaxe požadavku textu**

Při aktualizaci existujícího indexu se text musí obsahovat původní definici schématu, a nová pole, který chcete přidat, jakož i upravené vyhodnocování profily, trochu a možnosti CORS, pokud existuje. Pokud nejsou změny možnosti vyhodnocování profily a CORS, je nutné zahrnout původní z vytvoření indexu. Obecné doporučené vzor pro aktualizace je načíst definici indexu s GET, upravte ho a potom jej aktualizovat s PUT.

Syntaxe schématu použít k vytvoření indexu je zde uveden ke zvýšení pohodlí. V tématu [Create Index](#CreateIndex) další podrobnosti.

    {
      "name": (optional) "name_of_index",
      "fields": [
        {
          "name": "name_of_field",
          "type": "Edm.String | Collection(Edm.String) | Edm.Int32 | Edm.Int64 | Edm.Double | Edm.Boolean | Edm.DateTimeOffset | Edm.GeographyPoint",
          "searchable": true (default where applicable) | false (only Edm.String and Collection(Edm.String) fields can be searchable),
          "filterable": true (default) | false,
          "sortable": true (default where applicable) | false (Collection(Edm.String) fields cannot be sortable),
          "facetable": true (default where applicable) | false (Edm.GeographyPoint fields cannot be facetable),
          "key": true | false (default, only Edm.String fields can be keys),
          "retrievable": true (default) | false, 
          "analyzer": "name of the analyzer used for search and indexing", (only if 'searchAnalyzer' and 'indexAnalyzer' are not set)
          "searchAnalyzer": "name of the search analyzer", (only if 'indexAnalyzer' is set and 'analyzer' is not set)
          "indexAnalyzer": "name of the indexing analyzer" (only if 'searchAnalyzer' is set and 'analyzer' is not set)
        }
      ],
      "suggesters": [
        {
          "name": "name of suggester",
          "searchMode": "analyzingInfixMatching" (other modes may be added in the future),
          "sourceFields": ["field1", "field2", ...]
        }
      ],
      "scoringProfiles": [
        {
          "name": "name of scoring profile",
          "text": (optional, only applies to searchable fields) {
            "weights": {
              "searchable_field_name": relative_weight_value (positive numbers),
              ...
            }
          },
          "functions": (optional) [
            {
              "type": "magnitude | freshness | distance | tag",
              "boost": # (positive number used as multiplier for raw score != 1),
              "fieldName": "...",
              "interpolation": "constant | linear (default) | quadratic | logarithmic",
              "magnitude": {
                "boostingRangeStart": #,
                "boostingRangeEnd": #,
                "constantBoostBeyondRange": true | false (default)
              },
              "freshness": {
                "boostingDuration": "..." (value representing timespan leading to now over which boosting occurs)
              },
              "distance": {
                "referencePointParameter": "...", (parameter to be passed in queries to use as reference location, see "scoringParameter" for syntax details)
                "boostingDistance": # (the distance in kilometers from the reference location where the boosting range ends)
              },
              "tag": {
                "tagsParameter": "..." (parameter to be passed in queries to specify list of tags to compare against target field, see "scoringParameter" for syntax details)
              }
            }
          ],
          "functionAggregation": (optional, applies only when functions are specified)
            "sum (default) | average | minimum | maximum | firstMatching"
        }
      ],
      "analyzers":(optional)[ ... ],
      "charFilters":(optional)[ ... ],
      "tokenizers":(optional)[ ... ],
      "tokenFilters":(optional)[ ... ],
      "defaultScoringProfile": (optional) "...",
      "corsOptions": (optional) {
        "allowedOrigins": ["*"] | ["origin_1", "origin_2", ...],
        "maxAgeInSeconds": (optional) max_age_in_seconds (non-negative integer)
      }
    }


**Odpověď**

Pro úspěšné žádosti: "204 žádný obsah".

Ve výchozím nastavení bude prázdný text odpovědi. Ale pokud `Prefer` hlavička požadavku je nastaven na `return=representation`, text odpovědi bude obsahovat JSON pro definici indexu, která byla aktualizována. V takovém případě bude kód stavu úspěch "200 OK".

**Aktualizace definice indexu s vlastní analyzátory**

Jakmile je definovány analyzátor, tokenizátor, token filtru nebo filtr char, nemůže být upraven. Nové lze přidat do existujícího indexu, pouze pokud `allowIndexDowntime` je příznak nastaven na hodnotu true v indexu žádost o aktualizaci: 

`PUT https://[search service name].search.windows.net/indexes/[index name]?api-version=[api-version]&allowIndexDowntime=true`

Všimněte si, že tato operace bude put indexu offline pro alespoň několik sekund, způsobuje vaší indexování a požadavků na dotazy k selhání. Výkon a zápisu dostupnost index může být zasažená několik minut po aktualizaci indexu nebo delší dobu velmi velký indexy.

<a name="ListIndexes"></a>

## <a name="list-indexes"></a>Seznam indexů
**Seznamu indexy** operace vrátí seznam hodnot indexy aktuálně ve službě Azure Search.

    GET https://[service name].search.windows.net/indexes?api-version=[api-version]
    api-key: [admin key]

**Požadavek**

Je požadován pro všechny žádosti o služby protokol HTTPS. **Seznamu indexy** požadavek se dá vytvořit pomocí metody GET.

`api-version=[string]`(povinné). Verze preview je `api-version=2015-02-28-Preview`. V tématu [verze služby vyhledávání](http://msdn.microsoft.com/library/azure/dn864560.aspx) podrobnosti a alternativní verze.

**Hlavičky požadavku**

Následující seznam popisuje hlavičky žádosti požadované a volitelné.

* `api-key`: Vyžaduje se. `api-key` Se používá k ověření požadavku na vaši službu vyhledávání. Je řetězcovou hodnotu, jedinečné pro vaši službu. **Seznamu indexy** musí zahrnovat požadavek `api-key` nastavit na klíč správce (na rozdíl od klíč dotazů).

Budete také potřebovat názvu služby pro vytvoření adresy URL žádosti. Můžete získat název služby a `api-key` z řídicího panelu služby na portálu Azure. V tématu [vytvoření služby Azure Search na portálu](search-create-service-portal.md) nápovědu navigace stránky.

**Text žádosti**

Žádné.

**Odpověď**

Stavový kód: 200 OK se vrátí pro úspěšné odpovědi.

Tady je odpovědi na příkladu:

    {
      "value": [
        {
          "name": "Books",
          "fields": [
            {"name": "ISBN", ...},
            ...
          ]
        },
        {
          "name": "Games",
          ...
        },
        ...
      ]
    }

Všimněte si, že můžete filtrovat odpovědi dolů pouze vlastnosti, které vás zajímají. Například pokud chcete pouze seznam názvů index, použijte prostředí OData `$select` dotazu možnost:

    GET /indexes?api-version=2015-02-28-Preview&$select=name

V takovém případě odpověď z výše uvedeném příkladu by měly vypadat následovně:

    {
      "value": [
        {"name": "Books"},
        {"name": "Games"},
        ...
      ]
    }

To je užitečné pro ušetří šířku pásma, pokud máte spoustu indexy ve vyhledávací službě.

<a name="GetIndex"></a>

## <a name="get-index"></a>Získat Index
**Získat Index** operaci získá definici indexu z Azure Search.

    GET https://[service name].search.windows.net/indexes/[index name]?api-version=[api-version]
    api-key: [admin key]

**Požadavek**

Protokol HTTPS je vyžadována pro žádosti o služby. **Získat Index** požadavek se dá vytvořit pomocí metody GET.

[Název indexu] v identifikátoru URI požadavku určuje index, který mají být vráceny od kolekce indexů.

`api-version=[string]`(povinné). Verze preview je `api-version=2015-02-28-Preview`. V tématu [verze služby vyhledávání](http://msdn.microsoft.com/library/azure/dn864560.aspx) podrobnosti a alternativní verze.

**Hlavičky požadavku**

Následující seznam popisuje hlavičky žádosti požadované a volitelné.

* `api-key`: `api-key` Se používá k ověření požadavku na vaši službu vyhledávání. Je řetězcovou hodnotu, jedinečné pro vaši službu. **Získat Index** musí zahrnovat požadavek `api-key` nastavit na klíč správce (na rozdíl od klíč dotazů).

Budete také potřebovat názvu služby pro vytvoření adresy URL žádosti. Můžete získat název služby a `api-key` z řídicího panelu služby na portálu Azure. V tématu [vytvoření služby Azure Search na portálu](search-create-service-portal.md) nápovědu navigace stránky.

**Text žádosti**

Žádné.

**Odpověď**

Stavový kód: 200 OK se vrátí pro úspěšné odpovědi.

Podívejte se na příklad JSON v [vytvářet a aktualizovat Index](#CreateUpdateIndexExample) příklad datové části odpovědi.

<a name="DeleteIndex"></a>

## <a name="delete-index"></a>Odstranit Index
**Odstranit Index** operace odebere index a související dokumenty ze služby Azure Search. Název indexu můžete získat z řídicího panelu služby na portálu Azure nebo z rozhraní API. V tématu [seznamu indexy](#ListIndexes) podrobnosti.

    DELETE https://[service name].search.windows.net/indexes/[index name]?api-version=[api-version]
    api-key: [admin key]

**Požadavek**

Protokol HTTPS je vyžadována pro žádosti o služby. **Odstranit Index** požadavek se dá vytvořit pomocí metodu DELETE.

[Název indexu] v identifikátoru URI požadavku určuje index, který chcete odstranit z kolekce indexů.

`api-version=[string]`(povinné). Verze preview je `api-version=2015-02-28-Preview`. V tématu [verze služby vyhledávání](http://msdn.microsoft.com/library/azure/dn864560.aspx) podrobnosti a alternativní verze.

**Hlavičky požadavku**

Následující seznam popisuje hlavičky žádosti požadované a volitelné.

* `api-key`: Vyžaduje se. `api-key` Se používá k ověření požadavku na vaši službu vyhledávání. Je hodnotu řetězce pro adresu URL služby jedinečné. **Odstranit Index** musí zahrnovat požadavek `api-key` záhlaví nastavit klíče správce (na rozdíl od klíč dotazů).

Budete také potřebovat názvu služby pro vytvoření adresy URL žádosti. Můžete získat název služby a `api-key` z řídicího panelu služby na portálu Azure. V tématu [vytvoření služby Azure Search na portálu](search-create-service-portal.md) nápovědu navigace stránky.

**Text žádosti**

Žádné.

**Odpověď**

: Stav 204 ne obsahu je vrácen kód pro úspěšné odpovědi.

<a name="GetIndexStats"></a>

## <a name="get-index-statistics"></a>Získat statistiku indexu
**Získat statistiku Index** operace z Azure Search vrátí počet dokumentů pro aktuální index a využití úložiště.

    GET https://[service name].search.windows.net/indexes/[index name]/stats?api-version=[api-version]
    api-key: [admin key]

> [!NOTE]
> Statistika na dokument počet a velikost úložiště se shromažďují každých několik minut, ne v reálném čase. Statistiky vrácený toto rozhraní API proto nemusí odrážet změny způsobené poslední operace indexování.
> 
> 

**Požadavek**

Je požadován pro všechny požadavky služby protokol HTTPS. **Získat statistiku Index** požadavek se dá vytvořit pomocí metody GET.

[Název indexu] v identifikátoru URI požadavku informuje službu, kterou chcete vrátit index statistiky pro zadaný index.

`api-version=[string]`(povinné). Verze preview je `api-version=2015-02-28-Preview`. V tématu [verze služby vyhledávání](http://msdn.microsoft.com/library/azure/dn864560.aspx) podrobnosti a alternativní verze.

**Hlavičky požadavku**

Následující seznam popisuje hlavičky žádosti požadované a volitelné.

* `api-key`: `api-key` Se používá k ověření požadavku na vaši službu vyhledávání. Je řetězcovou hodnotu, jedinečné pro vaši službu. **Získat statistiku Index** musí zahrnovat požadavek `api-key` nastavit na klíč správce (na rozdíl od klíč dotazů).

Budete také potřebovat názvu služby pro vytvoření adresy URL žádosti. Můžete získat název služby a `api-key` z řídicího panelu služby na portálu Azure. V tématu [vytvoření služby Azure Search na portálu](search-create-service-portal.md) nápovědu navigace stránky.

**Text žádosti**

Žádné.

**Odpověď**

Stavový kód: 200 OK se vrátí pro úspěšné odpovědi.

Text odpovědi je v následujícím formátu:

    {
      "documentCount": number,
      "storageSize": number (size of the index in bytes)
    }

<a name="TestAnalyzer"></a>

## <a name="test-analyzer"></a>Analyzátor testu
**Analyzovat rozhraní API** ukazuje, jak analyzátor dělí text do tokenů.

    POST https://[service name].search.windows.net/indexes/[index name]/analyze?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin key]

**Požadavek**

Je požadován pro všechny požadavky služby protokol HTTPS. **Analyzovat rozhraní API** požadavek se dá vytvořit pomocí metody POST.

`api-version=[string]`(povinné). Verze preview je `api-version=2015-02-28-Preview`. V tématu [verze služby vyhledávání](http://msdn.microsoft.com/library/azure/dn864560.aspx) podrobnosti a alternativní verze.

**Hlavičky požadavku**

Následující seznam popisuje hlavičky žádosti požadované a volitelné.

* `api-key`: `api-key` Se používá k ověření požadavku na vaši službu vyhledávání. Je řetězcovou hodnotu, jedinečné pro vaši službu. **Analyzovat rozhraní API** musí zahrnovat požadavek `api-key` nastavit na klíč správce (na rozdíl od klíč dotazů).

Budete také potřebovat název indexu a název služby můžete vytvořit adresu URL požadavku. Můžete získat název služby a `api-key` z řídicího panelu služby na portálu Azure. V tématu [vytvoření služby Azure Search na portálu](search-create-service-portal.md) nápovědu navigace stránky.

**Text žádosti**

    {
      "text": "Text to analyze",
      "analyzer": "analyzer_name"
    }

nebo

    {
      "text": "Text to analyze",
      "tokenizer": "tokenizer_name",
      "tokenFilters": (optional) [ "token_filter_name" ],
      "charFilters": (optional) [ "char_filter_name" ]
    }

`analyzer_name`, `tokenizer_name`, `token_filter_name` a `char_filter_name` musí být platné názvy předdefinované nebo vlastní analyzátorů, tokenizers, token filtry a filtry znak pro index. Další informace o procesu analýzy lexikální najdete [Analysis ve službě Azure Search](https://aka.ms/azsanalysis).

**Odpověď**

Stavový kód: 200 OK se vrátí pro úspěšné odpovědi.

Text odpovědi je v následujícím formátu:

    {
      "tokens": [
        {
          "token": string (token),
          "startOffset": number (index of the first character of the token),
          "endOffset": number (index of the last character of the token),
          "position": number (position of the token in the input text)
        },
        ...
      ]
    }

**Analýza příklad rozhraní API**

**Požadavek**

    {
      "text": "Text to analyze",
      "analyzer": "standard"
    }

**Odpověď**

    {
      "tokens": [
        {
          "token": "text",
          "startOffset": 0,
          "endOffset": 4,
          "position": 0
        },
        {
          "token": "to",
          "startOffset": 5,
          "endOffset": 7,
          "position": 1
        },
        {
          "token": "analyze",
          "startOffset": 8,
          "endOffset": 15,
          "position": 2
        }
      ]
    }

- - -
<a name="DocOps"></a>

## <a name="document-operations"></a>Operace dokumentu
Ve službě Azure Search index je uložená v cloudu a vyplněny dokumentů JSON, které odešlete do této služby. Všechny dokumenty, které můžete nahrávat na server tvoří souhrnu dat hledání. Dokumenty obsahují pole, z nichž některé jsou tokenizovaného do hledaných termínů jako jejich uložení. `/docs` Segment adresy URL v rozhraní API služby Azure Search představuje kolekci dokumentů v indexu. Všechny operace provést na kolekci, například odeslání, sloučení, odstranění nebo dotazování dokumentů proveďte umístit v rámci jedné index, proto adresy URL pro tyto operace bude vždy začínat `/indexes/[index name]/docs` pro název daného indexu.

Kód aplikace musíte vygenerovat buď dokumentů JSON nahrát do služby Azure Search, nebo můžete použít [indexer](https://msdn.microsoft.com/library/dn946891.aspx) načíst dokumenty, pokud je zdroj dat databázi SQL Azure nebo Azure Cosmos DB. Obvykle indexy budou naplněny z jedné datové sady, které zadáte.

Měli byste mít jeden dokument pro každou položku, kterou chcete vyhledat. Film pronájem aplikace může mít jeden dokument na film, storefront aplikace může mít jeden dokument za SKU, online výukových kurzů aplikace může mít jeden dokument za kurzu, firma research může mít jeden dokument pro každý academic dokumentu v jejich úložiště a tak dále.

Dokumenty obsahovat jeden či více polí. Pole může obsahovat text, který je tokenizovaného do podmínek vyhledávání ve službě Azure Search, jakož i bez tokenizovaného nebo jiné než textové hodnoty, které mohou být používány filtry, nebo profily vyhodnocování. Názvy, datové typy a funkce vyhledávání, které jsou podporovány pro každé pole určuje schéma indexu. Jedno z polí ve schématu každý index musí být určeny jako ID a každý dokument musí mít hodnotu v poli ID, která jednoznačně identifikuje tento dokument v indexu. Všechna ostatní pole dokumentu jsou volitelné a použije výchozí hodnotu null, pokud nezadaný. Všimněte si, že hodnoty null nemusí provádět žádné místo v indexu vyhledávání.

Předtím, než můžete odesílat dokumenty, musí již jste vytvořili index ve službě. V tématu [Create Index](#CreateIndex) podrobnosti o tento první krok.

<a name="AddOrUpdateDocuments"></a>

## <a name="add-update-or-delete-documents"></a>Přidání, aktualizace nebo odstranění dokumentů
Můžete nahrát, sloučení, sloučení nebo odeslání nebo odstranění dokumentů ze zadaného indexu pomocí HTTP POST. Pro velké množství aktualizací se doporučuje dávkování dokumentů až (1000 dokumentů na jednu dávku) nebo o 16 MB na jednu dávku.

    POST https://[service name].search.windows.net/indexes/[index name]/docs/index?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin key]

**Požadavek**

Je požadován pro všechny žádosti o služby protokol HTTPS. Můžete nahrát, sloučení, sloučení nebo odeslání nebo odstranění dokumentů ze zadaného indexu pomocí HTTP POST.

[Název indexu] obsahuje identifikátoru URI požadavku zadání index, který o odeslání dokumentů. Najednou můžete pouze odeslání dokumentů pro jeden index.

`api-version=[string]`(povinné). Verze preview je `api-version=2015-02-28-Preview`. V tématu [verze služby vyhledávání](http://msdn.microsoft.com/library/azure/dn864560.aspx) podrobnosti a alternativní verze.

**Hlavičky požadavku**

Následující seznam popisuje hlavičky žádosti požadované a volitelné.

* `Content-Type`: Vyžaduje se. Tuto možnost nastavíte na`application/json`
* `api-key`: Vyžaduje se. `api-key` Se používá k ověření požadavku na vaši službu vyhledávání. Je řetězcovou hodnotu, jedinečné pro vaši službu. **Přidat dokumenty** musí zahrnovat požadavek `api-key` záhlaví nastavit klíče správce (na rozdíl od klíč dotazů).

Budete také potřebovat názvu služby pro vytvoření adresy URL žádosti. Můžete získat název služby a `api-key` z řídicího panelu služby na portálu Azure. V tématu [vytvoření služby Azure Search na portálu](search-create-service-portal.md) nápovědu navigace stránky.

**Text žádosti**

Text žádosti obsahuje jeden nebo více dokumentů indexovaných. Dokumenty jsou identifikovány jedinečný klíč. Každý dokument je přidružené akci: odeslání, sloučení, mergeOrUpload nebo odstranit. Žádostí o nahrání musí obsahovat data dokumentu jako sada párů klíč/hodnota.

    {
      "value": [
        {
          "@search.action": "upload (default) | merge | mergeOrUpload | delete",
          "key_field_name": "unique_key_of_document", (key/value pair for key field from index schema)
          "field_name": field_value (key/value pairs matching index schema)
            ...
        },
        ...
      ]
    }

> [!NOTE]
> Dokument klíče může obsahovat pouze písmena, číslice, pomlčky ("-"), podtržítka ("_") a symboly rovná se ("="). Další podrobnosti najdete v tématu [pravidla po pojmenování](https://msdn.microsoft.com/library/azure/dn857353.aspx).
> 
> 

**Akce dokumentu**

* `upload`: Nahrávání akce je podobná "upsert", kde bude dokument vložený, pokud je nový a aktualizovaný nebo nahrazený, pokud existuje. Všimněte si, že všechna pole jsou nahrazena v případě, že aktualizace.
* `merge`: Sloučení aktualizuje stávající dokument se zadanými poli. Pokud dokument neexistuje, sloučení selže. Každé pole zadané ve sloučení nahradí stávající pole v dokumentu. To zahrnuje i pole typu `Collection(Edm.String)`. Například pokud dokument obsahuje pole "značky" s hodnotou `["budget"]` a vy spustíte sloučení s hodnotou `["economy", "pool"]` pro "značky", bude konečná hodnota pole "značky" `["economy", "pool"]`. Zruší **není** být `["budget", "economy", "pool"]`.
* `mergeOrUpload`: chová jako `merge` Pokud již dokument s daným klíčem v indexu existuje. Pokud dokument neexistuje, chová se jako `upload` s novým dokumentem.
* `delete`: Odstranění odebere z indexu zadaný dokument. Všimněte si, že všechna zadaná pole v `delete` operaci kromě pole klíče budou ignorovány. Pokud chcete odebrat z dokumentu jednotlivá pole, použijte `merge` místo a jednoduše nastavte hodnotu pole explicitně na `null`.

**Odpověď**

Pro úspěšné odpovědi, což znamená, že všechny položky byly úspěšně indexované je vrátit stavovým kódem 200 (OK). To vyplývá z `status` vlastnost je nastavená na hodnotu true pro všechny položky, stejně jako `statusCode` nastavenou na hodnotu 201 (pro nově nahraném dokumenty) nebo 200 (pro sloučené nebo odstraněné dokumenty):

    {
      "value": [
        {
          "key": "unique_key_of_new_document",
          "status": true,
          "errorMessage": null,
          "statusCode": 201
        },
        {
          "key": "unique_key_of_merged_document",
          "status": true,
          "errorMessage": null,
          "statusCode": 200
        },
        {
          "key": "unique_key_of_deleted_document",
          "status": true,
          "errorMessage": null,
          "statusCode": 200
        }
      ]
    }  

Stavový kód 207 (více Status) je vrácena, pokud nebyla alespoň jedna položka úspěšně indexovaná. Položky, které nebyly indexovány mají `status` pole nastaveno na hodnotu false. `errorMessage` a `statusCode` vlastnosti označí důvod pro indexování chybu:

    {
      "value": [
        {
          "key": "unique_key_of_document_1",
          "status": false,
          "errorMessage": "The search service is too busy to process this document. Please try again later.",
          "statusCode": 503
        },
        {
          "key": "unique_key_of_document_2",
          "status": false,
          "errorMessage": "Document not found.",
          "statusCode": 404
        },
        {
          "key": "unique_key_of_document_3",
          "status": false,
          "errorMessage": "Index is temporarily unavailable because it was updated with the 'allowIndexDowntime' flag set to 'true'. Please try again later.",
          "statusCode": 422
        }
      ]
    }  

Následující tabulka vysvětluje různé každý dokument stavové kódy, které mohou být vráceny v odpovědi. Mějte na paměti některé naznačují potíže s na žádost, zatímco ostatní označují dočasné chybový stav. K tomu, které by měl opakovat po prodlevě.

<table style="font-size:12">
    <tr>
        <th>Stavový kód</th>
        <th>Význam</th>
        <th>Opakovatelná</th>
        <th>Poznámky</th>
    </tr>
    <tr>
        <td>200</td>
        <td>Dokument byl úspěšně upravit nebo odstranit.</td>
        <td>neuvedeno</td>
        <td>Operace odstranění se <a href="https://en.wikipedia.org/wiki/Idempotence">idempotent</a>. To znamená i v případě, že klíč dokumentu v indexu neexistuje, pokusu o operaci odstranění s tímto klíčem bude mít za následek 200 stavový kód.</td>
    </tr>
    <tr>
        <td>201</td>
        <td>Dokument byl úspěšně vytvořen.</td>
        <td>neuvedeno</td>
        <td></td>
    </tr>
    <tr>
        <td>400</td>
        <td>V dokumentu, který brání probíhá indexování došlo k chybě.</td>
        <td>Ne</td>
        <td>Chybová zpráva v odpovědi bude určení toho, co se v dokumentu.</td>
    </tr>
    <tr>
        <td>404</td>
        <td>Dokument nelze sloučit, protože neexistuje daným klíčem v indexu.</td>
        <td>Ne</td>
        <td>Této chybě nedochází pro nahrávání vzhledem k tomu, že se vytváření nových dokumentů a nespustí se pro odstranění vzhledem k tomu, že jsou <a href="https://en.wikipedia.org/wiki/Idempotence">idempotent</a>.</td>
    </tr>
    <tr>
        <td>409</td>
        <td>Při pokusu o indexu dokument byl zjištěn konflikt verzí.</td>
        <td>Ano</td>
        <td>Tomu může dojít, když se pokoušíte více než jednou souběžně indexu stejného dokumentu.</td>
    </tr>
    <tr>
        <td>422</td>
        <td>Index je dočasně nedostupný, protože byla aktualizována 'allowIndexDowntime' příznak nastaven na hodnotu "true".</td>
        <td>Ano</td>
        <td></td>
    </tr>
    <tr>
        <td>503</td>
        <td>Vyhledávací služba je dočasně nedostupná, pravděpodobně z důvodu velké zatížení.</td>
        <td>Ano</td>
        <td>Váš kód čekat, než v tomto případě nebo riziko prodloužit nedostupnost služby.</td>
    </tr>
</table> 

**Poznámka:**: Pokud váš klientský kód často zaznamená 207 odpověď, jedním z možných důvodů je, že systém je zatížení. Můžete to ověřit kontrolou `statusCode` vlastnost 503. Pokud je to tento případ, doporučujeme ***omezení indexování požadavky***. Jinak hodnota Pokud indexování provozu nepodporuje subside, systém může spustit odmítat všechny požadavky s 503 chyby.

Kód stavu 429 označuje, že jste překročili kvótu pro počet dokumentů na index. Musíte vytvořit nový index nebo upgrade na vyšší limity kapacity.

**Příklad:**

    {
      "value": [
        {
          "@search.action": "upload",
          "hotelId": "1",
          "baseRate": 199.0,
          "description": "Best hotel in town",
          "description_fr": "Meilleur hôtel en ville",
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
          "description_fr": "Hôtel le moins cher en ville",
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
          "@search.action": "merge",
          "hotelId": "3",
          "baseRate": 279.99,
          "description": "Surprisingly expensive",
          "lastRenovationDate": null
        },
        {
          "@search.action": "delete",
          "hotelId": "4"
        }
      ]
    }
- - -
<a name="SearchDocs"></a>

## <a name="search-documents"></a>Vyhledávání dokumentů
A **vyhledávání** operace se objeví jako požadavek GET nebo POST a určuje parametry, které poskytují kritéria pro výběr odpovídající dokumenty.

    GET https://[service name].search.windows.net/indexes/[index name]/docs?[query parameters]
    api-key: [admin or query key]

    POST https://[service name].search.windows.net/indexes/[index name]/docs/search?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin or query key]

**Kdy použít POST místo GET**

Při použití metody GET protokolu HTTP k volání **vyhledávání** rozhraní API, musíte vědět, že nesmí překročit délka adresy URL žádosti 8 KB. Je to obvykle dostatečně pro většinu aplikací. Některé aplikace však vytvořit velmi velké dotazy nebo výrazy filtru OData. Pro tyto aplikace pomocí HTTP POST je lepší volbou, protože umožňuje větší filtry a dotazy, než metoda GET. S POST počet termíny nebo klauzule v dotazu je omezující faktor, ne podle velikosti vytvořeného nezpracovaná dotaz vzhledem k tomu, že je přibližně 16 MB omezení velikosti pro metodu POST.

> [!NOTE]
> I když maximální velikost požadavku POST je velmi velké, nemůže být libovolně komplexní dotazy vyhledávání a výrazy filtru. V tématu [syntaxe dotazů Lucene](https://msdn.microsoft.com/library/mt589323.aspx) a [syntaxe výrazu OData](https://msdn.microsoft.com/library/dn798921.aspx) pro další informace o omezeních složitost vyhledávání dotazu a filtr.
> 
> 

**Požadavek**

Protokol HTTPS je vyžadována pro žádosti o služby. **Vyhledávání** požadavek se dá vytvořit pomocí metody GET nebo POST.

Identifikátoru URI požadavku určuje index, který do dotazu, pro všechny dokumenty, které odpovídají parametry. V řetězci dotazu v případě požadavky GET jsou zadány parametry a v žádosti subjekt v případě POST požadavky.

Jako osvědčený postup při vytváření požadavky GET, nezapomeňte [kódování URL](https://msdn.microsoft.com/library/system.uri.escapedatastring.aspx) parametry specifického dotazu při přímé volání rozhraní REST API. Pro **vyhledávání** operace, to zahrnuje:

* `$filter`
* `facet`
* `highlightPreTag`
* `highlightPostTag`
* `search`
* `moreLikeThis`

Kódování URL se doporučuje jenom na výše uvedené parametry dotazu. Pokud jste omylem kódování URL řetězec dotazu celý (vše za?), by došlo k přerušení požadavky.

Navíc kódování URL je nutné pouze při volání rozhraní REST API přímo pomocí GET. Žádné kódování URL je nezbytné při volání metody **vyhledávání** pomocí POST, nebo při použití [klientské knihovny .NET](https://msdn.microsoft.com/library/dn951165.aspx), který zpracovává kódování URL za vás.

<a name="SearchQueryParameters"></a>
**Parametry dotazu**

**Hledání** přijímá několik parametrů, které poskytují kritéria dotazu a také určit chování vyhledávání. Zadejte tyto parametry v adrese URL řetězec dotazu při volání metody **vyhledávání** prostřednictvím GET a jako vlastnosti JSON v textu požadavku při volání metody **vyhledávání** přes POST. Syntaxe pro některé parametry se mírně liší mezi GET a POST. Tyto rozdíly jsou popsány podle vhodnosti níže:

`search=[string]`(volitelné) - text k vyhledání. Všechny `searchable` prohledány jsou ve výchozím nastavení není-li `searchFields` je zadán. Při hledání `searchable` tokenizovaného polí a vlastní text vyhledávání, tak více podmínek je možné oddělit prázdné znaky (například: `search=hello world`). Vyhledat všechny termín, použijte `*` (může to být užitečné pro dotazy Booleovský filtr). Tento parametr vynechá má stejný účinek jako jeho nastavení na hodnotu `*`. V tématu [jednoduchá syntaxe dotazů](https://msdn.microsoft.com/library/dn798920.aspx) pro konkrétní na syntaxe vyhledávání.

* **Poznámka:**: výsledky mohou být někdy překvapivé při dotazování přes `searchable` pole. Tokenizátor obsahuje logiku pro zpracování případů, které jsou společné pro angličtinu text jako apostrofy, čárky ve čísel atd. Například `search=123,456` bude shodovat s jeden termín 123,456 místo jednotlivých podmínky 123 a 456, protože čárkami jsou použity jako oddělovače tisíc pro velké počty v angličtině. Z tohoto důvodu doporučujeme používat mezer spíše než interpunkce oddělit podmínkami `search` parametr.

`searchMode=any|all`(volitelné, použije se výchozí hodnota `any`) – ať některého nebo všech hledané termíny musí odpovídat k sečtení dokumentu jako shoda.

`searchFields=[string]`(volitelné) – seznam názvů oddělených čárkami pole pro vyhledávání pro zadaný text. Cílová pole. musí být označen jako `searchable`.

`queryType=simple|full`(volitelné, použije se výchozí hodnota `simple`) – Pokud je nastaven na "jednoduchý" hledaný text interpretovat pomocí jednoduchého dotazovací jazyk, který umožňuje pro symboly, například +, * a "". Dotazy jsou vyhodnocovány napříč všechna prohledatelná pole (nebo pole uvedené v `searchFields`) v každém dokumentu ve výchozím nastavení. Když je typ dotazu nastavený na `full` hledaný text interpretována pomocí jazyka dotazů Lucene, což umožňuje specifické pole a vyvážené hledání. V tématu [jednoduchá syntaxe dotazů](https://msdn.microsoft.com/library/dn798920.aspx) a [syntaxe dotazů Lucene](https://msdn.microsoft.com/library/mt589323.aspx) pro konkrétní na syntaxe vyhledávání. 

> [!NOTE]
> Rozsah vyhledávání v Lucene dotazovací jazyk nepodporuje považuje $filter který nabízí podobné funkce.
> 
> 

`moreLikeThis=[key]`(volitelné) **Důležité:** tato funkce je k dispozici v `2015-02-28-Preview`. Tuto možnost nelze použít v dotazu, který obsahuje parametr hledání textu `search=[string]`. `moreLikeThis` Parametr Vyhledá dokumenty, které jsou podobné dokumentu určeného klíč dokumentu. Při hledání požadavku s `moreLikeThis`, vygeneruje se seznam podmínek vyhledávání, na základě frekvence a vzácnost podmínek ve zdrojovém dokumentu. Tyto podmínky jsou poté použít k vytvoření žádosti. Ve výchozím nastavení, obsah všech `searchable` pole jsou považovány za Pokud `searchFields` se používá k omezení, které prohledány.  

`$skip=#`(volitelné) – počet výsledků hledání tak, aby přeskočil; Nemůže být vyšší než 100 000. Pokud potřebujete ke kontrole dokumentů v pořadí, ale nemůžete použít `$skip` kvůli tomuto omezení, zvažte použití `$orderby` na klíč zcela seřazené a `$filter` s rozsahem dotazu místo.

> [!NOTE]
> Při volání metody **vyhledávání** pomocí POST, tento parametr je s názvem `skip` místo `$skip`.
> 
> 

`$top=#`(volitelné) – počet výsledků hledání pro načtení. To lze použít ve spojení s `$skip` implementaci klienta stránkování výsledků vyhledávání.

> [!NOTE]
> Při volání metody **vyhledávání** pomocí POST, tento parametr je s názvem `top` místo `$top`.
> 
> 

`$count=true|false`(volitelné, použije se výchozí hodnota `false`)-určuje, jestli se načíst celkový počet výsledků. Toto je počet všechny dokumenty, které odpovídají `search` a `$filter` parametry, ignoruje `$top` a `$skip`. Nastavení této hodnoty na `true` může mít dopad na výkon. Všimněte si, že vrácený počet je sblížení.

> [!NOTE]
> Při volání metody **vyhledávání** pomocí POST, tento parametr je s názvem `count` místo `$count`.
> 
> 

`$orderby=[string]`(volitelné) – seznam výrazů textový soubor s oddělovači na výsledky seřaďte podle. Každý výraz může být název pole nebo volání `geo.distance()` funkce. Každý výraz může následovat `asc` uvést vzestupné řazení a `desc` k označení sestupném. Výchozí hodnota je vzestupné pořadí. TIES bude rozděleno podle skóre shodu dokumentů. Pokud žádné `$orderby` je zadána, je výchozí pořadí řazení sestupně podle skóre shodu dokumentu. Existuje limit 32 klauzule pro `$orderby`.

> [!NOTE]
> Při volání metody **vyhledávání** pomocí POST, tento parametr je s názvem `orderby` místo `$orderby`.
> 
> 

`$select=[string]`(volitelné) – seznam polí, oddělených čárkami k načtení. Pokud tento parametr zadán, jsou zahrnuty všechny pole označené jako získat ve schématu. Můžete také explicitní žádost o všechna pole nastavením tohoto parametru na `*`.

> [!NOTE]
> Při volání metody **vyhledávání** pomocí POST, tento parametr je s názvem `select` místo `$select`.
> 
> 

`facet=[string]`(nula nebo více) – pole do omezující vlastnosti podle. Řetězec může volitelně obsahovat parametry k přizpůsobení používání faset, vyjádřené jako textový soubor s oddělovači `name:value` páry. Jsou platné parametry:

* `count`(maximální počet omezující vlastnosti podmínky; výchozí hodnota je 10). Neexistuje žádné maximální, ale vyšší hodnoty zpoplatněná odpovídající snížení výkonu, zejména pokud pole Fasetové obsahuje velký počet jedinečných podmínky.
  * Příklad: `facet=category,count:5` získá horní pěti kategorií ve výsledcích omezující vlastnosti.  
  * **Poznámka:**: Pokud `count` parametru je menší než počet jedinečných podmínky, nemusí být přesné výsledky. Příčinou je způsob používání omezujících vlastností dotazy jsou rozmístěny v horizontálních oddílů. Zvýšení `count` obvykle zvyšuje přesnost počtů termín, ale v snížený výkon.
* `sort`(jeden z `count` seřadit *sestupném* podle počtu, `-count` seřadit *vzestupném* podle počtu, `value` seřadit *vzestupném* hodnotu nebo `-value` seřadit *sestupném* podle hodnoty)
  * Příklad: `facet=category,count:3,sort:count` získá první tři kategorie ve výsledcích omezující vlastnosti v sestupném pořadí podle počtu dokumentů s každý název města. Například pokud jsou hlavní tří kategorií rozpočtu, Motel a možnost a nároky má 5 přístupů, Motel má 6 a možnost má 4, pak kbelíků bude v pořadí Motel, rozpočtu, možnost.
  * Příklad: `facet=rating,sort:-value` vytváří kbelíků pro všechny možné hodnocení v sestupném pořadí podle hodnoty. Například pokud hodnocení od 1 do 5, kbelíků bude objednáno 5, 4, 3, 2, 1, bez ohledu na to, kolik dokumenty odpovídají každé hodnocení.
* `values`(oddělený kanálu číselný nebo `Edm.DateTimeOffset` hodnoty určující dynamickou sadu hodnot omezující vlastnosti položky)
  * Příklad: `facet=baseRate,values:10|20` vytvoří tři kbelíků: jeden pro základní míra 0 až do, ale není včetně 10, jeden pro 10 až s výjimkou 20 a jeden pro 20 nebo vyšší.
  * Příklad: `facet=lastRenovationDate,values:2010-02-01T00:00:00Z` vytvoří dvě kbelíků: jeden pro hotels renovovanou před. února 2010 a jeden pro hotels renovovanou února 1. 2010 nebo novější.
* `interval`(interval celé číslo větší než 0. v případě čísel nebo `minute`, `hour`, `day`, `week`, `month`, `quarter`, `year` pro hodnoty času datum)
  * Příklad: `facet=baseRate,interval:100` vytváří kbelíků založené na základní míra rozsahy velikost 100. Například pokud základní sazby všechny až 60 $ $600, budou existovat intervaly 0-100, 100 200, 200 300, 300 400, 400-500 a 500 600.
  * Příklad: `facet=lastRenovationDate,interval:year` vytváří jeden sady pro každý rok po hotels byly renovovanou.
* `timeoffset`([+-] hh: mm, [+-] hh: mm, nebo [+-] hh) `timeoffset` je volitelný. Můžete sloučit pouze s `interval` možnost a pouze v případě, že se použije pro pole typu `Edm.DateTimeOffset`. Hodnota určuje posunem od standardu UTC čas k účtu pro nastavení času hranice.
  * Příklad: `facet=lastRenovationDate,interval:day,timeoffset:-01:00` používá den hranic, která se spouští v 01:00:00 UTC (půlnoc v časovém pásmu cíl)
* **Poznámka:**: `count` a `sort` lze spojit do stejné omezující vlastnost specifikace, ale nelze kombinovat s `interval` nebo `values`, a `interval` a `values` nemůže být společně kombinovány.
* **Poznámka:**: omezující vlastnosti Interval na datum a čas se vypočítávají podle času UTC, pokud `timeoffset` není zadán. Příklad: pro `facet=lastRenovationDate,interval:day`, den hranic začíná na 00:00:00 UTC. 

> [!NOTE]
> Při volání metody **vyhledávání** pomocí POST, tento parametr je s názvem `facets` místo `facet`. Také můžete zadat jako pole JSON řetězců, kde každý řetězec je výraz samostatné omezující vlastnosti.
> 
> 

`$filter=[string]`(volitelné) – výraz strukturovaných vyhledávání v standardní syntaxi OData.

> [!NOTE]
> Při volání metody **vyhledávání** pomocí POST, tento parametr je s názvem `filter` místo `$filter`.
> 
> 

`highlight=[string]`(volitelné) – označuje sadu názvů polí oddělených čárkami, které používá pro stiskněte klávesu. Pouze `searchable` pole lze použít pro přístupů zvýraznění.

`highlightPreTag=[string]`(volitelné, použije se výchozí hodnota `<em>`) – řetězec značky, která přidá narazí označuje. Musí být nastavena s `highlightPostTag`.

> [!NOTE]
> Při volání metody **vyhledávání** pomocí GET, vyhrazené znaky v adrese URL musí být kódovaný v procentech (například místo # % 23).
> 
> 

`highlightPostTag=[string]`(volitelné, použije se výchozí hodnota `</em>`)-tag řetězec, který připojí k průchodu označuje. Musí být nastavena s `highlightPreTag`.

> [!NOTE]
> Při volání metody **vyhledávání** pomocí GET, vyhrazené znaky v adrese URL musí být kódovaný v procentech (například místo # % 23).
> 
> 

`scoringProfile=[string]`(volitelné) – název profilu vyhodnocování vyhodnotit odpovídat skóre pro párování dokumenty k řazení výsledků.

`scoringParameter=[string]`(nula nebo více) – označuje hodnoty pro každý parametr definovaný ve funkci vyhodnocování (například `referencePointParameter`) ve formátu `name-value1,value2,...`.

* Například pokud profil vyhodnocování definuje funkci parametr s názvem "mylocation" parametr řetězce dotazu by `&scoringParameter=mylocation--122.2,44.8`. První Čárka odděluje název ze seznamu hodnotu druhý čárka je součást první hodnota (zeměpisné délky v tomto příkladu).
* Pro výpočet skóre parametry, jako je značky zvyšovat skóre, který může obsahovat čárky můžete vyhnuli tyto hodnoty v seznamu pomocí jednoduchých uvozovek a být. Pokud hodnoty samotné obsahovat jednoduché uvozovky, můžete je vyhnuli předchozího.
  * Například pokud máte značku zvyšovat skóre parametr s názvem "mytag" a vy chcete zvýšit ve značce hodnoty "Hello, O'Brien" a "Smith", dotaz možnost řetězce by `&scoringParameter=mytag-'Hello, O''Brien',Smith`. Všimněte si, že uvozovky jsou pouze požadované hodnoty, které obsahují čárkami.

> [!NOTE]
> Při volání metody **vyhledávání** pomocí POST, tento parametr je s názvem `scoringParameters` místo `scoringParameter`. Navíc je zadat jako pole JSON řetězců, kde je každý řetězec samostatné `name-values` pár.
> 
> 

`minimumCoverage`(volitelné, výchozí hodnota je 100)-číslo mezi 0 a 100 procentuální hodnota indexu, která musí být předmětem vyhledávací dotaz v pořadí pro dotaz na hlášené jako úspěšné. Ve výchozím nastavení, musí být k dispozici celý index nebo `Search` vrátí stavový kód protokolu HTTP 503. Pokud nastavíte `minimumCoverage` a `Search` úspěšné, se vrátí HTTP 200 a zahrnout `@search.coverage` hodnotu v odpovědi procentuální hodnota indexu, pro který je zahrnutý v dotazu.

> [!NOTE]
> Nastavení tohoto parametru na hodnotu nižší než 100 může být užitečná pro zajištění dostupnosti vyhledávání i pro služby s pouze jednu repliku. Ne všechny odpovídající dokumenty jsou však zaručena nacházet ve výsledcích hledání. Pokud se pro vaše aplikace důležitější než dostupnosti pro vyvolání vyhledávání, pak je vhodné ponechat `minimumCoverage` na výchozí hodnotě 100.
> 
> 

`api-version=[string]`(povinné). Verze preview je `api-version=2015-02-28-Preview`. V tématu [verze služby vyhledávání](http://msdn.microsoft.com/library/azure/dn864560.aspx) podrobnosti a alternativní verze.

Poznámka: pro tuto operaci `api-version` je zadána jako parametr dotazu v adrese URL bez ohledu na to, jestli volání **vyhledávání** s GET nebo POST.

**Hlavičky požadavku**

Následující seznam popisuje hlavičky žádosti požadované a volitelné.

* `api-key`: `api-key` Se používá k ověření požadavku na vaši službu vyhledávání. Je hodnotu řetězce pro adresu URL služby jedinečné. **Vyhledávání** žádosti můžete zadat buď klíč správce, nebo klíč dotazu pro `api-key`.

Budete také potřebovat názvu služby pro vytvoření adresy URL žádosti. Můžete získat název služby a `api-key` z řídicího panelu služby na portálu Azure. V tématu [vytvoření služby Azure Search na portálu](search-create-service-portal.md) nápovědu navigace stránky.

**Text žádosti**

U metody GET: žádné.

Pro metodu POST:

    {
      "count": true | false (default),
      "facets": [ "facet_expression_1", "facet_expression_2", ... ],
      "filter": "odata_filter_expression",
      "highlight": "highlight_field_1, highlight_field_2, ...",
      "highlightPreTag": "pre_tag",
      "highlightPostTag": "post_tag",
      "minimumCoverage": # (% of index that must be covered to declare query successful; default 100),
      "moreLikeThis": "document_key" (mutually exclusive with "search" parameter),
      "orderby": "orderby_expression",
      "scoringParameters": [ "scoring_parameter_1", "scoring_parameter_2", ... ],
      "scoringProfile": "scoring_profile_name",
      "search": "simple_query_expression",
      "searchFields": "field_name_1, field_name_2, ...",
      "searchMode": "any" (default) | "all",
      "select": "field_name_1, field_name_2, ...",
      "skip": # (default 0),
      "top": #
    }

**Pokračování částečné vyhledávání odpovědí**

Někdy Azure Search nemůže vrátit všechny požadované výsledky v odpověď o jedné vyhledávání. K tomu může dojít různých důvodů, například kdy dotaz vyžadovali příliš mnoho dokumentů není zadáním `$top` nebo zadáte hodnotu `$top` , protože je příliš velký. V takových případech bude obsahovat Azure Search `@odata.nextLink` poznámky v textu odpovědi a také `@search.nextPageParameters` Pokud byl požadavek POST. Hodnoty těchto poznámek můžete formulovali jiného vyhledávání požadavek na načtení v další části hledání odpovědi. Tento postup se nazývá ***pokračování*** původní hledání požadavku a poznámky se obvykle označují jako ***pokračování tokeny***. V tématu [níže uvedený příklad](#SearchResponse) podrobnosti o syntaxi těchto poznámek a jejich umístění v textu odpovědi. 

Z důvodů, proč Azure Search může vrátit pokračování tokeny jsou specifické pro implementaci a může se změnit. Robustní klientů musí být vždy připraven pro zpracování případy, kdy jsou vráceny méně dokumenty, než se očekává a je zahrnuté pokračovat v načítání dokumenty token pokračování. Všimněte si také, že musí používat stejnou metodu HTTP jako původní žádost aby bylo možné pokračovat. Například pokud jste odeslali požadavek GET, všechny žádosti pokračování odeslání musíte taky použít GET (a stejně tak pro metodu POST).

<a name="SearchResponse"></a>
**Odpověď**

Stavový kód: 200 OK se vrátí pro úspěšné odpovědi.

    {
      "@odata.count": # (if $count=true was provided in the query),
      "@search.coverage": # (if minimumCoverage was provided in the query),
      "@search.facets": { (if faceting was specified in the query)
        "facet_field": [
          {
            "value": facet_entry_value (for non-range facets),
            "from": facet_entry_value (for range facets),
            "to": facet_entry_value (for range facets),
            "count": number_of_documents
          }
        ],
        ...
      },
      "@search.nextPageParameters": { (request body to fetch the next page of results if not all results could be returned in this response and Search was called with POST)
        "count": ... (value from request body if present),
        "facets": ... (value from request body if present),
        "filter": ... (value from request body if present),
        "highlight": ... (value from request body if present),
        "highlightPreTag": ... (value from request body if present),
        "highlightPostTag": ... (value from request body if present),
        "minimumCoverage": ... (value from request body if present),
        "moreLikeThis": ... (value from request body if present),
        "orderby": ... (value from request body if present),
        "scoringParameters": ... (value from request body if present),
        "scoringProfile": ... (value from request body if present),
        "search": ... (value from request body if present),
        "searchFields": ... (value from request body if present),
        "searchMode": ... (value from request body if present),
        "select": ... (value from request body if present),
        "skip": ... (page size plus value from request body if present),
        "top": ... (value from request body if present minus page size),
      },
      "value": [
        {
          "@search.score": document_score (if a text query was provided),
          "@search.highlights": {
            field_name: [ subset of text, ... ],
            ...
          },
          key_field_name: document_key,
          field_name: field_value (retrievable fields or specified projection),
          ...
        },
        ...
      ],
      "@odata.nextLink": (URL to fetch the next page of results if not all results could be returned in this response; Applies to both GET and POST)
    }

**Příklady:**

Další příklady naleznete na [syntaxe výrazu OData pro službu Azure Search](https://msdn.microsoft.com/library/azure/dn798921.aspx) stránky.

1)    Vyhledávání v indexu seřazeny sestupně podle data.

    GET /indexes/hotels/docs? hledání = * & $orderby = lastRenovationDate desc & verze api-version = 2015-02-28-Preview

    POST /indexes/hotels/docs/search? api-version = 2015-02-28-Preview {"Vyhledat": "*", "orderby": "lastRenovationDate desc"}

2)    V rámci Fasetové vyhledávání v rejstříku a načíst omezující vlastnosti pro kategorií, hodnocení, značky, jakož i položky s baseRate v konkrétních oblastech:

    GET /indexes/hotels/docs? hledání = test & omezující vlastnost = kategorie & omezující vlastnost = hodnocení & omezující vlastnost = značky & omezující vlastnost = baseRate, hodnoty: 80 | 150 | 220 & verze api-version = 2015-02-28-Preview

    POST /indexes/hotels/docs/search? api-version = 2015-02-28-Preview {"Vyhledat": "test", "omezující vlastnosti": ["kategorie", "hodnocení", "značky", "baseRate, hodnoty: 80 | 150 | 220"]}

3)    Pomocí filtru, zúžit předchozí výsledky dotazu Fasetové poté, co uživatel klikne na hodnocení 3 a kategorie "Motel":

    GET /indexes/hotels/docs? hledání = test & omezující vlastnost = značky & omezující vlastnost = baseRate, hodnoty: 80 | 150 | 220 & $filter = hodnocení eq 3 a kategorie eq "Motel" & verze api-version = 2015-02-28-Preview

    POST /indexes/hotels/docs/search? api-version = 2015-02-28-Preview {"Vyhledat": "test", "omezující vlastnosti": ["značky", "baseRate, hodnoty: 80 | 150 | 220"], "filtr": "hodnocení eq 3 a kategorie eq"Motel""}

4) V rámci Fasetové vyhledávání nastavte horní limit na jedinečný podmínky vrácených dotazem. Výchozí hodnota je 10, ale můžete zvýšit nebo snížit, tato hodnota pomocí `count` parametr na `facet` atribut:

    GET /indexes/hotels/docs? hledání = test & omezující vlastnost = města, počet: 5 & verze api-version = 2015-02-28-Preview

    POST /indexes/hotels/docs/search? api-version = 2015-02-28-Preview {"Vyhledat": "test", "omezující vlastnosti": ["města, počet: 5"]}

5)    Vyhledávání v indexu v rámci konkrétních polí; Například pro specifický jazyk pole:

    GET /indexes/hotels/docs? hledání = hôtel & searchFields = description_fr & verze api-version = 2015-02-28-Preview

    POST /indexes/hotels/docs/search? api-version = 2015-02-28-Preview {"Vyhledat": "hôtel", "searchFields": "description_fr"}

6) Index vyhledávání napříč více polí. Můžete například ukládat a dotaz prohledatelná pole v několika jazycích, všechny ve stejném indexu.  Pokud angličtinu a francouzštinu popisy existují společně ve stejném dokumentu, můžete se vrátit některého nebo všech výsledků dotazu:

    GET /indexes/hotels/docs? hledání = hotelů & searchFields = popis, description_fr & verze api-version = 2015-02-28-Preview

    POST /indexes/hotels/docs/search? api-version = 2015-02-28-Preview {"Vyhledat": "hotelů", "searchFields": "popis, description_fr"}

Všimněte si, že dotaz lze použít pouze jeden index najednou. Nevytvářejte více indexů pro jednotlivé jazyky, pokud máte v plánu dotazu po jednom.

7)    Stránkování - Get na 1. stránku položek (velikost stránky je 10):

    GET /indexes/hotels/docs? hledání = * & $skip = 0 & $top = 10 & verze api-version = 2015-02-28-Preview

    POST /indexes/hotels/docs/search? api-version = 2015-02-28-Preview {"Vyhledat": "*", "přeskočení": 0, "top": 10}

8)    Stránkování - Get na 2. stránku položek (velikost stránky je 10):

    GET /indexes/hotels/docs? hledání = * & $skip = 10 & $top = 10 & verze api-version = 2015-02-28-Preview

    POST /indexes/hotels/docs/search? api-version = 2015-02-28-Preview {"Vyhledat": "*", "přeskočení": "top" 10: 10}

9)    Načtěte konkrétní sadu pole:

    GET /indexes/hotels/docs? hledání = * & $select = hotelName, popis a rozhraní api-version = 2015-02-28-Preview

    POST /indexes/hotels/docs/search? api-version = 2015-02-28-Preview {"Vyhledat": "*", "Vyberte": "hotelName, popis"}

10)  Načtení dokumenty odpovídající výraz konkrétní filtru:

    GET /indexes/hotels/docs? $filter = (baseRate ge 60 a baseRate lt 300) nebo hotelName eq 'Zvláštní zůstat' & verze api-version = 2015-02-28-Preview

    POST /indexes/hotels/docs/search? api-version = 2015-02-28-Preview {"filtr": "(baseRate ge 60 a baseRate lt 300) nebo hotelName eq"Zvláštní zůstat""}

11) Hledání fragmenty index a vraťte se přístupů označuje

    GET /indexes/hotels/docs? hledání = něco & zvýrazněte = Popis & verze api-version = 2015-02-28-Preview

    POST /indexes/hotels/docs/search? api-version = 2015-02-28-Preview {"Vyhledat": "něco co", "highlight": "Popis"}

12) Vyhledávání v indexu a vrátit dokumenty seřadit z blíže dále od odkazem na umístění

    GET /indexes/hotels/docs? vyhledávání něco = & $orderby=geo.distance (umístění, geography'POINT(-122.12315 47.88121)') & verze api-version = 2015-02-28-Preview

    POST /indexes/hotels/docs/search? api-version = 2015-02-28-Preview {"Vyhledat": "něco co", "orderby": "geo.distance (umístění, geography'POINT(-122.12315 47.88121)')"}

13) Vyhledávání v indexu za předpokladu, že je profil vyhodnocování názvem "geo" s dvě funkce pro výpočet skóre podle vzdálenosti, jeden definování parametr s názvem "currentLocation" a jeden definování parametr s názvem "lastLocation"

    GET /indexes/hotels/docs? vyhledávání něco = & scoringProfile = geograficky & scoringParameter = currentLocation – 122.123,44.77233 & scoringParameter = lastLocation – 121.499,44.2113 & verze api-version = 2015-02-28-Preview

    POST /indexes/hotels/docs/search? api-version = 2015-02-28-Preview {"Vyhledat": "něco co", "scoringProfile": "geo", "scoringParameters": ["currentLocation – 122.123,44.77233", "lastLocation – 121.499,44.2113"]}

14) Hledání dokumentů do indexu pomocí [jednoduchá syntaxe dotazů](https://msdn.microsoft.com/library/dn798920.aspx). Tento dotaz vrací hotels kde prohledatelná pole obsahovat podmínky "pohodlí" a "umístění", ale ne "motel":

    GET /indexes/hotels/docs? hledání = pohodlí + umístění-motel & searchMode = all & verze api-version = 2015-02-28-Preview

    POST /indexes/hotels/docs/search? api-version = 2015-02-28-Preview {"Vyhledat": "pohodlí + umístění-motel", "searchMode": "vše"}

Všimněte si použití `searchMode=all` výše. Tento parametr včetně přepíše výchozí hodnoty `searchMode=any`, zajistit který `-motel` znamená "A není" místo "Nebo není". Bez `searchMode=all`, získáte "Nebo není" který rozbalí než omezuje výsledky hledání, a to může být counter-intuitive u některých uživatelů.

15) Hledání dokumentů do indexu pomocí [syntaxe dotazů lucene](https://msdn.microsoft.com/library/mt589323.aspx). Tento dotaz vrací hotels, kde pole kategorie obsahuje termín "rozpočet" a všechna prohledatelná pole obsahující frázi "nedávno renovovanou". Dokumenty obsahující frázi "nedávno renovovanou" jsou řazeny výše v důsledku hodnotu nárůst termín (3)

    GET /indexes/hotels/docs? hledání = kategorie: nároky a \"nedávno renovovanou\"^ 3 & searchMode = all & verze api-version = 2015-02-28-Preview & Typ úplné =

    POST /indexes/hotels/docs/search? api-version = 2015-02-28-Preview {"Vyhledat": "kategorie: nároky a \"nedávno renovovanou\"^ 3", "typ": "úplná", "searchMode": "vše"}

<a name="LookupAPI"></a>

## <a name="lookup-document"></a>Vyhledávání dokumentů
**Vyhledávání dokumentů** operace dokumentu načte z Azure Search. To je užitečné, když uživatel klikne na konkrétní výsledek a chcete vyhledat konkrétní podrobnosti o tomto dokumentu.

    GET https://[service name].search.windows.net/indexes/[index name]/docs/[key]?[query parameters]
    api-key: [admin or query key]

**Požadavek**

Protokol HTTPS je vyžadována pro žádosti o služby. **Vyhledávání dokumentů** požadavek konstruovat následujícím způsobem.

    GET /indexes/[index name]/docs/key?[query parameters]

Alternativně můžete tradiční syntaxe OData pro vyhledávání klíčů:

    GET /indexes('[index name]')/docs('[key]')?[query parameters]

Identifikátoru URI požadavku zahrnuje [název indexu] a [klíče], který dokument k načtení z index, který zadáte. Najednou můžete získat pouze jeden dokument. Použití **vyhledávání** získat více dokumentů v jedné žádosti.

**Parametry dotazu**

`$select=[string]`(volitelné) – seznam polí, oddělených čárkami k načtení. Pokud tento parametr nezadáte, nebo nastavte `*`, všechna pole, které jsou označeny jako získat ve schématu jsou zahrnuty v projekci.

`api-version=[string]`(povinné). Verze preview je `api-version=2015-02-28-Preview`. V tématu [verze služby vyhledávání](http://msdn.microsoft.com/library/azure/dn864560.aspx) podrobnosti a alternativní verze.

Poznámka: pro tuto operaci `api-version` je zadána jako parametr dotazu.

**Hlavičky požadavku**

Následující seznam popisuje hlavičky žádosti požadované a volitelné.

* `api-key`: `api-key` Se používá k ověření požadavku na vaši službu vyhledávání. Je hodnotu řetězce pro adresu URL služby jedinečné. **Vyhledávání dokumentů** žádosti můžete zadat buď klíč správce, nebo klíč dotazu pro `api-key`.

Budete také potřebovat názvu služby pro vytvoření adresy URL žádosti. Můžete získat název služby a `api-key` z řídicího panelu služby na portálu Azure. V tématu [vytvoření služby Azure Search na portálu](search-create-service-portal.md) nápovědu navigace stránky.

**Text žádosti**

Žádné.

**Odpověď**

Stavový kód: 200 OK se vrátí pro úspěšné odpovědi.

    {
      field_name: field_value (fields matching the default or specified projection)
    }

**Příklad**

Vyhledávání dokumentu, který má klíč: 2.

    GET /indexes/hotels/docs/2?api-version=2015-02-28-Preview

Vyhledávání dokumentu, který má klíč pomocí syntaxe OData 3:

    GET /indexes('hotels')/docs('3')?api-version=2015-02-28-Preview

<a name="CountDocs"></a>

## <a name="count-documents"></a>Počet dokumentů
**Počet dokumentů** operace načte počet dokumentů v indexu vyhledávání. `$count` Syntaxe je součástí protokolu OData.

    GET https://[service name].search.windows.net/indexes/[index name]/docs/$count?api-version=[api-version]
    Accept: text/plain
    api-key: [admin or query key]

**Požadavek**

Protokol HTTPS je vyžadována pro žádosti o služby. **Počet dokumentů** požadavek se dá vytvořit pomocí metody GET.

[Název indexu] v identifikátoru URI požadavku informuje službu, kterou chcete vrátit počet všechny položky v kolekci dokumenty, které se zadaným indexem.

`api-version=[string]`(povinné). Verze preview je `api-version=2015-02-28-Preview`. V tématu [verze služby vyhledávání](http://msdn.microsoft.com/library/azure/dn864560.aspx) podrobnosti a alternativní verze.

**Hlavičky požadavku**

Následující seznam popisuje hlavičky žádosti požadované a volitelné.

* `Accept`: Tato hodnota musí být nastavena na `text/plain`.
* `api-key`: `api-key` Se používá k ověření požadavku na vaši službu vyhledávání. Je hodnotu řetězce pro adresu URL služby jedinečné. **Počet dokumentů** žádosti můžete zadat buď klíč správce, nebo klíč dotazu pro `api-key`.

Budete také potřebovat názvu služby pro vytvoření adresy URL žádosti. Můžete získat název služby a `api-key` z řídicího panelu služby na portálu Azure. V tématu [vytvoření služby Azure Search na portálu](search-create-service-portal.md) nápovědu navigace stránky.

**Text žádosti**

Žádné.

**Odpověď**

Stavový kód: 200 OK se vrátí pro úspěšné odpovědi.

Text odpovědi obsahuje hodnotu počtu jako celé číslo ve formátu v prostém textu.

<a name="Suggestions"></a>

## <a name="suggestions"></a>Návrhy
**Návrhy** operace načte návrhy založené na vstupu částečné vyhledávání. Obvykle se používá v oknech vyhledávání zajistit našeptávání návrhy, jak uživatelé zadávají hledaný text.

Požadavky na návrh cílem návrhy cílové dokumenty, tak navrhované text může být opakována v případě více dokumentů candidate odpovídají stejné hledání vstup. Můžete použít `$select` pro načtení další pole dokumentu (včetně klíč dokumentu), aby se dá zjistit, který dokument je zdrojem pro každý návrhu.

A **návrhy** operaci se objeví jako požadavek GET nebo POST.

    GET https://[service name].search.windows.net/indexes/[index name]/docs/suggest?[query parameters]
    api-key: [admin or query key]

    POST https://[service name].search.windows.net/indexes/[index name]/docs/suggest?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin or query key]

**Kdy použít POST místo GET**

Při použití metody GET protokolu HTTP k volání **návrhy** rozhraní API, musíte vědět, že nesmí překročit délka adresy URL žádosti 8 KB. Je to obvykle dostatečně pro většinu aplikací. Některé aplikace však vytvořit velmi velké dotazy, konkrétně výrazy filtru OData. Pro tyto aplikace pomocí HTTP POST je lepší volbou, protože umožňuje větší filtry než metoda GET. S POST počet klauzulí ve filtru je omezující faktor, ne podle velikosti vytvořeného řetězec nezpracovaná filtru vzhledem k tomu, že je přibližně 16 MB omezení velikosti pro metodu POST.

> [!NOTE]
> I když maximální velikost požadavku POST je velmi velké, nemůže být libovolně komplexní výrazech filtru. V tématu [syntaxe výrazu OData](https://msdn.microsoft.com/library/dn798921.aspx) pro další informace o omezeních složitost filtru.
> 
> 

**Požadavek**

Protokol HTTPS je vyžadována pro žádosti o služby. **Návrhy** požadavek se dá vytvořit pomocí metody GET nebo POST.

Identifikátoru URI požadavku určuje název indexu dotazu. V řetězci dotazu v případě požadavky GET jsou zadány parametry, jako je například částečně vstupní hledaný termín, a v žádosti subjekt v případě POST požadavky.

Jako osvědčený postup při vytváření požadavky GET, nezapomeňte [kódování URL](https://msdn.microsoft.com/library/system.uri.escapedatastring.aspx) parametry specifického dotazu při přímé volání rozhraní REST API. Pro **návrhy** operace, to zahrnuje:

* `$filter`
* `highlightPreTag`
* `highlightPostTag`
* `search`

Kódování URL se doporučuje jenom na výše uvedené parametry dotazu. Pokud jste omylem kódování URL řetězec dotazu celý (vše za?), by došlo k přerušení požadavky.

Navíc kódování URL je nutné pouze při volání rozhraní REST API přímo pomocí GET. Žádné kódování URL je nezbytné při volání metody **návrhy** pomocí POST, nebo při použití [klientské knihovny .NET](https://msdn.microsoft.com/library/dn951165.aspx), který zpracovává kódování URL za vás.

**Parametry dotazu**

**Návrhy** přijímá několik parametrů, které poskytují kritéria dotazu a také určit chování vyhledávání. Zadejte tyto parametry v adrese URL řetězec dotazu při volání metody **návrhy** prostřednictvím GET a jako vlastnosti JSON v textu požadavku při volání metody **návrhy** přes POST. Syntaxe pro některé parametry se mírně liší mezi GET a POST. Tyto rozdíly jsou popsány podle vhodnosti níže:

`search=[string]`-vyhledávání text, který má používat pro návrh dotazy. Musí být minimálně 1 znak a maximálně 100 znaků.

`highlightPreTag=[string]`(volitelné) – řetězec značek, které přidá k vyhledání přístupů. Musí být nastavena s `highlightPostTag`.

> [!NOTE]
> Při volání metody **návrhy** pomocí GET, vyhrazené znaky v adrese URL musí být kódovaný v procentech (například místo # % 23).
> 
> 

`highlightPostTag=[string]`(volitelné) – řetězec značek, které připojí k vyhledání přístupů. Musí být nastavena s `highlightPreTag`.

> [!NOTE]
> Při volání metody **návrhy** pomocí GET, vyhrazené znaky v adrese URL musí být kódovaný v procentech (například místo # % 23).
> 
> 

`suggesterName=[string]`-Název modulu pro návrhy zadané v `suggesters` kolekce, která je součástí definice indexu. A `suggester` určuje pole, která hledat podmínky navrhované dotazu. V tématu [trochu](#Suggesters) podrobnosti.

`fuzzy=[boolean]`(volitelné, výchozí = false) – Pokud je vlastnost nastavena na hodnotu true, toto rozhraní API najdete návrhy i tehdy, je nahrazovanou nebo chybí znak v hledaný text. Zatímco to poskytuje lepší podmínky v některých scénářích kompromisnímu výkon, protože vyhledávání s fuzzy logikou návrhu pomalejší a spotřebovávat více prostředků.

`searchFields=[string]`(volitelné) – seznam názvů oddělených čárkami pole pro vyhledávání pro zadaný hledaný text. Cílová pole. musí být povolen pro návrhy.

`$top=#`(volitelné, výchozí = 5)-počet návrhy pro načtení. Musí být číslo v rozsahu od 1 do 100.

> [!NOTE]
> Při volání metody **návrhy** pomocí POST, tento parametr je s názvem `top` místo `$top`.
> 
> 

`$filter=[string]`(volitelné) – výraz, který filtruje dokumenty za návrhy.

> [!NOTE]
> Při volání metody **návrhy** pomocí POST, tento parametr je s názvem `filter` místo `$filter`.
> 
> 

`$orderby=[string]`(volitelné) – seznam výrazů textový soubor s oddělovači na výsledky seřaďte podle. Každý výraz může být název pole nebo volání `geo.distance()` funkce. Každý výraz může následovat `asc` uvést vzestupné řazení a `desc` k označení sestupném. Výchozí hodnota je vzestupné pořadí. Existuje limit 32 klauzule pro `$orderby`.

> [!NOTE]
> Při volání metody **návrhy** pomocí POST, tento parametr je s názvem `orderby` místo `$orderby`.
> 
> 

`$select=[string]`(volitelné) – seznam polí, oddělených čárkami k načtení. Pokud tento parametr nezadáte, pouze dokumentu klíč a návrhu je vrácen. Všechna pole můžete explicitně žádostí nastavením tohoto parametru na `*`.

> [!NOTE]
> Při volání metody **návrhy** pomocí POST, tento parametr je s názvem `select` místo `$select`.
> 
> 

`minimumCoverage`(volitelné, výchozí hodnota je 80)-číslo mezi 0 a 100 procentuální hodnota indexu, která musí být předmětem dotazu návrhy v pořadí pro dotaz na hlášené jako úspěšné. Ve výchozím nastavení, musí být k dispozici alespoň 80 % indexu nebo `Suggest` vrátí stavový kód protokolu HTTP 503. Pokud nastavíte `minimumCoverage` a `Suggest` úspěšné, se vrátí HTTP 200 a zahrnout `@search.coverage` hodnotu v odpovědi procentuální hodnota indexu, pro který je zahrnutý v dotazu.

> [!NOTE]
> Nastavení tohoto parametru na hodnotu nižší než 100 může být užitečná pro zajištění dostupnosti vyhledávání i pro služby s pouze jednu repliku. Ne všechny odpovídající návrh je však zaručena nacházet ve výsledcích. Pokud se pro vaše aplikace důležitější než dostupnosti pro vyvolání, pak je nejlepší nižší `minimumCoverage` pod jeho výchozí hodnota 80.
> 
> 

`api-version=[string]`(povinné). Verze preview je `api-version=2015-02-28-Preview`. V tématu [verze služby vyhledávání](http://msdn.microsoft.com/library/azure/dn864560.aspx) podrobnosti a alternativní verze.

Poznámka: pro tuto operaci `api-version` je zadána jako parametr dotazu v adrese URL bez ohledu na to, jestli volání **návrhy** s GET nebo POST.

**Hlavičky požadavku**

Následující seznam popisuje hlavičky žádosti požadované a volitelné

* `api-key`: `api-key` Se používá k ověření požadavku na vaši službu vyhledávání. Je hodnotu řetězce pro adresu URL služby jedinečné. **Návrhy** žádosti můžete zadat buď klíč správce, nebo klíč dotazu, jako `api-key`.

Budete také potřebovat názvu služby pro vytvoření adresy URL žádosti. Můžete získat název služby a `api-key` z řídicího panelu služby na portálu Azure. V tématu [vytvoření služby Azure Search na portálu](search-create-service-portal.md) nápovědu navigace stránky.

**Text žádosti**

U metody GET: žádné.

Pro metodu POST:

    {
      "filter": "odata_filter_expression",
      "fuzzy": true | false (default),
      "highlightPreTag": "pre_tag",
      "highlightPostTag": "post_tag",
      "minimumCoverage": # (% of index that must be covered to declare query successful; default 80),
      "orderby": "orderby_expression",
      "search": "partial_search_input",
      "searchFields": "field_name_1, field_name_2, ...",
      "select": "field_name_1, field_name_2, ...",
      "suggesterName": "suggester_name",
      "top": # (default 5)
    }

**Odpověď**

Stavový kód: 200 OK se vrátí pro úspěšné odpovědi.

    {
      "@search.coverage": # (if minimumCoverage was provided in the query),
      "value": [
        {
          "@search.text": "...",
          "<key field>": "..."
        },
        ...
      ]
    }

Pokud možnost projekce slouží k načtení pole, která jsou součástí každý element pole:

    {
      "@search.coverage": # (if minimumCoverage was provided in the query),
      "value": [
        {
          "@search.text": "...",
          "<key field>": "..."
          <other projected data fields>
        },
        ...
      ]
    }

**Příklad**

Načtení 5 návrhy, kde je vstupní částečné vyhledávání 'lux.

    GET /indexes/hotels/docs/suggest?search=lux&$top=5&suggesterName=sg&api-version=2015-02-28-Preview

    POST /indexes/hotels/docs/suggest?api-version=2015-02-28-Preview
    {
      "search": "lux",
      "top": 5,
      "suggesterName": "sg"
    }
