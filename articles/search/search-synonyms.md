---
pageTitle: Synonyms in Azure Search (preview) | Microsoft Docs
description: "Předběžná dokumentace pro funkci synonyma (preview), který je v rozhraní API REST služby Azure Search."
services: search
documentationCenter: 
authors: mhko
manager: pablocas
editor: 
ms.service: search
ms.devlang: rest-api
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 07/07/2016
ms.author: nateko
ms.openlocfilehash: 739a0ad77c68ea74ec25bc80c7539ac8b3f18201
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="synonyms-in-azure-search-preview"></a>Synonyma ve službě Azure Search (preview)

Synonyma na vyhledávacích webech přidružit ekvivalentní podmínky, které implicitně zvětšit rozsah dotazu, aniž by uživatel musel ve skutečnosti zadejte termín. Například zadány termín "pes" a synonymum přidružení "canine" a "štěněte", všechny dokumenty obsahující "pes", "PSA" nebo "štěněte" bude spadat do rozsahu dotazu.

Ve službě Azure Search synonymum rozšíření se provádí v době dotazu. Synonymum maps můžete přidat na služby s bez přerušení na existující operace. Můžete přidat **synonymMaps** vlastnost do definice pole bez nutnosti znovu sestavte index. Další informace najdete v tématu [aktualizace indexu](https://docs.microsoft.com/rest/api/searchservice/update-index).

## <a name="feature-availability"></a>Dostupnost funkcí

Funkci synonyma je aktuálně ve verzi preview a podporuje jenom v nejnovější verzi preview rozhraní api-version (verze api-version = 2016-09-01-Preview). Podpora webu Azure Portal se v současnosti neposkytuje. Verze rozhraní API je určen na žádost, je možné kombinovat všeobecně dostupná (GA) a zobrazit jejich náhled rozhraní API ve stejné aplikaci. Však může změnit preview, které nejsou v rámci smlouvy o úrovni služeb a funkcí rozhraní API, tak nedoporučujeme jejich používání v produkční aplikace.

## <a name="how-to-use-synonyms-in-azure-search"></a>Jak používat synonyma ve službě Azure search

Ve službě Azure Search vychází synonymum podpory synonymum mapy, které můžete definovat a odeslat do služby. Tyto mapy tvoří nezávislý prostředek (např. indexy nebo zdroje dat) a mohou být využívána žádné prohledávatelné pole v jakékoli indexu ve vyhledávací službě.

Mapuje synonymum a indexy jsou zachována nezávisle. Po definování mapu synonymum a nahrajte ho do vaší služby, můžete povolit funkci synonymum na pole tak, že přidáte novou vlastnost s názvem **synonymMaps** v definici pole. Vytvoření, aktualizace a odstranění mapu synonymum, je vždy celé dokumentu operaci, což znamená, že nelze vytvořit, aktualizaci nebo odstranění částí mapy synonymum postupně. Aktualizace i jeden záznam nevyžaduje znovu načíst.

Začlenění synonyma do vyhledávací aplikaci je dvoustupňový proces:

1.  Přidáte mapu synonymum do služby vyhledávání prostřednictvím rozhraní API níže.  

2.  Nakonfigurujte prohledávatelné pole, které chcete použít mapování synonymum v definici indexu.

### <a name="synonymmaps-resource-apis"></a>Rozhraní API SynonymMaps prostředků

#### <a name="add-or-update-a-synonym-map-under-your-service-using-post-or-put"></a>Přidat nebo aktualizovat mapu synonymum v rámci služby, pomocí POST nebo BLOKOVAT.

Synonymum maps se odešlou do služby prostřednictvím POST nebo PUT. Každé pravidlo musí být odděleny znak nového řádku (\n). Můžete definovat až 5 000 pravidel na synonymum mapy ve bezplatné služby a 10 000 pravidel ve všech dalších skladových položek. Každé pravidlo může mít maximálně 20 rozšíření.

V této verzi preview musí být synonymum mapy ve formátu Apache Solr, který je popsáno níže. Pokud máte existující slovník synonymum v jiném formátu a chcete používat přímo, dejte nám vědět, na [UserVoice](https://feedback.azure.com/forums/263029-azure-search).

Můžete vytvořit nové mapování synonymum pomocí HTTP POST, jako v následujícím příkladu:

    POST https://[servicename].search.windows.net/synonymmaps?api-version=2016-09-01-Preview
    api-key: [admin key]

    {  
       "name":"mysynonymmap",
       "format":"solr",
       "synonyms": "
          USA, United States, United States of America\n
          Washington, Wash., WA => WA\n"
    }

Alternativně můžete použít PUT a zadejte název mapy synonymum v identifikátoru URI. Pokud mapy synonymum neexistuje, bude vytvořen.

    PUT https://[servicename].search.windows.net/synonymmaps/mysynonymmap?api-version=2016-09-01-Preview
    api-key: [admin key]

    {  
       "format":"solr",
       "synonyms": "
          USA, United States, United States of America\n
          Washington, Wash., WA => WA\n"
    }

##### <a name="apache-solr-synonym-format"></a>Apache Solr synonymum formátu

Formát Solr podporuje ekvivalentní a explicitní synonymum mapování. Pravidla mapování splňovat specifikaci filtru synonymum s otevřeným zdrojem Apache Solr, popsané v tomto dokumentu: [SynonymFilter](https://cwiki.apache.org/confluence/display/solr/Filter+Descriptions#FilterDescriptions-SynonymFilter). Zde je ukázka pravidla pro ekvivalentní synonyma.
```
              USA, United States, United States of America
```

S tímto pravidlem výše, vyhledávací dotaz rozbalte "USA" možnost "USA" nebo "USA" nebo "USA".

Šipka je označený jako explicitní mapování "= >". -Li zadána, termín posloupnost vyhledávací dotaz, který odpovídá levé straně "= >" bude nahrazena adresou alternativy na pravé straně. Zadané pravidlo níže, vyhledávací dotazy "Washington", "Wash." nebo "WA" budou všechny být přepsána pro "WA". Explicitní mapování pouze platí v určeném směru a není dotaz přepište "WA" na "Washington" v tomto případě.
```
              Washington, Wash., WA => WA
```

#### <a name="list-synonym-maps-under-your-service"></a>Seznam synonymum mapuje v rámci služby.

    GET https://[servicename].search.windows.net/synonymmaps?api-version=2016-09-01-Preview
    api-key: [admin key]

#### <a name="get-a-synonym-map-under-your-service"></a>Načíst synonymum mapu v rámci služby.

    GET https://[servicename].search.windows.net/synonymmaps/mysynonymmap?api-version=2016-09-01-Preview
    api-key: [admin key]

#### <a name="delete-a-synonyms-map-under-your-service"></a>Odstranění mapy synonyma v rámci služby.

    DELETE https://[servicename].search.windows.net/synonymmaps/mysynonymmap?api-version=2016-09-01-Preview
    api-key: [admin key]

### <a name="configure-a-searchable-field-to-use-the-synonym-map-in-the-index-definition"></a>Nakonfigurujte prohledávatelné pole, které chcete použít mapování synonymum v definici indexu.

Nové vlastnosti pole **synonymMaps** lze použít k určení synonymum mapu, která bude používat pro prohledávatelné pole. Synonymum mapy prostředků úrovně služby a lze odkazovat pomocí libovolného pole index v rámci služby.

    POST https://[servicename].search.windows.net/indexes?api-version=2016-09-01-Preview
    api-key: [admin key]

    {
       "name":"myindex",
       "fields":[
          {
             "name":"id",
             "type":"Edm.String",
             "key":true
          },
          {
             "name":"name",
             "type":"Edm.String",
             "searchable":true,
             "analyzer":"en.lucene",
             "synonymMaps":[
                "mysynonymmap"
             ]
          },
          {
             "name":"name_jp",
             "type":"Edm.String",
             "searchable":true,
             "analyzer":"ja.microsoft",
             "synonymMaps":[
                "japanesesynonymmap"
             ]
          }
       ]
    }

**synonymMaps** lze zadat pro prohledatelná pole typu 'Edm.String' nebo 'Collection(Edm.String)'.

> [!NOTE]
> V této verzi preview může mít pouze jeden synonymum mapovat na pole. Pokud chcete použít více mapami synonymum, dejte nám vědět, na [UserVoice](https://feedback.azure.com/forums/263029-azure-search).

## <a name="impact-of-synonyms-on-other-search-features"></a>Dopad synonyma na jiných vyhledávacích funkcí

Funkci synonyma přepíše původní dotaz s synonyma pomocí operátoru OR. Z tohoto důvodu přístupů zvýrazňování a vyhodnocování profily považovat za původní termín a synonyma ekvivalent.

Synonymum funkce se vztahuje na vyhledávací dotazy a nebude použitelná pro filtry, nebo omezující vlastnosti. Podobně návrhy založen pouze na původní termín; synonymum shoduje se nezobrazí v odpovědi.

Synonymum rozšíření se nevztahují na zástupný znak hledaných termínů; Předpona, přibližné a podmínky regex nejsou rozšířit.

## <a name="tips-for-building-a-synonym-map"></a>Tipy pro sestavování synonymum mapy

- Mapu stručný a dobře navrženou synonymum je efektivnější než vyčerpávající seznam možných shod. Nadměrně velké nebo složité slovník trvat delší dobu a analyzovat mají vliv na latenci dotazu, pokud dotaz zasahuje do mnoha synonyma. Místo odhad, při které by mohly používat podmínky, můžete získat skutečný podmínky prostřednictvím [hledání sestava analýzy provozu](search-traffic-analytics.md).

- Jako předběžná verze i ověřování využijí, povolení a pak použít tato sestava přesněji určit podmínky, které bude využívat synonymum shody a pak ho nadále používat jako ověření, že je mapu synonymum generovala lepší výsledek. V předdefinované sestavě dlaždice "Nejčastější dotazy vyhledávání" a "nula výsledek vyhledávací dotazy" získáte potřebné informace.

- Můžete vytvořit více mapami synonymum pro vyhledávací aplikaci (například podle jazyka, pokud vaše aplikace podporuje základní vícejazyčnou zákazníka). V současné době pole lze použít pouze jeden z nich. Vlastnosti synonymMaps můžete kdykoli aktualizovat.

## <a name="next-steps"></a>Další kroky

- Pokud máte existující index ve vývojovém prostředí (mimo produkční), Experimentujte s slovník malé najdete, jak přidání synonyma změny možnosti vyhledávání, včetně dopad na vyhodnocování profily, zvýrazňování a návrhy.

- [Povolit Analýza provozu vyhledávání](search-traffic-analytics.md) pomocí předdefinované sestavy Power BI další podmínky, které se používají na maximum a ty, které vrátí nulové dokumenty. Díky tyto přehledy, zkontrolovat, jestli slovníku zahrnout synonyma pro neproduktivní dotazy, které by měl být řešení na dokumenty v indexu.
