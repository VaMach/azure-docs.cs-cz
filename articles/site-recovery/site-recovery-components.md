---
title: Jak funguje Site Recovery? | Dokumentace Microsoftu
description: "Tento článek přináší přehled architektury Site Recovery."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: jwhit
editor: 
ms.assetid: c413efcd-d750-4b22-b34b-15bcaa03934a
ms.service: site-recovery
ms.workload: backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/14/2017
ms.author: raynew
translationtype: Human Translation
ms.sourcegitcommit: a087df444c5c88ee1dbcf8eb18abf883549a9024
ms.openlocfilehash: 4674985363bc1267449e018ab15a53757a8fd32d
ms.lasthandoff: 03/15/2017


---
# <a name="how-does-azure-site-recovery-work"></a>Jak funguje Azure Site Recovery?

Tento článek popisuje základní architekturu služby [Azure Site Recovery](site-recovery-overview.md) a komponenty, které jí umožňují fungovat.

Jakékoli dotazy můžete publikovat na konci tohoto článku nebo na [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="replicate-to-azure"></a>Replikování do Azure

Do Azure můžete replikovat následující:

- **VMware:** Místní virtuální počítače VMware spuštěné na [podporovaném hostiteli](site-recovery-support-matrix-to-azure.md#support-for-datacenter-management-servers). Můžete replikovat virtuální počítače VMware s [podporovanými operačními systémy](site-recovery-support-matrix-to-azure.md#support-for-replicated-machine-os-versions).
- **Hyper-V:** Místní virtuální počítače Hyper-V spuštěné na [podporovaných hostitelích](site-recovery-support-matrix-to-azure.md#support-for-datacenter-management-servers).
- **Fyzické počítače:** Místní fyzické servery s Windows nebo Linuxem spuštěné v [podporovaných operačních systémech](site-recovery-support-matrix-to-azure.md#support-for-replicated-machine-os-versions). Můžete replikovat virtuální počítače Hyper-V s jakýmkoli hostovaným operačním systémem, který [podporuje technologie Hyper-V a Azure](https://technet.microsoft.com/en-us/windows-server-docs/compute/hyper-v/supported-windows-guest-operating-systems-for-hyper-v-on-windows).

## <a name="vmware-to-azure"></a>Z VMware do Azure

Dál je uvedeno, co potřebujete pro replikaci virtuálních počítačů VMware do Azure.

Oblast | Komponenta | Podrobnosti
--- | --- | ---
**Azure** | Pro Azure potřebujete účet Azure, účet úložiště Azure a síť Azure. | Účty úložiště a sítě můžou být klasické účty nebo účty Resource Manageru.<br/><br/>  Replikovaná data jsou uložena v účtu úložiště a virtuální počítače Azure se z replikovaných dat vytvoří, jakmile dojde k převzetí místních služeb při selhání. Virtuální počítače Azure se připojí k virtuální síti Azure po svém vytvoření.
**Konfigurační server** | Na jednom serveru pro správu (virtuální počítač VMware) jsou spuštěné všechny místní komponenty – konfigurační server, procesový server a hlavní cílový server. | Konfigurační server koordinuje komunikaci mezi místním prostředím a Azure a spravuje replikaci dat.
 **Procesový server:**  | Obvykle se instaluje na konfigurační server. | Funguje jako replikační brána. Přijímá data replikace, optimalizuje je pomocí ukládání do mezipaměti, komprese a šifrování a odesílá je do úložiště Azure.<br/><br/> Procesový server se také stará o nabízenou instalaci služby Mobility na chráněné počítače a provádí automatické zjišťování virtuálních počítačů VMware.<br/><br/> Jak vaše nasazení poroste, můžete přidávat další samostatné vyhrazené procesní servery pro zpracování zvyšujícího se objemu replikačních přenosů.
 **Hlavní cílový server** | Obvykle se instaluje na místní konfigurační server. | Zpracovává replikační data během navracení služeb z Azure po obnovení.<br/><br/> Pokud je objem přenosů při navracení služeb po obnovení příliš vysoký, můžete nasadit samostatný hlavní cílový server pro účely navrácení služeb po obnovení.
**Servery VMware** | Virtuální počítače VMware jsou hostované na serverech vSphere ESXi a ke správě hostitelů doporučujeme použít server vCenter. | Servery VMware přidáte do trezoru služby Recovery Services.<br/><br/> I
**Replikované počítače** | Na každý virtuální počítač VMware, který chcete replikovat, se nainstaluje služba Mobility. Můžete ji nainstalovat na každý počítač ručně, nebo pomocí nabízené instalace z procesového serveru.

**Obr. 1: Komponenty pro replikaci z VMware do Azure**

![Komponenty](./media/site-recovery-components/arch-enhanced.png)

### <a name="replication-process"></a>Proces replikace

1. Nastavíte nasazení, včetně komponent Azure, a trezor služby Recovery Services. V trezoru zadáte zdroj a cíl replikace, nastavíte konfigurační server, přidáte servery VMware, vytvoříte zásady replikace, nasadíte službu Mobility, povolíte replikaci a spustíte test převzetí služby.
2.  Počítače se začnou replikovat v souladu se zásadami replikace a počáteční kopie dat se replikuje do úložiště Azure.
4. Replikace rozdílových dat do Azure začne po dokončení počáteční replikace. Sledované změny se pro jednotlivé počítače ukládají do souboru .hrl.
    - Počítače, které se replikují, komunikují s konfiguračním serverem kvůli správě replikace na příchozím portu HTTPS 443.
    - Replikující se počítače odesílají data do procesového serveru na příchozím portu HTTPS 9443 (nastavitelné).
    - Konfigurační server orchestruje správu replikace s Azure přes odchozí port HTTPS 443.
    - Procesový server přijímá data ze zdrojového počítače, optimalizuje je a šifruje, a pak je odesílá do úložiště Azure přes odchozí port 443.
    - Když povolíte konzistenci mezi více virtuálními počítači, budou spolu počítače v replikační skupině komunikovat na portu 20004. Konzistence více virtuálních počítačů znamená, že seskupíte víc virtuálních počítačů do replikační skupiny, v rámci které se sdílí body obnovení konzistentní vzhledem k selháním a konzistentní vzhledem k aplikacím, když dojde k převzetí služeb při selhání. To je užitečné, pokud je počítačích spuštěná stejná úloha a je třeba, aby zůstala konzistentní.
5. Provoz se přes internet replikuje do veřejných koncových bodů úložiště Azure. Alternativně můžete použít [veřejný partnerský vztah](https://docs.microsoft.com/en-us/azure/expressroute/expressroute-circuit-peerings#public-peering) Azure ExpressRoute. Přenos replikačních dat přes síť site-to-site VPN z místního serveru do Azure není podporovaný.

**Obr. 2: Replikace z VMware do Azure**

![Rozšířené](./media/site-recovery-components/v2a-architecture-henry.png)

### <a name="failover-and-failback"></a>Převzetí služeb při selhání a navrácení služeb po obnovení

1. Po ověření správného fungování testovacího převzetí služeb při selhání můžete podle potřeby spouštět neplánovaná převzetí služeb při selhání do Azure. Plánované převzetí služeb není podporované.
2. Můžete převzít služby při selhání jednoho počítače nebo vytvořit [plány obnovení](site-recovery-create-recovery-plans.md) pro převzetí služeb při selhání více virtuálních počítačů.
3. V průběhu převzetí služeb při selhání se v Azure vytvoří replika virtuálního počítače. Po potvrzení procesu převzetí služeb můžete začít používat úlohu na replikovaném virtuálním počítači Azure.
4. Až bude vaše místní lokalita opět dostupná, můžete službu navrátit. Nastavíte infrastrukturu navrácení služeb po obnovení, zahájíte replikaci počítače ze sekundární lokality na primární a spustíte neplánované převzetí služeb při selhání ze sekundární lokality. Jakmile toto navrácení služeb potvrdíte, budou data zpět v místní lokalitě a budete muset opět povolit replikaci do Azure. [Další informace](site-recovery-failback-azure-to-vmware.md)

Pro navrácení služby po obnovení existuje několik požadavků:


- **Dočasný procesní server v Azure**: Pokud chcete po převzetí služeb při selhání tyto služby po obnovení vrátit, budete muset pro zpracování replikace z Azure nastavit virtuální počítač Azure nakonfigurovaný jako procesní server. Tento virtuální počítač je možné po navrácení služeb po obnovení odstranit.
- **Připojení k síti VPN:** Pro navrácení služeb po obnovení budete potřebovat připojení k síti VPN (nebo Azure ExpressRoute) nastavené ze sítě Azure k místní lokalitě.
- **Samostatný lokální hlavní cílový server:** Místní hlavní cílový server se stará o navrácení služeb po obnovení. Hlavní cílový server je ve výchozím nastavení nainstalován na serveru pro správu, ale pokud po obnovení vracíte větší objemy přenosů, měli byste pro tento účel nastavit samostatný místní hlavní cílový server.
- **Zásady navrácení služeb po obnovení:** Pro zpětnou replikaci do vaší místní lokality budete potřebovat zásady navrácení služeb. Ty se vytvoří automaticky při vytvoření zásad replikace.

**Obr. 3: Navrácení služeb po obnovení – VMware nebo fyzický server**

![Navrácení služeb po obnovení](./media/site-recovery-components/enhanced-failback.png)

## <a name="physical-to-azure"></a>Z fyzických do Azure

Když replikujete fyzické místní servery do Azure, replikace používá také stejné komponenty a procesy jako [VMware do Azure](#vmware-replication-to-azure), ale všimněte si těchto rozdílů:

- Jako konfigurační server můžete místo virtuálního počítače VMware použít fyzický server.
- Pro navrácení služeb po obnovení budete potřebovat místní infrastrukturu VMware. Služby po obnovení nelze navrátit na fyzický server.

## <a name="hyper-v-to-azure"></a>Z Hyper-V do Azure

Dál je uvedeno, co potřebujete pro replikaci virtuálních počítačů Hyper-V do Azure.

**Oblast** | **Komponenta** | **Podrobnosti**
--- | --- | ---
**Azure** | Pro Azure potřebujete účet Microsoft Azure, účet úložiště Azure a síť Azure. | Účty úložiště a sítě můžou být klasické účty nebo účty Resource Manageru.<br/><br/> Replikovaná data jsou uložena v účtu úložiště a virtuální počítače Azure se z replikovaných dat vytvoří, jakmile dojde k převzetí místních služeb při selhání.<br/><br/> Virtuální počítače Azure se připojí k virtuální síti Azure po svém vytvoření.
**Server VMM** | Hostitelé Hyper-V umístění v cloudech VMM | Pokud jsou hostitelé Hyper-V spravovaní v cloudech VMM, zaregistrujete server VMM v trezoru služby Recovery Services.<br/><br/> Na serveru VMM nainstalujete zprostředkovatele Site Recovery, který bude orchestrovat replikaci pomocí Azure.<br/><br/> Ke konfiguraci mapování sítě potřebujete mít nastavenou logickou síť a síť virtuálních počítačů. Síť virtuálních počítačů musí být propojena na logickou síť, která je přidružena ke cloudu.
**Hostitel Hyper-V** | Servery Hyper-V je možné nasadit se serverem VMM nebo bez. | Pokud není k dispozici server VMM, zprostředkovatel Site Recovery se nainstaluje na hostitele, kde bude orchestrovat replikaci pomocí Site Recovery přes internet. Pokud je k dispozici server VMM, zprostředkovatel se nainstaluje na něj, a ne na hostitele.<br/><br/> Agent služby Recovery Services se nainstaluje na hostitele, kde bude zpracovávat replikaci dat.<br/><br/> Komunikace z poskytovatele i agenta je zabezpečená a šifrovaná. Šifrují se rovněž replikovaná data v úložišti Azure.
**Virtuální počítače Hyper-V** | Na hostitelském serveru Hyper-V potřebujete jeden nebo několik virtuálních počítačů. | Na virtuálních počítačích není výslovně potřeba nic instalovat.


### <a name="replication-process"></a>Proces replikace

1. Nastavíte komponenty Azure. Doporučujeme připravit účty úložiště a sítě ještě než začnete s nasazením Site Recovery.
2. Vytvoříte trezor služby Replication Services pro Site Recovery a nakonfigurujete jeho nastavení, včetně těchto:
    - Nastavení zdroje a cíle. Pokud nespravujete hostitele Hyper-V v cloudu VMM, vytvoříte pro cíl kontejner lokality Hyper-V a přidáte do něj hostitele Hyper-V. Pokud hostitele Hyper-V spravujete pomocí VMM, je zdrojem cloud VMM. Cílem je Azure.
    - Instalace zprostředkovatele Azure Site Recovery a agenta Microsoft Azure Recovery Services Pokud máte server VMM, zprostředkovatel bude nainstalován na něj a agent na každého hostitele Hyper-V. Pokud VMM nemáte, nainstalujte zprostředkovatele i agenta na každého hostitele.
    - Vytvoříte zásady replikace pro lokalitu Hyper-V nebo pro cloud VMM. Zásady se použijí pro všechny virtuální počítače na hostitelích v lokalitě nebo v cloudu.
    - Povolíte replikaci pro virtuální počítače Hyper-V. Počáteční replikace proběhne podle nastavených zásad replikace.
4. Pak budou sledovány změny dat a bude probíhat rozdílová replikace do Azure. Sledované změny se pro jednotlivé položky ukládají do souboru .hrl.
5. Nakonec otestujete převzetí služeb při selhání, abyste měli jistotu, že všechno funguje.

### <a name="failover-and-failback-process"></a>Proces převzetí služeb při selhání a navrácení služeb po obnovení

1. Můžete spustit plánované nebo neplánované [převzetí služeb při selhání](site-recovery-failover.md) z místních virtuálních počítačů Hyper-V do Azure. Pokud spustíte plánovanou operaci, dojde k ukončení zdrojových virtuálních počítačů, aby se zcela předešlo možné ztrátě dat.
2. Můžete převzít službu při selhání jednoho počítače nebo vytvořit [plány zotavení](site-recovery-create-recovery-plans.md) a orchestrovat převzetí služeb více počítačů.
4. Po převzetí služeb při selhání byste v Azure měli vidět vytvořené replikované virtuální počítače. Virtuálnímu počítači můžete přiřadit veřejnou IP adresu, když je to třeba.
5. Po potvrzení procesu převzetí služeb můžete začít používat úlohu na replikovaném virtuálním počítači Azure.
6. Až bude vaše místní lokalita opět dostupná, můžete [navrátit služby po obnovení](site-recovery-failback-from-azure-to-hyper-v.md). Zahájíte plánované převzetí služeb při selhání z Azure do primární lokality. Pro plánované převzetí služeb při selhání můžete vybrat navrácení služeb po obnovení do stejného virtuálního počítače nebo do alternativního umístění a synchronizovat změny mezi Azure a místním systémem, aby nedošlo ke ztrátě dat. Jakmile budou místní virtuální počítače vytvořené, můžete převzetí služeb při selhání potvrdit.

**Obr. 4: Replikace z lokality Hyper-V do Azure**

![Komponenty](./media/site-recovery-components/arch-onprem-azure-hypervsite.png)

**Obr. 5: Replikace z Hyper-V v cloudech VMM do Azure**

![Komponenty](./media/site-recovery-components/arch-onprem-onprem-azure-vmm.png)


## <a name="replicate-to-a-secondary-site"></a>Replikace virtuálních počítačů VMware do sekundární lokality

Do sekundární lokality můžete replikovat následující:

- **VMware:** Místní virtuální počítače VMware spuštěné na [podporovaném hostiteli](site-recovery-support-matrix-to-sec-site.md#on-premises-servers). Můžete replikovat virtuální počítače VMware s [podporovanými operačními systémy](site-recovery-support-matrix-to-sec-site.md#support-for-replicated-machine-os-versions).
- **Fyzické počítače:** Místní fyzické servery s Windows nebo Linuxem spuštěné v [podporovaných operačních systémech](site-recovery-support-matrix-to-sec-site.md#support-for-replicated-machine-os-versions).
- **Hyper-V:** Místní virtuální počítače Hyper-V spuštěné na [podporovaných hostitelích Hyper-V](site-recovery-support-matrix-to-sec-site.md#on-premises-servers) spravované v cloudech VMM. [Podporovaní hostitelé](site-recovery-support-matrix-to-azure.md#support-for-datacenter-management-servers). Můžete replikovat virtuální počítače Hyper-V s jakýmkoli hostovaným operačním systémem, který [podporuje technologie Hyper-V a Azure](https://technet.microsoft.com/en-us/windows-server-docs/compute/hyper-v/supported-windows-guest-operating-systems-for-hyper-v-on-windows).


## <a name="vmwarephysical-to-a-secondary-site"></a>Z VMware/fyzických do sekundární lokality

Replikujete virtuální počítače VMware nebo fyzické servery do sekundární lokality pomocí nástroje InMage Scout.

### <a name="components"></a>Komponenty

**Oblast** | **Komponenta** | **Podrobnosti**
--- | --- | ---
**Azure** | InMage Scout. | K získání nástroje InMage Scout potřebujete předplatné Azure.<br/><br/> Po vytvoření trezoru služby Recovery Services si stáhnete InMage Scout a nainstalujete nejnovější aktualizace pro nastavení nasazení.
**Procesový server** | Umístěný v primární lokalitě | Procesový server nasadíte, aby se staral o ukládání do mezipaměti, kompresi a optimalizaci dat.<br/><br/> Také obstará nabízenou instalaci nástroje Unified Agent na počítače, které chcete chránit.
**Konfigurační server** | Umístěný v sekundární lokalitě | Konfigurační server spravuje, konfiguruje a monitoruje nasazení – buď pomocí webu pro správu, nebo konzoly vContinuum.
**Server vContinuum** | Volitelné. Instaluje se do stejné lokality jako konfigurační server. | Poskytuje konzoli pro správu a monitorování chráněného prostředí.
**Hlavní cílový server** | Umístěný v sekundární lokalitě | Na hlavní cílový server se ukládají replikovaná data. Přijímá data z procesního serveru, vytváří repliku počítače v sekundární lokalitě a ukládá body pro uchovávání dat.<br/><br/> Počet hlavních cílových serverů, které potřebujete, závisí na počtu počítačů, které chráníte.<br/><br/> Pokud chcete po obnovení vrátit služby do primární lokality, potřebujete hlavní cílový server i tam. Na tento server je třeba nainstalovat Unified Agent.
**VMware ESX/ESXi a server vCenter** |  Virtuální počítače jsou hostované na hostitelích ESX nebo ESXi. Hostitelé se spravují pomocí serveru vCenter. | Pro replikaci virtuálních počítačů VMware budete potřebovat infrastrukturu VMware.
**Virtuální počítače / fyzické servery** |  Nástroj Unified Agent nainstalovaný na virtuálních počítačích nebo fyzických serverech VMware, které chcete replikovat. | Agent funguje jako zprostředkovatel komunikace mezi všemi součástmi.


### <a name="replication-process"></a>Proces replikace

1. V každé lokalitě si nastavte servery představující jednotlivé součásti (konfigurační, procesní, hlavní cílový) a nainstalujte si Unified Agent na počítače, které chcete replikovat.
2. Po počáteční replikaci odesílá agent na každém počítači na procesní server rozdílové replikační změny.
3. Procesní server tato data optimalizuje a přenese je na hlavní cílový server v sekundární lokalitě. Konfigurační server spravuje replikační proces.

**Obr. 6: Replikace z VMware do VMware**

![Z VMware do VMware](./media/site-recovery-components/vmware-to-vmware.png)



## <a name="hyper-v-to-a-secondary-site"></a>Z Hyper-V do sekundární lokality

Dál je uvedeno, co potřebujete pro replikaci virtuálních počítačů Hyper-V do sekundární lokality.


**Oblast** | **Komponenta** | **Podrobnosti**
--- | --- | ---
**Azure** | Potřebujete účet Microsoft Azure. |
**Server VMM** | Doporučujeme jeden server VMM v primární lokalitě a další v sekundární lokalitě. | Oba servery VMM musí být připojené k internetu.<br/><br/> Každý server musí mít alespoň jeden privátní cloud VMM s nastavenou profilovou sadou schopností Hyper-V.<br/><br/> Nainstalujete zprostředkovatele Azure Site Recovery na server VMM. Zprostředkovatel koordinuje a orchestruje replikaci pomocí služby Site Recovery přes internet. Komunikace mezi poskytovatelem a Azure je zabezpečená a šifrovaná.
**Server Hyper-V** |  Jeden nebo několik hostitelských serverů Hyper-V v primárním a sekundárním cloudu VMM.<br/><br/> Servery by měly být připojené k internetu.<br/><br/> Data se replikují mezi primárním a sekundárním hostitelským serverem Hyper-V přes síť LAN nebo VPN na základě protokolu Kerberos nebo ověření certifikátem.  
**Virtuální počítače Hyper-V** | Umístěné na zdrojovém hostitelském serveru Hyper-V. | Zdrojový hostitelský server musí mít alespoň jeden virtuální počítač, který chcete replikovat.

### <a name="replication-process"></a>Proces replikace

1. Připravíte účet Azure.
2. Vytvoříte trezor služby Replication Services pro Site Recovery a nakonfigurujete jeho nastavení, včetně těchto:

    - Zdroj a cíl replikace (primární a sekundární lokality)
    - Instalace zprostředkovatele Azure Site Recovery a agenta Microsoft Azure Recovery Services Zprostředkovatel bude nainstalován na servery VMM a agent na každého hostitele Hyper-V.
    - Vytvoříte zásady replikace pro zdrojový cloud VMM. Zásady se použijí pro všechny virtuální počítače na hostitelích v cloudu.
    - Povolíte replikaci pro virtuální počítače Hyper-V. Počáteční replikace proběhne podle nastavených zásad replikace.
4. Pak budou sledovány změny dat a bude probíhat rozdílová replikace. Sledované změny se pro jednotlivé položky ukládají do souboru .hrl.
5. Nakonec otestujete převzetí služeb při selhání, abyste měli jistotu, že všechno funguje.

**Obrázek 7: Replikace z VMM do VMM**

![Z lokálního prostředí do lokálního prostředí](./media/site-recovery-components/arch-onprem-onprem.png)

### <a name="failover-and-failback"></a>Převzetí služeb při selhání a navrácení služeb po obnovení

1. Můžete spustit plánované nebo neplánované [převzetí služeb při selhání](site-recovery-failover.md) mezi místními lokalitami. Pokud spustíte plánovanou operaci, dojde k ukončení zdrojových virtuálních počítačů, aby se zcela předešlo možné ztrátě dat.
2. Můžete převzít službu při selhání jednoho počítače nebo vytvořit [plány zotavení](site-recovery-create-recovery-plans.md) a orchestrovat převzetí služeb více počítačů.
4. Pokud proběhlo neplánované převzetí služeb při selhání sekundární lokalitou, po provedení převzetí služeb nebudou počítače v sekundární lokalitě chráněné pomocí replikace. Pokud jste spustili plánované převzetí služeb při selhání, počítače v sekundárním umístění chráněné budou.
5. Po potvrzení převzetí služeb můžete začít používat úlohu na replikovaném virtuálním počítači.
6. Až bude primární lokalita opět dostupná, zahájíte zpětnou replikaci ze sekundární lokality do primární. Po zpětné replikaci budou virtuální počítače v chráněném stavu, ale sekundární datové centrum bude stále aktivním umístěním.
7. Chcete-li z primární lokality opět udělat aktivní, zahajte plánované převzetí služeb ze sekundární lokality do primární, následované další zpětnou replikací.


## <a name="next-steps"></a>Další kroky

- [Další informace](site-recovery-hyper-v-azure-architecture.md) o pracovním postupu replikace Hyper-V
- [Kontrola požadavků](site-recovery-prereq.md)

