---
title: "Přizpůsobit Azure migrovat nastavení assessment | Microsoft Docs"
description: "Popisuje, jak nastavit a spustit posouzení pro migraci virtuálních počítačů VMware do Azure pomocí Azure Plánovač migrace"
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: article
ms.date: 02/26/2018
ms.author: raynew
ms.openlocfilehash: efb4ad59d25a0c1209e4f0f6cd406c2f0d48159c
ms.sourcegitcommit: 088a8788d69a63a8e1333ad272d4a299cb19316e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="customize-an-assessment"></a>Přizpůsobení posouzení

[Azure migrací](migrate-overview.md) vytvoří posuzování s výchozí vlastnosti. Po vytvoření posouzení, můžete upravit výchozí vlastnosti pomocí pokynů v tomto článku.


## <a name="edit-assessment-properties"></a>Upravit vlastnosti hodnocení

1. V **vyhodnocování** migrace projekt, vyberte hodnocení a klikněte na tlačítko **upravit vlastnosti**.
2. Změnit vlastnosti v souladu s v následující tabulce:

    **Nastavení** | **Podrobnosti** | **Výchozí**
    --- | --- | ---
    **Cílové umístění** | Umístění Azure, do kterého chcete migrovat.<br/><br/> Azure migrací aktuálně podporuje 30 oblastí, včetně Austrálie – východ, Austrálie – jihovýchod, Brazílie – Jih, Střední Kanada, Východní Kanada, střed, střed USA, východní Čína, severní Čína, východní Asie, východní USA, Německo centrální, Německo – severovýchod, východní USA 2, Japonsko – Východ, Japonsko – Západ, Korejská – střed, Korejská – Jih, střed USA – sever, Severní Evropa, střed USA – Jih, jihovýchodní Asie, – Jih, Indie, Spojené království – Jih, Spojené království – Západ, střed USA – Západ, západní Evropa, Západní Indie, západ USA a západní US2. |  Západní USA 2 je výchozí umístění.
    **Redundance úložiště** | Typ redundance úložiště, který bude používat k virtuálním počítačům Azure po migraci. | [Místně redundantní úložiště (LRS)](../storage/common/storage-redundancy.md#locally-redundant-storage) je výchozí hodnota. Azure podporuje pouze migrací spravovaných disků na základě hodnocení a spravované disky se podporují jenom LRS, proto vlastnost aktuálně má jenom možnost LRS. 
    **Kritéria pro změnu velikosti** | Kritérium, podle kterého Azure Migrate určí správnou velikost virtuálních počítačů pro Azure. Můžete provést buď nezadávejte *na základě výkonu* změně velikosti nebo velikost virtuálních počítačů *jako místní*, bez ohledu historie výkonu. | Nastavení velikosti na základě výkonu je výchozí možností.
    **Historie výkonu** | Doba trvání vzít v úvahu pro vyhodnocení výkonu virtuálních počítačů. Tato vlastnost platí jenom při nastavení velikosti kritérium *výkonu na základě velikosti*. | Výchozí hodnota je jeden den.
    **Percentil využití** | Hodnota percentilu nastavit pro optimalizaci velikosti vzorku výkonu. Tato vlastnost platí jenom při nastavení velikosti kritérium *výkonu na základě velikosti*.  | Výchozí hodnota je 95. percentil.
    **Cenová úroveň** | Můžete zadat [cenovou úroveň (Basic nebo Standard)](../virtual-machines/windows/sizes-general.md) cílových virtuálních počítačů Azure. Pokud například plánujete migrovat produkční prostředí, měli byste zvážit úroveň Standard, která poskytuje virtuální počítače s nízkou latencí, ale může být dražší. Na druhou stranu, pokud máte prostředí pro vývoj a testování, měli byste zvážit úroveň Basic s virtuálními počítači s vyšší latencí, která je levnější. | Ve výchozím nastavení se použije úroveň [Standard](../virtual-machines/windows/sizes-general.md).
    **Faktor komfortu** | Azure Migrate při posuzování počítá s rezervou (faktor komfortu). Tato rezerva se použije nad rámec dat o využití počítače pro virtuální počítače (procesor, paměť, disk a síť). Důvodem použití faktoru komfortu jsou problémy, jako jsou sezónní využití, krátká historie výkonu a pravděpodobný růst budoucího využití.<br/><br/> Například z virtuálního počítače s 10 jádry a 20% využitím je normálně ve výsledku virtuální počítač se 2 jádry. S faktorem komfortu 2,0× je však výsledkem virtuální počítač se 4 jádry. | Výchozí nastavení je 1.3 x.
    **Nabídka** | [Nabídka Azure](https://azure.microsoft.com/support/legal/offer-details/) zapsaných do. | [Průběžné platby](https://azure.microsoft.com/offers/ms-azr-0003p/) je výchozí.
    **Měna.** | Měna fakturace. | Výchozí hodnota je USD.
    **Diskontní (%)** | Jakékoliv slevy specifické pro předplatné, se zobrazí nad nabídku Azure. | Výchozí nastavení je 0 %.
    **Výhody Azure hybridní** | Zadejte, pokud máte programu software assurance a jsou způsobilé pro [Azure hybridní Benefit](https://azure.microsoft.com/pricing/hybrid-use-benefit/). Pokud nastavíte hodnotu Ano, ceny systému Windows Azure jsou považovány za pro virtuální počítače Windows. | Výchozí hodnota je Yes (Ano).

3. Klikněte na tlačítko **Uložit** aktualizovat hodnocení.


## <a name="next-steps"></a>Další postup

[Přečtěte si další informace](concepts-assessment-calculation.md) o tom, jak se v rámci posouzení počítají náklady.
