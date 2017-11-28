---
title: "Přizpůsobit Azure migrovat nastavení assessment | Microsoft Docs"
description: "Popisuje, jak nastavit a spustit posouzení pro migraci virtuálních počítačů VMware do Azure pomocí Azure Plánovač migrace"
services: migrate
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: a068b9c7-5f87-4fe1-90b9-3be48d91aa3f
ms.service: migrate
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 11/21/2017
ms.author: raynew
ms.openlocfilehash: 6db4ff050dbf8d4e08415226931e71f964525068
ms.sourcegitcommit: 5bced5b36f6172a3c20dbfdf311b1ad38de6176a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/27/2017
---
# <a name="customize-an-assessment"></a>Přizpůsobení posouzení

[Azure migrací](migrate-overview.md) vytvoří posuzování s výchozím nastavením. Po vytvoření posouzení, můžete upravit tato výchozí nastavení pomocí pokynů v tomto článku.


## <a name="edit-assessment-values"></a>Upravit hodnoty hodnocení

1. V projektu Azure migrovat **vyhodnocování** vyberte hodnocení a klikněte na tlačítko **upravit vlastnosti**.
2. Upravte nastavení podle následující tabulky.

    **Nastavení** | **Podrobnosti** | **Výchozí**
    --- | --- | ---
    **Cílové umístění** | Azure umístění, do kterého chcete migrovat. |  Aktuálně je podporována pouze východní USA.
    **Redundance úložiště** | Typ úložiště, který bude používat k virtuálním počítačům Azure po migraci. | Pouze [místně redundantní úložiště (LRS)](../storage/common/storage-redundancy.md#locally-redundant-storage) replikace v současné době podporuje.
    **Faktor pohodlí** | Pohodlí. faktor je vyrovnávací paměť, která se používá při hodnocení. Účet pro akcí, například sezónní využití krátkodobých historie výkonu, pravděpodobně v budoucnu zvýšit v využití pomocí něj. | Výchozí nastavení je 1.3 x.
    **Prováděcí historie** | Čas používaný k vyhodnocení historie výkonu. | Výchozí hodnota je jeden měsíc.
    **Percentil využití** | Hodnota percentilu ke zvážení prováděcí historie. | Výchozí hodnota je 95 %.
    **Cenová úroveň** | Můžete zadat [cenová úroveň](https://azure.microsoft.com/blog/basic-tier-virtual-machines-2/) pro virtuální počítač.  | Ve výchozím nastavení [standardní](../virtual-machines/windows/sizes-general.md) vrstvy se používá.
    **Nabídka** | [Azure nabízí](https://azure.microsoft.com/support/legal/offer-details/) které se týkají. | [Průběžné platby](https://azure.microsoft.com/offers/ms-azr-0003p/) je výchozí.
    **Měna.** | Měna fakturace. | Výchozí hodnota je USD.
    **Diskontní (%)** | Jakékoliv slevy specifické pro předplatné, se zobrazí nad žádné nabídky. | Výchozí nastavení je 0 %.
    **Výhody použití Azure hybridní** | Určuje, jestli jste v registraci [Azure hybridní použití zvýhodnění](https://azure.microsoft.com/pricing/hybrid-use-benefit/). Pokud nastavíte hodnotu Ano, ceny systému Windows Azure jsou pro virtuální počítače Windows zvažte. | Výchozí hodnota je Yes (Ano).

3. Klikněte na tlačítko **Uložit** aktualizovat hodnocení.


## <a name="next-steps"></a>Další kroky

[Další informace](concepts-assessment-calculation.md) o tom, jak jsou vypočítávány vyhodnocování.
