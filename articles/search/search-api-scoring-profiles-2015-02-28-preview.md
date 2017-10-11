---
title: "Vyhodnocování profilů (Azure Search REST API verze 2015-02-28-Preview) | Microsoft Docs"
description: "Služba Azure Search je hostované cloudové vyhledávací službě, která podporuje ladění výsledků seřazený podle uživatelské profily vyhodnocování."
services: search
documentationcenter: 
author: HeidiSteen
manager: jhubbard
editor: 
ms.assetid: bfd62649-13d7-40b3-a8fa-85521a15084d
ms.service: search
ms.devlang: rest-api
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.author: heidist
ms.date: 10/27/2016
ms.openlocfilehash: a67637d149a84313270c03d21acf8a9c1870be05
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="scoring-profiles-azure-search-rest-api-version-2015-02-28-preview"></a>Vyhodnocování profilů (Azure Search REST API verze 2015-02-28-Preview)
> [!NOTE]
> Tento článek popisuje vyhodnocování profily v nástroji [2015-02-28-Preview](search-api-2015-02-28-preview.md). Aktuálně není žádný rozdíl mezi `2016-09-01` verze popsané na [MSDN](http://msdn.microsoft.com/library/azure/mt183328.aspx) a `2015-02-28-Preview` zde popsané verze, ale tento dokument nabízíme přesto chcete-li poskytovat dokumentu pokrytí napříč celou rozhraní API.
>
>

## <a name="overview"></a>Přehled
Vyhodnocování odkazuje výpočet skóre vyhledávání pro každou položku vrátila ve výsledcích hledání. Skóre je slouží jako ukazatel relevance položky v rámci aktuální operace vyhledávání. Čím více bodů, více příslušné položky. Ve výsledcích hledání jsou položky seřazené z vysoké na dolní, podle skóre vyhledávání pro každou položku vypočítat pořadí.

Služba Azure Search používá výchozí vyhodnocování k výpočtu počáteční skóre, ale můžete přizpůsobit výpočtu prostřednictvím profil vyhodnocování. Vyhodnocování profily získáte větší kontrolu nad hodnocení položek ve výsledcích hledání. Můžete například zvýšit položky podle jejich potenciální výnosy, zvýšení úrovně novější položky nebo možná zvýšení položky, které byly v inventáři příliš dlouhý.

Profil vyhodnocování je součástí definice indexu skládá z pole, funkce a parametry.

Získáte představu o profil vyhodnocování vypadá jako, následující příklad ukazuje jednoduchý profil s názvem 'geograficky'. Tato zvyšuje položky, které mají hledaný termín v `hotelName` pole. Používá také `distance` funkce upřednostnit položky, které jsou v rámci deset kilometrech aktuální umístění. Pokud někdo hledá výraz 'DIČ' a 'DIČ, se stane se jednat o část názvu hotelů, bude zobrazovat na vyšších pozicích ve výsledcích hledání dokumenty, které zahrnují hotels s 'DIČ'.

    "scoringProfiles": [
      {
        "name": "geo",
        "text": {
          "weights": { "hotelName": 5 }
        },
        "functions": [
          {
            "type": "distance",
            "boost": 5,
            "fieldName": "location",
            "interpolation": "logarithmic",
            "distance": {
              "referencePointParameter": "currentLocation",
              "boostingDistance": 10
            }
          }
        ]
      }
    ]

Chcete-li použít tento profil vyhodnocování, je formulovali dotaz k určení profilu na řetězec dotazu. V dotazu níže, Všimněte si, parametr dotazu `scoringProfile=geo` v požadavku.

    GET /indexes/hotels/docs?search=inn&scoringProfile=geo&scoringParameter=currentLocation--122.123,44.77233&api-version=2015-02-28-Preview

Tento dotaz hledá výraz 'DIČ' a předá v aktuálním umístění. Všimněte si, že tento dotaz obsahuje dalších parametrů, jako například `scoringParameter`. Parametry dotazu jsou popsané v [vyhledávání dokumentů (API služby Azure Search)](search-api-2015-02-28-preview.md#SearchDocs).

Klikněte na tlačítko [příklad](#example) zobrazíte podrobnější příklad profil vyhodnocování.

## <a name="what-is-default-scoring"></a>Co je vyhodnocování výchozí?
Vyhodnocování vypočítá skóre vyhledávání pro každou položku v sadě rank seřazený výsledek. Každá položka v sadě výsledků hledání je přiřazen skóre vyhledávání a potom seřazeny nejvyšší nejméně časté. Položky s vyšší skóre se vrátí do aplikace. Ve výchozím nastavení, jsou vráceny top 50, ale můžete použít `$top` parametr vrátit menší nebo větší počet položek (až 1000 jedinou odpověď).

Ve výchozím nastavení se vypočítá skóre vyhledávání podle statistické vlastnosti dat a dotazu. Služba Azure Search Vyhledá dokumenty, které zahrnují podmínky vyhledávání v řetězci dotazu (některé nebo všechny, v závislosti na `searchMode`), upřednostňuje dokumentů, které obsahují velký počet instancí hledaný termín. Skóre vyhledávání se zvyšuje i vyšší, pokud je přes svátek data, ale v tomto dokumentu běžné výjimečných podmínek. Základ pro tento přístup k výpočetní závažnosti se označuje jako TF-IDF nebo (termín frekvence inverzní dokumentu frekvenci).

Za předpokladu, že neexistuje žádné vlastní řazení, výsledky, jsou pak seřazeny podle skóre vyhledávání dřív, než se vrátí volající aplikace. Pokud `$top` není zadán, 50 položek, které mají nejvyšší hledání, jsou vráceny skóre.

Hledání skóre hodnoty můžete opakovat po dobu je sada výsledků dotazu. Například můžete mít 10 položek se skóre 1.2, 20 položky se skóre 1.0 a 20 položky se skóre 0,5. Pokud více přístupů stejné skóre vyhledávání, řazení stejné scored položky není definována a není stabilní. Spusťte dotaz znovu a může se zobrazit položky posunutí pozice. Zadána dvě položky s identické skóre není zaručeno, která se zobrazí jako první.

## <a name="when-to-use-custom-scoring"></a>Kdy použít vlastní vyhodnocování
Pokud není výchozí řazení chování přejděte daleko dostatek v splňuje obchodních cílů, měli byste vytvořit jeden nebo více profily vyhodnocování. Například můžete rozhodnout, že závažnost hledání by měl upřednostnit nově přidané položky. Podobně můžete mít pole, které obsahuje zisku nebo některé jiné pole označující potenciál. Zvyšovat skóre přístupů, které přinášejí výhody do vaší firmy může být důležitým faktorem při rozhodování, pokud chcete používat profily vyhodnocování.

Na základě důležitosti řazení se také implementuje pomocí profily vyhodnocování. Vezměte v úvahu Hledat výsledky na stránkách, které jste použili v minulosti, která umožňují seřadit ceníku, datum, hodnocení ani užitek. Vyhodnocování profily ve službě Azure Search, jednotky, relevance, možnost. Definice relevance řídí můžete zabezpečuje pomocí vhodné obchodních cílů a typ vyhledáváním, kterou chcete doručovat.

<a name="example"></a>

## <a name="example"></a>Příklad
Jak jsme uvedli, přizpůsobené vyhodnocování se implementuje pomocí vyhodnocování profily definice ve schématu indexu.

Tento příklad ukazuje schéma indexu s dva profily vyhodnocování (`boostGenre`, `newAndHighlyRated`). Jakýkoli dotaz proti tento index, který zahrnuje buď profil jako parametr dotazu bude profil používat skóre pro sadu výsledků dotazu.

    {
      "name": "musicstoreindex",
      "fields": [
        { "name": "key", "type": "Edm.String", "key": true },
        { "name": "albumTitle", "type": "Edm.String" },
        { "name": "albumUrl", "type": "Edm.String", "filterable": false },
        { "name": "genre", "type": "Edm.String" },
        { "name": "genreDescription", "type": "Edm.String", "filterable": false },
        { "name": "artistName", "type": "Edm.String" },
        { "name": "orderableOnline", "type": "Edm.Boolean" },
        { "name": "rating", "type": "Edm.Int32" },
        { "name": "tags", "type": "Collection(Edm.String)" },
        { "name": "price", "type": "Edm.Double", "filterable": false },
        { "name": "margin", "type": "Edm.Int32", "retrievable": false },
        { "name": "inventory", "type": "Edm.Int32" },
        { "name": "lastUpdated", "type": "Edm.DateTimeOffset" }
      ],
      "scoringProfiles": [
        {
          "name": "boostGenre",
          "text": {
            "weights": {
              "albumTitle": 1.5,
              "genre": 5,
              "artistName": 2
            }
          }
        },
        {
          "name": "newAndHighlyRated",
          "functions": [
            {
              "type": "freshness",
              "fieldName": "lastUpdated",
              "boost": 10,
              "interpolation": "quadratic",
              "freshness": {
                "boostingDuration": "P365D"
              }
            },
            {
              "type": "magnitude",
              "fieldName": "rating",
              "boost": 10,
              "interpolation": "linear",
              "magnitude": {
                "boostingRangeStart": 1,
                "boostingRangeEnd": 5,
                "constantBoostBeyondRange": false
              }
            }
          ]
        }
      ],
      "suggesters": [
        {
          "name": "sg",
          "searchMode": "analyzingInfixMatching",
          "sourceFields": ["albumTitle", "artistName"]
        }
      ]
    }


## <a name="workflow"></a>Pracovní postupy
Pokud chcete implementovat vlastní vyhodnocování chování, přidáte profil vyhodnocování schématu, která definuje index. Může mít až 16 vyhodnocování profily v indexu (viz [omezení služby](search-limits-quotas-capacity.md)), ale v době v jakékoli dané dotazu lze určit pouze jeden profil.

Začínat [šablony](#bkmk_template) uvedené v tomto tématu.

Zadejte název. Vyhodnocování profily jsou volitelné, ale pokud přidáte jednu, název je povinný. Dodržujte zásady vytváření názvů pro pole (začíná písmenem, zabraňuje speciální znaky a vyhrazená slova). V tématu [pravidla pojmenování](http://msdn.microsoft.com/library/azure/dn857353.aspx) Další informace.

Tělo profil vyhodnocování se vytvářejí na základě vyvážené pole a funkce.

### <a name="weights"></a>Váhu
`weights` Vlastnost profil vyhodnocování Určuje dvojice název hodnota, které přiřaďte relativní váhu pole. V [příklad](#example), jsou tato pole albumTitle genre a artistName boosted 1.5, 5, 2, v uvedeném pořadí. Proč je genre boosted mnohem vyšší než ostatní? Pokud se provádí vyhledávání přes data, která je poněkud homogenní (jako je tomu u 'genre' v `musicstoreindex`), může být nutné větší odchylky v relativní váhu. Například v `musicstoreindex`, 'rock' zobrazí jako obou genre a v popisech genre stejně jako obsahuje jiné spojení. Pokud chcete, aby genre k převáží nad popis genre, bude nutné pole genre mnohem vyšší relativní váhu.

### <a name="functions"></a>Funkce
Funkce se používají, pokud je potřeba pro konkrétní kontexty další výpočty. Funkce platné typy jsou `freshness`, `magnitude`, `distance` a `tag`. Jednotlivé funkce obsahuje parametry, které jsou jedinečné pro ho.

* `freshness`Pokud chcete zvýšit jak starý, nebo nový položku je. Tuto funkci lze použít pouze s pole data a času (`Edm.DataTimeOffset`). Poznámka: `boostingDuration` atribut se používá jenom s aktuálnosti funkce.
* `magnitude`by měla být použita, pokud chcete zvýšit na základě jak horní nebo nízkou je číselná hodnota. Scénáře, které volají pro tuto funkci zahrnují zvyšovat skóre marže, nejvyšší cenu, nejnižší cena nebo počet souborů ke stažení. Můžete nechat provést zpětnou rozsahu, vysoké nízká, pokud chcete, aby vzoru inverzní (například k položkám nárůst nižší cenou víc než vyšší za cenu položky). Zadaný rozsah ceny ze 100 USD $ 1, byste měli nastavit `boostingRangeStart` na 100 a `boostingRangeEnd` v 1 pro zvýšení nižší cenou položky. Tato funkce slouží pouze s dvojitou a celé číslo pole.
* `distance`by měl použít, pokud chcete zvýšit blízkosti nebo zeměpisné umístění. Tuto funkci lze použít pouze s `Edm.GeographyPoint` pole.
* `tag`by měl použít, pokud chcete zvýšit podle značky společné mezi dokumenty a vyhledávací dotazy. Tuto funkci lze použít pouze s `Edm.String` a `Collection(Edm.String)` pole.

#### <a name="rules-for-using-functions"></a>Pravidla pro používání funkce
* Typ funkce (aktuálnosti, rozsahem, vzdálenost, značka) musí být malá písmena.
* Funkce nesmí obsahovat hodnoty null nebo prázdný. Konkrétně Pokud zahrnete název pole, musíte ho nastavit.
* Funkce lze použít pouze na filtrovatelných pole. V tématu [Create Index](search-api-2015-02-28-preview.md#CreateIndex) Další informace o filtrování pole.
* Funkce lze použít pouze na pole, které jsou definovány v kolekci polí indexu.

Po definování indexu sestavení indexu tím, že nahrajete schéma indexu, za nímž následuje dokumenty. V tématu [Create Index](search-api-2015-02-28-preview.md#CreateIndex) a [přidat nebo aktualizovat dokumenty](search-api-2015-02-28-preview.md#AddOrUpdateDocuments) pokyny týkající se těchto operací. Po index, byste měli mít funkční vyhodnocování profil, který pracuje s daty vyhledávání.

<a name="bkmk_template"></a>

## <a name="template"></a>Šablona
Tato část uvádí syntaxi a šablony pro vyhodnocování profily. Odkazovat na [indexu reference na atribut](#bkmk_indexref) v další části Popis atributů.

    ...
    "scoringProfiles": [
      {
        "name": "name of scoring profile",
        "text": (optional, only applies to searchable fields) {
          "weights": {
            "searchable_field_name": relative_weight_value (positive #'s),
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
            }

            // (- or -)

            "freshness": {
              "boostingDuration": "..." (value representing timespan over which boosting occurs)
            }

            // (- or -)

            "distance": {
              "referencePointParameter": "...", (parameter to be passed in queries to use as reference location)
              "boostingDistance": # (the distance in kilometers from the reference location where the boosting range ends)
            }

            // (- or -)

            "tag": {
              "tagsParameter": "..." (parameter to be passed in queries to specify list of tags to compare against target field)
            }
          }
        ],
        "functionAggregation": (optional, applies only when functions are specified)
            "sum (default) | average | minimum | maximum | firstMatching"
      }
    ],
    "defaultScoringProfile": (optional) "...",
    ...

<a name="bkmk_indexref"></a>

## <a name="scoring-profile-property-reference"></a>Odkaz na vlastnost profilu vyhodnocování
> [!NOTE]
> Vyhodnocování funkce lze použít pouze na pole, které jsou filtrování.
>
>

| Vlastnost | Popis |
| --- | --- |
| `name` |Vyžaduje se. Toto je název profilu vyhodnocování. Postupuje stejnými zásadami vytváření názvů pole. Ho musí začínat písmenem, nemůže obsahovat tečky, dvojtečky nebo @ symboly a nesmí začínat frázi "azureSearch" (malá a velká písmena). |
| `text` |Obsahuje vlastnost váhu. |
| `weights` |Volitelné. Dvojice název hodnota, která určuje název pole a relativní váhy. Relativní váhu musí být kladné celé číslo nebo číslo s plovoucí desetinnou čárkou. Můžete zadat název pole bez odpovídající váhu. Vah se používají k označení význam jedno pole vzhledem k jiné. |
| `functions` |Volitelné. Všimněte si, že vyhodnocování funkce lze použít pouze na pole, které jsou filtrování. |
| `type` |Požaduje se pro funkce vyhodnocování. Určuje typ funkce, která má použít. Platné hodnoty patří `magnitude`, `freshness`, `distance` a `tag`. Můžete zahrnout více než jednu funkci každý profil vyhodnocování. Název funkce musí být malá písmena. |
| `boost` |Požaduje se pro funkce vyhodnocování. Kladné číslo použije jako násobitel základního skóre. Nesmí se rovnat 1. |
| `fieldName` |Požaduje se pro funkce vyhodnocování. Vyhodnocování funkce lze použít pouze na pole, které jsou součástí kolekce polí indexu a které jsou filtrování. Kromě toho každý typ funkce zavádí další omezení (podle aktuálnosti se používá s pole data a času, podle magnitudy se celé číslo nebo double poli, vzdálenost vložením polí, umístění a značky s řetězec nebo řetězec kolekci polí). Zadávat lze pouze jedno pole za definici funkce. Pokud chcete používat rozsahem dvakrát v jednom profilu, musíte například obsahovat dvě definice rozsahem, jeden pro každé pole. |
| `interpolation` |Požaduje se pro funkce vyhodnocování. Definuje sklon pro kterou zvyšování skóre od začátku rozsahu na konec rozsahu. Platné hodnoty patří `linear` (výchozí), `constant`, `quadratic`, a `logarithmic`. V tématu [nastavit interpolace](#bkmk_interpolation) podrobnosti. |
| `magnitude` |Odhad vyhodnocování funkce se používají pro úpravu pořadí na základě rozsahu hodnot pro číselné pole. Některé z nejběžnějších příklady použití tohoto jsou:<ul><li>Z hodnocení hvězdičkami: Alter vyhodnocování na základě hodnoty v poli "Hvězdičky hodnocení". Při dvou položek jsou relevantní, zobrazí se první položka s vyšší hodnocení.</li><li>Okraj: Pokud dva dokumenty jsou relevantní, prodejce chtít zvýšení dokumenty, které mají vyšší okraje nejdřív.</li><li>Klikněte na tlačítko počty: aplikace, které sledují kliknutím prostřednictvím akce pro produkty nebo stránky, můžete použít rozsahem nárůst položek, které se mají tendenci se získat nejvíce provoz.</li><li>Stáhněte si počty: pro aplikace, sledovat stažení a umožňuje funkce rozsahem, můžete zvýšit položek, které mají nejvíce stahování.</li></ul> |
| `magnitude:boostingRangeStart` |Nastaví počáteční hodnotu rozsahu, který stanoví skóre pro magnitudu. Hodnota musí být celé číslo nebo číslo s plovoucí desetinnou čárkou. U hodnocení hvězdičkami 1 až 4 by to bylo 1. Pro rozsahy nad 50 % by to bylo 50. |
| `magnitude:boostingRangeEnd` |Nastaví koncovou hodnotu rozsahu, který stanoví skóre pro magnitudu. Hodnota musí být celé číslo nebo číslo s plovoucí desetinnou čárkou. U hodnocení hvězdičkami 1 až 4 by to bylo 4. |
| `magnitude:constantBoostBeyondRange` |Platné hodnoty jsou true nebo false (výchozí). Pokud je nastavena na hodnotu true, bude se skóre bude pokračovat pro dokumenty, které mají hodnotu cílového pole, které je vyšší než horní mez rozsahu. Pokud hodnotu NEPRAVDA, nárůst této funkce se neaplikuje na dokumenty s hodnotou cílového pole, která spadá mimo rozsah. |
| `freshness` |Aktuálnosti vyhodnocování funkce se používá pro úpravu pořadí položek na základě hodnot v polích typu DateTimeOffset. Položka s pozdější může být například vyšší než starší položky seřazeny. (Všimněte si, že je také možné rank věci, jako je kalendář události s budoucí data tak, že položky blíže současnosti můžete jejíž pořadí se určí vyšší než položky Další v budoucnu.) V aktuálním vydání služby bude opraven jeden konec rozsahu na aktuální čas. Je čas v minulosti založené na druhém konci `boostingDuration`. Pro zvýšení rozsah časů v budoucnosti použít jako záporné `boostingDuration`. Rychlost zvyšovat skóre změní z maximální a minimální rozsah je dáno interpolace použít profil vyhodnocování (viz následující obrázek). Chcete-li obrátit na zvýšení skóre faktor použít, zvolte faktor zesílení menší než 1. |
| `freshness:boostingDuration` |Nastaví konec platnosti, po kterém se u konkrétního dokumentu přestane zvyšovat skóre. V tématu [nastavit boostingDuration](#bkmk_boostdur) v následující části Syntaxe a příklady. |
| `distance` |Vzdálenost, o kterou vyhodnocování funkce se používají pro úpravu skóre dokumentů na základě o tom, zavřít nebo daleko jsou od referenční geografické polohy. Odkaz na umístění je zadána jako součást dotazu parametr (pomocí `scoringParameter` parametr dotazu) jako fyzický pevný disk, lat argument. |
| `distance:referencePointParameter` |Parametr, který se má předávat do dotazů jako referenční umístění. scoringParameter je parametr dotazu. V tématu [vyhledávání dokumentů](search-api-2015-02-28-preview.md#SearchDocs) popisy parametry dotazu. |
| `distance:boostingDistance` |Číslo, které určuje vzdálenost v kilometrech od referenčního místa, které končí oblast zvýšení skóre. |
| `tag` |Značky vyhodnocování funkce se používá pro úpravu skóre dokumentů podle značek v dokumentech a vyhledávací dotazy. Bude boosted dokumenty, které obsahují značky společné s vyhledávací dotaz. Značky pro vyhledávací dotaz je zadat jako parametr vyhodnocování v každé žádosti o vyhledávání (pomocí `scoringParameter` parametr dotazu). |
| `tag:tagsParameter` |Parametr, který se bude předávat v dotazech a bude určovat značky pro konkrétní žádost. `scoringParameter`je parametr dotazu. V tématu [vyhledávání dokumentů](search-api-2015-02-28-preview.md#SearchDocs) popisy parametry dotazu. |
| `functionAggregation` |Volitelné. Platí, pouze pokud je zadán funkce. Platné hodnoty patří: `sum` (výchozí), `average`, `minimum`, `maximum`, a `firstMatching`. Skóre vyhledávání je jediná hodnota, která se počítá z více proměnných, včetně víc funkcí. Tento atributy Určuje, jak se zvyšuje všech funkcí zkombinované do jednoho agregační nárůst, který se potom použije k základní dokumentu skóre. Základní hodnocení je založeno na hodnotě tf-idf vypočítaný z dokumentu a vyhledávací dotaz. |
| `defaultScoringProfile` |Při provádění žádost o vyhledávání, pokud není zadán žádný profil vyhodnocování, vyhodnocování výchozím nastavení je použité (tf-idf pouze). Výchozí název profilu vyhodnocování lze nastavit zde způsobuje Azure Search používat tento profil, když žádný konkrétní profil je uveden v požadavku hledání. |

<a name="bkmk_interpolation"></a>

## <a name="set-interpolations"></a>Sada interpolace
Interpolace umožňují definovat sklon, pro kterou zvyšování skóre od začátku rozsahu na konec rozsahu. Můžete použít následující interpolace:

* `Linear`: Pro položky, které jsou v rozsahu, max a min bude provedeno nárůst položky v rámci neustále klesá. Lineární je výchozí interpolace pro profil vyhodnocování.
* `Constant`: Pro položky, které jsou v rámci začátek a konec rozsahu se použijí konstantní nárůst rank výsledky.
* `Quadratic`: Ve srovnání s lineární interpolace, který má neustále klesá nárůst Kvadratická sníží původně menší tempem a potom jako se blíží koncový rozsah, sníží v intervalu mnohem vyšší. Tato možnost interpolace není povolena ve značce funkce vyhodnocování.
* `Logarithmic`: Ve srovnání s lineární interpolace, který má neustále klesá nárůst toto zobrazení se původně sníží tempem vyšší a potom jako se blíží koncový rozsah, sníží v mnohem menší intervalu. Tato možnost interpolace není povolena ve značce funkce vyhodnocování.

<a name="Figure1"></a> ![][1]

<a name="bkmk_boostdur"></a>

## <a name="set-boostingduration"></a>Sada boostingDuration
`boostingDuration`je atribut aktuálnosti funkce. Použijte je k nastavení vypršení platnosti období, po které zvyšovat skóre se u konkrétního dokumentu přestane. Například pro zvýšení řadu produktů nebo brand propagační dobu 10 dní, zadali byste období 10 dnů jako "P10D" pro tyto dokumenty. Nebo pro zvýšení nadcházející události v příští týden zadejte "-P7D".

`boostingDuration`musí být naformátovaná jako hodnota "hodnoty doby podle" XSD (omezená podmnožina hodnoty doby trvání ISO 8601). Je tento vzor: `[-]P[nD][T[nH][nM][nS]]`.

Následující tabulka obsahuje několik příkladů.

| Doba trvání | boostingDuration |
| --- | --- |
| 1 den |"P1D" |
| 2 dny a 12 hodin |"P2DT12H" |
| 15 minut |"PT15M" |
| 30 dní, 5 hodin, 10 minut, a 6.334 sekund |"P30DT5H10M6.334S" |

Další příklady najdete v tématu [schématu XML: datové typy (adrese W3.org webový server)](http://www.w3.org/TR/xmlschema11-2/).

**Viz také**
[rozhraní API REST služby vyhledávání systému Azure](http://msdn.microsoft.com/library/azure/dn798935.aspx) na webu MSDN <br/>
[Vytvoření indexu (Azure Search rozhraní API)](http://msdn.microsoft.com/library/azure/dn798941.aspx) na webu MSDN<br/>
[Přidat profil vyhodnocování do indexu vyhledávání](http://msdn.microsoft.com/library/azure/dn798928.aspx) na webu MSDN<br/>

<!--Image references-->
[1]: ./media/search-api-scoring-profiles-2015-02-28-Preview/scoring_interpolations.png
