---
title: "Replikace virtuálních počítačů Hyper-V v cloudech VMM do Azure | Dokumentace Microsoftu"
description: "Tento článek popisuje, jak replikovat virtuální počítače Hyper-V na hostitelích Hyper-V, které jsou umístěné v cloudech System Center VMM, do Azure."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: jwhit
editor: 
ms.assetid: 9d526a1f-0d8e-46ec-83eb-7ea762271db5
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 02/06/2017
ms.author: raynew
ms.translationtype: Human Translation
ms.sourcegitcommit: eeb56316b337c90cc83455be11917674eba898a3
ms.openlocfilehash: ed97f2aeb19566b12342e5194ac8a01293f453bf
ms.contentlocale: cs-cz
ms.lasthandoff: 04/03/2017


---
# <a name="replicate-hyper-v-virtual-machines-in-vmm-clouds-to-azure"></a>Replikace virtuálních počítačů Hyper-V v cloudech VMM do Azure
> [!div class="op_single_selector"]
> * [Azure Portal](site-recovery-vmm-to-azure.md)
> * [PowerShell – Resource Manager](site-recovery-vmm-to-azure-powershell-resource-manager.md)
> * [Portál Classic](site-recovery-vmm-to-azure-classic.md)
> * [PowerShell – Classic](site-recovery-deploy-with-powershell.md)
>
>

Služba Azure Site Recovery přispívá ke strategii zachování plynulého chodu podniku a zotavení po havárii (BCDR) tím, že orchestruje replikaci, převzetí služeb při selhání a obnovení virtuálních počítačů a fyzických serverů. Počítače je možné replikovat do Azure nebo do sekundárního místního datového centra. Rychlý přehled najdete v článku [Co je Azure Site Recovery?](site-recovery-overview.md)

## <a name="overview"></a>Přehled
Tento článek popisuje postup nasazení služby Size Recovery k replikaci virtuálních počítačů Hyper-V na hostitelských serverech technologie Hyper-V, které jsou na privátních cloudech VMM, do Azure.

Článek uvádí předpoklady pro scénář a ukazuje, jak nastavit trezor Site Recovery, získat zprostředkovatele Azure Site Recovery nainstalovaného na zdrojovém serveru VMM, zaregistrovat server v trezoru, přidat účet úložiště Azure, nainstalovat agenta Služeb zotavení Azure na hostitelských serverech technologie Hyper-V, nakonfigurovat nastavení ochrany pro cloudy VMM, které budou použité pro všechny chráněné virtuální počítače, a pak pro tyto virtuální počítače povolit ochranu. Nakonec otestujete převzetí služeb při selhání, abyste měli jistotu, že všechno funguje podle očekávání.

