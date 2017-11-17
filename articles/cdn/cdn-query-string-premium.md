---
title: "Řízení ukládání do mezipaměti řetězce dotazu - Premium chování Azure CDN | Microsoft Docs"
description: "Azure CDN při ukládání řetězců dotazů ovládací prvky, jak jsou soubory do mezipaměti, které obsahují řetězce dotazu."
services: cdn
documentationcenter: 
author: zhangmanling
manager: erikre
editor: 
ms.assetid: 99db4a85-4f5f-431f-ac3a-69e05518c997
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/09/2017
ms.author: mazha
ms.openlocfilehash: 2021b5b7602605a7c264e9cd575399077691da34
ms.sourcegitcommit: 7d107bb9768b7f32ec5d93ae6ede40899cbaa894
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/16/2017
---
# <a name="control-azure-content-delivery-network-caching-behavior-with-query-strings---premium"></a>Ovládací prvek Azure Content Delivery Network ukládání do mezipaměti chování řetězce dotazu - Premium
> [!div class="op_single_selector"]
> * [Standard](cdn-query-string.md)
> * [Azure CDN Premium od společnosti Verizon](cdn-query-string-premium.md)
> 
> 

## <a name="overview"></a>Přehled
S Azure Content Delivery Network (CDN), můžete řídit, jak jsou soubory uložené v mezipaměti pro webový požadavek, který obsahuje řetězec dotazu. Řetězec dotazu v žádosti o webovou se řetězec dotazu je část požadavku, který se nachází za otazník (?). Řetězec dotazu může obsahovat jeden nebo více páry klíč hodnota, ve kterých pole názvu a hodnoty jsou odděleny znak rovná se (=). Jednotlivé páry klíč hodnota je oddělených ampersandem (&). Například `http://www.contoso.com/content.mov?field1=value1&field2=value2`. Pokud řetězec dotazu požadavku existuje více než jednu dvojici klíč / hodnota, jejich pořadí není důležité. 

> [!IMPORTANT]
> Produktů CDN standard a premium poskytují stejné funkce mezipaměti řetězec dotazu, ale uživatelské rozhraní se liší.  Tento článek popisuje rozhraní pro **Azure CDN Premium od společnosti Verizon**. Pro dotaz řetězec ukládání do mezipaměti s **Azure CDN Standard od společnosti Akamai** a **Azure CDN Standard od společnosti Verizon**, najdete v části [řízení chování ukládání do mezipaměti CDN požadavky s řetězci dotazů](cdn-query-string.md).
>

K dispozici jsou tři režimy řetězec dotazu:

- **Standardní mezipaměti**: výchozí režim. V tomto režimu hraničního uzlu CDN předává řetězce dotazu z žadatel k počátku na první požadavek a ukládá do mezipaměti asset. Všechny následné žádosti pro asset, které se zpracovávají z uzlu edge ignorovat řetězců dotazů do mezipaměti asset vypršení platnosti.
- **Ne mezipaměti**: V tomto režimu žádostí s řetězci dotazu se neukládají do mezipaměti v uzlu edge CDN. Hraničního uzlu načte asset přímo z tohoto počátku a předává je pro žadatele s každou žádostí.
- **Jedinečný mezipaměti**: V tomto režimu každou žádost s jedinečnou adresou URL, včetně řetězce dotazu, je považován za jedinečný prostředek s vlastní mezipaměti. Například odpověď z tohoto počátku pro žádost o `example.ashx?q=test1` je uloží do mezipaměti na uzlu edge a vrátit pro následné mezipaměti s stejné řetězec dotazu. Žádost o `example.ashx?q=test2` se uloží do mezipaměti jako samostatné asset s vlastní nastavení time to live.

## <a name="changing-query-string-caching-settings-for-premium-cdn-profiles"></a>Změna nastavení pro profily CDN premium ukládání řetězců s dotazy
1. Otevřete profil CDN a pak klikněte na **spravovat**.
   
    ![Tlačítko Spravovat profil CDN](./media/cdn-query-string-premium/cdn-manage-btn.png)
   
    Otevře se na portálu pro správu CDN.
2. Najeďte myší **HTTP velké** a potom přejděte myší **nastavení mezipaměti** plovoucím panelem nabídky. Klikněte na tlačítko **ukládání do mezipaměti řetězce dotazu**.
   
    Zobrazí se možnosti ukládání do mezipaměti řetězce dotazu.
   
    ![Řetězec dotazu CDN možnosti ukládání do mezipaměti](./media/cdn-query-string-premium/cdn-query-string.png)
3. Vyberte režim řetězec dotazu a pak klikněte na **aktualizace**.

> [!IMPORTANT]
> Protože trvá, než se registrace rozšíří v rámci CDN, změny nastavení mezipaměti řetězec nemusí být okamžitě viditelné. Pro **Azure CDN Premium od společnosti Verizon** profily, šíření obvykle dokončení během 90 minut, ale v některých případech může trvat déle.
 

