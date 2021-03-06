---
title: "Odebrat procesy Hybrid Runbook Worker služby Azure Automation | Microsoft Docs"
description: "Tento článek obsahuje informace o správě nasazené Azure automatizace procesů Hybrid Runbook Worker, který umožňuje spuštění sad runbook na počítačích ve vašem místním prostředí datového centra nebo cloudu."
services: automation
documentationcenter: 
author: georgewallace
manager: carmonm
editor: tysonn
ms.assetid: 
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/07/2017
ms.author: magoedte
ms.openlocfilehash: 602b868073da6bd1f64099c0f344c9b492abaff0
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/01/2018
---
# <a name="remove-azure-automation-hybrid-runbook-workers"></a>Odebrání funkce Hybrid Runbook Worker služby Azure Automation

Funkce hybridní pracovní proces Runbooku automatizace Azure umožňuje spuštění sad runbook přímo na počítači, který hostuje roli a s prostředky v prostředí pro správu těchto místních prostředků. Tento článek vás provede kroky k odebrání hybridního pracovního procesu na místní počítač.

## <a name="removing-hybrid-runbook-worker"></a>Odebrání hybridní pracovní proces Runbooku

Jeden nebo více procesy Hybrid Runbook Worker můžete odebrat ze skupiny nebo můžete odebrat skupiny, v závislosti na vaše požadavky.  K odebrání hybridní pracovní proces Runbooku místním počítači, proveďte následující kroky.

1. Na portálu Azure přejděte na svůj účet Automation.  
2. Z **nastavení** vyberte **klíče** a poznamenejte si hodnoty pro pole **URL** a **primární přístupový klíč**.  Tyto informace jsou potřeba pro další krok.
3. Otevřete relaci prostředí PowerShell v režimu správce a spusťte následující příkaz: `Remove-HybridRunbookWorker -url <URL> -key <PrimaryAccessKey>`.  Použití **-Verbose** přepínač pro podrobný protokol proces odebrání.

> [!NOTE]
> Microsoft Monitoring Agent nebude odstraněn z počítače, funkce a konfiguraci role hybridní pracovní proces Runbooku.  

## <a name="remove-hybrid-worker-groups"></a>Odebrat skupiny hybridní pracovní proces

Odebrat skupinu, je nutné nejprve odebrat hybridní pracovní proces Runbooku z každý počítač, který je členem skupiny postupem uvedena výše a potom proveďte následující kroky k odebrání skupiny.  

1. Otevřete účet Automation na portálu Azure.
1. V části **automatizace procesu** vyberte **skupinám Hybrid Worker**. Vyberte skupinu, kterou chcete odstranit.  Po výběru konkrétní skupinu **skupinu hybridních pracovních procesů** zobrazí se okno Vlastnosti.<br> ![Skupina hybridních pracovních procesů Runbook okno](media/automation-hybrid-runbook-worker/automation-hybrid-runbook-worker-group-properties.png)   
2. V okně Vlastnosti pro vybranou skupinu, klikněte na **odstranit**.  Zobrazí se zpráva s žádostí o potvrzení této akce vyberte **Ano** Opravdu chcete pokračovat.<br> ![Dialogové okno potvrzení odstranění skupiny](media/automation-hybrid-runbook-worker/automation-hybrid-runbook-worker-confirm-delete.png)<br> Dokončení tohoto procesu může trvat několik sekund a průběh zpracování můžete sledovat prostřednictvím možnosti nabídky **Oznámení**. 

## <a name="next-steps"></a>Další kroky

* Zkontrolujte [spuštění sad runbook na hybridní pracovní proces Runbooku](automation-hrw-run-runbooks.md) se dozvíte, jak nakonfigurovat své sady runbook k automatizaci procesů ve své místní datové centrum nebo jiné cloudové prostředí.
* Informace o instalaci Windows hybridní pracovní procesy Runbooku najdete v tématu [Azure Automation Windows Hybrid Runbook Worker](automation-windows-hrw-install.md).
* Informace o instalaci Linux hybridní pracovní procesy Runbooku najdete v tématu [Azure Automation Linux Hybrid Runbook Worker](automation-linux-hrw-install.md).