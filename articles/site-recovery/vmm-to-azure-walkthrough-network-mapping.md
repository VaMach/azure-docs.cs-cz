---
title: "Konfigurace mapování sítě pro replikaci virtuálních počítačů Hyper-V v cloudech VMM do Azure s Azure Site Recovery | Microsoft Docs"
description: "Popisuje postup konfigurace mapování sítě při replikaci virtuálních počítačů Hyper-V v cloudech VMM do Azure s Azure Site Recovery"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: jwhit
editor: tysonn
ms.assetid: 8e7d868e-00f3-4e8b-9a9e-f23365abf6ac
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/23/2017
ms.author: raynew
ms.openlocfilehash: ed6f73d8baea5af0d2aa5f0ae885f305911ccc82
ms.sourcegitcommit: 422efcbac5b6b68295064bd545132fcc98349d01
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/29/2017
---
# <a name="step-9-configure-network-mapping-for-hyper-v-replication-with-vmm-to-azure"></a>Krok 9: Konfigurace mapování sítě pro replikaci technologie Hyper-V (s nástrojem VMM) do Azure.

Po nastavení [zdrojové a cílové nastavení replikace](vmm-to-azure-walkthrough-source-target.md), použijte tento článek ke konfiguraci mapování sítě pro mapování mezi místními sítěmi virtuálních počítačů nástroje VMM a sítě Azure.

POST dotazy a na konci tohoto článku nebo na [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="before-you-start"></a>Než začnete

- Další informace o [mapování sítě](vmm-to-azure-walkthrough-network.md#network-mapping-for-replication-to-azure).
- [Příprava mapování sítě VMM](vmm-to-azure-walkthrough-network.md#prepare-vmm-for-network-mapping). 
- Ověřte, že jsou virtuální počítače na serveru VMM připojené k síti virtuálních počítačů a že jste vytvořili aspoň jednu virtuální síť Azure. Na jednu síť Azure je možné namapovat několik sítí virtuálních počítačů.

## <a name="configure-mapping"></a>Konfigurace mapování

Nakonfigurujte mapování následujícím způsobem:

1. V části **Infrastruktura Site Recovery** > **Mapování sítí** > **Mapování sítě** klikněte na ikonu **+Mapování sítě**.

    ![Mapování sítě](./media/vmm-to-azure-walkthrough-network-mapping/network-mapping1.png)
2. V části **Přidání mapování sítě** vyberte zdrojový server VMM a jako cíl vyberte **Azure**.
3. Ověřte předplatné a model nasazení po převzetí služeb při selhání.
4. V poli **Zdrojová síť** vyberte ze seznamu přidruženého k tomuto serveru VMM zdrojovou místní síť virtuálních počítačů, kterou chcete namapovat.
5. V seznamu **Cílová síť** vyberte síť Azure, ve které budou po vytvoření umístěné virtuální počítače Azure repliky. Pak klikněte na **OK**.

    ![Mapování sítě](./media/vmm-to-azure-walkthrough-network-mapping/network-mapping2.png)

Když se začne mapovat síť, dojde k tomuto:

* Když začne mapování, existující virtuální počítače ve zdrojové síti virtuálních počítačů jsou připojené k cílové síti. Nové virtuální počítače připojené ke zdrojové síti virtuálních počítačů jsou po replikaci připojené k namapované síti Azure.
* Pokud upravíte existující mapování sítě, virtuální počítače repliky se připojí pomocí nového nastavení.
* Pokud má cílová síť více podsítí a jedna z těchto podsítí má stejný název jako podsíť, ve které je umístěný zdrojový virtuální počítač, pak se virtuální počítač repliky po převzetí služeb při selhání připojí k této cílové podsíti.
* Pokud neexistuje žádná cílová podsíť s odpovídajícím názvem, připojí se virtuální počítač k první podsíti v síti.



## <a name="next-steps"></a>Další kroky

Přejděte na [krok 10: vytvoření zásady replikace](vmm-to-azure-walkthrough-replication.md)
