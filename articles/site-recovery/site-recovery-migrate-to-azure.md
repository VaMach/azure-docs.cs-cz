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
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 04/05/2017
ms.author: raynew
ms.translationtype: Human Translation
ms.sourcegitcommit: ef1e603ea7759af76db595d95171cdbe1c995598
ms.openlocfilehash: f4dfe430fba51bd009431ca72279a21be55e3a40
ms.contentlocale: cs-cz
ms.lasthandoff: 06/16/2017


---
# Migrace do Azure pomocí Site Recovery
<a id="migrate-to-azure-with-site-recovery" class="xliff"></a>

V tomto článku najdete přehled používání služby Azure Site Recovery k migraci virtuálních počítačů a fyzických serverů.

Site Recovery je služba Azure, která přispívá ke strategii BCDR orchestrací replikace místních fyzických serverů a virtuálních počítačů do cloudu (Azure) nebo do sekundárního datového centra. Pokud dojde k výpadkům ve vašem primárním umístění, předáte služby při selhání do sekundárního umístění, aby aplikace a úlohy zůstaly dostupné. Až se obnoví normální provozní podmínky, vrátíte služby po obnovení zpět do primárního umístění. Potřebujete další informace [O službě Site Recovery?](site-recovery-overview.md) Službu Site Recovery můžete také použít k migraci existujících místních úloh do Azure pro uspíšení přechodu na cloud a využití nabídky funkcí, které Azure nabízí.

Abyste získali rychlý přehled o tom, jak provést migraci, prohlédněte si toto video.
>[!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/ASRHowTo-Video2-Migrate-Virtual-Machines-to-Azure/player]

