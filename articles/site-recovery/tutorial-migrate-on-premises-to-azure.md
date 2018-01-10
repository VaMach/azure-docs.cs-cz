---
title: "Migrovat místní počítače do Azure s Azure Site Recovery | Microsoft Docs"
description: "Tento článek popisuje, jak migrovat místní počítače do Azure pomocí Azure Site Recovery."
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: tutorial
ms.date: 01/07/2018
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: ee9397406cbca21d8bd53019d9daac5a037f508c
ms.sourcegitcommit: 6fb44d6fbce161b26328f863479ef09c5303090f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/10/2018
---
# <a name="migrate-on-premises-machines-to-azure"></a>Migrace místních počítačů do Azure

Kromě použití [Azure Site Recovery](site-recovery-overview.md) služby ke správě a orchestraci zotavení po havárii místní počítače a virtuální počítače Azure pro účely provozní kontinuitu a zotavení po havárii (BCDR), můžete použít také lokality Obnovení ke správě migrace místní počítačů do Azure.


V tomto kurzu se dozvíte, jak migrovat místní virtuální počítače a fyzické servery do Azure. V tomto kurzu se naučíte:

> [!div class="checklist"]
> * Vyberte cíle replikace
> * Nastavení zdrojové a cílové prostředí
> * Nastavení zásad replikace
> * Povolení replikace
> * Spuštění testu migrace a ujistěte se, zda že vše funguje podle očekávání
> * Spuštění jednorázové převzetí služeb při selhání do Azure

Toto je třetí kurz v řadě. V tomto kurzu předpokládá, že jste už dokončili úkoly v předchozí kurzy:

1. [Příprava Azure](tutorial-prepare-azure.md)
2. Příprava místní [VMware](tutorial-prepare-on-premises-vmware.md) nebo servery Hyper-V.

Než začnete, je vhodné zkontrolovat [VMware](concepts-vmware-to-azure-architecture.md) nebo [technologie Hyper-V](concepts-hyper-v-to-azure-architecture.md) architektury pro zotavení po havárii.


## <a name="prerequisites"></a>Požadavky

Exportované sadou paravirtualized ovladače zařízení nejsou podporovány.


## <a name="create-a-recovery-services-vault"></a>Vytvoření trezoru Služeb zotavení

