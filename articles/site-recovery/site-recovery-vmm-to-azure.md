---
title: "Replikace virtuálních počítačů Hyper-V v cloudech VMM do Azure | Dokumentace Microsoftu"
description: "Orchestrace replikace, převzetí služeb při selhání a obnovení virtuálních počítačů Hyper-V spravovaných v cloudech VMM produktu System Center do Azure"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: jwhit
editor: tysonn
ms.assetid: 8e7d868e-00f3-4e8b-9a9e-f23365abf6ac
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 06/14/2017
ms.author: raynew
ms.translationtype: HT
ms.sourcegitcommit: 83f19cfdff37ce4bb03eae4d8d69ba3cbcdc42f3
ms.openlocfilehash: 958b61f5de732a882e0a2682b8dd4e18504a6ae7
ms.contentlocale: cs-cz
ms.lasthandoff: 08/21/2017

---
# <a name="replicate-hyper-v-virtual-machines-in-vmm-clouds-to-azure-using-site-recovery-in-the-azure-portal"></a>Replikace virtuálních počítačů Hyper-V v cloudech VMM do Azure pomocí služby Site Recovery na webu Azure Portal
> [!div class="op_single_selector"]
> * [Azure Portal](site-recovery-vmm-to-azure.md)
> * [Azure Classic](site-recovery-vmm-to-azure-classic.md)
> * [PowerShell – Resource Manager](site-recovery-vmm-to-azure-powershell-resource-manager.md)
> * [PowerShell – Classic](site-recovery-deploy-with-powershell.md)


Tento článek vám pomůže replikovat místní virtuální počítače Hyper-V spravované v cloudech System Center Virtual Machine Manager (VMM) do platformy Azure přes službu [Azure Site Recovery](site-recovery-overview.md) na webu Azure Portal.

