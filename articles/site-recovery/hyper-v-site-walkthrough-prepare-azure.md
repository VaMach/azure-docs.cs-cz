---
title: "Příprava prostředků Azure k replikaci virtuálních počítačů technologie Hyper-V (bez System Center VMM) do Azure pomocí Azure Site Recovery | Microsoft Docs"
description: "Popisuje, co potřebujete zavedené v Azure před zahájením replikace virtuálních počítačů technologie Hyper-V (bez VMM) do Azure pomocí Azure Site Recovery"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 28fa722c-675e-4637-98eb-7ccbf3806d69
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/21/2017
ms.author: raynew
ms.openlocfilehash: 1a30cadaab7e053184f0be133f1da5bfddc1fd91
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="step-5-prepare-azure-resources-for-hyper-v-replication-to-azure"></a>Krok 5: Příprava prostředků Azure pro replikaci technologie Hyper-V do Azure.

Postupujte podle pokynů v tomto článku Příprava prostředků Azure, takže můžete replikovat místní virtuální počítače Hyper-V (bez System Center VMM) do Azure pomocí [Azure Site Recovery](site-recovery-overview.md) služby.

Po přečtení tohoto článku, post jakékoli komentáře v dolní části, nebo požádat technické dotazy na [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="before-you-start"></a>Než začnete

Zajistěte, aby si přečetli [požadavky](hyper-v-site-walkthrough-prerequisites.md)

## <a name="set-up-an-azure-account"></a>Nastavit účet Azure

- Získání [účet Microsoft Azure](http://azure.microsoft.com/).
- Můžete začít s [bezplatnou zkušební verzí](https://azure.microsoft.com/pricing/free-trial/).
- Zkontrolujte podporované oblasti pro obnovení lokality v rámci geografickou dostupností v [Azure Site Recovery podrobnosti o cenách](https://azure.microsoft.com/pricing/details/site-recovery/).
- Další informace o [cenách za Site Recovery](site-recovery-faq.md#pricing)a získat [podrobnosti o cenách](https://azure.microsoft.com/pricing/details/site-recovery/).


## <a name="set-up-an-azure-network"></a>Nastavení sítě Azure

- Nastavte síť Azure. Virtuální počítače Azure jsou umístěny v této síti, když jste vytvořili po převzetí služeb při selhání.
- Síť musí být ve stejné oblasti jako trezor služeb zotavení
- Site Recovery na portálu Azure můžete použít nastavení sítích [Resource Manager](../resource-manager-deployment-model.md), nebo v klasickém režimu.
- Doporučujeme nastavit síť ještě před tím, než začnete. Pokud to neuděláte, budete to muset udělat při nasazení služby Site Recovery.
- Další informace o [ceny virtuální sítě](https://azure.microsoft.com/pricing/details/virtual-network/).


## <a name="set-up-an-azure-storage-account"></a>Nastavení účtu úložiště Azure

- Site Recovery replikuje místní počítače do úložiště Azure. Virtuální počítače Azure jsou vytvořené z úložiště po převzetí služeb při selhání.
- Nastavit standard nebo premium [účtu úložiště Azure](../storage/common/storage-create-storage-account.md#create-a-storage-account) k ukládání dat replikovaných do Azure.
- [Storage úrovně Premium](../storage/common/storage-premium-storage.md) se obvykle používá u virtuálních počítačů, které je třeba neustále vysoké vstupně-výstupní výkon a nízká latence pro zatížení s intenzivním vstupně-výstupní operace hostitele.
- Pokud pro ukládání replikovaných dat chcete používat účet Storage úrovně Premium, musíte mít také účet Storage úrovně Standard pro ukládání protokolů replikace, do kterých se zaznamenávají průběžné změny místních dat.
- V závislosti na modelu prostředků, který chcete použít pro převzal virtuálních počítačích Azure, které nastavíte účet v [režimu Resource Manager](../storage/common/storage-create-storage-account.md), nebo [klasickém režimu](../storage/common/storage-create-storage-account.md).
- Doporučujeme, abyste před zahájením nastavení účtu úložiště. Pokud to neuděláte, budete muset udělat při nasazování Site Recovery. Tyto účty musí být ve stejné oblasti jako trezor služeb zotavení.
- Nelze přesunout úložiště účtů používaných při obnovení lokality, mezi skupinami prostředků v rámci stejného předplatného, nebo v různých předplatných.


## <a name="next-steps"></a>Další kroky

Přejděte na [krok 6: Příprava technologie Hyper-V prostředky](hyper-v-site-walkthrough-prepare-hyper-v.md)
