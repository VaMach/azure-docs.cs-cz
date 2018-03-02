---
title: "Reakce na události s využitím upozornění Azure Log Analytics | Microsoft Docs"
description: "Tento kurz vám pomůže porozumět používání upozornění v Log Analytics k identifikaci důležitých informací v úložišti OMS a proaktivnímu upozorňování na problémy nebo vyvolávání akcí ve snaze je opravit."
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
ms.openlocfilehash: fcfaa849f67ffcfa69672d116837e96d318c2124
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/21/2018
---
# <a name="respond-to-events-with-log-analytics-alerts"></a>Reakce na události s využitím upozornění Log Analytics
Upozornění v Log Analytics identifikují důležité informace v úložišti Log Analytics. Vytvářejí se na základě pravidel upozornění, která automaticky a v pravidelných intervalech spouští prohledávání protokolů. Pokud výsledky prohledávání protokolů splňují konkrétní kritéria, vytvoří se záznam upozornění, který je možné nakonfigurovat k provedení automatizované reakce.  Tento kurz je pokračováním kurzu [Vytváření a sdílení řídicích panelů s daty Log Analytics](log-analytics-tutorial-dashboards.md).   

V tomto kurzu se naučíte:

> [!div class="checklist"]
> * Vytvoření pravidla upozornění
> * Konfigurace pravidla upozornění k odeslání e-mailového oznámení

K dokončení příkladu v tomto kurzu potřebujete existující virtuální počítač [připojený k pracovnímu prostoru Log Analytics](log-analytics-quick-collect-azurevm.md).  

## <a name="log-in-to-azure-portal"></a>Přihlášení k webu Azure Portal
Přihlaste se k webu Azure Portal na adrese [https://portal.azure.com](https://portal.azure.com). 

## <a name="create-alerts"></a>Vytváření upozornění

Upozornění se vytvářejí na základě pravidel upozornění, která automaticky a v pravidelných intervalech spouští prohledávání protokolů.  Můžete vytvářet upozornění na základě konkrétních metrik výkonu, vytvoření určitých událostí, chybějící události nebo počtu událostí vytvořených v konkrétním časovém intervalu.  Upozornění můžete například využít k upozorňování, když průměrné využití procesoru překročí určitou prahovou hodnotu nebo když se vygeneruje událost v případě, že není spuštěná určitá služba ve Windows nebo proces démon v Linuxu.   Pokud výsledky prohledávání protokolů splňují konkrétní kritéria, vytvoří se záznam upozornění. Pravidlo pak může automaticky spustit jednu nebo více akcí a proaktivně vás upozornit nebo vyvolat jiný proces. 

V následujícím příkladu vytvoříte pravidlo upozornění na základě měření metriky, které vytvoří upozornění pro každý objekt počítače v dotazu, jehož hodnota překračuje 90 % prahové hodnoty.

1. Na webu Azure Portal klikněte na **Všechny služby**. V seznamu prostředků zadejte **Log Analytics**. Seznam se průběžně filtruje podle zadávaného textu. Vyberte **Log Analytics**.
2. Spusťte portál OMS tak, že vyberete Portál OMS a na stránce **Přehled** vyberete **Prohledávání protokolů**.  
3. V horní části portálu vyberte **Oblíbené** a v pravém podokně **Uložená hledání** vyberte dotaz *Virtuální počítače Azure – Využití procesoru*.  
4. Kliknutím na **Upozornění** v horní části stránky otevřete obrazovku **Přidat pravidlo upozornění**.  
5. Nakonfigurujte pravidlo upozornění s použitím následujících informací:  
   a. Zadejte **Název** upozornění, například *Využití procesoru virtuálního počítače překročilo 90 %*.  
   b. Jako **Časový interval** zadejte časový rozsah dotazu, například *30*.  Dotaz vrátí pouze záznamy vytvořené v tomto rozsahu před aktuálním časem.  
   c. **Frekvence upozornění** určuje, jak často se má dotaz spouštět.  V tomto příkladu zadejte *5* minut, což spadá do našeho zadaného časového intervalu.  
   d. Vyberte **Měření metriky**, zadejte *90* v části **Agregovaná hodnota** a zadejte *3* v části **Aktivovat upozornění na základě**   
   e. V části **Akce** zakažte e-mailové oznámení.
6. Kliknutím na **Uložit** dokončete pravidlo upozornění. Pravidlo se okamžitě spustí.<br><br> ![Příklad pravidla upozornění](media/log-analytics-tutorial-response/log-analytics-alert-01.png)

Záznamy upozornění vytvořené pravidly upozornění v Log Analytics jako Typ mají **Upozornění** a jako SourceSystem mají **OMS**.<br><br> ![Příklad vygenerovaných událostí upozornění](media/log-analytics-tutorial-response/log-analytics-alert-events-01.png)  

## <a name="alert-actions"></a>Akce upozornění
Pomocí upozornění můžete provádět pokročilé akce, například vytvořit e-mailové oznámení, spustit [runbook služby Automation](../automation/automation-runbook-types.md), pomocí webhooku vytvořit záznam o incidentu v systému pro správu incidentů ITSM nebo jako reakci na splnění kritérií upozornění použít [řešení ITSM konektor](log-analytics-itsmc-overview.md).   

Akce e-mailu odešle e-mail s podrobnostmi o upozornění jednomu nebo více příjemcům. Můžete zadat předmět e-mailu, ale jeho obsah má standardní formát, který vytvoří Log Analytics.  Teď aktualizujeme pravidlo upozornění vytvořené dříve a nakonfigurujeme ho tak, aby místo aktivního monitorování záznamů upozornění pomocí prohledávání protokolů odesílalo e-mailová oznámení.     

1. Na portálu OMS v horní nabídce vyberte **Nastavení** a pak **Upozornění**.
2. V seznamu pravidel upozornění klikněte na ikonu tužky vedle dříve vytvořeného upozornění.
3. V části **Akce** povolte e-mailová oznámení.
4. Zadejte **Předmět** e-mailu, například *Využití procesoru překročilo 90 % prahové hodnoty*.
5. Do pole **Příjemci** přidejte adresy jednoho nebo více příjemců e-mailu.  Pokud zadáte více než jednu adresu, oddělte adresy středníkem (;).
6. Kliknutím na **Uložit** dokončete pravidlo upozornění. Pravidlo se okamžitě spustí.<br><br> ![Pravidlo upozornění s e-mailovým oznámením](media/log-analytics-tutorial-response/log-analytics-alert-02.png)

## <a name="next-steps"></a>Další kroky
V tomto kurzu jste zjistili, jak můžou pravidla upozornění proaktivně identifikovat problémy spouštěním prohledávání protokolů v naplánovaných intervalech a reagovat na problémy v případě, že výsledky prohledávání splní konkrétní kritéria.  

Na tomto odkazu najdete předem připravené ukázky skriptů pro Log Analytics.  

> [!div class="nextstepaction"]
> [Ukázky skriptů pro Log Analytics](powershell-samples.md)