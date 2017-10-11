---
title: "Mapování sítě pro replikaci virtuálního počítače technologie Hyper-V do sekundární lokality s Azure Site Recovery | Microsoft Docs"
description: "Popisuje, jak k mapování sítě při replikaci virtuálních počítačů Hyper-V do sekundární lokalita VMM s Azure Site Recovery."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 461b7c1c-ef61-4005-aeec-2324e727a3d0
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/30/2017
ms.author: raynew
ms.openlocfilehash: 606e2d3d0e31b9d80200105e812f9d7bbbcf939c
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="step-7-map-networks-for-hyper-v-vm-replication-to-a-secondary-site"></a>Krok 7: Mapovat sítě pro replikaci virtuálního počítače technologie Hyper-V do sekundární lokality


Po nastavení [zdrojové a cílové nastavení](vmm-to-vmm-walkthrough-source-target.md) replikaci virtuálních počítačů technologie Hyper-V (VM) do sekundární lokality System Center Virtual Machine Manager (VMM), použijte tento článek ke konfiguraci mapování sítě pro replikaci virtuálního počítače (VM) technologie Hyper-V do sekundární lokality, pomocí [Azure Site Recovery](site-recovery-overview.md).

Po přečtení tohoto článku můžete publikovat jakékoli dotazy nebo připomínky na jeho konci nebo na [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="before-you-start"></a>Než začnete

- Další informace o [mapování sítě](vmm-to-vmm-walkthrough-network.md#network-mapping-overview) před zahájením.
- Zkontrolujte, zda jsou virtuální počítače na serverech VMM připojen k síti virtuálních počítačů.

## <a name="configure-network-mapping"></a>Konfigurace mapování sítě

1. V **mapování sítě** > **mapování sítí**, klikněte na tlačítko **+ mapování sítě**.
2. V **přidání mapování sítě** vyberte zdrojové a cílové servery VMM. Sítě virtuálních počítačů, které jsou přidružené k serverům VMM jsou načteny.
3. V **zdrojové síti**, vyberte síť, kterou chcete použít v seznamu sítě virtuálních počítačů spojené s primárním serverem VMM.
4. V **Cílová síť**, vyberte síť, kterou chcete použít na sekundárním serveru VMM. Pak klikněte na **OK**.

    ![Mapování sítě](./media/vmm-to-vmm-walkthrough-network-mapping/network-mapping2.png)

Když se začne mapovat síť, dojde k tomuto:

* Všechny existující repliky virtuálních počítačů, které odpovídají zdrojové síti virtuálních počítačů se připojí k cílové síti virtuálních počítačů.
* Nové virtuální počítače, které jsou připojené ke zdrojové síti virtuálních počítačů se připojí k namapované cílové síti po replikaci.
* Pokud upravíte existující mapování s novou sítí, virtuální počítače repliky budou připojené pomocí nového nastavení.
* Pokud má cílová síť více podsítí a jedna z těchto podsítí má stejný název jako podsíť, ve které je umístěný zdrojový virtuální počítač, pak se virtuální počítač repliky po převzetí služeb při selhání připojí k této cílové podsíti. Pokud neexistuje žádná cílová podsíť s odpovídajícím názvem, připojí se virtuální počítač k první podsíti v síti.



## <a name="next-steps"></a>Další kroky

Přejděte na [krok 8: Nakonfigurujte zásady replikace](vmm-to-vmm-walkthrough-replication.md).