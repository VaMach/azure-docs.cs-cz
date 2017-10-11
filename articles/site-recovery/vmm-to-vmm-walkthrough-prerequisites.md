---
title: "Přečtěte si požadavky pro replikaci technologie Hyper-V do sekundární lokalita VMM s Azure Site Recovery | Microsoft Docs"
description: "Popisuje požadavky pro replikaci virtuálních počítačů Hyper-V do sekundární lokalita VMM s Azure Site Recovery."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 21ff0545-8be5-4495-9804-78ab6e24add6
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/30/2017
ms.author: raynew
ms.openlocfilehash: 7897a30bf1774609ca8e6037dabcd5fbf4151271
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="step-2-review-the-prerequisites-and-limitations-for-hyper-v-vm-replication-to-a-secondary-vmm-site"></a>Krok 2: Kontrola předpoklady a omezení pro replikaci virtuálního počítače technologie Hyper-V do sekundární lokality VMM


Po zkontrolování [architekturu scénáře](vmm-to-vmm-walkthrough-architecture.md), přečtěte si tento článek k musíte rozumět požadavky nasazení, při replikaci místní technologie Hyper-V virtuálních počítačů (VM) spravované v cloudech System Center Virtual Machine Manager (VMM), sekundární lokality pomocí [Azure Site Recovery](site-recovery-overview.md) na portálu Azure.

Po přečtení tohoto článku můžete publikovat jakékoli dotazy nebo připomínky na jeho konci nebo na [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="prerequisites-and-limitations"></a>Požadavky a omezení

**Požadavek** | **Podrobnosti**
--- | ---
**Azure** | A [Microsoft Azure](http://azure.microsoft.com/) předplatné.<br/><br/> Můžete začít s [bezplatnou zkušební verzí](https://azure.microsoft.com/pricing/free-trial/).<br/><br/> [Další informace](https://azure.microsoft.com/pricing/details/site-recovery/) o cenách za Site Recovery<br/><br/> Zkontrolujte podporované oblasti pro obnovení lokality v rámci geografickou dostupností v [Azure Site Recovery podrobnosti o cenách](https://azure.microsoft.com/pricing/details/site-recovery/).
**Servery VMM** | Doporučujeme, že abyste měli dva servery VMM, jeden v primární lokalitě a jeden v sekundární.<br/><br/> Replikace mezi cloudy na jednom serveru VMM je podporována.<br/><br/> Servery VMM by měl používat minimálně System Center 2012 SP1 s nejnovějšími aktualizacemi.<br/><br/> Servery VMM potřebovat přístup k Internetu.
**Cloudy VMM** | Každý server VMM musí mít na jeden nebo více cloudů a všechny cloudy musí mít sadu profilů kapacity technologie Hyper-V. <br/><br/>Cloudy musí obsahovat jeden nebo více skupin hostitelů VMM.<br/><br/> Pokud máte pouze jeden server VMM, musí být aspoň dva cloudy, tak, aby fungoval jako primární i sekundární.
**Hyper-V** | Servery Hyper-V musí běžet minimálně Windows Server 2012 s rolí Hyper-V, a mít nainstalované nejnovější aktualizace.<br/><br/> Server Hyper-V by měl obsahovat jeden nebo více virtuálních počítačů.<br/><br/>  Hostitelské servery Hyper-V by měl být umístěn v skupiny hostitelů v primárních a sekundárních cloudech VMM.<br/><br/> Pokud spustíte technologie Hyper-V v clusteru na Windows Server 2012 R2, nainstalujte [aktualizovat 2961977](https://support.microsoft.com/kb/2961977)<br/><br/> Pokud spustíte technologie Hyper-V v clusteru na Windows Server 2012, zprostředkovatele clusteru se nevytvoří automaticky, pokud máte statické IP adresy na základě clusteru. Zprostředkovatel clusteru nakonfigurujte ručně. [Další informace](http://social.technet.microsoft.com/wiki/contents/articles/18792.configure-replica-broker-role-cluster-to-cluster-replication.aspx).<br/><br/> Servery Hyper-V potřebují přístup k Internetu.




## <a name="next-steps"></a>Další kroky

Přejděte na [krok 3: plánování sítě](vmm-to-vmm-walkthrough-network.md).
