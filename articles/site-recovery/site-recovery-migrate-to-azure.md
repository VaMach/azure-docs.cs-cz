---
title: O migraci v Azure Site Recovery | Microsoft Docs
description: "Tento článek popisuje, jak migrovat místní a virtuální počítače Azure pomocí služby Azure Site Recovery."
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: article
ms.date: 01/07/2018
ms.author: raynew
ms.openlocfilehash: 966d532a33626a8fcc3a3b93790d203aadfd81b4
ms.sourcegitcommit: 6fb44d6fbce161b26328f863479ef09c5303090f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/10/2018
---
# <a name="about-migration"></a>O migraci

Přečtěte si tento článek rychlý přehled jak [Azure Site Recovery](site-recovery-overview.md) služba pomáhá při migraci počítačů. 

Tady je je možné migrovat pomocí Site Recovery:

- **Migrace z místního do Azure**: migrace virtuálních počítačů Hyper-V místní virtuální počítače VMware a fyzické servery do Azure. Po dokončení migrace budou úlohy spouštěné na místních počítačích běžet na virtuálních počítačích Azure. 
- **Migrovat v rámci Azure:** Migrují se virtuální počítače Azure mezi oblastmi Azure. 
- **Migrovat AWS:** Migrují se instance Windows AWS do virtuálních počítačů Azure IaaS. 


## <a name="what-do-we-mean-by-migration"></a>Co myslíme pojmem migrace?

Kromě pomocí Site Recovery pro zotavení po havárii místní a virtuální počítače Azure, můžete k migraci je služba Site Recovery. Jaký je rozdíl?

- Pro zotavení po havárii můžete počítače v pravidelných intervalech replikaci do Azure. Když dojde k výpadku, převzetí služeb při selhání na počítače z primární lokality do sekundární lokality Azure a přistupovat k nim z ní. Když primární lokality je opět k dispozici, předáte služby zpět z Azure.
- Pro migraci můžete replikaci lokálních počítačů do Azure nebo virtuálních počítačích Azure v sekundární oblasti. Potom převzetí služeb při selhání virtuálního počítače z primární lokality, sekundární a dokončení procesu migrace. Žádné navrácení služeb po obnovení se neprovede.  


## <a name="migration-scenarios"></a>Scénáře migrace

**Scénář** | **Podrobnosti**
--- | ---
**Migrace z místního do Azure** | Můžete migrovat místní virtuální počítače VMware, virtuální počítače Hyper-V a fyzických serverů do Azure. K tomu dokončit téměř stejný postup jako pro zotavení po havárii úplné. Jednoduše nemáte počítače zpět z Azure do místní lokality nezdaří.
**Migrace mezi oblastmi Azure** | Virtuální počítače Azure můžete migrovat z jedné oblasti Azure do jiné. Po dokončení migrace můžete nakonfigurovat zotavení po havárii pro virtuální počítače Azure nyní v sekundární oblasti, ke které jste migrovali.
**Migrace AWS do Azure** | Můžete migrovat instance AWS do virtuálních počítačů Azure. Site Recovery zpracovává instance AWS jako fyzické servery pro pro účely migrace. 

## <a name="next-steps"></a>Další postup

- [Migrace místních počítačů do Azure](tutorial-migrate-on-premises-to-azure.md)
- [Migrace virtuálních počítačů z jedné oblasti Azure do jiné](tutorial-migrate-azure-to-azure.md)
- [Migrace AWS do Azure](tutorial-migrate-aws-to-azure.md)
