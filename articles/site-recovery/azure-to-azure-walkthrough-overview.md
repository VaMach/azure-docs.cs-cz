---
title: "Replikace virtuálních počítačů Azure mezi oblastmi Azure | Microsoft Docs"
description: "Shrnuje kroky potřebné k replikaci virtuálních počítačů Azure mezi oblastí Azure se službou Azure Site Recovery na portálu Azure"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmon
editor: 
ms.assetid: 6dd36239-4363-4538-bf80-a18e71b8ec67
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/01/2017
ms.author: raynew
ms.openlocfilehash: 9258613161a61e36b1d0c5796d5763c916d66859
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="replicate-azure-vms-between-regions-with-azure-site-recovery"></a>Replikace virtuálních počítačů Azure mezi oblastmi s Azure Site Recovery

>Tento článek obsahuje přehled kroků nezbytných k replikaci adresáře Azure virtuální počítače (VM) v jedné oblasti Azure k virtuálním počítačům Azure v jiné oblasti. 

>[!NOTE]
>
> Azure replikace virtuálního počítače je aktuálně ve verzi preview.

POST dotazy a v dolní části tohoto článku nebo na [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="step-1-review-architecture"></a>Krok 1: Kontrola architektura

Před zahájením nasazení, zkontrolujte na architekturu scénáře a součásti, které je třeba nasadit.

Přejděte na [krok 1: Přečtěte si architektura](azure-to-azure-walkthrough-architecture.md)


## <a name="step-2-review-prerequisites"></a>Krok 2: Kontrola předpokladů

Zkontrolujte, zda máte požadavky Azure v místech, včetně předplatného, virtuálních sítí, účty úložiště a požadavky virtuálního počítače.

Přejděte na [krok 2: ověření předpoklady a omezení](azure-to-azure-walkthrough-prerequisites.md)


## <a name="step-3-plan-networking"></a>Krok 3: Plánování sítě

Zkontrolujte, zda odchozí připojení nastavená na virtuálních počítačích Azure, které chcete replikovat, a nastavení připojení z místní.

Přejděte na [krok 4: plánování sítě](azure-to-azure-walkthrough-network.md)



## <a name="step-4-create-a-vault"></a>Krok 4: Vytvoření trezoru 

Budete muset nastavit trezor služeb zotavení pro orchestraci a správu replikace a zadejte oblasti zdroje.

Přejděte na [krok 4: vytvoření trezoru](azure-to-azure-walkthrough-vault.md)


## <a name="step-5-enable-replication"></a>Krok 5: Povolení replikace


K povolení replikace, nakonfigurujte nastavení cílového umístění, nastavení zásad replikace a vyberte virtuální počítače Azure, který chcete replikovat. Když povolíte, dojde k počáteční replikaci virtuálního počítače.

Přejděte na [krok 5: povolení replikace](azure-to-azure-walkthrough-enable-replication.md)


## <a name="step-6-run-a-test-failover"></a>Krok 6: Spuštění testu převzetí služeb

Po dokončení počáteční replikace a je spuštěna rozdílová replikace, můžete spustit převzetí služeb při selhání a ujistěte se, že vše funguje podle očekávání.

Přejděte na [krok 6: spustit testovací převzetí služeb](azure-to-azure-walkthrough-test-failover.md)


