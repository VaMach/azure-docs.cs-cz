---
title: "Verze rozhraní API 2016 modulu Azure Search služby REST-09-01-Preview | Microsoft Docs"
description: "Azure Search služby REST verze rozhraní API 2016-09-01-Preview zahrnuje povolenými experimentálními funkcemi, jako je například synonyma a moreLikeThis hledání."
services: search
documentationcenter: na
author: mhko
manager: jlembicz
editor: 
ms.assetid: 3dba3bf8-9c83-42f6-82bc-04727bd11037
ms.service: search
ms.devlang: rest-api
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: search
ms.date: 10/25/2017
ms.author: nateko
ms.openlocfilehash: 082c207f892fcc277d30d66c6165dd9920d3ab27
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/03/2017
---
# <a name="azure-search-service-rest-api-version-2016-09-01-preview"></a>Rozhraní API REST služby vyhledávání systému Azure: Verze 2016-09-01-Preview
Tento článek je referenční dokumentaci k nástroji pro `api-version=2016-09-01-Preview`. Tato verze preview rozšiřuje všeobecně dostupná verzí [rozhraní api-version = 2016-09-01](https://msdn.microsoft.com/library/dn798935.aspx), tím, že poskytuje následující povolenými experimentálními funkcemi:

* [Synonyma rozhraní API](search-synonyms.md) nahrání synonymum mapy a rozbalte možnost hledání.
* [`moreLikeThis`parametr dotazu](search-more-like-this.md) najít dokumenty, které se vztahují k určitému dokumentu.

Přesvědčte se, zda Pokud chcete cílit na verzi preview rozhraní API `api-version=2016-09-01-Preview` můžete vyzkoušet tyto povolenými experimentálními funkcemi. Následující příklad ukazuje, jak ve verzi preview verze rozhraní api je uveden v provedení dalších jako tento dotaz.

    GET https://[service name].search.windows.net/indexes/[index name]/docs?moreLikeThis=a1&api-version=2016-09-01-Preview

> [!NOTE]
> Funkce Preview jsou k dispozici pro testování a experimenty s cílem shromažďování zpětné vazby a mohou podléhat změnám. **Důrazně nedoporučujeme používání preview rozhraní API v produkční aplikace.**

Služba Azure Search je k dispozici v několika verzích. Naleznete [verze služby vyhledávání](http://msdn.microsoft.com/library/azure/dn864560.aspx) podrobnosti.
