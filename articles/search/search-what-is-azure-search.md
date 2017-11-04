---
title: Co je Azure Search | Microsoft Docs
description: "Služba Azure Search je plně spravovaná hostované cloudové vyhledávací službu. Další informace v přehledu této funkce."
services: search
manager: jhubbard
author: ashmaka
documentationcenter: 
ms.assetid: 50bed849-b716-4cc9-bbbc-b5b34e2c6153
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 06/26/2017
ms.author: ashmaka
ms.openlocfilehash: 9893be47ec0c2f58ca206ec7c1bce13734513390
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/03/2017
---
# <a name="what-is-azure-search"></a>Co je Azure Search?
Služba Azure Search je řešení vyhledávání jako služby cloud, které poskytuje vývojářům rozhraní API a nástroje pro přidání bohaté vyhledáváním přes data v aplikacích pro webové, mobilní a enterprise.

Funkce je zveřejněna prostřednictvím jednoduchou [REST API](/rest/api/searchservice/) nebo [.NET SDK](search-howto-dotnet-sdk.md) který zakrývá složitost vyplývajících technologie vyhledávání. Kromě rozhraní API portál Azure poskytuje správu a podporu při vytváření prototypu. Microsoft spravuje infrastruktury a dostupnost.

<a name="feature-drilldown"></a>

## <a name="feature-summary"></a>Souhrn funkcí

