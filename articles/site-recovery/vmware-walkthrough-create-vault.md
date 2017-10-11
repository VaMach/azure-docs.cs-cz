---
title: "Nastavení trezoru pro replikace VMware do Azure pomocí Azure Site Recovery | Microsoft Docs"
description: "Shrnuje kroky, které budete muset nastavit trezor pro replikace VMware do Azure pomocí Azure Site Recovery"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 8bce940e-f19f-4418-8360-aee7b073519a
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: raynew
ms.openlocfilehash: dca95ad46b8de587140c3573ba6ed5702a122032
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="step-7-set-up-a-vault-for-vmware-replication-to-azure"></a>Krok 7: Nastavení trezoru pro replikace VMware do Azure


Tento článek popisuje, jak nastavit trezor a určit, co chcete replikaci z vaší místní umístění, do Azure pomocí [Azure Site Recovery](site-recovery-overview.md) službu na portálu Azure.


POST dotazy a na konci tohoto článku nebo na [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).




## <a name="create-a-recovery-services-vault"></a>Vytvoření trezoru Služeb zotavení

[!INCLUDE [site-recovery-create-vault](../../includes/site-recovery-create-vault.md)]

## <a name="select-a-protection-goal"></a>Vyberte cíl ochrany

Vyberte, jak chcete počítače replikovat a kam je chcete replikovat.

1. Klikněte na tlačítko **trezory služeb zotavení** > trezoru.
2. V nabídce prostředků, klikněte na tlačítko **Site Recovery** > **Příprava infrastruktury** > **cíl ochrany**.
3. V **cíl ochrany**, vyberte **do Azure** > **Ano, s hypervisoru VMware vSphere**.



## <a name="next-steps"></a>Další kroky

Přejděte na [krok 8: nastavit zdroje a cíle](vmware-walkthrough-source-target.md)
