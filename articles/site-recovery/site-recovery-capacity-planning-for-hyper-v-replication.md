---
title: "Technologie Hyper-V plánování kapacity nástroj pro Azure Site Recovery | Microsoft Docs"
description: "Tento článek popisuje, jak spustit nástroj plánování kapacity serveru technologie Hyper-V Azure Site Recovery"
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
ms.date: 11/28/2017
ms.author: nisoneji
ms.openlocfilehash: 80c3dcb65bd74bcfa3acc5f12dd5c45cd1c06455
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/09/2018
---
# <a name="hyper-v-capacity-planning-tool-for-site-recovery"></a>Technologie Hyper-V plánování kapacity nástroj pro obnovení lokality

Nové vylepšené verze [Azure Site Recovery nasazení Planner pro technologii Hyper-V nasazení Azure](site-recovery-hyper-v-deployment-planner.md) je nyní k dispozici. Nahradí původní nástroj. Nový nástroj pro plánování nasazení. Tento nástroj nabízí následující pokyny:

* Vyhodnocení podmínky virtuálních počítačů, na základě počtu disky, velikost disku, IOPS, změn a několik vlastností virtuálního počítače
* Šířka pásma sítě musí versus assessment plánovaný bod obnovení
* Požadavky na infrastrukturu Azure
* Požadavky na infrastrukturu na pracovišti
* Počáteční replikace dávkování pokyny
* Odhadované náklady na obnovení po havárii celkový do Azure

Jako součást nasazení Azure Site Recovery musíte pochopit replikace a nároky na šířku pásma. Plánování nástroj Site Recovery kapacity technologie Hyper-V vám pomůže určit tyto požadavky pro replikaci virtuálního počítače technologie Hyper-V.

Tento článek popisuje, jak spustit nástroj plánování kapacity technologie Hyper-V. Pomocí tohoto nástroje spolu s informacemi v [kapacitní plánování Site Recovery](site-recovery-capacity-planner.md).

## <a name="before-you-start"></a>Než začnete
Spusťte nástroj na uzlu serveru nebo clusteru Hyper-V v primární lokalitě. Chcete-li spustit nástroj, třeba hostitelské servery technologie Hyper-V:

* Operační systém: Windows Server 2012 nebo 2012 R2
* Paměť: 20 MB (minimálně)
* Procesoru: zatížení 5 procent (minimálně)
* Místo na disku: 5 MB (minimálně)

Před spuštěním nástroje je nutné připravit primární lokality. Pokud budete replikovat mezi dvěma místními servery a chcete provést kontrolu šířky pásma, je nutné připravit server repliky.

## <a name="step-1-prepare-the-primary-site"></a>Krok 1: Příprava primární lokality

1. V primární lokalitě zkontrolujte seznam všech technologie Hyper-V virtuální počítače chcete replikovat. Seznam hostitelů technologie Hyper-V nebo clustery, ve které jsou umístěny. Nástroj můžete spustit pro více samostatných hostitelů nebo pro jeden cluster, ale ne obojí společně. Je také nutné ho spustit odděleně pro každý operační systém. Shromážděte následující informace o Hyper-V serverů:

   * Samostatné servery systému Windows Server 2012
   * Clustery systému Windows Server 2012
   * Samostatné servery Windows Server 2012 R2
   * Windows Server 2012 R2 clusters

2. Povolte vzdálený přístup ke službě WMI na všech hostitelích Hyper-V a clustery. Tento příkaz spustit na každém serveru nebo clusteru a ujistěte se, zda pravidla brány firewall a nastavení oprávnění uživatele:

        netsh firewall set service RemoteAdmin enable
3. Povolte monitorování výkonu na serverech a clustery následujícím způsobem:

   a. Otevřít bránu Windows Firewall s **pokročilým zabezpečením** modul snap-in. 
   
   b. Vyberte příchozí pravidlo **síťového přístupu COM + (DCOM-IN)**. Vyberte všechna pravidla v **Vzdálená správa protokolu událostí** skupiny.

## <a name="step-2-prepare-a-replica-server-on-premises-to-on-premises-replication"></a>Krok 2: Příprava serveru repliky (místní na místní replikaci)
Pokud replikujete do Azure, nemusíte tento krok.

Doporučujeme, abyste nastavili jeden hostitel Hyper-V jako server pro obnovení tak, aby k němu pro kontrolu šířky pásma, lze replikovat fiktivní virtuálního počítače. Tento krok můžete vynechat, ale nemůže měřit šířku pásma, pokud je to provést.