Po přečtení tohoto článku můžete publikovat jakékoli dotazy nebo připomínky na jeho konci nebo na [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

Pokud chcete migrovat počítače do Azure (bez navrácení služeb po obnovení), najdete další informace v [tomto článku](site-recovery-migrate-to-azure.md).


## <a name="deployment-steps"></a>Kroky nasazení

Podle informací uvedených v tomto článku dokončete tento postup nasazení:


1. [Přečtěte si další informace](site-recovery-components.md) o architektuře pro toto nasazení. Dále si [můžete přečíst](site-recovery-hyper-v-azure-architecture.md), jak ve službě Site Recovery funguje replikace Hyper-V.
2. Ověřte požadavky a omezení.
3. Nastavte síť Azure a účty úložiště.
4. Připravte místní server VMM a hostitele Hyper-V.
5. Vytvořte trezor služby Recovery Services. Trezor obsahuje konfigurační nastavení a orchestruje replikaci.
6. Zadejte nastavení zdroje. Zaregistrujte server VMM v trezoru. Nainstalujte zprostředkovatele Azure Site Recovery na server VMM a na hostitele Hyper-V agenta Microsoft Recovery Services.
7. Nastavte cíl a replikaci.
8. Povolte replikaci pro virtuální počítače.
9. Otestujete převzetí služeb při selhání, abyste měli jistotu, že všechno funguje, jak má.



## <a name="prerequisites"></a>Požadavky


**Požadavek na podporu** | **Podrobnosti**
--- | ---
**Azure** | Přečtěte si o [požadavcích na Azure](site-recovery-prereq.md#azure-requirements).
**Místní servery** | Přečtěte si [další informace](site-recovery-prereq.md#disaster-recovery-of-hyper-v-vms-in-vmm-clouds-to-azure) o požadavcích pro místní server VMM a hostitele Hyper-V.
**Místní virtuální počítače Hyper-V** | Na virtuálních počítačích, které chcete replikovat, musí běžet [podporovaný operační systém](site-recovery-support-matrix-to-azure.md#support-for-replicated-machine-os-versions) a počítače musí splňovat [požadavky na Azure](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).
**Adresy URL Azure** | Server VMM potřebuje přístup k těmto adresám URL:<br/><br/> [!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)]<br/><br/> Pokud máte zavedená pravidla brány firewall založená na IP adrese, zkontrolujte, že tato pravidla umožňují komunikaci s Azure.<br/></br> Povolte [Rozsahy IP adres datového centra Azure](https://www.microsoft.com/download/confirmation.aspx?id=41653) a port HTTPS (443).<br/></br> Povolte rozsahy IP adres pro oblast Azure svého předplatného a pro oblast Západní USA (používá se pro řízení přístupu a správu identit).


## <a name="prepare-for-deployment"></a>Příprava nasazení
Při přípravě nasazení musíte:

1. [Nastavit síť Azure](#set-up-an-azure-network), ve které budou virtuální počítače umístěné po převzetí služeb při selhání.
2. [Nastavit účet úložiště Azure](#set-up-an-azure-storage-account) pro replikovaná data.
3. [Připravit server VMM](#prepare-the-vmm-server) pro nasazení Site Recovery.
4. Připravit se na mapování sítě. Nastavte sítě tak, abyste mohli nakonfigurovat mapování sítě během nasazování Site Recovery.

### <a name="set-up-an-azure-network"></a>Nastavení sítě Azure
Budete potřebovat síť Azure, ke které se připojí virtuální počítače Azure vytvořené po převzetí služeb při selhání.

* Síť by měla být ve stejném umístění jako trezor služby Recovery Services.
* V závislosti na modelu prostředků, který budete chtít použít pro virtuální počítače Azure, které převezmou služby po selhání, nastavíte síť Azure v [režimu Resource Manageru](../virtual-network/virtual-networks-create-vnet-arm-pportal.md) nebo [klasickém režimu](../virtual-network/virtual-networks-create-vnet-classic-pportal.md).
* Doporučujeme nastavit síť ještě před tím, než začnete. Pokud to neuděláte, budete to muset udělat při nasazení služby Site Recovery.
Sítě Azure používané pro Site Recovery se nedají [přesouvat](../azure-resource-manager/resource-group-move-resources.md) v rámci stejného předplatného ani mezi jinými předplatnými.

### <a name="set-up-an-azure-storage-account"></a>Nastavení účtu úložiště Azure
* Abyste mohli uchovávat data replikovaná do Azure, potřebujete účet Azure Storage úrovně Standard nebo Premium. [Storage úrovně Premium](../storage/common/storage-premium-storage.md) slouží pro virtuální počítače, které potřebují trvale vysoký výkon na vstupu a výstupu a nízkou latenci pro hostování intenzivních úloh vstupu a výstupu. Pokud pro ukládání replikovaných dat chcete používat účet Storage úrovně Premium, musíte mít také účet Storage úrovně Standard pro ukládání protokolů replikace, do kterých se zaznamenávají průběžné změny místních dat. Účet musí být ve stejné oblasti jako trezor Služeb zotavení.
* V závislosti na modelu prostředků, který budete chtít použít pro virtuální počítače Azure, které převezmou služby po selhání, nastavíte účet v [režimu Resource Manageru](../storage/common/storage-create-storage-account.md) nebo [klasickém režimu](../storage/common/storage-create-storage-account.md).
* Doporučujeme nastavit účet ještě před tím, než začnete. Pokud to neuděláte, budete to muset udělat při nasazení služby Site Recovery.
- Mějte na paměti, že účty úložiště používané pro Site Recovery se nedají [přesouvat](../azure-resource-manager/resource-group-move-resources.md) v rámci stejného předplatného ani mezi jinými předplatnými.

### <a name="prepare-the-vmm-server"></a>Příprava serveru VMM
* Zkontrolujte, že server VMM splňuje příslušné [požadavky](#prerequisites).
* Během nasazování Site Recovery můžete určit, že všechny cloudy na serveru VMM musí být dostupné na webu Azure Portal. Pokud chcete, aby se na portálu zobrazovaly pouze konkrétní cloudy, můžete toto nastavení povolit v cloudu pomocí konzoly pro správu VMM.

### <a name="prepare-for-network-mapping"></a>Příprava mapování sítě
Během nasazování Site Recovery musíte nastavit mapování sítě. Mapování sítě zajišťuje mapování mezi zdrojovými sítěmi virtuálních počítačů VMM a cílovými sítěmi Azure, aby mohlo proběhnout následující:

* Počítače, které předávají služby při selhání ve stejné síti, se musí být schopny připojit k sobě navzájem i v případě, že nepředají služby při selhání stejným způsobem nebo ve stejném plánu obnovení.
* Pokud je v cílové síti Azure nastavená síťová brána, virtuální počítače Azure se musí být schopny připojit k místním virtuálním počítačům.
* Abyste mohli nastavit mapování sítě, musíte provést následující:

  * Zajistěte, aby virtuální počítače na zdrojovém hostitelském serveru Hyper-V byly připojené k síti virtuálních počítačů ve VMM. Tato síť musí být propojená na logickou síť, která je přidružená ke cloudu.
  * Síť Azure, jak je popsána [výše](#set-up-an-azure-network)

## <a name="create-a-recovery-services-vault"></a>Vytvoření trezoru Služeb zotavení
1. Přihlaste se k webu [Azure Portal](https://portal.azure.com).
2. Klikněte na **Nový** > **Monitorování a správa** > **Backup a Site Recovery (OMS)**.

    ![Nový trezor](./media/site-recovery-vmm-to-azure/new-vault3.png)
3. Do pole **Název** zadejte popisný název pro identifikaci trezoru. Máte-li více předplatných, vyberte jedno z nich.
4. [Vytvořte skupinu prostředků](../azure-resource-manager/resource-group-template-deploy-portal.md) nebo vyberte existující skupinu. Zadejte oblast Azure. Počítače budou replikovány do této oblasti. Informace o tom, které oblasti jsou podporované, najdete v části s geografickou dostupností v tématu s [podrobnostmi o cenách Azure Site Recovery](https://azure.microsoft.com/pricing/details/site-recovery/).
5. Pokud chcete mít k trezoru rychlý přístup z řídicího panelu, klikněte na **Připnout na řídicí panel** > **Vytvořit trezor**.

    ![Nový trezor](./media/site-recovery-vmm-to-azure/new-vault.png)

Nový trezor se zobrazí v části **Řídicí panel** > **Všechny prostředky** a v hlavním okně **Trezory Recovery Services**.


## <a name="select-the-protection-goal"></a>Výběr cíle ochrany

Vyberte, jak chcete počítače replikovat a kam je chcete replikovat.

1. V okně **Trezory Recovery Services** vyberte trezor.
2. V části **Začínáme** klikněte na **Site Recovery** > **Připravit infrastrukturu** > **Cíl ochrany**.

    ![Zvolte cíle.](./media/site-recovery-vmm-to-azure/choose-goals.png)
3. V části **Cíl ochrany** vyberte **To Azure** (Do Azure) a vyberte **Yes, with Hyper-V** (Ano, s technologií Hyper-V). Výběrem možnosti **Ano** potvrďte, že ke správě hostitelů technologie Hyper-V a lokality obnovení používáte VMM. Pak klikněte na **OK**.

## <a name="set-up-the-source-environment"></a>Nastavení zdrojového prostředí

Nainstalujte zprostředkovatele Azure Site Recovery na server VMM a zaregistrujte serveru v trezoru. Nainstalujte agenta Služeb zotavení Azure na hostitele Hyper-V.

1. Klikněte na **Připravit infrastrukturu** > **Zdroj**.

    ![Nastavení zdroje](./media/site-recovery-vmm-to-azure/set-source1.png)

2. V okně **Připravit zdroj** klikněte na **+ VMM** a přidejte server VMM.

    ![Nastavení zdroje](./media/site-recovery-vmm-to-azure/set-source2.png)

3. V okně **Přidat server** zkontrolujte, že se v části **Typ serveru** zobrazuje **server System Center VMM** a že server VMM splňuje [obecné požadavky a požadavky na adresu URL](#prerequisites).
4. Stáhněte si instalační soubor zprostředkovatele Azure Site Recovery.
5. Stáhněte si registrační klíč. Budete ho potřebovat, když spustíte instalaci. Klíč je platný pět dní od jeho vygenerování.

    ![Nastavení zdroje](./media/site-recovery-vmm-to-azure/set-source3.png)


## <a name="install-the-provider-on-the-vmm-server"></a>Instalace zprostředkovatele na server VMM

1. Na serveru VMM spusťte instalační soubor zprostředkovatele.
2. V rámci **Microsoft Update** můžete vyjádřit výslovný souhlas s aktualizacemi, aby se aktualizace zprostředkovatele nainstalovaly v souladu s vašimi zásadami Microsoft Update.
3. V okně **Instalace** přijměte nebo změňte výchozí umístění instalace zprostředkovatele a klikněte na **Nainstalovat**.

    ![Umístění instalace](./media/site-recovery-vmm-to-azure/provider2.png)
4. Po dokončení instalace klikněte na **Zaregistrovat**, aby se server VMM zaregistroval v trezoru.
5. Na stránce **Nastavení trezoru** klikněte na **Procházet** a vyberte soubor klíče trezoru. Určuje předplatné Azure Site Recovery a název trezoru.

    ![Registrace serveru](./media/site-recovery-vmm-to-azure/provider10.PNG)
6. V části **Připojení k Internetu** určete, jak se zprostředkovatel, který běží na serveru VMM, připojí k Site Recovery přes internet.

   * Pokud chcete, aby se zprostředkovatel připojil přímo, vyberte **Připojit k Azure Site Recovery přímo bez proxy serveru**.
   * Pokud váš existující proxy server vyžaduje ověření nebo chcete použít vlastní proxy server, vyberte **Připojit k Azure Site Recovery pomocí proxy serveru**.
   * Pokud budete používat vlastní proxy server, musíte zadat adresu, port a přihlašovací údaje.
   * Pokud používáte proxy server, měli byste už mít povolené adresy URL popisované v [požadavcích](#on-premises-prerequisites).
   * Pokud používáte vlastní proxy server, vytvoří se automaticky účet VMM RunAs (DRAProxyAccount) pomocí zadaných přihlašovacích údajů proxy serveru. Proxy server nakonfigurujte tak, aby tento účet bylo možné úspěšně ověřit. Nastavení účtu VMM RunAs můžete upravovat v konzole VMM. V části **Nastavení** rozbalte položku **Zabezpečení** > **Účty Spustit jako** a potom změňte heslo pro DRAProxyAccount. Budete muset službu VMM restartovat, aby se toto nastavení projevilo.

     ![Internet](./media/site-recovery-vmm-to-azure/provider13.PNG)
7. Přijměte nebo změňte umístění certifikátu SSL, který je automaticky generován pro šifrování dat. Tento certifikát se používá, pokud povolíte šifrování dat pro cloud chráněný platformou Azure na portálu Azure Site Recovery. Uchovávejte tento certifikát v bezpečí. Při spuštění předání služeb při selhání do Azure ho budete potřebovat k dešifrování, pokud je povolené šifrování dat.

    > [!NOTE]
    > K šifrování neaktivních uložených dat doporučujeme používat funkci šifrování, kterou poskytuje Azure, namísto používání možnosti šifrování dat, kterou poskytuje Azure Site Recovery. Funkci šifrování, kterou poskytuje Azure, je možné zapnout pro účet úložiště a pomáhá dosahovat lepšího výkonu, protože o šifrování a dešifrování se stará úložiště Azure.
    > [Další informace o šifrování služby Storage z Azure](https://docs.microsoft.com/en-us/azure/storage/storage-service-encryption).
    
8. Do pole **Název serveru** zadejte popisný název, který bude identifikovat server VMM v trezoru. V konfiguraci clusteru zadejte název role clusteru VMM.
9. Pokud chcete synchronizovat metadata pro všechny cloudy na serveru VMM v trezoru, zaškrtněte políčko **Synchronizovat metadata cloudu**. Tuto akci stačí na každém serveru provést pouze jednou. Pokud nechcete provádět synchronizaci se všemi cloudy, můžete toto políčko nechat nezaškrtnuté a synchronizovat každý cloud jednotlivě ve vlastnostech cloudu v konzole VMM. Kliknutím na **Zaregistrovat** proces dokončete.

    ![Registrace serveru](./media/site-recovery-vmm-to-azure/provider16.PNG)
10. Spustí se registrace. Po dokončení registrace se server zobrazí v části **Infrastruktura Site Recovery** > **Servery VMM**.


## <a name="install-the-azure-recovery-services-agent-on-hyper-v-hosts"></a>Instalace agenta Služeb zotavení Azure na hostitele Hyper-V

1. Po nastavení zprostředkovatele si musíte stáhnout instalační soubor pro agenta Recovery Services. Spusťte instalační program na každém serveru Hyper-V v cloudu VMM.

    ![Lokality Hyper-V](./media/site-recovery-vmm-to-azure/hyperv-agent1.png)
2. Na stránce **Kontrola předpokladů** klikněte na **Další**. Automaticky se nainstalují všechny chybějící požadované součásti.

    ![Požadavky agenta Služeb zotavení](./media/site-recovery-vmm-to-azure/hyperv-agent2.png)
3. V části **Nastavení instalace** přijměte nebo změňte umístění instalace a mezipaměti. Mezipaměť můžete nakonfigurovat na jednotce, na které je minimálně 5 GB dostupného úložiště. Pro mezipaměť ale doporučujeme disk minimálně s 600 GB volného místa. Pak klikněte na **Nainstalovat**.
4. Po dokončení instalace klikněte na **Zavřít**.

    ![Registrace agenta MARS](./media/site-recovery-vmm-to-azure/hyperv-agent3.png)

### <a name="command-line-installation"></a>Instalace pomocí příkazového řádku
Agenta Služeb zotavení Microsoft Azure můžete nainstalovat z příkazového řádku pomocí následujícího příkazu:

     marsagentinstaller.exe /q /nu

### <a name="set-up-internet-proxy-access-to-site-recovery-from-hyper-v-hosts"></a>Nastavení internetového přístupu proxy serveru k Site Recovery z hostitelů Hyper-V

Agent Služeb zotavení, který běží na hostitelích Hyper-V, potřebuje internetový přístup k Azure pro replikaci virtuálního počítače. Pokud získáváte přístup k internetu prostřednictvím serveru proxy, nastavte ho následujícím způsobem:

1. Otevřete modul snap-in Microsoft Azure Backup konzoly MMC na hostiteli Hyper-V. Ve výchozím nastavení je na ploše nebo v umístění C:\Program Files\Microsoft Azure Recovery Services Agent\bin\wabadmin zástupce služby Microsoft Azure Backup.
2. V modulu snap-in klikněte na **Změnit vlastnosti**.
3. Na kartě **Konfigurace proxy** zadejte informace o proxy serveru.

    ![Registrace agenta MARS](./media/site-recovery-vmm-to-azure/mars-proxy.png)
4. Zkontrolujte, že má agent přístup k adresám URL popsaným v části s [požadavky](#on-premises-prerequisites).

## <a name="set-up-the-target-environment"></a>Nastavení cílového prostředí
Zadejte účet úložiště Azure, který se má používat pro replikaci, a síť Azure, ke které se virtuální počítače Azure připojí po převzetí služeb při selhání.

1. Klikněte na **Připravit infrastrukturu** > **Cíl** a vyberte předplatné a skupinu prostředků, ve kterých chcete vytvořit virtuální počítače, pro které bylo provedeno převzetí služeb při selhání. Vyberte model nasazení (Classic nebo Resource Manager), který chcete v Azure použít pro virtuální počítače, pro které bylo provedeno převzetí služeb při selhání.

    ![Úložiště](./media/site-recovery-vmm-to-azure/enablerep3.png)

2. Site Recovery zkontroluje, že máte minimálně jednu kompatibilní síť a účet úložiště Azure.

    ![Úložiště](./media/site-recovery-vmm-to-azure/compatible-storage.png)

3. Pokud jste ještě nevytvořili účet úložiště a chcete ho vytvořit pomocí Resource Manageru, klikněte na **+ Účet úložiště** a můžete to provést přímo tady.  V okně **Vytvořit účet úložiště** zadejte název účtu, typ, předplatné a umístění. Účet by měl být ve stejném umístění jako trezor Služeb zotavení.

   ![Úložiště](./media/site-recovery-vmm-to-azure/gs-createstorage.png)


   * Pokud chcete vytvořit účet úložiště pomocí klasického modelu, můžete to udělat na webu Azure Portal. [Další informace](../storage/common/storage-create-storage-account.md)
   * Pokud pro replikovaná data používáte účet úložiště Storage úrovně Premium, nastavte si ještě účet úložiště úrovně Standard pro ukládání protokolů replikace, do kterých se zaznamenávají průběžné změny místních dat.
5. Pokud jste ještě nevytvořili síť Azure a chcete ji vytvořit pomocí modelu Resource Manageru, klikněte na **+Síť** a můžete to provést přímo tady. V okně **Vytvořit virtuální síť** zadejte název sítě, rozsah adres, podrobnosti o podsíti, předplatné a umístění. Síť by měla být ve stejném umístění jako trezor Služeb zotavení.

   ![Síť](./media/site-recovery-vmm-to-azure/gs-createnetwork.png)

   Pokud chcete vytvořit síť pomocí klasického modelu, udělejte to na portálu Azure Portal. [Další informace](../virtual-network/virtual-networks-create-vnet-classic-pportal.md).

### <a name="configure-network-mapping"></a>Konfigurace mapování sítě

* [Projděte si](#prepare-for-network-mapping) rychlý přehled toho, co mapování sítě dělá.
* Ověřte, že jsou virtuální počítače na serveru VMM připojené k síti virtuálních počítačů a že jste vytvořili aspoň jednu virtuální síť Azure. Na jednu síť Azure je možné namapovat několik sítí virtuálních počítačů.

Nakonfigurujte mapování následujícím způsobem:

1. V části **Infrastruktura Site Recovery** > **Mapování sítí** > **Mapování sítě** klikněte na ikonu **+Mapování sítě**.

    ![Mapování sítě](./media/site-recovery-vmm-to-azure/network-mapping1.png)
2. V části **Přidání mapování sítě** vyberte zdrojový server VMM a jako cíl vyberte **Azure**.
3. Ověřte předplatné a model nasazení po převzetí služeb při selhání.
4. V poli **Zdrojová síť** vyberte ze seznamu přidruženého k tomuto serveru VMM zdrojovou místní síť virtuálních počítačů, kterou chcete namapovat.
5. V seznamu **Cílová síť** vyberte síť Azure, ve které budou po vytvoření umístěné virtuální počítače Azure repliky. Pak klikněte na **OK**.

    ![Mapování sítě](./media/site-recovery-vmm-to-azure/network-mapping2.png)

Když se začne mapovat síť, dojde k tomuto:

* Když začne mapování, existující virtuální počítače ve zdrojové síti virtuálních počítačů jsou připojené k cílové síti. Nové virtuální počítače připojené ke zdrojové síti virtuálních počítačů jsou po replikaci připojené k namapované síti Azure.
* Pokud upravíte existující mapování sítě, virtuální počítače repliky se připojí pomocí nového nastavení.
* Pokud má cílová síť více podsítí a jedna z těchto podsítí má stejný název jako podsíť, ve které je umístěný zdrojový virtuální počítač, pak se virtuální počítač repliky po převzetí služeb při selhání připojí k této cílové podsíti.
* Pokud neexistuje žádná cílová podsíť s odpovídajícím názvem, připojí se virtuální počítač k první podsíti v síti.

## <a name="configure-replication-settings"></a>Konfigurace nastavení replikace
1. Pokud chcete vytvořit novou zásadu replikace, klikněte na **Připravit infrastrukturu** > **Nastavení replikace** > **+Vytvořit a přidružit**.

    ![Síť](./media/site-recovery-vmm-to-azure/gs-replication.png)
2. V části **Vytvořit a přidružit zásady** zadejte název zásady.
3. V části **Frekvence kopírování** určete, jak často chcete replikovat rozdílová data po počáteční replikaci (každých 30 sekund, 5 minut nebo 15 minut).

    > [!NOTE]
    >  Frekvence každých 30 sekund se nepodporuje při replikaci do Storage úrovně Premium. Omezení je určeno počtem snímků na jeden objekt blob (100), který Storage úrovně Premium podporuje. [Další informace](../storage/common/storage-premium-storage.md#snapshots-and-copy-blob)

4. V části **Uchování bodu obnovení** zadejte (v hodinách), jak dlouhý bude interval uchovávání dat pro jednotlivé body obnovení. Chráněné počítače je možné obnovit do libovolného bodu v rámci tohoto intervalu.
5. V nastavení **Frekvence snímků konzistentní vzhledem k aplikacím** určete, jak často (1–12 hodin) se mají vytvářet body obnovení obsahující snímky konzistentní vzhledem k aplikacím. Technologie Hyper-V používá dva typy snímků – standardní snímek, což je přírůstkový snímek celého virtuálního počítače, a snímek konzistentní vzhledem k aplikacím, který vytvoří snímek dat aplikací ve virtuálním počítači v daném okamžiku. Snímky konzistentní vzhledem k aplikacím využívají službu Stínová kopie svazku (VSS), aby bylo zajištěno, že aplikace budou při pořízení snímku v konzistentním stavu. Pokud povolíte snímky konzistentní vzhledem k aplikacím, bude to mít vliv na výkon aplikací běžících na zdrojových virtuálních počítačích. Zajistěte, aby byla hodnota, kterou nastavíte, menší než počet dalších bodů obnovení, které nakonfigurujete.
6. V nastavení **Čas spuštění počáteční replikace** určete, kdy se má spustit počáteční replikace. Při replikaci se využívá šířka pásma vašeho internetového připojení, může být proto vhodné naplánovat ji na dobu mimo špičky.
7. V nastavení **Šifrovat úložiště dat ve službě Azure** určete, jestli chcete v úložišti Azure šifrovat neaktivní data. Pak klikněte na **OK**.

    ![Zásady replikace](./media/site-recovery-vmm-to-azure/gs-replication2.png)
8. Když vytvoříte novou zásadu, automaticky se přidruží ke cloudu VMM. Klikněte na tlačítko **OK**. K této zásadě replikace můžete přidružit další cloudy VMM (a virtuální počítače v nich) v části **Replikace** > název_zásady > **Přidružit cloud VMM**.

    ![Zásady replikace](./media/site-recovery-vmm-to-azure/policy-associate.png)

## <a name="capacity-planning"></a>Plánování kapacity

Teď, když máte nastavenou základní infrastrukturu, začněte přemýšlet o plánování kapacity a zjistěte, jestli nepotřebujete další prostředky.

Site Recovery nabízí plánovač kapacity, který vám pomůže přidělit správné prostředky pro vaše zdrojové prostředí, součásti Site Recovery, sítě a úložiště. Plánovač můžete spustit v rychlém režimu pro odhady založené na průměrném počtu virtuálních počítačů/disků a úložišti nebo v podrobném režimu, ve kterém budete zadávat hodnoty na úrovni zpracovávaných úloh. Než začnete, potřebujete:

* Získat informace o prostředí replikace, včetně virtuální počítačů, disků na virtuální počítač a úložišti na disk.
* Odhad míry každodenních změn s ohledem na replikovaná data. Může vám s tím pomoct nástroj [Capacity planner for Hyper-V Replica](https://www.microsoft.com/download/details.aspx?id=39057).

1. Kliknutím na **Stáhnout** si nástroj stáhněte a pak ho spusťte. [Přečtěte si doprovodný článek](site-recovery-capacity-planner.md) k nástroji.
2. Po dokončení vyberte pro otázku **Have you run the Capacity Planner** (Spustili jste plánovač kapacity) odpověď **Yes** (Ano).

   ![Plánování kapacity](./media/site-recovery-vmm-to-azure/gs-capacity-planning.png)

   Přečtěte si další informace o [řízení šířky pásma sítě](#network-bandwidth-considerations)




## <a name="enable-replication"></a>Povolení replikace

Než začnete, zkontrolujte, že váš uživatelský účet Azure má požadovaná [oprávnění](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines) k replikaci nového virtuálního počítače do Azure.

Teď následujícím způsobem povolte replikaci:

1. Klikněte na **Krok 2: Replikujte aplikaci** > **Zdroj**. Po prvním povolení replikace kliknutím na **+Replikovat** v trezoru povolte replikaci pro další počítače.

    ![Povolení replikace](./media/site-recovery-vmm-to-azure/enable-replication1.png)
2. V okně **Zdroj** vyberte server VMM a cloud, ve kterém jsou umístění hostitelé technologie Hyper-V. Pak klikněte na **OK**.

    ![Povolení replikace](./media/site-recovery-vmm-to-azure/enable-replication-source.png)
3. V nastavení **Cíl** vyberte předplatné, model nasazení po převzetí služeb při selhání a účet úložiště, který používáte pro replikovaná data.

    ![Povolení replikace](./media/site-recovery-vmm-to-azure/enable-replication-target.png)
4. Vyberte účet úložiště, který chcete použít. Pokud chcete použít jiný účet úložiště, než ten, který máte, musíte si ho [vytvořit](#set-up-an-azure-storage-account). Pokud pro replikovaná data používáte účet Storage úrovně Premium, musíte si nastavit ještě účet Storage úrovně Standard pro ukládání protokolů replikace, do kterých se zaznamenávají průběžné změny místních dat. Pokud chcete vytvořit účet úložiště pomocí modelu Resource Manageru, klikněte na **Vytvořit nový**. Pokud chcete vytvořit účet úložiště pomocí klasického modelu, můžete to udělat [na webu Azure Portal](../storage/common/storage-create-storage-account.md). Pak klikněte na **OK**.
5. Vyberte síť Azure a podsíť, ke kterým se připojí virtuální počítače Azure, když se po převzetí služeb při selhání vytvoří. Výběrem možnosti **Nakonfigurovat pro vybrané počítače** použijte nastavení sítě pro všechny počítače, které jste vybrali pro ochranu. Vyberte **Nakonfigurovat později** a vyberte síť Azure pro konkrétní počítač. Pokud chcete použít jinou síť než tu, kterou máte, musíte si ji [vytvořit](#set-up-an-azure-network). Pokud chcete vytvořit síť pomocí modelu Resource Manageru, klikněte na **Vytvořit nový**. Pokud chcete vytvořit síť pomocí klasického modelu, udělejte to [na portálu Azure Portal](../virtual-network/virtual-networks-create-vnet-classic-pportal.md). V odpovídajícím případě vyberte podsíť. Pak klikněte na **OK**.
6. V nastavení **Virtuální počítače** > **Výběr virtuálních počítačů** klikněte a vyberte každý počítač, který chcete replikovat. Můžete vybrat pouze počítače, pro které je možné povolit replikaci. Pak klikněte na **OK**.

    ![Povolení replikace](./media/site-recovery-vmm-to-azure/enable-replication5.png)

7. V nastavení **Vlastnosti** > **Konfigurace vlastností** vyberte operační systém pro vybrané virtuální počítače a disk operačního systému.

    - Ověřte, že název virtuálního počítače Azure (název cíle) splňuje [požadavky na virtuální počítače Azure](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).   
    - Ve výchozím nastavení jsou pro replikaci vybrány všechny disky virtuálního počítače, ale zrušením zaškrtnutí můžete požadované disky z replikace vyloučit.

        - Disky můžete vyloučit třeba proto, abyste zmenšili šířku pásma replikace. Možná nebudete chtít replikovat disky s dočasnými daty nebo daty, která se obnovují při každém restartování počítače nebo aplikace (jako je například soubor pagefile.sys nebo databáze tempdb Microsoft SQL Serveru). Disk můžete z replikace vyloučit zrušením výběru disku.
        - Vyloučit můžete jenom základní disky. Disky operačního systému vyloučit nelze.
        - Doporučujeme, abyste nevylučovali dynamické disky. Site Recovery nemůže zjistit, jestli je virtuální pevný disk ve virtuálním počítači hosta základní nebo dynamický. Pokud nejsou vyloučeny všechny disky se závislými dynamickými svazky, chráněné dynamické disky se budou při převzetí služeb při selhání na virtuálním počítači zobrazovat jako chybné a data na takovém disku budou nepřístupná.
        - Po povolení replikace už není možné přidávat nebo odebírat disky pro replikaci. Pokud chcete přidat nebo vyloučit disk, budete muset zakázat ochranu virtuálního počítače a potom ji znovu povolit.
        - Disky, které ručně vytvoříte v Azure, nebude možné po navrácení služeb obnovit. Například pokud provedete převzetí služeb při selhání u tří disků a dva disky vytvoříte přímo ve virtuálním počítači Azure, po obnovení z Azure do Hyper-V se navrátí pouze tři disky, u nichž se provedlo převzetí služeb při selhání. Ručně vytvořené disky není možné zahrnout do navrácení služeb po obnovení ani do zpětné replikace z Hyper-V do Azure.
        - Pokud vyloučíte disk, který je nezbytný pro provoz aplikace, po převzetí služeb při selhání do Azure jej budete muset v Azure znovu ručně vytvořit, aby se replikovaná aplikace mohla spustit. Alternativně můžete do plánu obnovení integrovat službu Azure Automation, která disk vytvoří během převzetí služeb při selhání počítače.

    Kliknutím na **OK** uložte změny. Později můžete nastavit další vlastnosti.

    ![Povolení replikace](./media/site-recovery-vmm-to-azure/enable-replication6-with-exclude-disk.png)

8. V **Nastavení replikace** > **Konfigurace nastavení replikace** vyberte zásadu replikace, kterou chcete použít pro chráněné virtuální počítače. Pak klikněte na **OK**. Zásady replikace můžete změnit v části **Zásady replikace** > název_zásady > **Upravit nastavení**. Změny, které použijete, se použijí pro počítače, které už replikujete, a nové počítače.

   ![Povolení replikace](./media/site-recovery-vmm-to-azure/enable-replication7.png)

Průběh úlohy **Zapnout ochrany** můžete sledovat v části **Úlohy** > **Úlohy Site Recovery**. Po spuštění úlohy **Dokončit ochranu** je počítač připravený k převzetí služeb při selhání.

### <a name="view-and-manage-vm-properties"></a>Zobrazení a správa vlastností virtuálního počítače

Doporučujeme ověřit vlastnosti zdrojového počítače. Mějte na paměti, že název virtuálního počítače Azure musí být v souladu s [požadavky na virtuální počítače Azure](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).

1. V části **Chráněné položky** klikněte na **Replikované položky** a vyberte počítač. Zobrazí se jeho podrobnosti.

    ![Povolení replikace](./media/site-recovery-vmm-to-azure/vm-essentials.png)
2. V části **Vlastnosti** můžete zobrazit informace o replikaci a převzetí služeb při selhání pro virtuální počítač.

    ![Povolení replikace](./media/site-recovery-vmm-to-azure/test-failover2.png)
3. V části **Výpočty a síť** > **Výpočetní vlastnosti** můžete zadat název a cílovou velikost virtuálního počítače Azure. Podle potřeby upravte název tak, aby byl souladu s [požadavky Azure](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements). Můžete také zobrazit a upravit informace o cílové síti, podsíti a IP adrese, která je přiřazená k virtuálnímu počítači Azure.
Poznámky:

   * Můžete nastavit cílovou IP adresu. Pokud adresu nezadáte, bude počítač, který převezme služby při selhání, používat DHCP. Pokud nastavíte adresu, která není k dispozici pro převzetí služeb při selhání, převzetí služeb při selhání se nezdaří. Stejnou cílovou IP adresu je možné použít pro testovací převzetí služeb při selhání, pokud je adresa k dispozici v testovací síti převzetí služeb při selhání.
   * Počet síťových adaptérů závisí na velikosti, kterou zadáte pro cílový virtuální počítač, a to následujícím způsobem:

     * Pokud je počet síťových adaptérů na zdrojovém počítači menší nebo roven počtu adaptérů, které jsou povolené pro velikost cílového počítače, pak bude mít cíl stejný počet adaptérů jako zdroj.
     * Pokud počet adaptérů pro zdrojový virtuální počítač překračuje počet povolený pro cílovou velikost, použije se maximální velikost cíle.
     * Pokud má například zdrojový počítač dva síťové adaptéry a velikost cílového počítače podporuje čtyři, bude mít cílový počítač dva adaptéry. Pokud má zdrojový počítač dva adaptéry, ale podporovaná velikost cíle podporuje pouze jeden, bude mít cílový počítač pouze jeden adaptér.     
     * Pokud má virtuální počítač více síťových adaptérů, připojí se všechny ke stejné síti.

     ![Povolení replikace](./media/site-recovery-vmm-to-azure/test-failover4.png)

4. V části **Disky** uvidíte operační systém a datové disky ve virtuálním počítači, který bude replikován.

#### <a name="managed-disks"></a>Managed Disks

V části **Výpočty a síť** > **Výpočetní vlastnosti** můžete pro virtuální počítač nastavit Použít spravované disky na hodnotu Ano, pokud chcete spravované disky připojit k počítači při migraci do Azure. Spravované disky zjednodušují správu disků virtuálních počítačů Azure IaaS tím, že spravují účty úložiště přidružené k diskům virtuálních počítačů. [Přečtěte si další informace o spravovaných discích](https://docs.microsoft.com/en-us/azure/storage/storage-managed-disks-overview).

   - Spravované disky se vytvoří a připojí k virtuálnímu počítači jenom v případě, že při selhání služby převezme Azure. Když zapnete ochranu, data z místních počítačů se budou i nadále replikovat do účtů úložiště.
   Spravované disky je možné vytvořit pouze pro virtuální počítače nasazené pomocí modelu nasazení Resource Manageru.  

  > [!NOTE]
  > U počítačů se spravovanými disky se momentálně nepodporuje navrácení služeb po obnovení z Azure do místního prostředí Hyper-V. Nastavte vlastnost Použít spravované disky na Ano jenom v případě, že máte v plánu tento počítač migrovat do Azure.

   - Když nastavíte vlastnost Použít spravované disky na Ano, budete moci vybrat jenom skupiny dostupnosti ve skupině prostředků, která má vlastnost Použít spravované disky nastavenou na Ano. Důvodem je to, že virtuální počítače se spravovanými disky můžou být součástí jenom skupin dostupnosti, které mají vlastnost Použít spravované disky nastavenou na Ano. Při vytváření skupin dostupnosti nezapomeňte vlastnost Použít spravované disky nastavit podle toho, jak chcete se spravovanými disky nakládat při převzetí služeb při selhání.  Stejně tak když nastavíte Použít spravované disky na Ne, budete moci vybrat jenom skupiny dostupnosti ve skupině prostředků, která má vlastnost Použít spravované disky nastavenou na Ne. [Přečtěte si další informace o spravovaných discích a skupinách dostupnosti](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/manage-availability#use-managed-disks-for-vms-in-an-availability-set).

  > [!NOTE]
  > Pokud byl někdy účet úložiště používaný pro replikaci zašifrován pomocí šifrování služby Storage, vytvoření spravovaných disků se během převzetí služeb při selhání nezdaří. Buď můžete vlastnost Použít spravované disky nastavit na Ne a pokusit se o převzetí služeb při selhání, nebo můžete zakázat ochranu virtuálního počítače a chránit ho pomocí účtu úložiště, který nikdy neměl povoleno šifrování služby Storage.
  > [Další informace o šifrování služby Storage a o spravovaných discích](https://docs.microsoft.com/en-us/azure/storage/storage-managed-disks-overview#managed-disks-and-encryption)


## <a name="test-the-deployment"></a>Otestování nasazení

Pokud budete chtít otestovat nasazení, můžete spustit test převzetí služeb při selhání pro jediný virtuální počítač nebo plán obnovení, který obsahuje jeden nebo více virtuálních počítačů.

### <a name="before-you-start"></a>Než začnete

 - Pokud se chcete po převzetí služeb při selhání připojit k virtuálním počítačům Azure pomocí protokolu RDP, přečtěte si informace o [přípravě připojení](site-recovery-test-failover-to-azure.md#prepare-to-connect-to-azure-vms-after-failover).
 - Abyste mohli převzetí služeb při selhání plně otestovat, musíte zkopírovat Active Directory a DNS do testovacího prostředí. [Další informace](site-recovery-active-directory.md#test-failover-considerations).

### <a name="run-a-test-failover"></a>Spuštění testovacího převzetí služeb při selhání

1. Pokud chcete převzít služby při selhání pro jeden virtuální počítač, klikněte v části **Replikované položky** na virtuální počítač > **+Testovací převzetí služeb při selhání**.
2. Pokud chcete pro převzetí služeb při selhání použít plán obnovení, klikněte v části **Plány obnovení** pravým tlačítkem myši na plán > **Otestovat převzetí služeb při selhání**. Pokud chcete vytvořit plán obnovení, [postupujte podle těchto pokynů](site-recovery-create-recovery-plans.md).
3. V části **Testovací převzetí služeb při selhání** vyberte síť Azure, ke které se virtuální počítače Azure připojí, když dojde k převzetí služeb při selhání.
4. Kliknutím na **OK** zahajte převzetí služeb při selhání. Pokud chcete sledovat průběh, otevřete kliknutím na virtuální počítač jeho vlastnosti, případně můžete kliknout na úlohu **Testovací převzetí služeb při selhání** v části **Úlohy Site Recovery**.
5. Po dokončení převzetí služeb při selhání by se vám také měl zobrazit počítač Azure repliky na portálu Azure Portal > **Virtuální počítače**. Měli byste zajistit, aby měl virtuální počítač odpovídající velikost, byl připojený k odpovídající síti a aby běžel.
6. Pokud jste připravili připojení po převzetí služeb při selhání, měli byste být schopni se k virtuálnímu počítači Azure připojit.
7. Až budete hotovi, klikněte v plánu obnovení na **Cleanup test failover** (Vyčistit po testu převzetí při selhání). V části **Poznámky** si zaznamenejte a uložte jakékoli připomínky související s testovacím převzetím služeb při selhání. Tím odstraníte virtuální počítače, které se vytvořily během testu.

Další podrobnosti najdete v článku [Testování převzetí služeb při selhání pomocí Azure](site-recovery-test-failover-to-azure.md).

## <a name="monitor-the-deployment"></a>Monitorování nasazení

Tady je postup, jak monitorovat nastavení konfigurace, stav a stav nasazení Site Recovery:

1. Klikněte na název trezoru. Tím se dostanete na řídicí panel **Základy**. V tomto řídicím panelu uvidíte úlohy Site Recovery, stav replikace, plány obnovení, stav serveru a události.  Řídicí panel **Základy** si můžete přizpůsobit, aby se na něm zobrazovaly dlaždice a rozložení, které jsou pro vás nejužitečnější, včetně stavu dalších trezorů Site Recovery a Backup.

    ![Základy](./media/site-recovery-vmm-to-azure/essentials.png)
2. V části **Stav** můžete monitorovat problémy na místních serverech (servery VMM nebo konfigurační servery) a události vyvolané službou Site Recovery za posledních 24 hodin.
3. Na dlaždicích **Replikované položky**, **Plány obnovení** a **Úlohy Site Recovery** můžete spravovat a monitorovat replikaci. Podrobnosti o úlohách si můžete zobrazit v části **Úlohy** > **Úlohy Site Recovery**.

## <a name="command-line-installation-for-the-azure-site-recovery-provider"></a>Instalace zprostředkovatele Azure Site Recovery pomocí příkazového řádku

Zprostředkovatele Azure Site Recovery je možné nainstalovat z příkazového řádku. Tuto metodu je možné použít k instalaci zprostředkovatele na jádro serveru pro Windows Server 2012 R2.

1. Stáhněte si instalační soubor zprostředkovatele a registrační klíč do složky. Příklad: C:\ASR.
2. Spuštěním těchto příkazů na příkazovém řádku se zvýšenými oprávněními vyextrahujte instalační program zprostředkovatele:

            C:\Windows\System32> CD C:\ASR
            C:\ASR> AzureSiteRecoveryProvider.exe /x:. /q
3. Spuštěním tohoto příkazu nainstalujte součásti:

            C:\ASR> setupdr.exe /i
4. Potom spuštěním těchto příkazů zaregistrujte server v trezoru:

        CD C:\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin
        C:\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin\> DRConfigurator.exe /r  /Friendlyname <friendly name of the server> /Credentials <path of the credentials file> /EncryptionEnabled <full file name to save the encryption certificate>       

Kde:

* **/Credentials**: Povinný parametr, který určuje, kde je umístěný soubor registračního klíče.  
* **/FriendlyName**: Povinný parametr pro název serveru hostitele technologie Hyper-V, který se zobrazí na portálu Azure Site Recovery.
* * **/EncryptionEnabled**: Volitelný parametr, který je možné použít při replikaci virtuálních počítačů Hyper-V v cloudech VMM do Azure. Zadejte, jestli chcete virtuální počítače v Azure šifrovat (šifrování neaktivních dat). Zkontrolujte, jestli má název souboru příponu **.pfx**. Šifrování je ve výchozím nastavení vypnuté.

    > [!NOTE]
    > K šifrování neaktivních uložených dat doporučujeme používat funkci šifrování, kterou poskytuje Azure, namísto používání možnosti šifrování (možnost EncryptionEnabled), kterou poskytuje Azure Site Recovery. Funkci šifrování, kterou poskytuje Azure, je možné zapnout pro účet úložiště a pomáhá dosahovat lepšího výkonu, protože o šifrování a dešifrování se stará úložiště  
    > Azure.
    > [Další informace o šifrování služby Storage v Azure](https://docs.microsoft.com/en-us/azure/storage/storage-service-encryption).
    
* **/proxyAddress**: Volitelný parametr, který určuje adresu proxy serveru.
* **/proxyport**: Volitelný parametr, který určuje port proxy serveru.
* **/proxyUsername** Volitelný parametr, který určuje uživatelské jméno proxy (pokud proxy server vyžaduje ověření).
* **/proxyPassword**: Volitelný parametr, který určuje heslo k ověření proxy serveru (pokud proxy server vyžaduje ověření).


### <a name="network-bandwidth-considerations"></a>Aspekty šířky pásma sítě
Pomocí nástroje Capacity Planner můžete vypočítat šířku pásma, kterou potřebujete pro replikaci (počáteční a pak rozdílovou). K řízení velikosti šířky pásma využívané pro replikaci máte tyto možnosti:

* **Omezení šířky pásma:** Přenos Hyper-V, který se replikuje do sekundární lokality, prochází konkrétním hostitelem Hyper-V. Šířku pásma na hostitelském serveru můžete omezit.
* **Optimalizace šířky pásma:** Šířku pásma používanou pro replikaci můžete ovlivnit pomocí několika klíčů registru.

#### <a name="throttle-bandwidth"></a>Omezení šířky pásma
1. Otevřete modul snap-in Microsoft Azure Backup konzoly MMC na hostitelském serveru Hyper-V. Ve výchozím nastavení je na ploše nebo v umístění C:\Program Files\Microsoft Azure Recovery Services Agent\bin\wabadmin zástupce služby Microsoft Azure Backup.
2. V modulu snap-in klikněte na **Změnit vlastnosti**.
3. Na kartě **Omezování** vyberte **Povolit omezování šířky pásma internetu u operací zálohování** a nastavte limity pro pracovní a nepracovní dobu. Platné rozsahy jsou 512 kB/s až 102 MB/s.

    ![Omezení šířky pásma](./media/site-recovery-vmm-to-azure/throttle2.png)

Pro nastavení omezování můžete také použít rutinu [Set OBMachineSetting](https://technet.microsoft.com/library/hh770409.aspx). Tady je ukázkový skript:

    $mon = [System.DayOfWeek]::Monday
    $tue = [System.DayOfWeek]::Tuesday
    Set-OBMachineSetting -WorkDay $mon, $tue -StartWorkHour "9:00:00" -EndWorkHour "18:00:00" -WorkHourBandwidth  (512*1024) -NonWorkHourBandwidth (2048*1024)

**Set-OBMachineSetting -NoThrottle** označuje, že není požadováno žádné omezování.

#### <a name="influence-network-bandwidth"></a>Ovlivnění šířky pásma sítě
Hodnota registru **UploadThreadsPerVM** řídí počet vláken, která se používají pro přenos dat (počáteční nebo rozdílové replikace) disku. Vyšší hodnota zvětšuje šířku pásma sítě využívané pro replikaci. Hodnota registru **DownloadThreadsPerVM** určuje počet vláken používaných pro přenos dat během navracení služeb po obnovení.

1. V registru přejděte na **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Replication**.

   * Upravením hodnoty **UploadThreadsPerVM** (nebo pokud neexistuje, pak klíč vytvořte) můžete řídit vlákna používaná pro replikaci disku.
   * Upravením hodnoty **DownloadThreadsPerVM** (pokud neexistuje, pak klíč vytvořte) můžete řídit vlákna používaná pro navrácení služeb (přenosu) po obnovení z Azure.
2. Výchozí hodnota je 4. V síti s „nadměrným zřízením“ je třeba tyto klíče registru změnit z výchozích hodnot. Maximum je 32. Monitorováním provozu hodnotu optimalizujte.

## <a name="next-steps"></a>Další kroky

Po dokončení počáteční replikace a po otestování nasazení můžete podle potřeby vyvolat převzetí služeb při selhání. Přečtěte si [další informace](site-recovery-failover.md) o různých typech převzetí služeb při selhání a o tom, jak je spustit.

