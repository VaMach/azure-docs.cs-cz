---
title: "Nastavit zdroje a cíle pro replikaci technologie Hyper-V do sekundární lokality s Azure Site Recovery | Microsoft Docs"
description: "Popisuje, jak nastavit zdroje a cíle při replikaci virtuálních počítačů Hyper-V do sekundární lokalita VMM s Azure Site Recovery."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: fa7809f1-7633-425f-b25d-d10d004e8d0b
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/30/2017
ms.author: raynew
ms.openlocfilehash: 07135e9b5e619971a59cc22ec68a0a4e8bcaabe1
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="step-6-set-up-the-replication-source-and-target"></a>Krok 6: Nastavení replikace zdroje a cíle


Po vytvoření služby zotavení trezoru pro replikaci technologie Hyper-V do sekundární lokalita VMM s [Azure Site Recovery](site-recovery-overview.md), použijte tento článek k nastavení zdrojové a cílové umístění pro replikaci. 

Po přečtení tohoto článku můžete publikovat jakékoli dotazy nebo připomínky na jeho konci nebo na [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).




## <a name="set-up-the-source-environment"></a>Nastavení zdrojového prostředí

Nainstalujte zprostředkovatele Azure Site Recovery na serverech VMM a zjišťovat a zaregistrujte server v trezoru.

1. Klikněte na tlačítko **krok 1: Příprava infrastruktury** > **zdroj**.

    ![Nastavení zdroje](./media/vmm-to-vmm-walkthrough-source-target/goals-source.png)
2. V okně **Připravit zdroj** klikněte na **+ VMM** a přidejte server VMM.

    ![Nastavení zdroje](./media/vmm-to-vmm-walkthrough-source-target/set-source1.png)
3. V **přidat Server**, zkontrolujte, zda **serveru System Center VMM** se zobrazí v **typ serveru** a že VMM server splňuje požadavky [požadavky](#prerequisites).
4. Stáhněte si instalační soubor zprostředkovatele Azure Site Recovery.
5. Stáhněte si registrační klíč. Budete ho potřebovat, když spustíte instalaci. Klíč je platný pět dní od jeho vygenerování.

    ![Nastavení zdroje](./media/vmm-to-vmm-walkthrough-source-target/set-source3.png)
6. Nainstalujte zprostředkovatele Azure Site Recovery na server VMM. Nemusíte explicitně nic instalovat na hostitelských serverech technologie Hyper-V.


## <a name="install-the-azure-site-recovery-provider"></a>Nainstalujte zprostředkovatele Azure Site Recovery

1. Spusťte instalační soubor na každý server VMM zprostředkovatele. Pokud VMM je nasazen v clusteru, takto prvním instalaci:
    -  Nainstalujte poskytovatele na aktivní uzel a dokončením instalace zaregistrujte VMM server v trezoru.
    - Potom nainstalujte zprostředkovatele na ostatních uzlech. Všechny uzly clusteru by měl spuštěna stejná verze zprostředkovatele.
2. Instalační program spustí několik kontrol požadovaných součástí a požádá o oprávnění k zastavení služby VMM. Služba VMM se restartuje automaticky po dokončení instalace. Pokud instalujete na clusteru VMM, se zobrazí výzva k zastavení Cluster role.
3. V **Microsoft Update**, můžete vyjádřit výslovný souhlas k určení, že se aktualizace zprostředkovatele nainstalovaly v souladu s vašimi zásadami Microsoft Update.
4. V **instalace**, přijmout nebo upravte výchozí umístění instalace a klikněte na tlačítko **nainstalovat**.

    ![Umístění instalace](./media/vmm-to-vmm-walkthrough-source-target/provider-location.png)
5. Po dokončení instalace klikněte na tlačítko **zaregistrovat** zaregistrujte server v trezoru.

    ![Umístění instalace](./media/vmm-to-vmm-walkthrough-source-target/provider-register.png)
6. V poli **Název trezoru** ověřte název trezoru, ve kterém bude server zaregistrovaný. Klikněte na *Další*.

    ![Registrace serveru](./media/vmm-to-vmm-walkthrough-source-target/vaultcred.png)
7. V **připojení k Internetu**, určete, jak se zprostředkovatel, který běží na serveru VMM připojuje k Azure.

    ![Nastavení internetu](./media/vmm-to-vmm-walkthrough-source-target/proxydetails.png)

   - Můžete určit, zda zprostředkovatel by měl připojit přímo k Internetu, nebo prostřednictvím proxy serveru.
   - Zadejte nastavení proxy serveru v případě potřeby.
   - Pokud používáte proxy server, vytvoří se účet VMM RunAs (DRAProxyAccount) automaticky pomocí přihlašovacích údajů, zadaný proxy serveru. Proxy server nakonfigurujte tak, aby tento účet bylo možné úspěšně ověřit. V konzole VMM lze upravovat nastavení účtu RunAs > **nastavení** > **zabezpečení** > **účty spustit jako**. Restartujte službu VMM k aktualizaci změn.
8. V části **Registrační klíč** vyberte klíč, který jste si stáhli z Azure Site Recovery a zkopírovali na server VMM.
9. Nastavení šifrování se používá pouze při replikaci virtuálních počítačů Hyper-V v cloudech VMM do Azure. Pokud provádíte replikaci do sekundární lokality, tak se nepoužívá.
10. Do pole **Název serveru** zadejte popisný název, který bude identifikovat server VMM v trezoru. V konfiguraci clusteru zadejte název role clusteru VMM.
11. V **synchronizovat metadata cloudu**vyberte, jestli chcete synchronizovat metadata pro všechny cloudy na serveru VMM v trezoru. Tuto akci stačí na každém serveru provést pouze jednou. Pokud nechcete, aby k synchronizaci veškerých cloudů, můžete toto políčko nechat nezaškrtnuté a synchronizovat každý cloud jednotlivě ve vlastnostech cloudu v konzole nástroje VMM.
12. Dokončete proces kliknutím na **Další**. Po registraci načte Azure Site Recovery metadata ze serveru VMM. Server se zobrazí na kartě **Servery VMM** stránky **Servery** v trezoru.

    ![Server](./media/vmm-to-vmm-walkthrough-source-target/provider13.png)
13. Jakmile je k dispozici v konzole Site Recovery na server v **zdroj** > **připravit zdroj** vyberte VMM server a vyberte cloud, ve kterém se nachází hostiteli Hyper-V. Pak klikněte na **OK**.

Zprostředkovatel můžete také nainstalovat z příkazového řádku:

[!INCLUDE [site-recovery-rw-provider-command-line](../../includes/site-recovery-rw-provider-command-line.md)]


## <a name="set-up-the-target-environment"></a>Nastavení cílového prostředí

Vyberte cílový server VMM a cloud:

1. Klikněte na tlačítko **připravit infrastrukturu** > **cíl**a vyberte cílovém serveru VMM, kterou chcete použít.
2. Zobrazí se cloudy na serveru, které jsou synchronizovány s Site Recovery. Vyberte cílový cloud.

   ![cíl](./media/vmm-to-vmm-walkthrough-source-target/target-vmm.png)



## <a name="next-steps"></a>Další kroky

Přejděte na [krok 7: nakonfigurování mapování sítě](vmm-to-vmm-walkthrough-network-mapping.md).
