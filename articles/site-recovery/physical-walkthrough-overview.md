---
title: "Replikovat fyzické místní servery do Azure s Azure Site Recovery | Microsoft Docs"
description: "Obsahuje přehled kroků pro replikaci úlohy běžící na fyzických serverech místní Windows nebo Linuxem do Azure se službou Azure Site Recovery."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 20122f01-929a-4675-b85b-a9b99d2618bc
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: raynew
ms.openlocfilehash: 0a09b35e98dc0b2f5283c2a707a3a2b8ac9a39f2
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="replicate-physical-servers-to-azure-with-site-recovery"></a>Replikace fyzických serverů do Azure pomocí Site Recovery

Tento článek obsahuje přehled kroků nezbytných k replikaci místní Windows nebo Linuxem fyzických serverů do Azure, pomocí [Azure Site Recovery](site-recovery-overview.md) službu na portálu Azure.


## <a name="step-1-review-architecture-and-prerequisites"></a>Krok 1: Posouzení architektura a požadavky

Před zahájením nasazení, zkontrolujte na architekturu scénáře a ujistěte se, že rozumíte všechny součásti, které je třeba nastavit nasazení.

Přejděte na [krok 1: Přečtěte si architektura](physical-walkthrough-architecture.md)


## <a name="step-2-review-prerequisites"></a>Krok 2: Kontrola předpokladů

Ujistěte se, že máte požadavky na místě pro každou součást nasazení:

- **Požadavky Azure**: budete potřebovat účet Microsoft Azure, sítě Azure a účty úložiště.
- **Místní součásti Site Recovery**: budete potřebovat počítač se systémem součásti Site Recovery na místě.
- **Replikovat počítače**: servery, které chcete replikovat potřebovat pro dosažení souladu s místními a požadavky pro Azure.

Přejděte na [krok 2: Přečtěte si požadavky a omezení](physical-walkthrough-prerequisites.md)

## <a name="step-3-plan-capacity"></a>Krok 3: Plánování kapacity

Pokud provádíte úplné nasazení potřebujete zjistit, jaké prostředky replikace, které potřebujete. Pokud jste to nějakou rychlou nastavit tak, aby testovací prostředí, můžete tento krok přeskočit.

Přejděte na [Krok 3: Plánování kapacity](physical-walkthrough-capacity.md)

## <a name="step-4-plan-networking"></a>Krok 4: Plánování sítě

Budete muset provést některé síťové plánování zajistit, že virtuální počítače Azure, jsou připojeny k sítím po převzetí služeb při selhání a která, ke kterým mají správné IP adresy.

Přejděte na [krok 4: plánování sítě](physical-walkthrough-network.md)

##  <a name="step-5-prepare-azure-resources"></a>Krok 5: Příprava prostředků Azure

Nastavte před zahájením Azure sítím a úložišti. 

Přejděte na [krok 5: Příprava Azure](physical-walkthrough-prepare-azure.md)


## <a name="step-6-set-up-a-vault"></a>Krok 6: Nastavení trezoru

Nastavíte trezor služeb zotavení pro orchestraci a správu replikace. Když nastavíte trezor, zadejte co chcete replikovat a kam chcete replikovat, aby.

Přejděte na [krok 6: nastavení trezoru](physical-walkthrough-create-vault.md)

## <a name="step-7-configure-source-and-target-settings"></a>Krok 7: Konfigurace nastavení zdroje a cíle

Nakonfigurujte nastavení pro zdrojové a cílové lokality (Azure). Nastavení zdroje zahrnuje systémem Unified instalačního programu pro instalaci součásti Site Recovery na místě.

Přejděte na [krok 7: nastavit zdroje a cíle](physical-walkthrough-source-target.md)

## <a name="step-8-set-up-a-replication-policy"></a>Krok 8: Nastavení zásad replikace

Můžete nastavit zásadu k určení jak fyzických serverů musí replikovat.

Přejděte na [krok 8: nastavení zásad replikace](physical-walkthrough-replication.md)

## <a name="step-9-install-the-mobility-service"></a>Krok 9: Instalace služby Mobility

Služba Mobility musí být nainstalovaná na každém serveru, který chcete replikovat. Existuje několik způsobů, jak nastavit službu s instalace push nebo pull.

Přejděte na [krok 9: instalace služby Mobility](physical-walkthrough-install-mobility.md)

## <a name="step-10-enable-replication"></a>Krok 10: Povolení replikace

Jakmile je na serveru běží služba Mobility, můžete povolit pro něho replikaci. Když povolíte, dojde k počáteční replikaci virtuálního počítače.

Přejděte na [krok 10: povolení replikace](physical-walkthrough-enable-replication.md)

## <a name="step-11-run-a-test-failover"></a>Krok 11: Spuštění testu převzetí služeb

Po dokončení počáteční replikace a je spuštěna rozdílová replikace, můžete spustit převzetí služeb při selhání a ujistěte se, že vše funguje podle očekávání.

Přejděte na [krok 11: spustit testovací převzetí služeb](physical-walkthrough-test-failover.md)

