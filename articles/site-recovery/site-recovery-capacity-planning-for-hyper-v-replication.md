---
title: "Nástroje Plánovač kapacity technologie Hyper-V Azure Site Recovery | Microsoft Docs"
description: "Tento článek popisuje, jak spustit nástroje Hyper-V capacity planner pro Azure Site Recovery"
services: site-recovery
documentationcenter: na
author: rayne-wiselman
manager: jwhit
editor: 
ms.assetid: 2bc3832f-4d6e-458d-bf0c-f00567200ca0
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 10/30/2017
ms.author: nisoneji
ms.openlocfilehash: db790f9dc56605b5b752e7ab797903e32b2fc675
ms.sourcegitcommit: 43c3d0d61c008195a0177ec56bf0795dc103b8fa
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/01/2017
---
# <a name="hyper-v-capacity-planner-tool-for-site-recovery"></a>Nástroje Plánovač kapacity technologie Hyper-V pro obnovení lokality

Jako součást nasazení Azure Site Recovery musíte pochopit replikace a nároky na šířku pásma. Nástroje Hyper-V capacity planner pro Site Recovery pomáhá to provést pro replikaci virtuálního počítače technologie Hyper-V.

Tento článek popisuje, jak ke spuštění nástroje capacity planner technologie Hyper-V. Tento nástroj slouží spolu s informacemi v [plánování kapacity pro Site Recovery](site-recovery-capacity-planner.md).

## <a name="before-you-start"></a>Než začnete
Spusťte nástroj na uzlu serveru nebo clusteru Hyper-V v primární lokalitě. Hostitelské servery technologie Hyper-V potřebuje ke spuštění nástroje:

* Operační systém: Windows Server 2012 nebo 2012 R2
* Paměť: 20 MB (minimálně)
* Procesoru: zatížení 5 procent (minimálně)
* Místo na disku: 5 MB (minimálně)

Před spuštěním nástroje je nutné připravit primární lokality. Pokud replikujete mezi dvěma místními lokalitami a chcete provést kontrolu šířky pásma, je nutné připravit server repliky.

## <a name="step-1-prepare-the-primary-site"></a>Krok 1: Příprava primární lokality

1. V primární lokalitě zkontrolujte seznam všech virtuálních počítačů technologie Hyper-V, které chcete replikovat, a hostitele nebo Clustery Hyper-V, na kterých se nachází. Nástroj můžete spustit pro více samostatných hostitelů, nebo pro jeden cluster, ale ne obojí společně. Je také nutné ho spustit odděleně pro každý operační systém, které byste měli získat informace o serverech technologie Hyper-V následujícím způsobem:

   * Samostatné servery systému Windows Server 2012
   * Clustery systému Windows Server 2012
   * Samostatné servery Windows Server 2012 R2
   * Clustery systému Windows Server 2012 R2
2. Povolte vzdálený přístup k rozhraní WMI na všech hostitelích Hyper-V a clustery. Tento příkaz spusťte na každém serveru nebo clusteru, a ujistěte se, že jsou nastavené pravidla brány firewall a oprávnění uživatele:

        netsh firewall set service RemoteAdmin enable
3. Povolte monitorování výkonu u serverů a clusterů, následujícím způsobem:

   * Otevřít bránu Windows Firewall s **pokročilým zabezpečením** modul snap-in a potom povolit příchozí následující pravidla: **síťového přístupu COM + (DCOM-IN)** a všechna pravidla v **skupiny Vzdálená správa protokolu událostí**.

## <a name="step-2-prepare-a-replica-server-on-premises-to-on-premises-replication"></a>Krok 2: Příprava serveru repliky (místní na místní replikaci)
Nemusíte to dělat, když replikujete do Azure.

Doporučujeme že nastavit jeden hostitel Hyper-V jako server pro obnovení, tak, aby k němu pro kontrolu šířky pásma, lze replikovat fiktivní virtuálního počítače.  To můžete přeskočit, ale nebudete moct měřit šířku pásma, pokud je to provést.

1. Pokud chcete použít uzlem clusteru zprostředkovatele replik technologie Hyper-V můžete nakonfigurovat repliku:

   * V **správce serveru**, otevřete **Správce clusteru převzetí služeb při selhání**.
   * Připojte se ke clusteru, zvýrazněte název clusteru a klikněte na **akce** > **konfigurovat roli** otevřete Průvodce vysokou dostupností.
   * V **vybrat roli**, klikněte na tlačítko **zprostředkovatele replik technologie Hyper-V**. V průvodci zadejte **název pro rozhraní NetBIOS** a **IP adresu** má být použit jako bod připojení ke clusteru (označovaný jako klientský přístupový bod). **Zprostředkovatele replik technologie Hyper-V** bude nakonfigurován, výsledkem je název klientského přístupového bodu, který si všimněte.
   * Ověřte, že se role zprostředkovatele replik technologie Hyper-V úspěšně přepne do režimu online a mohou přecházet mezi všemi uzly v clusteru. Chcete-li to provést, klikněte pravým tlačítkem na roli, přejděte na **přesunout**a potom klikněte na **vybrat uzel**. Vyberte uzel > **OK**.
   * Pokud používáte ověřování pomocí certifikátů, zajistěte, aby všechny uzly clusteru a přístupové body mají nainstalovaný certifikát klienta.
