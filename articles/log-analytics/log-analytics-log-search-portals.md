---
title: "Portály pro vytváření a úpravy dotazů protokolu v Azure Log Analytics | Microsoft Docs"
description: "Tento článek popisuje portály, které můžete použít v Azure Log Analytics můžete vytvářet a upravovat protokolu hledání."
services: log-analytics
documentationcenter: 
author: bwren
manager: carmonm
editor: 
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/26/2017
ms.author: magoedte; bwren
ms.openlocfilehash: b205f226d95d94b938a70a834ac0147e76d459ea
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="portals-for-creating-and-editing-log-queries-in-azure-log-analytics"></a>Portály pro vytváření a úpravy dotazů protokolu v Azure Log Analytics

Protokol hledání v různých místech analýzy protokolů se použít k načtení dat z pracovního prostoru.  Ve skutečnosti vytváření a úpravy dotazů kromě práce interaktivně se vrácená data, když, máte dvě možnosti, které jsou popsány níže.  

## <a name="log-search"></a>Prohledávání protokolů 
Stránka hledání protokolů je dostupný z portálu Azure.  Je vhodné pro vytvoření základní dotazy, které lze vytvořit na jeden řádek.  Hledání protokolů můžete používat bez spuštění externí portál a slouží k provádění různých funkcí s protokolu hledání, včetně vytvoření pravidla výstrah, vytvoření skupin počítačů a export výsledků dotazu.  

Hledání protokolů poskytuje více funkcí pro úpravu dotaz bez nutnosti úplné znalost dotazovacího jazyka.  Zobrazí souhrn těchto funkcí v [vytvořit protokolu hledání v Azure Log Analytics pomocí hledání protokolů](log-analytics-log-search-log-search-portal.md).


![Stránka vyhledávání protokolu](media/log-analytics-log-search-portals/log-search-portal.png)

## <a name="advanced-analytics-portal"></a>Pokročilé analýzy portálu
Portálu pro pokročilou analýzu je vyhrazené portál, který poskytuje pokročilé funkce není k dispozici v hledání protokolů z portálu Azure.  Funkce zahrnují možnost upravit dotaz na více řádků, selektivně spuštění kódu, závislé na kontextu Intellisense a inteligentní analýzy.  Portál Advanced Analytics je nejvhodnější pro návrh složitých dotazů, které jsou buď jako vyhledávání protokolu uložit nebo kopírovat a vkládat data do jiných elementů analýzy protokolů.  Spuštění portálu Advanced Analytics z odkaz na stránce hledání protokolů.

![Pokročilé analýzy portálu](media/log-analytics-log-search-portals/advanced-analytics-portal.png)


Z důvodu jeho pokročilé funkce obvykle použijete portálu pokročilou analýzu jako svůj primární nástroj pro vytváření a úpravy dotazů.  Jednou jste zjistili, že dotaz funguje podle očekávání, a potom budete zkopírujte a vložte ho jinde například stránka vyhledávání protokolu nebo Návrhář zobrazení.  Protože portál Advanced Analytics podporuje dotazy s více řádky, i když je nutné vzít v úvahu následující při kopírování dotazu z tento portál.

- Komentáře musí odebrat z dotazu před má zkopírovat a vložit do jiného umístění.  Můžete okomentovat řádek tak, že před s dvě lomítka (/ /).  Když vložíte více dotazu řádku do jednoho řádku, jsou odebrány konce řádků.  Pokud jsou zahrnuty komentáře, jsou všechny znaky po první komentář považovány za součást komentář.


## <a name="next-steps"></a>Další kroky

- Provede kurz k používání [hledání protokolů](log-analytics-tutorial-viewdata.md) Další informace o vytváření dotazů pomocí dotazu jazyka
- Podívejte se [Advanced Analytics portál](https://go.microsoft.com/fwlink/?linkid=856587) k vytvoření složitých dotazů a použít jako vývojové prostředí pro svoje vyhledávání protokolu.

