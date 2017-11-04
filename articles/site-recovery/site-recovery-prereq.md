---
title: "Předpoklady pro replikaci do Azure pomocí Azure Site Recovery | Microsoft Docs"
description: "Další informace o požadavcích na replikaci virtuálních počítačů a fyzických počítačů do Azure pomocí služby Azure Site Recovery."
services: site-recovery
documentationcenter: 
author: rajani-janaki-ram
manager: jwhit
editor: tysonn
ms.assetid: e24eea6c-50a7-4cd5-aab4-2c5c4d72ee2d
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 10/19/2017
ms.author: raynew
ms.openlocfilehash: cb7c3892dc0acbffdec636e5e7b3ce063660153c
ms.sourcegitcommit: 804db51744e24dca10f06a89fe950ddad8b6a22d
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/30/2017
---
#  <a name="prerequisites-for-replication-from-on-premises-to-azure-by-using-site-recovery"></a>Předpoklady pro replikaci z místního do Azure pomocí Site Recovery

> [!div class="op_single_selector"]
> * [Replikace z Azure do Azure](site-recovery-azure-to-azure-prereq.md)
> * [Replikace z místního do Azure](site-recovery-prereq.md)

Azure Site Recovery můžete podporovat kontinuity podnikových procesů a strategie po havárii (BCDR) tím, že orchestruje replikaci virtuálního počítače Azure (VM) k jiné oblasti Azure. Site Recovery také replikuje místní fyzických serverů a virtuálních počítačů do cloudu (Azure) nebo do sekundárního datacentra. Pokud ve vaší primární lokalitě dojde k výpadku, můžete selhání do sekundárního umístění, aby aplikace a úlohy zůstaly dostupné. Zpět do primárního umístění může selhat, pokud primární umístění obnoví normální provozní. Další informace o Site Recovery najdete v tématu [co je Site Recovery?](site-recovery-overview.md).

V tomto článku jsme shrnují předpoklady pro od Site Recovery replikaci z místního počítače do Azure.