| Kategorie | Funkce |
|----------|----------|
|Fulltextové vyhledávání a analýza textu | [Fulltextové vyhledávání](search-lucene-query-architecture.md) je primární případ použití pro většinu aplikací na základě hledání. Dotazy můžete formulovali pomocí podporovaných syntaxe. <br/><br/>[**Jednoduchá syntaxe dotazů** ](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search) poskytuje logické operátory, operátory hledání fráze, přípony operátory, operátorů.<br/><br/>[**Syntaxe dotazů Lucene** ](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search) zahrnuje všechny operace v jednoduchý syntaktický s rozšířeními pro přibližné vyhledávání, vyhledávání blízkých výrazů, termín zvyšovat skóre a regulární výrazy.| 
| Integrace dat | Indexů Azure Search přijímat data z jakéhokoli zdroje, zadaný je odeslán jako datovou strukturu JSON. <br/><br/> Volitelně můžete pro podporované zdroje dat v Azure, můžete použít [ **indexery** ](search-indexer-overview.md) automaticky Procházet [Azure SQL Database](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md), [Azure Cosmos DB](search-howto-index-documentdb.md), nebo [úložiště objektů Azure Blob](search-howto-indexing-azure-blob-storage.md) synchronizaci indexu vyhledávání je obsahu s primární data store. Můžete provádět Azure Blob indexery *hádání dokumentu toho* pro [indexování formáty souborů hlavní](search-howto-indexing-azure-blob-storage.md), včetně dokumentů Microsoft Office, PDF a HTML. |
| Lingvistické analýzy | Analyzátory jsou komponenty používá u textů zpracování během operace indexování a vyhledávání. Existují dva typy. <br/><br/>[**Vlastní lexikální analyzátorů** ](https://docs.microsoft.com/rest/api/searchservice/custom-analyzers-in-azure-search) se používají pro komplexní vyhledávací dotazy výslovnosti porovnáváním a regulární výrazy. <br/><br/>[**Analyzátory jazyka** ](https://docs.microsoft.com/rest/api/searchservice/language-support) z Lucene nebo Microsoft se používají pro konkrétní jazyk linguistics včetně příkaz časů, pohlaví, nestandardní množném čísle podstatná jména (například "myši' oproti 'myši'), word zrušte vícesložkovém, inteligentně zpracování dělení slov (pro jazyky, bez mezer) a další. |
| Geograficky vyhledávání | Služba Azure Search zpracovává, filtrů a zobrazí zeměpisné umístění. Umožňuje uživatelům procházet data podle blízkosti výsledků na vyhledávacím fyzické umístění. [Přehrát toto video](https://channel9.msdn.com/Shows/Data-Exposed/Azure-Search-and-Geospatial-Data) nebo [Zkontrolujte tato ukázka](https://github.com/Azure-Samples/search-dotnet-asp-net-mvc-jobs) Další informace. |
| Funkce uživatelského rozhraní | [**Hledání návrhy** ](https://docs.microsoft.com/rest/api/searchservice/suggesters) lze povolit pro našeptávání dotazů v panelu vyhledávání. Skutečné dokumenty v indexu jsou navrhované jako uživatelé zadání částečné vyhledávání. <br/><br/>[**Fasetové navigace** ](https://docs.microsoft.com/azure/search/search-faceted-navigation) je povolená prostřednictvím parametru jeden dotaz. Služba Azure Search vrátí struktura Fasetové navigace, který můžete použít jako kódu seznam kategorií pro účely filtrování řízené samotným (například k položkám katalogu filtru cena range nebo brand). <br/><br/> [**Filtry** ](https://docs.microsoft.com/rest/api/searchservice/odata-expression-syntax-for-azure-search) lze začlenit Fasetové navigace do uživatelského rozhraní aplikace, zvýšit formulování dotazu a na základě zadaného uživatelem nebo vývojáře kritéria filtru. Vytvoření filtrů pomocí syntaxe OData.<br/><br/> [**Zvýrazňování** ](https://docs.microsoft.com/rest/api/searchservice/Search-Documents) platí formátování textu na odpovídající – klíčové slovo ve výsledcích hledání. Můžete vybrat pole, která vrátí zvýrazněná fragmenty.<br/><br/>[**Řazení** ](https://docs.microsoft.com/rest/api/searchservice/Search-Documents) je poskytovat několik polí prostřednictvím schéma indexu a potom přepínat stav v době dotazů s parametrem jeden vyhledávání.<br/><br/> [**Stránkování** ](search-pagination-page-layout.md) a omezení výsledky hledání je jednoduchá pomocí ovládacího prvku jemně ujít Azure Search nabízí než výsledky hledání.  
| Důležitost | [**Jednoduché vyhodnocování** ](/rest/api/searchservice/add-scoring-profiles-to-a-search-index) je klíčovou výhodou Azure Search. Vyhodnocování profily se používají pro modelování relevance jako funkce hodnot v samotné dokumenty. Například můžete chtít novější produkty nebo slevou produktů a zobrazovat na vyšších pozicích ve výsledcích hledání. Můžete také vytvořit vyhodnocování profilů pomocí značek pro individuální vyhodnocování podle preferencí vyhledávání zákazníka jste sledovat a uloženy odděleně. |
| Monitorování a vytváření sestav | [**Analýza provozu vyhledávání** ](search-traffic-analytics.md) jsou shromažďovat a analyzovat k odemknutí přehledy z co psaní do vyhledávacího pole uživatelů. <br/><br/>Metriky pro dotazy na za sekundu, latence a omezení zachycení a uvedený v portálu stránky s bez další nezbytné konfigurace. Můžete také snadno monitorování index a počty dokumentů, aby kapacity můžete upravit podle potřeby. Další informace najdete v tématu [služby správy](search-manage.md) |
| Nástroje pro vytváření prototypů a kontroly | Na portálu, můžete použít [ **Průvodce importem dat** ](search-import-data-portal.md) indexery, Návrhář index stát až index, konfigurace a [ **Průzkumník služby Search** ](search-explorer.md)Upřesnit profily vyhodnocování a testování dotazů. Můžete také otevřít žádný index k zobrazení jeho schématu. |
| Infrastruktura | **Vysokou dostupností platformy** zajistí prostředí služby velmi spolehlivé vyhledávání. Při změně měřítka správně, [Azure Search nabízí SLA 99,9 %](https://azure.microsoft.com/support/legal/sla/search/v1_0/).<br/><br/> **Plně spravované a škálovatelné** jako řešení začátku do konce, vyžaduje Azure Search absolutně žádné správu infrastruktury. Služby, můžete přizpůsobit svým potřebám pomocí příjmu v dvěma rozměry zpracovat další úložiště dokumentů, vyšší zatížení dotazu nebo obojí.

## <a name="how-to-use-azure-search"></a>Jak používat Azure Search
### <a name="step-1-provision-service"></a>Krok 1: Zřizování služby
Můžete číselníku si služby Azure Search v [portál Azure](https://portal.azure.com/) nebo pomocí [rozhraní API pro správu prostředků Azure](/rest/api/searchmanagement/). Můžete buď bezplatné služby sdílet s jiným odběratelům nebo [placené vrstvy](https://azure.microsoft.com/pricing/details/search/) , vyhrazuje vlastní prostředky, které jsou používány pouze služby. U placených úrovní je možné škálovat službu v dvě dimenze: 

- Přidáte repliky růst vaší dostatečnou kapacitu pro zpracování dotazu v případě velkého zatížení.   
- Přidáte oddíly růst úložiště pro další dokumenty. 

Podle dokumentu propustnost úložiště a dotaz zpracování samostatně, můžete nakalibrovat financování na základě požadavků produkční.

### <a name="step-2-create-index"></a>Krok 2: Vytvoření indexu
Předtím, než můžete nahrát vyhledávat obsah, je nutné zadat indexu Azure Search. Index je jako tabulku databáze, která obsahuje vaše data a může přijmout vyhledávací dotazy. Můžete definovat schéma indexu mapovat tak, aby odrážela strukturu dokumenty, které chcete hledat, podobně jako na pole v databázi.

Schéma může být vytvořené na portálu Azure nebo programově pomocí [.NET SDK](search-howto-dotnet-sdk.md) nebo [REST API](/rest/api/searchservice/).

### <a name="step-3-index-data"></a>Krok 3: Index dat
Po definování indexu, jste připravení nahrát obsah. Můžete použít push nebo pull modelu.

Pull model načítá data z externích zdrojů. Je podporován prostřednictvím *indexery* , zjednodušit a automatizovat aspektů přijímání dat, jako je například připojení k čtení a serializaci dat. [Indexery](/rest/api/searchservice/Indexer-operations) jsou k dispozici pro databázi Cosmos Azure, Azure SQL Database, úložiště objektů Blob Azure a SQL Server hostované ve virtuálním počítači Azure. Indexer pro můžete nakonfigurovat na vyžádání nebo naplánované aktualizace.

Push model je zajišťováno prostřednictvím sady SDK nebo rozhraní REST API, používá k odeslání aktualizované dokumentů do indexu. Nabízet data z prakticky libovolný datové sady ve formátu JSON. V tématu [přidání, aktualizace nebo odstranění dokumentů](/rest/api/searchservice/addupdate-or-delete-documents) nebo [jak používat sadu .NET SDK)](search-howto-dotnet-sdk.md) pokyny k načítání dat.

### <a name="step-4-search"></a>Krok 4: vyhledávání
Po naplnění indexu, můžete [vydávat vyhledávací dotazy](/rest/api/searchservice/Search-Documents) do vašeho koncového bodu služby jednoduché požadavky HTTP pomocí rozhraní REST API nebo .NET SDK.

## <a name="how-azure-search-compares"></a>Jak se porovnává Azure Search

Zákazníci často požádat, jak Azure Search porovná s jinými řešeními související vyhledávání. Následující tabulka shrnuje hlavní rozdíly.

| Ve srovnání s | Hlavní rozdíly |
|--|--|
|Bing | [Rozhraní API služby Bing webové Search](https://docs.microsoft.com/azure/cognitive-services/bing-web-search/) indexy v vyhledávače Bing.com hledá odpovídající podmínky odešlete. Indexy jsou vytvořeny z jazyka HTML, XML a dalších webového obsahu na veřejné weby. [Hledání vlastní Bing](https://docs.microsoft.com/azure/cognitive-services/bing-custom-search/) nabízí technologie prohledávacího modulu pro web typy obsahu, vymezeny do jednotlivých webů.<br/><br/>Služba Azure Search vyhledá indexu, které definujete, vyplní s daty a dokumenty, které vlastníte, často z různých zdrojů. Vyhledávání systému Azure má capabilies prohledávacího modulu pro některé zdroje dat prostřednictvím [indexery](search-indexer-overview.md), ale žádné dokument JSON, který vyhovuje na schéma indexu můžete vkládání do jediné, konsolidované s možností vyhledávání prostředků. |
|Hledání v databázi | [Fulltextové vyhledávání SQL serveru](https://docs.microsoft.com/sql/relational-databases/search/full-text-search) je pro obsah interní databázového systému, v tabulkách SQL. <br/><br/>Služba Azure Search ukládá obsah z heterogenní zdrojů a nabízí specializované text zpracování funkce jako je například analýzy jazykové a vlastní. [Textu v plném znění vyhledávacího webu](search-lucene-query-architecture.md) ve službě Azure Search je založený na Apache Lucene oborový standard v načítání informací. <br/><br/>Využití prostředků je jiný bod důraz. Vyhledávání v přirozeném jazyce, je často výpočetně náročné. Přesměrovává vyhledávání vyhrazené řešení zachovává prostředky pro zpracování transakcí. Externího vyhledáváním můžete snadno upravit měřítko tak, aby odpovídaly dotazu svazku.|
|Vyhrazené hledání řešení | Na pracovišti nebo v cloudu řešení služby jsou vyhrazené hledání řešení s funkcemi celé spektrum. Hledání technologie obvykle nabízejí kontrolu nad indexování a dotaz kanály, přístup k širší dotazu a filtrování syntaxe, řízení celého pořadí a relevance a funkcích pro řízené samotným a inteligentní vyhledávání. <br/><br/>Můžete najít vyhrazené hledání řešení nabídnut jako cloudová služba nebo jako samostatný server hostovaný místně nebo na virtuálním počítači. Cloudová služba je správnou volbou, pokud chcete [řešení na klíč s minimální režii a údržby a upravit škálování](#cloud-service-advantage). <br/><br/>V rámci cloudu zlepší několik poskytovatelů nabízejí funkce porovnatelný z hlediska směrný plán s fulltextové vyhledávání, hledání geograficky a schopnost zvládat určitá úroveň nejednoznačnosti v hledání vstupy. Obvykle má [specializované funkce](#feature-drilldown), nebo jednoduchosti a celkové jednoduchost rozhraní API, nástrojů a správy, který určuje nejlepší. |

Mezi poskytovatelů cloudu Azure Search je nejsilnější pro fulltextové vyhledávání úlohy nad obsahu úložiště a databáze v Azure, pro aplikace, které závisí hlavně na hledání pro načítání informací o i obsahu navigace. Klíče síly patří:

+ Integrace Azure dat (pro indexaci) ve vrstvě indexování
+ Portál Azure pro centrální správy
+ Azure škálování, spolehlivosti a dostupnosti špičkových
+ Jazykové a vlastní analýzy s analyzátory pro plnou fulltextového vyhledávání v 56 jazycích
+ [Základní funkce společné pro hledání zaměřená na aplikace](#feature-drilldown): vyhodnocování, používání faset, návrhy, synonyma, geograficky vyhledávání a další.

> [!Note]
> Zdroje dat není Azure jsou plně podporované, ale závisí na metodika nabízené vyšší nároky na kód, nikoli indexery. Pomocí rozhraní API, můžete předat všechny kolekce dokumentů JSON do indexu Azure Search.

Mezi naše zákazníky vezměte v úvahu využívat širokou škálu funkcí ve službě Azure Search zejména online katalogů, -obchodní programy a dokumentu zjišťování aplikací.

## <a name="rest-api--net-sdk"></a>Rozhraní REST API | .net SDK

Při celou řadu úloh můžete provést na portálu, Azure Search je určena pro vývojáře, kteří chtějí integrovat do stávajících aplikací funkce vyhledávání. Nejsou k dispozici následující programovací rozhraní.

|Platforma |Popis |
|-----|------------|
|[REST](/rest/api/searchservice/) | Příkazy HTTP nepodporuje žádné programovací platformy a jazyk, včetně Xamarin, Java a JavaScript|
|[.NET SDK](search-howto-dotnet-sdk.md) | Obálka .NET pro REST API nabízí efektivní kódování v jazyce C# a dalších jazyků spravovaného kódu cílení na rozhraní .NET Framework |

## <a name="free-trial"></a>Bezplatná zkušební verze
Předplatitelé služby Azure můžete [zřídit službu v úrovni Free](search-create-service-portal.md).

Pokud nejste odběratele, můžete [zdarma otevřít účet Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F). Získáte kredity pro vyzkoušení placených služeb Azure. Až se vyčerpáte, můžete účet ponechat a používat [bezplatné služby Azure](https://azure.microsoft.com/free/). Platební karty se nikdy účtovat Pokud explicitně změnit nastavení a požádejte o nestrhne.

Alternativně můžete [aktivovat výhody pro předplatitele MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F): vaše předplatné MSDN vám dává kredity každý měsíc, které můžete použít pro placené služby Azure. 

## <a name="how-to-get-started"></a>Jak začít

1. Vytvoření služby v [úroveň Free](search-create-service-portal.md).

2. Krok prostřednictvím jednoho nebo více z následujících kurzů. 

  + [Jak používat sadu .NET SDK](search-howto-dotnet-sdk.md) ukazuje hlavní kroky ve spravovaném kódu.  
  + [Začínáme s rozhraním API REST](https://github.com/Azure-Samples/search-rest-api-getting-started) ukazuje stejný postup pomocí rozhraní REST API.  
  + [Vytvoření první indexu v portálu](search-get-started-portal.md) pomocí integrované funkce indexování a prototypu.   

Vyhledávací weby jsou běžné ovladače z načítání informací v mobilních aplikacích, na webu a v úložištích firemní data. Služba Azure Search nabízí nástroje pro vytváření prostředí vyhledávání podobná nastavením na velké komerčních webů.

V tomto videu 9 minutu z manažer programu Cavanagh počítači zjistěte, jak integrace vyhledávací může hodit vaší aplikace. Krátký ukázky se týkají klíčových funkcí v Azure Search a jak vypadá obvyklý pracovní postup. 

>[!VIDEO https://channel9.msdn.com/Events/Connect/2016/138/player]
 
+ 0 až 3 minut obsahuje klíčové funkce a případy použití.
+ 3 4 minut popisuje zřizování služby. 
+ 4 až 6 minut popisuje Import dat průvodce použít k vytvoření indexu pomocí nemovitosti předdefinované datové sady.
+ 6 9 minut popisuje Průzkumník služby Search a různé dotazy.


