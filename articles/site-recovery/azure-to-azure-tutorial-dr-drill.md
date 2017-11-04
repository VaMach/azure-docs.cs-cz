---
title: "Spuštění postupu zotavení po havárii pro virtuální počítače Azure sekundární oblasti Azure s Azure Site Recovery (Preview)"
description: "Informace o spouštění postupu zotavení po havárii pro virtuální počítače Azure sekundární oblasti Azure pomocí služby Azure Site Recovery."
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/01/2017
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: 5bcd3d64714951508d984c17326e845ae4842670
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/01/2017
---
# <a name="run-a-disaster-recovery-drill-for-azure-vms-to-a-secondary-azure-region-preview"></a>Spuštění postupu zotavení po havárii pro virtuální počítače Azure sekundární oblasti Azure (Preview)

[Azure Site Recovery](site-recovery-overview.md) služby přispívá ke strategii obchodní kontinuitu a po havárii (BCDR) obnovení ponechat obchodních aplikací stále a spuštěním dostupné během plánovaných a neplánovaných výpadků. Site Recovery spravuje a orchestruje zotavení po havárii místní počítače a virtuální počítače Azure (VM), včetně replikace, převzetí služeb při selhání a obnovení.

V tomto kurzu se dozvíte, jak spouštět postupu zotavení po havárii pro virtuální počítač Azure, z jedné oblasti Azure na jiný, testovací převzetí služeb. Přechod ověří strategie replikace bez ztráty dat nebo výpadek a nemá vliv produkční prostředí. V tomto kurzu se naučíte:

> [!div class="checklist"]
> * Zkontrolujte splnění předpokladů
> * Spustit testovací převzetí služeb pro jeden virtuální počítač

## <a name="prerequisites"></a>Požadavky

- Před spuštěním testu převzetí služeb, doporučujeme ověřit vlastnosti virtuálního počítače a ujistěte se, že je vše podle očekávání.  Přístup k vlastnosti virtuálního počítače v **replikované položky**. **Essentials** okno zobrazuje informace o nastavení počítače a stav.
- Doporučujeme že použít samostatnou síť virtuálních počítačů Azure pro testovací převzetí služeb a ne výchozí sítě, ke které byl nastaven při povolení replikace.


## <a name="run-a-test-failover"></a>Spuštění testovacího převzetí služeb při selhání

1. V **nastavení** > **replikované položky**, klikněte na virtuální počítač **+ testovací převzetí služeb při selhání** ikonu.

2. V **testovací převzetí služeb při selhání**, vyberte bod obnovení pro převzetí:

   - **Nejnovější zpracované**: selhání virtuálního počítače na nejnovější bod obnovení, který byl zpracován službou Site Recovery. Zobrazí se časové razítko. Pomocí této možnosti žádný čas strávený, zpracování dat, takže nabízí nízkou RTO (plánovanou dobu obnovení)
   - **Nejnovější aplikace konzistentní**: tuto možnost převezme všech virtuálních počítačů na nejnovější bod obnovení konzistentních s aplikací. Zobrazí se časové razítko.
   - **Vlastní**: Vyberte libovolného bodu obnovení.

3. Vyberte cíl Azure virtuální sítě, které virtuální počítače Azure v sekundární oblasti se připojí, potom, co dojde převzetí služeb při selhání.

4. Chcete-li spustit převzetí služeb při selhání, klikněte na tlačítko **OK**. Chcete-li sledovat průběh, klikněte na virtuálním počítači otevřete jeho vlastnosti. Nebo můžete kliknout **testovací převzetí služeb při selhání** úlohy v název trezoru > **nastavení** > **úlohy** > **úlohy Site Recovery**.
5. Po dokončení převzetí repliku virtuálního počítače Azure se zobrazí na portálu Azure > **virtuální počítače**. Ujistěte se, že je virtuální počítač spuštěný, odpovídající velikost a připojený k příslušné síti.
6. Chcete-li odstranit virtuální počítače, které byly vytvořeny během testu převzetí služeb, klikněte na tlačítko **vyčistit testovací převzetí služeb při selhání** na replikované položky nebo plán obnovení. V **poznámky**, zaznamenejte a uložte jakékoli připomínky související s testovací převzetí služeb.

## <a name="next-steps"></a>Další kroky

> [!div class="nextstepaction"]
> [Spustit selhání produkční](azure-to-azure-tutorial-failover-failback.md)
