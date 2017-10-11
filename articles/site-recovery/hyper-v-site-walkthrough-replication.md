---
title: "Nastavení zásad replikace pro replikaci virtuálních počítačů technologie Hyper-V (bez System Center VMM) do Azure s Azure Site Recovery | Microsoft Docs"
description: "Shrnuje kroky, které je třeba nastavit zásady replikace při replikaci virtuálních počítačů Hyper-V do úložiště Azure"
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 1777e0eb-accb-42b5-a747-11272e131a52
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/22/2017
ms.author: raynew
ms.openlocfilehash: ca5bec5cf1152e6259b9fe7a869edd2d62b88e1a
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="step-9-set-up-a-replication-policy-for-hyper-v-vm-replication-to-azure"></a>Krok 9: Nastavení zásad replikace pro virtuální počítač Hyper-V replikaci do Azure.

Tento článek popisuje, jak nastavit zásadu replikace při replikaci virtuálních počítačů Hyper-V do Azure (bez System Center VMM) pomocí [Azure Site Recovery](site-recovery-overview.md) službu na portálu Azure.


POST dotazy a na konci tohoto článku nebo na [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="about-snapshots"></a>O snímky

Technologie Hyper-V používá dva typy snímků – standardní snímek, což je přírůstkový snímek celého virtuálního počítače, a snímek konzistentní vzhledem k aplikacím, který vytvoří snímek dat aplikací ve virtuálním počítači v daném okamžiku.
    - Snímky konzistentní vzhledem k aplikacím využívají službu Stínová kopie svazku (VSS), aby bylo zajištěno, že aplikace budou při pořízení snímku v konzistentním stavu.
    - Pokud povolíte snímky konzistentní s aplikacemi, bude mít vliv výkon aplikací běžících na zdrojových virtuálních počítačích. Zajistěte, aby byla hodnota, kterou nastavíte, menší než počet dalších bodů obnovení, které nakonfigurujete.

## <a name="set-up-a-replication-policy"></a>Nastavení zásad replikace

1. Pokud chcete vytvořit novou zásadu replikace, klikněte na **Připravit infrastrukturu** > **Nastavení replikace** > **+Vytvořit a přidružit**.

    ![Síť](./media/hyper-v-site-walkthrough-replication/gs-replication.png)
2. V části **Vytvořit a přidružit zásady** zadejte název zásady.
3. V části **Frekvence kopírování** určete, jak často chcete replikovat rozdílová data po počáteční replikaci (každých 30 sekund, 5 minut nebo 15 minut).

    > [!NOTE]
    > Frekvence každých 30 sekund se nepodporuje při replikaci do Storage úrovně Premium. Omezení je určeno počtem snímků na jeden objekt blob (100), který Storage úrovně Premium podporuje. [Další informace](../storage/common/storage-premium-storage.md#snapshots-and-copy-blob).

4. V **uchování bodu obnovení**, zadejte v hodinách, jak dlouho je okno uchování pro každý bod obnovení. Virtuální počítače můžete obnovit do libovolného bodu v rámci časového období.
5. V **frekvence snímkování konzistentní aplikace vzhledem**, zadejte, jak často (1 – 12 hodin) body obnovení obsahující snímky konzistentní s aplikacemi jsou vytvořeny.
6. V **čas spuštění počáteční replikace**, určete, kdy má spustit počáteční replikaci. Při replikaci se využívá šířka pásma vašeho internetového připojení, může být proto vhodné naplánovat ji na dobu mimo špičky. Pak klikněte na **OK**.

    ![Zásady replikace](./media/hyper-v-site-walkthrough-replication/gs-replication2.png)

Když vytvoříte novou zásadu, je automaticky spojen s web Hyper-V. Je možné přidružit web Hyper-V (a virtuální počítače v něm) víc zásad replikace v **replikace** > název zásady > **přidružit web Hyper-V**.



## <a name="next-steps"></a>Další kroky

Přejděte na [krok 10: povolení replikace](hyper-v-site-walkthrough-enable-replication.md)
