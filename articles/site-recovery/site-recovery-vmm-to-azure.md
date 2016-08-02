<properties
    pageTitle="Replikace virtuálních počítačů Hyper-V v clouduech VMM do Azure přes Azure Site Recovery pomocí portálu Azure | Microsoft Azure"
    description="Popisuje způsob nasazení Azure Site Recovery za účelem orchestrace replikace, převzetí služeb při selhání a obnovení virtuálních počítačů Hyper-V v cloudech VMM do Azure pomocí portálu Azure."
    services="site-recovery"
    documentationCenter=""
    authors="rayne-wiselman"
    manager="jwhit"
    editor="tysonn"/>

<tags
    ms.service="site-recovery"
    ms.workload="backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="05/10/2016"
    ms.author="raynew"/>

# Replikace virtuálních počítačů Hyper-V v clouduech VMM do Azure přes Azure Site Recovery pomocí portálu Azure | Microsoft Azure

> [AZURE.SELECTOR]
- [Portál Azure](site-recovery-vmm-to-azure.md)
- [Portál Azure Classic](site-recovery-vmm-to-azure-classic.md)
- [PowerShell ARM](site-recovery-vmm-to-azure-powershell-resource-manager.md)
- [PowerShell Classic](site-recovery-deploy-with-powershell.md)

Vítejte v Azure Site Recovery! Tento článek vám pomůže replikovat místní virtuální počítače Hyper-V spravované v cloudech System Center Virtual Machine Manager (VMM) do platformy Azure přes Azure Site Recovery na portálu Azure.

> [AZURE.NOTE] Azure má dva různé [modely nasazení](../resource-manager-deployment-model
> ) pro vytváření prostředků a práci s nimi: Azure Resource Manager (ARM) a klasický model. Azure má také dva portály – portál Azure Clasic, který podporuje klasický model nasazení, a portál Azure s podporou pro oba modely nasazení. 


Azure Site Recovery na portálu Azure nabízí celou řadu nových funkcí:

- Na portálu Azure jsou služby Azure Backup a Azure Site Recovery sloučeny do jednoho trezoru Služeb zotavení tak, abyste mohli nastavit a spravovat BCDR (Business Continuity and Disaster Recovery) z jednoho umístění. Jednotný řídicí panel umožňuje sledovat a spravovat operace v rámci vašich místních lokalit a veřejného cloudu Azure.
- Uživatelé s předplatným Azure, kteří jsou zřízení pomocí poskytovatele CSP (Cloud Solution Provider), teď můžou spravovat operace Site Recovery na portálu Azure.
- Site Recovery na portálu Azure může replikovat počítače do účtů úložiště ARM. Site Recovery při převzetí služeb při selhání vytvoří v Azure virtuální počítače na základě modelu ARM.
- I nadále podporuje replikaci do klasických účtů úložiště. Site Recovery při převzetí služeb při selhání vytvoří virtuální počítače pomocí klasického modelu.


