---
title: "Nastavit zotavení po havárii pro místní virtuální počítače Hyper-V do Azure s Azure Site Recovery | Microsoft Docs"
description: "Zjistěte, jak nastavit zotavení po havárii místní virtuální počítače Hyper-V do Azure se službou Azure Site Recovery."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 45e9b4dc-20ca-4c14-bee3-cadb8d9b8b24
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/18/2017
ms.author: raynew
ms.openlocfilehash: 4d43fb03ce1c54a47315b8c3a5c83ec2082bcab9
ms.sourcegitcommit: d41d9049625a7c9fc186ef721b8df4feeb28215f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/02/2017
---
# <a name="set-up-disaster-recovery-of-on-premises-hyper-v-vms-to-azure"></a>Nastavit zotavení po havárii místní virtuální počítače Hyper-V do Azure

[Azure Site Recovery](site-recovery-overview.md) služby přispívá ke strategie zotavení po havárii správy a Orchestrace replikace, převzetí služeb při selhání a navrácení služeb po obnovení místního počítače a virtuální počítače Azure (VM).

V tomto kurzu se dozvíte, jak nastavit zotavení po havárii místní virtuální počítače Hyper-V do Azure. Tento kurz je relevantní pro virtuální počítače Hyper-V, které jsou spravované v cloudech System Center Virtual Machine Manager (VMM) a ty, které nejsou. V tomto kurzu se naučíte:

> [!div class="checklist"]
> * Nastavení Azure a místní požadavky
> * Vytvoření trezoru služeb zotavení pro Site Recovery 
> * Nastavit zdroje a cíle replikace prostředí 
> * Nastavit mapování sítě (Pokud je technologie Hyper-V spravované nástrojem System Center VMM)
> * Vytvoření zásady replikace
> * Povolení replikace pro virtuální počítač


## <a name="prerequisites"></a>Požadavky

Pro absolvování tohoto kurzu potřebujete:

