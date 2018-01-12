---
title: "Zařadit aktualizace a změna sledování řešení pro Azure Automation | Microsoft Docs"
description: "Zjistěte, jak připojit aktualizace a sledování řešení pro Azure Automation změn."
services: automation
documentationcenter: 
author: eamonoreilly
manager: 
editor: 
ms.assetid: edae1156-2dc7-4dab-9e5c-bf253d3971d0
ms.service: automation
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/11/2017
ms.author: eamono
ms.custom: mvc
ms.openlocfilehash: e277aa44dfe625780d72a78010f0638c73a6b182
ms.sourcegitcommit: 9292e15fc80cc9df3e62731bafdcb0bb98c256e1
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/10/2018
---
# <a name="onboard-update-and-change-tracking-solutions-to-azure-automation"></a>Zařadit aktualizace a změna sledování řešení Azure Automation.

V tomto kurzu zjistíte, jak automaticky zařadit aktualizace, sledování změn a inventáře řešení pro virtuální počítače Azure Automation:

> [!div class="checklist"]
> * Připojit virtuální počítač Azure
> * Povolit řešení
> * Instalace a aktualizace moduly
> * Importovat runbook registrace
> * Spuštění runbooku

## <a name="prerequisites"></a>Požadavky

K dokončení tohoto kurzu, je potřeba splnit následující nástroje:

