---
title: "Kontrola architektury pro replikaci Hyper-V do sekundární lokality s využitím služby Azure Site Recovery | Dokumentace Microsoftu"
description: "Tento článek obsahuje přehled architektury pro replikaci místních virtuálních počítačů Hyper-V do sekundární lokality System Center VMM s využitím služby Azure Site Recovery."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 26475782-a21a-408a-b089-35382d7e010e
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/10/2017
ms.author: raynew
ms.openlocfilehash: a7a493097a4eaacc2c8d8449906b4a57eb411827
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="hyper-v-replication-to-a-secondary-site"></a>Replikaci technologie Hyper-V do sekundární lokality

Tento článek popisuje komponenty a procesy využívané při replikaci místních virtuálních počítačů Hyper-V v cloudech System Center Virtual Machine Manager (VMM) do sekundární lokality VMM s využitím služby [Azure Site Recovery](site-recovery-overview.md) na webu Azure Portal.


## <a name="architectural-components"></a>Komponenty architektury

Následující obrázek a tabulka poskytují souhrnné zobrazení komponenty používané pro replikaci technologie Hyper-V do sekundární lokality.

**Komponenta** | **Požadavek** | **Podrobnosti**
--- | --- | ---
**Azure** | Předplatné Azure | V předplatném Azure vytvoříte trezor služby Recovery Services sloužící k orchestraci a správě replikace mezi umístěními VMM.
**Server VMM** | Potřebujete primární a sekundární umístění VMM. | Doporučujeme jeden server VMM v primární lokalitě a další v sekundární lokalitě.
**Server Hyper-V** |  Jeden nebo několik hostitelských serverů Hyper-V v primárním a sekundárním cloudu VMM. | Data se replikují mezi primárním a sekundárním hostitelským serverem Hyper-V přes síť LAN nebo VPN na základě protokolu Kerberos nebo ověření certifikátem.  
**Virtuální počítače Hyper-V** | Na hostitelském serveru Hyper-V. | Zdrojový hostitelský server musí mít alespoň jeden virtuální počítač, který chcete replikovat.

**Místní místní architektury**

![Z lokálního prostředí do lokálního prostředí](./media/concepts-hyper-v-to-secondary-architecture/arch-onprem-onprem.png)

## <a name="replication-process"></a>Proces replikace

1. Když se aktivuje počáteční replikace [snímku virtuálního počítače technologie Hyper-V](https://technet.microsoft.com/library/dd560637.aspx) pořízení snímku.
2. Virtuální pevné disky na virtuální počítač se replikují jeden po druhém, do sekundárního umístění.
3. Pokud probíhá počáteční replikace, dojde ke změnám disku 
4. Po dokončení počáteční replikace začne rozdílová replikace. Technologie Hyper-V Replica Replication Tracker sleduje změny jako protokoly replikace technologie Hyper-V (.hrl). Tyto protokolové soubory jsou umístěny ve stejné složce jako disky. Každý disk má přidružený soubor .hrl odeslaný do sekundárního umístění. Soubory snímků a protokolů spotřebovávají prostředky disku v době, kdy probíhá počáteční replikace.
5. Rozdílové změny na disku v protokolu se synchronizují a sloučí s nadřazeným diskem.
6. 

## <a name="failover-and-failback-process"></a>Proces převzetí služeb při selhání a navrácení služeb po obnovení

1. Můžete převzít jeden počítač nebo vytvořit plány obnovení, orchestraci převzetí služeb při selhání více počítačů.
2. Můžete spustit plánovaném nebo neplánovaném převzetí služeb při selhání mezi místními lokalitami. Pokud spustíte plánovanou operaci, dojde k ukončení zdrojových virtuálních počítačů, aby se zcela předešlo možné ztrátě dat.
    - Pokud neplánované převzetí služeb při selhání pro sekundární lokalitu, můžete provést po převzetí služeb při selhání počítače v sekundárním umístění nejsou chráněné.
    - Pokud jste spustili plánované převzetí služeb při selhání, počítače v sekundárním umístění chráněné budou.
3. Po spuštění počáteční převzetí služeb při selhání předat ji spustit, přístup k zatížení z virtuálního počítače repliky.

Když primární umístění opět k dispozici, může selhat zpět.

1. Zahájení zpětná replikace, do zahájení na primární replikace ze sekundární lokality. Po zpětné replikaci budou virtuální počítače v chráněném stavu, ale sekundární datové centrum bude stále aktivním umístěním.
2. Chcete-li z primární lokality opět udělat aktivní, zahajte plánované převzetí služeb ze sekundární lokality do primární, následované další zpětnou replikací.



## <a name="next-steps"></a>Další kroky

Podívat se na matici podpory postupujte podle kurzu povolit replikaci technologie Hyper-V mezi cloudy VMM.
Spusťte převzetí služeb při selhání a navrácení služeb po obnovení.
