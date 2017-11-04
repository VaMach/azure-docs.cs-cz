---
title: "Vytvářet a sdílet řídicí panely dat Azure Log Analytics | Microsoft Docs"
description: "Tento kurz vám pomůže pochopit jak řídicí panely analýzy protokolů můžete vizualizovat všechny uložený protokol hledání, budete jeden přehledu zobrazení prostředí."
services: log-analytics
documentationcenter: log-analytics
author: MGoedtel
manager: carmonm
editor: 
ms.assetid: abb07f6c-b356-4f15-85f5-60e4415d0ba2
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 09/14/2017
ms.author: magoedte
ms.custom: mvc
ms.openlocfilehash: 272945134b534a5ded794379ce5e96b0902a4227
ms.sourcegitcommit: e6029b2994fa5ba82d0ac72b264879c3484e3dd0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/24/2017
---
# <a name="create-and-share-dashboards-of-log-analytics-data"></a>Vytvářet a sdílet řídicí panely dat analýzy protokolů

Analýzy protokolů řídicích panelů můžete vizualizovat všechny uložený protokol hledání, což vám umožní najít, korelaci a sdílet IT provozních dat v organizaci.  Tento kurz se zaměřuje na vytvoření vyhledávání protokolu, který se použije pro podporu sdílené řídicí panel, který budou mít přístup váš tým podpory IT operace.  Získáte informace o těchto tématech:

> [!div class="checklist"]
> * Vytvořit sdílený řídicí panel portálu Azure
> * Vizualizace vyhledávání protokolu výkonu 
> * Přidat hledání protokolů na sdílené řídicí panel 
> * Přizpůsobení na dlaždici sdílené řídicí panel

Pokud chcete provést v příkladu v tomto kurzu, musí mít existující virtuální počítač [připojené k pracovní prostor analýzy protokolů](log-analytics-quick-collect-azurevm.md).  
 
