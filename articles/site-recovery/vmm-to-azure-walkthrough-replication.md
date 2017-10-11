---
title: "Nastavení zásad replikace pro virtuální počítač Hyper-V (s nástrojem VMM) replikaci do Azure s Azure Site Recovery | Microsoft Docs"
description: "Popisuje, jak nastavit zásadu replikace pro virtuální počítač Hyper-V (s nástrojem VMM) replikaci do Azure s Azure Site Recovery"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: ee808b20-324b-4198-b831-edb65b95e8b7
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/23/2017
ms.author: raynew
ms.openlocfilehash: 592e1c3f647e5b1f1d9aa776657e8f89b60349e1
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="step-10-set-up-a-replication-policy-for-hyper-v-vm-replication-with-vmm-to-azure"></a>Krok 10: Nastavení zásad replikace pro replikaci virtuálních počítačů technologie Hyper-V (s nástrojem VMM) do Azure.


Po nastavení [mapování sítě](vmm-to-azure-walkthrough-network-mapping.md), použijte tento článek nakonfigurovat zásadu replikace T\to replikovat místní virtuální počítače Hyper-V spravované v cloudech System Center Virtual Machine Manager (VMM) do Azure, pomocí [Azure Site Recovery](site-recovery-overview.md) službu na portálu Azure.

Po přečtení tohoto článku můžete publikovat jakékoli dotazy nebo připomínky na jeho konci nebo na [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).



## <a name="create-a-policy"></a>Vytvořit zásadu

1. Pokud chcete vytvořit novou zásadu replikace, klikněte na **Připravit infrastrukturu** > **Nastavení replikace** > **+Vytvořit a přidružit**.

    ![Síť](./media/vmm-to-azure-walkthrough-replication/gs-replication.png)
2. V části **Vytvořit a přidružit zásady** zadejte název zásady.
3. V části **Frekvence kopírování** určete, jak často chcete replikovat rozdílová data po počáteční replikaci (každých 30 sekund, 5 minut nebo 15 minut).

    > [!NOTE]
    >  Frekvence každých 30 sekund se nepodporuje při replikaci do Storage úrovně Premium. Omezení je určeno počtem snímků na jeden objekt blob (100), který Storage úrovně Premium podporuje. [Další informace](../storage/common/storage-premium-storage.md#snapshots-and-copy-blob)

4. V části **Uchování bodu obnovení** zadejte (v hodinách), jak dlouhý bude interval uchovávání dat pro jednotlivé body obnovení. Chráněné počítače je možné obnovit do libovolného bodu v rámci tohoto intervalu.
5. V nastavení **Frekvence snímků konzistentní vzhledem k aplikacím** určete, jak často (1–12 hodin) se mají vytvářet body obnovení obsahující snímky konzistentní vzhledem k aplikacím. Technologie Hyper-V používá dva typy snímků – standardní snímek, což je přírůstkový snímek celého virtuálního počítače, a snímek konzistentní vzhledem k aplikacím, který vytvoří snímek dat aplikací ve virtuálním počítači v daném okamžiku. Snímky konzistentní vzhledem k aplikacím využívají službu Stínová kopie svazku (VSS), aby bylo zajištěno, že aplikace budou při pořízení snímku v konzistentním stavu. Pokud povolíte snímky konzistentní vzhledem k aplikacím, bude to mít vliv na výkon aplikací běžících na zdrojových virtuálních počítačích. Zajistěte, aby byla hodnota, kterou nastavíte, menší než počet dalších bodů obnovení, které nakonfigurujete.
6. V nastavení **Čas spuštění počáteční replikace** určete, kdy se má spustit počáteční replikace. Při replikaci se využívá šířka pásma vašeho internetového připojení, může být proto vhodné naplánovat ji na dobu mimo špičky.
7. V nastavení **Šifrovat úložiště dat ve službě Azure** určete, jestli chcete v úložišti Azure šifrovat neaktivní data. Pak klikněte na **OK**.

    ![Zásady replikace](./media/vmm-to-azure-walkthrough-replication/gs-replication2.png)
8. Když vytvoříte novou zásadu, automaticky se přidruží ke cloudu VMM. Klikněte na tlačítko **OK**. K této zásadě replikace můžete přidružit další cloudy VMM (a virtuální počítače v nich) v části **Replikace** > název_zásady > **Přidružit cloud VMM**.

    ![Zásady replikace](./media/vmm-to-azure-walkthrough-replication/policy-associate.png)



## <a name="next-steps"></a>Další kroky

Přejděte na [krok 11: povolení replikace](vmm-to-azure-walkthrough-enable-replication.md)
