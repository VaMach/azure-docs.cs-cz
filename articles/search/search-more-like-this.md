---
title: "moreLikeThis ve službě Azure Search (preview) | Microsoft Docs"
description: "Předběžná dokumentace pro funkci moreLikeThis (preview), který je v rozhraní API REST služby Azure Search."
services: search
documentationCenter: na
authors: mhko
manager: jlembicz
editor: na
ms.service: search
ms.devlang: rest-api
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 10/27/2016
ms.author: nateko
ms.openlocfilehash: 20f6a1166d47799f15275cb5b6a9ea4934ae57e2
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/03/2017
---
# <a name="morelikethis-in-azure-search-preview"></a>moreLikeThis ve službě Azure Search (preview)

`moreLikeThis=[key]`je parametr dotazu v [rozhraní API služby Search](https://docs.microsoft.com/rest/api/searchservice/search-documents). Zadáním `moreLikeThis` parametr ve vyhledávací dotaz, můžete najít dokumenty, které jsou podobné dokumentu určeného klíč dokumentu. Při hledání požadavku s `moreLikeThis`, dotazu je generována hledané výrazy, které popisují dokument nejlépe extrahovat z daného dokumentu. Vygenerovaný dotaz pak slouží k podání žádosti o vyhledávání. Ve výchozím nastavení, obsah všech `searchable` pole jsou považovány za, pokud `searchFields` parametr se používá k omezení pole. `moreLikeThis` Parametr nelze použít s parametrem vyhledávání `search=[string]`.

## <a name="examples"></a>Příklady 

Dole je příklad dotazu moreLikeThis. Tento dotaz najde dokumentů, jejichž pole Popis jsou nejvíce podobná pole zdrojový dokument podle specifikace `moreLikeThis` parametr.

```  
Get /indexes/hotels/docs?moreLikeThis=1002&searchFields=description&api-version=2016-09-01-Preview
```  

```  
POST /indexes/hotels/docs/search?api-version=2016-09-01-Preview
    {  
      "moreLikeThis": "1002",  
      "searchFields": "description"  
    }  
```  

## <a name="feature-availability"></a>Dostupnost funkcí

Funkci moreLikeThis je aktuálně ve verzi preview a podporuje jenom v api verze preview, `2015-02-28-Preview` a `2016-09-01-Preview`. Verze rozhraní API je určen na žádost, je možné kombinovat všeobecně dostupná (GA) a zobrazit jejich náhled rozhraní API ve stejné aplikaci. Však může změnit preview, které nejsou v rámci smlouvy o úrovni služeb a funkcí rozhraní API, tak nedoporučujeme jejich používání v produkční aplikace.