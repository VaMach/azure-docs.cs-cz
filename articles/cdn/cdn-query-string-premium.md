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
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: 145067c2ce50b41c4783f4de4052c0e7cb529fc7
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="control-azure-cdn-caching-behavior-with-query-strings---premium"></a>Ovládací prvek Azure CDN ukládání do mezipaměti chování řetězce dotazu - Premium
> [!div class="op_single_selector"]
> * [Standard](cdn-query-string.md)
> * [Azure CDN Premium od společnosti Verizon](cdn-query-string-premium.md)
> 
> 

## <a name="overview"></a>Přehled
Řetězec dotazu ukládání do mezipaměti ovládací prvky, jak jsou soubory do mezipaměti, které obsahují řetězce dotazu.

> [!IMPORTANT]
> Standard a Premium CDN produkty poskytovat stejné funkce mezipaměti řetězec dotazu, ale uživatelské rozhraní se liší.  Tento dokument popisuje rozhraní pro **Azure CDN Premium od společnosti Verizon**.  Pro dotaz řetězec ukládání do mezipaměti s **Azure CDN Standard od společnosti Akamai** a **Azure CDN Standard od společnosti Verizon**, najdete v části [řízení chování ukládání do mezipaměti CDN požadavky s řetězci dotazů](cdn-query-string.md).
> 
> 

K dispozici jsou tři režimy:

* **Standardní mezipaměti**: Toto je výchozí režim.  CDN hraničního uzlu předá řetězec dotazu z žadatel počátek na první požadavek a mezipaměti asset.  Všechny následné žádosti pro tento prostředek, které jsou obsluhovány z uzlu edge bude ignorovat řetězec dotazu do vypršení platnosti mezipaměti asset.
* **Ne mezipaměti**: V tomto režimu žádostí s řetězci dotazu se neukládají do mezipaměti v uzlu edge CDN.  Hraničního uzlu načte asset přímo z tohoto počátku a předává je pro žadatele s každou žádostí.
* **Jedinečný mezipaměti**: Tento režim považuje za jedinečný prostředek s vlastní mezipaměti každou žádost s řetězec dotazu.  Například odpověď z tohoto počátku pro žádost o *foo.ashx?q=bar* by uloží do mezipaměti na uzlu edge a vrátit pro následné mezipaměti s Tento stejný řetězec dotazu.  Žádost o *foo.ashx?q=somethingelse* by do mezipaměti jako samostatné asset s vlastním časem TTL.

## <a name="changing-query-string-caching-settings-for-premium-cdn-profiles"></a>Změna nastavení pro profily CDN premium ukládání řetězců s dotazy
1. Okno profil CDN, klikněte **spravovat** tlačítko.
   
    ![Tlačítko Spravovat okno profil CDN](./media/cdn-query-string-premium/cdn-manage-btn.png)
   
    Otevře se na portálu pro správu CDN.
2. Najeďte myší **HTTP velké** a potom přejděte myší **nastavení mezipaměti** plovoucím panelem.  Klikněte na **ukládání do mezipaměti řetězce dotazu**.
   
    Zobrazí se možnosti ukládání do mezipaměti řetězce dotazu.
   
    ![Řetězec dotazu CDN možnosti ukládání do mezipaměti](./media/cdn-query-string-premium/cdn-query-string.png)
3. Po provedení váš výběr, klikněte na **aktualizace** tlačítko.

> [!IMPORTANT]
> Změny nastavení nemusí být okamžitě viditelné, protože trvá, než se registrace rozšíří v rámci CDN.  V případě profilů <b>Azure CDN společnosti Verizon</b> je šíření obvykle hotové během 90 minut, ale někdy může trvat déle.
> 
> 

