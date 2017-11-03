---
title: "Sledování změn ve virtuálních počítačích Azure | Microsoft Docs"
description: "Použijte ke sledování změn v souborech a registru na virtuálních počítačích sledování změn."
services: automation
documentationcenter: automation
author: eslesar
manager: carmonm
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: automation
ms.devlang: 
ms.topic: article
ms.tgt_pltfrm: 
ms.workload: infrastructure
ms.date: 09/25/2017
ms.author: eslesar
ms.custom: 
ms.openlocfilehash: 5c6e8390ec8533fc7ab281c212e47a6982b30f1a
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="track-changes-in-your-azure-virtual-machines"></a>Sledovat změny v Azure virtuální počítače

Pokud povolíte sledování změn, můžete sledovat změny k souborům a klíčům registru systému Windows na virtuálních počítačích. Identifikace změny konfigurace vám mohou pomoci přesně určit provozní problémy.

Můžete povolit sledování přímo z virtuálního počítače Azure změn.

Pokud virtuální počítač Azure nemáte, můžete vytvořit pomocí následujících pokynů [rychlé spuštění Windows](../virtual-machines/windows/quick-create-portal.md) nebo [rychlý start Linux](../virtual-machines/linux/quick-create-portal.md) článku.

## <a name="sign-in-to-the-azure-portal"></a>Přihlášení k webu Azure Portal
Přihlaste se k webu [Azure Portal](https://portal.azure.com/).

## <a name="enable-change-tracking-for-an-azure-virtual-machine"></a>Povolit sledování změn pro virtuální počítač Azure

1. V levém podokně webu Azure Portal vyberte **Virtuální počítače**.
2. V seznamu vyberte virtuální počítač.
3. V virtuální počítače okno, v části **operace**, vyberte **sledování změn**. 

   ![Virtuální počítač zařadit sledování změn](./media/automation-vm-change-tracking/change-onboard-vm-blade.png)  
    **Povolit správu aktualizace** otevře se okno.

    Chcete-li zjistit, zda je povoleno sledování změn pro tento virtuální počítač se provádí ověřování. Pokud není povoleno sledování změn, zobrazí se banner, která poskytuje možnost povolit řešení.

   ![Hlavička zařadit konfigurace sledování změn](./media/automation-vm-change-tracking/change-onboard-banner.png)

4. Pokud chcete řešení povolit, klikněte na banner. Pokud nemáte následující položky, se přidávají automaticky:

   * [Analýza protokolu](../log-analytics/log-analytics-overview.md) prostoru
   * [Automatizace](../automation/automation-offering-get-started.md) účtu

5. Vyberte analýzy protokolů pracovní místa pro uložení protokoly s daty z sledování změn vybrat účet Automation sledování změn, a potom vyberte **povolit**.  
    Stavový řádek vás bude informovat o povolování řešení. Tento proces může trvat až 15 minut.

## <a name="configure-change-tracking"></a>Konfigurace sledování změn

Jakmile je povoleno sledování změn, **sledování změn** se zobrazí v okně. 

Chcete-li zvolit, které soubory a klíče registru pro sledování, vyberte **upravit nastavení**.

   ![Upravit nastavení sledování změn](./media/automation-vm-change-tracking/change-edit-settings.png)

   **Prostoru konfigurace** otevře se okno. 

V **prostoru konfigurace** okně Přidání klíče registru systému Windows, soubory systému Windows nebo Linux soubory sledovat, jak je uvedeno v následujících třech částech.

### <a name="add-a-windows-registry-key"></a>Přidání klíče registru Windows

1. Na **registru systému Windows** vyberte **přidat**.  
    **Přidat registru systému Windows pro sledování změn** otevře se okno.

   ![Sledování změn přidat registru](./media/automation-vm-change-tracking/change-add-registry.png)

2. V části **povoleno**, vyberte **True**.
3. V **název položky** zadejte popisný název.
4. (Volitelné) V **skupiny** zadejte název skupiny.
5. V **klíče registru Windows** pole, zadejte název klíče registru, kterou chcete sledovat.
6. Vyberte **Uložit**.

### <a name="add-a-windows-file"></a>Přidání souboru systému Windows

1. Na **soubory Windows** vyberte **přidat**.  
    **Přidat soubor systému Windows pro sledování změn** otevře se okno.

   ![Sledování změn přidat soubor systému Windows](./media/automation-vm-change-tracking/change-add-win-file.png)

2. V části **povoleno**, vyberte **True**.
3. V **název položky** zadejte popisný název.
4. (Volitelné) V **skupiny** zadejte název skupiny.
5. V **zadejte cestu** zadejte úplnou cestu a název souboru, kterou chcete sledovat.
6. Vyberte **Uložit**.

### <a name="add-a-linux-file"></a>Přidá soubor Linux

1. Na **Linux soubory** vyberte **přidat**.  
    **Přidat Linux souboru pro sledování změn** otevře se okno.

   ![Sledování změn přidat soubor Linux](./media/automation-vm-change-tracking/change-add-linux-file.png)

2. V části **povoleno**, vyberte **True**.
3. V **název položky** zadejte popisný název.
4. (Volitelné) V **skupiny** zadejte název skupiny.
5. V **zadejte cestu** zadejte úplnou cestu a název souboru, kterou chcete sledovat.
6. V **typ cesty** vyberte buď **soubor** nebo **Directory**.
7. V části **rekurze**, abyste mohli sledovat změny pro zadanou cestu a všechny soubory a cesty v něm, vyberte **na**. Pokud chcete sledovat pouze vybrané cesty nebo soubor, vyberte **vypnout**.
8. V části **použití Sudo**, sledovat soubory, které vyžadují `sudo` přístup, vyberte příkaz **na**. Jinak vyberte možnost **vypnout**.
9. Vyberte **Uložit**.

## <a name="view-changes"></a>Zobrazení změn

V **sledování změn** okně uvidíte změny v různých kategoriích pro virtuální počítač v čase.

   ![Sledování změn přidat zobrazení změn](./media/automation-vm-change-tracking/change-view-changes.png)

Můžete vybrat kategorií a časové rozmezí změny k zobrazení. V horní části okna můžete zobrazit grafické reprezentace změn v průběhu času. V dolní části okna můžete zobrazit seznam posledních změn.

## <a name="view-change-tracking-log-data"></a>Zobrazení dat sledování změn protokolu

Sledování změn generuje data protokolu, který je odeslán k analýze protokolů. Vyhledat v protokolech spuštěním dotazy, vyberte **analýzy protokolů** v horní části **sledování změn** okno.

   ![Změna sledování analýzy protokolů](./media/automation-vm-change-tracking/change-log-analytics.png)

Další informace o spuštění a hledání souborů protokolů v analýzy protokolů najdete v tématu [analýzy protokolů](../log-analytics/log-analytics-overview.md).

## <a name="next-steps"></a>Další kroky

* Další informace o sledování změn, najdete v části [sledovat změny softwaru ve vašem prostředí s řešením změnit sledování](../log-analytics/log-analytics-change-tracking.md).
* Další informace o správě aktualizací pro virtuální počítače, v tématu [řešení správy aktualizací v OMS](../operations-management-suite/oms-solution-update-management.md).
