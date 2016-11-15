---
title: "Dotazování indexu Azure Search | Dokumentace Microsoftu"
description: "Sestavení vyhledávacího dotazu ve službě Azure Search a použití parametrů hledání k filtrování a řazení výsledků vyhledávání."
services: search
manager: jhubbard
documentationcenter: 
author: ashmaka
ms.assetid: 69205d7a-363f-4b92-a53f-6ca818a3d2c7
ms.service: search
ms.devlang: na
ms.workload: search
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.date: 08/29/2016
ms.author: ashmaka
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 910ccb17119a3668ba99d7d056502d51e6266bd4


---
# <a name="query-your-azure-search-index"></a>Dotazování indexu Azure Search
> [!div class="op_single_selector"]
> * [Přehled](search-query-overview.md)
> * [Azure Portal](search-explorer.md)
> * [.NET](search-query-dotnet.md)
> * [REST](search-query-rest-api.md)
> 
> 

Při odesílání vyhledávacích dotazů do služby Azure Search existuje několik parametrů, které lze zadat vedle vlastních slov zadaných do vyhledávacího pole vaší aplikace. Tyto parametry dotazu vám umožňují získat větší kontrolu nad fulltextovým vyhledáváním.

Níže je seznam se stručným vysvětlením běžných použití parametrů dotazu ve službě Azure Search. Úplné vysvětlení parametrů dotazu a jejich chování naleznete na podrobných stránkách pro [REST API](https://msdn.microsoft.com/library/azure/dn798927.aspx) a [.NET SDK](https://msdn.microsoft.com/library/azure/microsoft.azure.search.models.searchparameters_properties.aspx).

## <a name="types-of-queries"></a>Typy dotazů
Služba Azure Search nabízí mnoho možností pro vytvoření velmi výkonných dotazů. Dva hlavní typy dotazů, které budete používat, jsou `search` a `filter`. Dotaz `search` vyhledává jeden nebo více výrazů ve všech *prohledávatelných* polích v indexu a funguje podobně, jak jste zvyklí u vyhledávacích webů Google nebo Bing. Dotaz `filter` vyhodnocuje logický výraz na všech *filtrovatelných* polích v indexu. Na rozdíl od dotazů `search` dotazy `filter` porovnávají přesný obsah pole, to znamená, že u polí s řetězci rozlišují malá a velká písmena.

Vyhledávání a filtrování lze používat společně nebo samostatně. Budete-li je používat společně, nejdříve se na celý index použije filtr a na výsledcích filtru se pak provede vyhledávání. Používání filtrů tak může být užitečné pro zlepšení výkonu dotazů zmenšením sady dokumentů, které musí dotaz vyhledávání zpracovat.

Syntaxe pro výrazy filtru je podmnožinou [jazyka filtrování OData](https://msdn.microsoft.com/library/azure/dn798921.aspx). Pro vyhledávací dotazy můžete použít [zjednodušenou syntaxi](https://msdn.microsoft.com/library/azure/dn798920.aspx) nebo [syntaxi dotazů Lucene](https://msdn.microsoft.com/library/azure/mt589323.aspx), o kterých pojednáváme níže.

### <a name="simple-query-syntax"></a>Jednoduchá syntaxe dotazů
[Jednoduchá syntaxe dotazů](https://msdn.microsoft.com/library/azure/dn798920.aspx) je výchozím jazykem dotazů ve službě Azure Search. Jednoduchá syntaxe dotazů podporuje mnoho běžných operátorů hledání, včetně operátorů AND, OR, NOT a operátorů fráze, přípony a priority.

### <a name="lucene-query-syntax"></a>Syntaxe dotazů Lucene
[Syntaxe dotazů Lucene](https://msdn.microsoft.com/library/azure/mt589323.aspx) vám umožňuje používat velmi rozšířený a expresivní jazyk dotazů vyvinutý jako součást [Apache Lucene](https://lucene.apache.org/core/4_10_2/queryparser/org/apache/lucene/queryparser/classic/package-summary.html).

Pomocí této syntaxe dotazů můžete snadno dosáhnout následujících schopností: [dotazy v rámci pole](https://msdn.microsoft.com/library/azure/mt589323.aspx#bkmk_fields), [přibližné vyhledávání](https://msdn.microsoft.com/library/azure/mt589323.aspx#bkmk_fuzzy), [vyhledávání blízkých výrazů](https://msdn.microsoft.com/library/azure/mt589323.aspx#bkmk_proximity), [zvýšení skóre termínu](https://msdn.microsoft.com/library/azure/mt589323.aspx#bkmk_termboost), [vyhledávání pomocí regulárních výrazů](https://msdn.microsoft.com/library/azure/mt589323.aspx#bkmk_regex), [vyhledávání pomocí zástupných znaků](https://msdn.microsoft.com/library/azure/mt589323.aspx#bkmk_wildcard), [základy syntaxe](https://msdn.microsoft.com/library/azure/mt589323.aspx#bkmk_syntax) a [dotazy s logickými operátory](https://msdn.microsoft.com/library/azure/mt589323.aspx#bkmk_boolean).

## <a name="ordering-results"></a>Řazení výsledků
Když přijímáte výsledky vyhledávacího dotazu, můžete požadovat, aby služba Azure Search vracela výsledky seřazené podle hodnot v určitém poli. Ve výchozím nastavení služba Azure Search řadí výsledky podle skóre vyhledávání každého dokumentu, které je odvozeno z [TF-IDF](https://en.wikipedia.org/wiki/Tf%E2%80%93idf).

Chcete-li, aby služba Azure Search vracela výsledky seřazené podle hodnoty místo skóre vyhledávání, můžete použít parametr vyhledávání `orderby`. Můžete zadat hodnotu parametru `orderby`, chcete-li zahrnout názvy polí a volání [funkce `geo.distance()`](https://msdn.microsoft.com/library/azure/dn798921.aspx) pro geoprostorové hodnoty. U každého výrazu může následovat `asc` k určení, že výsledky mají být ve vzestupném pořadí, nebo `desc` k určení, že výsledky mají být v sestupném pořadí. Ve výchozím nastavení se používá vzestupné pořadí.

## <a name="paging"></a>Stránkování
Služba Azure Search umožňuje snadnou implementaci stránkování výsledků vyhledávání. Pomocí parametrů `top` a `skip` můžete plynule vydávat vyhledávací požadavky, které vám umožní získat úplnou sadu výsledků vyhledávání ve spravovatelných, seřazených podmnožinách, které umožňují dobré postupy v uživatelském rozhraní vyhledávání. Při získávání těchto menších podmnožin výsledků můžete také získat počet dokumentů v úplné sadě výsledků vyhledávání.

Další informace o stránkování výsledků vyhledávání naleznete v článku [Stránkování výsledků vyhledávání ve službě Azure Search](search-pagination-page-layout.md).

## <a name="hit-highlighting"></a>Zvýrazňování položek
Ve službě Azure Search je zvýrazňování přesné části výsledků vyhledávání, která odpovídá vyhledávacímu dotazu, umožněno pomocí parametrů `highlight`, `highlightPreTag` a `highlightPostTag`. Můžete určit, u jakých *prohledávatelných* polí má být odpovídající text zvýrazněný, stejně tak můžete zadat značky řetězce, které se mají připojit k začátku a ke konci odpovídajícího text vráceného službou Azure Search.




<!--HONumber=Nov16_HO2-->


