---
title: 'Site Recovery: Časté otázky | Microsoft Docs'
description: V tomto článku naleznete odpovědi na běžné otázky o Azure Site Recovery.
services: site-recovery
documentationcenter: ''
author: rayne-wiselman
manager: jwhit
editor: ''

ms.service: site-recovery
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 03/27/2016
ms.author: raynew

---
# Azure Site Recovery: Časté otázky (FAQ)
Přečtěte si tento článek, ve kterém najdete časté otázky ke službě Azure Site Recovery.

Pokud i po přečtení tohoto článku máte dotazy, můžete je uvést ve spodní části článku nebo na [Fóru Azure Recovery Services](https://social.msdn.microsoft.com/Forums/azure/home?forum=hypervrecovmgr).

## Obecné
### K čemu Site Recovery slouží?
Site Recovery přispívá ke strategii pro provozní kontinuitu a zotavení po havárii (BCDR) tím, že orchestruje a automatizuje replikaci z místních virtuálních počítačů a fyzických serverů do Azure nebo do sekundárního datacentra. [Další informace](site-recovery-overview.md).

### Co dokáže Site Recovery replikovat?
* **Virtuální počítače Hyper-V**: Site Recovery dokáže replikovat jakoukoli úlohu spuštěnou na podporovaném virtuálním počítači Hyper-V. 
* **Fyzické servery**: Site Recovery dokáže replikovat jakoukoli úlohu spuštěnou na podporovaném fyzickém serveru s Windows nebo Linuxem.
* **Virtuální počítače VMware**: Site Recovery dokáže replikovat jakoukoli úlohu spuštěnou na podporovaném virtuálním počítači VMware.

### Co potřebuji v Hyper-V?
U hostitelských serverů Hyper-V se vaše potřeby odvíjí od scénáře nasazení. Požadavky pro Hyper-V si můžete projít v těchto tématech:

* [Replikace virtuálních počítačů Hyper-V (bez VMM) do Azure](site-recovery-hyper-v-site-to-azure.md#before-you-start)
* [Replikace virtuálních počítačů Hyper-V (s VMM) do Azure](site-recovery-vmm-to-azure.md#before-you-start)
* [Replikace virtuálních počítačů Hyper-V do sekundárního datacentra](site-recovery-vmm-to-vmm.md#before-you-start)

Pro hosty běžící na hostitelském serveru Hyper-V:

* Pokud replikujete do sekundární lokality, přečtěte si o [podporovaných hostovaných operačních systémech pro virtuální počítače Hyper-V](https://technet.microsoft.com/library/mt126277.aspx).
* Pokud replikujete do Azure, Site Recovery podporuje stejné hostované operační systémy, které [podporuje Azure](https://technet.microsoft.com/library/cc794868%28v=ws.10%29.aspx).

### Mohu chránit virtuální počítače, pokud Hyper-V běží na klientském operačním systému?
Ne, virtuální počítače se musí nacházet na hostitelském serveru Hyper-V, který běží na podporovaném serveru s Windows. Pokud potřebujete ochránit klientský počítač, můžete jej replikovat jako fyzický počítač do [Azure](site-recovery-vmware-to-azure-classic.md) nebo do [sekundárního datacentra](site-recovery-vmware-to-vmware.md).

### Jaké úlohy mohu ochránit pomocí Site Recovery?
Site Recovery dokáže replikovat jakékoli úlohy spuštěné na podporovaném virtuálním počítači nebo fyzickém serveru. Site Recovery poskytuje podporu replikace s ohledem na aplikace, aby je bylo možné obnovit do použitelného stavu. Integruje se s aplikacemi Microsoftu, například SharePointem, Exchange, Dynamics, SQL Serverem a Active Directory, a úzce spolupracuje s hlavními dodavateli včetně Oracle, SAP, IBM a Red Hatu. [Další informace](site-recovery-workload.md) o ochraně úloh.

### Musí se hostitelé Hyper-V nacházet v cloudech System Center VMM, aby je bylo možné replikovat pomocí Site Recovery?
Pokud chcete replikovat do sekundárního datacentra, virtuální počítače Hyper-V musí být umístěny na hostitelských serverech Hyper-V v cloudu VMM. Pokud chcete replikovat do Azure, můžete replikovat virtuální počítače na hostitelských serverech Hyper-V s cloudy VMM i bez nich. [Další informace](site-recovery-hyper-v-site-to-azure.md). 

### Mohu nasadit Site Recovery s VMM, když mám jen jeden server VMM?
Ano. Můžete buď replikovat virtuální počítače na serverech Hyper-V v cloudu VMM do Azure, nebo replikovat mezi cloudy VMM na jednom serveru. Pro replikaci mezi lokálními nasazeními doporučujeme mít server VMM jak na primární, tak na sekundární lokalitě. [Další informace](site-recovery-single-vmm.md)

### Jaké fyzické servery mohu ochránit?
Ochránit prostřednictvím Azure nebo sekundární lokality můžete fyzické servery, na kterých běží podporované operační systémy Windows a Linux. [Informace](site-recovery-vmware-to-azure-classic.md#before-you-start-deployment) o požadavcích na operační systémy pro replikaci do Azure nebo na sekundární lokalitu. Mějte na paměti, že fyzické servery, které se replikují do úložiště Azure, poběží po převzetí služeb při selhání v Azure jako virtuální počítače. Pokud budete z Azure vracet služby po obnovení zpět na místní lokalitu, není v tuto chvíli podporováno navrácení služeb na fyzický server. Navrátit služby po obnovení lze pouze na virtuální počítač běžící ve VMware.

### Jaké virtuální počítače VMware mohu ochránit?
Pokud chcete ochránit virtuální počítače VMware, potřebujete hypervisor vSphere a virtuální počítače, ve kterých běží nástroje VMware. Pro správu hypervisorů také doporučujeme mít server VMware vCenter. [Další informace](site-recovery-vmware-to-azure-classic.md#before-you-start-deployment) o přesných požadavcích na replikaci serverů VMware a virtuálních počítačů do Azure nebo na sekundární lokalitu.

### Mohu pomocí Site Recovery spravovat zotavení po havárii pro pobočky?
Ano. Když pomocí Site Recovery orchestrujete replikaci a převzetí služeb při selhání na pobočkách, můžete orchestrovat a zobrazit všechny úlohy poboček na centrálním místě. Z centrály můžete snadno provádět převzetí služeb při selhání a spravovat zotavení po havárii na všech pobočkách, aniž byste na nich museli být přítomni. 

## Zabezpečení
### Posílají se replikační data do služby Site Recovery?
Ne, Site Recovery nezachycuje replikovaná data ani nemá žádné informace o tom, co běží na virtuálních počítačích nebo fyzických serverech.

Replikační data se vyměňují mezi lokálními hostiteli Hyper-V, hypervisory VMware nebo fyzickými servery a úložištěm Azure nebo sekundární lokalitou.  Site Recovery nemá schopnost tato data zachytit. Do služby Site Recovery se odesílají jen metadata, která jsou nezbytná k orchestraci replikace a převzetí služeb při selhání. 

Site Recovery má certifikaci ISO 27001:2005 a probíhá u něj vyhodnocení HIPAA, DPA a FedRAMP JAB.

### Abychom dodrželi předpisy, musí i metadata z našich lokálních prostředí zůstat ve stejné geografické oblasti. Lze toho pomocí Site Recovery dosáhnout?
Ano. Když v některé oblasti vytvoříte trezor Site Recovery, zajišťujeme, že všechna metadata, která potřebujeme k umožnění a orchestraci replikace a převzetí služeb při selhání zůstávají uvnitř této geografické oblasti.

### Šifruje Site Recovery replikaci?
Pro virtuální počítače a fyzické servery replikující mezi místními lokalitami je podporováno šifrování během přenosu. Pro virtuální počítače a fyzické servery, které se replikují do Azure, je podporováno jak šifrování během přenosu, tak šifrování v klidovém stavu (v Azure). 

## Replikace
### Vztahují se na replikaci virtuálních počítačů do Azure nějaké požadavky?
Virtuální počítače, které chcete replikovat do Azure, by měly splňovat [požadavky pro Azure](site-recovery-best-practices.md#azure-virtual-machine-requirements).

### Mohu do Azure replikovat virtuální počítače Hyper-V generace 2?
Ano. Site Recovery převede virtuální počítače z generace 2 na generaci 1 během převzetí služeb při selhání. Při navrácení služeb po obnovení se virtuální počítač převede zpět na generaci 2. [Další informace](https://azure.microsoft.com/blog/2015/04/28/disaster-recovery-to-azure-enhanced-and-were-listening/).

### Pokud replikuji do Azure, jak platím za virtuální počítače Azure?
Během běžné replikace se data replikují do geograficky redundantního úložiště Azure. Nemusíte tedy platit žádné poplatky za virtuální počítače Azure IaaS.  Když spustíte převzetí služeb při selhání do Azure, Site Recovery automaticky vytvoří virtuální počítače Azure IaaS. Následně se vám budou fakturovat výpočetní prostředky, které v Azure spotřebujete.

### Existuje sada SDK, pomocí které mohu automatizovat postupy workflow Site Recovery?
Ano. Postupy workflow Site Recovery je možné automatizovat pomocí rozhraní REST API, PowerShellu a sady Azure SDK. Další informace o [nasazení Site Recovery pomocí PowerShellu a Azure Resource Manageru](site-recovery-deploy-with-powershell-resource-manager.md).

### Pokud replikuji do Azure, jaký typ účtu úložiště potřebuji?
Potřebujete účet úložiště se [standardním geograficky redundantním úložištěm](../storage/storage-introduction.md#replication-for-durability-and-high-availability). Storage úrovně Premium se v tuto chvíli nepodporuje.

### Jak často je možné replikovat data?
* **Hyper-V:** Virtuální počítače Hyper-V s Windows Serverem 2012 R2 je možné replikovat každých 30 sekund, 5 minut nebo 15 minut. Pokud jste nastavili replikaci SAN, bude replikace synchronní.
* **VMware a fyzické servery:** Četnost replikací zde není relevantní. Replikace bude průběžná. 

### Je možné rozšířit replikaci z existující lokality pro obnovení na jinou?
Rozšířená nebo zřetězená replikace není podporována. [Pošlete nám svůj názor](https://feedback.azure.com/forums/256299-site-recovery/suggestions/6097959-support-for-exisiting-extended-replication/) na tuto funkci.

### Mohu při první replikaci do Azure provést offline replikaci?
Toto není podporováno. [Pošlete nám svůj názor](https://feedback.azure.com/forums/256299-site-recovery/suggestions/6227386-support-for-offline-replication-data-transfer-from/) na tuto funkci.

### Mohu z replikace vyloučit konkrétní disky?
Toto není podporováno. [Pošlete nám svůj názor](https://feedback.azure.com/forums/256299-site-recovery/suggestions/6418801-exclude-disks-from-replication/) na tuto funkci.

### Mohu replikovat virtuální počítače s dynamickými disky?
Dynamické disky jsou podporovány, pokud se provádí replikace virtuálních počítačů Hyper-V. Podporovány jsou i v případě, že se replikují virtuální počítače VMware a fyzické počítače a používáte [rozšířené nasazení](site-recovery-vmware-to-azure-classic.md). Mějte na paměti, že disk s operačním systémem musí být základní disk.

### Je možné omezovat šířku pásma vyhrazenou pro přenosy replikace Hyper-V?
* Pokud replikujete virtuální počítače Hyper-V mezi dvěma místními lokalitami, můžete použít Windows QoS. Zde je ukázkový skript: 
  
        New-NetQosPolicy -Name ASRReplication -IPDstPortMatchCondition 8084 -ThrottleRate (2048*1024)
        gpupdate.exe /force
* Pokud replikujete virtuální počítače Hyper-V do Azure, můžete omezování nakonfigurovat pomocí následující ukázkové rutiny PowerShellu:
  
        Set-OBMachineSetting -WorkDay $mon, $tue -StartWorkHour "9:00:00" -EndWorkHour "18:00:00" -WorkHourBandwidth (512*1024) -NonWorkHourBandwidth (2048*1024)
* [Další informace](https://support.microsoft.com/en-us/kb/3056159) o omezování přenosu Hyper-V
* [Další informace](site-recovery-vmware-to-azure-classic.md#capacity-planning) o omezování replikace VMware do Azure

## Převzetí služeb při selhání
### Jakým způsobem získám přístup k virtuálním počítačům Azure po převzetí služeb při selhání do Azure?
K virtuálním počítačům Azure můžete přistoupit přes zabezpečené internetové připojení, síť site-to-site VPN nebo přes Azure ExpressRoute. Komunikace přes připojení VPN probíhá na vnitřních portech sítě Azure, na kterých jsou umístěny virtuální počítače. Komunikace přes internet je mapována na veřejné koncové body cloudové služby Azure pro virtuální počítače.

### Jak Azure při převzetí služeb při selhání zaručuje, že jsou má data odolná?
Služba Azure je pro odolnost navržena. Služba Site Recovery je vytvořena tak, aby v případě potřeby při selhání přebíralo služby sekundární datacentrum Azure. To je v souladu se smlouvou Azure SLA. Pokud se to stane, zajišťujeme, že metadata a trezory zůstávají ve stejné geografické oblasti, kterou jste si zvolili pro svůj trezor. 

### Pokud replikuji mezi dvěma lokalitami, co se stane, pokud na mé primární lokalitě dojde k nečekanému výpadku?
Ze sekundární lokality můžete aktivovat neplánované převzetí služeb při selhání. Site Recovery k provedení převzetí služeb při selhání nepotřebuje připojení z primární lokality.

### Je převzetí služeb při selhání automatické?
Převzetí služeb při selhání není automatické. Převzetí služeb zahajujete jediným kliknutím na portálu nebo je možné využít [rutiny PowerShellu pro Site Recovery](https://msdn.microsoft.com/library/dn850420.aspx) a s jejich pomocí převzetí služeb aktivovat. Navrácení služeb po obnovení obnáší sérii akcí na portálu Site Recovery.

Pokud chcete automatizovat převzetí služeb při selhání, můžete pomocí místního Orchestratoru nebo Operations Manageru detekovat selhání virtuálního počítače a prostřednictvím sady SDK aktivovat převzetí služeb.

## 
### Jsem poskytovatel služeb. Funguje Site Recovery pro vyhrazené i sdílené modely infrastruktury?
Ano, Site Recovery podporuje jak vyhrazené, tak sdílené modely infrastruktury.

### Sdílí se v případě poskytovatelů služeb identita klienta se službou Site Recovery?
Ne. Vaši klienti nepotřebují přístup k portálu Site Recovery. Pouze správce poskytovatele služeb pracuje s portálem.

### Přesunou se někdy data aplikací mých klientů do Azure?
Při replikaci mezi lokalitami ve vlastnictví poskytovatele služeb se data aplikací do Azure nikdy nepřenášejí. Data se šifrují během přenosu a replikují se přímo mezi lokalitami poskytovatele služeb.

Pokud replikujete do Azure, data aplikací se posílají do úložiště Azure, ale nikoli do služby Site Recovery. Během přenosu se data šifrují a v Azure zůstávají zašifrovaná. 

### Obdrží mí klienti fakturu za služby Azure?
Ne. Azure má fakturační vztah přímo s poskytovatelem služeb. Za generování konkrétních faktur pro své klienty má plnou odpovědnost poskytovatel služeb.

### Pokud replikuji do Azure, potřebujeme mít virtuální počítače v Azure spuštěné nepřetržitě?
Ne. Data se replikují na geograficky redundantní účet úložiště Azure ve vašem předplatném. Když provedete testovací převzetí služeb při selhání (rutina pro zotavení po havárii) nebo skutečné převzetí, Site Recovery ve vašem předplatném automaticky vytvoří virtuální počítače.

### Zajišťujete při replikaci do Azure izolaci na úrovni klienta?
Ano.

### Jaké platformy aktuálně podporujete?
Podporujeme nasazení Azure Pack, Cloud Platform System a System Center (2012 a vyšší). Další informace o integraci Azure Packu v tématu [Konfigurace ochrany virtuálních počítačů](https://technet.microsoft.com/library/dn850370.aspx).

### Podporujete nasazení s jediným Azure Packem a jediným serverem VMM?
Ano, můžete replikovat virtuální počítače Hyper-V a Azure nebo mezi lokalitami poskytovatele služeb.  Mějte na paměti, že pokud budete replikovat mezi lokalitami poskytovatele služeb, není k dispozici integrace runbooku Azure.

## Další kroky
* Projděte si [přehled Site Recovery](site-recovery-overview.md).
* Seznamte se s [architekturou Site Recovery](site-recovery-components.md).  

<!--HONumber=Jun16_HO2-->


