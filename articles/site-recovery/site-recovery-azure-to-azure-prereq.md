---
title: "Předpoklady pro replikaci do Azure pomocí Azure Site Recovery | Microsoft Docs"
description: "Tento článek shrnuje požadavky na replikaci virtuálních počítačů a fyzických počítačů do Azure pomocí služby Azure Site Recovery."
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
ms.date: 08/01/2017
ms.author: rajanaki
ms.openlocfilehash: fb5b8c9ac96ac44d0112919664a177f33ef392da
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
#  <a name="prerequisites-for-replicating-azure-virtual-machines-to-another-region-by-using-azure-site-recovery"></a>Předpoklady pro replikovat virtuální počítače Azure pomocí Azure Site Recovery na jiné oblasti

> [!div class="op_single_selector"]
> * [Replikace z Azure do Azure](site-recovery-azure-to-azure-prereq.md)
> * [Replikace z místního do Azure](site-recovery-prereq.md)

Služba Azure Site Recovery přispívá ke strategii obchodní kontinuitu a po havárii obnovení (BCDR) tím, že orchestruje:
* Replikace virtuálních počítačích Azure jiné oblasti Azure.
* Replikace místní fyzických serverů a virtuálních počítačů do Azure nebo do sekundárního datacentra. 

Pokud dojde k výpadkům ve vašem primárním umístění, můžete selhání do sekundárního umístění, aby aplikace a úlohy zůstaly dostupné. Zpět do primárního umístění může selhat, až se obnoví normální provozní podmínky. Další informace o Site Recovery najdete v tématu [co je Site Recovery?](site-recovery-overview.md).

Tento článek obsahuje souhrn nezbytné součásti potřebné k zahájení z místního na Azure Site Recovery replikace.

Odeslat všechny komentáře v dolní části článku, nebo požádat technické dotazy na [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="azure-requirements"></a>Požadavky na Azure

**Požadavek** | **Podrobnosti**
--- | ---
**Účet Azure** | A [Microsoft Azure](http://azure.microsoft.com/) účtu.<br/><br/> Můžete začít s [bezplatnou zkušební verzí](https://azure.microsoft.com/pricing/free-trial/).
**Služba Site Recovery** | Další informace o cenách za Site Recovery najdete v tématu [cenách za Site Recovery](https://azure.microsoft.com/pricing/details/site-recovery/). Doporučujeme vytvořit trezor služeb zotavení v cílové oblasti Azure, který chcete použít jako umístění pro obnovení po havárii. Například pokud zdrojové virtuální počítače běží v oblasti Východ USA a chcete replikovat na střed USA, doporučujeme vytvořte trezor nejprve v střed USA.|
**Azure kapacity** | Pro cíl oblast Azure, který chcete použít jako umístění pro obnovení po havárii musíte mít předplatné s dostatečnou kapacitu pro virtuální počítače, účty úložiště a síťové součásti. Obraťte se na podporu, aby zvýšit kapacitu.
**Pokynů pro virtualizované úložiště** | Ujistěte se, že [pokynů pro virtualizované úložiště](../storage/common/storage-scalability-targets.md#scalability-targets-for-virtual-machine-disks) pro váš zdroj Azure virtuální počítače, aby se zabránilo problémům s výkonem. Pokud budete postupovat podle výchozího nastavení, Site Recovery vytvoří účty požadované úložiště na základě konfigurace zdroje. Pokud přizpůsobit a vyberte vlastní nastavení, ujistěte se, že [cíle škálovatelnosti pro disky virtuálního počítače](../storage/common/storage-scalability-targets.md#scalability-targets-for-virtual-machine-disks).
**Pokyny sítě** | Budete muset povolených odchozí připojení ze svého virtuálního počítače Azure pro konkrétní adresy URL nebo IP adresu rozsahy. Další podrobnosti najdete [sítě pokyny pro replikaci virtuálních počítačů Azure](site-recovery-azure-to-azure-networking-guidance.md) článku.
**Virtuální počítač Azure** | Zkontrolujte, zda všechny nejnovější kořenové certifikáty jsou k dispozici v systému Windows nebo virtuálního počítače s Linuxem. Pokud nejsou zadány nejnovější kořenové certifikáty, virtuální počítač nelze zaregistrovat k Site Recovery z důvodu omezení zabezpečení.

>[!NOTE]
>Další informace o podpoře pro konkrétní konfigurace, najdete [matici podpory](site-recovery-support-matrix-azure-to-azure.md).

## <a name="next-steps"></a>Další kroky
- Další informace o [sítě pokyny pro replikaci virtuálních počítačů Azure](site-recovery-azure-to-azure-networking-guidance.md).
- Začněte chránit vaše úlohy [replikovat virtuální počítače Azure](site-recovery-azure-to-azure.md).
