---
title: Jak funguje replikace VMware do Azure v Azure Site Recovery? | Dokumentace Microsoftu
description: "Tento článek obsahuje přehled komponent a architektury používané při replikaci místních virtuálních počítačů VMware a fyzických serverů do Azure s využitím služby Azure Site Recovery."
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
ROBOTS: NOINDEX, NOFOLLOW
redirect_url: vmware-walkthrough-architecture
ms.translationtype: Human Translation
ms.sourcegitcommit: 138f04f8e9f0a9a4f71e43e73593b03386e7e5a9
ms.openlocfilehash: 81f02c1277ae8a2c377ca0d6db67ec4211e9aa5e
ms.contentlocale: cs-cz
ms.lasthandoff: 06/28/2017

---



<a id="how-does-vmware-replication-to-azure-work-in-site-recovery" class="xliff"></a>

# Jak funguje replikace VMware do Azure v Site Recovery?

Tento článek popisuje komponenty a procesy používané při replikaci místních virtuálních počítačů VMware a fyzických serverů se systémem Windows/Linux do Azure s využitím služby [Azure Site Recovery](site-recovery-overview.md).

Při replikaci fyzických místních serverů do Azure se také používají stejné komponenty a procesy jako při replikaci virtuálních počítačů VMware, ale s následujícími rozdíly:

- Jako konfigurační server můžete místo virtuálního počítače VMware použít fyzický server.
- Pro navrácení služeb po obnovení budete potřebovat místní infrastrukturu VMware. Služby po obnovení nelze navrátit na fyzický server.

