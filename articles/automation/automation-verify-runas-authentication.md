---
title: "Ověření konfigurace účtu Azure Automation | Dokumentace Microsoftu"
description: "Tento článek popisuje způsob ověření, že konfigurace vašeho účtu Automation je nastavená správně."
services: automation
documentationcenter: 
author: mgoedtel
manager: carmonm
editor: 
ms.assetid: 
ms.service: automation
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 04/14/2017
ms.author: magoedte
translationtype: Human Translation
ms.sourcegitcommit: e851a3e1b0598345dc8bfdd4341eb1dfb9f6fb5d
ms.openlocfilehash: 5bed2616e15a2e5f52e79d0c28159b568e7a1de3
ms.lasthandoff: 04/15/2017

---

# <a name="test-azure-automation-run-as-account-authentication"></a>Test ověření účtu Azure Automation Spustit jako
Po úspěšném vytvoření účtu Automation můžete provést jednoduchý test a zkontrolovat, že se jde úspěšně ověřit v nasazení Azure Resource Manager nebo Azure Classic pomocí nově vytvořeného nebo aktualizovaného účtu Automation Spustit jako.    

## <a name="automation-run-as-authentication"></a>Ověření pomocí účtu Automation Spustit jako

1. Na webu Azure Portal otevřete účet Automation, který jste vytvořili dříve.  
2. Kliknutím na dlaždici **Runbooky** otevřete seznam runbooků.
3. Vyberte runbook **AzureAutomationTutorialScript** a kliknutím na tlačítko **Spustit** runbook spusťte.  Zobrazí se výzva s dotazem, jestli chcete runbook spustit.
4. Vytvoří se [úloha runbooku](automation-runbook-execution.md), zobrazí se okno Úloha a na dlaždici **Souhrn úlohy** se zobrazí stav úlohy.  
5. Počáteční stav úlohy bude *Zařazeno ve frontě*. To označuje, že čekáte na zpřístupnění pracovního procesu runbooku v cloudu. Když pracovní proces úlohu přijme, změní se stav na *Spouštění*, a když se runbook skutečně spustí, změní se na *Spuštěno*.  
6. Po dokončení úlohy runbooku by se měla zobrazit zpráva **Dokončeno**.<br> ![Test runbooku objektu zabezpečení](media/automation-verify-runas-authentication/job-summary-automationtutorialscript.png)<br>
7. Pokud chcete zobrazit podrobné výsledky runbooku, klikněte na dlaždici **Výstup**.
8. V okně **Výstup** byste měli vidět, že se úspěšně ověřil a vrátil seznam všech prostředků, které jsou ve skupině prostředků dostupné.
9. Zavřením okna **Výstup** se vrátíte do okna **Souhrn úlohy**.
10. Zavřete **Souhrn úlohy** a odpovídající okno runbooku **AzureAutomationTutorialScript**.

## <a name="classic-run-as-authentication"></a>Ověření pomocí účtu Spustit jako pro Azure Classic
Pokud budete spravovat prostředky v modelu nasazení Classic, proveďte následující kroky a zkontrolujte, že se můžete úspěšně ověřit pomocí nového účtu Spustit jako pro Azure Classic.     

1. Na webu Azure Portal otevřete účet Automation, který jste vytvořili dříve.  
2. Kliknutím na dlaždici **Runbooky** otevřete seznam runbooků.
3. Vyberte runbook **AzureClassicAutomationTutorialScript** a kliknutím na tlačítko **Spustit** runbook spusťte.  Zobrazí se výzva s dotazem, jestli chcete runbook spustit.
4. Vytvoří se [úloha runbooku](automation-runbook-execution.md), zobrazí se okno Úloha a na dlaždici **Souhrn úlohy** se zobrazí stav úlohy.  
5. Počáteční stav úlohy bude *Zařazeno ve frontě*. To označuje, že čekáte na zpřístupnění pracovního procesu runbooku v cloudu. Když pracovní proces úlohu přijme, změní se stav na *Spouštění*, a když se runbook skutečně spustí, změní se na *Spuštěno*.  
6. Po dokončení úlohy runbooku by se měla zobrazit zpráva **Dokončeno**.<br><br> ![Test runbooku objektu zabezpečení](media/automation-verify-runas-authentication/job-summary-automationclassictutorialscript.png)<br>  
7. Pokud chcete zobrazit podrobné výsledky runbooku, klikněte na dlaždici **Výstup**.
8. V okně **Výstup** byste měli vidět, že se úspěšně ověřil a vrátil seznam všech klasických virtuálních počítačů v rámci předplatného.
9. Zavřením okna **Výstup** se vrátíte do okna **Souhrn úlohy**.
10. Zavřete **Souhrn úlohy** a odpovídající okno runbooku **AzureClassicAutomationTutorialScript**.

## <a name="next-steps"></a>Další kroky
* První kroky s powershellovými runbooky najdete v článku [Můj první powershellový runbook](automation-first-runbook-textual-powershell.md).
* Další informace o vytváření grafického obsahu najdete v článku [Vytváření grafického obsahu v Azure Automation](automation-graphical-authoring-intro.md).

