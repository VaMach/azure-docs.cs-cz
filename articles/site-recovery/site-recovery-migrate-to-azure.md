---
title: "Migrace místních virtuálních počítačů a fyzických serverů do Azure s využitím Site Recovery | Dokumentace Microsoftu"
description: "Tento článek popisuje migraci místních virtuálních počítačů a fyzických serverů do Azure pomocí Azure Site Recovery."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: c413efcd-d750-4b22-b34b-15bcaa03934a
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 10/30/2017
ms.author: raynew
ms.openlocfilehash: 423a1727efb0e1fd54eb0f8d5971ace3f8efc6cb
ms.sourcegitcommit: 43c3d0d61c008195a0177ec56bf0795dc103b8fa
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/01/2017
---
# <a name="migrate-to-azure-with-site-recovery"></a>Migrace do Azure pomocí Site Recovery

V tomto článku se seznámíte s použitím služby [Azure Site Recovery](site-recovery-overview.md) k migraci místních virtuálních počítačů a fyzických serverů do virtuálních počítačů Azure.

## <a name="before-you-start"></a>Než začnete

Podívejte se na toto video se stručným přehledem kroků vyžadovaných při migraci do Azure.
>[!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/ASRHowTo-Video2-Migrate-Virtual-Machines-to-Azure/player]


## <a name="what-do-we-mean-by-migration"></a>Co myslíme pojmem migrace?

Můžete nasadit službu Site Recovery pro replikaci místních virtuálních počítačů a fyzických serverů a jejich migraci.

- Při replikaci můžete nakonfigurovat místní počítače, aby se do Azure replikovaly pravidelně. Když potom dojde k výpadku, můžete při selhání převzít služby počítačů z místní lokality do Azure a přistupovat k nim tam. Až bude vaše místní lokalita opět dostupná, můžete navrátit služby po obnovení z Azure.
- Když použijete Site Recovery pro migraci, replikujete místní počítače do Azure. Potom převezmete při selhání jejich služby z místní lokality do Azure a dokončíte proces migrace. Žádné navrácení služeb po obnovení se neprovede.  

## <a name="what-can-site-recovery-migrate"></a>Co může Site Recovery migrovat?

Můžete:

- **Migrovat z místního prostředí:** Migrují se místní počítače Hyper-V a VMware a fyzické servery do Azure. Po dokončení migrace budou úlohy spouštěné na místních počítačích běžet na virtuálních počítačích Azure. 
- **Migrovat v rámci Azure:** Migrují se virtuální počítače Azure mezi oblastmi Azure. 
- **Migrovat AWS:** Migrují se instance Windows AWS do virtuálních počítačů Azure IaaS. 

## <a name="migrate-from-on-premises-to-azure"></a>Migrace z místního prostředí do Azure

Při migraci místních virtuálních počítačů Hyper-V a VMware a fyzických serverů postupujete téměř stejně jako při plné replikaci. 


## <a name="migrate-between-azure-regions"></a>Migrace mezi oblastmi Azure

Při migraci virtuálních počítačů Azure mezi oblastmi postupujete téměř stejně jako při plné migraci.

1. [Povolíte replikaci](azure-to-azure-tutorial-enable-replication.md)) pro počítače, které chcete migrovat.
2. Rychle [otestujete převzetí služeb při selhání](azure-to-azure-tutorial-dr-drill.md), abyste měli jistotu, že všechno funguje.
3. Potom spustíte [neplánované převzetí služeb při selhání](azure-to-azure-tutorial-failover-failback.md) pomocí možnosti **Dokončit migraci**.
4. Po dokončení migrace můžete [nastavit replikaci pro zotavení po havárii](site-recovery-azure-to-azure-after-migration.md) z oblasti Azure, do které jste migrovali, do sekundární oblasti.



## <a name="migrate-aws-to-azure"></a>Migrace AWS do Azure

Můžete migrovat instance AWS do virtuálních počítačů Azure.
- V tomto scénáři je podporována pouze migrace. Jinými slovy, můžete replikovat instance AWS a předat jejich služby při selhání do Azure, ale nemůžete navracet služby po obnovení.
- Pro účely migrace se s instancemi AWS zachází stejně jako s fyzickými servery. Nastavíte trezor služby Recovery Services, nasadíte místní konfigurační server pro správu replikace, přidáte jej do trezoru a zadáte nastavení replikace.
- Povolíte replikaci pro počítače, které chcete migrovat, a spustíte rychlé testovací převzetí služeb při selhání. Potom spustíte neplánované převzetí služeb při selhání pomocí možnosti **Dokončit migraci**.






## <a name="next-steps"></a>Další kroky

- [Migrace místních počítačů do Azure](tutorial-migrate-on-premises-to-azure.md)
- [Migrace virtuálních počítačů z jedné oblasti Azure do jiné](site-recovery-migrate-azure-to-azure.md)
- [Migrace AWS do Azure](tutorial-migrate-aws-to-azure.md)
