---
title: "Migrace do Azure pomocí Site Recovery | Dokumentace Microsoftu"
description: "Tento článek obsahuje přehled migrace virtuálních počítačů a fyzických serverů do Azure pomocí Azure Site Recovery"
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
ms.date: 01/04/2017
ms.author: raynew
translationtype: Human Translation
ms.sourcegitcommit: 20aecf6df4af0b89fb6cafbd2047facdb345bae5
ms.openlocfilehash: 8f658cb80a43e0eb21585ea851251b1339ab546b


---
# <a name="migrate-to-azure-with-site-recovery"></a>Migrace do Azure pomocí Site Recovery?

V tomto článku najdete přehled používání služby Azure Site Recovery k migraci virtuálních počítačů a fyzických serverů.

Organizace potřebují strategii BCDR, která určuje, jak aplikace, úlohy a data zůstanou spuštěné a dostupné během plánovaných a neplánovaných výpadků a jak co nejdříve obnovit normální provozní podmínky. Strategie BCDR by měla zajistit bezpečnost a obnovitelnost firemních dat a zajistit, aby v případě, že dojde k havárii, byly zpracovávané úlohy stále k dispozici.

Site Recovery je služba Azure, která přispívá ke strategii BCDR orchestrací replikace místních fyzických serverů a virtuálních počítačů do cloudu (Azure) nebo do sekundárního datového centra. Pokud dojde k výpadkům ve vašem primárním umístění, předáte služby při selhání do sekundárního umístění, aby aplikace a úlohy zůstaly dostupné. Až se obnoví normální provozní podmínky, vrátíte služby po obnovení zpět do primárního umístění. Potřebujete další informace [O službě Site Recovery?](site-recovery-overview.md)

Tento článek popisuje postup nasazení na webu [Azure Portal](https://portal.azure.com). [Portál Azure Classic](https://manage.windowsazure.com/) můžete použít k udržování existujících trezorů Site Recovery, ale ne k vytváření nových.

Případné připomínky můžete připojit v dolní části stránky. Technické dotazy můžete zadávat ve [fóru Služeb zotavení Azure](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="what-do-we-mean-by-migration"></a>Co myslíme pojmem migrace?

Můžete nasadit Site Recovery a zajistit úplnou replikaci místních virtuálních počítačů a fyzických serverů do Azure nebo sekundární lokality. Můžete replikovat počítače, převzít z primární lokality služby při selhání v případě výpadku a po obnovení primární lokality je navrátit. Kromě úplné replikace můžete pomocí Site Recovery migrovat virtuální počítače a fyzické servery do Azure, aby uživatelé měli přístup k úlohám počítače z virtuálních počítačů Azure. Migrace zahrnuje replikaci a převzetí služeb při selhání z primární lokality do Azure. Na rozdíl od úplné replikace ale nezahrnuje mechanismus navrácení služeb po obnovení.

## <a name="what-can-site-recovery-migrate"></a>Co může Site Recovery migrovat?

Můžete:

- Migrovat úlohy běžící na místních virtuálních počítačích Hyper-V, virtuální počítače VMware a fyzické servery na virtuální počítače Azure. V tomto scénáři můžete také provádět úplnou replikaci a navrácení služeb po obnovení.
- Migrovat [virtuální počítače Azure IaaS](site-recovery-migrate-azure-to-azure.md) mezi oblastmi Azure. V tomto scénáři je aktuálně podporovaná pouze migrace, což znamená, že nemůžete navrátit služby po obnovení.
- Migrovat [instance Windows AWS](site-recovery-migrate-aws-to-azure.md) do virtuálních počítačů Azure IaaS. V tomto scénáři je aktuálně podporovaná pouze migrace, což znamená, že nemůžete navrátit služby po obnovení.

## <a name="migrate-on-premises-vms-and-physical-servers"></a>Migrace místních virtuálních počítačů a fyzických serverů

Při migraci místních virtuálních počítačů Hyper-V, virtuálních počítačů VMware a fyzických serverů postupujete téměř stejně jako při běžné replikaci. Nastavíte trezor služby Recovery Services, nakonfigurujete požadované servery pro správu (podle toho, co chcete migrovat), přidáte je do trezoru a zadáte nastavení replikace. Povolíte replikaci pro počítače, které chcete migrovat, a spustíte rychlé testovací převzetí služeb při selhání, abyste ověřili, že všechno funguje, jak by mělo.

Po ověření, že prostředí replikace funguje, použijete plánované nebo neplánované převzetí služeb při selhání v závislosti na tom, [kterou možnost váš scénář podporuje](site-recovery-failover.md#failover-and-failback). U migrace není nutné provádět převzetí služeb při selhání ani cokoli odstraňovat. Místo toho vyberete možnost **Dokončit migraci** pro každý počítač, který chcete migrovat. Akce **Dokončit migraci** dokončí proces migrace, odebere z počítače replikaci a zastaví fakturaci za Site Recovery pro daný počítač.

## <a name="migrate-between-azure-regions"></a>Migrace mezi oblastmi Azure

Pomocí Site Recovery můžete migrovat virtuální počítače Azure mezi oblastmi. V tomto scénáři je podporována pouze migrace. Jinými slovy, můžete replikovat virtuální počítače Azure a předat jejich služby při selhání do jiné oblasti, ale nemůžete navracet služby po obnovení. V tomto scénáři nastavíte trezor služby Recovery Services, nasadíte místní konfigurační server pro správu replikace, přidáte jej do trezoru a zadáte nastavení replikace. Povolíte replikaci pro počítače, které chcete migrovat, a spustíte rychlé testovací převzetí služeb při selhání. Potom spustíte neplánované převzetí služeb při selhání pomocí možnosti **Dokončit migraci**.

## <a name="migrate-aws-to-azure"></a>Migrace AWS do Azure

Můžete migrovat instance AWS do virtuálních počítačů Azure. V tomto scénáři je podporována pouze migrace. Jinými slovy, můžete replikovat instance AWS a předat jejich služby při selhání do Azure, ale nemůžete navracet služby po obnovení. Pro účely migrace se s instancemi AWS zachází stejně jako s fyzickými servery. Nastavíte trezor služby Recovery Services, nasadíte místní konfigurační server pro správu replikace, přidáte jej do trezoru a zadáte nastavení replikace. Povolíte replikaci pro počítače, které chcete migrovat, a spustíte rychlé testovací převzetí služeb při selhání. Potom spustíte neplánované převzetí služeb při selhání pomocí možnosti **Dokončit migraci**.




## <a name="next-steps"></a>Další kroky

- [Migrace virtuálních počítačů VMware do Azure](site-recovery-vmware-to-azure.md)
- [Migrace fyzických serverů do Azure](site-recovery-vmware-to-azure.md)
- [Migrace virtuálních počítačů Hyper-V v cloudech VMM do Azure](site-recovery-vmm-to-azure.md)
- [Migrace virtuálních počítačů Hyper-V bez VMM do Azure](site-recovery-hyper-v-site-to-azure.md)
- [Migrace virtuálních počítačů Azure mezi oblastmi Azure](site-recovery-migrate-azure-to-azure.md)
- [Migrace instancí AWS do Azure](site-recovery-migrate-aws-to-azure.md)



<!--HONumber=Jan17_HO1-->


