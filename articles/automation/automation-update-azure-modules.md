---
title: "Aktualizovat Azure modulů ve službě Azure Automation | Microsoft Docs"
description: "Tento článek popisuje, jak můžete nyní aktualizovat společnými moduly prostředí Azure PowerShell, které jsou dostupné ve výchozím nastavení ve službě Azure Automation."
services: automation
documentationcenter: 
author: eslesar
manager: carmonm
editor: tysonn
ms.assetid: 
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/13/2017
ms.author: magoedte
ms.openlocfilehash: 6bd259f3da1005228b8137415c30660221507909
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-update-azure-powershell-modules-in-azure-automation"></a>Postup aktualizace modulů prostředí Azure PowerShell ve službě Azure Automation

Nejběžnější modulů prostředí Azure PowerShell jsou dostupné ve výchozím nastavení v jednotlivých účtů Automation.  Tým služby Azure aktualizuje Azure moduly pravidelně, tak v účtu Automation jsme poskytují způsob, jak můžete aktualizovat moduly v účtu, jakmile je nová verze dostupná z portálu.  

Protože moduly se pravidelně aktualizují v product group, změny můžou nastat pomocí rutiny zahrnuté, což může mít negativní vliv na vaše sady runbook v závislosti na typu změnu, jako je například přejmenování parametr nebo zcela místo začne rutiny. Nechcete-li vaše sady runbook a procesy, které budou automatizovat, které mají vliv, doporučujeme testování a ověření než budete pokračovat.  Pokud nemáte určené pro tento účel vyhrazený účet Automation, zvažte vytvoření jednoho tak, aby při vývoji vaší sady runbook, kromě iterativní změny jako aktualizace moduly Powershellu můžete otestovat mnoha různých scénářů a permutací.  Po ověření výsledky a jste použili potřebné změny, pokračovat v migraci žádné sady runbook, který vyžaduje úpravu koordinace a provést aktualizaci, jak je popsáno níže v produkčním prostředí.     

## <a name="updating-azure-modules"></a>Aktualizace Azure moduly

1. V modulech je okno účtu Automation existuje možnost **moduly Azure aktualizace**.  Je vždy povolena.<br><br> ![Aktualizovat možnost moduly Azure v okně moduly](media/automation-update-azure-modules/automation-update-azure-modules-option.png)

2. Klikněte na tlačítko **moduly Azure aktualizace** a zobrazí se oznámení o potvrzení, která požaduje, pokud chcete pokračovat.<br><br> ![Moduly Azure oznámení o aktualizaci](media/automation-update-azure-modules/automation-update-azure-modules-popup.png)

3. Klikněte na tlačítko **Ano** a zahájí proces aktualizace modulu.  Proces aktualizace trvá asi 15-20 minut aktualizovat následující moduly:

  * Azure
  * Azure.Storage
  * AzureRm.Automation
  * AzureRm.Compute
  * AzureRm.Profile
  * AzureRm.Resources
  * AzureRm.Sql
  * AzureRm.Storage

    Pokud moduly jsou již aktuální, proces dokončí za několik sekund.  Po dokončení procesu aktualizace budete informováni.<br><br> ![Aktualizovat stav aktualizace moduly Azure](media/automation-update-azure-modules/automation-update-azure-modules-updatestatus.png)

> [!NOTE]
> Služby Azure Automation bude používat nejnovější modulů ve vašem účtu Automation, při spuštění novou naplánovanou úlohu.    

Pokud používáte rutiny z těchto modulů prostředí Azure PowerShell ve vašich sadách runbook ke správě prostředků Azure, je třeba provést proces aktualizace každý měsíc, nebo proto, aby zajistil, že máte nejnovější moduly.

## <a name="next-steps"></a>Další kroky

* Další informace o integrační moduly a jak vytvořit vlastní moduly k další integraci automatizace s jinými systémy, služby nebo řešení, najdete v části [moduly integrace](automation-integration-modules.md).

* Zvažte použití integrace zdroj ovládacího prvku [Githubu Enterprise](automation-scenario-source-control-integration-with-github-ent.md) nebo [Visual Studio Team Services](automation-scenario-source-control-integration-with-vsts.md) centrálně spravovat a řídit verzích portfolio automatizace sady runbook a konfigurace.  