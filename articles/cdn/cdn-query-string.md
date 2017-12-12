---
title: "Řízení Azure Content Delivery Network ukládání do mezipaměti chování řetězce dotazu | Microsoft Docs"
description: "Azure CDN při ukládání řetězců dotazů ovládací prvky, jak se mezipaměti soubory, které obsahují řetězce dotazu."
services: cdn
documentationcenter: 
author: zhangmanling
manager: erikre
editor: 
ms.assetid: 17410e4f-130e-489c-834e-7ca6d6f9778d
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/09/2017
ms.author: mazha
ms.openlocfilehash: 9ffd05a0eb4d976dc40a1c5d45fd22ebf9bd4db1
ms.sourcegitcommit: 4ac89872f4c86c612a71eb7ec30b755e7df89722
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/07/2017
---
# <a name="control-azure-content-delivery-network-caching-behavior-with-query-strings"></a>Ovládací prvek Azure Content Delivery Network ukládání do mezipaměti chování řetězce dotazu
> [!div class="op_single_selector"]
> * [Standard](cdn-query-string.md)
> * [Azure CDN Premium od společnosti Verizon](cdn-query-string-premium.md)
> 

## <a name="overview"></a>Přehled
S Azure Content Delivery Network (CDN), můžete řídit, jak jsou soubory uložené v mezipaměti pro webový požadavek, který obsahuje řetězec dotazu. Řetězec dotazu v žádosti o webovou se řetězec dotazu je část požadavku, který se nachází za otazník (?). Řetězec dotazu může obsahovat jeden nebo více páry klíč hodnota, ve kterých pole názvu a hodnoty jsou odděleny znak rovná se (=). Jednotlivé páry klíč hodnota je oddělených ampersandem (&). Například `http://www.contoso.com/content.mov?field1=value1&field2=value2`. Pokud řetězec dotazu požadavku existuje více než jednu dvojici klíč / hodnota, jejich pořadí není důležité. 

> [!IMPORTANT]
> Produktů CDN standard a premium poskytují stejné funkce mezipaměti řetězec dotazu, ale uživatelské rozhraní se liší.  Tento článek popisuje rozhraní pro **Azure CDN Standard od společnosti Akamai** a **Azure CDN Standard od společnosti Verizon**. Pro dotaz řetězec ukládání do mezipaměti s **Azure CDN Premium od společnosti Verizon**, najdete v části [řízení chování ukládání do mezipaměti CDN požadavky s řetězci dotazů - Premium](cdn-query-string-premium.md).

K dispozici jsou tři režimy řetězec dotazu:

- **Ignorovat řetězce dotazů**: výchozí režim. V tomto režimu hraničního uzlu CDN předává řetězce dotazu z žadatel k počátku na první požadavek a ukládá do mezipaměti asset. Všechny následné žádosti pro asset, které se zpracovávají z uzlu edge ignorovat řetězců dotazů do mezipaměti asset vypršení platnosti.
- **Nepoužívat ukládání do mezipaměti pro řetězce dotazů**: V tomto režimu žádostí s řetězci dotazu se neukládají do mezipaměti v uzlu edge CDN. Hraničního uzlu načte asset přímo z tohoto počátku a předává je pro žadatele s každou žádostí.
- **Ukládat do mezipaměti každou jedinečnou adresu URL**: V tomto režimu každou žádost s jedinečnou adresou URL, včetně řetězce dotazu, je považován za jedinečný prostředek s vlastní mezipaměti. Například odpověď z tohoto počátku pro žádost o `example.ashx?q=test1` je uloží do mezipaměti na uzlu edge a vrátit pro následné mezipaměti s stejné řetězec dotazu. Žádost o `example.ashx?q=test2` se uloží do mezipaměti jako samostatné asset s vlastní nastavení time to live.

## <a name="changing-query-string-caching-settings-for-standard-cdn-profiles"></a>Změna nastavení pro standardní profily CDN ukládání řetězců s dotazy
1. Otevřete profil CDN a potom vyberte koncového bodu CDN, které chcete spravovat.
   
   ![Koncové body profil CDN](./media/cdn-query-string/cdn-endpoints.png)
   
2. V levém podokně v části nastavení, klikněte na tlačítko **ukládání do mezipaměti pravidla**.
   
    ![Ukládání do mezipaměti CDN tlačítko pravidla](./media/cdn-query-string/cdn-caching-rules-btn.png)
   
3. V **chování ukládání řetězců s dotazy** seznam, vyberte režim řetězec dotazu a pak klikněte na tlačítko **Uložit**.
   
   ![Řetězec dotazu CDN možnosti ukládání do mezipaměti](./media/cdn-query-string/cdn-query-string.png)

> [!IMPORTANT]
> Protože trvá, než se registrace rozšíří v rámci CDN, změny nastavení mezipaměti řetězec nemusí být okamžitě viditelné. V případě profilů **Azure CDN od Akamai** je šíření obvykle hotové během jedné minuty. V případě profilů **Azure CDN od společnosti Verizon** je šíření obvykle hotové během 90 minut, ale někdy může trvat déle.


