---
title: "Vytvoření indexu Azure Search pomocí sady .NET SDK | Dokumentace Microsoftu"
description: "Vytvořte index v kódu pomocí sady Azure Search .NET SDK."
services: search
documentationcenter: 
author: brjohnstmsft
manager: jhubbard
editor: 
tags: azure-portal
ms.assetid: 3a851647-fc7b-4fb6-8506-6aaa519e77cd
ms.service: search
ms.devlang: dotnet
ms.workload: search
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.date: 08/29/2016
ms.author: brjohnst
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 87757a16f1fa31be97f6f8a0e39c6adbf2513828


---
# <a name="create-an-azure-search-index-using-the-net-sdk"></a>Vytvoření indexu Azure Search pomocí sady .NET SDK
> [!div class="op_single_selector"]
> * [Přehled](search-what-is-an-index.md)
> * [Azure Portal](search-create-index-portal.md)
> * [.NET](search-create-index-dotnet.md)
> * [REST](search-create-index-rest-api.md)
> 
> 

Tento článek vás provede procesem vytvoření [indexu](https://msdn.microsoft.com/library/azure/dn798941.aspx) Azure Search pomocí sady [Azure Search .NET SDK](https://msdn.microsoft.com/library/azure/dn951165.aspx).

Předtím, než podle těchto pokynů vytvoříte index, byste už měli mít [vytvořenou službu Azure Search](search-create-service-portal.md).

Všimněte si, že ukázkový kód v tomto článku je napsán v jazyce C#. Úplný zdrojový kód najdete [na GitHubu](http://aka.ms/search-dotnet-howto).

## <a name="i-identify-your-azure-search-services-admin-apikey"></a>I. Identifikace klíče api-key správce služby Azure Search
Teď, když máte zřízenou službu Azure Search, jste skoro připraveni vydávat žádosti na koncový bod služby pomocí sady .NET SDK. Nejprve budete muset získat jeden z klíčů správce (api-key) vytvořených pro vyhledávací službu, kterou jste zřídili. .NET SDK bude tento klíč api-key odesílat v každém požadavku na vaši službu. Platný klíč vytváří na základě žádosti vztah důvěryhodnosti mezi aplikací, která žádost odeslala, a službou, která ji zpracovává.

1. Pokud chcete najít klíče api-key svojí služby, musíte se přihlásit k [webu Azure Portal](https://portal.azure.com/).
2. Přejděte do okna služby Azure Search.
3. Klikněte na ikonu klíčů.

Vaše služba bude mít *klíče správce* a *klíče dotazů*.

* Primární a sekundární *klíče správce* udělují úplná práva ke všem operacím, včetně možnosti spravovat službu, vytvářet a odstraňovat indexy, indexery a zdroje dat. Existují dva klíče, takže pokud se rozhodnete znovu vygenerovat primární klíč, můžete dál používat sekundární klíč, a naopak.
* Vaše *klíče dotazů* udělují přístup jen pro čtení k indexům a dokumentům a obvykle se distribuují klientským aplikacím, které vydávají požadavky hledání.

Pro účely vytvoření indexu můžete použít primární nebo sekundární klíč správce.

<a name="CreateSearchServiceClient"></a>

## <a name="ii-create-an-instance-of-the-searchserviceclient-class"></a>II. Vytvoření instance třídy SearchServiceClient
Chcete-li začít používat sadu Azure Search .NET SDK, budete muset vytvořit instanci třídy `SearchServiceClient`. Tato třída obsahuje několik konstruktorů. Ten, který chcete, přijímá jako parametry název vaší vyhledávací služby a objekt `SearchCredentials`. `SearchCredentials` zabalí váš klíč api-key.

Následující kód vytvoří novou instanci `SearchServiceClient` pomocí hodnot pro název vyhledávací služby a klíč api-key, které jsou uložené v konfiguračním souboru aplikace (`app.config` nebo `web.config`):

```csharp
string searchServiceName = ConfigurationManager.AppSettings["SearchServiceName"];
string adminApiKey = ConfigurationManager.AppSettings["SearchServiceAdminApiKey"];

SearchServiceClient serviceClient = new SearchServiceClient(searchServiceName, new SearchCredentials(adminApiKey));
```

`Indexes` má vlastnost `SearchServiceClient`. Tato vlastnost poskytuje všechny metody, které potřebujete k vytváření, výpisu, aktualizaci nebo odstranění indexů Azure Search.

> [!NOTE]
> Třída `SearchServiceClient` spravuje připojení k vyhledávací službě. Aby se zabránilo otevírání příliš mnoha připojení, měli byste se pokusit sdílet jednu instanci třídy `SearchServiceClient` v rámci aplikace, pokud je to možné. Její metody jsou bezpečné pro přístup z více vláken a takové sdílení umožňují.
> 
> 

<a name="DefineIndex"></a>

## <a name="iii-define-your-azure-search-index-using-the-index-class"></a>III. Definování indexu Azure Search pomocí třídy `Index`
Jediné volání metody `Indexes.Create` vytvoří váš index. Tato metoda přebírá jako parametr objekt `Index`, který definuje index Azure Search. Je nutné vytvořit objekt `Index` a provést jeho inicializaci následujícím způsobem:

1. Nastavte vlastnost `Name` objektu `Index` na název indexu.
2. Nastavte vlastnost `Fields` objektu `Index` na pole objektů `Field`. Každý z objektů `Field` definuje chování pole v indexu. Můžete zadat název pole do konstruktoru, společně s datovým typem (nebo analyzátor pro pole řetězce). Můžete také nastavit další vlastnosti, například `IsSearchable`, `IsFilterable` atd.

Při navrhování indexu je důležité zohlednit uživatelskou práci při vyhledávání a potřeby podniku, protože každému objektu `Field` se musí přiřadit [příslušné vlastnosti](https://msdn.microsoft.com/library/azure/dn798941.aspx). Tyto vlastnosti určují, které funkce vyhledávání (filtrování, používání omezujících vlastností, řazení fulltextového vyhledávání atd.) se použijí u kterých polí. Pro vlastnost, kterou nenastavíte explicitně, se použije jako výchozí hodnota třídy `Field` zákaz odpovídající funkce hledání, pokud ji specificky nepovolíte.

V našem příkladu jsme nazvali index „hotels“ a pole jsme definovali takto:

```csharp
var definition = new Index()
{
    Name = "hotels",
    Fields = new[]
    {
        new Field("hotelId", DataType.String)                       { IsKey = true, IsFilterable = true },
        new Field("baseRate", DataType.Double)                      { IsFilterable = true, IsSortable = true, IsFacetable = true },
        new Field("description", DataType.String)                   { IsSearchable = true },
        new Field("description_fr", AnalyzerName.FrLucene),
        new Field("hotelName", DataType.String)                     { IsSearchable = true, IsFilterable = true, IsSortable = true },
        new Field("category", DataType.String)                      { IsSearchable = true, IsFilterable = true, IsSortable = true, IsFacetable = true },
        new Field("tags", DataType.Collection(DataType.String))     { IsSearchable = true, IsFilterable = true, IsFacetable = true },
        new Field("parkingIncluded", DataType.Boolean)              { IsFilterable = true, IsFacetable = true },
        new Field("smokingAllowed", DataType.Boolean)               { IsFilterable = true, IsFacetable = true },
        new Field("lastRenovationDate", DataType.DateTimeOffset)    { IsFilterable = true, IsSortable = true, IsFacetable = true },
        new Field("rating", DataType.Int32)                         { IsFilterable = true, IsSortable = true, IsFacetable = true },
        new Field("location", DataType.GeographyPoint)              { IsFilterable = true, IsSortable = true }
    }
};
```

Hodnoty vlastností jsme pro každý objekt `Field` pečlivě zvolili podle toho, jak se pravděpodobně použijí v aplikaci. Například je pravděpodobné, že lidé hledající hotely se budou zajímat o výskyty klíčového slova v poli `description`, takže jsme pro toto pole povolili fulltextové vyhledávání nastavením vlastnosti `IsSearchable` na hodnotu `true`.

Upozorňujeme, že právě jedno pole v indexu typu `DataType.String` musí být určené jako *klíčové* pole, a to nastavením vlastnosti `IsKey` na hodnotu `true` (viz `hotelId` v předchozím příkladu).

Výše uvedená definice indexu používá pro pole `description_fr` vlastní analyzátor jazyka, protože je určené k ukládání francouzského textu. Další informace o analyzátorech jazyka najdete v [tématu jazykové podpory na webu MSDN](https://msdn.microsoft.com/library/azure/dn879793.aspx) a příslušném [příspěvku na blogu](https://azure.microsoft.com/blog/language-support-in-azure-search/).

> [!NOTE]
> Všimněte si, že předáním `AnalyzerName.FrLucene` do konstruktoru bude objekt `Field` automaticky typu `DataType.String` a bude mít vlastnost `IsSearchable` nastavenou na `true`.
> 
> 

## <a name="iv-create-the-index"></a>IV. Vytvoření indexu
Nyní, když jste inicializovali objekt `Index`, můžete vytvořit index jednoduchým voláním metody `Indexes.Create` pro objekt `SearchServiceClient`:

```csharp
serviceClient.Indexes.Create(definition);
```

V případě úspěšného požadavku dojde k normálnímu vrácení z metody. Pokud dojde k problému s požadavkem, jako je například neplatný parametr, vyvolá metoda výjimku `CloudException`.

Pokud jste s indexem hotovi a chcete ho odstranit, stačí zavolat metodu `Indexes.Delete` pro objekt `SearchServiceClient`. Takto bychom například odstranili index „hotels“:

```csharp
serviceClient.Indexes.Delete("hotels");
```

> [!NOTE]
> Příklad kódu v tomto článku používá pro jednoduchost synchronní metody sady Azure Search .NET SDK. Doporučujeme ve vlastních aplikacích použít asynchronní metody, aby aplikace byly škálovatelné a dobře reagovaly. Například ve výše uvedených příkladech můžete použít `CreateAsync` a `DeleteAsync` namísto `Create` a `Delete`.
> 
> 

## <a name="next"></a>Další
Po vytvoření indexu Azure Search budete připravení [nahrát do indexu obsah](search-what-is-data-import.md), abyste mohli začít prohledávat data.




<!--HONumber=Nov16_HO2-->