Případné připomínky můžete publikovat na konci tohoto článku nebo na [fóru služby Azure Site Recovery](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


<a id="architectural-components" class="xliff"></a>

## Komponenty architektury

Při replikaci virtuálních počítačů VMware a fyzických serverů do Azure se využívá řada komponent.

**Komponenta** | **Umístění** | **Podrobnosti**
--- | --- | ---
**Azure** | Pro Azure potřebujete účet Azure, účet úložiště Azure a síť Azure. | Replikovaná data jsou uložena v účtu úložiště a virtuální počítače Azure se z replikovaných dat vytvoří, jakmile dojde k převzetí místních služeb při selhání. Virtuální počítače Azure se připojí k virtuální síti Azure po svém vytvoření.
**Konfigurační server** | Jeden místní server pro správu (virtuální počítač VMWare), na kterém jsou spuštěné všechny místní komponenty potřebné pro nasazení, včetně konfiguračního serveru, procesového serveru a hlavního cílového serveru. | Komponenta konfiguračního serveru koordinuje komunikaci mezi místním prostředím a Azure a spravuje replikaci dat.
 **Procesový server:**  | Obvykle se instaluje na konfigurační server. | Funguje jako replikační brána. Přijímá data replikace, optimalizuje je pomocí ukládání do mezipaměti, komprese a šifrování a odesílá je do úložiště Azure.<br/><br/> Procesový server se také stará o nabízenou instalaci služby Mobility na chráněné počítače a provádí automatické zjišťování virtuálních počítačů VMware.<br/><br/> Jak vaše nasazení poroste, můžete přidávat další samostatné vyhrazené procesní servery pro zpracování zvyšujícího se objemu replikačních přenosů.
 **Hlavní cílový server** | Obvykle se instaluje na místní konfigurační server. | Zpracovává replikační data během navracení služeb z Azure po obnovení.<br/><br/> Pokud je objem přenosů při navracení služeb po obnovení příliš vysoký, můžete nasadit samostatný hlavní cílový server pro účely navrácení služeb po obnovení.
**Servery VMware** | Virtuální počítače VMware jsou hostované na serverech vSphere ESXi a ke správě hostitelů doporučujeme použít server vCenter. | Servery VMware přidáte do trezoru služby Recovery Services.
**Replikované počítače** | Na každý virtuální počítač VMware, který chcete replikovat, se nainstaluje služba Mobility. Můžete ji nainstalovat na každý počítač ručně, nebo pomocí nabízené instalace z procesového serveru.

Další informace o požadavcích pro nasazení a pro jednotlivé komponenty najdete v [matici podpory](site-recovery-support-matrix-to-azure.md).

**Obr. 1: Komponenty pro replikaci z VMware do Azure**

![Komponenty](./media/site-recovery-components/arch-enhanced.png)

<a id="replication-process" class="xliff"></a>

## Proces replikace

1. Nastavíte nasazení, včetně komponent Azure, a trezor služby Recovery Services. V trezoru zadáte zdroj a cíl replikace, nastavíte konfigurační server, přidáte servery VMware, vytvoříte zásady replikace, nasadíte službu Mobility, povolíte replikaci a spustíte test převzetí služby.
2.  Počítače se začnou replikovat v souladu se zásadami replikace a počáteční kopie dat se replikuje do úložiště Azure.
4. Replikace rozdílových dat do Azure začne po dokončení počáteční replikace. Sledované změny se pro jednotlivé počítače ukládají do souboru .hrl.
    - Počítače, které se replikují, komunikují s konfiguračním serverem kvůli správě replikace na příchozím portu HTTPS 443.
    - Replikující se počítače odesílají data do procesového serveru na příchozím portu HTTPS 9443 (nastavitelné).
    - Konfigurační server orchestruje správu replikace s Azure přes odchozí port HTTPS 443.
    - Procesový server přijímá data ze zdrojového počítače, optimalizuje je a šifruje, a pak je odesílá do úložiště Azure přes odchozí port 443.
    - Když povolíte konzistenci mezi více virtuálními počítači, budou spolu počítače v replikační skupině komunikovat na portu 20004. Konzistence více virtuálních počítačů znamená, že seskupíte víc virtuálních počítačů do replikační skupiny, v rámci které se sdílí body obnovení konzistentní vzhledem k selháním a konzistentní vzhledem k aplikacím, když dojde k převzetí služeb při selhání. To je užitečné, pokud je počítačích spuštěná stejná úloha a je třeba, aby zůstala konzistentní.
5. Provoz se přes internet replikuje do veřejných koncových bodů úložiště Azure. Alternativně můžete použít [veřejný partnerský vztah](../expressroute/expressroute-circuit-peerings.md#public-peering) Azure ExpressRoute. Přenos replikačních dat přes síť site-to-site VPN z místního serveru do Azure není podporovaný.

**Obr. 2: Replikace z VMware do Azure**

![Rozšířené](./media/site-recovery-components/v2a-architecture-henry.png)

<a id="failover-and-failback-process" class="xliff"></a>

## Proces převzetí služeb při selhání a navrácení služeb po obnovení

1. Po ověření správného fungování testovacího převzetí služeb při selhání můžete podle potřeby spouštět neplánovaná převzetí služeb při selhání do Azure. Plánované převzetí služeb není podporované.
2. Můžete převzít služby při selhání jednoho počítače nebo vytvořit [plány obnovení](site-recovery-create-recovery-plans.md) pro převzetí služeb při selhání více virtuálních počítačů.
3. V průběhu převzetí služeb při selhání se v Azure vytvoří replika virtuálního počítače. Po potvrzení procesu převzetí služeb můžete začít používat úlohu na replikovaném virtuálním počítači Azure.
4. Až bude vaše místní lokalita opět dostupná, můžete službu navrátit. Nastavíte infrastrukturu navrácení služeb po obnovení, zahájíte replikaci počítače ze sekundární lokality na primární a spustíte neplánované převzetí služeb při selhání ze sekundární lokality. Jakmile toto navrácení služeb potvrdíte, budou data zpět v místní lokalitě a budete muset opět povolit replikaci do Azure. [Další informace](site-recovery-failback-azure-to-vmware.md)

Pro navrácení služby po obnovení existuje několik požadavků:


- **Dočasný procesní server v Azure**: Pokud chcete po převzetí služeb při selhání tyto služby po obnovení vrátit, budete muset pro zpracování replikace z Azure nastavit virtuální počítač Azure nakonfigurovaný jako procesní server. Tento virtuální počítač je možné po navrácení služeb po obnovení odstranit.
- **Připojení k síti VPN:** Pro navrácení služeb po obnovení budete potřebovat připojení k síti VPN (nebo Azure ExpressRoute) nastavené ze sítě Azure k místní lokalitě.
- **Samostatný lokální hlavní cílový server:** Místní hlavní cílový server se stará o navrácení služeb po obnovení. Hlavní cílový server je ve výchozím nastavení nainstalován na serveru pro správu, ale pokud po obnovení vracíte větší objemy přenosů, měli byste pro tento účel nastavit samostatný místní hlavní cílový server.
- **Zásady navrácení služeb po obnovení:** Pro zpětnou replikaci do vaší místní lokality budete potřebovat zásady navrácení služeb. Ty se vytvoří automaticky při vytvoření zásad replikace.
- **Infrastruktura VMware:** Musíte navrátit služby po obnovení do místního virtuálního počítače VMware. To znamená, že potřebujete místní infrastrukturu VMware, i když místní fyzické servery replikujete do Azure.

**Obr. 3: Navrácení služeb po obnovení – VMware nebo fyzický server**

![Navrácení služeb po obnovení](./media/site-recovery-components/enhanced-failback.png)


<a id="next-steps" class="xliff"></a>

## Další kroky

Kontrola [matice podpory](site-recovery-support-matrix-to-azure.md)

