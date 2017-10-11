---
title: "Replikace virtuálních počítačů technologie Hyper-V do sekundární lokalita VMM s Azure Site Recovery | Microsoft Docs"
description: "Poskytuje přehled pro replikaci virtuálních počítačů Hyper-V do sekundární lokalita VMM pomocí portálu Azure."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: jwhit
editor: 
ms.assetid: 476ca82a-8f5c-4498-9dcf-e1011d60ed59
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: raynew
ms.openlocfilehash: b422dd2cf23426de2f154a553b38509082536309
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="replicate-hyper-v-virtual-machines-in-vmm-clouds-to-a-secondary-vmm-site"></a>Replikace virtuálních počítačů technologie Hyper-V v cloudech VMM do sekundární lokalita VMM

> [!div class="op_single_selector"]
> * [Azure Portal](site-recovery-vmm-to-vmm.md)
> * [Portál Classic](site-recovery-vmm-to-vmm-classic.md)
> * [PowerShell – Resource Manager](site-recovery-vmm-to-vmm-powershell-resource-manager.md)
>
>

Tento článek obsahuje přehled kroků nezbytných replikovat místní technologie Hyper-V virtuální počítače (VM) spravovat v nástroji System Center Virtual Machine Manager (VMM) cloudy do sekundárního umístění VMM pomocí [Azure Site Recovery](site-recovery-overview.md) na portálu Azure.

Po přečtení tohoto článku můžete publikovat jakékoli dotazy nebo připomínky na jeho konci nebo na [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="step-1-review-the-scenario-architecture"></a>Krok 1: Posouzení na architekturu scénáře

Před zahájením nasazení, zkontrolujte na architekturu scénáře a ujistěte se, že rozumíte všechny součásti, které potřebujete k nasazení.

Přejděte na [krok 1: Přečtěte si architekturu](vmm-to-vmm-walkthrough-architecture.md).

## <a name="step-2-review-prerequisites-and-limitations"></a>Krok 2: Kontrola předpoklady a omezení

Ujistěte se, že rozumíte nasazení předpoklady a omezení.

**Požadavky Azure**: budete potřebovat předplatné Microsoft Azure a trezoru služeb zotavení Azure, orchestraci a správě replikace.
**Na místní servery VMM a hostitelé Hyper-V**: Ujistěte se, že jsou servery VMM a hostitelé Hyper-V kompatibilní a připravený pro nasazení Site Recovery.

Přejděte na [krok 2: ověření předpoklady a omezení](vmm-to-vmm-walkthrough-prerequisites.md).

## <a name="step-3-plan-networking"></a>Krok 3: Plánování sítě

Budete muset provést některé síťové plánování nezapomeňte nakonfigurovat mapování sítě mezi sítěmi virtuálních počítačů nástroje VMM při nasazení tohoto scénáře.

Přejděte na [krok 3: plánování sítě](vmm-to-vmm-walkthrough-network.md).


## <a name="step-4-prepare-vmm-and-hyper-v"></a>Krok 4: Příprava VMM a technologie Hyper-V

Připravte servery VMM a hostitelé Hyper-V pro nasazení Site Recovery.

Přejděte na [krok 4: Příprava na místní servery](vmm-to-vmm-walkthrough-vmm-hyper-v.md).

## <a name="step-5-set-up-a-vault"></a>Krok 5: Nastavení trezoru

Nastavení trezoru služeb zotavení. Trezor obsahuje konfigurační nastavení a orchestruje replikaci.

[Krok 5: Nastavení trezoru](vmm-to-vmm-walkthrough-create-vault.md).

## <a name="step-6-set-up-source-and-target-settings"></a>Krok 6: Nastavení zdrojové a cílové nastavení

Nastavte zdrojové a cílové umístění VMM replikace. Přidejte servery VMM do trezoru a stáhnout instalační soubory pro součásti Site Recovery. Spusťte instalační program zprostředkovatele Azure Site Recovery na serveru VMM. Instalační program nainstaluje zprostředkovatele na serveru VMM a zaregistruje server v trezoru. Nainstalujte agenta služeb zotavení Microsoft na každém hostiteli technologie Hyper-V.

Přejděte na [krok 6: nastavení nastavení zdrojové a cílové](vmm-to-vmm-walkthrough-source-target.md).

## <a name="step-7-configure-network-mapping"></a>Krok 7: Nakonfigurování mapování sítě

Mapování sítě virtuálních počítačů nástroje VMM ve zdrojové a cílové umístění. Po převzetí služeb při selhání jsou virtuální počítače vytvořené v cílové síti, která se mapuje na zdrojové síti virtuálních počítačů, ve kterém se nachází zdrojový virtuální počítač technologie Hyper-V.

Přejděte na [krok 7: nakonfigurování mapování sítě](vmm-to-vmm-walkthrough-network-mapping.md).


## <a name="step-8-set-up-a-replication-policy"></a>Krok 8: Nastavení zásad replikace

Zadejte, jak bude replikován virtuální počítače mezi umístěními VMM.

Přejděte na [krok 8: nastavte zásady replikace](vmm-to-vmm-walkthrough-replication.md).


## <a name="step-9-enable-replication-for-vms"></a>Krok 9: Povolení replikace pro virtuální počítače

Vyberte virtuální počítače, které chcete replikovat. Povolení virtuálního počítače pro replikace spustí počáteční replikace do sekundární lokality, za nímž následuje probíhající rozdílová replikace.

Přejděte na [krok 9: povolení replikace](vmm-to-vmm-walkthrough-enable-replication.md).


## <a name="step-10-run-a-test-failover"></a>Krok 10: Spustit testovací převzetí služeb

Otestujete převzetí služeb při selhání, abyste měli jistotu, že všechno funguje, jak má.

Přejděte na [krok 10: spustit testovací převzetí služeb](vmm-to-vmm-walkthrough-test-failover.md).