1. Pokud chcete použít uzlu clusteru jako replika, nakonfigurujte zprostředkovatele replik technologie Hyper-V:

   a. V **správce serveru**, otevřete **Správce clusteru převzetí služeb při selhání**.

   b. Připojte se ke clusteru a zvýrazní název clusteru. Vyberte **akce** > **konfigurovat roli** otevřete Průvodce vysokou dostupností.

   c. V **vybrat roli**, vyberte **zprostředkovatele replik technologie Hyper-V**. V průvodci zadejte název **název pro rozhraní NetBIOS**. Zadejte adresu pro **IP adresu** má být použit jako bod připojení ke clusteru (označovaný jako klientský přístupový bod). **Zprostředkovatele replik technologie Hyper-V** je nakonfigurován, výsledkem název přístupového bodu klienta, který si všimněte.

   d. Ověřte, že se role zprostředkovatele replik technologie Hyper-V úspěšně přepne do režimu online a může převzít mezi všemi uzly v clusteru. Klikněte pravým tlačítkem na roli a vyberte **přesunout** > **vybrat uzel**. Vyberte uzel a pak vyberte **OK**.

   e. Pokud používáte ověřování pomocí certifikátů, zajistěte, aby všechny uzly clusteru a přístupové body mají nainstalovaný certifikát klienta.

2. Chcete-li server repliky, proveďte tyto kroky:

   a. Pro cluster, otevřete **Správce clusteru selhání** a připojte se ke clusteru. Vyberte **role**a potom vyberte roli. Vyberte **nastavení replikace** > **povolit tento cluster jako server repliky**. Pokud používáte cluster jako serveru repliky, musíte mít roli zprostředkovatele replik technologie Hyper-V, který je přítomen v clusteru v primární lokalitě.

   b. Samostatný server, otevřete **Správce technologie Hyper-V**. V **akce** podokně, vyberte **nastavení technologie Hyper-V** pro server, které chcete povolit. V **konfiguraci replikace**, vyberte **povolit tento počítač jako server repliky**.

3. Nastavení ověřování, proveďte tyto kroky:

   a. V části **ověřování a porty**vyberte, jak ověřit primární server a ověřování porty. Pokud používáte certifikát, vyberte **vybrat certifikát** vyberte jednu. Použijte protokol Kerberos, pokud primární server a obnovení hostitelů Hyper-V ve stejné doméně nebo v důvěryhodných doménách. Použijte certifikáty pro jiné domény nebo pracovní skupiny nasazení.

   b. V části **ověřování a úložiště**, povolit **žádné** ověřeného (primárního) serveru, aby odesílat data replikace na tento server repliky.

   ![Konfigurace replikace](./media/site-recovery-capacity-planning-for-hyper-v-replication/image1.png)

   c. Pokud chcete zkontrolovat, zda je spuštěna naslouchací proces protokolu nebo portu, které jste zadali, spusťte **netsh http show servicestate**. 

4. Nastavení brány firewall. Při instalaci technologie Hyper-V jsou pravidla brány firewall vytvoří aby povolovala přenosy na výchozí porty (HTTPS na 443) a protokolu Kerberos na 80. Tato pravidla povolte následujícím způsobem:

    - Ověření certifikátu v clusteru (443):``Get-ClusterNode | ForEach-Object {Invoke-command -computername \$\_.name -scriptblock {Enable-Netfirewallrule -displayname "Hyper-V Replica HTTPS Listener (TCP-In)"}}``
    - Ověřování protokolem Kerberos v clusteru (80):``Get-ClusterNode | ForEach-Object {Invoke-command -computername \$\_.name -scriptblock {Enable-Netfirewallrule -displayname "Hyper-V Replica HTTP Listener (TCP-In)"}}``
    - Ověření certifikátu na samostatný server:``Enable-Netfirewallrule -displayname "Hyper-V Replica HTTPS Listener (TCP-In)"``
    - Ověřování protokolu Kerberos na samostatný server:``Enable-Netfirewallrule -displayname "Hyper-V Replica HTTP Listener (TCP-In)"``

## <a name="step-3-run-the-capacity-planning-tool"></a>Krok 3: Spuštění nástroje pro plánování kapacity
Po přípravě primární lokalitu a nastavit server pro obnovení, můžete spustit nástroj.

