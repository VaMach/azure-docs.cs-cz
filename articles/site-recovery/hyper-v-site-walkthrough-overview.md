---
title: "Replikace virtuálních počítačů technologie Hyper-V do Azure s Azure Site Recovery | Microsoft Docs"
description: "Popisuje, jak k orchestraci replikace, převzetí služeb při selhání a obnovení místní virtuální počítače Hyper-V do Azure"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: efddd986-bc13-4a1d-932d-5484cdc7ad8d
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/21/2017
ms.author: raynew
ms.openlocfilehash: da10b213bc2543942b5ac77cf5c5d8547c00220c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="replicate-hyper-v-virtual-machines-without-vmm-to-azure"></a>Replikace virtuálních počítačů technologie Hyper-V (bez VMM) do Azure 

> [!div class="op_single_selector"]
> * [Azure Portal](site-recovery-hyper-v-site-to-azure.md)
> * [Azure Classic](site-recovery-hyper-v-site-to-azure-classic.md)
> * [PowerShell – Resource Manager](site-recovery-deploy-with-powershell-resource-manager.md)
>
>

Tento článek obsahuje přehled kroků nezbytných k replikaci virtuálních počítačů Hyper-V místní do Azure, pomocí [Azure Site Recovery](site-recovery-overview.md) na portálu Azure. Ve virtuálních počítačích této nasazení technologie Hyper-V nejsou spravovány nástrojem System Center Virtual Machine Manager (VMM).


Po přečtení tohoto článku, post jakékoli komentáře v dolní části, nebo požádat technické dotazy na [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="step-1-review-architecture-and-prerequisites"></a>Krok 1: Posouzení architektura a požadavky

Před zahájením nasazení, zkontrolujte na architekturu scénáře a ujistěte se, že rozumíte všechny součásti, které potřebujete k nasazení

Přejděte na [krok 1: Přečtěte si architektura](hyper-v-site-walkthrough-architecture.md)


## <a name="step-2-review-prerequisites"></a>Krok 2: Kontrola předpokladů

Ujistěte se, že máte požadavky na místě pro každou součást nasazení:

- **Požadavky Azure**: budete potřebovat účet Microsoft Azure, sítě Azure a účty úložiště.
- **Požadavky technologie Hyper-V na místě**: Zajistěte, aby hostitelů Hyper-V jsou připraveny pro nasazení Site Recovery.
- **Replikovat virtuální počítače**: virtuální počítače, které chcete replikovat musí splňovat požadavky pro Azure.

Přejděte na [krok 2: ověření předpoklady a omezení](hyper-v-site-walkthrough-prerequisites.md)

## <a name="step-3-plan-capacity"></a>Krok 3: Plánování kapacity

Pokud provádíte úplné nasazení potřebujete zjistit, jaké prostředky replikace, které potřebujete. Existuje několik nástrojů, které vám pomůžou vám to. Přejděte ke kroku 2. Pokud provádíte stručně si pro testovací prostředí můžete přeskočit tento krok.

Přejděte na [Krok 3: Plánování kapacity](hyper-v-site-walkthrough-capacity.md)

## <a name="step-4-plan-networking"></a>Krok 4: Plánování sítě

Budete muset provést některé síťové plánování zajistit, že virtuální počítače Azure, jsou připojeny k sítím po převzetí služeb při selhání a která, ke kterým mají správné IP adresy.

Přejděte na [krok 4: plánování sítě](hyper-v-site-walkthrough-network.md)

##  <a name="step-5-prepare-azure-resources"></a>Krok 5: Příprava prostředků Azure

Nastavte před zahájením Azure sítím a úložišti. Můžete to provést během nasazení, ale doporučujeme, abyste že to uděláte před zahájením.

Přejděte na [krok 5: Příprava Azure](hyper-v-site-walkthrough-prepare-azure.md)


## <a name="step-6-prepare-hyper-v"></a>Krok 6: Příprava technologie Hyper-V

Ujistěte se, že technologie Hyper-V servery splňují požadavky na nasazení Site Recovery.

Přejděte na [krok 6: Příprava technologie Hyper-V](hyper-v-site-walkthrough-prepare-hyper-v.md)

## <a name="step-7-set-up-a-vault"></a>Krok 7: Nastavení trezoru

Budete muset nastavit orchestraci a Správa replikace v trezoru služeb zotavení. Když nastavíte trezor, zadejte co chcete replikovat a kam chcete replikovat, aby.

Přejděte na [krok 7: vytvoření trezoru](hyper-v-site-walkthrough-create-vault.md)

## <a name="step-8-configure-source-and-target-settings"></a>Krok 8: Konfigurace nastavení zdroje a cíle

Nastavte zdroje a cíle, který se používá pro replikaci. Nastavení nastavení zdroje, zahrnuje přidání hostitelů Hyper-V na Hyper-V lokalitě, instalace agenta zprostředkovatele služby Site Recovery a služeb zotavení na každém hostiteli technologie Hyper-V a registrace lokality v trezoru služeb zotavení.

Přejděte na [krok 8: nastavit zdroje a cíle](hyper-v-site-walkthrough-source-target.md)

## <a name="step-9-set-up-a-replication-policy"></a>Krok 9: Nastavení zásad replikace

Nastavení zásad a zadejte nastavení replikace pro virtuální počítače Hyper-v trezoru.

Přejděte na [krok 9: nastavení zásad replikace](hyper-v-site-walkthrough-replication.md)


## <a name="step-10-enable-replication"></a>Krok 10: Povolení replikace

Až budete mít zásadu replikace na místě, po povolení, dojde k počáteční replikaci virtuálního počítače.

Přejděte na [krok 10: povolení replikace](hyper-v-site-walkthrough-enable-replication.md)

## <a name="step-11-run-a-test-failover"></a>Krok 11: Spuštění testu převzetí služeb

Po dokončení počáteční replikace a je spuštěna rozdílová replikace, můžete spustit převzetí služeb při selhání a ujistěte se, že vše funguje podle očekávání.

Přejděte na [krok 11: spustit testovací převzetí služeb](hyper-v-site-walkthrough-test-failover.md)
