---
title: "Postup stránkování výsledků vyhledávání ve službě Azure Search | Microsoft Docs"
description: "Stránkování ve službě Azure Search, hostované cloudové vyhledávací službě v Microsoft Azure."
services: search
documentationcenter: 
author: HeidiSteen
manager: jhubbard
editor: 
ms.assetid: a0a1d315-8624-4cdf-b38e-ba12569c6fcc
ms.service: search
ms.devlang: rest-api
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 08/29/2016
ms.author: heidist
ms.openlocfilehash: 1054e15a2751c53aad5dbc8054c4cec41102dee9
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-page-search-results-in-azure-search"></a>Jak v Azure Search stránkovat výsledky hledání
Tento článek obsahuje pokyny týkající se používání rozhraní API REST služby vyhledávání Azure k implementaci standardní elementy stránka s výsledky hledání, například celkový počet, načtení dokumentu, pořadí řazení a navigace.

V každém případě dál uvedených souvisejících se stránkami možnosti, které přispívají data nebo informace na stránce s výsledky hledání jsou zadaná pomocí [vyhledávání dokumentů](http://msdn.microsoft.com/library/azure/dn798927.aspx) požadavky odeslané do služby Azure Search. Žádosti o zahrnovat příkaz GET, cestu a parametry dotazu, které informovat o tom, co je požadované služby a postup formulovali odpovědi.

> [!NOTE]
> Počet elementů, například adresa URL služby a cesta, příkaz HTTP, zahrnuje žádost platná `api-version`a tak dále. Jako stručný výtah jsme oříznut příklady ke zvýraznění syntaxe, které se týkají stránkování. Najdete v tématu [rozhraní REST API služby Azure Search](http://msdn.microsoft.com/library/azure/dn798935.aspx) dokumentace podrobnosti o žádosti o syntaxi.
> 
> 

## <a name="total-hits-and-page-counts"></a>Celkový počet přístupů a počty stránky
Zobrazuje celkový počet výsledků vrácených z dotazu a pak vrácení výsledků v menší bloky dat, je nezbytné, aby prakticky všechny stránky vyhledávání.

![][1]

Ve službě Azure Search, můžete použít `$count`, `$top`, a `$skip` parametry, které vrátí tyto hodnoty. Následující příklad ukazuje ukázková žádost pro celkový počet přístupů, vrátí jako `@OData.count`:

        GET /indexes/onlineCatalog/docs?$count=true

Dokumenty ve skupinách 15 načíst a také se zobrazují celkový počet přístupů do začínající na první stránce:

        GET /indexes/onlineCatalog/docs?search=*$top=15&$skip=0&$count=true

Přestránkování výsledky vyžaduje obě `$top` a `$skip`, kde `$top` Určuje, kolik položek k vrácení v dávce, a `$skip` Určuje, kolik položek Pokud chcete vynechat. V následujícím příkladu každý stránka zobrazuje vedle 15 položky indikován přírůstkové přeskočí v `$skip` parametr.

        GET /indexes/onlineCatalog/docs?search=*$top=15&$skip=0&$count=true

        GET /indexes/onlineCatalog/docs?search=*$top=15&$skip=15&$count=true

        GET /indexes/onlineCatalog/docs?search=*$top=15&$skip=30&$count=true

## <a name="layout"></a>Rozložení
Na stránce výsledky hledání můžete zobrazit miniatury, podmnožinu pole a odkaz na stránku plnou verzi produktu.

 ![][2]

Ve službě Azure Search, byste použili `$select` a příkaz vyhledávání implementovat toto prostředí.

Chcete-li vrátit podmnožinu pole vedle sebe rozložení:

        GET /indexes/ onlineCatalog/docs?search=*&$select=productName,imageFile,description,price,rating 

Bitové kopie a mediálních souborů nejsou přímo s možností vyhledávání a by měly být uložené v jiné platformy úložiště, jako je například úložiště objektů Blob v Azure, jak snížit náklady. V indexu a dokumenty zadejte pole, které obsahuje adresu URL externí obsah. Pak můžete toto pole jako odkaz na obrázek. Adresu URL do bitové kopie musí být v dokumentu.

Načtení stránky produktu popis pro **onClick** událostí, použijte [vyhledávání dokumentů](http://msdn.microsoft.com/library/azure/dn798929.aspx) předávání v klíči dokumentu pro načtení. Datový typ klíče je `Edm.String`. V tomto příkladu je *246810*. 

        GET /indexes/onlineCatalog/docs/246810

## <a name="sort-by-relevance-rating-or-price"></a>Řazení podle relevance, hodnocení a ceny
Pořadí řazení často výchozí nastavení relevance, ale je běžné, aby alternativní řazení objednávky snadno dostupné, aby zákazníci můžete rychle změnit pořadí existující výsledky do různých rank pořadí.

 ![][3]

Ve službě Azure Search, řazení podle `$orderby` výrazu pro všechna pole, které jsou uloženy jako`"Sortable": true.`

Relevance důrazně souvisí s profily vyhodnocování. Můžete vyhodnocování výchozí, což závisí na text analýzy a statistiky zařadit všechny výsledky, pořadí s vyšší skóre přejít do dokumentů s více nebo silnější odpovídá na hledaný termín.

Alternativní pořadí řazení obvykle souvisejí s **onClick** události, které zpětné volání pro metodu, která vytvoří pořadí řazení. Například uděleno tento element stránky:

 ![][4]

Vytvoříte metodu, která přijme jako vstup možnost vybrané řazení a vrátí seřazený seznam pro kritéria spojená s tuto možnost.

 ![][5]

> [!NOTE]
> Při vyhodnocování výchozí stačí pro mnoho scénářů, doporučujeme místo vytvoření relevance na základě vlastní profil vyhodnocování. Vlastní profil vyhodnocování poskytuje způsob, jak nárůst položky, které jsou více užitečné k vaší firmě. V tématu [přidat profil vyhodnocování](http://msdn.microsoft.com/library/azure/dn798928.aspx) Další informace. 
> 
> 

## <a name="faceted-navigation"></a>Fasetová navigace
Navigace vyhledávání je běžné na stránka s výsledky, často nacházející se v horní části stránky na straně. Ve službě Azure Search poskytuje Fasetové navigace řízené samotným hledání podle předdefinovaných filtrů. V tématu [Fasetové navigace ve službě Azure Search](search-faceted-navigation.md) podrobnosti.

## <a name="filters-at-the-page-level"></a>Filtry na úrovni stránky
Pokud návrh vašeho řešení zahrnuty vyhrazené vyhledávání stránky pro určité typy obsahu (například prodejní online aplikace, která má oddělení uvedených v horní části stránky), můžete vložit výraz filtru společně **onClick** událost a otevřete stránku v odkazující stavu. 

Můžete odeslat filtr s nebo bez výrazu vyhledávání. Například následující požadavek bude filtrovat název značky, vrácení pouze dokumenty, které odpovídají ho.

        GET /indexes/onlineCatalog/docs?$filter=brandname eq ‘Microsoft’ and category eq ‘Games’

V tématu [vyhledávání dokumentů (API služby Azure Search)](http://msdn.microsoft.com/library/azure/dn798927.aspx) Další informace o `$filter` výrazy.

## <a name="see-also"></a>Viz také
* [Rozhraní API REST služby vyhledávání systému Azure](http://msdn.microsoft.com/library/azure/dn798935.aspx)
* [Operace indexu](http://msdn.microsoft.com/library/azure/dn798918.aspx)
* [Operace dokumentu](http://msdn.microsoft.com/library/azure/dn800962.aspx)
* [Kurzy o službě Azure Search a video](search-video-demo-tutorial-list.md)
* [Fasetové navigace ve službě Azure Search](search-faceted-navigation.md)

<!--Image references-->
[1]: ./media/search-pagination-page-layout/Pages-1-Viewing1ofNResults.PNG
[2]: ./media/search-pagination-page-layout/Pages-2-Tiled.PNG
[3]: ./media/search-pagination-page-layout/Pages-3-SortBy.png
[4]: ./media/search-pagination-page-layout/Pages-4-SortbyRelevance.png
[5]: ./media/search-pagination-page-layout/Pages-5-BuildSort.png 
