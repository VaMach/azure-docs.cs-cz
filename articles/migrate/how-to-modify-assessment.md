---
title: "Přizpůsobit Azure migrovat nastavení assessment | Microsoft Docs"
description: "Popisuje, jak nastavit a spustit posouzení pro migraci virtuálních počítačů VMware do Azure pomocí Azure Plánovač migrace"
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: article
ms.date: 12/12/2017
ms.author: raynew
ms.openlocfilehash: ce47790f6214864afdba33eb5cbe3a9e49b81cd5
ms.sourcegitcommit: aaba209b9cea87cb983e6f498e7a820616a77471
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/12/2017
---
# <a name="customize-an-assessment"></a>Přizpůsobení posouzení

[Azure migrací](migrate-overview.md) vytvoří posuzování s výchozím nastavením. Po vytvoření posouzení, můžete upravit tato výchozí nastavení pomocí pokynů v tomto článku.


## <a name="edit-assessment-values"></a>Upravit hodnoty hodnocení

1. V projektu Azure migrovat **vyhodnocování** vyberte hodnocení a klikněte na tlačítko **upravit vlastnosti**.
2. Upravte nastavení podle následující tabulky.

    **Nastavení** | **Podrobnosti** | **Výchozí**
    --- | --- | ---
    **Cílové umístění** | Umístění Azure, do kterého chcete migrovat. |  Západní USA 2 je výchozí umístění.
    **Redundance úložiště** | Typ úložiště, které budou po migraci používat virtuální počítače Azure. | Pouze [místně redundantní úložiště (LRS)](../storage/common/storage-redundancy.md#locally-redundant-storage) replikace v současné době podporuje.
    **Faktor komfortu** | Pohodlí. faktor je vyrovnávací paměť, která se používá při hodnocení. Účet pro akcí, například sezónní využití krátkodobých historie výkonu, pravděpodobně v budoucnu zvýšit v využití pomocí něj. | Výchozí nastavení je 1.3 x.
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
