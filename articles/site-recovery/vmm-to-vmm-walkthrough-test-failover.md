---
title: "Spustit testovací převzetí služeb pro replikaci virtuálního počítače technologie Hyper-V do sekundární lokality s Azure Site Recovery | Microsoft Docs"
description: "Popisuje, jak spustit testovací převzetí služeb pro replikaci virtuálního počítače technologie Hyper-V do sekundární lokality System Center VMM s Azure Site Recovery."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: a3fd11ce-65a1-4308-8435-45cf954353ef
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/30/2017
ms.author: raynew
ms.openlocfilehash: 23d235d326273e7ec59feee6588a39f685401e52
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="step-10-run-a-test-failover-for-hyper-v-replication-to-a-secondary-site"></a>Krok 10: Spuštění testovací převzetí služeb při selhání pro replikaci technologie Hyper-V do sekundární lokality


Po povolení replikace pro virtuální počítače Hyper-V (VM) s [Azure Site Recovery](site-recovery-overview.md), použijte tento článek spustit testovací převzetí služeb. Testovací převzetí služeb ověřuje, že replikace pracuje bez dopadu na produkční prostředí. 


Po přečtení tohoto článku můžete publikovat jakékoli dotazy nebo připomínky na jeho konci nebo na [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="before-you-start"></a>Než začnete

- Když se aktivuje testovací převzetí služeb, můžete v síti, ke kterému se připojí testovací repliku virtuálních počítačů. [Další informace](site-recovery-test-failover-vmm-to-vmm.md#network-options-in-site-recovery) o možnostech sítě.
- Doporučujeme, abyste si zvolili síť, kterou jste vybrali při mapování sítě.
- Podle pokynů v tomto článku popisují postup převzít jeden virtuální počítač. Přečtěte si informace o [vytváření plánů obnovení](site-recovery-create-recovery-plans.md) Pokud chcete převzetí služeb při selhání víc virtuálních počítačů současně.
- Přečtěte si informace o [omezení pro testovací převzetí služeb při selhání](site-recovery-test-failover-vmm-to-vmm.md#things-to-note).
- IP adresa, zadaný pro virtuální počítač během testovacího převzetí služeb při selhání je stejnou IP adresu, která bude přijímat virtuálního počítače plánovaném nebo neplánovaném převzetí služeb při selhání (za předpokladu, že IP adresa je k dispozici v testovací síti převzetí služeb při selhání). Pokud IP adresa není k dispozici v testovací síti převzetí služeb při selhání, virtuální počítač přijímá jinou IP adresu, která je k dispozici v testovací síti převzetí služeb při selhání.
- Pokud chcete provést převzetí služeb při selhání v produkční sítě, pro úplné ověření počítače připojení k síti začátku do konce, Všimněte si, že:
    - Primární virtuální počítač by měl vypnout, pokud provádíte testu převzetí služeb. Dva virtuální počítače se stejnou identitou bude spuštěna ve stejné síti, v opačném ve stejnou dobu. 
    - Pokud provedete změny k testování virtuálních počítačů, tyto změny budou ztraceny, když vyčistit testovací převzetí služeb. Tyto změny nejsou replikovat zpět na primární virtuální počítač.
    - Testování produkční síť vede k výpadku pro produkční zatížení. Požádejte uživatele není pro použití aplikace v průběhu postupu zotavení po havárii.  


## <a name="run-a-test-failover-for-a-vm"></a>Spustit testovací převzetí služeb pro virtuální počítač

1. K převzetí služeb při selhání jeden virtuální počítač, v **replikované položky**, klikněte na virtuální počítač > **testovací převzetí služeb při selhání**.
2. V **testovací převzetí služeb při selhání**, zadejte, jak se připojí testovací virtuální počítače k sítím po převzetí služeb při selhání test. 
3. Kliknutím na **OK** zahajte převzetí služeb při selhání. Sledovat průběh **úlohy** kartě.
5. Po dokončení převzetí služeb při selhání ověřte, že testovací virtuální počítače úspěšně spustit.
6. Když jste hotovi, klikněte na tlačítko **vyčistit testovací převzetí služeb při selhání** v plánu obnovení.
7. V **poznámky**, zaznamenejte a uložte jakékoli připomínky související s testovací převzetí služeb. Tento krok odstraní virtuální počítače a sítě, které byly vytvořeny během testovacího převzetí služeb při selhání.


## <a name="next-steps"></a>Další kroky

Po nasazení otestujete, další informace o dalších typů [převzetí služeb při selhání](site-recovery-failover.md).
