---
title: "Převzetí služeb při selhání a selhání zálohování virtuálních počítačů Azure replikovat na sekundární oblast Azure s Azure Site Recovery (Preview)"
description: "Zjistěte, jak převzetí služeb při selhání a selhání zpětné replikace virtuálních počítačů Azure sekundární oblasti Azure s Azure Site Recovery"
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 11/01/2017
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: 02b709bb8dbab5d10ce9f4cf6155ff26ce229298
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/01/2017
---
# <a name="fail-over-and-fail-back-azure-vms-between-azure-regions-preview"></a>Převzetí služeb při selhání a selhání zpět Azure virtuální počítače mezi oblastmi Azure (Preview)

[Azure Site Recovery](site-recovery-overview.md) služby přispívá ke strategie zotavení po havárii správy a Orchestrace replikace, převzetí služeb při selhání a navrácení služeb po obnovení místního počítače a virtuální počítače Azure (VM).

Tento kurz popisuje, jak k převzetí služeb jednoho virtuálního počítače Azure sekundární oblasti Azure. Poté, co jste při selhání, můžete zpět na primární oblasti nezdaří případě, že je k dispozici. V tomto kurzu se naučíte:

> [!div class="checklist"]
> * Převzít služby virtuálních počítačů Azure
> * Znovu nastavte ochranu sekundární virtuální počítač Azure, tak, aby se replikuje do primární oblasti
> * Navrácení služeb po obnovení sekundární virtuální počítač
> * Znovu nastavte ochranu primární virtuální počítač zpět do sekundární oblasti

## <a name="prerequisites"></a>Požadavky

- Ujistěte se, když jste dokončili [postupu zotavení po havárii](azure-to-azure-tutorial-dr-drill.md) zkontrolujte všechno funguje podle očekávání.
- Ověřte vlastnosti virtuálního počítače, před spuštěním testu převzetí služeb. Virtuální počítač musí být v souladu s [požadavky pro Azure](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).

## <a name="run-a-failover-to-the-secondary-region"></a>Spuštění převzetí služeb při selhání pro sekundární oblast

1. V **replikované položky**, vyberte virtuální počítač, který chcete převzetí služeb při selhání > **převzetí služeb při selhání**

   ![Převzetí služeb při selhání](./media/azure-to-azure-tutorial-failover-failback/failover.png)

2. V **převzetí služeb při selhání**, vyberte **bod obnovení** k převzetí služeb při selhání. Můžete použít jednu z následujících možností:

   * **Nejnovější** (výchozí): Tato možnost zpracuje všechna data ve službě Site Recovery a poskytuje nejnižší cíl bodu obnovení (RPO).
   * **Nejnovější zpracované**: Tato možnost obnoví virtuální počítač na nejnovější bod obnovení, který zpracovává službou Site Recovery.
   * **Vlastní**: Tato možnost slouží k převzetí služeb při selhání bod konkrétní obnovení. Tato možnost je užitečná pro provedení testu převzetí služeb.

3. Vyberte **vypnout počítač před zahájením převzetí služeb při selhání** Pokud chcete, aby Site Recovery se pokusit o proveďte vypnutí zdrojových virtuálních počítačích než převzetí služeb při selhání. Převzetí služeb při selhání pokračovat i v případě, že vypnutí selže.

4. Podle průběh převzetí služeb při selhání **úlohy** stránky.

5. Po převzetí služeb při selhání ověřte tak, že přihlášení k ní virtuální počítač. Pokud chcete přejít jiný bod obnovení pro virtuální počítač, pak můžete použít **změnit bod obnovení** možnost.

6. Jakmile budete spokojeni s neúspěšný přes virtuální počítač, můžete **potvrdit** převzetí služeb při selhání.
   Potvrzení odstraní všechny body obnovení k dispozici ve službě. **Změnit bod obnovení** možnost již není k dispozici.

## <a name="reprotect-the-secondary-vm"></a>Znovu nastavte ochranu sekundární virtuální počítač

Po převzetí služeb při selhání virtuálního počítače je potřeba nastavte ji znovu tak, aby replikuje zpět na primární oblasti.

1. Ujistěte se, že virtuální počítač **převzetí služeb při selhání, které jsou potvrzené** stavu a zkontrolujte, zda je k dispozici primární oblasti a vy budete schopní vytváření a přístup k nové prostředky v ní.
2. V **trezoru** > **replikované položky**, klikněte pravým tlačítkem na virtuální počítač, který při selhání a pak vyberte **znovu nastavit ochranu**.

   ![Klikněte pravým tlačítkem myši znovu nastavte ochranu](./media/azure-to-azure-tutorial-failover-failback/reprotect.png)

2. Všimněte si, že směr ochrany, sekundární primární oblasti, je již vybrána.
3. Zkontrolujte **skupinu prostředků, sítě, úložiště a dostupnost sady** informace. Všechny prostředky označené (Nový) jsou vytvořené jako součást operace opětovné ochrany.
4. Klikněte na tlačítko **OK** k aktivaci úlohy opětovné ochrany. Tato úloha doplňuje pro cílovou lokalitu s nejnovější data. Poté replikuje se rozdíly primární oblasti. Virtuální počítač je nyní v chráněném stavu.

## <a name="fail-back-to-the-primary-region"></a>Navrácení služeb po obnovení primární oblasti

Po virtuální počítače jsou znovu, vám může selhat zpět na primární oblasti pro potřeby. Chcete-li to provést, postupujte [převzetí služeb při selhání](#run-a-failover) pokyny.
