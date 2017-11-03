---
title: "Reakce na události s výstrahami, Azure Log Analytics | Microsoft Docs"
description: "Tento kurz vám pomůže pochopit výstrahy v analýzy protokolů k identifikaci důležité informace ve svém úložišti OMS a proaktivně upozorňují na problémy nebo vyvolání akce se pokusit o opravte je."
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
ms.date: 09/20/2017
ms.author: magoedte
ms.custom: mvc
ms.openlocfilehash: 3ab8d32eb4b3f2748249f40139de76c8e7f4d971
ms.sourcegitcommit: e6029b2994fa5ba82d0ac72b264879c3484e3dd0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/24/2017
---
# <a name="respond-to-events-with-log-analytics-alerts"></a>Reakce na události s výstrahami analýzy protokolů
Výstrahy v Log Analytics identifikovat důležité informace do úložiště analýzy protokolů.  Jsou vytvořené pomocí pravidla výstrah, které automaticky spustit vyhledávání protokolu v pravidelných intervalech a pokud výsledky hledání protokolů odpovídají konkrétním kritériím je vytvořen záznam výstrah a dá se provést automatické odpovědi.  Tento kurz je určen pokračování [řídicí panely vytvoření a sdílení dat analýzy protokolů](log-analytics-tutorial-dashboards.md) kurzu.   

V tomto kurzu se naučíte:

> [!div class="checklist"]
> * Vytvořit pravidlo výstrahy
> * Konfigurace pravidla výstrahy k odesílání e-mailové oznámení

Pokud chcete provést v příkladu v tomto kurzu, musí mít existující virtuální počítač [připojené k pracovní prostor analýzy protokolů](log-analytics-quick-collect-azurevm.md).  

## <a name="log-in-to-azure-portal"></a>Přihlaste se k portálu Azure
Přihlaste se k portálu Azure v [https://portal.azure.com](https://portal.azure.com). 

## <a name="create-alerts"></a>Vytváření upozornění

Výstrahy jsou vytvářeny pravidla výstrah, které automaticky spustit vyhledávání protokolu v pravidelných intervalech.  Můžete vytvořit oznámení na základě metriky výkonu specifických nebo když se vytváří určité události, nedojde událost nebo určitý počet událostí jsou vytvořené v rámci konkrétní časové okno.  Výstrahy můžete například použít upozornění v případě průměrné využití procesoru překračuje prahovou hodnotu určité události se vygeneruje, když konkrétní služby systému Windows nebo Linux démon není spuštěn.   Pokud výsledky hledání protokolů odpovídají konkrétním kritériím se vytvoří záznam výstrahy. Pravidlo lze poté automaticky spouštět jednu nebo více akcí proaktivně oznámíme vám výstrahy nebo vyvolání jiný proces. 

V následujícím příkladu vytvoříme metriky měření pravidlo výstrahy, které vytvoří výstrahu při každém objektu počítače v dotazu s hodnotou, která překračuje prahovou hodnotu 90 %.

1. Na portálu Azure klikněte na tlačítko **další služby** v levém dolním nalezen. V seznamu prostředků zadejte **Log Analytics**. Seznam se průběžně filtruje podle zadávaného textu. Vyberte **protokolu analýzy**...
2. Spusťte na portálu OMS výběrem portálu OMS a na **přehled** vyberte **hledání protokolů**.  
3. Vyberte **Oblíbené** z horní části portálu a v **uložená hledání** na pravé straně vyberte dotaz *virtuálních počítačích Azure - využití procesoru*.  
4. Klikněte na tlačítko **výstrahy** v horní části stránky otevřete **přidat pravidlo výstrahy** obrazovky.  
5. Pravidlo výstrahy nakonfigurujte následující informace:  
   a. Zadejte **název** výstrahy, jako například *překročena využití procesoru virtuálního počítače > 90*  
   b. Pro **časové okno**, zadejte časový interval pro dotaz, jako například *30*.  Dotaz vrátí pouze záznamy, které byly vytvořeny v tomto rozsahu aktuální čas.  
   c. **Výstrahy frekvence** Určuje, jak často se má spustit dotaz.  V tomto příkladu zadejte *5* minut, které se vyskytují v našich časový interval, který je zadán.  
   d. Vyberte **metrika měří období** a zadejte *90* pro **agregovaná hodnota** a zadejte *3* pro **aktivační událost upozornění na základě**   
   e. V části **akce**, zakažte e-mailové oznámení.
6. Klikněte na tlačítko **Uložit** k dokončení pravidlo výstrahy. Zahájí se okamžitě spuštěna.<br><br> ![Příklad pravidlo výstrahy](media/log-analytics-tutorial-response/log-analytics-alert-01.png)

Výstrahy záznamy vytvořené pravidla výstrah v analýzy protokolů mít typ **výstraha** a SourceSystem z **OMS**.<br><br> ![Příklad výstrah generovaných událostí](media/log-analytics-tutorial-response/log-analytics-alert-events-01.png)  

## <a name="alert-actions"></a>Akce upozornění
Můžete provádět pokročilé akce s výstrahami, jako je vytváření e-mailové oznámení, spusťte [sady Automation runbook](../automation/automation-runbook-types.md), použijte k vytvoření incidentu záznamů v systému správy incidentů ITSM nebo s webhook, jehož [IT Řešení pro správu konektoru služby](log-analytics-itsmc-overview.md) jako odpověď při splnění kritérií výstrah.   

E-mailu akce odeslání e-mailu s detaily výstrahy jednoho nebo více příjemců. Můžete zadat předmět e-mailu, ale jeho obsah je standardní formát vytvořená pomocí analýzy protokolů.  Umožňuje pravidlo výstrahy dříve vytvořili a nakonfigurovat ji k e-mailová oznámení o aktualizaci můžete místo aktivně monitorování pro výstrahy záznam s hledání protokolů.     

1. Na portálu OMS v horní nabídce vyberte **nastavení** a pak vyberte **výstrahy**.
2. Ze seznamu pravidla výstrah klikněte na ikonu tužky vedle výstrahy na vytvořili dříve.
3. V části **akce** povolte e-mailová oznámení.
4. Zadejte **subjektu** pro e-mailu, jako například *prahová hodnota překročena využití procesoru > 90*.
5. Přidejte jeden nebo více příjemců e-mailu v adresy **příjemce** pole.  Pokud zadáte víc než jednou adresou, jednotlivé adresy oddělujte středníkem (;).
6. Klikněte na tlačítko **Uložit** k dokončení pravidlo výstrahy. Zahájí se okamžitě spuštěna.<br><br> ![Pravidlo upozornění s e-mailových oznámení](media/log-analytics-tutorial-response/log-analytics-alert-02.png)

## <a name="next-steps"></a>Další kroky
V tomto kurzu naučili, jak výstrahy pravidla aktivně identifikovat a reagovat na problémy při spuštění protokolu hledání v naplánované intervalech a odpovídají konkrétním kritériím.  

Chcete-li zobrazit předdefinovaných ukázky skriptu analýzy protokolů na tento odkaz.  

> [!div class="nextstepaction"]
> [Přihlaste se ukázky skriptu Analytics](powershell-samples.md)