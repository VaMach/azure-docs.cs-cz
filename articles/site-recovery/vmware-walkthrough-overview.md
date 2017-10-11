---
title: "Replikace virtuálních počítačů VMware do Azure s Azure Site Recovery | Microsoft Docs"
description: "Poskytuje přehled kroků pro replikaci úlohy běžící na virtuálních počítačích VMware do Azure"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: dab98aa5-9c41-4475-b7dc-2e07ab1cfd18
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: raynew
ms.openlocfilehash: db6f5f95929503e82a529dba26b56af1edb0767f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="replicate-vmware-vms-to-azure-with-site-recovery"></a>Replikace virtuálních počítačů VMware do Azure pomocí Site Recovery

Tento článek obsahuje přehled kroků nezbytných k replikaci na lokální virtuální počítače VMware do Azure, pomocí [Azure Site Recovery](site-recovery-overview.md) službu na portálu Azure.


![Proces nasazení](./media/vmware-walkthrough-overview/vmware-to-azure-process.png)

**Obrázek 1: Souhrn procesu nasazení**

## <a name="step-1-review-architecture-and-prerequisites"></a>Krok 1: Posouzení architektura a požadavky

Před zahájením nasazení, zkontrolujte na architekturu scénáře a ujistěte se, že rozumíte všechny součásti, které potřebujete k nasazení

Přejděte na [krok 1: Přečtěte si architektura](vmware-walkthrough-architecture.md)


## <a name="step-2-review-prerequisites"></a>Krok 2: Kontrola předpokladů

Ujistěte se, že máte požadavky na místě pro každou součást nasazení:

- **Požadavky Azure**: budete potřebovat účet Microsoft Azure, sítě Azure a účty úložiště.
- **Místní součásti Site Recovery**: budete potřebovat počítač se systémem součásti Site Recovery na místě.
- **Místní požadavky VMware**: je nutné nastavit účtů, aby Site Recovery přístup serverů VMware a virtuálních počítačů.
- **Replikovat virtuální počítače**: virtuální počítače, které chcete replikovat potřebují k dosažení souladu s požadavky na Azure a nainstalována součást služby Mobility.

Přejděte na [krok 2: Přečtěte si požadavky a omezení](vmware-walkthrough-prerequisites.md)

## <a name="step-3-plan-capacity"></a>Krok 3: Plánování kapacity

Pokud provádíte úplné nasazení potřebujete zjistit, jaké prostředky replikace, které potřebujete. Existuje několik nástrojů, které vám pomůžou vám to. Přejděte ke kroku 2. Pokud provádíte stručně si pro testovací prostředí můžete přeskočit tento krok.

Přejděte na [Krok 3: Plánování kapacity](vmware-walkthrough-capacity.md)

## <a name="step-4-plan-networking"></a>Krok 4: Plánování sítě

Budete muset provést některé síťové plánování zajistit, že virtuální počítače Azure, jsou připojeny k sítím po převzetí služeb při selhání a která, ke kterým mají správné IP adresy.

Přejděte na [krok 4: plánování sítě](vmware-walkthrough-network.md)

##  <a name="step-5-prepare-azure-resources"></a>Krok 5: Příprava prostředků Azure

Nastavte před zahájením Azure sítím a úložišti. Můžete to provést během nasazení, ale doporučujeme, abyste že to uděláte před zahájením.

Přejděte na [krok 5: Příprava Azure](vmware-walkthrough-prepare-azure.md)


## <a name="step-6-prepare-vmware"></a>Krok 6: Příprava VMware

Je třeba nastavit účty, které budou používat Site Recovery na:

- Přístup k VMware virtualizace serverů automaticky zjistit virtuální počítače.
- Virtuální počítače přístup k instalaci služby Mobility. Každý virtuální počítač, který chcete replikovat, musí mít nainstalován předtím, než můžete povolit pro něho replikaci agenta služby Mobility.

Přejděte na [krok 6: Příprava VMware](vmware-walkthrough-prepare-vmware.md)

## <a name="step-7-set-up-a-vault"></a>Krok 7: Nastavení trezoru

Budete muset nastavit orchestraci a Správa replikace v trezoru služeb zotavení. Když nastavíte trezor, zadejte co chcete replikovat a kam chcete replikovat, aby.

Přejděte na [krok 7: nastavení trezoru](vmware-walkthrough-create-vault.md)

## <a name="step-8-configure-source-and-target-settings"></a>Krok 8: Konfigurace nastavení zdroje a cíle

Nastavte zdroje a cíle, který se používá pro replikaci. Nastavení nastavení zdroje zahrnuje systémem Unified instalačního programu pro instalaci součásti Site Recovery na místě.

Přejděte na [krok 8: nastavit zdroje a cíle](vmware-walkthrough-source-target.md)

## <a name="step-9-set-up-a-replication-policy"></a>Krok 9: Nastavení zásad replikace

Nastavení zásady zadat nastavení replikace pro virtuální počítače VMware v trezoru.

Přejděte na [krok 9: nastavení zásad replikace](vmware-walkthrough-replication.md)

## <a name="step-10-install-the-mobility-service"></a>Krok 10: Nainstalujte službu Mobility

Služba Mobility musí být nainstalovaná na jednotlivé virtuální počítače, které chcete replikovat. Existuje několik způsobů, jak nastavit službu s instalace push nebo pull.

Přejděte na [krok 10: Nainstalujte službu Mobility](vmware-walkthrough-install-mobility.md)

## <a name="step-11-enable-replication"></a>Krok 11: Povolení replikace

Jakmile je na virtuálním počítači běží služba Mobility můžete povolit pro něho replikaci. Když povolíte, dojde k počáteční replikaci virtuálního počítače.

Přejděte na [krok 11: povolení replikace](vmware-walkthrough-enable-replication.md)

## <a name="step-12-run-a-test-failover"></a>Krok 12: Spuštění testu převzetí služeb

Po dokončení počáteční replikace a je spuštěna rozdílová replikace, můžete spustit převzetí služeb při selhání a ujistěte se, že vše funguje podle očekávání.

Přejděte na [krok 12: spustit testovací převzetí služeb](vmware-walkthrough-test-failover.md)
