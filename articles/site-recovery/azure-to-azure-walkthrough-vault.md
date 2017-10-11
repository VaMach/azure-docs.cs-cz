---
title: "Nastavení úložiště pro virtuální počítač Azure repliction mezi oblastmi s Azure Site Recovery | Microsoft Docs"
description: "Shrnuje kroky, které budete muset nastavit trezor Azure replikace mezi oblastmi Azure pomocí Azure Site Recovery"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmon
editor: 
ms.assetid: 40472189-3d80-4963-b175-8bddcbc2f61f
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/01/2017
ms.author: raynew
ms.openlocfilehash: e03d17992ee0b12049636e40188950bcc4a6f31e
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="step-4-set-up-a-vault-for-azure-to-azure-replication"></a>Krok 4: Nastavení trezoru pro replikaci Azure do Azure

Po [plánování sítě](azure-to-azure-walkthrough-network.md), použijte tento článek k nastavení trezoru, pro virtuální počítače Azure (VM) replikující se do jiné oblasti Azure, pomocí [Azure Site Recovery](site-recovery-overview.md) službu na portálu Azure.

- Po dokončení článek, měli byste mít nastavení trezoru služeb zotavení.
- Případné připomínky můžete publikovat na konci tohoto článku nebo na [fóru služby Azure Site Recovery](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).



>[!NOTE]
>
> Azure replikace virtuálního počítače je aktuálně ve verzi preview.




## <a name="create-a-vault"></a>Vytvoření trezoru

[!INCLUDE [site-recovery-create-vault](../../includes/site-recovery-create-vault.md)]

>[!NOTE]
>
> Doporučujeme vytvořit trezor služeb zotavení v umístění, kde chcete replikovat virtuální počítače. Například, pokud cílové umístění je střed USA, vytvořte trezor v **střed USA**.


## <a name="next-steps"></a>Další kroky

Přejděte na [krok 5: povolení replikace](azure-to-azure-walkthrough-enable-replication.md)
