---
title: "Kontrola architektury pro replikaci Hyper-V do sekundární lokality s využitím služby Azure Site Recovery | Dokumentace Microsoftu"
description: "Tento článek obsahuje přehled architektury pro replikaci místních virtuálních počítačů Hyper-V do sekundární lokality System Center VMM s využitím služby Azure Site Recovery."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 07099161-4cc7-4f32-8eb9-2a71bbf0750b
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/30/2017
ms.author: raynew
ms.translationtype: HT
ms.sourcegitcommit: fff84ee45818e4699df380e1536f71b2a4003c71
ms.openlocfilehash: b78cd0d5a5395873afaddc8856004775f447e8ea
ms.contentlocale: cs-cz
ms.lasthandoff: 08/01/2017

---
# <a name="step-1-review-the-architecture-for-hyper-v-replication-to-a-secondary-site"></a>Krok 1: Kontrola architektury pro replikaci Hyper-V do sekundární lokality

Tento článek popisuje komponenty a procesy využívané při replikaci místních virtuálních počítačů Hyper-V v cloudech System Center Virtual Machine Manager (VMM) do sekundární lokality VMM s využitím služby [Azure Site Recovery](site-recovery-overview.md) na webu Azure Portal.

Jakékoli dotazy můžete publikovat na konci tohoto článku nebo na [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).



## <a name="architectural-components"></a>Komponenty architektury

Dál je uvedeno, co potřebujete pro replikaci virtuálních počítačů Hyper-V do sekundární lokality VMM.

**Komponenta** | **Umístění** | **Podrobnosti**
--- | --- | ---
**Azure** | Předplatné Azure. | V předplatném Azure vytvoříte trezor služby Recovery Services sloužící k orchestraci a správě replikace mezi umístěními VMM.
**Server VMM** | Potřebujete primární a sekundární umístění VMM. | Doporučujeme jeden server VMM v primární lokalitě a další v sekundární lokalitě. 
**Server Hyper-V** |  Jeden nebo několik hostitelských serverů Hyper-V v primárním a sekundárním cloudu VMM. | Data se replikují mezi primárním a sekundárním hostitelským serverem Hyper-V přes síť LAN nebo VPN na základě protokolu Kerberos nebo ověření certifikátem.  
**Virtuální počítače Hyper-V** | Na hostitelském serveru Hyper-V. | Zdrojový hostitelský server musí mít alespoň jeden virtuální počítač, který chcete replikovat.

## <a name="replication-process"></a>Proces replikace

1. Nastavíte účet Azure, vytvoříte trezor služby Recovery Services a určíte, co chcete replikovat.
2. Nakonfigurujete nastavení replikace zdroje a cíle, což zahrnuje instalaci zprostředkovatele služby Azure Site Recovery na serverech VMM a agenta Microsoft Azure Recovery Services na každém hostiteli Hyper-V.
3. Vytvoříte zásady replikace pro zdrojový cloud VMM. Zásady se použijí pro všechny virtuální počítače na hostitelích v cloudu.
4. Povolíte replikaci pro každou službu VMM a dojde k počáteční replikaci virtuálního počítače podle zvoleného nastavení.
5. Po počáteční replikaci se zahájí replikace rozdílových změn. Sledované změny se pro jednotlivé položky ukládají do souboru .hrl.


![Z lokálního prostředí do lokálního prostředí](./media/vmm-to-vmm-walkthrough-architecture/arch-onprem-onprem.png)

## <a name="failover-and-failback-process"></a>Proces převzetí služeb při selhání a navrácení služeb po obnovení

1. Můžete spustit plánované nebo neplánované [převzetí služeb při selhání](site-recovery-failover.md) mezi místními lokalitami. Pokud spustíte plánovanou operaci, dojde k ukončení zdrojových virtuálních počítačů, aby se zcela předešlo možné ztrátě dat.
2. Můžete převzít službu při selhání jednoho počítače nebo vytvořit [plány zotavení](site-recovery-create-recovery-plans.md) a orchestrovat převzetí služeb více počítačů.
4. Pokud proběhlo neplánované převzetí služeb při selhání sekundární lokalitou, po provedení převzetí služeb nebudou počítače v sekundární lokalitě chráněné pomocí replikace. Pokud jste spustili plánované převzetí služeb při selhání, počítače v sekundárním umístění chráněné budou.
5. Po potvrzení převzetí služeb můžete začít používat úlohu na replikovaném virtuálním počítači.
6. Až bude primární lokalita opět dostupná, zahájíte zpětnou replikaci ze sekundární lokality do primární. Po zpětné replikaci budou virtuální počítače v chráněném stavu, ale sekundární datové centrum bude stále aktivním umístěním.
7. Chcete-li z primární lokality opět udělat aktivní, zahajte plánované převzetí služeb ze sekundární lokality do primární, následované další zpětnou replikací.



## <a name="next-steps"></a>Další kroky

Přejděte ke [kroku 2: Kontrola požadavků a omezení](vmm-to-vmm-walkthrough-prerequisites.md).