2. Povolte serveru repliky:

   * Pro cluster s podporou otevřete Správce clusteru selhání, připojte se ke clusteru a klikněte na tlačítko **role** > vyberte role > **nastavení replikace** > **povolit tento cluster jako server repliky**. Pokud používáte cluster jako serveru repliky, musíte mít roli zprostředkovatele replik technologie Hyper-V, který je přítomen v clusteru v primární lokalitě.
   * Samostatný server otevřete Správce technologie Hyper-V. V **akce** podokně klikněte na tlačítko **nastavení technologie Hyper-V** pro server, které chcete povolit a v **konfiguraci replikace** klikněte na tlačítko **povolit tento počítač jako server repliky**.
3. Nastavení ověřování:

   * V **ověřování a porty**vyberte, jak ověřit primární server a ověřování porty. Pokud používáte klikněte na certifikát **vybrat certifikát** vyberte jednu. Použijte protokol Kerberos, pokud primární server a obnovení hostitelů Hyper-V ve stejné doméně nebo v důvěryhodných doménách. Použijte certifikáty pro jiné domény nebo pracovní skupiny nasazení.
   * V **ověřování a úložiště**, povolit **žádné** ověřeného (primárního) serveru, aby odesílat data replikace na tento server repliky.

     ![](./media/site-recovery-capacity-planning-for-hyper-v-replication/image1.png)
   * Spustit **netsh http show servicestate**, zkontrolujte, zda je spuštěna naslouchací proces pro protokol nebo port, který jste zadali:  
4. Nastavení brány firewall. Při instalaci technologie Hyper-V jsou pravidla brány firewall vytvoří aby povolovala přenosy na výchozí porty (HTTPS na 443, protokolu Kerberos na 80). Tato pravidla povolte následujícím způsobem:
  - Ověření certifikátu v clusteru (443):``Get-ClusterNode | ForEach-Object {Invoke-command -computername \$\_.name -scriptblock {Enable-Netfirewallrule -displayname "Hyper-V Replica HTTPS Listener (TCP-In)"}}``
  - Ověřování protokolem Kerberos v clusteru (80):``Get-ClusterNode | ForEach-Object {Invoke-command -computername \$\_.name -scriptblock {Enable-Netfirewallrule -displayname "Hyper-V Replica HTTP Listener (TCP-In)"}}``
  - Ověření certifikátu na samostatný server:``Enable-Netfirewallrule -displayname "Hyper-V Replica HTTPS Listener (TCP-In)"``
  - Ověřování protokolu Kerberos na samostatný server:``Enable-Netfirewallrule -displayname "Hyper-V Replica HTTP Listener (TCP-In)"``

## <a name="step-3-run-the-capacity-planner-tool"></a>Krok 3: Spuštění nástroje capacity planner
Poté, co jste připravili primární lokalitu a nastavit server pro obnovení, můžete spustit nástroj.

