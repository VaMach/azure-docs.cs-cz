---
title: "Správa virtuálního počítače Azure se shromažďováním dat pro inventarizaci | Dokumentace Microsoftu"
description: "Správa virtuálního počítače se shromažďováním dat pro inventarizaci"
services: automation
keywords: inventory, automation, change, tracking
author: jennyhunter-msft
ms.author: jehunte
ms.date: 09/13/2017
ms.topic: hero-article
manager: carmonm
ms.translationtype: HT
ms.sourcegitcommit: c3a2462b4ce4e1410a670624bcbcec26fd51b811
ms.openlocfilehash: 5291d112c2cdf157543fe301d5a5c80f3fe561ae
ms.contentlocale: cs-cz
ms.lasthandoff: 09/25/2017

---

# <a name="manage-an-azure-virtual-machine-with-inventory-collection"></a>Správa virtuálního počítače Azure se shromažďováním dat pro inventarizaci

Sledování inventáře pro virtuální počítač Azure je možné povolit na stránce prostředků příslušného počítače. Tato metoda poskytuje uživatelské rozhraní v prohlížeči pro nastavení a konfiguraci shromažďování dat pro inventarizaci.

Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/) před tím, než začnete.
Pokud ještě nemáte virtuální počítač Azure, vytvořte si [virtuální počítač](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/quick-create-portal) před tím, než začnete.

## <a name="log-in-to-the-azure-portal"></a>Přihlášení k portálu Azure Portal

Přihlaste se k portálu [Azure Portal](https://portal.azure.com/).

## <a name="enable-inventory-collection-from-the-virtual-machine-resource-page"></a>Povolení shromažďování dat pro inventarizaci na stránce prostředků virtuálního počítače

1. Na obrazovce vlevo vyberte **Virtuální počítače**.
1. Ze seznamu vyberte virtuální počítač.
1. Z nabídky prostředků v části **Operace** vyberte **Inventarizace (Preview)**.
1. V horní části stránky se zobrazí banner s oznámením, že řešení není povoleno. Kliknutím na banner řešení povolte.
1. Vyberte pracovní prostor Log Analytics, ve kterém se budou ukládat vaše protokoly dat. Pokud v dané oblasti nemáte k dispozici žádné pracovní prostory, zobrazí se výzva k vytvoření výchozího pracovního prostoru a účtu Automation. Kliknutím na **Povolit** spusťte připojování vašeho počítače.

   ![Zobrazení možností připojení](./media/automation-vm-inventory/inventory-onboarding-options.png)  

1. Stavový řádek vás bude informovat o povolování řešení. Tento proces může trvat až 15 minut. Během této doby můžete okno zavřít nebo ho nechat otevřené a jakmile bude řešení povoleno, zobrazí se v něm oznámení. Stav nasazení můžete monitorovat v podokně oznámení.

   ![Zobrazení řešení inventarizace ihned po připojení](./media/automation-vm-inventory/inventory-onboarded.png)

1. Po dokončení nasazení stavový řádek zmizí. Systém v tuto chvíli stále shromažďuje data inventáře, ale data ještě nemusí být viditelná. Úplné shromáždění dat může trvat až 24 hodin.

## <a name="configure-your-inventory-settings"></a>Konfigurace nastavení inventarizace

Ve výchozím nastavení jsou pro shromažďování nakonfigurovány software, služby Windows a linuxové procesy démon. Pokud chcete shromažďovat data pro inventarizaci registru a souborů systému Windows, nakonfigurujte nastavení shromažďování dat pro inventarizaci.

1. V zobrazení **Inventarizace (Preview)** vyberte tlačítko **Upravit nastavení** v horní části stránky.
2. Pokud chcete přidat nové nastavení shromažďování, pomocí karet, na kterých je uvedeno **Registr systému Windows**, **Soubory Windows** a **Soubory Linuxu**, přejděte do kategorie nastavení, které chcete přidat. V horní části stránky klikněte na **Přidat**.
3. Pokud chcete zobrazit podrobné informace o všech vlastnostech nastavení a jejich popisy, přejděte na [stránku dokumentace k inventarizaci](https://aka.ms/configinventorydocs).

## <a name="disconnecting-your-virtual-machine-from-management"></a>Odpojení virtuálního počítače od správy

Odebrání počítače ze správy inventáře:

1. V nabídce vlevo na webu Azure Portal klikněte na **Log Analytics** a pak kliknutím vyberte pracovní prostor, který jste použili při připojování vašeho virtuálního počítače.
1. Na stránce Log Analytics v nabídce prostředků vyberte **Virtuální počítače** z kategorie **Zdroje dat pracovního prostoru**. 
1. Vyberte virtuální počítač, který chcete odebrat ze seznamu. Vedle textu „Tento pracovní prostor“ ve sloupci **Připojení OMS** se u něj zobrazí zelené zaškrtnutí. V horní části další stránky klikněte na **Odpojit** a v potvrzovacím dialogovém okně klikněte na **Ano**, aby se počítač odpojil od správy.

## <a name="next-steps"></a>Další kroky

* Informace o správě změn nastavení souborů a registru na virtuálních počítačích najdete v tématu věnovaném řešení [Change Tracking](../log-analytics/log-analytics-change-tracking.md).
* Informace o správě aktualizací pro Windows a balíčky na virtuálních počítačích najdete v tématu věnovaném řešení [Update Management](../operations-management-suite/oms-solution-update-management.md).

