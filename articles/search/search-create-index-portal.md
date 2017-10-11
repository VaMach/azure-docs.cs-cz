---
title: "Vytvoření indexu (portál – Azure Search) | Microsoft Docs"
description: "Vytvoření indexu pomocí portálu Azure."
services: search
manager: jhubbard
author: heidisteen
documentationcenter: 
ms.assetid: 
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 06/20/2017
ms.author: heidist
ms.openlocfilehash: a7d98ab0937a7d3f932d5df34c19ae091129804e
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="create-an-azure-search-index-using-the-azure-portal"></a>Vytvoření indexu Azure Search pomocí portálu Azure
> [!div class="op_single_selector"]
> * [Přehled](search-what-is-an-index.md)
> * [Azure Portal](search-create-index-portal.md)
> * [.NET](search-create-index-dotnet.md)
> * [REST](search-create-index-rest-api.md)
> 
> 

Použít předdefinované index návrháře portálu Azure k prototypu nebo vytvořte [indexu vyhledávání](search-what-is-an-index.md) ke spuštění na služby Azure Search. 

## <a name="prerequisites"></a>Požadavky

Tento článek předpokládá [předplatného Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) a [služby Azure Search](search-create-service-portal.md).  

## <a name="find-your-search-service"></a>Vyhledejte svoji službu vyhledávání
1. Přihlaste se na stránku portálu Azure a zkontrolovat [vyhledávací služby pro vaše předplatné](https://portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices)
2. Vyberte službu Azure Search.

## <a name="name-the-index"></a>Název indexu

1. Klikněte **přidat index** tlačítka na panelu příkazů v horní části stránky.
2. Název indexu Azure Search. 
   * Začínat písmenem.
   * Použít jenom malá písmena, číslice nebo spojovníky ("-").
   * Omezení názvu až 60 znaků.

  Název indexu stane součástí adresu URL koncového bodu pro připojení k indexu a odesílání požadavků HTTP v REST API služby Azure Search.

## <a name="define-the-fields-of-your-index"></a>Definování polí indexu

Zahrnuje sestavení indexu *polí kolekce* , který definuje prohledávatelná data v indexu. Přesněji řečeno určuje strukturu dokumentů, které můžete nahrávat na server samostatně. Kolekce polí zahrnuje povinná a nepovinná pole s názvem a zadán s atributy indexu určující, jak můžete použít pole.

1. V **přidat Index** okně klikněte na tlačítko **pole >** otevřete vysouvací okno Definice pole. 

2. Přijměte vygenerovaného *klíč* pole typu Edm.String. Ve výchozím nastavení, je pole s názvem *id* ho mohli přejmenovat, tak dlouho, dokud splňuje řetězec, ale [pravidla po pojmenování](https://docs.microsoft.com/rest/api/searchservice/Naming-rules). Klíčové pole je povinné pro každý index Azure Search a musí být řetězec.

3. Přidáte pole, definujte plně dokumenty, které odešlete. Pokud obsahovat dokumenty *id*, *hotelů název*, *adresu*, *města*, a *oblast*, vytvořte odpovídající pole pro každé z nich v indexu. Zkontrolujte [návrh pokyny v následující části](#design) pomoc při nastavování atributů.

4. Volitelně lze přidáte interně všechna pole, které se používají ve výrazech filtru. Atributy na poli může být nastaven na pole vyloučit z operace hledání.

5. Po dokončení klikněte na tlačítko **OK** uložte a vytvořit index.

## <a name="tips-for-adding-fields"></a>Tipy pro přidávání polí

Vytvoření indexu na portálu je klávesnice náročné. Pomocí následujících tento pracovní postup minimalizujte kroky:

1. Nejprve vytvoříte seznam polí zadáním názvů a nastavení datových typů.

2. Potom použijte zaškrtávací políčka v horní části každé atribut hromadné povolit nastavení pro všechna pole a selektivně vymazat u několik polí, které ji nevyžadují. Například pole řetězce jsou obvykle prohledávatelné. Takto může klikněte na **Retrievable** a **Searchable** do obou návratové hodnoty pole ve výsledcích hledání, jakož i povolit fulltextové vyhledávání na pole. 

<a name="design"></a>
## <a name="design-guidance-for-setting-attributes"></a>Pokyny k nastavení atributů návrhu

I když můžete kdykoli přidat nová pole, jsou zamčené existující definice pole po dobu jeho existence index. Z tohoto důvodu vývojáři obvykle používají pro vytvoření jednoduché indexy, testování nápady nebo pomocí portálu stránek k vyhledání nastavení portálu. Časté iteraci přes návrh rejstříku je efektivnější, pokud budete postupovat podle použití metody založené na kódu, takže můžete znovu sestavit index snadno.

Analyzátory a trochu jsou přidruženy k pole před uložením index. Ujistěte se, že klikněte na tlačítko prostřednictvím každé záložkách stránce Přidat analyzátory jazyka nebo trochu k definici indexu.

Řetězec pole jsou často označena jako **Searchable** a **Retrievable**.

Pole používá, chcete-li zúžit výsledky hledání obsahují **Sortable**, **Filterable**, a **Facetable**.

Atributy pole určit, jak se používá pole, jako je například, zda je použit v fulltextové vyhledávání, Fasetové navigace, operace řazení a tak dále. Následující tabulka popisuje každý atribut.

|Atribut|Popis|  
|---------------|-----------------|  
|**s možností vyhledávání**|Full-text prohledávatelné vztahují lexikální analýzy, jako je dělení slov během indexování. Pokud nastavíte na hodnotu jako "slunečného dne" prohledávatelné pole, interně ho bude možné rozdělit na jednotlivé tokeny "slunečného" a "dne". Podrobnosti najdete v tématu [jak úplné textové vyhledávání funguje](search-lucene-query-architecture.md).|  
|**filtrování**|Odkazovaná v **$filter** dotazy. Filtrování pole typu `Edm.String` nebo `Collection(Edm.String)` nejsou prováděny dělení slov, tak porovnání jsou pro jenom přesné shody. Pokud nastavíte toto pole f "slunečného den", například `$filter=f eq 'sunny'` se najít žádné shody, ale `$filter=f eq 'sunny day'` bude. |  
|**řazení**|Ve výchozím nastavení systém řadí výsledky podle skóre, ale můžete nakonfigurovat na základě polí v dokumentech řazení. Pole typu `Collection(Edm.String)` nemůže být **řazení**. |  
|**facetable (kategorizovatelné)**|Obvykle se používá v prezentaci výsledky hledání, který obsahuje počet přístupů podle kategorie (například hotels v konkrétním městě). Tuto možnost nelze použít s pole typu `Edm.GeographyPoint`. Pole typu `Edm.String` , které jsou **filtrovatelných**, **řazení**, nebo **facetable** může být maximálně 32 kilobajtů délku. Podrobnosti najdete v tématu [vytvoření indexu (rozhraní API REST)](https://docs.microsoft.com/rest/api/searchservice/create-index).|  
|**klíč**|Jedinečný identifikátor pro dokumenty v indexu. Právě jedno pole musí být zvolena jako klíčové pole a musí být typu `Edm.String`.|  
|**získat**|Určuje, zda může být pole vrácené ve výsledku hledání. To je užitečné, pokud chcete použít pole (například *zisku*) jako filtr, řazení a vyhodnocování mechanismus, ale nepodporují má pole, které chcete být viditelné pro koncového uživatele. Tento atribut musí být `true` pro `key` pole.|  

## <a name="create-the-hotels-index-used-in-example-api-sections"></a>Vytvoření indexu hotels použít v částech příklad rozhraní API

Dokumentace k Azure Search rozhraní API obsahuje příklady kódu jednoduchý vybaveným *hotels* index. Na následujících snímcích obrazovky, uvidíte definici indexu, včetně analyzátor francouzštinu zadaný během definici indexu, které můžete znovu vytvořit jako cvičení postupů na portálu.

![](./media/search-create-index-portal/field-definitions.png)

![](./media/search-create-index-portal/set-analyzer.png)

## <a name="next-steps"></a>Další kroky

Po vytvoření indexu Azure Search, můžete přesunout na další krok: [nahrát prohledávatelná data do indexu](search-what-is-data-import.md).

Alternativně může trvat i hlubší pohled na indexy. Kromě kolekce polí indexu také určuje analyzátorů, trochu, vyhodnocování profily a nastavení CORS. Na portálu pro definování běžných prvků poskytuje stránky s kartami: polí, analyzátorů a trochu. K vytvoření nebo úpravě další prvky, můžete použít rozhraní API REST nebo .NET SDK.

## <a name="see-also"></a>Viz také

 [Jak funguje fulltextové vyhledávání](search-lucene-query-architecture.md)  
 [Rozhraní API REST služby vyhledávání](https://docs.microsoft.com/rest/api/searchservice/) [.NET SDK](https://docs.microsoft.com/dotnet/api/overview/azure/search?view=azure-dotnet)