- Ujistěte se, že rozumíte [scénář architektura a komponenty](concepts-hyper-v-to-azure-architecture.md).
- Zkontrolujte [podporu požadavků](site-recovery-support-matrix-to-azure.md) pro všechny součásti.
- Ujistěte se, že chcete replikovat virtuální počítače v souladu s [požadavky virtuálního počítače Azure](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).
- Zjistěte, plánování kapacity:
    - Použití [nástroje Plánovač kapacity technologie Hyper-V](http://aka.ms/asr-capacity-planner-excel)a pokuste se zjistit klidové vytížení.
    - Použití [lokality obnovení Capacity Planner](http://aka.ms/asr-capacity-planner-excel) k analýze na zdrojové prostředí, odhad šířky pásma a cíle požadavky.
- Příprava Azure. Potřebujete předplatné Azure, virtuální sítě Azure a účet úložiště.
- Příprava místního hostitele Hyper-V a servery VMM, je-li to relevantní.





### <a name="set-up-an-azure-account"></a>Nastavit účet Azure

Získat Microsoft [účet Azure](http://azure.microsoft.com/).

- Můžete začít s [bezplatnou zkušební verzí](https://azure.microsoft.com/pricing/free-trial/).
- Další informace o [cenách za Site Recovery](site-recovery-faq.md#pricing)a získat [podrobnosti o cenách](https://azure.microsoft.com/pricing/details/site-recovery/).
- Zjistit, které [oblasti jsou podporované](https://azure.microsoft.com/pricing/details/site-recovery/) Site Recovery.

### <a name="verify-azure-account-permissions"></a>Ověřte oprávnění pro účet Azure

Ujistěte se, že váš účet Azure má oprávnění potřebná k replikaci virtuálních počítačů.

- Zkontrolujte [oprávnění](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines) je potřeba replikovat počítače do Azure
- Ověřte a upravte [přístupu podle rolí](../active-directory/role-based-access-control-configure.md) oprávnění. 


### <a name="set-up-an-azure-network"></a>Nastavení sítě Azure

Nastavení [síť Azure](../virtual-network/virtual-network-get-started-vnet-subnet.md).

- Virtuální počítače Azure jsou umístěny v této síti, když jste vytvořili po převzetí služeb při selhání.
- Síť by měla být ve stejném umístění jako trezor služby Recovery Services.


### <a name="set-up-an-azure-storage-account"></a>Nastavení účtu úložiště Azure

Nastavení [účtu úložiště Azure](../storage/common/storage-create-storage-account.md#create-a-storage-account).

- Site Recovery replikuje místní počítače do úložiště Azure. Virtuální počítače Azure jsou vytvořené z úložiště po převzetí služeb při selhání.
- Účet úložiště musí být ve stejné oblasti jako trezor služeb zotavení.
- Účet úložiště může být standardní nebo [premium](../virtual-machines/windows/premium-storage.md).
- Pokud jste nastavili prémiový účet, potřebujete další standardní účet pro data protokolu.

### <a name="prepare-hyper-v-hosts"></a>Příprava hostitele Hyper-V

1. Ověřte, že splňují hostitele technologie Hyper-V [podporu požadavků](site-recovery-support-matrix-to-azure.md#support-for-datacenter-management-servers).
2. Ujistěte se, že hostitelé mají přístup k tyto adresy URL:

    [!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)]
    
    - Všechna pravidla brány firewall založená na adresu IP by měl povolit komunikaci s Azure.
    - Povolte [Rozsahy IP adres datového centra Azure](https://www.microsoft.com/download/confirmation.aspx?id=41653) a port HTTPS (443).
    - Povolte rozsahy IP adres pro oblast Azure svého předplatného a západní USA (používá se pro přístup k řízení a identity management).

### <a name="prepare-vmm-servers"></a>Připravit servery VMM

Pokud hostitele Hyper-V jsou spravovány nástrojem VMM, je nutné připravit na místním serveru VMM. 

- Ověřte, že VMM server splňuje [podporu požadavků](site-recovery-support-matrix-to-azure.md#support-for-datacenter-management-servers).
- Ujistěte se, že VMM server obsahuje minimálně jeden cloud, s jeden nebo více skupin hostitelů. Hostitel Hyper-V, na kterém běží virtuální počítače se musí nacházet v cloudu.
- VMM server potřebuje přístup k Internetu, přístup k následujícím adresám URL:

    [!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)]
    
    - Všechna pravidla brány firewall založená na adresu IP by měl povolit komunikaci s Azure.
    - Povolte [Rozsahy IP adres datového centra Azure](https://www.microsoft.com/download/confirmation.aspx?id=41653) a port HTTPS (443).
    - Povolte rozsahy IP adres pro oblast Azure svého předplatného a pro oblast Západní USA (používá se pro řízení přístupu a správu identit).
- Příprava serveru VMM pro mapování sítě.


#### <a name="prepare-vmm-for-network-mapping"></a>Příprava VMM mapování sítě

Pokud používáte VMM, [mapování sítě](site-recovery-network-mapping.md) mapy mezi místními sítěmi virtuálních počítačů nástroje VMM a virtuálních sítí Azure. Mapování zajistí, že virtuální počítače Azure připojeni k správného síťového, když jste vytvořili po převzetí služeb při selhání.

Připravte VMM mapování sítě následujícím způsobem:

1. Zajistěte, aby byla [logická síť VMM](https://docs.microsoft.com/system-center/vmm/network-logical) který je spojen s cloudem, ve které se nacházejí hostitelů Hyper-V.
2. Zajistěte, abyste měli [síť virtuálních počítačů](https://docs.microsoft.com/system-center/vmm/network-virtual) propojené k logické síti.
3. Virtuální počítače připojte k síti virtuálních počítačů.

## <a name="create-a-recovery-services-vault"></a>Vytvoření trezoru Služeb zotavení

[!INCLUDE [site-recovery-create-vault](../../includes/site-recovery-create-vault.md)]


## <a name="select-a-protection-goal"></a>Vyberte cíl ochrany

Vyberte obsah k replikovat a kam jej chcete replikovat.

1. V trezoru, klikněte na tlačítko **Site Recovery** > **Příprava infrastruktury** > **cíl ochrany**.
2. V **cíl ochrany**, vyberte **do Azure**> **Ano, s technologií Hyper-V**. 
    - Pokud hostitele Hyper-V nejsou spravované přes VMM, vyberte **ne** potvrďte, že nepoužíváte VMM.
    - Pokud jsou hostitelé spravované v cloudech VMM, vyberte **Ano**.

## <a name="set-up-the-source-environment"></a>Nastavení zdrojového prostředí

Při nastavování zdrojové prostředí, nainstalujte zprostředkovatele Azure Site Recovery a agenta služeb zotavení Azure a zaregistrujte na místní servery v trezoru. 

1. V **Příprava infrastruktury**, klikněte na tlačítko **zdroj**. 
    - Pokud nepoužíváte VMM, klikněte na tlačítko **serveru technologie Hyper-V +**a zadejte název lokality. Pak klikněte na tlačítko **+ technologie Hyper-V Server**a do lokality přidat hostitele nebo cluster.
    - Pokud používáte VMM, v **připravit zdroj**, klikněte na tlačítko **+ VMM** přidání serveru VMM. V **přidat Server**, zkontrolujte, zda **serveru System Center VMM** se zobrazí v **typ serveru**.
2. Stáhněte si komponenty zprostředkovatel a agent v závislosti na vašem prostředí.
    - Technologie Hyper-V pouze stáhněte si instalační soubor zprostředkovatele. Spuštění souboru na každém hostiteli technologie Hyper-V k instalaci poskytovatele i agenta.
        
        ![Zprostředkovatel bez VMM](./media/tutorial-hyper-v-to-azure/download-no-vmm.png)
    
    - Technologie Hyper-V s nástrojem VMM stáhněte si zprostředkovatel a agent samostatně. Spusťte instalaci zprostředkovatele na serveru VMM. Spusťte instalaci agenta na každém hostiteli technologie Hyper-V.
    
        ![Zprostředkovatel a agent nástroje VMM](./media/tutorial-hyper-v-to-azure/download-vmm.png)
    
3. Stáhnout registrační klíč trezoru Musíte to, když spustíte instalační program zprostředkovatele. Klíč je platný pět dní od jeho vygenerování.

### <a name="install-components"></a>Nainstalujte komponenty

Nainstalujte součásti jako summmarized v tabulce. 

**Komponenta** | **Podrobnosti** | **Technologie Hyper-V pouze** | **Technologie Hyper-V s nástrojem VMM**
--- | --- | --- | ---
**Zprostředkovatele Azure Site Recovery** | Orchestruje replikaci do Azure. | Nainstalujte na každém hostiteli technologie Hyper-V | Nainstalujte na VMM server
**Agent služeb zotavení** | Zpracovává replikaci dat | Nainstalujte na každém hostiteli technologie Hyper-V | Nainstalujte na hostitele Hyper-V


#### <a name="install-the-provider-on-hyper-v-without-vmm"></a>Nainstalujte zprostředkovatele na technologie Hyper-V bez VMM

Nainstalujte zprostředkovatele na každém hostiteli technologie Hyper-V, které jste přidali do lokality Hyper-V. Pokud instalujete na clusteru s podporou technologie Hyper-V, spusťte instalační program na každém uzlu clusteru. Instalace a registrace každého uzlu clusteru technologie Hyper-V zajistí, že jsou chráněné virtuální počítače, i v případě, že migraci mezi uzly.

1. V rámci **Microsoft Update** můžete vyjádřit výslovný souhlas s aktualizacemi, aby se aktualizace zprostředkovatele nainstalovaly v souladu s vašimi zásadami Microsoft Update.
2. V okně **Instalace** přijměte nebo změňte výchozí umístění instalace zprostředkovatele a klikněte na **Nainstalovat**.
4. V **nastavení trezoru**, klikněte na tlačítko **Procházet** a vyberte soubor klíče trezoru, který jste si stáhli. Zadejte předplatné Azure Site Recovery, název trezoru a Web Hyper-V, do které patří server technologie Hyper-V.
5. V **nastavení proxy serveru**, zadejte, jak se zprostředkovatel, který běží na serveru VMM nebo hostitele Hyper-V připojí k Site Recovery přes internet.
    * Přímé připojení, vyberte **připojit se přímo k Azure Site Recovery bez serveru proxy**.
    * Proxy server, vyberte **připojit k Azure Site Recovery pomocí proxy serveru**. V případě potřeby zadejte adresu proxy serveru, port a přihlašovací údaje.
6. Po server je zaregistrovaný v trezoru, klikněte na tlačítko **Dokončit**.

Načte Azure Site Recovery metadata ze serveru technologie Hyper-V a server se zobrazí v **infrastruktura Site Recovery** > **hostitelů Hyper-V**.


#### <a name="install-the-recovery-services-agent-on-hyper-v-host-without-vmm"></a>Nainstalujte agenta služeb zotavení na hostiteli Hyper-V bez VMM

Pokud používáte VMM ve vašem nasazení, spusťte instalaci agenta služeb zotavení na každém hostiteli technologie Hyper-V.

1. V nastavení Průvodce > **Kontrola předpokladů**, klikněte na tlačítko **Další**. Automaticky se nainstalují všechny chybějící požadované součásti.

    ![Požadavky agenta Služeb zotavení](./media/tutorial-hyper-v-to-azure/hyperv-agent-prerequisites.png)
3. V části **Nastavení instalace** přijměte nebo změňte umístění instalace a mezipaměti. Jednotka mezipaměti vyžaduje minimálně 5 GB úložiště. Doporučujeme, abyste na jednotku s 600 GB nebo více volného místa. Pak klikněte na **Nainstalovat**.
4. V **instalace**, po dokončení instalace, klikněte na **Zavřít** ukončíte průvodce.

##### <a name="set-up-internet-access-via-a-proxy"></a>Nastavení přístupu k Internetu prostřednictvím proxy serveru

Agent služeb zotavení na hostiteli technologie Hyper-V potřebuje internetový přístup k Azure pro replikaci virtuálního počítače. Pokud získáváte přístup k internetu prostřednictvím serveru proxy, nastavte ho následujícím způsobem:

1. Otevřete modul snap-in Microsoft Azure Backup konzoly MMC na hostiteli Hyper-V. Ve výchozím nastavení je k dispozici na ploše nebo C:\Program Files\Microsoft Azure Recovery Services Agent\bin\wabadmin zástupce služby Microsoft Azure Backup.
2. V modulu snap-in klikněte na **Změnit vlastnosti**.
3. Na **konfiguraci proxy serveru** zadejte informace o proxy serveru.

    ![Registrace agenta MARS](./media/tutorial-hyper-v-to-azure/mars-proxy.png)
4. Zkontrolujte, zda agent může dosáhnout [požadované adresy URL](#prepare-hyper-v-hosts).

#### <a name="install-the-provider-on-the-vmm-server-hyper-v-with-vmm"></a>Nainstalujte zprostředkovatele na serveru VMM (Hyper-V s nástrojem VMM)

1. V rámci **Microsoft Update** můžete vyjádřit výslovný souhlas s aktualizacemi, aby se aktualizace zprostředkovatele nainstalovaly v souladu s vašimi zásadami Microsoft Update.
2. V okně **Instalace** přijměte nebo změňte výchozí umístění instalace zprostředkovatele a klikněte na **Nainstalovat**. 
3. Nyní můžete zaregistrujte VMM server v trezoru. V **nastavení trezoru**, klikněte na tlačítko **Procházet** a vyberte soubor klíče trezoru, který jste si stáhli. Zadejte předplatné Azure Site Recovery a název trezoru.

    ![Registrace serveru](./media/tutorial-hyper-v-to-azure/provider-vault-settings.png)

4. V **nastavení proxy serveru**, zadejte, jak se zprostředkovatel, který běží na serveru VMM nebo hostitele Hyper-V připojí k Site Recovery přes internet.

    * Přímé připojení, vyberte **připojit se přímo k Azure Site Recovery bez serveru proxy**.
    * Proxy server, vyberte **připojit k Azure Site Recovery pomocí proxy serveru**. V případě potřeby zadejte adresu proxy serveru, port a přihlašovací údaje.
    - Účet VMM RunAs (DRAProxyAccount) se vytvoří automaticky pomocí přihlašovacích údajů, zadaný proxy serveru. Proxy server nakonfigurujte tak, aby tento účet bylo možné úspěšně ověřit. V konzole VMM lze upravovat nastavení účtu RunAs > **nastavení** > **zabezpečení** > **účty spustit jako**. Restartujte službu VMM k aktualizaci změn.
5. V **šifrování dat**, určete, zda chcete šifrovat všechna data odesílaná do Azure. Pokud vyberete tuto možnost obnovení lokality vydá certifikát. Budete potřebovat později na dešifrovat data tohoto certifikátu.
6. V **serveru VMM**, zadejte popisný název pro identifikaci serveru VMM v trezoru. V konfiguraci clusteru zadejte název role clusteru VMM. V **synchronizovat metadata cloudu s trezorem**vyberte, jestli chcete synchronizovat metadata pro všechny cloudy na serveru VMM v trezoru. Tuto akci stačí na každém serveru provést pouze jednou. Pokud nechcete, aby k synchronizaci veškerých cloudů, můžete toto políčko nechat nezaškrtnuté a synchronizovat každý cloud jednotlivě ve vlastnostech cloudu v konzole nástroje VMM.

Po dokončení registrace pomocí Azure Site Recovery je načíst metadata ze serveru a serveru VMM se zobrazí v **infrastruktura Site Recovery**.






## <a name="set-up-the-target-environment"></a>Nastavení cílového prostředí

Vyberte a ověřte cílové prostředky. 

1. Klikněte na tlačítko **připravit infrastrukturu** > **cíl**.
2. Vyberte předplatné a skupině prostředků, ve kterém se vytvoří virtuální počítače Azure po převzetí služeb při selhání. Vyberte model nasazení, který chcete použít pro virtuální počítače v Azure.

3. Site Recovery zkontroluje, že máte minimálně jednu kompatibilní síť a účet úložiště Azure.

## <a name="configure-network-mapping-with-vmm"></a>Konfigurace mapování sítě (s nástrojem VMM)

Pokud používáte VMM, nastavte síťové mapování.

1. V části **Infrastruktura Site Recovery** > **Mapování sítí** > **Mapování sítě** klikněte na ikonu **+Mapování sítě**.
2. V **přidání mapování sítě**, vyberte zdrojový server VMM. Vyberte **Azure** jako cíl.
3. Ověřte předplatné a model nasazení po převzetí služeb při selhání.
4. V **zdrojové síti**, vyberte zdrojovou síť virtuálních počítačů na místě.
5. V **Cílová síť**, vyberte síť Azure, ve které virtuální počítače Azure repliky budou umístěné, když jste vytvořili po převzetí služeb při selhání. Pak klikněte na **OK**.

    ![Mapování sítě](./media/tutorial-hyper-v-to-azure/network-mapping-vmm.png)

## <a name="create-a-replication-policy"></a>Vytvoření zásady replikace

1. Klikněte na tlačítko **připravit infrastrukturu** > **nastavení replikace** > **+ vytvořit a přidružit**.
2. V části **Vytvořit a přidružit zásady** zadejte název zásady.
3. V části **Frekvence kopírování** určete, jak často chcete replikovat rozdílová data po počáteční replikaci (každých 30 sekund, 5 minut nebo 15 minut).

    > [!NOTE]
    >  Frekvence každých 30 sekund se nepodporuje při replikaci do Storage úrovně Premium. Omezení je určeno počtem snímků na jeden objekt blob (100), který Storage úrovně Premium podporuje. [Další informace](../virtual-machines/windows/premium-storage.md#snapshots-and-copy-blob).

4. V **uchování bodu obnovení**, zadejte, jak dlouho bude interval uchovávání dat (v hodinách) pro každý bod obnovení. Chráněné počítače je možné obnovit do libovolného bodu v rámci tohoto intervalu.
5. V nastavení **Frekvence snímků konzistentní vzhledem k aplikacím** určete, jak často (1–12 hodin) se mají vytvářet body obnovení obsahující snímky konzistentní vzhledem k aplikacím. Technologie Hyper-V používá dva typy snímků:
    - **Standardní snímek**: je přírůstkový snímek celého virtuálního počítače.
    - **Snímky konzistentní s aplikací**: pořídí snímek dat aplikací ve virtuálním počítači v daném okamžiku. Služba Stínová kopie svazku (VSS) zajišťuje, že aplikace budou v konzistentním stavu při pořízení snímku. Povolení snímky konzistentní s aplikacemi ovlivňuje výkon aplikace na zdrojové virtuální počítače. Nastaví hodnotu, která je menší než počet dalších bodů obnovení, které nakonfigurujete.
6. V nastavení **Čas spuštění počáteční replikace** určete, kdy se má spustit počáteční replikace. Replikace se provede přes pásma vašeho internetového připojení, takže můžete chtít naplánovat mimo špičku.
7. V nastavení **Šifrovat úložiště dat ve službě Azure** určete, jestli chcete v úložišti Azure šifrovat neaktivní data. Pak klikněte na **OK**.

    ![Zásady replikace](./media/tutorial-hyper-v-to-azure/replication-policy.png)


Když vytvoříte novou zásadu je automaticky spojen s VMM cloud nebo web Hyper-V.

## <a name="enable-replication"></a>Povolení replikace


1. Klikněte na tlačítko **replikujte aplikaci** > **zdroj**. 
2. V **zdroj**, vyberte server lokality nebo službou VMM technologie Hyper-V nebo cloud. Pak klikněte na **OK**.
3. V **cíl**, ověřte Azure jako cíl, předplatné trezoru a modelu, kterou chcete použít v Azure po převzetí služeb při selhání.
4. Vyberte účet úložiště pro replikovaná data a síť Azure, ve kterém virtuální počítače Azure budou umístěné po převzetí služeb při selhání.
5. V **virtuální počítače** > **vyberte**, vyberte virtuální počítače, které chcete replikovat. Pak klikněte na **OK**.

 Můžete sledovat průběh **povolení ochrany** akce v **úlohy** > **úlohy Site Recovery**. Po **dokončení ochrany** úloha dokončena počáteční replikace je dokončena a virtuální počítač je připraven k převzetí služeb při selhání.

## <a name="next-steps"></a>Další kroky
[Spuštění postupu zotavení po havárii](tutorial-dr-drill-azure.md)
