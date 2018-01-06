---
title: "Nastavení sady Runbook | Microsoft Docs"
description: "Popisuje konfiguraci nastavení pro sady runbook v Azure Automation a jak ji pomocí portálu Azure a prostředí Windows PowerShell změnit."
services: automation
documentationcenter: 
author: georgewallace
manager: stevenka
editor: tysonn
ms.assetid: a726f20c-a952-48b8-88ee-36d76aa3ac61
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/11/2016
ms.author: bwren
ms.openlocfilehash: 527c40c4a0c5c9a0eb4745f542a8afcc63281416
ms.sourcegitcommit: 0e1c4b925c778de4924c4985504a1791b8330c71
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/06/2018
---
# <a name="runbook-settings"></a>Nastavení runbooku
Každá sada runbook ve službě Azure Automation má několik nastavení, která pomáhají identifikovat a změnit její chování při protokolování. Každá z těchto nastavení je popsána níže následují procedury na tom, jak je upravit.

## <a name="settings"></a>Nastavení
### <a name="name-and-description"></a>Název a popis
Název sady runbook nelze změnit po jeho vytvoření. Popis je volitelný a může mít až 512 znaků.

### <a name="tags"></a>Značky
Značky umožňují přiřadit různá slova a slovní spojení, která pomáhají sadu runbook identifikovat. Například když odešlete sady runbook [Galerie prostředí PowerShell](https://www.powershellgallery.com/), zadejte konkrétní značky k identifikaci kategorie, které sada runbook by měl být uvedený v. Můžete určit více značek pro sadu runbook oddělte je čárkami.

### <a name="logging"></a>Protokolování
Ve výchozím nastavení nejsou podrobné a průběh záznamy zapisují do historie úlohy. Můžete změnit nastavení pro konkrétní runbook tyto záznamy protokolu. Další informace o tyto záznamy, najdete v části [výstup a zprávy Runbooku](automation-runbook-output-and-messages.md).

## <a name="changing-runbook-settings"></a>Změna nastavení sady runbook

### <a name="changing-runbook-settings-with-the-azure-portal"></a>Změna nastavení sady runbook pomocí portálu Azure
Můžete změnit nastavení sady runbook na portálu Azure z **nastavení** okno pro sadu runbook.

1. Na portálu Azure vyberte **automatizace** a pak klikněte na název účtu automation.
2. Vyberte **Runbooky** kartě.
3. Klikněte na název sady runbook a přejdete do okna nastavení pro sadu runbook. Odsud můžete zadat nebo změnit značky, popis sady runbook, nakonfigurovat nastavení trasování a protokolování a přístup k nástrojů podpory, které pomáhají při řešení problémů.     

### <a name="changing-runbook-settings-with-windows-powershell"></a>Změna nastavení sady runbook pomocí prostředí Windows PowerShell
Můžete použít [Set-AzureRmAutomationRunbook](https://msdn.microsoft.com/library/mt603786.aspx) rutiny změnit nastavení pro sady runbook. Pokud chcete zadat více značek, můžete buď zadat pole nebo jeden řetězec s čárkami hodnoty na parametr značky. Můžete získat aktuální značky s [Get-AzureRmAutomationRunbook](https://msdn.microsoft.com/library/mt603728.aspx).

Následující vzorové příkazy ukazují, jak nastavit vlastnosti pro sady runbook. Tato ukázka přidá tři značky pro existující značky a určuje, zda mají být protokolovány podrobných záznamů.

    $automationAccountName = "MyAutomationAccount"
    $runbookName = "Sample-TestRunbook"
    $tags = (Get-AzureRmAutomationRunbook -ResourceGroupName "ResourceGroup01" `
    –AutomationAccountName $automationAccountName –Name $runbookName).Tags
    $tags += "Tag1,Tag2,Tag3"
    Set-AzureRmAutomationRunbook -ResourceGroupName "ResourceGroup01" `
    –AutomationAccountName $automationAccountName –Name $runbookName –LogVerbose $true –Tags $tags

## <a name="next-steps"></a>Další postup
* Další informace o vytvoření a ze sady runbook načíst výstupní a chybové zprávy naleznete v tématu [výstup a zprávy Runbooku](automation-runbook-output-and-messages.md) 
* Abyste pochopili, jak přidat sadu runbook, která již byla vyvinuta komunity nebo jiný zdroj, nebo vytvořte vlastní sady runbook viz [vytvoření nebo import Runbooku](automation-creating-importing-runbook.md) 

