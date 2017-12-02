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
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/30/2017
ms.author: eamono
ms.openlocfilehash: 9ae5e9ebcbcf3af61318eac98defc6b249417f41
ms.sourcegitcommit: 80eb8523913fc7c5f876ab9afde506f39d17b5a1
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/02/2017
---
# <a name="onboard-update-and-change-tracking-solutions-to-azure-automation"></a>Zařadit aktualizace a změna sledování řešení Azure Automation.

V tomto kurzu zjistíte, jak automaticky zařadit aktualizace, sledování změn a inventáře řešení pro virtuální počítače Azure Automation:

> [!div class="checklist"]
> * Připojit virtuální počítač Azure ručně.
> * Instalace a aktualizace požadované moduly Azure.
> * Importujte sady runbook této onboards virtuálních počítačích Azure.
> * Spuštění runbooku této onboards virtuálních počítačích Azure automaticky.

## <a name="prerequisites"></a>Požadavky

K dokončení tohoto kurzu, je potřeba splnit následující nástroje.
+ Předplatné Azure. Pokud ještě žádné nemáte, můžete si [aktivovat výhody pro předplatitele MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) nebo si zaregistrovat [bezplatný účet](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
+ [Účet Automation](automation-offering-get-started.md) ke správě počítače.

## <a name="onboard-an-azure-virtual-machine-manually"></a>Připojit virtuální počítač Azure ručně

1.  Otevřete účet Automation.
2.  Klikněte na stránce inventáře.
![Zařadit řešení inventáře](media/automation-onboard-solutions/inventory-onboard.png)
3.  Vyberte existující pracovní prostor analýzy protokolů nebo vytvořit nový. Klikněte na tlačítko Povolit.
4.  Po dokončení inventáře a sledování řešení registrace oznámení o změně, klikněte na stránce Správa aktualizací.
![Zařadit řešení aktualizace](media/automation-onboard-solutions/update-onboard.png)
4.  Klikněte na tlačítko Povolit tento onboards řešení aktualizací.
5.  Po dokončení registrace oznámení o aktualizaci řešení a potom klikněte na stránce Sledování změn.
![Využití sledování změn](media/automation-onboard-solutions/change-tracking-onboard-vm.png)
6.  Klikněte na tlačítko Přidat virtuální počítač Azure.
![Vyberte virtuální počítač pro sledování změn](media/automation-onboard-solutions/enable-change-tracking.png)
7.  Vyberte virtuální počítač Azure a klikněte na tlačítko Povolit mohl připojit k řešení inventáře a sledování změn.
8.  Po dokončení registrace oznámení virtuálního počítače, klikněte na stránce Správa aktualizací.
![Zařadit virtuálních počítačů pro správu aktualizací](media/automation-onboard-solutions/update-onboard-vm.png)
9.  Klikněte na tlačítko Přidat virtuální počítač Azure.
![Vyberte virtuální počítač pro správu aktualizací](media/automation-onboard-solutions/enable-update.png)
10.  Vyberte virtuální počítač Azure a klikněte na tlačítko Povolit mohl připojit k řešení pro správu aktualizací.

## <a name="install-and-update-required-azure-modules"></a>Instalace a aktualizace požadované moduly Azure

Je potřeba aktualizovat na nejnovější moduly Azure a import AzureRM.OperationalInsights úspěšně automatizovat registrace řešení.
1.  Klikněte na stránce modulů.
![Aktualizovat moduly](media/automation-onboard-solutions/update-modules.png)
2.  Klikněte na tlačítko moduly Azure aktualizace, která aktualizuje na nejnovější verzi. Počkejte na dokončení aktualizace.
3.  Klikněte na tlačítko Procházet galerii otevře v galerii modulů.
![Import modulu OperationalInsights](media/automation-onboard-solutions/import-operational-insights-module.png)
4.  Vyhledejte AzureRM.OperationalInsights a importovat tento modul do účtu Automation.

## <a name="import-a-runbook-that-onboards-solutions-to-azure-vms"></a>Import sady runbook této onboards řešení na virtuálních počítačích Azure

1.  Klikněte na stránce sady Runbook v kategorii "AUTOMATIZACE PROCESŮ".
2.  Klikněte na tlačítko "Procházet galerii".
3.  Vyhledejte "aktualizace a sledování změn" a importovat sady runbook do účtu Automation.
![Importovat runbook registrace](media/automation-onboard-solutions/import-from-gallery.png)
4.  Klikněte na "Upravit" zobrazení zdroje sady Runbook a klikněte na tlačítko "Publikovat".
![Importovat runbook registrace](media/automation-onboard-solutions/publish-runbook.png)

## <a name="start-the-runbook-that-onboards-azure-vms-automatically"></a>Spuštění sady runbook této onboards virtuálních počítačích Azure automaticky

Musíte mít zařazený nemá sledování změn nebo aktualizovat řešení na virtuální počítač Azure ke spuštění této sady runbook. Vyžaduje existující virtuální počítač a skupině prostředků s zařazený nemá řešení pro parametry.
1.  Otevřete sadu runbook povolit MultipleSolution.
![Několik sad runbook řešení](media/automation-onboard-solutions/runbook-overview.png)
2.  Klikněte na tlačítko start a zadejte následující hodnoty pro parametry.
    *   VMNAME. Název existující virtuální počítač mohl připojit k aktualizaci nebo řešení pro sledování změn. Ponechte prázdné a všechny virtuální počítače ve skupině prostředků jsou zařazený nemá.
    *   VMRESOURCEGROUP. Název skupiny prostředků, kterou virtuální počítač je členem skupiny.
    *   ID ODBĚRU. ID předplatného zařadit do nového virtuálního počítače se nachází. Ponechat prázdné a používá se předplatné pracovního prostoru. Když je zadané ID jiného předplatného, účet Spustit jako pro tento účet služby automation by přidat také jako Přispěvatel pro toto předplatné.
    *   ALREADYONBOARDEDVM. Název virtuálního počítače, který byl ručně zařazený, nemá aktualizace nebo vlastnost ChangeTracking řešení.
    *   ALREADYONBOARDEDVMRESOURCEGROUP. Název skupiny prostředků, kterou virtuální počítač je členem skupiny.
    *   SOLUTIONTYPE. Zadejte "Aktualizace" nebo "Vlastnost ChangeTracking"
    
    ![Parametry sady runbook více řešení](media/automation-onboard-solutions/runbook-parameters.png)
3.  Kliknutím na OK spustíte úlohu runbook.
4.  Monitorovat průběh a všechny chyby na stránce úlohy sady runbook.

## <a name="next-steps"></a>Další kroky

Další informace najdete v tématu [plánování sad runbook](automation-schedules.md).