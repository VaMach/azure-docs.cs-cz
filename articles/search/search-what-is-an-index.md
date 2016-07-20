<properties
    pageTitle="Vytvoření indexu Azure Search | Microsoft Azure | Hostovaná cloudová vyhledávací služba"
    description="Co je index ve službě Azure Search a jak se používá?"
    services="search"
    documentationCenter=""
authors="ashmaka"
/>

<tags
    ms.service="search"
    ms.devlang="na"
    ms.workload="search"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.date="05/31/2016"
    ms.author="ashmaka"/>

# Vytvoření indexu Azure Search
> [AZURE.SELECTOR]
- [Přehled](search-what-is-an-index.md)
- [Portál](search-create-index-portal.md)
- [.NET](search-create-index-dotnet.md)
- [REST](search-create-index-rest-api.md)

## Co je index?

 *Index* je trvalé úložiště *dokumentů* a jiných objektů, které používá služba Azure Search. Dokument je jednou jednotkou prohledávatelných dat v indexu. Například internetový prodejce může mít dokument pro každou prodanou položku, zatímco tisková agentura může mít dokument pro každý článek atd. Mapování těchto položek do známějších databázových ekvivalentů: *indexů* se koncepčně podobá *tabulce*, kdy *dokumenty* přibližně odpovídají *řádkům* tabulky.

Když přidáváte nebo nahráváte dokumenty a odesíláte vyhledávací dotazy do služby Azure Search, svoje požadavky odesíláte do konkrétního indexu ve vyhledávací službě.

## Typy a atributy polí v indexu Azure Search

Při definování schématu musíte zadat název, typ a atributy každého pole v indexu. Typ pole klasifikuje data, která jsou v tomto poli uložená. Atributy se nastavují u jednotlivých polí a určují, jak se příslušné pole použije. Následující tabulky poskytují výčet typů a atributů, které můžete zadat.


### Typy polí
|Typ|Popis|
|------------|-----------|
|*Edm.String*|Text, který jde volitelně tokenizovat k fulltextovému hledání (dělení slov, rozklad atd).|
|*Collection(Edm.String)*|Seznam řetězců, které jde volitelně tokenizovat k fulltextovému hledání. Ačkoli neexistuje žádné teoretické omezení počtu položek v kolekci, na kolekce se vztahuje 16MB omezení velikosti datové části.|
|*Edm.Boolean*|Obsahuje hodnoty true nebo false.|
|*Edm.Int32*|32bitové celočíselné hodnoty.|
|*Edm.Int64*|64bitové celočíselné hodnoty.|
|*Edm.Double*|Číselné údaje s dvojitou přesností.|
|*Edm.DateTimeOffset*|Hodnoty data a času ve formátu OData V4 (například `yyyy-MM-ddTHH:mm:ss.fffZ` nebo `yyyy-MM-ddTHH:mm:ss.fff[+/-]HH:mm`).|
|*Edm.GeographyPoint*|Bod představující geografické umístění na zeměkouli.|

Podrobnější informace o datových typech podporovaných službou Azure Search najdete v tématu [Podporované datové typy na webu MSDN](https://msdn.microsoft.com/library/azure/dn798938.aspx).



### Atributy polí
|Atribut|Popis|
|------------|-----------|
|*Klíč*|Řetězec obsahující jedinečné ID jednotlivých dokumentů, které slouží k vyhledávání dokumentů. Každý index musí mít jeden klíč. Jenom jedno pole může být klíč a jeho typ musí být nastavený na Edm.String.|
|*Retrievable*|Určuje, jestli může být pole vrácené ve výsledku hledání.|
|*Filterable*|Umožňuje použít pole ve filtrovacích dotazech.|
|*Sortable*|Umožňuje dotazu seřadit výsledky hledání podle tohoto pole.|
|*Facetable*|Umožňuje použití pole ve struktuře [fasetové navigace](search-faceted-navigation.md) k filtrování, které je řízené samotným uživatelem. Jako fasety obvykle nejlépe fungují pole, která obsahují opakované hodnoty použitelné k seskupení více dokumentů (například více dokumentů, které spadají pod jednu značku nebo kategorii služeb).|
|*Searchable*|Označí pole jako fulltextově prohledávatelné.|

Podrobnější informace o atributech indexu služby Azure Search najdete v tématu [Atributy indexu na webu MSDN](https://msdn.microsoft.com/library/azure/dn798941.aspx).



## Pokyny pro definování schématu indexu

Při navrhování indexu si ve fázi plánování pečlivě promyslete každé rozhodnutí. Při navrhování indexu je důležité zohlednit uživatelskou práci při vyhledávání a potřeby podniku, protože každému poli se musí přiřadit [správné atributy](https://msdn.microsoft.com/library/azure/dn798941.aspx). Při změně indexu po nasazení je nutné znovu sestavit a načíst data.


Pokud se v průběhu času změní požadavky na úložiště dat, můžete zvýšit nebo snížit kapacitu přidáním nebo odebráním oddílů. Podrobnosti najdete v tématu [Správa služby Search v Azure](search-manage.md) nebo [Omezení služby](search-limits-quotas-capacity.md).



<!--HONumber=Jun16_HO2-->


