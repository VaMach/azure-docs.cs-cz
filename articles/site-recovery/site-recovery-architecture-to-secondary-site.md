---
title: "Jak ve službě Azure Site Recovery funguje replikace místních počítačů do sekundární místní lokality? | Dokumentace Microsoftu"
description: "Tento článek obsahuje přehled komponent a architektury používané při replikaci místních virtuálních počítačů a fyzických serverů do sekundární lokality s využitím služby Azure Site Recovery."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: c413efcd-d750-4b22-b34b-15bcaa03934a
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/29/2017
ms.author: raynew
ms.translationtype: Human Translation
ms.sourcegitcommit: db18dd24a1d10a836d07c3ab1925a8e59371051f
ms.openlocfilehash: fca95c63964b955db7ddfbe53250702cc8af122e
ms.contentlocale: cs-cz
ms.lasthandoff: 06/15/2017

---
# <a name="how-does-on-premises-machine-replication-to-a-secondary-site-work-in-site-recovery"></a>Jak ve službě Site Recovery funguje replikace místních počítačů do sekundární lokality?

Tento článek popisuje komponenty a procesy používané při replikaci místních virtuálních počítačů a fyzických serverů do Azure s využitím služby [Azure Site Recovery](site-recovery-overview.md).

Do sekundární místní lokality můžete replikovat následující:
- Místní virtuální počítače Hyper-V na clustery Hyper-V a samostatné hostitele, které jsou spravované v cloudech System Center Virtual Machine Manageru (VMM).
- Místní virtuální počítače VMware a fyzické servery s Windows nebo Linuxem. V tomto scénáři replikaci spravuje Scout.

Jakékoli dotazy můžete publikovat na konci tohoto článku nebo na [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="replicate-hyper-v-vms-to-a-secondary-on-premises-site"></a>Replikace virtuálních počítačů Hyper-V do sekundární místní lokality


### <a name="architectural-components"></a>Komponenty architektury

Dál je uvedeno, co potřebujete pro replikaci virtuálních počítačů Hyper-V do sekundární lokality.

**Komponenta** | **Umístění** | **Podrobnosti**
--- | --- | ---
**Azure** | Budete potřebovat účet Microsoft. |
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

**Obrázek 1: Replikace z VMM do VMM**

![Z lokálního prostředí do lokálního prostředí](./media/site-recovery-components/arch-onprem-onprem.png)

### <a name="failover-and-failback-process"></a>Proces převzetí služeb při selhání a navrácení služeb po obnovení

1. Můžete spustit plánované nebo neplánované [převzetí služeb při selhání](site-recovery-failover.md) mezi místními lokalitami. Pokud spustíte plánovanou operaci, dojde k ukončení zdrojových virtuálních počítačů, aby se zcela předešlo možné ztrátě dat.
2. Můžete převzít službu při selhání jednoho počítače nebo vytvořit [plány zotavení](site-recovery-create-recovery-plans.md) a orchestrovat převzetí služeb více počítačů.
4. Pokud proběhlo neplánované převzetí služeb při selhání sekundární lokalitou, po provedení převzetí služeb nebudou počítače v sekundární lokalitě chráněné pomocí replikace. Pokud jste spustili plánované převzetí služeb při selhání, počítače v sekundárním umístění chráněné budou.
5. Po potvrzení převzetí služeb můžete začít používat úlohu na replikovaném virtuálním počítači.
6. Až bude primární lokalita opět dostupná, zahájíte zpětnou replikaci ze sekundární lokality do primární. Po zpětné replikaci budou virtuální počítače v chráněném stavu, ale sekundární datové centrum bude stále aktivním umístěním.
7. Chcete-li z primární lokality opět udělat aktivní, zahajte plánované převzetí služeb ze sekundární lokality do primární, následované další zpětnou replikací.




## <a name="replicate-vmware-vmsphysical-servers-to-a-secondary-site"></a>Replikace virtuálních počítačů nebo fyzických serverů do sekundární lokality

Replikujete virtuální počítače VMware nebo fyzické servery do sekundární lokality pomocí nástroje InMage Scout a pomocí těchto komponent architektury:


### <a name="architectural-components"></a>Komponenty architektury

**Komponenta** | **Umístění** | **Podrobnosti**
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

**Obrázek 2: Replikace VMware do VMware**

![Z VMware do VMware](./media/site-recovery-components/vmware-to-vmware.png)


## <a name="next-steps"></a>Další kroky

Kontrola [matice podpory](site-recovery-support-matrix-to-sec-site.md)

