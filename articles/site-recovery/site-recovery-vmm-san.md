---
title: "Replikace virtuálních počítačů technologie Hyper-V v nástroji VMM se sítě SAN pomocí Azure Site Recovery | Microsoft Docs"
description: "Tento článek popisuje, jak replikovat virtuální počítače Hyper-V mezi dvěma lokalitami s Azure Site Recovery pomocí replikace SAN."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: jwhit
editor: 
ms.assetid: eb480459-04d0-4c57-96c6-9b0829e96d65
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/14/2017
ms.author: raynew
ms.openlocfilehash: 3df38802fcdc86e4553253d38c49faff455f873e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="replicate-hyper-v-vms-in-vmm-clouds-to-a-secondary-site-with-azure-site-recovery-by-using-san"></a>Replikace virtuálních počítačů technologie Hyper-V v cloudech VMM do sekundární lokality s Azure Site Recovery pomocí sítě SAN


Pomocí tohoto článku, pokud chcete nasadit [Azure Site Recovery](site-recovery-overview.md) ke správě replikace virtuálních počítačů technologie Hyper-V (spravované v cloudech System Center Virtual Machine Manager) na sekundární lokalita VMM pomocí Azure Site Recovery na portálu classic. Tento scénář není k dispozici v nové verzi portálu Azure.



