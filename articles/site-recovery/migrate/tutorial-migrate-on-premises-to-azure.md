---
title: "Migrovat místní počítače do Azure s Azure Site Recovery | Microsoft Docs"
description: "Tento článek popisuje, jak migrovat místní počítače do Azure pomocí Azure Site Recovery."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: jwhit
editor: 
ms.assetid: ddb412fd-32a8-4afa-9e39-738b11b91118
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 11/27/2017
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: e268a6d53a9f0b001ad0da8c9ce7ea1a9046960c
ms.sourcegitcommit: 29bac59f1d62f38740b60274cb4912816ee775ea
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/29/2017
---
# <a name="migrate-on-premises-machines-to-azure"></a>Migrace místních počítačů do Azure

[Azure Site Recovery](../site-recovery-overview.md) služby spravuje a orchestruje replikaci, převzetí služeb při selhání a navrácení služeb po obnovení místního počítače a virtuální počítače Azure (VM).

V tomto kurzu se dozvíte, jak migrovat místní virtuální počítače a fyzické servery do Azure pomocí Site Recovery. V tomto kurzu se naučíte:

> [!div class="checklist"]
> * Nastavte požadavky pro nasazení
> * Vytvoření trezoru služeb zotavení pro Site Recovery
> * Nasazení serverů pro správu na místě
> * Nastavit zásadu replikace a povolení replikace
> * Spuštění postupu zotavení po havárii a ujistěte se, všechno funguje
> * Spuštění jednorázové převzetí služeb při selhání do Azure

## <a name="overview"></a>Přehled

Počítač se migrovat povolením pro něho replikaci a selhání do Azure.


## <a name="prerequisites"></a>Požadavky

Zde je, co musíte udělat pro účely tohoto kurzu.

- [Příprava](../tutorial-prepare-azure.md) prostředky Azure, včetně předplatné Azure, virtuální sítě Azure a účet úložiště.
- [Příprava](../tutorial-prepare-on-premises-vmware.md) VMware místní serverů VMware a virtuálních počítačů.
- Všimněte si, že nejsou podporovány exportované sadou paravirtualized ovladače zařízení.


## <a name="create-a-recovery-services-vault"></a>Vytvoření trezoru Služeb zotavení

[!INCLUDE [site-recovery-create-vault](../../../includes/site-recovery-create-vault.md)]

## <a name="select-a-protection-goal"></a>Vyberte cíl ochrany

Vyberte, jak chcete počítače replikovat a kam je chcete replikovat.
1. Klikněte na tlačítko **trezory služeb zotavení** > trezoru.
2. V nabídce prostředků, klikněte na tlačítko **Site Recovery** > **Příprava infrastruktury** > **cíl ochrany**.
3. V **cíl ochrany**, vyberte možnost:
    - **VMware**: vyberte **do Azure** > **Ano, s hypervisoru VMWare vSphere**.
    - **Fyzický počítač**: vyberte **do Azure** > **není virtualizované/jiné**.
    - **Technologie Hyper-V**: vyberte **do Azure** > **Ano, s Hyper-V**.


## <a name="set-up-the-source-environment"></a>Nastavení zdrojového prostředí

- [Nastavit](../tutorial-vmware-to-azure.md#set-up-the-source-environment) zdrojové prostředí pro virtuální počítače VMware.
- [Nastavit](../tutorial-physical-to-azure.md#set-up-the-source-environment) zdrojové prostředí pro fyzických serverů.
- [Nastavit](../tutorial-hyper-v-to-azure.md#set-up-the-source-environment) zdrojové prostředí pro virtuální počítače Hyper-V.

## <a name="set-up-the-target-environment"></a>Nastavení cílového prostředí

Vyberte a ověřte cílové prostředky.

1. Klikněte na **Připravit infrastrukturu** > **Cíl** a vyberte předplatné Azure, které chcete použít.
2. Zadejte model nasazení cíl.
3. Site Recovery zkontroluje, že máte minimálně jednu kompatibilní síť a účet úložiště Azure.

## <a name="create-a-replication-policy"></a>Vytvoření zásady replikace

- [Nastavte zásady replikace](../tutorial-vmware-to-azure.md#create-a-replication-policy) pro virtuální počítače VMware.


## <a name="enable-replication"></a>Povolení replikace

- [Povolit replikaci](../tutorial-vmware-to-azure.md#enable-replication) pro virtuální počítače VMware.


## <a name="run-a-test-migration"></a>Spuštění testu migrace

Spuštění [testovací převzetí služeb při selhání](../tutorial-dr-drill-azure.md) do Azure, abyste měli jistotu, všechno funguje podle očekávání.


## <a name="migrate-to-azure"></a>Migrace do Azure

Spusťte převzetí služeb při selhání pro počítače, které chcete migrovat.

1. V **nastavení** > **replikované položky** klikněte na počítač > **převzetí služeb při selhání**.
2. V **převzetí služeb při selhání** vyberte **bod obnovení** k převzetí služeb při selhání. Vyberte nejnovější bod obnovení.
3. Nastavení klíče šifrování není relevantní pro tento scénář.
4. Vyberte **vypnout počítač před zahájením převzetí služeb při selhání** Pokud chcete, aby Site Recovery se pokusit o proveďte vypnutí zdrojových virtuálních počítačích než převzetí služeb při selhání. Převzetí služeb při selhání pokračovat i v případě, že vypnutí selže. Můžete sledovat průběh převzetí služeb při selhání **úlohy** stránky.
5. Zkontrolujte, že virtuální počítač Azure se zobrazí v Azure podle očekávání.
6. V **replikované položky**, klikněte pravým tlačítkem na virtuální počítač > **dokončení migrace**. To dokončení procesu migrace, zastaví replikace pro virtuální počítač a zastaví fakturace Site Recovery pro virtuální počítač.

    ![Dokončení migrace](./media/tutorial-migrate-on-premises-to-azure/complete-migration.png)


> [!WARNING]
> **Nemáte zrušení převzetí služeb při selhání v průběhu**: replikace virtuálních počítačů se zastaví před zahájením převzetí služeb při selhání. Pokud zrušíte převzetí služeb při selhání v průběhu, zastaví převzetí služeb při selhání, ale nebude znovu replikaci virtuálního počítače.

V některých scénářích převzetí služeb při selhání vyžaduje další zpracování, který trvá přibližně osm až deset minut na dokončení. Možná jste si všimli již testovací převzetí služeb při selhání dobu fyzických serverů, počítačů, virtuální počítače VMware, které nemají povoluje služby DHCP a virtuální počítače VMware, který není k dispozici následující ovladače pro spouštění VMware Linux: miniport storvsc, vmbus, storflt, intelide, atapi.


## <a name="next-steps"></a>Další kroky

> [!div class="nextstepaction"]
> [Replikace virtuálních počítačů Azure do jiné oblasti po migraci na Azure](site-recovery-azure-to-azure-after-migration.md)