Jakékoli dotazy nebo připomínky můžete publikovat na konci tohoto článku nebo na [fóru služby Azure Site Recovery](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="architecture"></a>Architektura
![Architektura](./media/site-recovery-vmm-to-azure-classic/topology.png)

* Zprostředkovatel služby Azure Site Recovery je nainstalován na VMM během nasazování služby Site Recovery a VMM server je zaregistrován v trezoru Site Recovery. Zprostředkovatel komunikováním se službou Site Recovery zpracovává orchestraci replikace.
* Agent Služeb zotavení Azure je nainstalován na hostitelských serverech technologie Hyper-V během nasazování Site Recovery. Zpracovává replikaci dat do úložiště Azure.

## <a name="azure-prerequisites"></a>Požadavky Azure
Tady je seznam toho, co budete potřebovat v Azure.

| **Požadavek** | **Podrobnosti** |
| --- | --- |
| **Účet Azure** |Budete potřebovat účet [Microsoft Azure](https://azure.microsoft.com/). Můžete začít s [bezplatnou zkušební verzí](https://azure.microsoft.com/pricing/free-trial/). [Další informace](https://azure.microsoft.com/pricing/details/site-recovery/) o cenách za Site Recovery |
| **Úložiště Azure** |K ukládání replikovaných dat budete potřebovat účet úložiště Azure. Replikovaná data jsou uložena v úložišti Azure a virtuální počítače Azure se spouštějí, když je třeba převzít služby při selhání. <br/><br/>Potřebujete [účet se standardním geograficky redundantním úložištěm](../storage/storage-redundancy.md#geo-redundant-storage). Účet musí být ve stejné oblasti jako služba Site Recovery a musí být přidružený ke stejnému předplatnému. Poznámka: Replikace do účtů Storage úrovně Premium není aktuálně podporována a neměla by se používat.<br/><br/>[Přečtěte si informace o](../storage/storage-introduction.md) úložišti Azure. |
| **Síť Azure** |Budete potřebovat virtuální síť Azure, ke které se budou virtuální počítače Azure připojovat, když dojde k převzetí služeb při selhání. Virtuální síť Azure musí být ve stejné oblasti jako trezor Site Recovery. |

## <a name="on-premises-prerequisites"></a>Místní požadavky
Tady je seznam toho, co budete potřebovat místně.

| **Požadavek** | **Podrobnosti** |
| --- | --- |
| **VMM** |Budete potřebovat minimálně jeden server VMM nasazený jako fyzický nebo virtuální samostatný server nebo jako virtuální cluster. <br/><br/>Na serveru VMM by měl běžet System Center 2012 R2 s nejnovější kumulativními aktualizacemi.<br/><br/>Na serveru VMM budete muset mít nakonfigurovaný aspoň jeden cloud.<br/><br/>Zdrojový cloud, který chcete chránit, musí obsahovat minimálně jednu skupinu hostitelů VMM.<br/><br/>Další informace o nastavení cloudů najdete na blogu Keithema Mayera v příspěvku [Návod: vytvoření privátních cloudů pomocí produktu System Center 2012 SP1 VMM](http://blogs.technet.com/b/keithmayer/archive/2013/04/18/walkthrough-creating-private-clouds-with-system-center-2012-sp1-virtual-machine-manager-build-your-private-cloud-in-a-month.aspx). |
| **Hyper-V** |Budete potřebovat jeden nebo více hostitelských serverů Hyper-V nebo clusterů v cloudu VMM. Hostitelský server by měl mít a jeden nebo více virtuálních počítačů. <br/><br/>Na serveru Hyper-V musí běžet minimálně **Windows Server 2012 R2** s rolí Hyper-V nebo **Microsoft Hyper-V Server 2012 R2** a musí být na něm nainstalované nejnovější aktualizace.<br/><br/>Jakýkoli server Hyper-V obsahující virtuální počítače, které chcete chránit, musí být v cloudu VMM.<br/><br/>Pokud používáte technologii Hyper-V v clusteru, je důležité vědět, že zprostředkovatel clusteru se nevytvoří automaticky, pokud máte clustery založené na statických IP adresách. Budete ho muset nakonfigurovat ručně. [Další informace](https://www.petri.com/use-hyper-v-replica-broker-prepare-host-clusters) v najdete blogu Aidana Finna. |
| **Chráněné počítače** | Virtuální počítače, které chcete chránit, by měly splňovat [požadavky Azure](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements). |

## <a name="network-mapping-prerequisites"></a>Požadavky na mapování sítě
Při ochraně virtuálních počítačů v síti Azure mapování mapuje sítě virtuálních počítačů na zdrojovém serveru VMM a cílové sítě Azure, aby bylo možné následující:

* Všechny počítače, u kterých dojde k převzetí služeb při selhání ve stejné síti, se budou moct připojit k sobě navzájem, a to bez ohledu na to, v jakém jsou plánu obnovení.
* Pokud je v cílové síti Azure nastavená síťová brána, virtuální počítače se budou moct připojit k jiným místním virtuálním počítačům.
* Pokud nenakonfigurujete mapování sítě, budou se moct po předání služeb při selhání do Azure připojit k sobě navzájem pouze virtuální počítače, u kterých dojde k převzetí služeb při selhání v rámci stejného plánu obnovení.

Pokud chcete nasadit mapování sítě, budete potřebovat následující:

* Virtuální počítače, který chcete chránit na zdrojovém serveru VMM, musí být připojené k síti virtuálních počítačů. Tato síť musí být propojená na logickou síť, která je přidružená ke cloudu.
* Síť Azure, ke které se budou moct po převzetí služeb při selhání připojit replikované virtuální počítače. Tuto síť vyberete v okamžiku převzetí služeb při selhání. Síť musí být ve stejné oblasti jako vaše předplatné Azure Site Recovery.


Připravte sítě v nástroji VMM:

   * [Nastavení logických sítí](https://technet.microsoft.com/library/jj721568.aspx).
   * [Nastavení sítí virtuálních počítačů](https://technet.microsoft.com/library/jj721575.aspx).


## <a name="step-1-create-a-site-recovery-vault"></a>Krok 1: Vytvoření trezoru Site Recovery
1. Přihlaste se k [portálu pro správu](https://portal.azure.com) ze serveru VMM, který chcete zaregistrovat.
2. Klikněte na **Data Services** > **Služby zotavení** > **Trezor Site Recovery**.
3. Klikněte na **Vytvořit nový** > **Rychlé vytvoření**.
4. Jako **Název** zadejte popisný název pro identifikaci trezoru.
5. V rozevírací nabídce **Oblast** vyberte zeměpisnou oblast trezoru. Informace o tom, které oblasti jsou podporované, najdete v tématu s [podrobnostmi o cenách Azure Site Recovery](https://azure.microsoft.com/pricing/details/site-recovery/) v části s geografickou dostupností.
6. Klikněte na **Vytvořit trezor**.

    ![Nový trezor](./media/site-recovery-vmm-to-azure-classic/create-vault.png)

Podle informací na stavovém řádku si ověřte, že se trezor úspěšně vytvořil. Trezor bude uvedený jako **aktivní** na hlavní stránce Služeb zotavení.

## <a name="step-2-generate-a-vault-registration-key"></a>Krok 2: Vygenerování registračního klíče trezoru
Vygenerujte registrační klíč v trezoru. Po tom, co si stáhnete zprostředkovatele Azure Site Recovery a nainstalujete ho na server VMM, použijete tento klíč k registraci serveru VMM v trezoru.

1. Na stránce **Služby zotavení** kliknutím na trezor otevřete stránku Rychlý start. Stránku Rychlý Start je možné kdykoli otevřít pomocí ikony.

    ![Ikona Rychlý start](./media/site-recovery-vmm-to-azure-classic/qs-icon.png)
2. V rozevíracím seznamu vyberte **Mezi místní lokalitou VMM a Microsoft Azure**.
3. V části **Připravit servery VMM** klikněte na **Vygenerovat soubor registračního klíče**. Soubor klíče se vygeneruje automaticky a je platný po dobu 5 dní od vygenerování. Pokud k portálu Azure nepřistupujete ze serveru VMM, budete si muset tento soubor zkopírovat na server.

    ![Registrační klíč](./media/site-recovery-vmm-to-azure-classic/register-key.png)

## <a name="step-3-install-the-azure-site-recovery-provider"></a>Krok 3: Nainstalování zprostředkovatele Azure Site Recovery
1. V části **Rychlý Start** > **Připravit servery VMM** si kliknutím na **Stáhnout zprostředkovatele Microsoft Azure Site Recovery pro instalaci na servery VMM** stáhněte nejnovější verzi instalačního souboru zprostředkovatele.
2. Spusťte tento soubor na zdrojovém serveru VMM.

   > [!NOTE]
   > Pokud je server VMM nasazený v clusteru a instalujete zprostředkovatele poprvé, nainstalujte ho na aktivní uzel a dokončením instalace zaregistrujte server VMM v trezoru. Potom zprostředkovatele nainstalujte na ostatní uzly. Je třeba upozornit na to, že pokud zprostředkovatele upgradujete, budete ho muset upgradovat na všech uzlech, protože na všech by měla běžet stejná verze zprostředkovatele.
   >
   >
3. Instalační program provádí kontrolu požadavků, které musí být splněné, a požádá o oprávnění k zastavení služby VMM, aby bylo možné spustit instalační program zprostředkovatele. Služba VMM se po dokončení instalace automaticky restartuje. Pokud zprostředkovatele instalujete na clusteru VMM, budete vyzváni k zastavení role Cluster.
4. V nastavení služby **Microsoft Update** můžete vyjádřit výslovný souhlas se stahováním a instalací aktualizací. Když bude toto nastavení povoleno, aktualizace zprostředkovatele se budou instalovat v souladu s vašimi zásadami pro službu Microsoft Update.

    ![Aktualizace Microsoft Update](./media/site-recovery-vmm-to-azure-classic/updates.png)
5. Umístění instalace pro zprostředkovatele je nastaveno na **<SystemDrive>\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin**. Klikněte na **Nainstalovat**.

   ![InstallLocation](./media/site-recovery-vmm-to-azure-classic/install-location.png)
6. Po nainstalování zprostředkovatele kliknutím na **Zaregistrovat** zaregistrujte server v trezoru.

    ![InstallComplete](./media/site-recovery-vmm-to-azure-classic/install-complete.png)
7. V poli **Název trezoru** ověřte název trezoru, ve kterém bude server zaregistrovaný. Klikněte na *Další*.

    ![Registrace serveru](./media/site-recovery-vmm-to-azure-classic/vaultcred.PNG)
8. V části **Připojení k internetu** určete, jak se zprostředkovatel, který běží na serveru VMM, připojí k internetu. Pokud chcete použít výchozí nastavení připojení k internetu nakonfigurované na serveru, vyberte **Připojit pomocí stávajícího nastavení proxy**.

    ![Nastavení internetu](./media/site-recovery-vmm-to-azure-classic/proxydetails.PNG)

   * Pokud chcete používat vlastní proxy server, měli byste ho nastavit před instalací zprostředkovatele. Při konfiguraci nastavení vlastního proxy serveru proběhne test, aby se zkontrolovalo připojení k proxy serveru.
   * Pokud používáte vlastní proxy server nebo váš výchozí proxy server vyžaduje ověření, budete muset zadat podrobnosti o proxy serveru, včetně jeho adresy a portu.
   * Ze serveru VMM a hostitelů Hyper-V by měly být přístupné následující adresy URL
     * *.hypervrecoverymanager.windowsazure.com
     * *.accesscontrol.windows.net
     * *.backup.windowsazure.com
     * *.blob.core.windows.net
     * *.store.core.windows.net
   * Povolte IP adresy popsané v [rozsazích IP adres Azure Datacenter](https://www.microsoft.com/download/confirmation.aspx?id=41653) a protokol HTTPS (443). Musíte také povolit rozsahy IP adres oblasti Azure, kterou budete chtít používat, a oblasti Západní USA.
   * Pokud používáte vlastní proxy server, vytvoří se automaticky účet VMM RunAs (DRAProxyAccount) pomocí zadaných přihlašovacích údajů proxy serveru. Proxy server nakonfigurujte tak, aby tento účet bylo možné úspěšně ověřit. Nastavení účtu VMM RunAs můžete upravovat v konzole VMM. Uděláte to tak, že otevřete pracovní prostor **Nastavení**, rozbalíte možnost **Zabezpečení**, kliknete na **Účty Spustit jako** a pak upravíte heslo pro DRAProxyAccount. Budete muset službu VMM restartovat, aby se toto nastavení projevilo.
9. V části **Registrační klíč** vyberte klíč, který jste si stáhli z Azure Site Recovery a zkopírovali na server VMM.
10. Nastavení šifrování se používá pouze při replikaci virtuálních počítačů Hyper-V v cloudech VMM do Azure. Pokud provádíte replikaci do sekundární lokality, tak se nepoužívá.
11. Do pole **Název serveru** zadejte popisný název, který bude identifikovat server VMM v trezoru. V konfiguraci clusteru zadejte název role clusteru VMM.
12. V části **Synchronizovat metadata cloudu** vyberte, zda chcete synchronizovat metadata pro všechny cloudy na serveru VMM v trezoru. Tuto akci stačí na každém serveru provést pouze jednou. Pokud nechcete provádět synchronizaci se všemi cloudy, můžete toto políčko nechat nezaškrtnuté a synchronizovat každý cloud jednotlivě ve vlastnostech cloudu v konzole VMM.
13. Dokončete proces kliknutím na **Další**. Po registraci načte Azure Site Recovery metadata ze serveru VMM. Server se zobrazí na kartě **Servery VMM** stránky **Servery** v trezoru.

    ![Poslední stránka](./media/site-recovery-vmm-to-azure-classic/provider13.PNG)

Po registraci načte Azure Site Recovery metadata ze serveru VMM. Server se zobrazí na kartě **Servery VMM** stránky **Servery** v trezoru.

### <a name="command-line-installation"></a>Instalace pomocí příkazového řádku
Zprostředkovatele Azure Site Recovery je možné nainstalovat také pomocí následujícího příkazu z příkazového řádku. Tuto metodu je možné použít k instalaci zprostředkovatele na jádro serveru pro Windows Server 2012 R2.

1. Stáhněte si instalační soubor zprostředkovatele a registrační klíč do složky. Příklad: C:\ASR.
2. Zastavte službu System Center Virtual Machine Manager.
3. Z příkazového řádku se zvýšenými oprávněními pak pomocí následujících příkazů vyextrahujte instalační program zprostředkovatele:

        C:\Windows\System32> CD C:\ASR
        C:\ASR> AzureSiteRecoveryProvider.exe /x:. /q
4. Nainstalujte zprostředkovatele následujícím způsobem:

        C:\ASR> setupdr.exe /i
5. Zaregistrujte zprostředkovatele následujícím způsobem:

        CD C:\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin
        C:\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin\> DRConfigurator.exe /r  /Friendlyname <friendly name of the server> /Credentials <path of the credentials file> /EncryptionEnabled <full file name to save the encryption certificate>       

Kde parametry mají následující význam:

* **/Credentials** je povinný parametr, který určuje, kde je umístěn soubor registračního klíče.  
* **/FriendlyName** je povinný parametr pro název serveru hostitele technologie Hyper-V, který se zobrazí na portálu Azure Site Recovery.
* **/EncryptionEnabled** je volitelný parametr umožňující určit, jestli chcete virtuální počítače v Azure šifrovat (šifrování neaktivních dat). Název souboru musí mít příponu **.pfx**.
* **/proxyAddress** je volitelný parametr, který určuje adresu proxy serveru.
* **/proxyport** je volitelný parametr, který určuje port proxy serveru.
* **/proxyUsername** je volitelný parametr, který určuje uživatelské jméno proxy.
* **/proxyPassword** je volitelný parametr, který určuje heslo k proxy serveru.  

## <a name="step-4-create-an-azure-storage-account"></a>Krok 4: Vytvoření účtu úložiště Azure
1. Pokud nemáte účet úložiště Azure, vytvořte si ho kliknutím na **Přidat účet úložiště Azure**.
2. Vytvořte účet s povolenou geografickou replikací. Musí být ve stejné oblasti jako služba Azure Site Recovery a musí být přidružený ke stejnému předplatnému.

    ![Účet úložiště](./media/site-recovery-vmm-to-azure-classic/storage.png)

> [!NOTE]
> [Migrace účtů úložiště](../azure-resource-manager/resource-group-move-resources.md) napříč skupinami prostředků v rámci stejného předplatného nebo napříč předplatnými se pro účty úložiště použité k nasazení Site Recovery nepodporuje.
>
>

## <a name="step-5-install-the-azure-recovery-services-agent"></a>Krok 5: Instalace agenta Služeb zotavení Azure
Nainstalujte agenta služby Azure Site Recovery na každém serveru hostitele technologie Hyper-V v cloudu VMM.

1. Kliknutím na **Rychlý Start** > **Stáhnout agenta služby Azure Site Recovery a nainstalovat ho na hostitele** si stáhněte nejnovější verzi instalačního souboru agenta.

    ![Instalace agenta Služeb zotavení Azure](./media/site-recovery-vmm-to-azure-classic/install-agent.png)
2. Spusťte instalační soubor na každém serveru hostitele technologie Hyper-V.
3. Na stránce **Kontrola předpokladů** klikněte na **Další**. Automaticky se nainstalují všechny chybějící požadované součásti.

    ![Požadavky agenta Služeb zotavení](./media/site-recovery-vmm-to-azure-classic/agent-prereqs.png)
4. Na stránce **Nastavení instalace** určete, kam chcete agenta nainstalovat, a vyberte umístění mezipaměti, do kterého se nainstalují metadata záloh. Pak klikněte na **Nainstalovat**.
5. Po dokončení instalace kliknutím na **Zavřít** dokončete průvodce.

    ![Registrace agenta MARS](./media/site-recovery-vmm-to-azure-classic/agent-register.png)

### <a name="command-line-installation"></a>Instalace pomocí příkazového řádku
Agenta Služeb zotavení Microsoft Azure můžete také nainstalovat z příkazového řádku pomocí tohoto příkazu:

    marsagentinstaller.exe /q /nu

## <a name="step-6-configure-cloud-protection-settings"></a>Krok 6: Nakonfigurování nastavení ochrany cloudu
Po zaregistrování serveru VMM můžete nakonfigurovat nastavení ochrany cloudu. Při instalaci zprostředkovatele jste povolili možnost **Synchronizovat data cloudu s trezorem**, takže všechny cloudy na serveru VMM se v trezoru objeví na kartě <b>Chráněné položky</b>.

![Publikovaný cloud](./media/site-recovery-vmm-to-azure-classic/clouds-list.png)

1. Na stránce Rychlý Start klikněte na **Nastavení ochrany pro cloudy VMM**.
2. Na kartě **Chráněné položky** klikněte na cloud, který chcete nakonfigurovat, a přejděte na kartu **Konfigurace**.
3. V seznamu **Cíl** vyberte **Azure**.
4. V seznamu **Účet úložiště** vyberte účet úložiště Azure, který používáte pro replikaci.
5. Možnost **Šifrovat uložená data** nastavte na **Vypnuto**. Toto nastavení určuje, že se data mají při replikaci mezi místní lokalitou a Azure šifrovat.
6. V části **Frekvence kopírování** nechejte vybrané výchozí nastavení. Tato hodnota určuje, jak často se mají synchronizovat data mezi zdrojovým a cílovým umístěním.
7. V části **Zachovávat body obnovení po tuto dobu** nechejte vybrané výchozí nastavení. Pokud zůstane nastavená výchozí hodnota nula, bude na hostitelském serveru repliky uložený jenom nejnovější bod obnovení pro primární virtuální počítač.
8. V nastavení **Frekvence snímků konzistentních s aplikací** nechejte zadané výchozí nastavení. Tato hodnota určuje, jak často se mají vytvářet snímky. Snímky využívají službu Stínová kopie svazku (VSS), aby bylo zajištěno, že aplikace budou při pořízení snímku v konzistentním stavu.  Pokud budete nastavovat vlastní hodnotu, musí být menší než počet dalších bodů obnovení, které nakonfigurujete.
9. V nastavení **Čas spuštění replikace** určete, kdy se má spustit počáteční replikace dat do Azure. Použije se časové pásmo na serveru hostitele technologie Hyper-V. Doporučujeme naplánovat si počáteční replikaci na dobu mimo špičku.

    ![Nastavení replikace cloudu](./media/site-recovery-vmm-to-azure-classic/cloud-settings.png)

Po uložení nastavení se vytvoří úloha, kterou je možné monitorovat na kartě **Úlohy**. Pro všechny hostitelské servery technologie Hyper-V ve zdrojovém cloudu VMM se nakonfiguruje replikace.

Po uložení je možné nastavení cloudu změnit na kartě **Konfigurace**. Pokud budete chtít cílové umístění nebo cílový účet úložiště upravit, budete muset odebrat konfiguraci cloudu a potom cloud znovu nakonfigurovat. Je potřeba počítat s tím, že pokud změníte účet úložiště, změna se po změně účtu úložiště aplikuje pouze na virtuální počítače, pro které je povolená ochrana. Stávající virtuální počítače se nebudou migrovat do nového účtu úložiště.

## <a name="step-7-configure-network-mapping"></a>Krok 7: Nakonfigurování mapování sítě
Před zahájením mapování sítě ověřte, jestli jsou virtuální počítače na zdrojovém serveru VMM připojené k síti virtuálních počítačů. Dále pak vytvořte jednu nebo více virtuálních sítí Azure. Na jednu síť Azure je možné namapovat více sítí virtuálních počítačů.

1. Na obrazovce Rychlý start klikněte na **Mapovat sítě**.
2. Na kartě **Sítě** v části **Zdrojové umístění** vyberte zdrojový server VMM. V části **Cílové umístění** vyberte Azure.
3. V části se **zdrojovými** sítěmi se zobrazí seznam sítí virtuálních počítačů, které jsou přidružené k serveru VMM. V části s **cílovými** sítěmi se zobrazí sítě Azure přidružené k předplatnému.
4. Vyberte zdrojovou síť virtuálních počítačů a klikněte na **Mapovat**.
5. Na stránce pro **výběr cílové sítě** vyberte cílovou síť Azure, kterou chcete použít.
6. Kliknutím na značku zaškrtnutí dokončete proces mapování.

    ![Nastavení replikace cloudu](./media/site-recovery-vmm-to-azure-classic/map-networks.png)

Po uložení nastavení se spustí úloha pro sledování postupu mapování a je možné ji monitorovat na kartě Úlohy. Jakékoli existující virtuální počítače repliky, které odpovídají zdrojové síti virtuálních počítačů, budou připojené k cílovým sítím Azure. Nové virtuální počítače, které jsou připojené ke zdrojové síti virtuálních počítačů, budou po replikaci připojené k namapované síti Azure. Pokud upravíte existující mapování s novou sítí, virtuální počítače repliky budou připojené pomocí nového nastavení.

Pamatujte, že pokud má cílová síť více podsítí a jedna z těchto podsítí má stejný název jako podsíť, ve které se nachází zdrojový virtuální počítač, bude virtuální počítač repliky po převzetí služeb při selhání připojen k této cílové podsíti. Pokud neexistuje žádná cílová podsíť s odpovídajícím názvem, připojí se virtuální počítač k první podsíti v síti.

> [!NOTE]
> [Migrace sítí](../azure-resource-manager/resource-group-move-resources.md) napříč skupinami prostředků v rámci stejného předplatného nebo napříč předplatnými se pro sítě použité k nasazení Site Recovery nepodporuje.
>
>

## <a name="step-8-enable-protection-for-virtual-machines"></a>Krok 8: Povolení ochrany pro virtuální počítače
Po správném nakonfigurování serverů, cloudů a sítí můžete povolit ochranu pro virtuální počítače v cloudu. Je třeba počítat s následujícím:

* Virtuální počítače, musí splňovat [požadavky Azure](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).
* Aby bylo možné povolit ochranu, musí být pro virtuální počítač nastavené vlastnosti operačního systému a disku operačního systému. Vlastnost můžete nastavit při vytváření virtuálního počítače v nástroji VMM pomocí šablony virtuálního počítače. Tyto vlastnosti také můžete nastavit pro existující virtuální počítače, a to na kartách **Obecné** a **Konfigurace hardwaru** ve vlastnostech virtuálního počítače. Pokud tyto vlastnosti nenastavíte v nástroji VMM, budete je moct nakonfigurovat na portálu Azure Site Recovery.

    ![Vytvoření virtuálního počítače](./media/site-recovery-vmm-to-azure-classic/enable-new.png)

    ![Změna vlastností virtuálního počítače](./media/site-recovery-vmm-to-azure-classic/enable-existing.png)

1. Když budete chtít povolit ochranu, klikněte na kartě **Virtuální počítače** v cloudu, ve kterém se nachází virtuální počítač, na **Povolit ochranu** > **Přidat virtuální počítače**.
2. Ze seznamu virtuálních počítačů v cloudu vyberte ten, který chcete chránit.

    ![Povolení ochrany virtuálního počítače](./media/site-recovery-vmm-to-azure-classic/select-vm.png)

    Průběh akce **Povolit ochranu** můžete sledovat na kartě **Úlohy**, včetně počáteční replikace. Po spuštění úlohy **Dokončit ochranu** je virtuální počítač připraven k převzetí služeb při selhání. Když je povolena ochrana a virtuální počítače jsou replikovány, uvidíte je v Azure.

    ![Úloha ochrany virtuálního počítače](./media/site-recovery-vmm-to-azure-classic/vm-jobs.png)

1. Ověřte vlastnosti virtuálního počítače a podle potřeby je upravte.

    ![Ověření virtuálních počítačů](./media/site-recovery-vmm-to-azure-classic/vm-properties.png)
2. Na kartě **Konfigurace** ve vlastnostech virtuálního počítače je možné upravit následující vlastnosti sítě.

* **Počet síťových adaptérů na cílovém virtuálním počítači:** Počet síťových adaptérů závisí na velikosti, kterou zadáte pro cílový virtuální počítač. Počet adaptérů podporovaných velikostí virtuálního počítače zjistíte ze [specifikace velikosti virtuálního počítače](../virtual-machines/linux/sizes.md). Když změníte velikost virtuálního počítače a uložíte nastavení, počet síťových adaptérů se změní při příštím otevření stránky **Konfigurace**. Počet síťových adaptérů cílových virtuálních počítačů představuje minimální počet síťových adaptérů na zdrojovém virtuálním počítači a maximální počet síťových adaptérů, který je podporovaný zvolenou velikostí virtuálního počítače, a to následujícím způsobem:

  * Pokud je počet síťových adaptérů na zdrojovém počítači menší nebo roven počtu adaptérů, které jsou povolené pro velikost cílového počítače, pak bude mít cíl stejný počet adaptérů jako zdroj.
  * Pokud počet adaptérů pro zdrojový virtuální počítač překračuje počet povolený pro cílovou velikost, použije se maximální velikost cíle.
  * Pokud má například zdrojový počítač dva síťové adaptéry a velikost cílového počítače podporuje čtyři, bude mít cílový počítač dva adaptéry. Pokud má zdrojový počítač dva adaptéry, ale podporovaná velikost cíle podporuje pouze jeden, bude mít cílový počítač pouze jeden adaptér.     
* **Síť cílového virtuálního počítače:** Sítě, ke které se virtuální počítač připojí, je dána mapováním sítě zdrojového virtuálního počítače. Pokud má zdrojový virtuální počítač více než jeden síťový adaptér a zdrojové sítě jsou namapované na různé sítě na cíli, budete muset zvolit jednu z cílových sítí.
* **Podsíť každého síťového adaptéru:** Pro každý síťový adaptér můžete vybrat podsíť, ke které se virtuální počítač při převzetí služeb při selhání připojí.
* **Cílová IP adresa:** Pokud je síťový adaptér zdrojového virtuálního počítače nakonfigurovaný tak, aby používal statickou IP adresu, můžete zadat IP adresu cílového virtuálního počítače. Pomocí této funkce můžete zachovat IP adresu zdrojového virtuálního počítače po převzetí služeb při selhání. Pokud není zadána žádná IP adresa, pak je síťovému adaptéru přiřazena libovolná dostupná IP adresa v okamžiku převzetí služeb při selhání. Pokud je zadána cílová IP adresa, ale ta už se používá jiným virtuálním počítačem spuštěným v Azure, převzetí služeb při selhání se nezdaří.  

    ![Úprava vlastností sítě](./media/site-recovery-vmm-to-azure-classic/multi-nic.png)

> [!NOTE]
> Virtuální počítače s Linuxem se statickými IP adresami nejsou podporované.
>
>

## <a name="test-the-deployment"></a>Otestování nasazení
Pokud chcete nasazení otestovat, můžete spustit testovací převzetí služeb při selhání pro jeden virtuální počítač nebo vytvořit plán obnovení, který se skládá z více virtuálních počítačů, a spustit testovací převzetí služeb při selhání pro celý plán.  

Testovací převzetí služeb při selhání simuluje váš mechanismus převzetí služeb při selhání a zotavení v izolované síti. Poznámky:

* Pokud se chcete připojit k virtuálnímu počítači v Azure pomocí Vzdálené plochy po převzetí služeb po selhání, pak před spuštěním testu převzetí služeb povolte na virtuálním počítači připojení ke Vzdálené ploše.
* Po převzetí služeb při selhání použijete veřejnou IP adresu pro připojení k virtuálnímu počítači v Azure pomocí Vzdálené plochy. Pokud to chcete provést, zkontrolujte, že nemáte žádné zásady domény, které by vám bránily v připojení k virtuálnímu počítači pomocí veřejné adresy.

> [!NOTE]
> Aby byl při předání služeb při selhání do Azure zajištěn nejvyšší možný výkon, nainstalujte na virtuální počítač agenta Azure. Umožní rychlejší spouštění a pomáhá při řešení potíží. Stáhněte si [agenta pro Linux](https://github.com/Azure/WALinuxAgent) nebo [agenta pro Windows](http://go.microsoft.com/fwlink/?LinkID=394789).
>
>

### <a name="create-a-recovery-plan"></a>Vytvoření plánu obnovení
1. Na kartě **Plány obnovení** přidejte nový plán. Do pole **Typ zdroje** zadejte název, **VMM**, a do pole **Zdroj** zadejte zdrojový server VMM. Cíl bude Azure.

    ![Vytvoření plánu obnovení](./media/site-recovery-vmm-to-azure-classic/recovery-plan1.png)
2. Na stránce pro **výběr virtuálních počítačů** vyberte virtuální počítače, které chcete přidat do plánu obnovení. Tyto virtuální počítače se přidají do výchozí skupiny plánu obnovení – Skupina 1. V jednom plánu obnovení bylo otestováno maximálně 100 virtuálních počítačů.

* Pokud chcete před přidáním virtuálních počítačů do plánu ověřit jejich vlastnosti, klikněte na virtuální počítač na stránce vlastností cloudu, ve kterém se nachází. Vlastnosti virtuálního počítače můžete také konfigurovat v konzole VMM.
* Pro všechny virtuální počítače, které jsou zobrazené, byla povolena ochrana. Seznam obsahuje jak virtuální počítače, pro které je povolená ochrana a byla pro ně dokončena počáteční replikace, tak ty, pro které je povolená ochrana, ale u kterých se stále ještě čeká na provedení počáteční replikace. Převzetí služeb při selhání v rámci plánu obnovení je možné pouze u virtuálních počítačů s dokončenou počáteční replikací.

    ![Vytvoření plánu obnovení](./media/site-recovery-vmm-to-azure-classic/select-rp.png)

Po vytvoření se plán obnovení zobrazí na kartě **Plány obnovení**. Můžete také k plánu obnovení přidat [Runbooky služby Azure Automation](site-recovery-runbook-automation.md) pro automatizaci akcí během převzetí služeb při selhání.

### <a name="run-a-test-failover"></a>Spuštění testovacího převzetí služeb při selhání
Testovací předání služeb při selhání do Azure je možné spustit dvěma způsoby.

* **Testovací převzetí služeb při selhání bez sítě Azure:** Tento typ testovacího převzetí služeb při selhání ověří, že virtuální počítač se zobrazí správně v Azure. Virtuální počítač nebude po převzetí služeb při selhání připojen k žádné síti Azure.
* **Testovací převzetí služeb při selhání se sítí Azure:** Tento typ převzetí služeb při selhání kontroluje, že se očekávaným způsobem zobrazí celé prostředí replikace a že virtuální počítače, které převzaly služby při selhání, budou připojené k zadané cílové síti Azure. Aby bylo zajištěno, že bude použita správná podsíť, vybere se pro testovací převzetí služeb při selhání podsíť testovacího virtuálního počítače na základě podsítě virtuálního počítače repliky. Tím se replikace liší od standardní replikace, při které je podsíť virtuálního počítače repliky založena na podsíti zdrojového virtuálního počítače.

Pokud chcete spustit testovací předání služeb při selhání pro virtuální počítač, pro který je povolená ochrana, do Azure bez zadání cílové sítě Azure, nemusíte dělat žádné další přípravy. Pokud chcete spustit testovací převzetí služeb při selhání s cílovou sítí Azure, budete muset vytvořit novou síť Azure, která bude izolovaná od produkční sítě Azure (což je výchozí chování při vytváření nových sítí v Azure). Podívejte se na další podrobnosti o tom, jak [spustit testovací převzetí služeb](site-recovery-failover.md).

Také budete muset nastavit infrastrukturu, aby replikovaný virtuální počítač fungoval podle očekávání. Například virtuální počítač s řadičem domény a DNS je možné replikovat do Azure pomocí Azure Site Recovery a je možné ho vytvořit v testovací síti pomocí testovacího převzetí služeb při selhání. Další podrobnosti najdete v tématu věnovaném [aspektům, které je třeba zvážit při testování převzetí služeb při selhání pro Active Directory](site-recovery-active-directory.md#test-failover-considerations).

Pokud chcete spustit testovací převzetí služeb při selhání, udělejte toto:

1. Na kartě **Plány obnovení** vyberte plán a klikněte na **Testovací převzetí služeb při selhání**.
2. Na stránce s **potvrzením testovacího převzetí služeb při selhání** vyberte možnost **Žádné** nebo konkrétní síť Azure.  Je třeba upozornit na to, že pokud vyberete možnost Žádné, testovací převzetí služeb při selhání zkontroluje, jestli se virtuální počítač replikoval správně do Azure, ale nezkontroluje konfiguraci sítě replikace.

    ![Žádná síť](./media/site-recovery-vmm-to-azure-classic/test-no-network.png)
3. Pokud je pro cloud povolené šifrování dat, vyberte v nastavení **Šifrovací klíč** certifikát, který byl vydán během instalace zprostředkovatele na serveru VMM, když jste zapnuli možnost povolení šifrování dat pro cloud.
4. Na kartě **Úloha** můžete sledovat průběh převzetí služeb při selhání. Měli byste také mít možnost zobrazit si testovací repliku virtuálního počítače na portálu Azure. Pokud máte nastaven přístup k virtuálním počítačům ze své místní sítě, můžete iniciovat připojení k virtuálnímu počítači přes Vzdálenou plochu.
5. Když převzetí služeb při selhání dosáhne fáze **Dokončit testování**, dokončete proces kliknutím na **Dokončit test**. Přechodem k podrobnostem na kartě **Úloha** můžete sledovat průběh a stav převzetí služeb při selhání a provádět všechny potřebné akce.
6. Po převzetí služeb při selhání budete moct zobrazit testovací repliku virtuálního počítače na webu Azure Portal. Pokud máte nastaven přístup k virtuálním počítačům ze své místní sítě, můžete iniciovat připojení k virtuálnímu počítači přes Vzdálenou plochu. Udělejte toto:

   1. Ověřte, že se virtuální počítače úspěšně spustí.
   2. Pokud se chcete připojit k virtuálnímu počítači v Azure pomocí Vzdálené plochy po převzetí služeb po selhání, pak před spuštěním testu převzetí služeb povolte na virtuálním počítači připojení ke Vzdálené ploše. Také budete muset na virtuálním počítači přidat koncový bod RDP. Můžete k tomu použít [Runbooky služby Azure Automation](site-recovery-runbook-automation.md).
   3. Pokud po převzetí služeb při selhání použijete k připojení k virtuálnímu počítači v Azure pomocí vzdálené plochy veřejnou IP adresu, nesmíte mít žádné zásady domény, které by vám bránily v připojení k virtuálnímu počítači pomocí veřejné adresy.
7. Po dokončení testování udělejte toto:

   * Klikněte na **Testovací převzetí služeb při selhání se dokončilo**. Vyčistěte testovací prostředí. Testovací virtuální počítač se automaticky vypne a odstraní.
   * Klikněte na **Poznámky** a zaznamenejte a uložte jakékoli připomínky související s testovacím převzetím služeb při selhání.


## <a name="next-steps"></a>Další kroky
Další informace o [nastavení plánů obnovení](site-recovery-create-recovery-plans.md) a [převzetí služeb při selhání](site-recovery-failover.md).

