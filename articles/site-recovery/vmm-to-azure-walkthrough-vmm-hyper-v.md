---
title: "Příprava System Center VMM pro replikaci technologie Hyper-V do Azure | Microsoft Docs"
description: "Popisuje, jak připravit server System Center VMM pro replikaci technologie Hyper-V do Azure pomocí Azure Site Recovery"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: afcd81ae-d192-4013-a0af-3dac45b3c7e9
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 07/23/2017
ms.author: raynew
ms.openlocfilehash: ec118ed837dbf140083b3ae1e4ecd41c81562018
ms.sourcegitcommit: 422efcbac5b6b68295064bd545132fcc98349d01
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/29/2017
---
# <a name="step-6-prepare-vmm-servers-and-hyper-v-hosts-for-hyper-v-replication-to-azure"></a>Krok 6: Připravte servery VMM a hostitelé Hyper-V pro replikaci technologie Hyper-V do Azure.

Po nastavení [Azure součásti](vmm-to-azure-walkthrough-prepare-azure.md) pro nasazení, postupujte podle pokynů v tomto článku připravit místní servery VMM a hostitelé Hyper-V pro interakci s Azure Site Recovery.

Po přečtení tohoto článku, post jakékoli komentáře v dolní části, nebo požádat technické dotazy na [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="prepare-vmm-servers"></a>Připravit servery VMM

- Musíte mít alespoň jeden server VMM, který splňuje požadavky na podporu pro Site Recovery replikaci (site-recovery-support-matrix-to-azure.md#support-for-datacenter-management-servers).
- Zajistěte, aby Připravili jste server VMM pro [mapování sítě](vmm-to-azure-walkthrough-network.md#network-mapping-for-replication-to-azure).
- Ujistěte se, že můžete přístup k serveru VMM pro tyto adresy URL

    [!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)]
    
- Pokud máte zavedená pravidla brány firewall založená na IP adrese, zkontrolujte, že tato pravidla umožňují komunikaci s Azure.
- Povolte [Rozsahy IP adres datového centra Azure](https://www.microsoft.com/download/confirmation.aspx?id=41653) a port HTTPS (443).
- Povolte rozsahy IP adres pro oblast Azure svého předplatného a pro oblast Západní USA (používá se pro řízení přístupu a správu identit).

Během nasazování Site Recovery stáhněte zprostředkovatele služby Site Recovery a nainstalovat na každý server VMM. VMM server je zaregistrován v trezoru služeb zotavení.




## <a name="next-steps"></a>Další kroky

Přejděte na [krok 7: vytvoření trezoru](vmm-to-azure-walkthrough-create-vault.md)

