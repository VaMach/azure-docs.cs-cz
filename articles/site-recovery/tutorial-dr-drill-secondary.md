---
title: "Spustit postupu zotavení po havárii na sekundární místní sítí pomocí Azure Site Recovery | Microsoft Docs"
description: "Další informace o spouštění postupu zotavení po havárii na sekundární místní sítí pomocí Azure Site Recovery"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 445878e2-6682-49ba-914d-4c6824ab08a6
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 09/18/2017
ms.author: raynew
ms.openlocfilehash: 8737c2a22bd729cfc15d5448e1ec0becef643fd5
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="run-a-disaster-recovery-drill-for-hyper-v-vms-to-your-secondary-on-premises-site"></a>Spuštění postupu zotavení po havárii pro virtuální počítače Hyper-V do sekundárního místního webu

[Azure Site Recovery](site-recovery-overview.md) služby přispívá ke strategie zotavení po havárii správy a Orchestrace replikace, převzetí služeb při selhání a navrácení služeb po obnovení místního počítače a virtuální počítače Azure (VM).

V tomto kurzu se dozvíte, jak spustit postupu zotavení po havárii pro virtuální počítače Hyper-V na sekundární místní lokalitu. Virtuální počítače Hyper-V musí být spravovaný v privátním cloudu System Center Virtual Machine Manager (VMM). Přechod ověří strategie replikace bez ztráty dat nebo výpadek a nemá vliv, které mají vliv na produkční prostředí. V tomto kurzu se naučíte:

> [!div class="checklist"]
> * Příprava předpokladů pro testovací převzetí služeb při selhání
> * Spuštění testovací převzetí služeb při selhání pro jeden počítač


## <a name="prerequisites"></a>Požadavky

- Můžete spustit testovací převzetí služeb pomocí několika možností sítě v sekundární lokalitě. Můžete spustit převzetí služeb při selhání bez připojení k síti, s existující síť nebo nechat automaticky vytvořit v testovací síti Site Recovery. 
**Pokud chcete použít existující produkční sítě pro testovací převzetí služeb:**:
    - Primární virtuální počítač by měl vypnout, pokud provádíte testu převzetí služeb. Jinak dva virtuální počítače se stejnou identitou bude spuštěna ve stejné síti, ve stejnou dobu. 
    - Pokud provedete změny testovací virtuální počítače, změny budou ztraceny, když vyčistit testovací převzetí služeb. Změny nejsou replikovat zpět na primární virtuální počítač.
    - Testování produkční síť způsobí, že výpadky v produkčním prostředí. Požádejte uživatele, není pro použití související aplikace v průběhu postupu zotavení po havárii. 
- Testovací síti repliky nemusí shodovat s typem logická síť VMM používá pro testovací převzetí služeb při selhání. Ale některé kombinace nefungují. Například pokud replika používá DHCP a izolace založené na síti VLAN, protože je nutné fondy IP adres, nemůžete použít virtualizace sítě systému Windows testovací převzetí služeb při selhání sítě. 
- Doporučujeme vám, že nepoužíváte síť, kterou jste vybrali pro mapování sítě pro testovací převzetí služeb při selhání.


## <a name="run-a-test-failover-for-a-vm"></a>Spustit testovací převzetí služeb pro virtuální počítač

1. V **replikované položky**, klikněte na virtuální počítač > **testovací převzetí služeb při selhání**.
2. V **testovací převzetí služeb při selhání**, zadejte, jak se připojí testovací virtuální počítače k sítím po převzetí služeb při selhání test. Doporučujeme vám, že pro účely tohoto kurzu vám nechat automaticky vytvořit v testovací síti Site Recovery. [Další informace](site-recovery-test-failover-vmm-to-vmm.md#network-options-in-site-recovery).
3. Kliknutím na **OK** zahajte převzetí služeb při selhání. Sledovat průběh **úlohy** kartě.
4. Po dokončení převzetí služeb při selhání ověřte, že testovací virtuální počítače úspěšně spustit.
5. Když jste hotovi, klikněte na tlačítko **vyčistit testovací převzetí služeb při selhání**. To odstraní testovací virtuální počítače a vytvořené během testovacího převzetí služeb při selhání.
6. V **poznámky**, zaznamenejte a uložte jakékoli připomínky související s testovací převzetí služeb. 


## <a name="next-steps"></a>Další kroky

[Spustit selhání produkční](tutorial-vmm-to-vmm-failover-failback.md)






