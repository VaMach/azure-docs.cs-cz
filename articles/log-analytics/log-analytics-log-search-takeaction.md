---
title: "Uživatel spustil v analýzy protokolů Azure Automation Runbook akci | Microsoft Docs"
description: "Tento článek popisuje, jak spouštět runbook služby automatizace ze Log Analytics vyhledávání výsledek na vyžádání."
services: log-analytics
documentationcenter: 
author: mgoedtel
manager: carmonm
editor: tysonn
ms.assetid: 
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/04/2017
ms.author: magoedte
ms.openlocfilehash: ff938697add98f3d21b4971175432335ee2e39ba
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="take-action-with-an-automation-runbook-from-a-log-analytics-log-search-result"></a>Akce s Runbook služby automatizace z výsledků na vyhledávacím protokolu analýzy protokolů

Ve výsledku hledání protokolu v Azure Log Analytics můžete nyní vybrat **akce** spustit runbook služby automatizace.  Sada runbook lze opravit problém nebo provést další akce, jako shromažďovat informace o odstraňování potíží, e-mailovou zprávu, nebo vytvořte žádost o služby. 

## <a name="components-and-features-used"></a>Použité komponenty a funkce
* [Účet Azure Automation.](../automation/automation-offering-get-started.md)
* [Pracovní prostor analýzy protokolů](../log-analytics/log-analytics-overview.md)

## <a name="to-initiate-runbook-from-log-search"></a>Inicializace sady runbook z protokolu hledání

Provést akci pro událost a zahájit sady runbook z výsledků hledání protokolu, začněte vytvořením hledání protokolů a z výsledků můžete vyvolat runbook na vyžádání.  Toho lze dosáhnout z protokolu funkce hledání v Azure nebo [portálu OMS](../log-analytics/log-analytics-log-searches.md).  V tomto příkladu jsme vyhledávání protokolu z portálu Azure s základní ukázka této funkce.

1. Portál Azure, v nabídce centra klikněte na možnost **další služby** a vyberte **analýzy protokolů**.  
2. V okně Log Analytics vyberte pracovní prostor analýzy protokolů a v okně prostoru vyberte **hledání protokolů**.  
3. V okně hledání protokolů hledání protokolu.  
4. Ve výsledcích hledání protokolu, klikněte na tlačítko se třemi tečkami nalevo od jednoho pole a z místní nabídce vyberte **provést akci pro**.<br><br> ![Vyberte akci trvat od výsledek hledání](./media/log-analytics-log-search-takeaction/log-search-takeaction-menuoption.png) 
5. V okně proveďte akce vyberte **spuštění sady runbook**a na **spuštění sady runbook** okno můžete vybrat spuštění sady runbook.  Všechny sady runbook můžete vybrat v účtu Automation, který bude propojen pracovní prostor analýzy protokolů.  Je třeba počítat s následujícím:

    * Sady Runbook jsou uspořádané podle značky
    * Hodnoty vstupní parametr Runbooku je možné vybrat přímo z pole výsledek hledání.  Rozevíracím seznamu se zobrazí zobrazení všechna dostupná pole z výsledku lze vybírat.  
    * Můžete spustit sadu runbook na [hybridní pracovní proces runbooku](../automation/automation-hybrid-runbook-worker.md) nainstalovaného v počítači, který má problém, pokud máte odpovídající skupinu hybridní pracovní proces Runbooku, který obsahuje pouze tento počítač jako člena.  Pokud název skupinu hybridních pracovních procesů odpovídá názvu počítače, který je ve výsledku hledání protokolu, bude automaticky vybrán skupině.    

6. Po kliknutí na tlačítko **spustit**, abyste mohli zkontrolovat stav úlohy se otevře okno úlohy runbooku.   

Pokud vyberete runbook, který byl nakonfigurován tak, aby se [volat z upozornění na analýzy protokolů](../automation/automation-invoke-runbook-from-omsla-alert.md), má vstupní parametr s názvem **WebhookData** tedy **objekt** typu.  Pokud vstupní parametr je povinný, potřebujete předat výsledky hledání v sadě runbook, takže ho převést řetězec ve formátu JSON na typ objektu, který umožňuje filtrovat konkrétní položky, které budete odkazovat v aktivity sady runbook.  To uděláte tak, že vyberete **hledání výsledek (objekt)** z rozevíracího seznamu.<br><br> ![Vyberte datový objekt Webhooku pro parametr sady runbook](media/log-analytics-log-search-takeaction/select-runbook-and-properties.png)   
    
## <a name="next-steps"></a>Další kroky

* Zkontrolujte [analýzy protokolů protokolu vyhledávání odkaz](log-analytics-search-reference.md) zobrazíte všechna pole hledání a omezující vlastnosti, které jsou k dispozici v analýzy protokolů.
* Informace o tom, které má být vyvolán runbook služby automatizace automaticky, zkontrolujte [volání runbook služby automatizace Azure z výstrahu analýzy protokolů OMS](../automation/automation-invoke-runbook-from-omsla-alert.md).  