Tento článek popisuje postup nasazení na webu [Azure Portal](https://portal.azure.com). [Portál Azure Classic](https://manage.windowsazure.com/) můžete použít k udržování existujících trezorů Site Recovery, ale ne k vytváření nových.

Případné připomínky můžete připojit v dolní části stránky. Technické dotazy můžete zadávat ve [fóru Služeb zotavení Azure](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## Co myslíme pojmem migrace?
<a id="what-do-we-mean-by-migration" class="xliff"></a>

Můžete nasadit službu Site Recovery pro replikaci místních virtuálních počítačů a fyzických serverů do Azure nebo sekundární lokality. Můžete replikovat počítače, provádět převzetí služeb při selhání z primární lokality při výpadcích a vracet je zpět, když je výpadek vyřešen. Kromě toho můžete pomocí Site Recovery migrovat virtuální počítače a fyzické servery do Azure, aby k nim uživatelé měli přístup jako k virtuálním počítačům Azure. Migrace zahrnuje replikaci a převzetí služeb při selhání z primární lokality do Azure a pokyn dokončení migrace.

## Co může Site Recovery migrovat?
<a id="what-can-site-recovery-migrate" class="xliff"></a>

Můžete:

- Migrovat úlohy běžící na místních virtuálních počítačích Hyper-V, virtuální počítače VMware a fyzické servery na virtuální počítače Azure. V tomto scénáři můžete také provádět úplnou replikaci a navrácení služeb po obnovení.
- Migrovat [virtuální počítače Azure IaaS](site-recovery-migrate-azure-to-azure.md) mezi oblastmi Azure. V tomto scénáři je aktuálně podporovaná pouze migrace, což znamená, že nemůžete navrátit služby po obnovení.
- Migrovat [instance Windows AWS](site-recovery-migrate-aws-to-azure.md) do virtuálních počítačů Azure IaaS. V tomto scénáři je aktuálně podporovaná pouze migrace, což znamená, že nemůžete navrátit služby po obnovení.

## Migrace místních virtuálních počítačů a fyzických serverů
<a id="migrate-on-premises-vms-and-physical-servers" class="xliff"></a>

Při migraci místních virtuálních počítačů Hyper-V, VMware a fyzických serverů postupujete téměř stejně jako při běžné replikaci.

1. Nastavte trezor služby Recovery Services.
2. Nakonfigurujte požadované servery pro správu (VMware, VMM, Hyper-V – podle toho, co chcete migrovat), přidejte je do trezoru a zadejte nastavení replikace.
3. Povolte replikaci pro počítače, které chcete migrovat.
4. Po počáteční migraci spusťte rychlé testovací převzetí služeb při selhání, abyste zjistili, že všechno funguje, jak má.
5. Po ověření, že prostředí replikace funguje, použijete plánované nebo neplánované převzetí služeb při selhání v závislosti na tom, [kterou možnost váš scénář podporuje](site-recovery-failover.md). Doporučujeme, že abyste použili plánované převzetí služeb při selhání, pokud je to možné.
6. U migrace není nutné provádět převzetí služeb při selhání ani ho odstraňovat. Místo toho vyberete možnost **Dokončit migraci** pro každý počítač, který chcete migrovat.
     - V části **Replikované položky** klikněte pravým tlačítkem na virtuální počítač a klikněte na **Dokončit migraci**. Akci dokončete kliknutím na **OK**. Můžete sledovat průběh ve vlastnostech virtuálního počítače monitorováním úlohy dokončení migrace v **úlohách Site Recovery**.
     - Akce **Dokončit migraci** dokončí proces migrace, odebere z počítače replikaci a zastaví fakturaci za Site Recovery pro daný počítač.

![completemigration](./media/site-recovery-hyper-v-site-to-azure/migrate.png)

## Migrace mezi oblastmi Azure
<a id="migrate-between-azure-regions" class="xliff"></a>

Pomocí Site Recovery můžete migrovat virtuální počítače Azure mezi oblastmi. V tomto scénáři je podporována pouze migrace. Jinými slovy můžete replikovat virtuální počítače Azure a předat jejich služby při selhání do jiné oblasti, ale nemůžete navracet služby po obnovení. V tomto scénáři nastavíte trezor služby Recovery Services, nasadíte místní konfigurační server pro správu replikace, přidáte jej do trezoru a zadáte nastavení replikace. Povolíte replikaci pro počítače, které chcete migrovat, a spustíte rychlé testovací převzetí služeb při selhání. Potom spustíte neplánované převzetí služeb při selhání pomocí možnosti **Dokončit migraci**.

## Migrace AWS do Azure
<a id="migrate-aws-to-azure" class="xliff"></a>

Můžete migrovat instance AWS do virtuálních počítačů Azure. V tomto scénáři je podporována pouze migrace. Jinými slovy, můžete replikovat instance AWS a předat jejich služby při selhání do Azure, ale nemůžete navracet služby po obnovení. Pro účely migrace se s instancemi AWS zachází stejně jako s fyzickými servery. Nastavíte trezor služby Recovery Services, nasadíte místní konfigurační server pro správu replikace, přidáte jej do trezoru a zadáte nastavení replikace. Povolíte replikaci pro počítače, které chcete migrovat, a spustíte rychlé testovací převzetí služeb při selhání. Potom spustíte neplánované převzetí služeb při selhání pomocí možnosti **Dokončit migraci**.




## Další kroky
<a id="next-steps" class="xliff"></a>

- [Migrace virtuálních počítačů VMware do Azure](site-recovery-vmware-to-azure.md)
- [Migrace virtuálních počítačů Hyper-V v cloudech VMM do Azure](site-recovery-vmm-to-azure.md)
- [Migrace virtuálních počítačů Hyper-V bez VMM do Azure](site-recovery-hyper-v-site-to-azure.md)
- [Migrace virtuálních počítačů Azure mezi oblastmi Azure](site-recovery-migrate-azure-to-azure.md)
- [Migrace instancí AWS do Azure](site-recovery-migrate-aws-to-azure.md)
- [Připravte migrované počítače na povolení replikace](site-recovery-azure-to-azure-after-migration.md) do jiné oblasti pro případ, že by bylo nutné je zotavit po havárii.
- Začněte chránit svoje úlohy pomocí [replikace virtuálních počítačů Azure](site-recovery-azure-to-azure.md).

