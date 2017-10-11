---
title: "Replikace virtuálních počítačů technologie Hyper-V do sekundární lokality s Azure Site Recovery | Microsoft Docs"
description: "Popisuje, jak replikovat virtuální počítače Hyper-V v cloudech VMM do sekundární lokalita VMM pomocí portálu Azure."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: jwhit
editor: 
ms.assetid: b33a1922-aed6-4916-9209-0e257620fded
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/14/2017
ms.author: raynew
ms.openlocfilehash: 777bddea6b1cb325a6f8ede00196b18e1746d80c
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="replicate-hyper-v-virtual-machines-in-vmm-clouds-to-a-secondary-vmm-site-using-the-azure-portal"></a>Replikace virtuálních počítačů technologie Hyper-V v cloudech VMM do sekundární lokalita VMM pomocí portálu Azure
> [!div class="op_single_selector"]
> * [Azure Portal](site-recovery-vmm-to-vmm.md)
> * [Portál Classic](site-recovery-vmm-to-vmm-classic.md)
> * [PowerShell – Resource Manager](site-recovery-vmm-to-vmm-powershell-resource-manager.md)
>
>

Tento článek popisuje, jak replikovat místní technologie Hyper-V virtuální počítače spravované v cloudech System Center Virtual Machine Manager (VMM), sekundární lokality pomocí [Azure Site Recovery](site-recovery-overview.md) na portálu Azure. Další informace o této [architekturu scénáře](site-recovery-components.md).

