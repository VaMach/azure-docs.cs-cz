---
title: "Povolení replikace pro fyzické servery replikující se do Azure s Azure Site Recovery | Microsoft Docs"
description: "Shrnuje kroky je nutné povolit replikaci do Azure pro fyzické servery pomocí služby Azure Site Recovery"
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 519c5559-7032-4954-b8b5-f24f5242a954
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: raynew
ms.openlocfilehash: 42f35c53eec06a346281fd90c97aecfd2269307d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="step-10-enable-replication-for-physical-servers-to-azure"></a>Krok 10: Povolení replikace pro fyzických serverů do Azure


Tento článek popisuje, jak povolit replikaci pro místní Windows nebo Linuxem fyzických serverů do Azure, pomocí [Azure Site Recovery](site-recovery-overview.md) službu na portálu Azure.

POST dotazy a na konci tohoto článku nebo na [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="before-you-start"></a>Než začnete

- Servery musí mít [nainstalovat službu mobility](physical-walkthrough-install-mobility.md).
- Virtuální počítač připravený na nabízenou instalaci, procesový server automaticky nainstaluje služba Mobility při povolení replikace.
- Když povolíte stroj pro replikaci, Azure uživatelský účet potřebuje konkrétní [oprávnění](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines) zajistit budete moct používat úložiště Azure a vytvořit virtuální počítače Azure.
- Když přidáváte nebo odebíráte IP adres pro servery, může trvat až 15 minut nebo déle pro změny vstoupily v platnost a se objevily na portálu.


## <a name="exclude-disks-from-replication"></a>Vyloučení disků z replikace

Ve výchozím nastavení jsou všechny disky na počítači replikují. Disky můžete z replikace vyloučit. Například možná nebudete chtít replikovat disky s dočasná data nebo data, která se má aktualizovat pokaždé, když je počítač nebo restartování aplikace (například pagefile.sys nebo databázi tempdb systému SQL Server). [Další informace](site-recovery-exclude-disk.md)

## <a name="replicate-servers"></a>Replikace serverů

1. Klikněte na **Krok 2: Replikujte aplikaci** > **Zdroj**.
2. V **zdroj**, vyberte konfigurační server.
3. V **počítač typ**, vyberte **fyzických počítačů**.
4. Vyberte procesový server. Pokud jste nevytvořili žádné servery další proces bude konfigurační server. Pak klikněte na **OK**.
5. V **cíl**, vyberte předplatné a skupině prostředků, ve kterém chcete vytvořit neúspěšný přes virtuální počítače. Vyberte model nasazení, kterou chcete použít v Azure (klasický nebo prostředek management), pro selhání virtuálních počítačů.
6. Vyberte účet úložiště Azure, který chcete použít pro replikaci dat. Pokud nechcete použít účet, který jste již nastavili, můžete vytvořit nový.
7. Vyberte **síť Azure** a **podsíť** který připojit virtuální počítače Azure po převzetí služeb při selhání. Výběrem možnosti **Nakonfigurovat pro vybrané počítače** použijte nastavení sítě pro všechny počítače, které jste vybrali pro ochranu. Vyberte **Nakonfigurovat později** a vyberte síť Azure pro konkrétní počítač. Pokud nechcete použít stávající síť, můžete vytvořit jeden.

    ![Povolení replikace](./media/physical-walkthrough-enable-replication/targetsettings.png)

8. V **fyzické počítače**, klikněte na tlačítko **+ fyzický počítač** a zadejte **název** a **IP adresu**. Vyberte operační systém počítače, který chcete replikovat. Jak dlouho trvá několik minut, dokud nebudou zjištěny a uvedené v seznamu počítačů.
9. V **vlastnosti** > **konfigurovat vlastnosti**, vyberte účet, který se použije pro automaticky instalaci služby Mobility na počítači procesní server.
10. Ve výchozím nastavení jsou všechny disky replikovat. Klikněte na tlačítko **všechny disky** a zrušte zaškrtnutí všech disků, které nechcete, aby k replikaci. Pak klikněte na **OK**. Později můžete nastavit další vlastnosti virtuálního počítače.

    ![Povolení replikace](./media/physical-walkthrough-enable-replication/enable-replication6.png)
11. V **nastavení replikace** > **nakonfigurujete nastavení replikace**, ověřte, zda je vybrán správný replikace zásad. Pokud změníte zásady, změny se použijí pro replikaci počítač a pro nové počítače.
12. Povolit **konzistence pro víc Virtuálních** Pokud chcete shromažďovat počítače do replikační skupiny a zadejte název pro skupinu. Pak klikněte na **OK**. Poznámky:

    * Počítače v replikační skupiny replikovat společně a mít sdílené body obnovení konzistentní při selhání a konzistentní s aplikací, když se převzetí služeb při selhání.
    * Doporučujeme vám, že můžete shromažďování fyzických serverů, aby odpovídaly úlohy. Povolení konzistence více virtuálních počítačů může ovlivnit výkon pracovního vytížení a musí být použit pouze pokud počítače běží stejné zatížení a potřebujete konzistenci.

13. Klikněte na tlačítko **povolit replikaci**. Můžete sledovat průběh **povolení ochrany** úlohy v **nastavení** > **úlohy** > **úlohy Site Recovery**. Po spuštění úlohy **Dokončit ochranu** je počítač připravený k převzetí služeb při selhání.

## <a name="next-steps"></a>Další kroky

Přejděte na [krok 11: spustit testovací převzetí služeb](physical-walkthrough-test-failover.md)