1. [Stáhněte si](https://www.microsoft.com/download/details.aspx?id=39057) nástroj z webu Microsoft Download Center.

2. Spusťte nástroj z jednoho z primárních serverů nebo jednoho z uzlů z clusteru primární. Klikněte pravým tlačítkem na soubor .exe a potom vyberte **spustit jako správce**.

3. V **než začnete**, zadejte, jak dlouho chcete shromažďovat data. Doporučujeme spustit nástroj během pracovní doby, abyste měli jistotu, že data zástupcem. Pokud chcete ověřit připojení k síti pouze, můžete shromáždit pro pouze několik minut.

    ![Než začnete](./media/site-recovery-capacity-planning-for-hyper-v-replication/image2.png)

4. V **primární lokality podrobnosti**, zadejte název serveru nebo plně kvalifikovaný název domény pro samostatný hostitel. Pro cluster zadejte plně kvalifikovaný název domény pro klientský přístupový bod, název clusteru nebo libovolného uzlu v clusteru. Vyberte **Next** (Další). Nástroj automaticky rozpozná název, který je spuštěn na serveru. Nástroj převezme virtuálních počítačů, které pro zadané servery se dá sledovat.

    ![Podrobnosti o primární lokality](./media/site-recovery-capacity-planning-for-hyper-v-replication/image3.png)

5. V **podrobnosti lokality repliky**, pokud replikovat do Azure nebo pokud replikovat do sekundárního datacentra a nenastavili server repliky, vyberte **zahrnující lokality repliky testy přeskočte**. Pokud replikujete do sekundárního datacentra a nastavit typ repliky, zadejte plně kvalifikovaný název domény, bude samostatný server nebo klientský přístupový bod pro cluster v **Server name (nebo) CAP zprostředkovatele repliky technologie Hyper-V**.

    ![Podrobné informace o webu repliky](./media/site-recovery-capacity-planning-for-hyper-v-replication/image4.png)

6. V **rozšířené repliky podrobnosti**, vyberte **testy zahrnující lokality rozšířené repliky přeskočte**. Tyto testy nejsou podporovány službou Site Recovery.

7. V **vyberte virtuální počítače replikace**, tento nástroj se připojuje k serveru nebo clusteru. Zobrazuje virtuální počítače a disky spuštěných na primárním serveru, na základě nastavení, které jste zadali na **primární lokality podrobnosti** stránky. Virtuální počítače, které již jsou povoleny pro replikaci nebo které nejsou spuštěné se nezobrazí. Vyberte virtuální počítače, pro které chcete shromažďovat metriky. Výběr virtuálních pevných disků automaticky shromažďuje data pro virtuální počítače, příliš.

8. Pokud jste nakonfigurovali na serveru repliky nebo clusteru, v **sítě informace**, zadejte přibližnou šířky pásma sítě WAN má být použit mezi lokalitami primárním serverem a repliky. Pokud jste nakonfigurovali ověření certifikátu, vyberte certifikáty.

    ![Informace o síti](./media/site-recovery-capacity-planning-for-hyper-v-replication/image5.png)

9. V **Souhrn**, zkontrolujte nastavení. Vyberte **Další** zahájíte shromažďování metrik. Nástroj průběh a stav se zobrazí na **vypočítat kapacitu** stránky. Po ukončení běhu nástroje vyberte **zobrazit sestavu** zobrazte výstup. Ve výchozím nastavení, sestavy a protokoly jsou uloženy v **%systemdrive%\Users\Public\Documents\Capacity Planner**.

   ![Vypočítat kapacity](./media/site-recovery-capacity-planning-for-hyper-v-replication/image6.png)

## <a name="step-4-interpret-the-results"></a>Krok 4: Interpretovat výsledky

Tady jsou důležité metriky. Metriky, které zde nejsou uvedeny, můžete ignorovat. Nejsou relevantní pro Site Recovery.

### <a name="on-premises-to-on-premises-replication"></a>Místně na místní replikaci

* Dopad replikace na primárním hostitelem výpočetní a paměti
* Dopad replikace na primárním hostitelem a hostiteli obnovení místo na disku úložiště a IOPS
* Celková šířka pásma vyžadovaná pro replikaci rozdílů (MB/s)
* Zjištěnou šířku pásma sítě mezi primárním hostitelem a hostiteli obnovení (MB/s)
* Návrh pro ideální počet active paralelní přenosů mezi dvěma hostiteli nebo clustery

### <a name="on-premises-to-azure-replication"></a>Místním nasazením a Azure replikace

* Dopad replikace na primárním hostitelem výpočetní a paměti
* Dopad replikace na místo na disku úložiště a IOPS primárním hostitelem
* Celková šířka pásma vyžadovaná pro replikaci rozdílů (MB/s)

## <a name="more-resources"></a>Další zdroje informací

* Další informace o tomto nástroji najdete v tématu dokumentu, který doprovází nástroj pro stažení.
* Podívejte se na návod nástroj na Keithema Mayera [blog na TechNetu](http://blogs.technet.com/b/keithmayer/archive/2014/02/27/guided-hands-on-lab-capacity-planner-for-windows-server-2012-hyper-v-replica.aspx).
* [Získat výsledky](site-recovery-performance-and-scaling-testing-on-premises-to-on-premises.md) z testování výkonu pro místně na místní replikaci technologie Hyper-V.

## <a name="next-steps"></a>Další postup

Po dokončení plánování kapacity, můžete nasadit Site Recovery:
* [Replikace virtuálních počítačů Hyper-V v cloudech VMM do Azure](site-recovery-vmm-to-azure.md)
* [Replikace virtuálních počítačů technologie Hyper-V (bez VMM) do Azure](site-recovery-hyper-v-site-to-azure.md)
* [Replikace virtuálních počítačů technologie Hyper-V mezi lokalitami VMM](site-recovery-vmm-to-vmm.md)
