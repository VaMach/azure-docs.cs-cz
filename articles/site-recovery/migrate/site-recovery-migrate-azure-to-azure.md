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
ms.date: 11/27/2017
ms.author: raynew
ms.openlocfilehash: f645150aa7b93ebbd98899cc8fdd495dfeaf3076
ms.sourcegitcommit: 29bac59f1d62f38740b60274cb4912816ee775ea
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/29/2017
---
# <a name="migrate-azure-iaas-virtual-machines-between-azure-regions-with-azure-site-recovery"></a>Migrovat virtuální počítače Azure IaaS mezi oblastmi Azure s Azure Site Recovery

Vítejte v Azure Site Recovery! Tento článek použijte, pokud chcete migrovat virtuální počítače Azure mezi oblastmi Azure.
>[!NOTE]
>
> Replikace virtuálních počítačů Azure do jiné oblasti pro obnovení a migrace potřebovat po havárii, najdete v části [tento dokument](../site-recovery-azure-to-azure.md). Replikace obnovení lokality pro virtuální počítače Azure je aktuálně ve verzi preview.


## <a name="prerequisites"></a>Požadavky
Zde je nutné pro toto nasazení:

* **Virtuální počítače IaaS**: virtuálních počítačů, které chcete migrovat. Tyto virtuální počítače migrujete tak, že je považuje jako fyzické počítače.

## <a name="deployment-steps"></a>Kroky nasazení
Tato část popisuje kroky nasazení nového portálu Azure.

1. [Vytvoření trezoru](../site-recovery-azure-to-azure.md#create-a-recovery-services-vault).
2. [Povolit replikaci...] /(Site-Recovery-Azure-to-Azure.MD) pro virtuální počítače, které chcete migrovat a vyberte Azure jako zdroj.
  >[!NOTE]
  >
  > Aktuálně nejsou podporované nativní replikace virtuálních počítačů Azure pomocí spravovaných disků. Můžete použít možnost "Fyzické do Azure" v [tento dokument](../site-recovery-vmware-to-azure.md) k migraci virtuálních počítačů s spravované disky.
3. [Spuštění převzetí služeb při selhání](../site-recovery-failover.md). Po dokončení počáteční replikace můžete spustit převzetí služeb při selhání z jedné oblasti Azure do jiné. Volitelně můžete vytvořit plán obnovení a spusťte převzetí služeb při selhání, migrovat několik virtuálních počítačů mezi oblastmi. [Další informace](../site-recovery-create-recovery-plans.md) o plány obnovení.

## <a name="next-steps"></a>Další kroky
Další informace o jiných scénářích replikace v [co je Azure Site Recovery?](../site-recovery-overview.md)