1. [Stáhněte si](https://www.microsoft.com/download/details.aspx?id=39057) nástroj z webu Microsoft Download Center.
2. Spusťte nástroj z jednoho z primárních serverů (nebo jeden z uzlů z primární clusteru). Klikněte pravým tlačítkem na soubor .exe a potom vyberte **spustit jako správce**.
3. V **před zahájením**, zadejte, jak dlouho chcete shromažďovat data. Doporučujeme že spustit nástroj během pracovní doby, abyste měli jistotu, že data zástupcem. Pokud zkoušíte pouze ověření síťového připojení, můžete shromáždit pro pouze několik minut.

    ![](./media/site-recovery-capacity-planning-for-hyper-v-replication/image2.png)
4. V **podrobnosti primární lokality**, zadejte název serveru nebo plně kvalifikovaný název domény pro samostatné hostitele nebo clusteru zadejte plně kvalifikovaný název domény klienta přijmout bodu, clusteru název nebo libovolný uzel v clusteru a pak klikněte na tlačítko **Další**. Nástroj automaticky rozpozná název, který je spuštěn na serveru. Nástroj převezme virtuálních počítačů, které pro zadané servery se dá sledovat.

    ![](./media/site-recovery-capacity-planning-for-hyper-v-replication/image3.png)
5. V **podrobnosti lokality repliky**, pokud se při replikaci do Azure, nebo pokud se při replikaci do sekundárního datacentra a nenastavili server repliky, vyberte **zahrnující lokality repliky testy přeskočte**. Pokud replikujete do sekundárního datacentra a nastavili jste si typ repliky, zadejte plně kvalifikovaný název domény serveru samostatné nebo klientský přístupový bod pro cluster, do **Server name (nebo) CAP zprostředkovatele repliky technologie Hyper-V**.

    ![](./media/site-recovery-capacity-planning-for-hyper-v-replication/image4.png)
6. V **rozšířené repliky podrobnosti**, povolit **testy zahrnující lokality rozšířené repliky přeskočte**. Tyto testy nejsou podporovány službou Site Recovery.
7. V **vyberte virtuální počítače replikace**, disky spuštěných na primárním serveru, v souladu s nastavení jste zadali na nástroje připojení k serveru nebo clusteru a zobrazí se virtuální počítače a **primární lokality podrobnosti** stránky. Virtuální počítače, které již jsou povoleny pro replikaci nebo který neběží, nebudou zobrazeny. Vyberte virtuální počítače, pro které chcete shromažďovat metriky. Výběr virtuálních pevných disků automaticky shromažďuje data pro virtuální počítače příliš.
8. Pokud jste nakonfigurovali na serveru repliky nebo clusteru, v **sítě informace**, přibližný šířky pásma sítě WAN si myslíte, bude použit mezi primárním a repliky lokality a vyberte certifikáty, pokud jste nakonfigurovali ověření certifikátem.

    ![](./media/site-recovery-capacity-planning-for-hyper-v-replication/image5.png)
9. V **Souhrn**, zkontrolujte nastavení a klikněte na **Další** zahájíte shromažďování metrik. Nástroj průběh a stav se zobrazí na **vypočítat kapacitu** stránky. Po ukončení běhu nástroje, klikněte na tlačítko **zobrazit sestavu** zobrazte výstup. Ve výchozím nastavení, sestavy a protokoly jsou uloženy v **%systemdrive%\Users\Public\Documents\Capacity Planner**.

   ![](./media/site-recovery-capacity-planning-for-hyper-v-replication/image6.png)

## <a name="step-4-interpret-the-results"></a>Krok 4: Interpretovat výsledky

Tady jsou důležité metriky. Metriky, které zde nejsou uvedeny, můžete ignorovat. Nejsou relevantní pro Site Recovery.

### <a name="on-premises-to-on-premises-replication"></a>Místně na místní replikaci

* Dopad replikace na primárním hostitelem výpočty, a paměti
* Dopad replikaci na primární, místo na disku úložiště obnovení hostitelů, IOPS
* Celková šířka pásma vyžadovaná pro replikaci rozdílů (MB/s)
* Zjištěnou šířku pásma sítě mezi primárním hostitelem a hostiteli obnovení (MB/s)
* Návrh pro ideální počet active paralelní přenosů mezi dvěma hostitele nebo clustery

### <a name="on-premises-to-azure-replication"></a>Místním nasazením a Azure replikace

* Dopad replikace na primárním hostitelem výpočty, a paměti
* Dopad replikace primárním hostitelem úložiště místa na disku, IOPS
* Celková šířka pásma vyžadovaná pro replikaci rozdílů (MB/s)

## <a name="more-resources"></a>Další zdroje informací
* Podrobné informace o tomto nástroji najdete v tématu dokumentu, který doprovází nástroj pro stažení.
* Podívejte se na návod nástroj na Keithema Mayera [blog na TechNetu](http://blogs.technet.com/b/keithmayer/archive/2014/02/27/guided-hands-on-lab-capacity-planner-for-windows-server-2012-hyper-v-replica.aspx).
* [Získat výsledky](site-recovery-performance-and-scaling-testing-on-premises-to-on-premises.md) z našich testování výkonu pro místně na místní replikaci technologie Hyper-V

## <a name="next-steps"></a>Další kroky

Po dokončení plánování kapacity můžete začít nasazovat Site Recovery:

* [Replikace virtuálních počítačů Hyper-V v cloudech VMM do Azure](site-recovery-vmm-to-azure.md)
* [Replikace virtuálních počítačů technologie Hyper-V (bez VMM) do Azure](site-recovery-hyper-v-site-to-azure.md)
* [Replikace virtuálních počítačů technologie Hyper-V mezi lokalitami VMM](site-recovery-vmm-to-vmm.md)
