---
title: "Povolit replikaci technologie Hyper-V do sekundární lokality s Azure Site Recovery | Microsoft Docs"
description: "Popisuje, jak povolit replikaci pro virtuální počítače Hyper-V replikuje do sekundární lokality System Center VMM s Azure Site Recovery."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: d8782d14-9fef-4396-8912-ff945efc851b
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/30/2017
ms.author: raynew
ms.openlocfilehash: 6673d192dbc18bfc955d9e7e3c55893512511ffb
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="step-9-enable-replication-to-a-secondary-site-for-hyper-v-vms"></a>Krok 9: Povolení replikace do sekundární lokality pro virtuální počítače Hyper-V


Po nastavení zásad replikace, použijte tento článek k povolení replikace do sekundární lokality System Center Virtual Machine Manager (VMM) pro místní technologie Hyper-V virtuální počítače (VM), pomocí [Azure Site Recovery](site-recovery-overview.md).

Po přečtení tohoto článku můžete publikovat jakékoli dotazy nebo připomínky na jeho konci nebo na [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).



## <a name="select-vms-to-replicate"></a>Vyberte virtuální počítače k replikaci

1. Klikněte na **Krok 2: Replikujte aplikaci** > **Zdroj**. 

    ![Povolení replikace](./media/vmm-to-vmm-walkthrough-enable-replication/enable-replication1.png)

2. V **zdroj**, vyberte VMM server a cloudu, ve které se nacházejí hostitelů Hyper-V, které chcete replikovat. Pak klikněte na **OK**.

    ![Povolení replikace](./media/vmm-to-vmm-walkthrough-enable-replication/enable-replication2.png)
3. V **cíl**, ověřte sekundární server VMM a cloud.
4. V **virtuální počítače**, vyberte virtuální počítače, které chcete chránit ze seznamu.

    ![Povolení ochrany virtuálního počítače](./media/vmm-to-vmm-walkthrough-enable-replication/enable-replication5.png)

Můžete sledovat průběh **povolení ochrany** akce v **úlohy** > **úlohy Site Recovery**. Po **dokončení ochrany** úloha dokončena počáteční replikace je dokončena a virtuální počítač je připraven k převzetí služeb při selhání.

Poznámky:

- Můžete také povolit ochranu pro virtuální počítače v konzole nástroje VMM. Klikněte na tlačítko **povolení ochrany** na panelu nástrojů ve vlastnostech virtuálního počítače > **Azure Site Recovery** kartě.
- Po povolení replikace, můžete zobrazit vlastnosti pro virtuální počítač v **replikované položky**. Na **Essentials** řídicí panel, se zobrazí informace o zásadách replikace pro virtuální počítač a jeho stav. Klikněte na tlačítko **vlastnosti** další podrobnosti.

## <a name="onboard-existing-vms"></a>Zařadit stávající virtuální počítače

Pokud máte existující virtuální počítače v nástroji VMM, které jsou replikace s replikou technologie Hyper-V, můžete připojit je pro Azure Site Recovery replikaci následujícím způsobem:

1. Ujistěte se, že serveru technologie Hyper-V, který hostuje stávající virtuální počítač nachází v primárním cloudu, a že serveru technologie Hyper-V, který hostuje virtuální počítač repliky se nachází v sekundární cloudu.
2. Ujistěte se, že zásady replikace je nakonfigurován pro primárního cloudu VMM.
3. Povolení replikace pro primární virtuální počítač. Azure Site Recovery a VMM zkontrolujte, zda je zjištěna stejného hostitele repliky a virtuální počítač a Azure Site Recovery bude opakovaně používat a opětovnému zavedení replikace použitím zadaného nastavení.


## <a name="next-steps"></a>Další kroky

Přejděte na [krok 10: spustit testovací převzetí služeb](vmm-to-vmm-walkthrough-test-failover.md).