Můžete odesílat jakékoli komentáře v dolní části článku. Také můžete pokládat v technické dotazy [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="azure-requirements"></a>Požadavky na Azure

**Požadavek** | **Podrobnosti**
--- | ---
**Účet Azure** | A [účet Microsoft Azure](http://azure.microsoft.com/). Můžete začít s [bezplatnou zkušební verzí](https://azure.microsoft.com/pricing/free-trial/).
**Služba Site Recovery** | Další informace o cenách služby Azure Site Recovery najdete v tématu [cenách za Site Recovery](https://azure.microsoft.com/pricing/details/site-recovery/). |
**Azure Storage** | Potřebujete účet Azure Storage k ukládání replikovaných dat. Účet úložiště musí být ve stejné oblasti jako trezor služby Azure Recovery Services. Virtuální počítače Azure se vytvoří, když dojde k převzetí služeb při selhání.<br/><br/> V závislosti na modelu prostředků, který chcete používat pro převzetí služeb při selhání virtuálního počítače Azure, které můžete nastavit účet pomocí [modelu nasazení Azure Resource Manager](../storage/common/storage-create-storage-account.md) nebo [modelu nasazení classic](../storage/common/storage-create-storage-account.md).<br/><br/>Můžete použít [geograficky redundantní úložiště](../storage/common/storage-redundancy.md#geo-redundant-storage) nebo místně redundantní úložiště. Doporučujeme použít geograficky redundantní úložiště. S geograficky redundantním úložištěm je zajištěna odolnost dat v případě výpadku oblasti nebo pokud není možné obnovit primární oblast.<br/><br/> Můžete použít účet úložiště Azure úrovně standard, nebo můžete použít Azure Premium Storage. [Storage úrovně Premium](https://docs.microsoft.com/azure/storage/storage-premium-storage) se obvykle používá u virtuálních počítačů, které je třeba neustále vysoké vstupně-výstupní výkon a nízkou latencí. Storage úrovně premium můžete hostovat virtuální počítač I náročnými úlohy. Pokud použijete službu Storage úrovně Premium pro replikovaná data, potřebujete také účet úložiště úrovně Standard. V účtu úložiště úrovně Standard se ukládají protokoly replikace, do kterých se zaznamenávají průběžné změny místních dat.<br/><br/>
**Omezení úložiště** | Nelze přesunout účty úložiště, které používají v Site Recovery k jiné skupině prostředků, nebo přesunout do nebo pomocí jiné předplatné.<br/><br/> V současné době replikace na prémiové účty úložiště v centrální Indie a – Jih, Indie není k dispozici.
**Síť Azure** | Budete potřebovat síť Azure, ke kterému se virtuální počítače Azure připojovat po převzetí služeb při selhání. Síť Azure musí být ve stejné oblasti jako trezor služby Recovery Services.<br/><br/> Na portálu Azure můžete vytvořit síť Azure pomocí [modelu nasazení Resource Manager](../virtual-network/virtual-networks-create-vnet-arm-pportal.md) nebo [modelu nasazení classic](../virtual-network/virtual-networks-create-vnet-classic-pportal.md).<br/><br/> Pokud replikujete do Azure z System Center Virtual Machine Manager (VMM), musíte nastavit mapování sítě mezi sítěmi virtuálních počítačů nástroje VMM a sítě Azure. Tím se zajistí, že se virtuální počítače Azure po převzetí služeb při selhání připojit k odpovídající sítě.
**Omezení sítě** | Nelze přesunout sítě účty, které používáte v Site Recovery k jiné skupině prostředků, nebo přesunout do nebo pomocí jiné předplatné.
**Mapování sítě** | Pokud budete replikovat virtuální počítače Microsoft Hyper-V v cloudech VMM, musíte nastavit mapování sítě. Tím se zajistí, že virtuální počítače Azure připojit k odpovídající sítě, když jste vytvořili po převzetí služeb při selhání.

> [!NOTE]
> Následující části popisují požadavky pro různé součásti prostředí zákazníků. Další informace o podpoře pro konkrétní konfigurace najdete v tématu [matici podpory](site-recovery-support-matrix.md).
>

## <a name="disaster-recovery-of-vmware-vms-or-physical-windows-or-linux-servers-to-azure"></a>Zotavení po havárii virtuálních počítačů VMware nebo fyzických serverů systému Windows nebo Linux na Azure
Následující součásti jsou zapotřebí pro obnovení po havárii virtuálních počítačů VMware nebo fyzických serverů systému Windows nebo Linux. Toto jsou kromě těch, které jsou popsané v [požadavky pro Azure](#azure-requirements).


### <a name="configuration-server-or-additional-process-server"></a>Konfigurace serveru nebo další procesového serveru

Nastavte na místním počítači jako konfigurační server k orchestraci komunikace mezi místními servery a Azure. Následující tabulku rozhovory o požadavcích na systém a software virtuálního počítače, který může být nakonfigurován jako konfigurační server.

> [!IMPORTANT]
> Pokud nasazujete konfigurační Server pro ochranu virtuálních počítačů VMware, doporučujeme jej jako nasadíte **vysoce dostupné (HA)** virtuálního počítače.

[!INCLUDE [site-recovery-configuration-server-requirements](../../includes/site-recovery-configuration-and-scaleout-process-server-requirements.md)]

### <a name="vmware-vcenter-or-vsphere-host-prerequisites"></a>VMware vCenter nebo vSphere hostitele požadovaných součástí

| **Komponenta** | **Požadavky** |
| --- | --- |
| **vSphere** | Musí mít jeden nebo více hypervisory VMware vSphere.<br/><br/>Hypervisory musí být spuštěna vSphere verze 6.0, 5.5 nebo 5.1, s nejnovějšími aktualizacemi.<br/><br/>Doporučujeme, abyste tento vSphere hostitelů a serverů vCenter být ve stejné síti jako procesní server. Pokud jste nastavili vyhrazené procesový server, jedná se o síť, kde se nachází na konfiguračním serveru. |
| **vCenter** | Doporučujeme nasadit server VMware vCenter pro správu hostitelů vSphere. Musí být spuštěna vCenter verze 6.0 nebo 5.5, s nejnovějšími aktualizacemi.<br/><br/>**Omezení**: Site Recovery nepodporuje replikaci mezi instancemi řešení vMotion vCenter. Úložiště DRS a Storage vMotion také nejsou podporovány na hlavním cíli virtuálních počítačů po opětovné ochrany operaci.|

### <a name="replicated-machine-prerequisites"></a>Požadavky replikovaného počítače

| **Komponenta** | **Požadavky** |
| --- | --- |
| **Místní počítače** (virtuální počítače VMware) | Replikované virtuální počítače musí mít nástroje VMware nainstalovaná a spuštěná.<br/><br/> Virtuální počítače, musí splňovat [požadavky Azure](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements) pro vytvoření virtuálního počítače Azure.<br/><br/>Kapacita disku na každém chráněném počítači nemůže být větší než 1,023 GB. <br/><br/>Minimální 2 GB volného místa na disku pro instalaci je vyžadován pro instalaci součásti.<br/><br/>Pokud chcete povolit konzistence více virtuálních počítačů, musíte otevřít port 20004 v místní bráně firewall virtuálních počítačů.<br/><br/>Názvy počítačů musí být mezi 1 a 63 znaků (můžete použít písmena, číslice a pomlčky). Název musí začínat písmenem nebo číslicí a končit písmenem nebo číslicí. <br/><br/>Až poprvé povolíte replikaci pro počítač, můžete upravit název Azure.<br/><br/> |
| **Počítače s Windows** (fyzické nebo VMware) | Na počítači musí běžet některý z následujících podporovaných operačních systémů 64-bit: <br/>– Windows Server 2016 (jádra serveru, Server s desktopovým prostředím)<br/>-Windows Server 2012 R2<br/>– Windows Server 2012<br/>-Windows Server 2008 R2 s aktualizací SP1 nebo novější verze<br/><br/> Musí být nainstalován operační systém na jednotce c:. Disk operačního systému musí být základní disk systému Windows a nikoli dynamické. Datový disk může být dynamické.<br/><br/>|
| **Počítače se systémem Linux** (fyzické nebo VMware) | Na počítači musí běžet některý z následujících podporovaných operačních systémů 64-bit: <br/>-Red Hat Enterprise Linux 5.2 5,11 6.1 k 6.9 7.0 na 7.3<br/>-CentOS 5.2 5,11 6.1 k 6.9 7.0 na 7.3<br/>-Ubuntu 14.04 LTS serveru (seznam verzí podporované jádra pro Ubuntu najdete v tématu [podporované operační systémy](site-recovery-support-matrix-to-azure.md#support-for-replicated-machine-os-versions))<br/>-Ubuntu 16.04 LTS server (seznam verzí podporované jádra pro Ubuntu najdete v tématu [podporované operační systémy](site-recovery-support-matrix-to-azure.md#support-for-replicated-machine-os-versions))<br/>-Debian 7 nebo Debian 8<br/>-Oracle Enterprise Linux verze 6.5 nebo 6.4, systémem Red Hat kompatibilní jádra nebo nedělitelné Enterprise jádra verze 3 (UEK3)<br/>-SUSE Linux Enterprise Server 11 SP4 nebo SUSE Linux Enterprise Server 11 SP3<br/><br/>Položky, které mapování názvu místního hostitele na IP adresy přidružené všechny síťové adaptéry musí mít vaše soubory/etc/hosts na chráněných počítačích.<br/><br/>Po převzetí služeb při selhání Pokud se chcete připojit k virtuální počítač Azure se systémem Linux pomocí klienta Secure Shell (SSH), ujistěte se, že služba SSH v chráněném počítači je nastavena na automatické spuštění při spuštění systému. Také se ujistěte, že pravidla brány firewall umožňují službě SSH připojit pro chráněný počítač.<br/><br/>Název hostitele, přípojné body, názvy zařízení a Linux systémové cesty a názvy souborů (například/etc / a USR) musí být v angličtině jenom.<br/><br/>Následující adresáře (Pokud nastavený jako samostatné oddíly nebo systému souborů) musí být na stejném disku (disk operačního systému) na zdrojovém serveru:<br/>- kořenovém<br/>- / spouštěcí<br/>-usr<br/>-/ usr/místní<br/>-/var<br/>- / atd<br/><br/>V současné době XFS v5 funkcí, jako například metadata kontrolního součtu, nejsou podporovány službou Site Recovery v systémech souborů s XFS. Ověřte, jestli nejsou vaše systémy souborů XFS funkcí v5. Můžete použít nástroj xfs_info ke kontrole tzv XFS pro oddíl. Pokud **ftype** je nastaven na **1**, XFS v5 funkce jsou používány.<br/><br/>Na serverech Red Hat Enterprise Linux 7 a CentOS 7 nástroj lsof musí být nainstalovaná a k dispozici.<br/><br/>


## <a name="disaster-recovery-of-hyper-v-vms-to-azure-no-vmm"></a>Zotavení po havárii virtuálních počítačů technologie Hyper-V do Azure (bez VMM)

Následující součásti jsou zapotřebí pro obnovení po havárii virtuálních počítačů technologie Hyper-V v cloudech VMM. Toto jsou kromě těch, které jsou popsané v [požadavky pro Azure](#azure-requirements).

| **Požadavek** | **Podrobnosti** |
| --- | --- |
| **Hostitel Hyper-V** |Jeden nebo více místními servery musí používat Windows Server 2012 R2 s nejnovější aktualizace a povolenou roli technologie Hyper-V nebo Microsoft Hyper-V Server 2012 R2.<br/><br/>Servery Hyper-V musí mít jeden nebo více virtuálních počítačů.<br/><br/>Servery Hyper-V musí být připojen k Internetu, buď přímo nebo prostřednictvím proxy serveru.<br/><br/>Servery Hyper-V musí mít opravy popsané v článku znalostní báze [2961977](https://support.microsoft.com/kb/2961977) nainstalována.
|**Zprostředkovatel a agent**| Během nasazování Site Recovery nainstalujete zprostředkovatele Azure Site Recovery. Instalace zprostředkovatele také nainstaluje agenta služeb zotavení Azure na každém serveru technologie Hyper-V, který běží virtuální počítače, které chcete chránit. <br/><br/>Všechny servery Hyper-V v trezoru Site Recovery musí mít stejné verze zprostředkovatele a agentem.<br/><br/>Zprostředkovatel musí připojit k Site Recovery přes Internet. Může být odeslán provoz přímo nebo prostřednictvím proxy serveru. Založený na protokolu HTTPS proxy není podporováno. Proxy server musí umožňovat přístup k následujícím adresám URL:<br/><br/> [!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)]<br/><br/>Pokud máte na serveru pravidla brány firewall založená na adresu IP, ujistěte se, že tato pravidla umožňují komunikaci s Azure.<br/><br/> Povolit [rozsahy IP adres Azure datacenter](https://www.microsoft.com/download/confirmation.aspx?id=41653)a protokol HTTPS (port 443).<br/><br/> Povolte rozsahy IP adres pro oblast Azure svého předplatného a západní USA (používá se pro přístup k řízení a identity management).


## <a name="disaster-recovery-of-hyper-v-vms-in-vmm-clouds-to-azure"></a>Zotavení po havárii virtuálních počítačů technologie Hyper-V v cloudech VMM do Azure

Následující součásti jsou zapotřebí pro obnovení po havárii virtuálních počítačů technologie Hyper-V v cloudech VMM. Toto jsou kromě těch, které jsou popsané v [požadavky pro Azure](#azure-requirements).

| **Požadavek** | **Podrobnosti** |
| --- | --- |
| **Nástroj Virtual Machine Manager** |Musí mít jeden nebo více serverů VMM běžících na System Center 2012 R2 nebo novější. Každý server VMM musí mít minimálně jeden cloud nakonfigurovaný. <br/><br/>V cloudu, musíte mít:<br/>-Jeden nebo více skupinu hostitelů VMM.<br/>-Jeden nebo více hostitelské servery Hyper-V nebo clusterů v každé skupině hostitelů.<br/><br/>Další informace o nastavení cloudů VMM najdete v tématu [vytvoření cloudu v nástroji Virtual Machine Manager 2012](http://social.technet.microsoft.com/wiki/contents/articles/2729.how-to-create-a-cloud-in-vmm-2012.aspx). |
| **Hyper-V** |Na hostitelských serverech Hyper-V musí běžet minimálně Windows Server 2012 R2 s povolenou rolí Hyper-V nebo Microsoft Hyper-V Server 2012 R2. Musí být nainstalované nejnovější aktualizace.<br/><br/> Server Hyper-V musí mít jeden nebo více virtuálních počítačů.<br/><br/> Hostitelský server Hyper-V nebo cluster, který obsahuje virtuální počítače, které chcete replikovat, musí být spravovaný v cloudu VMM.<br/><br/>Servery Hyper-V musí být připojen k Internetu, buď přímo nebo prostřednictvím proxy serveru.<br/><br/>Servery Hyper-V musí mít opravy popsané v článku znalostní báze [2961977](https://support.microsoft.com/kb/2961977) nainstalována.<br/><br/>Hostitelské servery Hyper-V potřebují přístup k Internetu pro replikaci dat do Azure. |
| **Zprostředkovatel a agent** |Během nasazování Azure Site Recovery nainstalujte zprostředkovatele Azure Site Recovery na serveru VMM. Nainstalujte agenta služeb zotavení na hostitele Hyper-V. Zprostředkovatel a agent nutné se připojit k Azure přímo přes Internet nebo prostřednictvím proxy serveru. Proxy server založený na protokolu HTTPS se nepodporuje. Proxy server na VMM server a hostitelé technologie Hyper-V musí umožňovat přístup k: <br/><br/>[!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)] <br/><br/>Pokud máte na serveru VMM pravidla brány firewall založená na adresu IP, ujistěte se, že tato pravidla umožňují komunikaci s Azure.<br/><br/> Povolit [rozsahy IP adres Azure datacenter](https://www.microsoft.com/download/confirmation.aspx?id=41653) a HTTPS (port 443).<br/><br/>Povolte rozsahy IP adres pro oblast Azure pro vaše předplatné a západní USA (používá se pro přístup k řízení a identity management).<br/><br/> |

### <a name="replicated-machine-prerequisites"></a>Požadavky replikovaného počítače

| **Komponenta** | **Podrobnosti** |
| --- | --- |
| **Chráněné virtuální počítače** | Site Recovery podporuje všechny operační systémy, které jsou podporovány [Azure](https://technet.microsoft.com/library/cc794868%28v=ws.10%29.aspx).<br/><br/>Virtuální počítače, musí splňovat [požadavky Azure](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements) pro vytvoření virtuálního počítače Azure. Názvy počítačů musí být mezi 1 a 63 znaků (můžete použít písmena, číslice a pomlčky). Název musí začínat písmenem nebo číslicí a končit písmenem nebo číslicí. <br/><br/>Název virtuálního počítače můžete upravit, můžete po povolení replikace pro virtuální počítač. <br/><br/> Kapacita disku pro každý chráněný počítač nemůže být větší než 1,023 GB. Virtuální počítač může mít až 16 disků (až 16 TB).<br/><br/>


## <a name="disaster-recovery-of-hyper-v-vms-in-vmm-clouds-to-a-customer-owned-site"></a>Zotavení po havárii virtuálních počítačů technologie Hyper-V v cloudech VMM do lokality vlastněných zákazníků

Následující součásti jsou zapotřebí pro obnovení po havárii virtuálních počítačů technologie Hyper-V v cloudech VMM do lokality vlastněných zákazníků. Toto jsou kromě těch, které jsou popsané v [požadavky pro Azure](#azure-requirements).

| **Komponenta** | **Podrobnosti** |
| --- | --- |
| **Nástroj Virtual Machine Manager** |  Doporučujeme nasadit na server VMM v primární lokalitě a sekundární lokality.<br/><br/> Chcete-li replikovat mezi cloudy na jednom serveru VMM, je nutné aspoň dva cloudy, které jsou nakonfigurované na serveru VMM.<br/><br/> Servery VMM musí používat minimálně System Center 2012 SP1, s nejnovějšími aktualizacemi.<br/><br/> Každý server VMM musí mít minimálně jeden cloud. Všechny cloudy musí mít sady profilů kapacity technologie Hyper-V. <br/><br/>Cloudy musí mít jeden nebo více skupin hostitelů VMM. Další informace o nastavení cloudů VMM najdete v tématu [Příprava pro nasazení Azure Site Recovery](https://msdn.microsoft.com/library/azure/dn469075.aspx#BKMK_Fabric). |
| **Hyper-V** | Servery Hyper-V musí běžet minimálně Windows Server 2012 s rolí Hyper-V povolena, a mít nainstalované nejnovější aktualizace.<br/><br/> Server Hyper-V musí mít jeden nebo více virtuálních počítačů.<br/><br/>  Hostitelské servery Hyper-V se musí nacházet ve skupinách hostitelů v primárních a sekundárních cloudech VMM.<br/><br/> Pokud spustíte technologie Hyper-V v clusteru na Windows Server 2012 R2, doporučujeme nainstalovat aktualizaci popsanou v článku znalostní báze Knowledge Base [2961977](https://support.microsoft.com/kb/2961977).<br/><br/> Pokud se spuštění technologie Hyper-V v clusteru na Windows Server 2012 a máte statické IP adresy na základě clusteru, zprostředkovatele clusteru se nevytvoří automaticky. Je nutné ručně nakonfigurovat zprostředkovatele clusteru. Další informace o zprostředkovatele clusteru najdete v tématu [konfigurace se role zprostředkovatele replik pro replikaci clustery](http://social.technet.microsoft.com/wiki/contents/articles/18792.configure-replica-broker-role-cluster-to-cluster-replication.aspx). |
| **Poskytovatel** | Během nasazování Site Recovery nainstalujte zprostředkovatele Azure Site Recovery na servery VMM. Zprostředkovatel komunikováním se službou Site Recovery přes protokol HTTPS (port 443) pro orchestraci replikace. Dojde k replikaci dat mezi servery primární a sekundární Hyper-V, přes síť LAN nebo prostřednictvím připojení VPN.<br/><br/> Zprostředkovatele, který běží na serveru VMM potřebuje přístup k následujícím adresám URL:<br/><br/>[!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)] <br/><br/>Zprostředkovatele služby Site Recovery musí umožňovat komunikaci brány firewall ze serverů VMM do [rozsahy IP adres Azure datacenter](https://www.microsoft.com/download/confirmation.aspx?id=41653)a povolit protokol HTTPS (port 443). |


## <a name="url-access"></a>Adresa URL přístup
Tyto adresy URL musí být dostupný z hostitelské servery technologie Hyper-V, VMware a VMM:

|**ADRESA URL** | **Nástroj VMM do nástroje VMM** | **Z VMM do Azure** | **Z Hyper-V do Azure** | **Z VMware do Azure** |
|--- | --- | --- | --- | --- |
|``*.accesscontrol.windows.net`` | Povolit | Povolit | Povolit | Povolit |
|``*.backup.windowsazure.com`` | Nepožaduje se | Povolit | Povolit | Povolit |
|``*.hypervrecoverymanager.windowsazure.com`` | Povolit | Povolit | Povolit | Povolit |
|``*.store.core.windows.net`` | Povolit | Povolit | Povolit | Povolit |
|``*.blob.core.windows.net`` | Nepožaduje se | Povolit | Povolit | Povolit |
|``https://dev.mysql.com/get/archives/mysql-5.5/mysql-5.5.37-win32.msi`` | Nepožaduje se | Nepožaduje se | Nepožaduje se | Povolit stažení SQL |
|``time.windows.com`` | Povolit | Povolit | Povolit | Povolit|
|``time.nist.gov`` | Povolit | Povolit | Povolit | Povolit |
