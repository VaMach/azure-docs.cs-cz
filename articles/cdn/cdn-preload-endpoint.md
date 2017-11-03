---
title: "Předběžné načtení prostředků na koncový bod Azure CDN | Microsoft Docs"
description: "Zjistěte, jak předem načíst obsah uložený v mezipaměti na koncový bod Azure CDN."
services: cdn
documentationcenter: 
author: smcevoy
manager: erikre
editor: 
ms.assetid: 5ea3eba5-1335-413e-9af3-3918ce608a83
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: 1f2dcd9a91bb6e883cbef06373c1acd98bf8d45f
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="pre-load-assets-on-an-azure-cdn-endpoint"></a>Předběžné načtení prostředků v koncovém bodu Azure CDN
[!INCLUDE [cdn-verizon-only](../../includes/cdn-verizon-only.md)]

Ve výchozím nastavení prostředky nejprve mezipaměti, jako jsou požadovali. To znamená, že první požadavek každou oblast, může trvat déle, vzhledem k tomu, že nebudete mít servery edge obsah do mezipaměti a bude třeba předat požadavek na původní server. Předběžné načítání obsahu zabraňuje tato první přístupů latence.

Kromě zajištění lepší zkušeností zákazníků, předem načítání vaše prostředky uložené v mezipaměti může také přinést snížení síťový provoz na původním serveru.

> [!NOTE]
> Předběžné načítání prostředků je užitečné pro velké události nebo obsah, který současně dostupné pro velký počet uživatelů, jako je nová verze film nebo aktualizace softwaru.
> 
> 

Tento kurz vás provede předběžné načítání obsahu v mezipaměti na všech uzlech edge Azure CDN.

## <a name="walkthrough"></a>Názorný postup
1. V [portálu Azure](https://portal.azure.com), vyhledejte profil CDN obsahující chcete předběžné načtení koncový bod.  Otevře se okno profil.
2. Klikněte na koncový bod v seznamu.  Otevře se okno koncový bod.
3. V okně koncového bodu CDN klikněte na tlačítko zatížení.
   
    ![Okno koncový bod CDN](./media/cdn-preload-endpoint/cdn-endpoint-blade.png)
   
    Otevře se okno zatížení.
   
    ![Okno zatížení CDN](./media/cdn-preload-endpoint/cdn-load-blade.png)
4. Zadejte úplnou cestu každý prostředek, který chcete načíst (například `/pictures/kitten.png`) v **cesta** textové pole.
   
   > [!TIP]
   > Další **cesta** textová pole se zobrazí, když zadáte text, který vám umožní sestavit seznam více prostředků.  Prostředky můžete odstranit ze seznamu kliknutím na tlačítko se třemi tečkami (...).
   > 
   > Cesty musí být relativní adresa URL, která odpovídá následující [regulární výraz](https://msdn.microsoft.com/library/az24scfc.aspx):  
   > >Načíst cestu k souboru jedním `@"^(?:\/[a-zA-Z0-9-_.%=\u0020]+)+$"`;  
   > >Načíst jeden soubor s řetězec dotazu`@"^(?:\?[-_a-zA-Z0-9\/%:;=!,.\+'&\u0020]*)?$";`  
   > 
   > Každý prostředek musí mít svůj vlastní cesta.  Není k dispozici žádná funkce zástupných znaků pro předběžné načítání prostředků.
   > 
   > 
   
    ![Tlačítko zatížení](./media/cdn-preload-endpoint/cdn-load-paths.png)
5. Klikněte **zatížení** tlačítko.
   
    ![Tlačítko zatížení](./media/cdn-preload-endpoint/cdn-load-button.png)

> [!NOTE]
> Existuje omezení 10 zatížení požadavků za minutu za profil CDN. 50 cest jsou povoleny na základě požadavku. Každá cesta může mít délku cesty 1024 znaků.
> 
> 

## <a name="see-also"></a>Viz také
* [Vyprázdnění koncového bodu Azure CDN](cdn-purge-endpoint.md)
* [Azure referenční dokumentace rozhraní API REST CDN - mazání nebo předběžné načtení koncový bod](https://msdn.microsoft.com/library/mt634451.aspx)