1. Přihlaste se k webu [Azure Portal](https://portal.azure.com) > **Recovery Services**.
2. Klikněte na **Nový** > **Monitorování a správa** > **Backup a Site Recovery**.
3. V **název**, zadejte popisný název **ContosoVMVault**. Pokud máte více než jedno předplatné, vyberte příslušný.
4. Vytvořte skupinu prostředků **ContosoRG**.
5. Zadejte oblast Azure. Informace o tom, které oblasti jsou podporované, najdete v části s geografickou dostupností v tématu s [podrobnostmi o cenách Azure Site Recovery](https://azure.microsoft.com/pricing/details/site-recovery/).
6. Chcete-li rychle získat přístup k úložišti z řídicího panelu, klikněte na tlačítko **připnout na řídicí panel** a pak klikněte na **vytvořit**.

   ![Nový trezor](./media/tutorial-migrate-on-premises-to-azure/onprem-to-azure-vault.png)

Nový trezor se přidá do **řídicí panel** pod **všechny prostředky**a v hlavním **trezory služeb zotavení** stránky.



## <a name="select-a-replication-goal"></a>Vyberte cíle replikace

Vyberte, jak chcete počítače replikovat a kam je chcete replikovat.
1. Klikněte na tlačítko **trezory služeb zotavení** > trezoru.
2. V nabídce prostředků, klikněte na tlačítko **Site Recovery** > **Příprava infrastruktury** > **cíl ochrany**.
3. V **cíl ochrany**, vyberte, chcete migrovat.
    - **VMware**: vyberte **do Azure** > **Ano, s hypervisoru VMWare vSphere**.
    - **Fyzický počítač**: vyberte **do Azure** > **není virtualizované/jiné**.
    - **Technologie Hyper-V**: vyberte **do Azure** > **Ano, s Hyper-V**. Pokud virtuální počítače Hyper-V jsou spravovány nástrojem VMM, vyberte **Ano**.


## <a name="set-up-the-source-environment"></a>Nastavení zdrojového prostředí

- [Nastavit](tutorial-vmware-to-azure.md#set-up-the-source-environment) zdrojové prostředí pro virtuální počítače VMware.
- [Nastavit](tutorial-physical-to-azure.md#set-up-the-source-environment) zdrojové prostředí pro fyzických serverů.
- [Nastavit](tutorial-hyper-v-to-azure.md#set-up-the-source-environment) zdrojové prostředí pro virtuální počítače Hyper-V.

## <a name="set-up-the-target-environment"></a>Nastavení cílového prostředí

Vyberte a ověřte cílové prostředky.

1. Klikněte na **Připravit infrastrukturu** > **Cíl** a vyberte předplatné Azure, které chcete použít.
2. Zadejte model nasazení Resource Manager.
3. Site Recovery zkontroluje, že máte minimálně jednu kompatibilní síť a účet úložiště Azure.

## <a name="set-up-a-replication-policy"></a>Nastavení zásad replikace

- [Nastavte zásady replikace](tutorial-vmware-to-azure.md#create-a-replication-policy) pro virtuální počítače VMware.
- [Nastavte zásady replikace](tutorial-physical-to-azure.md#create-a-replication-policy) pro fyzických serverů.
- [Nastavte zásady replikace](tutorial-hyper-v-to-azure.md#set-up-a-replication-policy) pro virtuální počítače Hyper-V.


## <a name="enable-replication"></a>Povolení replikace

- [Povolit replikaci](tutorial-vmware-to-azure.md#enable-replication) pro virtuální počítače VMware.
- [Povolit replikaci](tutorial-physical-to-azure.md#enable-replication) pro fyzických serverů.
- [Povolit replikaci](tutorial-hyper-v-to-azure.md#enable-replication) pro virtuální počítače Hyper-V.


## <a name="run-a-test-migration"></a>Spuštění testu migrace

Spuštění [testovací převzetí služeb při selhání](tutorial-dr-drill-azure.md) do Azure, abyste měli jistotu, všechno funguje podle očekávání.


## <a name="migrate-to-azure"></a>Migrace do Azure

Spusťte převzetí služeb při selhání pro počítače, které chcete migrovat.

1. V **nastavení** > **replikované položky** klikněte na počítač > **převzetí služeb při selhání**.
2. V **převzetí služeb při selhání** vyberte **bod obnovení** k převzetí služeb při selhání. Vyberte nejnovější bod obnovení.
3. Nastavení klíče šifrování není relevantní pro tento scénář.
4. Vyberte **vypnout počítač před zahájením převzetí služeb při selhání**. Site Recovery se pokusí provést vypnutí zdrojových virtuálních počítačích než převzetí služeb při selhání. Převzetí služeb při selhání pokračovat i v případě, že vypnutí selže. Můžete sledovat průběh převzetí služeb při selhání **úlohy** stránky.
5. Zkontrolujte, že virtuální počítač Azure se zobrazí v Azure podle očekávání.
6. V **replikované položky**, klikněte pravým tlačítkem na virtuální počítač > **dokončení migrace**. To dokončení procesu migrace, zastaví replikace pro virtuální počítač a zastaví fakturace Site Recovery pro virtuální počítač.

    ![Dokončení migrace](./media/tutorial-migrate-on-premises-to-azure/complete-migration.png)


> [!WARNING]
> **Nemáte zrušení převzetí služeb při selhání v průběhu**: replikace virtuálních počítačů se zastaví před zahájením převzetí služeb při selhání. Pokud zrušíte převzetí služeb při selhání v průběhu, zastaví převzetí služeb při selhání, ale nebude znovu replikaci virtuálního počítače.

V některých scénářích převzetí služeb při selhání vyžaduje další zpracování, který trvá přibližně osm až deset minut na dokončení. Možná jste si všimli již testovací převzetí služeb při selhání dobu fyzických serverů, počítačů, virtuální počítače VMware, které nemají povoluje služby DHCP a virtuální počítače VMware, který není k dispozici následující ovladače pro spouštění VMware Linux: miniport storvsc, vmbus, storflt, intelide, atapi.


## <a name="next-steps"></a>Další postup

V tomto kurzu jste migrovali virtuální počítače na místní na virtuálních počítačích Azure. Teď můžete nakonfigurovat zotavení po havárii pro virtuální počítače Azure.

> [!div class="nextstepaction"]
> [Nastavit zotavení po havárii](site-recovery-azure-to-azure-after-migration.md) pro virtuální počítače Azure po migraci z místního webu.
