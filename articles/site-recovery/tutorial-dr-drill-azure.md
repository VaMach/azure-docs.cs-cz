---
title: "Spuštění postupu zotavení po havárii pro místní počítače do Azure s Azure Site Recovery | Microsoft Docs"
description: "Další informace o postupu zotavení po havárii z místně spustíte do Azure s Azure Site Recovery"
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: tutorial
ms.date: 12/31/2017
ms.author: raynew
ms.openlocfilehash: f7dc5e2df95a64685a8b70d25e839c371d4fc2de
ms.sourcegitcommit: 85012dbead7879f1f6c2965daa61302eb78bd366
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/02/2018
---
# <a name="run-a-disaster-recovery-drill-to-azure"></a>Spuštění postupu zotavení po havárii do Azure

Tento kurz ukazuje, jak spouštět postupu zotavení po havárii pro místní počítač do Azure, pomocí testovací převzetí služeb. Přechod ověří strategie replikace bez ztráty dat. V tomto kurzu se naučíte:

> [!div class="checklist"]
> * Nastavit izolovanou síť pro testovací převzetí služeb
> * Příprava připojení k virtuálnímu počítači Azure po převzetí služeb při selhání
> * Spuštění testovací převzetí služeb při selhání pro jeden počítač

Toto je čtvrtý kurz v řadě. Tento kurz předpokládá, že jste už dokončili úkoly v předchozí kurzy.

1. [Příprava Azure](tutorial-prepare-azure.md)
2. [Příprava VMware v místním prostředí](tutorial-prepare-on-premises-vmware.md)
3. [Nastavení zotavení po havárii](tutorial-vmware-to-azure.md)

## <a name="verify-vm-properties"></a>Ověřte vlastnosti virtuálního počítače

Před spuštěním testu převzetí služeb, ověřte vlastnosti virtuálního počítače a ujistěte se, že virtuální počítač splňuje [požadavky pro Azure](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).

1. V **chráněné položky**, klikněte na tlačítko **replikované položky** > virtuálních počítačů.
2. V **replikované položky** podokně je souhrn informace o virtuálních počítačů, stav, a nejnovější dostupný bod obnovení. Klikněte na tlačítko **vlastnosti** zobrazíte další podrobnosti.
3. V **výpočty a síť**, můžete upravit název Azure, skupinu prostředků, cílovou velikost [skupinu dostupnosti](../virtual-machines/windows/tutorial-availability-sets.md)a spravovat nastavení disku.
   
      >[!NOTE]
      Navrácení služeb po obnovení do místního počítače Hyper-V z virtuálních počítačů Azure s spravované disky není aktuálně podporován. Pokud chcete migrovat místní virtuální počítače do Azure, aniž by selhání je zpět, měli používat jenom možnost spravované disky pro převzetí služeb při selhání.
   
4. Můžete zobrazit a upravit nastavení sítě, včetně sítě a podsítě, ve kterém virtuální počítač Azure budou umístěné po převzetí služeb při selhání a IP adresu, která bude přiřazena k němu.
5. V **disky**, zobrazí se informace o operačním systému a datové disky na virtuálním počítači.

## <a name="run-a-test-failover-for-a-single-vm"></a>Spustit testovací převzetí služeb pro jeden virtuální počítač

Když spustíte testovací převzetí služeb, dojde k následujícímu:

1. Kontrola předpokladů postupné a zkontrolujte, zda jsou všechny podmínky pro převzetí služeb při selhání na místě.
2. Převzetí služeb při selhání zpracuje data, takže aby bylo možné vytvořit virtuální počítač Azure. Pokud vyberte nejnovější bod obnovení, bod obnovení vytvořen z dat.
3. Virtuální počítač Azure je vytvořený pomocí dat, která zpracuje v předchozím kroku.

Spusťte testovací převzetí služeb takto:

1. V **nastavení** > **replikované položky**, klikněte na virtuální počítač > **+ testovací převzetí služeb při selhání**.
2. Vyberte **nejnovější zpracovat** bod obnovení pro účely tohoto kurzu. To převezme virtuálních počítačů k nejnovějšímu dostupnému bodu v čase. Zobrazí se časové razítko. Pomocí této možnosti je žádné čas strávený zpracováváním dat, takže nabízí nízkou RTO (plánovanou dobu obnovení).
3. V **testovací převzetí služeb při selhání**, vyberte cíl Azure sítě, které virtuální počítače Azure připojí po převzetí služeb při selhání.
4. Kliknutím na **OK** zahajte převzetí služeb při selhání. Průběh můžete sledovat kliknutím na virtuálním počítači otevřete jeho vlastnosti. Nebo můžete kliknout **testovací převzetí služeb při selhání** úlohy v název trezoru > **nastavení** > **úlohy** >
   **úlohy Site Recovery**.
5. Po dokončení převzetí repliku virtuálního počítače Azure se zobrazí na portálu Azure > **virtuální počítače**. Zkontrolujte, zda je virtuální počítač odpovídající velikost, byl připojený k síti správné, a zda je spuštěna.
6. Teď by měla být možné se připojit k replikovaný virtuální počítač v Azure.
7. Chcete-li odstranit virtuální počítače Azure vytvořené během testu převzetí služeb, klikněte na tlačítko **vyčistit testovací převzetí služeb při selhání** ve virtuálním počítači. V **poznámky**, zaznamenejte a uložte jakékoli připomínky související s testovací převzetí služeb.

V některých scénářích převzetí služeb při selhání vyžaduje další zpracování, který trvá přibližně osm až deset minut na dokončení. Možná jste si všimli již testovací převzetí služeb při selhání časy pro počítače se systémem VMware Linux, virtuální počítače VMware, které nemají povoluje služby DHCP a virtuální počítače VMware, který není k dispozici následující ovladače pro spouštění: miniport storvsc, vmbus, storflt, intelide, atapi.

## <a name="next-steps"></a>Další postup

> [!div class="nextstepaction"]
> [Spustit převzetí služeb při selhání a navrácení služeb po obnovení pro virtuální počítače VMware místní](tutorial-vmware-to-azure-failover-failback.md).
