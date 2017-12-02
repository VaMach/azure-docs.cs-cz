---
title: "Integrace služby Azure Automation s událostí mřížky | Microsoft Docs"
description: "Zjistěte, jak se automaticky při vytvoření nového virtuálního počítače přidat značku a odeslat oznámení Teams společnosti Microsoft."
keywords: "automatizace sady runbook, týmů, události mřížky, virtuálního počítače,"
services: automation
documentationcenter: 
author: eamonoreilly
manager: 
editor: 
ms.assetid: 0dd95270-761f-448e-af48-c8b1e82cd821
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/28/2017
ms.author: eamono
ms.openlocfilehash: 6798f98755ad1d70d316b074643700f7b3e25ee7
ms.sourcegitcommit: 80eb8523913fc7c5f876ab9afde506f39d17b5a1
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/02/2017
---
# <a name="integrating-azure-automation-with-event-grid-and-microsoft-teams"></a>Integrace služby Azure Automation s událostí mřížky a týmy Microsoftu

V tomto kurzu se naučíte:

> [!div class="checklist"]
> * Importujte událostí mřížky ukázkové sady runbook.
> * Vytvořte webhook volitelné týmy.
> * Vytvoření webhooku pro sadu runbook.
> * Vytvořte odběr událostí mřížky.
> * Vytvořte virtuální počítač, který aktivuje sady runbook.

## <a name="prerequisites"></a>Požadavky

K dokončení tohoto kurzu, je potřeba splnit následující nástroje.
+ [Účet Automation](../automation/automation-offering-get-started.md) pro uložení sady runbook, která se aktivuje z předplatného mřížky události.

## <a name="import-event-grid-sample-runbook"></a>Import událostí mřížky ukázkové sady runbook
1.  Otevřete účet Automation a klikněte na stránce sady Runbook.
2.  Klikněte na tlačítko "Procházet galerii".
![Seznam Runbook z uživatelského rozhraní](media/ensure-tags-exists-on-new-virtual-machines/event-grid-runbook-list.png)
3.  Vyhledejte "Události mřížky" a importovat sady runbook do účtu Automation.
![Importovat Galerie sady runbook](media/ensure-tags-exists-on-new-virtual-machines/gallery-event-grid.png)
4.  Klikněte na "Upravit" zobrazení zdroje sady Runbook a klikněte na tlačítko "Publikovat".
![Publikovat runbook z uživatelského rozhraní](media/ensure-tags-exists-on-new-virtual-machines/publish-runbook.png)

## <a name="create-an-optional-teams-webhook"></a>Vytvoření webhook volitelné týmy
1.  V Teams společnosti Microsoft zvolte Další možnosti (...) vedle názvu kanál a zvolte konektory.
![Týmy připojení](media/ensure-tags-exists-on-new-virtual-machines/teams-webhook.png)
2.  Vyhledejte v seznamu konektory pro příchozí Webhooku a klikněte na tlačítko Přidat.
![Týmy webhooku připojení](media/ensure-tags-exists-on-new-virtual-machines/select-teams-webhook.png)
3.  Zadejte AzureAutomationIntegration pro název a klikněte na tlačítko vytvořit.
![Týmy webhooku](media/ensure-tags-exists-on-new-virtual-machines/configure-teams-webhook.png)
4.  Webhook zkopírujte do schránky a uložte jej. Adresa URL webhooku se používá k odesílání informací o Teams společnosti Microsoft.
5.  Vyberte Hotovo, uložíte webhooku.

## <a name="create-a-webhook-for-the-runbook"></a>Vytvoření webhooku pro sadu runbook
1.  Otevřete runbook VMWrite sledovat.
2.  Klikněte na tlačítko webhooku přidat a Webhooky ![vytvoření webhooku](media/ensure-tags-exists-on-new-virtual-machines/add-webhook.png)
2.  Jako název zadejte "WatchVMEventGrid" a zkopírujte adresu URL do schránky a uložit.
![Nakonfigurujte název webhooku](media/ensure-tags-exists-on-new-virtual-machines/configure-webhook-name.png)
3.  Vyberte parametry a zadejte adresu URL webhooku Teams společnosti Microsoft a WEBHOOKDATA ponechat prázdné.
![Konfigurovat parametry webhooku](media/ensure-tags-exists-on-new-virtual-machines/configure-webhook-parameters.png)
4.  Vyberte OK, abyste vytvořili webhook automatizace sady runbook.

## <a name="create-an-event-grid-subscription"></a>Vytvořit odběr událostí mřížky
1.  Klikněte na stránku mřížky událostí z přehled účtu Automation.
![Seznam událostí mřížky](media/ensure-tags-exists-on-new-virtual-machines/event-grid-list.png)
2.  Klikněte na tlačítko Nový odběr událostí.
3.  Konfigurace odběru s následujícími informacemi:
    *   Zadejte název AzureAutomation. 
    *   V typu tématu zvolte předplatných Azure.
    *   Zrušte zaškrtnutí políčka "přihlásit k odběru pro všechny typy událostí"
    *   Typy událostí zvolte úspěch zápisu prostředků.
    *   V koncovém bodu odběratele zadejte adresu URL Webhooku pro sadu runbook VMWrite sledovat.
    *   Ve filtru předpony zadejte předplatné a skupina prostředků, kterou chcete hledat vytvořit nové virtuální počítače. By měl vypadat jako /subscriptions/124aa551-849d-46e4-a6dc-0bc4895422aB/resourcegroups/ContosoResourceGroup/providers/Microsoft.Compute/virtualMachines ![seznam událostí mřížky](media/ensure-tags-exists-on-new-virtual-machines/configure-event-grid-subscription.png)
6.  Klikněte na tlačítko "Vytvořit" se uložení odběru událostí mřížky.

## <a name="create-vm-that-triggers-runbook"></a>Vytvoření virtuálního počítače, který aktivuje sady runbook
1.  Vytvoření nového virtuálního počítače ve skupině prostředků, které jste zadali v případě, že předplatné mřížky předpony filtru.
2.  Kukátko – VMWrite runbook by měla být volána, novou značku přidat do virtuálního počítače.
![VMTag](media/ensure-tags-exists-on-new-virtual-machines/vm-tag.png)
3.  Nové zprávy se odesílá do kanálu týmy.

![Týmy oznámení](media/ensure-tags-exists-on-new-virtual-machines/teams-vm-message.png)

## <a name="next-steps"></a>Další kroky
V tomto kurzu nastavíte integraci mezi událostí mřížky a automatizace. Naučili jste se tyto postupy:

> [!div class="checklist"]
> * Importujte událostí mřížky ukázkové sady runbook.
> * Vytvořte webhook volitelné týmy.
> * Vytvoření webhooku pro sadu runbook.
> * Vytvořte odběr událostí mřížky.
> * Vytvořte virtuální počítač, který aktivuje sady runbook.

> [!div class="nextstepaction"]
> [Vytvoření a trasy vlastních událostí s událostí mřížky](../event-grid/custom-event-quickstart.md)