Po přečtení tohoto článku můžete jakékoli svoje připomínky publikovat v dolní části v komentářích Disqus. Technické dotazy můžete zadávat ve [fóru Služeb zotavení Azure](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## Přehled

Organizace potřebují strategii BCDR, která určuje, jak aplikace, úlohy a data zůstanou spuštěné a dostupné během plánovaných a neplánovaných výpadků a jak co nejdříve obnovit normální provozní podmínky. Strategie BCDR by měla zajistit bezpečnost a obnovitelnost firemních dat a zajistit, aby v případě, že dojde k havárii, byly zpracovávané úlohy stále k dispozici.

Site Recovery je služba Azure, která přispívá ke strategie BCDR orchestrací replikace místní fyzických serverů a virtuálních počítačů do cloudu (Azure) nebo do sekundárního datového centra. Pokud dojde k výpadkům ve vašem primárním umístění, předáte služby při selhání do sekundárního umístění, aby aplikace a úlohy zůstaly dostupné. Až se obnoví normální provozní podmínky, vrátíte služby po obnovení zpět do primárního umístění. Další informace najdete v článku [Co je Azure Site Recovery](site-recovery-overview.md).

Tento článek obsahuje všechny informace, které potřebujete, abyste mohli replikovat místní virtuální počítače Hyper-V v cloudech VMM do Azure. Obsahuje přehled architektury, informace o plánování a kroky nasazení pro konfiguraci Azure, místní servery, nastavení replikace a plánování kapacity. Po nastavení infrastruktury můžete povolit replikaci na počítačích, které chcete chránit, a zkontrolovat, že převzetí služeb při selhání funguje, jak má.


## Výhody pro firmy

- Site Recovery poskytuje ochranu mimo pracoviště pro firemní úlohy a aplikace běžící na virtuálních počítačích Hyper-V. 
- Portál Služeb zotavení poskytuje jedno centrální umístění pro nastavování, správu a sledování replikace, převzetí služeb při selhání a zotavení. 
- Snadno můžete spustit převzetí služeb při selhání ze své místní infrastruktury do Azure a navrátit služby po obnovení z Azure na hostitelské servery technologie Hyper-V ve vaší místní lokalitě. 
- Pro plány obnovy je možné nakonfigurovat několik počítačů, aby u vrstvených úloh aplikací docházelo ke společnému převzetí služeb při selhání. 


## Architektura scénáře


Toto jsou součásti scénáře: 

- **Server VMM:** Místní server VMM s jedním nebo více cloudy.
- **Hostitel nebo cluster Hyper-V:** Hostitelské servery nebo clustery technologie Hyper-V, které jsou spravované v cloudech VMM.
- **Zprostředkovatel služby Azure Site Recovery a agent Služeb zotavení:** Během nasazování nainstalujte zprostředkovatele Azure Site Recovery na server VMM a agenta Služeb zotavení Microsoft Azure na hostitelské servery technologie Hyper-V. Zprostředkovatel na serveru VMM replikuje orchestraci komunikováním se službou Site Recovery prostřednictvím protokolu HTTPS 443. Agent na hostitelském serveru Hyper-V ve výchozím nastavení replikuje data do úložiště Azure přes protokol HTTPS 443.
- **Azure:** Budete potřebovat předplatné Azure, účet úložiště Azure pro ukládání replikovaných dat a virtuální síť Azure, aby byly virtuální počítače Azure po převzetí služeb při selhání připojené k síti.

![Topologie E2A](./media/site-recovery-vmm-to-azure/architecture.png)


## Požadavky Azure

Tady je seznam všeho, co budete potřebovat v Azure k nasazení tohoto scénáře.

**Požadavek** | **Podrobnosti**
--- | ---
**Účet Azure**| Budete potřebovat účet [Microsoft Azure](http://azure.microsoft.com/). Můžete začít s [bezplatnou zkušební verzí](https://azure.microsoft.com/pricing/free-trial/). [Další informace](https://azure.microsoft.com/pricing/details/site-recovery/) o cenách za Site Recovery 
**Úložiště Azure** | K ukládání replikovaných dat budete potřebovat účet úložiště Azure úrovně Standard. Můžete používat účet úložiště LRS nebo GRS. Doporučujeme účet úložiště GRS, aby byla zajištěna odolnost dat v případě oblastního výpadku nebo pokud není možné obnovit primární oblast. [Další informace](../storage/storage-redundancy.md). Účet musí být ve stejné oblasti jako trezor Služeb zotavení.<br/><br/>Storage úrovně Premium není v tuto chvíli podporován.<br/><br/> Replikovaná data jsou uložena v úložišti Azure a virtuální počítače Azure jsou vytvářeny, když dojde k převzetí služeb při selhání. <br/><br/> [Přečtěte si informace o](../storage/storage-introduction.md) úložišti Azure.
**Síť Azure** | Budete potřebovat virtuální síť Azure, ke které se budou virtuální počítače Azure připojovat, když dojde k převzetí služeb při selhání. Síť musí být ve stejné oblasti jako trezor Služeb zotavení.

## Místní požadavky

Tady je seznam všeho, co potřebujete místně.

**Požadavek** | **Podrobnosti**
--- | ---
**VMM**| Jeden nebo více serverů VMM běžících na řešení System Center 2012 R2. Každý server VMM musí mít nakonfigurovaný minimálně jeden cloud. Cloud by měl obsahovat:<br/><br/> Minimálně jednu skupinu hostitelů VMM.<br/><br/> Minimálně jeden hostitelský server nebo cluster Hyper-V v každé skupině hostitelů.<br/><br/>[Další informace](http://www.server-log.com/blog/2011/8/26/vmm-2012-and-the-clouds.html) o nastavení cloudů VMM.
**Hyper-V** | Hostitelské servery Hyper-V musí běžet na systému Windows Server minimálně ve verzi 2012 R2 s rolí Hyper-V a musí mít nainstalované nejnovější aktualizace.<br/><br/> Server Hyper-V by měl obsahovat jeden nebo více virtuálních počítačů.<br/><br/> Hostitelský server nebo cluster Hyper-V obsahující virtuální počítače, které chcete replikovat, musí být spravovaný v cloudu VMM.<br/><br/>Servery Hyper-V by měl být připojené k internetu, a to buď přímo nebo prostřednictvím proxy serveru.<br/><br/>Na serverech Hyper-V by měly být nainstalované opravy uvedené v článku [2961977](https://support.microsoft.com/kb/2961977).<br/><br/>Hostitelské servery Hyper-V potřebují mít přístup k internetu pro replikaci dat do Azure. 
**Zprostředkovatel a agent** | Během nasazování Azure Site Recovery nainstalujete na server VMM zprostředkovatele Azure Site Recovery a na hostitele Hyper-V pak agenta Služeb zotavení. Zprostředkovatel a agent se budou muset připojit k Azure přes internet přímo nebo prostřednictvím proxy serveru. Proxy server HTTPS není podporovaný. Proxy server na serveru VMM a hostitelé Hyper-V by měly umožnit přístup k: <br/><br/> *.hypervrecoverymanager.windowsazure.com <br/><br/> *.accesscontrol.windows.net <br/><br/> *.backup.windowsazure.com <br/><br/> *.blob.core.windows.net <br/><br/> *.store.core.windows.net<br/><br/>Pokud máte na serveru VMM pravidla brány firewall založená na IP adrese, zkontrolujte, jestli tato pravidla umožňují komunikaci s Azure. Budete muset povolit [rozsahy IP adres Azure Datacenter](https://www.microsoft.com/download/confirmation.aspx?id=41653) a protokol HTTPS (433).<br/><br/>Povolte rozsahy IP adres pro oblast Azure svého předplatného a pro oblast Západní USA.<br/><br/>Kromě toho bude proxy server na serveru VMM potřebovat přístup k https://www.msftncsi.com/ncsi.txt


## Požadavky na chráněný počítač


**Požadavek** | **Podrobnosti**
--- | ---
**Chráněné virtuální počítače** | Před předáním služeb virtuálního počítače při selhání budete muset zajistit, aby byl název, který bude přiřazen virtuálnímu počítači Azure, v souladu s [požadavky Azure](site-recovery-best-practices.md#azure-virtual-machine-requirements). Tento název můžete po povolení replikace pro virtuální počítač upravit. <br/><br/> Kapacita jednotlivých disků na chráněných počítačích by neměla být větší než 1 023 GB. Virtuální počítač může mít až 64 disků (tedy až 64 TB).<br/><br/> Sdílené hostované clustery disků nejsou podporované.<br/><br/> Není podporované spouštění přes rozhraní UEFI (Unified Extensible Firmware Interface) / EFI (Extensible Firmware Interface).<br/><br/> Pokud zdrojový virtuální počítač využívá funkci seskupování síťových adaptérů, převede se po převzetí služeb při selhání do Azure na jeden síťový adaptér.<br/><br/>Ochrana virtuálních počítačů s Linuxem a statickou IP adresou není podporována.

## Příprava nasazení

Při přípravě nasazení bude nutné provést toto:

1. [Nastavit síť Azure](#set-up-an-azure-network), ve které budou 
2. virtuální počítače umístěné po převzetí služeb při selhání. 
2. [Nastavit účet úložiště Azure](#set-up-an-azure-storage-account) pro replikovaná data.
4. [Připravit server VMM](#prepare-the-vmm-server) pro nasazení Site Recovery. 
5. [Připravit mapování sítě](#prepare-for-network-mapping). Nastavte sítě tak, abyste mohli nakonfigurovat mapování sítě během nasazování Site Recovery.

### Nastavení sítě Azure

Budete potřebovat síť Azure, aby se k ní virtuální počítače Azure vytvořené po převzetí služeb při selhání připojily.

- Síť musí být ve stejné oblasti jako ta, ve které budete nasazovat trezor Služeb zotavení.
- V závislosti na modelu prostředků, který budete chtít použít pro virtuální počítače Azure, které převezmou služby po selhání, nastavíte síť Azure v [režimu ARM](../virtual-network/virtual-networks-create-vnet-arm-pportal.md) nebo [klasickém režimu](../virtual-network/virtual-networks-create-vnet-classic-pportal.md).
- Doporučujeme nastavit síť ještě před tím, než začnete. Pokud to neuděláte, budete to muset udělat při nasazování Site Recovery.


### Nastavení účtu úložiště Azure

- K ukládání dat replikovaných do Azure budete potřebovat účet úložiště Azure úrovně Standard. Účet musí být ve stejné oblasti jako trezor Služeb zotavení.
- V závislosti na modelu prostředků, který budete chtít použít pro virtuální počítače Azure, které převezmou služby po selhání, nastavíte účet v [režimu ARM](../storage/storage-create-storage-account.md) nebo [klasickém režimu](../storage/storage-create-storage-account-classic-portal.md).
- Doporučujeme nastavit účet ještě před tím, než začnete. Pokud to neuděláte, budete to muset udělat při nasazování Site Recovery.

### Příprava serveru VMM

- Zkontrolujte, že server VMM splňuje příslušné [požadavky](#on-premises-prerequisites).
- Během nasazování Site Recovery můžete určit, že všechny cloudy na serveru VMM musí být k dispozici na portálu Azure. Pokud chcete, aby se na portálu zobrazovaly pouze konkrétní cloudy, můžete toto nastavení povolit v cloudu v konzole pro správu VMM. 


### Příprava mapování sítě

Během nasazování Site Recovery budete muset nastavit mapování sítě. Mapování sítě zajišťuje mapování mezi zdrojovými sítěmi virtuálních počítačů VMM a cílovými sítěmi Azure, aby bylo umožněno následující:

- Počítače, které předávají služby při selhání ve stejné síti, se musí být schopny připojit k sobě navzájem i v případě, že nepředají služby při selhání stejným způsobem nebo ve stejném plánu obnovení.
- Pokud je v cílové síti Azure nastavená síťová brána, virtuální počítače Azure se musí být schopny připojit k místním virtuálním počítačům.
- Abyste mohli nastavit mapování sítě, budete muset připravit následující:

    - Zajistěte, aby virtuální počítače na zdrojovém hostitelském serveru Hyper-V byly připojené k síti virtuálních počítačů ve VMM. Tato síť musí být propojená na logickou síť, která je přidružená ke cloudu.
    - Síť Azure, jak je popsána [výše](#set-up-an-azure-network)

- [Další informace](site-recovery-network-mapping.md) o tom, jak funguje mapování sítě.


## Vytvoření trezoru Služeb zotavení

1. Přihlaste se k [portálu Azure](https://portal.azure.com).
2. Klikněte na **Nové** > **Správy** > **Služby zotavení**. Případně můžete kliknout na **Procházet** > **Trezory Služeb zotavení** > **Přidat**.

    ![Nový trezor](./media/site-recovery-vmm-to-azure/new-vault3.png)

3. Do pole **Název** zadejte popisný název pro identifikaci trezoru. Máte-li více předplatných, vyberte jedno z nich.
4. [Vytvořte novou skupinu prostředků](../resource-group-portal.md#create-resource-group) nebo vyberte existující. Zadejte oblast Azure. Počítače budou replikovány do této oblasti. Informace o tom, které oblasti jsou podporované, najdete v části s geografickou dostupností v tématu s [podrobnostmi o cenách Azure Site Recovery](https://azure.microsoft.com/pricing/details/site-recovery/).
4. Pokud chcete mít k trezoru rychlý přístup z řídicího panelu, klikněte na tlačítko **Připnout na řídicí panel** a potom klikněte na tlačítko **Vytvořit trezor**.

    ![Nový trezor](./media/site-recovery-vmm-to-azure/new-vault-settings.png)

Nový trezor se zobrazí v části **Řídicí panel** > **Všechny prostředky** a v hlavním okně **Trezory Služeb zotavení**.

## Začínáme

Site Recovery nabízí postup Začínáme, který vám pomůže co nejrychleji provést nasazení. Postup Začínáme zkontroluje požadavky a provede vás kroky nasazení Site Recovery ve správném pořadí.

V postupu Začínáme vyberete typ počítačů, které chcete replikovat, a zvolíte, kam je chcete replikovat. Nastavíte místní servery, účty úložiště Azure a sítě. Vytvoříte zásady replikace a naplánujete kapacitu. Po nastavení infrastruktury povolíte replikaci pro virtuální počítače. Můžete spustit převzetí služeb při selhání pro konkrétní počítače nebo vytvořit plány obnovení pro převzetí služeb při selhání více počítačů.

Začněte s postupem Začínáme tím, že zvolíte, jak chcete Site Recovery nasadit. Konkrétní postup Začínáme se vždy mírně liší v závislosti na vašich požadavcích replikace.



## Krok 1: Volba cílů ochrany

Vyberte, jak chcete počítače replikovat a kam je chcete replikovat.

1. V okně **Trezory Služeb zotavení** vyberte trezor a klikněte na **Nastavení**.
2. V části **Začínáme** klikněte na **Site Recovery** > **Krok 1: Připravte infrastrukturu** > **Cíl ochrany**.

    ![Zvolte cíle.](./media/site-recovery-vmm-to-azure/choose-goals.png)

3. V části **Cíl ochrany** vyberte **To Azure** (Do Azure) a vyberte **Yes, with Hyper-V** (Ano, s technologií Hyper-V). Výběrem možnosti **Ano** potvrďte, že ke správě hostitelů technologie Hyper-V a lokality obnovení používáte VMM. Pak klikněte na **OK**.

    ![Zvolte cíle.](./media/site-recovery-vmm-to-azure/choose-goals2.png)



## Krok 2: Nastavení zdrojového prostředí

Nainstalujte zprostředkovatele Azure Site Recovery na server VMM a zaregistrujte serveru v trezoru. Nainstalujte agenta Služeb zotavení Azure na hostitele Hyper-V.

1. Klikněte na **Krok 2: Připravte infrastrukturu** > **Zdroj**. 

    ![Nastavení zdroje](./media/site-recovery-vmm-to-azure/set-source1.png)

2. V okně **Připravit zdroj** klikněte na **+ VMM** a přidejte server VMM.

    ![Nastavení zdroje](./media/site-recovery-vmm-to-azure/set-source2.png)

3. V okně **Přidat server** zkontrolujte, že se v části **Typ serveru** zobrazí **server VMM System Center** a že server VMM splňuje [obecné požadavky a požadavky na adresu URL](#on-premises-prerequisites). 
4. Stáhněte si instalační soubor zprostředkovatele Azure Site Recovery.
5. Stáhněte si registrační klíč. Budete ho potřebovat, když spustíte instalaci. Klíč je platný 5 dní od jeho vygenerování. 

    ![Nastavení zdroje](./media/site-recovery-vmm-to-azure/set-source3.png)

6. Nainstalujte zprostředkovatele Azure Site Recovery na server VMM.


### Nastavení zprostředkovatele Azure Site Recovery

1.  Spusťte instalační soubor zprostředkovatele.
2. V rámci **Microsoft Update** můžete vyjádřit výslovný souhlas s aktualizacemi, aby se aktualizace zprostředkovatele nainstalovaly v souladu s vašimi zásadami Microsoft Update.
3. V okně **instalace** přijměte nebo změňte výchozí umístění instalace zprostředkovatele a klikněte na **Nainstalovat**.

    ![Umístění instalace](./media/site-recovery-vmm-to-azure/provider2.png)

4. Po dokončení instalace klikněte na **Zaregistrovat**, aby se server VMM zaregistroval v trezoru.
5. V části **Připojení k Internetu** určete, jak se zprostředkovatel, který běží na serveru VMM, připojí k Site Recovery přes internet.

    - Pokud chcete, aby se zprostředkovatel připojil přímo, vyberte **Připojit přímo bez proxy serveru**.
    - Pokud chcete nastavit připojení s proxy serverem, který je aktuálně nastavený na serveru, vyberte **Připojit se s existujícím nastavením proxy serveru**.
    - Pokud váš stávající proxy server vyžaduje ověření nebo pokud chcete používat vlastní proxy server, vyberte **Připojit se s vlastním nastavením proxy serveru**.
    - Pokud budete používat vlastní proxy server, budete muset zadat adresu, port a přihlašovací údaje.
    - Pokud používáte proxy server, měli byste už mít povolené adresy URL popisované v [požadavcích](#on-premises-prerequisites).
    - Pokud používáte vlastní proxy server, vytvoří se automaticky účet VMM RunAs (DRAProxyAccount) pomocí zadaných přihlašovacích údajů proxy serveru. Proxy server nakonfigurujte tak, aby tento účet bylo možné úspěšně ověřit. Nastavení účtu VMM RunAs můžete upravovat v konzole VMM. V části **Nastavení** rozbalte položku **Zabezpečení** > **Účty Spustit jako** a potom změňte heslo pro DRAProxyAccount. Budete muset službu VMM restartovat, aby se toto nastavení projevilo.

    ![Internet](./media/site-recovery-vmm-to-azure/provider3.png)

6. Na stránce **Nastavení trezoru** klikněte na **Procházet** a vyberte soubor klíče trezoru. Určuje předplatné Azure Site Recovery a název trezoru.

    ![Registrace serveru](./media/site-recovery-vmm-to-azure/provider4.png)

7. Přijměte nebo změňte umístění certifikátu SSL, který je automaticky generován pro šifrování dat. Tento certifikát se používá, pokud povolíte šifrování dat pro cloud chráněný platformou Azure na portálu Azure Site Recovery. Uchovávejte tento certifikát v bezpečí. Při spuštění předání služeb při selhání do Azure ho budete potřebovat k dešifrování, pokud je povolené šifrování dat.

    ![Registrace serveru](./media/site-recovery-vmm-to-azure/provider5.png)

8. Do pole **Název serveru** zadejte popisný název, který bude identifikovat server VMM v trezoru. V konfiguraci clusteru zadejte název role clusteru VMM.
13. Pokud chcete synchronizovat metadata pro všechny cloudy na serveru VMM v trezoru, zaškrtněte políčko **Synchronizovat metadata cloudu**. Tuto akci stačí na každém serveru provést pouze jednou. Pokud nechcete provádět synchronizaci se všemi cloudy, můžete toto políčko nechat nezaškrtnuté a synchronizovat každý cloud jednotlivě ve vlastnostech cloudu v konzole VMM. Kliknutím na **Zaregistrovat** proces dokončete.

    ![Registrace serveru](./media/site-recovery-vmm-to-azure/provider6.png)

9. Spustí se registrace. Po dokončení registrace se server zobrazí v okně **Nastavení** > **Servery** v trezoru.


#### Instalace zprostředkovatele Azure Site Recovery pomocí příkazového řádku

Zprostředkovatele Azure Site Recovery je možné nainstalovat z příkazového řádku. Tuto metodu je možné použít k instalaci zprostředkovatele na jádro serveru pro Windows Server 2012 R2.

1. Stáhněte si instalační soubor zprostředkovatele a registrační klíč do složky. Například C:\ASR.
2. Spuštěním těchto příkazů na příkazovém řádku se zvýšenými oprávněními vyextrahujte instalační program zprostředkovatele:

            C:\Windows\System32> CD C:\ASR
            C:\ASR> AzureSiteRecoveryProvider.exe /x:. /q
3. Spuštěním tohoto příkazu nainstalujte součásti:

            C:\ASR> setupdr.exe /i

4. Potom spuštěním těchto příkazů zaregistrujte server v trezoru:

        CD C:\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin
        C:\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin\> DRConfigurator.exe /r  /Friendlyname <friendly name of the server> /Credentials <path of the credentials file> /EncryptionEnabled <full file name to save the encryption certificate>       

Kde:

- **/Credentials** je povinný parametr, který určuje, kde je umístěn soubor registračního klíče.  
- **/FriendlyName** je povinný parametr pro název serveru hostitele technologie Hyper-V, který se zobrazí na portálu Azure Site Recovery.
- - **/EncryptionEnabled:** je volitelný parametr, který je možné použít při replikaci virtuálních počítačů Hyper-V v cloudech VMM do Azure. Zadejte, jestli chcete virtuální počítače v Azure šifrovat (šifrování neaktivních dat). Zkontrolujte, jestli má název souboru příponu **.pfx**. Šifrování je ve výchozím nastavení vypnuté.
- **/proxyAddress** je volitelný parametr, který určuje adresu proxy serveru.
- **/proxyport** je volitelný parametr, který určuje port proxy serveru.
- **/proxyUsername** je volitelný parametr, který určuje uživatelské jméno proxy (pokud proxy server vyžaduje ověření).
- **/proxyPassword** je volitelný parametr, který určuje heslo k ověření proxy serveru (pokud proxy server vyžaduje ověření).


### Instalace agenta Služeb zotavení Azure na hostitele Hyper-V

1. Po nastavení zprostředkovatele si musíte stáhnout instalační soubor pro agenta Služeb zotavení Azure. Spusťte instalační program na každém serveru Hyper-V v cloudu VMM. 

    ![Lokality Hyper-V](./media/site-recovery-vmm-to-azure/hyperv-agent1.png)

2. Na stránce **Kontrola předpokladů** klikněte na **Další**. Automaticky se nainstalují všechny chybějící požadované součásti.

    ![Požadavky agenta Služeb zotavení](./media/site-recovery-vmm-to-azure/hyperv-agent2.png)

3. Na stránce **Nastavení instalace** přijměte nebo změňte umístění instalace a mezipaměti. Mezipaměť můžete nakonfigurovat na jednotce, na které je minimálně 5 GB dostupného úložiště. Pro mezipaměť ale doporučujeme disk minimálně s 600 GB volného místa. Pak klikněte na **Nainstalovat**.
4. Po dokončení instalace klikněte na **Zavřít**.
    
    ![Registrace agenta MARS](./media/site-recovery-vmm-to-azure/hyperv-agent3.png)

#### Instalace agenta Služeb zotavení Azure pomocí příkazového řádku

Agenta Služeb zotavení Microsoft Azure můžete nainstalovat z příkazového řádku pomocí následujícího příkazu: 

     marsagentinstaller.exe /q /nu

#### Nastavení internetového přístupu proxy serveru k Site Recovery z hostitelů Hyper-V

Agent Služeb zotavení, který běží na hostitelích Hyper-V, potřebuje internetový přístup k Azure pro replikaci virtuálního počítače. Pokud získáváte přístup k internetu prostřednictvím serveru proxy, nastavte ho následujícím způsobem:

1. Otevřete modul snap-in Microsoft Azure Backup konzoly MMC na hostiteli Hyper-V. Ve výchozím nastavení je na ploše nebo v umístění C:\Program Files\Microsoft Azure Recovery Services Agent\bin\wabadmin zástupce služby Microsoft Azure Backup.
2. V modulu snap-in klikněte na **Změnit vlastnosti**.
3. Na kartě **Konfigurace proxy** zadejte informace o proxy serveru.

    ![Registrace agenta MARS](./media/site-recovery-vmm-to-azure/mars-proxy.png)

4. Zajistěte, aby měl agent přístup k adresám URL popsaným v části s [požadavky](#on-premises-prerequisites).


## Krok 3: Nastavení cílového prostředí

Zadejte účet úložiště Azure, který se má používat pro replikaci, a síť Azure, ke které se virtuální počítače Azure připojí po převzetí služeb při selhání.

1.  Klikněte na **Připravit infrastrukturu** > **Cíl** a vyberte předplatné Azure, které chcete použít.
2.  Zadejte model nasazení, který chcete použít pro virtuální počítače po převzetí služeb při selhání.
3.  Site Recovery zkontroluje, že máte minimálně jednu kompatibilní síť a účet úložiště Azure.

    ![Úložiště](./media/site-recovery-vmm-to-azure/compatible-storage.png)   

4.  Pokud jste ještě nevytvořili účet úložiště a chcete ho vytvořit pomocí modelu ARM, klikněte na **+ Účet úložiště** a můžete to provést přímo tady.  V okně **Vytvořit účet úložiště** zadejte název účtu, typ, předplatné a umístění. Účet by měl být ve stejném umístění jako trezor Služeb zotavení.

    ![Úložiště](./media/site-recovery-vmm-to-azure/gs-createstorage.png) 

    Poznámky:

    - Pokud chcete vytvořit účet úložiště pomocí klasického modelu, můžete to udělat na portálu Azure. [Další informace](../storage/storage-create-storage-account-classic-portal.md)
    - Pokud pro replikovaná data používáte účet úložiště Storage úrovně Premium, budete si muset nastavit ještě účet úložiště úrovně Standard pro ukládání protokolů replikace, do kterých se zaznamenávají průběžné změny místních dat.

4.  Pokud jste ještě nevytvořili síť Azure a chcete ji vytvořit pomocí modelu ARM, klikněte na **+Síť** a můžete to provést přímo tady. V okně **Vytvořit virtuální síť** zadejte název sítě, rozsah adres, podrobnosti o podsíti, předplatné a umístění. Síť by měla být ve stejném umístění jako trezor Služeb zotavení.

    ![Síť](./media/site-recovery-vmm-to-azure/gs-createnetwork.png)

    Pokud chcete vytvořit síť pomocí klasického modelu, můžete to udělat na portálu Azure. [Další informace](../virtual-network/virtual-networks-create-vnet-classic-pportal.md).

### Konfigurace mapování sítě

- [Projděte si](#prepare-for-network-mapping) rychlý přehled toho, co mapování sítě dělá. [Tady](site-recovery-network-mapping.md) najdete podrobnější vysvětlení.
- Ověřte, že jsou virtuální počítače na serveru VMM připojené k síti virtuálních počítačů a že jste vytvořili aspoň jednu virtuální síť Azure. Na jednu síť Azure je možné namapovat více sítí virtuálních počítačů.

Nakonfigurujte mapování následujícím způsobem:

1. V části **Nastavení** > **Infrastruktura Site Recovery** > **Mapování sítí** > **Mapování sítě** klikněte na ikonu **+Mapování sítě**.

    ![Mapování sítě](./media/site-recovery-vmm-to-azure/network-mapping1.png)

2. Na stránce **Přidání mapování sítě** vyberte zdrojový server VMM a jako cíl vyberte **Azure**.
3. Ověřte předplatné a model nasazení po převzetí služeb při selhání.
4. V poli **Zdrojová síť** vyberte ze seznamu přidruženého k tomuto serveru VMM zdrojovou místní síť virtuálních počítačů, kterou chcete namapovat. 
5. V seznamu **Cílová síť** vyberte síť Azure, ve které budou umístěné virtuální počítače Azure repliky, když se zprovozní. Pak klikněte na **OK**.

    ![Mapování sítě](./media/site-recovery-vmm-to-azure/network-mapping2.png)

Když se začne mapovat síť, dojde k tomuto:

- Existující virtuální počítače ve zdrojové síti virtuálních počítačů se v okamžiku, kdy začne mapování, připojí k cílové síti. Nové virtuální počítače připojené ke zdrojové síti virtuálních počítačů se v okamžiku, kdy dojde k replikaci, připojí k namapované síti Azure. 
- Pokud upravíte existující mapování sítě, virtuální počítače repliky se připojí pomocí nového nastavení.
- Pokud má cílová síť více podsítí a jedna z těchto podsítí má stejný název jako podsíť, ve které je umístěný zdrojový virtuální počítač, pak se virtuální počítač repliky po převzetí služeb při selhání připojí k této cílové podsíti.
- Pokud neexistuje žádná cílová podsíť s odpovídajícím názvem, připojí se virtuální počítač k první podsíti v síti.



## Krok 4: Nakonfigurování nastavení replikace


1. Pokud chcete vytvořit novou zásadu replikace, klikněte na **Připravit infrastrukturu** > **Nastavení replikace** > **+Vytvořit a přidružit**.

    ![Síť](./media/site-recovery-vmm-to-azure/gs-replication.png)

2. V části **Vytvořit a přidružit zásady** zadejte název zásady.
3. V části **Frekvence kopírování** určete, jak často chcete replikovat rozdílová data po počáteční replikaci (každých 30 sekund, 5 minut nebo 15 minut).
4. V části **Uchování bodu obnovení** zadejte (v hodinách), jak dlouhý bude interval uchovávání dat pro jednotlivé body obnovení. Chráněné počítače je možné obnovit do libovolného bodu v rámci tohoto intervalu.
6. V nastavení **Frekvence snímků konzistentní vzhledem k aplikacím** určete, jak často (1–12 hodin) se mají vytvářet body obnovení obsahující snímky konzistentní vzhledem k aplikacím. Technologie Hyper-V používá dva typy snímků – standardní snímek, což je přírůstkový snímek celého virtuálního počítače, a snímek konzistentní vzhledem k aplikacím, který vytvoří snímek dat aplikací ve virtuálním počítači v daném okamžiku. Snímky konzistentní vzhledem k aplikacím využívají službu Stínová kopie svazku (VSS), aby bylo zajištěno, že aplikace budou při pořízení snímku v konzistentním stavu. Pokud povolíte snímky konzistentní vzhledem k aplikacím, bude to mít vliv na výkon aplikací běžících na zdrojových virtuálních počítačích. Zajistěte, aby byla hodnota, kterou nastavíte, menší než počet dalších bodů obnovení, které nakonfigurujete.
3. V nastavení **Čas spuštění počáteční replikace** určete, kdy se má spustit počáteční replikace. Při replikaci se využívá šířka pásma vašeho internetového připojení, může být proto vhodné naplánovat ji na dobu mimo špičky.
5. V nastavení **Šifrovat úložiště dat ve službě Azure** určete, jestli chcete v úložišti Azure šifrovat neaktivní data. Pak klikněte na **OK**.

    ![Zásady replikace](./media/site-recovery-vmm-to-azure/gs-replication2.png)

6. Když vytvoříte novou zásadu, automaticky se přidruží ke cloudu VMM. Klikněte na **OK**. K této zásadě replikace můžete přidružit další cloudy VMM (a virtuální počítače v nich), a to tady: **Nastavení** > **Replikace** > Název zásady > **Přidružit cloud VMM**. 

    ![Zásady replikace](./media/site-recovery-vmm-to-azure/policy-associate.png)

## Krok 5: Plánování kapacity

Teď, když máte nastavenou základní infrastrukturu, můžete začít přemýšlet o plánování kapacity a zjistit, jestli nepotřebujete další prostředky.

Site Recovery nabízí plánovač kapacity (Capacity Planner), který vám pomůže přidělit správné prostředky pro vaše zdrojové prostředí, součásti Site Recovery, sítě a úložiště. Plánovač můžete spustit v rychlém režimu pro odhady založené na průměrném počtu virtuálních počítačů/disků a úložišti nebo v podrobném režimu, ve kterém budete zadávat hodnoty na úrovni zpracovávaných úloh. Než začnete, budete muset:

- Získat informace o prostředí replikace, včetně virtuální počítačů, disků na virtuální počítač a úložišti na disk.
- Odhad míry každodenních změn s ohledem na replikovaná data. Může vám s tím pomoct nástroj [Capacity planner for Hyper-V Replica](https://www.microsoft.com/download/details.aspx?id=39057).

1.  Kliknutím na **Stáhnout** si nástroj stáhněte a pak ho spusťte. [Přečtěte si doprovodný článek](site-recovery-capacity-planner.md) k nástroji.
2.  Po dokončení vyberte pro otázku **Have you run the Capacity Planner** (Spustili jste plánovač kapacity) odpověď **Yes** (Ano).

    ![Plánování kapacity](./media/site-recovery-vmm-to-azure/gs-capacity-planning.png)

### Aspekty šířky pásma sítě

Pomocí nástroje Capacity Planner můžete vypočítat šířku pásma, kterou potřebujete pro replikaci (počáteční a pak rozdílovou). K řízení velikosti šířky pásma využívané pro replikaci máte tyto možnosti:

- **Omezení šířky pásma:** Přenos Hyper-V, který se replikuje do sekundární lokality, prochází konkrétním hostitelem Hyper-V. Šířku pásma na hostitelském serveru můžete omezit.
- **Optimalizace šířky pásma:** Šířku pásma používanou pro replikaci můžete ovlivnit pomocí několika klíčů registru.

#### Omezení šířky pásma

1. Otevřete modul snap-in Microsoft Azure Backup konzoly MMC na hostitelském serveru Hyper-V. Ve výchozím nastavení je na ploše nebo v umístění C:\Program Files\Microsoft Azure Recovery Services Agent\bin\wabadmin zástupce služby Microsoft Azure Backup.
2. V modulu snap-in klikněte na **Změnit vlastnosti**.
3. Na kartě **Omezování** vyberte **Povolit omezování šířky pásma internetu u operací zálohování** a nastavte limity pro pracovní a nepracovní dobu. Platné rozsahy jsou 512 kB/s až 102 MB/s.

    ![Omezení šířky pásma](./media/site-recovery-vmm-to-azure/throttle2.png)

Pro nastavení omezování můžete také použít rutinu [Set OBMachineSetting](https://technet.microsoft.com/library/hh770409.aspx). Tady je ukázkový skript:

    $mon = [System.DayOfWeek]::Monday 
    $tue = [System.DayOfWeek]::Tuesday
    Set-OBMachineSetting -WorkDay $mon, $tue -StartWorkHour "9:00:00" -EndWorkHour "18:00:00" -WorkHourBandwidth  (512*1024) -NonWorkHourBandwidth (2048*1024)

**Set-OBMachineSetting -NoThrottle** označuje, že není požadováno žádné omezování.


#### Ovlivnění šířky pásma sítě

Hodnota registru **UploadThreadsPerVM** řídí počet vláken, která se používají pro přenos dat (počáteční nebo rozdílové replikace) disku. Vyšší hodnota zvětšuje šířku pásma sítě využívané pro replikaci. Hodnota registru **DownloadThreadsPerVM** určuje počet vláken používaných pro přenos dat během navracení služeb po obnovení.

1. V registru přejděte na **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Replication**.
    
    - Upravením hodnoty **UploadThreadsPerVM** (nebo pokud neexistuje, pak klíč vytvořte) můžete řídit vlákna používaná pro replikaci disku. 
    - Upravením hodnoty **DownloadThreadsPerVM** (pokud neexistuje, pak klíč vytvořte) můžete řídit vlákna používaná pro navrácení služeb (přenosu) po obnovení z Azure. 
2. Výchozí hodnota je 4. V síti s „nadměrným zřízením“ je třeba tyto klíče registru změnit z výchozích hodnot. Maximum je 32. Monitorováním provozu hodnotu optimalizujte. 

## Krok 6: Povolení replikace

Teď následujícím způsobem povolte replikaci:

1. Klikněte na **Krok 2: Replikujte aplikaci** > **Zdroj**. Až poprvé povolíte replikaci, pak kliknutím na **+Replikovat** v trezoru povolíte replikaci pro další počítače.

    ![Povolení replikace](./media/site-recovery-vmm-to-azure/enable-replication1.png)

2. V okně **Zdroje** > vyberte server VMM a cloud, ve kterém jsou umístění hostitelé technologie Hyper-V. Pak klikněte na **OK**.

    ![Povolení replikace](./media/site-recovery-vmm-to-azure/enable-replication-source.png)

3. V nastavení **Cíl** vyberte předplatné, model nasazení po převzetí služeb při selhání a účet úložiště, který používáte pro replikovaná data.

    ![Povolení replikace](./media/site-recovery-vmm-to-azure/enable-replication-target.png)

4. Vyberte účet úložiště, který chcete použít. Pokud chcete použít jiný účet úložiště, než ten, který máte, musíte si ho [vytvořit](#set-up-an-azure-storage-account). Pokud chcete vytvořit účet úložiště pomocí modelu ARM, klikněte na **Vytvořit nový**. Pokud chcete vytvořit účet úložiště pomocí klasického modelu, můžete to udělat [na portálu Azure](../storage/storage-create-storage-account-classic-portal.md). Pak klikněte na **OK**.
5. Vyberte síť Azure a podsíť, ke kterým se připojí virtuální počítače Azure, když se zprovozní po převzetí služeb při selhání. Výběrem možnosti **Nakonfigurovat pro vybrané počítače** použijte nastavení sítě pro všechny počítače, které jste vybrali pro ochranu. Vyberte **Nakonfigurovat později** a vyberte síť Azure pro konkrétní počítač. Pokud chcete použít jinou síť než tu, kterou máte, musíte si ji [vytvořit](#set-up-an-azure-network). Pokud chcete vytvořit síť pomocí modelu ARM, klikněte na **Vytvořit nový**. Pokud chcete vytvořit síť pomocí klasického modelu, můžete to udělat [na portálu Azure](../virtual-network/virtual-networks-create-vnet-classic-pportal.md). V odpovídajícím případě vyberte podsíť. Pak klikněte na **OK**.
6. V nastavení **Virtuální počítače** > **Výběr virtuálních počítačů** klikněte a vyberte každý počítač, který chcete replikovat. Můžete vybrat pouze počítače, pro které je možné povolit replikaci. Pak klikněte na **OK**.

    ![Povolení replikace](./media/site-recovery-vmm-to-azure/enable-replication5.png)

5. V nastavení **Vlastnosti** > **Konfigurace vlastností** vyberte operační systém pro vybrané virtuální počítače a disk operačního systému. Pak klikněte na **OK**. Později můžete nastavit další vlastnosti. 

    ![Povolení replikace](./media/site-recovery-vmm-to-azure/enable-replication6.png)

    
12. V **Nastavení replikace** > **Konfigurace nastavení replikace** vyberte zásadu replikace, kterou chcete použít pro chráněné virtuální počítače. Pak klikněte na **OK**. Zásady replikace můžete změnit v **Nastavení** > **Zásady replikace** > název zásady > **Upravit nastavení**. Změny, které použijete, se použijí pro počítače, které už replikujete, a nové počítače.

    ![Povolení replikace](./media/site-recovery-vmm-to-azure/enable-replication7.png)

Průběh úlohy **povolení ochrany** můžete sledovat tady: **Nastavení** > **Úlohy** > **Úlohy Site Recovery**. Po spuštění úlohy **Dokončit ochranu** je počítač připravený k převzetí služeb při selhání.

### Zobrazení a správa vlastností virtuálního počítače

Doporučujeme ověřit vlastnosti zdrojového počítače. Mějte na paměti, že název virtuálního počítače Azure musí být v souladu s [požadavky na virtuální počítače Azure](site-recovery-best-practices.md#azure-virtual-machine-requirements).

1. Klikněte na **Nastavení** > **Chráněné položky** > **Replikované položky** > a vyberte počítač. Zobrazí se jeho podrobnosti.

    ![Povolení replikace](./media/site-recovery-vmm-to-azure/vm-essentials.png)

2. V části **Vlastnosti** můžete zobrazit informace o replikaci a převzetí služeb při selhání pro virtuální počítač.

    ![Povolení replikace](./media/site-recovery-vmm-to-azure/test-failover2.png)

3. V části **Výpočty a síť** > **Výpočetní vlastnosti** můžete zadat název a cílovou velikost virtuálního počítače Azure. Podle potřeby upravte název tak, aby byl souladu s [požadavky Azure](site-recovery-best-practices.md#azure-virtual-machine-requirements). Můžete také zobrazit a upravit informace o cílové síti, podsíti a IP adrese, která bude přiřazená k virtuálnímu počítači Azure. Je třeba počítat s následujícím:

    - Můžete nastavit cílovou IP adresu. Pokud adresu nezadáte, bude počítač, který převezme služby při selhání, používat DHCP. Pokud nastavíte adresu, která není k dispozici pro převzetí služeb při selhání, převzetí služeb při selhání se nezdaří. Stejnou cílovou IP adresu je možné použít pro testovací převzetí služeb při selhání, pokud je adresa k dispozici v testovací síti převzetí služeb při selhání.
    - Počet síťových adaptérů závisí na velikosti, kterou zadáte pro cílový virtuální počítač, a to následujícím způsobem:

        - Pokud je počet síťových adaptérů na zdrojovém počítači menší nebo roven počtu adaptérů, které jsou povolené pro velikost cílového počítače, pak bude mít cíl stejný počet adaptérů jako zdroj.
        - Pokud počet adaptérů pro zdrojový virtuální počítač překračuje počet povolený pro cílovou velikost, použije se maximální velikost cíle.
        - Pokud má například zdrojový počítač dva síťové adaptéry a velikost cílového počítače podporuje čtyři, bude mít cílový počítač dva adaptéry. Pokud má zdrojový počítač dva adaptéry, ale podporovaná velikost cíle podporuje pouze jeden, bude mít cílový počítač pouze jeden adaptér.     
        - Pokud má virtuální počítač více síťových adaptérů, připojí se všechny ke stejné síti.

    ![Povolení replikace](./media/site-recovery-vmm-to-azure/test-failover4.png)

5.  V části **Disky** uvidíte operační systém a datové disky ve virtuálním počítači, který bude replikován. 



## Krok 7: Otestování nasazení

Pokud budete chtít otestovat nasazení, můžete spustit test převzetí služeb při selhání pro jediný virtuální počítač nebo plán obnovení, který obsahuje jeden nebo více virtuálních počítačů.


### Příprava převzetí služeb při selhání

- Pokud chcete spustit testovací převzetí služeb při selhání, doporučujeme vytvořit novou síť Azure, která bude izolovaná od produkční sítě Azure (což je výchozí chování při vytváření nových sítí v Azure). [Další informace](site-recovery-failover.md#run-a-test-failover) o spuštění testovacích převzetí služeb při selhání.
- Aby byl při předání služeb při selhání do Azure zajištěn nejvyšší možný výkon, nainstalujte na chráněný počítač agenta Azure. Umožní rychlejší spouštění a pomáhá při řešení potíží. Nainstalujte agenta pro [Linux](https://github.com/Azure/WALinuxAgent) nebo [Windows](http://go.microsoft.com/fwlink/?LinkID=394789). 
- Abyste mohli nasazení plně otestovat, budete potřebovat infrastrukturu, aby replikovaný počítač fungoval podle očekávání. Pokud chcete testovat službu Active Directory a DNS, můžete vytvořit virtuální počítač jako řadič domény s DNS a ten pak replikovat do Azure pomocí Azure Site Recovery. Další informace najdete v tématu věnovaném tomu, [co je třeba zvážit v případě testovacích převzetí služeb při selhání pro Active Directory](site-recovery-active-directory.md#considerations-for-test-failover).
- Pokud chcete místo testovacího převzetí služeb při selhání spustit neplánované převzetí služeb při selhání, je třeba počítat s tímto:

    - Pokud je to možné, měli byste před spuštěním neplánovaného převzetí služeb při selhání vypnout primární počítače. To zajistí, že nebudete mít ve stejnou dobu spuštěný jak zdrojový počítač, tak počítač repliky. 
    - Když spustíte neplánované převzetí služeb při selhání, zastaví se tím replikace dat z primárních počítačů, takže po zahájení neplánovaného převzetí služeb při selhání nebudou přenášena žádná rozdílová data. Navíc platí, že pokud spustíte neplánované převzetí služeb při selhání pro plán obnovení, poběží až do dokončení, a to i v případě, že dojde k chybě.

### Příprava připojení k virtuálním počítačům Azure po převzetí služeb při selhání

Pokud se chcete po převzetí služeb při selhání připojit k virtuálním počítačům Azure pomocí protokolu RDP, je nutné provést následující kroky:

**Na místním počítači před převzetí služeb při selhání:**

- Pro přístup prostřednictvím internetu povolte RDP, zajistěte, aby byla přidána pravidla TCP a UDP pro **veřejné** sítě, a zajistěte, aby byl protokol RDP v nastavení **Brána Windows Firewall** -> **Povolené aplikace a funkce** povolený pro všechny profily.
- Pro přístup přes připojení S2S povolte na počítači protokol RDP a zajistěte, aby byl protokol RDP v nastavení **Brána Windows Firewall** -> **Povolené aplikace a funkce** povolený pro **domény** a **privátní** sítě.
- Nainstalujte na místní počítač [agenta virtuálního počítače Azure](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409).
- Zkontrolujte, že je zásada SAN operačního systému nastavená na OnlineAll. [Další informace]( https://support.microsoft.com/kb/3031135)
- Před spuštěním převzetí služeb při selhání vypněte službu IPSec.

**Ve virtuálním počítači Azure po převzetí služeb při selhání:**

- Přidejte veřejný koncový bod pro protokol RDP (portu 3389) a zadejte přihlašovací údaje pro přihlášení.
- Zkontrolujte, že nemáte žádné zásady domény, které by vám bránily v připojení k virtuálnímu počítači pomocí veřejné adresy.
- Pokuste se připojit. Pokud se nemůže připojit, ověřte, že virtuální počítač běží. Další tipy pro řešení potíží si můžete přečíst v tomto [článku](http://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx).

Pokud chcete po převzetí služeb při selhání získat přístup k virtuálnímu počítači Azure s Linuxem pomocí klienta Secure Shell (ssh), postupujte následovně:

**Na místním počítači před převzetí služeb při selhání:**

- Zkontrolujte, že služba Secure Shell na virtuálním počítači Azure je nastavená na automatické spuštění při spuštění systému.
- Zkontrolujte, jestli pravidla brány firewall umožňují službě SSH připojit se k ní.

**Ve virtuálním počítači Azure po převzetí služeb při selhání:**

- Pravidla skupiny zabezpečení sítě na virtuálním počítači, který převzal služby při selhání, a v podsíti Azure, ke které je připojený, musí povolovat příchozí připojení k portu SSH.
- Musí být vytvořený veřejný koncový bod, aby byla povolena příchozí připojení na portu SSH (ve výchozím nastavení je to port TCP 22).
- Pokud se k virtuálnímu počítači přistupuje prostřednictvím připojení VPN (Express Route nebo S2S VPN), je možné klienta použít k přímému připojení k virtuálnímu počítači přes protokol SSH.


### Spuštění testovacího převzetí služeb při selhání

Pokud chcete spustit testovací převzetí služeb při selhání, udělejte toto:

1. Pokud chcete převzít služby při selhání pro jeden virtuální počítač, klikněte v nastavení **Nastavení** > **Replikované položky** na virtuální počítač > **+Testovací převzetí služeb při selhání**.
2. Pokud chcete pro převzetí služeb při selhání použít plán obnovení, klikněte v **Nastavení** > **Plány obnovení** pravým tlačítkem myši na plán > **Testovací převzetí služeb při selhání**. Pokud chcete vytvořit plán obnovení, [postupujte podle těchto pokynů](site-recovery-create-recovery-plans.md).

3. V části **Testovací převzetí služeb při selhání** vyberte síť Azure, ke které se virtuální počítače Azure připojí, když dojde k převzetí služeb při selhání.
4. Kliknutím na **OK** zahajte převzetí služeb při selhání. Pokud chcete sledovat průběh, otevřete kliknutím na virtuální počítač jeho vlastnosti, případně můžete kliknout na úlohu **Testovací převzetí služeb při selhání** v **Nastavení** > **Úlohy Site Recovery**.
5. Když převzetí služeb při selhání dosáhne fáze **Dokončit testování**, udělejte toto:

    1. Zobrazte si virtuální počítač repliky na portálu Azure. Ověřte, že se virtuální počítač úspěšně spustí.
    2. Pokud máte nastaven přístup k virtuálním počítačům ze své místní sítě, můžete iniciovat připojení k virtuálnímu počítači přes Vzdálenou plochu.
    3. Kliknutím na **Dokončit test** testovací převzetí služeb při selhání dokončete.
    4. Klikněte na **Poznámky** a zaznamenejte a uložte jakékoli připomínky související s testovacím převzetím služeb při selhání.
    5. Klikněte na **Testovací převzetí služeb při selhání se dokončilo**. Vyčistěte testovací prostředí. Testovací virtuální počítač se automaticky vypne a odstraní.
    6. V této fázi se odstraní všechny prvky nebo virtuální počítače automaticky vytvořené službou Site Recovery v průběhu testovacího převzetí služeb při selhání. Neodstraní se žádné další prvky, které jste pro testovací převzetí služeb při selhání vytvořili vy sami.
    
    > [AZURE.NOTE] Pokud bude testovací převzetí služeb při selhání pokračovat déle než dva týdny, vynutí se jeho ukončení.

6. Po dokončení převzetí služeb při selhání by se vám také měl zobrazit počítač Azure repliky na portálu Azure > **Virtuální počítače**. Měli byste zajistit, aby měl virtuální počítač odpovídající velikost, byl připojený k odpovídající síti a aby běžel.
7. Pokud jste [připravili připojení po převzetí služeb při selhání](#prepare-to-connect-to-Azure-VMs-after-failover), měli byste být schopni se k virtuálnímu počítači Azure připojit.


## Monitorování nasazení

Tady je postup, jak monitorovat nastavení konfigurace, stav a stavu nasazení Site Recovery:

1. Klikněte na název trezoru. Tím se dostanete na řídicí panel **Základy**. V tomto řídicím panelu uvidíte úlohy Site Recovery, stav replikace, plány obnovení, stav serveru a události.  Řídicí panel Základy si můžete přizpůsobit, aby se na něm zobrazovaly dlaždice a rozložení, které jsou pro vás nejužitečnější, včetně stavu dalších trezorů Site Recovery a Backup.

    ![Základy](./media/site-recovery-vmm-to-azure/essentials.png)

2. Na dlaždici **Stav** můžete monitorovat servery lokality (servery VMM nebo konfigurační servery), na kterých dochází k potížím, a události vyvolané službou Site Recovery za posledních 24 hodin.
3. Replikaci můžete spravovat a monitorovat na dlaždicích **Replikované položky**, **Plány obnovení** a **Úlohy Site Recovery**. Podrobnosti o úlohách si můžete zobrazit tady: **Nastavení** -> **Úlohy** -> **Úlohy Site Recovery**.


## Další kroky

Po nasazení a zprovoznění nasazení si můžete přečíst [další informace](site-recovery-failover.md) o různých typech převzetí služeb při selhání.



<!--HONumber=Jun16_HO2-->