Po přečtení tohoto článku můžete publikovat jakékoli dotazy nebo připomínky na jeho konci nebo na [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="prerequisites"></a>Požadavky

**Požadavek** | **Podrobnosti**
--- | ---
**Azure** | Potřebujete účet [Microsoft Azure](http://azure.microsoft.com/). Můžete začít s [bezplatnou zkušební verzí](https://azure.microsoft.com/pricing/free-trial/). [Další informace](https://azure.microsoft.com/pricing/details/site-recovery/) o cenách za Site Recovery
**Místní VMM** | Doporučujeme, že abyste měli dva servery VMM, jeden v primární lokalitě a jeden v sekundární.<br/><br/> Můžete replikovat mezi cloudy na jednom serveru VMM.<br/><br/> Servery VMM by měl používat minimálně System Center 2012 SP1 s nejnovějšími aktualizacemi.<br/><br/> Servery VMM potřebovat přístup k Internetu.
**Cloudy VMM** | Každý server VMM musí mít na jeden nebo více cloudů a všechny cloudy musí mít sadu profilů kapacity technologie Hyper-V. <br/><br/>Cloudy musí obsahovat jeden nebo více skupin hostitelů VMM.<br/><br/> Pokud máte pouze jeden server VMM, musí být aspoň dva cloudy, tak, aby fungoval jako primární i sekundární.
**Hyper-V** | Servery Hyper-V musí běžet minimálně Windows Server 2012 s rolí Hyper-V, a mít nainstalované nejnovější aktualizace.<br/><br/> Server Hyper-V by měl obsahovat jeden nebo více virtuálních počítačů.<br/><br/>  Hostitelské servery Hyper-V by měl být umístěn v skupiny hostitelů v primárních a sekundárních cloudech VMM.<br/><br/> Pokud spustíte technologie Hyper-V v clusteru na Windows Server 2012 R2, nainstalujte [aktualizovat 2961977](https://support.microsoft.com/kb/2961977)<br/><br/> Pokud spustíte technologie Hyper-V v clusteru na Windows Server 2012, zprostředkovatele clusteru se nevytvoří automaticky, pokud máte statické IP adresy na základě clusteru. Zprostředkovatel clusteru nakonfigurujte ručně. [Další informace](http://social.technet.microsoft.com/wiki/contents/articles/18792.configure-replica-broker-role-cluster-to-cluster-replication.aspx).<br/><br/> Servery Hyper-V potřebují přístup k Internetu.
**Adresy URL** | Servery VMM a hostitelé Hyper-V by měly mít k dosažení těchto adres URL:<br/><br/> [!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)]

## <a name="deployment-steps"></a>Kroky nasazení

Zde je provést:

1. Ověřte požadavky.
2. Připravte VMM server a hostitelé technologie Hyper-V.
3. Vytvořte trezor služby Recovery Services. Trezor obsahuje konfigurační nastavení a orchestruje replikaci.
4. Zadejte nastavení zdroje, cíl a replikace.
5. Nasazení služby Mobility na virtuálních počítačích chcete replikovat.
6. Příprava pro replikaci a zapnout replikaci pro virtuální počítače Hyper-V.
7. Otestujete převzetí služeb při selhání, abyste měli jistotu, že všechno funguje, jak má.

## <a name="prepare-vmm-servers-and-hyper-v-hosts"></a>Příprava serverů VMM a hostitelé Hyper-V

Příprava pro nasazení:

1. Zajistěte, aby VMM server a hostitelé technologie Hyper-V souladu s požadavky popsané výše a mohou dosáhnout požadované adresy URL.
2. Nastavení sítí VMM tak, aby můžete nakonfigurovat [mapování sítě](#network-mapping-overview).

    - Zajistěte, aby virtuální počítače na zdrojovém hostitelském serveru Hyper-V byly připojené k síti virtuálních počítačů ve VMM. Tato síť musí být propojená na logickou síť, která je přidružená ke cloudu.
    Ověřte, že sekundární cloudu, ve kterém se použít k obnovení má odpovídající síť virtuálních počítačů konfigurovanou. Tato síť virtuálních počítačů musí být propojena na logickou síť, která je přidružená sekundární cloudu.

3. Příprava [jednotné nasazení serveru](#single-vmm-server-deployment), pokud chcete replikovat virtuální počítače mezi cloudy na stejném serveru VMM.

## <a name="create-a-recovery-services-vault"></a>Vytvoření trezoru Služeb zotavení
1. Přihlaste se k webu [Portál Azure](https://portal.azure.com).
2. Klikněte na **Nové** > **Správy** > **Služby zotavení**.
3. Do pole **Název** zadejte popisný název pro identifikaci trezoru. Máte-li více předplatných, vyberte jedno z nich.
4. [Vytvořte skupinu prostředků](../azure-resource-manager/resource-group-template-deploy-portal.md) nebo vyberte existující skupinu. Zadejte oblast Azure. Počítače jsou replikovány do této oblasti. Informace o tom, které oblasti jsou podporované, najdete v části s geografickou dostupností v tématu s [podrobnostmi o cenách Azure Site Recovery](https://azure.microsoft.com/pricing/details/site-recovery/).
5. Pokud chcete mít k trezoru rychlý přístup z řídicího panelu, klikněte na **Připnout na řídicí panel** > **Vytvořit trezor**.

    ![Nový trezor](./media/site-recovery-vmm-to-vmm/new-vault-settings.png)

Nový trezor se zobrazí na **řídicí panel**v **všechny prostředky**a v hlavním **trezory služeb zotavení** okno.


## <a name="choose-a-protection-goal"></a>Vyberte cíl ochrany

Vyberte, jak chcete počítače replikovat a kam je chcete replikovat.

2. Klikněte na tlačítko **Site Recovery** > **krok 1: připravte infrastrukturu** > **cíl ochrany**.
3. Vyberte **k obnovení lokality**a vyberte **Ano, s technologií Hyper-V**.
4. Vyberte **Ano** indikující, že používáte VMM ke správě hostitelů technologie Hyper-V.
5. Vyberte **Ano** Pokud máte sekundární server VMM. Pokud nasazujete replikace mezi cloudy na jednom serveru VMM, klikněte na tlačítko **ne**. Pak klikněte na **OK**.

    ![Zvolte cíle.](./media/site-recovery-vmm-to-vmm/choose-goals.png)

## <a name="set-up-the-source-environment"></a>Nastavení zdrojového prostředí

Nainstalujte zprostředkovatele Azure Site Recovery na serverech VMM a zjišťovat a zaregistrujte server v trezoru.

1. Klikněte na tlačítko **krok 1: Příprava infrastruktury** > **zdroj**.

    ![Nastavení zdroje](./media/site-recovery-vmm-to-vmm/goals-source.png)
2. V okně **Připravit zdroj** klikněte na **+ VMM** a přidejte server VMM.

    ![Nastavení zdroje](./media/site-recovery-vmm-to-vmm/set-source1.png)
3. V **přidat Server**, zkontrolujte, zda **serveru System Center VMM** se zobrazí v **typ serveru** a že VMM server splňuje požadavky [požadavky](#prerequisites).
4. Stáhněte si instalační soubor zprostředkovatele Azure Site Recovery.
5. Stáhněte si registrační klíč. Budete ho potřebovat, když spustíte instalaci. Klíč je platný pět dní od jeho vygenerování.

    ![Nastavení zdroje](./media/site-recovery-vmm-to-vmm/set-source3.png)
6. Nainstalujte zprostředkovatele Azure Site Recovery na server VMM. Nemusíte explicitně nic instalovat na hostitelských serverech technologie Hyper-V.


### <a name="install-the-azure-site-recovery-provider"></a>Nainstalujte zprostředkovatele Azure Site Recovery

1. Spusťte instalační soubor na každý server VMM zprostředkovatele. Pokud VMM je nasazen v clusteru, takto prvním instalaci:
    -  Nainstalujte poskytovatele na aktivní uzel a dokončením instalace zaregistrujte VMM server v trezoru.
    - Potom nainstalujte zprostředkovatele na ostatních uzlech. Všechny uzly clusteru by měl spuštěna stejná verze zprostředkovatele.
2. Instalační program spustí několik kontrol požadovaných součástí a požádá o oprávnění k zastavení služby VMM. Služba VMM se restartuje automaticky po dokončení instalace. Pokud instalujete na clusteru VMM, se zobrazí výzva k zastavení Cluster role.
3. V **Microsoft Update**, můžete vyjádřit výslovný souhlas k určení, že se aktualizace zprostředkovatele nainstalovaly v souladu s vašimi zásadami Microsoft Update.
4. V **instalace**, přijmout nebo upravte výchozí umístění instalace a klikněte na tlačítko **nainstalovat**.

    ![Umístění instalace](./media/site-recovery-vmm-to-vmm/provider-location.png)
5. Po dokončení instalace klikněte na tlačítko **zaregistrovat** zaregistrujte server v trezoru.

    ![Umístění instalace](./media/site-recovery-vmm-to-vmm/provider-register.png)
6. V poli **Název trezoru** ověřte název trezoru, ve kterém bude server zaregistrovaný. Klikněte na *Další*.

    ![Registrace serveru](./media/site-recovery-vmm-to-vmm-classic/vaultcred.PNG)
7. V **připojení k Internetu**, určete, jak se zprostředkovatel, který běží na serveru VMM připojuje k Azure.

    ![Nastavení internetu](./media/site-recovery-vmm-to-vmm-classic/proxydetails.PNG)

   - Můžete určit, zda zprostředkovatel by měl připojit přímo k Internetu, nebo prostřednictvím proxy serveru.
   - Zadejte nastavení proxy serveru v případě potřeby.
   - Pokud používáte proxy server, vytvoří se účet VMM RunAs (DRAProxyAccount) automaticky pomocí přihlašovacích údajů, zadaný proxy serveru. Proxy server nakonfigurujte tak, aby tento účet bylo možné úspěšně ověřit. V konzole VMM lze upravovat nastavení účtu RunAs > **nastavení** > **zabezpečení** > **účty spustit jako**. Restartujte službu VMM k aktualizaci změn.
8. V části **Registrační klíč** vyberte klíč, který jste si stáhli z Azure Site Recovery a zkopírovali na server VMM.
9. Nastavení šifrování se používá pouze při replikaci virtuálních počítačů Hyper-V v cloudech VMM do Azure. Pokud provádíte replikaci do sekundární lokality, tak se nepoužívá.
10. Do pole **Název serveru** zadejte popisný název, který bude identifikovat server VMM v trezoru. V konfiguraci clusteru zadejte název role clusteru VMM.
11. V **synchronizovat metadata cloudu**vyberte, jestli chcete synchronizovat metadata pro všechny cloudy na serveru VMM v trezoru. Tuto akci stačí na každém serveru provést pouze jednou. Pokud nechcete, aby k synchronizaci veškerých cloudů, můžete toto políčko nechat nezaškrtnuté a synchronizovat každý cloud jednotlivě ve vlastnostech cloudu v konzole nástroje VMM.
12. Dokončete proces kliknutím na **Další**. Po registraci načte Azure Site Recovery metadata ze serveru VMM. Server se zobrazí na kartě **Servery VMM** stránky **Servery** v trezoru.

    ![Server](./media/site-recovery-vmm-to-vmm-classic/provider13.PNG)
13. Jakmile je k dispozici v konzole Site Recovery na server v **zdroj** > **připravit zdroj** vyberte VMM server a vyberte cloud, ve kterém se nachází hostiteli Hyper-V. Pak klikněte na **OK**.

Zprostředkovatel můžete také nainstalovat z příkazového řádku:

[!INCLUDE [site-recovery-rw-provider-command-line](../../includes/site-recovery-rw-provider-command-line.md)]


## <a name="set-up-the-target-environment"></a>Nastavení cílového prostředí

Vyberte cílový server VMM a cloud.

1. Klikněte na tlačítko **připravit infrastrukturu** > **cíl**a vyberte cílovém serveru VMM, kterou chcete použít.
2. Zobrazí se cloudy na serveru, které jsou synchronizovány s Site Recovery. Vyberte cílový cloud.

   ![cíl](./media/site-recovery-vmm-to-vmm/target-vmm.png)

## <a name="set-up-replication-settings"></a>Nakonfigurování nastavení replikace

- Když vytvoříte zásadu replikace, všechny hostitele pomocí zásad, musí mít stejný operační systém. Cloudu VMM může obsahovat hostitelů Hyper-V s různými verzemi systému Windows Server, ale v takovém případě musíte víc zásad replikace.
- Můžete provádět počáteční replikaci v režimu offline. [Další informace](#prepare-for-initial-offline-replication)

1. Pokud chcete vytvořit novou zásadu replikace, klikněte na **Připravit infrastrukturu** > **Nastavení replikace** > **+Vytvořit a přidružit**.

    ![Síť](./media/site-recovery-vmm-to-vmm/gs-replication.png)
2. V části **Vytvořit a přidružit zásady** zadejte název zásady. Typ zdroje a cíle musí být **technologie Hyper-V**.
3. V **verze hostitele technologie Hyper-V**, vyberte, jaký operační systém běží na hostiteli.
4. V **typ ověřování** a **port ověřování**, zadejte, jak ověření přenosů mezi primárními a obnovovacími hostitelské servery technologie Hyper-V. Vyberte **certifikát** pouze tehdy, je pracovní prostředí protokolu Kerberos. Azure Site Recovery automaticky nakonfiguruje certifikáty pro ověřování pomocí protokolu HTTPS. Nemusíte dělat nic ručně. Ve výchozím nastavení bude otevřen port 8083 a 8084 (pro certifikáty) v bráně Windows Firewall na hostitelských serverech technologie Hyper-V. Pokud vyberete **Kerberos**, lístek protokolu Kerberos se použije pro vzájemné ověřování serverů hostitele. Všimněte si, že toto nastavení platí jen pro hostitelské servery technologie Hyper-V v systému Windows Server 2012 R2.
5. V části **Frekvence kopírování** určete, jak často chcete replikovat rozdílová data po počáteční replikaci (každých 30 sekund, 5 minut nebo 15 minut).
6. V části **Uchování bodu obnovení** zadejte (v hodinách), jak dlouhý bude interval uchovávání dat pro jednotlivé body obnovení. Chráněné počítače je možné obnovit do libovolného bodu v rámci tohoto intervalu.
7. V **frekvence snímkování konzistentní aplikace vzhledem**, zadejte, jak často (1 – 12 hodin) body obnovení obsahující snímky konzistentní s aplikacemi jsou vytvořeny. Technologie Hyper-V používá dva typy snímků – standardní snímek, což je přírůstkový snímek celého virtuálního počítače, a snímek konzistentní vzhledem k aplikacím, který vytvoří snímek dat aplikací ve virtuálním počítači v daném okamžiku. Snímky konzistentní vzhledem k aplikacím využívají službu Stínová kopie svazku (VSS), aby bylo zajištěno, že aplikace budou při pořízení snímku v konzistentním stavu. Pokud povolíte snímky konzistentní s aplikacemi, bude mít vliv výkon aplikací běžících na zdrojových virtuálních počítačích. Zajistěte, aby byla hodnota, kterou nastavíte, menší než počet dalších bodů obnovení, které nakonfigurujete.
8. V **kompresi přenosu dat**, zadejte, zda by měl být komprimované replikovaná data, která se přenáší.
9. Vyberte **odstranit repliku virtuálního počítače**, chcete-li určit, že virtuální počítač repliky měla by být odstraněna, pokud zakažte ochranu pro zdrojový virtuální počítač. Pokud povolíte toto nastavení, když zakažte ochranu pro zdrojový virtuální počítač se odebere z konzole Site Recovery, nastavení obnovení lokality pro nástroj VMM se odeberou z konzoly nástroje VMM a replika odstraněn.
10. V **počáteční metodu replikace**, pokud replikujete přes síť, určete, zda chcete spustit počáteční replikaci nebo ji můžete naplánovat. Pokud chcete uložit šířku pásma sítě, můžete chtít naplánovat dobu mimo špičky. Pak klikněte na **OK**.

     ![Zásady replikace](./media/site-recovery-vmm-to-vmm/gs-replication2.png)
11. Když vytvoříte novou zásadu, automaticky se přidruží ke cloudu VMM. V **zásady replikace**, klikněte na tlačítko **OK**. K této zásadě replikace můžete přidružit další Cloudy VMM (a virtuální počítače v nich) **replikace** > název zásady > **přidružit Cloud VMM**.

     ![Zásady replikace](./media/site-recovery-vmm-to-vmm/policy-associate.png)


### <a name="configure-network-mapping"></a>Konfigurace mapování sítě

- Další informace o [mapování sítě](#prepare-for-network-mapping) před zahájením.
- Zkontrolujte, zda jsou virtuální počítače na serverech VMM připojen k síti virtuálních počítačů.


1. V **infrastruktura Site Recovery** > **mapování sítě** > **mapování sítí**, klikněte na tlačítko **+ mapování sítě**.

    ![Mapování sítě](./media/site-recovery-vmm-to-azure/network-mapping1.png)
2. V **přidání mapování sítě** vyberte zdrojové a cílové servery VMM. Sítě virtuálních počítačů, které jsou přidružené k serverům VMM jsou načteny.
3. V **zdrojové síti**, vyberte síť, kterou chcete použít v seznamu sítě virtuálních počítačů spojené s primárním serverem VMM.
4. V **Cílová síť**, vyberte síť, kterou chcete použít na sekundárním serveru VMM. Pak klikněte na **OK**.

    ![Mapování sítě](./media/site-recovery-vmm-to-vmm/network-mapping2.png)

Když se začne mapovat síť, dojde k tomuto:

* Všechny existující repliky virtuálních počítačů, které odpovídají zdrojové síti virtuálních počítačů se připojí k cílové síti virtuálních počítačů.
* Nové virtuální počítače, které jsou připojené ke zdrojové síti virtuálních počítačů se připojí k namapované cílové síti po replikaci.
* Pokud upravíte existující mapování s novou sítí, virtuální počítače repliky budou připojené pomocí nového nastavení.
* Pokud má cílová síť více podsítí a jedna z těchto podsítí má stejný název jako podsíť, ve které je umístěný zdrojový virtuální počítač, pak se virtuální počítač repliky po převzetí služeb při selhání připojí k této cílové podsíti. Pokud neexistuje žádná cílová podsíť s odpovídajícím názvem, připojí se virtuální počítač k první podsíti v síti.

### <a name="configure-storage-mapping"></a>Konfigurace úložiště mapování.

[Mapování úložiště](#prepare-for-storage-mapping) není podporována v nový portál Azure. Ale můžete povolit, pomocí prostředí Powershell. [Další informace](site-recovery-vmm-to-vmm-powershell-resource-manager.md#step-7-configure-storage-mapping).

## <a name="step-5-capacity-planning"></a>Krok 5: Plánování kapacity

Teď, když máte nastavenou základní infrastrukturu, začněte přemýšlet o plánování kapacity a zjistěte, jestli nepotřebujete další prostředky.

- Stažení a spuštění [Azure Site Recovery Capacity planner](site-recovery-capacity-planner.md), shromažďovat informace o prostředí replikace, včetně virtuálních počítačů, disků na virtuální počítač a úložišti na disk.
- Poté, co jste shromažďují informace o replikaci v reálném čase, můžete změnit zásady NetQos pro řízení šířky pásma replikace pro virtuální počítače. Další informace o [omezení provozu replika technologie Hyper-V](http://www.thomasmaurer.ch/2013/12/throttling-hyper-v-replica-traffic/), na blogu Thomase Maurer blogu. Další informace získáte [rutiny New-NetQosPolicy](https://technet.microsoft.com/library/hh967468.aspx.).

## <a name="enable-replication"></a>Povolení replikace

1. Klikněte na **Krok 2: Replikujte aplikaci** > **Zdroj**. Po povolení replikace pro první, kliknutí **+ replikovat** v trezoru povolíte replikaci pro další počítače.

    ![Povolení replikace](./media/site-recovery-vmm-to-vmm/enable-replication1.png)
2. V **zdroj**, vyberte VMM server a cloudu, ve které se nacházejí hostitelů Hyper-V, které chcete replikovat. Pak klikněte na **OK**.

    ![Povolení replikace](./media/site-recovery-vmm-to-vmm/enable-replication2.png)
3. V **cíl**, ověřte sekundární server VMM a cloud.
4. V **virtuální počítače**, vyberte virtuální počítače, které chcete chránit ze seznamu.

    ![Povolení ochrany virtuálního počítače](./media/site-recovery-vmm-to-vmm/enable-replication5.png)

Můžete sledovat průběh **povolení ochrany** akce v **úlohy** > **úlohy Site Recovery**. Po **dokončení ochrany** úloha dokončí, virtuální počítač je připraven k převzetí služeb při selhání.

Poznámky:

- Můžete také povolit ochranu pro virtuální počítače v konzole nástroje VMM. Klikněte na tlačítko **povolení ochrany** na panelu nástrojů ve vlastnostech virtuálního počítače > **Azure Site Recovery** kartě.
- Po povolení replikace, můžete zobrazit vlastnosti pro virtuální počítač v **replikované položky**. Na **Essentials** řídicí panel, se zobrazí informace o zásadách replikace pro virtuální počítač a jeho stav. Klikněte na tlačítko **vlastnosti** další podrobnosti.

### <a name="onboard-existing-virtual-machines"></a>Zařadit existující virtuální počítače
Pokud máte existující virtuální počítače v nástroji VMM, které jsou replikace s replikou technologie Hyper-V, můžete připojit je pro Azure Site Recovery replikaci následujícím způsobem:

1. Ujistěte se, že serveru technologie Hyper-V, který hostuje stávající virtuální počítač nachází v primárním cloudu, a že serveru technologie Hyper-V, který hostuje virtuální počítač repliky se nachází v sekundární cloudu.
2. Ujistěte se, že zásady replikace je nakonfigurován pro primárního cloudu VMM.
3. Povolení replikace pro primární virtuální počítač. Azure Site Recovery a VMM zkontrolujte, zda je zjištěna stejného hostitele repliky a virtuální počítač a Azure Site Recovery bude opakovaně používat a opětovnému zavedení replikace použitím zadaného nastavení.

## <a name="test-your-deployment"></a>Otestujte nasazení

Chcete-li otestovat nasazení, můžete spustit [testovací převzetí služeb při selhání](site-recovery-test-failover-vmm-to-vmm.md) pro jeden virtuální počítač, nebo [vytvoření plánu obnovení](site-recovery-create-recovery-plans.md) obsahující jeden nebo více virtuálních počítačů.



## <a name="prepare-for-offline-initial-replication"></a>Příprava pro počáteční replikaci prováděnou offline

Můžete provést offline replikaci dat prvotní kopie. Můžete to následujícím způsobem připravit:

* Na zdrojovém serveru zadejte cestu umístění, ze kterého bude export dat probíhat. Přiřaďte úplné řízení pro systém souborů NTFS a sdílené složky oprávnění ke službě VMM na cestu pro export. Na cílovém serveru zadejte cestu umístění, ze kterého dojde k importu. Přiřaďte stejné oprávnění pro tuto cestu importu.
* Pokud je import nebo export cesta sdílená, přiřaďte správce, Power Users, Print Operator nebo operátor serveru členství ve skupině pro účet služby VMM na vzdáleném počítači, na kterém sdílený se nachází.
* Pokud používáte všechny účty spustit jako přidejte hostitele, na cestám pro import a export, přiřaďte pro čtení a oprávnění k zápisu do účty spustit jako v nástroji VMM.
* Import a export sdílené složky nesmí nacházet na libovolném počítači se používá jako server hostitele technologie Hyper-V, protože konfigurace zpětné smyčky není podporována technologií Hyper-V.
* Ve službě Active Directory na každém Hyper-V serveru hostitele, který obsahuje virtuální počítače, který chcete chránit, povolit a nakonfigurovat omezené delegování důvěřovat vzdálených počítačích, na kterých cestám pro import a export nacházejí, následujícím způsobem:
  1. Na řadiči domény otevřete **Active Directory Users and Computers**.
  2. Ve stromu konzoly klikněte na tlačítko **DomainName** > **počítače**.
  3. Klikněte pravým tlačítkem na název serveru hostitele technologie Hyper-V > **vlastnosti**.
  4. Na **delegování** , klikněte na **důvěřovat tomuto počítači pro delegování pouze určeným službám**.
  5. Klikněte na tlačítko **používající libovolný protokol pro ověřování**.
  6. Klikněte na tlačítko **přidat** > **uživatelé a počítače**.
  7. Zadejte název počítače, který je hostitelem cestu pro export > **OK**. Ze seznamu dostupných služeb, podržte stisknutou klávesu CTRL a klikněte na tlačítko **cifs** > **OK**. Opakujte pro název počítače, který je hostitelem cestu importu. Opakujte podle potřeby pro další hostitelské servery technologie Hyper-V.



## <a name="prepare-for-network-mapping"></a>Příprava mapování sítě
Mapování mapuje sítě mezi sítěmi virtuálních počítačů nástroje VMM na primární a sekundární servery VMM pro:

* Optimálně umístíte virtuální počítače repliky na sekundární hostitelů Hyper-V po převzetí služeb při selhání.
* Po převzetí služeb při selhání připojte replikované virtuální počítače k příslušné sítě virtuálních počítačů.

Poznámky:
- Mapování sítě můžete nakonfigurovat sítě virtuálních počítačů na dvěma servery VMM nebo na jednom serveru VMM, pokud dvě lokality jsou spravovány stejném serveru.
- Při mapování správně nakonfigurovaná a je povolená replikace, virtuální počítač v primární lokalitě se připojí k síti a jeho repliky v cílovém umístění, budou připojené k jeho namapované síťové.
- Pokud sítě byla nastavili správně v nástroji VMM, když při mapování sítě vyberte cílovou síť virtuálních počítačů, cloudy VMM zdroje, které používají zdrojové síti virtuálních počítačů zobrazí se, společně s sítí dostupných cílových virtuálních počítačů v cílové cloudech, které se používají pro ochranu.
- Pokud má cílová síť více podsítí a jedna z těchto podsítí má stejný název jako podsíť, na kterém se nachází zdrojový virtuální počítač, pak virtuální počítač repliky se připojí k této cílové podsíti po převzetí služeb při selhání. Pokud neexistuje žádná cílová podsíť s odpovídajícím názvem, připojí se virtuální počítač k první podsíti v síti.


Tady je příklad pro ilustraci tento mechanismus. Podívejme se na organizaci s dvěma umístěními v New Yorku a Chicagu.

| **Umístění** | **Server VMM** | **Sítě virtuálních počítačů** | **Mapovat na** |
| --- | --- | --- | --- |
| New York |VMM NewYork |VMNetwork1 NewYork |Mapovat do Chicaga VMNetwork1 |
| VMNetwork2 NewYork |Není mapováno | | |
| Chicago |VMM Chicago |VMNetwork1 Chicago |Mapovat na VMNetwork1 NewYork |
| VMNetwork2 Chicago |Není mapováno | | |

Tento příklad:

* Když virtuální počítač repliky se vytvoří pro virtuální počítač, který je připojen k VMNetwork1 NewYork, bude připojen k VMNetwork1 Chicagu.
* Když virtuální počítač repliky se vytvoří pro VMNetwork2 NewYork nebo VMNetwork2 Chicagu, nebude připojen k žádné síti.

Zde je, jak jsou cloudy VMM nastavit v našem příkladu organizace a logické sítě přidružené ke cloudům.

### <a name="cloud-protection-settings"></a>Nastavení ochrany cloudu
| **Chráněném cloudu** | **Ochrana cloudu** | **Logické sítě (New Yorku)** |
| --- | --- | --- |
| GoldCloud1 |GoldCloud2 | |
| SilverCloud1 |SilverCloud2 | |
| GoldCloud2 |<p>Není k dispozici</p><p></p> |<p>LogicalNetwork1 NewYork</p><p>LogicalNetwork1 Chicago</p> |
| SilverCloud2 |<p>Není k dispozici</p><p></p> |<p>LogicalNetwork1 NewYork</p><p>LogicalNetwork1 Chicago</p> |

### <a name="logical-and-vm-network-settings"></a>Nastavení sítě logické a virtuální počítač
| **Umístění** | **Logické sítě** | **Přidružené sítě virtuálních počítačů** |
| --- | --- | --- |
| New York |LogicalNetwork1 NewYork |VMNetwork1 NewYork |
| Chicago |LogicalNetwork1 Chicago |VMNetwork1 Chicago |
| LogicalNetwork2Chicago |VMNetwork2 Chicago | |

### <a name="target-networks"></a>Cílové sítě
Na základě tohoto nastavení, když vyberete Cílová síť virtuálních počítačů, v následující tabulce jsou možnosti, které budou k dispozici.

| **Výběr** | **Chráněném cloudu** | **Ochrana cloudu** | **Cílová síť k dispozici** |
| --- | --- | --- | --- |
| VMNetwork1 Chicago |SilverCloud1 |SilverCloud2 |Dostupné |
| GoldCloud1 |GoldCloud2 |Dostupné | |
| VMNetwork2 Chicago |SilverCloud1 |SilverCloud2 |Není k dispozici |
| GoldCloud1 |GoldCloud2 |Dostupné | |


### <a name="failback"></a>Navrácení služeb po obnovení
Pokud chcete zobrazit, co se stane, že v případě navrácení služeb po obnovení (zpětná replikace), Předpokládejme, že VMNetwork1 NewYork je namapována na VMNetwork1-Chicagu s následujícím nastavením.

| **Virtuální počítač** | **Připojení k síti virtuálních počítačů** |
| --- | --- |
| VM1 |VMNetwork1 sítě |
| Virtuálního počítače 2 (repliky VM1) |VMNetwork1 Chicago |

S těmito nastaveními pojďme si shrnout, co se stane, že v několika možných scénářích.

| **Scénář** | **Výsledek** |
| --- | --- |
| Žádná změna v vlastnosti sítě virtuálních počítačů 2 po převzetí služeb při selhání. |Zůstane připojené ke zdrojové síti virtuálních počítačů 1. |
| Vlastnosti sítě virtuálních počítačů 2 se změní po převzetí služeb při selhání a není připojen. |1 virtuální počítač není připojen. |
| Vlastnosti sítě virtuálních počítačů 2 se změní po převzetí služeb při selhání a je připojený k VMNetwork2 Chicagu. |Pokud není namapované VMNetwork2 Chicagu, bude odpojen virtuálních počítačů 1. |
| Mapování sítě VMNetwork1 Chicagu se změní. |1 virtuální počítač bude připojený k síti nyní mapováno na VMNetwork1 Chicagu. |


## <a name="prepare-for-single-server-deployment"></a>Příprava pro nasazení jednoho serveru


Pokud máte pouze jeden server VMM, můžete replikovat virtuální počítače v hostitelích technologie Hyper-V v cloudu VMM do [Azure](site-recovery-vmm-to-azure.md) nebo sekundární cloudu VMM. První možnost doporučujeme, protože replikace mezi cloudy není bezproblémový. Pokud chcete replikovat mezi cloudy, můžete replikovat jeden samostatný server VMM nebo s jeden server VMM nasazený v roztažené clusteru se systémem Windows

### <a name="standalone-vmm-server"></a>Server samostatné VMM

V tomto scénáři nasazení jednoho serveru VMM jako virtuální počítač v primární lokalitě a replikaci tohoto virtuálního počítače na sekundární lokalitu pomocí Site Recovery a replika technologie Hyper-V.

1. **Nastavit VMM na virtuálním počítači technologie Hyper-V**. Doporučujeme společné umístění instanci serveru SQL použitou nástrojem VMM na stejný virtuální počítač. Tím ušetříte čas, jako je potřeba vytvořit jenom jednoho virtuálního počítače. Pokud chcete použít vzdálenou instanci systému SQL Server a k výpadku dojde, budete muset obnovit tuto instanci, než bude možné obnovit VMM.
2. **Zkontrolujte má VMM server nakonfigurované alespoň dva cloudy**. K jednomu cloudu bude obsahovat virtuální počítače chcete replikovat a dalších cloudu bude sloužit jako sekundárního umístění. Cloud, který obsahuje virtuální počítače, které chcete chránit, by měly splňovat [požadavky](#prerequisites).
3. Nastavte Site Recovery, jak je popsáno v tomto článku. Vytvoření a registraci serveru VMM v trezoru, nastavte zásady replikace a povolení replikace. Zdrojové a cílové názvy VMM budou stejné. Zadejte že počáteční replikace probíhá přes síť.
4. Při nastavování mapování sítě Namapujte síť virtuálních počítačů pro cloud primární síť virtuálních počítačů pro sekundární cloudu.
5. V konzole Správce technologie Hyper-V na hostitele Hyper-V, který obsahuje virtuálního počítače VMM povolte replika technologie Hyper-V a povolit replikaci na virtuálním počítači. Zajistěte, aby že si nepřidáte VMM virtuálního počítače do cloudů, které jsou chráněné službou Site Recovery k zajištění, že nejsou službou Site Recovery přepsat nastavení repliky technologie Hyper-V.
6. Pokud vytvoříte plány obnovení pro převzetí služeb při selhání použijete stejný server VMM pro zdrojové a cílové.
7. V výpadku dokončení převzetí služeb při selhání a obnovit takto:

   1. Spusťte neplánované převzetí služeb při selhání v konzole Správce technologie Hyper-V v sekundární lokalitě, při selhání primárního virtuálního počítače VMM do sekundární lokality.
   2. Ověřte, zda je virtuální počítač VMM nahoru a spuštěna a v trezoru, spusťte neplánovaný proces převzetí služeb při selhání z primárního na sekundární cloudů virtuálních počítačů. Potvrdit převzetí služeb při selhání a vyberte alternativní bod obnovení v případě potřeby.
   3. Po dokončení neplánované převzetí služeb při selhání, všechny prostředky jsou přístupné z primární lokality znovu.
   4. Pokud primární lokalita je k dispozici, v konzole Správce technologie Hyper-V v sekundární lokalitě, povolte reverzní replikaci pro virtuální počítač VMM. Replikace pro virtuální počítač spustí z sekundární pro primární.
   5. Spusťte plánované převzetí služeb při selhání v konzole Správce technologie Hyper-V v sekundární lokalitě, při selhání virtuálního počítače VMM do primární lokality. Potvrďte převzetí služeb při selhání. Tak, aby VMM virtuální počítač znovu replikaci z primárního na sekundární povolíte zpětnou replikaci.
   6. V trezoru služeb zotavení povolte reverzní replikaci pro virtuální počítače, na zahájení replikace je ze sekundární pro primární zatížení.
   7. Trezor služeb zotavení spusťte plánované převzetí služeb při selhání pro navrácení služeb po obnovení virtuálních počítačů pracovního vytížení k primární lokalitě. Potvrzení převzetí služeb při selhání na jeho dokončení. Povolte na zahájení replikace virtuálních počítačů pracovního vytížení z primárního na sekundární zpětné replikace.

### <a name="stretched-vmm-cluster"></a>Roztažené clusteru VMM

Místo nasazování samostatný server VMM jako virtuální počítač, který replikuje do sekundární lokality, můžete provést VMM, které jsou vysoce dostupné po nasazení jako virtuální počítač v clusteru s podporou převzetí služeb při selhání systému Windows. To poskytuje pružnost úloh a ochranu proti selhání hardwaru. Nasazení pomocí Site Recovery virtuálního počítače VMM musí být nasazené v clusteru s podporou funkce stretch geograficky vzdálených lokalit. Použijte následující postup:

1. Instalace VMM na virtuálním počítači v clusteru s podporou převzetí služeb při selhání systému Windows a vyberte možnost spustit během instalace serveru jako vysoce dostupný.
2. Instanci systému SQL Server, který je používán VMM by měla být replikována se skupinami dostupnosti AlwaysOn serveru SQL, tak, aby bylo repliku databáze v sekundární lokalitě.
3. Postupujte podle pokynů v tomto článku Vytvoření trezoru, registraci serveru a nastavení ochrany. Je třeba zaregistrovat každý server VMM v clusteru, trezor služeb zotavení. Chcete-li to provést, nainstalujte poskytovatele na aktivním uzlu a registraci serveru VMM. Potom zprostředkovatele nainstalujte na jiných uzlech.
4. Když dojde k výpadku, serverem VMM a jeho odpovídající databázi systému SQL Server jsou převzetí služeb při selhání a získat přístup ze sekundární lokality.



## <a name="prepare-for-storage-mapping"></a>Příprava na mapování úložiště


Nastavení úložiště mapování pomocí mapování klasifikace úložiště na zdroj a cíl servery VMM proveďte následující:

  * **Určení cílového úložiště pro virtuální počítače repliky**– pevný disk zdrojový virtuální počítač bude replikovat do úložiště, které jste zadali (sdílené složce protokolu SMB nebo clusteru sdílené svazky (CSV)) v cílovém umístění.
  * **Umístění virtuálního počítače repliky**– mapování úložiště se používá k optimální umístění virtuálních počítačů repliky na hostitelských serverech technologie Hyper-V. Virtuální počítače repliky se umístí na hostitelích, kteří mohou přistupovat ke klasifikaci namapované úložiště.
  * **Žádné úložiště mapování**– Pokud nenakonfigurujete mapování úložiště, virtuální počítače budou replikovány do výchozího umístění úložiště zadaný na serveru hostitele technologie Hyper-V, který je přidružený virtuální počítač repliky.

Poznámky:
- Můžete nastavit mapování mezi dvěma cloudy VMM na jednom serveru.
- Klasifikace úložiště musí být k dispozici pro umístěné v cloudech zdrojové a cílové skupiny hostitelů.
- Klasifikace nemusíte mít stejný typ úložiště. Například můžete namapovat klasifikace zdroj, který obsahuje sdílené složky protokolu SMB na cílové klasifikaci, který obsahuje sdílené svazky clusteru.

### <a name="example"></a>Příklad
Pokud při výběru zdrojového a cílového serveru VMM během úložiště mapování klasifikace správně konfigurovány ve VMM, zobrazí se zdrojové a cílové klasifikace. Tady je příklad úložiště sdílených složek a klasifikace pro organizace s dvěma umístěními v New Yorku a Chicagu.

| **Umístění** | **Server VMM** | **Sdílené složky (zdroj)** | **Klasifikace (zdroj)** | **Mapovat na** | **Sdílené složky (cíl)** |
| --- | --- | --- | --- | --- | --- |
| New York |VMM_Source |SourceShare1 |ZLATÝ |GOLD_TARGET |TargetShare1 |
| SourceShare2 |STŘÍBRNÁ |SILVER_TARGET |TargetShare2 | | |
| SourceShare3 |BRONZOVÁ |BRONZE_TARGET |TargetShare3 | | |
| Chicago |VMM_Target | |GOLD_TARGET |Není mapováno | |
|  |SILVER_TARGET |Není mapováno | | | |
|  |BRONZE_TARGET |Není mapováno | | | |

Tento příklad:

* Když virtuální počítač repliky se vytvoří pro jakýkoli virtuální počítač na ZLATÝ úložiště (SourceShare1), bude replikován do úložiště GOLD_TARGET (TargetShare1).
* Když virtuální počítač repliky se vytvoří pro jakýkoli virtuální počítač na STŘÍBRNÝM úložiště (SourceShare2), bude replikován do úložiště SILVER_TARGET (TargetShare2) a tak dále.

### <a name="multiple-storage-locations"></a>Více umístění úložiště
Pokud cílový klasifikace je přiřazen k více sdílených složek protokolu SMB nebo sdílené svazky clusteru, umístění optimální úložiště bude automaticky vybrána, když je virtuální počítač je chráněný. Pokud je k dispozici zadaná klasifikace žádné vhodnou cílovou úložiště, výchozí umístění úložiště zadaný na hostiteli technologie Hyper-V se používá k umístění repliky virtuální pevné disky.

V následující tabulce ukazují, jak klasifikaci úložiště a sdílenými svazky clusteru se nastavují v našem příkladu.

| **Umístění** | **Klasifikace** | **Přidruženého úložiště** |
| --- | --- | --- |
| New York |ZLATÝ |<p>C:\ClusterStorage\SourceVolume1</p><p>\\FileServer\SourceShare1</p> |
| STŘÍBRNÁ |<p>C:\ClusterStorage\SourceVolume2</p><p>\\FileServer\SourceShare2</p> | |
| Chicago |GOLD_TARGET |<p>C:\ClusterStorage\TargetVolume1</p><p>\\FileServer\TargetShare1</p> |
| SILVER_TARGET |<p>C:\ClusterStorage\TargetVolume2</p><p>\\FileServer\TargetShare2</p> | |

Tato tabulka shrnuje chování, pokud povolíte ochranu pro virtuální počítače (VM1 - VM5) v tomto příkladu prostředí.

| **Virtuální počítač** | **Úložiště zdroje.** | **Zdroj klasifikace** | **Mapovat cílového úložiště** |
| --- | --- | --- | --- |
| VM1 |C:\ClusterStorage\SourceVolume1 |ZLATÝ |<p>C:\ClusterStorage\SourceVolume1</p><p>\\\FileServer\SourceShare1</p><p>Obě GOLD_TARGET</p> |
| VIRTUÁLNÍHO POČÍTAČE 2 |\\FileServer\SourceShare1 |ZLATÝ |<p>C:\ClusterStorage\SourceVolume1</p><p>\\FileServer\SourceShare1</p> <p>Obě GOLD_TARGET</p> |
| VM3 |C:\ClusterStorage\SourceVolume2 |STŘÍBRNÁ |<p>C:\ClusterStorage\SourceVolume2</p><p>\FileServer\SourceShare2</p> |
| VM4 |\FileServer\SourceShare2 |STŘÍBRNÁ |<p>C:\ClusterStorage\SourceVolume2</p><p>\\FileServer\SourceShare2</p><p>Obě SILVER_TARGET</p> |
| VM5 |C:\ClusterStorage\SourceVolume3 |Není k dispozici |Žádné mapování, takže se používá výchozí umístění úložiště pro hostitele Hyper-V |



### <a name="data-privacy-overview"></a>Přehled dat o ochraně osobních údajů

Tato tabulka shrnuje, jak jsou uložena data v tomto scénáři:

- - -
| Akce | **Podrobnosti** | **Shromážděná data** | **Použití** | **Požadované** |
| --- | --- | --- | --- | --- |
| **Registrace** | Zaregistrujte VMM server v trezoru služeb zotavení. Pokud chcete později zrušit registraci serveru, můžete tak učinit odstraněním informace o serveru z portálu Azure. | Po registraci serveru VMM shromažďuje, procesy, Site Recovery a přenese metadata o serverem VMM a názvy cloudech VMM detekovaných službou Site Recovery. | Data se používá k identifikaci a komunikovat s příslušný server VMM a nakonfigurovat nastavení pro příslušné cloudy VMM. | Tato funkce se vyžaduje. Pokud chcete odesílat výše uvedené informace pro obnovení lokality byste neměli používat služby Site Recovery. |
| **Povolení replikace** | Zprostředkovatele Azure Site Recovery je nainstalován na serveru VMM a je kanál pro komunikaci se službou Site Recovery. Zprostředkovatel je dynamická knihovna (DLL) hostované v procesu VMM. Po instalaci zprostředkovatele bude tato funkce "Obnovení Datacentra" získá povolena v konzole pro správu VMM. Nové a stávající virtuální počítače můžete povolit tuto funkci povolit ochranu pro virtuální počítač. |S touto sadou vlastností odešle zprostředkovatel název a ID virtuálního počítače pro obnovení lokality.  Ve Windows Server 2012 nebo Windows Server 2012 R2 Hyper-V repliky je povolena replikace. Získá data virtuálního počítače replikují z jednoho hostitele technologie Hyper-V na jiný (obvykle umístěné v datovém centru různých "obnovení"). |Site Recovery metadata používá k naplnění informace o virtuálních počítačů na portálu Azure. | Tato funkce je základní součástí služby a nelze po zapnutí vypnout. Pokud nechcete, aby odesílat výše uvedené informace, nemáte povolení ochrany Site Recovery pro virtuální počítače. Všimněte si, že všechna data odesílaná zprostředkovatelem Site Recovery je zaslaného prostřednictvím protokolu HTTPS. |
| **Plán obnovení** | Plány obnovení pomoct vám vytvořit plán orchestration pro obnovení datového centra. Můžete definovat pořadí, ve kterém má být spuštěn virtuální počítače nebo skupiny virtuálních počítačů v lokalitě pro obnovení. Můžete také zadat všechny automatizované skripty být spustit nebo všechny ručně prováděné akce, které mají být provedeny v době obnovení pro každý virtuální počítač. Převzetí služeb při selhání se obvykle aktivuje na úrovni plán obnovení pro koordinované obnovení. | Site Recovery shromažďuje, procesy a odesílá metadata pro plán obnovení, včetně metadata virtuálního počítače a metadata všechny skripty pro automatizaci a poznámky k ruční akce. |Metadata slouží k vytvoření plánu obnovení na portálu Azure. |Tato funkce je základní součástí služby a nelze po zapnutí vypnout. Pokud nechcete, aby odesílat výše uvedené informace pro obnovení lokality, nevytvářejte plány obnovení. |
| **Mapování sítě** | Mapuje informace o síťové z primární datové centrum pro obnovení datového centra. Při obnovení virtuálních počítačů v lokalitě pro obnovení, pomáhá mapování sítě v navazování připojení k síti. |Site Recovery shromažďuje, procesy a odesílá metadata logické sítě pro každou lokalitu (primární i datacenter). |Metadata jsou používána k naplnění nastavení sítě, takže můžete namapovat informace o síti. | Tato funkce je základní součástí služby a nelze po zapnutí vypnout. Pokud nechcete, aby odesílat výše uvedené informace pro obnovení lokality, nepoužívejte mapování sítě. |
| **Převzetí služeb při selhání (plánované/neplánované/test)** | Převzetí služeb při selhání převezme virtuálních počítačů z jedné spravovat nástroj VMM datového centra na jiný. Akce převzetí služeb při selhání se aktivuje ručně na portálu Azure. |Zprostředkovatel na serveru VMM je upozornění události převzetí služeb při selhání pomocí Site Recovery a spouští akce převzetí služeb při selhání na hostiteli technologie Hyper-V prostřednictvím rozhraní VMM. Skutečné převzetí služeb při selhání virtuálního počítače je z jednoho hostitele technologie Hyper-V na jiný a zpracováván Windows Server 2012 nebo Windows Server 2012 R2 Hyper-V repliky. Zádi Site Recovery používá informace odesílané do naplnit stav informace o převzetí služeb při selhání akce na portálu Azure. | Tato funkce je základní součástí služby a nelze po zapnutí vypnout. Pokud nechcete, aby odesílat výše uvedené informace pro obnovení lokality, nepoužívejte převzetí služeb při selhání. |

## <a name="next-steps"></a>Další kroky

Po nasazení otestujete, další informace o dalších typů [převzetí služeb při selhání](site-recovery-failover.md)
