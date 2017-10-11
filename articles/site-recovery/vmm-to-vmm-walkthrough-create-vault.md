---
title: "Vytvoření trezoru pro replikaci technologie Hyper-V do sekundární lokality s Azure Site Recovery | Microsoft Docs"
description: "Popisuje postup vytvoření trezoru při replikaci virtuálních počítačů Hyper-V do sekundární lokality System Center VMM s Azure Site Recovery."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: ff65dbfb-cb26-410e-ab48-76971625db08
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/30/2017
ms.author: raynew
ms.openlocfilehash: 28cfcf12b2e369f96664c163c0b6f2aa8a6ddcb9
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="step-5-create-a-vault-for-hyper-v-replication-to-a-secondary-site"></a>Krok 5: Vytvoření trezoru pro replikaci technologie Hyper-V do sekundární lokality

Jakmile připravíte místní [servery System Center Virtual Machine Manager (VMM) a hostitele nebo Clustery Hyper-V](vmm-to-vmm-walkthrough-vmm-hyper-v.md) pro replikaci technologie Hyper-V do sekundární lokality pomocí [Azure Site Recovery](site-recovery-overview.md), můžete vytvořit Trezor služeb zotavení a vyberte scénář replikace.

Po přečtení tohoto článku můžete publikovat jakékoli dotazy nebo připomínky na jeho konci nebo na [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="create-a-recovery-services-vault"></a>Vytvoření trezoru Služeb zotavení

[!INCLUDE [site-recovery-create-vault](../../includes/site-recovery-create-vault.md)]


## <a name="choose-a-protection-goal"></a>Vyberte cíl ochrany

Vyberte, jak chcete počítače replikovat a kam je chcete replikovat.

1. Klikněte na tlačítko **Site Recovery** > **krok 1: připravte infrastrukturu** > **cíl ochrany**.
2. Vyberte **k obnovení lokality**a vyberte **Ano, s technologií Hyper-V**.
3. Vyberte **Ano** indikující, že používáte VMM ke správě hostitelů technologie Hyper-V.
4. Vyberte **Ano** Pokud máte sekundární server VMM. Pokud nasazujete replikace mezi cloudy na jednom serveru VMM, klikněte na tlačítko **ne**. Pak klikněte na **OK**.

    ![Zvolte cíle.](./media/vmm-to-vmm-walkthrough-create-vault/choose-goals.png)



## <a name="next-steps"></a>Další kroky

Přejděte na [krok 6: nastavení replikace zdrojové a cílové](vmm-to-vmm-walkthrough-source-target.md).