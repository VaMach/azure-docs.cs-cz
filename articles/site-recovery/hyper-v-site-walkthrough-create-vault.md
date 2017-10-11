---
title: "Nastavení úložiště pro replikaci technologie Hyper-V (bez System Center VMM) do Azure pomocí Azure Site Recovery | Microsoft Docs"
description: "Shrnuje kroky, které budete muset nastavit trezor pro replikaci technologie Hyper-V do Azure pomocí Azure Site Recovery"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: a936b3e2-0dbe-47ac-a98e-5285d96dc786
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/22/2017
ms.author: raynew
ms.openlocfilehash: 8212ff011633c3a89d3310e828b6d5f1cda6ce3f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="step-7-set-up-a-vault-for-hyper-v-replication"></a>Krok 7: Nastavení trezoru pro replikaci technologie Hyper-V

Tento článek popisuje, jak nastavit trezor a určit, co chcete replikaci z vaší místní umístění, do Azure pomocí [Azure Site Recovery](site-recovery-overview.md) službu na portálu Azure.


POST dotazy a na konci tohoto článku nebo na [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="create-a-recovery-services-vault"></a>Vytvoření trezoru Služeb zotavení

[!INCLUDE [site-recovery-create-vault](../../includes/site-recovery-create-vault.md)]



## <a name="select-a-protection-goal"></a>Vyberte cíl ochrany

Vyberte, jak chcete počítače replikovat a kam je chcete replikovat.

1. Klikněte na tlačítko **trezory služeb zotavení** > trezoru.
2. V nabídce prostředků, klikněte na tlačítko **Site Recovery** > **Příprava infrastruktury** > **cíl ochrany**.
3. V **cíl ochrany**, vyberte **do Azure** > **Ano, s technologií Hyper-V**. Vyberte **ne** potvrďte, že nepoužíváte VMM. 

    ![Zvolte cíle.](./media/hyper-v-site-walkthrough-create-vault/choose-goals2.png)



## <a name="next-steps"></a>Další kroky

Přejděte na [krok 8: nastavit zdroje a cíle](hyper-v-site-walkthrough-source-target.md)
