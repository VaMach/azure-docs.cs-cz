---
title: "Dotazování indexu Azure Search pomocí webu Azure Portal | Dokumentace Microsoftu"
description: "Vydejte vyhledávací dotaz v Průzkumníku služby Search na webu Azure Portal."
services: search
manager: jhubbard
documentationcenter: 
author: ashmaka
ms.assetid: 8e524188-73a7-44db-9e64-ae8bf66b05d3
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.date: 08/29/2016
ms.author: ashmaka
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: a23372112e17703a3399e1bdc9eaf73b85a1f80d


---
# <a name="query-your-azure-search-index-using-the-azure-portal"></a>Dotazování indexu Azure Search pomocí webu Azure Portal
> [!div class="op_single_selector"]
> * [Přehled](search-query-overview.md)
> * [Azure Portal](search-explorer.md)
> * [.NET](search-query-dotnet.md)
> * [REST](search-query-rest-api.md)
> 
> 

Tento návod vám ukáže, jak dotazovat index Azure Search na webu Azure Portal.

Je nutné, abyste před zahájením tohoto postupu [vytvořili index Azure Search](search-what-is-an-index.md) a [naplnili ho daty](search-what-is-data-import.md).

## <a name="i-go-to-your-azure-search-blade"></a>I. Přejděte do okna Azure Search
1. V nabídce na levé straně [webu Azure Portal](https://portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices) klikněte na možnost Všechny prostředky.
2. Vyberte službu Azure Search.

## <a name="ii-select-the-index-you-would-like-to-search"></a>II. Vyberte index, který chcete prohledat
1. Na dlaždici Indexy vyberte index, který chcete prohledat.

![](./media/search-explorer/pick-index.png)

## <a name="iii-click-on-the-search-explorer-tile"></a>III. Klikněte na dlaždici Průzkumník služby Search
![](./media/search-explorer/search-explorer-tile.png)

## <a name="iii-start-searching"></a>III. Spusťte hledání
1. Pokud chcete prohledat index Azure Search, pište do pole *Řetězec dotazu* a stiskněte **Hledat**.
   
   * Jestliže používáte Průzkumník služby Search, můžete zadat kterýkoli z [parametrů dotazu](https://msdn.microsoft.com/library/dn798927.aspx).
2. V části *Výsledky* budou výsledky dotazu uvedené v nezpracovaném formátu JSON, jaký byste obdrželi v textu odpovědi HTTP při vydávání žádostí hledání do rozhraní REST API služby Azure Search.
3. Řetězec dotazu se automaticky parsuje do správné adresy URL žádosti za účelem odeslání žádosti HTTP do rozhraní REST API služby Azure Search.

![](./media/search-explorer/search-bar.png)




<!--HONumber=Nov16_HO2-->