* Předplatné Azure. Pokud ještě žádné nemáte, můžete si [aktivovat výhody pro předplatitele MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) nebo si zaregistrovat [bezplatný účet](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
* [Účet Automation](automation-offering-get-started.md) ke správě počítače.
* [Virtuální počítač](../virtual-machines/windows/quick-create-portal.md) pro připojení.

## <a name="onboard-an-azure-vm"></a>Připojit virtuální počítač Azure

Na základní desce Azure Virtual Machines automaticky, stávajícího virtuálního počítače musí být zařazený, nemá s sledování změn nebo řešení pro správu aktualizací. V tomto kroku, můžete připojit virtuální počítač s správy aktualizací a sledování změn.

### <a name="enable-change-tracking-and-inventory"></a>Povolit sledování změn a inventáře

Sledování změn a inventáře řešení poskytuje schopnost [sledování změn](automation-vm-change-tracking.md) a [inventáře](automation-vm-inventory.md) na virtuálních počítačích. V tomto kroku povolíte řešení na virtuálním počítači.

1. V nabídce vlevo vyberte **účty Automation**a pak v seznamu vyberte svůj účet automation.
1. Vyberte **inventáře** pod **správy konfigurace**.
1. Vyberte existující pracovní prostor analýzy protokolů nebo vytvořit nový. Klikněte **povolit** tlačítko.

![Zařadit řešení aktualizace](media/automation-onboard-solutions/inventory-onboard.png)

Po dokončení inventáře a sledování řešení registrace oznámení o změně, klikněte na **správy aktualizací** pod **CONFIGURATION MANAGEMENT**.

### <a name="enable-update-management"></a>Povolení správy aktualizací

Řešení pro správu aktualizací můžete spravovat aktualizace a opravy pro virtuální počítače Windows Azure. Můžete vyhodnotit stav dostupné aktualizace, naplánovat instalaci požadovaných aktualizací, a zobrazit výsledky nasazení aktualizace byly úspěšně použity k virtuálnímu počítači. V tomto kroku povolíte řešení pro virtuální počítač.

1. Z vašeho účtu Automation vyberte **Správa aktualizací** pod **správy aktualizací**.
1. Vybraný pracovní prostor Log analytics je ve stejném pracovním prostoru použité v předchozím kroku. Klikněte na tlačítko **povolit** se budou registrovat do řešení pro správu aktualizací.

![Zařadit řešení aktualizace](media/automation-onboard-solutions/update-onboard.png)

Řešení pro správu aktualizací je během instalace, se zobrazí nápis informující o blue. Pokud je povoleno řešení vyberte **sledování změn** pod **CONFIGURATION MANAGEMENT** přejít k dalšímu kroku.

### <a name="select-azure-vm-to-be-managed"></a>Vyberte virtuální počítač Azure ke správě

Teď, když jsou povolena řešení, můžete přidat virtuální počítač Azure na těchto řešení zařadit do provozu.

1. Z vašeho účtu Automation na **sledování změn** vyberte **+ přidat virtuální počítač Azure** pro přidání virtuálního počítače.

1. Vyberte virtuální počítač ze seznamu a vyberte **povolit**. Tato akce umožňuje změnu sledovacích a inventáře řešení pro virtuální počítač.

   ![Povolit aktualizace řešení pro virtuální počítač](media/automation-onboard-solutions/enable-change-tracking.png)

1. Po registraci oznámení virtuálního počítače dokončení z vašeho účtu Automation vyberte **Správa aktualizací** pod **správy aktualizací**.

1. Vyberte **+ přidat virtuální počítač Azure** pro přidání virtuálního počítače.

1. Vyberte virtuální počítač ze seznamu a vyberte **povolit**. Tato akce umožňuje řešení pro správu aktualizací pro virtuální počítač.

   ![Povolit aktualizace řešení pro virtuální počítač](media/automation-onboard-solutions/enable-update.png)

> [!NOTE]
> Pokud není počkat na dokončení při povolování další řešení řešení se zobrazí zpráva s oznámením: *na tento nebo jiný virtuální počítač probíhá instalace jiné řešení. Po dokončení této instalace povolit tlačítko je k dispozici a můžete požadovat instalace řešení na tomto virtuálním počítači.*

## <a name="install-and-update-modules"></a>Instalace a aktualizace moduly

Je potřeba aktualizovat na nejnovější moduly Azure a import `AzureRM.OperationalInsights` úspěšně automatizovat registrace řešení.

## <a name="update-azure-modules"></a>Aktualizovat Azure moduly

Z vašeho účtu Automation vyberte **moduly** pod **SDÍLENÉ prostředky**. Vyberte **aktualizovat moduly Azure** aktualizovat na nejnovější verzi Azure moduly. Vyberte **Ano** na výzva k aktualizaci všech existujících Azure modulů na nejnovější verzi.

![Aktualizovat moduly](media/automation-onboard-solutions/update-modules.png)

### <a name="install-azurermoperationalinsights-module"></a>Instalace modulu AzureRM.OperationalInsights

Z **moduly** vyberte **procházet galerii** otevřete v galerii modulů. Vyhledejte AzureRM.OperationalInsights a importovat tento modul do účtu Automation.

![Import modulu OperationalInsights](media/automation-onboard-solutions/import-operational-insights-module.png)

## <a name="import-the-onboarding-runbook"></a>Importovat runbook registrace

1. Vyberte svůj účet Automation na **sady Runbook** pod **AUTOMATIZACE procesu**.
1. Vyberte **procházet galerii**.
1. Vyhledejte **aktualizovat a sledování změn**, klikněte na tlačítko runbook a vyberte **Import** na **zobrazit zdroj** stránky. Vyberte **OK** k importu sady runbook do účtu Automation.

  ![Importovat runbook registrace](media/automation-onboard-solutions/import-from-gallery.png)

1. Na **Runbook** vyberte **upravit**, pak vyberte **publikovat**. Na **publikovat Runbook** dialogovém okně, vyberte **Ano** Publikovat sadu runbook.

## <a name="start-the-runbook"></a>Spuštění runbooku

Musíte mít zařazený nemá sledování změn nebo aktualizovat řešení na virtuální počítač Azure ke spuštění této sady runbook. Vyžaduje existující virtuální počítač a skupině prostředků s zařazený nemá řešení pro parametry.

1. Otevřete sadu runbook povolit MultipleSolution.

   ![Několik sad runbook řešení](media/automation-onboard-solutions/runbook-overview.png)

1. Klikněte na tlačítko start a zadejte následující hodnoty pro parametry.

   * **VMNAME** -ponechat prázdné. Název existující virtuální počítač mohl připojit k aktualizaci nebo řešení pro sledování změn. Protože tato hodnota prázdná, jsou všechny virtuální počítače ve skupině prostředků zařazený nemá.
   * **VMRESOURCEGROUP** -název skupiny prostředků pro virtuální počítače jako zařazený nemá.
   * **ID PŘEDPLATNÉHO** -ponechat prázdné. ID odběru nový virtuální počítač jako zařazený nemá. Pokud prázdné, použije se předplatné pracovního prostoru. Když je zadané ID jiného předplatného, účet Spustit jako pro tento účet služby automation by přidat také jako Přispěvatel pro toto předplatné.
   * **ALREADYONBOARDEDVM** -název virtuálního počítače, který byl ručně zařazený, nemá aktualizace nebo vlastnost ChangeTracking řešení.
   * **ALREADYONBOARDEDVMRESOURCEGROUP** -název skupiny prostředků, který virtuální počítač je členem skupiny.
   * **SOLUTIONTYPE** -zadejte **aktualizace** nebo **vlastnost ChangeTracking**

   ![Povolit MultipleSolution parametry runbooku](media/automation-onboard-solutions/runbook-parameters.png)

1. Vyberte **OK** při spuštění úlohy sady runbook.
1. Monitorovat průběh a všechny chyby na stránce úlohy sady runbook.

## <a name="next-steps"></a>Další postup

V tomto kurzu jste se naučili:

> [!div class="checklist"]
> * Připojit virtuální počítač Azure ručně.
> * Instalace a aktualizace požadované moduly Azure.
> * Importujte sady runbook této onboards virtuálních počítačích Azure.
> * Spuštění runbooku této onboards virtuálních počítačích Azure automaticky.

Další informace o plánování sad runbook na tento odkaz.

> [!div class="nextstepaction"]
> [Plánování sad runbook](automation-schedules.md).
