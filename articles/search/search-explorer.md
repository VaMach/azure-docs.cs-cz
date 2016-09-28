<properties
    pageTitle="Dotazování indexu Azure Search pomocí webu Azure Portal | Microsoft Azure | Hostovaná cloudová vyhledávací služba"
    description="Vydejte vyhledávací dotaz v Průzkumníku služby Search na webu Azure Portal."
    services="search"
    documentationCenter=""
    authors="ashmaka"
/>

<tags
    ms.service="search"
    ms.devlang="NA"
    ms.workload="search"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.date="08/29/2016"
    ms.author="ashmaka"
/>

# Dotazování indexu Azure Search pomocí webu Azure Portal
> [AZURE.SELECTOR]
- [Přehled](search-query-overview.md)
- [Portál](search-explorer.md)
- [.NET](search-query-dotnet.md)
- [REST](search-query-rest-api.md)

Tento návod vám ukáže, jak dotazovat index Azure Search na webu Azure Portal.

Je nutné, abyste před zahájením tohoto postupu [vytvořili index Azure Search](search-what-is-an-index.md) a [naplnili ho daty](search-what-is-data-import.md).

## I. Přejděte do okna Azure Search
1. V nabídce na levé straně [webu Azure Portal](https://portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices) klikněte na možnost Všechny prostředky.
2. Vyberte službu Azure Search.

## II. Vyberte index, který chcete prohledat
1. Na dlaždici Indexy vyberte index, který chcete prohledat.

![](./media/search-explorer/pick-index.png)

## III. Klikněte na dlaždici Průzkumník služby Search
![](./media/search-explorer/search-explorer-tile.png)

## III. Spusťte hledání
1. Pokud chcete prohledat index Azure Search, pište do pole *Řetězec dotazu* a stiskněte **Hledat**.
 * Jestliže používáte Průzkumník služby Search, můžete zadat kterýkoli z [parametrů dotazu](https://msdn.microsoft.com/library/dn798927.aspx).

2. V části *Výsledky* budou výsledky dotazu uvedené v nezpracovaném formátu JSON, jaký byste obdrželi v textu odpovědi HTTP při vydávání žádostí hledání do rozhraní REST API služby Azure Search.
3. Řetězec dotazu se automaticky parsuje do správné adresy URL žádosti za účelem odeslání žádosti HTTP do rozhraní REST API služby Azure Search.

![](./media/search-explorer/search-bar.png)



<!--HONumber=Sep16_HO3-->


