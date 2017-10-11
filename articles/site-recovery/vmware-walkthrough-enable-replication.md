---
title: "Povolení replikace pro virtuální počítače VMware replikaci do Azure s Azure Site Recovery | Microsoft Docs"
description: "Shrnuje kroky je nutné povolit replikaci do Azure pro virtuální počítače VMware pomocí služby Azure Site Recovery"
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
ms.openlocfilehash: 470b9ddd8df4a4e74ec7174f79020c252323e502
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="step-11-enable-replication-for-vmware-virtual-machines-to-azure"></a>Krok 11: Povolení replikace pro virtuální počítače VMware do Azure


Tento článek popisuje, jak povolit replikaci pro místní virtuální počítače VMware do Azure, pomocí [Azure Site Recovery](site-recovery-overview.md) službu na portálu Azure.

POST dotazy a na konci tohoto článku nebo na [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="before-you-start"></a>Než začnete

- Musí mít virtuální počítače VMware [nainstalovat službu mobility](vmware-walkthrough-install-mobility.md). – Pokud je virtuální počítač připravený na nabízenou instalaci, procesový server automaticky nainstaluje službu Mobility, když aktivujete replikaci.
- Azure uživatelský účet potřebuje konkrétní [oprávnění](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines) k povolení replikace virtuálních počítačů do Azure
- Když přidáváte nebo odebíráte virtuálních počítačů, může trvat až 15 minut nebo déle pro změny vstoupily v platnost a se objevily na portálu.
- Čas poslední zjištěné můžete zkontrolovat pro virtuální počítače v **konfigurační servery** > **poslední obraťte se na**.
- Chcete-li přidat virtuální počítače bez čekání na naplánovaného zjišťování, zvýrazněte konfigurační server (nemáte klikněte na něj) a klikněte na tlačítko **aktualizovat**.



## <a name="exclude-disks-from-replication"></a>Vyloučení disků z replikace

Ve výchozím nastavení jsou všechny disky na počítači replikují. Disky můžete z replikace vyloučit. Například možná nebudete chtít replikovat disky s dočasná data nebo data, která se má aktualizovat pokaždé, když je počítač nebo restartování aplikace (například pagefile.sys nebo databázi tempdb systému SQL Server). [Další informace](site-recovery-exclude-disk.md)

## <a name="replicate-vms"></a>Replikace virtuálních počítačů

Než začnete, podívejte se na rychlé video s přehledem

>[!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/VMware-to-Azure-with-ASR-Video3-Protect-VMware-Virtual-Machines/player]

1. Klikněte na **Krok 2: Replikujte aplikaci** > **Zdroj**.
2. V **zdroj**, vyberte konfigurační server.
3. V **počítač typ**, vyberte **virtuální počítače**.
4. V **vCenter vSphere Hypervisor**, vyberte server vCenter, který spravuje hostitelů vSphere nebo vyberte hostitele.
5. Vyberte procesový server. Pokud jste nevytvořili žádné servery další proces bude konfigurační server. Pak klikněte na **OK**.

    ![Povolení replikace](./media/vmware-walkthrough-enable-replication/enable-replication2.png)

6. V **cíl**, vyberte předplatné a skupině prostředků, ve kterém chcete vytvořit neúspěšný přes virtuální počítače. Vyberte model nasazení, kterou chcete použít v Azure (klasický nebo prostředek management), pro selhání virtuálních počítačů.


7. Vyberte účet úložiště Azure, který chcete použít pro replikaci dat. Pokud nechcete použít účet, který jste již nastavili, můžete vytvořit nový.

8. Vyberte síť Azure a podsíť, ke kterým se připojí virtuální počítače Azure, když se po převzetí služeb při selhání vytvoří. Výběrem možnosti **Nakonfigurovat pro vybrané počítače** použijte nastavení sítě pro všechny počítače, které jste vybrali pro ochranu. Vyberte **Nakonfigurovat později** a vyberte síť Azure pro konkrétní počítač. Pokud nechcete použít stávající síť, můžete vytvořit jeden.

    ![Povolení replikace](./media/vmware-walkthrough-enable-replication/enable-rep3.png)
9. V nastavení **Virtuální počítače** > **Výběr virtuálních počítačů** klikněte a vyberte každý počítač, který chcete replikovat. Můžete vybrat pouze počítače, pro které je možné povolit replikaci. Pak klikněte na **OK**.

    ![Povolení replikace](./media/vmware-walkthrough-enable-replication/enable-replication5.png)
10. V **vlastnosti** > **konfigurovat vlastnosti**, vyberte účet, který se použije pro automaticky instalaci služby Mobility na počítači procesní server.
11. Ve výchozím nastavení jsou všechny disky replikovat. Klikněte na tlačítko **všechny disky** a zrušte zaškrtnutí všech disků, které nechcete, aby k replikaci. Pak klikněte na **OK**. Později můžete nastavit další vlastnosti virtuálního počítače.

    ![Povolení replikace](./media/vmware-walkthrough-enable-replication/enable-replication6.png)
11. V **nastavení replikace** > **nakonfigurujete nastavení replikace**, ověřte, zda je vybrán správný replikace zásad. Pokud změníte zásady, změny se použijí pro replikaci počítač a pro nové počítače.
12. Povolit **konzistence pro víc Virtuálních** Pokud chcete shromažďovat počítače do replikační skupiny a zadejte název pro skupinu. Pak klikněte na **OK**. Poznámky:

    * Počítače v replikační skupiny replikovat společně a mít sdílené body obnovení konzistentní při selhání a konzistentní s aplikací, když se převzetí služeb při selhání.
    * Doporučujeme vám, že shromáždíte virtuálních počítačů a fyzických serverů společně, aby odpovídaly vašich úloh. Povolení konzistence více virtuálních počítačů může ovlivnit výkon pracovního vytížení a musí být použit pouze pokud počítače běží stejné zatížení a potřebujete konzistenci.

    ![Povolení replikace](./media/vmware-walkthrough-enable-replication/enable-replication7.png)
13. Klikněte na tlačítko **povolit replikaci**. Můžete sledovat průběh **povolení ochrany** úlohy v **nastavení** > **úlohy** > **úlohy Site Recovery**. Po spuštění úlohy **Dokončit ochranu** je počítač připravený k převzetí služeb při selhání.

## <a name="next-steps"></a>Další kroky

Přejděte na [krok 12: spustit testovací převzetí služeb](vmware-walkthrough-test-failover.md)
