---
title: "Služby Azure Automation Linux hybridní pracovní proces Runbooku | Microsoft Docs"
description: "Tento článek obsahuje informace o instalaci služby Azure Automation hybridní pracovní proces Runbooku umožňující spouštění sad runbook v počítačích se systémem Linux v místní datové centrum nebo cloudového prostředí."
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
ms.date: 09/07/2017
ms.author: magoedte
ms.openlocfilehash: 938e4f4fa3326db23ea4c2b499c783de78dcfa76
ms.sourcegitcommit: fa28ca091317eba4e55cef17766e72475bdd4c96
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/14/2017
---
# <a name="how-to-deploy-a-linux-hybrid-runbook-worker"></a>Postup nasazení Linux Hybrid Runbook Worker

Runbooky ve službě Azure Automation nemají přístup k prostředkům v ostatních cloudů nebo v místním prostředí, protože spustit i v cloudu Azure. Funkce hybridní pracovní proces Runbooku automatizace Azure umožňuje spuštění sad runbook přímo na počítači, který hostuje roli a s prostředky v prostředí pro správu těchto místních prostředků. Sady Runbook jsou uložené a spravované ve službě Azure Automation a pak doručí jeden nebo více určenými počítači.

Tato funkce je znázorněno na následujícím obrázku:<br>  

![Přehled hybridních služby Runbook Worker](media/automation-offering-get-started/automation-infradiagram-networkcomms.png)

Technický přehled role hybridní pracovní proces Runbooku, najdete v části [přehled architektury automatizace](automation-offering-get-started.md#automation-architecture-overview). Zkontrolujte následující informace ohledně [požadavky na hardware a software](automation-offering-get-started.md#hybrid-runbook-worker) a [informace pro přípravu síti](automation-offering-get-started.md#network-planning) než začnete nasazovat hybridní pracovní proces Runbooku.  Po úspěšném nasazení služby runbook worker, zkontrolujte [spuštění sad runbook na hybridní pracovní proces Runbooku](automation-hrw-run-runbooks.md) se dozvíte, jak nakonfigurovat své sady runbook k automatizaci procesů ve své místní datové centrum nebo jiné cloudové prostředí.     

## <a name="hybrid-runbook-worker-groups"></a>Skupinám hybrid Runbook Worker
Každý hybridní pracovní proces Runbooku je členem skupiny hybridní pracovní proces Runbooku, který zadáte při instalaci agenta.  Skupina může obsahovat jednoho agenta, ale můžete nainstalovat více agentů ve skupině pro vysokou dostupnost.

Při spuštění sady runbook pro Hybrid Runbook Worker, určete skupinu, která běží na.  Členové skupiny určit, které pracovní obsluhuje požadavek.  Nelze zadat konkrétní pracovního procesu.

## <a name="installing-linux-hybrid-runbook-worker"></a>Instalace Linux hybridní pracovní proces Runbooku
K instalaci a konfiguraci Hybrid Runbook Worker ve vašem počítači Linux, postupujte podle velmi splněny následující proces pro ručně nainstalujte a nakonfigurujte roli.   Vyžaduje povolení **Automation Hybrid Worker** řešení v vaším pracovním prostorem OMS a pak spustit sadu příkazů pro registraci počítače jako pracovní proces a přidejte ji do nové nebo existující skupiny. 

Než budete pokračovat, musíte svůj účet Automation je spojena se pracovní prostor analýzy protokolů a také primární klíč pro svůj účet Automation.  Obě z portálu můžete najít a vyberete svůj účet Automation a vyberete **prostoru** pro dané ID pracovního prostoru a výběr **klíče** pro primární klíč.  

1.  Povolte řešení "Automation Hybrid Worker" v OMS. To můžete provést buď:

   1. Z Galerie řešení v [portálu OMS](https://mms.microsoft.com) povolit **Automation Hybrid Worker** řešení
   2. Spusťte následující rutinu:

        ```powershell
         $null = Set-AzureRmOperationalInsightsIntelligencePack -ResourceGroupName  <ResourceGroupName> -WorkspaceName <WorkspaceName> -IntelligencePackName  "AzureAutomation" -Enabled $true
        ```

2.  Spusťte následující příkaz, změna hodnot pro parametry *-w*, *-k*, *-g*, a *-e*. Pro *-g* parametr nahraďte název skupiny hybridní pracovní proces Runbooku, které se musí připojit nový Linux Hybrid Runbook Worker hodnotu. Pokud název již neexistuje v účtu Automation, nové skupiny Hybrid Runbook Worker budou provedeny s tímto názvem.
    
    ```
    sudo python /opt/microsoft/omsconfig/modules/nxOMSAutomationWorker/DSCResources/MSFT_nxOMSAutomationWorkerResource/automationworker/scripts/onboarding.py --register -w <OMSworkspaceId> -k <AutomationSharedKey> -g <hybridgroupname> -e <automationendpoint>
    ```
3. Po dokončení příkazu v okně skupinám Hybrid Worker na portálu Azure se zobrazí nové skupiny a počet členů, nebo pokud existující skupiny, se zvýší počet členů.  Můžete vybrat skupinu ze seznamu na **skupinám Hybrid Worker** a vyberte **hybridní pracovní procesy** dlaždici.  Na **hybridní pracovní procesy** okně uvidíte každého člena skupiny uvedené.  


## <a name="turning-off-signature-validation"></a>Vypnutí ověřování podpisu 
Ve výchozím nastavení Linux hybridní pracovní procesy Runbooku vyžadovat ověření podpisu. Pokud spustíte nepodepsaný runbook proti pracovní, zobrazí se chyba obsahující "Podpis ověření se nezdařilo". Chcete-li vypnout ověřování podpisu, spusťte následující příkaz, nahraďte druhý parametr vaše ID pracovního prostoru OMS:

    ```
    sudo python /opt/microsoft/omsconfig/modules/nxOMSAutomationWorker/DSCResources/MSFT_nxOMSAutomationWorkerResource/automationworker/scripts/require_runbook_signature.py --false <OMSworkspaceId>
    ```
   
## <a name="next-steps"></a>Další kroky

* Zkontrolujte [spuštění sad runbook na hybridní pracovní proces Runbooku](automation-hrw-run-runbooks.md) se dozvíte, jak nakonfigurovat své sady runbook k automatizaci procesů ve své místní datové centrum nebo jiné cloudové prostředí.
* Pokyny o tom, jak odebrat procesy Hybrid Runbook Worker najdete v tématu [odebrat Azure automatizace procesů Hybrid Runbook Worker](automation-remove-hrw.md)