Odeslat všechny komentáře na konci tohoto článku. Získejte odpovědi na technické dotazy v [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="why-replicate-with-san-and-site-recovery"></a>Proč replikovat pomocí sítě SAN a Site Recovery?

* Síť SAN představuje řešení podnikové úrovni, škálovatelné replikace tak, aby primární lokalitu obsahující technologie Hyper-V síti SAN můžete replikovat logických jednotek do sekundární lokality pomocí sítě SAN. Úložiště spravované nástrojem VMM, a se službou Site Recovery orchestrovat replikaci a převzetí služeb při selhání.
* Site Recovery ve spolupráci se několik [partnery úložiště sítě SAN](http://social.technet.microsoft.com/wiki/contents/articles/28317.deploying-azure-site-recovery-with-vmm-and-san-supported-storage-arrays.aspx) k zajištění replikace v úložiště Fibre Channel a iSCSI.  
* Použijte existující infrastrukturu sítě SAN k ochraně důležitých aplikací nasazených v clusterech technologie Hyper-V. Virtuální počítače, lze replikovat jako skupina tak, aby vícevrstvé aplikace mohou být přebrány konzistentně.
* Replikace sítě SAN zajišťuje konzistenci replikace v rámci různých vrstev aplikace s synchronizovaná replikace pro nízkou RTO a plánovaný bod obnovení a synchronního replikace pro vysokou flexibilitu (v závislosti na vaší možnosti pole úložiště).  
* Můžete spravovat úložiště sítě SAN v prostředcích infrastruktury VMM a použít SMI-S pro vyhledávání existující úložiště v nástroji VMM.  

Poznámky:
- Site-to-site s replikací pomocí sítě SAN, není k dispozici na portálu Azure. Je k dispozici na portálu classic. Nové trezory nelze vytvořit na portálu classic. Je možné udržovat existující trezorů.
- Replikace z sítě SAN do Azure se nepodporuje.
- Nelze replikovat sdílené soubory Vhdx nebo logické jednotky (LUN), které jsou přímo připojené k virtuálním počítačům prostřednictvím standardu iSCSI nebo Fibre Channel. Clustery hostů, lze replikovat.


## <a name="architecture"></a>Architektura

![Architektura sítě SAN](./media/site-recovery-vmm-san/architecture.png)

- **Azure**: nastavíte trezor Site Recovery na portálu Azure.
- **Úložiště SAN**: úložiště sítě SAN je spravován v prostředcích infrastruktury VMM. Přidejte zprostředkovatele úložiště, vytvoření klasifikací úložiště a nastavit fondy úložiště.
- **VMM nebo Hyper-V**: doporučujeme, aby server VMM v každé lokalitě. Nastavit privátní cloudy VMM a umístěte Clustery Hyper-V v tyto cloudy. Během nasazení zprostředkovatele Azure Site Recovery je nainstalován na každý server VMM a server je zaregistrovaný v trezoru. Zprostředkovatel komunikováním se službou Site Recovery pro správu replikace, převzetí služeb při selhání a navrácení služeb po obnovení.
- **Replikace**: Po nastavení úložiště v nástroji VMM a konfigurovat replikaci ve službě Site Recovery, se replikují mezi primárním a sekundárním úložiště SAN. Site Recovery se neodesílají žádná data replikace.
- **Převzetí služeb při selhání**: povolit převzetí služeb při selhání na portálu Site Recovery. Není nulové ztráty dat během převzetí služeb při selhání, protože je synchronní replikace.
- **Navrácení služeb po obnovení**: pro navrácení služeb po obnovení, povolíte zpětnou replikaci k přenosu rozdílové změny ze sekundární lokality do primární lokality. Po dokončení zpětné replikace spustíte z sekundární pro primární plánované převzetí služeb při selhání. Tato plánované převzetí služeb při selhání zastaví virtuální počítače repliky v sekundární lokalitě a spustí je v primární lokalitě.


## <a name="before-you-start"></a>Než začnete


**Požadavky** | **Podrobnosti**
--- | ---
**Azure** |Potřebujete účet [Microsoft Azure](https://azure.microsoft.com/). Můžete začít s [bezplatnou zkušební verzí](https://azure.microsoft.com/pricing/free-trial/). [Další informace](https://azure.microsoft.com/pricing/details/site-recovery/) o cenách za Site Recovery Vytvoření trezoru služby Azure Site Recovery ke konfiguraci a správu replikace a převzetí služeb při selhání.
**VMM** | Můžete použít jeden server VMM a replikovat mezi cloudy jiné, ale doporučujeme jeden VMM v primární lokalitě a jeden v sekundární lokalitě. VMM se dá nasadit jako fyzický nebo virtuální samostatný server nebo jako cluster. <br/><br/>Na serveru VMM by měl běžet System Center 2012 R2 nebo novějším s nejnovější kumulativní aktualizace.<br/><br/> Potřebujete alespoň jeden cloud nakonfigurované na primárním serveru VMM, které chcete chránit a k jednomu cloudu nakonfigurované na sekundárním serveru VMM, který chcete používat pro převzetí služeb při selhání.<br/><br/> Zdrojový cloud musí obsahovat jeden nebo více skupin hostitelů VMM.<br/><br/> Sada profilů kapacity technologie Hyper-V musí mít všechny cloudy VMM.<br/><br/> Další informace o nastavení cloudů VMM najdete v tématu [nasazení privátního cloudu virtuálních počítačů](https://technet.microsoft.com/en-us/system-center-docs/vmm/scenario/cloud-overview).
**Hyper-V** | Budete potřebovat minimálně jeden cluster technologie Hyper-V v primárních a sekundárních cloudech VMM.<br/><br/> Zdrojový cluster technologie Hyper-V musí obsahovat jeden nebo více virtuálních počítačů.<br/><br/> Skupiny hostitelů VMM v primárních a sekundárních lokalit musí obsahovat alespoň jeden z clusterů Hyper-V.<br/><br/>Na hostiteli a cílovým serverem, technologie Hyper-V musí být systém Windows Server 2012 nebo novějším s role Hyper-V a nainstalované nejnovější aktualizace.<br/><br/> Pokud používáte technologii Hyper-V v clusteru a máte statické IP adresy na základě clusteru, není automaticky vytvořit zprostředkovatele clusteru. Je nutné ho nakonfigurovat ručně. Další informace najdete v tématu [Příprava hostitelské clustery pro repliku technologie Hyper-V](https://www.petri.com/use-hyper-v-replica-broker-prepare-host-clusters).
**Úložiště SAN** | Můžete replikovat virtuální počítače hostovaný clusterovaný s úložiště iSCSI nebo kanál, nebo pomocí sdílených virtuálních pevných disků (vhdx).<br/><br/> Budete potřebovat dvě pole SAN: jeden v primární lokalitě a jeden v sekundární lokalitě.<br/><br/> Síťové infrastruktury měli nastavit mezi poli. Partnerský vztah a replikace by měl být nakonfigurovaný. Replikace licencí měli nastavit v souladu s požadavky na pole úložiště.<br/><br/>Nastavení sítě mezi servery hostitele technologie Hyper-V a pole úložiště tak, aby hostitele může komunikovat s logických jednotek úložiště pomocí standardu iSCSI nebo Fibre Channel.<br/><br/> Zkontrolujte [podporovaná pole úložišť](http://social.technet.microsoft.com/wiki/contents/articles/28317.deploying-azure-site-recovery-with-vmm-and-san-supported-storage-arrays.aspx).<br/><br/> Poskytovatelé SMI-S od výrobců pole úložiště by měly být nainstalovány a pole SAN se mají spravovat nástrojem zprostředkovatele. Nastavení zprostředkovatele podle pokynů výrobce.<br/><br/>Ujistěte se, že poskytovatel SMI-S tohoto pole je na serveru, který bude mít server VMM přístup přes síť s IP adresu nebo plně kvalifikovaný název domény.<br/><br/> Každé pole SAN musí mít jeden nebo více fondů úložiště k dispozici.<br/><br/> Primární server VMM by měla spravovat pole primární a sekundární server VMM by měla spravovat sekundární pole.
**Mapování sítě** | Nastavte mapování sítě, aby se po převzetí služeb při selhání optimálně umístit replikované virtuální počítače na sekundární hostitelské servery technologie Hyper-V, a tak, že jste připojení k příslušné sítě virtuálních počítačů. Pokud nenakonfigurujete mapování sítě, virtuální počítače replik se nepřipojí k žádné síti po převzetí služeb při selhání.<br/><br/> Ujistěte se, že VMM sítě jsou správně nakonfigurovaná tak, aby můžete nastavit mapování sítě během nasazování Site Recovery. V nástroji VMM by měl být virtuální počítače na zdrojovém hostiteli technologie Hyper-V připojen k síti virtuálních počítačů nástroje VMM. Tato síť musí být propojená na logickou síť, která je přidružená ke cloudu.<br/><br/> Cílový cloud by měl mít odpovídající síť virtuálních počítačů a je zase musí být propojena na odpovídající logické sítě, který je přidružen cílový cloud.<br/><br/>.

## <a name="step-1-prepare-the-vmm-infrastructure"></a>Krok 1: Příprava infrastruktury VMM
Příprava infrastruktury VMM, budete muset:

1. Ověřte cloudech VMM.
2. Integrovat a klasifikaci úložiště sítě SAN v nástroji VMM.
3. Vytvoření logické jednotky a přidělovat úložiště.
4. Vytvořte replikační skupiny.
5. Nastavení sítí virtuálních počítačů.

### <a name="verify-vmm-clouds"></a>Ověřte cloudech VMM

Ujistěte se, že vaše cloudech VMM je správně nastavena před zahájením nasazení Site Recovery.

### <a name="integrate-and-classify-san-storage-in-the-vmm-fabric"></a>Integrovat a klasifikaci úložiště sítě SAN v prostředcích infrastruktury VMM

1. V konzole VMM, přejděte na **Fabric** > **úložiště** > **přidat prostředky** > **zařízení úložiště**.
2. V **přidat zařízení úložiště** průvodci vyberte **vyberte typ poskytovatele úložiště** a vyberte **zařízení sítě SAN a NAS zjištěná a spravovaný pomocí poskytovatele SMI-S**.

    ![Typ poskytovatele](./media/site-recovery-vmm-san/provider-type.png)

3. Na **zadat protokol a adresu poskytovatele úložiště SMI-S** vyberte **CIMXML SMI-S** a zadejte nastavení pro připojení k poskytovateli.
4. V **zprostředkovatele IP adresu nebo plně kvalifikovaný název domény** a **TCP/IP port**, zadejte nastavení pro připojení k poskytovateli. Připojení protokolem SSL můžete použít pouze pro CIMXML SMI-S.

    ![Poskytovatel připojení](./media/site-recovery-vmm-san/connect-settings.png)
5. V **účet Spustit jako**, zadejte účet VMM spustit jako, který může přístup k poskytovateli, nebo vytvořit účet.
6. Na **shromáždit informace** stránky, VMM se pokusí automaticky zjistit a naimportovat informace o úložném zařízení. Pokud chcete opakovat zjišťování, klikněte na tlačítko **vyhledat poskytovatele**. Pokud proces zjišťování úspěšné, zjištěných úložných polí, fondy úložiště, výrobce a model, kapacity jsou uvedeny na stránce.

    ![Zjišťování úložiště](./media/site-recovery-vmm-san/discover.png)
7. V **vyberte fondy úložiště tak, aby umístěny pod správu a přiřaďte klasifikaci**, vyberte fondy úložišť, že VMM bude spravovat a přiřadit jim klasifikaci. Informace o logických jednotkách importovány z fondů úložiště. Vytvoření logické jednotky LUN, na základě aplikací, které je třeba chránit, jejich požadavky na kapacitu a požadavky na co musí replikovat společně.

    ![Klasifikace úložiště](./media/site-recovery-vmm-san/classify.png)

### <a name="create-luns-and-allocate-storage"></a>Vytvoření logické jednotky a přidělit úložiště

Vytvoření logické jednotky LUN, na základě aplikací, které je třeba chránit, požadavky na kapacitu a požadavky na co musí replikovat společně.

1. Po úložiště se zobrazí v rámci prostředí prostředků infrastruktury VMM, můžete [zřízení logické jednotky LUN](https://technet.microsoft.com/en-us/system-center-docs/vmm/manage/manage-storage-host-groups#create-a-lun-in-vmm).

     > [!NOTE]
     > Nepřidáte virtuální pevné disky pro virtuální počítač, který jsou povolené pro replikaci do logických jednotek. Nejsou-li tyto logické jednotky do replikační skupiny Site Recovery, nebudou zjištěna službou Site Recovery.
     >

2. Přidělte kapacita úložiště pro cluster hostitelů Hyper-V tak, aby VMM můžete nasadit do zřízené úložiště dat virtuálního počítače:

   * Před přidělení úložiště do clusteru, musíte přidělit do skupiny hostitelů VMM, na kterém se nachází clusteru. Další informace najdete v tématu [postup přidělení logických jednotek úložiště do skupiny hostitelů v nástroji VMM](https://technet.microsoft.com/library/gg610686.aspx) a [postup přidělení fondů úložiště do skupiny hostitelů v nástroji VMM](https://technet.microsoft.com/library/gg610635.aspx).
   * Přidělit kapacity úložiště do clusteru, jak je popsáno v [postup konfigurace úložiště na clusteru hostitelů Hyper-V v nástroji VMM](https://technet.microsoft.com/library/gg610692.aspx).

    ![Typ poskytovatele](./media/site-recovery-vmm-san/provider-type.png)
3. V **zadat protokol a adresu poskytovatele úložiště SMI-S**, vyberte **SMI-S CIMXML**. Zadejte nastavení pro připojení k poskytovateli. Pouze pro CIMXML SMI-S můžete použít připojení protokolem SSL.

    ![Poskytovatel připojení](./media/site-recovery-vmm-san/connect-settings.png)
4. V **účet Spustit jako**, zadejte účet VMM spustit jako, který může přístup k poskytovateli, nebo vytvořit účet.
5. V **shromáždit informace**, VMM se pokusí automaticky zjistit a naimportovat informace o úložném zařízení. Pokud budete muset opakovat, klikněte na tlačítko **vyhledat poskytovatele**. Při procesu zjišťování úspěšné, pole úložišť, fondy úložiště, výrobce a model, kapacity jsou uvedeny na stránce.

    ![Zjišťování úložiště](./media/site-recovery-vmm-san/discover.png)
7. V **vyberte fondy úložiště tak, aby umístěny pod správu a přiřaďte klasifikaci**, vyberte fondy úložišť, že VMM spravovat a přiřadit jim klasifikaci. Informace o logických jednotkách importovány z fondů úložiště.

    ![Klasifikace úložiště](./media/site-recovery-vmm-san/classify.png)


### <a name="create-replication-groups"></a>Vytvořte replikační skupiny

Vytvořte skupinu replikace, která zahrnuje všechny logické jednotky, které bude potřeba replikovat společně.

1. V konzole VMM otevřete **replikační skupiny** karta Vlastnosti pole úložiště a pak klikněte na **nový**.
2. Vytvořte replikační skupinu.

    ![Skupiny replikace sítě SAN](./media/site-recovery-vmm-san/rep-group.png)

### <a name="set-up-networks"></a>Nastavení sítě

Pokud chcete nakonfigurovat mapování sítě, postupujte takto:

1. V tématu mapování sítě Site Recovery.
2. Připravte sítě virtuálních počítačů v nástroji VMM:

   * [Nastavení logických sítí](https://technet.microsoft.com/en-us/system-center-docs/vmm/manage/manage-network-logical-networks).
   * [Nastavení sítí virtuálních počítačů](https://technet.microsoft.com/en-us/system-center-docs/vmm/manage/manage-network-vm-networks).


## <a name="step-2-create-a-vault"></a>Krok 2: Vytvoření trezoru

1. Přihlaste se k [portál Azure](https://portal.azure.com) ze serveru VMM, který chcete zaregistrovat v trezoru.
2. Rozbalte položku **datové služby** > **služeb zotavení**a potom klikněte na **trezor Site Recovery**.
3. Klikněte na **Vytvořit nový** > **Rychlé vytvoření**.
4. Jako **Název** zadejte popisný název pro identifikaci trezoru.
5. V rozevírací nabídce **Oblast** vyberte zeměpisnou oblast trezoru. Pokud chcete zkontrolovat oblasti jsou podporované, najdete v části [Azure Site Recovery podrobnosti o cenách](https://azure.microsoft.com/pricing/details/site-recovery/).
6. Klikněte na **Vytvořit trezor**.

    ![Nový trezor](./media/site-recovery-vmm-san/create-vault.png)

Podle informací na stavovém řádku si ověřte, že se trezor úspěšně vytvořil. Trezor bude uvedený jako **Active** v hlavním **služeb zotavení** stránky.

### <a name="register-the-vmm-servers"></a>Registrace serveru VMM

1. Otevřete **rychlý Start** stránku z **služeb zotavení** stránky. Rychlý Start můžete otevřít také kdykoli výběrem ikony.

    ![Ikona Rychlý start](./media/site-recovery-vmm-san/quick-start-icon.png)
2. V rozevíracím seznamu vyberte **mezi Hyper-V místní lokality pomocí replikace pole**.

    ![Registrační klíč](./media/site-recovery-vmm-san/select-san.png)
3. V **připravit servery VMM**, stáhněte si nejnovější verzi instalační soubor zprostředkovatele Azure Site Recovery.
4. Spusťte tento soubor na zdrojovém serveru VMM. Pokud VMM je nasazen v clusteru a instalujete zprostředkovatele poprvé, nainstalujte poskytovatele na aktivní uzel a dokončením instalace zaregistrujte VMM server v trezoru. Potom zprostředkovatele nainstalujte na ostatní uzly. Pokud zprostředkovatele upgradujete, budete muset upgradovat na všech uzlech, aby měli tutéž verzi zprostředkovatele.
5. Instalační program zkontroluje požadavky a žádosti o oprávnění k zastavení služby VMM, chcete-li spustit instalační program zprostředkovatele. Služba se automaticky restartuje po dokončení instalace. Na clusteru VMM budete vyzváni k zastavení Cluster role.
6. V **Microsoft Update**, můžete vyjádřit výslovný souhlas pro aktualizace a aktualizace zprostředkovatele se nainstalují souladu s vašimi zásadami Microsoft Update.

    ![Aktualizace Microsoft Update](./media/site-recovery-vmm-san/ms-update.png)

7. Ve výchozím umístění instalace poskytovatele je <SystemDrive>\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin. Klikněte na tlačítko **nainstalovat** zahájíte.

    ![Umístění instalace](./media/site-recovery-vmm-san/install-location.png)

8. Jakmile je poskytovatel nainstalován, klikněte na tlačítko **zaregistrovat** k registraci serveru VMM v trezoru.

    ![Instalace byla dokončena](./media/site-recovery-vmm-san/install-complete.png)

9. V **připojení k Internetu**, zadejte, jak se zprostředkovatel připojuje k Internetu. Vyberte **použít výchozí nastavení proxy serveru systému** Pokud chcete použít výchozí nastavení připojení k Internetu na serveru.

    ![Nastavení internetu](./media/site-recovery-vmm-san/proxy-details.png)

   * Pokud chcete používat vlastní proxy server, ho nastavte před instalací zprostředkovatele. Při konfiguraci nastavení vlastního proxy serveru, testovací běhy zkontrolujte připojení k proxy serveru.
   * Pokud používáte vlastní proxy server, nebo pokud váš výchozí proxy server vyžaduje ověření, měli byste zadat podrobnosti o proxy serveru, včetně adresu a port.
   * Požadované adresy URL musí být přístupný ze serveru VMM.
   * Pokud používáte vlastní proxy server, se automaticky vytvoří účet VMM spustit jako (DRAProxyAccount) pomocí přihlašovacích údajů k zadané proxy. Proxy server nakonfigurujte tak, aby tento účet může ověřit. Můžete upravit nastavení účtu spustit jako v konzole nástroje VMM (**nastavení** > **zabezpečení** > **účty spustit jako** > **DRAProxyAccount**). Služba nástroje VMM, aby tato změna se projeví po restartování.
10. V **registrační klíč**, vyberte klíč, který jste stáhli z portálu a zkopírovali do serveru VMM.
11. V poli **Název trezoru** ověřte název trezoru, ve kterém bude server zaregistrovaný.

    ![Registrace serveru](./media/site-recovery-vmm-san/vault-creds.png)
12. Nastavení šifrování se používá pouze pro nástroj VMM na Azure replikaci. Můžete ji ignorovat.

    ![Registrace serveru](./media/site-recovery-vmm-san/encrypt.png)
13. Do pole **Název serveru** zadejte popisný název, který bude identifikovat server VMM v trezoru. V konfiguraci clusteru zadejte název role clusteru VMM.
14. V **synchronizovat metadata cloudu počáteční**vyberte, jestli chcete synchronizovat metadata pro všechny cloudy na serveru VMM. Tuto akci stačí na každém serveru provést pouze jednou. Pokud nechcete provádět synchronizaci se všemi cloudy, můžete toto políčko nechat nezaškrtnuté a synchronizovat každý cloud jednotlivě ve vlastnostech cloudu v konzole VMM.

    ![Registrace serveru](./media/site-recovery-vmm-san/friendly-name.png)

15. Dokončete proces kliknutím na **Další**. Po registraci načte Azure Site Recovery metadata ze serveru VMM. Server se zobrazí v **servery** > **servery VMM** v trezoru.

### <a name="command-line-installation"></a>Instalace z příkazového řádku

Zprostředkovatele Azure Site Recovery lze nainstalovat také pomocí následující příkazový řádek. Tato metoda slouží k instalaci zprostředkovatele na jádro serveru pro Windows Server 2012 R2.

1. Stáhněte si instalační soubor zprostředkovatele a registrační klíč do složky. Příklad: C:\ASR.
2. Zastavte službu VMM.
3. Extrahujte instalační program zprostředkovatele. Jako správce spusťte tyto příkazy:

        C:\Windows\System32> CD C:\ASR
        C:\ASR> AzureSiteRecoveryProvider.exe /x:. /q
4. Nainstalujte zprostředkovatele:

        C:\ASR> setupdr.exe /i
5. Zaregistrujte zprostředkovatele:

        CD C:\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin
        C:\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin\> DRConfigurator.exe /r  /Friendlyname <friendly name of the server> /Credentials <path of the credentials file> /EncryptionEnabled <full file name to save the encryption certificate>         

Parametry:

* **/ Credentials**: vyžaduje se parametr pro umístění, ve kterém je umístěn soubor registračního klíče.  
* **/ FriendlyName**: vyžaduje se parametr pro název serveru hostitele technologie Hyper-V, který se zobrazí na portálu Azure Site Recovery.
* **/ EncryptionEnabled**: volitelný parametr použít pouze v případě replikace z nástroje VMM do Azure.
* **/proxyAddress**: Volitelný parametr, který určuje adresu proxy serveru.
* **/proxyport**: Volitelný parametr, který určuje port proxy serveru.
* **/ proxyusername**: volitelný parametr, který určuje uživatelské jméno proxy (pokud proxy server vyžaduje ověření).
* **/ proxypassword**: volitelný parametr, který určuje heslo pro ověřování pomocí serveru proxy (pokud proxy server vyžaduje ověření).

## <a name="step-3-map-storage-arrays-and-pools"></a>Krok 3: Mapování polí úložiště a fondy

Mapování polí primární a sekundární k určení, které fond úložiště sekundární přijímá data replikace z primární fondu. Mapování úložiště před konfigurací replikace, protože informace o mapování se používá, pokud povolíte ochranu pro replikační skupiny.

Než začnete, zkontrolujte, že se v trezoru objeví cloudech VMM. Cloudy jsou zjištěna při synchronizaci veškerých cloudů v průběhu instalace zprostředkovatele nebo při synchronizaci konkrétní cloudu v konzole VMM.

1. Klikněte na tlačítko **prostředky** > **úložiště serveru** > **mapy zdrojové a cílové pole**.
    ![Registrace serveru](./media/site-recovery-vmm-san/storage-map.png)

2. Vyberte pole úložišť v primární lokalitě a mapovat je na pole úložiště v sekundární lokalitě. V **fondy úložiště**, vyberte zdroj a cíl fondu úložiště pro mapování.

    ![Registrace serveru](./media/site-recovery-vmm-san/storage-map-pool.png)

## <a name="step-4-configure-replication-settings"></a>Krok 4: Konfigurace nastavení replikace

Po registraci servery VMM se nakonfigurujte nastavení ochrany cloudu.

1. Na **rychlý Start** klikněte na tlačítko **nastavení ochrany pro cloudy VMM**.
2. Na **chráněné položky** vyberte cloud **konfigurace**.
3. V **cíl**, vyberte **VMM**.
4. V **cílové umístění**, vyberte server VMM, který spravuje cloudu, kterou chcete použít pro obnovení.
5. V **cílí na cloud**, vyberte cílový cloud, kterou chcete použít pro převzetí služeb při selhání virtuálního počítače.
   * Doporučujeme vám, že vybíráte cílový cloud, který splňuje požadavky na obnovení pro virtuální počítače, který budete chránit.
   * Cloud lze přiřadit pouze jeden cloud pár – jako primární nebo cílový cloud.
6. Site Recovery ověřuje, že cloudy mají přístup k úložišti sítě SAN a že jsou mapované pole úložiště.
7. Pokud je ověření úspěšné, v **typ replikace**, vyberte **SAN**.

Po uložení nastavení se vytvoří úloha, která lze sledovat na **úlohy** kartě. Nastavení můžete změnit na **konfigurace** kartě. Pokud chcete upravit cílové umístění nebo cílový cloud, musíte odebrat konfiguraci cloudu a potom cloud znovu nakonfigurovat.

## <a name="step-5-enable-network-mapping"></a>Krok 5: Povolení mapování sítě

1. Na **rychlý Start** klikněte na tlačítko **mapovat sítě**.
2. Vyberte zdrojový server VMM a potom vyberte cílovém serveru VMM, ke kterému se mapovat sítě. Zobrazí se seznam zdrojové sítě a jejich přidružené cílové sítě. Pro sítě, které nejsou namapované je zobrazen na prázdnou hodnotu. Klikněte na informační ikonu u zdrojové a cílové názvy sítě zobrazíte podsítě pro každou síť.
3. Vyberte síť, v **sítě na zdroji**a potom klikněte na **mapy**. Služba zjistí sítě virtuálních počítačů na cílovém serveru a zobrazí je.

    ![Architektura sítě SAN](./media/site-recovery-vmm-san/network-map1.png)
4. Vyberte jeden ze sítě virtuálních počítačů z cílového serveru VMM.

    ![Architektura sítě SAN](./media/site-recovery-vmm-san/network-map2.png)

5. Když vyberete cílové sítě, zobrazí se počet chráněných cloudů, které používají zdrojové síti. Zobrazí se také k dispozici cílové sítě. Doporučujeme vám, že vyberete Cílová síť, která je k dispozici pro všechny cloudy, které používáte pro replikaci.
6. Kliknutím na značku zaškrtnutí dokončete proces mapování. Spustí úloha, která sleduje průběh. Můžete zobrazit na **úlohy** kartě.

## <a name="step-6-enable-replication-for-replication-groups"></a>Krok 6: Povolení replikace pro replikační skupiny

Než povolíte ochranu pro virtuální počítače, budete muset povolit replikaci pro replikační skupiny úložiště.

1. Na **vlastnosti** stránky primárního cloudu na portálu Site Recovery, otevřete **virtuální počítače** a klikněte na **přidat skupinu replikace**.
2. Vyberte jednu nebo více replikační skupiny VMM, které jsou spojené s cloudem, ověřte zdrojové a cílové pole a zadejte četnost replikace.

Site Recovery, VMM a zprostředkovatele SMI-S zřízení cílového webu úložiště logických jednotek a povolit replikaci úložiště. Je-li již replikovaná replikační skupiny, Site Recovery opětovně používá existující relaci replikace a aktualizuje informace.

## <a name="step-7-enable-protection-for-virtual-machines"></a>Krok 7: Povolení ochrany pro virtuální počítače


Při replikaci je skupina úložišť, povolte ochranu pro virtuální počítače v konzole nástroje VMM s jedním z následujících metod:

* **Nový virtuální počítač**: při vytváření virtuálního počítače, replikaci povolíte a přiřaďte virtuálního počítače k replikační skupině.
  Tato možnost používá VMM inteligentní umístění k optimální umístění úložiště virtuálního počítače na logické jednotky replikační skupiny. Site Recovery orchestruje vytvoření stínové virtuálního počítače v sekundární lokalitě a přiděluje kapacitu tak, že replikované virtuální počítače lze spustit po převzetí služeb při selhání.
* **Existující virtuální počítač**: Pokud virtuální počítač je už nasazená v nástroji VMM, můžete povolit replikaci a provést migraci úložiště do replikační skupiny. Po dokončení VMM a Site Recovery zjistit nový virtuální počítač a spustit správu ve službě Site Recovery. Stín virtuální počítač je vytvořen v sekundární lokalitě a přidělení kapacity tak, že lze spustit virtuální počítač repliky po převzetí služeb při selhání.

![Povolení ochrany](./media/site-recovery-vmm-san/enable-protect.png)

Když virtuální počítače jsou povolená pro replikaci, zobrazí se v konzole Site Recovery. Můžete zobrazit vlastnosti virtuálního počítače, sledovat stav a sledovat skupiny replikace převzetí služeb při selhání, které obsahují víc virtuálních počítačů. V replikaci sítě SAN všechny virtuální počítače přidružené skupině replikace musí převzetí služeb při selhání společně. Je to proto, že nastane převzetí služeb při selhání ve vrstvě úložiště dříve. Je důležité správně skupiny replikační skupiny a místní společně pouze přidružené virtuální počítače.

> [!NOTE]
> Po povolení replikace pro virtuální počítač, nepřidáte své virtuální pevné disky k logickým jednotkám Pokud se nacházejí v replikační skupině Site Recovery. Virtuální pevné disky se zjistit pomocí Site Recovery pouze v případě, že se nacházejí v replikační skupině Site Recovery.
>
>

Můžete sledovat průběh, včetně počáteční replikace **úlohy** kartě. Po spuštění úlohy dokončení ochrany, je připraven k převzetí služeb při selhání virtuálního počítače.

![Úloha ochrany virtuálního počítače](./media/site-recovery-vmm-san/job-props.png)

## <a name="step-8-test-the-deployment"></a>Krok 8: Testování nasazení

Otestujte nasazení zajistit, že selhání virtuálních počítačů přes podle očekávání. K tomuto účelu vytvořte plán obnovení a spustit testovací převzetí služeb.

1. Na **plány obnovení** , klikněte na **vytvořit plán obnovení**.
2. Zadejte název plánu obnovení a vyberte zdrojové a cílové servery VMM. Zdrojový server musí mít virtuální počítače, které jsou povolené pro převzetí služeb při selhání a obnovení. Vyberte **SAN** k zobrazení pouze cloudy, které jsou nakonfigurované pro replikaci sítě SAN.

    ![Vytvoření plánu obnovení](./media/site-recovery-vmm-san/r-plan.png)

3. V **vyberte virtuální počítače**, vyberte replikační skupiny. Všechny virtuální počítače přidružené ke skupině jsou přidány do plánu obnovení. Tyto virtuální počítače jsou přidány do výchozí skupiny plánu obnovení (skupina 1). V případě potřeby můžete přidat další skupiny. Po dokončení replikace virtuální počítače jsou číslované podle pořadí skupiny plánu obnovení.

    ![Vyberte virtuální počítače](./media/site-recovery-vmm-san/r-plan-vm.png)
4. Po vytvoření plánu obnovení se zobrazí v seznamu na **plány obnovení** kartě. Vyberte plán a zvolte **testovací převzetí služeb při selhání**.
5. Na **potvrzení převzetí služeb při selhání** vyberte **žádné**. Tato možnost povolena selhání replikované virtuální počítače se nepřipojí k žádné síti. To testy nezdaří virtuálních počítačů přes podle očekávání, že ji nebude testovacího prostředí sítě. Další informace o jiných možností sítě najdete v tématu [Site Recovery převzetí služeb při selhání](site-recovery-failover.md).

    ![Vyberte testovací síti](./media/site-recovery-vmm-san/test-fail1.png)

6. Testovací virtuální počítač je vytvořen na stejném hostiteli jako hostitele, ve kterém se nachází replice virtuálního počítače. Není přidáno do cloudu, ve kterém se nachází replice virtuálního počítače.
2. Po dokončení replikace repliku virtuálního počítače bude mít jinou IP adresu než primární virtuální počítač. Pokud vystavujete adresy ze serveru DHCP, se neaktualizují automaticky. Pokud nepoužíváte protokol DHCP a chcete stejné adresy, budete muset spustit několik skriptů.
3. Spusťte tento skript pro načtení IP adresu:

       $vm = Get-SCVirtualMachine -Name <VM_NAME>
       $na = $vm[0].VirtualNetworkAdapters>
       $ip = Get-SCIPAddress -GrantToObjectID $na[0].id
       $ip.address  

4. Spusťte tento ukázkový skript aktualizace DNS. Zadejte IP adresu, kterou jste získali.

       [string]$Zone,
       [string]$name,
       [string]$IP
       )
       $Record = Get-DnsServerResourceRecord -ZoneName $zone -Name $name
       $newrecord = $record.clone()
       $newrecord.RecordData[0].IPv4Address  =  $IP
       Set-DnsServerResourceRecord -zonename $zone -OldInputObject $record -NewInputObject $Newrecord


## <a name="next-steps"></a>Další kroky

Po spuštění převzetí služeb při selhání zkontrolujte, že vaše prostředí funguje podle očekávání, najdete v článku [Site Recovery převzetí služeb při selhání](site-recovery-failover.md) Další informace o různých typech převzetí služeb při selhání.
