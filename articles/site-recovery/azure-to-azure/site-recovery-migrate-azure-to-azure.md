---
title: "Migrace virtuálních počítačů Azure IaaS mezi oblastmi Azure | Microsoft Docs"
description: "Pomocí Azure Site Recovery k migraci virtuálních počítačů Azure IaaS z jedné oblasti Azure do jiné."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: jwhit
editor: tysonn
ms.assetid: 8a29e0d9-0010-4739-972f-02b8bdf360f6
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/31/2017
ms.author: raynew
ms.openlocfilehash: 805582d89ee906e21fbe588e895ce0fe3a926a66
ms.sourcegitcommit: cfd1ea99922329b3d5fab26b71ca2882df33f6c2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/30/2017
---
# <a name="migrate-azure-iaas-virtual-machines-between-azure-regions-with-azure-site-recovery"></a>Migrovat virtuální počítače Azure IaaS mezi oblastmi Azure s Azure Site Recovery

Pokud chcete migrovat virtuální počítače Azure (VM) pomocí tohoto článku mezi oblastí Azure, pomocí [Site Recovery](../site-recovery-overview.md) služby.

## <a name="prerequisites"></a>Požadavky

Je třeba virtuální počítače IaaS, kterou chcete migrovat.

## <a name="deployment-steps"></a>Kroky nasazení

1. [Vytvoření trezoru](azure-to-azure-tutorial-enable-replication.md#create-a-vault).
2. [Povolit replikaci](azure-to-azure-tutorial-enable-replication.md#enable-replication) pro virtuální počítače, které chcete migrovat a vyberte Azure jako zdroj. V současné době nepodporuje nativní replikace virtuálních počítačů Azure pomocí spravovaných disků.
3. [Spuštění převzetí služeb při selhání](../site-recovery-failover.md). Po dokončení počáteční replikace můžete spustit převzetí služeb při selhání z jedné oblasti Azure do jiné. Volitelně můžete vytvořit plán obnovení a spusťte převzetí služeb při selhání, migrovat několik virtuálních počítačů mezi oblastmi. [Další informace](../site-recovery-create-recovery-plans.md) o plány obnovení.

## <a name="next-steps"></a>Další kroky
Další informace o scénáře replikace v [co je Azure Site Recovery?](../site-recovery-overview.md)
