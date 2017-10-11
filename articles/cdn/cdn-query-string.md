---
title: "Řízení ukládání do mezipaměti řetězce dotazu chování Azure CDN | Microsoft Docs"
description: "Azure CDN při ukládání řetězců dotazů ovládací prvky, jak jsou soubory do mezipaměti, které obsahují řetězce dotazu."
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
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: 8d79626fa8516f226a82d3dac693c2033904c91d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="control-azure-cdn-caching-behavior-with-query-strings"></a>Ovládací prvek Azure CDN ukládání do mezipaměti chování řetězce dotazu
> [!div class="op_single_selector"]
> * [Standard](cdn-query-string.md)
> * [Azure CDN Premium od společnosti Verizon](cdn-query-string-premium.md)
> 
> 

## <a name="overview"></a>Přehled
Řetězec dotazu ukládání do mezipaměti ovládací prvky, jak jsou soubory do mezipaměti, které obsahují řetězce dotazu.

> [!IMPORTANT]
> Standard a Premium CDN produkty poskytovat stejné funkce mezipaměti řetězec dotazu, ale uživatelské rozhraní se liší.  Tento dokument popisuje rozhraní pro **Azure CDN Standard od společnosti Akamai** a **Azure CDN Standard od společnosti Verizon**.  Pro dotaz řetězec ukládání do mezipaměti s **Azure CDN Premium od společnosti Verizon**, najdete v části [řízení chování ukládání do mezipaměti CDN požadavky s řetězci dotazů - Premium](cdn-query-string-premium.md).
> 
> 

K dispozici jsou tři režimy:

* **Ignorovat řetězce dotazů**: Toto je výchozí režim.  CDN hraničního uzlu předá řetězec dotazu z žadatel počátek na první požadavek a mezipaměti asset.  Všechny následné žádosti pro tento prostředek, které jsou obsluhovány z uzlu edge bude ignorovat řetězec dotazu do vypršení platnosti mezipaměti asset.
* **Nepoužívat ukládání do mezipaměti pro URL s řetězci dotazů**: V tomto režimu žádostí s řetězci dotazu se neukládají do mezipaměti v uzlu edge CDN.  Hraničního uzlu načte asset přímo z tohoto počátku a předává je pro žadatele s každou žádostí.
* **Ukládat do mezipaměti každou jedinečnou adresu URL**: Tento režim považuje za jedinečný prostředek s vlastní mezipaměti každou žádost s řetězec dotazu.  Například odpověď z tohoto počátku pro žádost o *foo.ashx?q=bar* by uloží do mezipaměti na uzlu edge a vrátit pro následné mezipaměti s Tento stejný řetězec dotazu.  Žádost o *foo.ashx?q=somethingelse* by do mezipaměti jako samostatné asset s vlastním časem TTL.

## <a name="changing-query-string-caching-settings-for-standard-cdn-profiles"></a>Změna nastavení pro standardní profily CDN ukládání řetězců s dotazy
1. Z okno profil CDN klikněte na koncový bod CDN, které chcete spravovat.
   
    ![Koncové body okno profil CDN](./media/cdn-query-string/cdn-endpoints.png)
   
    Otevře se okno koncového bodu CDN.
2. Klikněte **konfigurace** tlačítko.
   
    ![Tlačítko Spravovat okno profil CDN](./media/cdn-query-string/cdn-config-btn.png)
   
    Otevře se okno Konfigurace CDN.
3. Vyberte nastavení z **chování ukládání řetězců s dotazy** rozevíracího seznamu.
   
    ![Řetězec dotazu CDN možnosti ukládání do mezipaměti](./media/cdn-query-string/cdn-query-string.png)
4. Po provedení váš výběr, klikněte na **Uložit** tlačítko.

> [!IMPORTANT]
> Změny nastavení nemusí být okamžitě viditelné, protože trvá, než se registrace rozšíří v rámci CDN.  V případě profilů <b>Azure CDN společnosti Akamai</b> je šíření obvykle hotové během jedné minuty.  V případě profilů <b>Azure CDN společnosti Verizon</b> je šíření obvykle hotové během 90 minut, ale někdy může trvat déle.
> 
> 

