<properties
    pageTitle="Jak funguje Site Recovery? | Microsoft Azure"
    description="Tento článek přináší přehled architektury Site Recovery."
    services="site-recovery"
    documentationCenter=""
    authors="rayne-wiselman"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="site-recovery"
    ms.workload="backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="03/27/2016"
    ms.author="raynew"/>

# Jak funguje Azure Site Recovery?

Tento článek vám pomůže porozumět základní architektuře služby Azure Site Recovery a komponentám, které jí umožňují fungovat. 

Jakékoli dotazy nebo připomínky můžete publikovat na konci tohoto článku nebo na [fóru služby Azure Site Recovery](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## Přehled

Organizace potřebují strategii pro provozní kontinuitu a zotavení po havárii (BCDR), která určuje, jakým způsobem zůstanou aplikace, procesy a data dostupné během plánovaných a neplánovaných výpadků a jakým způsobem se co nejdříve obnoví normální pracovní podmínky.

Site Recovery je služba Azure, která přispívá ke strategii BCDR tím, že orchestruje replikaci lokálních fyzických serverů a virtuálních počítačů do cloudu (Azure) nebo do sekundární lokality. Pokud dojde k výpadkům ve vaší primární lokalitě, předáte služby do sekundární lokality, aby aplikace a úlohy zůstaly dostupné. Až se obnoví normální provozní podmínky, předáte služby zpět do primární lokality.

Site Recovery je možné nasadit pro orchestraci replikace v řadě scénářů:

- **Replikace virtuálních počítačů VMware**: Lokální virtuální počítače VMware můžete replikovat do [Azure](site-recovery-vmware-to-azure-classic.md) nebo [sekundárního datacentra](site-recovery-vmware-to-vmware.md).
- **Replikace fyzických počítačů**: Fyzické počítače s Windows nebo Linuxem můžete replikovat do [Azure](site-recovery-vmware-to-azure-classic.md) nebo [sekundárního datacentra](site-recovery-vmware-to-vmware.md).
- **Replikace virtuálních počítačů Hyper-V spravovaných v cloudech System Center VMM**: Lokální virtuální počítače Hyper-V v cloudech VMM můžete replikovat do [Azure](site-recovery-vmm-to-azure.md) nebo [sekundárního datacentra](site-recovery-vmm-to-vmm.md). 
- **Replikace virtuálních počítačů Hyper-V (bez VMM)**: Virtuální počítače Hyper-V nespravované přes VMM můžete replikovat do [Azure](site-recovery-hyper-v-site-to-azure.md).
- **Migrace virtuálních počítačů**: Pomocí Site Recovery můžete [migrovat virtuální počítače Azure IaaS](site-recovery-migrate-azure-to-azure.md) mezi regiony nebo [migrovat instance AWS Windows](site-recovery-migrate-aws-to-azure.md) na virtuální počítače Azure IaaS. Aktuálně je podporována pouze migrace, což znamená, že při selhání můžete převzít služby těchto virtuálních počítačů, ale nemůžete je po obnovení navrátit.

Site Recovery může replikovat většinu aplikací běžících na těchto virtuálních počítačích a fyzických serverech. Úplný souhrn podporovaných aplikací naleznete v tématu [Jaké úlohy je možné chránit pomocí Azure Site Recovery?](site-recovery-workload.md).

## Replikace lokálních virtuálních počítačů VMware nebo fyzických serverů do Azure

Aktuálně jsou pro replikaci virtuálních počítačů VMware nebo fyzických serverů s Windows nebo Linuxem do Azure k dispozici dvě různé architektury:

- [Starší verze architektury](site-recovery-vmware-to-azure-classic-legacy.md): Tato architektura se nesmí používat pro nová nasazení. 
- [Rozšířená architektura](site-recovery-vmware-to-azure-classic.md): Toto je nejnovější architektura a měla by se používat pro všechna nová nasazení. Pokud jste již tento scénář nasadili do starší verze architektury, [podívejte se, jak provést migraci](site-recovery-vmware-to-azure-classic-legacy.md#migrate-to-the-enhanced-deployment) do rozšířeného nasazení.

V rozšířeném nasazení budete muset nastavit lokální server pro správu se všemi součástmi Site Recovery. Na každém počítači, který chcete chránit, automaticky (nabízeně) nainstalujete službu Mobility (případně to provedete ručně). Po počáteční replikaci odesílá služba Mobility na počítači rozdílová replikační data na procesní server, který je před odesláním do úložiště Azure optimalizuje.

![Rozšířené](./media/site-recovery-components/arch-enhanced.png)
![Rozšířené](./media/site-recovery-components/arch-enhanced2.png)

### Lokálně
Zde je seznam, co potřebujete lokálně:

- **Server pro správu**: Budete potřebovat počítač se systémem Windows Server 2012 R2, který bude fungovat jako server pro správu. Na tento server nainstalujete pomocí jednoho instalačního souboru všechny tyto součásti Site Recovery:

    - **Konfigurační server**: Koordinuje komunikaci mezi lokálním prostředím a Azure a spravuje replikaci a obnovení dat.
    - **Procesní server**: Funguje jako replikační brána. Přijímá replikační data z chráněných zdrojových počítačů, optimalizuje je pomocí ukládání do mezipaměti, komprese a šifrování a odesílá je do úložiště Azure. Rovněž se stará o nabízenou instalaci služby Mobility na chráněné počítače a provádí automatického zjišťování virtuálních počítačů VMware. Jak vaše nasazení poroste, můžete přidávat další samostatné vyhrazené procesní servery pro zpracování zvyšujícího se objemu replikačních přenosů.
    - **Hlavní cílový server**: Zpracovává replikační data během navracení služeb z Azure po obnovení. 
- **Hostitelé VMware ESX/ESXi a server vCenter**: Budete potřebovat nejméně jeden hostitelský server ESX/ESXi, na kterém poběží virtuální počítače VMware. Doporučujeme vám, abyste nasadili server vCenter pro správu těchto hostitelů. **Poznámka:** **I v případě, že fyzické servery replikujete, musíte služby po obnovení vrátit na VMware**. Když replikujete fyzický server, poběží po převzetí služeb při selhání do Azure jako virtuální počítač Azure. Po obnovení se služby vrátí do lokálního systému jako virtuální počítače VMware. 
    
- **Virtuální počítače/fyzické servery**: Na každý počítač, který chcete replikovat do Azure, bude nutné nainstalovat službu Mobility. Tato služba zaznamenává datové zápisy na počítači a předává je na procesní server. Tuto součást můžete nainstalovat ručně nebo ji může automaticky (v režimu nabízení) nainstalovat procesní server, když aktivujete replikaci pro počítač.

### Azure

Zde je seznam toho, co budete potřebovat v infrastruktuře Azure:     - **Účet Azure**: Budete potřebovat účet Microsoft Azure.
    - **Úložiště Azure**: K ukládání replikovaných dat budete potřebovat účet úložiště Azure. Replikovaná data jsou uložena v úložišti Azure a virtuální počítače Azure se spouštějí, když je třeba převzít služby při selhání. 
    - **Síť Azure**: Budete potřebovat virtuální síť Azure, ke které se budou spuštěné virtuální počítače Azure připojovat, když dojde k převzetí služeb při selhání. 
    
    
### Navrácení služeb po obnovení

Když se po obnovení služby vracejí do místního systému, probíhá to vždy na virtuální počítače VMware, a to i v případě převzetí služeb při selhání fyzického serveru. Zde je seznam toho, co budete potřebovat:

- **Dočasný procesní server v Azure**: Pokud chcete po převzetí služeb při selhání tyto služby po obnovení vrátit, budete muset pro zpracování replikace z Azure nastavit virtuální počítač Azure nakonfigurovaný jako procesní server. Tento virtuální počítač je možné po navrácení služeb po obnovení odstranit.
- **Připojení k síti VPN**: Pro navrácení služeb po obnovení budete potřebovat připojení k síti VPN (nebo Azure ExpressRoute) nastavené ze sítě Azure k místní lokalitě.
- **Samostatný lokální hlavní cílový server**: Místní hlavní cílový server se stará o navrácení služeb po obnovení. Hlavní cílový server je ve výchozím nastavení nainstalován na serveru pro správu, ale pokud po obnovení vracíte větší objemy přenosů, měli byste pro tento účel nastavit samostatný místní hlavní cílový server. 

![Rozšířené navrácení služeb po obnovení](./media/site-recovery-components/enhanced-failback.png)

[Zde jsou další informace](site-recovery-vmware-to-azure-classic.md#before-you-start-deployment) o požadavcích u rozšířeného nasazení.
[Zde jsou další informace](site-recovery-failback-azure-to-vmware-classic.md) o navrácení služeb po obnovení u rozšířeného nasazení.




## Replikace virtuálních počítačů Hyper-V v cloudech VMM do Azure

Pokud uplatňujete tento scénář, pak během nasazování Site Recovery nainstalujete zprostředkovatele Azure Site Recovery na server VMM. Zprostředkovatel koordinuje a orchestruje replikaci pomocí služby Site Recovery přes internet. Agent Služeb zotavení Azure se nainstaluje během nasazení Site Recovery na hostitelský server Hyper-V a replikace dat mezi ním a úložištěm Azure probíhá přes protokol HTTPS 443. Komunikace z poskytovatele i agenta je zabezpečená a šifrovaná. Šifrují se rovněž replikovaná data v úložišti Azure.

- Lokálně: 
    - **Server VMM**: Alespoň jeden server VMM nastavený přinejmenším s jedním privátním cloudem VMM. Server by měl být běžet na platformě System Center 2012 R2 a mít připojení k internetu. Pokud chcete zajistit, že virtuální počítače Azure budou připojeny k síti po převzetí služeb při selhání, budete muset nastavit síťové mapování. Abyste toho dosáhli, musí být zdrojové virtuální počítače připojeny k síti virtuálních počítačů ve VMM. Tato síť virtuálních počítačů musí být propojena na logickou síť, která je přidružena ke cloudu.
    - **Server Hyper-V**: Alespoň jeden hostitelský server Hyper-V umístěný v cloudu VMM. Na hostitelích Hyper-V by měl běžet Windows Server 2012 R2.
    - **Chráněné počítače**: Zdrojový hostitelský server Hyper-V musí mít přinejmenším jeden virtuální počítač, který chcete chránit.
    
- Azure: 
    - **Účet Azure**: Budete potřebovat účet Microsoft Azure.
    - **Úložiště Azure**: K ukládání replikovaných dat budete potřebovat účet úložiště Azure. Replikovaná data jsou uložena v úložišti Azure a virtuální počítače Azure se spouštějí, když je třeba převzít služby při selhání.
    - **Síť Azure**: Pokud chcete nastavit mapování sítě, aby virtuální počítače Azure zůstaly po převzetí služeb při selhání připojeny k sítím, budete muset nastavit síť Azure.

    ![Z VMM do Azure](./media/site-recovery-components/arch-onprem-onprem-azure-vmm.png)

Seznamte se s přesnými [požadavky na nasazení](site-recovery-vmm-to-azure.md#before-you-start).

## Replikace virtuálních počítačů VMware nebo fyzických serverů do sekundární lokality

Pokud chcete replikovat virtuální počítače VMware nebo fyzické servery se systémem Windows či Linux do sekundární lokality, stáhněte si nástroj InMage Scout, který je součástí předplatného Azure Site Recovery. V každé lokalitě si nastavte servery představující jednotlivé součásti (konfigurační, procesní, hlavní cílový) a nainstalujte si Unified Agent na počítače, které chcete replikovat. Po počáteční replikaci odesílá agent na každém počítači rozdílové replikační změny na procesní server. Procesní server tato data optimalizuje a přenese je na hlavní cílový server v sekundární lokalitě. Konfigurační server spravuje replikační proces.

![Z VMware do VMware](./media/site-recovery-components/vmware-to-vmware.png)

### Místní primární lokalita

- **Procesní server**: Nastavte si v primární lokalitě součást, kterou představuje procesní server. Ten se bude starat o ukládání do mezipaměti, kompresi a optimalizaci data. Také obstará nabízenou instalaci nástroje Unified Agent na počítače, které chcete chránit. 
- **VMware ESX/ESXi a server vCenter**: Pokud chráníte virtuální počítače VMware, budete potřebovat hypervisor VMware EXS/ESXi a volitelně server VMware vCenter pro správu hypervisorů.
- **Virtuální počítače/fyzické servery**: Virtuální počítače VMware nebo fyzické servery s Windows nebo Linuxem, které chcete chránit, budou potřebovat, aby byl nainstalován Unified Agent. Unified Agent je také nainstalován na počítačích, které fungují jako hlavní cílový server. Agent funguje jako zprostředkovatel komunikace mezi všemi součástmi. 
    
### Místní sekundární lokalita
 
- **Konfigurační server**: Jde o první součást, kterou nainstalujete, a to na sekundární lokalitu pro účely správy, konfigurace a monitorování vašeho nasazení – buď pomocí webu pro správu, nebo konzole vContinuum. V nasazení je pouze jediný konfigurační server a musí být nainstalován na počítači se systémem Windows Server 2012 R2.
- **Server vContinuum**: Je nainstalován ve stejném umístění (sekundární lokalita) jako konfigurační server. Poskytuje konzoli pro správu a monitorování chráněného prostředí. Ve výchozí instalaci je server vContinuum prvním hlavním cílovým serverem a je na něm nainstalován Unified Agent.
- **Hlavní cílový server**: Hlavní cílový server uchovává replikovaná data. Přijímá data z procesního serveru, vytváří repliku počítače v sekundární lokalitě a ukládá body pro uchovávání dat. Počet hlavních cílových serverů, které potřebujete, závisí na počtu počítačů, které chráníte. Pokud chcete po obnovení služby vrátit do primární lokality, potřebujete hlavní cílový server i tam. 

### Azure

Tento scénář nasadíte pomocí nástroje InMage Scout. K jeho získání budete potřebovat předplatné Azure. Po vytvoření trezoru Site Recovery si stáhnete InMage Scout a nainstalujete nejnovější aktualizace pro nastavení nasazení.


## Replikace virtuálních počítačů Hyper-V do Azure (bez VMM)

Pokud chcete virtuální počítače Hyper-V nespravované v cloudech VMM replikovat do Azure, nainstalujete během nasazování Site Recovery zprostředkovatele Azure Site Recovery a agenta Služeb zotavení Azure na hostitele Hyper-V. Zprostředkovatel koordinuje a orchestruje replikaci pomocí služby Site Recovery přes internet. Agent zpracovává replikovaná data dat přes HTTPS 443. Komunikace z poskytovatele i agenta je zabezpečená a šifrovaná. Šifrují se rovněž replikovaná data v úložišti Azure.

![Z lokality Hyper-V do Azure](./media/site-recovery-components/arch-onprem-azure-hypervsite.png)

### Lokálně

- **Server Hyper-V**: Alespoň jeden hostitelský server Hyper-V. Na hostitelích Hyper-V by měl běžet Windows Server 2012 R2.
- **Chráněné počítače**: Zdrojový hostitelský server Hyper-V musí mít přinejmenším jeden virtuální počítač, který chcete chránit.
    
### Azure

- **Účet Azure**: Budete potřebovat účet Microsoft Azure.
- **Úložiště Azure**: K ukládání replikovaných dat budete potřebovat účet úložiště Azure. Replikovaná data jsou uložena v úložišti Azure a virtuální počítače Azure se spouštějí, když je třeba převzít služby při selhání.

[Zde jsou další informace](site-recovery-hyper-v-site-to-azure.md#before-you-start) o požadavcích na nasazení.


## Replikace virtuálních počítačů Hyper-V v cloudech VMM do Azure

Pokud uplatňujete tento scénář, pak během nasazování Site Recovery nainstalujete zprostředkovatele Azure Site Recovery na server VMM a agenta Služeb zotavení Azure na hostitele Hyper-V. Zprostředkovatel koordinuje a orchestruje replikaci pomocí služby Site Recovery přes internet. Agent zpracovává replikovaná data dat přes HTTPS 443. Komunikace z poskytovatele i agenta je zabezpečená a šifrovaná. Zašifrovaná jsou rovněž data replikovaná v úložišti Azure (v klidovém stavu).

![Z VMM do Azure](./media/site-recovery-components/arch-onprem-onprem-azure-vmm.png)

### Lokálně

- **Server VMM**: Alespoň jeden server VMM nastavený přinejmenším s jedním privátním cloudem VMM. Server by měl být běžet na platformě System Center 2012 R2 a mít připojení k internetu. Pokud chcete zajistit, že virtuální počítače Azure budou připojeny k síti po převzetí služeb při selhání, budete muset nastavit síťové mapování. K tomu potřebujete připojit zdrojové virtuální počítače k síti virtuálních počítačů ve VMM. Tato síť musí být propojena na logickou síť, která je přidružena ke cloudu.
- **Server Hyper-V**: Alespoň jeden hostitelský server Hyper-V umístěný v cloudu VMM. Na hostitelích Hyper-V by měl běžet Windows Server 2012 R2.
- **Chráněné počítače**: Zdrojový hostitelský server Hyper-V musí mít přinejmenším jeden virtuální počítač, který chcete chránit.
    
### Azure

- **Účet Azure**: Budete potřebovat účet Microsoft Azure.
- **Úložiště Azure**: K ukládání replikovaných dat budete potřebovat účet úložiště Azure. Replikovaná data jsou uložena v úložišti Azure a virtuální počítače Azure se spouštějí, když je třeba převzít služby při selhání.
- **Síť Azure**: Pokud chcete zajistit, aby virtuální počítače Azure zůstaly připojeny k sítím po převzetí služeb při selhání, budete muset nastavit síťové mapování. K tomu budete potřebovat síť Azure.

[Zde jsou další informace](site-recovery-vmm-to-azure.md#before-you-start) o požadavcích na nasazení.

## Replikace virtuálních počítačů Hyper-V do sekundárního datacentra

Pokud uplatňujete tento scénář, pak během nasazování Site Recovery nainstalujete zprostředkovatele Azure Site Recovery na server VMM. Zprostředkovatel koordinuje a orchestruje replikaci pomocí služby Site Recovery přes internet. Data se replikují mezi primárním a sekundárním hostitelským serverem Hyper-V přes síť LAN nebo VPN na základě protokolu Kerberos nebo ověření certifikátem. Komunikace z poskytovatele i mezi hostitelskými servery Hyper-V je zabezpečená a šifrovaná. 

![Z lokálního prostředí do lokálního prostředí](./media/site-recovery-components/arch-onprem-onprem.png)

### Lokálně

- **Server VMM**: Doporučujeme mít jeden server VMM v primární lokalitě a další v sekundární lokalitě, přičemž každý by měl obsahovat alespoň jeden privátní cloud VMM. Na serveru by měl běžet přinejmenším System Center 2012 SP1 s nejnovějšími aktualizacemi a měl by být připojen k internetu. Cloudy musí mít nastavenu profilovou sadu schopností Hyper-V.
- **Server Hyper-V**: Hostitelské servery Hyper-V by se měly nacházet v primárních a sekundárních cloudech VMM. Na hostitelských serverech musí běžet přinejmenším Windows Server 2012, na kterém jsou nainstalovány nejnovější aktualizace, a musí být připojeny k internetu.
- **Chráněné počítače**: Zdrojový hostitelský server Hyper-V musí mít přinejmenším jeden virtuální počítač, který chcete chránit.
    
### Azure

Budete potřebovat předplatné Azure.

[Zde jsou další informace](site-recovery-vmm-to-vmm.md#before-you-start) o požadavcích na nasazení.


## Replikace virtuálních počítačů Hyper-V do sekundárního datacentra s replikací sítě SAN

V tomto scénáři nainstalujete během nasazování Site Recovery zprostředkovatele Azure Site Recovery na servery VMM. Zprostředkovatel koordinuje a orchestruje replikaci pomocí služby Site Recovery přes internet. Data se replikují mezi primárním a sekundárním polem úložiště s použitím synchronní replikace v síti SAN.

![Replikace sítě SAN](./media/site-recovery-components/arch-onprem-onprem-san.png)

### Lokálně

- **Pole SAN**: [Podporované pole SAN](http://social.technet.microsoft.com/wiki/contents/articles/28317.deploying-azure-site-recovery-with-vmm-and-san-supported-storage-arrays.aspx) spravované primárním serverem VMM. Síť SAN sdílí síťovou infrastrukturu s jiným polem SAN v sekundární lokalitě.
- **Server VMM**: Doporučujeme mít jeden server VMM v primární lokalitě a další v sekundární lokalitě, přičemž každý by měl obsahovat alespoň jeden privátní cloud VMM. Na serveru by měl běžet přinejmenším System Center 2012 SP1 s nejnovějšími aktualizacemi a měl by být připojen k internetu. Cloudy musí mít nastavenu profilovou sadu schopností Hyper-V.
- **Server Hyper-V**: Hostitelské servery Hyper-V nacházející se v primárních a sekundárních cloudech VMM. Na hostitelských serverech musí běžet přinejmenším Windows Server 2012, na kterém jsou nainstalovány nejnovější aktualizace, a musí být připojeny k internetu.
- **Chráněné počítače**: Zdrojový hostitelský server Hyper-V musí mít přinejmenším jeden virtuální počítač, který chcete chránit.
    
### Azure

Budete potřebovat předplatné Azure.  

[Zde jsou další informace](site-recovery-vmm-san.md#before-you-start) o požadavcích na nasazení.


## Životní cyklus ochrany Hyper-V

Tento pracovní postup nastiňuje proces ochrany, replikace a převzetí služeb při selhání u virtuálních počítačů Hyper-V. 

1. **Povolení ochrany**: Nastavíte trezor Site Recovery, nakonfigurujete nastavení replikace pro cloud VMM nebo web Hyper-V a povolíte ochranu pro virtuální počítače. Spustí se úloha s názvem **Povolení ochrany** a lze ji monitorovat na kartě **Úlohy**. V rámci úlohy se zkontroluje, zda počítač splňuje požadavky, a potom se vyvolá metoda [CreateReplicationRelationship](https://msdn.microsoft.com/library/hh850036.aspx), která nastaví replikaci do Azure na základě nastavení, které jste nakonfigurovali. Úloha **Povolení ochrany** také vyvolá metodu [StartReplication](https://msdn.microsoft.com/library/hh850303.aspx) pro inicializaci úplné replikace virtuálního počítače.
2. **Počáteční replikace**: Pořídí se snímek virtuálního počítače a virtuální pevné disky se replikují jeden po druhém, dokud se všechny nezkopírují do Azure nebo do sekundárního datacentra. Čas potřebný k dokončení závisí na velikosti virtuálního počítače, šířce pásma sítě a metodě počáteční replikace. Pokud dojde ke změnám na disku v době, kdy probíhá počáteční replikace, Hyper-V Replica Replication Tracker je zaznamená jako protokoly replikace technologie Hyper-V (.hrl), které jsou umístěny ve stejné složce jako disky. Každý disk má přidružený soubor .hrl, který se odešle do sekundárního úložiště. Mějte na paměti, že soubory snímků a protokolů spotřebovávají prostředky disku v době, kdy probíhá počáteční replikace. Po dokončení počáteční replikace se snímek virtuálního počítače odstraní a rozdílové změny na disku v protokolu se synchronizují a sloučí.
3. **Dokončení ochrany**: Po počáteční replikaci nakonfiguruje úloha **Dokončení ochrany** síťová a další postreplikační nastavení tak, aby virtuální počítač byl chráněn. Pokud replikujete do Azure, možná bude nutné upravit nastavení pro virtuální počítač tak, aby byl připraven k převzetí služeb při selhání. V tomto bodě můžete spustit test převzetí služeb při selhání a zkontrolovat, zda vše funguje podle očekávání.
4. **Replikace**: Po počáteční replikaci se zahájí rozdílová synchronizace, a to v souladu s nastavením replikace. 
    - **Selhání replikace**: Pokud rozdílová replikace selže a úplná replikace by byla náročná, pokud jde o šířku pásma nebo čas, pak dojde k resynchronizaci. Pokud například soubory .hrl dosáhnou 50 % velikosti disku, pak se u virtuálního počítače stanoví nutnost resynchronizace. Resynchronizace minimalizuje množství dat odesílaných v rámci výpočtů kontrolních součtů zdrojových a cílových virtuálních počítačů tím, že se posílají pouze rozdíly. Po dokončení synchronizace bude pokračovat rozdílová replikace. Ve výchozím nastavení je automatické spuštění resynchronizace naplánováno na mimopracovní dobu, ale můžete ji u virtuálního počítače spustit i ručně.
    - **Chyba replikace**: Pokud dojde k chybě replikace, je předdefinován opakovaný pokus. Jde-li o neopravitelnou chybu, například související s ověřením nebo autorizací, nebo je-li v neplatném stavu počítač představující repliku, k opakovanému pokusu nedojde. Pokud se jedná o opravitelnou chybu, jako je chyba sítě nebo nedostatek volného místa na disku či paměti, pak se pokusy opakují, ale postupně se mezi nimi prodlužuje interval (proběhnou po 1, 2, 4, 8 a 10 minutách a pak každých 30 minut).
4. **Plánované/neplánované převzetí služeb při selhání**: Podle potřeby můžete spustit plánované, nebo neplánované převzetí služeb při selhání. Pokud spustíte plánovanou operaci, dojde k ukončení zdrojových virtuálních počítačů, aby se zcela předešlo možné ztrátě dat. Virtuální počítače představující repliky přejdou po svém vytvoření do stavu čekání na potvrzení. Pro dokončení převzetí služeb při selhání je musíte potvrdit, což ale neplatí při replikaci sítě SAN, protože v takovém případě proběhne potvrzení automaticky. Po nastavení a spuštění primární lokality může proběhnout navrácení služeb po obnovení. Pokud jste nastavili replikaci do Azure, bude zpětná replikace automatická. V opačném případě ji spustíte ručně.
 

![pracovní postup](./media/site-recovery-components/arch-hyperv-azure-workflow.png)

## Další kroky

[Příprava nasazení](site-recovery-best-practices.md)



<!--HONumber=Jun16_HO2-->