## <a name="log-in-to-azure-portal"></a>Přihlaste se k portálu Azure
Přihlaste se k portálu Azure v [https://portal.azure.com](https://portal.azure.com). 

## <a name="create-a-shared-dashboard"></a>Vytvořit sdílený řídicí panel

Je první věcí, které se zobrazí po přihlášení k portálu Microsoft Azure [řídicí panel](../azure-portal/azure-portal-dashboards.md).<br> ![Řídicí panel portálu Azure](media/log-analytics-tutorial-dashboards/log-analytics-portal-dashboard.png)

Zde můžete najednou přenést provozních dat, která je nejdůležitější, aby IT napříč všechny vaše prostředky Azure, včetně telemetrie z Azure Log Analytics.  Předtím, než krok jsme do vizualizace hledání protokolů, můžeme nejprve vytvořit řídicí panel a její sdílení.  To umožňuje nám se získat stranou před použitím jsme naše příklad výkonu protokolu hledání, které se vykreslují jako spojnicový graf a přidejte ji do řídicího panelu.  

Chcete-li vytvořit řídicí panel, vyberte **novým řídicím panelem** tlačítko vedle názvu aktuálního řídicího panelu.<br> ![Vytvořit nový řídicí panel portálu Azure](media/log-analytics-tutorial-dashboards/log-analytics-create-dashboard-01.png)

Tato akce vytvoří řídicí panel nový, prázdný, privátní a vloží je do režimu přizpůsobení, kde můžete název řídicího panelu a přidání nebo změna uspořádání dlaždic. Umožňuje upravit název řídicího panelu a zadejte *ukázkový řídicí panel* pro tento kurz a pak vyberte **provádí přizpůsobení**.<br><br> ![Uložit přizpůsobené řídicí panel Azure](media/log-analytics-tutorial-dashboards/log-analytics-create-dashboard-02.png)

Když vytváříte řídicí panel, je soukromé ve výchozím nastavení, což znamená, že jste jediná osoba, která můžete zobrazit. Chcete-li vidět další uživatelé, použijte **sdílenou složku** tlačítko, které se zobrazí spolu s jinými příkazy řídicího panelu.<br> ![Sdílení nové řídicího panelu na portálu Azure](media/log-analytics-tutorial-dashboards/log-analytics-share-dashboard.png) 

Zobrazí se výzva vybrat skupinu prostředků pro řídicí panel publikování a odběru. Pro usnadnění práce je portálu publikování prostředí příručky směrem vzor umístění řídicí panely ve skupině prostředků jste volali metodu **řídicí panely**.  Ověřte předplatném vybraném a pak klikněte na tlačítko **publikovat**.  Řídí přístup k informace zobrazené na řídicím panelu s [řízení přístupu na základě prostředků Azure](../active-directory/role-based-access-control-configure.md).   

## <a name="visualize-a-log-search"></a>Vizualizace hledání protokolů

Můžete vytvořit základní dotazů na jeden řádek z portálu hledání protokolů na portálu Azure. Portálu hledání protokolů můžete používat bez spuštění externí portál a slouží k provádění různých funkcí s protokolu hledání, včetně vytvoření pravidla výstrah, vytvoření skupin počítačů a export výsledků dotazu. 

[Pokročilou analýzu portál](https://docs.loganalytics.io/docs/Learn/Getting-Started/Getting-started-with-the-Analytics-portal) je vyhrazené portál, který poskytuje pokročilé funkce není k dispozici na portálu hledání protokolů. Funkce zahrnují možnost upravit dotaz na více řádků, selektivně spuštění kódu, závislé na kontextu Intellisense a inteligentní analýzy. Na portálu pokročilé analýzy bude vytvoření zobrazení výkonu v grafických formulářů, uložit pro budoucí vyhledávání a připnout na sdílené vytvořený řídicí panel.   

Spuštění portálu Advanced Analytics z odkaz na portálu hledání protokolů.<br> ![Spuštění portálu pokročilé analýzy](media/log-analytics-tutorial-dashboards/log-analytics-advancedportal-01.png)

V portálu analýzy zadejte následující dotaz vrátí pouze procesoru využití záznamy pro počítače se systémy Windows a Linux, seskupené podle počítače a TimeGenerated a v visual grafu zobrazí:

```
Perf | where CounterName == "% Processor Time" and ObjectName == "Processor" and InstanceName == "_Total" | summarize AggregatedValue = avg(CounterValue) by bin(TimeGenerated, 1m), Computer | render timechart
```

Uložte dotaz tak, že vyberete **uložení dotazu** tlačítko v pravém horním rohu.<br> ![Uložení dotazu z portálu pokročilé analýzy](media/log-analytics-tutorial-dashboards/log-analytics-advancedportal-02.png)<br><br> V **uložit dotaz** ovládací panely, zadejte název, jako *virtuálních počítačích Azure - využití procesoru* a pak klikněte na **Uložit**.  Tímto způsobem můžete vytvořit knihovnu běžné dotazy vyhledávání s nebo změnit, aniž byste museli znovu zcela zápisu.  Nakonec to připnout na sdílené řídicí panel vytvořený výběrem **Pin graf na řídicí panel Azure** tlačítko ze střední pravém horním rohu stránky.  

Teď, když máme dotazu připnuli k řídicímu panelu, si všimnete, že má obecný název a komentář pod ním.<br> ![Ukázkový řídicí panel Azure](media/log-analytics-tutorial-dashboards/log-analytics-modify-dashboard-01.png)<br><br>  Jsme měli přejmenujte jej na něco smysluplného, který můžete tyto zobrazení ho snadno pochopit.  Klikněte pravým tlačítkem na dlaždici a vyberte **úpravy dlaždice**.  Jakmile budete hotovi, přizpůsobení nadpisu a podnadpisu pro dlaždici, klikněte na možnost **aktualizace**.  Hlavička se zobrazí s žádostí o publikování změn nebo zrušení.  Klikněte na tlačítko **publikovat změny** a pak zavřete **upravit dlaždice** podokně ovládacího prvku.  

![Dokončená konfigurace ukázkový řídicí panel](media/log-analytics-tutorial-dashboards/log-analytics-modify-dashboard-02.png)

## <a name="next-steps"></a>Další kroky
V tomto kurzu jste zjistili, jak vytvořit řídicí panel portálu Azure a přidejte do ní hledání protokolů.  Přechod na další kurzu se dozvíte, jiné odpovědi, které můžete implementovat podle výsledků vyhledávání protokolu.  

> [!div class="nextstepaction"]
> [Reakce na události s výstrahami analýzy protokolů](log-analytics-tutorial-response.md)